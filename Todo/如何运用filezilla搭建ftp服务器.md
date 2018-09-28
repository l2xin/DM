## 如何运用filezilla搭建ftp服务器

 http://blog.sina.com.cn/s/blog_4cd978f90102xpph.html

![此博文包含图片](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)	

(2018-07-16 10:29:54)

[![img](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)转载*▼*](javascript:;)

| 标签： [ftp](http://search.sina.com.cn/?c=blog&q=ftp&by=tag) [filezilla](http://search.sina.com.cn/?c=blog&q=filezilla&by=tag) | 分类： [互联网知识](http://blog.sina.com.cn/s/articlelist_1289320697_2_1.html) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

如题，今天分享下我如何用filezilla搭建ftp服务器！

filezilla中文免费下载官方地址：https://www.filezilla.cn/

[![如何运用filezilla搭建ftp服务器](http://s12.sinaimg.cn/mw690/001pfRd7zy7m4vTs7R9ab&690)](http://photo.blog.sina.com.cn/showpic.html#blogid=4cd978f90102xpph&url=http://album.sina.com.cn/pic/001pfRd7zy7m4vTs7R9ab)
下载服务端后，直接双击进行安装！

默认点击下一步即可直至安装成功！

安装过程这里不再赘述，一直下一步，在跳出弹窗时勾选“Always connect to this server”，然后点击“Connect”即可（密码可自行设置）；
[![如何运用filezilla搭建ftp服务器](http://s15.sinaimg.cn/mw690/001pfRd7zy7m4BQJR5c4e&690)](http://photo.blog.sina.com.cn/showpic.html#blogid=4cd978f90102xpph&url=http://album.sina.com.cn/pic/001pfRd7zy7m4BQJR5c4e)



当出现：

Connecting to server 127.0.0.1:14147...

Connected, waiting for authentication

Logged on

表示登录成功，也表示filezilla安装成功！



中间遇到的问题：

![如何运用filezilla搭建ftp服务器](http://s13.sinaimg.cn/mw690/001pfRd7zy7m4CAFKiw6c&690)

问题与警告



问题：You appear to be behind a NAT router. Please configure the passive mode settings and forward a range of ports in your router.

解决方法：

“Edit”-“Setting”或直接点击设置按钮（齿轮）;

选择“Passive mode settings”选项卡，勾选“Use the following IP:”并填写服务器的IP地址，之后点击“OK”保存；

[![如何运用filezilla搭建ftp服务器](http://s4.sinaimg.cn/mw690/001pfRd7zy7m4CLtWanb3&690)](http://photo.blog.sina.com.cn/showpic.html#blogid=4cd978f90102xpph&url=http://album.sina.com.cn/pic/001pfRd7zy7m4CLtWanb3)



接下来的提示信息中不再提示上述问题；

另外上面的设置中如果没有设置“Use custom port range”，那么在客户端连接服务端读取目录时就会报以下的错误：响应: 425 Can't open data connection for transfer of "/"

这个问题主要是由于使用Passive Mode模式造成的。

解决方法：在上面的设置窗口中要勾选该项，设置端口范围，并在后面的防火墙设置中，将端口范围加入到入站端口中。



警告：Warning: FTP over TLS is not enabled, users cannot securely log in.

解决方法：启用TLS传输，具体操作如下：

“Edit”-“Setting”或直接点击设置按钮（齿轮）;

选择“FTP over TLS settings”选项卡，点击“Generate new certificate...”；

生成验证时Key size”根据自己的喜好选择即可，其他信息可以根据自己的情况随意填写，然后选择保存地址（最好放到安装路径下） “；



![如何运用filezilla搭建ftp服务器](http://s5.sinaimg.cn/mw690/001pfRd7zy7m4CRmDQM44&690)

名称默认为“certificate.crt”就好，最终选择生成；

提示“Certificate generated successfully”则说明生成没有问题，点击“确定”关闭弹窗；

点击“OK”保存设置；

之后的信息提示不再出现警告。



接下来就是创建“Group”，“Users”并设置“Shared folders”



不做详细解说，只需注意：



添加用户时为用户分配组；



为用户分配文件夹的权限，并指定Home文件夹（即“Set as home dir”，路径前出现“H”即可），如下图；

![如何运用filezilla搭建ftp服务器](http://s12.sinaimg.cn/mw690/001pfRd7zy7m4CUQlh99b&690)

端口设置



默认端口：21 加密端口：990 自定义的端口范围：10000-10200（根据自己的情况更改）

可以自行设置，但是需要注意的是无论使用什么端口，在后面一定要添加到防火墙的入站规则中去。



在本地安装客户端



通过客户端连接服务器就可以了。

初次连接时会提示如下，选择信任，确定即可。

[![如何运用filezilla搭建ftp服务器](http://s13.sinaimg.cn/mw690/001pfRd7zy7m4CXin5afc&690)](http://photo.blog.sina.com.cn/showpic.html#blogid=4cd978f90102xpph&url=http://album.sina.com.cn/pic/001pfRd7zy7m4CXin5afc)





当客户端连接ftp服务器时提示：

425 Can't open data connection for transfer of "/"



针对这个问题，我操作了两个步骤：

1.FileZilla FTP Server->Edit->Settings->Passive mode settings，指定被动模式使用的端口范围，将Use custom port range前面打开，设置端口范围为6000到6666，然后在Windows防火墙中打开这些端口。

[![如何运用filezilla搭建ftp服务器](http://s11.sinaimg.cn/mw690/001pfRd7zy7m4DE3kJYfa&690)](http://photo.blog.sina.com.cn/showpic.html#blogid=4cd978f90102xpph&url=http://album.sina.com.cn/pic/001pfRd7zy7m4DE3kJYfa)

2.在防火墙
[![如何运用filezilla搭建ftp服务器](http://s11.sinaimg.cn/mw690/001pfRd7zy7m4DTejeW0a&690)](http://photo.blog.sina.com.cn/showpic.html#blogid=4cd978f90102xpph&url=http://album.sina.com.cn/pic/001pfRd7zy7m4DTejeW0a)



OK，问题解决，客户端连接正常，可以列表。