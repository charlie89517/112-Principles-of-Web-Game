# Node & 坐標系統

在正式介紹 Node 之前，我們必須先來簡單提一下 Cocos 的場景編輯。

## 編輯器使用

### 層級管理器

以樹狀的方式表達場景上的節點層級，2D 遊戲在一般情況下，圖層順序將直接受層級影響。
以下圖為例，Logo 圖位於背景（Background）之上，而進度條 UI 則又位於 Logo 之上。
![層級管理器](https://i.imgur.com/xos6e0E.png)

<hr>
上方的輸入框可以以 Node名稱、Component名稱、UUID、資源UUID 等方式進行搜索

![指定搜索類別](https://i.imgur.com/dtZftBO.png){: style="height:200px"}
![搜索結果](https://i.imgur.com/V3SNeoP.png){: style="height:400px"}

!!! Note

    在Cocos中，每一個節點、資源都有其獨立的UUID，
    節點的UUID會記錄於場景的檔案中，資源則是放進Cocos專案時，編輯器會自動產生meta檔案，
    因此就算在資源未變動的情況下，擅自更動或移除meta檔，都會導致有使用到該資源的地方關聯發生問題，
    而發生 `missing asset` 或 `missing script` 的錯誤。

節點可透過拖拉的方式直接調整層級

![拖拉節點](https://i.imgur.com/UIcDp5l.gif)

### 資源管理器

顯示各項遊戲資源，**需存放於`/assets`中**

![資源管理器](https://i.imgur.com/HNw7yRw.png)

### 屬性檢查器

顯示並編輯當前選擇的節點屬性及組件

![屬性檢查器](https://i.imgur.com/GbHko1t.png)

### 預覽遊戲

- **方法 1：於網頁預覽**
- **方法 2：直接於編輯器預覽**
  ![預覽遊戲](https://i.imgur.com/yuwfbpw.png){: style="height:400px"}

!!! Note

    建議使用瀏覽器預覽進行Debug，因目前編輯器預覽較容易出現Bug，且瀏覽器上較方便Debug。

## Cocos 坐標系統

在 Cocos Creator 中，坐標系統主要使用的是`世界坐標系`和`節點坐標系`。
這兩種坐標系都是採用笛卡爾右手坐標，遵循 X 右正左負、Y 上正下負的規則，只差在坐標的基準點不同。

![坐標系統](https://i.imgur.com/DCkaYwZ.png)[^1]

### 本地(節點)坐標系

節點坐標系是相對於某個節點的相對坐標系統，該系統的基準點將依據節點的錨點（Anchor Point）而定，通常用於表示其子節點的的相對位置，Cocos 編輯器中屬性檢查器所顯示的 `Position` 即為本地坐標系。

!!! Info

    關於錨點 Anchor Point 的詳細介紹，可以參考[本篇](/webgame-engine/cocos/common/ui-transform-and-anchor.md)

### 世界坐標系

世界坐標系是整個遊戲場景的坐標系統，它是一個絕對的坐標系統，不受任何節點的影響。世界坐標系通常用來表示遊戲中的絕對位置，如畫面中的某個點在整個場景中的位置。

若腳本需要取得指定 Node 的世界坐標，Cocos 也有提供快速的方法直接取得，以下為一個簡單的範例：

```ts
// 印出指定節點的 相對坐標 與 世界坐標
private printNodePosition(node: Node): void {
  console.log(node.position); // 該 Node 相對於父節點的本地坐標
  console.log(node.worldPosition); // 該 Node 的世界坐標
}
```

### Vec3 Class

Vec3 是 Cocos 節點的 3 維向量類別，Node 的 `Position`、`Scale` 亦是以此類型表達，其最核心的 3 個屬性即為`x`, `y`, `z`，一般要創建 Vec3 實例的方式有以下幾種：

```ts
import { Vec3, v3 } from "cc";

const method1 = new Vec3(); // x:0, y:0, z:0
const method2 = new Vec3(1); // x:1, y:0, z:0
const method3 = new Vec3(1, 2); // x:1, y:2, z:0
const method4 = new Vec3(1, 2, 3); // x:1, y:2, z:3
// 語法糖 v3 ， 功用等效於 new Vec3
const method5 = v3(); // x:0, y:0, z:0
const method6 = v3(1, 2, 3); // x:1, y:2, z:3
```

Vec3 也提供一些簡易的向量計算方法，詳細的 API 可以參考 [官方文件](https://docs.cocos.com/creator/2.2/api/zh/classes/Vec3.html)

```ts
import { v3 } from "cc";

const position = v3(); // x:0, y:0, z:0
const position2 = v3(1, 1, 0); // x:1, y:1, z:0
position.add3f(1, 2, 3); // 運算結果: position = (1, 2, 3)
position.add(position2); // 運算結果: position = (2, 3, 3)
```

!!! Note

    除了`Vec3`以外，Cocos還提供了`Vec2`、`Vec4`，實際用法與`Vec3`大同小異，這邊就不多贅述。

### 坐標系的轉換

當腳本編寫時有遇到世界坐標、本地坐標系之間的轉換、甚至是兩個節點坐標間的轉換時，我們可以借助 UITransfrom 所提供的介面來快速的處理這些問題。

關於 UITransform 的詳細介紹，可以參考[本篇](/webgame-engine/cocos/common/ui-transform-and-anchor.md)

## Node 節點

- 組成場景的基礎單位
- 負責管理基本的轉換（Transform）屬性
- 每個節點之間是樹狀關係
- 可以掛載多個元件（Component）腳本

### Node 基礎屬性

- **Position**：坐標
- **Rotation**：旋轉
- **Scale**：縮放比例
- **Active**：激活狀態 (設為 `false` 時即不顯示 Node)
- 詳細的 API 可以參考 **[官方文件](https://docs.cocos.com/creator/3.6/api/zh/class/Node)**

### 創建新的 Node

在層級管理器中選擇 Node，點擊右鍵>創建，即可在選擇的 Node 下建立一個新的 Node。
![建立Node](https://i.imgur.com/N90d0s9.png)
建立出來的 Node 可以透過場景編輯器快速調整基礎屬性
![編輯場景上的Node](https://i.imgur.com/FDTETQs.gif)

### 透過腳本控制節點屬性

在編寫 Component 時，可能需要變更節點坐標、縮放等屬性，本章節僅會簡單示範 Node 基本屬性該如何用程式動態調整，Component 完整介紹請看 [Component 介紹](./basic.md)

```ts
// 取得節點目前坐標 (本地坐標系)
private getNodePosition(node: Node): Vec3 {
  return node.position;
}
// 調整節點坐標
private setNodePosition(node: Node, pos: Vec3): void {
  node.setPosition(pos); // 注意: 此處設定的坐標為本地坐標
  // 也可以用 node.setPosition(x, y, z);
}
// 取得節點目前是否處於激活狀態 (會被顯示出來)
private getNodeVisible(node: Node): boolean {
  return node.active;
}
// 設定節點是否處於激活狀態 (會被顯示出來)
private setNodeActive(node: Node, visible: boolean): void {
  node.active = visible;
}
// 取得節點目前2D旋轉角度
private getNodePosition(node: Node): number {
  return node.angle;
}
// 設定節點旋轉角度 (2D旋轉: 以Z軸為軸心)
private setNodeAngle(node: Node, angle: number): void {
  node.angle = angle;
}
// 取得節點目前3D旋轉角度
private getNodePosition(node: Node): Quat {
  return node.rotation;
}
// 設定節點旋轉角度
private setNodeRotation(node: Node, rotation: Quat): void {
  node.setRotation(rotation);
  // 也可以用 node.setRotation(x, y, z, w);
}
// 取得節點目前縮放比
private getNodeScale(node: Node): Vec3 {
  return node.scale;
}
// 設定節點縮放比
private setNodeRotation(node: Node, scale: Vec3): void {
  node.setScale(scale);
  // 也可以用 node.setScale(x, y, z);
}
```

!!! Warning

    因為Cocos將`position`、`scale`等屬性封裝為Readonly，因此當直接使用`node.scale = v3()`直接賦值時，編輯器會提示Error。

[^1]: Cocos 坐標系統: https://docs.cocos.com/creator/manual/zh/concepts/scene/coord.html
