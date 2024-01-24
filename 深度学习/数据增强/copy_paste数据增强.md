## 脚本的开发日志

第一阶段，实现把一张图像上的目标随机粘贴，没考虑重合的问题，没做大批量的遍历：

```
import cv2
import xml.etree.ElementTree as ET
import numpy as np
import copy

# 定义图像和标签文件的路径
image_path = 'E:/Project/Python/smartcar/ourdataset/data_outback/datax/Images/2011.jpg'
annotation_path = 'E:/Project/Python/smartcar/ourdataset/data_outback/datax/Annotations/2011.xml'

# 定义复制的数量
num_copies = 2

# 定义复制对象的缩放范围
scale_range = (0.9, 1.1)

# 读取图像
image = cv2.imread(image_path)

# 解析XML标签文件
annotation_tree = ET.parse(annotation_path)
annotation_root = annotation_tree.getroot()

# 获取图像尺寸
image_height, image_width, _ = image.shape

# 循环复制对象
for i in range(num_copies):
    # 随机选择一个目标对象
    object_idx = np.random.randint(0, len(annotation_root.findall('object')))
    object_elemx = annotation_root.findall('object')[object_idx]

    object_elem = copy.deepcopy(object_elemx)

    # 解析XML获取目标对象的信息
    name = object_elem.find('name').text
    xmin = int(object_elem.find('bndbox/xmin').text)
    ymin = int(object_elem.find('bndbox/ymin').text)
    xmax = int(object_elem.find('bndbox/xmax').text)
    ymax = int(object_elem.find('bndbox/ymax').text)

    object_roi = image[ymin:ymax, xmin:xmax].copy()

    # 计算目标对象的宽度和高度
    width = xmax - xmin
    height = ymax - ymin

    # 随机生成缩放比例
    scale = np.random.uniform(scale_range[0], scale_range[1])

    # 根据缩放比例计算复制对象的新宽度和新高度
    new_width = int(width * scale)
    new_height = int(height * scale)

    # 随机生成复制对象的位置
    new_xmin = np.random.randint(0, image_width - new_width)
    new_ymin = np.random.randint(0, image_height - new_height)

    new_xmax = new_xmin + (xmax - xmin)
    new_ymax = new_ymin + (ymax - ymin)

    image[new_ymin:new_ymax, new_xmin:new_xmax] = object_roi

    # 更新目标对象的位置和尺寸
    object_elem.find('bndbox/xmin').text = str(new_xmin)
    object_elem.find('bndbox/ymin').text = str(new_ymin)
    object_elem.find('bndbox/xmax').text = str(new_xmin + new_width)
    object_elem.find('bndbox/ymax').text = str(new_ymin + new_height)

    # 复制目标对象的标签
    new_object_elem = ET.Element('object')
    ET.SubElement(new_object_elem, 'name').text = name
    bbox_elem = ET.SubElement(new_object_elem, 'bndbox')
    ET.SubElement(bbox_elem, 'xmin').text = str(new_xmin)
    ET.SubElement(bbox_elem, 'ymin').text = str(new_ymin)
    ET.SubElement(bbox_elem, 'xmax').text = str(new_xmin + new_width)
    ET.SubElement(bbox_elem, 'ymax').text = str(new_ymin + new_height)
    annotation_root.append(new_object_elem)


cv2.imwrite('E:\\Project\\Python\\smartcar\\ourdataset\\data_outback\\datax\\aug\\augmented_2011.jpg', image)
annotation_tree.write('E:\\Project\\Python\\smartcar\\ourdataset\\data_outback\\datax\\aug\\augmented_2011.xml')
```

更新优化：

通过计算生成的框位置和所有已有的框进行遍历，计算iou，我不希望有重合，考虑切割时覆盖的背景会遮盖目标，设置iou阈值为0.05。

