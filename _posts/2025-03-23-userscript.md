---
published: true
---
### 右键菜单

之前在说[run-at](https://bbs.tampermonkey.net.cn/%5Bhttps%3A//www.tampermonkey.net/documentation.php#_run_at%5D(https://www.tampermonkey.net/documentation.php%23_run_at))的时候,有一个属性,不知道大家有没有注意,当时也没有详细讲解.除了 `document-start/body/end/idle`这些控制脚本运行时间的外,还有一个 `context-menu`的属性,用于右键点击菜单的时候执行脚本.如果你使用了 `context-menu`属性,那么当你在所匹配的页面上右键时,菜单中就会以你脚本名显示出一菜单项,点击之后就是执行你的脚本代码.

```js
// ==UserScript==
// @name         右键菜单和valude demo
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://bbs.tampermonkey.net.cn/*
// @run-at       context-menu
// ==/UserScript==

alert("菜单被点击了");
```

#### 自定义的菜单

除了使用 `@run-at context-menu`生成菜单外,油猴还提供了两个函数[GM\_registerMenuCommand](https://bbs.tampermonkey.net.cn/%5Bhttps%3A//www.tampermonkey.net/documentation.php#GM_registerMenuCommand%5D(https://www.tampermonkey.net/documentation.php%23GM_registerMenuCommand))和[GM\_unregisterMenuCommand](https://bbs.tampermonkey.net.cn/%5Bhttps%3A//www.tampermonkey.net/documentation.php#GM_unregisterMenuCommand%5D(https://www.tampermonkey.net/documentation.php%23GM_unregisterMenuCommand)),分别用于注册菜单和删除菜单.更加灵活,但是不能显示在页面上,需要到油猴图标脚本那里去进行点击.

```js
// ==UserScript==
// @name         右键菜单和valude demo
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://bbs.tampermonkey.net.cn/*
// @grant    GM_registerMenuCommand
// @grant    GM_unregisterMenuCommand
// ==/UserScript==

let id=GM_registerMenuCommand ("自定义的菜单", function(){
   alert('菜单点击');
   GM_unregisterMenuCommand(id);//删除菜单
}, "h");
// 第三个参数 accessKey 为快捷键，输入h即可触发。本脚本在点击一次之后会将菜单删除。
```

https://bbs.tampermonkey.net.cn/thread-271-1-1.html

---

run some script when we click it.
  https://github.com/Tampermonkey/tampermonkey/issues/613
  
Please check:

// @run\-at context-menu

[https://tampermonkey.net/documentation.php#\_run\_at](https://tampermonkey.net/documentation.php#_run_at)

and

GM\_registerMenuCommand('Run my Script', function() {
   alert('it runs!');
})

[https://tampermonkey.net/documentation.php#GM\_registerMenuCommand](https://tampermonkey.net/documentation.php#GM_registerMenuCommand)
