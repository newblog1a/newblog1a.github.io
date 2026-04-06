https://x.com/dongxi_nlp/status/2040774718211371476

Fetching thread on X

這條 X 貼文（來自 AI 研究者

[@dongxi\_nlp](https://x.com/dongxi_nlp)

）分享了他使用 Anthropic 的 Claude Code（簡稱 cc） 幾天後，最大的心得：

Transcript is chronological  
File content is stateful

簡單翻譯與解釋這句話是在講 Claude Code 處理上下文（context） 的兩種不同方式：

1.  Transcript is chronological（對話記錄是按時間順序的）
    -   Transcript 指的是整個聊天記錄、使用者與 AI 的對話歷史。
    -   它會嚴格按照時間順序保留和處理。
    -   意思是：AI 會記得你之前說過什麼、做了什麼步驟，先後順序很重要，不能亂。
    -   這很像一般的聊天機器人，歷史訊息就是一條時間線。
2.  File content is stateful（檔案內容是狀態式的 / 有狀態的）
    -   File content 指的是程式碼檔案、專案中的文件內容。
    -   它不是單純按時間順序堆疊，而是維持一個「最新狀態」。
    -   無論你之前改過幾次檔案，AI 永遠看到的是目前最新的版本（就像一個活的、持續更新的狀態）。
    -   每次你修改檔案後，AI 都會更新這個「狀態」，不會因為時間順序而把舊版本混進來。

為什麼這很重要？（為什麼是「最大收穫」）在開發 AI 程式碼代理（coding agent）時，最常見的兩個大問題就是：

-   對話歷史混亂：如果把檔案修改記錄也混在時間線裡，很容易搞不清「現在檔案到底長什麼樣子」。
-   檔案狀態過時：AI 可能還在用很久之前的舊檔案內容來思考，導致生成的程式碼出錯。

Claude Code 的做法很巧妙地把兩者分開：

-   對話（Transcript） 用時間線來保持邏輯連貫性（你說了什麼、AI 怎麼回應、你又要求什麼…）。
-   檔案（File content） 用「狀態機」的方式，永遠給 AI 一個乾淨、準確的當前專案狀態。

這樣就能大幅減少「幻覺」（hallucination）和「檔案過時」的問題，讓 AI 在複雜的程式開發流程中更可靠。
