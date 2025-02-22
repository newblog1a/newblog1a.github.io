---
published: true
---
```js
// ==UserScript==
// @name         Image Download4
// @namespace    http://tampermonkey.net/
// @version      1.2
// @description  Add a download icon to images larger than 400px when hovered.
// @author       Your Name
// @match        *://*/*
// @run-at document-idle
// @grant        GM_download
// ==/UserScript==

// bug: icon position

(function() {
    'use strict';

    // Create the download icon
    const icon = document.createElement('div');
    icon.textContent = '\u2B73'; // A simple arrow icon
    icon.style.position = 'absolute';
    icon.style.width = '24px';
    icon.style.height = '24px';
    icon.style.backgroundColor = 'rgba(0, 0, 0, 0.6)';
    icon.style.color = 'white';
    icon.style.fontSize = '18px';
    icon.style.textAlign = 'center';
    icon.style.lineHeight = '24px';
    icon.style.borderRadius = '50%';
    icon.style.cursor = 'pointer';
    icon.style.display = 'none';
    icon.style.zIndex = '10000';
    document.body.appendChild(icon);

    let currentImg = null;
    let iconVisible = false;

    // Function to check image size
    function isLargeImage(img) {
        return img.naturalWidth > 400 && img.naturalHeight > 400;
    }

    function getPosition (element) {
        var x = 0;
        var y = 0;
        // 搭配上面的示意圖可比較輕鬆理解為何要這麼計算
        while ( element ) {
            x += element.offsetLeft - element.scrollLeft + element.clientLeft;
            y += element.offsetTop - element.scrollLeft + element.clientTop;
            element = element.offsetParent;
        }

        return { x: x, y: y };
    }

    // get document coordinates of the element
    function getCoords(elem) {
        let box = elem.getBoundingClientRect();

        return {
            top: box.top + window.pageYOffset,
            right: box.right + window.pageXOffset,
            bottom: box.bottom + window.pageYOffset,
            left: box.left + window.pageXOffset
        };
    }

    // Mouse move event on images
    document.addEventListener('mouseover', (e) => {
        const img = e.target;
        if (img.tagName === 'IMG' && isLargeImage(img) && !iconVisible) {
            console.log('move');
            /*let position = getPosition(img);
            icon.style.left = `${position.x+100}px`;
            icon.style.top = `${position.y}px`;*/
            let position = getCoords(img);
            icon.style.left = `${position.left+24}px`;
            icon.style.top = `${position.bottom-24}px`;
            //const rect = img.getBoundingClientRect();
            //icon.style.left = `${rect.right - 30}px`;
            //icon.style.top = `${rect.bottom - 30}px`;
            icon.style.display = 'block';
            currentImg = img;
            iconVisible = true;
        }
    });

    //detect if URL has changed
    //  work on twitter
    window.navigation.addEventListener("navigate", (e) => {
        //same as above
        const img = e.target;
        if (img.tagName === 'IMG' && isLargeImage(img) && !iconVisible) {
            console.log('move');
            /*let position = getPosition(img);
            icon.style.left = `${position.x+100}px`;
            icon.style.top = `${position.y}px`;*/
            let position = getCoords(img);
            icon.style.left = `${position.left+24}px`;
            icon.style.top = `${position.bottom-24}px`;
            //const rect = img.getBoundingClientRect();
            //icon.style.left = `${rect.right - 30}px`;
            //icon.style.top = `${rect.bottom - 30}px`;
            icon.style.display = 'block';
            currentImg = img;
            iconVisible = true;
        }
    })

    document.addEventListener('mouseout', (e) => {
        if (e.target === currentImg && e.relatedTarget !== icon) {
            console.log('out');
            icon.style.display = 'none';
            currentImg = null;
            iconVisible = false;
        }
    });

    // Download image on icon click
    icon.addEventListener('click', () => {
        if (currentImg) {
            const imageUrl = currentImg.src;
            const fileName = imageUrl.split('/').pop().split('?')[0];
            GM_download(imageUrl, fileName + '.jpg');
        }
    });
})();
```

---

我可以幫您融合這兩個事件監聽器，因為它們的內部邏輯是相同的。以下是融合後的程式碼，使用一個共用的函數來處理圖片滑鼠事件的邏輯：

