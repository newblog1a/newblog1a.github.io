---
published: true
---
## A New Post

```
bash::mintty
/icon "%ConEmuDir%\..\git-for-windows\usr\share\git\git-for-windows.ico"
-new_console:d:C:\Users\john "%ConEmuDir%\..\git-for-windows\usr\bin\mintty.exe" /bin/bash -l

bash::bash
/icon "%CMDER_ROOT%\icons\cmder.ico"
set "PATH=%ConEmuDir%\..\git-for-windows\usr\bin;%PATH%" & %ConEmuDir%\..\git-for-windows\git-cmd.exe --no-cd --command=%ConEmuBaseDirShort%\conemu-msys2-64.exe "%ConEmuDir%\..\git-for-windows\usr\bin\bash.exe" --login -i -new_console:d:C:\Users\john 
```
