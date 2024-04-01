## git命令


```$ git init``` 					                        #把当前目录变成git可以管理的仓库  
```$ git clone "git地址"``` 			                  #克隆项目  
```$ git add readme.txt``` 			                #添加一个文件，也可以添加文件夹  
```$ git add -A``` 					                      #添加全部文件  
```$ git rm test.txt``` 			                   #删除一个文件，也可以删除文件夹  
```$ git commit -a -m “some commit”```        #提交修改  
```$ git status``` 					                      #查看是否还有未提交
```$ git log``` 						                        #查看最近日志  
```$ git reset --hard HEAD^``` 			            #版本回退一个版本  
```$ git reset --hard HEAD^^``` 		            #版本回退两个版本  
```$ git reset --hard HEAD~100``` 		          #版本回退多个版本  
```$ git remote add origin +地址```  		        #远程仓库的提交（第一次链接）  
```$ git push -u origin master```		           #仓库关联  
```$ git push``` 						                       #远程仓库的提交（第二次及之后）  
```$ git fetch``` 						                      #从远程获取代码库  
```$ git tag xxx``` 					                     #打tag  
```$ git tag``` 						                        #显示所有tag  
```$ git push --tag``` 					                  #提交tag  
```$ git branch -a``` 					                   #显示所有分支  
```$ git checkout 分支名``` 		 	               #切换分支  
```$ git merge git分支``` 		  	                #合并分支  
```$ git diff```                              #查看本地与暂存区代码的区别