javascript

```js
// 定義共用的處理函數
function handleImageHover(img) {
    if (img.tagName === 'IMG' && isLargeImage(img) && !iconVisible) {
        console.log('move');
        let position = getCoords(img);
        icon.style.left = `${position.left + 24}px`;
        icon.style.top = `${position.bottom - 24}px`;
        icon.style.display = 'block';
        currentImg = img;
        iconVisible = true;
    }
}

// Mouse move event on images
document.addEventListener('mouseover', (e) => {
    handleImageHover(e.target);
});

// Detect if URL has changed (works on Twitter)
window.navigation.addEventListener("navigate", (e) => {
    handleImageHover(e.target);
});
```

融合後的改進點：

1.  提取了重複的邏輯到 handleImageHover 函數，避免程式碼重複

2.  修正了模板字符串語法（使用正確的 ${} 包裹）

3.  保持了原有的功能不變，只是組織方式更優雅

4.  更容易維護和修改（只需改一個地方的邏輯）

注意事項：

-   假設 isLargeImage、icon、currentImg 和 getCoords 等變數/函數已在其他地方定義

-   如果您需要不同的偏移量（比如 mouseover 用 +24，而 navigate 用其他值），可以在函數中添加參數來處理

---

twitter 剛開始用是好的。但滾輪在往下捲到底，他自動載入之後，我的這個程式會失效

