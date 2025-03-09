---
published: true
---
You:
Geekbench 5 (Single core)
Core i3-12100 1716
Core i3-10100 1104
Core i7-3770  802 
Core i5-4440  771

Geekbench 6 (Single core)
Core i7-3770  670 
Core i5-4440  1030 #why?

為什麼i7-3770在Geekbench 6變的比i5-4440少

ChatGPT:
這個現象可能是因為 Geekbench 6 相較於 Geekbench 5，在測試方法上有所改變，導致某些 CPU 表現出乎意料的變化。以下是可能的原因：

### 1. Geekbench 6 的測試負載改變

- Geekbench 6 更加側重於現代工作負載，包括更大的資料集、更複雜的記憶體操作、以及更高的 AVX 負載。
- i5-4440 支援 AVX2，而 i7-3770 只有 AVX，這可能影響了一些測試的結果，使 i5-4440 受益更多。
- Geekbench 6 可能強化了 記憶體頻寬與延遲的影響，而 i7-3770 的 DDR3 記憶體頻寬較低，影響了成績。

### 2. IPC（每時脈指令數）與架構差異

- i7-3770 是 Ivy Bridge (22nm, 2012)
- i5-4440 是 Haswell (22nm, 2013)
- Haswell 架構的 IPC 比 Ivy Bridge 更高，特別是在 Geekbench 6 這類新測試環境下，可能能夠彌補時脈上的劣勢。

### 3. 時脈差異影響降低

- i7-3770：3.4 GHz (單核最高 3.9 GHz)
- i5-4440：3.1 GHz (單核最高 3.3 GHz)
- 在 Geekbench 5，時脈對分數影響較大，因此 i7-3770 佔優勢。
- 在 Geekbench 6，指令集優化和 IPC 影響更大，導致 i5-4440 反而表現更好。

### 4. Windows 記憶體管理與 SMT（超執行緒）影響

- i7-3770 有 4 核 8 線程（SMT），但 i5-4440 只有 4 核 4 線程。
- 在新版本的 Geekbench 6，某些工作負載可能因為記憶體調度和 SMT 資源競爭，使得 i7-3770 單核表現下降。

---

## 總結

1. Geekbench 6 測試方法改變，可能讓 AVX2 的 i5-4440 受益較多。
2. Haswell（i5-4440）在 IPC 上優於 Ivy Bridge（i7-3770）。
3. 時脈對單核影響降低，Geekbench 6 更偏向架構與指令集的優化。
4. 超執行緒（SMT）可能在某些測試場景中產生額外開銷，影響 i7-3770 分數。

所以，i5-4440 在 Geekbench 6 單核測試中得分比 i7-3770 高，是合理的現象，代表新測試更強調架構與指令集，而不是純粹的時脈。
