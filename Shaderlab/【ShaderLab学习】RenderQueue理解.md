
#### RenderQueue

默认情况下，Unity会基于对象距离摄像机的远近来排序你的对象。因此，当一个对象离摄像机越近，它就会优先绘制在其他更远的对象上面。对于大多数情况这是有效并合适的，但是在一些特殊情况下，你可能想要自己控制对象的绘制顺序。而使用Tags{}块我们就可以得到这样的控制。



Unity提供给我们一些默认的渲染队列，每一个对应一个唯一的值，来指导Unity绘制对象到屏幕上。这些内置的渲染队列被称为Background, Geometry, AlphaTest, Transparent, Overlay。这些队列不是随便创建的，它们是为了让我们更容易地编写Shader并处理实时渲染的。下面的表格描述了这些渲染队列的用法：

示例：
```GLSL
    Tags { "Queue"="Geometry" }
    Tags { "Queue"="Geometry-20" }
```

*同时需在SubShader中显示声明`ZWrite Off`，通知Unity我们会重写物体的渲染深度排序。*



|Properties | Value | 渲染队列描述 | |
|-|-|-|-|
|Background | 1000 |	This render queue is rendered before any others.|这个队列通常被最先渲染（比如 天空盒）。
|Geometry | 2000 |		Opaque geometry uses this queue.| 这是默认的渲染队列。它被用于绝大多数对象。不透明几何体使用该队列。 |
|AlphaTest | 2450 |		Alpha tested geometry uses this queue. | 需要开启透明度测试的物体。Unity5以后从Geometry队列中拆出来,因为在所有不透明物体渲染完之后再渲染会比较高效。 |
|GeometryLast | -- |		Last render queue that is considered "opaque". | 所有Geometry和AlphaTest队列的物体渲染完后 |
|Transparent | 3000 |		This render queue is rendered after Geometry and AlphaTest, in back-to-front order. | 所有Geometry和AlphaTest队列的物体渲染完后，再按照从后往前的顺序进行渲染,任何使用了透明度混合的物体都应该使用该队列（例如玻璃和粒子效果）
|Overlay | 4000 |		This render queue is meant for overlay effects. | 该队列用于实现一些叠加效果，适合最后渲染的物体（如镜头光晕）。


#### 深度缓冲


深度缓冲（depth buffer | z-buffer）决定哪些物体渲染在前面，哪些物体渲染在后面。基本思想：根据深度缓冲中的值来决定该片元距离摄像机的距离（开启**深度测试**的前提下），当渲染这个片元时，把它的深度值和已经存在在深度缓冲中的值进行比较（开启**深度写入**的前提下），如果它的值距离摄像机更远，说明这个片元不用渲染（有物体挡住了它），否则，这个片元应该覆盖掉颜色缓冲中的像素值，并把它的深度值写入深度缓冲中（开启**深度写入**的前提下）。

关于深度缓冲更详细的分析：https://blog.csdn.net/liyaxin2010/article/details/83382940

而要实现透明效果，第一种办法开启**透明度测试（Alpha Test）**，但是这种无法得到真正的半透明混合；另一种是**透明度混合（Alpha Blend）**。

后面再来详解这两种实现的区别。

---

##### 参考：
* https://docs.unity3d.com/ScriptReference/Rendering.RenderQueue.html
