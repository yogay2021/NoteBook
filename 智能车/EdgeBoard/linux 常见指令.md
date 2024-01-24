## ls 列举当前目录下的文件

list-列举当前目录下的文件

![image-20230203201733304](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230203201733304.png)

## cd 切换文件路径

 Change Directory-切换文件路径

![image-20230203201817535](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230203201817535.png)

##  mkdir 新建一个新目录

Make Directory-新建一个新目录

![image-20230203201944343](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230203201944343.png)



## pwd 显示当前目录的绝对路径

 Print Working Directory-显示当前目录的绝对路径

![image-20230203202030240](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230203202030240.png)

## rm 删除给定的文件

 Remove-删除给定的文件

包含三种用法：

> 删除文件 rm [文件名]
>
> 删除文件夹及其子文件 rm -rf [文件夹名]
>
> 删除当前目录下的所有文件 和文件夹 rm -rf *

## mv 移动文件或修改文件名称

 Move-移动文件或修改文件名称

包含两种用法：

> 移动A文件或文件夹到B文件夹下 mv [A文件名] [B文件夹名] 
>
> 将A文件重命名为C文 件 mv [A文件名] [C文件名]

## cp 对文件进行复制

 Copy-对文件进行复制

包含两种用法：

> 复制文件A到文件夹B中 copy [A文件名] [B文件夹名]
>
> 复制文件夹C到文件夹B 中 copy -r [C文件夹名] [B文件夹名]



## cat 查看文件内容

concatenate and print files-查看文件内容

>  cat [文件名]

## tar 压缩文件

用法：

> 压缩文件为tar.gz文件 ：
>
>  tar -zcvf [test.tar(压缩后文件名)].gz [test(欲压缩的文件名)] 
>
> 解压文件：
>
>  tar -zxvf [test（解压文件名）].tar.gz

## ZIP压缩文件

zip -r test.zip 文件路径

## unzip 解压zip格式的压缩文件

 unzip [目标zip]



## vim 文本编辑器

包含三种模式：是命令模式（Command mode），输入模式（Insert mode）和底线命令模式 （Last line mode）

**使用vim建立一个test.txt文件**

> vim test.txt
>
> 输入 【  i 】 编写文件
>
> 按下ESC退出
>
> 输入  【 :wq 】  ，离开文本编辑

## reboot 重启系统



## poweroff 关机



