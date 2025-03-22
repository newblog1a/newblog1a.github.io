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

## CLI

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

scoop install main/jpegoptim

Caesium Image Compressor
  https://saerasoft.com/caesium
  https://github.com/Lymphatus/caesium-clt
npm ...
pngquant is a command-line utility and a library for lossy compression of PNG images.
  https://pngquant.org/

## mozjpeg

jpegtran optimize without changing filename
  jpegtran -copy none -optimize -outfile image.jpg image.jpg
  https://stackoverflow.com/questions/5579183/jpegtran-optimize-without-changing-filename
  
Q: Same output and input
No, cjpeg doesn't support that.
  https://github.com/mozilla/mozjpeg/issues/248
  
Q: Using `jpegtran` and `mozjpeg` together give some confusing
There is no `mozjpeg` command. This project ships `cjpeg` and `jpegtran`. Are you using a 3rd party tool that wraps them or renames them?
  https://github.com/mozilla/mozjpeg/issues/229
  
mozjpeg/usage.txt
  https://github.com/mozilla/mozjpeg/blob/master/usage.txt
  
$ cjpeg -quality 80 foo.bmp > bar.jpg
  https://hacks.mozilla.org/2014/08/using-mozjpeg-to-create-efficient-jpegs/
  
## webp

original files: 153MB

```py
subprocess.run(["jpegoptim", "--strip-all", "--max=85", file_path])
```
63.6MB


```py
subprocess.run(["jpegtran", "-optimize", "-progressive", "-copy", "none", "-outfile", file_path, file_path])
```
install mozjpeg from scoop
file size: 66.6MB


```py
from PIL import Image               # import the PIL.Image module
img = Image.open("image.webp")      # open your image
img.save("image2.webp", quality=70) # save the image with the given quality
```
37.7MB
  https://wenchen1997.com/blog/python/images-to-webp
  https://wenchen1997.com/blog/webp-test
  https://stackoverflow.com/questions/63225613/is-there-a-way-to-set-image-quality-for-webp-images-in-python
