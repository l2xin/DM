# AlphaTest & AlphaBlend


# 前言
* **透明度测试**：它采用一种“霸道极端”的机制，只要一个片元的透明度不满足条件（通常是小于某个阈值），那么它对应的片元就会被舍弃。被舍弃的片元将不会再进行任何处理，也不会对颜色缓冲产生任何影响；否则，就会按照普通的不透明物体的处理方式来处理它，即进行**深度测试**，**深度写入**。也就是说，透明度测试是不需要关闭深度写入的，它和其他不透明物体最大的不同就是它会根据透明度来舍弃一些片元。虽然简单，但是它产生的效果也很极端，要么完全透明，即看不到，要么完全不透明。

* **透明度混合**：这种方法可以得到真正的半透明效果。它会使用当前片元的透明度作为混合因子，与已经存储在颜色缓冲中的颜色值进行混合，得到新的颜色。但是，透明度混合需要关闭深度写入，这使得我们要非常小心物体的渲染顺序。需要注意的是，透明度混合只关闭了深度写入，但没有关闭深度测试。这意味着，当使用透明度混合渲染一个片元时，还是会比较它的深度值与当前深度缓冲中的深度值，如果它的深度值距离摄像机更远，那么就不会再进行混合操作了。这一点决定了，当一个不透明物体出现在一个透明物体的前面，而我们先渲染了不透明物体，它仍然可以正常地遮挡住透明物体。也就是说，对于透明度混合来说，深度缓冲是只读的。


# Alpha Test

## 官方文档

https://docs.unity3d.com/Manual/SL-AlphaTest.html

采用一种比较极端的机制，只要一个片元的透明度不满足某个条件（通常是小于某阈值），就直接舍弃该片元。否则就会按照普通的不透明物体来处理，进行**深度测试**，**深度写入**。
    
## 语义：

* `AlphaTest Off` 不测试，全渲染

* `AlphaTest Comparison AlphaValue[0-1]` 通过比较运算符得出是否渲染

| Comparison | Desc |
|-|-|
| Greater | 	Only render pixels whose alpha is greater than AlphaValue. |
| GEqual  | 	Only render pixels whose alpha is greater than or equal to AlphaValue. |
| Less    | 	Only render pixels whose alpha value is less than AlphaValue. |
| LEqual  | 	Only render pixels whose alpha value is less than or equal to from AlphaValue. |
| Equal   | 	Only render pixels whose alpha value equals AlphaValue. |
| NotEqual| 	Only render pixels whose alpha value differs from AlphaValue. |
| Always  | 	Render all pixels. This is functionally equivalent to AlphaTest Off. |
| Never   | 	Don’t render any pixels. |


## Surface Shader 示例:
``` C
Shader "Simple Alpha Test" {
    Properties {···}
    SubShader {
        Pass {
            AlphaTest Greater 0.5
            ···
        }
    }
}
```
``` C
Shader "Cutoff Alpha" {
    Properties {
        _Cutoff ("Alpha cutoff", Range (0,1)) = 0.5
    }
    SubShader {
        Pass {
            AlphaTest Greater [_Cutoff]
           ···
        }
    }
}
```

## Vextex & Fragment Shader 示例：

### 第一种
和Surface Shader中一样的语义，作用也相同。

### 第二种
在片元着色器中使用clip函数，clip函数是CG中的一个函数，定义如下：

**函数:** void clip(float4 x);void clip(float3 x);void clip(float2 x);void clip(float1 x);void clip(float x);
**参数:**裁剪时使用的标量或者矢量条件。
**描述:**如果给定参数的任何一个分量是负数，就会舍弃当前像素的输出颜色。等同于以下的代码：
``` C
void clip(float4 x)
{
    if(any(x < 0))
    {
        discard;
    }
}
```

