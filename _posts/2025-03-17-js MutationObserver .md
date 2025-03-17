---
published: true
---
ex:
```js
// 指定想要監聽的 DOM 節點
const targetNode = document.getElementById('myElement');

// 定義觀察選項
const config = {
  attributes: true, // 監聽屬性變更
  childList: true,  // 監聽子節點變動
  characterData: true, // 監聽文本內容改變
  subtree: true    // 監聽所有子節點的變動
};

const callback = (mutationsList) => {
  mutationsList.forEach((mutation) => {
    console.log('Mutation type:', mutation.type);

    if (mutation.type === 'attributes') {
      console.log('Attribute changed:', mutation.attributeName);
    } else if (mutation.type === 'childList') {
      console.log('Child nodes changed:', mutation);
    } else if (mutation.type === 'characterData') {
      console.log('Text content changed:', mutation.target.data);
    }
  });
};

// 建立 MutationObserver 實例，並傳入 callback 函式
const observer = new MutationObserver(callback);

// 開始觀察
observer.observe(targetNode, config);

// 停止觀察
observer.disconnect();
```
  https://ithelp.ithome.com.tw/articles/10351886
  
## option

## MutationObserver 實體執行 `observe(element, options)`

`options` 有以下的屬性可以使用：

**1\. 偵測更動的類型（必須擇一開啟）：**

-   `attributes` （Boolean） 偵測屬性的更動
-   `childList` （Boolean）偵測子節點的更動
-   `charactorData` （Boolean）偵測字元內容更動

**2\. 偵測更動的範圍：**

-   `subtree`（Boolean）對其節點下所有延伸的節點進行觀察
-   `attributeFilter` （String\[\]）過濾偵測更動的屬性，需要 `attributes` 設為 `ture`

**3\. 捕抓更動前的狀態：**

-   `attributeOldValue` （Boolean）屬性更動前的值
-   `characterDataOldValue` （Boolean）字元內容更動前的值

  https://medium.com/@alexian853/mutationobserver-dom-%E5%AE%87%E5%AE%99%E7%9A%84%E8%A7%80%E5%AF%9F%E8%80%85-3c2e8d503b1c
  
如果 **只有** `subtree: true`，但沒有 `childList: true`，則**不會監聽子節點的新增或刪除**，因為 `subtree` 只是擴展範圍，不是監聽行為本身。
  
## monitor new node

```js
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MutationObserver 測試</title>
</head>
<body>

    <div id="container">
        <p>原始內容</p>
    </div>
    <button id="add">新增節點A</button>
	<button id="add_b">新增節點B</button>

    <script>
        // 監聽的目標節點
        const targetNode = document.getElementById("container");

        // 設置監聽的變化類型
        const config = { childList: true,
						 subtree: true};

        // 當變化發生時執行的回調函式
        const callback = function(mutationsList) {
            for (const mutation of mutationsList) {
                if (mutation.type === "childList") {
                    console.log("偵測到新增節點:", mutation.addedNodes);
                }
            }
        };

        // 建立 MutationObserver 實例
        const observer = new MutationObserver(callback);

        // 開始監聽
        observer.observe(targetNode, config);

        // 點擊按鈕新增節點
        document.getElementById("add").addEventListener("click", function() {
            const newNode = document.createElement("p");
			newNode.id = "nodeA";
            newNode.textContent = "新加入的節點";
            targetNode.appendChild(newNode);
        });
		document.getElementById("add_b").addEventListener("click", function() {
            const newNode = document.createElement("p");
			newNode.id = "nodeB";
            newNode.textContent = "新加入的節點B";
            document.getElementById("nodeA").appendChild(newNode);
        });
    </script>

</body>
</html>
```
