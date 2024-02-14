# Slider

![Slider實例][Slider Example]

## Slider 屬性

| 屬性           | 功能說明                                    |
| -------------- | ------------------------------------------- |
| **Handle**     | Slider 上按鈕的 Sprite                      |
| **Direction**  | Slider 的方向，分為`Horizontal`和`Vertical` |
| **Progress**   | 當前的進度值，區間為`0~1`                   |
| **ClickEvent** | Slider 滑動事件的列表                       |

### Slider Event Structure

![Slider Event Structure][Slider Event Example]

| 屬性              | 功能說明                               |
| ----------------- | -------------------------------------- |
| `Target`          | 帶有 Script 的 Node                    |
| `Component`       | 該 Node 底下 Script 的名稱             |
| `Handler`         | Script 底下欲 call 的 function 名稱    |
| `CustomEventData` | 可以指定任意的字符作為最後一個參數傳入 |

!!!Warning

    建議此類的事件一率寫在Script中，盡量不要使用拉Node的方式在場景中綁定事件，方便Debug追蹤及後續維護。

## Slider 範例

```ts
import { _decorator, Component, Slider } from "cc";
const { ccclass, property } = _decorator;

@ccclass("SliderHandler")
export class SliderHandler extends Component {
  protected onLoad(): void {
    const slider = this.getComponent(Slider);
    if (slider) {
      slider.node.on("slide", this.onSliderChange, this);
    }
  }

  private onSliderChange(slider: Slider): void {
    // slider.progress表示的是目前slider所在位置對於整個bar的比例
    console.log("Slider value:", slider.progress);
  }
}
```

## 參考資訊

[Cocos Creator 官方－Slider](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider.html?h=slider)

[Cocos Creator 官方－Slider API](https://docs.cocos.com/creator/3.6/api/zh/class/Slider)

[Slider Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider/slider-inspector.png "圖片來源 : Slider 组件参考"
[Slider Event Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider/slider-event.png "圖片來源 : Slider 组件参考"
