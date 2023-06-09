基于CNN的区域特定多尺度特征提取的两阶段停车位检测

## 0. 引言

在自动驾驶系统的设计中，停车位的检测一直是一项具有挑战性的任务。本文将带大家精读2021 CVPR的论文"基于CNN的区域特定多尺度特征提取的两阶段停车位检测"，该论文阐述了一种全新的两阶段停车位检测方法，使用区域特定的多尺度特征提取，具有开创性的价值。

## 1. 论文信息

标题：CNN-based Two-Stage Parking Slot Detection Using Region-Specific Multi-Scale Feature Extraction

作者：Quang Huy Bui and Jae Kyu Suhr

来源：2021 Computer Vision and Pattern Recognition(CVPR)

原文链接：https://arxiv.org/abs/2108.06185

## 2. 摘要

基于深度学习的对象检测方法可以分为一阶段和两阶段方法。虽然众所周知，两阶段方法在一般对象检测中优于一阶段方法，但是到目前为止，它们在停车位检测中表现相似。

我们认为这是因为两阶段方法还没有被充分地专用于停车位检测。因此，本文提出了一个高度专业化的两阶段停车位检测器，它使用区域特定的多尺度特征提取。

在第一阶段，所提出的方法通过估计停车位的中心、长度和方向来找到停车位的入口作为区域提议。

该方法的第二阶段指定包含所需信息最多的特定区域，并从中提取特征。也就是说，仅从最包含位置和方向信息的特定区域中单独提取位置和方向的特征。

此外，多分辨率特征地图被用来提高定位和分类的准确性。一个高分辨率特征图用于提取详细信息(位置和方向)，而另一个低分辨率特征图用于提取语义信息(类型和占用)。

在实验中，使用两个大规模的公共停车位检测数据集对所提出的方法进行了定量评估，其性能优于先前的方法，包括一阶段和两阶段方法。

## 3. 算法分析

如图1所示为作者提出的利用区域特定的多尺度特征提取的两阶段方法的总体架构，输入的AVM图像被插入到主干网络中用于特征图提取。

该算法在第一阶段使用区域建议网络(RPN)粗略定位停车位入口，在第二阶段使用停车位检测网络(SDN)和停车位分类网络(SCN)精确估计停车位的位置和属性。


