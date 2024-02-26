# 介紹

## Cocos Creator

Cocos Creator 是一款開源的跨平台遊戲引擎，它提供了一個全面的開發環境，包括場景編輯器、資源管理、測試和發佈等功能。Cocos Creator 內建了遊戲開發中常用的功能模組，讓開發者可以輕鬆地進行遊戲開發，專注於內容創造上。

![Cocos 生態](https://i.imgur.com/VXzxAmu.png)

<center>▲ Cocos Creator 生態[^1]</center>
<br/>

![Cocos Workflow](https://docs.cocos.com/creator/3.6/manual/zh/getting-started/introduction/structure-engine.png)

<center>▲ Cocos Creator 系統架構[^2]</center>

![Cocos Workflow](https://i.imgur.com/g5WQJD8.png)

<center>▲ Cocos Creator Workflow[^3]</center>

### 跨平台

Cocos Creator 提供跨平台的發布功能，開發者只需要開發一套系統，就可以達到全平台運行的目的。目前支援的平台主要如下：

- Web 平台
- Windows
- Android
- iOS
- MacOS
- 華為 HarmonyOS
- 各類小遊戲平台（淘寶小程序、微信小遊戲、Tiktok 小遊戲等）

!!! Note

    本堂課程僅關注於網頁 Web平台上，有關於跨平台發布的設定，可以自行參考[官方文件](https://docs.cocos.com/creator/3.6/manual/zh/editor/publish/)。

### 使用 Cocos 開發的遊戲

#### [Pokemon Master Ex](https://pokemonmasters-game.com/zh-TW)

![Pokemon Master Ex](https://i.imgur.com/bLd1vO6.png)

#### [Ubisoft Nano](https://nano.ubisoft.com/)

![Ubisoft Nano](https://i.imgur.com/0Qa514g.png)

#### [Paper Bride Series (紙嫁衣系列)](https://store.steampowered.com/app/2600570/5/)

![Paper Bride Series](https://i.imgur.com/uPkheJk.png)

#### 其他遊戲

![Paper Bride Series](https://download.cocos.com/CocosWww/2022/06/%E5%9B%BE%E7%89%871-1.png)

<center>▲ Cocos Creator 官方提供[^4]</center>

### 開源引擎

Cocos Creator 作為開源的遊戲引擎，允許開發者自行修改、擴充引擎功能，也開放開發者發送 PR，一同改善 Cocos 引擎！

目前 Cocos Creator 官方將引擎分為兩塊：Typescript 引擎 與 Native 引擎，開源於 [GitHub](https://github.com/cocos/cocos-engine/) & [Gitee](https://gitee.com/mirrors_cocos-creator/engine/) 上，詳細的修改方式可以參考[官方文件](https://docs.cocos.com/creator/3.6/manual/zh/advanced-topics/engine-customization.html)。

![開源引擎](https://i.imgur.com/8xQIy4n.png)

### 官方範例與開發資源

除了開源的引擎以外，Cocos Creator 官方亦提供一些簡單的遊戲範例與相關資源，幫助開發者能參考這些遊戲上手 Cocos 的開發。

有關於官方提供的資源，可以參考此篇[官方文件](https://docs.cocos.com/creator/3.6/manual/zh/getting-started/support.html)

## JavaScript & TypeScript

Cocos Creator 選擇 JavaScript / TypeScript 做為開發語言，而在 3.X 以上的版本時則主要以 TypeScript 進行開發，JavaScript 僅能作為[插件腳本](https://docs.cocos.com/creator/3.6/manual/zh/scripting/external-scripts.html)使用。

!!! Note

    雖然 Cocos Creator 2.X 版本可使用 JavaScript 編寫 Cocos Component，仍建議使用 TypeScript 進行開發，因為其擁有靜態型別檢查等特性，有利於 Debug 及後續維護。

開發 Cocos Creator 遊戲時，也可以使用 npm 安裝各種外部模組（如：[axios](https://www.youtube.com/watch?v=iw8ptzel4sk)、[encryptjs](https://www.npmjs.com/package/encryptjs) 等），但需要注意：npm 中不少 package 都依賴於 Node.js，無法使用於原生與網頁平台上。

!!! Warning

    有一部分的 npm package 可能是直接依賴於瀏覽器的 DOM API，會導致其可以正常運作於網頁平台上，但不能運作於原生平台中，選用 package 時應多加注意。

在 [JS/TS](/webgame-engine/learn-js/basic-and-syntax/) 的章節中，會針對這兩種語言進行較詳細的介紹。

[^1]: [36 氪 - 不只是游戏引擎，「Cocos 」完成 5000 万美元 B 轮融资](http://china.36kr.com/p/1693563623168896?column=%20&navId=24)
[^2]: [Cocos Creator 官方－系統架構](https://docs.cocos.com/creator/3.6/manual/zh/getting-started/introduction/#%E6%9E%B6%E6%9E%84%E7%89%B9%E8%89%B2)
[^3]: [Cocos Creator 官方－Workflow](https://docs.cocos.com/creator/3.6/manual/zh/getting-started/introduction/#%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B%E8%AF%B4%E6%98%8E)
[^4]: [Cocos Creator 官方－上架遊戲](https://www.cocos.com/post/54963e5ec11d76db476ac1c915c76dbf)
