# **Prefab**

## **Prefab**

Prefab 是一種可重複使用的物件模板。使用 Prefab 的好處在於，你可以在場景中多次使用相同的物件，而不必每次都從頭開始創建。這使得遊戲開發更加高效，特別是當你需要多次使用相同物件時，例如重複的敵人、道具或 UI 元素。

由 Prefab 資源(Prefab Asset)生成的實例(Prefab Instance)可以繼承模板的數據，又可以有自己客製化的數據修改。

### **建立 Prefab 資源**

有兩個方法能建立 Prefab 資源

1. **使用場景中現有節點建立**

   在場景中將節點編輯好之後，直接將節點從 **層級管理器** 拖曳到 **資源管理器** 中即可完成 Prefab 資源的建立。

   > 建立完成後，原節點將自動變成該 Prefab 資源的實例

   <figure>
       <img src="/webgame-engine/assets/prefab/create-prefab.gif" alt="使用場景中現有節點建立"/>
       <figcaption>使用場景中現有節點建立[^note1]</figcaption>
   </figure>

2. **於資源管理器建立**

   點選 **資源管理器** 左上方的 **+** 按鈕，選擇 **Node Prefab** 建立；

   或於 **資源管理器** 空白處右鍵，選擇 **create** ，然後選擇 **Node Prefab** 。

   <figure>
       <img src="/webgame-engine/assets/prefab/create.png" alt="於資源管理器建立"/>
       <figcaption>於資源管理器建立[^note1]</figcaption>
   </figure>

### **使用 Prefab 資源**

將 Prefab 資源從 **資源管理器** 拖曳到 **層級管理器** 或 **場景編輯器** ，即可在場景中產生一個 Prefab 實例。

> 使用 Prefab 資源建立的實例在 **層級管理器** 中以綠色呈現

<figure>
    <img src="/webgame-engine/assets/prefab/use-prefab.gif" alt="使用Prefab資源"/>
    <figcaption>使用Prefab資源<sup id="fnref3:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

### **編輯 Prefab 資源**

在 **資源管理器** 中按兩下 Prefab 資源即可從場景編輯模式切換到 Prefab 編輯模式(左上方顯示 PREFAB 字樣)。

編輯完成後，點選左上方的 **Save** 按鈕即可儲存編輯後的 Prefab 資源，之後點選 **Close** 按鈕即可返回場景編輯模式。

> 對 Prefab 資源的更動會套用至所有關聯的實例。

<figure>
    <img src="/webgame-engine/assets/prefab/prefab-edit-mode.gif" alt="編輯Prefab資源"/>
    <figcaption>編輯Prefab資源<sup id="fnref4:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

### **編輯 Prefab 實例**

#### 通用操作

在 **層級管理器** 中點選 Prefab 實例，**屬性檢查器** 的頂部會出現 5 個可操作的按鈕。

<figure>
    <img src="/webgame-engine/assets/prefab/edit-prefab.png" alt="Prefab實例通用操作"/>
    <figcaption>Prefab實例通用操作</figcaption>
</figure>

| 按鈕                                                        | 說明                                                                                   |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| ![edit](/webgame-engine/assets/prefab/edit.png)             | 進入 Prefab 資源編輯模式。                                                             |
| ![disconnect](/webgame-engine/assets/prefab/disconnect.png) | 斷開實例與 Prefab 資源的關聯。                                                         |
| ![location](/webgame-engine/assets/prefab/location.png)     | 在 **資源管理器** 中標示出 Prefab 資源的位置。                                         |
| ![reset](/webgame-engine/assets/prefab/reset.png)           | 將目前實例還原回與 Prefab 資源相同，其中名字、位置和旋轉不會還原回 Prefab 資源的設定。 |
| ![update](/webgame-engine/assets/prefab/update.png)         | 將目前實例的所有資料更新到所關聯的 Prefab 資源。                                       |

#### 新增節點

在 Prefab 實例下新增節點，在節點名字的右側會有一個 **+** ，它的資料儲存在 Prefab 實例下，不會影響關聯的 Prefab 資源的資料。

<figure>
    <img src="/webgame-engine/assets/prefab/prefab-mounted-children.png" alt="Prefab實例新增節點"/>
    <figcaption>Prefab實例新增節點<sup id="fnref5:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

#### 新增元件

在 Prefab 實例下新增元件，在元件名字的右側會有一個 **+** ，它的資料儲存在 Prefab 實例下，不會影響關聯的 Prefab 資源的資料。

<figure>
    <img src="/webgame-engine/assets/prefab/instance-add-component.png" alt="Prefab實例新增元件"/>
    <figcaption>Prefab實例新增元件<sup id="fnref6:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

#### 刪除元件

在 Prefab 實例下刪除包含在 Prefab 資源的元件，會在 **屬性檢查器** 上增加一條刪除元件的資料，它的資料儲存在 Prefab 實例下，不會影響關聯的 Prefab 資源的資料。

<figure>
    <img src="/webgame-engine/assets/prefab/instance-remove-component.png" alt="Prefab實例刪除元件"/>
    <figcaption>Prefab實例刪除元件</figcaption>
</figure>

同時在這個被刪除的資料後面出現以下兩個按鈕

| 按鈕                                                | 說明                                     |
| --------------------------------------------------- | ---------------------------------------- |
| ![reset](/webgame-engine/assets/prefab/reset.png)   | 在實例中還原刪除的元件。                 |
| ![update](/webgame-engine/assets/prefab/update.png) | 將該刪除的元件在 Prefab 資源中同步刪除。 |

## **NodePool**

NodePool 用於節點管理，將不需要的節點回收進 NodePool，需要使用時再從 NodePool 中取出，避免反覆創建及銷毀。

NodePool 特別適用於節點需要反覆創建和刪除的場景，如遊戲中的子彈。

> 不同的節點應使用不同的 NodePool 存放

### **使用方法**

- import

  ![import](/webgame-engine/assets/prefab/NodePool/import.png)

- constructors

  ![constructors](/webgame-engine/assets/prefab/NodePool/constructors.png)

- .get()

  從 NodePool 中取出節點。

  > 如果 NodePool 中沒有節點則回傳 null。

  ![get()](/webgame-engine/assets/prefab/NodePool/get.png)

- .put()

  將節點放入 NodePool 中。

  ![put()](/webgame-engine/assets/prefab/NodePool/put.png)

- .size()

  取得目前 NodePool 中的節點數量。

  ![size()](/webgame-engine/assets/prefab/NodePool/size.png)

- .clear()

  清空 NodePool 中的節點。

  ![clear()](/webgame-engine/assets/prefab/NodePool/clear.png)

[^note1]: [Cocos Creator 3.6 Manual - Prefab](https://docs.cocos.com/creator/3.6/manual/en/asset/prefab.html)
[^note2]: [Cocos Creator 3.6 API - NodePool](https://docs.cocos.com/creator/3.6/api/en/class/NodePool)
