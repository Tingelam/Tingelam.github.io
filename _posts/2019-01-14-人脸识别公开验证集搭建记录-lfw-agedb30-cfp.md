---
title: 人脸识别公开验证集搭建记录-lfw-agedb30-cfp
published: true
layout: post

---

[TOC]

#  人脸识别公开验证集搭建记录-lfw-agedb30-cfp

需求：方便验证模型泛化能力，参考Arcface[^1]验证集使用方案，使用我们人脸对齐数据，搭建lfw/agedb30/cfp等标准验证集。

# 验证集数据信息

| Val_set | ID_num | Details（均为10次验证求平均）                                          |
| :-----: | :----: | :----------------------------------------------------------- |
|   LFW[^2]   | 6000对 | 共5749个ID，13233张不同姿态、表情的图片。参考unrestricted with labelled outside data的准则，此处验证集使用官方提供的6000对ID数据。 |
| Agedb[^3] | 6000对 | 共440个ID，12240张不同姿态、表情、**年龄**、**性别**的图片。同一个ID中，最大最小年龄差分别为3岁和101岁，所有ID的平均年龄为49岁。根据不同的年龄差把所有数据划分为4个年龄段（年龄差5岁、10岁、20岁以及30岁）。其中每个年龄段的数据包括300对正样本、300对负样本。此处验证集**使用年龄差为30**的数据，命名为agedb30。 |
| CFP_FF/CFP_FP[^4] | 7000对 | 共500个ID，**每个ID包括10张正脸图和4张侧脸图**。评测标准包括正脸照-与正脸照比对，正脸照与侧脸照比对两种。其中每种又包括350对正样本，350对负样本。 |







[^1]: Deng, Jiankang, Jia Guo, and Stefanos Zafeiriou. "Arcface: Additive angular margin loss for deep face recognition." *arXiv preprint arXiv:1801.07698* (2018).
[^2]: G. B. Huang, M. Ramesh, T. Berg, and E. Learned-Miller.

Labeled faces in the wild: A database for studying face
recognition in unconstrained environments. Technical report, Technical Report 07-49, University of Massachusetts,
Amherst, 2007. 

[^3]: J. Deng, Y. Zhou, and S. Zafeiriou. Marginal loss for deep face recognition. In CVPRW, 2017.

[^4]: S. Sengupta, J.-C. Chen, C. Castillo, V. M. Patel, R. Chellappa, and D. W. Jacobs. Frontal to profile face verification
in the wild. In WACV, pages 1–9, 2016.