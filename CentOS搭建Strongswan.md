假设你的服务器公网ip是`99.99.99.99` , 下文中出现的指令中的`“你的服务器公网ip”`替换成`99.99.99.99`

#### 1、安装strongswan

```python
yum install strongswan
```

#### 2、创建证书

```shell
strongswan pki --gen --outform pem > ca.key.pem
strongswan pki --self --in ca.key.pem --dn "C=CN, O=Org, CN=Org Me CA" --ca --lifetime 3650 --outform pem > ca.cert.pem
```

```shell
strongswan pki --gen --outform pem > server.key.pem
strongswan pki --pub --in server.key.pem --outform pem > server.pub.pem
strongswan pki --pub --in server.key.pem | strongswan pki --issue --lifetime 3601 --cacert ca.cert.pem --cakey ca.key.pem --dn "C=CN, O=Org, CN=Org Me CA" --san="99.99.99.99" --san="99.99.99.99" --flag serverAuth --flag ikeIntermediate --outform pem > server.cert.pem
```

注意：

1. 这里`C=CN, O=Org, CN=Org Me CA` Org指机构名字 Me是自己名字 随便填。
2. `--san="99.99.99.99" --san="99.99.99.99"`  如果已经有域名 可以把一个改成域名

#### 3、安装证书

```shell
cp -r ca.key.pem /etc/strongswan/ipsec.d/private/
cp -r ca.cert.pem /etc/strongswan/ipsec.d/cacerts/
cp -r server.cert.pem /etc/strongswan/ipsec.d/certs/
cp -r server.pub.pem /etc/strongswan/ipsec.d/certs/
cp -r server.key.pem /etc/strongswan/ipsec.d/private/
```

#### 4、配置VPN

```sh
vi /etc/strongswan/ipsec.conf
```

```shell
config setup  
    uniqueids=never #允许多个客户端使用同一个证书

conn %default  #定义连接项, 命名为 %default 所有连接都会继承它
     compress = yes #是否启用压缩, yes 表示如果支持压缩会启用.
     dpdaction = hold #当意外断开后尝试的操作, hold, 保持并重连直到超时.
     dpddelay = 30s #意外断开后尝试重连时长
     dpdtimeout = 60s #意外断开后超时时长, 只对 IKEv1 起作用
     inactivity = 300s #闲置时长,超过后断开连接.
     leftdns = 8.8.8.8,8.8.4.4 #指定服务端与客户端的dns, 多个用","分隔
     rightdns = 8.8.8.8,8.8.4.4

conn IKEv2-BASE
     leftca = "C=CN, O=Org, CN=Org Me CA" #服务器端根证书DN名称，与 --dn 内容一致 
     leftsendcert = always #是否发送服务器证书到客户端
     rightsendcert = never #客户端不发送证书

conn IKEv2-EAP  
     keyexchange=ikev2       #默认的密钥交换算法, ike 为自动, 优先使用 IKEv2
     left=%any       #服务器端标识,%any表示任意  
     leftid= 你的服务器公网ip     #服务器端ID标识，你的服务器公网ip(99.99.99.99)  
     leftsubnet=0.0.0.0/0        #服务器端虚拟ip, 0.0.0.0/0表示通配.  
     leftcert = server.cert.pem     #服务器端证书  
     leftauth=pubkey     #服务器校验方式，使用证书  
     right=%any      #客户端标识，%any表示任意  
     rightsourceip = 10.1.0.0/16    #客户端IP地址分配范围  
     rightauth=eap-mschapv2  #eap-md5#客户端校验方式#KEv2 EAP(Username/Password)   
     also=IKEv2-BASE
     eap_identity = %any #指定客户端eap id
     rekey = no #不自动重置密钥
     fragmentation = yes #开启IKE 消息分片
     auto = add  #当服务启动时, 应该如何处理这个连接项. add 添加到连接表中.
```

#### 5、修改 dns 配置

```shell
vi /etc/strongswan/strongswan.d/charon.conf
```

```shell
charon {
   duplicheck.enable = no #同时连接多个设备,把冗余检查关闭.

    # windows 公用 dns
    dns1 = 8.8.8.8
    dns2 = 8.8.4.4

    #以下是日志输出, 生产环境请关闭.
    filelog {
        /var/log/charon.log {
            # add a timestamp prefix
            time_format = %b %e %T
            # prepend connection name, simplifies grepping
            ike_name = yes
            # overwrite existing files
            append = no
            # increase default loglevel for all daemon subsystems
            default = 1
            # flush each line to disk
            flush_line = yes
        }
    }

}
```

```shell
vi /etc/strongswan/strongswan.conf
```

```shell
charon {
        load_modular = yes
        duplicheck.enable = no
        compress = yes
        plugins {
                include strongswan.d/charon/*.conf
        }
        dns1 = 8.8.8.8
        nbns1 =8.8.4.4
}
include strongswan.d/*.conf
```

#### 6、配置用户和密码

