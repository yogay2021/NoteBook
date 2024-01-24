# 基于HAAS 的物联网嵌入式技术学习

## ==**LED使用**==

### LED接口

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220165355449.png" alt="image-20221220165355449" style="zoom: 50%;" />

---

## ==**按键使用**==

> 当按下开关（四个开关）时，检测到低电平；当松开按键时，检测到高电平。

### 按键接口

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220175131597.png" alt="image-20221220175131597" style="zoom:50%;" />

### 实践案例

> 实现按下 KEY1 红灯亮起5秒后熄灭

main函数

```python
import utime   # 延时函数在utime库中
from driver import GPIO

gpio_irq = GPIO()
gpio_red = GPIO()

def irq_handler(data):
    global gpio_red
    gpio_red.write(1)
    utime.sleep_ms(5000)
    gpio_red.write(0)

gpio_irq.open("GPIO23")
gpio_red.open("redled")

while (True):
    gpio_irq.on(irq_handler)
```

json配置：

```pyhon
{
"name":"GPIO",
"io":{
  "GPIO23": {
    "type": "GPIO",
    "port": 23,
    "dir": "irq",
    "pull": "pullup",
    "intMode": "falling"
  },
  "redled": {
    "type": "GPIO",
    "port": 36,
    "dir": "output",
    "pull": "pullup"
  }
  }
}
```

---

## ==蜂鸣器==

### 蜂鸣器接口

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220202335583.png" alt="image-20221220202335583" style="zoom:33%;" />

### 实践案例

> 实现七个音节的播放

```python
from driver import PWM
from time import sleep

buzzer = PWM()

def buzzeStartRing(freq):
    global buzzer
    param = {'freq':freq, 'duty': 50 }
    buzzer.setOption(param)

def buzzeStopRing():
    global buzzer
    param = {'freq':1, 'duty': 1 }
    buzzer.setOption(param)

if __name__ == "__main__":
    Tone_CL = [525, 589, 661, 700, 786, 882, 990]
    for f in Tone_CL:
        buzzer.open("buzzer")
        buzzeStartRing(f)
        sleep(1)
        buzzer.close()
        buzzer.open("buzzer")
        buzzeStopRing()
        sleep(1)
        buzzer.close()
```

配置文件：

```python
{
"name":"GPIO",
"io":{
  "GPIO23": {
    "type": "GPIO",
    "port": 23,
    "dir": "irq",
    "pull": "pullup",
    "intMode": "falling"
  },
  "redled": {
    "type": "GPIO",
    "port": 36,
    "dir": "output",
    "pull": "pullup"
  },
  "buzzer": {
    "type": "PWM",
    "port": 0
  }
  }
}
```

---

## ==OLED显示==

### framebuf帧缓冲

#### 构建对象

```
framebuf.FrameBuffer(buffer, width, height, format, stride=width)
```

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220210509501.png" alt="image-20221220210509501" style="zoom: 33%;" />

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220210540613.png" alt="image-20221220210540613" style="zoom:33%;" />

#### 绘制形状

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220212444565.png" alt="image-20221220212444565" style="zoom:33%;" />

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220212458424.png" alt="image-20221220212458424" style="zoom:33%;" />

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220212510729.png" alt="image-20221220212510729" style="zoom:33%;" />

---

## ==传感器==







---

## ==GPIO==

### **创建GPIO对象**

```python
from driver import GPIO 
gpio = GPIO()
```

> GPIO对象创建成功，返回GPIO对象；GPIO对象创建失败，抛出ENOMEN异常。

### **打开GPIO对象**

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220165750684.png" alt="image-20221220165750684" style="zoom:50%;" />

### **关闭GPIO设备**

![image-20221220165959928](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220165959928.png)

### **获取GPIO设备输入电平**

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220170039876.png" alt="image-20221220170039876" style="zoom:50%;" />

### **设置GPIO设备输出电平**

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220170128167.png" alt="image-20221220170128167" style="zoom:50%;" />

### **设置GPIO设备中断回调函数**

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220175600544.png" alt="image-20221220175600544" style="zoom:50%;" />

> 回调函数在main中编写，配置项中的intmode即中断方式的定义

### **board.json中的GPIO类型设备属性配置项**

![image-20221220170223394](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220170223394.png)

实例代码：

```python
{
  "name":"GPIO23",
  "type": "GPIO",
  "port": 23,
  "dir": "irq",
  "pull": "pullup",
  "intMode": "falling"
}
```

---

## ==PWM==

### 创建PWM对象

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220202616766.png" alt="image-20221220202616766" style="zoom:50%;" />

### 打开PWM对象

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220202714011.png" alt="image-20221220202714011" style="zoom:50%;" />

### 关闭PWM对象

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220202850957.png" alt="image-20221220202850957" style="zoom:50%;" />



### 设置脉宽调制参数

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220203029709.png" alt="image-20221220203029709" style="zoom: 50%;" />

### 读取设置的参数

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220203107076.png" alt="image-20221220203107076" style="zoom:50%;" />



### json参数的配置

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220203201315.png" alt="image-20221220203201315" style="zoom:50%;" />

实例：

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220203223880.png" alt="image-20221220203223880" style="zoom: 50%;" />

---

## ==SPI==

### 创建SPI

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220205245291.png" alt="image-20221220205245291" style="zoom:50%;" />

### 打开SPI对象

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220205354296.png" alt="image-20221220205354296" style="zoom:50%;" />

### 关闭SPI对象

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220205638873.png" alt="image-20221220205638873" style="zoom: 50%;" />

### 接受数据

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220205804726.png" alt="image-20221220205804726" style="zoom:50%;" />

### 发送数据

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220205859964.png" alt="image-20221220205859964" style="zoom:50%;" />

### json配置

![image-20221220210015949](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220210015949.png)

实例：

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221220210036900.png" alt="image-20221220210036900" style="zoom: 67%;" />

