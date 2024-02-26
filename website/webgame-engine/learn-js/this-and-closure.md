# This 與 閉包

## 關於This

!!! quote 
    
    this 值由被呼叫的函式來決定。它不能在執行期間被指派，每次函式呼叫調用的值也可能不同

    this 是一個特殊的值，無法於執行期間被覆蓋

    在 **嚴格模式** 與 **非嚴格模式**下有所不同

### This的查找

對於一般的 function，查找 this 的範圍會從調用者(caller)往上查找：

```javascript
"use strict";
let obj = {
  prop: 300,
  fn: function () {
    return this.prop;
  },
};

function outerFn() {
  return this.prop;
}

obj.fn(); // 300;

obj.fn2 = outerFn;

obj.fn2(); // 300, 因為此時 outerFn 繫結於 obj 的成員位址

outerFn(); // Error, this 並沒有 prop 這個成員

```
`this`的判斷，是先依照是不是作為某個物件的屬性或方法被調用

在上述的例子中，因為 `outerFn` 是直接以一個 function 被調用，而不是某個物件底下的方法，所以 `this` 的值為 `undefined`

但經過 `obj.fn2` 指向 `outerFn`，此時 `obj.fn2` 同樣是調用 `outerFn`，但是是以 `obj`的成員被調用

因此 `this` 的值相當於 `obj`

!!! tip

    `this` 的查找優先以上層物件的調用為主

舉例來說：

```js
function fn() {
  return this.prop;
}

let obj = {
  prop: 100,
  foo: fn,
  sub: {
    prop: 200,
    foo: fn
  }
}

obj.foo();  // 100, 因為此時是以 "obj" 的成員被調用, this = obj.sub

obj.sub.foo();  // 200, 因為此時是以 "obj.sub" 的成員被調用, this = obj.sub

// 修改obj.sub為
obj.sub = {
  foo: fn
}
obj.sub.foo() // undefined, 此時同樣以 "obj.sub" 的成員被調用, 但是 obj.sub 已經不存在 prop 屬性了

obj.sub.__proto__.prop = 300;
obj.sub.foo() // 300, 最直接的引用是 obj.sub, 該物件沒有prop成員, 但是原型鏈(__proto__)存在prop成員

```

!!! danger

    在上述的例中，通過 **proto** 屬性直接修改一個物件的原型(Prototype)

    但真正開發中，直接改變物件的原型是一件很不建議的事情，同時也會影響到所有參照原型的實例


### This的綁定

五種this的綁定方式

1. 預設綁定（Default Binding）
2. 隱含綁定（Implicit Binding）
3. 明確綁定（Explicit Binding）
4. new 綁定（new Binding）
5. 箭頭函式綁定

**隱含綁定：**

當函式作為對象的方法被調用時，this指向該物件

```js
const obj = {
  name: 'Alice',
  sayName() {
    console.log(this.name);
  }
};

obj.sayName(); // 輸出 "Alice"

```

**明確綁定：**

使用 `call`、`apply`、`bind`，明確指出要綁定給 this 的物件。

```js
function sayName() {
  console.log(this.name);
}

const obj1 = {name: 'Alice'};
const obj2 = {name: 'Bob'};
const obj3 = {name: 'Kevin'};

sayName.call(obj1); // 輸出 "Alice"
sayName.apply(obj2); // 輸出 "Bob"
sayName.bind(obj3)(); // 輸出 "Kevin"

```

**`new` 綁定：**

使用 `new` 運算符生成構造函式時，this 指向新創建的物件。

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}
const person = new Person('Alice', 20);
console.log(person.name); // 輸出 "Alice"
```


**箭頭函式綁定：**

`箭頭函式中的 this 指向，始終指向箭頭函式的上下文`和調用方式無關。

```js
const obj = {
  name: 'Alice',
  sayName() {
    const innerFunc = () => {
      console.log(this.name);
    };
    innerFunc();
  }
};

obj.sayName(); // 輸出 "Alice"

```

## This指向

!!! quote
    在函式執行的過程中，`this` 一旦被確定，那就不能更改了

**全域中的`this`**

-  嚴格模式下：全域中的 `this` 指向 `undefined` ，並非全域物件
   
```js
'use strict';

console.log(this === undefined); // 輸出 true

