
## **UIOpacity 介紹**

可以透過該component調整UI的透明度，而實現淡入淡出的效果

![ProgressBar示例](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/uiopacity/ui-opacity.png)


### **UIOpacity 屬性**

| 屬性                | 功能說明                                                    |
| ------------------- | ----------------------------------------------------------- |
|**Opacity**            |透明度`0-255`(透明)~(不透明)| 

### **UIOpacity 範例**

```ts
const opacityComp = this.getComponent(UIOpacity);
opacityComp.opacity = 157;
```

### **UIOpacity API**

[UIOpacity API](https://docs.cocos.com/creator/3.6/api/zh/class/UIOpacity)