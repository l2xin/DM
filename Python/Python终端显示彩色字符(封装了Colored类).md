
# Python终端显示彩色字符(封装了Colored类)


## 问题         
有时候需要在终端显示彩色的字符，即根据需要显示不同颜色的字符串，比如我们要在终端打印一行错误提示信息，要把它弄成红色的。其实这个在Python中很好实现，使用转义序列来实现不同颜色的显示，转义序列以ESC开头，它的ASCII码八进制为 \033。

显示格式为：`\033[显示方式;前景色;背景色m`         

用这种原生的转义序列输出，在linux下完全支持，但是在windows下确存在兼容问题，比如在win10下可以正常显示颜色，在win7下确不支持。因此可以使用python标准库提供的colorama模块输出彩色字体，这个模块是跨平台的，内部实现也是采用转义序列来显示颜色的，只不过对windows平台做了特殊处理，因此完全兼容linux和windows各个版本。
         
## 解决办法
以下封装了一个Colored类，提供了两个版本：
* 第一个版本采用原生的转义字符序列输出各种颜。
* 第二个版本用python标准库的**colorama**模块兼容windows和linux。当要在终端打印彩色字体时直接调
用对应的方法即可，很方便。

## 一. Colored版本1:采用原生的转义字符序列---对windows有的版本不支持(比如win7)，linux完美支持
``` python
#coding:gbk
# ------------------------------------------------
#   python终端显示彩色字符类，可以调用不同的方法
# 选择不同的颜色.使用方法看示例代码就很容易明白.
# ------------------------------------------------
#
# 显示格式: \033[显示方式;前景色;背景色m
# ------------------------------------------------
# 显示方式             说明
#   0                 终端默认设置
#   1                 高亮显示
#   4                 使用下划线
#   5                 闪烁
#   7                 反白显示
#   8                 不可见
#   22                非粗体
#   24                非下划线
#   25                非闪烁
#
#   前景色             背景色            颜色
#     30                40              黑色
#     31                41              红色
#     32                42              绿色
#     33                43              黃色
#     34                44              蓝色
#     35                45              紫红色
#     36                46              青蓝色
#     37                47              白色
# ------------------------------------------------
class Colored(object):
    # 显示格式: \033[显示方式;前景色;背景色m
    # 只写一个字段表示前景色,背景色默认
    RED = '\033[31m'       # 红色
    GREEN = '\033[32m'     # 绿色
    YELLOW = '\033[33m'    # 黄色
    BLUE = '\033[34m'      # 蓝色
    FUCHSIA = '\033[35m'   # 紫红色
    CYAN = '\033[36m'      # 青蓝色
    WHITE = '\033[37m'     # 白色
 
    #: no color
    RESET = '\033[0m'      # 终端默认颜色
 
    def color_str(self, color, s):
        return '{}{}{}'.format(
            getattr(self, color),
            s,
            self.RESET
        )
 
    def red(self, s):
        return self.color_str('RED', s)
 
    def green(self, s):
        return self.color_str('GREEN', s)
 
    def yellow(self, s):
        return self.color_str('YELLOW', s)
 
    def blue(self, s):
        return self.color_str('BLUE', s)
 
    def fuchsia(self, s):
        return self.color_str('FUCHSIA', s)
 
    def cyan(self, s):
        return self.color_str('CYAN', s)
 
    def white(self, s):
        return self.color_str('WHITE', s)
 
# ----------使用示例如下:-------------
color = Colored()
print color.red('I am red!')
print color.green('I am gree!')
print color.yellow('I am yellow!')
print color.blue('I am blue!')
print color.fuchsia('I am fuchsia!')
print color.cyan('I am cyan!')
print color.white('I am white')

```

### 颜色对比图(根据需要自己设置对应的值):

![/Image/Python/print颜色.png](/Image/Python/print颜色.png)

### 运行效果:
![/Image/Python/print颜色-02.png](/Image/Python/print颜色-02.png)

<br>

## 二. Colored版本2:采用python标准库的colorama模块--兼容linux和windows各个版本:

``` python
# -----------------colorama模块的一些常量---------------------------
# Fore: BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE, RESET.
# Back: BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE, RESET.
# Style: DIM, NORMAL, BRIGHT, RESET_ALL
#
 
from colorama import  init, Fore, Back, Style
init(autoreset=True)
class Colored(object):
 
    #  前景色:红色  背景色:默认
    @staticmethod
    def red(s):
        return Fore.RED + s + Fore.RESET
 
    #  前景色:绿色  背景色:默认
    @staticmethod
    def green(s):
        return Fore.GREEN + s + Fore.RESET
 
    #  前景色:黄色  背景色:默认
    @staticmethod
    def yellow(s):
        return Fore.YELLOW + s + Fore.RESET
 
    #  前景色:蓝色  背景色:默认
    @staticmethod
    def blue(s):
        return Fore.BLUE + s + Fore.RESET
 
    #  前景色:洋红色  背景色:默认
    @staticmethod
    def magenta(s):
        return Fore.MAGENTA + s + Fore.RESET
 
    #  前景色:青色  背景色:默认
    @staticmethod
    def cyan(s):
        return Fore.CYAN + s + Fore.RESET
 
    #  前景色:白色  背景色:默认
    @staticmethod
    def white(s):
        return Fore.WHITE + s + Fore.RESET
 
    #  前景色:黑色  背景色:默认
    @staticmethod
    def black(s):
        return Fore.BLACK
 
    #  前景色:白色  背景色:绿色
    @staticmethod
    def white_green(s):
        return Fore.WHITE + Back.GREEN + s + Fore.RESET + Back.RESET
 
# print(Colored.red('I am red!'))
# print(Colored.green('I am gree!'))
# print(Colored.yellow('I am yellow!'))
# print(Colored.blue('I am blue!'))
# print(Colored.magenta('I am magenta!'))
# print(Colored.cyan('I am cyan!'))
# print(Colored.white('I am white!'))
# print(Colored.white_green('I am white green!'))
```

### 运行效果:
![/Image/Python/print颜色-03.png](/Image/Python/print颜色-03.png)

----

## 本文转自：[https://blog.csdn.net/qianghaohao/article/details/52117082](https://blog.csdn.net/qianghaohao/article/details/52117082)