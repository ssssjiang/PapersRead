# LOAM论文阅读笔记

Ji, Z. , and  S. Singh . "LOAM: Lidar Odometry and Mapping in Real-time." Robotics: Science and Systems Conference 2014.

原文链接：【2014】LOAM: Lidar Odometry and Mapping in Real-time：https://www.ri.cmu.edu/pub_files/2014/7/Ji_LidarMapping_RSS2014_v8.pdf

## 开源代码

开源代码：https://github.com/topics/loam  代码较古老，未使用eigen，ceres等第三方库，不推荐阅读

自动求导：https://github.com/HKUST-Aerial-Robotics/A-LOAM ceres使用自动求导方式，效率较低

解析求导：https://github.com/wh200720041/floam ceres使用解析求导方式，效率较aloam高了许多

q: 此处的一个疑问： aloam和floam都是基于loam改写整理得到的，但kitti数据集上的评分，都较loam低许多，不知原因为何。

## 论文笔记

loam在没有配置高准确性的激光雷达和惯导的情况下，能够同时达到低漂移和和低计算复杂性。主要思想是复杂的同步定位与建图问题的分离，通过两个算法来进行大量变量的同步优化。算法1作为里程计，以一种高频但是低准确度的方式来估计雷达的速度。算法2以另一种较低数量级的频率运行，用于和点云进行一个较好的配准。两种方法的融合，允许算法能够实时进行建图。算法通过大量实验和KITTI数据集进行评价。结果表明，算法能够达到批量处理方法的水平。

- 介绍
- 相关工作
- 符号和任务描述
- 系统概述
  - 雷达硬件
  - 软件系统概述
- 雷达里程计
  - 特征点提取
  - 特征点关联
  - 运动估计
  - 雷达里程计算法
- 雷达建图



- 实验
  - 室内&室外实验
  - IMU的对算法的提升
  - 利用KITTI数据集测试
  
    
  
- 结论和未来工作展望

  添加闭环。与IMU通过kalman进行融合，以便进一步降低位姿漂移。