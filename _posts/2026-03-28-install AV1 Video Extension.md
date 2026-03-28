## 如果你想確認 AV1 Video Extension 是否已正確安裝，可以執行：

`Get-AppxPackage *AV1VideoExtension*`

## install

下載正確的檔案
去 https://store.rg-adguard.net/
輸入 AV1 Video Extension 的 Store 網址：
https://www.microsoft.com/store/productId/9MVZQVXJBQ9V

**右鍵下載**

**直接安裝 .AppxBundle，不要解壓縮成.appx**
`Microsoft.AV1VideoExtension_2.0.7.0_neutral_~_8wekyb3d8bbwe.AppxBundle`

grok:
```md
當你**直接用 Add-AppxPackage 安裝 .appxbundle** 時，Windows 的 Appx 安裝引擎會：

-   自動解析 bundle 裡面的內容
-   自動選擇適合你系統的版本（通常是 x64）
-   正確處理內部的相依性（dependencies）
-   避免版本衝突或架構不匹配

而當你**解壓 .appxbundle 後，只安裝單獨的 Microsoft.AV1VideoExtension\_8wekyb3d8bbwe.x64.appx** 時，會發生以下問題：

1.  **相依性驗證失敗** → 系統認為缺少某些 framework 或其他必要元件（這正是你看到的 **0x80073CF3**「dependency or conflict validation」錯誤的主要原因）。
2.  **版本或架構資訊不完整** → bundle 會帶有完整的 manifest 和資源索引，單獨的 .appx 有時缺少這些資訊。
3.  **衝突更容易發生** → 尤其當系統中已經有舊版 AV1 Extension 的殘留時。
```

# other

## install .appx error

```
PS C:\Users\xxx\Downloads\Microsoft.AV1VideoExtension_2.0.7.0_neutral_~_8wekyb3d8bbwe        λ Add-AppxPackage -Path C:\Users\xxx\Downloads\Microsoft.AV1VideoExtension_2.0.7.0_neutral_~_8wekyb3d8bbwe\Microsoft.AV1VideoExtension_8wekyb3d8bbwe.x64.appx                              Add-AppxPackage : Deployment failed with HRESULT: 0x80073CF3, 套套件件無無法法進進行行更更新新、、相相依依性性或或衝衝突突驗驗證證。。
                          來來自自  (Microsoft.AV1VideoExtension_8wekyb3d8bbwe.x64.appx)  之之套套件件 Microsoft.AV1VideoExtension_2.0
                                                        .7.0_x64__8wekyb3d8bbwe 上上使使用用目目標標磁磁碟碟區區 C: 進進行行的的部部署署 Add 操操作作失失敗敗，，錯錯誤誤為為 0x80073CF3。。請請參參閱閱 http://go.microsoft.com                                                                      /fwlink/?LinkId=235160，，了了解解如如何何診診斷斷應應用用程程式式部部署署問問題題。。
                      NOTE: For additional information, look for [ActivityId] 02627ccb-b2d2-0004-620e-1e03d2b2dc01  in the Event Log or use the command line Get-AppPackageLog -ActivityID 02627ccb-b2d2-0004-620
e-1e03d2b2dc01
At line:1 char:1
+ Add-AppxPackage -Path C:\Users\xxx\Downloads\Microsoft.AV1VideoExten ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : WriteError: (C:\Users\john\D...d8bbwe.x64.appx:String) [Add-Ap
   pxPackage], IOException
    + FullyQualifiedErrorId : DeploymentError,Microsoft.Windows.Appx.PackageManager.Commands
   .AddAppxPackageCommand
```

## 解壓縮.appx後，找不到.exe

AV1 Video Extension（以及大多數 Microsoft Store 的「Video Extension / Codec Extension」）不是傳統的 .exe 應用程式，而是 系統層級的 Media Foundation Codec Extension。
它主要由 DLL 檔案 + AppxManifest.xml 組成，沒有獨立的 .exe 可執行檔，所以你解壓 .appx 後找不到 .exe 是完全正確的。
那篇 CSDN 文章裡提到的「解壓 .appx 得到 .exe」只適用於有主視窗的普通 UWP App（例如某些遊戲或工具），不適用於 AV1 Video Extension 這種 codec 套件。
