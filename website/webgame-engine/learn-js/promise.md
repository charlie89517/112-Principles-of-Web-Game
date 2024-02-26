#非同步程式設計

一個簡單的例子，說明一下"非同步"的情境

在假日起床後，你打算做以下幾件事情：

- 刷牙洗臉 (5分鐘)
- 洗衣服 (1小時15分鐘)
- 上廁所 (15分鐘)
- 享用早餐 (25分鐘)

一早起來先盥洗後，放下衣服去洗，上個廁所，然後享用早餐

在"同步"的情況下，會發生以下的狀況：

當盥洗後去洗衣服時，即使肚子餓了也不能用早餐；亦不能去上廁所，因為**洗衣服**是個**阻塞事件**

換句話說，當執行一個 Task，且該 Task 不可被中斷 **(阻塞 Block)**，就可以粗略地說是同步程式

實際上的情況會更加複雜,因為會區分為：

- 同步 (Synchronize)
- 非同步 (Asynchronize)
- 阻塞 (block)
- 非阻塞 (non-block)
  
這裡不討論太深入，先理解第一個概念：

!!! info

    在程式中，所有的Task都是不可中斷的，就可以說是同步程式設計

## 現實中的狀況

在生活中，也有很多非同步的情境：

- 以上個例子來說，當把衣服丟進洗衣機洗後，就會離開做其他事情了
- 煮泡麵時，通常不會倒水後，還繼續等待三分鐘都不做其他事情
- 去銀行時，先抽取號碼牌，等到輪到自己的號碼，才去櫃檯
  
試想一下上面的幾個情境：洗衣服時，要在洗衣機旁等待1小時；去銀行時，要在櫃台排隊直到自己到櫃檯前...

這些都是很浪費時間的情況，而以程式設計來說，通常非同步設計會用在

- **I/O** 發生時(非常重要)
- 某個操作耗費時間甚鉅

若 CPU 進行資料的運算需要 10µs，而等待硬碟把資料傳輸到記憶體需要 1 ms

客觀來說，耗時約為 1ms + 10µs = 1.01ms；對於CPU來說，絕大多數的時間都在**等待**資料傳輸

對於網頁設計來說，經典的例子是：當網頁上有圖片需要顯示時，不會等待圖片下載完成，而是會先渲染頁面的其餘部分

---


## AJAX

