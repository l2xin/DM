
## RenderType

在Unity Shader中会经常在SubShader中使用Tags，其中就会涉及RenderType。

``` C {class=line-numbers}
SubShader{
    Tags{ "RenderType" = "Opaque" }
    ...
}
```

### 内置的RenderType标签：
 >  http://www.ceeger.com/Components/SL-ShaderReplacement.html

* Opaque: most of the shaders (Normal, Self Illuminated, Reflective, terrain shaders). 
用于大多数着色器（法线着色器、自发光着色器、反射着色器以及地形的着色器）。
* Transparent: most semitransparent shaders (Transparent, Particle, Font, terrain additive pass shaders). 
用于半透明着色器（透明着色器、粒子着色器、字体着色器、地形额外通道的着色器）。
* TransparentCutout: masked transparency shaders (Transparent Cutout, two pass vegetation shaders). 
* 蒙皮透明着色器（Transparent Cutout，两个通道的植被着色器）。
* Background: Skybox shaders. 天空盒着色器。
* Overlay: GUITexture, Halo, Flare shaders. 光晕着色器、闪光着色器。
* TreeOpaque: terrain engine tree bark. 地形引擎中的树皮。
* TreeTransparentCutout: terrain engine tree leaves. 地形引擎中的树叶。
* TreeBillboard: terrain engine billboarded trees. 地形引擎中的广告牌树。
* Grass: terrain engine grass. 地形引擎中的草。
* GrassBillboard: terrain engine billboarded grass. 地形引擎何中的广告牌草。


### 功能说明

> http://www.ceeger.com/Components/SL-ShaderReplacement.html

Unity可以运行时替换符合特定RenderType的Shader。主要通过Camera.RenderWithShader或者Camera.SetReplacementShader这两个接口来实现。


#### Camera.RenderWithShader

``` C
public void RenderWithShader(Shader shader, string replacementTag);
```

> https://docs.unity3d.com/ScriptReference/Camera.RenderWithShader.html

#### Camera.SetReplacementShader

``` C
public void SetReplacementShader(Shader shader, string replacementTag);
```

> https://docs.unity3d.com/Manual/SL-ShaderReplacement.html

#### 两个接口的区别

RenderWithShader与SetReplacementShader的区别是RenderWithShader是 **当前帧** 用指定的Shader渲染，SetReplacementShader是替换后的 **每一帧** 用指定的Shader渲染。

当把代码SetReplacementShader换成RenderWithShader发现没什么效果，主要是因为RenderWithShader是当前帧用指定的Shader渲染，要与SetReplacementShader同样的效果，必须每帧都调用RenderWithShader。注意必须在`OnGUI`函数里调用。
<br></br>

### 功能验证

调用`Camera.SetReplacementShader(Shader,"RenderType")`时，相机会使用指定的Shader来替代场景中的其他Shader来对场景进行渲染。


比如现在有以下几个Shader:

``` C
Shader "Shader1"{
    Properties(···)
    SubShader{
        Tags{ "RenderType"="Opaque" }
        Pass = {···}
    }

    SubShader{
        Tags{ "RenderType"="Transparent" }
        Pass = {···}
    }
}
```

场景中一部分物体使用的是Shader2:

``` C
Shader "Shader2"{
    Properties(···)
    SubShader{
        Tags{ "RenderType"="Opaque" }
        Pass = {···}
    }
}
```

另一部分物体使用的是Shader3:

``` C
Shader "Shader3"{
    Properties(···)
    SubShader{
        Tags{ "RenderType"="Transparent" }
        Pass = {···}
    }
}
```

#### 用法1

调用以下方法（*参数2为""*）：
``` C
Camera.SetPlacementShader(Shader1, "");
```
执行代码之后，场景中的所有物体都使用Shader1进行渲染。
<br></br>


#### 用法2

如果第二个参数不为空，如：
``` C
Camera.SetPlacementShader(Shader1, "RenderType");
```

这种情况下,首先在场景中找到标签中包含该字符串（这里为“RenderType”）的Shader,再去看该字符串对应的数值是否与Shader1中该字符串的值一致，**如果一致，则替代渲染，否则不渲染**。

上面几个Shader的代码，由于Shader2中包含"RenderType"="Opaque"，而且Shader1中的第一个SubShader中包含"RenderType"="Opaque"，因此将Shader1中的第一个SubShader替换场景中的所有Shader2，同理，将Shader1中的第二个SubShader替换场景中的所有的Shader3。
<br></br>


#### 用法3

另外，也可以自定义第二个参数，Shader代码如下:

``` C
Shader "Shader1"{
    Properties(···)
    SubShader{
        Tags{ "RenderType"="Opaque" "CheckRenderTypeTag="On" }
        Pass = {···}
    }

    SubShader{
        Tags{ "RenderType"="Transparent" "CheckRenderTypeTag="Off" }
        Pass = {···}
    }
}
```

``` C
Shader "Shader2"{
    Properties(···)
    SubShader{
        Tags{ "RenderType"="Opaque" "CheckRenderTypeTag="On" }
        Pass = {···}
    }
}
```

``` C
Shader "Shader3"{
    Properties(···)
    SubShader{
        Tags{ "RenderType"="Transparent" "CheckRenderTypeTag="Off" }
        Pass = {···}
    }
}
```

调用以下方法:
```
Camera.SetReplacementShader(Shader1, "CheckRenderTypeTag");  
```
最后的结果是，Shader1的第一个SubShader将会替换Shader2和Shader3(因为“CheckRenderTypeTag”对应的数值匹配)。

---

### 参考：
* https://docs.unity3d.com/ScriptReference/Camera.html
* https://docs.unity3d.com/Manual/SL-ShaderReplacement.html
* http://www.ceeger.com/Components/SL-ShaderReplacement.html
* https://blog.csdn.net/u013477973/article/details/80607989
* https://blog.csdn.net/zyq20130118/article/details/52816483
