---
layout: default
title: Jekyll で 日本語検索
nav_order: 2
parent: jekyll
# has_children: true
has_toc: false
---

# Jekyll で **日本語検索** を使いたいときの導入方法

このサイトでも導入している [Just The Docs](https://github.com/just-the-docs/just-the-docs) のテーマにおいて、日本語検索ができなかったので対応する。

## Step 1.
ライブラリに 以下を設置する。
ファイル自体は []() ここからコピペで持ってくる。

## Step 2.
`_layouts/head.html` に以下を記述する
``` 
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
```

## 補足
このやり方ではうまくいかないってのは、js の既知問題らしい。

## 参考
+ []()
