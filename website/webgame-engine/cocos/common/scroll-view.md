
# ScrollView

ScrollView 是一種具有捲動功能的容器，它提供一種方式可以在有限的顯示區域內瀏覽更多的內容。 通常 ScrollView 會與 Mask 元件搭配使用，同時也可以加入 ScrollBar 元件來顯示瀏覽內容的位置。

![ScrollView 呈現範例][ScrollView Example]

## ScrollView 屬性

![ScrollView UI範例](/webgame-engine/assets/cocos/common/ScrollView/ScrollViewUIExample.PNG)

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**content**| Node，用來創建 ScrollView 的可滾動內容，通常這可能是一個包含一張巨大圖片的節點。 |
|**Horizontal**| 是否允許橫向捲動 |
|**Vertical**| 是否允許縱向滾動 |
|**Inertia**| 捲動的時候是否有加速度 |
|**Brake**| 滾動後的減速係數。 範圍為 `0-1`，如果是 1 則立刻停止滾動，如果是 0，則會一直滾動到 `content` 的邊界 |
|**Elastic**| 是否會回彈 |
|**BounceDuration**| Float，回彈所需要的時間。範圍是 `0-10` |
|**HorizontalScrollBar**| 帶有 ScrollBar 的 Node，來顯示 content 在水平方向上的位置 |
|**VerticalScrollBar**|  帶有 ScrollBar 的 Node，來顯示 content 在垂直方向上的位置 |
|**ScrollEvents**| 滾動事件註冊列表 |

## ScrollView 範例

!!! note

    ScrollView 這種比較複雜的UI元件，建議右鍵新增直接選ScrollView，編輯器會直接建一個完整的範例出來，就不用在新增 Component 後還要手動新增 `content` 等相關節點。


```ts
import { _decorator, Component, ScrollView } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('ScrollViewHandler')
export class ScrollViewHandler extends Component {
  protected onLoad(): void {
    const scrollView = this.getComponent(ScrollView);
    // 尋找節點上是否有附加 ScrollView，有的話才監聽事件
    if (scrollView) {
      this.node.on(ScrollView.EventType.SCROLLING, this.onScrolling, this);
      this.node.on(ScrollView.EventType.TOUCH_UP, this.onTouchUP, this);
      this.node.on(ScrollView.EventType.SCROLL_BEGAN, this.onScrollBegan, this);
    }
  }

  private onScrolling(scrollView : ScrollView): void {
    console.log("scrolling");
  }

  private onTouchUP(scrollView: ScrollView): void {
    console.log("onTouch_UP");
  }

  private onScrollBegan(scrollView: ScrollView): void {
    console.log("ScrollBegan");
  }
}
```

## 參考資訊

[Cocos Creator 官方－ScrollView](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/scrollview.html?h=scroll)

[Cocos Creator 官方－ScrollView API](https://docs.cocos.com/creator/3.6/api/zh/class/ScrollView)


[ScrollView Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/scroll/scrollview-content.png "圖片來源 : ScrollView 组件参考"