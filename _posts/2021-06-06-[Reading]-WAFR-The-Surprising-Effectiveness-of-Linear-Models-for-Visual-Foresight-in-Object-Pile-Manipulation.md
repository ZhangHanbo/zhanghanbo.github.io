---
layout: post
title:  "WAFR2020: The Surprising Effectiveness of Linear Models for Visual Foresight in Object Pile Manipulation"
date:   2021-06-06 18:00:40
blurb: "Here is an example of blog."
og_image: /assets/img/content/post-example/Banner.jpg
---

<img src="{{ "/assets/img/blog/WAFR2020_H.J._Terry_Suh/introduction.png" | absolute_url }}" alt="introduction" class="post-pic"/>
<br />

#### Table of Contents
1. [Abtract](#Abtract)
5. [Discussion](#Discussion)

#### Abtract

这篇文章发表于WAFR 2020。该论文研究了基于模型的物体堆操作策略（如一堆切好的胡萝卜）。该论文使用了三种方法对
物体堆的动力学进行建模：1.线性模型；2.深度自编码器模型；3.面向物体的动力学模型。实验结果表明，深度自编码器模型
的泛化能力在该任务上是最差的。其余两个模型都有较好的泛化能力，能够在真实场景的任务中取得较好的性能，而线性模型
在三种模型中表现是最好的。

#### Discussion

具体算法细节和实验结果可参考原文。这里总结一些这篇文章中值得思考的点：

1. 为什么线性模型在三种模型中表现最好？众所周知，线性模型可以作为一种最简单的深度模型。其形式简单，参数少，对于
特定数据集有最优的解析解。但是其作为最简单的深度模型（也即仅有一层的不包含激活函数的全连接网络），在20000+数据
的情况下仍然表现出比深度模型更好的泛化能力。这是值得思考的。一个更深层次的问题：在什么任务上线性模型能够取得更好
的性能？什么时候需要用深度学习，什么时候需要用线性回归？基于这篇文章的任务，初步的猜测是由于本文对于物体堆操作的
建模是较为简单的，包括：1）简单的状态表征（当前图像减背景后非零值点较少）；2）简单的动力学（仅有推动的动作，因此
大部分的非零像素点都不会有较大移动）。因此，本文提出的使用线性动力学模型的方法还需在更复杂的环境中测试其有效性。

2. 本文虽然是基于模型的控制，但却并不涉及长时规划算法。可以看到，即使在最复杂的任务设置下，使用单步的贪婪搜索策略
也能取得较好的性能。一个简单的想法是，对于较为简单的任务，规划算法并不能够带来足够的性能提升。相反，对于这种任务较
简单，动力学模型缺并不精确的任务，使用长时规划甚至有可能出现性能的退化。原因在于长时规划所获取的最优动作，往往建立
在对未来状态的准确估计上。

3. 文章中DVF-Origin和DVF-Affine的对比说明了深度学习似乎并不适合学习形如s'=f(s,a)的动力学模型。相反，对于离散
的动作空间，通过单独的学习不同动作对应的动力学模型会取得更好的性能。可能的原因在于如果将动作a作为输入送入网络进行
训练，那么在测试过程中所出现的未知动作会导致模型产生不可预测的错误输出。如在推碎萝卜的任务中，如果对于某个未知动作
a，深度学习获取的动力学模型判断a是一个最优动作，但a并未与物体堆接触，那么很有可能会造成所学策略在测试时陷入死循环，
从而造成任务失败。

##### REFERENCES

[^1]: Suh, H. J., and Russ Tedrake. "The surprising effectiveness of linear models for visual foresight in object pile manipulation." arXiv preprint arXiv:2002.09093 (2020).
