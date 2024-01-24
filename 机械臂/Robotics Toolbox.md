# 安装

在官网下载后缀为mltbx的工具箱文件：

>  http://www.petercorke.com/Robotics_Toolbox.html 

直接拖入matlab即可安装



# 快速上手

## 绘图

### 展示坐标系

```matlab
 trplot2(T1)
```

### 打点

```matlab
plot_point([1;1],'+')
```





## 二维位姿

### SE2 - 平移旋转矩阵生成

```matlab
T = SE2(x,y,theta)
%(x,y)表示平移 ，theta表示旋转角度（弧度制）
```

![image-20230504112005140](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230504112005140.png)

EG：

```matlab
>> T1 = SE2(0,0,0)
T1 = 
         1         0         0
         0         1         0
         0         0         1
>> T2 = SE2(1,3,pi/3)
T2 = 
    0.5000   -0.8660         1
    0.8660    0.5000         3
         0         0         1
>> trplot2(T1)
>> hold on
>> trplot2(T2)
```

![image-20230504133732527](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230504133732527.png)

## 三维位姿

### rotx - 绕X轴旋转矩阵生成

```matlab
>> TX = rotx(pi/4)
TX =

    1.0000         0         0
         0    0.9999   -0.0137
         0    0.0137    0.9999
>> trplot(TX)
```

![image-20230504150702417](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230504150702417.png)

![image-20230504150313726](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230504150313726.png)

### roty - 绕Y轴旋转

```matlab
>> TY = roty(pi/5)

TY =

    0.9999         0    0.0110
         0    1.0000         0
   -0.0110         0    0.9999
```

![image-20230504150923965](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230504150923965.png)

### rotz - 绕Z轴旋转

```matlab
>> TZ = rotz(pi/2)

TZ =

    0.9996   -0.0274         0
    0.0274    0.9996         0
         0         0    1.0000
```

![image-20230504151104023](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230504151104023.png)



## 建模



### Link - 创建一个连杆



![image-20230504205909285](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230504205909285.png)

> 关节角度，连杆偏距



























