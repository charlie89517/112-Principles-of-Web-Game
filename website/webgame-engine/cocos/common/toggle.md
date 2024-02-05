
## **Toggle 介紹**

![Toggle實例](/webgame-engine/assets/cocos/common/Toggle/ToggleUIExample.png)

### **Toggle 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Target**| 點擊時需要作用於哪個目標 |
|**Interactable**| Toggle 是否可以互動 |
|**isChecked**| Toggle 當下的狀態 |
|**CheckMark**| 圖片，**isChecked**為`True`時要顯示的圖片 |
|**Transition**| 選項包括`NONE`、`COLOR`、`SPRITE `、`SCALE` |
|**CheckEvent**| Toggle按鈕點擊事件的列表 |

> 注意 : 因為Toggle繼承自Button，所以其他屬性請移駕至 [Button](button.md)

### **Toggle 範例**

![Toggle Example](/webgame-engine/assets/cocos/common/Toggle/Toggle.gif)

```ts
import { _decorator, Component, Toggle } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('ToggleHandler')
export class ToggleHandler extends Component {
  onLoad() {
    const toggle = this.getComponent(Toggle);
    if (toggle) {
      toggle.node.on('toggle', this.onToggleChange, this);
    }
  }

  onToggleChange(toggle: Toggle) {
    const isChecked = toggle.isChecked;
    console.log('Toggle state changed:', isChecked ? 'On' : 'Off');
  }
}
```

### **Toggle API**

[Toggle API](https://docs.cocos.com/creator/3.6/api/zh/class/Toggle)