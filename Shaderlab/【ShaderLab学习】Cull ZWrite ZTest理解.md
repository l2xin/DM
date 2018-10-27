

## ShaderLab: Culling & Depth Testing


![scene](F:/mine/PipelineCullDepth.png)

### Cull 表面剔除


``` GLSL
Cull Back | Front | Off
```

| ShaderLab  | Desc | 说明 |
|-|-|-|
|`Off`| Disables culling - all faces are drawn. Used for special effects.| 不剔除|
|`Back`| Don’t render polygons facing away from the viewer (default). |剔除背面（内表面）|
|`Front`|Don’t render polygons facing towards the viewer. Used for turning objects inside-out. | 剔除正面（外表面）|

----

### ZWrite

``` GLSL
ZWrite On | Off
```
> Controls whether pixels from this object are written to the depth buffer (default is On).

ZWrite可以取的值为：On/Off，**默认值为On**，代表是否要将像素的深度写入深度缓存中(同时还要看ZTest是否通过)。


### ZTest

#### 语义

```GLSL
ZTest Less | Greater | LEqual | GEqual | Equal | NotEqual | Always
```
> Controls whether pixels from this object are written to the depth buffer (default is On).

#### 作用探讨

ZTest可以取的值为：`Greater/GEqual/Less/LEqual/Equal/NotEqual/Always/Never/Off`，**默认值为LEqual**,**ZTest Off 等同于 ZTest Always**，代表通过比较深度来更改颜色缓存的值。例如当取默认值的情况下，如果将要绘制的新像素的z值小于等于深度缓存中的值，则将用新像素的颜色值更新深度缓存中对应像素的颜色值。

需要注意的是，当ZTest取值为Off时，表示的是关闭深度测试，等价于取值为Always，而不是Never！Always指的是直接将当前像素颜色(不是深度)写进颜色缓冲区中；而Never指的是不要将当前像素颜色写进颜色缓冲区中，相当于消失。


还是有点懵，再探讨探讨~~


* **什么是深度？**
    深度其实就代表该像素在世界空间中距离摄像机的距离。离相机越远，深度值(Z)越大;
    <br/>
* **什么是深度缓存**
    深度缓存中存储着`准备要绘制在屏幕上的像素点`的深度值。如果启用深度缓冲区，在绘制每个像素之前，会把该像素的深度值和深度缓冲区的深度值进行比较。如果<font color=#0000ff>*新像素深度值 < 深度缓存深度值*</font>，则新像素值取代本来该点的值，缓冲区的深度值也替换成新像素点的深度值；反之，<font color=#ff0000>新像素值被遮挡，其颜色值和深度将被丢弃</font>。
    <br/>
* **什么是深度测试**
    深度测试默认情况是将要绘制的新像素的z值与深度缓冲区对应位置的z值的进行比较，如果比深度缓存中的值小，就用新像素的颜色值更新深度缓冲中对应像素的<font color=#ff0000>颜色值</font>。
    <br/>
* **为什么需要深度**
    如果不使用深度测试，后绘制的物体会把先绘制的物体覆盖掉。有了深度缓冲后，绘制顺序就不那么重要，可以按照远近（z值）来正常显示。
    <br/>

#### 结论：
>1. ZTest通过，ZWrite为On：写入深度缓冲区，写入颜色缓冲区； 
>2. ZTest通过，ZWrite为Off：不写深度缓冲区，写入颜色缓冲区； 
>3. ZTest失败，ZWrite为On：不写深度缓冲区，不写颜色缓冲区； 
>4. ZTest失败，ZWrite为Off：不写深度缓冲区，不写颜色缓冲区；

#### 验证：
测试环境：
White Cube 坐标（1.5，0.5，1）
Blue Cube 坐标（0，0，0）
Cemera 坐标(0, 0 -10)
![scene](F:/mine/TestZWrite.png)

```GLSL {class=line-numbers}
Shader "Custom/ZTestBlue" 
{
	Properties
	{
		_MainTex("Base (RGB)", 2D) = "white" {}
	}
	SubShader
	{
		Tags 
		{ 
			"RenderType" = "Opaque" 
			"Queue" = "Geometry+200"
		}
		LOD 200

		ZWrite Off
		ZTest Off

		CGPROGRAM
		#pragma surface surf Lambert

		sampler2D _MainTex;

		struct Input
		{
			float2 uv_MainTex;
		};

		void surf(Input IN, inout SurfaceOutput o) 
		{
			half4 c = tex2D(_MainTex, IN.uv_MainTex);
			o.Albedo = c.rgb;
			o.Alpha = c.a;
		}
		ENDCG
	}
	FallBack "Diffuse"
}
```


```GLSL {class=line-numbers}
Shader "Custom/ZTestWhite" 
{
    //同上(调整ZWrite和ZTest进行测试)
}
```

#### 验证结果 : 
注意ZWrite默认值为On，ZTest默认值为LEqual。
```GLSL
"//ZWrite Off" == "ZWrite On"
"//ZTest Off" == "ZTest LEqual"
"ZTest Off" == "ZTest Always"
```
|BlueZWrite | BlueZTest | WhiteZWrite | WhiteZTest | 运行结果 | 分析 |
|-|-|-|-|-|-|
|`//Off` |`//Off`|`//Off`|`//Off`| ![](F:/mine/TestZWrite_0_0_0_0.png) |  头像离相机近ZTest通过，ZWrite为On,颜色值写入缓存，深度值写入缓存;白色值ZTest不通过颜色不写入，深度值也不写入，所以头像在前。  
|`Off` |`Off`|`Off`|`Off`| ![](F:/mine/TestZWrite_1_1_1_1.png)   |  头像ZTest通过,颜色值写入缓存，ZWrite为Off，深度值不写入;白色ZTest为Always所以颜色值写入，ZWrite为Off深度值不写入，所以交汇部分白色。 
|`Off` |`Off`|`//Off`|`//Off`| ![](F:/mine/TestZWrite_1_1_1_1.png)  |  头像ZTest通过,颜色值写入缓存，ZWrite为Off，深度值不写入;白色ZTest通过所以颜色值写入，ZWrite为On深度值写入，所以交汇部分白色。     
|`//Off` |`//Off`|`Off`|`Off`| ![](F:/mine/TestZWrite_1_1_1_1.png)   |  头像ZTest通过,颜色值写入缓存，ZWrite为On，深度值写入;白色ZTest通过所以颜色值写入，ZWrite为Off深度值不写入，所以交汇部分白色。        

其他类推，就不一一列举了。

<br/>

## 参考

* https://docs.unity3d.com/Manual/SL-CullAndDepth.html
