
# ShaderLab: SubShader Tags


# 作用
SubShaders使用标签来告诉引擎*如何以及何时将其渲染*。

# 语义：
``` GLSL
Tags { "TagName1" = "Value1" "TagName2" = "Value2" } 
```
指定TagName1具有Value1的值，TagName2具有Value2的值。标签的数目不受限制。

标签基本上是键值对（key-value pairs）。 在SubShader内部，标签用于确定渲染顺序和子渲染器的其他参数。 请注意，**Unity识别的以下标记必须在SubShader节内，而不是Pass**！

除了由Unity识别的内置标签，你也可以使用自己的标签，并使用`Material.GetTag`函数查询它们。

# 常见Tag

##  Rendering Order - Queue tag 控制渲染顺序-队列标签
你可以使用队列标签来确定对象的绘制顺序。 Shader决定其对象属于哪个渲染队列，这样所有透明的Shader能保证在所有不透明的Shader后渲染。

有四个预定义的渲染队列，但是在预定义的渲染队列之间可以有更多的队列。 
预定义队列为：

* Background：最先被调用的渲染，通常用来渲染背景。

* Geometry (default)：默认值，大多数物体使用此队列，用来渲染不透明的物体。

* AlphaTest :alpha tested的物体使用此队列。 它是与几何一个独立的队列，因为在绘制所有实体后，渲染经过alpha-tested的对象更有效率。

* Transparent：这个渲染队列是在Geometry和AlphaTest之后，以从后往前的顺序渲染的。 任何alpha混合（alpha-blended ）（即不写入深度缓冲区depth buffer的Shader）应该在这里。例如玻璃，粒子效果。

* Overlay： 此渲染队列用于叠加效果。 任何最后呈现的东西都应该在这里（例如镜头光晕）。

对于特殊用途，可以使用中间队列。在内部，每个队列由整数索引表示： 
Background=1000，Geometry=2000，AlphaTest=2450，Transparent=3000和Overlay=4000.

如果着色器使用这样的队列： 
Tags { "Queue" = "Geometry+1" } 
这将使对象在所有不透明物体后，但在透明物体之前渲染，作为渲染队列索引将是2001（Geometry+1）。 这在你希望某些物体始终在其他物体集之间绘制的情况下非常有用。 例如，在大多数情况下，透明的水应该在不透明物体之后但在透明物体之前绘制。

队列的值到2500（“Geometry+ 500”）被认为“不透明”，优化对象的绘制顺序以获得最佳性能。 对于“透明物体”考虑较高的渲染队列，并按距离对物体进行排序，从最远的一个开始渲染，并以最近的一个结束。 天空盒在所有不透明和所有透明对象之间绘制。

更多细节可见我的另一篇文章：[【ShaderLab学习】RenderQueue理解](https://blog.csdn.net/liyaxin2010/article/details/83384117)

## RenderType tag 渲染类型
`RenderType`标签将shaders分类为多个预定义组，Unity可以运行时替换符合特定RenderType的所有Shader。

Shader更换是从脚本使用 Camera.RenderWithShader 或 Camera.SetReplacementShader 函数来完成的。这两个函数均以一个shader和一个replacementTag作为参数。

工作流程：相机还是像平常一样来渲染场景，物体也还是使用原有的材质，但是真实的shader是在使用时来进行改变：

如果replacementTag是空的，那么场景中的所有物体都用给定的替换着色器来进行渲染。
如果replacementTag不是空的，那么对于每个物体，将进行以下方式进行渲染： 
  * 按照标签值来查找物体真正的着色器。 
  * 如果没有标签，物体将不进行渲染。 
  * 在替换Shader中，我们可以根据一个具有查找值的标签来找到一个子着色器subshader，如果没有这样的子着色器，   该物体将不进行渲染。现在可以应用子着色器来渲染物体。 

详细使用可见我的另一篇文章：[【ShaderLab学习】RenderType理解](https://blog.csdn.net/liyaxin2010/article/details/83449410)


## DisableBatching 禁用批处理 有三个可能的值： 
* “True”（始终禁用此着色器的批处理） 
* “False”（不禁用批处理;**这是默认值**） 
* “LODFading”（当LOD衰减活动时禁用批处理;主要用于树）

> 当shader中需要对顶点进行偏移的时候，一般把该项设置为“True”

## ForceNoShadowCasting 强制无阴影投射
* “True” (不会投射阴影)
* “False” (默认值)

>如果给出了ForceNoShadowCasting标签并且值为“True”，则使用此subshader渲染的对象将永远不会投射阴影。

## IgnoreProjector 忽略投影机
* “True” (表示不接受Projector组件的投影，常用于半透明物体)
* “False”

## CanUseSpriteAtlas 是否可以使用Sprite图集
* “True”
* “False”

## PreviewType 材质在inspector视图下的预览样式
* “Spheres?”（材质显示为球体,默认值）
* “Plane”（显示为2D）
* “Skybox”（将显示为天空盒）




# 参考：

* [https://docs.unity3d.com/Manual/SL-SubShaderTags.html](https://docs.unity3d.com/Manual/SL-SubShaderTags.html)
* [https://blog.csdn.net/treepulse/article/details/53484505 比较坑,像是机器翻译的](https://blog.csdn.net/treepulse/article/details/53484505) 
* [【ShaderLab学习】RenderType理解](https://blog.csdn.net/liyaxin2010/article/details/83449410)
* [【ShaderLab学习】RenderQueue理解](https://blog.csdn.net/liyaxin2010/article/details/83384117)





