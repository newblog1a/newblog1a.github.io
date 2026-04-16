---
title: Code Diagrams
---
pydeps vs Pyan vs Pyreverse

The primary difference between pydeps, Pyan, and Pyreverse lies in the "level" of the code they visualize: pydeps focuses on high-level module imports, Pyreverse focuses on object-oriented class structures (UML), and Pyan focuses on granular function-to-function call graphs.

Pyan # function; 圖很亂，大量重疊

pydeps # import

## pydeps 

```
uv run pydeps app.py -x textual -x datetime
```

👉 這是你版本的「正確 exclude 用法」

## Pyreverse 

Pyreverse 在 pylint 包中，需要下載 pylint 以及繪圖軟體 graphviz。

```
pip install pylint
scoop install graphviz
```

接下來就可以開始分析，語法是 `pyreverse -o <格式> -p <名稱> <要分析的檔案>`。分析的檔案也可以直接是一整個 package，我們以 gallery-dl 為例，用法會變成

```
git clone --depth=1 https://github.com/mikf/gallery-dl.git
pyreverse -o png -p test ./gallery-dl/gallery_dl

```

如果遇到解析度問題可以設定輸出成 pdf，首先要轉成 dot 檔案再經由 graphviz 轉換成 pdf，並且加上顏色，然後只顯示類別名稱，指令如下

```
pyreverse --colorized --only-classnames -o dot -p gallery-dl ./gallery-dl/gallery_dl
dot -Tpdf classes_gallery-dl.dot -o classes_gallery-dl.pdf
```

[https://zsl0621.cc/memo/python/pyreverse-usage](https://zsl0621.cc/memo/python/pyreverse-usage)

---



Diagrams：代碼即架構圖 (Diagrams as Code)

Mermaid & UML

Pyan & Pylint：自動分析程式碼架構 

若你的專案很大，想**自動分析**現有代碼的呼叫關係： 

- **Pyan**：可以分析 Python 代碼並生成模組間的呼叫關係圖 (Call Graph)。
- **Pyreverse** (隨 Pylint 安裝)：自動根據你的專案代碼產生 UML 類別圖。

