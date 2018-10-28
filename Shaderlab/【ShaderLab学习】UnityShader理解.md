# Unity Shader基础理解



## 渲染管线—模型是如何绘制到屏幕上的

渲染管线主要分为：应用程序阶段、顶点处理、面处理、光栅化、像素处理。下面就来具体说说。

1、应用程序阶段：这个比较好理解，比如我们使用unity开始游戏，创建一个物体有，物体要正确显示，需要CPU计算好，物体的顶点坐标、法向量、纹理坐标、纹理等数据，然后传给通过数据总线传给图形硬件。

2、顶点处理：通过一系列的坐标系转换，将模型的顶点在摄像机前进行位移，并最终投影到摄像机的投影屏幕上。转换完了以后再进行些雾化、材质属性、和光照处理等。

​      ![顶点处理](/Users/l2xin/Documents/Images/Note/%E9%A1%B6%E7%82%B9%E5%A4%84%E7%90%86.png)

3、面处理：面包括剔除、截取等。剔除包括正面剔除、反面剔除等，剔除是为了效率考虑，不如有些显示不到的面，剔除后的话，不会再进行其它运算了。

4、光栅化：屏幕坐标是浮点数，像素坐标是整数，这里面转换就是光栅化。如下图，你大概也能看出来转换原理了。

​      ![光栅化](/Users/l2xin/Documents/Images/Note/%E5%85%89%E6%A0%85%E5%8C%96.png)

5、像素处理：对每个像素区域进行着色、对像素贴上贴图。



## Shader到底适合干啥？

**Shader**是给GPU执行的程序,中文叫着色器，直接运行在图形处理单元上，可以让我们直接操作显卡渲染功能。

UV动画，水, 雾 等一些效果, 这些用程序逻辑实现（CPU）比较困难，性能还不好；

举例：把一张图片置灰，插入一个像素Shader，每个像素在纹理着色的时候把整个RGBA求一个灰度出来，这样就变成一个灰色的图片，这就是一个典型的像素Shader的使用；

再比如：UV动画，在模型上面再贴一张图，不断地改变新贴图的纹理坐标，就产生滚动的效果，做Logo流光效果很合适。

**顶点vertex阶段： 干预模型形态**。模型顶点进来了以后，我们可以通过shader把顶点变换成另外一种形状，比如我们有一个网格，是直来直去的那种，我们可以让shader干预一下这个网格上的顶点，让它变得弯弯的像波浪那种的样子，再贴上纹理。　

**片元fragment阶段：干预像素着色**。当我们成像上去后，点的颜色是直接和贴图uv对应，还是根据灯光做光照运算，又或者随时间变化颜色渐变，就可以在这一阶段进行特殊处理。



## 目前面向GPU的编程语言主要有三种:

HLSL 语言 通过Direct3D编写的着色器程序，只能在Direct3D里面使用;
Cg 语言 NVIDIA和微软合作提供的语言,与C相似，Direct3D和opengl都支持；
GLSL语言 支持OpenGL上编写Shader程序;

Unity是跨平台的游戏引擎，所以它重点支持cg语言，但是它又不直接使用cg语言，而是自己搞一个shaderLab语言来做着色程序的编写，这样我们就可以使用Unity的shaderLab的语法来生成各种各样平台不同的着色程序。

我们下面所说的shader就是使用Unity的shaderLab来编写的着色程序。




## Unity3D的四种Shader

1. **Fixed function shader** 固定渲染管线Shader, 基本用于高级Shader在老显卡无法显示时的备用Shader（弃用）。

2. **Vertex and Fragment Shader** 最强大的Shader类型，属于可编程渲染管线。使用的是CG/HLSL语言。

3. **Surface Shader** Unity3d推荐的Shader类型。它是一个代码生成器，帮我们将重复的代码省去了，使得编写Shader更为容易。使用的也是CG/HLSL语言。

4. **Compute Shader** 这是Unity3D新增的一种。看下度娘百科对它的介绍：

   > Compute Shader技术是微软DirectX 11 API新加入的特性，在Compute Shader的帮助下，程序员可直接将GPU作为并行处理器加以利用，GPU将不仅具有3D渲染能力，也具有其他的运算能力，也就是我们说的GPGPU的概念和物理加速运算。

​          Compute Shader 使用HLSL语言。



## Shader Lab基本格式

1: 定义一个Shader,每一个着色程序都要有一个Shader
　　Shader “name” { // name shader名字
　　// 定义的一些属性，定义在这里的会在属性查看器里面显示; 
　　[Propeties] 
　　// 子着色器列表，一个Shader必须至少有一个子着色器; 
　　Subshaders: {....}
　　// 如果子着色器显卡不支持，就会降级,即Fallback操作;
　　[Fallback]
}

Properties

1:name(“display name”, type) = 值;
　　name指的是属性的名字，Unity中用下划线开始_Name;
　　display name是在属性检查器的名字;
　　type: 这个属性的类型
　　值: 只这个属性的默认值;

2: 类型:
　　Float, Int, Color(num, num, num, num)(0 ~ 1) Vector(4维向量), Range(start, end)
　　2D: 2D纹理属性;
　　Rect: 矩形纹理属性;
　　Cube: 立方体纹理属性;
　　3D: 3D纹理属性;
　　name(“displayname”, 2D) = “name” {options}

3: Options: 纹理属性选项
　　TexGen:纹理生成模式,纹理自动生成纹理坐标的模式;顶点shader将会忽略这个选项; 
　　ObjectLinear, EyeLinear, SphereMap, CubeReflect CubeNormal
　　LightmapMod: 光照贴图模式如果设置这个选项,纹理会被渲染器的光线贴图所影响。

例子

1: _Range (“range value”, Range(0, 1)) = 0.3; // 定义一个范围
2: _Color(“color”, Color) = (1, 1, 1, 1); // 定义一个颜色
3: _FloatValue(“float value”, Float) = 1 // 定义一个浮点
4: _MainTex (“Albedo”, Cube) = “skybox” {TexGen CubeReflect} // 定义一个立方贴图纹理属性;

Fallback

1:降级: 定义在所有子着色器之后,如果没有任何子着色器能运行，则尝试降级;
2: Fallback “着色器名称”;
3: Fallback Off;
没有降级，并且不会打印任何警告;



## 参考：

https://docs.unity3d.com/Manual/SL-Shader.html

https://blog.csdn.net/jxw167/article/details/54695181

 

 