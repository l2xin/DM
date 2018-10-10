编写Unity Shader时有很多内置函数，在网上查找了一些，把他们记录了下来，以供大家参考使用




参考网址：<http://www.cppblog.com/lai3d/archive/2008/10/23/64889.html>

 

这个是MSDN上的，DirectX Documentation里也有，其实也就是HLSL的内置函数

#   

# Intrinsic Functions (DirectX HLSL)

The following table lists the intrinsic functions available in HLSL. Each function has a brief description, and a link to a reference page that has more detail about the input argument and return type.

| Name                                                         | Syntax                           | Description                                                  |
| ------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| [abs](http://msdn.microsoft.com/en-us/library/bb509562%28VS.85%29.aspx) | abs(x)                           | Absolute value (per component).                              |
| [acos](http://msdn.microsoft.com/en-us/library/bb509563%28VS.85%29.aspx) | acos(x)                          | Returns the arccosine of each component of x.                |
| [all](http://msdn.microsoft.com/en-us/library/bb509564%28VS.85%29.aspx) | all(x)                           | Test if all components of x are nonzero.                     |
| [any](http://msdn.microsoft.com/en-us/library/bb509565%28VS.85%29.aspx) | any(x)                           | Test if any component of x is nonzero.                       |
| [asfloat](http://msdn.microsoft.com/en-us/library/bb509570%28VS.85%29.aspx) | asfloat(x)                       | Convert the input type to a float.                           |
| [asin](http://msdn.microsoft.com/en-us/library/bb509571%28VS.85%29.aspx) | asin(x)                          | Returns the arcsine of each component of x.                  |
| [asint](http://msdn.microsoft.com/en-us/library/bb509572%28VS.85%29.aspx) | asint(x)                         | Convert the input type to an integer.                        |
| [asuint](http://msdn.microsoft.com/en-us/library/bb509573%28VS.85%29.aspx) | asuint(x)                        | Convert the input type to an unsigned integer.               |
| [atan](http://msdn.microsoft.com/en-us/library/bb509574%28VS.85%29.aspx) | atan(x)                          | Returns the arctangent of x.                                 |
| [atan2](http://msdn.microsoft.com/en-us/library/bb509575%28VS.85%29.aspx) | atan2(y, x)                      | Returns the arctangent of of two values (x,y).               |
| [ceil](http://msdn.microsoft.com/en-us/library/bb509577%28VS.85%29.aspx) | ceil(x)                          | Returns the smallest integer which is greater than or equal to x. |
| [clamp](http://msdn.microsoft.com/en-us/library/bb509578%28VS.85%29.aspx) | clamp(x, min, max)               | Clamps x to the range [min, max].                            |
| [clip](http://msdn.microsoft.com/en-us/library/bb509579%28VS.85%29.aspx) | clip(x)                          | Discards the current pixel, if any component of x is less than zero. |
| [cos](http://msdn.microsoft.com/en-us/library/bb509583%28VS.85%29.aspx) | cos(x)                           | Returns the cosine of x.                                     |
| [cosh](http://msdn.microsoft.com/en-us/library/bb509584%28VS.85%29.aspx) | cosh(x)                          | Returns the hyperbolic cosine of x.                          |
| [cross](http://msdn.microsoft.com/en-us/library/bb509585%28VS.85%29.aspx) | cross(x, y)                      | Returns the cross product of two 3D vectors.                 |
| [D3DCOLORtoUBYTE4](http://msdn.microsoft.com/en-us/library/bb509586%28VS.85%29.aspx) | D3DCOLORtoUBYTE4(x)              | Swizzles and scales components of the 4D vector x to compensate for the lack of UBYTE4 support in some hardware. |
| [ddx](http://msdn.microsoft.com/en-us/library/bb509588%28VS.85%29.aspx) | ddx(x)                           | Returns the partial derivative of x with respect to the screen-space x-coordinate. |
| [ddy](http://msdn.microsoft.com/en-us/library/bb509589%28VS.85%29.aspx) | ddy(x)                           | Returns the partial derivative of x with respect to the screen-space y-coordinate. |
| [degrees](http://msdn.microsoft.com/en-us/library/bb509590%28VS.85%29.aspx) | degrees(x)                       | Converts x from radians to degrees.                          |
| [determinant](http://msdn.microsoft.com/en-us/library/bb509591%28VS.85%29.aspx) | determinant(m)                   | Returns the determinant of the square matrix m.              |
| [distance](http://msdn.microsoft.com/en-us/library/bb509592%28VS.85%29.aspx) | distance(x, y)                   | Returns the distance between two points.                     |
| [dot](http://msdn.microsoft.com/en-us/library/bb509594%28VS.85%29.aspx) | dot(x, y)                        | Returns the dot product of two vectors.                      |
| [exp](http://msdn.microsoft.com/en-us/library/bb509595%28VS.85%29.aspx) | exp(x)                           | Returns the base-e exponent.                                 |
| [exp2](http://msdn.microsoft.com/en-us/library/bb509596%28VS.85%29.aspx) | exp2(x)                          | Base 2 exponent (per component).                             |
| [faceforward](http://msdn.microsoft.com/en-us/library/bb509598%28VS.85%29.aspx) | faceforward(n, i, ng)            | Returns -n * sign(•(i, ng)).                                 |
| [floor](http://msdn.microsoft.com/en-us/library/bb509599%28VS.85%29.aspx) | floor(x)                         | Returns the greatest integer which is less than or equal to x. |
| [fmod](http://msdn.microsoft.com/en-us/library/bb509601%28VS.85%29.aspx) | fmod(x, y)                       | Returns the floating point remainder of x/y.                 |
| [frac](http://msdn.microsoft.com/en-us/library/bb509603%28VS.85%29.aspx) | frac(x)                          | Returns the fractional part of x.                            |
| [frexp](http://msdn.microsoft.com/en-us/library/bb509604%28VS.85%29.aspx) | frexp(x, exp)                    | Returns the mantissa and exponent of x.                      |
| [fwidth](http://msdn.microsoft.com/en-us/library/bb509608%28VS.85%29.aspx) | fwidth(x)                        | Returns abs(ddx(x)) + abs(ddy(x))                            |
| [GetRenderTargetSampleCount](http://msdn.microsoft.com/en-us/library/bb943996%28VS.85%29.aspx) | GetRenderTargetSampleCount()     | Returns the number of render-target samples.                 |
| [GetRenderTargetSamplePosition](http://msdn.microsoft.com/en-us/library/bb943997%28VS.85%29.aspx) | GetRenderTargetSamplePosition(x) | Returns a sample position (x,y) for a given sample index.    |
| [isfinite](http://msdn.microsoft.com/en-us/library/bb509612%28VS.85%29.aspx) | isfinite(x)                      | Returns true if x is finite, false otherwise.                |
| [isinf](http://msdn.microsoft.com/en-us/library/bb509613%28VS.85%29.aspx) | isinf(x)                         | Returns true if x is +INF or -INF, false otherwise.          |
| [isnan](http://msdn.microsoft.com/en-us/library/bb509614%28VS.85%29.aspx) | isnan(x)                         | Returns true if x is NAN or QNAN, false otherwise.           |
| [ldexp](http://msdn.microsoft.com/en-us/library/bb509616%28VS.85%29.aspx) | ldexp(x, exp)                    | Returns x * 2exp                                             |
| [length](http://msdn.microsoft.com/en-us/library/bb509617%28VS.85%29.aspx) | length(v)                        | Returns the length of the vector v.                          |
| [lerp](http://msdn.microsoft.com/en-us/library/bb509618%28VS.85%29.aspx) | lerp(x, y, s)                    | Returns x + s(y - x).                                        |
| [lit](http://msdn.microsoft.com/en-us/library/bb509619%28VS.85%29.aspx) | lit(n • l, n • h, m)             | Returns a lighting vector (ambient, diffuse, specular, 1)    |
| [log](http://msdn.microsoft.com/en-us/library/bb509620%28VS.85%29.aspx) | log(x)                           | Returns the base-e logarithm of x.                           |
| [log10](http://msdn.microsoft.com/en-us/library/bb509621%28VS.85%29.aspx) | log10(x)                         | Returns the base-10 logarithm of x.                          |
| [log2](http://msdn.microsoft.com/en-us/library/bb509622%28VS.85%29.aspx) | log2(x)                          | Returns the base-2 logarithm of x.                           |
| [max](http://msdn.microsoft.com/en-us/library/bb509624%28VS.85%29.aspx) | max(x, y)                        | Selects the greater of x and y.                              |
| [min](http://msdn.microsoft.com/en-us/library/bb509625%28VS.85%29.aspx) | min(x, y)                        | Selects the lesser of x and y.                               |
| [modf](http://msdn.microsoft.com/en-us/library/bb509627%28VS.85%29.aspx) | modf(x, out ip)                  | Splits the value x into fractional and integer parts.        |
| [mul](http://msdn.microsoft.com/en-us/library/bb509628%28VS.85%29.aspx) | mul(x, y)                        | Performs matrix multiplication using x and y.                |
| [noise](http://msdn.microsoft.com/en-us/library/bb509629%28VS.85%29.aspx) | noise(x)                         | Generates a random value using the Perlin-noise algorithm.   |
| [normalize](http://msdn.microsoft.com/en-us/library/bb509630%28VS.85%29.aspx) | normalize(x)                     | Returns a normalized vector.                                 |
| [pow](http://msdn.microsoft.com/en-us/library/bb509636%28VS.85%29.aspx) | pow(x, y)                        | Returns xy.                                                  |
| [radians](http://msdn.microsoft.com/en-us/library/bb509637%28VS.85%29.aspx) | radians(x)                       | Converts x from degrees to radians.                          |
| [reflect](http://msdn.microsoft.com/en-us/library/bb509639%28VS.85%29.aspx) | reflect(i, n)                    | Returns a reflection vector.                                 |
| [refract](http://msdn.microsoft.com/en-us/library/bb509640%28VS.85%29.aspx) | refract(i, n, R)                 | Returns the refraction vector.                               |
| [round](http://msdn.microsoft.com/en-us/library/bb509642%28VS.85%29.aspx) | round(x)                         | Rounds x to the nearest integer                              |
| [rsqrt](http://msdn.microsoft.com/en-us/library/bb509643%28VS.85%29.aspx) | rsqrt(x)                         | Returns 1 / sqrt(x)                                          |
| [saturate](http://msdn.microsoft.com/en-us/library/bb509645%28VS.85%29.aspx) | saturate(x)                      | Clamps x to the range [0, 1]                                 |
| [sign](http://msdn.microsoft.com/en-us/library/bb509649%28VS.85%29.aspx) | sign(x)                          | Computes the sign of x.                                      |
| [sin](http://msdn.microsoft.com/en-us/library/bb509651%28VS.85%29.aspx) | sin(x)                           | Returns the sine of x                                        |
| [sincos](http://msdn.microsoft.com/en-us/library/bb509652%28VS.85%29.aspx) | sincos(x, out s, out c)          | Returns the sine and cosine of x.                            |
| [sinh](http://msdn.microsoft.com/en-us/library/bb509653%28VS.85%29.aspx) | sinh(x)                          | Returns the hyperbolic sine of x                             |
| [smoothstep](http://msdn.microsoft.com/en-us/library/bb509658%28VS.85%29.aspx) | smoothstep(min, max, x)          | Returns a smooth Hermite interpolation between 0 and 1.      |
| [sqrt](http://msdn.microsoft.com/en-us/library/bb509662%28VS.85%29.aspx) | sqrt(x)                          | Square root (per component)                                  |
| [step](http://msdn.microsoft.com/en-us/library/bb509665%28VS.85%29.aspx) | step(a, x)                       | Returns (x >= a) ? 1 : 0                                     |
| [tan](http://msdn.microsoft.com/en-us/library/bb509670%28VS.85%29.aspx) | tan(x)                           | Returns the tangent of x                                     |
| [tanh](http://msdn.microsoft.com/en-us/library/bb509671%28VS.85%29.aspx) | tanh(x)                          | Returns the hyperbolic tangent of x                          |
| [tex1D](http://msdn.microsoft.com/en-us/library/bb509672%28VS.85%29.aspx) | tex1D(s, t)                      | 1D texture lookup.                                           |
| [tex1Dbias](http://msdn.microsoft.com/en-us/library/bb509673%28VS.85%29.aspx) | tex1Dbias(s, t)                  | 1D texture lookup with bias.                                 |
| [tex1Dgrad](http://msdn.microsoft.com/en-us/library/bb509674%28VS.85%29.aspx) | tex1Dgrad(s, t, ddx, ddy)        | 1D texture lookup with a gradient.                           |
| [tex1Dlod](http://msdn.microsoft.com/en-us/library/bb509675%28VS.85%29.aspx) | tex1Dlod(s, t)                   | 1D texture lookup with LOD.                                  |
| [tex1Dproj](http://msdn.microsoft.com/en-us/library/bb509676%28VS.85%29.aspx) | tex1Dproj(s, t)                  | 1D texture lookup with projective divide.                    |
| [tex2D](http://msdn.microsoft.com/en-us/library/bb509677%28VS.85%29.aspx) | tex2D(s, t)                      | 2D texture lookup.                                           |
| [tex2Dbias](http://msdn.microsoft.com/en-us/library/bb509678%28VS.85%29.aspx) | tex2Dbias(s, t)                  | 2D texture lookup with bias.                                 |
| [tex2Dgrad](http://msdn.microsoft.com/en-us/library/bb509679%28VS.85%29.aspx) | tex2Dgrad(s, t, ddx, ddy)        | 2D texture lookup with a gradient.                           |
| [tex2Dlod](http://msdn.microsoft.com/en-us/library/bb509680%28VS.85%29.aspx) | tex2Dlod(s, t)                   | 2D texture lookup with LOD.                                  |
| [tex2Dproj](http://msdn.microsoft.com/en-us/library/bb509681%28VS.85%29.aspx) | tex2Dproj(s, t)                  | 2D texture lookup with projective divide.                    |
| [tex3D](http://msdn.microsoft.com/en-us/library/bb509682%28VS.85%29.aspx) | tex3D(s, t)                      | 3D texture lookup.                                           |
| [tex3Dbias](http://msdn.microsoft.com/en-us/library/bb509683%28VS.85%29.aspx) | tex3Dbias(s, t)                  | 3D texture lookup with bias.                                 |
| [tex3Dgrad](http://msdn.microsoft.com/en-us/library/bb509684%28VS.85%29.aspx) | tex3Dgrad(s, t, ddx, ddy)        | 3D texture lookup with a gradient.                           |
| [tex3Dlod](http://msdn.microsoft.com/en-us/library/bb509685%28VS.85%29.aspx) | tex3Dlod(s, t)                   | 3D texture lookup with LOD.                                  |
| [tex3Dproj](http://msdn.microsoft.com/en-us/library/bb509686%28VS.85%29.aspx) | tex3Dproj(s, t)                  | 3D texture lookup with projective divide.                    |
| [texCUBE](http://msdn.microsoft.com/en-us/library/bb509687%28VS.85%29.aspx) | texCUBE(s, t)                    | Cube texture lookup.                                         |
| [texCUBEbias](http://msdn.microsoft.com/en-us/library/bb509688%28VS.85%29.aspx) | texCUBEbias(s, t)                | Cube texture lookup with bias.                               |
| [texCUBEgrad](http://msdn.microsoft.com/en-us/library/bb509689%28VS.85%29.aspx) | texCUBEgrad(s, t, ddx, ddy)      | Cube texture lookup with a gradient.                         |
| [texCUBElod](http://msdn.microsoft.com/en-us/library/bb509690%28VS.85%29.aspx) | tex3Dlod(s, t)                   | Cube texture lookup with LOD.                                |
| [texCUBEproj](http://msdn.microsoft.com/en-us/library/bb509691%28VS.85%29.aspx) | texCUBEproj(s, t)                | Cube texture lookup with projective divide.                  |
| [transpose](http://msdn.microsoft.com/en-us/library/bb509701%28VS.85%29.aspx) | transpose(m)                     | Returns the transpose of the matrix m.                       |
| [trunc](http://msdn.microsoft.com/en-us/library/cc308065%28VS.85%29.aspx) | trunc(x)                         | Truncates floating-point value(s) to integer value(s)        |

## 

表 3-1 HLSL内置函数

 函数名            用法 

abs                         计算输入值的绝对值。

acos                        返回输入值反余弦值。

all                           [测试](http://lib.csdn.net/base/softwaretest)非0值。

any                         测试输入值中的任何非零值。

asin                         返回输入值的反正弦值。

atan                        返回输入值的反正切值。

atan2                       返回y/x的反正切值。

ceil                         返回大于或等于输入值的最小整数。

clamp                      把输入值限制在[min, max]范围内。

clip                         如果输入向量中的任何元素小于0，则丢弃当前像素。

cos                         返回输入值的余弦。

cosh                       返回输入值的双曲余弦。

cross                      返回两个3D向量的叉积。

ddx                         返回关于屏幕坐标x轴的偏导数。

ddy                         返回关于屏幕坐标y轴的偏导数。

degrees                   弧度到角度的转换

determinant              返回输入矩阵的值。

distance                   返回两个输入点间的距离。

dot                          返回两个向量的点积。

exp                         返回以e为底数，输入值为指数的指数函数值。

exp2                       返回以2为底数，输入值为指数的指数函数值。

faceforward             检测多边形是否位于正面。

floor                       返回小于等于x的最大整数。

fmod                       返回a / b的浮点余数。

frac                        返回输入值的小数部分。

frexp                       返回输入值的尾数和指数

fwidth                     返回 abs ( ddx (x) + abs ( ddy(x))。

isfinite                     如果输入值为有限值则返回true，否则返回false。

isinf                        如何输入值为无限的则返回true。

isnan                       如果输入值为NAN或QNAN则返回true。

ldexp                       frexp的逆运算，返回 x * 2 ^ exp。

len / lenth                返回输入向量的长度。

lerp                         对输入值进行插值计算。

lit                            返回光照向量（环境光，漫反射光，镜面高光，1）。

log                          返回以e为底的对数。

log10                      返回以10为底的对数。

log2                        返回以2为底的对数。

max                        返回两个输入值中较大的一个。

min                         返回两个输入值中较小的一个。

modf                       把输入值分解为整数和小数部分。

mul                         返回输入矩阵相乘的积。

normalize                 返回规范化的向量，定义为 x / length(x)。

pow                        返回输入值的指定次幂。

radians                    角度到弧度的转换。

reflect                     返回入射光线i对表面法线n的反射光线。

refract                     返回在入射光线i，表面法线n，折射率为eta下的折射光线v。

round                      返回最接近于输入值的整数。

rsqrt                       返回输入值平方根的倒数。

saturate                   把输入值限制到[0, 1]之间。

sign                        计算输入值的符号。

sin                          计算输入值的正弦值。

sincos                     返回输入值的正弦和余弦值。

sinh                        返回x的双曲正弦。

smoothstep              返回一个在输入值之间平稳变化的插值。

sqrt                         返回输入值的平方根。

step                        返回（x >= a）? 1 : 0。

tan                          返回输入值的正切值。

fanh                        返回输入值的双曲线切线。

transpose                 返回输入矩阵的转置。

tex1D*                    1D纹理查询。

tex2D*                    2D纹理查询。

tex3D*                    3D纹理查询。


texCUBE*                立方纹理查询。

# Intrinsic Functions (DirectX HLSL)



The following table lists the intrinsic functions available in HLSL. Each function has a brief description, and a link to a reference page that has more detail about the input argument and return type.

| Name                                                         | Description                                                  | Minimum shader model |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- |
| [**abs**](http://preview.library.microsoft.com/en-us/library/bb509562(v=vs.85).aspx) | Absolute value (per component).                              | 11                   |
| [**acos**](http://preview.library.microsoft.com/en-us/library/bb509563(v=vs.85).aspx) | Returns the arccosine of each component of x.                | 11                   |
| [**all**](http://preview.library.microsoft.com/en-us/library/bb509564(v=vs.85).aspx) | Test if all components of x are nonzero.                     | 11                   |
| [**AllMemoryBarrier**](http://preview.library.microsoft.com/en-us/library/ff471350(v=vs.85).aspx) | Blocks execution of all threads in a group until all memory accesses have been completed. | 5                    |
| [**AllMemoryBarrierWithGroupSync**](http://preview.library.microsoft.com/en-us/library/ff471351(v=vs.85).aspx) | Blocks execution of all threads in a group until all memory accesses have been completed and all threads in the group have reached this call. | 5                    |
| [**any**](http://preview.library.microsoft.com/en-us/library/bb509565(v=vs.85).aspx) | Test if any component of x is nonzero.                       | 11                   |
| [**asdouble**](http://preview.library.microsoft.com/en-us/library/dd607357(v=vs.85).aspx) | Reinterprets a cast value into a double.                     | 5                    |
| [**asfloat**](http://preview.library.microsoft.com/en-us/library/bb509570(v=vs.85).aspx) | Convert the input type to a float.                           | 4                    |
| [**asin**](http://preview.library.microsoft.com/en-us/library/bb509571(v=vs.85).aspx) | Returns the arcsine of each component of x.                  | 11                   |
| [**asint**](http://preview.library.microsoft.com/en-us/library/bb509572(v=vs.85).aspx) | Convert the input type to an integer.                        | 4                    |
| [**asuint**](http://preview.library.microsoft.com/en-us/library/ff471354(v=vs.85).aspx) | Reinterprets the bit pattern of a 64-bit type to a uint.     | 5                    |
| [**asuint**](http://preview.library.microsoft.com/en-us/library/bb509573(v=vs.85).aspx) | Convert the input type to an unsigned integer.               | 4                    |
| [**atan**](http://preview.library.microsoft.com/en-us/library/bb509574(v=vs.85).aspx) | Returns the arctangent of x.                                 | 11                   |
| [**atan2**](http://preview.library.microsoft.com/en-us/library/bb509575(v=vs.85).aspx) | Returns the arctangent of of two values (x,y).               | 11                   |
| [**ceil**](http://preview.library.microsoft.com/en-us/library/bb509577(v=vs.85).aspx) | Returns the smallest integer which is greater than or equal to x. | 11                   |
| [**clamp**](http://preview.library.microsoft.com/en-us/library/bb204824(v=vs.85).aspx) | Clamps x to the range [min, max].                            | 11                   |
| [**clip**](http://preview.library.microsoft.com/en-us/library/bb204826(v=vs.85).aspx) | Discards the current pixel, if any component of x is less than zero. | 11                   |
| [**cos**](http://preview.library.microsoft.com/en-us/library/bb509583(v=vs.85).aspx) | Returns the cosine of x.                                     | 11                   |
| [**cosh**](http://preview.library.microsoft.com/en-us/library/bb509584(v=vs.85).aspx) | Returns the hyperbolic cosine of x.                          | 11                   |
| [**countbits**](http://preview.library.microsoft.com/en-us/library/ff471355(v=vs.85).aspx) | Counts the number of bits (per component) in the input integer. | 5                    |
| [**cross**](http://preview.library.microsoft.com/en-us/library/bb509585(v=vs.85).aspx) | Returns the cross product of two 3D vectors.                 | 11                   |
| [**D3DCOLORtoUBYTE4**](http://preview.library.microsoft.com/en-us/library/bb509586(v=vs.85).aspx) | Swizzles and scales components of the 4D vector xto compensate for the lack of UBYTE4 support in some hardware. | 11                   |
| [**ddx**](http://preview.library.microsoft.com/en-us/library/bb509588(v=vs.85).aspx) | Returns the partial derivative of x with respect to the screen-space x-coordinate. | 21                   |
| [**ddx_coarse**](http://preview.library.microsoft.com/en-us/library/ff471361(v=vs.85).aspx) | Computes a low precision partial derivative with respect to the screen-space x-coordinate. | 5                    |
| [**ddx_fine**](http://preview.library.microsoft.com/en-us/library/ff471362(v=vs.85).aspx) | Computes a high precision partial derivative with respect to the screen-space x-coordinate. | 5                    |
| [**ddy**](http://preview.library.microsoft.com/en-us/library/bb509589(v=vs.85).aspx) | Returns the partial derivative of x with respect to the screen-space y-coordinate. | 21                   |
| [**ddy_coarse**](http://preview.library.microsoft.com/en-us/library/ff471364(v=vs.85).aspx) | Computes a low precision partial derivative with respect to the screen-space y-coordinate. | 5                    |
| [**ddy_fine**](http://preview.library.microsoft.com/en-us/library/ff471365(v=vs.85).aspx) | Computes a high precision partial derivative with respect to the screen-space y-coordinate. | 5                    |
| [**degrees**](http://preview.library.microsoft.com/en-us/library/bb509590(v=vs.85).aspx) | Converts x from radians to degrees.                          | 11                   |
| [**determinant**](http://preview.library.microsoft.com/en-us/library/bb509591(v=vs.85).aspx) | Returns the determinant of the square matrix m.              | 11                   |
| [**DeviceMemoryBarrier**](http://preview.library.microsoft.com/en-us/library/ff471366(v=vs.85).aspx) | Blocks execution of all threads in a group until all device memory accesses have been completed. | 5                    |
| [**DeviceMemoryBarrierWithGroupSync**](http://preview.library.microsoft.com/en-us/library/ff471367(v=vs.85).aspx) | Blocks execution of all threads in a group until all device memory accesses have been completed and all threads in the group have reached this call. | 5                    |
| [**distance**](http://preview.library.microsoft.com/en-us/library/bb509592(v=vs.85).aspx) | Returns the distance between two points.                     | 11                   |
| [**dot**](http://preview.library.microsoft.com/en-us/library/bb509594(v=vs.85).aspx) | Returns the dot product of two vectors.                      | 1                    |
| [**dst**](http://preview.library.microsoft.com/en-us/library/ff471368(v=vs.85).aspx) | Calculates a distance vector.                                | 5                    |
| [**EvaluateAttributeAtCentroid**](http://preview.library.microsoft.com/en-us/library/ff471394(v=vs.85).aspx) | Evaluates at the pixel centroid.                             | 5                    |
| [**EvaluateAttributeAtSample**](http://preview.library.microsoft.com/en-us/library/ff471395(v=vs.85).aspx) | Evaluates at the indexed sample location.                    | 5                    |
| [**EvaluateAttributeSnapped**](http://preview.library.microsoft.com/en-us/library/ff471396(v=vs.85).aspx) | Evaluates at the pixel centroid with an offset.              | 5                    |
| [**exp**](http://preview.library.microsoft.com/en-us/library/bb509595(v=vs.85).aspx) | Returns the base-e exponent.                                 | 11                   |
| [**exp2**](http://preview.library.microsoft.com/en-us/library/bb509596(v=vs.85).aspx) | Base 2 exponent (per component).                             | 11                   |
| [**f16tof32**](http://preview.library.microsoft.com/en-us/library/ff471397(v=vs.85).aspx) | Converts the float16 stored in the low-half of the uint to a float. | 5                    |
| [**f32tof16**](http://preview.library.microsoft.com/en-us/library/ff471399(v=vs.85).aspx) | Converts an input into a float16 type.                       | 5                    |
| [**faceforward**](http://preview.library.microsoft.com/en-us/library/bb509598(v=vs.85).aspx) | Returns -n * sign(dot(i, ng)).                               | 11                   |
| [**firstbithigh**](http://preview.library.microsoft.com/en-us/library/ff471400(v=vs.85).aspx) | Gets the location of the first set bit starting from the highest order bit and working downward, per component. | 5                    |
| [**firstbitlow**](http://preview.library.microsoft.com/en-us/library/ff471401(v=vs.85).aspx) | Returns the location of the first set bit starting from the lowest order bit and working upward, per component. | 5                    |
| [**floor**](http://preview.library.microsoft.com/en-us/library/bb509599(v=vs.85).aspx) | Returns the greatest integer which is less than or equal to x. | 11                   |
| [**fmod**](http://preview.library.microsoft.com/en-us/library/bb509601(v=vs.85).aspx) | Returns the floating point remainder of x/y.                 | 11                   |
| [**frac**](http://preview.library.microsoft.com/en-us/library/bb509603(v=vs.85).aspx) | Returns the fractional part of x.                            | 11                   |
| [**frexp**](http://preview.library.microsoft.com/en-us/library/bb509604(v=vs.85).aspx) | Returns the mantissa and exponent of x.                      | 21                   |
| [**fwidth**](http://preview.library.microsoft.com/en-us/library/bb509608(v=vs.85).aspx) | Returns abs(ddx(x)) + abs(ddy(x))                            | 21                   |
| [**GetRenderTargetSampleCount**](http://preview.library.microsoft.com/en-us/library/bb943996(v=vs.85).aspx) | Returns the number of render-target samples.                 | 4                    |
| [**GetRenderTargetSamplePosition**](http://preview.library.microsoft.com/en-us/library/bb943997(v=vs.85).aspx) | Returns a sample position (x,y) for a given sample index.    | 4                    |
| [**GroupMemoryBarrier**](http://preview.library.microsoft.com/en-us/library/ff471403(v=vs.85).aspx) | Blocks execution of all threads in a group until all group shared accesses have been completed. | 5                    |
| [**GroupMemoryBarrierWithGroupSync**](http://preview.library.microsoft.com/en-us/library/ff471404(v=vs.85).aspx) | Blocks execution of all threads in a group until all group shared accesses have been completed and all threads in the group have reached this call. | 5                    |
| [**InterlockedAdd**](http://preview.library.microsoft.com/en-us/library/ff471406(v=vs.85).aspx) | Performs a guaranteed atomic add of value to the dest resource variable. | 5                    |
| [**InterlockedAnd**](http://preview.library.microsoft.com/en-us/library/ff471407(v=vs.85).aspx) | Performs a guaranteed atomic and.                            | 5                    |
| [**InterlockedCompareExchange**](http://preview.library.microsoft.com/en-us/library/ff471409(v=vs.85).aspx) | Atomically compares the input to the comparison value and exchanges the result. | 5                    |
| [**InterlockedCompareStore**](http://preview.library.microsoft.com/en-us/library/ff471410(v=vs.85).aspx) | Atomically compares the input to the comparison value.       | 5                    |
| [**InterlockedExchange**](http://preview.library.microsoft.com/en-us/library/ff471411(v=vs.85).aspx) | Assigns value to dest and returns the original value.        | 5                    |
| [**InterlockedMax**](http://preview.library.microsoft.com/en-us/library/ff471412(v=vs.85).aspx) | Performs a guaranteed atomic max.                            | 5                    |
| [**InterlockedMin**](http://preview.library.microsoft.com/en-us/library/ff471413(v=vs.85).aspx) | Performs a guaranteed atomic min.                            | 5                    |
| [**InterlockedOr**](http://preview.library.microsoft.com/en-us/library/ff471414(v=vs.85).aspx) | Performs a guaranteed atomic or.                             | 5                    |
| [**InterlockedXor**](http://preview.library.microsoft.com/en-us/library/ff471415(v=vs.85).aspx) | Performs a guaranteed atomic xor.                            | 5                    |
| [**isfinite**](http://preview.library.microsoft.com/en-us/library/bb509612(v=vs.85).aspx) | Returns true if x is finite, false otherwise.                | 11                   |
| [**isinf**](http://preview.library.microsoft.com/en-us/library/bb509613(v=vs.85).aspx) | Returns true if x is +INF or -INF, false otherwise.          | 11                   |
| [**isnan**](http://preview.library.microsoft.com/en-us/library/bb509614(v=vs.85).aspx) | Returns true if x is NAN or QNAN, false otherwise.           | 11                   |
| [**ldexp**](http://preview.library.microsoft.com/en-us/library/bb509616(v=vs.85).aspx) | Returns x * 2exp                                             | 11                   |
| [**length**](http://preview.library.microsoft.com/en-us/library/bb509617(v=vs.85).aspx) | Returns the length of the vector v.                          | 11                   |
| [**lerp**](http://preview.library.microsoft.com/en-us/library/bb509618(v=vs.85).aspx) | Returns x + s(y - x).                                        | 11                   |
| [**lit**](http://preview.library.microsoft.com/en-us/library/bb509619(v=vs.85).aspx) | Returns a lighting vector (ambient, diffuse, specular, 1)    | 11                   |
| [**log**](http://preview.library.microsoft.com/en-us/library/bb509620(v=vs.85).aspx) | Returns the base-e logarithm of x.                           | 11                   |
| [**log10**](http://preview.library.microsoft.com/en-us/library/bb509621(v=vs.85).aspx) | Returns the base-10 logarithm of x.                          | 11                   |
| [**log2**](http://preview.library.microsoft.com/en-us/library/bb509622(v=vs.85).aspx) | Returns the base-2 logarithm of x.                           | 11                   |
| [**mad**](http://preview.library.microsoft.com/en-us/library/ff471418(v=vs.85).aspx) | Performs an arithmetic multiply/add operation on three values. | 5                    |
| [**max**](http://preview.library.microsoft.com/en-us/library/bb509624(v=vs.85).aspx) | Selects the greater of x and y.                              | 11                   |
| [**min**](http://preview.library.microsoft.com/en-us/library/bb509625(v=vs.85).aspx) | Selects the lesser of x and y.                               | 11                   |
| [**modf**](http://preview.library.microsoft.com/en-us/library/bb509627(v=vs.85).aspx) | Splits the value x into fractional and integer parts.        | 11                   |
| [**mul**](http://preview.library.microsoft.com/en-us/library/bb509628(v=vs.85).aspx) | Performs matrix multiplication using x and y.                | 1                    |
| [**noise**](http://preview.library.microsoft.com/en-us/library/bb509629(v=vs.85).aspx) | Generates a random value using the Perlin-noise algorithm.   | 11                   |
| [**normalize**](http://preview.library.microsoft.com/en-us/library/bb509630(v=vs.85).aspx) | Returns a normalized vector.                                 | 11                   |
| [**pow**](http://preview.library.microsoft.com/en-us/library/bb509636(v=vs.85).aspx) | Returns xy.                                                  | 11                   |
| [**Process2DQuadTessFactorsAvg**](http://preview.library.microsoft.com/en-us/library/ff471426(v=vs.85).aspx) | Generates the corrected tessellation factors for a quad patch. | 5                    |
| [**Process2DQuadTessFactorsMax**](http://preview.library.microsoft.com/en-us/library/ff471427(v=vs.85).aspx) | Generates the corrected tessellation factors for a quad patch. | 5                    |
| [**Process2DQuadTessFactorsMin**](http://preview.library.microsoft.com/en-us/library/ff471428(v=vs.85).aspx) | Generates the corrected tessellation factors for a quad patch. | 5                    |
| [**ProcessIsolineTessFactors**](http://preview.library.microsoft.com/en-us/library/ff471429(v=vs.85).aspx) | Generates the rounded tessellation factors for an isoline.   | 5                    |
| [**ProcessQuadTessFactorsAvg**](http://preview.library.microsoft.com/en-us/library/ff471430(v=vs.85).aspx) | Generates the corrected tessellation factors for a quad patch. | 5                    |
| [**ProcessQuadTessFactorsMax**](http://preview.library.microsoft.com/en-us/library/ff471431(v=vs.85).aspx) | Generates the corrected tessellation factors for a quad patch. | 5                    |
| [**ProcessQuadTessFactorsMin**](http://preview.library.microsoft.com/en-us/library/ff471432(v=vs.85).aspx) | Generates the corrected tessellation factors for a quad patch. | 5                    |
| [**ProcessTriTessFactorsAvg**](http://preview.library.microsoft.com/en-us/library/ff471433(v=vs.85).aspx) | Generates the corrected tessellation factors for a tri patch. | 5                    |
| [**ProcessTriTessFactorsMax**](http://preview.library.microsoft.com/en-us/library/ff471434(v=vs.85).aspx) | Generates the corrected tessellation factors for a tri patch. | 5                    |
| [**ProcessTriTessFactorsMin**](http://preview.library.microsoft.com/en-us/library/ff471435(v=vs.85).aspx) | Generates the corrected tessellation factors for a tri patch. | 5                    |
| [**radians**](http://preview.library.microsoft.com/en-us/library/bb509637(v=vs.85).aspx) | Converts x from degrees to radians.                          | 1                    |
| [**rcp**](http://preview.library.microsoft.com/en-us/library/ff471436(v=vs.85).aspx) | Calculates a fast, approximate, per-component reciprocal.    | 5                    |
| [**reflect**](http://preview.library.microsoft.com/en-us/library/bb509639(v=vs.85).aspx) | Returns a reflection vector.                                 | 1                    |
| [**refract**](http://preview.library.microsoft.com/en-us/library/bb509640(v=vs.85).aspx) | Returns the refraction vector.                               | 11                   |
| [**reversebits**](http://preview.library.microsoft.com/en-us/library/ff471437(v=vs.85).aspx) | Reverses the order of the bits, per component.               | 5                    |
| [**round**](http://preview.library.microsoft.com/en-us/library/bb509642(v=vs.85).aspx) | Rounds x to the nearest integer                              | 11                   |
| [**rsqrt**](http://preview.library.microsoft.com/en-us/library/bb509643(v=vs.85).aspx) | Returns 1 / sqrt(x)                                          | 11                   |
| [**saturate**](http://preview.library.microsoft.com/en-us/library/bb509645(v=vs.85).aspx) | Clamps x to the range [0, 1]                                 | 1                    |
| [**sign**](http://preview.library.microsoft.com/en-us/library/bb509649(v=vs.85).aspx) | Computes the sign of x.                                      | 11                   |
| [**sin**](http://preview.library.microsoft.com/en-us/library/bb509651(v=vs.85).aspx) | Returns the sine of x                                        | 11                   |
| [**sincos**](http://preview.library.microsoft.com/en-us/library/bb509652(v=vs.85).aspx) | Returns the sine and cosine of x.                            | 11                   |
| [**sinh**](http://preview.library.microsoft.com/en-us/library/bb509653(v=vs.85).aspx) | Returns the hyperbolic sine of x                             | 11                   |
| [**smoothstep**](http://preview.library.microsoft.com/en-us/library/bb509658(v=vs.85).aspx) | Returns a smooth Hermite interpolation between 0 and 1.      | 11                   |
| [**sqrt**](http://preview.library.microsoft.com/en-us/library/bb509662(v=vs.85).aspx) | Square root (per component)                                  | 11                   |
| [**step**](http://preview.library.microsoft.com/en-us/library/bb509665(v=vs.85).aspx) | Returns (x >= a) ? 1 : 0                                     | 11                   |
| [**tan**](http://preview.library.microsoft.com/en-us/library/bb509670(v=vs.85).aspx) | Returns the tangent of x                                     | 11                   |
| [**tanh**](http://preview.library.microsoft.com/en-us/library/bb509671(v=vs.85).aspx) | Returns the hyperbolic tangent of x                          | 11                   |
| [**tex1D(s, t)**](http://preview.library.microsoft.com/en-us/library/bb509672(v=vs.85).aspx) | 1D texture lookup.                                           | 1                    |
| [**tex1D(s, t, ddx, ddy)**](http://preview.library.microsoft.com/en-us/library/ff471388(v=vs.85).aspx) | 1D texture lookup.                                           | 21                   |
| [**tex1Dbias**](http://preview.library.microsoft.com/en-us/library/bb509673(v=vs.85).aspx) | 1D texture lookup with bias.                                 | 21                   |
| [**tex1Dgrad**](http://preview.library.microsoft.com/en-us/library/bb509674(v=vs.85).aspx) | 1D texture lookup with a gradient.                           | 21                   |
| [**tex1Dlod**](http://preview.library.microsoft.com/en-us/library/bb509675(v=vs.85).aspx) | 1D texture lookup with LOD.                                  | 31                   |
| [**tex1Dproj**](http://preview.library.microsoft.com/en-us/library/bb509676(v=vs.85).aspx) | 1D texture lookup with projective divide.                    | 21                   |
| [**tex2D(s, t)**](http://preview.library.microsoft.com/en-us/library/bb509677(v=vs.85).aspx) | 2D texture lookup.                                           | 11                   |
| [**tex2D(s, t, ddx, ddy)**](http://preview.library.microsoft.com/en-us/library/ff471389(v=vs.85).aspx) | 2D texture lookup.                                           | 21                   |
| [**tex2Dbias**](http://preview.library.microsoft.com/en-us/library/bb509678(v=vs.85).aspx) | 2D texture lookup with bias.                                 | 21                   |
| [**tex2Dgrad**](http://preview.library.microsoft.com/en-us/library/bb509679(v=vs.85).aspx) | 2D texture lookup with a gradient.                           | 21                   |
| [**tex2Dlod**](http://preview.library.microsoft.com/en-us/library/bb509680(v=vs.85).aspx) | 2D texture lookup with LOD.                                  | 3                    |
| [**tex2Dproj**](http://preview.library.microsoft.com/en-us/library/bb509681(v=vs.85).aspx) | 2D texture lookup with projective divide.                    | 21                   |
| [**tex3D(s, t)**](http://preview.library.microsoft.com/en-us/library/bb509682(v=vs.85).aspx) | 3D texture lookup.                                           | 11                   |
| [**tex3D(s, t, ddx, ddy)**](http://preview.library.microsoft.com/en-us/library/ff471391(v=vs.85).aspx) | 3D texture lookup.                                           | 21                   |
| [**tex3Dbias**](http://preview.library.microsoft.com/en-us/library/bb509683(v=vs.85).aspx) | 3D texture lookup with bias.                                 | 21                   |
| [**tex3Dgrad**](http://preview.library.microsoft.com/en-us/library/bb509684(v=vs.85).aspx) | 3D texture lookup with a gradient.                           | 21                   |
| [**tex3Dlod**](http://preview.library.microsoft.com/en-us/library/bb509685(v=vs.85).aspx) | 3D texture lookup with LOD.                                  | 31                   |
| [**tex3Dproj**](http://preview.library.microsoft.com/en-us/library/bb509686(v=vs.85).aspx) | 3D texture lookup with projective divide.                    | 21                   |
| [**texCUBE(s, t)**](http://preview.library.microsoft.com/en-us/library/bb509687(v=vs.85).aspx) | Cube texture lookup.                                         | 11                   |
| [**texCUBE(s, t, ddx, ddy)**](http://preview.library.microsoft.com/en-us/library/ff471392(v=vs.85).aspx) | Cube texture lookup.                                         | 21                   |
| [**texCUBEbias**](http://preview.library.microsoft.com/en-us/library/bb509688(v=vs.85).aspx) | Cube texture lookup with bias.                               | 21                   |
| [**texCUBEgrad**](http://preview.library.microsoft.com/en-us/library/bb509689(v=vs.85).aspx) | Cube texture lookup with a gradient.                         | 21                   |
| [**texCUBElod**](http://preview.library.microsoft.com/en-us/library/bb509690(v=vs.85).aspx) | Cube texture lookup with LOD.                                | 31                   |
| [**texCUBEproj**](http://preview.library.microsoft.com/en-us/library/bb509691(v=vs.85).aspx) | Cube texture lookup with projective divide.                  | 21                   |
| [**transpose**](http://preview.library.microsoft.com/en-us/library/bb509701(v=vs.85).aspx) | Returns the transpose of the matrix m.                       | 1                    |
| [**trunc**](http://preview.library.microsoft.com/en-us/library/cc308065(v=vs.85).aspx) | Truncates floating-point value(s) to integer value(s)        | 1                    |

