## Sprite 介紹

Sprite是 2D/3D 遊戲最常見的顯示圖像的方式，在節點上添加 Sprite 組件，就可以在場景中顯示項目資源中的圖片。

![Sprite實例](/webgame-engine/assets/cocos/common/SpriteUIExample.PNG)

### Sprite 屬性

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
|**Type**| 渲染模式，選項包括`Simple`、`Sliced`、`Tiled`、`Filled`四種模式。 |
|**CustomMaterial**| 自定義Material，使用方式參考官網[自定義Material](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/ui-material.html) |
|**Grayscale**| 開啟後，使用灰階渲染 |
|**Color**| 圖片的顏色 |
|**SpriteAtlas**| 顯示的圖片所屬的圖集(參考[Atlas](https://docs.cocos.com/creator/3.6/manual/zh/asset/atlas.html)) |
|**SpriteFrame**| 渲染 Sprite 使用的 SpriteFrame 圖片資源 |
|**SizeMode**| 指定 Sprite 的尺寸： <br> `Trimmed` 表示會使用原始圖片資源裁剪透明像素後的尺寸 <br> `Raw` 表示會使用原始圖片未經裁剪的尺寸 <br> `Custom` 表示會使用自定義尺寸。當用戶手動修改過 Size 屬性後，Size Mode 會被自動設置為 `Custom`，除非再次指定為前兩種尺寸 |
|**Trim**| 是否渲染原始圖像周圍的透明像素區域，請參考[圖像自動剪裁](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/trim.html) |

#### Sprite Type

Sprite 元件支援以下幾種**Type**：

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `Simple` | 根據原始圖片資源渲染 Sprite，一般在這個模式下我們不會手動修改節點的尺寸，來確保場景中顯示的圖像和美術人員生產的圖片比例一致 |
| `Sliced` | 圖片將被分割成九宮格，並按照一定規則進行縮放以適應可隨意設定的尺寸(size)。 通常用於 UI 元素，或將可以無限放大而不影響影像品質的圖片製作成九宮格圖來節省遊戲資源空間。 詳細資訊請閱讀 使用 [九宮格圖像](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/engine/sliced-sprite.html#-) |
| `Tiled` | 當 Sprite 的尺寸增加時，圖片不會被拉伸，而是會按照原始圖片的大小不斷重複，就像平鋪瓦片一樣將原始圖片鋪滿整個Sprite 規定的大小 。|
| `Filled` | 根據原點和填滿模式的設置，按照一定的方向和比例繪製原始圖片的一部分。 常用於進度條的動態展示 |

#### Type Filled

Type 屬性選擇填滿模式後，會出現一組新的屬性可供設定：

| 屬性   | 功能說明 |
| ------------------- | ------------------------------ |
| `FillType` | 填充類型選擇，有 `HORIZONTAL`（橫向填充）、`VERTICAL`（縱向填充）和 `RADIAL`（扇形填充）三種 |
| `FillStart` | 填滿起始位置的標準化數值（從 0 ~ 1，表示填充總量的百分比），選擇橫向填滿時，Fill Start 設為 0，就會從影像最左邊開始填滿 |
| `FillRange` | 填滿範圍的標準化數值（同樣從 0 ~ 1），設為 1，就會填滿最多整個原始影像的範圍 |
| `Filled` | 填滿中心點，此屬性只有選擇了 `RADIAL` 填滿類型才能修改。 決定了扇形填滿時會環繞 Sprite 上的哪個點 |

![Type Filled](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/sprite/radial.png)

#### Fill Range 填充範圍補充說明

在 `HORIZONTAL` 和 `VERTICAL` 這兩種填充類型下，`Fill Start` 設定的數值將影響填充總量，如果 `FillStart` 設為 0.5，那麼即使 `FillRange` 設為 1.0，實際填充的範圍仍然只有 Sprite 總大小的一半。

而 `RADIAL` 類型中 `FillStart` 只決定開始填充的方向，`FillStart` 為 0 時，從 x 軸正方向開始填充。 `FillRange` 決定填滿總量，值為 1 時將填滿整個圓形。`FillRange` 為正值時逆時針填充，為負值時順時針填充。

### Sprite API
[Sprite API](https://docs.cocos.com/creator/3.6/api/zh/class/Sprite)