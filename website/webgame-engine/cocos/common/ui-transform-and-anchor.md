# UITransform & Anchor

## UITransform

!!! Note

    此 Component 為Cocos Creator 3.0後從 Node 基礎屬性分離出來的，在 Cocos Creator 2.X 的版本時，此 Component 大多的介面皆為 Node 的一部分。

UITransform 同常會與其他渲染元件同時出現，主要提供兩項屬性

- **Content Size**：節點尺寸（寬、高）
- **Anchor Point**：錨點

![UITransform屬性](https://i.imgur.com/NpWLywr.png)

腳本可以透過以下程式碼抓取寬高等資訊，也可以反過來修改數值

```ts
private getUITransformInfo(): void {
  const component = this.getComponent(UITransform);
  console.log(`width: ${component.width} | height: ${component.height}`);
  console.log(`anchor point: (${component.anchorX}, ${component.anchorY})`);
}
```

除了上述兩項屬性外，UITranfrom 還提供了一些方便轉換坐標系統的方法

```ts
private onTouchStart(event: EventTouch) {
    const { x, y } = event.touch.getLocation(); // 點擊事件的位置，是一個世界坐標
    // 透過 `convertToNodeSpaceAR` 方法，將世界坐標轉為本地坐標系統
    const localPos = this.node.getComponent(UITransform).convertToNodeSpaceAR(v3(x, y));
    // 反過來將節點坐標(本地坐標系) 轉換為世界坐標
    // (與 this.node.worldPosition 相同功用，但可用於指定節點坐標以外的坐標進行轉換)
    const worldPos = this.node.getComponent(UITransform).convertToWorldSpaceAR(this.node.position);
}
```

## Anchor 錨點

Cocos 本地坐標系統將依據節點坐標及錨點來決定實際顯示位置，而 Anchor 代表的是 Node 的基準點，數值範圍是 左下角(0, 0) 至 右上角(1, 1)，預設情況下 Anchor 為 (0.5, 0.5)，位於節點中心位置，如下圖所示。
![Anchor範例](https://i.imgur.com/WxD7zMd.png)
