---
title: Code Diagrams
---
pydeps vs Pyan vs Pyreverse

The primary difference between pydeps, Pyan, and Pyreverse lies in the "level" of the code they visualize: pydeps focuses on high-level module imports, Pyreverse focuses on object-oriented class structures (UML), and Pyan focuses on granular function-to-function call graphs.

---



Diagrams：代碼即架構圖 (Diagrams as Code)

Mermaid & UML

Pyan & Pylint：自動分析程式碼架構 

若你的專案很大，想**自動分析**現有代碼的呼叫關係： 

-   **Pyan**：可以分析 Python 代碼並生成模組間的呼叫關係圖 (Call Graph)。

-   **Pyreverse** (隨 Pylint 安裝)：自動根據你的專案代碼產生 UML 類別圖。

