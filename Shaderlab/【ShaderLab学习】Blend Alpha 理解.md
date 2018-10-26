####Alpha混合

``` GLSL {class=line-numbers}
Blend Off

CGPROGRAM

#pragma vertex vert
#pragma fragment frag

#include "Lighting.cginc"

sampler2D _MainTex;
float4 _MainTex_ST;

struct a2v {
    float4 vertex : POSITION;
    float4 texcoord : TEXCOORD0;
};

struct v2f {
    float4 position : SV_POSITION;
    float2 uv : TEXCOORD0;
};

```

源颜色**source color**用**S**表示
目标颜色**destination color**用**D**表示
<br/>

---

==marked==

```puml
A -> B : Say
```


深度缓冲（depth buffer | z-buffer）决定哪些物体渲染在前面，哪些物体渲染在后面。基本思想：根据深度缓冲中的值来决定该片元距离摄像机的距离（开启**深度测试**的前提下），当渲染这个片元时，把它的深度值和已经存在在深度缓冲中的值进行比较（开启**深度写入**的前提下），如果它的值距离摄像机更远，说明这个片元不用渲染（有物体挡住了它），否则，这个片元应该覆盖掉颜色缓冲中的像素值，并把它的深度值写入深度缓冲中（开启**深度写入**的前提下）。

要实现透明效果，第一种开启**透明度测试（Alpha Test）**，但是这种办法无法得到真正的半透明混合；另一种是**透明度混合（Alpha Blend）**。

####Alpha Test

https://docs.unity3d.com/Manual/SL-AlphaTest.html

采用一种比较极端的机制，只要一个片元的透明度不满足某个条件（通常是小于某阈值），就直接舍弃该片元。否则就会按照普通的不透明物体来处理，进行**深度测试**，**深度写入**。这样得到的结果是完全不透明，要么完全透明看不到。
    
*语义：*
```GLSL
AlphaTest Off
```
```GLSL
AlphaTest comparison AlphaValue[0-1]
```
| comparison | desc |
|-|-|
| Greater | 	Only render pixels whose alpha is greater than AlphaValue. |
| GEqual  | 	Only render pixels whose alpha is greater than or equal to AlphaValue. |
| Less    | 	Only render pixels whose alpha value is less than AlphaValue. |
| LEqual  | 	Only render pixels whose alpha value is less than or equal to from AlphaValue. |
| Equal   | 	Only render pixels whose alpha value equals AlphaValue. |
| NotEqual| 	Only render pixels whose alpha value differs from AlphaValue. |
| Always  | 	Render all pixels. This is functionally equivalent to AlphaTest Off. |
| Never   | 	Don’t render any pixels. |


示例:
```GLSL
Shader "Simple Alpha Test" {
    Properties {
        _MainTex ("Base (RGB) Transparency (A)", 2D) = "" {}
    }
    SubShader {
        Pass {
            AlphaTest Greater 0.5
            SetTexture [_MainTex] { combine texture }
        }
    }
}
```


####Alpha Blend

    
