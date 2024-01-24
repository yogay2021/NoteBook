## 生成

>流程上是获取先获取预定义的标记字典，然后再绘制字典中的标注信息

```python
import cv2
import numpy as np
dictionary = cv2.aruco.Dictionary_get(cv2.aruco.DICT_6X6_250)
Image = np.zeros((200, 200), dtype=np.uint16)
Image = cv2.aruco.drawMarker(dictionary, 1, 200, Image, 1)
cv2.imshow("marker", Image)
cv2.waitKey(0)
```

![image-20230511101530156](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230511101530156.png)



## 检测与定位
