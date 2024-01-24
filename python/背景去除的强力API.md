使用的是backgroundremover，属于一种python库

在对应的虚拟环境中安装库

```text
pip install backgroundremover
```

在python中调用API，通过os去执行终端命令

```text
import os
os.system('backgroundremover -i "cg.jpg" -o "cg_output.jpg"')
```



实际的调用实例：

```python
import cv2
import os

os.system('backgroundremover -i "E:\\Project\\Python\\smartcar\\ourdataset\\data_outback\\dataset0\\dataset0\\tractor5.jpg" '
          '-o "E:\\Project\\Python\\smartcar\\ourdataset\\data_outback\\dataset0\\dataset_out\\tractor5.jpg"')
```

![image-20230401131100736](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230401131100736.png)
