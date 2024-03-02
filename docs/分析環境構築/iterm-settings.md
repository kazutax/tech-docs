---
layout: default
title: iTerm 環境設定
description: "iTerm で個人的に好きな環境設定についてまとめる"
nav_order: 3
parent: 分析環境構築
has_children: false
---
# iTerm 環境設定
{: .no_toc }

iTerm で個人的に好きな環境設定についてまとめる。

## Table of contents
{: .no_toc .text-delta }
- TOC
{:toc}

## Font and Theme
気に入ってるフォントとテーマを設定する手順

{: .note-title }
**参考:**<br>
・[oh my zsh　導入手順メモ(Mac)](https://qiita.com/NaokiIshimura/items/249bb1a101b626a59387)

### Powerline font のインストール
{% highlight zsh %}
git clone https://github.com/powerline/fonts
cd fonts
./install.sh
{% endhighlight %}

### cobalt2 theme のインストール

{% highlight zsh %}
git clone https://github.com/wesbos/Cobalt2-iterm.git
cd Cobalt2-iterm
cp cobalt2.zsh-theme ~/.oh-my-zsh/themes/
{% endhighlight %}

### zsh theme の変更

{% highlight zsh %}
$ vi ~/.zshrc

ZSH_THEME="cobalt2" 
# デフォルト値は「robbyrussell」
{% endhighlight %}

### UI からテーマ変更

1. iTerm2 > Preferences > Profiles > Colors
1. Change Font ボタンをクリックする
1. Color Presets.. プルダウンで import.. を選択する
1. `cobalt2.zsh-theme` を選択する
1. Color Presets.. プルダウンで Cobalt2 を選択する


## oh my zsh だと git branch でファイル vi で開いちゃう問題

`git branch` のページ出力をデフォルトで off に戻すには、`pager.branch` の設定を変更すればOK

{: .note-title }
**参考:**<br>
・[Git branch command behaves like 'less'](https://stackoverflow.com/questions/48341920/git-branch-command-behaves-like-less)

{% highlight zsh %}
git config --global pager.branch false
{% endhighlight %}

