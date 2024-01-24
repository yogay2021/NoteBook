

# 嵌入式HAL



![image-20230113095748973](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230113095748973.png)

## 实验一 

根据原理在STM32CUBEMX中配置管脚

### 固定频率闪亮LED

led电路原理图：

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221213112642232.png" alt="image-20221213112642232" style="zoom:67%;" />

代码块：

```c
HAL_GPIO_TogglePin(GPIOA,GPIO_PIN_5);
HAL_Delay(200);
```

_每隔200ms翻转PA5输出的电平_

---

### 按下按钮时点亮LED

按键电路原理图：

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221213112809374.png" alt="image-20221213112809374" style="zoom: 50%;" />

代码块：

```c
		if(HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_13) == 0)
		{
			HAL_GPIO_WritePin(GPIOA,GPIO_PIN_5,GPIO_PIN_SET);
		  HAL_Delay(120); //延迟以实现防抖
		}
			
		else
		{
			HAL_GPIO_WritePin(GPIOA,GPIO_PIN_5,GPIO_PIN_RESET);
			HAL_Delay(120);
		}
```

简单的ifelse判断按钮是否按下，然后分情况对LED管脚进行使能

---

### 键盘识别读取

键盘原理：

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221213233813311.png" alt="image-20221213233813311" style="zoom:50%;" />

管脚配置：

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221213233904845.png" alt="image-20221213233904845" style="zoom:33%;" />

调试思路：

每一行配置为输出，高电平，每一列配置为输入，上拉。当有按键按下时，每一列对应的输入都是高电平。  

依次对每一个行输出低电平 0，如果对应的按键没有按下，则列的输入仍然是高电平，如果对应的按键被按下，则列的输入是低电平，由此可以确定是那个按键按下。  

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20221213234133238.png" alt="image-20221213234133238" style="zoom:50%;" />

代码块：

```c
const char keymap[4][4] =
		{
			{'A','B','C','D'}, 
			{'E','F','G','H'}, 
			{'I','J','K','L'}, 
			{'M','N','O','P'} 
		};
		char key[4][4]=
			{{'0','0','0','0'},
			 {'0','0','0','0'}, 
			 {'0','0','0','0'}, 
			 {'0','0','0','0'}
			};
		
		int scanVal;
		for(int i=0;i<=3;i++){
			for(int j=0;j<=3;j++){
				
				if (j==0){
					HAL_GPIO_WritePin(GPIOB,GPIO_PIN_9,GPIO_PIN_RESET);}
				else if (j==1){
					HAL_GPIO_WritePin(GPIOB,GPIO_PIN_8,GPIO_PIN_RESET);}
				else if (j==2){
					HAL_GPIO_WritePin(GPIOA,GPIO_PIN_12,GPIO_PIN_RESET);}
				else if (j==3){
					HAL_GPIO_WritePin(GPIOA,GPIO_PIN_11,GPIO_PIN_RESET);}
				
				if(i==0){
					scanVal=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);}
				else if(i==1){
					scanVal=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);}
				else if(i==2){
					scanVal=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_8);}
				else if(i==3){
					scanVal=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_9);}
				
				if(scanVal==0)
					{
						key[i][j] =keymap[i][j] ;
					  HAL_Delay(200);
						
					if (j==0)
						{
						HAL_GPIO_WritePin(GPIOB,GPIO_PIN_9,GPIO_PIN_SET);
						}
					else if (j==1)
						{
						HAL_GPIO_WritePin(GPIOB,GPIO_PIN_8,GPIO_PIN_SET);
						}
					else if (j==2)
						{
						HAL_GPIO_WritePin(GPIOA,GPIO_PIN_12,GPIO_PIN_SET);
						}
					else if (j==3)
						{
						HAL_GPIO_WritePin(GPIOA,GPIO_PIN_11,GPIO_PIN_SET);
						}
					break;
				  }					
				if (j==0)
					{
					HAL_GPIO_WritePin(GPIOB,GPIO_PIN_9,GPIO_PIN_SET);
					}
				else if (j==1)
					{
					HAL_GPIO_WritePin(GPIOB,GPIO_PIN_8,GPIO_PIN_SET);
					}
				else if (j==2)
					{
					HAL_GPIO_WritePin(GPIOA,GPIO_PIN_12,GPIO_PIN_SET);
					}
				else if (j==3)
					{
					HAL_GPIO_WritePin(GPIOA,GPIO_PIN_11,GPIO_PIN_SET);
					}
			}
		}
```

