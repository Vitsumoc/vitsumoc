---
title: AI入门笔记（3）——神经网络感知器
date: 2023-11-27 15:07:11
categories: 
- AI学习
tags:
- AI
- 笔记
---

# 课程

https://github.com/microsoft/AI-For-Beginners/blob/main/lessons/3-NeuralNetworks/03-Perceptron/README.md

这是微软提供的AI-For-Beginners课程第三课，介绍了什么是神经网络中的感知器（Perceptron）

<!-- more -->

# 内容

感知器```Perceptron```是一种二元分类模型，总是能根据输入产生一个+1或-1的输出。

感知器进行计算时需要权重```weight```的参与，权重会导致感知器产生正确或错误的结果，训练的过程既是修改权重不断增加结果的正确率。