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