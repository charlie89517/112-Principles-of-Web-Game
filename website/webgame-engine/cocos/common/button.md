
# Button

Cocos Creator 提供的基礎 UI Component，提供了一個按鈕所需的功能，如點擊事件及按鈕交互的變化。

## Button 屬性

![Button範例](/webgame-engine/assets/cocos/common/Button/ButtonUIExample.png)

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Target**| 目標Node，點擊時需要作用於哪個目標 |
|**Interactable**| 可互動性，這個決定了Button是否可以與用戶互動。<br/>當此屬性處於 `false` 時，會依據 `Transition` 設定顯示`Disabled`狀態 |
|**Transition**| 按鈕狀態表現方式，將按鈕分為四種狀態：<br/>預設`Normal`、懸停`Hover`、按下`Pressed`、禁用`Disabled`。<br/>表現方式的選項則包括：無效果`NONE`、顏色變化`COLOR`、換圖`SPRITE `、縮放`SCALE` |
|**ClickEvent**| Button 按鈕點擊事件的列表 |

## Button Transition

Button 的 Transition 用來指定 Button 時，不同狀態時要如何表現。目前主要有 `NONE`、`COLOR`、`SPRITE `、`SCALE`。

### Color Transition 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Normal` | Button 在 `Normal` 狀態底下的顏色。|
| `Pressed` | Button 在 `Pressed` 狀態底下的顏色。|
| `Hover` | Button 在 `Hover` 狀態底下的顏色。|
| `Disabled` | Button 在 `Disabled` 狀態底下的顏色。|

### Sprite Transition 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Normal` | Button 在 `Normal` 狀態底下要顯示的Sprite |
| `Pressed` | Button 在 `Pressed` 狀態底下要顯示的Sprite |
| `Hover` | Button 在 `Hover` 狀態底下要顯示的Sprite |
| `Disabled` | Button 在 `Disabled` 狀態底下要顯示的Sprite |

### Scale Transition 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Duration` | 	Button 狀態切換所需要的時間。|
| `ZoomScale` | 當 Button 被點擊後， Button 會縮放到一個值，這個值等於 Button 的 scale * `zoomScale`，`zoomScale` 可以 < 0。|

## Button Event

Button 提供了 `Click` 事件，在 Button 處於 `interactable = true` 時，被點擊時放開後才會被觸發。

### Button Event Structure

![ButtonEvent範例](/webgame-engine/assets/cocos/common/Button/ButtonEventExample.PNG)

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|`Target`| 事件目標是指當事件發生時應該執行哪個節點上的腳本 |
|`Component`| 依據指定的Target，列出要指定使用哪一個 `Component` |
|`Handler`| 選擇當事件發生時，`Component`應該使用的函數或方法 |
|`CustomEventData`| 用於提供給觸發的方法參數 |

當 CustomEventData = "Test" 時，實際使用的情境如下：
```ts
public onClick(event, data) {
  console.log(data); // 印出 Test
}
```

!!!Warning

    建議此類的事件一率寫在Script中，盡量不要使用拉Node的方式在場景中綁定事件，方便Debug追蹤及後續維護。

## Button 範例

![ButtonClickEvent範例](/webgame-engine/assets/cocos/common/Button/ButtonClickExample.gif)

```ts
import { _decorator, Component, Button } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('ButtonClickHandler')
export class ButtonClickHandler extends Component {
  protected onLoad(): void {
    const button = this.getComponent(Button);
    if (button) {
      button.node.on(Button.EventType.CLICK, this.onButtonClick, this);
    }
  }

  private onButtonClick(event: EventTouch): void {
    console.log('Button clicked!');
  }
}
```

## Button 使用技巧

### 按鈕的交互性（interactable）與事件處理

 - 當 Button 的 **interactable** 屬性設置為 `false` 時，按鈕將處於禁用（disabled）狀態，這意味著按鈕的 Transition 狀態也會變成 `disabled` ，並且 **Button.EventType.CLICK** 事件將不會被觸發。
 - 即使按鈕處於禁用狀態，一般的 **Node.EventType** 系列事件（如 **TOUCH_END**）仍然可以被觸發。
 - 通常，開發者會通過程式碼動態控制按鈕的 `interactable` 屬性（button.interactable = boolean）來啟用或禁用按鈕。
 - 當需要處理點擊事件時，建議優先使用 **Button.EventType.CLICK** 而非 **Node.EventType** 系列事件。

## 事件發泡與阻止機制

 - Button組件會阻止事件的進一步傳遞（事件發泡），意即當使用者點擊按鈕時，Button的父節點不會接收到該事件。

!!! Tip

    有些開發者利用這一特性，通過在需要禁止點擊的區域覆蓋一個透明按鈕來阻止事件的觸發，實際上Cocos有提供專用於處理這種需求的Component：`BlockInputEvents`。

## 參考資訊

[Cocos Creator 官方－Button](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/button.html)

[Cocos Creator 官方－Button API](https://docs.cocos.com/creator/3.6/api/zh/class/Button)
