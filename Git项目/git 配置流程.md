# 配置
### 对主机git设置用户名和email地址：

$ git config --global user.name "用户名"

$ git config --global user.email "邮箱地址"

$ git config --global --list # 检查配置情况

### 和github建立ssh连接

1.检查主机的ssh密钥配置：

$ cd ~/.ssh

$ ls # 检查是否存在id_rsa以及id_rsa.pub

2.若无ssh配置，则生成

$ ssh-keygen -t rsa -C "xxx@xxx.com"

3.获取ssh密钥

$ cd ~/.ssh

$ cat id_rsa.pub

4.在github的setting里添加主机的ssh密钥

5.验证连接

$ ssh -T git@github.com

# 使用

1.在github创建仓库，获取git地址

2.在本地工作文件夹初始化仓库

$ git init

3.和远程仓库建立连接

$ git remote add origin git@github.com:yourName/repositoryname.git

4.本地上传至暂存区

$ git add . 

5.添加备注

$ git commit -am "备注信息"

6.上传到远程仓库

$ git push origin master


