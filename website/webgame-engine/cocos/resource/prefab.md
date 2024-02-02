# Prefab
## Prefab
Prefab是一種可重複使用的物件模板。使用Prefab的好處在於，你可以在場景中多次使用相同的物件，而不必每次都從頭開始創建。這使得遊戲開發更加高效，特別是當你需要多次使用相同物件時，例如重複的敵人、道具或UI元素。

由預製件資源(Prefab Asset)生成的實例(Prefab Instance)可以繼承模板的數據，又可以有自己客製化的數據修改。

### 建立預製件資源
有兩個方法能建立預製件資源

1. 使用場景中現有節點建立

    在場景中將節點編輯好之後，直接將節點從 **層級管理器** 拖曳到 **資源管理器** 中即可完成預製件資源的建立。

    >建立完成後，原節點將自動變成該預製件資源的實例

    <figure>
        <img src="/webgame-engine/assets/prefab/create-prefab.gif" alt="使用場景中現有節點建立"/>
        <figcaption>使用場景中現有節點建立[^note1]</figcaption>
    </figure>

2. 於資源管理器建立

    點選 **資源管理器** 左上方的 **+** 按鈕，選擇 **Node Prefab** 建立；
    
    或於 **資源管理器** 空白處右鍵，選擇 **create** ，然後選擇 **Node Prefab** 。

    <figure>
        <img src="/webgame-engine/assets/prefab/create.png" alt="於資源管理器建立"/>
        <figcaption>於資源管理器建立[^note1]</figcaption>
    </figure>

### 使用預製件資源
將預製件資源從 **資源管理器** 拖曳到 **層級管理器** 或 **場景編輯器** ，即可在場景中產生一個預製件的實例。

>使用預製件資源建立的實例在 **層級管理器** 中以綠色呈現

<figure>
    <img src="/webgame-engine/assets/prefab/use-prefab.gif" alt="使用預製件資源"/>
    <figcaption>使用預製件資源<sup id="fnref:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

### 編輯預製件資源
在 **資源管理器** 中按兩下預製件資源即可從場景編輯模式切換到預製件編輯模式(左上方顯示PREFAB字樣)。

編輯完成後，點選左上方的 **Save** 按鈕即可儲存編輯後的預製件資源，之後點選 **Close** 按鈕即可返回場景編輯模式。

> 對預製件資源的更動會套用至所有以該預製件資源產生的實例。

<figure>
    <img src="/webgame-engine/assets/prefab/prefab-edit-mode.gif" alt="編輯預製件資源"/>
    <figcaption>編輯預製件資源<sup id="fnref:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

### 編輯預製件實例

#### 通用操作
在 **層級管理器** 中點選預製件實例，**屬性檢查器** 的頂部會出現5個可操作的按鈕。

<figure>
    <img src="/webgame-engine/assets/prefab/edit-prefab.png" alt="預製件實例通用操作"/>
    <figcaption>預製件實例通用操作</figcaption>
</figure>

| 按鈕 | 說明 |
| ------ | ------ |
| ![edit](/webgame-engine/assets/prefab/edit.png)  | 進入預製件資源編輯模式。 |
| ![disconnect](/webgame-engine/assets/prefab/disconnect.png)  | 斷開實例與預製件資源的關聯。 |
| ![location](/webgame-engine/assets/prefab/location.png)  | 在 **資源管理器** 中標示出預製件資源的位置。 |
| ![reset](/webgame-engine/assets/prefab/reset.png)  | 將目前實例還原回與預製件資源相同，其中名字、位置和旋轉不會還原回預製件資源的設定。 |
| ![update](/webgame-engine/assets/prefab/update.png)  | 將目前實例的所有資料更新到所關聯的預製件資源。 |

#### 新增節點
在預製件實例下新增節點，在節點名字的右側會有一個 **+** ，它的資料儲存在預製件實例下，不會影響關聯的預製件資源的資料。

<figure>
    <img src="/webgame-engine/assets/prefab/prefab-mounted-children.png" alt="預製件實例新增節點"/>
    <figcaption>預製件實例新增節點<sup id="fnref:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>


#### 新增元件
在預製件實例下新增元件，在元件名字的右側會有一個 **+** ，它的資料儲存在預製件實例下，不會影響關聯的預製件資源的資料。

<figure>
    <img src="/webgame-engine/assets/prefab/instance-add-component.png" alt="預製件實例新增元件"/>
    <figcaption>預製件實例新增元件<sup id="fnref:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

#### 刪除元件
在預製件實例下刪除包含在預製件資源的元件，會在 **屬性檢查器** 上增加一條刪除元件的資料，它的資料儲存在預製件實例下，不會影響關聯的預製件資源的資料。

<figure>
    <img src="/webgame-engine/assets/prefab/instance-remove-component.png" alt="預製件實例刪除元件"/>
    <figcaption>預製件實例刪除元件</figcaption>
</figure>

同時在這個被刪除的資料後面出現以下兩個按鈕

| 按鈕 | 說明 |
| ------ | ------ |
| ![reset](/webgame-engine/assets/prefab/reset.png)  | 還原刪除的元件。 |
| ![update](/webgame-engine/assets/prefab/update.png)  | 將該刪除的元件在預製件資源中同步刪除。 |

## NodePool
 
![Example](/webgame-engine/assets/lfs.png)

[^note1]: [Cocos Creator 3.6 Manual - Prefab](https://docs.cocos.com/creator/3.6/manual/en/asset/prefab.html)
