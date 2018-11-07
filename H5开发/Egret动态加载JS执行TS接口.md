


## 问题

Egret使用TypeScript开发过程中，希望可以不重新运行游戏就能执行某些TS的接口,方面随时修改指令进行模拟操作。

## 代码如下

### Main.ts
``` typescript
var button = new eui.Button();
button.addEventListener(egret.TouchEvent.TOUCH_TAP, e=> {

    //一键运行指令
    let filePath = 'resource/run.js';
    if (RES.hasRes(filePath)) {
        RES.destroyRes(filePath, true);
    }

    RES.getResByUrl(filePath, (data, url) => {
        //演示调用js
        var obj: Object = eval(data);
        console.log("run_ts");
    }, this, RES.ResourceItem.TYPE_TEXT);

}, this);
```

### TestClassA.ts

``` typescript
class TestClassA{
    public id:number = 0;
    public run():any{
        console.log("TestClassA.run", this.id);
    }
}
```


### resource/run.js
``` js

Object({
    name: "哇哈哈",
    size: 1024,
    arr: ['a', 'bb', 'ccc'],
    say: function () {
        alert('我的名字叫:' + this.name);
    }
});

console.log("run js");

var a = new TestClassA();
a.id = 3;
a.run();
console.log(a);
```


## 注意

* `var obj: Object = eval(data);`这句通过eval()将RES.ResourceItem获得的字符串转换为Object格式，但是<font color=FF0000>**微信小游戏不支持eval等方法动态调用js脚本**</font>,所以这里也就基本只能是开发期间调使用了。

* `if (RES.hasRes(filePath)) {
        RES.destroyRes(filePath, true);
    }` 如果已经加载，先强行删掉。

## 参考
* [Egret-GDN-ts(Egret) 与 js 的调用](http://edn.egret.com/cn/docs/page/777)
* [Egret exml在微信小游戏这块儿的坑](https://blog.csdn.net/RICKShaozhiheng/article/details/80411516)
* [https://my.oschina.net/mixgame/blog/646816](https://my.oschina.net/mixgame/blog/646816)
* [http://developer.egret.com/cn/apidoc/index/name/RES.globalFunction](http://developer.egret.com/cn/apidoc/index/name/RES.globalFunction)


