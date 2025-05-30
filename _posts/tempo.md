---
layout: post
title: TEMPO: Prompt-based Generative Pre-trained Transformer for Time Series Forecasting
date: 2024-05-19 00:13:14
description: 
tags: note
categories: fsc-note
tabs: true
---

# TEMPO: Prompt-based Generative Pre-trained Transformer for Time Series Forecasting

## 1. 研究背景/动机
略

## 2. 创新点

## 3. 主要方法
![TEMPO模型架构图](/assets/img/pic/tempo/structure.jpg)
本文采用GPT作为backbone进行时间序列预测，引入提示工程帮助模型更好地适配来自不同领域的非平稳时序数据。时间序列被解耦为趋势项、季节项和残差项，并映射到相应的隐藏空间，构建GPT能够识别的输入。

### 时间序列解耦
- **趋势项**：通过计算指定滑窗内的均值得到。
- **周期项**：原序列减去趋势项后，使用Loess smoother计算得到。
- **残差项**：原序列减去趋势项和周期项后得到。

### 模型建模流程
使用patchTST架构，首先对周期项进行instance normalization，然后进行patching，随后送入embedding进行编码。

![patchTST架构图](/assets/img/pic/tempo/patchtst.jpg)

### Prompt设计
构建了一个prompt池，池中保留不同键值对，相似的输入时间序列倾向于从集合中检索出同一组提示词。采用得分匹配机制，将原始序列的embedding与键值对中的键进行相似度计算，选择k个最匹配的键，与原始序列的embedding拼接形成最终输入。

### GPT预测模型
使用预训练的GPT-2进行预测，有两种模型形式：一个是将处理后的趋势项、周期项和残差项拼接后输入GPT；另一个是独立地模型趋势项、周期项和残差项。GPT块内部在训练过程中冻结了前馈层，只更新位置嵌入层和层归一化层的梯度。采用LORA进行微调，以适应不同的时间序列分布。输出结果通过反归一化后累加以得到最终的预测值。

## 4. 数据集
还是很经典的那几个

## 5. 实验结果
效果还不错，具体见原文。

## 6. 实验环境
未开源，这里方法简单未复现