```sh
vi /etc/strongswan/ipsec.secrets
```

```shell
#ipsec.secrets - strongSwan IPsec secrets file

#使用证书验证时的服务器端私钥
#格式 : RSA <private key file> [ <passphrase> | %prompt ]
: RSA server.key.pem

#使用预设加密密钥, 越长越好
#格式 [ <id selectors> ] : PSK <secret>
admin : PSK "123456"

#EAP 方式, 格式同 psk 相同 (用户名/密码 例：oneAA/oneTT)
admin : EAP "123456"

#XAUTH 方式, 只适用于 IKEv1
#格式 [ <servername> ] <username> : XAUTH "<password>"
admin : XAUTH "123456"
```

注意：账号密码分别是 admin 123456 这个自己定义

#### 7、开启内核转发

```sh
vi /etc/sysctl.conf
```

配置里添加如下：

```sh
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding=1
```

#### 8、配置防火

```sh
vi /etc/firewalld/zones/public.xml
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <interface name="eth0"/>
  <service name="ssh"/>
  <service name="dhcpv6-client"/>
  <service name="ipsec"/>
  <port protocol="tcp" port="1723"/>
  <port protocol="tcp" port="47"/>
    <port protocol="tcp" port="1701"/>
    <port protocol="tcp" port="22"/>
    <masquerade/>
    <rule family="ipv4">
     <source address="10.1.0.0/16"/>
      <masquerade/>                                                                                                                                                                                    
    </rule>
    <rule family="ipv4">
      <source address="10.1.0.0/16"/>
      <forward-port to-port="4500" protocol="udp" port="4500"/>
    </rule>
    <rule family="ipv4">
      <source address="10.1.0.0/16"/>
     <forward-port to-port="500" protocol="udp" port="500"/>
   </rule>
  <masquerade/>
</zone>
```

#### 9、开启 防火墙/strongswan 以及 自动启动

```shell
systemctl enable firewalld
systemctl start firewalld
systemctl enable strongswan
systemctl start strongswan
```

#### 10、阿里云开放端口

> 登录阿里云管理控制台- -> 云服务器ECS- ->网络和安全- ->安全组- ->添加安全组规则：
> 授权策略：允许
> 协议类型：自定义UDP
> 端口范围：500/4500
> 授权类型：地址段访问
> 授权对象：0.0.0.0/0
> 优先级：100
> 描述：随便填

注意！添加完成后必须 重启 服务器

 

#### 11、证书安装及连接

 用ftp工具(例：FileZilla)下载 ca.key.pem 证书到本地。

​	

##### Windows10:

见另一篇文章https://blog.csdn.net/liyaxin2010/article/details/83148442



##### ios证书安装：

将 ca.cert.pem 用 ftp 导出 , 写邮件以附件的方式发到邮箱, 在ios Safari浏览器登录邮箱, 下载附件, 安装证书。

##### 设置-->VPN- ->添加VPN配置 

 例：类型：IKEv2
​        描述：随便填
​        服务器：你的服务器公网ip
​        远程ID：你的服务器公网ip
​        本地ID：不用填，空着
​        选择- ->用户名，填写-用户名-密码 - ->点击--完成

  回到VPN 界面- ->勾选你在描述里填写的内容显示- ->点击--连接



##### mac证书安装：

双击 ca.cert.pem -->选中你的证书-->显示简介-->信任-->始终信任(然后会弹框填写mac登录密码)。
步骤：系统编好设置- ->网络- ->点击+号- ->接口：VPN - ->VPN类型：IKEv2 - ->服务名称：随便- ->点击 创建。
接下来填写账户密码地址 

例：服务器地址：你的服务器公网ip                                                           
远程ID：你的服务器公网ip                                                           
本地ID：不用填，空着

点击- -鉴定设置- ->选择- ->用户名，填写-用户名-密码 - ->点击--连接



##### android：

去strongswan官网下载安装 

例：https://download.strongswan.org/Android/strongSwan-1.9.6.apk 
或：https://download.csdn.net/download/qq_29364417/10482582
或者编译源码：https://github.com/strongswan/strongswan/tree/master/src/frontends/android      
步骤：右上角选项-->CA证书-->再选择右上角选项-->导入证书-->找到ca.cert.pem点击即可。
​         回到主界面-->添加VPN配置-->例：服务器地址：你的服务器公网ip
​                                                           VPN类型：IKEv2 EAP(用户名/密码)，填写用户名和密码
​                                                           CA证书：选择刚才导入的ca.cert.pem证书
​                                                           点击右上角--保存

或者提前双击安装证书，这里选择自动。

![Screenshot_20181017-160536_1](E:\47.244.35.10-ftp\cacerts\Screenshot_20181017-160536_1.jpg)



参考：

https://blog.csdn.net/sqzhao/article/details/76093994

https://blog.csdn.net/wengzilai/article/details/78707134

https://blog.csdn.net/sqzhao/article/details/71307510

​	



