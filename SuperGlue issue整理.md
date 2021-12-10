# SuperGlue issue整理

#SuperGlue 

----------------------

隔一段时间就去看看别人的issue，说不定有我需要的。

[Question about superPoint keypoint count for Indoor/Outdoor image sets #3](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/3)

[About Sinkhorn algorithm use #4](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/4)

[Slack variables issue #6](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/6)

[Some details about homography trained models #16](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/16) 关于为什么不使用MMA评估

[Question regarding recoverPose #19](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/19)

:star: [Question about optimal transport training #24](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/24)

:star: :star:[Details for reproducing the indoor pose estimation experiment #31](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/31)

[I wonder how to perform the homography pretraining? Can it be a perspective transformation? #92](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/92)



[Adding random keypoints during training #90](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/90)

-- 提问的人在训练期间通过添加随机superpoint点来构建batch，提问了在ScanNet上的`训练细节`。

[is there some thing with your code ”compute_epipolar_error“? #84](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/84)

[Overfitting during training on MegaDepth #81](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/81)

[Question about training with Oxford and Paris dataset #79](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/79)

[The usage of 'bin_score' and 'match_threshold'? #78](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/78)

:star:[indoor dataset #62](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/62)

[How is the camera Intrinsic matrix in the yfcc_test_pairs_with_gt.txtfile obtained? #55](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/55)

:star: [Details for training on Megadepth #54](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/54)

:star: [关于原始论文中亚琛数据集结果的问题。 #52](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/52)

**！！！大惊喜！！D2Net提供训练的pipeline好家伙！！**

[About the pose of ScanNet #41](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/41)  using SUN3D dataset(preprocessed by OANet) and Megadepth(preprocessed by D2Net)

:star: [Have you compared your method with graph-match method? #38](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/38) 其他的图匹配算法

[Question about phototourism test pairs #27](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/27)

[Some details about homography trained models #16](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/16)



-----------------------------

:cherries: superGlue是以SuperPoint为输入训练的，所以它的效果和SuperPoint是强相关的，如果我们想要组合SuperPoint以外的特征进行训练，还是需要自己训练SuperGlue的模型。

--[Does the descriptor types limit the performance of SuperGlue? #9](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/9)

--------------------



## 并行处理

### 关于使SuperGlue可以按batch推断：

- 提问batch训练的话，如果图像提取的特征不够，应该填充什么样的特征和描述子，是否参与匹配和loss计算？

  [Question about superPoint keypoint count for Indoor/Outdoor image sets #3](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/3)

  -- 作者的意思是，我们生成一个随机keyPoint位置，给他们检测置信度设置为0，然后从这些位置的semi-dense descriptor map中重新采样描述子，这些会包括在`匹配`和`loss`里，这使得superGlue对误匹配更鲁棒（这是针对训练的做法），我不知道对于测试是不是也是这样。

--------------

- 测试环节batch推断，对填充的特征的处理：[batch mode on match_pairs.py? #25](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/25) 

  -- 如果测试期间加入的虚拟点如果参与匹配，将改变图的结构，为了避免匹配这些虚拟点，需要防止网络的所有层和Sinkhorn层关注它们，可以通过屏蔽注意力权重完成，在softmax之前将所有分数设置为-inf；

  See the `attn_mask` argument in https://pytorch.org/docs/stable/generated/torch.nn.MultiheadAttention.html

---------------------

- :star: [CUDA out of memory when sinkhorn_iterations == 100? #51](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/51) **code**

  提问者给出了一个批处理的实现。

----------------------

- [Is it possible to run with batch size > 1 #64](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/64)  

  提问者测试发现，批处理要求的keypoints点数相同，只要求keypoints0 和 keypoints1在内部点数相同，但是keypoints0 和 keypoints1可以点数不同。

  （但是方便起见，还是设置成点数相同吧）



### 单机多卡



## 训练细节

- [Question about test result of my train model on InLoc dataset. #103](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/103)

  提问人已经复现了SuperGlue训练代码，并且同样是在MegaDepth数据集上训练的，但是结果和原版有差异，提问有没有训练技巧。`未答复`

  作者回复关于重定位测试，在[比赛网站](https://www.visuallocalization.net/)上，他们还给出了针对每个数据集的更多细节。

---------------------

- :star:[SuperPoint model sample descriptor #18](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/18)

  关于训练期间对superpoint插值。

## 多平台部署

- [C++ Inference with MNN #102](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/102)

  **[MNNSuperGlue](https://github.com/Hanson0910/MNNSuperGlue)**

  楼主是个狠人，把superglue（test）在阿里开源的MNN框架上，用C++复现了一遍。

-----------------

- :cherries: 如果部署到C++上运行，1. 会更快；2. 生产环境：

  [mismatch problem in c++ #23](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/23) [Can you provide pt file #21](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/21) C++包装过的SuperGlue；

  :crying_cat_face: 23中给出的答案，我已经ctrl v到superglue_catkin中了，谢谢他！！

---------------------

- [how can i convert pth models to onnx models? #99](https://github.com/magicleap/SuperGluePretrainedNetwork/issues/99)

  热心人给了实现。

----------------

