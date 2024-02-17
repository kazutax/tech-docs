---
layout: default
title: Jekyll で 日本語検索
nav_order: 2
parent: jekyll
# has_children: true
has_toc: false
---

# Jekyll で日本語を検索に対応する

このサイトでも導入している [Just The Docs](https://github.com/just-the-docs/just-the-docs) のテーマにおいて、日本語検索ができなかったので対応する。

## Step 1.
`/assets/js/vendor/` 配下に 以下の js ファイルを設置する。<br>
ファイル自体は [lunr-languages (GitHub)](https://github.com/MihaiValentin/lunr-languages) からコピペで持ってくる。
```
lunr.stemmer.support.min.js
tinyseg.min.js
lunr.ja.min.js
```

## Step 2.
`_includes/head_custom.html` に以下を記述する
```
{% if site.search_enabled != false %}
   <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.min.js' | absolute_url }}"></script>
   <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.multi.min.js' | absolute_url }}"></script>
   <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.stemmer.support.min.js' | absolute_url }}"></script>
   <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.ru.min.js' | absolute_url }}"></script>
{% endif %}
```
```
{% if site.search_enabled != false %}
   <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.stemmer.support.min.js' | absolute_url }}"></script>
   <script type="text/javascript" src="{{ '/assets/js/vendor/tinyseg.min.js' | absolute_url }}"></script>
   <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.ja.min.js' | absolute_url }}"></script>
{% endif %}
```
## Step 3.
`assets/js/just-the-docs.js` にある `var index = lunr(function(){` とある行のすぐ下に次のスクリプトを記述する。
``` 
this.use(lunr.ja)
```

## 出力結果
できた。
![result image](/assets/images/js-search-results.jpg)


## 補足
複数言語対応として、[こちら](https://github.com/just-the-docs/just-the-docs/issues/59#issuecomment-1807080785) の方法があるものの、ロシア語など他の言語ではできるようだが、日本語だとうまくいかず、沼った。
```
this.use(lunr.multiLanguage('en', 'ru') );
```
どうやら js の既知問題らしい。

## 参考
+ [jekyll で検索を実装する方法- lunr の日本語対応](https://blog.tamesuu.com/2018/07/21/56/)
