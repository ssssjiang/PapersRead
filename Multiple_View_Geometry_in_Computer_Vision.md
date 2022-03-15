# Multiple_View_Geometry_in_Computer_Vision

## 附录

### Gaussian (Normal) and χ2 Distributions

#### A2.1 Gaussian probability distribution

Given a vector $\mathbf{X}$ of random variables $x_{i}$ for $i=1, \ldots, N$, with mean $\overline{\mathbf{X}}=E[\mathbf{x}]$, where $E[\cdot]$ represents the expected value, and $\Delta \mathbf{X}=\mathbf{X}-\overline{\mathbf{X}}$, the covariance matrix $\Sigma$ is an $N \times N$ matrix given by
$$
\Sigma=E\left[\Delta \mathbf{X} \Delta \mathbf{X}^{\top}\right]
$$
其中，$\sum_{i j}=E\left[\Delta x_{i} \Delta x_{j}\right]$，协方差矩阵是对称、正定的；

- 如果变量服从高斯分布：$P(\overline{\mathbf{x}}+\Delta \mathbf{x})=(2 \pi)^{-N / 2} \operatorname{det}\left(\Sigma^{-1}\right)^{1 / 2} \exp \left(-(\Delta \mathbf{x})^{\top} \Sigma^{-1}(\Delta \mathbf{x}) / 2\right)$，exp前面的因子只是使分布的 总积分等于1的归一化因子；

- 各向同性高斯分布：$P(\mathbf{x})=(\sqrt{2 \pi} \sigma)^{-N} \exp \left(-\sum_{i=1}^{N}\left(x_{i}-\bar{x}_{i}\right)^{2} / 2 \sigma^{2}\right)$，$\Sigma=\sigma^{2} \mathrm{I}$

  -- 这种情况下，X处的PDF就是点到均值的欧氏距离的函数；

- 马氏距离：$\|\mathbf{X}-\mathbf{Y}\|_{\Sigma}=\left((\mathbf{X}-\mathbf{Y})^{\top} \Sigma^{-1}(\mathbf{X}-\mathbf{Y})\right)^{1 / 2}$，$P(\mathbf{x}) \approx \exp \left(-\|\mathbf{x}-\overline{\mathbf{x}}\|_{\Sigma}^{2} / 2\right)$

  -- 一般情况下，X处的PDF是点到均值的马氏距离的函数；

- 坐标变化：$\begin{aligned} \exp \left(-(\mathbf{x}-\overline{\mathbf{X}})^{\top} \Sigma^{-1}(\mathbf{x}-\overline{\mathbf{X}}) / 2\right) &=\exp \left(-\left(\mathbf{x}^{\prime}-\overline{\mathbf{x}}^{\prime}\right)^{\top} U \Sigma^{-1} U^{\top}\left(\mathbf{x}^{\prime}-\overline{\mathbf{x}}^{\prime}\right) / 2\right) \\ &=\exp \left(-\left(\mathbf{x}^{\prime}-\overline{\mathbf{x}}^{\prime}\right)^{\top} D^{-1}\left(\mathbf{x}^{\prime}-\overline{\mathbf{X}}^{\prime}\right) / 2\right) \end{aligned}$

  其中，$\Sigma=\mathrm{U}^{\mathrm{T}} \mathrm{DU}$和$\mathrm{D}=\left(\sigma_{1}^{2}, \sigma_{2}^{2}, \ldots, \sigma_{N}^{2}\right)$，$\mathbf{X}^{\prime}=U X$ and $\bar{X}^{\prime}=U \bar{X}$

  -- $\mathbf{X}$ to $\mathbf{X}^{\prime}=\mathbf{U X}$是把一个一般的高斯PDF的协方差变换为具有对角协方差矩阵的，如过在利用D进行尺度变换，可以进一步将它变换为一个各向同性的高斯分布；

  -- 即用坐标变换可以把马氏距离变换成普通的欧氏距离；

### Parameter Estimation

### Matrix Properties and Decompositions

#### 正交矩阵

- 它的转置等于逆：$U^{\top} U=I$
- 列向量都有单位范数，且正交：$\mathbf{u}_{i}^{\top} \mathbf{u}_{j}=\delta_{i j}$；
- $\operatorname{det} \mathrm{U}=\pm 1$；
- n维正交矩阵形成一个群$S O_{n}$，其中的元素叫n维旋转；
- 保范性：一个向量乘以一个正交矩阵，范数不变；
- QR分解（类似的还有RQ、QL、LQ分解），Q指正交矩阵，R是上三角矩阵，直接法求P后续就需要用RQ分解P，求内参矩阵和旋转矩阵；



