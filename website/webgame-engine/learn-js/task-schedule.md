# Task Schedule

## JavaScript 執行環境概念（Runtime concepts）

JavaScript 是單線程（single threaded runtime）的程式語言，所有的程式碼片段都會在堆疊中（stack）被執行，而且一次只會執行一個程式碼片段。

!!! quote

    如果JavaScript是單執行緒的，那麼我們如何像在Java中那樣創建和運行執行緒？

    我們使用events或設定一段程式碼在給定時間執行，這種非同步性在 JavaScript 中稱為 event loop。

![js-v8](/webgame-engine/assets/task-schedule/js-visual-representation.svg)

JavaScript 是由 堆疊（Stack）、堆積（Heap）、任務佇列（Task Queue） 所組成的：

- Stack：用來是一種類似於數組的結構，用於追蹤當前正在執行的函數；
- Heap ：用來分配 `new` 創建的對象；
- Task Queue ：是用來處理非同步任務的，當該任務完成時，會指定對應回調進入佇列。

!!! info

    現代 JavaScript 引擎實作及很大程度地最佳化了該圖所描述的語意。




### 堆疊

```js
function foo(b) {
  var a = 10;
  return a + b + 11;
}

function bar(x) {
  var y = 3;
  return foo(x * y);
}

console.log(bar(7));

```

當呼叫 bar 時，會產生一個含有 bar 的參數及區域變數的 frame，而在 bar 呼叫了 foo 時，含有 foo 參數及變數的第二個 frame 就會被置於堆疊的最上面。當 foo 回傳後，最上面的 frame 會被抽離堆疊（僅留下 bar 的呼叫 frame）。然後當 bar 返回之後，堆疊就會清空。


### 阻塞

模擬一個情境：

在 C++ 中, 可能使用 `cin` 或是 `scanf` 來得到使用者的輸入, 程式碼如下：

```cpp
cin >> num;
cout << "Hello, world" << endl;
```

`cin` 會嘗試取得使用者輸入, 然後輸出 `"Hello, world"`。但是使用者輸入之前, 畫面是不會繼續渲染的, 這在交互式的 command interface 不是問題

但是到了 GUI 卻相當嚴重, 比方說登入頁面, 在你輸入帳號、密碼之前, 畫面上其他部分都停止繪製, 這不是一個好的體驗

## 概述非同步與同步

為了解決阻塞的問題，我們可以透過非同步（Asynchronous）的方式來解決

下面用一個常見的速食店的例子來說明同步與非同步的差異：

同步

1. 客人跟店員點餐
2. 店員收到你要的餐點開始製作
3. 首先炸薯條，等待薯條的時間
4. 接著去煎肉排，等待煎肉排的時間
5. 接著去倒飲料，等待倒飲料的時間
6. 最後製作漢堡並送給客人
   
店員一次只能做一件事情，開始炸薯條就必須等待薯條炸完，沒辦法利用空閑時間做其他事情。


   
非同步

1. 客人跟店員點餐
2. 店員收到你要的餐點開始製作
3. 首先炸薯條
4. 接著去煎肉排
5. 緊接著去倒飲料
6. 等待時間
7. 最後製作漢堡並送給客人

店員同樣一次也只能做一件事情，但在開始炸薯條之後他可以利用等待炸薯條的時間去煎肉排或倒飲料。


## JavaScript 引擎架構

![js-v8](/webgame-engine/assets/task-schedule/js-v8.png)

!!! quote 

    JavaScript 的並行模型（concurrency model）是基於「事件循環（event loop）」，其在運作上跟 C 或是 Java 有很大的不同。

我們之所以可以在瀏覽器中同時（concurrently）處理多個事情，是因為瀏覽器並非只是一個 JavaScript Runtime。

!!! note 
    JavaScript 的執行時期（Runtime）一次只能做一件事，但瀏覽器提供了更多不同的 API 讓我們使用，進而讓我們可以透過 event loop 搭配非同步的方式同時處理多個事項。


Event loop 的作用是去監控堆疊（call stack）和工作佇列（task queue），當堆疊當中沒有執行項目的時候，便把佇列中的內容拉到堆疊中去執行。

Task Queue 紀錄等待執行的工作, 由後方的Worker取出後執行, 完成後調用註冊的 Handler

比方說 setTimeout(fn, ms), 接受一個function和毫秒的數值, 就會在 N 毫秒後調用該方法

```js
console.log('Hello')

setTimeout(() => function cb(){
 console.log('There')
}, 5000) // 1秒後印出 'test'

console.log('Bye')
```

執行的過程可能如下：

1. setTimeout 中的 callback function 會被放到 WebAPIs 中，這時候，setTimeout 這個 function 就已經執行結束，並從堆疊中脫離
2. 當計時器的時間到時，會把要執行的 callback function 放到工作佇列（task queue）
3. 每次 Eventloop 的週期, 如果堆疊（stack）是空的，它便把佇列（queue）中的第一個項目放到堆疊當中；堆疊（stack）便會去執行這個項目。
   

證明 call stack 結束後, 才會執行 task queue 的工作如下：

```js
let arr = [];
setTimeout(() => arr.push(1), 0); // Enqueue - job thread
setTimeout(() => arr.push(2), 0); // Enqueue - job thread
setTimeout(() => arr.push(3), 0); // Enqueue - job thread
arr.push(4) // main thread
console.log(arr) // [4, 1, 2, 3]
```

該程式碼揭露的：因為前面三次push是放在job thread的, 因此狀況就好像：

```js
Call Stack = [fn, fn, fn];
Task Queue = [fn];

```

必須等到 Call Stack 清空後, 才會依序執行 Task Queue 內的工作

!!! info
    setTimeout 與 setInterval：

    1. setTimeout - 經過至少多少毫秒後, 應該調用 function
    2. setInterval - 每隔至少多少毫秒後, 應該調用 function




**setTimeout 0**

```js
console.log('Hello')

setTimeout(() => function cb(){
 console.log('There')
}, 0) // 1秒後印出 'test'

console.log('Bye')
```

1. Hello 會被放到堆疊中執行
2. 將 callback function 放到 WebAPIs 的計時器中
3. 當時間到時，把該 callback function 放到工作佇列（task queue）
4. 等到所有堆疊的內容都被清空後執行這個 callback function。


**Click Event**

```js
console.log('Started')

$.on('button', 'click', function onClick () {
  console.log('Clicked')
})

setTimeout(function onTimeout () {
  console.log('Timeout Finished')
}, 5000)

console.log('Done')
```

1. Started 被放到堆疊中執行
2. click 和 setTimeout 這兩個都是屬於 WebAPIs。將他們放到 WebAPIs 後，就會從堆疊中脫離
3. 最後執行 Done。
4. 當 setTimeout 的 Timer 時間到時，或者是 click 事件被觸發時，WebAPIs 會將 callback function 放到工作佇列中（task queue）
5. 當堆疊空掉的時候，event loop 就會把工作佇列中的內容搬到堆疊（stack）中加以執行。

!!! note 
    當我們點擊瀏覽器時，這個點擊事件的 callback function 並不是立即被執行的，而是先被放到工作佇列（queue）中，直到堆疊（stack）空了之後，才會被執行

!!! info
    這邊分享一個提供視覺化更方便瞭解整個流程的工具 [Loupe](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

