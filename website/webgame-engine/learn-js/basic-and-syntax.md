# Basic and Syntax

## **基礎(Basic)**

---

### **介紹(Introduction)**

- **指令(instructions) 在 JavaScript 中被稱為 [陳述式(statements)](#statements) 並以 分號(`;`) 分隔不同陳述式**
- **如果在一行中僅有一個陳述式，則陳述式後面可以不加上分號(`;`)**
- **JavaScript 是 大小寫有別的(Case-sensitive) 且使用 Unicode 字元集來編碼**

### **數值(Values)**
- **固定的數值稱為 [字符(Literals)](#literals)**
- **可變的數值稱為 [變數(Variables)](#variables)**

### **資料型別(Data Types)**

1. **基本型別(Primitives)**
    - **boolean - 布林型別(`true` or `false`)**
    - **number - 數字型別，使用 64位元雙精度浮點數([double-precision 64-bit floating point](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)) 格式存儲**
    - **string - 字串型別(sequence of characters)**
    - **symbol[^1] - 符號型別，唯一且不可變的識別符(unique and immutable identifier)**
    - **null - 空或無效型別(nonexistent or invalid object)**
    - **undefined - 未定義型別，會自動賦予給未賦值的變數(unassigned variables)**

2. **物件型別(Object)**
    - **詳見 [Class and Object](/webgame-engine/learn-js/class-and-object)**

### **關鍵字(Keywords)**
| JavaScript |  |  |  |  |  |  |  |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| abstract | arguments | await[^1] | boolean | break | byte | case | catch |
| char | class[^1] | const[^1] | continue | debugger | default | delete | do |
| double | else | enum[^1] | eval | export[^1] | extends[^1] | false | final |
| finally | float | for | function | goto | if | implements | import[^1] |
| in | instanceof | int | interface | let[^1] | long | native | new |
| null | package | private | protected | public | return | short | static |
| super[^1] | switch | synchronized | this | throw | throws | transient | true |
| try | typeof | var | void | volatile | while | with | yield |

### **註解(Comments)**
- **`//` `...` - 單行註解**
- **`/*` `...` `*/` - 跨行註解** 


## **語法(Syntax)**

---

### **字符(Literals)**
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
60 //十進制
```
```js
0b111100 //二進制
```
```js
074 //八進制
```
```js
0x3C //十六進制
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
- 範本字符(Template literals)[^1]  
> e.g.  
```js
`My Love in her attire doth show her wit,
It doth so well become her;
For every season she hath dressings fit,
For Winter, Spring, and Summer.
No beauty she doth miss
When all her robes are on:
But Beauty’s self she is
When all her robes are gone.`
```

### **變數(Variables)**

1. **命名規則(Naming conventions)**
    - **不能與 預保留關鍵字(reserved keywords) 同名**
    - **命名上可使用 字母(`a`~`z`, `A`~`Z`) 、 底線(`_`) 、 美元符號(`$`) 或 絕大多數的 Unicode 字母(e.g. `å`, `è`, `ü`) 作為開頭字元**
    - **命名上除了開頭可用字元外還可使用 數字(`0`~`9`) 作為後續字元**
    - **在命名變數上建議表明其變數所具備的意義或用處，且在 JavaScript 中習慣使用 小駝峰命名法(lower camel case) 來命名變數(e.g. `flyingDistance`, `eventCounter`, `animalAge`)**

2. **宣告方式(Declaring ways)**
    - **var - 用於在函式內宣告 函式範圍變數(function-scoped variables) 或是在函式外宣告 全域範圍變數(globally-scoped variables)**
    - **let - 用於宣告 區塊範圍變數(block-scoped local variables)**
    - **const - 用於宣告 區塊範圍常數(read-only block-scoped local variables)**

3. **宣告語法(Declaring syntax)**
    - **`var` | `let` | `const` `variableName` `=` `literalValue`**
    > e.g.  
    ```js
    let num = 123
    ```
    ```js
    const sex = ["Male", "Female"]
    ```

### **運算子(Operators)**
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
    > e.g. `x **= f()` 其等價於(equivalent to) `x = x ** f()`
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

### **表達式(Expressions)**
- 表達式由 [數值(values)](#values) 和 [運算子(operators)](#operators) 組合而成，並回傳 計算結果(evaluation)  
> e.g. 假設 `x` 為 `2`  
> `((x + 8) / 2) ** 2` return `25`  

### **陳述式(Statements)**
- 陳述式通常以下列關鍵字開頭，用於決定要執行的操作
    - var - 詳見[宣告方式(Declaring ways)](#declaring_ways)
    - let - 詳見[宣告方式(Declaring ways)](#declaring_ways)
    - const - 詳見[宣告方式(Declaring ways)](#declaring_ways)
    - if - 根據條件執行的語句區塊
    - switch - 在不同條件下執行的語句區塊
    - for - 在迴圈中執行的語句區塊
    - function - 宣告函式
    - return - 退出函式
    - try - 實現錯誤處理的語句區塊

### **函式(Functions)**
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
    
    calcHypotenuse(3, 4)
    ```

[^1]: New Features in ES5 and ES6.