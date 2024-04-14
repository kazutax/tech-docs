---
layout: default
title: サンプルサイズ計算
description: "A/Bテストの有効性は正しいサンプルサイズの選定に大きく依存する。A/Bテストにおけるサンプルサイズ計算の基礎と、カイ二乗検定およびt検定を中心にその方法を解説する。"
nav_order: 2
parent: 統計学（頻度論）
has_children: false
has_toc: false
# permalink: /index-stats
---

# A/Bテストのためのサンプルサイズ計算

+ A/Bテストの有効性は正しいサンプルサイズの選定に大きく依存する。
+ カイ二乗検定および独立標本t検定における、サンプルサイズ計算方法を以下に示す。

## ■ 前提

+ サンプルサイズを決定する際には、実験の **検出力**（第二種の過誤のリスクを制御）、**有意水準**（第一種の過誤のリスク）、**効果量**（実験群と対照群の差）を考慮する。
+ これらのパラメータを使ったサンプルサイズ計算とテスト設計は、テストの信頼性と有効性を確保するために重要。
+ 参考：[統計学｜検出力とはなんぞや ](https://note.com/hanaori/n/nc55ac8614799)

## ■ Python スクリプト

{% highlight python %}
# ライブラリ
import numpy as np
import scipy.stats as stats
import math
{% endhighlight %}

### ◆ カイ二乗検定の場合

{% highlight python %}
def calculate_sample_size_chisquare(w, alpha, power, ratio = 1):
    """
    カイ二乗検定のためのサンプルサイズ計算

    :param w: 効果量（Cohen's w）
    :param alpha: 有意水準
    :param power: 検定力（1-β）
    :param ratio: 介入群とコントロール群のサンプルサイズの比率（n1/n2）
    :return: 介入群とコントロール群の必要サンプルサイズ（n1, n2）
    """
    df = 1  # 2つの群の場合、自由度は1
    chi2_alpha = stats.chi2.ppf(1 - alpha, df)
    chi2_power = stats.chi2.ppf(power, df)
    n = (chi2_alpha + chi2_power) / (w ** 2)
    n1 = n * (1 + 1 / ratio)
    n2 = n * (1 + ratio) / ratio
    return math.ceil(n1), math.ceil(n2)
{% endhighlight %}

※ 効果量（Cohen's w）は以下で求める

{% highlight python %}
# Cohen's wを計算（カイ二乗統計量とn数から求める）
cohen_w = np.sqrt(chi_squared_stat / n)
{% endhighlight %}

※ 実行例
{% highlight python %}
# 例: w = 0.3, alpha = 0.05, power = 0.8, ratio = 1
n1, n2 = calculate_sample_size_chisquare(0.3, 0.05, 0.8, 1)
print(f"介入群のサンプルサイズ: {n1}, コントロール群のサンプルサイズ: {n2}")
{% endhighlight %}

### ◆ 独立標本t検定の場合
{% highlight python %}
def calculate_sample_size_ttest_cohens_d(d, alpha, power, ratio = 1):
    """
    効果量Cohen's dを使用した独立標本t検定のサンプルサイズ計算

    :param d: 効果量（Cohen's d）
    :param alpha: 有意水準（片側検定の場合）
    :param power: 検定力（1-β）
    :param ratio: 介入群とコントロール群のサンプルサイズの比率（n1/n2）
    :return: 介入群とコントロール群の必要サンプルサイズ（n1, n2）
    """
    # z値を計算
    z_alpha = stats.norm.ppf(1 - alpha)
    z_power = stats.norm.ppf(power)
    
    # nの計算
    n = ((z_alpha + z_power) ** 2) * (2 * (1 + 1/ratio)) / (d ** 2)
    
    # 介入群とコントロール群のサンプルサイズ
    n1 = n * (1 + 1 / ratio)
    n2 = n * (1 + ratio) / ratio
    return math.ceil(n1), math.ceil(n2)
{% endhighlight %}

※ 効果量（Cohen's d）は以下で求める

{% highlight python %}
# Cohen's dを計算（標準偏差が群間で等しいと仮定）
cohen_d = (mean_a - mean_b) / std_common
{% endhighlight %}

※ 実行例
{% highlight python %}
# 例: d = 0.5, alpha = 0.05, power = 0.8, ratio = 1
n1, n2 = calculate_sample_size_ttest_cohens_d(0.5, 0.05, 0.8, 1)
print(f"介入群のサンプルサイズ: {n1}, コントロール群のサンプルサイズ: {n2}")
{% endhighlight %}

## ■ 備考
+ A/Bテストに必要なサンプルサイズは、期待する効果量からどの程度必要か、上記のやり方で手に入る。
+ 必要な「テスト期間」についても、上記の効果量を正しく検定するサンプルサイズを得るのに、どの程度の期間を要するのか、から概算可能。
