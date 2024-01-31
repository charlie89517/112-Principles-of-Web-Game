
## 介紹

![Toggle實例](../../assets/ToggleUIExample.png)

### 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Target**| 點擊時需要作用於哪個目標 |
|**Interactable**| Toggle 是否可以互動 |
|**isChecked**| Toggle 當下的狀態 |
|**CheckMark**| 圖片，**isChecked**為`True`時要顯示的圖片 |
|**Transition**| 選項包括`NONE`、`COLOR`、`SPRITE `、`SCALE` |
|**CheckEvent**||

> 注意 : 因為Toggle繼承自Button，所以其他屬性請移駕至 Button

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
| `ZoomScale` | 當Button被點擊後，Button會縮放到一個值，這個值等於 Button 的 scale * `zoomScale`，`zoomScale` 可以 < 0|

### API 文件

[Toggle API](https://docs.cocos.com/creator/3.6/api/zh/class/Toggle)