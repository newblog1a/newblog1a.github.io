---
published: true
---
## v1

這個 Python 程式會：

1.  掃描 `pic` 資料夾，找出所有子資料夾（分類）。
2.  為每個分類建立一個區塊，顯示該分類內的所有 `.jpg` 圖片。
3.  產生 `index.html`，可以直接開啟瀏覽。

你可以放入 `pic` 資料夾，執行這段程式後，在瀏覽器打開 `index.html` 測試看看！

---

沒考慮到 圖檔在多層資料夾內。
全部圖檔都顯示在首頁。

首頁顯示:
```
資料夾名1
圖1 圖2...
資料夾名2
圖1 圖2...
```

```py
import os
from pathlib import Path

template = """<!DOCTYPE html>
<html lang='zh-TW'>
<head>
    <meta charset='UTF-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1.0'>
    <title>作品集</title>
    <style>
        body {{ font-family: Arial, sans-serif; }}
        .gallery {{ display: flex; flex-wrap: wrap; gap: 10px; }}
        .category {{ margin-bottom: 20px; }}
        img {{ max-width: 200px; height: auto; }}
    </style>
</head>
<body>
    <h1>作品集</h1>
    {content}
</body>
</html>"""

def generate_gallery(pic_folder):
    categories = sorted([d for d in os.listdir(pic_folder) if (Path(pic_folder) / d).is_dir()])
    content = ""
    
    for category in categories:
        content += f"<div class='category'><h2>{category}</h2><div class='gallery'>"
        images = sorted((Path(pic_folder) / category).glob("*.jpg"))
        for img in images:
            img_path = f"pic/{category}/{img.name}"
            content += f"<a href='{img_path}' target='_blank'><img src='{img_path}'></a>"
        content += "</div></div>"
    
    with open("index.html", "w", encoding="utf-8") as f:
        f.write(template.format(content=content))

# 指定圖片資料夾
generate_gallery("pic")
```

---

## v2

有考慮多層資料夾。
首頁每個資料夾只顯示一張圖。

首頁顯示:
```
資料夾1_圖1 資料夾2_圖1...
```

點擊圖後，進入該資料夾圖檔合集。

```py
import os
import shutil
from pathlib import Path
from jinja2 import Environment, FileSystemLoader
from PIL import Image

def create_thumbnail(image_path, thumb_path, size=(300, 300)):
    with Image.open(image_path) as img:
        img.thumbnail(size)
        img.save(thumb_path)

def scan_images(base_dir):
    image_data = {}
    for root, _, files in os.walk(base_dir):
        rel_path = os.path.relpath(root, base_dir)
        category = rel_path.replace(os.sep, '/')
        if category == '.':
            category = 'uncategorized'
        image_files = [f for f in files if f.lower().endswith(('jpg', 'jpeg', 'png'))]
        if image_files:
            image_data[category] = [os.path.join(rel_path, f) for f in image_files]
    return image_data

def generate_html(image_data, output_dir):
    env = Environment(loader=FileSystemLoader('templates'))
    os.makedirs(output_dir, exist_ok=True)
    
    # Generate index.html
    template = env.get_template('index.html')
    with open(os.path.join(output_dir, 'index.html'), 'w', encoding='utf-8') as f:
        f.write(template.render(categories=image_data))
        print("Writing index.html to templates/")
    
    # Generate category pages
    category_template = env.get_template('category.html')
    for category, images in image_data.items():
        category_path = os.path.join(output_dir, category.replace('/', '_') + '.html')
        with open(category_path, 'w', encoding='utf-8') as f:
            f.write(category_template.render(category=category, images=images))

def copy_images(image_data, src_dir, dst_dir):
    os.makedirs(dst_dir, exist_ok=True)
    for category, images in image_data.items():
        for img in images:
            src_path = os.path.join(src_dir, img)
            dst_path = os.path.join(dst_dir, img)
            os.makedirs(os.path.dirname(dst_path), exist_ok=True)
            shutil.copy2(src_path, dst_path)
            # Create thumbnail
            thumb_path = os.path.join(dst_dir, 'thumbs', img)
            os.makedirs(os.path.dirname(thumb_path), exist_ok=True)
            create_thumbnail(src_path, thumb_path)

def main():
    source_dir = 'pic'
    output_dir = 'site'
    static_img_dir = os.path.join(output_dir, 'images')
    os.makedirs('templates', exist_ok=True)
    
    image_data = scan_images(source_dir)
    copy_images(image_data, source_dir, static_img_dir)
    generate_html(image_data, output_dir)
    print("Site generated successfully!")



# Templates (Jinja2)
templates_dir = 'templates'
os.makedirs(templates_dir, exist_ok=True)

index_html = """
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>作品集</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        .gallery { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 10px; }
        .gallery img { width: 100%; height: auto; border-radius: 5px; transition: transform 0.2s; }
        .gallery img:hover { transform: scale(1.05); }
        .category { margin-top: 20px; }
    </style>
</head>
<body>
    <h1>作品集</h1>
    <div class="gallery">
        {% for category, images in categories.items() %}
        <a href="{{ category.replace('/', '_') }}.html">
            <img src="images/thumbs/{{ images[0] }}" alt="{{ category }}">
        </a>
        {% endfor %}
    </div>
</body>
</html>
"""

category_html = """
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ category }}</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        .gallery { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 10px; }
        .gallery img { width: 100%; height: auto; border-radius: 5px; transition: transform 0.2s; }
        .gallery img:hover { transform: scale(1.05); }
    </style>
</head>
<body>
    <h1>{{ category }}</h1>
    <div class="gallery">
        {% for img in images %}
        <img src="images/{{ img }}" alt="{{ img }}">
        {% endfor %}
    </div>
    <a href="index.html">回首頁</a>
</body>
</html>
"""

with open(os.path.join(templates_dir, 'index.html'), 'w', encoding='utf-8') as f:
    f.write(index_html)

with open(os.path.join(templates_dir, 'category.html'), 'w', encoding='utf-8') as f:
    f.write(category_html)

if __name__ == "__main__":
    main()
```

