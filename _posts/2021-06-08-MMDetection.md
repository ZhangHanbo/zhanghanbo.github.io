---
layout: post
title:  "A Brief Introduction to MMDetection"
date:   2021-06-06 18:00:40
blurb: ""
og_image: /assets/img/content/post-example/Banner.jpg
---

<img src="{{ "/assets/img/blog/MMDetection/mmdet-logo.png" | absolute_url }}" alt="mmdetection-logo" class="post-pic"/>
<br />

#### Table of Contents
1. [Abtract](#Abtract)
2. [Framework](#Framework)
3. [Modules](#Modules)
    * [mmcv](#mmcv)
    * [non-parametric modules](#non-parametric modules)
        * [anchor](#anchor)
        * [bbox-assigner](#bbox-assigner)
        * [bbox-sampler](#bbox-sampler)
        * [bbox-coder](#bbox-coder)
    * [parametric modules](#parametric modules)
        * [detector](#detector)
        * [backbone](#backbone)
        * [neck](#neck)
        * [roi-head](#roi-head)
        * [dense-head](#dense-head)
4. [Config](#Config)
    

#### Abtract

本篇博客将记录我对于MMDetection的简单理解。
MMDetection是由港中文组织开发的一个开源深度检测框架。
其模块化程度较好，但同时保留了很好的扩展性。
代码优雅，结构简练，变量与函数的命名清楚，注释完备。
最重要的是支持的算法种类非常多，给检测人带来了非常方便易用的一个框架体系。
此外，除了MMDetection外，相同系列的其他框架如MMClassification, MMPose等也分别在各自的视觉领域有相关的开源工程[^1]。

#### Framework

MMDetection结构非常简练，和已有的开源物体检测算法框架高度相似，但是却进行了非常完善的模块化设计。
其结构主要分为如下部分：
1. MMCV。这是一个在各个视觉工程中共享的CV包，包含了一些通用功能的实现。比如Runner机制，Registry机制，图片视频的读写、处理，可视化等。
2. MMDetection modules. 其中定义了不同的物体检测的模块，包含backbone, neck, head等参数化模块和anchor生成器，编码器，分配器，采样器等。
这些功能多为物体检测任务的特有功能。
3. 配置文件。MMDetection中的配置文件是分层的。所有配置文件均定义为字典。

在MMDetection中，训练模型的流程为：读取配置文件信息-->调用Registry模块使用配置信息初始化各个网络结构-->初始化数据集-->调用Runner完成网络的训练。

#### Modules

##### mmcv

MMCV可以说是包圆了所有MMDetection的外围工作。包括：
1. 从配置文件映射到具体的网络结构（Registry机制）：每个Registry可以理解为一个模块库。
当其被初始化时，库中没有模块。每当实现一个模块后，需要使用装饰器命令：
```@REGISTRY_NAME.register_module()```
来将模块注册到REGISTRY_NAME中。
在调用Registry对象进行模块初始化时，其作用可以理解为，输入为字典，输出为模块，非常方便易用。
2. 数据读取与预处理；
3. 训练过程的管理（Runner机制）。Runner可以理解为一个管理员。
当需要训练或者测试模型时，可以通过初始化一个Runner，并配置好相关的Runner设置，通过Runner中的Hook机制，就能够精准的控制从拿到数据数据集后到获取最终的输出前的一系列操作。
比如，在每个Epoch前要做什么，后边要做什么，每个Iteration前要做什么，后要做什么等等。

##### Modules

Module中定义了目前主流的视觉检测算法的各个模块。可以理解为一个积木库。
当我们想训练物体检测器时，只需要从这个积木库中取出合适的部件拼到一起，即可获取一个能够在标准数据集上训练和测试的物体检测模型。

#### Footnote
[^1]: https://openmmlab.com/.