#### RQ分解

三个旋转矩阵：$\mathbf{Q}_{x}=\left[\begin{array}{ccc}1 & & \\ & c & -s \\ & s & c\end{array}\right] \quad \mathbf{Q}_{y}=\left[\begin{array}{ccc}c & & s \\ & 1 & \\ -s & & c\end{array}\right] \quad \mathbf{Q}_{z}=\left[\begin{array}{ccc}c & -s & \\ s & c & \\ & & 1\end{array}\right]$

> 三个旋转矩阵的组合还可以和欧拉角对应；

如：右乘Qz的效果是，使第三列不变，前两列用原来两列的线性组合代替；

线性组合中，我们又可以通过设定c和s的角度参数$c=\cos (\theta)$ and $s=\sin (\theta)$是某个位置为0；

如：$\mathrm{A}_{21}$ to zero, 需要$c a_{21}+s a_{22}=0$，可以使$c=-a_{22} /\left(a_{22}^{2}+a_{21}^{2}\right)^{1 / 2}$和$s=a_{21} /\left(a_{22}^{2}+a_{21}^{2}\right)^{1 / 2}$；（要满足三角函数的约束）

- RQ 分解的步骤：

  > Objective Carry out the RQ decomposition of a 3 × 3 matrix A using Givens rotations. Algorithm:
  >
  > (i) Multiply by Qx so as to set A32 to zero.
  >
  > (ii) Multiply by Qy so as to set A31 to zero. This multiplication does not change the second column of A, hence A32 remains zero. 
  >
  > (iii) Multiply by Qz so as to set A21 to zero. The first two columns are replaced by linear combinations of themselves. Thus, A31 and A32 remain zero.

  结果：$\mathrm{AQ}_{x} \mathrm{Q}_{y} \mathrm{Q}_{z}=\mathrm{R}$，$\mathrm{A}=\mathrm{RQ}_{z}^{\top} \mathrm{Q}_{y}^{\top} \mathrm{Q}_{x}^{\top}$

#### Householder matrices and QR decomposition

`留`



#### 对称矩阵和反对称矩阵

反对称矩阵：$\mathrm{A}^{\top}=-\mathrm{A}$

性质：

- 特征值分解：

  - 实对称矩阵可以分解为$\mathrm{A}=\mathrm{UDU}^{\top}$，U是正交矩阵，D是实对角阵，因此一个是对称矩阵有实特征值，且特征向量两两正交；
  - 如果S是反对称矩阵，$\mathrm{S}=\mathrm{UBU}^{\top}$，其中B是分块对角矩阵：$\operatorname{diag}\left(a_{1} \mathrm{Z}, a_{2} \mathrm{Z}, \ldots, a_{m} \mathrm{Z}, 0, \ldots, 0\right)$, where $\mathrm{Z}=\left[\begin{array}{cc}0 & 1 \\ -1 & 0\end{array}\right]$，S的特征向量是纯虚数，且奇数阶的反对称矩阵必是奇异的；

- 叉乘：$[\mathbf{a}]_{\times}=\left[\begin{array}{ccc}0 & -a_{3} & a_{2} \\ a_{3} & 0 & -a_{1} \\ -a_{2} & a_{1} & 0\end{array}\right]$，$\mathrm{a}=\left(a_{1}, a_{2}, a_{3}\right)^{\top}$，a就是$[\mathbf{a}]_{\times}$的零向量；

  叉乘和反对称矩阵的关系是：$\mathbf{a} \times \mathbf{b}=[\mathbf{a}]_{\times} \mathbf{b}=\left(\mathbf{a}^{\top}[\mathbf{b}]_{\times}\right)^{\top}$

- 余因子和伴随矩阵：$M^*$是方阵M的余因子，${M^*}_{ij}=(-1)^{i+j} \operatorname{det} \hat{\mathrm{M}}_{i j}$，$\hat{\mathrm{M}}_{i j}$是M去掉第i行和第j列剩余的部分，余因子矩阵的转置是M的伴随矩阵：$\operatorname{adj}(\mathrm{M})$；

  - 如果M是可逆的，$\mathrm{M}^{*}=\operatorname{det}(\mathrm{M}) \mathrm{M}^{-\mathrm{T}}$；
  - 但不管M是否可逆：$\operatorname{adj}(M) M=M \operatorname{adj}(M)=\operatorname{det}(M) I$；
  - 如果M是3x3的矩阵，x y是两个列向量：$(\mathrm{Mx}) \times(\mathrm{My})=\mathrm{M}^{*}(\mathbf{x} \times \mathbf{y})$，也可以写成：$[\mathrm{Mx}]_{\times} \mathrm{M}=\mathrm{M}^{*}[\mathbf{x}]_{\times}$；
    - 令$\mathrm{t}=\mathrm{Mx}$，公式可以写成：$[\mathbf{t}]_{\times} \mathrm{M}=\mathrm{M}^{*}\left[\mathrm{M}^{-1} \mathbf{t}\right]_{\times}=\mathrm{M}^{-\mathrm{T}}\left[\mathrm{M}^{-1} \mathbf{t}\right]_{\times}($up to scale $)$

