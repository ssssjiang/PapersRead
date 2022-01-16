# Vins-Mono

![img](https://img-blog.csdn.net/20171130195553578?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2FuZ3NodWFpbHBw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 观测数据预处理

### 图像

- 提取角点，用KLT稀疏光流法跟踪；
- 关键帧策略：
  - 平均视差法；
  - 特征质量法；

### 预积分

预积分部分要计算出IMU观测数据的预积分值，以及残差的协方差矩阵和雅克比矩阵；

> 我理解的，一开始对预积分误差关于时间求导，建立关于时间的线性模型，是因为IMU的频率更高，关键帧之间是多一串前后相关的IMU数据的，对于一个IMU数据，我们建模为高斯白噪声，但是我们还需要知道这个噪声在一串IMU数据里是怎么传播的；

## 初始化

## 局部BA联合优化

### 参数

pose 7维 + (v ba bg)9维 + feature的逆深度 1维 + 相机和IMU之间的外参 7维；

### 步骤

1. add 所有的parameterblock；

2. 初始值放进parameterblock；

3. 如：new一个IMU factor：

   其中包括一个evaluate方法：

   1. param block：一条边对应的状态量；
   2. error-term: residual + 放入初值；
   3. 残差项对应的Jacobian（使用马氏距离）;
   4. 优化完；
   5. marg；

### IMU残差

> 残差项一共五个，每个都是3 x (p v q ba bg)维，即(5 x 3) x (5 x 3)维；

非线性优化的过程中也会计算ba, bg的更新量，但是不会重新积分，而是利用预积分阶段，推导的预积分误差线性模型，更新预积分量，且仅在ba, bg更新量较大时，才重新积分；

### Camera残差

> 残差项是一个，因为是在归一化相机坐标系下相减，其实也就是2维，参数是通过3D点关联的帧的位姿，和点的逆深度；维度是 2 x （7 + 7 +１）？
>
> （有点不太明白，深度和帧是关联的，所以这里的深度是指第一帧看到这个点的深度？）



## 重定位

## 全局图优化

## 回环检测

