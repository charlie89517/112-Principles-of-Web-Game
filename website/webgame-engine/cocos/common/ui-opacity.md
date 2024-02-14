
# UIOpacity 

可以透過該component調整UI的透明度，而實現半透明的效果。
與 **UITransform** 一樣是在 Cocos 3.X 時從 Node 中分離出來的屬性

![UIOpacity示例][UIOpacity Example]


## UIOpacity 屬性

| 屬性      | 功能說明   |
| ------------------- | ------------------- |
|**Opacity**|透明度`0-255`(透明)~(不透明)| 

## UIOpacity 範例

!!! note
    
    Parent Node 透明度被調整的話 會連帶影響到底下所有 Child Node

```ts
const opacityComp = this.getComponent(UIOpacity);
opacityComp.opacity = 157;
```

## 參考資訊

[Cocos Creator 官方－UIOpacity（透明度设置）组件参考](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/ui-opacity.html)

[Cocos Creator 官方－UIOpacity API](https://docs.cocos.com/creator/3.6/api/zh/class/UIOpacity)

[UIOpacity Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/uiopacity/ui-opacity.png "圖片來源：UIOpacity（透明度设置）组件参考"