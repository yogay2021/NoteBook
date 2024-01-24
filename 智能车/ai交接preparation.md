## 第一点、数据集

### 一、目前手头已经有的：

>1.手机拍摄8k+开源4k
>
>2.车拍摄多元素混合6k
>
>3.官方开源的数据集6k
>
>> 有两个问题：
>>
>> 1.线上使用，将错就错
>>
>> 2.线下使用，适当改错
>
>> 下载网址：[第十八届智能汽车完全模型组线下赛模型训练（以SSD-MoblienetV1为例） - 飞桨AI Studio (baidu.com)](https://aistudio.baidu.com/aistudio/datasetdetail/191561)

### 二、怎么添加？

图像来源？

用车载摄像头拍摄

> 1.手柄彩图
>
> 2.python脚本采图

怎么标注，做成数据集？

两个选择：

> 1.百度的数据处理平台：**easydata** （自动标注高效）
>
> 2.轻量化的小软件labelimg （少量错图迅速修改保存）

### 三、做一些增强

如果数据不均衡：

> 1.针对性的去除一些对象 (脚本识别标签，把一些目标位置像素直接置0)
>
> 2.补充一些对象 （copy-patse 数据增强）

==运动模糊==，翻转，旋转，对称，==颜色空间扰动，尺度变换==



## 第二点、模型

线上模型平台网址：

[【PP-YOLOE+】第18届全国大学生智能汽车竞赛百度完全模型组线上资格赛基线 - 飞桨AI Studio (baidu.com)](https://aistudio.baidu.com/aistudio/projectdetail/5513476?channelType=0&channel=0)

线下模型平台网址：

[2023年智能汽车竞赛“完全模型组”智慧农业-目标检测模型训练 - 飞桨AI Studio (baidu.com)](https://aistudio.baidu.com/aistudio/projectdetail/5143716)

[第十八届智能汽车完全模型组线下赛模型训练（以SSD-MoblienetV1为例） - 飞桨AI Studio (baidu.com)](https://aistudio.baidu.com/aistudio/projectdetail/5404910)



## 第三点、流程

### 一、官方教程

开源的文档教程：

[【Q&A】2023智能车-百度竞速完全模型组-#20230106 (shimo.im)](https://shimo.im/docs/QhG7PWaxuM47dK5F/read)

官方的培训视频：

[2023年全国大学生智能汽车竞赛百度完全模型组线上预选赛培训_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1S24y1H74V/?spm_id_from=333.337.search-card.all.click&vd_source=9d427f611a01aaa1c1131c20063c20df)