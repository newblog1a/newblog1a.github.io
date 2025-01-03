---
published: true
---
綁骨架
  Blender Tut | Fish Rig and Animation Driver Expression
  https://www.youtube.com/watch?v=eEcPR-zfQAo
Blender Jellyfish Animation Tutorial
  how to model, animate and texture a jellyfish in Blender.
  https://www.youtube.com/watch?v=A_Cdz-QBlT4
How to Create, Rig & Animate a Low Poly Rabbit
  https://www.youtube.com/watch?v=iMar3keWaUo
  
綠 陀
卡其 灰


蒙特卡羅模擬是一種數學技術，可預測不確定事件的可能結果。
例如，如果您想估算新產品的第一個月銷售額，可以將歷史銷售資料提供給蒙特卡羅模擬程式。該計劃將根據一般市場情況、產品價格和廣告預算等因素估計不同的銷售價值。
  https://aws.amazon.com/tw/what-is/monte-carlo-simulation/
蒙地卡羅模擬法是以機率為基礎的亂數取樣（Random Sampling）統計模型
蒙地卡羅樹搜尋（英語：Monte Carlo tree search；簡稱：MCTS）是一種用於某些決策過程的啟發式搜尋演算法
  zh.wikipedia.org/zh-tw/蒙特卡洛树搜索


白話一點講就是，把你要統計的東西(目標靶)劃出來後，然後讓電腦蒙眼射N次飛鏢，射完之後再將中靶的飛鏢取下來，然後用你剛剛射出去的 N 跟中靶的 X 去估算這個靶有多大。


那麼我們把這個想法套在求圓周率(π)上面，
我們可以在一個2*2的正方形裡面劃出一個半徑為1的圓(此時圓的面積為π)，
電腦設出去的飛鏢會命中區域內的哪一個點完全是隨機的(每一個點被射中的機率是一樣的)。
那麼根據統計與機率的理論告訴我們，
電腦射出去後中命圓的機率為：π/4，命中圓以外區域的機率為：1 - π/4。

假設我們 N=100，飛鏢射出去命中了78 發，則我們就可以預估圓周率(π) 約為：(78/100)*4 = 3.12。
當然啦~ 有背過圓周率的你一定會說 wait wait wait what 不對欸 不對 π 明明就等於3.1415926...
由於蒙地卡羅方法是基於統計跟機率，所以你也能想像其實上面範例中的N有一點太小了，答案容易在一個比較大的區間範圍內震盪，如果今天這個N 變的非常非常的大，那麼這個答案就會收斂在一個比較小的區間，算出來的答案就會非常接近正確答案了！
  https://jason-chen-1992.weebly.com/home/-monte-carlo-simulation
  

使用蒙地卡羅方法時通常需要 2 個步驟（又或者說滿足以下 2 個條件，就等同使用蒙地卡羅方法）：

1. 定義事件的機率分佈：針對要模擬的事件，設定其隨機變數的機率分佈（例如常態分佈、均勻分佈等）。
2. 進行多次隨機模擬：基於所定義的機率分佈，對事件進行大量的隨機模擬，統計各種可能結果的出現頻率，從而預測事件的結果範圍。

舉賭博為例，假設一個賭徒本金為 100 塊，勝率為 30%，贏了可以得到 2 塊，輸了本金少 1 塊，那麼我們就可以用蒙地卡羅方法模擬 1,000 次賭徒進行 100 場賭博之後的可能結果。

Python 程式碼表示的話
  https://myapollo.com.tw/blog/python-monte-carlo-method/