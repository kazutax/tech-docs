---
layout: default
title: Jekyll で Latex
nav_order: 1
parent: jekyll
# has_children: true
---

# Jekyll で Latex を使いたいときの導入方法
## Step 1.
以下を `_layouts/post.html` に記述する
``` 
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```
## Step 2.
markdown に書く
```
$latex$
```
$latex$


## 補足
[この記事](https://tex2e.github.io/blog/latex/mathjax-to-katex) によると KaTex の方が早いらしいが、GitHub Pages では設定がめんどくさそうなので、やめた。