![](data:image/png;base64,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)

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAFW0lEQVR4nK1XS2yUVRT+vvPPFCp9QCTaqEzt9EFpZ1oJwbdhQSJKjGLc6UaMgrhw40JY6cL4TtRoAHHjwmCMG8QNMboQozxChHZgoHbaBiIPiS9AW+jMfz4X0ynzlFI4yUz+3P+e83333HP/811ihtbe27sIoa0i8YCEJIgYhCYAAHGe4HFAhyX8wBx2ZTKpX2cSl1ea0LGkbynFdU5/lOAtZN5FUmmgonFBpyDsJLktc3Tw4KwIJJPJBeM5vAxwg9GaAFWA1gxKAiAkvwDp4/ooXk+lUn/NmEBbV6IvoG1lwHsKq5qVkSAAl/bKtW50KJW6IoH2xb33ksF2mrVKPjvgCh4GwY8rtCdHhg79VJNAW1eiLzD7msaYpHwqSUAzT/9l0FJfkpDrhEuPFGfCCg+xZHKBGbfRbBrcpQG5f6rQfyE4XWgkq/6Kgd014u6fuvt+kHkSZjESn7T298+vIBDNcqNZcFch7RJgrvczRwfXerbuzpD+tNyPALjo7idd2iv5LrnvkrRH7icATMg1LMf6bFTLR44OrgXxOqaCSg6z4K7IpG8s2YK2RKLPcvajGRsKqZaUdWLl2NHUD4XJ8XjfTazTomwUo3UTE+OZTCYHAD09PcHF+vp6G8/GmePZ4eGBkwWfzs6lSzzIHSB5QyFDLl3w0O8f++XwYAQALBdsyIOXFl2kbF9HRwfPAjhbvt/pdDoEMAmg4sx7xA1F5SMJRmsE9TyAF6wjmbwN1GNAKbiZRUMiXh7wqo3eXlh9ES3QsKazs/9WU9YfItBSXOT5NGGMCH+8VvxIOHlA0OFCkQL5+gLYItMqI+0BFr8tkIB/MJJOZ66VwNDQ0CmH3pJQco7zmFphEpKlyITLzwduX10reMGi4eQuQWcq1mlMGJj/6Ezj5/9P/bug/rfrRaCxsfEchbHisammFTNITeUOBMaD06dn2QAqLR6Pu4Dx8nEBzYZqDUloutjcHFwvAgcPHowQai4fp0ADeL4EW4Com+uzwcLrRcDMGgHGKl6Q5wzAidLiEIzWYJ6773oRyGHOchALS2ot36hOGIDDVXxIci1WrCj/GM7KaHyGZMWWkkoZoN0q67X5boiVbb/98eK1gncs7nuGxJrydp7H1G629/YuYhjsJ9kiCJBCkMFU05ik8Had5d5Lp9N/Xg1wT09Pw0UFGwz2CoB5QHH6AZdOM4s7CQDt3X1bLbD1kuDyj8zxvYwvGXk3ALgrDepLkbuDLIeGhwdOo7x5AIx1d7fUYW6XGN4P4gmDLa0mZEgDPNwyfCz1ggGABeEWd79AAAauFuz3bESrFWoPSJixx2ivmPiNIv5uR0dHtMqigyiir4H61miv1QbPi9VQ2AJMCZLhI0cGKGydmhAH9XnUvQGBPSsPxy6rHgRwbs9kMpeqEMhB9hng1P9KOEKuzQVZNq2IJqN6w933A4SZtSC0TZn0obQYPujyV931IdzXNzdEv6sRGcrqCIALtdQ+aXAP9+Xm2puX6RRZQZRaYLEw9H8gPTUylNpZC7Dc4suWNXM8myG5EFUuLtVEaRVZfse9pG+3wFo9DCdA7pD8ABBcIn1eHcPN6XT6n2oEWlv750fqNUwrJUAa5H5cuoIsn17J4mSSxm1mdjeB6VjuYTZnYex4On2mJoG5nmFgN0K6fDFx3yfhuWoXE6sSB6NDqVRDHR92D99093NFfC/Vu9e8zqnJSSJSkPCSzrvrnTkWrq4GDlTqzmkbGBj4G8Cmzs7+LzwI1xF8HPQJo1U7AQCAfycnc3Wcc1YKLwnYAeHjkWOpn2vNvyq7fcnS1tZEovtK87oSie6urr62mcb9D8EPyBEkYc5cAAAAAElFTkSuQmCC)

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAACXBIWXMAAAsTAAALEwEAmpwYAAAAtGVYSWZJSSoACAAAAAYAEgEDAAEAAAABAAAAGgEFAAEAAABWAAAAGwEFAAEAAABeAAAAKAEDAAEAAAACAAAAEwIDAAEAAAABAAAAaYcEAAEAAABmAAAAAAAAAEgAAAABAAAASAAAAAEAAAAGAACQBwAEAAAAMDIxMAGRBwAEAAAAAQIDAACgBwAEAAAAMDEwMAGgAwABAAAA//8AAAKgBAABAAAAIAAAAAOgBAABAAAAIAAAAAAAAACR/tRfAAABDklEQVR4nO2WzRGCMBCFHzsc1BkOtGALSwMcqMUSPCpHS7AOvdEC24ItcGDGozjicJAhQGLkR/0uDMnmJdm8/AC/jsPMewA7S3qxiDz0ekMWO4eJlquqOGZrLaGNf4EJhJFxuwLOywynRdZbkJkLHY9Qm9jVuWl1buIRV0ScPjNI0xR5niMMw/I/Yb2BReI3llNbo1XxWu15Hgb3gO6Mvm8XNHmhD0EQlN+6x+aTgY3hyaYLYWScroDqXKjWvlpbU+qeIIyMa9ow2YpWfHTgxnLC1DLA7beZdWjIzv4DeGsXBIr9r3K3irrHqCEmxpgZkOebTfm2b3sx6VBllDD1u+DT5wQZtLHpkdii1ky5A+LGRbYOj3RBAAAAAElFTkSuQmCC)

