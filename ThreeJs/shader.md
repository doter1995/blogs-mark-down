首先threejs基于webgl的，而webgl又是OpenGL ES 的。
 Shader其实就是一段执行在GPU上的程序，此程序使用OpenGL ES SL语言来编写。
常用的shader有两种：vertex shader和fragment shader（pian）
[opengles_shading_language文档](https://www.khronos.org/files/opengles_shading_language.pdf)
-- 起始文档写的很清楚,所以简单整理下。建议查看原文档
## 前置

1. shader会先运行vertex shader,然后运行fragment shader。
2. 数据类型

|类型|含义|
|--------|-------------|
|float&bool&int |   基本数据类型|
|vec2|包含了2个浮点数的向量|
|vec3     |           包含了3个浮点数的向量|
|vec4      |          包含了4个浮点数的向量|
|ivec2     |          包含了2个整数的向量|
|ivec3      |         包含了3个整数的向量|
|ivec4      |         包含了4个整数的向量|
|bvec2    |           包含了2个布尔数的向量|
|bvec3     |          包含了3个布尔数的向量|
|bvec4     |          包含了4个布尔数的向量|
|mat2      |          2*2维矩阵|
|mat3       |         3*3维矩阵|
|mat4|          4*4维矩阵|
|sampler1(2,3)D | 1(2,3)D纹理采样器|
|sampler1(2,3)DShadow |
|sampler1(2,3)DRect|
|sampler1(2,3)DRectShadow|
|samplerCube |

3. 数据声明
 * VS FS代表是否在vertex shader和fragment shader可用 

|名称|含义|VS|FS|
|--|--|--|--|
|const |只读常量|Ture|Ture|
|attribute |每个顶点数据的链接的数据|Ture|False|
|varying |顶点着色器和片段着色器之间的链接数据|Ture|Ture|
|uniform |着色器统一值|Ture|Ture|

## vertex shader（顶点着色器）
gl_Position
gl_PointSize
## fragment shader(片元着色器)
gl_FragColor
gl_FragData
gl_PointCoord
gl_FragCoord
gl_FrontFacing
## 通用内置方法

|Syntax|Description|
|--|--|
|radians(degree) | 角度变弧度|
|degrees(radian) | 弧度变角度|
| sin(angle), cos(angle), tan(angle)|
|asin(x)| arc sine, 返回弧度 [-PI/2, PI/2]|
|acos(x)| arc cosine,返回弧度 [0, PI]|
|atan(y, x)| arc tangent, 返回弧度 [-PI, PI]|
|atan(y/x)| arc tangent, 返回弧度 [-PI/2, PI/2]|
|||
|pow(x, y)| x的y次方；
|exp(x)| 指数, log(x)：
|exp2(x)| 2的x次方，
| log2(x)|
|sqrt(x)| x的根号|
| inversesqrt(x)| x根号的倒数
|||
|abs(x)| 绝对值|
|sign(x)| 符号, 1, 0 或 -1|
|floor(x)| 底部取整
|ceil(x)| 顶部取整
|fract(x)| 取小数部分
 |||
| mod(x, y)| 取模， x - y*floor(x/y)
|min(x, y)| 取最小值
|max(x, y)| 取最大值
|clamp(x, min, max)|  min(max(x, min), max);
|mix(x, y, a)| x, y的线性混叠， x(1-a) + y*a;
|step(edge, x)| 如 x
|smoothstep(edge0, edge1, x)| threshod  smooth transition时使用。 edge0<=edge0时为0.0， x>=edge1时为1.0
|   
|length(x)| 向量长度
|distance(p0, p1)| 两点距离， length(p0-p1);
|dot(x, y)| 点积，各分量分别相乘 后 相加
|cross(x, y)| 差积，x[1]*y[2]-y[1]*x[2], x[2]*y[0] - y[2]*x[0], x[0]*y[1] - y[0]*x[1]
|normalize(x)| 归一化， length(x)=1;
|faceforward(N, I, Nref)| 如 dot(Nref, I)< 0则N, 否则 -N
|reflect(I, N)| I的反射方向， I -2*dot(N, I)*N, N必须先归一化
|refract(I, N, eta)| 折射，k=1.0-eta*eta*(1.0 - dot(N, I) * dot(N, I)); 如k<0.0 则0.0，否则 eta*I - (eta*dot(N, I)+sqrt(k))*N
|   
|matrixCompMult(matX, matY)| 矩阵相乘, 每个分量 自行相乘， 即 r[i][j] = x[i][j]*y[i][j];矩阵线性相乘，直接用 *|
|lessThan(vecX, vecY)| 向量 每个分量比较 x < y
|lessThanEqual(vecX, vecY)| 向量 每个分量比较 x<=y
|greaterThan(vecX, vecY)| 向量 每个分量比较 x>y
|greaterThanEqual(vecX, vecY)| 向量 每个分量比较 x>=y
|equal(vecX, vecY)| 向量 每个分量比较 x==y
|notEqual(vecX, vexY)| 向量 每个分量比较 x!=y
|any(bvecX)| 只要有一个分量是true， 则true
|all(bvecX)| 所有分量是true， 则true
|not(bvecX)| 所有分量取反
|   
|texture2D(sampler2D, coord)| texture lookup
|texture2D(sampler2D, coord, bias)| LOD bias, mip-mapped texture
|texture2DProj(sampler2D, coord)|
|texture2DProj(sampler2D, coord, bias)|
|texture2DLod(sampler2D, coord, lod)|
|texture2DProjLod(sampler2D, coord, lod)|
|textureCube(samplerCube, coord)|
|textureCube(samplerCube, coord, bias)|
|textureCubeLod(samplerCube, coord, lod)|   
