---
layout: default
title: Jekyll で Latex
nav_order: 1
parent: jekyll
# has_children: true
has_toc: false
---

# Jekyll で **$$\LaTex$$** を使いたいときの導入方法

## Step 1.
`_layouts/head.html` に以下を記述する
``` 
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
```
## Step 2.
markdown に書く
```
$$E = mc^2$$
```
## 出力結果
$$E = mc^2$$

## 補足
jekyll では maxjax を使わずに KaTex を使った方がいいだの、_config.yml の設定が必要だのいろいろ歴史があったらしいが、現状（※2024年2月現在）は上記の mathjax 3 を使った方法がよさそう。

## 参考
+ [How to support latex in GitHub-pages?](https://stackoverflow.com/questions/26275645/how-to-support-latex-in-github-pages/72383929#72383929)