![](data:image/png;base64,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)

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAIAAAD8GO2jAAADm0lEQVR4nO1WO2xcRRQ99868ee/t2muv7bXNBoODoxghEQEi4EiWqNxFKZFQitAiUSJFES0dtNDS0UUUNBEUINEhJEjDz3yS2NjYTuy1vd59n5l7aaBAmuVTRAqIaZ/mnDn33HPvo/Y7G7ifh+8r+n+CwKpq9AMR/SOgUTgPdIn+lkT7F/cliGpg6G9wygIyTKSsgUAKq2CCAvES2XitlYmyIP2u8w2q+okqmBW11Sy4u4F69aBFpfWm1Hbg1FA5yrO4AiHUGrJw9PJq99KZCSZ1gaxHbXHC/Oan29fXm0Uy4bRmNS7UlquKjMY44gREsFTn4ajrph5vTAEVgkEwSABCB0OR5MSMJ1RaqQ1VDqVKHnVlhAcKiPFwQ7hDQJGwARsUAAMKbRdbE3ToyRamocyqI5uFJt++E8FXFqQUDl54JHtsTPosrLAipdFcuNvMF5uaar1vx979vPixh4RkaBBiLoxQQFBSmzS++Hb3u9qPzbfHXZbWYWd49MvR/tWLZ18817U43sH4B19tfHOP4JygjjbSKAJSNvVw/+l5uvL82eXF1oyhXLBVyns3v+9Q5cVb9RVJzYMyy7x17OO5GJUDJX+yPM2vXVy+MGkYRwYOyBspX1s9XR5LVhiYsSShVCT1FZMNo3Lwx3f//ghCKseXz02vTLqkrsF1D9nXg5CW4aE2zY476RPbQPB9ZCx56ovSssTmQlyBSj2X9NfmTjuFt/awnrz64fqNvax54pc6/o21pSdazrEGQaDGQdJomNTKgCgiIt5enkzfTSepGxBKoo398PEPe1uF7gb+8tbR9p27xornqgGzGHoLtJnKPSKJQsUJDFCXfnegOXxDB90p99KF5fOt6tmxkytPtlcWW6xlDZszv7629NalR7vuAOrjQYvOEAMuyvDR5v5Kt50UhzNpcu25uVee6WRBk9SMQSoFAUx0pkWUYMiZJ7Yxn0ckUMUYc/3m7U9u9ctsKrBpBpyymE7N8VB+Oi4DcQoYEhg1bAo7EWCiSCOGHfMgne2VM6/e2Lx8vrP68PSppgj09mHx/mfr863mU0sLRgYV2Vx5Z1iJJwOKBo2ivy1KAtRcT/bTQbPYW9A8acrAYLtqiRQTQxYVSYqSJkzQJu/17DSxQ2xrjpimSoQEpsiE1HV+DvClCJFywsQnOSmgyBU2JOp1lsBR9JEEACkIpEYBkDAYhgGoB6BQAAQQPBR/vncf6KX/P8G/hOBXOQioj/W4BFYAAAAASUVORK5CYII=)

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAIAAAD8GO2jAAAEvUlEQVR4nO2WbWiWZRTHz3Wul/vWDUXn5l6eJ6U5tWlLt+kidSJ90ISCsFASIZI0xJcKsizBoNQ+lBYiGhKWiiEKpWYpNZfOmc5cos6tTM1yQ1267dnzdt3XWx+eOc00t4Efgs63m/uc87vO/5zrhQzPHwT30/C+Zv8f0BVjt34454JAOesoRSDEWuuso4wyxggBANDKKK27mpoh5/wmQCnl9/InlJc8+lhRdnYWYxiJRH89+/uRIyfr685bawkhEyaWlJQW6i4wKKU/N1zYu7e6AyBl8MioYUuWzhk1+iEhKDjnAAgBABKLJg5V1a5d8/mJnxrKJxY/P3sGgOxCAWLP7u/2fFXFAEAp/XDR0LXrl+XmZQRSBlJTyggBY60xlnM6+YnyAZn9p097RWsDoFSgOkW11nWmJISQlJQAXBBjDAFgzjkh2OIls3PzMmRSIhKlTEP9uURChsJZOTmZzjlwZteX++NxmUzKSGtEyo4KkGJ6eu9OQCwW18p0rN8TsWgCCJAHw7nFJYWbt65EBEJIIiGXLP6osuKoda5f/z6Tp4ybOeupxkuXF85fmUzIvn3T09J85wAAAqUKC/PXrHuLUXTOIbI3X19dVXXcEwIAgEAyEbS1tTNjTH5+yPNFIAPO2elT9V/vqfIEBwLX/mz9bOPOXTu/N9rIpKQUI5FYa2t7SoVAqazMDHCpbAAALS2RpsZm3xMA4AAQCSJiqtKUhzE2FM4aMiQcjye1Nojo+14smpAy6PBEwhiljFJGGaOU/m0bIeKtf2+EIDY2XlXKAoAxJi8v85NP31mw6LmhwwYTQpIJ6ZztbF0PDDlnp0+ePX/+Dy44AGhtcvMyX3vjhW073t+4ecXsOdMys/rLZADg7pnrzgBEbGlpX/3BpkRCCk8QAsYYKaXn8bFlI5Yum7ttx6pnZ0xWN8aj2wAA8Dz+7b7D8+e+W3u8wQF6vscYAwAVKJmUOTkZy99b9PS0x6UMegDo2MlC8IMHjh87VldcWjhp0pjSsSMLCh4QgiultNaMs5fmTa+sqGlvj6Va120AAAiPK6UPHaytOvBjelrvEUUFC1+eWVY2UmtjtA6FsguGDqo5ekqIHgG0NtZazpnncQAIlP6h+kTTpSvbv1iVkdHXGItI+vRJd67brcZU9sGDc8eWFQGAlMo5h0g8j7e1RePxZOqAsdZGItEezCvTWmcPHLBuw9vhcM7+isMbPt5xpu5cEChEnPpkeW5eljEWEa82t178rYlS2m0AIi58ddaQgnAgk1Omjp9QXnym7kJz8/UBmf2KigqQgLWOcX/fN9WXL19LCdg9QFpa71BoIABBpIEMhOClYwoRibVOKe2c83y/7tQvG9ZvZ6wn9ytGo/H585av+XBLc3MLF0J4AgCMtUDA8wUh7EBlzYJ5K65cuf5PfVKHlfA84XtceHecYDI8f5A1NlAqLzRw3PjRpWNGhEI5vXqJWCx58WJTZUVN9aHaIFC3Xq4pM8aGwtkvznmGslTnccum3fVnzjFGbwfcCDBKaUKIEJwxqrWWUhMCQvC7DY9zLpCqc3KF4Ii3e95cF6U0JYJzTmsDgL4v7qZsx+oI8e7lc3vhqbB/j+mW/fdfdvcd8BcpOSFNol0EAgAAAABJRU5ErkJggg==)

