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
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/katex.min.css" integrity="sha384-yFRtMMDnQtDRO8rLpMIKrtPCD5jdktao2TV19YiZYWMDkUR5GQZR/NOVTdquEx1j" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/katex.min.js" integrity="sha384-9Nhn55MVVN0/4OFx7EE5kpFBPsEMZxKTCnA+4fqDmg12eCTqGi6+BB2LjY8brQxJ" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous" onload="renderMathInElement(document.body);"></script>

```
## Step 2.
`_config.yml`に以下を記述する
```
kramdown:
    math_engine: katex
```
## Step 3.
markdown に書く
```
$\LaTex code$
```
$\LaTex code$
$$\LaTex code$$


## 補足
[この記事](https://tex2e.github.io/blog/latex/mathjax-to-katex) によると KaTex の方が早いらしいが、GitHub Pages では設定がめんどくさそうなので、やめた。
