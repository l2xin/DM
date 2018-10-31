# 前言
刚开始用TypeScript，工作中可能要用到类似下面这样的json配置。

TestCfg.json文件如下:
``` json
{
	"1001": { "id": 1001, "name": "技能1" },
	"1002": { "id": 1002, "skillName": "技能2" }
}
```

# 实现



## 方法1

``` typescript
class ConfigManager{
    public static GetConfigByKey(tableName: string, key: number): any{
        ...
    }
}
```

这种方法接口调用:
``` typescript
let cfg = ConfigManager.GetConfigByKey("TestCfg_json", 1001);
```

`cfg.xxx`不会有任何提示。我们也看不到这个table有哪些属性，这就跟很烦了。

查了查Ts是支持泛型的，于是有了下面的想法：

## 方法2

配置表类，做成强类型，写好注释。。

``` typescript
interface IConfig{
    createByTuple(table:any);
}

/**
 * 测试配置表
 */
class TestCfg implements IConfig {
    /**这是id */
    public id: number;
    /**这是名字 */
    public name: string;

    createByTuple(jsonTable:any){
        this.id = jsonTable.id;
        this.name = jsonTable.name;
    }
}
```


按照C#的语法大抵应该是这个样子了：

``` c#
var testCfg = ConfigManager.GetConfigByKey<TestCfg>(1001);
```
然而语法很蛋疼，c#中传入泛型T是可以直接typeof(T)拿到类名称来做key，文件名直接通过传入的泛型参数计算得到，Ts却不行。查阅资料后得到解决方案。

#### 在泛型中使用类类型语法如下
``` typescript
function create<T>(c: {new(): T; }): T {
    return new c();
}

let configName: string = tType["name"];
```

这下问题解决！
<br></br>

### 完整代码如下：

``` typescript
/**
 * ConfigManager.ts 
 * @author l2xin
 */
class ConfigManager{

	private static m_allCfgDict = {};
	
	/**
	 * 读取某个表中某一行的数据
	 * @param tType T类
	 * @param key 下标
	 * @example let testCfg:TestCfg = GetConfigByKey<TestCfg>(TestCfg, 1001);
	 */
	public static GetConfigByKey<T extends IConfig>(tType: {new(): T}, key: number): T {
		let table = ConfigManager.GetAllConfig<T>(tType);
		if (table == null)
			return null;

		let result = table[String(key)];
		
		return result;
	}

	/**
	 * 读取某个表中所有数据
	 * @param tType T类
	 * @return 
	 * @example let allTestCfg = GetAllConfig<TestCfg>(TestCfg);
	 */
	public static GetAllConfig<T extends IConfig>(tType: {new(): T}) :{[id: string]: T }{
		let configName: string = tType["name"];
		if(this.m_allCfgDict[configName] == null){
			//加载单张表 用到再加载
			this.m_allCfgDict[configName] = {};
			let jsonTable = RES.getRes(configName + "_json");
			if (jsonTable != null) {
				for(let key in jsonTable){
					let oneCfg = new tType();
					oneCfg.createByTuple(jsonTable[key]);
					this.m_allCfgDict[configName][key] = oneCfg;
				}
			}
		}
		return this.m_allCfgDict[configName];
	}
}
```

#### 配置表文件 自动生成

``` typescript
/**
 * 所有配置表的类定义
 * @warning 自动生成 请勿手动修改
 * @author l2xin
 */
interface IConfig{
	createByTuple(table:any);
}

/**
 * 测试配置表
 */
class TestCfg implements IConfig {
	/**这是id */
	public id: number;
	/**这是名字 */
	public name: string;

	createByTuple(jsonTable:any){
		this.id = jsonTable.id;
		this.name = jsonTable.name;
	}
}

/**
 * 测试技能配置表
 */
class SkillCfg implements IConfig {
	/**这是id */
	public id: number;
	/**这是名字 */
	public skillName: string;

	createByTuple(jsonTable:any){
		this.id = jsonTable.id;
		this.skillName = jsonTable.skillName;
	}
}

```

#### 测试函数

``` typescript
private testLoadConfig(){
    let allTestCfg = ConfigManager.GetAllConfig<TestCfg>(TestCfg);
    for(let key in allTestCfg){
        let testCfg = allTestCfg[key];
        console.log(`GetAllConfig id:${testCfg.id} name:${testCfg.name}`);
    }

    let testCfg = ConfigManager.GetConfigByKey<TestCfg>(TestCfg, 1001);
    if(testCfg != null){
        console.log(`GetConfigByKey id:${testCfg.id} name:${testCfg.name}`);
    }

    let allSkillCfg = ConfigManager.GetAllConfig<SkillCfg>(SkillCfg);
    for(let key in allSkillCfg){
        let skillCfg = allSkillCfg[key];
        console.log(`GetAllConfig  SkillCfg id:${skillCfg.id} skillName:${skillCfg.skillName}`);
    }
    
}
```


### 运行
``` sh
GetAllConfig id:1001 name:白龙驹1 
GetAllConfig id:1002 name:白龙驹2  
GetConfigByKey id:1001 name:白龙驹1 
GetAllConfig  SkillCfg id:1001 skillName:技能1 
GetAllConfig  SkillCfg id:1002 skillName:技能2     
```


# 其他
* 如果希望一次把表全部加载
    ``` typescript
    /**
     * 如果希望一次把表全部加载
     */
    function InitAllConfig(){
        ConfigManager.GetAllConfig<TestCfg>(TestCfg);
        ConfigManager.GetAllConfig<SkillCfg>(SkillCfg);
    }
    ```
* 查了下语法支持**可以为null的类型** **可选参数**和**可选属性**,没想好能不能用的上,语法如下:

    [https://www.tslang.cn/docs/handbook/advanced-types.html](https://www.tslang.cn/docs/handbook/advanced-types.html)
    ``` typescript
    let sn: string | null = "bar";
    sn = null; // 可以

    function f(x: number, y?: number) {
        return x + (y || 0);
    }

    class C {
        a: number;
        b?: number;
    }
    ```

* module和namespace还没看太清楚，和预想的不一样，这个功能需要加个namespace.

* 配置表自动生成todo。。

# 后记

折腾折腾又半夜了，代码上传到github,睡觉.
[https://github.com/l2xin/EgretConfigManager](https://github.com/l2xin/EgretConfigManager)


# 参考
* [TypeScript学习笔记之 泛型]https://blog.csdn.net/yuzhiqiang_1993/article/details/54581822
* [TypeScript Generics(泛型)]https://www.cnblogs.com/ys-ys/p/5241783.html
* [https://www.tslang.cn/docs/handbook/generics.html](https://www.tslang.cn/docs/handbook/generics.html)
* [https://www.tslang.cn/docs/handbook/advanced-types.html](https://www.tslang.cn/docs/handbook/advanced-types.html)