- 3x3的反对称矩阵性质：在相差一个尺度因子的情况下：$[\mathbf{a}]_{\times}=[\mathbf{a}]_{\times}[\mathbf{a}]_{\times}[\mathbf{a}]_{\times}$(including scale $\left.[\mathbf{a}]_{\times}^{3}=-\|\mathbf{a}\|^{2}[\mathbf{a}]_{\times}\right)$

#### 正定对称矩阵

正定实对称矩阵性质：

- 一个对称矩阵A是正定的充分必要条件是：$\mathbf{x}^{\top} \mathbf{A x}>0$
- 一个正定对称矩阵A可以唯一的分解为$A=K K^{\top}$，其中K是对角元素全为正的上三角矩阵；



#### 旋转矩阵的表示

##### 3维中的旋转

- 形式：$e^{[\mathbf{t}]_{\times}}$

- The matrix $e^{[\mathbf{t}]_{\times}}$ is a rotation matrix representing a rotation through an angle  t about the axis represented by the vector t.

- $[\mathbf{t}]_{×}^{2}=\mathbf{t} \mathbf{t}^{\top}-\|\mathbf{t}\|^{2} \mathrm{I}$

  $\begin{aligned}
  e^{[\mathbf{t}]_{\times}} &=I+[\mathbf{t}]_{\times}+[\mathbf{t}]_{\times}^{2} / 2 !+[\mathbf{t}]_{\times}^{3} / 3 !+[\mathbf{t}]_{\times}^{4} / 4 !+\ldots \\
  &=I+\|\mathbf{t}\|[\hat{\mathbf{t}}]_{\times}+\|\mathbf{t}\|^{2}[\hat{\mathbf{t}}]_{\times}^{2} / 2 !-\|\mathbf{t}\|^{3}[\hat{\mathbf{t}}]_{\times} / 3 !-\|\mathbf{t}\|^{4}[\hat{\mathbf{t}}]_{\times}^{2} / 4 !+\ldots \\
  &=I+\sin \|\mathbf{t}\|[\hat{\mathbf{t}}]_{\times}+(1-\cos \|\mathbf{t}\|)[\hat{\mathbf{t}}]_{\times}^{2} \\
  &=I+\operatorname{sinc}\|\mathbf{t}\|[\mathbf{t}]_{\times}+\frac{1-\cos \|\mathbf{t}\|}{\|\mathbf{t}\|^{2}}[\mathbf{t}]_{\times}^{2} \\
  &=\cos \|\mathbf{t}\| \mathrm{I}+\operatorname{sinc}\|\mathbf{t}\|[\mathbf{t}]_{\times}+\frac{1-\cos \|\mathbf{t}\|}{\|\mathbf{t}\|^{2}} \mathbf{t t}^{\top}
  \end{aligned}$

  > 如果轴和角分离，||t|| = 角度，t仅表示方向，模长为1，上面的式子会简化很多；

  $\eta=\cos \frac{\phi}{2}, \quad \varepsilon=\mathbf{a} \sin \frac{\phi}{2}=\left[\begin{array}{l}a_{1} \sin (\phi / 2) \\ a_{2} \sin (\phi / 2) \\ a_{3} \sin (\phi / 2)\end{array}\right]=\left[\begin{array}{l}\varepsilon_{1} \\ \varepsilon_{2} \\ \varepsilon_{3}\end{array}\right]$

  再加上上面的式子，轴角表达 & 李代数 & 旋转矩阵，就关联了起来；

