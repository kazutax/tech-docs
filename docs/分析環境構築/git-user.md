---
layout: default
title: Git ユーザー登録
description: "ローカル環境に Git のインストールが完了したらユーザー名とメールアドレスを登録する"
nav_order: 2
parent: 分析環境構築
has_children: false
---

# Git ユーザー登録（ローカル）

git コマンドのインストールが完了したら、ターミナルを開いて、ユーザー名とメールアドレスを登録する。
※ 名前は任意に置き換えて

{% highlight zsh %}
$ git config --global user.name "kazuki.takahashi"
$ git config --global user.email "kazuki.takahashi@mail.com"
{% endhighlight %}

きちんと登録できたか確認する。

{% highlight zsh %}
$ git config --list
user.name=kazuki.takahashi
user.email=kazuki.takahashi@mail.com
{% endhighlight %}

ユーザー情報（名前とメールアドレス）の登録先は、物理的には　`~/.gitconfig` のなか。ターミナルで確認する。

{% highlight zsh %}
$ cat ~/.gitconfig
[user]
    nama = kazuki.takahashi
    email = kazuki.takahashi@mail.com
{% endhighlight %}

# GitHub に SSH接続

## ■ SSH 鍵の作成

Git では、ネットワークのプロトコルとしてHTTPSとSSH（Secure Shell）を利用することができる。HTTPS と違い操作の際にパスワードを聞かれなくて済むので、SSH で GitHub のリポジトリにアクセスするのが楽になる。

Git での作業を楽にするために、SSH 鍵を作成し、公開鍵を GitHub に登録する。<br>
SSH は秘密鍵と公開鍵によって認証を行うものであり、公開鍵をサーバーに置いて、ローカルの秘密鍵で認証する。

{: .warning}
秘密鍵は誰にも見せないように！

### Step 1. 既存鍵の確認
ターミナルを開き、鍵を作成する前に、念のため、上書きしないようにすでに鍵があるか確認！

{% highlight zsh %}
$ ls ~/.ssh
{% endhighlight %}

ここで秘密鍵の `id_rsa` や公開鍵の `id_rsa.pub` が存在する場合、すでに鍵を作成しているためスキップして鍵の登録へ進む。（名前をつけて鍵を作成している場合、別名の可能性もある。）

### Step 2. 鍵を作成

{% highlight zsh %}
$ ssh-keygen -C "kazuki.takahashi@mail.com"
{% endhighlight %}

（この後いくつか質問されるが、特にこだわりがなければ Enter）

パスフレーズ（PASSPHRASE）を入力し Enter。<br>
`id_rsa` や `id_rsa.pub` が作成されていれば、鍵の作成は成功。

{% highlight zsh %}
$ ls ~/.ssh
id_rsa          id_rsa.pub
{% endhighlight %}

### Step 3. 鍵を登録

GitHub の Settings から、SSH keys へ移動して、生成した公開鍵 `is_rsa.pub` をコピー＆ペースト。

Mac なら `pbcopy` が使えるので、そのままペースト。

{% highlight zsh %}
$ pbcopy < ~/.ssh/id_rsa.pub
{% endhighlight %}

これで完了。
