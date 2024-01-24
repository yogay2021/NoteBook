今天在处理数据集的标注问题：

在镜像网站上，最终成功下载了vott的安装包：

> [VoTT download | SourceForge.net](https://sourceforge.net/projects/vott.mirror/)

用VOTT进行数据集的标注：

使用过程参考了文章：

> [(236条消息) 标注工具 VoTT 详细教程_清欢守护者的博客-CSDN博客_vott标注工具](https://blog.csdn.net/irving512/article/details/107901599)

> 简单记录下用VOTT来标注数据集的流程：
>
> ==创建项目第一步工程设置：==
>
> 关注四个点：
>
> 1.项目名称  2.数据来源连接  3.数据输出连接  4. 标签种类Tags
>
> ![image-20230211205630025](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230211205630025-1676249584139-1.png)
>
> 建立数据连接时：
>
> 1.连接的名字 
>
> 2.provider选择local file system，然后选择本地存放未标注的图像数据集文件夹地址，或者是希望标注后输出的文件夹地址（具体看建立的连接属于source还是target）
>
> ![image-20230211210257424](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230211210257424-1676249584139-3.png)
>
> ---
>
> ==第二步给数据打框标注：==
>
> 这一步还可以用自动标注的功能，但是需要导入可用的模型。
>
> 这里我只能手动标注：
>
> 1437张图都是手动打的数据框，花了好几个小时，这种机械工作累人还浪费时间，后面得仔细研究下自动标注的方法。
>
> <img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230211211819936-1676249584139-5.png" alt="image-20230211211819936" style="zoom:67%;" />
>
> ==第三步配置导出设置：==
>
> provider选择希望的导出格式：
>
> ![image-20230211212304038](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230211212304038-1676249584139-7.png)
>
> ![image-20230211212227017](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230211212227017-1676249584139-9.png)
>
> ==第四步导出标注以及图像等==：
>
> ![image-20230211212418309](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230211212418309-1676249584140-11.png)
>
> ==希望再次打开工程时：==
>
> 选择vott文件打开即可，其保存路径是第一步工程设置时的输出路径。

最后导出的文件包含：

> annotations标签，jpegimages图像，imagesets则是记录标签类别、训练集图像名、测试集图像名的txt文件

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230211212545810-1676249584140-13.png" alt="image-20230211212545810" style="zoom:50%;" />

为了生成 记录训练集测试集图像名 的txt文件，需要运行一个demo：

```python
import os
import random

train_precent=0.8 #划分比例
xml="E:/DeskTop/photo/smart car train/labels/sm-model-PascalVOC-export/Annotations"
save="E:/DeskTop/photo/smart car train/labels/sm-model-PascalVOC-export/ImageSets/Main"
total_xml=os.listdir(xml)

num=len(total_xml)
tr=int(num*train_precent)
train=range(0,tr)

ftrain=open("E:/DeskTop/photo/smart car train/labels/sm-model-PascalVOC-export/ImageSets/Main/train.txt","w")
ftest=open("E:/DeskTop/photo/smart car train/labels/sm-model-PascalVOC-export/ImageSets/Main/test.txt","w")

for i in range(num):
    name=total_xml[i][:-4]+"\n"
    if i in train:
        ftrain.write(name)
    else:
        ftest.write(name)
ftrain.close()
ftest.close()
```

还需要生成记录数据集图像位置以及标签位置的 txt文件：

运行下面demo(data loadxml.py)：

```python
import os
import os.path as osp
import re
import random

devkit_dir = './'

def get_dir(devkit_dir,  type):
    return osp.join(devkit_dir, type)

def walk_dir(devkit_dir):
    filelist_dir = get_dir(devkit_dir, 'ImageSets/Main')
    annotation_dir = get_dir(devkit_dir, 'Annotations')
    img_dir = get_dir(devkit_dir, 'JPEGImages')
    trainval_list = []
    test_list = []
    added = set()

    for _, _, files in os.walk(filelist_dir):
        for fname in files:
            img_ann_list = []
            if re.match('train\.txt', fname):
                img_ann_list = trainval_list
            elif re.match('val\.txt', fname):
                img_ann_list = test_list
            else:
                continue
            fpath = osp.join(filelist_dir, fname)
            for line in open(fpath):
                name_prefix = line.strip().split()[0]
                if name_prefix in added:
                    continue
                added.add(name_prefix)
                ann_path = osp.join(annotation_dir, name_prefix + '.xml')
                img_path = osp.join(img_dir, name_prefix + '.jpg')
                assert os.path.isfile(ann_path), 'file %s not found.' % ann_path
                assert os.path.isfile(img_path), 'file %s not found.' % img_path
                img_ann_list.append((img_path, ann_path))

    return trainval_list, test_list

def prepare_filelist(devkit_dir, output_dir):
    trainval_list = []
    test_list = []
    trainval, test = walk_dir(devkit_dir)
    trainval_list.extend(trainval)
    test_list.extend(test)
    random.shuffle(trainval_list)
    with open(osp.join(output_dir, 'train.txt'), 'w') as ftrainval:
        for item in trainval_list:
            ftrainval.write(item[0] + ' ' + item[1] + '\n')

    with open(osp.join(output_dir, 'val.txt'), 'w') as ftest:
        for item in test_list:
            ftest.write(item[0] + ' ' + item[1] + '\n')

if __name__ == '__main__':
    prepare_filelist(devkit_dir, '.')
```

下图train以及val就是目标文件：

![image-20230213092642313](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230213092642313.png)

![image-20230213093149632](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230213093149632.png)