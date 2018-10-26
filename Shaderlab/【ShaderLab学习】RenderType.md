
#### RenderType

在Unity Shader中会经常在SubShader中使用Tags，其中就会涉及RenderType。

``` GLSL {class=line-numbers}
SubShader{
    Tags{ "RenderType" = "Opaque" }
    ...
}
```

内置的RenderType标签包括以下：
> http://www.ceeger.com/Components/SL-ShaderReplacement.html
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

Unity可以运行时替换符合特定RenderType的Shader。主要通过Camera.RenderWithShader或者Camera.SetReplacementShader配合使用。


https://docs.unity3d.com/ScriptReference/Camera.RenderWithShader.html
##### Camera.RenderWithShader

``` GLSL
public void RenderWithShader(Shader shader, string replacementTag);
```

##### Camera.SetReplacementShader

``` GLSL
public void SetReplacementShader(Shader shader, string replacementTag);
```



https://docs.unity3d.com/Manual/SL-ShaderReplacement.html
http://www.ceeger.com/Components/SL-ShaderReplacement.html

https://blog.csdn.net/u013477973/article/details/80607989?utm_source=blogxgwz0




---

##### 参考：
* http://www.ceeger.com/Components/SL-ShaderReplacement.html
