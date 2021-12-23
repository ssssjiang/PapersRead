# Bundle Adjustment — A Modern Synthesis

**标题 & 年份：**2000

**作者：**Bill Triggs1, Philip McLauchlan2, Richard Hartley3 and Andrew Fitzgibbon4

**Journal & doi：**

1)Read the title, abstract & introduction. 2) Read the sub-headings. 3) Read the conclusion. 4) Skim the references for familiar ones.

**1st pass (5 min)**

- 类别：Math Theory

- 背景：开创性的论文

- 正确性：:happy:

- 贡献：
  - 定义：Bundle adjustment is the problem of refining a visual reconstruction to produce jointly optimal structure and viewing parameterr (camera pose and/or calibration) estimates.
  
  - 损失函数的选择、鲁棒性；
  
    we will not assume a least squares / quadratic cost model. Instead, the cost will be modelled as a sum of opaque contributions from the independent information sources (individual observations, prior distributions, overfitting penalties . . . ).
  
  - 数值优化方法；

  - 线性收敛近似；
  
  - 更新和迭代方法；
  
  - 规范（基准）不变性；
  
  - 质量控制；
  
- 框架结构：

  - 介绍部分不太明白……
  - BA问题的投影模型和参数化；
  - 误差函数的选择；
  - 讨论网络结构：参数的交互和稀疏性；
  - 二阶类牛顿法优化；
  - 一阶收敛方法；
  - 讨论更新策略和递归过滤约束的方法；
  - 约束理论；
  - 详细介绍了用于监测参数估计的准确性和可靠性的质量控制方法；
  - 给出一些关于网络设计的简短提示，即如何放置镜头以确保准确、可靠的重建；
  - 总结主要结论，对这些方法给出建议：
    - bundle的发展历史和参考文献；
    - 矩阵分解、更新和协方差计算的细节；
    - 介绍一些实现BA的软件资源。

- 清晰度：继续！

**是否值得继续读：**这篇文章，更像一本书的形式，可以把第一章理解为“引子”，对不熟悉的领域，引子很难懂是正常的，以及，书的结构会在引子交代，1-pass看引子就可以了。

**2nd pass (1 hour)**

- 投影模型和问题的参数：SFM问题是一个非常复杂和异构的模型；

  - 实际参数化非常复杂，有的参数，比如旋转，它有四元数模长为1的约束，3D特征也可能不是点，是线或面，这两种没有一个global参数表达，还有远距离特征带来的数值问题；

  - 不同的参数化，原则上是等效的，但是其实通常具有不同的数值行为，会极大的影响BA的迭代速度和可靠性；
  - 合适的参数化，应该尽可能的均匀、有限，在选择的误差模型下，应该局部接近线性 -> 误差函数局部近似平方，非线性的模型的二阶近似 准确度越低，收敛越困难；

  :triangular_flag_on_post: 还是MVG里的内容要掌握扎实；

  

  

  

**3rd pass (4-5 hours)**

- 缺点：
  - 隐藏的假设：
  - 实验和分析上的疑点：
  - 缺失的引用：
- 优点：
  - 结论：
  - 可复用的技术：`表达或实现上的技术`
  - 隐藏的Efficacy：`作者没有提及，但是对我有利的点`

