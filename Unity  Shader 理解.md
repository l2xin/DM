# Unity  Shader 理解



网上有很多别人写好的shader，我们可以下载下来用或者修改学习。

Shader可以做出很多非常不错的效果，因为它是插在渲染管道里面的程序，一来是性能好，是GPU执行的，不需要CPU再去做额外的处理，二来就是可控性强，可以控制到每个顶点，每个像素的着色。

比如把一张图片置灰，插入一个像素Shader，每个像素在纹理着色的时候把整个RGBA求一个灰度出来，这样就变成一个灰色的图片，这就是一个典型的像素Shader的使用。

 

Shader

1: Shader是给GPU执行的程序,中文叫做着色器;
2: 着色器是运行在图形处理单元上，可以让开发人员直接操作图形硬件渲染功能；
3: shader能开发出很多好的效果，UV动画，水, 雾 等一些特效, 这些用程序开发出来(cpu)比较困难，性能还不好;
4: 渲染流水线, 模型投影, 定点着色;
5: shader一般主要有: 固定管线着色器, 顶点片元着色器, 表面着色器;
固定管线着色器(慢慢会被淘汰);
顶点shader: 干预模型形态的shader; 
像素shader: 干预像素着色的shader;
6: 模型定点运算的时候，可以加入顶点shader来干预顶点的位置;
顶点着色的时候，加入像素shader来干预像素的上色;

 

UV动画：在模型上面再贴一张图，不断地改变这个新贴图的纹理坐标，就产生不断滚动的效果，就像cocos案例里面乌龟在水里游，身体被波光覆盖的那个动画效果。

渲染流水线：一个3D模型拿到GPU里面渲染，有一个工作流水线，正是因为有了这些流水线，GPU才公开一些接口出来，方便程序员在流水线通道里面插入自己的代码。也就是说支持用户自己写代码插入流水线通道里面，来实现用户需要的特殊效果。

顶点shader：模型顶点进来了以后，我们可以通过shader把顶点变换成另外一种形状，比如我们有一个网格，是直来直去的那种，我们可以让shader干预一下这个网格上的顶点，让它变得弯弯的像波浪那种的样子，再贴上纹理。　　　　　

　　　　　　正弦波

像素shader：当我们成像上去后，要对点进行上色，哪个点要贴哪个纹理，所以需要对它们进行上色，上色的时候也可以用shader进行处理，比如刚才的乌龟的UV动画，把另外一个纹理和原来纹理结合。

　　　　　　岩浆，本来做好了纹理的，但是是静态的，我们可以通过动态地改变纹理坐标，让岩浆流动起来，实际上是改变了每个顶点着色的纹理坐标

 

 

Direct3D和opengl

1: 什么是Direct3D和opengl;
2: 目前面向GPU的编程语言主要有三种:
HLSL 语言 通过Direct3D编写的着色器程序，只能在Direct3D里面使用;
Cg 语言 NVIDIA和微软合作提供的语言,与C相似，Direct3D和opengl都支持；
GLSL语言 支持OpenGL上编写Shader程序;
3: Unity使用ShaderLab来进行着色程序的编写，对不同的平台进行编译，重点支持Cg语言;

 

显卡要支持Direct3D什么什么的......

Direct3D和opengl：PC发展史，PC实际上是继承在操作系统上面的，PC上面有各种各样的硬件在发展，如显卡，显卡就提出越来越多的把很多的图形单元放在GPU上面运算，可是我的显卡虽然支持这种计算，但是怎样告诉程序，所以要把显卡的功能做出一组API出来，公布给程序使用。

 

现在有两个端口，一个是操作系统是OS：Windows，Linux，一个是显卡产商Nvidia，AMD，两个显卡产商，它们之间要怎么统一标准，所以微软提出来一个叫DirectX的标准，图形的标准，里面定义很多硬件加速的接口，显卡产商就负责在显卡驱动里面来实现支持这些接口，所以当DirectX装好了以后，我们的代码使用DirectX的API，由于显卡的驱动接好了底层的API的实现，所以API就能够使用显卡加速，来获得很好的图形性能，这就是为什么显卡总要装一个驱动，因为它装好驱动以后要把图形图像的接口统一起来接好，这样通过DirectX就能访问得到显卡的硬件加速。

 

微软DirectX不开源的，而Linux的OpenGL是开源的，安卓，Linux，IOS，都支持OpenGL，而不支持DirectX，微软也支持OpenGL，大家在Windows上面习惯使用DirectX，在Linux上面习惯使用OpenGL

而Unity是跨平台的游戏引擎，所以在Windows上使用DirectX,，在Linux上使用OpenGL。

 

有了这两个标准以后，显卡产商就通过驱动的形式，把标准接入到标准库里面去，这样应用程序只要调用API就能够使用显卡加速，如果有的话。

同时DirectX和OpenGL也支持用户直接塞程序，比如塞shader给显卡执行，所以GPU显卡也有编程语言，有三种HLSL，Cg，GLSL语言

由于Unity是跨平台的游戏引擎，所以它重点支持cg语言，但是它又不直接使用cg语言，而是自己搞一个shaderLab语言来做着色程序的编写，这样我们就可以使用Unity的shaderLab的语法来生成各种各样平台不同的着色程序。

我们所说的shader就是使用Unity的shaderLab来编写的着色程序。

 

 

Shader Lab语法基础

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

 

 

SubShader

子着色器，里面包含一个一个连接通道的管道的合集

1: SubShader {[Tags], [CommonState], Pass {} }子着色器由 标签(Tags),通用状态,通道列表组成,它定义了一个渲染通道列表，并可选为所有通道初始化需要的通用状态;

pass就是通道，就是流水线上面一个一个的通道，物体模型经过一个一个通道，最终把经过通道处理的物体绘制出来。
2: SubShader渲染的时候，将优先渲染一个被每个通道所定义的对象。
3: 通道的类型: RegularPass, UsePass, GrabPass,
4: 在通道中定义状态同时对整个子着色器可见，那么所有的通道可以共享状态;

 

SubShader 语法

1: SubShader {
Tags {“Queue”, “Transparent” }
Pass {
Lighting Off // 关闭光照
....
}
}

 

Tags 

1: Tags {“标签1” = “value1” “key2” = “value2”}
2: 标签的类型:
Queue tag 队列标签;
RenderType tag 渲染类型标签;
DisableBatching tag 禁用批处理标签;
ForceNoShadowCasting Tag 强制不投阴影标签;
IgnoreProjecttor 忽略投影标签;
CanUseSpriteAtlas Tag,使用精灵图集标签;
PreviewType Tag预览类型标签;

 

Pass

通道，可以把一个一个的通道看成一个一个的盒子，物体经过盒子进行加工，然后出来，就可以了。

1: subshader 包装了一个渲染方案，这些方案由一个个通道(Pass)来执行的，SubShader可以包括很多通道块,每个Pass都能使几何体渲染一次;
2: Pass基本语法:
Pass { [Name and Tags] [RenderSetup] [Texture Setup]}
Pass块的Name引用此Pass,可以在其它着色器的Pass块中引用它，减少重复操作,Name命令必须打大写;

 