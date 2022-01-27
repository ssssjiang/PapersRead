# Bundle Adjustment in the Large

**标题 & 年份：**2010

**作者：**Sameer Agarwal, Noah Snavely, Steven M. Seitz and Richard Szeliski

**Journal & doi：**ECCV

1)Read the title, abstract & introduction. 2) Read the sub-headings. 3) Read the conclusion. 4) Skim the references for familiar ones.

**Link：**https://grail.cs.washington.edu/projects/bal/

**1st pass (5 min)**

- 类别：Math Theory

- 背景：`它与其他哪些论文有关? 用哪些理论基础来分析这个问题?`

- 正确性：`假设看起来是有效的吗?`

- 贡献：BA：联合非线性优化相机和点的参数，是SFM 任务中的关键部分，随着任务中图像数量的增加，BA的可扩展性至关重要；

  - 探索使用共轭梯度计算牛顿步；
  - 舒尔补技巧不仅限于基于分解的方法，还可以将其解释为一种预处理形式；
  - 在多个数据集上，建立多种BA问题，并使用它们评估六种不同BA 算法的性能；
  - 实验验证，进行简单的预处理后，truncated Newton为BA提供了最先进的性能；

- 框架结构：

  - 介绍：

    - SBA的实现基于`a dense Cholesky factorization of the reduced camera matrix`，具有平方时间复杂度、立方空间复杂度，不适用于数据量大的BA任务；
    - 大尺度的BA算法没有引起足够的重视，因为之前的数据集多是结构化的，矩阵极度稀疏；

    :triangular_flag_on_post: 先对BA 有个基本了解之后再来看，现在看不懂。

  - 

  - 

- 清晰度：`论文写得好吗?`

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





**参考文献**

4. Triggs, B., McLauchlan, P., R.I, H., Fitzgibbon, A.: Bundle Adjustment - A modern synthesis. In: Vision Algorithms’99. (1999) 298–372
5. :star:Lourakis, M., Argyros, A.A.: SBA: A software package for generic sparse bundle adjustment. TOMS 36 (2009) 2 
6. :star: Ni, K., Steedly, D., Dellaert, F.: Out-of-core bundle adjustment for large-scale 3d reconstruction. In: ICCV. (2007)
