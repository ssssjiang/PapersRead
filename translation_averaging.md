[TOC]

# Translation averaging

## QA

- 为什么translation averaging不用于工程？

- 为什么先做rotation averaging 再做 translation averaging？

  因为rotation averaging相比于translation averaging是一个更容易有稳定的解的问题。相对来说是一个well-posed的问题。并且解决rotation averaging问题后会有更多的信息提供给translation averaging。

- 增量式方法和全局式方法的输入都是相机之间的相对位姿，相对位置都没有尺度，为什么全局式方法受影响大？

  增量式方法中每一次相机注册中都利用PnP计算新的相机位姿，使得整体的尺度与初始化时相同。而全局式方法需要满足一个比较强的条件（parallel rigidity）才可能成功恢复相机的位置。Parallel rigidity依赖于无噪声的假设，如果有相对平移方向噪声，则会破坏parallel rigidity，进而导致无法获得稳定的解。

## Paper list

- [x] 2014, [Robust Global Translations with 1DSfM,](https://sci-hub.do/https://link.springer.com/chapter/10.1007/978-3-319-10578-9_5) Kyle Wilson and Noah Snavely, 283 cites

  主要贡献在1DSfM预处理，可帮助滤除错误的epipolar geometries。

- [x] 2015, [Robust Camera Location Estimation by Convex Programming](https://openaccess.thecvf.com/content_cvpr_2015/html/Ozyesil_Robust_Camera_Location_2015_CVPR_paper.html), [O Ozyesil](https://scholar.google.com/citations?user=LzQ9nVcAAAAJ&hl=en&oi=sra), [A Singer](https://scholar.google.com/citations?user=h67w8QsAAAAJ&hl=en&oi=sra), 115 cites

  主要贡献有两个：1. 使用parallel rigidity定义了translation averaging 问题在什么时候是 well-posedness，即当给定一个问题的时候，在这个问题的限定条件下，我们是否有足够的信息在无噪声的情况下稳定地唯一地估计出相机的位置；2. 对匹配鲁棒的 translation direction 估计方法，对outlier direction鲁棒的位置估计方法 (a convex program)

- [ ] 2018, [Very Large-Scale Global SfM by Distributed Motion Averaging](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhu_Very_Large-Scale_Global_CVPR_2018_paper.html), 53 cites

- [ ] 2018, [Baseline Desensitizing in Translation Averaging](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhuang_Baseline_Desensitizing_in_CVPR_2018_paper.html), Bingbing Zhuang, Loong-Fah Cheong, Gim Hee Lee; cites 18

- [ ] 2013, [Global fusion of relative motions for robust, accurate and scalable structure from motion](https://www.cv-foundation.org/openaccess/content_iccv_2013/html/Moulon_Global_Fusion_of_2013_ICCV_paper.html), Pierre Moulon, Pascal Monasse, Renaud Marlet, 285 cites

- [ ] R. Hartley, J. Trumpf, Y. Dai, and H. Li. Rotation averaging.IJCV, 103(3):267–305, 2013.

- [ ] 2016, [ShapeFit and ShapeKick for Robust, Scalable Structure from Motion](https://sci-hub.do/https://link.springer.com/chapter/10.1007/978-3-319-46478-7_18), 37 cites

- [ ] 2015, [Global Structure-from-Motion by Similarity Averaging](https://www.cv-foundation.org/openaccess/content_iccv_2015/html/Cui_Global_Structure-From-Motion_by_ICCV_2015_paper.html), 39 cites 

- [ ] 2013, [A global linear method for camera pose registration](https://sci-hub.do/https://ieeexplore.ieee.org/document/6751169), 62 cites

- [ ] 2015, [Linear global translation estimation with feature tracks](https://arxiv.org/abs/1503.01832), 49 cites

## 运动推断结构算法

从一个（无序的）图像集合中，通过估计相机运动来恢复出场景结构，Translation averaging 主要用于估计相机平移运动。

<img src="pics/image-20210825092702458.png" alt="image-20210825092702458" style="zoom: 50%;" />

一般方法是：1. 寻找图像间的特征点对应，并计算图像间的相对位姿；2. 估计相机绝对位姿；3. 估计三维结构。

- **增量式SfM**：第二步和第三步迭代进行，一般使用PnP算法每次注册一个或一批相机（估计相机位姿），三角化（估计三维结构），进行一次BA后继续使用PnP算法注册相机…
  特点是，容易累积误差，但是由于反复BA及RANSAC算法的反复使用，耗时长但是对外点鲁棒。

- **全局式SfM**：首先估计相机位姿，然后再进行三角化估计三维结构，通常进行一次全局BA。
  相机姿态：Rotation averaging，稳定并且可以高效求解。即使在窄基线情况下也可以得到很好的结果。
  相机位置：Translation averaging，全局估计相机位置是ill-conditioned问题（由于相机的相对平移尺度不确定）。受outlier和基线宽度影响很大。
  特点是全局估计相机位姿，没有累积误差的问题，但translation averaging相比增量式注册相机的方式需要很强的条件才能得到稳定（唯一）的解（在无噪声假设下需要满足parallel rigidity条件）。另外translation averaging提高鲁棒性的方法一般是预先处理相对位姿的外点，或者使用对相对平移方向鲁棒的cost function。而增量式方法可以使用RANSAC算法在多个环节提高鲁棒性。

## 0 1DSfM (1 Dimension projection Structure from Motion)

主要贡献是 1DSfM 预处理和一个 translation 问题的 solver。

步骤：

1. 预处理1DSfM：从所有的方向中移除outliers。具体做法是，降维至一维 (single-dimensional) 的子问题。
2. translation averaging。

### 0.0 Prerequisite

- **Minimum Feedback Arc Set**

> [Feedback arc set:](https://en.wikipedia.org/wiki/Feedback_arc_set) In [graph theory](https://en.wikipedia.org/wiki/Graph_theory) and [graph algorithms](https://en.wikipedia.org/wiki/Graph_algorithm), a **feedback arc set** or **feedback edge set** in a [directed graph](https://en.wikipedia.org/wiki/Directed_graph) is a subset of the edges of the graph that contains at least one edge out of every cycle in the graph.

<img src="pics/image-20210812232617524.png" alt="image-20210812232617524" style="zoom:50%;" />

- [n-sphere](https://en.wikipedia.org/wiki/N-sphere)

> 2-sphere：用 $S^2$ 表示，为三维欧式空间中ball的boundary。可以理解为三维球的表面。
>
> ![{\displaystyle S^{n}=\left\{x\in \mathbb {R} ^{n+1}:\left\|x\right\|=1\right\},}](https://wikimedia.org/api/rest_v1/media/math/render/svg/8c0f03464cae28b6970a0f1e26d458ea9a575adc)

> a [2-sphere](https://en.wikipedia.org/wiki/2-sphere) is an ordinary 2-dimensional [sphere](https://en.wikipedia.org/wiki/Sphere) in 3-dimensional Euclidean space, and is the boundary of an ordinary ball (3-ball).

- [**Graph embedding**](https://en.wikipedia.org/wiki/Graph_embedding)

### 0.1 Problem Formulation

从多个相机对的相对方向（有噪声）中估计绝对位置。希望在相差一个平移和一个放缩的情况下估计出唯一的相机绝对位置。

<img src="pics/image-20210825093444185.png" alt="image-20210825093444185" style="zoom: 67%;" />

输入是 Epipolar Geometry (EG) graph：结点是图像，边表示两个图像的联系（relative translation direction）。

**符号定义**

$R_i$：从世界坐标系到相机坐标系的映射（旋转矩阵）。

$t_i$：世界坐标系下的位置（平移向量）。

$V$：图像集合。

$$e_{ij}=\left(\hat{\mathbf{R}}_{i j}, \hat{\mathbf{t}}_{i j}^{l}\right)$$：边，$l$ 代表相对运动，在局部坐标系下定义的epipolar geomtry。$\hat{}$代表是测量值。

$E=\{e_{ij}\}$：边的集合

$G = (V,E)$：epipolar geometry graph

有：
$$
\begin{aligned} \hat{\mathbf{R}}_{i j} &=\mathbf{R}_{i}^{\top} \mathbf{R}_{j} \\ \lambda_{i j} \hat{\mathbf{t}}_{i j}^{l} &=\mathbf{R}_{i}^{\top}\left(\mathbf{t}_{j}-\mathbf{t}_{i}\right) \end{aligned}
$$
其中$\hat{\mathbf{t}}_{i j}^{l}$是相对平移的单位向量（方向）。

**步骤**

1. Rotation problem：非文章重点
2. 1DSfM 预处理
3. Translation problem

**Translation averaging**

世界坐标系下的相对平移方向：$\hat{\mathbf{t}}_{i j}=\mathbf{R}_{i} \hat{\mathbf{t}}_{i j}^{l}$。图嵌入问题：

<img src="pics/image-20210816121642679.png" alt="image-20210816121642679" style="zoom:50%;" />

### 0.2 1DSfM

1DSfM用于滤除错误的3D方向$\hat{\mathbf{t}}_{i j}$，将3D的**图嵌入**到1D上解决。下图是一个2D->1D的简单示例：

<img src="pics/image-20210816103012531.png" alt="image-20210816103012531" style="zoom: 50%;" />

假设将方向$\hat{\mathbf{t}}_{i j}$投影至$x$轴（同时需要给vertex $i$ 和 $j$ 赋坐标）：
$$
\hat{\mathbf{t}}_{i j} \cdot\langle 1,0,0\rangle=x_{i j}
$$
需要注意的是，$\hat{\mathbf{t}}_{i j}$代表方向，而一维的轴上只有左和右。如果$x_{i j}>0$则 $i$ 应该嵌入在 $j$ 的左边，反之亦然。这样可以确定$i$和$j$在$x$轴上的坐标。

方向向量在一个维度（方向）的投影$\hat{\mathbf{t}}_{i j} \cdot\langle 1,0,0\rangle=x_{i j}$应该符合顺序约束：所有在这个方向上的投影都应该满足“如果$x_{i j}>0$则 $i$ 应该在 $j$ 的左边，反之亦然。”的条件。更准确地说，在方向都正确的情况下，并且假设在输入数据中inlier noise方差较小，outlier noise比例很小的情况下，1D的轴上不应该存在环。如果存在环，则说明构成环的边中存在outlier。这样在1D projection下，问题就变成了一个 寻找[MINIMUM FEEDBACK ARC SET](https://en.wikipedia.org/wiki/Feedback_arc_set)的问题。

注意并不是在每个方向的投影中的 outlier 都不符合顺序约束。即顺序约束这个条件是不含outlier的必要不充分条件。在有inlier 噪声存在的情况下，如果在某个方向上的投影不符合顺序约束，则可能是outlier。因此可以在多个采样方向上进行投影，并计算 outlier 的权值，如果所有采样方向上的outlier权值和大于阈值，则reject。

### 0.3 Solving the Translations Problem

在滤除错误方向后，可以解平移平均的问题，利用LM方法解下面目标函数对应的的非线性最小二乘问题：
$$
\operatorname{err}_{c h}(\mathcal{T})=\sum_{(i, j) \in E} d_{c h}\left(\hat{\mathbf{t}}_{i j}, \frac{\mathbf{t}_{j}-\mathbf{t}_{i}}{\left\|\mathbf{t}_{j}-\mathbf{t}_{i}\right\|}\right)^{2}
$$

$$
d_{c h}(\mathbf{u}, \mathbf{v})=\|\mathbf{u}-\mathbf{v}\|_{2}
$$

其中，$d_{ch}$是平方弦距离（在$S^2$上定义）。

最后进行三角化和全局BA。



### 0.4 Limitations

1. sparsely connected场景：由于MINIMUM FEEDBACK ARC SET问题的性质和inlier/outlier noise的存在，1DSfM 预处理在sparsely connected场景中不准确。另外猜测目标函数中只有针对平移方向的项，如果场景联系稀疏，会导致度量不准确。

   下图是一个sparsely connected场景的重建结果，1DSfM重建失败。这其中三个建筑中，每两个建筑之间的联系都比较微弱。

![image-20210816164700571](pics/image-20210816164700571.png)

2. 对self consistent outliers敏感: ambiguous structures in scene, mirror reflection, etc.

3. 优化时仅考虑到相对平移方向，如何保证结果的稳定性或者唯一性？

   文章Robust Camera Location Estimation by Convex Programming中有相关叙述。

   <img src="pics/image-20210825093736820.png" alt="image-20210825093736820" style="zoom:50%;" />

## 1 Robust Camera Location Estimation by Convex Programming

主要贡献：

1. 使用parallel rigidity定义了translation averaging 问题在什么时候是 well-posedness，即当给定一个问题的时候，在这个问题的限定条件下，我们是否有足够的信息在无噪声的情况下稳定地估计出相机的位置。
2. 对匹配鲁棒的 translation direction 估计方法，对outlier direction鲁棒的位置估计方法 (a convex program)。

### 1.0 Prerequisite

[**Graph rigidity and Parallel Rigidity**](https://alexey.im/papers/graph-rigidity-preprint.pdf)

> The classical graph rigidity problem asks: given a graph G, and a length le for every edge, how can we draw this graph in d dimensions with the specified edge lengths? A graph is rigid if such a valid drawing cannot be continuously transformed into another one, while staying valid at every instant.
>
> Parallel rigidity: Parallel rigidity is analogous, but it fixes edge directions, leaving the lengths to vary freely. 
>
> 一个结论： In two dimensions, parallel rigidity is equivalent to classical rigidity (and both are very well understood).

**[rigidity matroid](https://en.wikipedia.org/wiki/Matroid)**

> In [combinatorics](https://en.wikipedia.org/wiki/Combinatorics), a branch of [mathematics](https://en.wikipedia.org/wiki/Mathematics), a **matroid** [/ˈmeɪtrɔɪd/](https://en.wikipedia.org/wiki/Help:IPA/English) is a structure that abstracts and generalizes the notion of [linear independence](https://en.wikipedia.org/wiki/Linear_independence) in [vector spaces](https://en.wikipedia.org/wiki/Vector_space). 
>
> In the mathematics of [structural rigidity](https://en.wikipedia.org/wiki/Structural_rigidity), a **rigidity matroid** is a [matroid](https://en.wikipedia.org/wiki/Matroid) that describes the number of [degrees of freedom](https://en.wikipedia.org/wiki/Degrees_of_freedom_(mechanics)) of an [undirected graph](https://en.wikipedia.org/wiki/Undirected_graph) with rigid edges of fixed lengths, embedded into [Euclidean space](https://en.wikipedia.org/wiki/Euclidean_space). 用于描述具有固定长度的刚性边的无向图的自由度。
>
>  A set of edges is independent if and only if, for every edge in the set, removing the edge would increase the number of degrees of freedom of the remaining subgraph. 图中的一系列边是independent的，当且仅当对于集合中的每个边，移除该边会提升形成的子图的自由度。

**Framework**

嵌入d维欧式空间中的无向图。从一个n nodes, m edges的framework中我们可以定义一个m行nd列的矩阵，这个矩阵是incidence matrix的一个拓展，称为 rigid matrix。

### 1.1 Well-posedness

**Parallel rigidity问题**

给定一个图G，固定每个边的方向（长度可以任意变化），如何在d维空间中构造这个图？定义：一个图是rigid，当构造出的图不能被连续的变换到另外一个构造出的图中，并且每个构造的图都满足该条件。

**Unique realization 问题**

上述构造出的多个图，如果全等（在相差一个放缩的情况下），则说明有唯一的实现。

对于d维的情况，parallel rigidity 等价于 unique realization。

输入是 Epipolar Geometry (EG) graph：结点是图像，边表示两个图像的联系（ relative translation direction ）。

**三个基本问题**

1. 在没有相对方向噪声的情况下，在图满足什么条件时，才可以稳定地唯一地估计出相机的绝对位置？

2. 如果不能稳定地唯一地估计出相机的位姿，怎么办？

3. 满足了以上条件，但有噪声怎么办？

- 第一个问题可以由parallel rigidity解释

<img src="pics/image-20210825094127429.png" alt="image-20210825094127429" style="zoom:50%;" />

- 第二个问题为寻找最大parallel rigid子图的问题
- 第三个问题在于寻找鲁棒的优化方法和外点（错误的匹配，错误的相对位置）滤除方法

