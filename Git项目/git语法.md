## git命令

```
git init 					   #把当前目录变成git可以管理的仓库
git clone git地址 			  #克隆项目
git add readme.txt 			   #添加一个文件，也可以添加文件夹
git add -A 					   #添加全部文件
git rm test.txt 			   #删除一个文件，也可以删除文件夹
git commit -a -m “some commit” #提交修改
git status 					   #查看是否还有未提交
git log 						#查看最近日志
git reset --hard HEAD^ 			#版本回退一个版本
git reset --hard HEAD^^ 		#版本回退两个版本
git reset --hard HEAD~100 		#版本回退多个版本
git remote add origin +地址  		#远程仓库的提交（第一次链接）
git push -u origin master		#仓库关联
git push 						#远程仓库的提交（第二次及之后）
git fetch 						#从远程获取代码库
git tag xxx 					#打tag
git tag 						#显示所有tag
git push --tag 					#提交tag
git branch -a 					#显示所有分支
git checkout 分支名 		 	 #切换分支
git merge git分支 		  	 #合并分支

```

仓库建立流程：

此文章较为详细：

> [(277条消息) Gitee创建仓库与上传文件或代码_gitee新建仓库导入代码_みさかMimi的博客-CSDN博客](https://blog.csdn.net/m0_62800996/article/details/126679100?spm=1001.2101.3001.6650.8&utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-8-126679100-blog-128840294.235^v28^pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-8-126679100-blog-128840294.235^v28^pc_relevant_default&utm_relevant_index=11)



## 上传流程：

1.把文件放入缓存区

```
git add .
```

2.设置提交注释

```
 git commit -am "笔记捏"
```

3.提交文件到云

```
git push
```



## 把云端文件同步到本地

```
git pull origin master
```