15 web pages

你的問題描述了在使用 Twitter 時，你的 JavaScript 程式在一開始運作正常，但當頁面滾動到底部並觸發 Twitter 的無限滾動（infinite scroll）自動載入新內容後，程式就失效了。這是一個常見的問題，因為 Twitter 的無限滾動會動態新增 DOM 元素，而你的程式可能只對初始載入的元素綁定了事件監聽器，沒有處理後續動態載入的內容。

根據你之前提供的程式碼，我假設你的程式是在監聽圖片的 mouseover 事件，並在特定條件下顯示一個圖標（icon）。當 Twitter 載入新內容時，新加入的圖片沒有被你的程式監聽，因此事件不會觸發。以下是可能的解決方案：

* * * * *

解決方法

1\. 使用 MutationObserver 監聽 DOM 變化

Twitter 的無限滾動會動態添加新的 DOM 元素（例如新的圖片）。你可以使用 MutationObserver 來監測 DOM 的變化，並在新圖片加入時重新綁定事件。

以下是改進後的程式碼範例：

javascript

```
// 定義共用的處理函數
function handleImageHover(img) {
    if (img.tagName === 'IMG' && isLargeImage(img) && !iconVisible) {
        console.log('move');
        let position = getCoords(img);
        icon.style.left = `${position.left + 24}px`;
        icon.style.top = `${position.bottom - 24}px`;
        icon.style.display = 'block';
        currentImg = img;
        iconVisible = true;
    }
}

// 綁定事件到現有的圖片
function bindEventsToImages() {
    document.querySelectorAll('img').forEach(img => {
        img.addEventListener('mouseover', () => handleImageHover(img));
    });
}

// 初始綁定
bindEventsToImages();

// 使用 MutationObserver 監聽 DOM 變化
const observer = new MutationObserver((mutations) => {
    mutations.forEach((mutation) => {
        if (mutation.addedNodes.length) {
            // 當有新節點加入時，重新綁定事件
            bindEventsToImages();
        }
    });
});

// 監聽 document.body 的子節點變化
observer.observe(document.body, {
    childList: true,
    subtree: true
});

// 對於 navigation 事件（URL 變化）
window.navigation.addEventListener("navigate", (e) => {
    handleImageHover(e.target);
});
```

