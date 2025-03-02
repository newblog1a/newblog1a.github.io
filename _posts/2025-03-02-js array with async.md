---
published: true
---
# JavaScript 非同步陣列處理

#### You:
javascript中，非同步風格要怎麼處理array

#### ChatGPT:
在 JavaScript 中，處理陣列時遇到非同步操作，通常會用以下三種方法來處理：

### 1. **`forEach`（不適合非同步）**

`forEach` **不會等** `async` 函式執行完畢，所以不適合用來執行 `await` 操作。

```javascript
const array = [1, 2, 3];

array.forEach(async (num) => {
  await new Promise(resolve => setTimeout(resolve, 1000)); // 模擬非同步操作
  console.log(num);
});
console.log('完成'); // 這行會先執行，因為 `forEach` 不會等 `await`
```

---

### 2. **`for...of`（適合 `await`）**

`for...of` **可以搭配 `await`，讓每次迭代等待非同步操作完成**。

```javascript
const array = [1, 2, 3];

async function processArray() {
  for (const num of array) {
    await new Promise(resolve => setTimeout(resolve, 1000)); // 模擬非同步操作
    console.log(num);
  }
  console.log('完成');
}

processArray();
```

📌 **優點**：可控順序，每次迭代都會等上一次 `await` 完成。\
📌 **缺點**：**不能並行**，需要等待前一個 `await` 執行完才能進入下一個。

---

### 3. **`map + Promise.all`（並行執行）**

如果希望所有非同步操作**同時執行**，而不是一個接一個，可以用 `map` 搭配 `Promise.all`。

```javascript
const array = [1, 2, 3];

async function processArray() {
  await Promise.all(
    array.map(async (num) => {
      await new Promise(resolve => setTimeout(resolve, 1000)); // 模擬非同步操作
      console.log(num);
    })
  );
  console.log('完成');
}

processArray();
```

📌 **優點**：**並行執行，效能較高**。\
📌 **缺點**：可能導致過多請求同時執行，對某些應用（如 API 請求）可能造成負擔。

---

### 總結

| 方法                  | 是否等待 `await` | 是否並行執行 |
| ------------------- | ------------ | ------ |
| `forEach`           | ❌ 否          | ❌ 否    |
| `for...of`          | ✅ 是          | ❌ 否    |
| `map + Promise.all` | ✅ 是          | ✅ 是    |

👉 **如果需要依序執行，使用 `for...of`**。\
👉 **如果需要並行執行，使用 `map + Promise.all`**。

你需要哪種方式呢？ 😊

---

上手使用 JavaScript 的 Map、Reduce 吧！
  https://fred-zone.blogspot.com/2017/01/javascript-mapreduce.html


所有的结果都清楚地表明 for 循环比 map / reduce / filter / find 更加快。

JavaScript性能测试-比较for/forEach/map/reduce/filter
  https://blog.csdn.net/m0_49039063/article/details/132431081
for 和 filter 的效率哪个更高？
  https://blog.csdn.net/HYeeee/article/details/110531288