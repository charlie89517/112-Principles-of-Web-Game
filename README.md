# Getting Started

[Material for Mkdocs ](https://squidfunk.github.io/mkdocs-material/getting-started/) 是一個基於 MkDocs（用於專案文件的靜態網站產生器）之上的文件框架。如果您熟悉 Python，則可以使用 Python 套件管理器 pip 安裝 Material for MkDocs。

## Install

```bash
pip install mkdocs mkdocs-material mkdocs-glightbox mkdocs-redirects
```

這將自動安裝所有相依性的相容版本：MkDocs、Markdown、Pygments 和 Python Markdown 擴充功能。 Material for MkDocs 始終致力於支援最新版本，因此無需單獨安裝這些軟體包。

接下來，請在專案根目錄並使用
```bash
mkdocs serve
```

## Markdown Syntax

請參考 [markdown guide](https://www.markdownguide.org/basic-syntax/)

## Extension

請參考[此處](https://squidfunk.github.io/mkdocs-material/reference/)

目前已配置的插件如下：

1. Admonition - 在不顯著中斷文件流程的情況下包含輔助內容的絕佳選擇。
2. Code Blocks - 程式碼區塊和範例是技術專案文件的重要組成部分
3. Diagrams - 圖表有助於傳達不同技術組件之間的複雜關係和互連，並且是專案文件的重要補充。 Material for MkDocs 與 [Mermaid.js](https://mermaid.js.org/) 整合，這是一個非常受歡迎且靈活的圖表繪製解決方案。
4. Footnotes - 為特定單字、片語或句子添加補充或附加資訊的好方法。
5. Image - 提供圖像對齊和圖像標題的樣式。

## Writing standards

1. 請先查看 `mkdocs.yml` 的 `nav` 欄位，並建立對應的資料夾與markdown檔案(需注意大小寫)

範例：
```yaml
# ... 
nav:
  - index.md
  - "網頁遊戲引擎原理與實做":
      - webgame-engine/index.md
      - 課程總覽:
          - webgame-engine/overview/cocos-creator.md # Cocos Creator
# ... more content
```

因此在專案的 `webgame-engine/overview` 中，存在 `cocos-creator.md` 檔案
該檔案的內容就是你被分配到的工作項目

2. Git lfs - 請依照[Managin large files](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github)設定 Git LFS
可以通過設定 `git lfs install --skip-smudge`，在 pull / clone 專案時，不必把其他協作者的圖片素材也拉下來，僅在必要時使用 `git lfs pull` 即可

3. 圖片素材
統一放置在 `webgame-engine/assets` 底下，目前有一個範例圖片 `lfs.png`
可以在該 `assets` 目錄底下額外新增資料夾，用來管理你的圖片素材。使用  Markdown 時，只需要使用絕對路徑引用即可

```md
![alter name](/webgame-engine/assets/path/to/my-image.png)
```

可以參考 `webgame-engine/overview/cocos-creator.md`

4. 補上參考文件
請參考 `Extension` 的 `Footnotes`，盡量明確指出使用的圖片素材來自哪個地方