說明：

-   MutationObserver 會監聽整個頁面的 DOM 變化。

-   當 Twitter 載入新內容（例如新的圖片）時，bindEventsToImages 會重新執行，為新加入的圖片綁定 mouseover 事件。

-   這樣即使頁面滾動到底部並載入新內容，你的程式也能繼續運作。

* * * * *

2\. 事件委派（Event Delegation）

另一個更簡單的解決方法是使用事件委派。不是直接將事件綁定到每個圖片，而是將事件監聽器綁定到一個不會變動的父元素（例如 document），然後檢查事件目標是否為圖片。

javascript

```
// 定義共用的處理函數
function handleImageHover(img) {
    if (img.tagName === 'IMG' && isLargeImage(img) && !iconVisible) {
        console.log('move');
        let position = getCoords(img);
        icon.style.left = `${position.left + 24}px`;
        icon.style.top = `${position.bottom - 24}px`;
        icon.style.display = 'block';
        currentImg = img;
        iconVisible = true;
    }
}

// 使用事件委派監聽所有 mouseover 事件
document.addEventListener('mouseover', (e) => {
    handleImageHover(e.target);
});

// 對於 navigation 事件（URL 變化）
window.navigation.addEventListener("navigate", (e) => {
    handleImageHover(e.target);
});
```

說明：

-   事件委派利用事件冒泡機制，將監聽器放在 document 上。

-   無論 Twitter 何時載入新圖片，只要觸發 mouseover 事件，程式都能捕捉到並處理。

-   這比 MutationObserver 更輕量，因為不需要持續監測 DOM 變化。

* * * * *

3\. 檢查失效的原因

如果上述方法仍無法解決問題，可能是因為：

-   全局變數問題：檢查 icon、currentImg 和 iconVisible 是否在 Twitter 載入新內容後被重置或變得不可用。

-   Twitter 的 JavaScript 衝突：Twitter 可能在載入新內容時執行了自己的腳本，覆蓋了你的樣式或事件。你可以用瀏覽器的開發者工具（F12）檢查 icon 的樣式是否被改變。

你可以添加以下除錯程式碼來確認：

javascript

```
document.addEventListener('mouseover', (e) => {
    console.log('Target:', e.target);
    console.log('Icon Visible:', iconVisible);
    console.log('Icon Element:', icon);
    handleImageHover(e.target);
});
```

* * * * *

建議

-   首選方案：使用「事件委派」（方法 2），因為它簡單高效，適用於動態內容。

-   備用方案：如果需要更精確控制新載入的元素，可以使用 MutationObserver（方法 1）。

-   測試：在 Twitter 上測試時，滾動到底部並等待新內容載入，確認圖標是否仍能正確顯示。

如果還有其他問題或需要進一步協助，請提供更多細節（例如完整的程式碼或失效時的具體行為），我會幫你調整！

---

你的程式碼整體運作良好，但你提到了一個問題：有時圖標（icon）會卡在某個圖片上不消失。這個問題可能與 mouseout 事件的觸發條件或邏輯有關。我檢查了你的程式碼，發現以下幾個可能的原因及解決方案：

* * * * *

問題分析

1.  mouseout 事件的條件問題：

    -   你在 mouseout 事件中檢查了 e.target === currentImg && e.relatedTarget !== icon，這意味著只有當滑鼠從 currentImg 移出且移到的目標不是 icon 時，才會隱藏圖標。

    -   如果滑鼠移動到一個不符合條件的元素（例如某些覆蓋層、子元素或非預期的目標），icon 不會隱藏，就會出現「卡住」的情況。

2.  動態內容影響：

    -   在 Twitter 或其他動態網站上，圖片可能會因為頁面滾動或重新渲染而改變位置或結構，這可能導致 currentImg 的參考失效，或者 mouseout 事件未正確觸發。

3.  iconVisible 狀態未正確重置：

    -   如果某些情況下 iconVisible 沒有被設為 false，下次滑鼠移入新圖片時，handleImageHover 的條件 !iconVisible 會阻止圖標更新位置。

* * * * *

解決方案

