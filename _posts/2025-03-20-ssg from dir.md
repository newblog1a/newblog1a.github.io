---
published: true
---
這個 Python 程式會：

1.  掃描 `pic` 資料夾，找出所有子資料夾（分類）。
2.  為每個分類建立一個區塊，顯示該分類內的所有 `.jpg` 圖片。
3.  產生 `index.html`，可以直接開啟瀏覽。

你可以放入 `pic` 資料夾，執行這段程式後，在瀏覽器打開 `index.html` 測試看看！

---

沒考慮到 圖檔在多層資料夾內

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