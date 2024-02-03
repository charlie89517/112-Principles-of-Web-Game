# 嚴格模式

## 調用方式(Invoking)
>  
```js
"use strict";

/*
    main code
*/
```

## 行為改變(Behavior changes)
- 將下列 先前可接受的誤用(previously-accepted mistakes) 轉變為 錯誤(errors)
    - 不允許賦值給未宣告的變數
    - 不允許賦值給無法賦值的物件屬性
    - 不允許刪除掉無法刪除的物件屬性
    - 不允許參數名稱重複
    - 不允許舊的八進位文字(以0為前綴)
    - 不允許在基本型別的數值中設定屬性
    - 不允許重複的屬性名稱
- 簡化範圍管理(Simplifying scope management)
    - 不允許 `with` 語句
    - 不允許 `eval()` 在呼叫它的範圍內建立變數
    - 允許在區塊語句內宣告函式
- 簡化 `eval` 和 `arguments`
    - 將 `eval` 和 `arguments` 視為關鍵字
    - 設定 參數(parameters) 將不會同步更改 引數索引(arguments indices)
- 安全化 JavaScript
    - 如果未指定所屬物件，則函式中的 `this` 將傳回 `undefined`
    - 不允許遍歷堆疊的屬性