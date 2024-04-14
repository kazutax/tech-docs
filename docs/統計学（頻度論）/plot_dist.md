---
layout: default
title: 有意水準と検出力
description: "有意水準と検出力"
nav_order: 1
parent: 統計学（頻度論）
has_children: false
has_toc: false
# permalink: /index-stats
---

# 有意水準と検出力

+ 工事中

## ■ 前提

+ サンプルサイズを決定する際には、実験の **検出力**（第二種の過誤のリスクを制御）、**有意水準**（第一種の過誤のリスク）、**効果量**（実験群と対照群の差）を考慮する。
+ これらのパラメータを使ったサンプルサイズ計算とテスト設計は、テストの信頼性と有効性を確保するために重要。
+ 参考：[統計学｜検出力とはなんぞや ](https://note.com/hanaori/n/nc55ac8614799)

## ■ Python スクリプト

{% highlight python %}
# ライブラリ
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import norm

%config InlineBackend.figure_format = 'retina'
{% endhighlight %}

### ◆ 可視化

{% highlight python %}
def plot_distributions(sample_size, tg_ratio, cg_mean, pooled_std, lift):
    # Calculate sample sizes for control and treatment groups
    cg_n = int(sample_size / (1 + tg_ratio))
    tg_n = sample_size - cg_n

    # Calculate the mean for the treatment group based on the lift
    tg_mean = cg_mean * lift

    # Calculate Cohen's d effect size
    effect_size = (tg_mean - cg_mean) / pooled_std

    # Calculate standard errors
    cg_se = pooled_std / np.sqrt(cg_n)
    tg_se = pooled_std / np.sqrt(tg_n)

    # Calculate the distributions under the null and alternative hypotheses
    x_range = np.linspace(cg_mean - 4 * cg_se, tg_mean + 4 * tg_se, 1000)
    y_cg = norm.pdf(x_range, cg_mean, cg_se)
    y_tg = norm.pdf(x_range, tg_mean, tg_se)

    # Set significance level and calculate critical value and power
    alpha = 0.05
    critical_value = norm.ppf(1 - alpha, cg_mean, cg_se)
    power = 1 - norm.cdf(critical_value, tg_mean, tg_se)
    
    # Visualization
    plt.figure(figsize = (10, 6))
    plt.plot(x_range, y_cg, label = 'Control Group ($H_0$: Null Hypothesis)', color='blue')
    plt.plot(x_range, y_tg, label = 'Treatment Group ($H_1$: Alternative Hypothesis)', color='red')
    plt.fill_between(x_range, 0, y_cg, where = (x_range > critical_value), color = 'blue', alpha = 0.3, label = 'Rejection Region (5% Significance)')
    plt.fill_between(x_range, 0, y_tg, where = (x_range > critical_value), color = 'red', alpha = 0.3, label = 'Power Region (Test Sensitivity)')
    plt.axvline(critical_value, color = 'grey', linestyle = '--', label = 'Critical Value')
    plt.title('Distribution of Means under $H_0$ and $H_1$')
    plt.xlabel('Mean')
    plt.ylabel('Density')
    plt.legend()
    plt.show()

    print("Mean CG: {:.2f}, Mean TG: {:.2f}".format(cg_mean, tg_mean))
    print("Effect Size (Cohen's d): {:.2f}".format(effect_size))
    print("Critical value: {:.2f}".format(critical_value))
    print("Power of the test: {:.2%}".format(power))
{% endhighlight %}

※ 出力結果

{% highlight python %}
# Example usage of the function
plot_distributions(sample_size = 1000, tg_ratio = 1, cg_mean = 2.3, pooled_std = 2, lift = 1.1)
{% endhighlight %}