```

- 非嚴格模式下：全域中的this指向全域物件，瀏覽器環境下是 `window`，在 Node.js 環境中，全域物件是 `global` 物件。


**函式中的`this`**


!!! note

    當函式作為方法調用時，this 指向調用該方法的函式

    當函式作為函式調用時，this 指向全域物件

    當函式被使用運算子調用時，this 指向新創建的物件

    當箭頭函式被調用時，this 指向箭頭函式的執行上下文

箭頭函式下，this 的指向固定為箭頭函式的上下文，也就是箭頭函式外層的執行上下文中的 this，不會根據函式的調用方式決定


## 箭頭函式(Arrow Function)

箭頭函式表示法：

```js
// ex.1
const sum = (a, b) => {
  return a + b;
};

// ex.2 當 `=>` 後接的是 expression 時, 可以當作回傳值
const sum = (a, b) => a + b // 行為同 ex.1

// ex.3
const sayHello = (name) => `Hello, ${name}`

// ex.4
const sayHello = name => `Hello, ${name}` // 只有一個參數時, 可以省略()

// ex.5
const returnObj = ( user ) => ({
  name: user.first + ' ' + user.last,
  age: user.age,
})

// 倘若使用 user => {}, 此時的 {} 會被視作 block statement, 使用 ({}) 則視為 object expression

```

### 箭頭函式對this的影響

this 對於一般函式來說，this 有幾種可能值：

- 作為 new 建構子來說，this指向物件本身
- 對於 strict mode 下直接調用函式，函式中的 this 是 `undefined`
- 作為物件的方法呼叫時，參考至物件上
  
而 arrow function () => {} 的行為，是基於詞法域(lexical)，而非語法語境(context)

!!! info

    箭頭函式當中的 this 綁定的是是定義時的物件，而不是使用時的物件。也就是說，在箭頭函式中，this 指稱的對象在被定義時就固定了，而不會隨著使用時的脈絡而改變。

    且`function a() {}` 以及 `let a = () => {}` 絕對是不同的東西

!!! note 

    要快速釐清 arrow function 與 一般 function 的使用時機時：

    使用 `function() {}` 宣告的時機：

    1. 在物件中，方法要參照物件本身
    2. 在類別中，宣告成員函式的情景
    3. 使用到 Generator `function*` 的情況
    4. 使用 arguments 的情況
   
    除此之外，都可以直接使用`() => {}` Arrow Function 的形式來宣告函式

    但原則上來說， 盡可能使用展開運算替代 arguments，因此動態參數的情況，也可以使用 arrow function

## 閉包

### 語法域與詞法域

通俗的解釋，語法域代表的是執行期間動態決定的行為，比方說普通函式的`this`、建構式的 `super`

而詞法域代表的是封閉範圍的前後文，如同變數的查找一樣，舉例來說：

```js
{
  // -- block 1
  let a = 100;
  let b = 200;
  {
    // -- block 2
    let c = 300;
    function fn() {
      // -- block 2.1
      console.log(a, b, c);
    }
    fn(); // 100, 200, 300
  }
  {
    // -- block 3
    function fn() {
      // -- block 3.1
      console.log(a, b, c);
    }
    fn(); // 錯誤, c 不存於該 block 3.1 以及 block 3
  }
}

