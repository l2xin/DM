
# Python类声明私有变量和私有方法


## 类的私有变量和私有方法
在Python中可以通过在属性变量名前加上双下划线定义属性为private;添加_变成protected.

---

## 语法规则：特殊变量命名

* _xx 以单下划线开头的表示的是protected类型的变量。即保护类型只能允许其本身与子类进行访问。若内部变量标示，如： 当使用“from M import”时，不会将以一个下划线开头的对象引入 。

* __xx 双下划线的表示的是私有类型的变量。只能允许这个类本身进行访问了，连子类也不可以用于命名一个类属性（类变量），调用时名字被改变（在类FooBar内部，__boo变成_FooBar__boo,如self._FooBar__boo）

* __xx__定义的是特殊方法。用户控制的命名空间内的变量或是属性，如init , __import__或是file 。只有当文档有说明时使用，不要自己定义这类变量。 （就是说这些是python内部定义的变量名）

这里强调说一下私有变量：

python默认的成员函数和成员变量都是公开的,没有像其他类似语言的public,private等关键字修饰.但是可以在变量前面加上两个下划线"_",这样的话函数或变量就变成私有的.这是python的私有变量轧压(这个翻译好拗口),英文是(private name mangling.) 
**情况就是当变量被标记为私有后,在变量的前端插入类名,再类名前添加一个下划线"_",即形成了_ClassName__变量名.**


---

## Python内置类属性
* __dict__ : 类的属性（包含一个字典，由类的数据属性组成）
* __doc__ :类的文档字符串
* __module__: 类定义所在的模块（类的全名是'__main__.className'，如果类位于一个导入模块mymod中，那么className.__module__ 等于 mymod）
* __bases__ : 类的所有父类构成元素（包含了一个由所有父类组成的元组）


---

## 测试代码

``` python
class ClassA():

    _name = 'protected类型的变量'
    __info = '私有类型的变量'

    def _func(self):
        print("这是一个protected类型的方法")
    def __func2(self):
        print('这是一个私有类型的方法')
    def get(self):
        return(self.__info)


a = ClassA()
print(dir(a))
# print(a.get())
# print(a.__doc__)
# print(a.__dict__)
# print(a.__module__)

# a.__func2()
# print(a.__info)
# a._func()
# print(a._name)

```
---

## 执行结果

### protected类型的变量和方法 在类的实例中可以获取和调用

`print(a._name)`
> protected类型的变量

`print(a._func)`
> 这是一个protected类型的方法



### private类型的变量和方法

`print(a.get())`
> 执行结果：私有类型的变量

`print(dir(a))`
> 执行结果：['_ClassA__func2', '_ClassA__info', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_func', '_name', 'get']

`print(a.__dict__)`
> 执行结果：{}

`print(a.__doc__)`
> 执行结果： None

`print(a.__module__)`
> 执行结果：__main__


`print(a.__info)`
> AttributeError: 'ClassA' object has no attribute '__info'

`a.__func2()`
> AttributeError: 'ClassA' object has no attribute '__func2'

<br>
---

# 参考：
* [Python3 面向对象 http://www.runoob.com/python3/python3-class.html](http://www.runoob.com/python3/python3-class.html)
