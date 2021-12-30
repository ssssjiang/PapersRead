# Patch2Pix: Epipolar-Guided Pixel-Level Correspondences

**标题 & 年份：**2021

**作者：**Qunjie Zhou, Torsten Sattler, Laura Leal-Taixe

**Journal & doi：**CVPR

1)Read the title, abstract & introduction. 2) Read the sub-headings. 3) Read the conclusion. 4) Skim the references for familiar ones.

**1st pass (5 min)**

- 类别：Match Algo

- 背景：

  - 目前把 特征提取和描述 & 特征匹配 & 外点拒绝，合到一个网络里的，匹配精度都不高；
  - 传统的SIFT & SURF特征在极端的光照变化，运动模糊和重复 / 弱纹理下是脆弱的；
  - 端到端的输入图像对到输出匹配对，最近已经有相关的工作，目前主要的挑战是，如何使输出的匹配对达到像素级精度；
    - 有的工作为了控制计算量，在相对低的分辨率下匹配，表现出更低的相对位姿估计准确度；

  - 作者认为，使用真值匹配对监督，会给训练提供更精确的信号，但是同时也会给学习的过程增加bias，如使用特定的特征detector的SFM pipeline生成的稀疏特征点作为监督，那么keypoint detector可能只是简单的学习去重复这些特征，而不是学习到more general features，最近已经有一些工作使用相对camera pose作为弱监督来学习局部特征，替代mean matching score loss；

- 正确性：

- 贡献：提出了一种新的匹配网络；

  - 我们的网络首先获得patch级别的match proposals，然后将其细化到像素级别；
  - 我们构造了一种新型的细化网络，同时通过回归细化match和拒绝outlier proposals，训练的过程不需要像素级的真值对应；
  - 展示了在匹配测试、单应估计、视觉重定位等环节的匹配精度的提升；
  - 我们的模型泛化到全监督的方案不需要重新训练，在室内室外重定位任务上效果都不错；`提这个，是为了用全监督的方法训练，配适重定位实验`

- 框架结构：

  - Patch2Pix: Match Refinement Network

    - Refinement: Pixel-level Matching
      - Feature Extraction
      - From match proposals to patches
      - Local Patch Expansion
      - Progressive Match Regression

    - Losses
      - Sampson distance
      - Classification loss
      - Geometric loss


    - Implementation Details

  - Evaluation on Geometrical Tasks

    -  Image Matching
    - Homography Estimation
    - Outdoor Localization on Aachen Day-Night
    - Indoor Localization on InLoc

  - Conclusion：同贡献；

- 清晰度：`论文写得好吗?`

**是否值得继续读：**

**2nd pass (1 hour)**



**3rd pass (4-5 hours)**

- 缺点：
  - 隐藏的假设：
  - 实验和分析上的疑点：
  - 缺失的引用：
- 优点：
  - 结论：
  - 可复用的技术：`表达或实现上的技术`
  - 隐藏的Efficacy：`作者没有提及，但是对我有利的点`



[47] Qianqian Wang, Xiaowei Zhou, Bharath Hariharan, and Noah Snavely. Learning feature descriptors using camera pose supervision. In ECCV, 2020. 1, 2, 3, 6, 7, 8, 10, 11 :star: