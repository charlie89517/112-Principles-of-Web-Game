# Getting Started

[Material for Mkdocs ](https://squidfunk.github.io/mkdocs-material/getting-started/) 是一個基於 MkDocs（用於專案文件的靜態網站產生器）之上的文件框架。如果您熟悉 Python，則可以使用 Python 套件管理器 pip 安裝 Material for MkDocs。

## Install

```bash
pip install mkdocs mkdocs-material mkdocs-glightbox
```

這將自動安裝所有相依性的相容版本：MkDocs、Markdown、Pygments 和 Python Markdown 擴充功能。 Material for MkDocs 始終致力於支援最新版本，因此無需單獨安裝這些軟體包。

並使用
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

1. 請在 `webgame-engine` 下建立 `<group>/<topic>.md` 的檔案，比方說若要撰寫
2. 
