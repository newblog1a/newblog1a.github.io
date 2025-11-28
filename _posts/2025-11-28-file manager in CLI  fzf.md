---
published: true
---
## 終端機的檔案總管 - fzf
fzf 是個命令列的模糊搜尋器，它會以輸入字串為搜尋目標，列出可能的結果清單

執行 `fzf` 命令就可以叫出當前目錄的列表，使用者可以輸入字串來限縮列表的結果。

    https://ithelp.ithome.com.tw/articles/10273714

雖然 fzf 預設是搜尋資料夾內的所有檔名，但 fzf 是可以接受 pipe 進來的資料，以行為單位搜尋

    https://myapollo.com.tw/blog/fzf-ease-your-life/
	
Is it possible to use fzf (command line fuzzy finder) with windows 10 git-bash?

    https://stackoverflow.com/questions/61943778/is-it-possible-to-use-fzf-command-line-fuzzy-finder-with-windows-10-git-bash
	
### No preview window on Windows

In the Git Bash shell that comes with Git for Windows, most of the fzf commands do not work. Running them opens an empty cmd shell.

    https://github.com/junegunn/fzf.vim/issues/1242
	
Q: fzf如何使用? 選了檔案後?

`fzf` 本身只是 **互動式模糊搜尋工具**，它的工作是「讓你選出一個字串」，選完後會把選到的字串 **印到標準輸出 (stdout)**，接下來要怎麼「用」這個字串，就看你怎麼接在後面。

    by gpt

```cmd
echo a.txt | start ""
```
fail

```cmd
cmd.exe /c start "" a.txt
```
success

```bash
cmd.exe /c start "" a.txt
```
fail

「只開啟 cmd 而不執行程式」，原因通常是 **路徑處理、引號、或 Git Bash 的 PATH 轉換問題**。
`MSYS2_ARG_CONV_EXCL="*"` 是一個 **環境變數**，用來告訴 **Git Bash / MSYS2** 「不要自動把 Windows 路徑轉成 Unix 路徑」。
    gpt

in bash:
cmd.exe /c start "" "C:\Users\xxx\Downloads\grokMarkdownFiles\a.txt" #fail
MSYS2_ARG_CONV_EXCL="*" cmd.exe /c start "" "C:\Users\xxx\Downloads\grokMarkdownFiles\a.txt" #success

$ MSYS2_ARG_CONV_EXCL="*" cmd.exe /c start "" $(pwd)/a.txt
無效的參數 - "/c".

```powershell
echo a.txt | ForEach-Object { start $_ }
```
success

```bash
echo "a.txt" | xargs -I {} cmd.exe /c start "" "{}"
```
fail

在 Git Bash 裡：

-   `start` 是 _bash built-in alias_，是 _shell command_，不是 _真正的 exe_
-   built-in command **不能吃 stdin（pipe）**

    gpt

## **用 fzf 選完檔案後，直接把結果複製到剪貼簿**
在 Git Bash / Windows 下，有幾種方法可以做到。

* * *

### 1️⃣ Git Bash 內建 clip 命令

Windows 有 `clip.exe`，可以把標準輸入內容複製到剪貼簿。

```bash
echo "a.txt" | clip
```

就會把 `"a.txt"` 放到 Windows 剪貼簿。

* * *

### 2️⃣ fzf 選檔後複製

```bash
fzf | clip
```

## fzf | clip 中文亂碼

ChatGPT:
你遇到的問題是 fzf 輸出中文到 Windows 剪貼簿 (`clip`) 出現亂碼。原因主要是 編碼不匹配，因為 Git Bash / fzf 默認是 UTF-8，而 `clip.exe` 在 Windows 預設接受 UTF-16 LE。

下面幾種方法可以解決：

---

### 方法 1：使用 `iconv` 轉換編碼

```bash
fzf | iconv -f UTF-8 -t UTF-16LE | clip
```

- `-f UTF-8`：原始編碼
- `-t UTF-16LE`：Windows 剪貼簿需要的編碼
- `clip`：把內容放到剪貼簿

---

### 方法 2：用 PowerShell 代替 `clip`

```bash
fzf | pwsh -Command "Set-Clipboard -Value ([Console]::In.ReadToEnd())"
```

- `pwsh` 或 `powershell` 都可以
- PowerShell 的剪貼簿支援 UTF-8，自動處理中文

---

### 方法 3：設定 Git Bash 的編碼

1. 確認 Git Bash 使用 UTF-8：

```bash
echo $LANG
```

應該是 `en_US.UTF-8` 或類似。
2\. 如果不是，可以在 `.bashrc` 加：

```bash
export LANG=en_US.UTF-8
```

---

💡 建議：最穩定的方法是 方法 1 或 2，直接處理編碼，避免亂碼。

如果你願意，我可以幫你寫一個 fzf 一鍵選中文檔名並複製到剪貼簿 的完整範例，支援中文。你希望我幫你做嗎？
