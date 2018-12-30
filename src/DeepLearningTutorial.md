---
title: DeepLearningTutorial
date: 2018-2-13 00:46:00
tags: deeplearning
---

## 1.Introduction

1. 通过训练集训练（找一个函数）用于判断测试集
2. framework=a set of function
3. 选择最好的函数=>通过设置起始点及梯度下降，找到最优点（难以保证是全局最优）




## 2.Why Deep

1. 神经网络层数越多，精度越高。（过拟合除外）
2. 模块化设计，高-瘦型比矮-胖型网络更节省神经元





## 3.About Code&Library

1. Keras

   + TensorFlow
   + Theano
   + CNTK

   [more detail](https://keras.io/)

2. Keras相应的[github项目](https://github.com/keras-team/keras/tree/master/examples)和[数据集](https://keras.io/datasets/)

3. 新手Demo：[MNIST](http://wiki.jikexueyuan.com/project/tensorflow-zh/tutorials/mnist_beginners.html)



## 4.Training

1. Tips for training DNN

   1. 选择合理的误差函数，如均方误差、交叉熵等
   2. mini-batch 选择一个合适的batch_size，用不算太大的样本代表总体估计出极值的方向（shuffle保证了样本的随机性）
   3. 选择合适的激发函数，如sigmoid/ReLU/softmax
   4. 合适的learn rate，可以从大到小自适应逐渐精确求解，为了避免plateau和saddle point的影响可以引入momentum

2. 训练集/测试集

   1. 训练集和测试集可能来自不同的source，well-trained的参数可能在test dataset上表现并不好
   2. 防止过拟合可以使用Early Stopping/Dropout

3. CNN(usually for image processing）

   1. 为了减少输入的规模，只取全图的一个小部分（pattern in image），用到了卷积的技术
   2. （卷积-池化）结构可以迭代多层，每次卷积后进行池化都能得到一张新的image，但是规模要小得多
   3. AlphaGo利用5x5卷积

4. RNN（NN with memory)

   1. 可以把之前得到的结果存储在memory中，用于干预之后的结果
   2. LSTM 长短期记忆网络（RNN的升级版）

5. 未来方向

   1. Ultra Deep Network

      由多种形态子网络连接而成，深度很大

   2. Attention Model

      模拟人脑对于当前信息只把attention放在局部，对全局信息加权进行处理

   3. Reinforcement Learning

      从结果的奖励/惩罚中学习知识，达到最优

   4. Unsupervised Learning

      例如把一幅油画的风格运用到照片中

      例如自动补全半张图

      例如机器画漫画

      ​

      ​

   ​

   ​

