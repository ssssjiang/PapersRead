# Robust Global Translations with 1DSfM

**标题 & 年份：**

**作者：**

**Journal & doi：**

1)Read the title, abstract & introduction. 2) Read the sub-headings. 3) Read the conclusion. 4) Skim the references for familiar ones.

**1st pass (5 min)**

- 类别：`这是什么类型的论文?测量的论文吗?对现有系统的分析?研究原型的描述?`

- 背景：global-sfm的优势：1. 计算成本低，2. 增量式的SFM处理图像是不平等的，图像被先处理或后处理，会对最终模型产生不成比例的影响，所以图像处理的顺序，有时候会导致drift加剧；

  global-sfm的缺点：global-sfm很难先验的推断哪些测量量是外点，而增量式建图在每个步骤中过滤掉与当前模型不一致的测量值；

  > 一般的global-sfm过程，先解global rotation，然后是translations，给出一系列对极几何对；

- 正确性：`假设看起来是有效的吗?`

- 贡献：

  - 提出1DSfM：一种移除外点的预处理过程，将难题简化为一维的子问题，推断变为组合计算？在1D投影下，translations 问题变为MINIMUM FEEDBACK ARC SET, 通过1D排序，可以恢复3D测量的一致性信息；
  - a simple translations solver based on squared chordal distance.

  和其他global-sfm方案一样，运行的比增量式的方案更快；

- 框架结构：

  - problem formulation；
  - Outlier Removal using 1DSfM；
  - Solving the Translations Problem
  - Solving the Translations Problem
    - Solving for Rotations.
    - Forming a Translations Problem
    - Cleaning with 1DSfM
    - Solving a Translations Problem

  - Results
    - 1DSfM.
    - Comparison to sequential SfM;
    - Comparison to [11]
    - Discussion.
    - Limitations

  - Conclusion
  - 展望：探索更多的聚合1DSfM子问题的方案，而不是简单的求和，来找出更复杂的异常值，例如由模棱两可的场景结构引起的异常值。

- 清晰度：先验知识欠缺的比较多，先看3篇参考文献的1-2pass，再来过这篇的2-pass；

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



\5. Chatterjee, A., Govindu, V.M.: Efficient and robust large-scale rotation averaging. In: ICCV (2013)

\11. Govindu, V.M.: Combining two-view constraints for motion estimation. In: CVPR (2001)

\24. Zach, C., Klopschitz, M., Pollefeys, M.: Disambiguating visual relationships using loop constraints. In: CVPR (2010)
