# Prettier & ESLint

## Prettier 是什麼？

Prettier 是一個程式碼格式化工具，解決了程式碼排版的爭議。它可以在瞬間將程式碼按照約定好的格式自動排版，提高團隊合作的效率。

## ESLint 是什麼？

ESLint 是 JavaScript 的 Linter 工具，用於即時發現程式碼品質問題。它可以檢查語法錯誤、潛在的錯誤和風格問題，確保程式碼符合一定的標準。

## 如何在 VSCode 中使用 Prettier

1. 打開 VSCode，進入 Extension。

   ![Prettier Extension](/webgame-engine/assets/coding-style/prettier.png)

2. 在搜尋欄位搜尋`Prettier`，並點擊選項右方的 Install

   ![Prettier Extension](/webgame-engine/assets/coding-style/prettier2.png)

3. 開啟 Setting，並搜尋`Default Formatter`

   ![Prettier Extension](/webgame-engine/assets/coding-style/prettier3.png)

   ![Prettier Extension](/webgame-engine/assets/coding-style/prettier5.png)

4. 找到 Default Formatter 設定並將其更改為`Prettier`選項

   ![Prettier Extension](/webgame-engine/assets/coding-style/prettier6.png)

5. 下載[`.prettierrc設定檔`](/webgame-engine/assets/coding-style/.prettierrc)，並將其放置於專案的根目錄中。如檔案名稱開頭少了`.`，請手動重新命名為`.prettierrc`

   ![Prettier Extension](/webgame-engine/assets/coding-style/prettier8.png)

現在，每次編輯完檔案時，可以右鍵程式碼編輯視窗並選擇`Format Document`來使用 Prettier 格式化程式碼。

![Prettier Extension](/webgame-engine/assets/coding-style/prettier7.png)

## 如何在 VSCode 使用 ESLint

1.  在專案根目錄中使用控制台 npm 指令：安裝 ESLint：

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint15.png)

    ```bash
    npm install eslint --save-dev
    ```

2.  在專案根目錄中使用控制台 npm 指令

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint19.png)

    ```bash
    npm install eslint-config-airbnb-base
    ```

3.  在專案根目錄中使用控制台 npm 指令

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint20.png)

    ```bash
    npm install --save-dev eslint-config-prettier
    ```

4.  下載[`.eslintrc.js`](/webgame-engine/assets/coding-style/.eslintrc.js)，並將其放置於專案的根目錄中。如檔案名稱開頭少了`.`，請手動重新命名為`.eslintrc.js`

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint13.png)

5.  接著請於 VSCode 中的`Extensions`頁面中搜尋`ESLint`，並選擇`Install`

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint12.png)

6.  安裝完成後重啟 VSCode，之後可於`OUTPUT`頁面中的`ESLint`選項下檢查是否正常安裝

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint21.png)

7.  想查看 ESLint 提示，可以於 VSCode 中打開 Problems 視窗（Ctrl+Shift+M），檢查建議。
    現在，VSCode 中已配置 ESLint，將即時提醒你可能的語法錯誤和問題。
    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint11.png)

<!--

3. 在專案根目錄執行以下命令進行 ESLint 設定，根據提示進行配置，建議選擇 `To check syntax and find problems`。按`Enter`鍵來確認執行


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint2.png)


      ```bash
      npm init @eslint/config
      ```

4. 使用`Enter`鍵選擇`JavaScript modules (import/export)`選項


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint3.png)



5. 使用鍵盤`上下方向鍵(↑↓)`鍵選擇`None of these`選項


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint4.png)


6. 使用鍵盤`上下左右鍵(←→)`鍵選擇`Yes`選項


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint5.png)


7. 使用鍵盤`Enter`鍵確認默認選項


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint6.png)

8. 使用鍵盤`Enter`鍵確認默認選項


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint7.png)

9. 使用鍵盤`上下左右鍵(←→)`鍵選擇`Yes`選項


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint8.png)


10. 使用鍵盤`Enter`鍵確認默認選項`npm`即可開始初始設定


    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint9.png)

11. 完成設定後如下圖所示:

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint10.png) -->

<!-- 14. 將`"test": "echo \"Error: no test specified\" && exit 1"`修改為`"lint": "eslint ./"`

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint17.png)

    ![Prettier Extension](/webgame-engine/assets/coding-style/eslint18.png)

      ```bash
      "lint": "eslint ./"
      ``` -->

## Prettier 和 ESLint 的整合

Prettier 和 ESLint 都有能力格式化程式碼，可能會導致格式衝突。我們建議將格式化工作交給 Prettier，僅使用 ESLint 的 Code Quality Rule。確保 ESLint 的配置中選擇 `To check syntax and find problems`。

此方式可讓 Prettier 和 ESLint 合作，確保程式碼風格的一致性，同時提升程式碼品質。

以上，你已經學會如何使用 Prettier 和 ESLint 在 VSCode 中提高程式碼品質和一致性。希望這份教學對你有所幫助