- 从旋转矩阵提取轴角：

  - 旋转轴：求解$(\mathrm{R}-\mathrm{I}) \mathrm{v}=0$，单位旋转轴v可以通过求相应的单位特征值的特征向量得到；

  - 角：$\begin{aligned} 2 \cos (\phi) &=(\operatorname{trace}(\mathrm{R})-1) \\ 2 \sin (\phi) \mathbf{v} &=\left(\mathrm{R}_{32}-\mathrm{R}_{23}, \mathrm{R}_{13}-\mathrm{R}_{31}, \mathrm{R}_{21}-\mathrm{R}_{12}\right)^{\top} \end{aligned}$

    > 第二个式子令$2 \sin (\phi) \mathbf{v}=\hat{\mathbf{v}}$，则$2 \sin (\phi)=\mathbf{v}^{\top} \hat{\mathbf{v}}$，角度可以从cos和sin中，用atctan函数计算；
    >
    > 有点人会用反余弦或反正弦来计算，但这种方法在数值上不是精确的，并且在pi位置求不到轴；

  - 如果写成单位轴向量和角度，和Rodrigues是重合的：$\mathrm{R}(\theta, \hat{\mathbf{t}})=\mathrm{I}+\sin \theta[\hat{\mathbf{t}}]_{\times}+(1-\cos \theta)[\hat{\mathbf{t}}]_{\times}^{2}$

    > 给出了轴角（李代数）表示的旋转，应用到某个向量x上，不必一定要构造矩阵表示，可以用Rodrigues公式；

##### 四元数

- 形式：（Hamiton）$\mathbf{q}=(\mathbf{v} \sin (\theta / 2), \cos (\theta / 2))^{\top}$

  这个表达式，代表关于旋转轴v，转过$\theta$；

- -q和q表示的是同一个旋转；



#### SVD分解



### Least-squares Minimization



### 迭代估计方法

#### Newton iteration



 #### 参数化法

- 如何产生一个好的参数化：

  - 参数化方法应该是局部连续的、可微和一对一的，总之是微分同胚的；

    > 欧拉角的旋转表示中，存在奇异点，且存在非常接近的点，在参数空间却相距非常远；

- 3D旋转的表示方法：

  轴-角表示：$\mathrm{R}(\theta, \hat{\mathbf{t}})$

  > 可以理解成李代数的方法吗？

  - 无旋转由$\mathbf{t} = 0$表示；
  - 逆旋转由-t表示，用符号$R(\mathbf{t})^{-1}=R(-\mathbf{t})$表示；
  - 如果是小t，那么旋转矩阵可以由$\mathrm{I}+[\mathbf{t}]_{\times}$逼近；
  - 对由t1和t2表示的小旋转，组合旋转在一阶精度上用$\mathbf{t}_{1}+\mathbf{t}_{2}$表示，换句话说$\mathrm{R}\left(\mathbf{t}_{1}\right) \mathrm{R}\left(\mathbf{t}_{2}\right) \approx \mathrm{R}\left(\mathbf{t}_{1}+\mathbf{t}_{2}\right)$，因此，对于一个小t，映射$\mathbf{t} \mapsto \mathrm{R}(\mathbf{t})$是一个群同构，事实上对于小t，此映射是一个等距变换，两个旋转之间的距离定义为旋转$\mathrm{R}_{1} \mathrm{R}_{2}^{-1}$的角度；
  - Any rotation may be represented as R(t) for some t such that $\|\mathbf{t}\| \leq \pi$. That is, any rotation is a rotation through an angle of at most π radians about some axis. The mapping$\mathbf{t} \mapsto \mathrm{R}(\mathbf{t})$is one-to-one for $\|\mathbf{t}\| < \pi$ and two-to-one for $\|\mathbf{t}\| < 2\pi$. If  $\|\mathbf{t}\| = 2\pi$, then R(t) is the identity map, regardless of t. Thus, the parametrization has a singularity at  $\|\mathbf{t}\| = 2\pi$.
  - 归一化：最好是使用$\|\mathbf{t}\| < \pi$，以便避开奇点，如果$\|\mathbf{t}\|>\pi$，可以用向量$(\|\mathbf{t}\|-2 \pi) \mathbf{t} /\|\mathbf{t}\|=\mathbf{t}(1-2 \pi /\|\mathbf{t}\|)$来表达相同的旋转；

- 齐次向量的参数化方法：

  如：四元数表达是一个冗余自由度的表达；

- n维球面的参数化方法

  通常在几何优化问题中，参数的某向量被要求在一个单位球上，比如两视图的BA，待估计参数是旋转R、位移t和点X，要被最小化的代价是相对图像测量的投影的几何残差问题，因为存在全局尺度多义性，可以通过要求$\|\mathbf{t}\|=1$解；

  > 但是后面带约束的优化做法，就看不懂了...