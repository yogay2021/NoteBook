## ‘读入图像

```python
import cv2
img = cv2.imread("E:/DeskTop/photo/dog2.jpg",cv2.IMREAD_GRAYSCALE)
# cv2.imread("图像绝对路径",写入图片的格式)
```

写入图像格式包含一下三种：

> 　　• cv2.IMREAD_COLOR  / 1：读入一副彩色图像。图像的透明度会被忽略，默认参数。
> 　　• cv2.IMREAD_GRAYSCALE / 0：以灰度模式读入图像
> 　　• cv2.IMREAD_UNCHANGED ：读入一幅图像，并且包括图像的 alpha 通道

---

## 显示图像

==**CV方法展示：**==

```python
cv2.imshow('image',img)
#cv2.imshow("图像框的名字",图像)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

> 如果希望调整显示的图像框的大小，可以添加下面代码：

```python 
cv2.namedWindow('image', cv2.WINDOW_NORMAL)
#cv2。namedWindow('图像框的名字',窗口的格式)
```

常用的窗口格式有：

> cv2.WINDOW_NORMAL : 可以自由调整窗口的大小
>
> cv2.WINDOW_AUTOSIZE ：窗口的大小根据图像本身固定

==**PLT方法展示：**==

```python
#plt.subplot(输出几行几列，图像位于第几个)
#plt.imshow(输出的图像)
#plt.title(输出标题)
plt.subplot(121),plt.imshow(img),plt.title('Input')
plt.subplot(122),plt.imshow(dst),plt.title('Output')
plt.show()
```

**==对于cv写入以及plt输出存在的色彩变化问题：==**

> 情形如下图所示，可以判断为是颜色通道有问题。
>
> 原因如下：
>
> cv2 读取的通道是 [B,G,R] 
>
> 而plt读取的通道是[R,G,B]
>
> 也就是说，以CV读入的图像的蓝色通道被作为红色通道值输出

![image-20230202171657962](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230202171657962.png)

> 解决方法如下：
>
> 将通道位置进行替换即可：
>
> ```python
> b,g,r = cv2.split(img_bgr)
> img_rgb = cv2.merge((r,g,b))
> ```

下面是一个以cv写入并以plt输出且修正通道的代码例子：

```python
img_bgr = cv2.imread("E:/DeskTop/photo/dog2.jpg",1)
b,g,r = cv2.split(img_bgr)
img_rgb = cv2.merge((r,g,b))
plt.subplot(131),plt.imshow(img_rgb),plt.title('Original')
plt.xticks([]), plt.yticks([])
```

---

## 保存图像

```python
cv2.imwrite("E:/DeskTop/photo/dog2_gray.jpg",img)
# cv2.imwrite("保存路径以及名字后缀"，保存的图像)
```

---

## 绘制

### 画线

```python
cv2.line(img,(0,0),(511,511),(255,0,0),5)
#cv2.line(图像，起点坐标，终点坐标，颜色（B,G,R），粗细)
```

### 画矩形

```python
cv2.rectangle(img,(0,0),(100,100),(0,0,255),4)
#cv2.rectangle(图像，起点坐标，终点坐标，颜色（B,G,R），粗细)
```

### 画圆

```python 
cv2.circle(img,(50,50),50,(0,0,255),4)
#cv2.circle(图像，圆心，半径，颜色（B,G,R），粗细)
```

## 视频流转图片

### **初始化**：cv2.VideoCapture()

> 用于打开摄像头并完成摄像头的初始化操作,视频文件的读取类似

```python
vc = cv2.VideoCapture('E:/DeskTop/智能车/document/模型图像.mp4')
```

### **检查初始化：**cv2.VideoCapture.isOpened() 

> 用`cv2.VideoCapture()`函数完成摄像头的初始化之后，为了防止初始化发生错误，用`cv2.VideoCapture.isOpened()`函数来检查初始化是否成功

```python
retval = cv2.VideoCapture.isOpened()
```

> 该函数会判断当前的摄像头是否初始化成功：
>
> - 如果成功：返回值retval为True
> - 如果不成功：返回值retval为False

### **捕获帧：**cv2.VideoCapture.read()

``` python
retval,image=cv2.VideoCapture.read()
```

> retval表示是否捕获成功，返回布尔类型。image返回捕获的帧信息（也就是图像）。如果没有捕获帧信息，该值为空

### 资源释放：cv2.VideoCapture.release()

```python
cv2.VideoCapture.release()
```

实例代码：

> 把视频流每一帧截取保存

```python
import cv2
vediox = cv2.VideoCapture('E:/DeskTop/photo/smart car train/tractor.mp4')
c = 0
rval = vediox.isOpened()
while rval:
    c = c + 1
    rval, frame = vediox.read()
    if rval:
        cv2.imwrite('E:/DeskTop/photo/smart car train/image/tractor/tractor' + str(c) + '.jpg', frame)
    else:
        break
vediox.release()
```

