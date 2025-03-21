---
published: true
---
chatgpt
imagemin本身是一個影像壓縮的「框架」，但不包含實際的壓縮功能。要使用 imagemin 來壓縮特定格式的圖片，必須額外安裝相應的插件

sharp與imagemin針對不同圖片的優化能力不一，沒有一定哪個比較好。速度上也無明顯的差別差別。
  https://tiffrrr.medium.com/webpack-webpack-imagemin%E5%A3%93%E7%B8%AE%E5%9C%96%E7%89%87-768928718807
  
plugin的選用與測試：
在npm上可以查到能用的plugin有非常多，例如。

無損壓縮：
JPEG: imagemin-jpegtran
PNG：imagemin-optipng

有損壓縮：
JPEG: imagemin-mozjpeg
PNG: imagemin-pngquant

imagemin，它是一款可以集成多个压缩库的工具，支持jpg，png，webp等等格式的图片压缩，比如pngquant，mozjpeg等等

Sharp vs Imagemin for Image Minification in Node.js
  https://blockqueue.io/blog/2024-09-22-sharp-vs-imagemin-comparison
  3mb.jpg Time Taken (Sharp) 6.38s;	Imagemin 9.31s

sharp
Resizing an image is typically 4x-5x faster than using the quickest ImageMagick and GraphicsMagick settings due to its use of libvips.
  https://www.npmjs.com/package/sharp
  https://www.npmjs.com/package/sharp-cli

---

# CLI

![2025-03-22 04_57_43-Github Compare - Brave.jpg]({{site.baseurl}}/img/2025-03-22 04_57_43-Github Compare - Brave.jpg)

https://www.githubcompare.com/tjko/jpegoptim+kud/jpegrescan+mozilla/mozjpeg

jpegoptim vs jpegtran vs mozjpeg
  https://stackoverflow.com/questions/36046782/jpegoptim-vs-jpegtran-vs-mozjpeg

```
# [python脚本使用mozjpeg批量压缩图片](https://www.cnblogs.com/ZerlinM/p/18615026 "发布于 2024-12-18 15:12")

安装mozjpeg

    npm install mozjpeg -g
```

## Windows
https://github.com/XhmikosR/jpegoptim-windows
2023

https://mozjpeg.codelove.de/binaries.html
2022