# JavaScript 與 TypeScript 環境安裝

## JavaScript

### 簡介
JavaScript 是一種高級、直譯式的程式語言，廣泛用於網頁開發，提供互動性和動態性。

### 安裝
不需要單獨安裝 JavaScript，瀏覽器（如 Chrome、Firefox、或 Edge）即可執行 JavaScript 代碼。

## Node.js

### 簡介
Node.js 是基於 Chrome V8 引擎的 JavaScript 開發的伺服器端運行環境。

### 安裝
1. 首先下載[Node.js](https://nodejs.org/en/download)安裝包並執行，選擇"Next"
![Image title](/website/webgame-engine/assets/javascript/nodejs.png)
![Image title](/website/webgame-engine/assets/javascript/nodejs2.png)
2. 直接勾選左下方的同意框後並選擇"Next"
![Image title](/website/webgame-engine/assets/javascript/nodejs3.png)
3. 繼續多次選擇"Next"以繼續安裝步驟
![Image title](/website/webgame-engine/assets/javascript/nodejs4.png)
![Image title](/website/webgame-engine/assets/javascript/nodejs5.png)
![Image title](/website/webgame-engine/assets/javascript/nodejs6.png)
4. 選擇"Install"以安裝
![Image title](/website/webgame-engine/assets/javascript/nodejs7.png)
5. 待安裝執行完畢後選擇"Finish"以結束安裝
![Image title](/website/webgame-engine/assets/javascript/nodejs8.png)
6. 安裝完成後，可以使用 `node -v` 和 `npm -v` 檢查安裝版本。

## TypeScript

### 簡介
TypeScript 是 JavaScript 的超集，提供靜態類型和其他增強功能，讓程式碼更易於維護和開發。

### 安裝
在cmd中使用 npm 安裝 TypeScript：

```bash
npm install -g typescript
```

## Visual Studio Code (VSCode)

### 簡介
Visual Studio Code 是一個強大的輕量級程式碼編輯器，支援多種程式語言，並提供豐富的擴充功能。

### 安裝
1. 下載並安裝 [Visual Studio Code](https://code.visualstudio.com/).
2. 打開 VSCode，進入 Extensions（擴充功能）頁面，安裝 "JavaScript (ES6) code snippets"
![Image title](/website/webgame-engine/assets/javascript/vscode.png)
![Image title](/website/webgame-engine/assets/javascript/vscode2.png)


## 確認安裝
在終端或命令提示字元中，輸入以下指令確認安裝版本：

```bash
node -v
npm -v
tsc -v
```

若顯示版本號，即表示安裝成功。

現在，你已經設定好 JavaScript 與 TypeScript 的開發環境，並搭配 Visual Studio Code 和 Node.js 進行開發。
