---
layout: default
title: Jekyll で Latex
nav_order: 1
parent: jekyll
# has_children: true
has_toc: false
---

# Jekyll で Latex を使いたいときの導入方法

## Step 1.
`_layouts/head.html` に以下を記述する
``` 
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
```
## Step 2.
markdown に書く
```
\begin{aligned}
E = mc^2
\end{aligned}
```
\begin{aligned}
E = mc^2
\end{aligned}

$`a^2 + b^2 = c^2`$
$$E = mc^2$$

## 参考
+ [How to support latex in GitHub-pages?](https://stackoverflow.com/questions/26275645/how-to-support-latex-in-github-pages/72383929#72383929)