---
layout: default
title: Jekyll で 日本語検索
description: "Jelyll で日本語検索する方法について"
nav_order: 2
parent: jekyll
# has_children: true
has_toc: false
---

# Jekyll で日本語検索に対応する

本サイトで導入している Jekyll テーマ [Just The Docs](https://github.com/just-the-docs/just-the-docs) で、日本語検索ができなかったので対応する。

## Step 1.
`/assets/js/vendor/` 配下に、以下の js ファイルを設置する。<br>
※ファイル自体は [lunr-languages (GitHub)](https://github.com/MihaiValentin/lunr-languages) からコピペで持ってくる。
```
lunr.stemmer.support.min.js
lunr.ja.min.js
tinyseg.js
```

## Step 2.
`_includes/head_custom.html` に以下を記述する。
```
{% raw %}{% if site.search_enabled != false %}
    <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.stemmer.support.min.js' | absolute_url }}"></script>
    <script type="text/javascript" src="{{ '/assets/js/vendor/tinyseg.js' | absolute_url }}"></script>
    <script type="text/javascript" src="{{ '/assets/js/vendor/lunr.ja.min.js' | absolute_url }}"></script>
{% endif %}{% endraw %}
```

{: .note}
`{% raw %}<script>{% endraw %}` タグの読み込み順序注意！

## Step 3.
`assets/js/just-the-docs.js` にある `var index = lunr(function(){` のすぐ下の行に `this.use(lunr.ja);` を追記する。<br>
以下の感じで：
``` 
{% raw %}var index = lunr(function(){
    
        this.use(lunr.ja); // ←これを追記
        
        this.ref('id');
        this.field('title', { boost: 200 });
        this.field('content', { boost: 2 });
        {%- if site.search.rel_url != false %}
        this.field('relUrl');{% endraw %}
```

## 出力結果
できた。
![result image](/tech-docs/assets/images/post-imgs/jekyll/ja-search-results.png)

## 補足
複数言語対応として、[こちら](https://github.com/just-the-docs/just-the-docs/issues/59#issuecomment-1807080785) の方法を試したが、日本語だとうまくいかず沼った。ロシア語など他の言語ではできるようだが、、
```
this.use(lunr.multiLanguage('en', 'ru') );
```
どうやら js の既知問題らしい。

## 参考
+ [jekyll で検索を実装する方法- lunr の日本語対応](https://blog.tamesuu.com/2018/07/21/56/)
