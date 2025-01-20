---
published: true
---
```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\git_gui]
@="Git &GUI Here"
"Icon"="C:\\tools\\Cmder\\vendor\\git-for-windows\\cmd\\git-gui.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\git_gui\command]
@="\"C:\\tools\\Cmder\\vendor\\git-for-windows\\cmd\\git-gui.exe\" \"--working-dir\" \"%v.\""
```

ref
add git gui to context menu
  https://stackoverflow.com/a/43596663