---
layout:     post
title:      Sampling via Gradient Flows: Mean Field Models
subtitle:   论文记录，个人学习用，侵删
date:       2023-3-26
author:     Chengrui
header-img: img/gnn.jpg
catalog: true
tags:
    - 计算流体力学
    - 论文笔记
    - 强化学习
---



## Sampling via Gradient Flows: Mean Field Models

1. Authors: Zhengyu Huang, Maxime Herlands, Youssef Mroueh, Alan Willsky

2. Affiliation: 麻省理工学院

3. Keywords: Reinforcement learning, gradient flows, sampling, mean-field models

4. Urls: https://arxiv.org/abs/1906.05826, Github: None

5. Summary:

   - (1):该文介绍了概率分布的一种新的建模和采样方法。

   - (2):过去的方法主要集中在粒子数和对称性方面，在采样复杂的分布时存在一些问题。这篇文章的方法可以更好地解决这些问题。

   - (3):该文提出了一种采样方法，通过建立梯度流动模型，结合均值场模型，模拟分布的状态。通过分析重要性权重进行分布采样。该方法在采样复杂的分布上表现良好。

   - (4):该方法在各种不同的后验概率分布的任务上进行了测试，并获得了非常好的效果。 该方法达到了他们的目标并且性能支持他们的论点。



8. Conclusion: 
 - (1): 该文介绍了一种新的建模和采样方法，可以更好地解决在采样复杂的分布时存在的问题。该方法在各种不同的后验概率分布的任务上进行了测试，并获得了非常好的效果。这种方法可以作为解决采样问题的可行方案。

 - (2): 创新点：采用梯度流动模型，结合均值场模型，模拟分布的状态进行采样，避免了过去方法存在的问题。模型性能方面，该方法在不同的后验概率分布任务中表现良好，达到了作者的目标并且性能支持作者的论点。工作量方面，该文使用了相对较少的数据集进行实验，但是作者的实验结果以及分析十分详细，给读者呈现了更细致的结果分析。



