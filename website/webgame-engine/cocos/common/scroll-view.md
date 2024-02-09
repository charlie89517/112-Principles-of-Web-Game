
## **ScrollView 介紹**

ScrollView 是一種具有捲動功能的容器，它提供一種方式可以在有限的顯示區域內瀏覽更多的內容。 通常 ScrollView 會與 Mask 元件搭配使用，同時也可以加入 ScrollBar 元件來顯示瀏覽內容的位置。

![ScrollView 呈現範例][ScrollView Example]
![ScrollView UI範例](/webgame-engine/assets/cocos/common/ScrollView/ScrollViewUIExample.PNG)

### **ScrollView 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**content**| Node，用來創建 ScrollView 的可滾動內容，通常這可能是一個包含一張巨大圖片的節點。 |
|**Horizontal**| 是否允許橫向捲動 |
|**Vertical**| 是否允許縱向滾動 |
|**Inertia**| 捲動的時候是否有加速度 |
|**Brake**| 滾動後的減速係數。 範圍為 `0-1`，如果是 1 則立刻停止滾動，如果是 0，則會一直滾動到 content 的邊界 |
|**Elastic**| 是否會回彈 |
|**BounceDuration**| Float，回彈所需要的時間。範圍是 `0-10` |
|**HorizontalScrollBar**| 帶有ScrollBar的Node，來顯示 content 在水平方向上的位置 |
|**VerticalScrollBar**|  帶有ScrollBar的Node，來顯示 content 在垂直方向上的位置 |
|**ScrollEvents**|  帶有ScrollBar的Node，來顯示 content 在垂直方向上的位置 |

#### **ScrollView Event Structure**

![ScrollView Event Structure](/webgame-engine/assets/cocos/common/ScrollView/ScrollViewEventExample.PNG)

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Target` | 帶有 Script 的 Node |
| `Component` | 該 Node 底下 Script 的名稱 |
| `Handler` | Script 底下欲 call 的 function 名稱 |
| `CustomEventData` | 可以指定任意的 string 作為最後一個參數傳入 |

### **ScrollView 範例**

!!! note
    類似ScrollView 這種比較複雜的UI元件，建議右鍵新增直接選ScrollView的話他會建一個完整的範例出來，就不用新增Component後還要手動新增節點


```ts
import { _decorator, Component, ScrollView } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('ScrollViewHandler')
export class ScrollViewHandler extends Component {
  onLoad() {
    const scrollView = this.getComponent(ScrollView);
    if (scrollView) {
      scrollView.node.on(ScrollView.EventType.SCROLLING, this.onScrolling, this);
      scrollView.node.on(ScrollView.EventType.TOUCH_UP, this.onTouchUP, this);
      scrollView.node.on(ScrollView.EventType.SCROLL_BEGAN, this.onScrollBegan, this);
    }
  }

  onScrolling(scrollView : ScrollView) {
    console.log("scrolling");
  }

  onTouchUP(scrollView: ScrollView) {
    console.log("onTouch_UP");
  }

  onScrollBegan(scrollView: ScrollView) {
    console.log("ScrollBegan");
  }
}
```

### ScrollView API
[Slider API](https://docs.cocos.com/creator/3.6/api/zh/class/ScrollView)

### REF

[ScrollView Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/scroll/scrollview-content.png "圖片來源 : ScrollView 组件参考"

[ScrollView 组件参考](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/scrollview.html?h=scroll)
