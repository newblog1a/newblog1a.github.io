---
published: true
---
Wiki(英文版)的說法
If left without power, worn out SSDs typically start to lose data after about one to two years in storage, depending on temperature. New drives are supposed to retain data for about ten years.[8] MLC and TLC based devices tend to lose data earlier than SLC-based devices. SSDs are not suited for archival use.

Youtube上有人做過"用高溫模擬長時間不用"的測試, 基本上保一年不是問題, 一年以上要看顆粒Spec


SSD是以儲存單元的電位差(電壓差)作為儲存模式，長時間不通電FLASH的自然漏電會導致壓降超過限度而無法判讀，單一電位的SLC影響最小，三階電位TLC影響相對大，實際漏電狀況跟FLASH品質有關
https://www.reddit.com/r/DataHoarder/comments/1ee1pts/just_heard_first_time_that_ssds_lose_data_if_left/?tl=zh-hant
請查 SSD Data Retention

SSD本身的韌體 (FW) 其實也是存在Flash的
只是有些SSD主控的策略他會將韌體放多份, 例如每顆Flash的每個die (1片flash可以封裝多個die)各放一份,
以防漏電FW損毀
每顆Flash的每個die的韌體區都漏電光的機率比較小, 所以只能說若無放資料只是相對比較沒影響, 不是絕對...

由於韌體本身也是存放在NAND最慘是主控找無韌體
  https://www.ptt.cc/bbs/Storage_Zone/M.1673563043.A.900.html


??MLC五年不通電也不容易掉資料 TLC大約2~3年 QLC一年 但這只是概估 引響因素還有溫度、濕度...等環境因素

和SSD同樣是NAND FLASH的SD記憶卡，我就從來沒遇到過長時間放置會壞掉或是掉資料

早期的MLC和SLC記憶卡和隨身碟
超過八年沒通電都沒事
新的TLC消費碟
大概超過一點就怪怪的 read disturb
  https://www.mobile01.com/topicdetail.php?f=490&t=6619143
  
SSD、USB 行動碟擺很久沒用資料會消失？
  https://blog.darkthread.net/blog/ssd-data-retention/
  
[閒聊] SSD數據保存期(不通電會變磚?)+選購雜談
  https://www.ptt.cc/bbs/PC_Shopping/M.1607941497.A.FBF.html
  https://www.ptt.cc/bbs/Storage_Zone/M.1673609166.A.58E.html
  https://www.ptt.cc/bbs/Storage_Zone/M.1607941552.A.82E.html