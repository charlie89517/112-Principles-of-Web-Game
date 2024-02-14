# **Dynamic load**

## **Dynamic load**

需要動態載入的資源必須放在 **resources** 資料夾或它的子資料夾下，配合 **resources.load** 等 interface 來動態載入。

> **resources** 資料夾需要在 **assets** 目錄下手動新建

<figure>
    <img src="/webgame-engine/assets/dynamic-load/resources-file-tree.png" alt="resources資料夾"/>
    <figcaption>resources資料夾<sup id="fnref:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

### **載入 SpriteFrame 或 Texture2D**

圖片設定為 **sprite-frame** 或 **texture** 或其他圖片類型後，將會在 **資源管理器** 中產生一個對應類型的資源。 但如果直接載入 **test_assets/image** ，得到的類型將會是 **ImageAsset**。必須指定路徑到具體的子資源，才能載入到圖片產生的 **SpriteFrame** 或 **Texture2D** 。

> test_assets/image 是載入 ImageAsset ， test_assets/image/spriteFrame 是載入 SpriteFrame ， test_assets/image/texture 是載入 Texture2D

```TypeScript
// 載入 SpriteFrame
resources.load('test_assets/image/spriteFrame', SpriteFrame, (err, spriteFrame) => {
    this.node.getComponent(Sprite).spriteFrame = spriteFrame;
});
```

```TypeScript
// 載入 Texture2D
resources.load('test_assets/image/texture', Texture2D, (err, texture) => {
    const spriteFrame = new SpriteFrame();
    spriteFrame.texture = texture;
    this.node.getComponent(Sprite).spriteFrame = spriteFrame;
});
```

> 如果指定了類型參數(第二個參數)，就會在路徑下尋找指定類型的資源。 在同一個路徑下同時包含了多個重名資源(例如同時包含 **player.clip** 和 **player.psd** )就需要宣告類型。 當你需要取得 **子資源**(例如取得 ImageAsset 的子資源 SpriteFrame )，就需要指定子資源的路徑。

### **載入 圖集(SpriteAtlas) 中的 SpriteFrame**

對從 **TexturePacker** 等第三方工具匯入的圖集而言，如果要載入其中的 **SpriteFrame**，則只能先載入圖集，再取得其中的 **SpriteFrame**。

```TypeScript
// 載入SpriteAtlas(圖集)，並取得其中一個SpriteFrame
// 注意atlas資源(plist)通常會和一個同名的圖片文件(png) 放在同一個目錄下，所以要在第二個參數指定資源類型
resources.load('test_assets/sheep', SpriteAtlas, (err, atlas) => {
    const frame = atlas.getSpriteFrame('sheep_down_0');
    this.node.getComponent(Sprite).spriteFrame = frame;
});
```

### **載入 FBX 或 glTF 模型中的資源**

將 FBX 模型或 glTF 模型匯入編輯器後，會解析出該模型中包含的相關資源如網格、材質、骨骼、動畫等，如下圖所示：

<figure>
    <img src="/webgame-engine/assets/dynamic-load/model.png" alt="model"/>
    <figcaption>模型解析<sup id="fnref:note1"><a class="footnote-ref" href="#fn:note1" role="doc-noteref">1</a></sup></figcaption>
</figure>

你可以在執行時動態載入模型中的單一資源，只需指定到某個具體子資源的路徑即可。

```TypeScript
// 載入模型中的網格資源
resources.load('Monster/monster', Mesh, (err, mesh) => {
    this.node.getComponent(MeshRenderer).mesh = mesh;
});

// 載入模型中的材質資源
resources.load('Monster/monster-effect', Material, (err, material) => {
    this.node.getComponent(MeshRenderer).material = material;
});

// 載入模型中的骨骼資源
resources.load('Monster/Armature', Skeleton, (err, skeleton) => {
    this.node.getComponent(SkinnedMeshRenderer).skeleton = skeleton;
});
```

### **資源批量載入**

**resources.loadDir** 可以載入相同路徑下的多個資源。

```TypeScript
// 載入 test_assets 目錄下所有資源
resources.loadDir('test_assets', (err, assets) {
    // ...
});

// 載入 test_assets 目錄下所有 SpriteFrame，並且取得它們的路徑
resources.loadDir('test_assets', SpriteFrame, (err, assets) {
    // ...
});
```

[^note1]: [Cocos Creator 3.6 Manual - Asset Loading](https://docs.cocos.com/creator/3.6/manual/en/asset/dynamic-load-resources.html)
