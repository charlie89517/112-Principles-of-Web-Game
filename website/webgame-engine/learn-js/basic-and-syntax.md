# 基礎型別與語法

## 資料型別(Data Types)

---

1. 基礎型別(Primitives)
    - boolean
    > 布林型別(`true` or `false`)
    - number
    > 數字型別，使用 64位元雙精度浮點數([double-precision 64-bit floating point](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)) 格式存儲
    - string
    > 字串型別(sequence of characters)
    - bigint
    > 整數型別，可為任意大小
    - symbol[^2]
    > 符號型別，唯一且不可變的識別符(unique and immutable identifier)
    - null
    > 空或無效型別(nonexistent or invalid object)
    - undefined
    > 未定義型別，會自動賦予給未賦值的變數(unassigned variables)

2. 函式型別(Function)
> [函式(Functions)](#functions) 在技術上來說是一種 **可被呼叫的物件(Callable Objects)**

3. 物件型別(Object)
> 詳見 [類別與物件](/webgame-engine/learn-js/class-and-object#object)



## 語法(Syntax)

---

### 介紹(Introduction)

- 指令(instructions) 在 JavaScript 中被稱為 [陳述式(statements)](#statements) 並以 分號(`;`) 分隔不同陳述式
- 如果在一行中僅有一個陳述式，則陳述式後面可以不加上分號(`;`)
- 一個陳述式可以跨越多行
- JavaScript 是 大小寫有別的(Case-sensitive) 且使用 Unicode 字元集來編碼

### 關鍵字(Keywords)
| **JavaScript** |  |  |  |  |  |  |  |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| abstract[^1] | arguments | await[^2] | boolean[^1] | break | byte[^1] | case | catch |
| char[^1] | class[^2] | const[^2] | continue | debugger | default | delete | do |
| double[^1] | else | enum[^2] | eval | export[^2] | extends[^2] | false | final[^1] |
| finally | float[^1] | for | function | goto[^1] | if | implements | import[^2] |
| in | instanceof | int[^1] | interface | let[^2] | long[^1] | native[^1] | new |
| null | package | private | protected | public | return | short[^1] | static |
| super[^2] | switch | synchronized[^1] | this | throw | throws[^1] | transient[^1] | true |
| try | typeof | var | void | volatile[^1] | while | with | yield |

### 註解(Comments)
- 單行註解
>
```js
// ...
```
- 跨行註解 
>
```js
/* ...
...
...
... */
```

### 數值(Values)
- 不用 容器(container) 參照的數值 稱為 [字符(Literals)](#literals)
- 使用 容器(container) 參照的數值 稱為 [變數(Variables)](#variables)

### 字符(Literals)
- 布林字符(Boolean literals)  
> e.g.  
```js
true
```
```js
false
```
- 整數字符(Numerical literals)  
> e.g.  
```js
60 // 十進制
```
```js
0b111100 // 二進制
```
```js
0o74 // 八進制
```
```js
0x3C // 十六進制(a~f 不區分大小寫)
```
```js
999999999999999999999999999999999999999n // BigInt
```
- 浮點數字符(Floating-point literals)  
> e.g.  
```js
3.1415926535
```
```js
-273.15
```
```js
6.626e-34
```
- 字串字符(String literals)  
> e.g.  
```js
"Hello World!"
```
```js
'bye bye~'
```
- 陣列字符(Array literals)  
> e.g.  
```js
["Male", "Female"]
```
```js
[1, "2", 3.0]
```
- 物件字符(Object literals)  
> e.g.  
```js
{ age: 25, sex: "Male", height: 182, weight: 75 }
```
```js
{ name: "Cindy", money: 3156721, phone: "0912-345-678" }
```
- 正則表達式字符(RegExp literals)  
> e.g.  
```js
/ab+c/
```
```js
/(\w+)\s+(\w+)/
```
- 範本字符(Template literals)[^2]  
> e.g.  
```js
// 基本字串
`In JavaScript '\n' is a line-feed.`

// 多行字串
`My Love in her attire doth show her wit,
It doth so well become her;
For every season she hath dressings fit,
For Winter, Spring, and Summer.
No beauty she doth miss
When all her robes are on:
But Beauty’s self she is
When all her robes are gone.`

// 字串插值
const name = "Cindy";
const age = 18;
`${name} is ${age} year-old.` // "Cindy is 18 year-old."
```

### 變數(Variables)

1. 命名規則(Naming conventions)
    - 不能與 預保留關鍵字(reserved keywords) 同名
    - 命名上可使用 字母(`a`~`z`, `A`~`Z`) 、 底線(`_`) 、 美元符號(`$`) 或 絕大多數的 Unicode 字母(e.g. `å`, `è`, `ü`) 作為開頭字元
    - 命名上除了開頭可用字元外還可使用 數字(`0`~`9`) 作為後續字元
    - 在命名變數上建議表明其變數所具備的意義或用處，且在 JavaScript 中習慣使用 小駝峰命名法(lower camel case) 來命名變數(e.g. `flyingDistance`, `eventCounter`, `animalAge`)

2. 宣告方式(Declaring ways)
    - `var`
    > 用於在函式內宣告 函式範圍變數(function-scoped variables) 或是在函式外宣告 全域範圍變數(globally-scoped variables)
    - `let`[^2]
    > 用於宣告 區塊範圍變數(block-scoped local variables)
    - `const`[^2]
    > 用於宣告 區塊範圍常數(read-only block-scoped local variables)

3. 宣告語法(Declaring syntax)
    - `var` | `let` | `const` `variableName` `=` `expression`
    > e.g.  
    ```js
    let num = 123 + 987
    ```
    ```js
    const sex = ["Male", "Female"]
    ```

### 運算子(Operators)
- 賦值運算子(Assignment operators)
    - 賦值(Assignment) `=`  
    > e.g. `x = f()`
    - 加法賦值或串接賦值(Addition assignment or Concatenation assignment) `+=`  
    > e.g. `x += f()` 其等價於(equivalent to) `x = x + f()`
    - 減法賦值(Subtraction assignment) `-=`  
    > e.g. `x -= f()` 其等價於(equivalent to) `x = x - f()`
    - 乘法賦值(Multiplication assignment) `*=`  
    > e.g. `x *= f()` 其等價於(equivalent to) `x = x * f()`
    - 除法賦值(Division assignment) `/=`  
    > e.g. `x /= f()` 其等價於(equivalent to) `x = x / f()`
    - 餘數賦值(Remainder assignment) `%=`  
    > e.g. `x %= f()` 其等價於(equivalent to) `x = x % f()`
    - 指數賦值(Exponentiation assignment) `**=`  
    > e.g. `x = f()` 其等價於(equivalent to) `x = x ** f()`
    - 左移賦值(Left shift assignment) `<<=`  
    > e.g. `x <<= f()` 其等價於(equivalent to) `x = x << f()`
    - 右移賦值(Right shift assignment) `>>=`  
    > e.g. `x >>= f()` 其等價於(equivalent to) `x = x >> f()`
    - 無號右移賦值(Unsigned right shift assignment) `>>>=`  
    > e.g. `x >>>= f()` 其等價於(equivalent to) `x = x >>> f()`
    - 位元 AND 賦值(Bitwise AND assignment) `&=`  
    > e.g. `x &= f()` 其等價於(equivalent to) `x = x & f()`
    - 位元 XOR 賦值(Bitwise XOR assignment) `^=`  
    > e.g. `x ^= f()` 其等價於(equivalent to) `x = x ^ f()`
    - 位元 OR 賦值(Bitwise OR assignment) `|=`  
    > e.g. `x |= f()` 其等價於(equivalent to) `x = x | f()`
    - 邏輯 AND 賦值(Logical AND assignment) `&&=`  
    > e.g. `x &&= f()` 其等價於(equivalent to) `x && (x = f())`
    - 邏輯 OR 賦值(Logical OR assignment) `||=`  
    > e.g. `x ||= f()` 其等價於(equivalent to) `x || (x = f())`
    - 空值合併賦值(Nullish coalescing assignment) `??=`  
    > e.g. `x ??= f()` 其等價於(equivalent to) `x ?? (x = f())`
- 比較運算子(Comparison operators)
    - 等於(Equal) `==`  
    > 運算元等價時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `5 == 5` return `true`  
    > `9 == "9"` return `true`  
    > `2 == 6` return `false`  
    - 不等於(Not equal) `!=`  
    > 運算元不等價時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `2 != 6` return `true`  
    > `5 != 5` return `false`  
    > `9 != "9"` return `false`  
    - 嚴格等於(Strict equal) `===`  
    > 運算元等價且型別相同時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `5 === 5` return `true`  
    > `9 === "9"` return `false`  
    > `2 === 6` return `false`  
    - 嚴格不等於(Strict not equal) `!==`  
    > 運算元不等價或型別不相同時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `2 !== 6` return `true`  
    > `9 !== "9"` return `true`  
    > `5 !== 5` return `false`  
    - 大於(Greater than) `>`  
    > 左方運算元大於右方運算元時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `2 > 6` return `false`  
    > `6 > 2` return `true`  
    > `5 > 5` return `false`  
    - 大於或等於(Greater than or equal) `>=`  
    > 左方運算元大於或等於右方運算元時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `2 >= 6` return `false`  
    > `6 >= 2` return `true`  
    > `5 >= 5` return `true`  
    - 小於(Less than) `<`  
    > 左方運算元小於右方運算元時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `2 < 6` return `true`  
    > `6 < 2` return `false`  
    > `5 < 5` return `false`  
    - 小於或等於(Less than or equal) `<=`  
    > 左方運算元小於或等於右方運算元時回傳 `true`，否則回傳 `false`  
    > e.g.  
    > `2 <= 6` return `true`  
    > `6 <= 2` return `false`  
    > `5 <= 5` return `true`  
- 算術運算子(Arithmetic operators)
    - 加法(Addition) `+`  
    > e.g. `2 + 6` return `8`  
    - 減法(Subtraction) `-`  
    > e.g. `2 - 6` return `-4`  
    - 乘法(Multiplication) `*`  
    > e.g. `2 * 6` return `12`  
    - 除法(Division) `/`  
    > e.g. `2 / 6` return `0.3333333333333333`  
    - 餘數(Remainder) `%`  
    > e.g. `2 % 6` return `2`  
    - 指數(Exponentiation) `**`  
    > e.g. `2 ** 6` return `64`  
    - 遞增(Increment) `++`  
    > e.g. 假設 `x` 為 `2`  
    > `++x` 設 `x` 為 `3` 後 return `3`  
    > `x++` return `2` 後 設 `x` 為 `3`  
    - 遞減(Decrement) `--`  
    > e.g. 假設 `x` 為 `2`  
    > `--x` 設 `x` 為 `1` 後 return `1`  
    > `x--` return `2` 後 設 `x` 為 `1`  
    - 一元正號(Unary plus) `+`  
    > e.g. `+"6"` return `6`  
    - 一元負號(Unary negation) `-`  
    > e.g. `-"6"` return `-6`  
- 位元運算子(Bitwise operators)
    - 位元 AND(Bitwise AND) `&`  
    > e.g. `0b1100 & 0b1010` return `8`(0b1000)  
    - 位元 OR(Bitwise OR) `|`  
    > e.g. `0b1100 | 0b1010` return `14`(0b1110)  
    - 位元 XOR(Bitwise XOR) `^`  
    > e.g. `0b1100 ^ 0b1010` return `6`(0b0110)  
    - 位元 NOT(Bitwise NOT) `~`  
    > e.g. `~0b1100` return `-13`(0b11111111111111111111111111110011)  
    - 左移(Left shift) `<<`  
    > e.g. `0b1100 << 2` return `48`(0b110000)  
    - 右移(Sign-propagating right shift) `>>`  
    > e.g. `0b11111111111111111111111111110011 >> 2` return `-4`(0b11111111111111111111111111111100)  
    - 無號右移(Zero-fill right shift) `>>>`  
    > e.g. `0b11111111111111111111111111110011 >>> 2` return `1073741820`(0b00111111111111111111111111111100)  
- 邏輯運算子(Logical operators)
    - 邏輯 AND(Logical AND) `&&`  
    > e.g.  
    > `true && true` return `true`  
    > `true && false` return `false`  
    > `false && false` return `false`  
    - 邏輯 OR(Logical OR) `||`  
    > e.g.  
    > `true || true` return `true`  
    > `true || false` return `true`  
    > `false || false` return `false`  
    - 邏輯 NOT(Logical NOT) `!`  
    > e.g.  
    > `!true` return `false`  
    > `!false` return `true`  
    - 空值合併(Nullish coalescing) `??`  
    > e.g.  
    > `null ?? "default"` return `"default"`  
    > `"hello" ?? "default"` return `"hello"`  
- 字串運算子(String operators)
    - 串接(Concatenation) `+`  
    > e.g. `"Hello " + "World!"` return `"Hello World!"`  
- 條件運算子(Conditional operator) `?:`
> e.g.  
> `6 > 4 ? "Yes" : "No"` return `"Yes"`  
> `2 > 6 ? "Yes" : "No"` return `"No"`  
- 逗點運算子(Comma operator) `,`
> e.g.  
> `"Front", "End"` return `"End"`  
> `5+1, 2-1` return `1`  
- 一元運算子(Unary operators)
    - delete  
    > e.g.  
    > `x = 5; delete x` return `true`(可刪除隱式宣告變數)  
    > `var x = 5; delete x` return `false`(無法刪除 var 宣告變數)  
    > `delete Math.PI` return `false`(無法刪除內建定義)  
    - typeof
    > e.g.  
    > `var num = 1; typeof num` return `"number"`  
    > `var num = "1"; typeof num` return `"string"`  
    - void
    > e.g.  
    > `void "doAnything()"` return `undefined`  
    > `void("doAnything()")` return `undefined`  
- 關係運算子(Relational operators)
    - in
    > e.g. `var person = { age: 25, sex: "Male", height: 182, weight: 75 }; age in person` return `true`  
    - instanceof
    > e.g. `"var happyNewYear = new Date(2024, 01, 01); happyNewYear instanceof Date` return `"true"`

### 表達式(Expressions)
- 表達式由 [數值(values)](#values) 和 [運算子(operators)](#operators) 組合而成，並回傳 計算結果(evaluation)  
> e.g. 假設 `x` 為 `2`  
> `((x + 8) / 2)  2` return `25`  

### 宣告式(Declarations)
- 宣告變數(Declaring variables)
    - `var`
    > 宣告函式範圍或全域範圍變數  
    > e.g.
    ```js
    var num = 5;
    ```
    - `let`
    > 宣告區塊範圍變數  
    > e.g.
    ```js
    let num = 5;
    ```
    - `const`
    > 宣告區塊範圍常數  
    > e.g.
    ```js
    const num = 5;
    ```
- 宣告函式與類別(Declaring Functions and classes)
    - `function`
    > 宣告 [函式(Functions)](#functions)  
    > e.g.  
    ```js
    function calcHypotenuse(leg1, leg2) {
        return Math.sqrt(leg1 * leg1 + leg2 * leg2);
    }

    console.log(calcHypotenuse(3, 4)) // 5
    ```
    - `function*`
    > 宣告生成器函式，與 `yield` 結合能簡單編寫迭代器  
    > e.g.  
    ```js
    function* generator(upperBound) {
        for (let i = 0; i <= upperBound; ++i) {
            yield i;
        }
    }

    for (const i of generator(5)){
        console.log(i); // 0 -> 1 -> 2 -> 3 -> 4 -> 5
    }
    ```
    - `async function`
    > 宣告非同步函式  
    > e.g.  
    ```js
    async function awaitFunc() {
        return await 5;
    }

    (async () => {
        console.log(await awaitFunc()) // 5
    })();
    ```
    - `async function*`
    > 宣告非同步生成器函式，與 `yield` 結合能簡單編寫迭代器  
    > e.g.  
    ```js
    async function* awaitIterFunc() {
        yield await 5;
        yield await 7;
        yield await 9;
    }

    (async () => {
        for await (const num of asyncIterfunc()) {
            console.log(num); // 5 -> 7 -> 9
        }
    })();
    ```
    - `class`
    > 宣告類別  
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
    ```

### 陳述式(Statements)
- 控制流程(Control flow)
    - `if...else`
    > 根據條件執行的語句區塊  
    > e.g.
    ```js
    const height = 160;
    if ( height > 170 ) {
        console.log("Tall");
    } else {
        console.log("Short");
    }
    ```
    - `switch`
    > 在不同條件下執行的語句區塊  
    > e.g.
    ```js
    const job = "Software engineer";
    switch (job) {
    case "Software engineer":
        console.log("Coding");
        break;
    case "Police":
        console.log("Patrol");
        break;
    case "Doctor":
        console.log("Surgery");
        break;
    default:
        console.log("Unemployed");
    }
    ```
    - `return`
    > 指定函式回傳值並退出函式  
    > e.g.
    ```js
    function sayHello() {
        return "hello";
    }
    ```
    - `break`
    > 中斷當前 loop 、 switch 或 label 的陳述式並將程式控制權轉移到被終止陳述式後的陳述式  
    > e.g.
    ```js
    while (true) {
        break;
    }
    ```
    - `continue`
    > 中斷執行當前(或標記 *optional) loop 中該次所迭代的陳述式，並繼續執行 loop 中的下次迭代  
    > e.g.
    ```js
    let times = 5
    for (let i = 0 ; i < times ; ++i) {
        if (i == 3) {
            continue;
        }
        console.log(i); // 0 -> 1 -> 2 -> 4
    }
    ```
    - `throw`
    > 拋出使用者所定義的例外狀況  
    > e.g.
    ```js
    throw new Error("I'm Error");
    ```
    - `try...catch`
    > 指定例外狀況捕捉範圍與回應的語句區塊  
    > e.g.
    ```js
    function errorFunc() {
        throw new Error("I'm Error");
    }

    try {
        // tryStatements
        errorFunc();
    } catch (exception) {
        // catchStatements
        console.error(exception);
    } finally {
        // finallyStatements
        console.log("End of run.");
    }

    // catch 的參數可省略、 finally 區塊亦可省略
    try {
        // tryStatements
    } catch {
        // catchStatements
    }
    ```
- 迭代(Iterations)
    - `for`
    > 由三個可選表達式組成的迴圈  
    > e.g.
    ```js
    /*
        initialization: 在迴圈開始之前計算一次的表達式(含賦值表達式)或變數宣告式
        condition: 每次迴圈迭代前要計算的表達式。若為true則執行該次迭代，否則中斷迴圈
        afterthought: 在迴圈迭代結束時計算的表達式
    */
    // for (initialization; condition; afterthought)
    for (let i = 0; i < 5; ++i) {
        console.log(i); // 0 -> 1 -> 2 -> 3 -> 4
    }
    ```
    - `while`
    > 由條件判斷的迴圈  
    > e.g.
    ```js
    let i = 0;
    /*
        condition: 每次迴圈迭代前要計算的表達式。若為true則執行該次迭代，否則中斷迴圈
    */
    // while (condition)
    while ( i < 5 ) {
        console.log(i); // 0 -> 1 -> 2 -> 3 -> 4
        ++i;
    }
    ```
    - `for...in`
    > 以任意順序迭代物件的可枚舉屬性(enumerable properties)  
    > e.g.
    ```js
    const object = { name: "Cindy", age: 31 };
    for (const property in object) {
        console.log(`${property}: ${object[property]}`); // name: Cindy -> age: 31
    }
    ```
    - `for...of`
    > 迭代可迭代物件(包括陣列、類​​別數組物件、迭代器和生成器)  
    > e.g.
    ```js
    const array1 = [5, 7, 9];
    for (const element of array1) {
        console.log(element); // 5 -> 7 -> 9
    }
    ```
    - `for await...of`
    > 迭代非同步可迭代物件  
    > e.g.
    ```js
    async function* asyncIterfunc() {
        yield 5;
        yield 7;
        yield 9;
    }

    (async () => {
        for await (const num of asyncIterfunc()) {
            console.log(num); // 5 -> 7 -> 9
        }
    })();
    ```
    - `do...while`
    > 由條件判斷的迴圈，至少執行一次  
    > e.g.
    ```js
    let i = 0;
    do {
        console.log(i); // 0 -> 1 -> 2 -> 3 -> 4 -> 5
        ++i;
    } while ( i < 5 );
    ```

### 函式(Functions)
1. 定義函式(Defining functions)
    - 一個函式由下列元件組成
        - 函式名稱
        - 包圍在括號(`()`)中，並由逗號(`,`)區隔的函式參數列表
        - 包圍在大括號(`{}`)中，定義函式功能的 [陳述式(statements)](#statements)

    > e.g.  
    ```js
    function calcHypotenuse(leg1, leg2) {
        return Math.sqrt(leg1 * leg1 + leg2 * leg2);
    }
    ```

2. 呼叫函式(Calling functions)
> e.g.  
```js
function calcHypotenuse(leg1, leg2) {
    return Math.sqrt(leg1 * leg1 + leg2 * leg2);
}

console.log(calcHypotenuse(3, 4)) // 5
```

### 提升(Hoisting)
- 讓變數和函數的 **宣告** 在編譯階段就被放入記憶體，但初始化的實際位置和程式碼中完全一樣
> e.g.  
```js
// 因為 calcHypotenuse 在編譯階段被 Hoisting，所以能被正常執行而不報錯
console.log(calcHypotenuse(3, 4)) // 5

function calcHypotenuse(leg1, leg2) {
    return Math.sqrt(leg1 * leg1 + leg2 * leg2);
}
```
```js
// 因為 num1, num2 在編譯階段被 Hoisting，所以能被正常執行而不報錯
// 但由於 num2 初始化的位置在打印之後，因此為預設的 undefined
num1 = 3;
console.log(num1, num2) // 3 undefined
var num1
var num2 = 5;
```

[^1]: Reserved as future keywords by older ECMAScript specifications (ES1 ~ ES3), 盡量不要作為變數名稱使用.
[^2]: New Features in ES5 and ES6.