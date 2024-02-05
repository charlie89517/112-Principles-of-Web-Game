
## **Button 介紹**

![Button範例](/webgame-engine/assets/cocos/common/Button/ButtonUIExample.png)

### **Button 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Target**| 目標Node，點擊時需要作用於哪個目標 |
|**Interactable**| 可互動性，這個決定了Button是否可以與用戶互動 |
|**Transition**| 選項包括`NONE`、`COLOR`、`SPRITE `、`SCALE` |
|**ClickEvent**| Button 按鈕點擊事件的列表 |

#### Button Transition

Button 的 Transition 用來指定 Button 時，不同狀態時要如何表現。目前主要有 `NONE`、`COLOR`、`SPRITE `、`SCALE`。

##### Color Transition 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Normal` | Button 在 `Normal` 狀態底下的顏色。|
| `Pressed` | Button 在 `Pressed` 狀態底下的顏色。|
| `Hover` | Button 在 `Hover` 狀態底下的顏色。|
| `Disabled` | Button 在 `Disabled` 狀態底下的顏色。|

##### Sprite Transition 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Normal` | Button 在 `Normal` 狀態底下要顯示的Sprite |
| `Pressed` | Button 在 `Pressed` 狀態底下要顯示的Sprite |
| `Hover` | Button 在 `Hover` 狀態底下要顯示的Sprite |
| `Disabled` | Button 在 `Disabled` 狀態底下要顯示的Sprite |

##### Scale Transition 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Duration` | 	Button 狀態切換所需要的時間。|
| `ZoomScale` | 當 Button 被點擊後， Button 會縮放到一個值，這個值等於 Button 的 scale * `zoomScale`，`zoomScale` 可以 < 0。|

#### Button ClickEvent

Button 只支援 Click 事件，每當被點擊後放開後才會使用相對應Function。

##### Button Event Structure

![ButtonEvent範例](/webgame-engine/assets/cocos/common/Button/ButtonEventExample.PNG)

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|`Target`| 事件目標是指當事件發生時應該執行哪個節點上的腳本 |
|`Component`| 填入腳本組件的名稱，這個腳本包含了將要執行的事件處理函數 |
|`Handler`| 選擇當事件發生時，應該使用的函數或方法 |
|`CustomEventData`| 欄位通常是用來傳遞一個 string |

### **Button 範例**

![ButtonClickEvent範例](/webgame-engine/assets/cocos/common/Button/ButtonClickExample.gif)

```ts
import { _decorator, Component, Button } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('ButtonClickHandler')
export class ButtonClickHandler extends Component {
  onLoad() {
    const button = this.getComponent(Button);
    if (button) {
      button.node.on(Button.EventType.CLICK, this.onButtonClick, this);
    }
  }
  onButtonClick() {
    console.log('Button clicked!');
  }
}
```

### **Button API**

[Button API](https://docs.cocos.com/creator/3.6/api/zh/class/Button)
