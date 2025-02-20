---
published: true
---
in build:
```
Download immutable action package 'actions/checkout@v3'
Download immutable action package 'actions/configure-pages@v3'
Download action repository 'ruby/setup-ruby@v1' (SHA:32110d4e311bd8996b2a82bf2a43b714ccc91777)
Download immutable action package 'actions/upload-pages-artifact@v1'
Getting action download info
Error: This request has been automatically failed because it uses a deprecated version of `actions/upload-artifact: v3`. Learn more: https://github.blog/changelog/2024-04-16-deprecation-notice-v3-of-the-artifact-actions/
```

reason:
Deprecation notice: v3 of the artifact actions
  https://github.blog/changelog/2024-04-16-deprecation-notice-v3-of-the-artifact-actions/
  
solve:
In .github/workflows/pages-deploy.yml
change to:
```
      - name: Upload site artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "_site${{ steps.pages.outputs.base_path }}"
```

new error in deploy:
```
Run actions/deploy-pages@v1
Artifact exchange URL: https://pipelinesghubeus15.actions.githubusercontent.com/SojonkfAnhC1eXvHQzHjrEdLHhhk5CZJjOgyUvmrdYQwddTeOh/_apis/pipelines/workflows/13443002252/artifacts?api-version=6.0-preview
Error: Error: No uploaded artifact was found! Please check if there are any errors at build step, or uploaded artifact name is correct.
```

solve:
copy pages-deploy.yml from
https://github.com/cotes2020/chirpy-starter/blob/main/.github/workflows/pages-deploy.yml

rew error in rest site:
```
Run bundle exec htmlproofer _site \
  bundle exec htmlproofer _site \
    \-\-disable-external \
    \-\-ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"
  shell: /usr/bin/bash -e {0}
  env:
    GITHUB_PAGES: true
htmlproofer 3.19.4 | Error:  Whoops, we can't understand your command.
htmlproofer 3.19.4 | Error:  invalid option: --ignore-urls
```

solve:
use old version in this part