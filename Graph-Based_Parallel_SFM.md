# PaperRead

**标题 & 年份：**Graph-Based Parallel Large Scale Structure from Motion

**作者：**Yu Chen, Shuhan Shen

**Journal & doi：**Pattern Recognition

1)Read the title, abstract & introduction. 2) Read the sub-headings. 3) Read the conclusion. 4) Skim the references for familiar ones.

**1st pass (5 min)**

- 类别：SFM
- 背景：`Icremental & global SFM` `maximum spanning tree` `minimum spanning tree` `Minimum Height Tree`
- 正确性：80%
- 贡献：
  - 提出了一种鲁棒的图像聚类方法，而连接性的提升来自MaxST；
  - 提出了一种基于图的子模型merge算法，由MinST来找到子图之间准确的变换关系；
  - 算法的时间复杂度是线性的；

- 清晰度：目前我对算法的了解还不够；

**是否值得继续读：**

**2nd pass (1 hour)**

这部分的笔记不一定要记在这里，这部分可以按之前我按内容分类记录的文档上。

**3rd pass (4-5 hours)**

- 缺点：
  - 隐藏的假设：
  - 实验和分析上的疑点：
  - 缺失的引用：
- 优点：
  - 结论：
  - 可复用的技术：`表达或实现上的技术`
  - 隐藏的Efficacy：`作者没有提及，但是对我有利的点`



**感兴趣的参考论文：**

1. Divide and conquer: Efficient large-scale structure from motion using graph partitioning [2016]
2. Shapefit and shapekick for robust, scalable structure from motion [2014]
3. Large scale sfm with the distributed camera model [2016]
4. Robust global translations with 1dsfm [2014] D
5. Distributed very large scale bundle adjustment by global camera consensus [2017]
6. Parallel structure from motion from local increment to global averaging [2017]
7. Very large-scale global sfm by distributed motion averaging [2018] D
8.  Baseline desensitizing in translation averaging [2018] D

我很伤心，这些论文竟然没有一篇有代码，但是至少陈煜的论文有代码……

这些都都先看电子版的1pass，总结更多的信息之后再决定打出来看哪些。
