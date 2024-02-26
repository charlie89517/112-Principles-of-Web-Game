# Toggle

Toggle 是繼承自 Button 的 UI Component，在 Button 的基礎上提供了選取（isChecked）狀態，常被用於開關按鈕。

## Toggle 屬性

![Toggle實例](/webgame-engine/assets/cocos/common/Toggle/ToggleUIExample.png)

| 屬性             | 功能說明                                    |
| ---------------- | ------------------------------------------- |
| **Target**       | 點擊時需要作用於哪個目標                    |
| **Interactable** | Toggle 是否可以互動                         |
| **isChecked**    | Toggle 當下的狀態                           |
| **CheckMark**    | 圖片，**isChecked**為`True`時要顯示的圖片   |
| **Transition**   | 選項包括`NONE`、`COLOR`、`SPRITE `、`SCALE` |
| **CheckEvent**   | Toggle 按鈕點擊事件的列表                   |

> 注意 : 因為 Toggle 繼承自 Button，所以其他屬性請移駕至 [Button](button.md)

## Toggle 範例

![Toggle Example](/webgame-engine/assets/cocos/common/Toggle/Toggle.gif)

```ts
import { _decorator, Component, Toggle } from "cc";
const { ccclass, property } = _decorator;

@ccclass("ToggleHandler")
export class ToggleHandler extends Component {
  protected onLoad(): void {
    const toggle = this.getComponent(Toggle);
    if (toggle) {
      // Toggle.EventType 也有提供從 Button 繼承過來的 CLICK
      toggle.node.on(Toggle.EventType.TOGGLE, this.onToggleChange, this);
    }
  }

  private onToggleChange(toggle: Toggle): void {
    const isChecked = toggle.isChecked;
    console.log("Toggle state changed:", isChecked ? "On" : "Off");
  }
}
```

## 參考資訊

[Cocos Creator 官方－Toggle](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/toggle.html?h=toggle)

[Cocos Creator 官方－Toggle API](https://docs.cocos.com/creator/3.6/api/zh/class/Toggle)
