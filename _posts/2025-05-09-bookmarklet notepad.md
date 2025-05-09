---
published: true
---
## A New Post


## 1
```js
javascript:!function()%7Blet e%3Dwindow.open("about%3Ablank"%2C"_blank")%3Be.document.write("Enter your note here.")%2Ce.document.title%3D"Quick Note"%2Ce.document.documentElement.setAttribute("contenteditable"%2C"")%2Ce.document.close()%7D()%3B
```

can't save

## 2
```html
data:text/html, <body contenteditable>
```

ok

## 3
```html
data:text/html, <body style='padding:0;margin:0;overflow:hidden;width:100%;height:100%;' contenteditable><textarea style="width:100%;position:absolute;top:0;left:0;right:0;bottom:0;padding:1em 10%;box-sizing:border-box;font-size:24px;font-family:monospace"></textarea><script>document.querySelector('textarea').focus()</script>
```

saved files is empty