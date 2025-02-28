---
published: true
---
# 網頁清理與MHTML保存

#### You:
想要解決的問題：想要保存網頁成.mhtml網頁。但是往往保存的檔案太大。

思路，想寫一個程式，要保存網頁之前執行，移除掉網頁不必要的原件，這樣應該可以有效降低.mhtml檔案的大小

#### ChatGPT:
這個思路可行！你可以用 Python 搭配 Selenium 或 Playwright 來載入網頁，然後透過 JavaScript 移除不必要的元素，比如廣告、評論區、側邊欄、隱藏內容等，最後再讓瀏覽器執行「保存為 .mhtml」。

大致步驟：

1. **載入網頁**：使用 Selenium 或 Playwright 開啟目標網頁。
2. **清理網頁**：執行 JavaScript 移除不必要的元素（例如 `document.querySelectorAll(".ads, .sidebar, .comments").forEach(el => el.remove());`）。
3. **保存為 .mhtml**：
   - Chrome 支援透過 `chrome://save-page-as-mhtml` 來存成 MHTML。
   - 也可以考慮使用 `save_as_mhtml()` 之類的函式。

你打算如何決定哪些元素是「不必要的」？要用預設規則，還是讓使用者自訂？


#### You:
網上有人實作reader mode功能，似乎可以利用在我的專案上

#### ChatGPT:
對！「Reader Mode（閱讀模式）」的核心原理就是過濾掉網頁中的廣告、導航欄、側邊欄等非主要內容，只保留主要文章。你可以利用現有的開源方案來實現你的需求，然後再導出 .mhtml。這樣可以省去自己手寫過濾規則的麻煩。

### 可行方案：

1. **Readability.js（Mozilla）**
   - Firefox 閱讀模式使用的核心庫。
   - 你可以在瀏覽器開發者工具 Console 測試：
     ```javascript
     let article = new Readability(document.cloneNode(true)).parse();
     console.log(article);
     ```
   - 這可以擷取主要內容（標題、正文、圖片等）。

2. **Mercury Parser（已停止維護，但仍可用）**
   - 這是一個更完整的解析器，可用於伺服器端或本地。
   - 可搭配 Node.js 使用：
     ```javascript
     const Mercury = require('@postlight/mercury-parser');
     Mercury.parse('https://example.com').then(result => console.log(result));
     ```

3. **Puppeteer + Reader Mode**
   - 用 Puppeteer 開啟 Chromium，然後在開啟的頁面上執行 `Readability.js`，過濾不必要內容後再存成 .mhtml。

你比較偏好哪種方式？如果你想用 Python，那麼 Playwright + JavaScript 會是一個不錯的選擇！


#### You:
幫我用Readability.js實作一段程式碼，可以直接執行在chrome devtool上。
輸出直接蓋掉原本的網頁即可

#### ChatGPT:
這段 JavaScript 程式碼可以直接在 Chrome 開發者工具 (DevTools) 的 Console 執行，使用 Readability.js 過濾不必要的元素，然後覆蓋原本的網頁內容，只留下主要文章。

### **使用方式**：

1. **打開 Chrome 開發者工具**（F12 或 `Ctrl + Shift + I`）。
2. **切換到 Console** 分頁。
3. **貼上以下程式碼並執行**：

```javascript
(async function () {
    // 載入 Readability.js
    const script = document.createElement('script');
    script.src = 'https://cdnjs.cloudflare.com/ajax/libs/readability/0.4.4/Readability.js';
    script.onload = function () {
        try {
            // 解析主要文章內容
            let article = new Readability(document.cloneNode(true)).parse();

            if (!article) {
                console.error("Readability 無法解析此網頁");
                return;
            }

            // 清空原本的網頁內容
            document.body.innerHTML = '';

            // 重新建立簡潔版的閱讀模式頁面
            let newContent = `
                <article style="max-width: 800px; margin: auto; font-family: Arial, sans-serif; line-height: 1.6; padding: 20px;">
                    <h1>${article.title}</h1>
                    <hr>
                    ${article.content}
                </article>
            `;

            document.body.innerHTML = newContent;
            document.title = article.title; // 設定標題
        } catch (error) {
            console.error("發生錯誤：", error);
        }
    };

    document.head.appendChild(script);
})();
```

### **功能說明**：

- 先動態載入 `Readability.js`（如果你已經有這個庫，可以省略）。
- 解析當前頁面的主要內容。
- 清空原本的 `document.body`，然後只顯示文章標題和內容。
- 讓頁面變得簡潔，類似「閱讀模式」。

這樣你就可以在任何網頁上快速啟動「閱讀模式」！你可以試試看，看看效果是否符合你的需求。 😊

---

# Sandboxed MHTML Script Block

#### You:
 Blocked script execution in 'file:///D:/data.mhtml' because the document's frame is sandboxed and the 'allow-scripts' permission is not set.

#### ChatGPT:
This error occurs because when you open an `.mhtml` file in a browser, it's treated as a local file and runs in a **sandboxed environment** for security reasons. By default, the browser does not allow executing scripts inside `.mhtml` files to prevent potential security risks.

### **Possible Solutions**

#### **1. Open the `.mhtml` file in a Text Editor**

Since `.mhtml` files are essentially archived webpages, you can open them with a text editor like Notepad++ or VS Code to inspect and manually extract the content.

#### **2. Use a Browser that Supports `.mhtml` Properly**

Some browsers, like **Internet Explorer (IE Mode in Edge)** or **Opera**, handle `.mhtml` files better than Chrome-based browsers.

#### **3. Convert `.mhtml` to HTML and Open Without Restrictions**

1. Open the `.mhtml` file in **Microsoft Edge**.
2. Right-click on the page and **Save As → Webpage, Complete (.html)**.
3. Open the `.html` file in a browser. This may allow script execution.

#### **4. Modify and Re-Save the `.mhtml` File**

Since `.mhtml` files are MIME-encoded archives, they often contain headers that restrict execution. You can:

- Extract the HTML part from the `.mhtml` file.
- Save it as `.html` and remove sandbox-related attributes (`sandbox`, `Content-Security-Policy`).
- Open the `.html` file in a browser.

#### **5. Use a Python Script to Extract and Modify the `.mhtml` File**

Since you're working with Python, you can use the `mhtml2html` library or `mammoth` to extract the HTML part and modify it before saving.

Would you like a Python script to automate this extraction and modification? 🚀
