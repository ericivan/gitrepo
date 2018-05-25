#### Centos 创建自定义git仓库



##### 安装git



> yum install git



或者自己去github下载git的源码编译也可以,https://github.com/git/git



##### 创建一个Git用户

```shell
# 创建用户

adduser git

# 设置密码

passwd git

#禁止shell登录

vim /etc/passwd

# 把最后一行最后的部分改成 git:/usr/bin/git-shell,注意如果是自己编译的话gitshell的路径不是这个哦
```



##### 创建客户端登录证书



> 这个我没弄出来,搞了半天不知道什么原因,先密码登录凉着..





##### 创建Git中间仓库



> 仓库创建在git的home目录

```shell
cd /home/git

#创建一个裸仓库

git init --bare test.git

# 修改文件所属用户组,注意这个非常重要,不然git操作的时候会报错

chown -R git:git test.git
```



##### 使用git hook

> 主要是利用git的钩子来实现自动部署



1.服务器初始化一个本地仓库,并且把本地仓库关联到中心仓库 



```shell
cd /var/www

git init

git remote add origin /home/git/test.git

# 该仓库也要改用户组哦

chown -R git:git /var/www
```



2.在中间仓库设置钩子

```shell
cd /home/git/test.git/hooks

vim post-receive
```

然后我们需要在刚才创建的文件里添加监本

```shell
cd /var/www
unset GIT_DIR
git pull origin master
```

编辑完成之后,我们给文件添加可执行权限,如果你是root用户操作,顺便也把用户组改一下

> chown +x post-receive

最后一步,我们可以在其他服务器clone我们的项目了

```shell
git cloen git@host:test.git
```
