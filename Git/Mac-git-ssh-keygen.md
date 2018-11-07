Mac git-ssh-keygen
===============================================


## Mac系统生成Git公钥

<br>

1.检查本机是否已有公钥

在终端中输入如下命令：
```
$ cd ~/.ssh
```
2.如果电脑中有以前遗留的密钥，将其删除掉

使用如下命令：
``` sh
$ mkdir key_backup
$ cp id_rsa* key_backup
$ rm id_rsa*
```

3.生成新的公钥

终端中输入如下命令

``` sh
$ ssh-keygen -t rsa -C "邮箱地址"
```
之后终端会提示几次密码设置，如果设置了密码，在向Git仓库进行代码交互操作时需要键入密码.

**注意：这里也可以全部回车带过，表示不需要密码，否则之后每次操作都需要输入密码。**

目录`Users/l2xin/.ssh/`之下有三个文件
* id_rsa	
* id_rsa.pub
* known_hosts git


4.向Git仓库中导入公钥
将`id_rsa.pub`中内容复制，在Git仓库中新建公钥，复制上去即可。


-----------


## 本地ssh设置 修改/etc/ssh/sshd_config 

``` sh
vi  /etc/ssh/sshd_config
```

``` sh
Protocol 2 (仅使用SSH2) 
PermitRootLogin yes (允许root用户使用SSH登陆，根据登录账户设置) 

ServerKeyBits 1024 (将serverkey的强度改为1024) 

PasswordAuthentication no (不允许使用密码方式登陆)

PermitEmptyPasswords no   (禁止空密码进行登陆) 

RSAAuthentication yes  （启用 RSA 认证） 

PubkeyAuthentication yes （启用公钥认证）

AuthorizedKeysFile   .ssh/authorized_keys 
```

然后就ok了：

``` sh
git clone git@github.com:用户名/项目名.git
```

## 其他

windows下也一样，生成的路径在c:/users/l2xin/ssh/。

----

## 参考
* [码云帮助中心-生成/添加SSH公钥](https://gitee.com/help/articles/4181)
* [码云帮助中心-Git配置多个SSH-Key](https://gitee.com/help/articles/4229)