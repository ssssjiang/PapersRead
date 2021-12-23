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

- 投影模型和问题的参数化：SFM问题是一个非常复杂和异构的模型；

  - 实际参数化非常复杂，有的参数，比如旋转，它有四元数模长为1的约束，欧拉角有奇点，3D特征也可能不是点，是线或面，这两种没有一个global参数表达，还有远距离特征带来的数值问题；

  - 不同的参数化，原则上是等效的，但是其实通常具有不同的数值行为，会极大的影响BA的迭代速度和可靠性；
  - 合适的参数化，应该尽可能的均匀、有限，在选择的误差模型下，应该局部接近线性 -> 误差函数局部近似平方，非线性的模型的二阶近似 准确度越低，收敛越困难；

  :triangular_flag_on_post: 还是MVG里的内容要掌握扎实；

  - 3D points：
    - 使用(X, Y, Z)或(X, Y, Z, 1)参数化，如果是非常远的点，那么(X, Y, Z)需要非常大的变化，才能对应到图像上的变化，那么在(X, Y, Z)空间，cost调整的步长会非常大。
    - 使用(X, Y, Z, W)，接近无穷大的行为是自然的、有限的、条件良好的，只要通过归一化到4维单位球上，把无穷远点的齐次向量保持在有限的值(W -> 0)。
    - 视觉上的无穷远点，对应的是虚拟点。如果图像噪声，恰好把一个点推到无穷远处，那这个真实的3D点最后可能被重建成虚拟点，这不可避免，因为视觉重建的几何和误差模型是投影，不是仿射；
  - Rotation：
    - 欧拉角：有奇点，部分区域覆盖不均匀；
    - :triangular_flag_on_post: 旋转应该被参数化为受$\|\boldsymbol{q}\|^{2}=1$约束的四元数，或者局部扰动$\delta R$，它可以是3参数小旋转近似，$\boldsymbol{\delta} \boldsymbol{R}=\left(\boldsymbol{I}+[\boldsymbol{\delta} \boldsymbol{r}]_{\times}\right)$，李代数？小欧拉角？旋转向量？
  - 状态更新$\mathrm{X} \rightarrow \mathrm{X}+\delta \mathrm{x}$：
    - 尽管有些状态更新无法用向量加法来精确表示，但我们先假设我们可以局部线性化状态流型，局部解决它可能受到内部约束，产生一个无约束的向量$ \delta X$，来更新状态；
    - 对cost函数局部泰勒展开$f(x+\delta x) \approx f(x)+\frac{d f}{d x} \delta x+\frac{1}{2} \delta x^{\top} \frac{d^{2} f}{d x^{2}} \delta x$，然后估计状态更新量$ \delta X$；
      - $ \delta X$不需要和X表达一致；
    - $\mathrm{X} \rightarrow \mathrm{X}+\delta \mathrm{x}$包含的不只是加法操作，比如：$q \rightarrow q+d q$更新四元数后，需要归一化$\|\boldsymbol{q}\|^{2}=1$来校正，$\boldsymbol{R} \rightarrow \boldsymbol{R}\left(1+[r]_{\times}\right)$更新后的旋转矩阵通常也不再是旋转矩阵；

- 误差建模：robust statistically-based error metrics based on total (inlier + outlier) log likelihoods should be used.

  - 合理的误差建模，应该正确地考虑到外点的存在；

  - 典型的 ML 成本函数是所有观察到的图像特征的预测误差的总负对数似然。 对于高斯误差分布，这简化为平方协方差加权预测误差的总和， MAP 估计器通常会添加成本项，使某些结构或相机校准参数偏向于它们的预期值；

  - 成本函数的设计：

    - ML / MAP：

      - ML选择的模型是最有可能观察到现有数据的模型，或者说，对于给定观察的总后验概率最高的模型；
      - MAP增加了先验信息，而ML也可以把这部分先验作为额外的“观测”，就估计而言，ML和MAP的区别纯粹是术语上的；

    - 成本函数的组成项来源多样，除了图像点重投影误差，还可以有：

      - GPS，IMU，已知的3D点，其他模型的估计（如：卡尔曼滤波）；
      - 先验：先验信息表达成软性的约束（相机标定或位姿）；
      - 惩罚：overfitting, regularization or description length penalties；

      BA的优势之一就是可以联合这些不同来源的信息；

    - 假设给定模型的源在统计上彼此独立，则总概率是来自各个源的概率的乘积，为了获得成本函数，我们采用log，因此总对数似然是各个源对数似然的总和；

    :orange: 这部分把成本函数拆开了解释：

    1. 各种源给到的观测数据都是存在误差的，它们的误差会满足某种分布，对于连续的数据可以用概率密度函数来表达这种分布，且各个源的数据分布是独立的；
    2. 各种源的数据，显式或隐式地和待估计的参数$X$有关，有的是对$X$的观测，有的观测变量和$X$有换算关系，需要一个概率模型把它们组合到一起；
       1. ML计算最大似然概率，就是把对$X$显示 / 隐式的采样数据的概率密度函数（PDF，也即分布）相乘，从得到的$X$的PDF中，挑出概率最大的$X$的取值；
       2. MAP还会把$X$的先验PDF乘进来；
       3. 如果我们对于观测的分布都预设为高斯分布，那么PDF的形式就确定了，是

  

**3rd pass (4-5 hours)**

- 缺点：
  - 隐藏的假设：
  - 实验和分析上的疑点：
  - 缺失的引用：
- 优点：
  - 结论：
  - 可复用的技术：`表达或实现上的技术`
  - 隐藏的Efficacy：`作者没有提及，但是对我有利的点`

