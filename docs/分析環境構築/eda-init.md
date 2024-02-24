---
layout: default
title: ローカル分析環境の量産
description: "M1 mac の環境で、個人的に現状一番使いやすい分析環境を構築する手順メモ。M1 Macbook + Pyenv + Poetry + Jupyter Notebook で構成。"
nav_order: 1
parent: 分析環境構築
has_children: false
---

# ローカル分析環境の量産

{: .highlight}
**前提条件**<br>
M1 mac の環境で、個人的に現状一番使いやすい分析環境を構築する手順メモ。<br>
**M1 Macbook** + **Pyenv** + **Poetry** + **Jupyter Notebook** で構成。

## ■ 概要

Python 環境構築にあたって，以下2種類のパッケージを使用する。

+ **Pyenv**: Python 仮想環境管理
+ **Poetry**: パッケージ管理

Anaconda は上記2つを1つにまとめたようなもので便利だったが、商用利用で有償となったので使うのやめた。
← 参考：[Sustaining our stewardship of the open-source data science community](https://www.anaconda.com/blog/sustaining-our-stewardship-of-the-open-source-data-science-community)<br>
また、Poetry 単体では Python のバージョンは管理できないようなので Pyenv を併用する。

Poetry と Pyenv の初回インストール後、日常使っていて新たに仮想環境立てる場面でいろいろ忘れてることが多いので、備忘のためにまとめる。

## ■ Poetry のインストール（初回のみ）

参考：[Poetry Docs](https://python-poetry.org/docs/)

以下ターミナルで実行してダウンロード

{% highlight zsh %}
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py \ß| python -
{% endhighlight %}

パスが通らないと思うので .zshrc ファイルを修正

{% highlight zsh %}
export PATH="$HOME/.poetry/bin"
{% endhighlight %}

バージョン確認ができればOK

{% highlight zsh %}
poetry --version
{% endhighlight %}

## ■ pyenv のインストール（初回のみ）

M1 Macbook システムに入っている標準の Python は ver.2.7（2021.11.27現在）

Python 環境は別途 M1 Mac に直接入れるか、仮想環境をたてて設定する（Python 3.x の bin ファイルをどこかに置いてパスを参照させる）必要がある。<br>
ネットでよく見かけるのは pyenv で環境を立ててその Python bin パスを Poetry に通すというやり方。（以下説明）

参考URL：https://github.com/pyenv/pyenv#basic-github-checkout 

以下ターミナルで実行してダウンロード。

{% highlight zsh %}
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
cd ~/.pyenv && src/configure && make -C src
{% endhighlight %}

パスが通らないと思うので .zprofile,  .zshrc ファイルを修正

{% highlight zsh %}
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zprofile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zprofile
echo 'eval "$(pyenv init --path)"' >> ~/.zprofile
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
{% endhighlight %}

ターミナルを再起動して以下を実行し、バージョンが確認できればOK

{% highlight zsh %}
pyenv --version
{% endhighlight %}

## ■ 任意の仮想環境を構築（量産手順）

`pyenv --version` と `poetry --version` を実行して、設定したい環境にそれぞれ疎通を確認。

### pyenv で必要な Python version をインストールする

任意の Python バージョンを利用するためにインストールする。実行できるバージョンを確認するには、

{% highlight zsh %}
pyenv install --list
{% endhighlight %}

で一覧を確認できる。その中から好きなバージョンを

{% highlight zsh %}
pyenv install 3.9.9
{% endhighlight %}

でダウンロード、インストールする。

環境にはインストールされたが M1 Mac がターミナルで `python` と打った時に反応するのはシステム側の Python bin となってる。<br>
`python` と打つことでシステム側の Python を、`python3` と打つことで今回インストールした 3.9.9 を使えるようにするには、本来的には以下のコマンドで実現できるはずなのだが、、、

{% highlight zsh %}
pyenv global system 3.9.9
{% endhighlight %}

残念ながら、**M1 Mac ではこれが反応しなそう** だったので、設定する Poetry の環境ごとに以下を実行して対応する。

{% highlight zsh %}
pyenv local 3.9.9
{% endhighlight %}

そもそも Pyenv では global は全体設定、local は特定のディレクトリ内でのバージョン設定となっており、設定していれば local が優先される。<br>
（この設定は次の **“Poetry env で Python version を変更する“** で初手として実行する。）

### Poetry env で Python version を変更する

（もしまだ作業ディレクトリが存在しなければ、）任意の場所に Poetry の Python 実行環境のディレクトリをおいて移動し、初手として `pyenv local x.x.x` を実行する。

※自分がよく使う設定は `$HOME/Poetry/` の配下にプロジェクトを配置 ex. `/env20211127`（日付別に名前分け）

{% highlight zsh %}
mkdir env20211127
cd env20211127
pyenv local 3.9.9
{% endhighlight %}

Pyenv 環境としては Python 3.9.9 が実行されるようになる。<br>
以下のコマンドを実行すると、Pyenv として持っている Python のバージョン一覧が表示され、「＊」がついた環境で実行できていることを確認できる。

{% highlight zsh %}
poetry init
poetry install
{% endhighlight %}

`poetry init` すると、対話式で質問が表示され、それらの質問に答えていくとプロジェクトの設定が完了し `pyproject.toml` という設定ファイルが作られる。

設定が完了したら、`poetry install` を行う。

{% highlight zsh %}
pyenv versions
{% endhighlight %}

次を実行して Poetry のバージョンを変更し、状態を確認する。

{% highlight zsh %}
poetry env use 3.9.9
poetry env info
{% endhighlight %}

Python のバージョンが切り替わる。<br>
もし、pyenv でとってきてないバージョンを指定しようとするとエラーとなる。同一のバージョンが複数ある場合にはフルパスで Python を指定すれば良い。

{% highlight zsh %}
poetry env use /fullpath/to/the/python/
{% endhighlight %}

※ pyenv の python の場所はどこ？
→ /Users/kazutak/.pyenv/versions/3.9.9/bin/python3.9 とかにあるよ

※ 自分コピペ用
{% highlight zsh %}
poetry env use /Users/kazutak/.pyenv/versions/3.9.9/bin/python3.9
{% endhighlight %}

### Jupyter Notebook で poetry 環境の Jupyter kernel を参照させる

分析環境に Jupyter Notebook をインストールする。

{% highlight zsh %}
poetry add jupyter notebook
{% endhighlight %}

参考：[https://gadgetlunatic.com/post/use-poetry-along-with-jupyter/](https://gadgetlunatic.com/post/use-poetry-along-with-jupyter/)

このまま、`poetry run jupyter notebook` のコマンドから Jupyter Notebook を起動して、その内で実行したコードに Poetry でインストールしたモジュールをインポートしようとすると、`ModuleNotFoundError` を吐く。<br>
原因は、システムにインストールされている Jupyter が使っている Python kernel からは Poetry プロジェクトの venv が参照できないため。

これを解決するために、Jupyter に Poetry が生成した venv のカーネルを追加する。<br>
はじめに ipykernel モジュールをプロジェクトに追加する。

{% highlight zsh %}
poetry add --dev ipykernel
{% endhighlight %}

`poetry shell` などで仮想環境に入ったのちに、次のコマンドを実行することで pyenv のカーネルを Jupyter に追加することができる。

{% highlight zsh %}
ipython kernel install --user --name=your-project-name
{% endhighlight %}

Jupyter 上でさきほど追加したカーネルを指定して実行することで、問題なくインポートを含むコードを実行することができる。Jupyter で使えるカーネルの一覧の確認方法は以下：

{% highlight zsh %}
jupyter kernelspec list
{% endhighlight %}

追加したカーネルの削除方法は以下：

{% highlight zsh %}
jupyter kernelspec remove your-kernel-name
{% endhighlight %}

## ■ 上記環境設定時の事件簿

### 【事件その1】Poery 管理のライブラリを kernel が参照しない問題（解決済）

普通に以下で Notebook を起動

{% highlight zsh %}
poetry run jupyter notebook
{% endhighlight %}

順調に起動し、設定済みの kernel で Notebook を利用可能、、と思ったらライブラリの読み込みで `poetry add` したはずのライブラリ読み込みエラーなった。。

![lib-error](/tech-docs/assets/images/post-imgs/env/lib-error.png)
<br>
`sys.version`、 `sys.executable` を確認してみると、pyenv で設定している python 環境に kernel が接続されていないことがある。

{% highlight python %}
import sys
print(sys.executable)
print(sys.version)
{% endhighlight %}

実行すると、意図しない python 環境（system）を参照してしまっているはず。

※ `poetry env info` で設定している仮想環境の python は期待挙動
<br>
![poetry-env-info](/tech-docs/assets/images/post-imgs/env/poetry-env-info.png)

{: .highlight}
**【解決策】**2021/12/31<br>
`poetry shell` で仮想環境をアクティブ化してから kernel を起動しないとダメ。

### 【事件その2】scipy・sklearn が入らない問題（解決済）

なぜか `scipy` が入らない。。もともと `scikit-learn` を入れて使いたかったが、依存関係を制御する.tomlファイルをいじっても解決せず。<br>
仕方ないから `miniforge` 環境を現時点の標準とする。（M1 じゃない Macbook Pro には同じ操作で問題なくインストールできた。偉い人が解決するまでは M1 では `miniforge` ユーザーとして頑張る）

2021/10/24 → `pyenv` のバージョンうまくいじると入ったりするかも？再現確認までできてない

{: .highlight}
**【解決策】**2023/4/22<br>
`pyproject.toml` で `python = ">=3.9, <3.13"` に指定して `poetry update` したら `poetry add scipy` が問題なく実行できた！

## ■ 参考URL：
+ [https://qiita.com/thesugar/items/1cd91eb3f5de7d390490](https://qiita.com/thesugar/items/1cd91eb3f5de7d390490)
+ [https://blog.mktia.com/pyenv-and-poetry-on-mac-m1/](https://blog.mktia.com/pyenv-and-poetry-on-mac-m1/)

