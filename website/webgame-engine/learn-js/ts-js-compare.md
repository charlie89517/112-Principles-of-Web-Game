# JS vs TS

## 學習曲線(Learning Curve)
- TypeScript 是 JavaScript 的 超集(Superset)，要編寫 TypeScript 程式碼就必須對 JavaScript 有基本的認識和了解
- 由於 TypeScript 是強型別且為物件導向的語言，因此需要先知道 OOPS(object-oriented programming system) 的概念

## 性能(Performance)
- TypeScript 的創建是為了克服 JavaScript 對大型複雜應用程式中的不足，因此 TypeScript 可以節省開發時間使開發人員更有效率
- TypeScript 程式碼在執行之前會被編譯成 JavaScript

## 支援(Support)
- TypeScript 提供 JavaScript 所不提供的變數宣告、函式範本和型別系統。它在語法方面類似於 JScript 和 .Net，支援 ECMAScript 2015 標準功能，包括模組、箭頭函式語法和類別
- 由於 Microsoft 支援 TypeScript，它擁有許多領先的框架和編輯器。透過與編輯器的緊密整合，它提供編譯期間的錯誤處理以避免執行時出現錯誤
- TypeScript 在很短的時間內就得到了普及並被許多企業所實施，並有一個非常活躍的社群

## 型別系統(type system)
- 有三種方法可以去指定型別 分別為 型別推斷(Type Inference) 、 型別註解(Type Annotation) 、 型別斷言(Type Assertions)

    1. 型別推斷(Type Inference)
    > e.g.  
    ```ts
    // TypeScript 能依據變數宣告的值來推斷型別
    let num = 6;
    num = "I am string"; // error
    ```

    2. 型別註解(Type Annotation)
    > e.g.  
    ```ts
    // TypeScript 透過能透過 冒號(:) 來手動指定型別
    let isHealth: boolean = true; // boolean variable
    let height: number = 162; // number variable
    let person: string = "Cindy"; // string variable
    ```

    3. 型別斷言(Type Assertions)
    > e.g.  
    ```ts
    // 語法1: 值 as 型別
    const num = 1 as number;

    // 語法2: <型別>值 *在 tsx 語法（React 的 jsx 語法的 ts 版）中無法使用
    const num = <number>1;
    ```
