---
published: true
---
‵```html
<bili-checkbox value="sync" label="同时转发到我的动态" checked="false"></bili-checkbox>
  <input type="checkbox" value="sync">
  
<div class="brt-editor" contenteditable="true" style="height: 32px;"></div>


<div id="editor" class="">
<div id="editor" class=" active ">
```

用事件，如果我focus在下面的節點，實行ccc()
```html
<div class="brt-editor" contenteditable="true" style="height: 32px;"></div>
```

---

## v1

```js
// ==UserScript==
// @name         Dynamic Node Watcher
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Monitor dynamically loaded nodes and execute ccc()
// @author       You
// @match        *://*/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function findCheckboxInShadow5(root = document) {
        if (!root) return null;

        // 先嘗試在當前 root 找到 bili-checkbox
        const biliCheckbox = root.querySelector('bili-checkbox[label="同时转发到我的动态"]');
        if (biliCheckbox) {
            return biliCheckbox;
        }

        // 遞迴搜尋所有 Web Component（擁有 shadowRoot 的元素）
        for (const el of root.querySelectorAll('*')) {
            if (el.shadowRoot) {
                const found = findCheckboxInShadow5(el.shadowRoot);
                if (found) return found;
            }
        }

        return null;
    }

    // 確保 DOM 載入完成後開始監聽
    function startObserving() {

        const checkComments = setInterval(() => {
                const commentSection = document.querySelector('.comment-section'); // 修改為正確的選擇器???

                // 查找 checkbox 並勾選
                const checkbox5 = findCheckboxInShadow5();
                if (checkbox5) {
                    checkbox5.setAttribute("checked", "true");
                }

                clearInterval(checkComments); // 停止定時器

            },
            500); // 每 500 毫秒檢查一次

    }

    document.addEventListener('DOMContentLoaded', startObserving);

})();
```

===

## v2

用MutationObserver監控網頁。
若發現節點加入shadow root，1.將其也加入監控。2.執行c。
c: 對shadow root內部掃描，檢查是否節點有留言版。

```js
function ccc(node) {
    console.log("Processing node:", node);
    // 在這裡執行你的 ccc() 處理邏輯
}

function observeShadowRoot(shadowRoot) {
	ccc(shadowRoot); //shadowRoot.host or shadowRoot ???
    const observer = new MutationObserver(mutations => {
        mutations.forEach(mutation => {
            mutation.addedNodes.forEach(node => {
                if (node.shadowRoot) {
                    observeShadowRoot(node.shadowRoot);
                }
            });
        });
    });
    observer.observe(shadowRoot, { childList: true, subtree: true });
}

const observer = new MutationObserver(mutations => {
    mutations.forEach(mutation => {
        mutation.addedNodes.forEach(node => {
            ccc(node);
            if (node.shadowRoot) {
                observeShadowRoot(node.shadowRoot);
            }
        });
    });
});

observer.observe(document.body, { childList: true, subtree: true });
```

===

## V3

You:
用IntersectionObserver。
當Viewpoint接觸 <bili-checked> 時，執行ccc()。
<bili-checked> 在 shadow root裡面。

ChatGPT:
`IntersectionObserver` 無法直接觀察 Shadow DOM 內的元素，但你可以觀察包含 `<bili-checked>` 的 shadow host（即擁有 shadow root 的元素），當它進入視口時，再透過 `shadowRoot.querySelector()` 找到 `<bili-checked>`，並執行 `ccc()`。

## 解法：

1. 找到 shadow host：假設 `<bili-checked>` 存在於某個 `custom-element` 內部（例如 `<bili-wrapper>`）。
2. 監聽 shadow host：使用 `IntersectionObserver` 觀察 `<bili-wrapper>` 是否進入視口。
3. 從 shadow root 獲取 `<bili-checked>`：當 `<bili-wrapper>` 進入視口後，透過 `shadowRoot.querySelector("bili-checked")` 取得 `<bili-checked>`，並執行 `ccc()`。

---

### 範例程式：

```javascript
// 假設 <bili-checked> 在 <bili-wrapper> 內的 shadow DOM 裡
const wrapper = document.querySelector("bili-wrapper");

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            // 進入視口時，從 shadow root 取出 <bili-checked>
            const shadowRoot = wrapper.shadowRoot;
            if (shadowRoot) {
                const biliChecked = shadowRoot.querySelector("bili-checked");
                if (biliChecked) {
                    ccc();
                }
            }
        }
    });
}, { threshold: 0.5 });  // threshold 設 0.5 表示 50% 可見時觸發

observer.observe(wrapper);
```

---

### 注意：

- `bili-wrapper` 必須是 `custom element`，且它的 `shadowRoot` 需設為 `open`（否則 `shadowRoot` 會是 `null`）。
- `threshold: 0.5` 表示當 `bili-wrapper` 50% 進入視口時觸發，你可根據需求調整數值。
- 若 `bili-checked` 在 `shadowRoot` 延遲加載，可能需要 `MutationObserver` 監聽變化。
