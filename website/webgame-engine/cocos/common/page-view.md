## **PageView 介紹**

PageView 是一種頁面的容器

![PageViewUI範例](/webgame-engine/assets/cocos/common/PageView/PageViewUIExample.PNG)

### **PageView 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**SizeMode**| 頁面檢視中每個頁面大小類型，選項包括 `Unified` 和 `Free` 類型 |
|**Content**| 它是一個Node的引用，用來創建 PageView 的可捲動內容 |
|**Direction**| Page滾動的方向 |
|**ScrollThreshold**| Page滾動的方向 |
|**Brake**| `0~1`，開啟慣性後，在使用者停止觸摸後滾動多快停止，0 表示永不停止，1 表示立刻停止 |
|**BounceDuration**| 回彈所需的時間。取值範圍是`0-10` |
|**AutoPageTurningThreshold**| 選項包括`NONE`、`COLOR`、`SPRITE `、`SCALE` |
|**Inertia**| Toggle按鈕點擊事件的列表 |