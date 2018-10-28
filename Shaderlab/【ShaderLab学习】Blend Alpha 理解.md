# Blend Alpha

源颜色**source color**用**S**表示
目标颜色**destination color**用**D**表示
<br/>

---

# 前言
* **透明度测试**：它采用一种“霸道极端”的机制，只要一个片元的透明度不满足条件（通常是小于某个阈值），那么它对应的片元就会被舍弃。被舍弃的片元将不会再进行任何处理，也不会对颜色缓冲产生任何影响；否则，就会按照普通的不透明物体的处理方式来处理它，即进行**深度测试**，**深度写入**。也就是说，透明度测试是不需要关闭深度写入的，它和其他不透明物体最大的不同就是它会根据透明度来舍弃一些片元。虽然简单，但是它产生的效果也很极端，要么完全透明，即看不到，要么完全不透明。

* **透明度混合**：这种方法可以得到真正的半透明效果。它会使用当前片元的透明度作为混合因子，与已经存储在颜色缓冲中的颜色值进行混合，得到新的颜色。但是，透明度混合需要关闭深度写入，这使得我们要非常小心物体的渲染顺序。需要注意的是，透明度混合只关闭了深度写入，但没有关闭深度测试。这意味着，当使用透明度混合渲染一个片元时，还是会比较它的深度值与当前深度缓冲中的深度值，如果它的深度值距离摄像机更远，那么就不会再进行混合操作了。这一点决定了，当一个不透明物体出现在一个透明物体的前面，而我们先渲染了不透明物体，它仍然可以正常地遮挡住透明物体。也就是说，对于透明度混合来说，深度缓冲是只读的。


# Alpha Test

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

### 验证结果

#### 原始图片：
   ![图片取自冯乐乐女神的书](/Image/AlphaTestImage.png)

#### _Cutoff设置为0.65:
   ![_Cutoff=0.65](/Image/AlphaTest_001.png)


## 参考
* [https://docs.unity3d.com/Manual/SL-AlphaTest.html](https://docs.unity3d.com/Manual/SL-AlphaTest.html)
* [https://blog.csdn.net/candycat1992/article/details/41599167](https://blog.csdn.net/candycat1992/article/details/41599167)
* [https://blog.csdn.net/github_34181815/article/details/76285864](https://blog.csdn.net/github_34181815/article/details/76285864)

<br>


# Alpha Blend

    
