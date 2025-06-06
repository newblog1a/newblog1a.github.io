---
published: true
---
## DNS 

我們每天經常使用的網站這麼多，像是 Facebook 、Gmail 、Dcard、Slack，如果世界上所有的網站，都使用 IP 位置來做位置的辨別，那麼你可以想像，我們可能要熟記好幾種數字組合才能夠維持正常的生活。

DNS 的出現就是為了解決這個問題
只要在搜尋列上輸入 facebook.com 就能夠快速前往臉書的網站
幫每個 IP 位置取個名字

## A Record 與 CNAME

- A Record : A 代表 Address ，也就是紀錄 IP Address 與 **網域名稱** 的配對。
- CNAME ： CNAME 是網域名稱的別名，用來將子域名指向另外一個主機的域名。最常見的就是將 [www.abc.com](http://www.abc.com/) (子域名） 指向 [abd.com](http://abd.com/)（購買的主域名） ，避免使用者因為意外而找不到網站。
- ALIAS : 與 CNAME 記錄很相像，差別在 CNAME 別名只能用於子域名，而 ALIAS 記錄能夠用在主域名。但這麼做會影響對主域名的域名解析，所以不能與 A 紀錄同時使用。

## 方案一：直接透過 A Record 設定

只要將它指向 Github 伺服器 的 IP ，也就是：
185.199.108.153
就可以讓域名順利解析為 Github 主機的位址了

## 方案二：CNAME 搭配 ALIAS 設定

在方案二中，我想要以將原來沒有 www. 前綴的主域名統一導向到有 www. 前綴的子域名的方式來完成：

* 將主域名的 ALIAS 設為加上 www. 前綴的子域名
* 將加上 www. 前綴的子域名， CNAME 設為 Github Page 的 URL

```
@	ALIAS	www.example.com
www	CNAME	example.github.io
```

通常會在域名前面加上 www. 當作網站位置是為了提醒用戶，這是一個公開的網站，藉此提升體驗。
加上了www. 前綴的域名，相對於主域名來說只是子域名而已，只是站在 SEO 或是追蹤流量的角度，一般還是建議兩種方式選一種（要不要加上www.) 並且統一使用，否則 Google 爬蟲會將這兩種域名視為兩個不同的網站。

ref
https://medium.com/%E5%89%8D%E7%AB%AF%E5%AF%A6%E5%8A%9B%E4%B8%89%E6%98%8E%E6%B2%BB/%E5%80%8B%E4%BA%BA%E6%8A%80%E8%A1%93%E7%AB%99%E4%B8%80%E6%8A%8A%E7%BD%A9-%E9%83%A8%E8%90%BD%E6%A0%BC%E5%BB%BA%E7%BD%AE%E5%A4%A7%E5%85%A8-%E4%BA%8C-%E5%B0%87-github-page-%E4%B8%B2%E4%B8%8A%E8%87%AA%E5%B7%B1%E7%9A%84%E5%9F%9F%E5%90%8D-8f7e11cf2687
