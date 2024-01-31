
## 介紹

![Button實例](../../assets/ButtonUIExample.png)

### 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Target**| 點擊時需要作用於哪個目標 |
|**Interactable**| Button 是否可以互動 |
|**Transition**| 選項包括`NONE`、`COLOR`、`SPRITE `、`SCALE` |
|**ClickEvent**| Button按鈕點擊事件的列表 |

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
| `Normal` | Button 在 `Normal` 狀態底下要顯示的Sprite。|
| `Pressed` | Button 在 `Pressed` 狀態底下要顯示的Sprite。|
| `Hover` | Button 在 `Hover` 狀態底下要顯示的Sprite。|
| `Disabled` | Button 在 `Disabled` 狀態底下要顯示的Sprite。|

##### Scale Transition 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Duration` | 	Button 狀態切換所需要的時間。|
| `ZoomScale` | 當Button被點擊後，Button會縮放到一個值，這個值等於 Button 的 scale * `zoomScale`，`zoomScale` 可以 < 0。|

#### Button ClickEvent

Button 只支援 Click 事件，每當被點擊後放開後才會使用相對應Function。

##### Button Event Structure

![Event Structure](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/button/button-event.png)

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Target` | 带有Script的Node。|
| `Component` | 該Node底下Script的名稱。|
| `Handler` | Script底下欲call的function名稱。|
| `CustomEventData` | 可以指定任意的字符作為最後一個參數傳入。 |

### API
[Button API](https://docs.cocos.com/creator/3.6/api/zh/class/Button)