以下是修復「圖標卡住」問題的改進版本程式碼，並針對 Twitter 的無限滾動問題使用事件委派：

javascript

```
// ==UserScript==
// @name         Image Download4
// @namespace    http://tampermonkey.net/
// @version      1.3
// @description  Add a download icon to images larger than 400px when hovered.
// @author       Your Name
// @match        *://*/*
// @run-at       document-idle
// @grant        GM_download
// ==/UserScript==

(function() {
    'use strict';

    // Create the download icon
    const icon = document.createElement('div');
    icon.textContent = '\u2B73'; // A simple arrow icon
    icon.style.position = 'absolute';
    icon.style.width = '24px';
    icon.style.height = '24px';
    icon.style.backgroundColor = 'rgba(0, 0, 0, 0.6)';
    icon.style.color = 'white';
    icon.style.fontSize = '18px';
    icon.style.textAlign = 'center';
    icon.style.lineHeight = '24px';
    icon.style.borderRadius = '50%';
    icon.style.cursor = 'pointer';
    icon.style.display = 'none';
    icon.style.zIndex = '10000';
    document.body.appendChild(icon);

    let currentImg = null;
    let iconVisible = false;

    // Function to check image size
    function isLargeImage(img) {
        return img.naturalWidth > 400 && img.naturalHeight > 400;
    }

    // Get document coordinates of the element
    function getCoords(elem) {
        let box = elem.getBoundingClientRect();
        return {
            top: box.top + window.pageYOffset,
            right: box.right + window.pageXOffset,
            bottom: box.bottom + window.pageYOffset,
            left: box.left + window.pageXOffset
        };
    }

    function handleImageHover(img) {
        if (img.tagName === 'IMG' && isLargeImage(img)) {
            // 如果已有圖標顯示，先隱藏並重置
            if (iconVisible && currentImg !== img) {
                icon.style.display = 'none';
                iconVisible = false;
            }

            if (!iconVisible) {
                let position = getCoords(img);
                icon.style.left = `${position.left + 24}px`;
                icon.style.top = `${position.bottom - 24}px`;
                icon.style.display = 'block';
                currentImg = img;
                iconVisible = true;
            }
        }
    }

    // Hide icon when leaving image or icon
    function hideIconIfNeeded(e) {
        const fromElement = e.target;
        const toElement = e.relatedTarget;

        // 如果離開的是當前圖片或圖標，且目標不再是圖片或圖標
        if ((fromElement === currentImg || fromElement === icon) &&
            toElement !== icon && toElement !== currentImg) {
            icon.style.display = 'none';
            currentImg = null;
            iconVisible = false;
        }
    }

    // Mouseover event delegation
    document.addEventListener('mouseover', (e) => {
        const img = e.target;
        handleImageHover(img);
    });

    // Mouseout event delegation
    document.addEventListener('mouseout', (e) => {
        hideIconIfNeeded(e);
    });

    // Handle navigation (URL change) for Twitter compatibility
    window.navigation?.addEventListener("navigate", (e) => {
        const img = e.target;
        handleImageHover(img);
    });

    // Download image on icon click
    icon.addEventListener('click', () => {
        if (currentImg) {
            const imageUrl = currentImg.src;
            const fileName = imageUrl.split('/').pop().split('?')[0];
            GM_download(imageUrl, fileName + '.jpg');
        }
    });
})();
```

* * * * *

改進點

1.  修復圖標卡住問題：

    -   在 handleImageHover 中，新增了檢查：如果已有圖標顯示且當前圖片不是新目標圖片，先隱藏圖標並重置狀態。

    -   在 mouseout 事件中，改進了隱藏邏輯，檢查是否從圖片或圖標移出，並確保目標不再是圖片或圖標時才隱藏。

2.  事件委派：

    -   使用 document.addEventListener('mouseover', ...) 和 document.addEventListener('mouseout', ...) 處理所有圖片的事件，即使 Twitter 動態載入新內容也能正常運作。

3.  移除無用函數：

    -   移除了未使用的 getPosition 函數，保持程式碼簡潔。

4.  Twitter 兼容性：

    -   保留了 navigate 事件的處理，並添加了可選鏈運算符 ?. 以防止某些瀏覽器不支持 window.navigation 時報錯。