``` GLSL
Shader "Custom/l2xin/T_AlphaTest"
{
    Properties 
    {
        _MainTex ("Albedo (RGB)", 2D) = "white" {}
        _Cutoff("Alhpa Cutoff", Range(0,1)) = 0.5
    }
    
    SubShader
    {
        Tags { "Queue"="AlphaTest" "IgnoreProjector"="True" "RenderType"="TransparentCutout"}
        
        Pass
        {
            Tags { "LightMode"="ForwardBase" }
            
            CGPROGRAM

            
            #pragma vertex vert
            #pragma fragment frag
            
            #include "Lighting.cginc"
            
            sampler2D _MainTex;
            float4 _MainTex_ST;
            fixed _Cutoff;            
            
            struct a2v{
                float4 vertex : POSITION;
                float4 texcoord: TEXCOORD0;
            };
            
            struct v2f{
                float4 pos : SV_POSITION;
                float2 uv : TEXCOORD0;
            };
            
            v2f vert(a2v v){
                v2f o;
                o.pos = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.texcoord, _MainTex);
                return o;
            }
            
            fixed4 frag(v2f i) :SV_Target{
                fixed4 texColor = tex2D(_MainTex, i.uv);
                //AlphaTest通过判断clip函数的参数，如果小于0本次fragment discard.
                clip(texColor.a - _Cutoff);
                return texColor;
            }
        
            ENDCG
        }
    }
    FallBack "Diffuse"
}

```

## 验证结果

### 原始图片：
   ![图片取自冯乐乐女神的书](/Image/AlphaTestImage.png)

### _Cutoff设置为0.65:
   ![_Cutoff=0.65](/Image/AlphaTest_001.png)

## 问题
AlphaTest得到的透明效果很极端，要么完全透明，要么完全不透明，就像在不透明的物体上挖了洞。而且，在边缘处效果参差不齐，因为边界处纹理的透明度变化的精度问题导致有锯齿，为了得到更加平滑的透明效果，考虑使用**透明度混合AlphaBlend**.


