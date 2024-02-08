
## **Slider 介紹**

![Slider實例][Slider Example]

### **Slider 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Handle**| Slider上按鈕的Sprite |
|**Direction**| Slider的方向，分為`Horizontal`和`Vertical` |
|**Progress**| 當前的進度值，區間為`0~1` |
|**ClickEvent**| Slider滑動事件的列表 |

#### **Slider Event Structure**

![Slider Event Structure][Slider Event Example]

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Target` | 帶有Script的Node |
| `Component` | 該Node底下Script的名稱 |
| `Handler` | Script底下欲call的function名稱 |
| `CustomEventData` | 可以指定任意的字符作為最後一個參數傳入 |

!!!tip
    建議此類的事件一率寫在 script 中，不要使用拉 Node 的方式綁定事件，方便 debug

### **Slider 範例**

```ts
import { _decorator, Component, Slider } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('SliderHandler')
export class SliderHandler extends Component {
  onLoad() {
    const slider = this.getComponent(Slider);
    if (slider) {
      slider.node.on('slide', this.onSliderChange, this);
    }
  }

  onSliderChange(slider: Slider) {
    // slider.progress表示的是目前slider所在位置對於整個bar的比例
    console.log('Slider value:', slider.progress);
  }
}
```

### **Slider API**

[Slider API](https://docs.cocos.com/creator/3.6/api/zh/class/Slider)

### REF

[Slider Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider/slider-inspector.png "圖片來源 : Slider 组件参考"

[Slider Event Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider/slider-event.png "圖片來源 : Slider 组件参考"

[Slider 组件参考](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider.html?h=slider)