---
published: true
---

How do I use floating-point arithmetic in bash?
  https://stackoverflow.com/questions/12722095/how-do-i-use-floating-point-arithmetic-in-bash
  https://unix.stackexchange.com/questions/412716/can-bash-do-floating-point-arithmetic-without-using-an-external-command
  
You can't. bash *only* does integers; you *must* delegate to a tool such as `bc`.

---

You:
bash
"""
start=$(date +%s) 
end=$(date +%s)
elapsed=$((end-start))
echo "Elapsed Time: $elapsed seconds"
"""

我想改成milliseconds

ChatGPT:
你可以使用 `date +%s%3N` 來獲取當前時間的毫秒數，完整的 Bash 腳本如下：

```bash
start=$(date +%s%3N)  # 取得當前時間（毫秒）
sleep 1  # 模擬程式運行（1秒）
end=$(date +%s%3N)  # 再次取得當前時間（毫秒）

elapsed=$((end - start))  # 計算時間差
echo "Elapsed Time: $elapsed milliseconds"
```

這樣 `elapsed` 變數就會顯示以毫秒計算的時間間隔。


You:
我想顯示seconds，但要顯示到小數點後三碼

ChatGPT:
你可以使用 `bc` 或 `awk` 來進行浮點數運算，這樣可以顯示秒數並保留到小數點後三位，例如：

```bash
start=$(date +%s%3N)  # 取得當前時間（毫秒）
sleep 1.5  # 模擬程式運行（1.5秒）
end=$(date +%s%3N)  # 再次取得當前時間（毫秒）

elapsed_ms=$((end - start))  # 計算時間差（毫秒）
elapsed_sec=$(awk "BEGIN {printf \"%.3f\", $elapsed_ms / 1000}")  # 轉換為秒並格式化輸出

echo "Elapsed Time: $elapsed_sec seconds"
```

這樣輸出會類似：

```
Elapsed Time: 1.500 seconds
```


You:
elapsed_sec=$((elapsed_ms / 1000))
elapsed_sec為什麼是整數，沒有小數點?

ChatGPT:
在 Bash 中，`$(( ... ))` 是整數運算，不支援小數點運算，所以：

```bash
elapsed_sec=$((elapsed_ms / 1000))
```

這行程式碼會進行整數除法，小數部分會被捨去。例如，如果 `elapsed_ms=1500`，計算結果會變成 `1`，而不是 `1.500`。

---

### 解決方法

如果要保留小數點後三位，可以用 `bc` 或 `awk` 來進行浮點數運算。

#### 方法 1：使用 `awk`

```bash
elapsed_sec=$(awk "BEGIN {printf \"%.3f\", $elapsed_ms / 1000}")
```

這行使用 `awk` 來計算 `elapsed_ms / 1000`，並格式化輸出到小數點後三位。

#### 方法 2：使用 `bc`

```bash
elapsed_sec=$(echo "scale=3; $elapsed_ms / 1000" | bc)
```

這行使用 `bc` 來計算 `elapsed_ms / 1000`，`scale=3` 代表保留三位小數。

這兩種方法都可以讓 `elapsed_sec` 保留小數點後三位！