## v3

v3基礎上，顯示資料夾名字 和 圖檔名字。

```py
import os
import shutil
from pathlib import Path
from jinja2 import Environment, FileSystemLoader
from PIL import Image

def create_thumbnail(image_path, thumb_path, size=(300, 300)):
    with Image.open(image_path) as img:
        img.thumbnail(size)
        img.save(thumb_path)

def scan_images(base_dir):
    image_data = {}
    for root, _, files in os.walk(base_dir):
        rel_path = os.path.relpath(root, base_dir)
        category = rel_path.replace(os.sep, '/')
        if category == '.':
            category = '未分類'
        image_files = [f for f in files if f.lower().endswith(('jpg', 'jpeg', 'png'))]
        if image_files:
            image_data[category] = [(f, os.path.join(rel_path, f)) for f in image_files]
    return image_data

def generate_html(image_data, output_dir):
    env = Environment(loader=FileSystemLoader(os.path.abspath('templates')))
    os.makedirs(output_dir, exist_ok=True)
    
    # Generate index.html
    template = env.get_template('index.html')
    with open(os.path.join(output_dir, 'index.html'), 'w', encoding='utf-8') as f:
        f.write(template.render(categories=image_data))
    
    # Generate category pages
    category_template = env.get_template('category.html')
    for category, images in image_data.items():
        category_path = os.path.join(output_dir, category.replace('/', '_') + '.html')
        with open(category_path, 'w', encoding='utf-8') as f:
            f.write(category_template.render(category=category, images=images))

def copy_images(image_data, src_dir, dst_dir):
    os.makedirs(dst_dir, exist_ok=True)
    for category, images in image_data.items():
        for img_name, img_path in images:
            src_path = os.path.join(src_dir, img_path)
            dst_path = os.path.join(dst_dir, img_path)
            os.makedirs(os.path.dirname(dst_path), exist_ok=True)
            shutil.copy2(src_path, dst_path)
            # Create thumbnail
            thumb_path = os.path.join(dst_dir, 'thumbs', img_path)
            os.makedirs(os.path.dirname(thumb_path), exist_ok=True)
            create_thumbnail(src_path, thumb_path)

def main():
    os.makedirs('templates', exist_ok=True)
    source_dir = 'pic'
    output_dir = 'site'
    static_img_dir = os.path.join(output_dir, 'images')
    
    image_data = scan_images(source_dir)
    copy_images(image_data, source_dir, static_img_dir)
    generate_html(image_data, output_dir)
    print("Site generated successfully!")

# Templates (Jinja2)
templates_dir = 'templates'
os.makedirs(templates_dir, exist_ok=True)

index_html = """
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>作品集</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        .gallery { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 10px; }
        .gallery img { width: 100%; height: auto; border-radius: 5px; transition: transform 0.2s; }
        .gallery img:hover { transform: scale(1.05); }
        .category { margin-top: 20px; }
    </style>
</head>
<body>
    <h1>作品集</h1>
    <div class="gallery">
        {% for category, images in categories.items() %}
        <div class="category">
            <h2><a href="{{ category.replace('/', '_') }}.html">{{ category }}</a></h2>
            <a href="{{ category.replace('/', '_') }}.html">
                <img src="images/thumbs/{{ images[0][1] }}" alt="{{ category }}">
            </a>
        </div>
        {% endfor %}
    </div>
</body>
</html>
"""

category_html = """
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ category }}</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        .gallery { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 10px; }
        .gallery img { width: 100%; height: auto; border-radius: 5px; transition: transform 0.2s; }
        .gallery img:hover { transform: scale(1.05); }
    </style>
</head>
<body>
    <h1>{{ category }}</h1>
    <div class="gallery">
        {% for img_name, img_path in images %}
        <div>
            <img src="images/{{ img_path }}" alt="{{ img_name }}">
            <p>{{ img_name }}</p>
        </div>
        {% endfor %}
    </div>
    <a href="index.html">回首頁</a>
</body>
</html>
"""

with open(os.path.join(templates_dir, 'index.html'), 'w', encoding='utf-8') as f:
    f.write(index_html)

with open(os.path.join(templates_dir, 'category.html'), 'w', encoding='utf-8') as f:
    f.write(category_html)

if __name__ == "__main__":
    main()
```