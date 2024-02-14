# Sprite

Sprite 是 2D/3D 遊戲最常見的顯示圖像的方式，在節點上添加 Sprite 組件，就可以在場景中顯示項目資源中的圖片。

## Sprite 屬性

![Sprite實例](/webgame-engine/assets/cocos/common/Sprite/SpriteUIExample.PNG)

| 屬性               | 功能說明                                                                                                                                                                                                                                                 |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Type**           | 渲染模式，選項包括`Simple`、`Sliced`、`Tiled`、`Filled`四種模式。                                                                                                                                                                                        |
| **CustomMaterial** | 自定義 Material，使用方式參考官網[自定義 Material](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/ui-material.html)                                                                                                            |
| **Grayscale**      | 開啟後，使用灰階渲染                                                                                                                                                                                                                                     |
| **Color**          | 圖片的顏色                                                                                                                                                                                                                                               |
| **SpriteAtlas**    | 顯示的圖片所屬的圖集(參考[Atlas](https://docs.cocos.com/creator/3.6/manual/zh/asset/atlas.html))                                                                                                                                                         |
| **SpriteFrame**    | 渲染 Sprite 使用的 SpriteFrame 圖片資源                                                                                                                                                                                                                  |
| **SizeMode**       | 指定 Sprite 的尺寸： <br> `Trimmed` 表示會使用原始圖片資源裁剪透明像素後的尺寸 <br> `Raw` 表示會使用原始圖片未經裁剪的尺寸 <br> `Custom` 表示會使用自定義尺寸。當用戶手動修改過 Size 屬性後，Size Mode 會被自動設置為 `Custom`，除非再次指定為前兩種尺寸 |
| **Trim**           | 是否渲染原始圖像周圍的透明像素區域                                                                                                                                                                                                                       |

### Sprite Trim

- 在 Cocos 引擎中，為了優化資源的使用，引擎會自動裁掉 Sprite 圖片周圍的無用透明區域。
- 當 `Sprite` 的 `Trim` 設為 `false` 時，可以避免自動裁減的影響。

![Sprite Trim](https://i.imgur.com/y1fIisZ.png)

!!!Warning

    這樣的自動裁剪可能會造成圖片在遊戲中對位錯誤，尤其是在進行逐幀動畫時，因為每一幀的尺寸可能會因為裁剪而不一致。為避免此類狀況會把 `Size Mode` 設成 `Raw` & `Trim` = `false` ，他就會用原始圖片尺寸、且不裁切透明區域。

資源優化的目的：

- 進行裁剪的主要目的是為了節省記憶體和其他資源。
- 一般來說，Texture（紋理）多半使用的是 RGBA8888 格式，這意味著每個像素（px）會佔用 32bit = 4Byte，以 500x500 為例 他的記憶體就會佔掉 500x500x4Byte = 1MB

### Sprite Type

Sprite 元件支援以下幾種**Type**：

| 屬性     | 功能說明                                                                                                                                                                                                                                                                                              |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Simple` | 根據原始圖片資源渲染 Sprite，一般在這個模式下我們不會手動修改節點的尺寸，來確保場景中顯示的圖像和美術人員生產的圖片比例一致                                                                                                                                                                           |
| `Sliced` | 圖片將被分割成九宮格，並按照一定規則進行縮放以適應可隨意設定的尺寸(size)。 通常用於 UI 元素，或將可以無限放大而不影響影像品質的圖片製作成九宮格圖來節省遊戲資源空間。 詳細資訊請閱讀 使用 [九宮格圖像](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/sliced-sprite.html#-) |
| `Tiled`  | 當 Sprite 的尺寸增加時，圖片不會被拉伸，而是會按照原始圖片的大小不斷重複，就像平鋪瓦片一樣將原始圖片鋪滿整個 Sprite 規定的大小 。                                                                                                                                                                     |
| `Filled` | 根據原點和填滿模式的設置，按照一定的方向和比例繪製原始圖片的一部分。 常用於進度條的動態展示                                                                                                                                                                                                           |

### Type Filled

Type 屬性選擇填滿模式後，會出現一組新的屬性可供設定：

| 屬性        | 功能說明                                                                                                                |
| ----------- | ----------------------------------------------------------------------------------------------------------------------- |
| `FillType`  | 填充類型選擇，有 `HORIZONTAL`（橫向填充）、`VERTICAL`（縱向填充）和 `RADIAL`（扇形填充）三種                            |
| `FillStart` | 填滿起始位置的標準化數值（從 0 ~ 1，表示填充總量的百分比），選擇橫向填滿時，Fill Start 設為 0，就會從影像最左邊開始填滿 |
| `FillRange` | 填滿範圍的標準化數值（同樣從 0 ~ 1），設為 1，就會填滿最多整個原始影像的範圍                                            |
| `Filled`    | 填滿中心點，此屬性只有選擇了 `RADIAL` 填滿類型才能修改。 決定了扇形填滿時會環繞 Sprite 上的哪個點                       |

![Type Filled][Filled Example]

### Fill Range 填充範圍補充說明

在 `HORIZONTAL` 和 `VERTICAL` 這兩種填充類型下，`Fill Start` 設定的數值將影響填充總量，如果 `FillStart` 設為 0.5，那麼即使 `FillRange` 設為 1.0，實際填充的範圍仍然只有 Sprite 總大小的一半。

而 `RADIAL` 類型中 `FillStart` 只決定開始填充的方向，`FillStart` 為 0 時，從 x 軸正方向開始填充。 `FillRange` 決定填滿總量，值為 1 時將填滿整個圓形。`FillRange` 為正值時逆時針填充，為負值時順時針填充。

## 常見的圖片格式

| 圖片格式 | 說明                                                                                                                                                                               |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PNG      | 靜態影像格式，是以非破壞性壓縮進行壓縮，導致 PNG 的檔案大小相較於 JPG 大了一些                                                                                                     |
| JPG      | 靜態影像壓縮格式，一種針對相片影像而廣泛使用的一種失真壓縮標準方法。由於是採用失真壓縮演算法，因此壓縮率比 PNG 還要大，但因其沒有透明度資訊，所以實際檔案大小通常會較 PNG 來的小。 |
| WebP     | Google 推出的一種圖檔規格，跟相當普級的 JPEG 同樣採用失真壓縮(Lossy Compression)的技術，讓圖片檔案能變小，在相同品質的情況下，可以比 JPEG 再小 25%~34%。                           |

!!! note

    - 一般含透明的圖片以WebP優先
    - 非透明圖片則使用JPG

## SpriteAtlas

圖集，亦稱為 **Sprite Sheet**，是遊戲開發中常用的美術資源。

它是通過專門的工具將多張圖片合併成一張大圖，並利用 `plist`、`atlas` 等文件格式來索引這些圖片。

在 Cocos Creator 中一般使用的圖集資源為 `plist` 格式，而 Spine 動畫則使用 `atlas` 格式。

| 優點     | 說明                                                                                                                                          |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 節省空間 | 合成圖集時會刪除每張圖片周圍的空白區域，並且可以實施各種優化算法，從而大幅度減少遊戲的包體大小和內存佔用。                                    |
| 渲染優化 | 當多個 Sprite 渲染的是來自同一張圖集的圖片時，這些 Sprite 可以通過同一個渲染批次來處理，這樣可以顯著降低 CPU 的運算時間，提升遊戲運行的效率。 |

!!! note
Cocos 本身有提供 AutoAtlas 自動化圖集生成，但因其在 Build 時才會生成圖集，一般開發階段時仍是載入碎圖，因此在開發大型專案時十分容易影響載入時間，一般建議使用 [Texture Packer](https://www.codeandweb.com/texturepacker) 等工具進行合圖。
　
但 AutoAtlas 在特定情況下仍有奇效：將它配置給 bmfont 資源時，可以不影響 `.fnt` 檔識別各個文字的坐標資訊。

## Compress Texture

在預設情況下，Creator 在構建的時候輸出的是原始圖片。如果在構建時需要對某一張圖片或者自動圖集進行壓縮，可以在 資源管理器 中選中這張圖片或圖集，然後在 屬性檢查器 中勾選 useCompressTexture，再在 presetId 中選擇圖片的紋理壓縮預設。設定完成後點擊右上角的綠色打勾按鈕，即可應用。

![Compress Texture](https://i.imgur.com/TrCgWaO.png)

## 參考資訊

[Cocos Creator 官方－Sprite](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/sprite.html?h=sprite)

[Cocos Creator 官方－Sprite API](https://docs.cocos.com/creator/3.6/api/zh/class/Sprite)

[Cocos Creator 官方－图集资源（Atlas）](https://docs.cocos.com/creator/3.6/manual/zh/asset/atlas.html)

[Cocos Creator 官方－压缩纹理](https://docs.cocos.com/creator/3.6/manual/zh/asset/compress-texture.html)

[圖片格式比較](https://medium.com/coding-girl-life/%E9%97%9C%E6%96%BC%E9%80%99%E4%BA%9B%E5%9C%96%E7%89%87%E6%A0%BC%E5%BC%8Fpng-jpeg-jpeg-xr-jpeg2000-svg-webp-%E4%BD%A0%E4%BA%86%E8%A7%A3%E5%A4%9A%E5%B0%91%E5%91%A2-88c63021f868)

[Filled Example]: https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/sprite/radial.png "圖片來源 : Sprite 组件参考"
[useCompressTexture Example]: https://docs.cocos.com/creator/3.6/manual/zh/asset/compress-texture/compress-texture.png "圖片來源 : 压缩纹理"
