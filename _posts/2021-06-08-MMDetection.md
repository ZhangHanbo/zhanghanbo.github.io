---
layout: post
title:  "A Brief Introduction to MMDetection"
date:   2021-06-08 18:00:40
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
5. [My Manuscipts](#My Manuscripts)
    * [Weights Initialization](#Weights Initialization)
    

#### Abtract

本篇将记录我对于MMDetection的简单理解。
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

在MMDetection中，训练模型的流程为：

```
读取配置文件信息
      ↓
调用Registry模块使用配置信息初始化各个网络结构
      ↓
初始化网络参数与数据集
      ↓
初始化runner，注册hook以定义整体训练流程
      ↓
调用Runner完成网络的训练。
```



#### Modules

##### mmcv

MMCV可以说是包圆了所有MMDetection的外围工作。包括：
1. 从配置文件映射到具体的网络结构（Registry机制）：每个Registry可以理解为一个模块库。
当其被初始化时，库中没有模块。每当实现一个模块后，需要使用装饰器命令：
```@REGISTRY_NAME.register_module()```
来将模块注册到REGISTRY_NAME中。
在调用Registry对象进行模块初始化时，其作用可以理解为，输入为定义配置和超参数的字典，输出为模块，非常方便易用。
2. 数据读取与预处理；
3. 训练过程的管理（Runner机制）。Runner可以理解为一个管理员。
当需要训练或者测试模型时，可以通过初始化一个Runner，并配置好相关的Runner设置，通过Runner中的Hook机制，就能够精准的控制从拿到数据数据集后到获取最终的输出前的一系列操作。
比如，在每个Epoch前要做什么，后边要做什么，每个Iteration前要做什么，后要做什么等等。

##### Modules

Module中定义了目前主流的视觉检测算法的各个模块。可以理解为一个积木库。
当我们想训练物体检测器时，只需要从这个积木库中取出合适的部件拼到一起，即可获取一个能够在标准数据集上训练和测试的物体检测模型。
在MMDetection中，所有的模块都会被注册到相应的Registry对象里边。
在希望调用改模块时，只需要调用对应的Registry对象，输入一个配置字典，即可返回对应的实例化模块。

#### Config

MMDetection中所有的配置文件都定义了一个python字典。
该字典是一个层级字典，其层级与网络结构（或其他构件如数据集等）一一对应。
特定层级字典的键值即为初始化对应网络结构或构件的__init__()函数的参数列表。
这样做的一个很大的好处就在于，只需要读字典，就能够比较准确地推断出整个模型包含的所有信息。

#### My Manuscripts

在这一章节我将详细记录一些在写代码过程中发现的觉得有意思的点，以防之后忘记。

##### Weights Initialization

在MMDetection中，初始化模型权重（包括读取预训练权重）的工作是由底层的MMCV完成的。
在程序中，权重初始化是通过module内一个名为init_cfg的属性完成的。
init_cfg和其他配置一样，也是一个字典，其必须包含type字段来指定初始化类型。
支持的类型包括：

* Constant
* Xavier
* Normal
* Uniform
* Kaiming
* Caffe2Xavier
* Pretrained

其他字段由type决定，以确定初始化过程中的各项参数。
如需要读取预训练权重时，需要指定type="Pretrained", checkpoint=your_pretrained_path。

使用init_cfg的注意事项
* 一般来说，使用init_cfg需要指定layer字段，以指定初始化的层类型。如：
```
import torch.nn as nn
from mmcv.cnn import initialize

class FooNet(nn.Module):
    def __init__(self):
        super().__init__()
        self.feat = nn.Conv1d(3, 1, 3)
        self.reg = nn.Conv2d(3, 3, 3)
        self.cls = nn.Linear(1, 2)

model = FooNet()

# 所有'Conv1d', 'Conv2d', 'Linear'类型的层均以相同的Constant初始化
init_cfg = dict(type='Constant', layer=['Conv1d', 'Conv2d', 'Linear'], val=1)
initialize(model, init_cfg)

# 分别初始化不同类型的层
init_cfg = [dict(type='Constant', layer='Conv1d', val=1),
            dict(type='Constant', layer='Conv2d', val=2),
            dict(type='Constant', layer='Linear', val=3)]
initialize(model, init_cfg)
```

* 如需对特定层进行初始化，需使用override参数。如：
```

# 属性名为reg的层使用val=3, bias=4的Constant进行初始化
# 其余'Conv1d','Conv2d'的层使用val=1, bias=2的Constant进行初始化
model = FooNet()
init_cfg = dict(type='Constant', layer=['Conv1d','Conv2d'], val=1, bias=2,
                override=dict(type='Constant', name='reg', val=3, bias=4))
initialize(model, init_cfg)
```

* 如果layer字段为None，则只有override中指定的层会被初始化。
且初始化方法由init_cfg中的类型和参数决定，override中的初始化参数会被忽略。

* 如果layer和override都是None，则不会有任何层被初始化。

* 使用预训练模型初始化时，需指定checkpoint参数。
此外，可通过指定prefix参数只初始化特定前缀的层。如：
```
import torchvision.models as models
from mmcv.cnn import initialize

# 使用torchvision自带的预训练模型初始化
model = models.resnet50()
init_cfg = dict(type='Pretrained',
                checkpoint='torchvision://resnet50')
initialize(model, init_cfg)

# 使用URL指定的预训练模型初始化前缀为backbone的层
url = 'http://download.openmmlab.com/mmdetection/v2.0/retinanet/'\
      'retinanet_r50_fpn_1x_coco/'\
      'retinanet_r50_fpn_1x_coco_20200130-c2398f9e.pth'
init_cfg = dict(type='Pretrained',
                checkpoint=url, prefix='backbone.')
initialize(model, init_cfg)
```

* 使用mmcv中的BaseModule, Sequential, ModuleList及其子类时，可通过传入init_cfg参数，并调用其init_weights进行初始化。如：
```
import torch.nn as nn
from mmcv.runner import BaseModule, Sequential, ModuleList

class FooConv1d(BaseModule):

    def __init__(self, init_cfg=None):
        super().__init__(init_cfg)
        self.conv1d = nn.Conv1d(4, 1, 4)

    def forward(self, x):
        return self.conv1d(x)

init_cfg = dict(type='Constant', layer='Conv1d', val=0., bias=1.)

# 将初始化配置传入模型，并调用模型的init_weights方法进行初始化
model = FooConv1d(init_cfg)
model.init_weights()
```

详情：https://mmcv.readthedocs.io/en/latest/cnn.html

##### Ground Truth Labels in Assign Result

与之前的认知不同，在MMDetection的Assign result里，类别是以0为开始的，背景被表示为-1. 
因此在训练网络时，所有的label值会被加1.


#### Footnote
[^1]: https://openmmlab.com/.
