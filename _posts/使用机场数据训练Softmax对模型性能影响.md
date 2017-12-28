---
title: 使用机场数据训练Softmax对模型性能影响
date: 2017-09-27 10:05:34
tags: work Softmax 机场 模型
---

# 说明

增加机场数据约50万ID，结合证件/现场标签训练Softmax（Softmax分数为判断两张照片是否为同一个人的概率），分别在SET测试集，BIGDATA10万干扰库测试集，Airport5k/12k（自建）验证模型识别率。

结果表明：增加数据进行fintun，训练Softmax时证件/现场照权值共享，且PLDA分数与Softmax分数融合的识别率最佳，接近不加数据Resnet_152 PLDA结果。自建测试集上新增数据效果提升显著。

具体说明如下：

# 一、模型训练信息
![](https://i.imgur.com/GFtUkGe.jpg)

# 二、测试结果

注：绿色为仅用机场数据重新训练的模型，蓝色为权值共享Fintun模型PLDA/Softmax识别率，红色为融合识别率。其中，SET3、BIGDATA集Softmax和融合识别率仍在测试。
![](https://i.imgur.com/08PSsp7.jpg)
  ![](https://i.imgur.com/RLagtvc.jpg)
# 三、结论与分析

1.仅使用机场数据重新训练，在自建测试集效果较好，在SET测试集效果非常差。

分析原因：数据分布过于单一，模型过拟合严重，鲁棒性较差。

2.使用权值共享方式或采取参数分支Fintun训练Softmax效果差别不大，PLDA在SET测试集上与初始模型（baseline）接近甚至有些微提升，在自建测试集上远高于baseline。

分析原因：总体数据更具多样性，且Fintun使模型倾向于机场数据分布。

3.仅看Softmax得分，识别效果普遍较低，且低不少。

分析原因：Softmax泛化能力较差，对于未见过数据学习较差或训练数据单一，鲁棒性差，场景变换等引起变化对模型影响大。

4.Softmax+PLDA分数融合识别率对于baseline有较大幅度提升（4~15个百分点），SET测试集上接近Resnet_152的效果，自建测试集上超过Resnet_152。

分析原因：Softmax与PLDA两种得分具有较好互补性。一方面，PLDA具有很好的泛化性、鲁棒性；另一方面，Softmax具有很强的机场数据倾向性。

从而使模型在保持SET测试集较高的识别率下，对机场测试集有很好的表现。

# 四、下一步计划

1.尝试新增数据验证数据单一性/多样性对模型影响。

2.尝试提升Softmax分数。

3.优化Softmax测试速度。
