# Comparative Evaluation of Hand-Crafted and Learned Local Features

**标题 & 年份：**2017

**作者：**Johannes L. Schonberger, Hans Hardmeier

**Journal & doi：**CVPR

1)Read the title, abstract & introduction. 2) Read the sub-headings. 3) Read the conclusion. 4) Skim the references for familiar ones.

**1st pass (5 min)**

- 类别：benchmark

- 背景：SFM, feature match

- 正确性：`假设看起来是有效的吗?`

- 贡献：提出一种评估方案，来评价特征。仅仅是匹配性能不能全面的评价描述子的质量，比如在非常相似的图像对中找到更多的匹配，并不代表在视角、光照变化的条件下有更好的匹配性能。

  - 提出了一种更细致的匹配性能评价方案
  - 从建图的角度评价描述子：注册图像的能力 + 位姿估计精度 + 场景的完整性；

  - 在视点、光照变化大的场景下评价；
  - 对与评估方案和使用的数据集，均公开。

- 框架结构：

  - 评价：介绍评价方案；
    - 设置和指标：
      - 评估描述子流程：特征提取，描述子匹配，几何验证，`Matching Metrics`，`Reconstruction Metrics`，数据集；
      - 结果和讨论：表现，`图像匹配`，`重建`；
    - 总结：在提出的评价方案下，手工特征效果优于学习特征，学习的特征在不同的数据集上表现方差较大，需要在大范围的数据集上才能评估出学习特征的质量。也说明学习特征需要更多的训练数据。

- 清晰度：可以。

**是否值得继续读：**先细看重点部分，有时间再通读。

**2nd pass (1 hour)**

- Matching Metrics：

  首先是在每个图像对上评价匹配性能的三个基础指标：

  - Putative Match Ratio：#Putative Matches / #Features `可以有`
  - [x] Precision：\#Inlier Matches / #Putative Matches
  - [x] Matching Score：#Inlier Matches / #Features
  -  Recall： #Inlier Matches / #True Matches`True Matches哪来？`

- Reconstruction Metrics：

  先用SFM对场景建一个稀疏的图，然后把它作为MVS的输入，获得场景的稠密表达：深度图、稠密点云，获得高质量的3D模型。

  - the number of registered images and sparse points：这两个量化建图的完整性；
  - number of observations per image：有多少点验证图像；
  - the track length：每个3D点有多少图像观测来验证；
  - reprojection error：表明重建的准确度，它主要受输入数据的冗余和准确度影响，而依赖于特征对应图的完整性和关键点定位的准确性。
  - metric pose accuracy of the camera locations：对于有真值的数据集可以评价这个。

  MVS 问题归结为多个视图之间的密集对应估计，要求准确的相机内外参估计。

  - the number of reconstructed dense points；
  - accuracy and absolute completeness of the dense reconstruction：有深度图真值的可以评价。

  

**3rd pass (4-5 hours)**

- 缺点：
  - 隐藏的假设：
  - 实验和分析上的疑点：
  - 缺失的引用：
- 优点：
  - 结论：
  - 可复用的技术：`表达或实现上的技术`
  - 隐藏的Efficacy：`作者没有提及，但是对我有利的点`

