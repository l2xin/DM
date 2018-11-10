
# Python遍历检测目录下Json文件是否合法

## 问题
解决非程序提交错误json导致功能异常，生成Json文件如果不合法不写入。

---

## JSON库简单介绍

使用 JSON 函数需要导入 json 库：import json。

|函数	|描述|
|-|-|
|json.dumps |	将 Python 对象编码成 JSON 字符串|
|json.loads	| 将已编码的 JSON 字符串解码为 Python 对象|
|json.load	| 传入文件路径读取+解码为 Python 对象|


详细使用：[Python JSON http://www.runoob.com/python/python-json.html](http://www.runoob.com/python/python-json.html)

----


## 遍历目录检查

``` python
#!/usr/bin/python
# -*- coding: utf-8 -*-

import os
import sys
import json

from Colored import Colored

# 检测json文件工具
# @author l2xin
class JsonCheckHelper:

    # 检测json目录下的所有.json文件是否合法 public 
    @staticmethod
    def checkJsonDir(rootdir):
        if not os.path.exists(rootdir):
            print(Colored.red("[jsonDir.check warning]]"),  "rootdir:%s not exists " % (Colored.red(rootdir)))
            return

        #列出文件夹下所有的目录与文件
        list = os.listdir(rootdir) 
        for i in range(0,len(list)):
            path = os.path.join(rootdir,list[i])
            if os.path.isfile(path):
                JsonCheckHelper.__checkJsonFile(path)
            else:
                JsonCheckHelper.checkJsonDir(path)
    
    # 检测单个.json文件是否合法 private
    @staticmethod
    def __checkJsonFile(filePath):
        if filePath.find(".json") == -1 :
            return

        with open(filePath, "r+", encoding='utf-8') as one_file:
            try:
                json.load(one_file)
                #两种写法一样效果
                #json.loads(one_file.read())
            except json.JSONDecodeError as err:
                print(Colored.red("[json.load Error]"), "filePath:%s  %s" % (filePath, err))

```

注：Colored 为打印辅助类,详情见另一篇: [
Python终端显示彩色字符(封装了Colored类)](https://blog.csdn.net/liyaxin2010/article/details/83927502)

---------

## 测试
``` python
JsonCheckHelper.checkJsonDir(sys.path[0] + "/json_client") 
```
### json_client目录不存在
> [jsonDir.check warning]] rootdir:/Users/l2xin/Documents/Gitee/TestH5/doc/json_client not exists

### json_client目录存在，遍历检测

>[json.load Error] filePath:/Users/l2xin/Documents/Gitee/TestH5/doc/jsonclient/gmconfig_client.json  Expecting ',' delimiter: line 10 column 71 (char 617)
[json.load Error] filePath:/Users/l2xin/Documents/Gitee/TestH5/doc/jsonclient/新建/gmconfig_client.json  Expecting ',' delimiter: line 10 column 71 (char 617)

<br>

-----------

## 参考:


* [python json模块 超级详解 https://www.cnblogs.com/tjuyuan/p/6795860.html](https://www.cnblogs.com/tjuyuan/p/6795860.html)

* [Python Json序列化与反序列化 https://www.cnblogs.com/diaosicai/p/6419833.html](https://www.cnblogs.com/diaosicai/p/6419833.html)

* [Python3 os.path()模块-文件/目录方法 http://www.runoob.com/python3/python3-os-path.html](http://www.runoob.com/python3/python3-os-path.html)