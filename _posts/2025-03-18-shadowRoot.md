---
published: true
---
You:
```html
<bili-checkbox value="sync" label="同时转发到我的动态" checked="false"></bili-checkbox>
```

裡面包含
```html
<input type="checkbox" value="sync">
```

請用javascript選到node(同时转发到我的动态)，然後 勾選裡面node的checkbox

ChatGPT:
可以使用 `querySelector` 找到 `<bili-checkbox>`，然後在它內部找到 `<input type="checkbox">`，並將 `checked` 設為 `true`，如下所示：

```javascript
const biliCheckbox = document.querySelector('bili-checkbox[label="同时转发到我的动态"]');
if (biliCheckbox) {
    const checkbox = biliCheckbox.querySelector('input[type="checkbox"]');
    if (checkbox) {
        checkbox.checked = true;
        checkbox.dispatchEvent(new Event('change', { bubbles: true })); // 觸發事件
    }
}
```

這段程式碼會：

1. 選擇 `<bili-checkbox>`，根據 `label` 屬性來匹配。
2. 在該元素內部找到 `input[type="checkbox"]`。
3. 將其 `checked` 設為 `true`。
4. 觸發 `change` 事件，以確保網站的事件監聽器可以正確響應變更。

如果 `bili-checkbox` 是 Web Component，可能需要使用 `shadowRoot`，請提供更多細節，以便確認最佳選取方式！


You:
bili-checkbox node裡面是shadowRoot

ChatGPT:
由於 `<bili-checkbox>` 是 Web Component，內部包含 `shadowRoot`，你需要透過 `shadowRoot` 來選取內部的 `<input type="checkbox">`。

試試這段 JavaScript：

```javascript
const biliCheckbox = document.querySelector('bili-checkbox[label="同时转发到我的动态"]');
if (biliCheckbox && biliCheckbox.shadowRoot) {
    const checkbox = biliCheckbox.shadowRoot.querySelector('input[type="checkbox"]');
    if (checkbox) {
        checkbox.checked = true;
        checkbox.dispatchEvent(new Event('change', { bubbles: true }));
    }
}
```

### 解釋：

1. 選取 `<bili-checkbox>` 元素，根據 `label` 屬性來匹配。
2. 存取 `shadowRoot`，因為 `<input>` 位於 Shadow DOM 內。
3. 從 `shadowRoot` 查找 `input[type="checkbox"]`。
4. 勾選 Checkbox 並觸發 `change` 事件，確保網站的事件監聽器能正常回應變更。

這樣應該可以成功選取並勾選 `bili-checkbox` 內的 checkbox！🚀


You:
bili-checkbox本身是web component, 該web component也是在另一個web component 內

ChatGPT:
因為 `<bili-checkbox>` 本身位於另一個 Web Component 內，所以你需要先找到外層 Web Component，然後再進入內部的 `shadowRoot`，逐層查找 `bili-checkbox` 和 `input[type="checkbox"]`。

### 解法：

```javascript
// 先找到外層的 Web Component（假設它是 <outer-component>）
const outerComponent = document.querySelector('outer-component');

if (outerComponent && outerComponent.shadowRoot) {
    // 在外層 Web Component 的 shadowRoot 中尋找 <bili-checkbox>
    const biliCheckbox = outerComponent.shadowRoot.querySelector('bili-checkbox[label="同时转发到我的动态"]');
    
    if (biliCheckbox && biliCheckbox.shadowRoot) {
        // 在 bili-checkbox 的 shadowRoot 內找到 checkbox
        const checkbox = biliCheckbox.shadowRoot.querySelector('input[type="checkbox"]');
        
        if (checkbox) {
            checkbox.checked = true;
            checkbox.dispatchEvent(new Event('change', { bubbles: true }));
        }
    }
}
```

### 解釋：

1. 找到外層 Web Component（你需要替換 `'outer-component'` 為實際的標籤名稱）。
2. 進入外層 Web Component 的 `shadowRoot`，查找 `<bili-checkbox>`。
3. 進入 `<bili-checkbox>` 的 `shadowRoot`，查找內部的 `<input type="checkbox">`。
4. 勾選 checkbox 並觸發 `change` 事件，確保變更被偵測到。

如果 Web Component 有多層嵌套，你可能需要多次 `.shadowRoot.querySelector(...)` 來進入更深層的 `shadowRoot`。如果不確定外層 Web Component 的標籤，可以先在 `console` 輸入：

```javascript
document.querySelector('bili-checkbox')?.getRootNode()
```

看看它的 `host` 是什麼，再逐步查找。


You:
有沒有辦法直接收尋全部的web component 裡面

ChatGPT:
你可以使用 `querySelectorAll('*')` 遍歷所有節點，並且遞迴進入每個 `shadowRoot`，搜尋 `bili-checkbox` 內的 `<input type="checkbox">`，這樣就能掃描所有 Web Components。

### 完整解法（遞迴搜尋所有 Shadow DOM 層級）

```javascript
function findCheckboxInShadow(root = document) {
    if (!root) return null;

    // 先嘗試在當前 root 找到 bili-checkbox
    const biliCheckbox = root.querySelector('bili-checkbox[label="同时转发到我的动态"]');
    if (biliCheckbox && biliCheckbox.shadowRoot) {
        const checkbox = biliCheckbox.shadowRoot.querySelector('input[type="checkbox"]');
        if (checkbox) return checkbox;
    }

    // 遞迴搜尋所有 Web Component（擁有 shadowRoot 的元素）
    for (const el of root.querySelectorAll('*')) {
        if (el.shadowRoot) {
            const found = findCheckboxInShadow(el.shadowRoot);
            if (found) return found;
        }
    }
    
    return null;
}

// 查找 checkbox 並勾選
const checkbox = findCheckboxInShadow();
if (checkbox) {
    checkbox.checked = true;
    checkbox.dispatchEvent(new Event('change', { bubbles: true }));
}
```

### 解釋

1. 先搜尋 `document` 內是否直接有 `bili-checkbox`，如果找到就直接從 `shadowRoot` 內選取 `input[type="checkbox"]`。
2. 遍歷所有元素，如果某個元素有 `shadowRoot`，就進入它的 `shadowRoot` 繼續查找。
3. 使用遞迴，確保可以進入所有層級的 Shadow DOM。
4. 一旦找到 checkbox，就勾選並觸發 `change` 事件，確保變更能夠被監聽。

這樣就能夠搜尋整個頁面（包含所有 Web Components）內的 `bili-checkbox`，並勾選它的 checkbox！