![图1 利用区域特定的多尺度特征提取的两阶段方法的总体架构](https://img-blog.csdnimg.cn/b762ae1fa69b4748ba155b5200228773.png)

在以前的停车线检测中，基本都是从特征图中提取整个区域提议的特征，或者从输入图像中裁剪区域提议的整个区域。而这一框架仅从包含相应信息最多的特定区域中，单独提取用于预测停车位的位置和方向的特征。该论文的贡献可以概括为如下几点：

(1) 提出了一种将两阶段通用目标检测应用于停车位检测任务的有效方法；

(2) 提出了区域特定的多尺度特征提取，提高了检测性能和定位精度；

(3) 使用两个大规模公共数据集给出了定量评估结果，并表明所提出的方法给出了最先进的性能。

### 3.1 区域提案网络

与以前使用平行四边形、四边形或旋转矩形捕获整个停车位的方法不同，作者所提出的方法将停车位入口生成为区域建议。这是因为AVM图像通常不包括整个停车槽，而停车槽入口本身包含足够的信息供汽车开始停车，如图2所示为算法提出的RPN网络信息。

![图2 区域提案网络(RPN)以及从中获得的详细信息](https://img-blog.csdnimg.cn/23f0e65ef3ed4c129b0ca004d8c6a77d.png)

在图2所示的RPN中，红色实线和箭头分别指示生成的停车位入口和停车位的方向。因为RPN可以为单个停车位找到多个入口，所以基于两个停车位不能重叠的事实，利用非最大抑制(NMS)来移除重复检测。如果两个入口的中心位置非常接近，则认为这两个入口是重复的。

### 3.2 特定区域多尺度特征提取

在将停车位入口生成为区域提议之后，算法从生成的区域提议指定的感兴趣区域(ROI)中提取特征。作者巧妙地考虑到了停车位在AVM图像中可以以任意方向出现，所以不同于用直立矩形和平行四边形作为ROI进行特征提取的方法，作者提出了一种使用多尺度特征图的区域特定感兴趣区域设计，称为区域特定多尺度特征提取，其原理如图3所示。同时，图4给出了所提出的特定区域多尺度特征提取的完整操作。

![图3 区域特定多尺度特征提取原理](https://img-blog.csdnimg.cn/e4f6add3b99d4fc3a411cba5b23b01ae.png)

(a)基于平行四边形的ROI指定；(b)-(d)区域特定的ROI指定，(b)显示位置区域，(c)显示方向区域，(d)显示类型和占用区域。

![图4 特定区域多尺度特征提取的操作](https://img-blog.csdnimg.cn/1cb83992199a4738b665b0fc838a7f36.png)

### 3.3 停车位检测和分类网络

利用所提出的特定区域多尺度特征提取所获得的特征，SDN检测停车位的精确位置和方向，而SCN对其类型和占用情况进行分类。图5分别给出了SDN和SCN的详细描述。

![图5 槽检测网络(SDN)和槽分类网络(SCN)](https://img-blog.csdnimg.cn/477a9480b32e40f48cd06df5dde2b7f3.png)
## 4. 实验

作者使用两个大规模公共停车位检测数据集对所提出的方法进行了定量评估:首尔国立大学数据集(SNU)和同济停车位数据集(PS2.0),如表1所示为两个数据集上的评估总结。

表1 SNU和PS2.0数据集上的评估总结

![](https://img-blog.csdnimg.cn/f3244985865e4d67b71c06ab4b8af371.jpeg)

### 4.1 SNU数据集上的性能

表2是作者提出的方法和之前两种最先进的一步和两步方法的检测性能对比，其中一阶段法比两阶段法表现稍好，这主要是因为两阶段方法还没有被充分地专用于停车位检测。

同时，结果表明两阶段方法在宽松标准下比其他方法大约好3%到5%,在严格标准下比其他方法好11%到13%，说明如果两阶段方法进行合理设计，那么是可以胜过一阶段方法的。此外，当收紧标准时，作者提出的方法的性能仅下降约12%，而其他方法的性能急剧下降约20%。

表2 SNU数据集上的停车线检测性能对比

![](https://img-blog.csdnimg.cn/41cf16ce3a1f41848eb95dedcf1a4ed8.png)

表3给出了三种方法的详细定位精度，表明作者所提出的方法的位置和方向误差都小于其他人。在自主停车系统中，定位精度非常重要，两阶段方法展示了更优秀的位置精度。

表3 SUN数据集上的停车线位置精度对比

![](https://img-blog.csdnimg.cn/35abd4b0c2df4126a6db55288c7fd4d2.png)

表4给出了消融实验的结果，实验集中在区域特定的感兴趣区域和多尺度特征图上进行。消融实验表明，作者所提出的区域特定的多尺度特征提取提高了停车位检测性能。

表4 消融实验

![](https://img-blog.csdnimg.cn/718f7bbaeb97453aada17bc7cb2fc6ef.png)

图6展示了包含在SNU数据集的测试图像中的各种停车情况下的停车位检测结果，结果显示各种停车线都可以被准确稳定地检测到。

![](https://img-blog.csdnimg.cn/bf2322a56bf34b99a290f7f9a12ea890.png)



图6 SNU数据集测试图像中的停车位检测结果

绿线、红线和蓝线分别表示垂直、平行和倾斜的停车位；实线和虚线分别表示空闲和占用的停车位。

### 4.2 PS2.0数据集上的性能

表5显示了PS2.0数据集上停车位检测性能的比较，作者所提出的方法在PS2.0数据集显示出比其他方法稍高的停车位检测性能。

表5 PS2.0数据集上停车位检测性能的比较

![](https://img-blog.csdnimg.cn/0ae2e36e11a7426d982a73ae6c62776e.png)

在PS2.0数据集上的差距不像在SNU数据集上那样明显，因为几乎所有的方法在该数据集上都已经达到了非常高的检测性能。这主要是由于PS2.0数据集的训练图像和测试图像之间的相似性。这种相似性使得很难用来比较不同方法的性能。表6比较了PS2.0数据集上的类型和占用分类性能。

表6 PS2.0数据集上停车位分类性能的比较

![](https://img-blog.csdnimg.cn/c5b4b2478397434caae6419d384fe6c0.png)

图7给出了在PS2.0数据集的测试图像中包含的各种停车情况下的停车位检测结果，这也表明了作者所提出的方法能够恰当地处理包含在PS2.0数据集中的各种情况。

![](https://img-blog.csdnimg.cn/93a981fbc14b4e9d93b540e2f5bf7d2f.png)

图7 在PS2.0数据集的测试图像中，给出了所提方法的停车位检测结果

绿线、红线和蓝线分别表示垂直、平行和倾斜的停车位；实线和虚线分别表示空闲和占用的停车位。

## 5. 结论

在论文"CNN-based Two-Stage Parking Slot Detection Using Region-Specific Multi-Scale Feature Extraction"中，作者提出了一种新的高度专业化的两阶段停车位检测方法，该方法在第一阶段寻找停车位入口作为区域建议，并在第二阶段从多尺度特征地图中提取区域特定特征以精确预测停车位的位置和属性。

最后使用两个大规模公共停车位检测数据集对该方法进行了定量评估，其检测性能和定位精度均优于以往方法。这一结果表明，如果充分专门化，两阶段方法优于一阶段方法，与一般对象检测的情况相同，这一方法对后续的停车线检测方法具有主要的借鉴意义。此外，作者也提到可以使用滤波器修剪和权重量化来优化网络，以在实时嵌入式系统中实现，读者也可将其作为一个研究方向。
