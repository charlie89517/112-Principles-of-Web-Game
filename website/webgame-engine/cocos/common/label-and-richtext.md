## **Label 介紹**

Label 組件用來顯示一段文字，文字可以是系統字體、TrueType 字體、BMFont 字體或藝術數字。另外，Label 還具有排版功能。

![Label 範例](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/label/label-property.png)

### **Label 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**CustomMaterial**| 自定義渲染材質，詳情請看官方文件 [自定義渲染材質](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/ui-material.html) |
|**Color**| 文字的顏色 |
|**String**| 欲顯示的內容 |
|**HorizontalAlign**| LableText中文字的水平對齊方式，選項包括`LEFT`、`CENTER`、`RIGHT` |
|**VerticalAlign**| LableText中文字的垂直對齊方式，選項包括`TOP`、`CENTER `、`BOTTOM` |
|**FontSize**| 字體大小 |
|**FontFamily**| 字型名稱，在使用系统字型時有效 |
|**LineHeight**| 行距 |
|**Overflow**| 文本的排版方式，選項包括`CLAMP`、`SHRINK`、`RESIZE_HEIGHT`，請參考官方文件[文字排版](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/label-layout.html) |
|**EnableWrapText**| 是否啟用換行（在**Overflow**設定為 `CLAMP`、`SHRINK` 時生效） |
|**Font**| 指定文本渲染需要的[字體資源](https://docs.cocos.com/creator/3.6/manual/zh/asset/font.html)。若要使用藝術數字字體，請參考[藝術數字資源](https://docs.cocos.com/creator/3.6/manual/zh/asset/label-atlas.html)進行設定。如果使用系統字體，則此屬性可以為空 |
|**UseSystemFont**| 是否使用系统字型 |
|**CacheMode**| 文本暫存類型，只對**系统字体** 或**TTF**字型有效，BMFont 字型不須設定。選項包括`NONE`、`BITMAP`、`CHAR`，詳情請參考[Cache Mode](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/label.html#%E6%96%87%E6%9C%AC%E7%BC%93%E5%AD%98%E7%B1%BB%E5%9E%8B%EF%BC%88cache-mode%EF%BC%89) |
|**IsBold**| 是否使用粗體，支援系统字型以及部分**TTF**字型。當**CacheMode**為`CHAR`時不生效 |
|**IsItalic**| 是否使用斜體，支援系统字型以及部分**TTF**字型。當**CacheMode**為`CHAR`時不生效 |
|**IsUnderline**| 是否使用底線，支援系统字型以及部分**TTF**字型。當**CacheMode**為`CHAR`時不生效 |

### **Label 範例**

```ts
import { _decorator, Component, RichText } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('RichTextHandler')
export class RichTextHandler extends Component {
  onLoad() {
    const richText = this.getComponent(RichText);
    if (richText) {
      richText.string = "Hello, <color=#00ff00>World</color>!";
    }
  }
}
```

### **Label API**

[Label API](https://docs.cocos.com/creator/3.6/api/zh/class/Label)

## **RichText 介紹**

RichText 組件用來顯示一段帶有不同樣式效果的文字，你可以通過一些簡單的 [BBCode 標籤](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/richtext.html#bbcode-%E6%A0%87%E7%AD%BE%E6%A0%BC%E5%BC%8F)來設置文字的樣式。目前支持的樣式有：顏色（color）、字體大小（size）、字體描邊（outline）、加粗（b）、斜體（i）、下劃線（u）、換行（br）、圖片（img）和點擊事件（on），並且不同的 BBCode 標籤是可以支持相互嵌套的。

![RichText 範例](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/richText/richtext.png)

### **RichText 屬性**

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| **String** | 欲顯示的內容 |
| **HorizontalAlign** | RichText中文字的水平對齊方式，選項包括`LEFT`、`CENTER`、`RIGHT` |
| **VerticalAlign** | RichText中文字的垂直對齊方式，選項包括`TOP`、`CENTER `、`BOTTOM` |
|**FontSize**| 字體大小，單位是point |
|**Font**| RichText字型，所有的label片段都會使用該TTF字型 |
|**FontFamily**| 字型名稱，在使用系统字型時有效 |
|**UseSystemFont**| 是否使用系统字型 |
|**MaxWidth**| RichText的最大寬度，填入0的話必須手動換行 |
|**LineHeight**| 行距，單位是point |
|**ImageAtlas**| 對於 img 標籤裡面的 src 屬性名稱，都需要在 imageAtlas 裡面找到一個有效的 spriteFrame，否則 img 標籤會判定為無效 |
|**HandleTouchEvent**| 選中此選項後，RichText 將阻止節點邊界框中的所有輸入事件（滑鼠和觸控），從而防止輸入事件穿透到底層節點 |

### **RichText 範例**

```ts
import { _decorator, Component, RichText } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('RichTextHandler')
export class RichTextHandler extends Component {
  onLoad() {
    const richText = this.getComponent(RichText);
    if (richText) {
      richText.string = "Hello, <color=#00ff00>World</color>!";
      richText.fontSize = 80;
      richText.lineHeight = 80;
    }
  }
}
```

### **RichText API**

[RichText API](https://docs.cocos.com/creator/3.6/api/zh/class/RichText)