---

## 实验二

### 按键中断输入采集实验

**在CUBEMX中的配置：**

==GPIO：==

![image-20230112164603802](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230112164603802.png)

LED灯PA5设置为输出以及按键PC13设置为EXIT中断

==NVIC：==

![image-20230112164819991](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230112164819991.png)

中断使能

==RCC时钟开启：==

![image-20230112164935057](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230112164935057.png)

**代码功能实现**

中断代码写在回调函数:(位于gpio.c)

```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
```

==**实现按键按下一次后亮灯，按键第二次后灭灯 :**==

```C
if( GPIO_Pin == GPIO_PIN_13)
{
		if( HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_13) == 0)
		{
			HAL_GPIO_TogglePin(GPIOA,GPIO_PIN_5);
		}
}
```

**==实现按键按下时亮灯，弹起后灭灯 ：=**=

```c
if( GPIO_Pin == GPIO_PIN_13)
	{
		while(1)
		{
		  if( HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_13) == 0)
		 {
			HAL_GPIO_WritePin(GPIOA,GPIO_PIN_5,GPIO_PIN_SET);
		 }
		 if( HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_13) == 1)
		 {
			HAL_GPIO_WritePin(GPIOA,GPIO_PIN_5,GPIO_PIN_RESET);
		 }
		}
	}
```

==**使用 diable_irq 关闭中断，尝试按键按下一次是否亮灯  :**==

在main函数里写入中断屏蔽：

```c
__disable_irq();
```

按下后不再亮灯，没有中断。

==**使用 disable_irq 关闭中断，delay()5-10 秒后，enable_irq，不断按按键，观察灯的状态**==

```c
	__disable_irq(); 
	HAL_Delay(8000);
	__enable_irq();
```

期间没有变化，灯始终不亮。

---

## 实验三

### 设置定时中断

==**用定时器TIM6，设计一个1s的定时中断，在中断中，翻转PA5的灯。**==

**CUBEMX配置：**

==GPIO：==

LED配置PA5输出即可

RCC：

配置HSE开启

TIM:

配置定时器TIM2，预分频以及装载周期

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230112165858807.png" alt="image-20230112165858807" style="zoom:50%;" />

此处时钟的频率为16MHZ

<img src="https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230112165940717.png" alt="image-20230112165940717" style="zoom:50%;" />

**思路：**

观察时钟树，配置预分频Prescaler以及周期Counter Period。

具体数值要考虑时钟树的主线频率，计算方法如下：

```
f(频率)=timer_clock/[(Prescaler+1)*(Counter_Period+1)]
```

具体的时间间隔也就是: ` T = 1 / f`

**代码：**

1.在main.c 的main函数中启用定时器：

```c 
HAL_TIM_Base_Start_IT(&htim2);
```

2.在tim.c中新建回调函数执行中断内的动作：

```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
	if(htim==&htim2)  
	{
		HAL_GPIO_TogglePin(GPIOA,GPIO_PIN_5);
	}
}
```

### 定时器输出方波

==**采用定时器TIM2 的通道1（PA5）输出一个1Khz的方波**==

配置A5输出串口为TIM2_CHANNEL1,配置PWM输出。

在main函数中开启PWM输出功能：

```c
HAL_TIM_PWM_Start(&htim2,TIM_CHANNEL_1);
```

==**将PA5输出的方波接到TIM3的输入捕获管脚，计算PA5方波的频率**==





## HAL_GPIO 常用库函数

### HAL_GPIO_Init

初始化引脚的工作模式：

实例：

```c
GPIO_InitTypeDef GPIO_InitStruct = {0};
GPIO_InitStruct.Pin = GPIO_PIN_5;
GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
GPIO_InitStruct.Pull = GPIO_NOPULL;
GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
```

### HAL_GPIO_ReadPin

读取引脚的电平状态、函数返回值为0或1:

实例：

```c
HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_13);
```

### HAL_GPIO_WritePin

给引脚写0或1的电平：

```c
HAL_GPIO_WritePin(GPIOA,GPIO_PIN_5,GPIO_PIN_RESET);
```

### HAL_GPIO_TogglePin

翻转引脚的电平状态:

```c
HAL_GPIO_TogglePin(GPIOA,GPIO_PIN_5);
```

























