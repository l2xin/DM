 

 

### 1.   控制面板打开->网络和Internet

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

### 2.   VPN –> 添加VPN连接

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

 

### 3.   VPN配置

​       VPN提供商-> 内置

​       连接名称：随便填

​       服务器地址：xx.xx.xx.xx（域名或者ip地址）

​       VPN类型：IKEV2 或者直接自动

​        登录信息的类型：用户和密码

​       账号密码

![1539849915479](C:\Users\liyaxin\AppData\Roaming\Typora\typora-user-images\1539849915479.png)



### 4.   修改注册表

Comman+R 打开运行输入regedit打开注册表

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image013.png)

 

 

 

在HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rasman\Parameters下添加

NegotiateDH2048_AES256  类型DWORD32 数值填1

 

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image015.jpg)

### 5.   安装证书

![1539849476283](C:\Users\liyaxin\AppData\Roaming\Typora\typora-user-images\1539849476283.png)



![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image021.jpg)

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image023.jpg)

 

#### 6.  连接

### ![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image025.jpg)



### 7. 如果显示已连接还是不能访问google.com

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image008.png)

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

 

 

### 8.   google.com

![img](file:///C:/Users/liyaxin/AppData/Local/Temp/msohtmlclip1/01/clip_image027.jpg)