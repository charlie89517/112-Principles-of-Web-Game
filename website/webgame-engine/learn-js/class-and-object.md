# 類別與物件

## ES5 以前的類別實現

先來看看 ES5 以前的類別是如何實現的：

```js
/* Class Example */
function Person(height, weight) {
  if (!(this instanceof Person))
    // No
    throw new Error("is constructor");
  this.height = height;
  this.weight = weight;
}

Person.prototype.calcBmi = function () {
  return this.weight / ((this.height * this.height) / 10000);
};

let p1 = new Person(150, 70);
let p2 = new Person(181, 75);
p1.calcBmi(); // 31.11111111111111
p2.calcBmi(); // 22.89307408198773
p1.calcBmi == p2.calcBmi; // true
```

這裡定義 `Person` 型別, 並宣告了 `calcBmi` 方法的實作。

當宣告了一個物件時, 便可以同時定義他的 `prototype`

當調用 function 的順序(e.g. `p1.calcBmi`)時, 會依序 [原型鏈](/webgame-engine/learn-js/dig-deep/#_3) 查找

`p1.calcBmi` -> `Person.prototype.calcBmi` -> `Object.prototype.calcBmi`

!!!question

    若回傳一個包含函式的物件, 其行為會與定義 Class 一樣嗎？

```js
/* Class-like Example */
function Person(height, weight) {
  return {
    height: height,
    weight: weight,
    calcBmi: function () {
      return this.weight / ((this.height * this.height) / 10000);
    },
  };
}

let p1 = Person(150, 70);
let p2 = Person(181, 75);
p1.calcBmi(); // 31.11111111111111
p2.calcBmi(); // 22.89307408198773
p1.calcBmi == p2.calcBmi; // false
```

這個行為"看起來"會跟使用 `new Person` 一樣, 但是有個非常嚴重的問題, 就是 `p1.calcBmi != p2.calcBmi`

這個問題的嚴重性在於, 假定定義了像是 `MyClass` 這種類別, 並且有個方法 `myFunction`, 當建立了 1000 個實例

myFunction 也會被建立 1000 次, 這對於記憶體的處理是非常不健康的

邏輯上, 成員變數應該保持在自己的 scope, 而方法(例如 `MyClass.myFunction`) 是一個獨立的 function, 由所有的 `MyClass` 共用該 function 位址

僅需要傳入自己的參考, MyClass 便會假設 `this` 是自己傳進來的參考

使用 C++ 來舉例, C++ 的 class 實作隱含了 `this` 參數, 比方說

```c++
class Person {
public:
    Person(double height, double weight): height_(height), weight_(weight) {}

    double calcBmi() {
        return this->weight_ / (this->height_ * this->height_ / 10000);
    }

private:
    double height_;
    double weight_;
}
```

實際上, `calcBmi` 的簽章會包含一個隱含的參數 `this`

```c++
double Person::calcBmi(Person* this) {
    return this->weight_ / (this->height_ * this->height_ / 10000);
}
```

如果要驗證這一點, 通過 `std::bind` 這個函式可以更好的觀察到

```c++
#include <iostream>
#include <functional>

class Person {
public:
    Person(double height, double weight): height_(height), weight_(weight) {}

    double calcBmi() {
        return this->weight_ / (this->height_ * this->height_ / 10000);
    }

    double height_;
    double weight_;
};

int main() {
  Person person(150, 70);

  auto fn = std::bind(&Person::calcBmi, &person);
  std::cout << fn(); // 31.1111
}
```

逐步拆解以上的過程：

1. `class Person` 宣告了 `calcBmi` function, 允許 `Person` 計算 BMI
2. 把 `fn` 通過 `std::bind` 繫結了 `Person::calcBmi` 這個函式, 並且把 `this` 的 Context 繫結在 `person` 上
3. 調用 `fn()` 時, 相當於調用了 `person.calcBmi()`

!!!note

    以筆者的理解來說明：

    在Class的實現上, 可以拆解為 `屬性` 以及 `方法`

    `屬性` 是由實例自行維護的數據區塊

    `方法` 則是所有的實例共享同樣的函式宣告與實作

    所有的方法雖然共用相同的function 區段, 但是因為隱含了 `*this`, 所以不同實例調用方法才會呈現不同的結果

    可以參考 MSDN C++ 上的 [__thiscall](https://learn.microsoft.com/en-us/cpp/cpp/thiscall?view=msvc-170)

    部分程式語言, 如 Rustlang, 則要求在成員的方法實作, 顯式宣告第一個參數為 `&self`

    在 JavaScript 上, 早期的 Class 實作要求使用 `function` 來宣告, 在 `Person` 該例中

    早期開發人員通過 `if(!(this instanceof Person))` 判斷 `Person` 是通過建構式被調用, 還是通過一般函式被調用

    因為一般函式與建構式的調用, this 的數值是不相同的(在下個章節進行討論)

    function 的定義統一被移到 `<Class Name>.prototype` 這個區段, 而屬性則由實例自行維護

    因此前兩個例子中：

    1. `Class Example` 所有的 `Person` 實例, 會共享 `Person.prototype.calcBmi` 的實現
    2. `Class-like Example` 所有的 `Person` 實例, 不會共享 `Person.prototype.calcBmi` 的實現, 相當於 `calcBmi` 的實現每次在 `Person()` 調用時, 都被重新宣告/實現一次。

    在例子2中, 使用的實例越多, 記憶體的使用則越劇烈

## ES6 以後的類別

- ES6 (ECMA 2016) 以後的標準, 提供了 `class` 與 `extends` 關鍵字
- `class` 實際上是一種特別的 [函式(Functions)](/webgame-engine/learn-js/basic-and-syntax/#functions)
- 函式宣告和類別宣告的一個重要差別在於函式宣告是 [可提升(Hoisted)](/webgame-engine/learn-js/basic-and-syntax/#hoisting) 的，但類別宣告不是
- `class` 可用兩種方式定義，分別是 [類別宣告(Class declarations)](#class_declarations) 和 [類別表達(Class expressions)](#class_expressions)

### 類別宣告(Class declarations)

```js
class Person {
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }
}
```

### 類別表達(Class expressions)

```js
// 匿名表達
const Person = class {
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }
};

// 具名表達
// 僅可在 PersonDetail class 內部使用該名稱
const Person = class PersonDetail {
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }
};
```

### 方法定義(Method definitions)

```js
class Person {
  // 倘若加上了 static 關鍵字, 其行為如同 C++ 的靜態方法(Static method):
  static introStr(name, age) {
    return "Hello! My name is " + name + ". I'm " + age + " years old.";
  }

  // 建構子(Constructor)
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }

  // 屬性獲取器(Getter)
  get bmi() {
    return this.calcBmi();
  }

  // 方法(Method)
  calcBmi() {
    return this.weight / ((this.height * this.height) / 10000);
  }
}

// Call Static Method
console.log(Person.introStr("Cindy", 25)); // Hello! My name is Cindy. I'm 25 years old.

// Call Constructor
const person = new Person(182, 75);

// Call Getter
console.log(person.bmi); // 22.642192971863302

// Call Method
console.log(person.calcBmi()); // 22.642192971863302
```

### 欄位宣告(Field declarations)

```js
class Person {
  // 公共欄位(Public fields) *可從類別外部使用公共欄位
  height = -1;

  // 私有欄位(Private fields) *不可從類別外部使用私有欄位
  #weight = -1;

  constructor(height, weight) {
    this.height = height;
    this.#weight = weight;
  }
}

const person = new Person(182, 75);

console.log(person.height); // 182
console.log(person.#weight); // Syntax error
```

### 繼承(Inheritance)

倘若多定義了 Adult 成年人類別, 只需要透過 extends 關鍵字即可

```js
// 父類別
class Person {
  name = "";
  age = -1;

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  introStr() {
    return `${this.name} is ${this.age} year-old`;
  }
}

// 子類別
class Adult extends Person {
  childCount = -1;

  constructor(name, age, childCount) {
    // 使用 super() 呼叫父類別的 constructor
    // super 關鍵字會依照不同 context, 決定 super 的數值。在此處中, 是調用 Parent Class 的建構式
    super(name, age);
    this.childCount = childCount;
  }

  introStr() {
    // 使用 super 可以呼叫父類別的 functions
    return `${super.introStr()} and has ${this.childCount} children`;
  }
}

const adult = new Adult("Cindy", 25, 2);

console.log(adult.name); // "Cindy"
console.log(adult.age); // 25
console.log(adult.childCount); // 2
console.log(adult.introStr()); // "Cindy is 25 year-old and has 2 children."
console.log("Is adult an instance of Person?", adult instanceof Person); // true

/*
    ES5 以前需要手動處理 prototype 的指向來模擬繼承敘述
*/
// 父類別
function Person(name, age) {
  this.name = "";
  this.age = -1;
  if (!(this instanceof Person)) throw new Error("is constructor");
  this.name = name;
  this.age = age;
}

Person.prototype.introStr = function () {
  return `${this.name} is ${this.age} year-old`;
};

// 子類別
function Adult(name, age, childCount) {
  // 模擬使用 super() 呼叫父類別的 constructor
  Person.call(this, name, age);
  this.childCount = childCount;
}

// 子類別擴展(extends)父類別
Adult.prototype = Object.create(Person.prototype);
Adult.prototype.constructor = Adult;
Adult.prototype.introStr = function () {
  // 模擬使用 super 可以呼叫父類別的 functions
  return `${Person.prototype.introStr.call(this)} and has ${
    this.childCount
  } children`;
};

const adult = new Adult("Cindy", 25, 2);

console.log(adult.name); // "Cindy"
console.log(adult.age); // 25
console.log(adult.childCount); // 2
console.log(adult.introStr()); // "Cindy is 25 year-old and has 2 children."
console.log("Is adult an instance of Person?", adult instanceof Person); // true
```

此外, 假設建構式不需要參數, 類別可以使用 new Person 或是 new Person() 的方式初始化, 他們的差異是運算子優先順序

因為成員存取算運子.的優先度比較高, 所以使用

new Person.introStr 會導致錯誤, 因為 Person 不存在 introStr 這個靜態方法, 但是使用

new Person().introStr 則不會出錯, 因為他實際上調用了 (new Person).introStr

!!!tip

    new Class().method() 等同於 (new Class).method();

## 物件(Object)

---

- 物件是用於儲存各種帶鍵容器和更複雜的資料型別
- 物件由 [建構子(Constructor)](#constructor) 、 [靜態方法(Static methods)](#static-methods) 、 [實例屬性(Instance properties)](#instance-properties) 和 [實例方法(Instance methods)](#instance-methods) 等元件組成

### 建構子(Constructor)

- `Object()`
  > e.g.

```js
const obj = new Object();
obj.num = 15;
console.log(obj); // { num: 15 }
```

### 靜態方法(Static methods)

- `Object.assign()`  
  將一個或多個來源物件的所有可枚舉自有屬性的值複製到目標物件中

- `Object.create()`  
  使用指定的原型物件和屬性建立一個新物件

- `Object.defineProperties()`  
  在物件中新增多個由給定描述符描述的命名屬性

- `Object.defineProperty()`  
  在物件中新增一個由給定描述符描述的命名屬性

- `Object.entries()`  
  傳回包含給定物件自有可枚舉字串屬性的所有 `[key, value]` 陣列

- `Object.freeze()`  
  凍結一個物件。其他程式碼不能刪除或更改其任何屬性

- `Object.fromEntries()`  
  從一個包含 `[key, value]` 對的可迭代物件中傳回一個新的物件 (`Object.entries`的反操作)

- `Object.getOwnPropertyDescriptor()`  
  傳回一個物件的已命名屬性的屬性描述符

- `Object.getOwnPropertyDescriptors()`  
  傳回一個包含物件所有自有屬性的屬性描述符的物件

- `Object.getOwnPropertyNames()`  
  傳回一個包含給定物件的所有自有可枚舉和不可枚舉屬性名稱的陣列

- `Object.getOwnPropertySymbols()`  
  傳回一個數組，它包含了指定物件所有自有 `symbol` 屬性

- `Object.getPrototypeOf()`  
  傳回指定物件的原型 (內部的 `[[Prototype]]` 屬性)

- `Object.hasOwn()`  
  如果指定屬性是指定物件的自有屬性，則傳回 `true` ，否則傳回 `false` 。如果該屬性是繼承的或不存在，則傳回 `false`

- `Object.is()`  
  比較兩個值是否相同。所有 `NaN` 值都相等 (這與 `==` 使用的 `IsLooselyEqual` 和 `===` 使用的 `IsStrictlyEqual` 不同)

- `Object.isExtensible()`  
  判斷物件是否可擴展

- `Object.isFrozen()`  
  判斷物件是否已經凍結

- `Object.isSealed()`  
  判斷對像是否已經封閉

- `Object.keys()`  
  傳回一個包含所有給定物件自有的可枚舉字串屬性名稱的陣列

- `Object.preventExtensions()`  
  防止物件的任何擴充

- `Object.seal()`  
  防止其他程式碼刪除物件的屬性

- `Object.setPrototypeOf()`  
  設定物件的原型 (內部 `[[Prototype]]` 屬性)

- `Object.values()`  
  傳回包含給定物件所有自有可枚舉字串屬性的值的陣列

### 實例屬性(Instance properties)

- `Object.prototype.constructor`  
  建立該實例物件的建構子，對於普通的 `Object` 實例，初始值為 `Object` 建構子，其它建構函式的實例都會從它們各自的 `Constructor.prototype` 物件繼承 `constructor` 屬性

### 實例方法(Instance methods)

- `Object.prototype.hasOwnProperty()`  
  傳回布林值，用來表示一個物件本身是否包含指定的屬性，該方法並不會尋找原型鏈上繼承來的屬性

- `Object.prototype.isPrototypeOf()`  
  傳回布林值，用於表示該方法所呼叫的物件是否在指定物件的原型鏈中

- `Object.prototype.propertyIsEnumerable()`  
  傳回布林值，指示指定屬性是否為物件的可枚舉自有屬性

- `Object.prototype.toLocaleString()`  
  呼叫 `toString()` 方法

- `Object.prototype.toString()`  
  傳回一個代表該物件的字串

- `Object.prototype.valueOf()`  
  傳回指定物件的基本類型值

### 標準內建物件(Standard built-in objects)

- 數值屬性(Value properties)
  - `Infinity`
  - `NaN`
  - `undefined`
  - `null`
- 函式屬性(Function properties)
  - `isFinite()`
  - `isNaN()`
  - `parseFloat()`
  - `parseInt()`
  - `decodeURI()`
  - `decodeURIComponent()`
  - `encodeURI()`
  - `encodeURIComponent()`
- 基礎物件(Fundamental objects)
  - `Object`
  - `Function`
  - `Boolean`
  - `Symbol`
- 錯誤物件(Error objects)
  - `Error`
  - `EvalError`
  - `InternalError`
  - `RangeError`
  - `ReferenceError`
  - `SyntaxError`
  - `TypeError`
  - `URIError`
- 數字、文字與日期物件(Number, text and date objects)
  - `Number`
  - `Math`
  - `String`
  - `RegExp`
  - `Date`
- 具索引的容器(Indexed collections)
  - `Array`
  - `Int8Array`
  - `Uint8Array`
  - `Uint8ClampedArray`
  - `Int16Array`
  - `Uint16Array`
  - `Int32Array`
  - `Uint32Array`
  - `Float32Array`
  - `Float64Array`
- 具鍵值的容器(Keyed collections)
  - `Map`
  - `Set`
  - `WeakMap`
  - `WeakSet`
- 結構化資料物件(Structured data objects)
  - `ArrayBuffer`
  - `SharedArrayBuffer`
  - `Atomics`
  - `DataView`
  - `JSON`
- 控制抽象化物件(Control abstraction objects)
  - `Iterator`
  - `AsyncIterator`
  - `Promise`
  - `GeneratorFunction`
  - `AsyncGeneratorFunction`
  - `Generator`
  - `AsyncGenerator`
  - `AsyncFunction`
- 記憶體管理物件(Managing memory objects)
  - `WeakRef`
  - `FinalizationRegistry`
- 映射物件(Reflection objects)
  - `Reflect`
  - `Proxy`