```

a, b 在同一個 block，而 c 在的 block 可以看到外部(block 1)
所以第一個 block 2 可以看到 a, b, c, 但是第二個僅能看到 a, b

這就是詞法域(其行為依照原始碼的樣子), 比較編譯器領域的說法是：Token 被宣告的位置

而閉包則複雜一點，以上面的例子來說，可以觀察出：
- block 允許巢狀
- 內部的 block 可以存取外部的 block
- 外部的 block 不可以存取內部的 block

### 閉包的概念

!!! quote

    當一個函式在訪問它所在的詞法環境之外的變數時，就形成了閉包。簡單來說，閉包就是一個函式能夠訪問其父級作用域中的變數


一個閉包通常由兩個部分組成：`函式本身` 和 `創建函式時的作用域`。

如果在函式內部定義了一個函式，並且這個函式訪問了父級函式的變數或參數，那麼這個內部函式就會形成一個閉包，因為它需要在父級函式執行完畢後，仍然能夠訪問到父級函式中的變數或參數。



!!! info

    **閉包的特點**

    1. 捕獲父函式作用域中的變數或參數，並在子函式返回後仍然保留對這些值的引用。
    2. 訪問父級函式的變數，即使父級函式已經返回，這就是所謂的“記憶效應”。
    3. 擴展函式作用域，允許外部訪問內部函式的作用域。這種能力使得 JavaScript 中可以使用很多強大的程式模式，例如模組化設計和函式柯里化。
    4. 可能導致記憶體洩漏和性能問題。如果閉包長時間持有對大型數據結構的引用，則可能會導致記憶體洩漏。此外，閉包可能會對程序性能產生一定程度的影響，因為每個閉包都需要維護一個作用域鏈。

!!! note

    **閉包的優缺點**

    優點：

    1. 封裝變數和方法，避免全局汙染和命名衝突，並提高代碼的可維護性和安全性。
    2. 創建私有作用域。從而限制外部訪問內部變數和方法的能力。
    3. 延長變數生命週期。將變數的生命週期延長到函式執行完成後，使其在函式外部仍然可被引用。
    4. 支持函式式編程，例如高階函式、柯里化和函式組合等。
    5. 可以使用在非同步編程中，閉包可以捕獲非同步操作期間所需的上下文和狀態，並在操作完成後提供回調。

    缺點：

    1. 記憶體洩漏。如果閉包長時間持有對大型數據結構的引用，則可能會導致記憶體洩漏。
    2. 性能問題。由於每個閉包都需要維護一個作用域鏈，它們可能會對程序性能產生一定程度的影響。
    3. 難以理解和除錯。複雜的嵌套函式和閉包嵌套可能會使代碼難以理解和除錯，因此需要小心使用。
    4. 容易出錯。錯誤使用閉包可能會導致應用程式中的 bug 和安全問題。
    5. 代碼複雜。使用閉包可能會增加代碼的複雜性，使其更難以閱讀和理解。

### 閉包的應用場景

**封裝變數**

使用閉包封裝全局變數，能防止變數汙染，透過創建一個函式並在其中定義變數來完成，然後將該函式返回為內部函式。透過這種方式，內部函式可以訪問外部函式的變數，但外部函式的變數不會暴露給全局範圍。

```js
function createCounter() {
  let count = 0;
  function increment() {
    count++;
    console.log(count);
  }
  return increment;
}
let counter = createCounter();
counter(); // 輸出 1
counter(); // 輸出 2
```

創建 `createCounter` 函式，該函式定義變數 `count` 和一個內部函式 increment。當我們調用 `createCounter` 並將其返回值賦給 counter 變數時，counter 變數實際上保存了 increment 的引用以及對 `count` 變數的引用。由於 `count` 變數是在 `createCounter` 函式內部定義的，因此它不會被其他代碼所訪問。

每次調用 `counter` 函式時，它都會執行 increment 中的代碼，並在控制台輸出當前計數器的值。由於 increment 函式捕獲了 `count` 變數，因此它可以增加計數器的值並在每次調用時輸出正確的結果。

**模組封裝**

利用閉包封裝了私有變數和方法，以便可以在需要時安全地公開公共介面。

```js
let myModule = (function () {
  let privateVariable = "I am private";

  function privateFunction() {
    console.log("This is a private function.");
  }

  return {
    publicVariable: "I am public",
    publicFunction: function () {
      console.log("This is a public function.");
    },
    accessPrivate: function () {
      console.log(privateVariable);
      privateFunction();
    },
  };
})();

console.log(myModule.publicVariable); // 輸出 "I am public"
myModule.publicFunction(); // 輸出 "This is a public function."
myModule.accessPrivate(); // 輸出 "I am private" 和 "This is a private function."

```
自調用函式表達式（Immediately Invoked Function Expression, IIFE）創建一個立即執行的函式，並使用閉包創建一個模組。在模組中，定義了一個私有變數 privateVariable 和一個私有方法 privateFunction。然後，我們返回一個具有公共變數和方法的對象字面量，以便可以在需要時從外部訪問它們。最後，我們將對象字面量賦給 myModule 變數，以便可以在其他代碼中使用它。

可以從外部訪問公共變數和方法，而不能直接訪問私有變數和方法。這樣可以確保私有狀態不會被意外修改或洩漏，並提高了代碼的可維護性和安全性。
