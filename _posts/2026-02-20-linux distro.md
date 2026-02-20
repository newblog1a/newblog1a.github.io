---
published: true
---
FreeBSD開發進度比Linux保守。在Linux用戶看來，現在還在堅持UNIX哲學的用戶很容易被當成瘋子，例如反Systemd用傳統init，批評Wayland breaks everything堅持使用X11等等。而他們的觀點來自於這些軟體不應該只服務Linux，還要考慮其他Unix-lkie的移植性才對。嗯，雖然BSD市占率不足1%。
  https://www.facebook.com/IvonHuan/posts/pfbid033iKrb3mezCLwMKdXG5rhxYu8DfwhEapgr9DDXJ2Ta8YoLKsGJ7d9AFjKzPvQyGdFl
  
最近Bazzite這個Linux發行版蠻紅的，很多人都想在遊戲掌機和Windows電腦安裝，達到類似SteamOS的效果。
這個系統是專門為玩遊戲設計的Fedora Atomic分支，並非基於SteamOS開發。試圖用bootc原子化更新+容器化的架構去模擬SteamOS的體驗。提供Steam全螢幕模式以及桌面模式。
實際裝了之後，Bazzite還不錯，雖然bloated（安裝ISO高達8GB）但是預裝的都是合理的軟體，是遊戲玩家會需要的東西。
  https://www.facebook.com/IvonHuan/posts/pfbid021gvfYQL6RNCi8U1T6HUw4tHia9Q1oXTM2asYvcD5vsbmxo6Yxzci3YSdZj2DUQ5hl
  
Fedora > CentOS Stream > RHEL > Rocky Linux與AlmaLinux
  不過跟Fedora比起來，CentOS Stream套件是缺很大的，沒有Fcitx5。需要重度依賴EPEL與RPM Fusion補充。
    EPEL是由Fedora開發者為各個版本的RHEL所維護的套件庫
  RHEL下游的Rocky Linux與AlmaLinux，則是由另外一群開發者維護。
  https://www.facebook.com/IvonHuan/posts/pfbid02u3QvmmDVqzWc4FeeESWeVw96LL3jHiMvNkjTDM4ENsjuteN3kYixPR9tTPnAfhhNl
  CentOS 被 Redhat 毀了，Rocky Linux 跟 almalinux 被 Redhat 毀了。只有Fedora還可以用。
  
Debian → Ubuntu → Linux Mint
  另有 Debian → LMDE
  Linux Mint（主流版）基於 Ubuntu LTS 版本
  另有 LMDE：Debian → LMDE
  
【2026年】適合新手入門的GNU/Linux發行版
  https://ivonblog.com/posts/linux-distros-for-beginners/
  
GNOME是Linux桌面的王，一半以上發行版都採用其為預設桌面，Ubuntu、RHEL、SUSE都在用，但是...
綜合Hakcer News的評論，GNOME桌面是一群不懂使用者的開發者做出來的桌面環境。有個比喻是說：GNOME開發者每天醒來就是在想怎麼折磨用戶。
  https://www.facebook.com/IvonHuan/posts/pfbid0mndLrsUvRkQBzJHszENvscobAW7v8GWXPgenGqtn7SNh5TXayjbGRp4temx4axHjl
  
systemd vs 傳統 init 系統（如 SysV init、OpenRC、runit 等）
  Sysv init末日已至。GNOME早就與Systemd深度綁定了，KDE遲早也會跟上潮流。
  https://www.facebook.com/IvonHuan/posts/pfbid06iX16kNWbddxcxWVn8R5T4w7AJmSybZ7RYLAAoNEq6ggLtCW59hsGDHjuYPz7NWXl
  Systemd有什麼問題？ 對喜好BSD用戶來說破壞了Unix原則

GNU/Linux桌面有往immutable不可變發展的趨勢，或者用更精確的術語來說，是Atomic原子化更新。
Android 走的方向看起來是對的，早在十年前，將系統分區掛載為唯讀，軟體透過不需要root權限的APK分發，系統更新用OTA覆蓋image，本身就是一個正確的選擇。
Linux社群發展出來的Flatpak和Snap，大概是新時代安裝軟體的最好方式，最接近Android分發APK的體驗。
  https://www.facebook.com/IvonHuan/posts/pfbid036Ne34Ba6w6Hjdx2bmgLfG6B5BC1MXiGhUJ83tJ5xfZHzLwFqfLEaH7oqannZiNa9l
  
  