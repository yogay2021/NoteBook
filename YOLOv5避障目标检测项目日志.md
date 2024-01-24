# YOLOv5避障目标检测项目日志

## 添加注意力机制：

> 我们添加了一个SE注意力模块，借鉴了论文：
>
> **《Squeeze-and-Excitation Networks》**
>
> https://arxiv.org/pdf/1709.01507.pdf
>
> 添加模块借鉴了文章：
>
> [(259条消息) 手把手带你YOLOv5/v7 添加注意力机制（并附上30多种顶会Attention原理图）2023/2/11更新_yolov5添加注意力机制_迪菲赫尔曼的博客-CSDN博客](https://blog.csdn.net/weixin_43694096/article/details/124443059?csdn_share_tail={"type"%3A"blog"%2C"rType"%3A"article"%2C"rId"%3A"124443059"%2C"source"%3A"weixin_43694096"})

> `SEnet`通过学习的方式自动获取每个特征通道的重要程度，并且利用得到的重要程度来提升特征并抑制对当前任务不重要的特征。`SEnet` 通过`Squeeze`模块和`Exciation`模块实现所述功能。

添加流程记录：

在model/commom.py的末尾添加SE注意力模块的网络结构

![image-20230318083322125](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230318083322125.png)



在models/yolo.py 中约220行处添加SE的模块名：

![image-20230318083625409](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230318083625409.png)



在新建的yolov5_SE.yaml配置文件中，在backbone的C3层与SPPF层之间添加SE层。

![image-20230318084046257](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230318084046257.png)

关注网络中添加SE模块之后，后续层的编号变化，并进行修正：

![image-20230318084511849](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230318084511849.png)

![image-20230318084753783](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230318084753783.png)

![image-20230318084834688](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230318084834688.png)

添加完成，开始训练：

>  下面是训练的一些基本参数，大部分设为默认未修改：

| epochs       | 20       |
| ------------ | -------- |
| batch-size   | 24       |
| img size     | 640 640  |
| workers      | 8        |
| 其余所有参数 | 未做更改 |

训练了15轮耗时14小时。





