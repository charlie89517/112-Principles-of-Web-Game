# Class and Object

## **類別(Class)**

---

- **類別實際上是一種特別的 [函式(Functions)](/webgame-engine/learn-js/basic-and-syntax/#functions)**
- **函式宣告和類別宣告的一個重要差別在於函式宣告是 提升(Hoisted) 的，但類別宣告不是**
- **類別可用兩種方式定義，分別是 [類別宣告(Class declarations)](#class_declarations) 和 [類別表達(Class expressions)](#class_expressions)**

### **類別宣告(Class declarations)**
> e.g.  
```js
class Person {
    constructor(height, weight) {
        this.height = height;
        this.weight = weight;
    }
}
```

### **類別表達(Class expressions)**
> e.g.  
```js
// unnamed(anonymous)
const Person = class {
    constructor(height, weight) {
        this.height = height;
        this.weight = weight;
    }
};

// named
// you can only use class name internally(class's body)
const Person = class PersonDetail {
    constructor(height, weight) {
        this.height = height;
        this.weight = weight;
    }
};
```

### **方法定義(Method definitions)**
> e.g.  
```js
class Person {
    // Static Method
    static introStr(name, age) {
        return "Hello! My name is " + name + ". I'm " + age + " years old.";
    }

    // Constructor
    constructor(height, weight) {
        this.height = height;
        this.weight = weight;
    }

    // Getter
    get bmi() {
        return this.calcBmi();
    }

    // Method
    calcBmi() {
        return this.weight / (this.height * this.height / 10000);
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

### **欄位宣告(Field declarations)**
> e.g.  
```js
class Person {
    // 公共欄位(Public fields) *可從類別外部使用公共欄位
    height = -1;

    // 私有欄位(Private fields) *不可從類別外部使用私有欄位
    #weight = -1;

    // Constructor
    constructor(height, weight) {
        this.height = height;
        this.#weight = weight;
    }
}

const person = new Person(182, 75);

console.log(person.height); // 182
console.log(person.#weight); // Syntax error
```

### **繼承(Inheritance)**
> e.g.  
```js
// 父類別
class Person {
    height = -1;
    weight = -1;

    constructor(height, weight) {
        this.height = height;
        this.weight = weight;
    }

    str() {
        return "Person";
    }
}

// 子類別
class Adult extends Person {
    childCount = -1;

    constructor(height, weight, childCount) {
        // 使用 super() 呼叫父類別的 constructor
        super(height, weight)
        // 使用 super 呼叫父類別的 functions
        console.log("Parent Class: " + super.str()) // "Parent Class: Person"
        this.childCount = childCount;
    }

    str() {
        return "Adult";
    }
}

const adult = new Adult(182, 75, 2);

console.log(adult.height); // 182
console.log(adult.weight); // 75
console.log(adult.childCount); // 2
console.log(adult.str()); // "Adult"
```


## **物件(Object)**

---

- 物件是用於儲存各種帶鍵容器和更複雜的資料型別
- 物件由 建構子(Constructor) 、 靜態方法(Static methods) 、 實例屬性(Instance properties) 和 實例方法(Instance methods) 組成

### **建構子(Constructor)**
- `Object()`
> e.g.  
```js
const obj = new Object();
obj.num = 15;
console.log(obj); // { num: 15 }
```

### **靜態方法(Static methods)**
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

### **實例屬性(Instance properties)**
- `Object.prototype.constructor`  
建立該實例物件的建構子，對於普通的 `Object` 實例，初始值為 `Object` 建構子，其它建構函式的實例都會從它們各自的 `Constructor.prototype` 物件繼承 `constructor` 屬性

### **實例方法(Instance methods)**
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
