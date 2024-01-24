## 获取文件夹中的文件列表

```python
files = os.listdir(folder_path)
```

> 返回文件名

![image-20230413163549194](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230413163549194.png)



## 拼接文件路径

```python
file_newpath = os.path.join(folder_path, file)
```

> 返回拼接后的路径

![image-20230413163849978](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230413163849978.png)



## 移除文件

```python
os.remove(file_path)
```



## 创建文件夹

```python
os.mkdir(folder_path)
```



## 验证文件是否存在

```python
os.path.exists(folder_path)
```

> 返回bool



## 文件重命名

```python
os.rename(old_path, new_path)
```



## 文件的读写操作

### 打开文件

```python
file = open("file.txt", "w")
```



### 关闭文件

```python
file.close()
```



### 读取数据

```
with open('/data_process/type/score5.txt', 'r') as f:
    data = f.read().splitlines()
```

> splitline能够选择读取时分割的依据，如‘，’

![image-20230413170122265](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230413170122265.png)



### 写入数据

```python 
datalist = [1,2,3,4,5,6,7,8]
with open("E:\\DeskTop\\photo\\bubble\\small\\testhaha.txt", "w") as file:
    for item in datalist:
        file.write(str(item) + "\n")
```

> 若路径文件夹不存在，会自动创建一个新的文件。

![image-20230413170612653](https://yoga-typora-photo.oss-cn-beijing.aliyuncs.com/typora_img/image-20230413170612653.png)

















