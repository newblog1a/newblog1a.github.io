---
published: true
---
how to install PIL with pip?

Use [Pillow](https://pillow.readthedocs.io/) which is the "new" or the replacement of [PIL](http://www.pythonware.com/products/pil), but has the same-named modules to preserve compatibility:

    pip install pillow
    
detail:
```
λ uv run ./a.py
Traceback (most recent call last):
  File "C:\Users\john\Downloads\d3\a.py", line 5, in <module>
    from PIL import Image
ModuleNotFoundError: No module named 'PIL'
john@SKY-20241029KDE ~/Downloads/d3 (master)
λ uv add PIL
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of pil and your project depends on pil, we can conclude that your
      project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to
        skip locking and syncing.
john@SKY-20241029KDE ~/Downloads/d3 (master)
λ uv add pillow
Resolved 4 packages in 964ms
Prepared 1 package in 3.67s
Installed 1 package in 36ms
 + pillow==11.1.0
```