```python
import cv2
import xml.etree.ElementTree as ET
import numpy as np
import copy

# 定义图像和标签文件的路径
image_path = 'E:/Project/Python/smartcar/ourdataset/data_outback/datax/Images/2011.jpg'
annotation_path = 'E:/Project/Python/smartcar/ourdataset/data_outback/datax/Annotations/2011.xml'

# 定义复制的数量
num_copies = 2

# 定义复制对象的缩放范围
scale_range = (1, 1)

# 读取图像
image = cv2.imread(image_path)

# 解析XML标签文件
annotation_tree = ET.parse(annotation_path)
annotation_root = annotation_tree.getroot()

# 获取图像尺寸
image_height, image_width, _ = image.shape

# 循环复制对象
num_copied = 0
while True:
    if num_copied == num_copies:
        break
    # 随机选择一个目标对象
    object_idx = np.random.randint(0, len(annotation_root.findall('object')))
    object_elemx = annotation_root.findall('object')[object_idx]

    object_elem = copy.deepcopy(object_elemx)

    # 解析XML获取目标对象的信息
    name = object_elem.find('name').text
    xmin = int(object_elem.find('bndbox/xmin').text)
    ymin = int(object_elem.find('bndbox/ymin').text)
    xmax = int(object_elem.find('bndbox/xmax').text)
    ymax = int(object_elem.find('bndbox/ymax').text)

    object_roi = image[ymin:ymax, xmin:xmax].copy()

    # 计算目标对象的宽度和高度
    width = xmax - xmin
    height = ymax - ymin

    # 随机生成缩放比例
    scale = np.random.uniform(scale_range[0], scale_range[1])

    # 根据缩放比例计算复制对象的新宽度和新高度
    new_width = int(width * scale)
    new_height = int(height * scale)

    # 随机生成复制对象的位置
    new_xmin = np.random.randint(0, image_width - new_width)
    new_ymin = np.random.randint(0, image_height - new_height)

    new_xmax = new_xmin + (xmax - xmin)
    new_ymax = new_ymin + (ymax - ymin)
    # new_xmax = new_xmin + new_width
    # new_ymax = new_ymin + new_height

    #定义生成框的重叠状态
    overlap = False
    #遍历所有的目标框，检查iou，在阈值范围之内的才判断为成功的copy
    for num_box in range(len(annotation_root.findall('object'))):
        object_elemxx = annotation_root.findall('object')[num_box]
        xminx = int(object_elemxx.find('bndbox/xmin').text)
        yminx = int(object_elemxx.find('bndbox/ymin').text)
        xmaxx = int(object_elemxx.find('bndbox/xmax').text)
        ymaxx = int(object_elemxx.find('bndbox/ymax').text)

        #求交集
        x_left = max(xminx, new_xmin)
        y_top = max(yminx, new_ymin)
        x_right = min(xmaxx, new_xmax)
        y_bottom = min(ymaxx, new_ymax)
        intersection = max(0, x_right - x_left) * max(0, y_bottom - y_top)

        #求并集
        union = (new_xmax-new_xmin)*(new_ymax-new_ymin)+(xmaxx-xminx)*(ymaxx-yminx)-intersection

        #iou
        iou = intersection/union
        print(iou)
        if iou > 0.05:
            overlap = True
            break

    if not overlap:
        image[new_ymin:new_ymax, new_xmin:new_xmax] = object_roi

        # 更新目标对象的位置和尺寸
        object_elem.find('bndbox/xmin').text = str(new_xmin)
        object_elem.find('bndbox/ymin').text = str(new_ymin)
        object_elem.find('bndbox/xmax').text = str(new_xmin + new_width)
        object_elem.find('bndbox/ymax').text = str(new_ymin + new_height)

        # 复制目标对象的标签
        new_object_elem = ET.Element('object')
        ET.SubElement(new_object_elem, 'name').text = name
        bbox_elem = ET.SubElement(new_object_elem, 'bndbox')
        ET.SubElement(bbox_elem, 'xmin').text = str(new_xmin)
        ET.SubElement(bbox_elem, 'ymin').text = str(new_ymin)
        ET.SubElement(bbox_elem, 'xmax').text = str(new_xmin + new_width)
        ET.SubElement(bbox_elem, 'ymax').text = str(new_ymin + new_height)
        annotation_root.append(new_object_elem)
        num_copied = num_copied + 1

cv2.imwrite('E:\\Project\\Python\\smartcar\\ourdataset\\data_outback\\datax\\aug\\augmented_2011.jpg', image)
annotation_tree.write('E:\\Project\\Python\\smartcar\\ourdataset\\data_outback\\datax\\aug\\augmented_2011.xml')

```

2023.04.19更新：

> 实现copy后目标的随机尺寸变化，也就是拉伸，根据论文更加暴力的效果更加显著

