# 生成器與協程

在談論協程之前，可以先從以下的圖片了解協程(corountine)的運作觀念：

![corountine](https://www.modernescpp.com/wp-content/uploads/2021/02/FunctionsVersusCoroutines-768x412.png)

coroutine(協程, 或稱**共常式**)和Function（或稱為子程式, 或稱**次常式**）都是用於程式碼塊執行的概念，但它們在執行方式和用途上有著根本的差異。

1. **執行方式**:
    - **Function**: 當你調用一個普通函式時，程序會進入該函式執行，並在函式執行完畢後返回調用處繼續執行。普通函式只有一個進入點和一個退出點，它們按照嚴格的 LIFO（後進先出）順序運行。
    - **Coroutine**: 協程則更加靈活。它們可以在執行過程中暫停（yield），然後在之後的某個時刻從上次暫停的地方繼續執行。這意味著協程可以有多個進入點和暫停點。

2. **狀態保持**:
    - **Function**: 一旦函式執行完畢，所有的狀態都會丟失，下一次調用時會重新開始。
    - **Coroutine**: 協程可以保持它們的狀態，這意味著變數在暫停和重新進入時保持其值。

3. **用途**:
    - **Function**: 適用於大多數的序列計算，當你需要一個簡單的執行流程時。
    - **Coroutine**: 非常適合於需要協作式多任務、非阻塞 I/O 操作或當你想要更細粒度的控制執行流程時。例如，它們被廣泛用於非同步程式，特別是在現代網絡框架中。

4. **效率**:
    - **Function**: 在調用和返回時可能會涉及到堆棧操作，這在函式呼叫非常頻繁的時候可能會影響效率。
    - **Coroutine**: 由於能夠暫停和恢復，協程可以減少某些情況下的性能開銷，尤其是在涉及到等待操作，如**I/O 操作**時。

5. **控制流**:
    - **Function**: 執行完一個函式再執行下一個，控制流是線性的。
    - **Coroutine**: 可以分時執行，控制流可能更加複雜，因為它們可以在多個點**暫停和恢復**。

在其他與語言如 Python 中，`async/await` 關鍵字允許你以近乎同步的方式編寫異步代碼，這背後就是依賴於協程的概念。

Lua、Go 或是即將推廣的 C++ 20 標準，協程也是一個重要的概念，它們在這些語言的非同步編程模型中扮演著關鍵角色。

## 協作式的核心概念

無論是**協**程還是**共**常式的任一翻譯，都在強調**協同運作**的概念

而與**共同協作**相對的概念，就是**搶占運作**，一個典型的例子就是大部分多執行緒程式設計，必須處理 Mutex 或是 Semaphore 的持有權

唯在持有`Flag`的情況下，才允許某一個Thread進入到關鍵區塊執行程式。

反過來說，協程的介紹就如 Wiki 所介紹的：

> 共常式可以通過yield（取其「退讓」之義而非「產生」）來呼叫其它共常式，接下來的每次共常式被呼叫時，從共常式上次yield返回的位置接著執行，通過yield方式轉移執行權的共常式之間不是呼叫者與被呼叫者的關係，而是彼此對稱、平等的。

### 協程範例

同時，Wiki[^1] 上的範例也相當明確：

```js
/**
 * corountine.js
 */
const queue = [];
const FULL_LOAD = 5;

function* produce() {
  while (true) {
    while (queue.length !== FULL_LOAD - 1) {
      const newData = Math.random().toString(36); // 模擬產生資料
      queue.push(newData);
    }
    yield;
  }
}

function* comsume() {
  while (true) {
    while (queue.length !== 0) {
      const data = queue.pop();
      console.log(data); // 模擬使用資料計算
    }
    yield;
  }
}

const compute = produce();
const use = comsume();

compute.next();
console.log('queue:', queue);
use.next();
console.log('queue:', queue);

```

### 執行結果

```js
// after invoke `compute.next()`
queue: [
  '0.ma0if60d78h',
  '0.vdx12v43urr',
  '0.s4a8l8pxnep',
  '0.9hx7gvwewud',
  '0.c99szh55dea'
]

// after invoke `use.next()`
0.c99szh55dea
0.9hx7gvwewud
0.s4a8l8pxnep
0.vdx12v43urr
0.ma0if60d78h

// check queue
queue: []
```

在生產者-消費者的模型中，為了讓程式效率最大化，必須符合以下限制：

- 佇列用來存放資料的空間有限
- 生產者要儘量在一次執行中向佇列多增加資料
- 當滿載時，需要**退出生產函式，把程式的執行權交給消費函式**
- 消費者要儘量在一次執行中把佇列中的資料使用掉
- 當滿載時，需要**退出消費函式，把程式的執行權交給生產函式**

當條件滿足後，生產/消費函式會進行**yield**，讓出自己的執行權，該過程是主動執行的，就好像**讓出**執行權，讓其函式可以運作
因此被稱作**協作式**的工作模型

以上的例子雖然也可以使用多執行緒進行處理，但是需要考量：

- 需要至少兩個執行緒處理
- `queue` 的存取是一個關鍵區塊，需要考慮互斥鎖
- 消費者與生產者該如何互相通知彼此執行

總體而言，多執行緒的程式設計雖然可以擁有更高的效能與控制精細度，但是也會導致設計的複雜度上升

## 生成器 (Generator)

在 JavaScript 中，也有對應的語法[^2]，改語法被稱為生成器(Generator)，或是另外一個常見的名稱**迭代器**

語法在上個小節中已經有示範了，就是在 `function` 後加上 `*`：

```js
function* generator() {
  for(let i = 1 ; i <= 10 ; ++i)
    yield i ** 2;
}

const seq = generator();

for(const powerOfNum of seq) {
  console.log(powerOfNum); // 依序輸出 1 ~ 10 的平方
}
```

!!! note
    呼叫生成器函式並不會讓裡面的程式碼立即執行，而是會回傳一個針對該函式的迭代器（iterator）物件。

    當呼叫迭代器的 next() 方法時，生成器函式將會執行到遭遇的第一個 yield 運算式，該運算式給定的值將從迭代器中回傳，如果是 yield* 則會交給另一個生成器函式處理。

在該例子中，`seq` 是一個生成器物件，使用 `for-of` 進行迭代時，都是從 `generator()` 的 `yield` 表達式回傳當前 i 的平方

此例中 `for-of` 等同於

```js
for (let i = seq.next(); !i.done ; i = seq.next()) {
  console.log(i)
  console.log(i.value);
}
```

也就是說，每一次迭代，都是調用 `next()` 這個方法，他會從上一次 `yield` 的敘述恢復執行，直到遇到下一個 `yield` 或是 `return`

### 加入 Return 的生成器函數

一個更修改的執行結果：

```js
// modify example:
function* generator() {
  for(let i = 1 ; i <= 10 ; ++i)
    yield i ** 2;
  return 1001;
}
```

![generator1](/webgame-engine/assets/javascript/generator1.png)

每當調用一個 `next()`，會回傳一個包含 `value` 與 `done` 的物件

倘若在 `yield` 後沒有回傳值，則 `value` 會是 `undefined`

若函式執行完畢，`done` 會是 `true`，反之為 `false`

而遇到 `return` 敘述，`value` 會是回傳值，且`done` 被設定為 `true`

一旦 `done` 為 `true`，雖然可以繼續呼叫`next()`，但是`value`會永遠是`undefined`，且`done` 恆為 `true`

### 生成器的狀態

![generator2](/webgame-engine/assets/javascript/generator2.png)

由 Debug Tools 顯示，其實 Generator 就是在 `initial` , `suspend` , `close` 狀態中切換

!!! note
    參考 C++ 20 的 [coroutine 規格](https://en.cppreference.com/w/cpp/language/coroutines#co_yield)

    ![C++20](/webgame-engine/assets/javascript/c++coroutine.png)

    本質就是允許中斷的函式

### 從外部注入數值

在 Generator 中，我們也可以把 `yield` 看成一個特殊的關鍵字，倘若在呼叫 `next()` 中傳遞了引數

則 `yield` 數值相當於傳入的引數

```js
function* dateSteper() {
  const d = new Date();
  while(true) {
    const message = yield;
    console.log(d.toLocaleDateString(), message);
    d.setTime(d.getTime() + 24 * 60 * 60 * 1000)
  }
}

const steper = dateSteper();

steper.next();
steper.next("First Meesage");
steper.next("Second Meesage");
steper.next("Third Meesage");
```

![yield value](/webgame-engine/assets/javascript/yield_value.png)

該範例的運作流程結果如下：

1. 首次調用 `steper.next()` 時，估算右值的時候 suspend，退出函式
2. 此時尚未為抵達`console.log` 敘述，不輸出訊息
3. 第二次調用 `steper.next()` 時，恢復估算右值，帶入"First Message"後進行 console.log
4. 下一次迴圈開始，每次`next()`都會恢復估算右值，並且在下一次估算右值**前**退出
5. 可以無窮調用 `next()`，每次都會輸出下一天的日期與帶入的Message

此例中，因為是個無窮迴圈，所以回傳的 `{ value , done }` 恆為 `{ undefined , false }`

!!! tips

    請思考一下，倘若敘述改成 `const message = yield 100`，則每次輸出與回傳的`value`數值為何

[^1]: [協程 Wiki](https://zh.wikipedia.org/zh-tw/%E5%8D%8F%E7%A8%8B)
[^2]: [JavaScript Generator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
