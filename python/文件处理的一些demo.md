## 遍历文件夹

```python
import os
# 遍历文件夹中的所有文件
def traverse_files(folder_path):
    for root, dirs, files in os.walk(folder_path):
        for file in files:
            file_path = os.path.join(root, file)
            # 对文件进行操作，这里可以根据需求进行自定义
            print(file_path)
# 示例：遍历当前文件夹下的所有文件
traverse_files('.')
```



## 批量处理图像输出到另外一个文件夹

```python
import cv2
import os
from tqdm import tqdm
import time

folder_path = "E:\\DeskTop\\photo\\smart car train\\road_img"
dst_path = "E:\\DeskTop\\photo\\smart car train\\road_img_2"
for root, dirs, files in os.walk(folder_path):
    for file in tqdm(files):
        file_path = os.path.join(root, file)
        img = cv2.imread(file_path,0)
        blur = cv2.GaussianBlur(img,(5,5),0)
        ret3,thimg = cv2.threshold(blur,0,255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)
        dstfile_path = os.path.join(dst_path,file)
        cv2.imwrite(dstfile_path,thimg)
```



## 文件批量移动

```python
import os
import glob
import shutil

source_folder_path = 'E:/Project/Python/smartcar/datasets/mydataset_aug2/train_img_fl/'
destination_folder_path = 'E:/Project/Python/smartcar/datasets/mydataset_aug2/'

# 获取指定文件夹中所有以".jpg"结尾的文件
jpg_files = glob.glob(os.path.join(source_folder_path, "*.jpg"))

# 遍历所有jpg文件并将它们移动到目标文件夹中
for jpg_file in jpg_files:
    # 构造目标文件的路径
    destination_path = os.path.join(destination_folder_path, os.path.basename(jpg_file))
    # 移动文件
    shutil.move(jpg_file, destination_path)

```

## 文件批量重命名

```python 
import os

# 定义文件夹路径和新文件名前缀
folder_path = 'E:/Project/Python/smartcar/datasets/mydataset_aug2/train_labels_fl/'
new_name_prefix = 'rotated'

# 获取文件夹下的所有文件名
file_list = os.listdir(folder_path)

# 遍历文件列表进行重命名
for i, file_name in enumerate(file_list):
    # 获取文件扩展名
    file_ext = os.path.splitext(file_name)[1]
    # 构建新文件名
    new_file_name = '{}_{}{}'.format(new_name_prefix, str(i+1), file_ext)
    # 拼接文件路径
    old_file_path = os.path.join(folder_path, file_name)
    new_file_path = os.path.join(folder_path, new_file_name)
    # 重命名文件
    os.rename(old_file_path, new_file_path)

```

## 对比两个文件夹中的文件名，移除不一样的文件（进行数据集对齐）

```python
import os

# 设置两个文件夹的路径
folder1 = "E:\\Project\\Python\\smartcar\\ourdataset\\DatasetId\\DatasetId_1787950_1679317159\\Annotations"
folder2 = "E:\\Project\\Python\\smartcar\\ourdataset\\DatasetId\\DatasetId_1787950_1679317159\\Images"

# 获取文件夹中的文件名
files1 = os.listdir(folder1)
files2 = os.listdir(folder2)

# 将文件名存储在集合中
file_names1 = set([os.path.splitext(file)[0] for file in files1])
file_names2 = set([os.path.splitext(file)[0] for file in files2])

# 找到文件名相同的文件
common_files = file_names1.intersection(file_names2)
print(common_files)
# 移除文件名不同的文件
for file in files1 + files2:
    file_name = os.path.splitext(file)[0]
    if file_name not in common_files:
        #os.remove(os.path.join(folder1, file))
        os.remove(os.path.join(folder2, file))
```

# 批量改变图像分辨率

```python
import cv2
import os
from tqdm import tqdm

curDir = os.curdir  # 获取当前执行python文件的文件夹
sourceDir = os.path.join(curDir, 'aruco_dataset')
resultDir = os.path.join(curDir, 'ed')

def resolution_lower_handler(sourceDir, resultDir):
    img_list = os.listdir(sourceDir)

    for i in tqdm(img_list):
        pic = cv2.imread(os.path.join(sourceDir, i), cv2.IMREAD_COLOR)
        pic_n = cv2.resize(pic, (1600, 1600))
        cv2.imwrite(os.path.join(resultDir, i), pic_n)

if __name__ == '__main__':
    resolution_lower_handler(sourceDir, resultDir)

```