```python
import cv2
import xml.etree.ElementTree as ET
import numpy as np
import copy
import os
import random
import tqdm

image_path = 'E:/Project/Python/smartcar/data_expend/testdemo/images/'
annota_path = 'E:/Project/Python/smartcar/data_expend/testdemo/Annotations/'

im_save = 'E:/Project/Python/smartcar/data_expend/testdemo/aug1/image/'
an_save = 'E:/Project/Python/smartcar/data_expend/testdemo/aug1/labels/'

files_list = os.listdir(image_path)
anno_list = os.listdir(annota_path)
turn = 0
for file in files_list:
    turn = turn + 1
    print(turn)
    for anno in anno_list:
        if file[:-4] == anno[:-4]:
            # 定义复制的数量
            num_copies = 2
            # 定义复制对象的缩放范围
            scale_range = (0.1, 2.0)

            # 获取文件地址
            img_path = os.path.join(image_path,file)
            anno_path = os.path.join(annota_path,anno)
            # 读取图像
            image = cv2.imread(img_path)

            # 解析XML标签文件
            annotation_tree = ET.parse(anno_path)
            annotation_root = annotation_tree.getroot()

            # 获取图像尺寸
            image_height, image_width, _ = image.shape

            # 循环复制对象
            num_copied = 0
            cycle_turn = 0
            while True:
                # 计算循环次数
                cycle_turn = cycle_turn + 1
                # 超过10次循环while就break，放弃这张图，防止卡程序
                if cycle_turn == 11:
                    break
                if num_copied == num_copies:
                    break
                # 随机选择一个目标对象
                object_idx = np.random.randint(0, len(annotation_root.findall('object')))
                object_elemx = annotation_root.findall('object')[object_idx]

                object_elem = copy.deepcopy(object_elemx)

                # 解析XML获取目标对象的信息
                name = object_elem.find('name').text
                xmin = int(object_elem.find('bndbox/xmin').text)
                ymin = int(object_elem.find('bndbox/ymin').text)
                xmax = int(object_elem.find('bndbox/xmax').text)
                ymax = int(object_elem.find('bndbox/ymax').text)

                # 计算目标对象的宽度和高度
                width = xmax - xmin
                height = ymax - ymin

                # 根据缩放比例计算复制对象的新宽度和新高度
                scalex = random.uniform(scale_range[0], scale_range[1])
                scaley = random.uniform(scale_range[0], scale_range[1])

                new_width = int(width * scalex)
                new_height = int(height * scaley)

                # 截取copy部分
                object_roi = image[ymin:ymax, xmin:xmax].copy()

                # 随机生成复制对象的位置
                new_xmin = np.random.randint(0, image_width - new_width)
                new_ymin = np.random.randint(0, image_height - new_height)

                # new_xmax = new_xmin + (xmax - xmin)
                # new_ymax = new_ymin + (ymax - ymin)
                new_xmax = new_xmin + new_width
                new_ymax = new_ymin + new_height

                # object_roi = cv2.resize(object_roi, fx=scalex, fy=scaley)
                object_roi = cv2.resize(object_roi,(new_width, new_height))

                # 定义生成框的重叠状态
                overlap = False
                # 遍历所有的目标框，检查iou，在阈值范围之内的才判断为成功的copy
                for num_box in range(len(annotation_root.findall('object'))):
                    object_elemxx = annotation_root.findall('object')[num_box]
                    xminx = int(object_elemxx.find('bndbox/xmin').text)
                    yminx = int(object_elemxx.find('bndbox/ymin').text)
                    xmaxx = int(object_elemxx.find('bndbox/xmax').text)
                    ymaxx = int(object_elemxx.find('bndbox/ymax').text)

                    # 求交集
                    x_left = max(xminx, new_xmin)
                    y_top = max(yminx, new_ymin)
                    x_right = min(xmaxx, new_xmax)
                    y_bottom = min(ymaxx, new_ymax)
                    intersection = max(0, x_right - x_left) * max(0, y_bottom - y_top)

                    # 求并集
                    union = (new_xmax-new_xmin)*(new_ymax-new_ymin)+(xmaxx-xminx)*(ymaxx-yminx)-intersection

                    # iou
                    iou = intersection/union
                    if iou > 0.02:
                        overlap = True
                        break

                if not overlap:
                    image[new_ymin:new_ymax, new_xmin:new_xmax] = object_roi

                    # 更新目标对象的位置和尺寸
                    object_elem.find('bndbox/xmin').text = str(new_xmin)
                    object_elem.find('bndbox/ymin').text = str(new_ymin)
                    object_elem.find('bndbox/xmax').text = str(new_xmin + new_width)
                    object_elem.find('bndbox/ymax').text = str(new_ymin + new_height)

                    # 复制目标对象的标签
                    new_object_elem = ET.Element('object')
                    ET.SubElement(new_object_elem, 'name').text = name
                    bbox_elem = ET.SubElement(new_object_elem, 'bndbox')
                    ET.SubElement(bbox_elem, 'xmin').text = str(new_xmin)
                    ET.SubElement(bbox_elem, 'ymin').text = str(new_ymin)
                    ET.SubElement(bbox_elem, 'xmax').text = str(new_xmin + new_width)
                    ET.SubElement(bbox_elem, 'ymax').text = str(new_ymin + new_height)
                    annotation_root.append(new_object_elem)
                    num_copied = num_copied + 1
                    # 写入文件
                    imgsave_path = os.path.join(im_save, file)
                    labelsave_path = os.path.join(an_save, anno)

                    cv2.imwrite(imgsave_path, image)
                    annotation_tree.write(labelsave_path)
```
