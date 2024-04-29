---
layout: default
title: Welchのt検定
description: "Welchのt検定は、2つの独立した母集団からの標本に基づいて、それらの母平均が等しいかどうかを検定するために使用される。特に、Welch の検定は2つの母集団が「異なる分散」を持つ場合に有用。従来の独立二標本t検定では2つの母集団が等しい分散を持つという仮定が必要"
nav_order: 4 
parent: 統計学（頻度論）
has_children: false
has_toc: false
# permalink: /index-stats
---

# Welchのt検定

Welchのt検定は、2つの独立した母集団からの標本に基づいて、それらの母平均が等しいかどうかを検定するために使用される。
特に、Welch の検定は2つの母集団が **異なる分散** を持つ場合（分散の等質性が仮定できない場合）に有用。

{: .highlight}
従来の **独立二標本t検定** では2つの母集団が **等しい分散** を持つという仮定が必要


## ■ どんな場面で使えるか

A/Bテストを例にすると、以下のような場面が **異なる分散** が起こりうる状況といえる：

> **例1. 異なるトラフィックボリューム**
+ A/B テストで、異なる広告戦略やマーケティング施策を用いた場合、**訪問者の数** がグループ間で大きく異なることがある。
+ 大きなトラフィックの差は、データの分散に影響を与える可能性があり、この場合、Welch の t 検定が適切となる。

> **例2. 非均一なユーザー行動**
+ 特定の **ユーザーセグメント**（例えば、新規ユーザー対リピーター）に対する施策をテストする場合、これらのグループが異なる行動パターンや反応を示す可能性がある。
+ グループ間で行動の Variance が異なる場合、Welch の検定を使用することで、これらの差異をより正確に評価可能となる。

> **例3. 異なるデバイスタイプ**
+ 例えば「PC ユーザー」と「スマホユーザー」の間で A/B テストを行う場合、これらのデバイスタイプは **使用状況やユーザーの行動** が大きく異なる可能性がある。
+ デバイスタイプによる行動の違いが分散に影響を与える場合、Welch の t 検定が適切。

## ■ 利点と注意点

### **利点**
+ <span style="color: blue; ">**分散の不等質性に対する堅牢性:**</span><br>
**異なる分散を持つグループ間でも有効**。通常のt検定（独立二標本t検定）では、二つのグループが等分散であるという仮定に基づいているが、この仮定が破れると、エラーリスクが高まる。
+ <span style="color: blue; ">**一般的な適用性:**</span><br>
**分散が等しい場合でも**、Welchのt検定を用いることによるクリティカルな統計的デメリットは以下の注意点を除き特にない。

### **注意点**
+ <span style="color: red; ">**自由度の減少:**</span><br>
Welchの検定では、通常のt検定と比較して **自由度が低く** なることがある。<br>
特にサンプルサイズが小さい場合には、p-value が大きくなりやすく、帰無仮説を棄却する力（検定力）が若干低下する可能性がある。
+ <span style="color: red; ">**解釈の変化:**</span><br>
自由度の調整により、検定統計量の解釈が少し異なる場合がある。（通常のt検定と比較して、Welch検定の方が **厳しい条件** で帰無仮説を棄却する）

## ■ Python による実行サンプル

{% highlight python %}
# ライブラリ
import pandas as pd
import numpy as np
{% endhighlight %}

### ◆ データの準備

まずは、サンプルデータを Pandas DataFrame として定義する。

{% highlight python %}
# サンプルデータの生成
np.random.seed(0)
data = {
    'group': ['TG'] * 50 + ['CG'] * 50,
    'cv_per_visitor': np.concatenate([np.random.normal(2.5, 0.5, 50), np.random.normal(2.0, 1.0, 50)])
}
df = pd.DataFrame(data)
{% endhighlight %}

出力イメージは以下
{% highlight python %}
df
{% endhighlight %}

|  | group | cv_per_visitor |
| :---: | :---: | :---: |
| 0 | TG | 3.382026 |
| 1 | TG | 2.700079 |
| ... | ... | ... |
| 98 | CG | 2.126912 |
| 99 | CG | 2.401989 |

+ `group` にて、TG（Treatment Group）と CG（Control Group）には、ランダムに1:1の割合でユーザーが割り当てられていると仮定。<br>
+ `cv_per_visitor` は訪問者毎の何かしらの CV 指標


### ◆ Welchのt検定の実行
Welchのt検定を行い、二つのグループ間での平均の差を検定する。<br>
ここでは `scipy.stats` の `ttest_ind` 関数を使用し、`equal_var = False` オプションを設定して分散の不等性を仮定している。

{: .highlight}
通常のt検定（独立二標本t検定）では、`equal_var = True` ← 両群の分散が等しいことを仮定する

{% highlight python %}
# TG と CG のデータを分ける
tg_data = df[df['group'] == 'TG']['cv_per_visitor']
cg_data = df[df['group'] == 'CG']['cv_per_visitor']

# Welchのt検定を実行
t_stat, p_value = ttest_ind(tg_data, cg_data, equal_var = False)
{% endhighlight %}

### ◆ 出力結果

{% highlight python %}
# 結果の表示
print(f"t統計量: {t_stat}, p値: {p_value}")
{% endhighlight %}

> t統計量: 4.003685078305505, p値: 0.00013393435904555088

結果より、TG と CG の間で平均 CV 数に統計的に有意な差があることが示された。<br>
（帰無仮説の棄却）
