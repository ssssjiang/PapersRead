# VIO-Aided Structure from Motion Under Challenging Environments
**标题 & 年份：**2021

**作者：**Zijie Jiang∗, Hajime Taira∗

**Journal & doi：**ICIT2021

1)Read the title, abstract & introduction. 2) Read the sub-headings. 3) Read the conclusion. 4) Skim the references for familiar ones.

**1st pass (5 min)**

- 类别：add VIO sfm
- 背景：`它与其他哪些论文有关? 用哪些理论基础来分析这个问题?`
- 正确性：`假设看起来是有效的吗?`
- 贡献：提出在序列化的数据集上，结合IMU测量（VIO），来帮助几何验证和增量式重建过程；
  - 使用VIO给出的几何先验过滤matches，这种方案在出现重复结构的场景中非常有效；
  - 使用VIO估计的结果作为增量式重建的初始值，设计了一种新的BA目标函数，平衡基于VIO和基于视觉的惩罚；
  - 设计了一种批量重建的pipeline，在保证模型精度的前提下，控制增量式的全局BA的计算成本；
  - 在重复纹理、若纹理等数据集上，和基于SFM的算法、基于VIO的算法对比；

- 框架结构：
  - 3D RECONSTRUCTION BY VIO-AIDED SFM
    - VIO-Aided Geometric Verification
    - Batched Incremental Reconstruction
      - Batched Image Registration
      - Bundle Adjustment with Relative Pose Constraints

  - 实验：
    -  Quantitative Evaluation for Reconstructed Odometry
      - Influence of Batch Size.

    - Qualitative Evaluation for Reconstructed 3D Models

  - 结论：在公开数据集的实验表明，提出的系统可以在图像为重建提供的视觉证据较少的环境中实现准确而稳健的 3D 重建。 此外，通过批量增量重建过程可以有效地减少重建的计算时间。
  - 展望：未来的工作之一是详细分析确定系统中的几个参数，这些参数与嘈杂 IMU 的可行性和由此产生的错误 VIO 估计高度相关。

- 清晰度：`论文写得好吗?`

**是否值得继续读：**

**2nd pass (1 hour)**

-  VIO-Aided Geometric Verification

  - input：image pairs, VIO 估计的T

  - 利用对极误差判断局部特征对应：

    $E E\left(f_{k, I_{t^{\prime}}}, f_{k, I_{t}}\right)=d\left(f_{k, I_{t^{\prime}}}, \hat{\mathbf{F}} f_{k, I_{t}}\right)$
    where $\hat{\mathbf{F}}=\mathbf{K}^{-\mathrm{T}} \hat{\mathbf{t}}_{\times} \hat{\mathbf{R}} \mathbf{K}^{-1}$

    -- 如果误差超过阈值，则判断为外点；

    -- 当外点率超过阈值时，就判定为错误的匹配对；

    `对极误差：`$\sum_{i} \frac{\left(\mathbf{x}_{i}^{\prime \top} \mathrm{F} \mathbf{x}_{i}\right)^{2}}{\left(\mathrm{~F} \mathbf{x}_{i}\right)_{1}^{2}+\left(\mathrm{F} \mathbf{x}_{i}\right)_{2}^{2}+\left(\mathrm{F}^{\top} \mathbf{x}_{i}^{\prime}\right)_{1}^{2}+\left(\mathrm{F}^{\top} \mathbf{x}_{i}^{\prime}\right)_{2}^{2}}$
  
  - 在过滤错误的匹配对，和错误的匹配对之后，会对图像对重新ransac估计图像之间的变换；

**思考**

1. 基于这个误差过滤的功能，我其实可以看看，在superglue匹配之后，需要被过滤的，多少是错误图像匹配对，多少是错误特征匹配对；

   1. 错误图像匹配对多：
      1. global找到了 错误的匹配对 -- > feature + superglue + ransac不能过滤这种匹配对；
      2. global找到了纹理重复的匹配对，重复纹理的确实不能被SuperGlue解决；
   2. 错误特征匹配对多：SuperGlue 不能拒绝错误匹配 / 产生错误匹配 --> ransac也不能过滤错误匹配；

2. 如果我们使用global建图结果过滤错误匹配，而global建图又依赖于双视图几何的结果，那么目前验证在global建图上有提升，是有意义的；

3. 如果在global环节是可以建图成功的，而用global筛一遍匹配之后，增量式也可以建图成功了，那么这说明什么？

   1. 理论上增量式建图，有纹理重复的结构：

      可能某一帧加入的时候，由于是错误的图像匹配对，注册了错误的pose，然后三角化出错误的3D点，之后就把错误传递下去了；

   2. 如果是错误的特征匹配对，在pnp环节有ransac，三角化环节也有ransac，已经有较多的过滤外点了？

4. SLAM里的建图，或者说maplab的建图是怎么做的？

**3rd pass (4-5 hours)**

- 缺点：
  - 隐藏的假设：
  - 实验和分析上的疑点：
  - 缺失的引用：
- 优点：
  - 结论：
  - 可复用的技术：`表达或实现上的技术`
  - 隐藏的Efficacy：`作者没有提及，但是对我有利的点`

