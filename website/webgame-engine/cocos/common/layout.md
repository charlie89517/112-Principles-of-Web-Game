
## 介紹

Layout 是用於 UI 容器節點的組件。這個組件為容器提供了各種布局功能，以自動排列所有子節點，從而使開發者可以方便地製作列表、頁面、網格容器等。

## 屬性

> 注意 : 屬性會跟著**Type**切換有所不同

| 屬性                | 功能說明                                                    |
| ------------------- | ----------------------------------------------------------- |
| **Type**              | 定義布局類型，包括無`NONE`、`HORIZONTAL`、`VERTICAL`、`GRID`|
| **ResizeMode**        | 容器大小調整模式，選項包括`NONE`、`CONTAINER`、`CHILDREN`。|
| **PaddingLeft**       | 容器中子物件左側邊距                                               |
| **PaddingRight**      | 容器中子物件右側邊距                                               |
| **PaddingTop**        | 容器中子物件上側邊距                                               |
| **PaddingBottom**     | 容器中子物件下側邊距                                               |
| **SpacingX**          | 子節點間的水平間隔。`NONE`模式無此属性                        |
| **SpacingY**          | 子節點間的垂直間隔。`NONE`模式無此属性                        |
| **HorizontalDirection** | 水平佈局時子節點的排列方向                                    |
| **VerticalDirection** | 垂直佈局時子節點的排列方向                                    |
| **CellSize**          | `GRID`中每個單元格的大小                                 |
| **StartAxis**         | `GRID`的起始軸線，影響排列順序                             |
| **AffectedByScale**   | 佈局是否受子節點縮放比例影響                                 |
| **AutoAlignment**     | 只在`HORIZONTAL`、`VERTICAL`生效，是否自動對齊子節點                           |
| **Constraint**        | 網格佈局中的約束類型，如固定行或固定列                           |
| **ConstraintNum**     | 約束數量，與**Constraint**屬性一起使用來指定行或列的數量           |

!!! note
    Layout 組件的某些屬性更改後會在下一幀更新，如果要立即更新請使用 `updateLayout` API

## Layout Type 



### Horizontal Layout

Layout Type 設置為 Horizontal 時，允許子元素橫向排列。當元素之間的距離或尺寸有變化時，Layout 組件可以自動調整，確保布局的整齊性。

- **AutoAlignment（自動對齊）**：在設定為橫向或縱向布局時有效，下圖為AutoAlignment取消後自行調整子節點的高度設定

![無使用AutoAlignment後可自行設定其高度](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/auto-layout/horizontal-no-align.png)

水平布局的幾個常見使用場景包括：

- 當容器尺寸改變時，子元素的寬度會根據 `ResizeMode` 的設定自動調整。
- 當子元素大小發生變化時，子元素的間距會根據 `ResizeMode` 的設定自動調整。
- 當子元素的 `Widget` 組件有變化時，子元素的位置會自動調整，保持布局的一致性。


### Vertical Layout

與Horizontal基本布局相似，不再贅述


## ResizeMode

接下來會展示**Type**與**ResizeMode**之間的關係，以下的介紹將以Type設定為`Horizontal`

### CHILDREN

會使Layout母節點的大小調整至與子節點的大小吻合[下左]

### CONTAINER

會使Layout子節點的大小調整至與母節點吻合[下右]

###

![ResizeMode實例](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/auto-layout/horizontal-resizemode.png)

施工ING