## 参考
* [https://docs.unity3d.com/Manual/SL-AlphaTest.html](https://docs.unity3d.com/Manual/SL-AlphaTest.html)
* [https://blog.csdn.net/candycat1992/article/details/41599167](https://blog.csdn.net/candycat1992/article/details/41599167)
* [https://blog.csdn.net/github_34181815/article/details/76285864](https://blog.csdn.net/github_34181815/article/details/76285864)

<br>


# Alpha Blend





**透明混合**使用当前片元的透明度作为混合因子，与已存储在颜色缓冲中的颜色值进行混合，得到新的颜色。需要注意的是，透明度混合需要关闭深度写入，这时候要注意物体的渲染顺序。 

* 源颜色（当前片元颜色）**source color**用**S**或者**SrcColor**表示
* 目标颜色（颜色缓冲中的颜色）**destination color**用**D**或者**DstColor**表示


## 官方文档：
[https://docs.unity3d.com/Manual/SL-Blend.html](https://docs.unity3d.com/Manual/SL-Blend.html)

## 语义：
首先，需要正确设置渲染队列：
``` GLSL
Tags { "Queue"="Transparent" }
```

其次，需要关闭ZWrite

``` GLSL
ZWrite Off
```

然后，可以指定混合函数，如：
``` GLSL
Blend SrcAlpha OneMinusSrcAlpha
```

|语义	|描述|
|-|-|
|Blend Off	|关闭混合|
Blend SrcFactor DstFactor	|开启混合，并设置混合因子。源颜色（片元颜色）乘以SrcFactor。而目标颜色（颜色缓存中的颜色）乘以DstFactor，将两者相加后存入颜色缓存。
Blend SrcFactor DstFactor，SrcFactorA DstFactorA |	和上面一样，只是使用不同的混合因子来混合透明通道
BlendOp BlendOperation	| 不是把源颜色和目标颜色简单相加后混合，而是使用BlendOperation对它们进行其他操作

## Blend operations 常用混合操作
|||
|-|-|
Add |　　将源和目标添加在一起。
Sub |　　从源中减去目标。 
RevSub | 从目标中减去源。 
Min 　|　使用较小的源和目标。 
Max 　|　使用较大的源和目标。


## Blend factors 混合因子
以下所有属性在Blend命令中都适用于SrcFactor＆DstFactor。 源指的是计算的颜色，目标是已经在屏幕上的颜色。 
*如果BlendOp使用逻辑运算，则忽略混合因子。* 
|||
|-|-|
One 　　　|　　　　　1 - 使用它让源或目标颜色完全通过 
Zero The 　|　　　　　0 - 使用此值删除源或目标值 
SrcColor 　　|　　　　源的Color(RGB)值 
SrcAlpha 　　|　　　　源的Alpha(a)值 
DstColor 　　|　　　　 目标的Color(RGB)值 
DstAlpha 　　|　　　　目标的Alpha(a)值 
OneMinusSrcColo　|　1 - source color. 
OneMinusSrcAlpha |　1 - source alpha. 
OneMinusDstColor |　1 - destination color. 
OneMinusDstAlpha　| 1 - destination alpha.

## 最常见的混合类型：

``` GLSL
Blend SrcAlpha OneMinusSrcAlpha // Traditional transparency　最常用的透明度混合
Blend One OneMinusSrcAlpha // Premultiplied transparency　预乘透明度
Blend One One // Additive　叠加的
Blend OneMinusDstColor One // Soft Additive　软叠加　
Blend DstColor Zero // Multiplicative
Blend DstColor SrcColor // 2x Multiplicative 两倍相乘 (2X Multiply) 
BlendOp Min //变暗
BlendOp Max //变亮
```

## 测试

Shader代码如下：
``` c
Shader "Custom/l2xin/T_AlphaBlend"
{
    Properties 
    {
        _MainTex ("Albedo (RGB)", 2D) = "white" {}
    }
    
    SubShader
    {
        Tags { "Queue"="Transparent" "IgnoreProjector"="True" "RenderType"="Transparent"}
        
        Pass
        {
            Tags { "LightMode"="ForwardBase" }
            
            //Alpha Blend
            Blend SrcAlpha OneMinusSrcAlpha
            
            CGPROGRAM

            
            #pragma vertex vert
            #pragma fragment frag
            
            #include "Lighting.cginc"
            
            sampler2D _MainTex;
            float4 _MainTex_ST;         
            
            struct a2v{
                float4 vertex : POSITION;
                float4 texcoord: TEXCOORD0;
            };
            
            struct v2f{
                float4 pos : SV_POSITION;
                float2 uv : TEXCOORD0;
            };
            
            v2f vert(a2v v){
                v2f o;
                o.pos = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.texcoord, _MainTex);
                return o;
            }
            
            fixed4 frag(v2f i) :SV_Target{
                fixed4 texColor = tex2D(_MainTex, i.uv);
                return texColor;
            }
        
            ENDCG
        }
    }
    FallBack "Diffuse"
}
```

### Blend SrcAlpha OneMinusSrcAlpha（最常用的半透明效果）

![Blend SrcAlpha OneMinusSrcAlpha](/Image/AlphaBlend_002.png)


## 更多展示效果

转自：[风宇冲Unity3D教程学院 http://blog.sina.com.cn/s/blog_471132920101d8z5.html](http://blog.sina.com.cn/s/blog_471132920101d8z5.html)

**在树叶使用的Shader中添加Blend代码**

* Blend zero one：仅显示背景的RGB部分，无Alpha透明通道处理。

   ![Blend SrcAlpha OneMinusSrcAlpha](/Image/AlphaBlend_003.png)

* Blend one  zero：  仅显示贴图的RGB部分，无Alpha透明通道处理。 A通道为0即本应该透明的地方也渲染出来了。
   
   ![Blend SrcAlpha OneMinusSrcAlpha](/Image/AlphaBlend_004.png)

* Blend one  one：贴图和背景叠加，无Alpha透明通道处理。仅仅是颜色rgb数值的叠加更趋近于白色即（1，1，1）了。
   
   ![Blend SrcAlpha OneMinusSrcAlpha](/Image/AlphaBlend_005.png)

* Blend SrcAlpha  zero：仅仅显示贴图，贴图含Alpha透明通道处理。但是贴图中的透明部分，即下图黑色部分没有颜色来显示，因为源颜色乘以alpha值0，为0；而混合目标的颜色乘以zero 0，也是0。所以透明部分显示的颜色为（0，0，0）
   
   ![Blend SrcAlpha OneMinusSrcAlpha](/Image/AlphaBlend_007.png)

* Blend SrcAlpha  OneMinusSrcAlpha：
最终颜色 = 源颜色 * 源透明值 + 目标颜色*（1 - 源透明值）
最常用的透明混合方式。贴图alpha值高的部分，显示得实，而混合的背景很淡。而alpha值高的部分，贴图显示得淡，而背景现实得实。

   ![Blend SrcAlpha OneMinusSrcAlpha](/Image/AlphaBlend_008.png)

## 参考
* [蛮牛https://docs.unity3d.com/Manual/SL-Blend.html](https://docs.unity3d.com/Manual/SL-Blend.html)
* [风宇冲http://blog.sina.com.cn/s/blog_471132920101d8z5.html](http://blog.sina.com.cn/s/blog_471132920101d8z5.html)
* [冯乐乐https://blog.csdn.net/candycat1992/article/details/41599167](https://blog.csdn.net/candycat1992/article/details/41599167)

<br><br>


# 后记-AlphaTest和AlphaBlend性能

Unity的官方文档中，提到了它们的性能问题——[https://docs.unity3d.com/Manual/SL-ShaderPerformance.html](https://docs.unity3d.com/Manual/SL-ShaderPerformance.html)

>       Fixed function AlphaTest or it’s programmable equivalent, clip(), has different performance characteristics on different platforms:
>* Generally it’s a small advantage to use it to cull out totally transparent pixels on most platforms.
> * However, on PowerVR GPUs found in iOS and some Android devices, alpha testing is expensive. Do not try to use it as “performance optimization” there, it will be slower.


> 总结一下，就是使用Alpha Test看似更简单，但其实在大多数平台上，相比与Alpha Blending，只有一点小小的性能提升。但是！！！在iOS和某些Android设备上，由于它们使用了PowerVR GPUs，因此Alpha Test的性能消耗反而会更大。因此，一个忠告就是，<font color=ff0000>尽可能使用Alpha Blending，而不要使用Alpha Test</font>。

> 我们会觉得很奇怪，没有关闭深度缓存，不需要计算混合颜色，仅仅调用来discard舍弃fragment不是非常简单的事吗？为什么在移动平台上反而效率更低呢？有句话叫，“简单粗暴”，可以用在这里。性能下降的原因就是它太粗暴了！（我乱说的你不要当真。。。）

> 好啦，言归正传~原因呢，就是之前提到的两遍检验。由于我经验有限，只能依靠强大的谷歌来找答案。我找到了这里、这里。总结一下，就是PowerVR GPUs使用了一种叫做“Deferred Tile-Based-Rendering”的技术。这种技术里有一个优化阶段，就是为了减少overdraw它会在调用fragment shader前判断哪些Tile是会被真正渲染的。也就说我们之前说的在FS之前做的“Depth Test”。但是，由于Alpha Test在fragment shader里使用了clip函数改变了fragment是否被渲染的结果，因此，GPUs就无法使用上述的优化策略了。也就是说，只要在完成了所有的fragment shader处理后，GPUs才知道哪些fragments会被真正渲染到屏幕上，这样，原先那些可以减少overdraw的优化就都无效了。


这一段摘自乐乐女神的博客:[https://blog.csdn.net/candycat1992/article/details/41599167 ](https://blog.csdn.net/candycat1992/article/details/41599167 )。我也没有理解到位，总之解困是尽可能使用**Alpha Blending**就对了。。






 