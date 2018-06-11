 百度—西交大 商家招牌的分类与检测 初赛
 
 运行环境 Ubuntu 16.04    Torch7 CUDA Version 8.0.61

 硬件 Titan X（Pascal Architecture）
 Final accuracy 99.3% on a single model
 ----------------------
 网络调整
 
 本次比赛我们用的网络是Facebook 在ImageNet 上预训练的 ResNeXt 101 64x4d
 为与比赛要解决的问题相适应我们对原始网络做了如下改动:

 1、将所有的 ReLU激活层改为LeakyReLU(0.1)   扩大激活层可以传播梯度的范围
 2、将7x7 SpatialAveragePooling替换为 SpatialAdaptiveAveragePooling使得网络可以以更高分辨率的图像作为输入
 3、由于本问题为100类的分类问题，所以将2048->1000 全连接层 2048->100 全连接层

 ----------------------
 同时对原始数据我们做了如下数据增强：

 1、将原始图片补充值为0 的像素点，使图片成为正方形，目的是为了保证图片的文字不变形。
 2、顺时针及逆时针30度的旋转。
 3、颜色及锐度的增强。

 ----------------------
 
 各个 .lua 文件功能的解释 (按运行次序)

 replaceLeakyReLU.lua  调整网络结构

 ImagePadding.lua   将原始图片补充值为0 的像素点，使图片成为正方形
  
 loader.lua    将训练集随机拆分为训练集与验证集，训练集及验证集图片数量比为4:1，并将训练集与测试集<图片名,标定>存为.t7文件

 create_weight.lua  统计训练集中的不同类别图像个数，以此确定交叉熵损失的权重，进一步平衡数据不均（虽然本数据数据分布较为均衡）

 main.lua 图片旋转，颜色及锐度图像增强，训练并存储网络模型，网络采用sgd来优化参数，初始学习率为0.01，学习率衰减为0.0004，当学习率小于0.0005时，学习率保持不变

 ResNet.lua  构造网络，在这里只是用于加载改好的预训练网络

 main_test.lua 在测试集上运行网络并生成csv文件 

----------------------------
 各文件夹功能

 cache 储存<图片名,标定> .t7文件，以及交叉熵损失权重

 datasets 储存padding后的数据集

 TrainedModel 储存训练得到的模型权重

百度云链接：https://pan.baidu.com/s/14pLSriTQhwH6rby74TT-lA 密码：888p
 
