# mac下自动保存 Git SSH Key 密码

原文：[https://blog.csdn.net/leeshunpeng/article/details/80518547](https://blog.csdn.net/leeshunpeng/article/details/80518547) 

首先尝试执行以下命令:
git config --global credential.helper osxkeychain

如果以上方法没有生效,则执行
ssh-add -K
或
ssh-add ~/.ssh/id_rsa
手动添加 Key 到 keychain中

但每次添加后，只在当前会话中有效，如果重启会话，会要求重新输入密码

为了不用每次都要输入密码，可以把命令卸载.bashrc 或者.zshrc 中，使得每次启动终端时，可以自动执行
