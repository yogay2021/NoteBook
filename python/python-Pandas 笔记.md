#  python-Pandas 笔记

## 基础操作

### 库导入

```+ python
import pandas as pd
```

### 读入数据集

```+python
pathx = ""
```

### 数据框的构造

```+ python
datalist = pd.read_csv(pathx, sep = '\t')
```

### 查看前n行的数据

```+ python
datalist.head(n)
```

### 查看数据行列大小

```+ python
datalist.shape #(行*列)
datalist.shape[0] #(行)
datalist.shape[1] #(列)
```

### 查看列名称

```python
datalist.columns
```

###  查看数据集的索引

```+ python
datalist.index
```

### 返回元素种类量

```+python
datalist['列名称'].nunique()
datalist.nunique()
```

### 求和

```+ python
求和结果 = datalist['列名称'].sum()
```





