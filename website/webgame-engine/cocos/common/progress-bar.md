### ProgressBar 組件介紹

## 介紹

ProgressBar 經常被用來表示完成進度，在Node中添加ProgressBar Component

![ProgressBar示例](https://docs.cocos.com/creator/3.6/manual/zh/ui-system/components/editor/progress/add-progressbar.png)

## 屬性

| 屬性 | 功能說明 |
| ------------------- | ------------------------------------- |
|`Bar Sprite`|進度條渲染所需要的`Sprite`component|
|`Mode`|選項包括`HORIZONTAL`、`VERTICAL`、`FILLED`|
|`Total Length`|當進度條為100%時`Bar Sprite`的總長度/總寬度。在`FILLED`模式下`Total Length`表示取`Bar Sprite`總顯示範圍的百分比，取值範圍從`0-1`|
|`Progress`|Float，表示當前進度，`0-1`|
|`Reverse` |Boolean，原進度條方向為左到右/上到下，True後變為右到左/下到上|

> 注意 : `ProgressBar`的`Mode`選擇`FILLED`的情况下，`Bar Sprite`的`Type`也需要設定為`FILLED`，否則會出現警告。