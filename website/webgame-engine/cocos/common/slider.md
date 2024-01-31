
## 介紹

![Slider實例](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider/slider-inspector.png)

### 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Handle**| Slider上按鈕的Sprite |
|**Direction**| Slider的方向，分為`Horizontal`和`Vertical` |
|**Progress**| 當前的進度值，區間為`0~1` |
|**ClickEvent**| Slider滑動事件的列表 |

##### Slider Event Structure

![Event Structure](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/slider/slider-event.png)

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Target` | 带有Script的Node。|
| `Component` | 該Node底下Script的名稱。|
| `Handler` | Script底下欲call的function名稱。|
| `CustomEventData` | 可以指定任意的字符作為最後一個參數傳入。 |

### API
[Button API](https://docs.cocos.com/creator/3.6/api/zh/class/Slider)