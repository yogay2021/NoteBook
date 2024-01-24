## 定义工程名 PROJECT

```
PROJECT(projectname [C] [Java])
```

> 指定工程名字以及支持的语言，默认支持所有语言

## 变量定义 set

```
set(COMMON_LIB_DIR "${PROJECT_SOURCE_DIR}/lib/")
```

## 定义可执行文件 add_executable

``` 
ADD_EXECUTABLE(hello ${SRC_LIST})
```

> 定义一个名为hello的可执行文件，相关源文件是SRC_LIST中定义的源文件列表