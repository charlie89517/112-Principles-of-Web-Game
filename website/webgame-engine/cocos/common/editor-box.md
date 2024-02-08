
## **Editor box 介紹**

一個 UI 組件，可以讓用戶輸入文字的文本框。

### **Editor box 屬性**

| 屬性   | 功能說明 |
| --- | --- |
|**BackgroundImage**| 用於設置背景圖片，這個 Sprite 將會顯示在 EditBox 的後方 |
|**FontColor**| 用來設定輸入文字的顏色 |
|**FontSize**| 用來設定輸入文字的大小 |
|**InputFlag**| 輸入旗標，可以設定為密碼模式，在這種模式下輸入的文字會顯示為星號（例如在 Android 平台上） |
|**InputMode**| 輸入模式，可以設定為 `ANY` 表示任何類型的輸入，或者其他專門的類型，例如數字、郵件地址等 |
|**LineHeight**| 用來設定輸入文字的行距 |
|**MaxLength**| 用來設定輸入文字的行距 |
|**Placeholder**| 提示文字 |
|**PlaceholderFontColor**| 提示文字的顏色 |
|**PlaceholderFontSize**| 提示文字的字體大小 |
|**PlaceholderLabel**| 提示文字的 Label |
|**ReturnType**| 指定行動裝置上面回車按鈕的樣式 |
|**String**| 用來獲取或設定 EditBox 當前的輸入的內容 |
|**TabIndex**| 用修改 DOM 輸入元素的 tabIndex，這個屬性只有在 Web 上面修改才有意義 |
|**TextLabel**| 用來顯示用戶輸入文本的 Label 組件 |

### **Editor box 事件**

| 屬性   | 功能說明 |
| --- | --- |
|**Editing Did Began**| 點擊輸入框(focus)時觸發 |
|**Editing Did Ended**| 取消輸入(按下回車或是點擊其他地方)時觸發 |
|**Text Changed**| 每一次的文字變更都會觸發 |

!!!tip
    建議此類的事件一率寫在 script 中，不要使用拉 Node 的方式綁定事件，方便 debug

```ts
import { _decorator, Component, EditBox, Label } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('EditorboxHandler')
export class EditorboxHandler extends Component {
  @property(EditBox)
  private editBox : EditBox = null;
  @property(Label)
  private label : Label = null;

  onLoad(): void {
    this.editBox.node.on(EditBox.EventType.TEXT_CHANGED, this.onTextChanged);
    this.editBox.node.on(EditBox.EventType.EDITING_DID_BEGAN, this.onEditingBagan, this);
    this.editBox.node.on(EditBox.EventType.EDITING_DID_ENDED, this.onEditingEnded, this);
  }

  private onTextChanged(editbox: EditBox) {
    this.label.string = editbox.string === '' ?`甚麼也沒輸入`:`您輸入了:${editbox.string}`;
  }
  
  private onEditingBagan(editBox: EditBox) {
    this.label.string = `點選了EditBox，請輸入吧!`;
  }

  private onEditingEnded(editBox: EditBox) {
    this.label.string = `EditBox輸入完畢!`;
  }
}
```

### **Editor box API**

[EditBox API](https://docs.cocos.com/creator/3.6/api/zh/class/EditBox)

### REF

[EditBox 组件参考](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/editbox.html?h=edit)