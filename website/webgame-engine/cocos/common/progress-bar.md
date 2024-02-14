# ProgressBar

用於表現進度條的效果。

## ProgressBar 屬性

![ProgressBar示例](/webgame-engine/assets/cocos/common/ProgressBar/ProgressBarUIExample.png)

| 屬性 | 功能說明 |
| ------------------- | ------------------------------------- |
|**Bar Sprite**|進度條渲染所需要的 `Sprite` Component|
|**Mode**|進度條展現方式，選項包括 水平`HORIZONTAL`、垂直`VERTICAL`、填充`FILLED`|
|**Total Length**|當進度條為 100% 時，**Bar Sprite**的總長度/總寬(高)度。在`FILLED`模式下**Total Length**表示取**Bar Sprite**總顯示範圍的百分比，取值範圍從 `0-1`|
|**Progress**|Float，表示當前進度，`0-1`|
|**Reverse** |Boolean，原進度條方向為左到右/上到下，True後變為右到左/下到上|

!!! Note

    **ProgressBar** 的 **Mode** 選擇 `FILLED` 的情况下，**Bar Sprite** 的 **Type** 也需要設定為 `FILLED`，否則會出現警告。

## 水平與垂直模式 `HORIZONTAL` & `VERTICAL`

當我們選擇 `HORIZONTAL` 或 `VERTICAL` 時，ProgressBar 會透過修改 **Bar Sprite** 的尺寸來感變進度條的顯示長度

在這在個模式底下 **Total Length** 的單位是 Pixel，用來指定當進度條到100%時 **Bar Sprite** 的長度

## 填充模式 FILLED

與 `HORIZONTAL`、`VERTICAL` 不同的是，`FILLED` 會通過一定的比例裁剪 **Bar Sprite**，因此我們需要對**Bar Sprite**中引用的**Sprite** Component做設定

首先將***Sprite***中的**Type**設定為`FILLED`，然後選擇一個方向`HORIZONTAL`、`VERTICAL`、`RADIAL`

> 下圖的範例顯示了當**Bar Sprite**的**Type**設定為`RADIAL`時不同的**Total Length**對其影響

![FILLED範例][Filled Example]

## 參考資訊

[Cocos Creator 官方－ProgressBar](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/progress.html)

[Cocos Creator 官方－常用 UI 控件(2.4)](https://docs.cocos.com/creator/2.4/manual/zh/ui/ui-components.html)

[Filled Example]: https://docs.cocos.com/creator/2.4/manual/zh/ui/ui-components/filled_radial.png "圖片來源 : 常用 UI 控件(2.4)"