---
published: true
---
in package.json:
```json
  "scripts": {
    "bloggerconvert": "./index.js",
    "pretest": "npm run bloggerconvert ./test-data/test-blog-with-simple-default-theme.xml",
    "test": "./smoke-test.js"
  },
```

show error:
```cmd
λ pnpm run test

> blogger-archive-converter@0.1.0 pretest C:\Users\john\a\blogger-archive-converter-master
> npm run bloggerconvert ./test-data/test-blog-with-simple-default-theme.xml


> blogger-archive-converter@0.1.0 bloggerconvert
> ./index.js ./test-data/test-blog-with-simple-default-theme.xml

'.' 不是內部或外部命令、可執行的程式或批次檔。
 ELIFECYCLE  Command failed with exit code 1.
```

NPM package 'bin' script for Windows
  https://stackoverflow.com/questions/10396305/npm-package-bin-script-for-windows
