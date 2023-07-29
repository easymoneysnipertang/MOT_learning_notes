# TransMOT
> TransMOT: Spatial-Temporal Graph Transformer for Multiple Object Tracking

## Abstract
提出TransMOT，利用graph transformer有效建模对象间空间和时间上的交互  
> TransMOT effectively models the interactions of a **large number** of objects by arranging the trajectories of the tracked objects as a set of **sparse weighted graphs**, and constructing **a spatial graph transformer encoder layer**, **a temporal transformer encoder layer**, and **a spatial graph transformer decoder layer based on the graphs**.

为了提高跟踪速度和精度，提出一种级联关联框架，可以处理低置信度检测和长时间遮挡，但需要大量的计算资源  
模型在MOT15、MOT16、MOT17、MOT20都是SOTA  

## Introduction
> There are two core tasks in this framework: accurate object detection and robust target association.(本文主要关注后者)  

Transformer的成功提出了一种通过自注意力机制建模时间依赖性的新范式  
作者提出现有Transformer方法（TrackFormer、TransTrack）三个缺陷：
1. 未考虑大量对象的时空结构 （*只能说没有直接地考虑，其实在decoder的query-key机制处，一个query代表一个object，帧间不断传输其实也考虑了*）
2. 需要大量计算资源和数据
3. 基于DETR的方法并没有达到SOTA

在TransMOT中，所有跟踪目标的轨迹都排列为一系列稀疏加权图，这些图是使用目标之间的空间关系构建的  
基于稀疏图，TransMOT构建了之前提到的3层编/解码器以对时空关系进行建模  
由于图的稀疏性，模型训练和推理效率更高  

![TransMOT](res/TransMOT.png)

另外，文章还提出了一种级联关联结构，通过将TransMOT整合到该框架中，无需额外学习关联部分  
作者最后挖坑，TransMOT还可以与其他检测器或者特征提取子网络结合，形成统一的端到端解决方案  

## Overview
![overview](res/TransMOT_1.png)

框架主要包含两部分：
- 检测和特征提取子网络
- 时空graph transformer关联自网络

在每一帧，检测和特征提取子网络（蓝色部分）产生M_t个候选检测目标建议，这个建议的集合记为O_t，另外这个网络也计算每个建议的视觉特征  
时空graph transformer为每个tracklet找到最佳建议，并检测特殊事件，例如遮挡、退出、进入  

如何找到每个tracklet的最佳匹配？视作一个优化问题，设计了一个affinity function，估计相似程度  
> 还是类似于一个二部匹配，tracklet就是query，最小化cost，cost是所有匹配affinity的加权和  
> 具体数学公式就不细看了

用稀疏带权图表示目标间的关系，并以此构造了高效的Transformer  
对跟踪对象和新生成的目标之间的关系（特殊事件）进行建模，它会生成一个分配矩阵At  
分配矩阵用于更新跟踪目标，而特殊事件由后续处理模块处理

## TransMOT
时空Transformer（TransMOT）使用当前帧的稀疏带权图θ^t和之前T帧的E^t-1来建模时空相关性并生成映射矩阵A^t  

### 1. Spatial-Temporal Graph Transformer Encoder
#### Spatial Graph Transformer Encoder Layer
#### Temporal Transformer Encoder Layer

### 2. Spatial Graph Transformer Decoder

### 3. Training

### 4. Cascade Association Framework

## Experiments