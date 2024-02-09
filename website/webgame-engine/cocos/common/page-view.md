## **PageView 介紹**

PageView 是一種頁面的容器

![PageViewUI範例](/webgame-engine/assets/cocos/common/PageView/PageViewUIExample.PNG)

### **PageView 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**SizeMode**| 決定 PageView 的尺寸模式，通常有`Unified`和`Free`兩種選擇，Unified模式會根據設定統一調整 PageView 的子頁面大小，而Free模式則允許自由設定每個子頁面的大小 |
|**Content**| 它是一個Node的引用，用來創建 PageView 的可捲動內容 |
|**Direction**| 設定 PageView 的滾動方向，選項包括`HORIZONTAL`、`VERTICAL` |
|**ScrollThreshold**| 滾動閾值，設定用戶在滑動時需要超過多少距離，PageView 才會判斷為有效的頁面滾動操作 |
|**AutoPageTurningThreshold**| 自動翻頁閾值，當用戶的滑動速度超過這個值時，PageView 將會自動滾動到下一頁或上一頁 |
|**Inertia**| 是否啟用慣性滾動 |
|**Brake**| 剎車系數，當慣性滾動啟用時，這個值決定滾動的減速程度，範圍是`0（無剎車）`到`1（立即停止）` |
|**Elastic**| 是否啟用回彈效果，當設為 `true` 時， PageView 滾動到邊界時會有彈性回彈 |
|**BounceDuration**| 回彈持續時間，設定回彈效果的持續時間，數值範圍是`0-10` |
|**Indicator**| PageView 的頁面指示器，顯示當前所在頁面的位置以及總頁面數 |
|**PageTurningEventTiming**| 這個屬性決定頁面翻轉需要花多少時間，單位為 sec |
|**CancelInnerEvents**| 設為`true`時，滑動content時會取消子節點上註冊的 Touch 事件，避免誤觸 |

#### **PageView Event Structure**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Target**| 事件目標是指當事件發生時應該執行哪個節點上的腳本 |
|**Component**| 填入腳本組件的名稱，這個腳本包含了將要執行的事件處理函數 |
|**Handler**| 選擇當事件發生時，應該使用的函數或方法 |
|**CustomEventData**| 欄位通常是用來傳遞一個 string |

!!!tip
    建議此類的事件一率寫在 script 中，不要使用拉 Node 的方式綁定事件，方便 debug
    
### **PageView 範例**

![PageViewEvent範例](/webgame-engine/assets/cocos/common/PageView/PageViewEventExample.PNG)

```ts
import { _decorator, Component, PageView } from 'cc';
const { ccclass, property } = _decorator;

@ccclass("PageViewHandler")
export class PageViewHandler extends Component {
  onLoad() {
    // 註冊PAGE_TURNING時call onPageTurning
    this.node.on(PageView.EventType.PAGE_TURNING, this.onPageTurning, this);
  }

  onPageTurning(pageView: PageView) {
    console.log("PAGE_TURNING")
  }
}
```

### **PageView API**

[PageView API](https://docs.cocos.com/creator/3.6/api/zh/class/PageView)

### REF

[PageView 组件参考](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/pageview.html?h=page)