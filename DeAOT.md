# AOT
> Associating Objects with Transformers for Video Object Segmentation

# DeAOT
> Decoupling Features in Hierarchical Propagation for Video Object Segmentation

## Abstract
一种更有效的半监督视频对象分割层次传播方法  

在ViT的基础上，结合objects（AOT），引入hierarchical propagation  
分层传播可以逐渐将信息从过去帧传到当前帧  
而对象特定信息增加会导致与未知对象的视觉信息的损失，于是本文提出分层传播方法中的解耦特征  
DeAOT将特定对象与不可知对象在两个独立分支中处理以达到解耦。其次，为了弥补双分支传播的额外计算，提出了一种有效模块来构建分层传播  