政府有提供一系列的開放資料,可供查詢：[水利署](https://fhy.wra.gov.tw/WraApi#/)

我們這邊就使用台灣雨量站的API來做舉例

剛好符合即將要做的事情：透過網路從遠端取得一些資料

經由 台灣雨量站API：[https://fhy.wra.gov.tw/WraApi/v1/Rain/Station?$top=30](https://fhy.wra.gov.tw/WraApi/v1/Rain/Station?$top=30) 可以得到以下的資料：



| 雨量站所在地址 | 縣市代碼 | 緯度       | 經度       | 測站代碼 | 測站中文名稱 | 流域代碼 | 流域名稱 |
| -------------- | -------- | ---------- | ---------- | -------- | ------------ | -------- | -------- |
| 南投縣集集鎮   | 10008    | 23.8263889 | 120.775    | 00H710   | 集集(2)      | 1510     | 濁水溪   |
| 南投縣仁愛鄉   | 10008    | 24.0908333 | 121.032222 | 00H810   | 惠蓀(2)      | 1430     | 烏溪     |
| 屏東縣屏東市   | 10013    | 22.655     | 120.466    | 00Q070   | 屏東(5)      | 1730     | 高屏溪   |

倘若是將該表格做成網頁,內容可能會是：

```js 
<html>
  <body>
    <div>
      <!-- 其他資料 -->
    </div>
    <table>
      <thead>
        <tr>
          <th>雨量站所在地址</th>
          <th>縣市代碼</th>
          <th>緯度</th>
          <th>經度</th>
          <th>測站代碼</th>
          <th>測站中文名稱</th>
          <th>流域代碼</th>
          <th>流域名稱</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>南投縣仁愛鄉</td>
          <td>10008</td>
          <td>24.090833333333332</td>
          <td>121.03222222222222</td>
          <td>00H810</td>
          <td>惠蓀(2)</td>
          <td>1430</td>
          <td>烏溪</td>
        </tr>
        <tr>
          <td>屏東縣屏東市</td>
          <td>10013</td>
          <td>22.654999999999998</td>
          <td>120.46638888888889</td>
          <td>00Q070</td>
          <td>屏東(5)</td>
          <td>1730</td>
          <td>高屏溪</td>
        </tr>
      </tbody>
    </table>
    <div>
      <!-- 其他資料 -->
    </div>
  </body>
</html>

```

這個假設的網頁，可能還包含了該表格以外的資料，使用 <!-- 其他資料 --> 替代，

假設上面的表格是會更新的(Ex. 每 30 分鐘一次)，每次都需要重新要求整個頁面，是很浪費效能的

因為用戶只關心**會變化的資料**，比方說上面的表格

在之後，會說明什麼是**RestAPI**，首先知道：

!!! info 

    WEB 應用常常依賴伺服器的資料,且這些資料在網頁上**可能**會常常變化

## 早期的實現

在過去 YAHOO 帳號還很流行的時候，許多人都會去辦一組信箱：

流程如下：

- 輸入一個帳號名稱
- 輸入你的姓名、基本資料
- 送出表單
- 喔，你有可能帳號名稱跟別人重複了、或是密碼不符合格式(比方說要包含大小寫英數字)
- 重新填寫表單
- 在隨後幾年(2010)，Google 進入大家的生活，同樣的流程：

輸入一個帳號名稱
準備輸入你的姓名、基本資料
**已經知道該帳號有沒有被註冊過了**
繼續填寫其他項目
若表單有錯誤，進行修正
提交申請表單
這在現今很常見的技術，由 Google 開始大量使用的技術之一 - AJAX

早在 Google 使用該方法之前，早就有這項技術，叫做`Asynchronous JavaScript And XML(AJAX)`

平常使用的網頁，其實大部分的畫面是固定的，僅有一小部分會變化，比方說：

- 圖書館館藏系統：只有搜尋結果的部分會改變
- 帳號註冊系統：表單都是一樣的，只是要檢查帳號、密碼合不合格
- Youtube：搜尋影片時，只有下方的影片清單會改變
  
諸多應用，因此提出一個概念：**能不能只交換需要的部分？**，或是先提交**部分資料**給伺服器進行處理

因為早期使用XML做為資料傳輸的格式(近幾年大部分使用JSON)，所以稱為 AJAX

概念如下：透過背景發起 Network I/O，並等到伺服器回應後，再把資料取出來使用，實現的程式碼如下

```js
const domain = 'fhy.wra.gov.tw';
const apiPath = 'WraApi/v1/Rain/Station';
const query = '$top=30';

const targetUrl = `https://${domain}/${apiPath}?${query}`;

let xhr = new XMLHttpRequest();

/* xhr.open(method, url) 以特定的HTTP方法開啟某個網址 */
xhr.open('get', targetUrl);

/* 當資料完成後,要做什麼事情 */
xhr.onload = function(e) {
  console.log(xhr.responseText);
}
/* 接近等效的程式碼：
xhr.addEventListener('load', e => {
  console.log(xhr.responseText);
}) */

/* 送出請求 */
xhr.send();

```

看到 `onload` 成員，當完成後，會發送一個事件，通知程式去把資料取出來

## 現在主流的做法

在ES 6(ECMA 2016)之後，推出了一系列的API，其中包含影響甚鉅的 `Promise`

而ES 7之後，則推出了 `async/await` ，更方便進行處理非同步的資料 

## Promise

### 關於Promise

從語法上講：Promise 是一個物件，而此物件代表一個即將完成、或失敗的非同步操作，以及它所產生的值。

從本意上講：它是保證，保證它過一段時間會給你一種結果



Promise有三種狀態：

- pending（等待）
- fulfilled（成功）
- rejected（失敗）


!!! note

    Promise 狀態一旦改變，就不會再變，創造 promise 實例後，他會立即執行

### Promise 的特點

1. promise的狀態不受外界的影響，就像我開頭說的是一個容器，除了非同步操作的結果其他手段無法改變 promise 的狀態。
2. 狀態一旦改變就不會改變，任何時候都會得到這個結果，狀態改變有兩種： 從 pending 變為 fulfilled 和從 pending 變為 rejected.


### Prmoise的使用

用實際的例子來說明,首先是 Promise 的函式簽章：

```js
function executor( resolve, reject ) {
  /* do something */
}

let promise = new Promise( executor );

```

`executor` 的型別是 Function，並接受兩個參數 `resolve` 和 `reject`，兩個參數都是 `function`

`resolve`：當操作成功，應該調用該方法

`reject`：當操作失敗，應該調用該方法

!!! info
    在部分程式設計書籍的說法，傳入一個 Function，被傳入的 Function 習慣稱做 callback 或是 handler

    並且稱接受/回傳一個 Function 的 Function 為 High-order Function(高階函式)

以該例中：

```js
function calc(callback) {
  let a = Math.random() * 100;
  let b = Math.random() * 100;
  return callback(a, b);
}

function add(a, b) {
  return a + b;
}

function mul(a, b) {
  return a * b;
}

calc(add); // return `Math.random() * 100` + `Math.random() * 100` 的值
calc(mul); // return `Math.random() * 100` * `Math.random() * 100` 的值

```

呼叫 calc 時，calc 內部會生成兩個隨機數字 `a`， `b`，並調用 `callback` 參數，該參數接受一個 Function

add 和 mul 這兩個被傳入的 function，通常叫做 `callback`

另一個例子，滑鼠點擊事件的函數簽章：

```js
htmlElement.addEventListener("click", (e) => {
  console.log(e);
});


```

`addEventListener` 接收兩個參數：第一個是事件種類，常用的有 `click`， `change`， `load` ... 等

第二個參數則是一個 `handler`，把事件物件傳給 `handler`，供 `handler` 使用

那麼回到 `Promise`，可以理解成 `Promise` 內部會生成兩個 `callback` 供使用

根據調用的 callback，決定 `Promise` 的狀態是成功的還是失敗的：

```js
let promise = new Promise((resolve, reject) => {
  const value = Math.random() * 1000;
  if(value > 500)
    resolve(value);
  else
    reject(value);
});

```

`new Promise` 回傳的實例，會提供 `then` 或是 `catch` 方法，分別對應 `resolve` 和 `reject` ：

```js
promise
  .then(value => console.log(value))   // 當 resolve 被調用時,進入該函式
  .catch(value => console.log(value)); // 當 reject 被調用時,進入該函式

```

這樣理解 Promise：一個未來會存在的數值，且狀態確定後，就不會改變了

狀態不會改變的意思是：


```js
let promise = new Promise((resolve, reject) => {
  const flag = true;
  resolve(true);
  reject(false); // 無效,已經呼叫了 resolve
});

promise
  .then((value) => console.log(value)) // print 'true';
  .catch((value) => console.log(value)); // 不會執行

```

這個就是 Promise 不變性的意思：

- 一開始處於 pending 狀態：還未調用 `resolve` 或是 `reject` 之前, 都處於該狀態
- 當 resolve 調用後：成為 fulfilled(實現) 狀態
- 當 reject 調用後：成為 rejected(拒絕) 狀態
Promise 一旦被決定是 fulfilled 還是 rejected 後, 就不會變成其他狀態了

而 Promise 只會被決定**一次**狀態,意思是：

```js
let promise = new Promise((resolve, reject) => {
  const flag = true;
  resolve(true); // 在該階段,Promise 成為 fulfilled 狀態
  resolve(false); // 無效,Promise的狀態已經被決定了
});


```

而 then 和 catch 的回傳值,會成為下一個 Promise 的值：


```js

let promise = new Promise((resolve, reject) => {
  resolve(10); // 必定會成功的 Promise
});

promise
  .then(value => {
    console.log(value); // print 10
    return value * 100
  })
  .then(value => {
    console.log(value); // print 1000
  });

```

Promise的出現，為帶來了一個重要的進展 - 可以針對非同步事件進行排序

以上個個章節的例子，要下載 fileA， fileB， fileC，且一定要依照A B C的順序

在上個章節，用`arr[0]、arr[1]、arr[2]`分別存入A、B、C的值，但是使用 Promise 後，可以改為：

```js
function download(url) {
  return new Promise( (resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('get', url);
    xhr.addEventListener('load', () => {
      resolve(xhr.responseText) // 下載完成後,調用 resolve
    })
    xhr.addEventListener('error', e => {
      reject(e.message) // 若失敗,調用 reject
    });
    xhr.send() //送出請求
  })
}


function downloadAll() {
  download(siteA)
    .then(data => {
      console.log(data);
      return download(siteB);
    })
    .then(data => {
      console.log(data);
      return download(siteC);
    })
    .then(data => {
      console.log(data);
    });
}

downloadAll() //依序呼叫 siteA、siteB、siteC 的下載內容

```



### 更深入的Promise 

#### 串接

Promise 有個特性：他可以類似串列一般，把本次的回傳值做為下一個 promise 的傳入

```js
let promise = new Promise((resolve, reject) => {
  const flag = Math.random() > 0.5; // Math.random() 會隨機回傳 0~1 之間的數字
  if (flag) {
    resolve(1, 2, 3, 4, 5); // 僅接受第一個參數
  } else {
    reject(10, 20, 30); // 僅接受第一個參數
  }
});

promise
  .then((a, b, c, d, e) => {
    console.log(a, b, c, d, e); // print 1, undefined * 4
    return 100;
  })
  .catch((a, b, c) => {
    console.log(a, b, c); // print 10, undefined * 2
    return -100;
  })
  .then((value) => {
    // 如果 flag 為 true,代表進入上一個 then,此時 value = 100
    // 反之 flag 為 false,代表進入上一個 catch,此時 value = -100
    console.log(value);
    return 10000;
  })
  .finally((e) => {
    // 可以調用 finally(),代表不論在 then 還是 catch 都要執行的事件
    console.log(e); // undefined,finally不接受任何參數
  });


```

在then當中可以傳入兩個 callback 分別為：

- `onFulfilled`：成功的值
- `onRejected`：失敗的原因

![js-promises](/webgame-engine/assets/promise/js-promises.png)

這裡引用 MDN 的 Promise 流程圖：起初在 `pending` 狀態,接下來根據 `fulfill` 或是 `reject`,調用 `onFulfillment` 或是 `onRejection`,此時就被稱為 `settled` 狀態

值得注意的地方是，可以看到其實 `then()` 是可以接受兩個 callback：


```js
const invokeFn = () => Promise.reject("oops!")

/* Example 1 */
invokeFn()
  .then(
    () => console.log("onFulfillment"),
    reason => console.log(`onReject ${reason}`)
  )
  .catch(
    reason => console.log(`ErrorCatch, ${reason}`)
  );

/* Example 2 */
invokeFn()
  .then(
    () => console.log("onFulfillment"),
  )
  .catch(
    reason => console.log(`ErrorCatch, ${reason}`)
  );

```

在舊一點的實作中，會特意把 `fulfill`、`reject`、`error` 三種情況分開

Ex. 當呼叫伺服器的API時，可能會發生：

- `200 OK` - 伺服器收到請求並允許
- `403 Forbidden` - 伺服器收到請求並拒絕
- 無回應 - 完全無回應， 可能是伺服器壞掉， 或是該站點根本不存在

對於客戶端來說， `onFulfillment` 對應到 `status 200`、`onReject` 對應到 `status 403`，最後 `onCatchError` 對應到伺服器無回應

- Promise.resolve(val) 回傳一個進入 `fulfill` 狀態的 Promise 物件
- Promise.reject(val) 回傳一個進入 `reject` 狀態的 Promise 物件

流程圖的第三階段，無論是 `then` 還是 `catch` 方法，都會會傳一個新的 `Promise` 物件



#### 進階練習

這就如上方的 downloadAll 例子，每一次的 then 都會回傳一個新的 Promise 物件，且 Promise 只會被決定一次狀態，因此可以提出兩種變體：

首先定義一個**模擬下載** 的 Promise 函式，接受兩個值：val 以及 isSuccess

```js
/* val 設定成當 Promise settled 時,應該回傳的值 */
/* isSuccess 則決定,該 Promise 的狀態是 `fulfill` 還是 `reject` */
const download = (val, isSuccess = true) => {
  if(isSuccess) {
    return Promise.resolve(`Fulfill: ${val}`);
  } else {
    return Promise.reject(`Reject: ${val}`);
  }
}

download("data A")
  /* stage 1 */
  .then(data => {
    console.log(`Savepoint 1: ${data}`);
    return download("data B");
  })
  .catch(err => {
    console.log(`Savepoint 2: ${err}`);
    return download("error-data B");
  })
  /* stage 2 */
  .then(data => {
    console.log(`Savepoint 3: ${data}`);
    return download("data C");
  })
  .catch(err => {
    console.log(`Savepoint 4: ${err}`);
    return download("error-data C");
  })
  /* stage 3 */
  .then(data => {
    console.log(`Savepoint 5: ${data}`);
  })
  .catch(err => {
    console.log(`Savepoint 6: ${err}`);
  })

```

簡單的拆解一下，理清這個範例的執行結果：

在第一次呼叫 download 時，第二個參數 `isSuccess` 為 true，因此該次執行結果是 `fulfill`

```js
download("data A") // fulfill

```

此時會經過 `Savepoint 1`，並印出 "Savepoint 1: Fulfill: data A"

下一行的 `download("data B")` 也是 `fulfill`，因此會略過 catch，進入到 `stage 2` 的 `Savepoint 3`，並印出 "Savepoint 3: Fulfill: data B"

同樣的，最後則會走到 `stage 3` 的 "Savepoint 5"，並印出 "Savepoint 5: Fulfill: data C"

最終輸出：

```js
Savepoint 1: Fulfill: data A
Savepoint 3: Fulfill: data B
Savepoint 5: Fulfill: data C

```

!!! tip

    尋找離當下 `Promise` 最近的 `then` 和 `catch`，再根據 settled 的狀態決定路徑

下面的例子，把"看不到"的路徑，先註解起來

```js
download("data A") // <--- 目前執行的位置
  /* stage 1 */
  .then(data => { // <--- 最近的 then
    console.log(`Savepoint 1: ${data}`);
    return download("data B");
  })
  .catch(err => { // <--- 最近的 catch
    console.log(`Savepoint 2: ${err}`);
    return download("error-data B");
  })
  /* stage 2 */
  // .then(data => {
  //   console.log(`Savepoint 3: ${data}`);
  //   return download("data C");
  // })
  // .catch(err => {
  //   console.log(`Savepoint 4: ${err}`);
  //   return download("error-data C");
  // })
  /* stage 3 */
  // .then(data => {
  //   console.log(`Savepoint 5: ${data}`);
  // })
  // .catch(err => {
  //   console.log(`Savepoint 6: ${err}`);
  // })

```
該次結果是成功，因此會進到 then，此時在 `Savepoint 1`：
```js
// download("data A") 
  /* stage 1 */
  .then(data => { 
    console.log(`Savepoint 1: ${data}`);
    return download("data B"); // <--- 目前執行的位置
  })
  .catch(err => { // <--- 最近的 catch
    console.log(`Savepoint 2: ${err}`);
    return download("error-data B");
  })
  /* stage 2 */
  .then(data => { // <--- 最近的 then
    console.log(`Savepoint 3: ${data}`);
    return download("data C");
  })
  // .catch(err => {
  //   console.log(`Savepoint 4: ${err}`);
  //   return download("error-data C");
  // })
  /* stage 3 */
  // .then(data => {
  //   console.log(`Savepoint 5: ${data}`);
  // })
  // .catch(err => {
  //   console.log(`Savepoint 6: ${err}`);
  // })

```

這次結果也是成功，因此會進到 then，此時在 `Savepoint 3`：

```js
// download("data A") 
  /* stage 1 */
  // .then(data => { 
  //   console.log(`Savepoint 1: ${data}`);
  //   return download("data B");
  // })
  // .catch(err => {
  //   console.log(`Savepoint 2: ${err}`);
  //   return download("error-data B");
  // })
  /* stage 2 */
  .then(data => { 
    console.log(`Savepoint 3: ${data}`);
    return download("data C"); // <--- 目前執行的位置
  })
  .catch(err => {
    console.log(`Savepoint 4: ${err}`);
    return download("error-data C");  // <--- 最近的 catch
  })
  /* stage 3 */
  .then(data => { // <--- 最近的 then
    console.log(`Savepoint 5: ${data}`);
  })
  // .catch(err => {
  //   console.log(`Savepoint 6: ${err}`);
  // })

```
最後的結果還是成功， 因此會進到 then，此時在 `Savepoint 5`：

修改範例， 比方說在 `stage 1` 的 `then` 扔出一個 Error：

```js
download("data A")
  /* stage 1 */
  .then(data => {
    console.log(`Savepoint 1: ${data}`);
    throw "Something wrong" // <------- 加入該行
    return download("data B");
  })
  .catch(err => {
    console.log(`Savepoint 2: ${err}`);
    return download("error-data B");
  })
  /* stage 2 */
  .then(data => {
    console.log(`Savepoint 3: ${data}`);
    return download("data C");
  })
  .catch(err => {
    console.log(`Savepoint 4: ${err}`);
    return download("error-data C");
  })
  /* stage 3 */
  .then(data => {
    console.log(`Savepoint 5: ${data}`);
  })
  .catch(err => {
    console.log(`Savepoint 6: ${err}`);
  })

```

此時的輸出順序就會是

```js
Savepoint 1: Fulfill: data A
Savepoint 2: Something wrong
Savepoint 3: Fulfill: error-data B
Savepoint 5: Fulfill: data C

```

更多資料請參考 [MDN - Promise](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Promise)

#### fetch API

ES 6 提供了 `fetch` API，就像是上面的 `download` 的實作，只是是由瀏覽器提供的WebAPI：

```js
const result = fetch(url, {
  method,  // HTTP Method, default 為 get
  headers, // HTTP 表頭, default為null
  body,     // 內容, default為null
  ...moreOptions
});

result
  .then(e => {
    return e.json() //把資料以JSON格式解讀
  })
  .then(json => {
    console.log(json)
  });

```
這就是最常用來抓取伺服器資料的方法，比方說上面那個抓取政府運輸資料的程式可改為：

```js
const domain = 'fhy.wra.gov.tw';
const apiPath = 'WraApi/v1/Rain/Station';
const query = '$top=30';

const targetUrl = `https://${domain}/${apiPath}?${query}`;

fetch(targetUrl)
  .then(res => res.json())


```

執行結果

![response](/webgame-engine/assets/promise/response.png)

!!! note

    如果請求的站點出現`404 NOT FOUND`，那麼當次 fetch 的狀態是 **fulfilled**

    因為 fetch 象徵的意義是對伺服器**發出請求**，而不是取得資料，而404 status一樣是伺服器的回傳結果

```js
fetch("httpp://www.google.com")
  .then(d => {
    console.log(true);
  })
  .catch(err => {
    console.log("Error")
  }) //進入 catch

```

這個例子中，誤把 `http` 打成 `httpp`，一個未知的協定，因此無法發出請求導致直接進入 catch 階段

```js
const domain = 'fhy.wra.gov.tw';
const apiPath = 'WraApi/v1/Rain/Station';
const query = '$top=30';

const targetUrl = `https://${domain}/${apiPath}?${query}`;

fetch(targetUrl)
  .then(res => {
    return res.text(); // 這次不使用 json(),而是使用 text() 取得未 paese 的內容
  })
  .then(content => {
    console.log(content);
    return JSON.parse(content+'}');
  })
  .catch(err => {
    // 會進入這裡,因為在 content 後加上 '}',導致無法順利解析成JSON格式
    console.log("Parse error")
  })

```

而上述的例子中，可以觀察到在 then 或是 catch 中 `throw Error`，會進入下個階段的 catch

更多資料請參考 [MDN - fetchAPI](https://developer.mozilla.org/en-US/docs/Web/API/fetch)





## async 與 await

### 介紹

Promise 雖然改善了 callback hell 的發生，但其實還是有一層的巢狀結構，而此時 `async/await` 的出現可以為我們解決這個問題。

`async function`：在 `function` 的前方加上一個 `async` 關鍵字，來指示該函式成為非同步函式。讓其內部以”同步的方式運行非同步“程式碼。

`await`：可以暫停非同步函式的運行（中止 Promise 的運行），直到非同步進入 resolve 或 reject，當接收完回傳值後繼續非同步函式的運行。


promise、then 寫法的程式碼，以 async 函式改寫方式如下：

Promise:

```js
function getData() {
  sendRequest()
    .then((rawData) => {
      return rawData.json();
    })
    .then((data) => {
      console.log(data)
    })
}

```

async/await:

```js
async function getData() {
  const rawData = await sendRequest();
  const data = await rawData.json();
  console.log(data);
}
```

###  特色

非同步函式有兩個特色：

1. 回傳值**永遠**都是 Promise 物件
2. 允許使用 await

回傳值永遠都是 Promise 物件的意思是, 不論 return 什麼值, async function 都會包裝成 Promise

```js
async function add(a, b) {
  return a + b;
}

let result = add(10, 20); // Promise Object, [[value]] = 30

result.then(value => console.log(value)) // print: 30
```

特色1：指示某個 function 是非同步事件，所以使用Promise封裝，無法直接使用 `console.log` 取得其值。

重點是特色2

await 可以取出 Promise 最後的回傳值,比方說：

```js
function return100() {
  return new Promise((resolve, reject) => resolve(100));
}

// 正常使用：
return100()
  .then(data => console.log(data)) // print 100


// 在 async function 中使用 await
async function get100() {
  const result = await return100();
  console.log(result) // print 100
}

```

async function 可以讓的非同步程式"看起來"像同步程式

如果以上面那個download A、B、C 的例子,就可以改成

```js
function download(url) {
  return fetch(url)
          .then(res => res.text());
}

async function processData() {
  const dataA = await download(siteA);
  const dataB = await download(siteB);
  const dataC = await download(siteC);
}

```

### 錯誤的處理

await 對應到 then 方法, 那麼 catch 呢？

直接使用 try { ... } catch { ... } 即可

```js
async function processData() {
  try {
    const dataA = await download(siteA);
    const dataB = await download(siteB);
    const dataC = await download(siteC);
  } catch {
    console.log("Download Failed");
  }
}

```

這就是 async/await 的使用方法

更多資料請參考 [MDN - async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

!!! note
    這裡花了極大的篇幅在解釋非同步程式設計,以及 Promise 物件的使用方法

    本章節可以說是 最重要的 概念,請務必深入理解 Promise 的概念

    非同步事件普遍存在於 WEB 與伺服器應用中

!!! info 

    **延伸補充：巢狀地獄(Callback Hell)**

    在早期，callback 常被拿來作為解決非同步型的的一種方法，通常都會在 callback 中嵌套另一個 callback 來達到非同步**依序**完成的目的

    但經過一層一層的嵌套後往往會導致程式碼難以閱讀以及維護的情況，就被稱為巢狀地獄

    而 Promise Chain 可以改善多層巢狀結構的問題，利用`then()`來串接，大幅地降低巢狀結構的層數，但還是會使用到 callback 。

    所以使用 `async/await` 可以更好的解決使用 callback 的問題。

    





