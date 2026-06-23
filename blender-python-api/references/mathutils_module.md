# mathutils_module

---
## mathutils

# mathutils Module Reference

mathutils 模块提供数学运算工具，用于向量、矩阵、四元数、欧拉角和颜色运算。

## Vector (向量)

向量用于表示 3D 空间中的方向和位置。

### 创建向量

```python
import mathutils

# 创建 2D 向量
vec2 = mathutils.Vector((1.0, 2.0))

# 创建 3D 向量
vec3 = mathutils.Vector((1.0, 2.0, 3.0))

# 创建 4D 向量
vec4 = mathutils.Vector((1.0, 2.0, 3.0, 4.0))

# 从列表创建
vec = mathutils.Vector([1.0, 2.0, 3.0])

# 从元组创建
vec = mathutils.Vector((1.0, 2.0, 3.0))
```

### 向量属性

```python
vec = mathutils.Vector((1.0, 2.0, 3.0))

# 向量长度
length = vec.length

# 向量长度的平方
length_squared = vec.length_squared

# 向量维度
size = len(vec)

# 归一化向量
normalized = vec.normalized()

# 向量是否归一化
is_normalized = vec.is_normalized

# 向量是否为零
is_zero = vec.is_zero
```

### 向量运算

```python
vec1 = mathutils.Vector((1.0, 2.0, 3.0))
vec2 = mathutils.Vector((4.0, 5.0, 6.0))

# 加法
result = vec1 + vec2

# 减法
result = vec1 - vec2

# 乘法 (标量)
result = vec1 * 2.0

# 除法 (标量)
result = vec1 / 2.0

# 点积
dot = vec1.dot(vec2)

# 叉积
cross = vec1.cross(vec2)

# 反射
reflected = vec1.reflect(vec2)

# 投影
projected = vec1.project(vec2)

# 线性插值
result = vec1.lerp(vec2, 0.5)

# 球面线性插值
result = vec1.slerp(vec2, 0.5)

# 旋转
rotated = vec1.rotate(mathutils.Euler((0.0, 0.0, math.radians(45))))

# 复制
copied = vec1.copy()
```

### 向量方法

```python
vec = mathutils.Vector((1.0, 2.0, 3.0))

# 归一化
vec.normalize()

# 反转向量
vec.negate()

# 缩放到指定长度
vec.resize(5.0)

# 获取角度
angle = vec.angle(vec2)

# 获取距离
distance = vec.distance(vec2)

# 获取距离的平方
distance_squared = vec.distance_squared(vec2)

# 获取中点
midpoint = vec.midpoint(vec2)

# 正交化
vec.orthogonalize(vec2)

# 旋转
vec.rotate(euler)

# 获取 XYZ 分量
x = vec.x
y = vec.y
z = vec.z

# 设置 XYZ 分量
vec.x = 1.0
vec.y = 2.0
vec.z = 3.0

# 转换为元组
tuple_vec = tuple(vec)

# 转换为列表
list_vec = list(vec)
```

## Matrix (矩阵)

矩阵用于表示变换。

### 创建矩阵

```python
import mathutils

# 创建单位矩阵
matrix = mathutils.Matrix()

# 创建 3x3 矩阵
matrix_3x3 = mathutils.Matrix((
    (1.0, 0.0, 0.0),
    (0.0, 1.0, 0.0),
    (0.0, 0.0, 1.0)
))

# 创建 4x4 矩阵
matrix_4x4 = mathutils.Matrix((
    (1.0, 0.0, 0.0, 0.0),
    (0.0, 1.0, 0.0, 0.0),
    (0.0, 0.0, 1.0, 0.0),
    (0.0, 0.0, 0.0, 1.0)
))

# 从列表创建
matrix = mathutils.Matrix([
    [1.0, 0.0, 0.0, 0.0],
    [0.0, 1.0, 0.0, 0.0],
    [0.0, 0.0, 1.0, 0.0],
    [0.0, 0.0, 0.0, 1.0]
])
```

### 矩阵构造方法

```python
# 平移矩阵
matrix = mathutils.Matrix.Translation((1.0, 2.0, 3.0))

# 旋转矩阵
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')

# 缩放矩阵
matrix = mathutils.Matrix.Scale(2.0, 4)

# 正交矩阵
matrix = mathutils.Matrix.OrthoProjection('Z', 4, 4, 0, 10)

# 视图矩阵
matrix = mathutils.Matrix.LookAt(
    eye=(0.0, -10.0, 0.0),
    target=(0.0, 0.0, 0.0),
    up=(0.0, 0.0, 1.0)
)

# 透视投影矩阵
matrix = mathutils.Matrix.PerspectiveProjection(
    fov=math.radians(45),
    aspect=1.0,
    near=0.1,
    far=100.0
)

# 从四元数创建矩阵
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))
matrix = quat.to_matrix().to_4x4()

# 从欧拉角创建矩阵
euler = mathutils.Euler((math.radians(45), 0.0, 0.0))
matrix = euler.to_matrix().to_4x4()
```

### 矩阵属性

```python
matrix = mathutils.Matrix()

# 矩阵维度
rows = len(matrix)
cols = len(matrix[0])

# 矩阵是否可逆
is_invertible = matrix.is_invertible

# 矩阵是否正交
is_orthogonal = matrix.is_orthogonal

# 矩阵是否单位矩阵
is_identity = matrix.is_identity

# 矩阵行列式
determinant = matrix.determinant()

# 矩阵迹
trace = matrix.trace()

# 矩阵转置
transposed = matrix.transposed()

# 矩阵逆
inverse = matrix.inverted()

# 矩阵伴随
adjugate = matrix.adjugate()

# 矩阵缩放因子
scale = matrix.to_scale()

# 矩阵旋转部分
rotation = matrix.to_quaternion()

# 矩阵平移部分
translation = matrix.to_translation()

# 矩阵欧拉角
euler = matrix.to_euler()
```

### 矩阵运算

```python
mat1 = mathutils.Matrix()
mat2 = mathutils.Matrix()
vec = mathutils.Vector((1.0, 2.0, 3.0))

# 矩阵乘法
result = mat1 @ mat2

# 矩阵向量乘法
result = mat1 @ vec

# 矩阵加法
result = mat1 + mat2

# 矩阵减法
result = mat1 - mat2

# 矩阵标量乘法
result = mat1 * 2.0

# 矩阵标量除法
result = mat1 / 2.0

# 矩阵幂运算
result = mat1 ** 2

# 矩阵插值
result = mat1.lerp(mat2, 0.5)

# 矩阵分解
translation, rotation, scale = mat1.decompose()

# 矩阵重新组合
matrix = mathutils.Matrix.LocRotScale(translation, rotation, scale)
```

### 矩阵方法

```python
matrix = mathutils.Matrix()

# 重置为单位矩阵
matrix.identity()

# 转置矩阵
matrix.transpose()

# 求逆矩阵
if matrix.is_invertible:
    matrix.invert()

# 归一化矩阵
matrix.normalize()

# 缩放矩阵
matrix.scale(scale_vector)

# 旋转矩阵
matrix.rotate(rotation_matrix)

# 平移矩阵
matrix.translate(translation_vector)

# 复制矩阵
copied = matrix.copy()

# 转换为 3x3 矩阵
matrix_3x3 = matrix.to_3x3()

# 转换为 4x4 矩阵
matrix_4x4 = matrix.to_4x4()

# 转换为列表
list_matrix = list(matrix)

# 转换为元组
tuple_matrix = tuple(tuple(row) for row in matrix)
```

## Euler (欧拉角)

欧拉角用于表示旋转。

### 创建欧拉角

```python
import mathutils
import math

# 创建欧拉角
euler = mathutils.Euler((math.radians(45), math.radians(30), math.radians(60)))

# 指定旋转顺序
euler = mathutils.Euler((math.radians(45), math.radians(30), math.radians(60)), 'XYZ')

# 从四元数创建
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))
euler = quat.to_euler()

# 从矩阵创建
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')
euler = matrix.to_euler()
```

### 欧拉角属性

```python
euler = mathutils.Euler((math.radians(45), math.radians(30), math.radians(60)))

# 旋转角度
x_angle = euler.x
y_angle = euler.y
z_angle = euler.z

# 旋转顺序
order = euler.order

# 转换为度数
x_deg = math.degrees(euler.x)
y_deg = math.degrees(euler.y)
z_deg = math.degrees(euler.z)

# 转换为四元数
quat = euler.to_quaternion()

# 转换为矩阵
matrix = euler.to_matrix()

# 复制欧拉角
copied = euler.copy()
```

### 欧拉角运算

```python
euler1 = mathutils.Euler((math.radians(45), 0.0, 0.0))
euler2 = mathutils.Euler((0.0, math.radians(30), 0.0))

# 欧拉角加法
result = euler1 + euler2

# 欧拉角减法
result = euler1 - euler2

# 欧拉角乘法
result = euler1 * 2.0

# 欧拉角除法
result = euler1 / 2.0

# 欧拉角插值
result = euler1.lerp(euler2, 0.5)

# 欧拉角旋转
result = euler1.rotate(euler2)

# 欧拉角反转
result = -euler1

# 欧拉角归一化
result = euler1.normalized()
```

## Quaternion (四元数)

四元数用于表示旋转，避免万向节锁问题。

### 创建四元数

```python
import mathutils
import math

# 创建四元数 (w, x, y, z)
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))

# 从轴角创建
quat = mathutils.Quaternion((0.0, 0.0, 1.0), math.radians(45))

# 从欧拉角创建
euler = mathutils.Euler((math.radians(45), 0.0, 0.0))
quat = euler.to_quaternion()

# 从矩阵创建
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')
quat = matrix.to_quaternion()

# 单位四元数
identity = mathutils.Quaternion()

# 旋转四元数
quat = mathutils.Quaternion.RotationDifference(vec1, vec2)
```

### 四元数属性

```python
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))

# 四元数分量
w = quat.w
x = quat.x
y = quat.y
z = quat.z

# 四元数长度
length = quat.length

# 四元数是否归一化
is_normalized = quat.is_normalized

# 四元数共轭
conjugate = quat.conjugated()

# 四元数逆
inverse = quat.inverted()

# 四元数轴角
axis, angle = quat.to_axis_angle()

# 转换为欧拉角
euler = quat.to_euler()

# 转换为矩阵
matrix = quat.to_matrix()

# 复制四元数
copied = quat.copy()
```

### 四元数运算

```python
quat1 = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))
quat2 = mathutils.Quaternion((0.0, 0.0, 0.0, 1.0))
vec = mathutils.Vector((1.0, 0.0, 0.0))

# 四元数乘法
result = quat1 @ quat2

# 四元数向量旋转
result = quat1 @ vec

# 四元数插值
result = quat1.lerp(quat2, 0.5)

# 球面线性插值
result = quat1.slerp(quat2, 0.5)

# 四元数归一化
quat1.normalize()

# 四元数旋转
quat1.rotate(quat2)

# 四元数反转
result = -quat1

# 四元数点积
dot = quat1.dot(quat2)

# 四元数指数
result = quat1.exp()

# 四元数对数
result = quat1.log()

# 四元数幂
result = quat1 ** 2
```

## Color (颜色)

颜色用于表示 RGB 和 HSV 颜色空间。

### 创建颜色

```python
import mathutils

# 创建 RGB 颜色
color = mathutils.Color((1.0, 0.5, 0.0))

# 创建 HSV 颜色
color = mathutils.Color()
color.hsv = (0.1, 0.8, 0.9)

# 从十六进制创建
color = mathutils.Color()
color.rgb = (0xFF / 255.0, 0x80 / 255.0, 0x00 / 255.0)

# 复制颜色
copied = color.copy()
```

### 颜色属性

```python
color = mathutils.Color((1.0, 0.5, 0.0))

# RGB 分量
r = color.r
g = color.g
b = color.b

# HSV 分量
h, s, v = color.hsv

# 亮度
brightness = color.v

# 饱和度
saturation = color.s

# 色相
hue = color.h

# 转换为元组
tuple_color = tuple(color)

# 转换为列表
list_color = list(color)

# 转换为十六进制
hex_color = f"#{int(color.r * 255):02x}{int(color.g * 255):02x}{int(color.b * 255):02x}"
```

### 颜色运算

```python
color1 = mathutils.Color((1.0, 0.0, 0.0))
color2 = mathutils.Color((0.0, 1.0, 0.0))

# 颜色加法
result = color1 + color2

# 颜色减法
result = color1 - color2

# 颜色乘法
result = color1 * 2.0

# 颜色除法
result = color1 / 2.0

# 颜色插值
result = color1.lerp(color2, 0.5)

# 颜色混合
result = color1.mix(color2, 0.5)

# 颜色反转
result = -color1

# 颜色亮度调整
color1.brightness = 0.8

# 颜色饱和度调整
color1.saturation = 0.6

# 颜色色相调整
color1.hue = 0.3
```

## 实用工具函数

### mathutils.geometry

几何运算工具函数。

```python
import mathutils.geometry

# 计算三角形面积
area = mathutils.geometry.area_tri(v1, v2, v3)

# 计算三角形法线
normal = mathutils.geometry.normal(v1, v2, v3)

# 计算点到线段的距离
distance = mathutils.geometry.distance_point_to_line(point, line_start, line_end)

# 计算点到平面的距离
distance = mathutils.geometry.distance_point_to_plane(point, plane_co, plane_no)

# 计算线段相交
intersect = mathutils.geometry.intersect_line_line(line1_start, line1_end, line2_start, line2_end)

# 计算线段平面相交
intersect = mathutils.geometry.intersect_line_plane(line_start, line_end, plane_co, plane_no)

# 计算三角形相交
intersect = mathutils.geometry.intersect_ray_tri(v1, v2, v3, ray_origin, ray_direction)

# 计算球体相交
intersect = mathutils.geometry.intersect_sphere_sphere(center1, radius1, center2, radius2)

# 计算包围盒
bbox = mathutils.geometry.box_fit_2d(points)
bbox = mathutils.geometry.box_fit_3d(points)
```

### mathutils.noise

噪声函数。

```python
import mathutils.noise

# 柏林噪声
value = mathutils.noise.noise(position)

# 湍流噪声
value = mathutils.noise.turbulence(position, octaves=6, hard=True)

# 分形布朗运动
value = mathutils.noise.fractal(position, H=1.0, lacunarity=2.0, octaves=6)

# 混合噪声
value = mathutils.noise.hybrid_multi_fractal(position, H=1.0, lacunarity=2.0, octaves=6)

# 多重分形
value = mathutils.noise.multi_fractal(position, H=1.0, lacunarity=2.0, octaves=6)

# 异向噪声
value = mathutils.noise.hetero_terrain(position, H=1.0, lacunarity=2.0, octaves=6)

# 山脊噪声
value = mathutils.noise.ridged_multi_fractal(position, H=1.0, lacunarity=2.0, octaves=6)

# 沃利噪声
value = mathutils.noise.voronoi(position)
```

## 完整示例：3D 变换系统

```python
import bpy
import mathutils
import math

# 创建变换系统
class TransformSystem:
    def __init__(self):
        self.position = mathutils.Vector((0.0, 0.0, 0.0))
        self.rotation = mathutils.Quaternion()
        self.scale = mathutils.Vector((1.0, 1.0, 1.0))
        
    def get_matrix(self):
        """获取变换矩阵"""
        translation_matrix = mathutils.Matrix.Translation(self.position)
        rotation_matrix = self.rotation.to_matrix().to_4x4()
        scale_matrix = mathutils.Matrix.Scale(self.scale.x, 4, (1.0, 0.0, 0.0))
        scale_matrix @= mathutils.Matrix.Scale(self.scale.y, 4, (0.0, 1.0, 0.0))
        scale_matrix @= mathutils.Matrix.Scale(self.scale.z, 4, (0.0, 0.0, 1.0))
        
        return translation_matrix @ rotation_matrix @ scale_matrix
    
    def set_matrix(self, matrix):
        """从矩阵设置变换"""
        self.position = matrix.to_translation()
        self.rotation = matrix.to_quaternion()
        self.scale = matrix.to_scale()
    
    def translate(self, translation):
        """平移"""
        self.position += translation
    
    def rotate(self, axis, angle):
        """绕轴旋转"""
        rotation_quat = mathutils.Quaternion(axis, angle)
        self.rotation = rotation_quat @ self.rotation
    
    def scale_by(self, scale_factor):
        """缩放"""
        self.scale *= scale_factor
    
    def look_at(self, target, up=(0.0, 0.0, 1.0)):
        """看向目标"""
        direction = (target - self.position).normalized()
        up_vector = mathutils.Vector(up).normalized()
        
        # 计算旋转矩阵
        z_axis = direction
        x_axis = up_vector.cross(z_axis).normalized()
        y_axis = z_axis.cross(x_axis)
        
        rotation_matrix = mathutils.Matrix((
            (x_axis.x, y_axis.x, z_axis.x),
            (x_axis.y, y_axis.y, z_axis.y),
            (x_axis.z, y_axis.z, z_axis.z)
        ))
        
        self.rotation = rotation_matrix.to_quaternion()
    
    def interpolate(self, other, factor):
        """插值到另一个变换"""
        self.position = self.position.lerp(other.position, factor)
        self.rotation = self.rotation.slerp(other.rotation, factor)
        self.scale = self.scale.lerp(other.scale, factor)

# 使用示例
transform = TransformSystem()

# 设置位置
transform.position = mathutils.Vector((2.0, 3.0, 1.0))

# 旋转
transform.rotate(mathutils.Vector((0.0, 0.0, 1.0)), math.radians(45))

# 缩放
transform.scale_by(2.0)

# 看向目标
transform.look_at(mathutils.Vector((0.0, 0.0, 0.0)))

# 获取变换矩阵
matrix = transform.get_matrix()

print(f"位置: {transform.position}")
print(f"旋转: {transform.rotation}")
print(f"缩放: {transform.scale}")
print(f"变换矩阵:\n{matrix}")
```

## 性能优化建议

1. **避免频繁创建对象**：重用向量、矩阵等对象，而不是频繁创建新对象
2. **使用原地操作**：对于需要修改的向量，使用 `vec.rotate()` 而不是 `vec = vec.rotated()`
3. **批量操作**：使用矩阵运算进行批量变换，而不是逐个变换
4. **使用适当的数学结构**：根据需求选择向量、四元数或欧拉角
5. **缓存计算结果**：对于重复使用的计算结果，进行缓存
6. **使用 numpy 集成**：对于大量数值计算，考虑使用 numpy 集成
    up=(0.0, 0.0, 1.0)
)
```

### 矩阵运算

```python
mat1 = mathutils.Matrix(((1.0, 0.0, 0.0), (0.0, 1.0, 0.0), (0.0, 0.0, 1.0)))
mat2 = mathutils.Matrix(((2.0, 0.0, 0.0), (0.0, 2.0, 0.0), (0.0, 0.0, 2.0)))

# 矩阵乘法
result = mat1 @ mat2

# 向量乘法
vec = mathutils.Vector((1.0, 2.0, 3.0))
result = mat1 @ vec

# 矩阵求逆
inverted = mat1.inverted()

# 矩阵转置
transposed = mat1.transposed()

# 矩阵行列式
determinant = mat1.determinant()

# 矩阵复制
copied = mat1.copy()
```

### 矩阵分解

```python
matrix = mathutils.Matrix((
    (1.0, 0.0, 0.0, 1.0),
    (0.0, 1.0, 0.0, 2.0),
    (0.0, 0.0, 1.0, 3.0),
    (0.0, 0.0, 0.0, 1.0)
))

# 分解为平移、旋转、缩放
loc, rot, scale = matrix.decompose()

# 获取平移
location = matrix.translation

# 获取旋转
rotation = matrix.to_quaternion()
rotation = matrix.to_euler()

# 获取缩放
scale = matrix.to_scale()
```

## Quaternion (四元数)

四元数用于表示旋转。

### 创建四元数

```python
import mathutils

# 创建单位四元数
quat = mathutils.Quaternion()

# 从轴和角度创建
quat = mathutils.Quaternion((0.0, 0.0, 1.0), math.radians(45))

# 从欧拉角创建
euler = mathutils.Euler((0.0, 0.0, math.radians(45)))
quat = euler.to_quaternion()

# 从矩阵创建
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')
quat = matrix.to_quaternion()

# 从分量创建
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))
```

### 四元数属性

```python
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))

# 四元数分量
w = quat.w
x = quat.x
y = quat.y
z = quat.z

# 四元数角度
angle = quat.angle

# 四元数轴
axis = quat.axis

# 四元数是否归一化
is_normalized = quat.is_normalized

# 四元数是否为零
is_zero = quat.is_zero
```

### 四元数运算

```python
quat1 = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))
quat2 = mathutils.Quaternion((0.0, 1.0, 0.0, 0.0))

# 四元数乘法
result = quat1 @ quat2

# 四元数共轭
conjugated = quat1.conjugated()

# 四元数求逆
inverted = quat1.inverted()

# 四元数归一化
normalized = quat1.normalized()

# 四元数点积
dot = quat1.dot(quat2)

# 四元数旋转
rotated = quat1.rotate(quat2)

# 四元数线性插值
result = quat1.slerp(quat2, 0.5)

# 四元数复制
copied = quat1.copy()
```

### 四元数方法

```python
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))

# 归一化
quat.normalize()

# 反转
quat.invert()

# 共轭
quat.conjugate()

# 旋转向量
vec = mathutils.Vector((1.0, 0.0, 0.0))
rotated = quat @ vec

# 转换为欧拉角
euler = quat.to_euler()

# 转换为矩阵
matrix = quat.to_matrix()

# 转换为轴和角度
axis, angle = quat.to_axis_angle()
```

## Euler (欧拉角)

欧拉角用于表示旋转。

### 创建欧拉角

```python
import mathutils

# 创建欧拉角
euler = mathutils.Euler((0.0, 0.0, 0.0))

# 从四元数创建
quat = mathutils.Quaternion((1.0, 0.0, 0.0, 0.0))
euler = quat.to_euler()

# 从矩阵创建
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')
euler = matrix.to_euler()

# 指定旋转顺序
euler = mathutils.Euler((0.0, 0.0, math.radians(45)), 'XYZ')
```

### 欧拉角属性

```python
euler = mathutils.Euler((0.0, 0.0, 0.0))

# 欧拉角分量
x = euler.x
y = euler.y
z = euler.z

# 旋转顺序
order = euler.order  # 'XYZ', 'XZY', 'YXZ', 'YZX', 'ZXY', 'ZYX'

# 是否包装
is_wrapped = euler.is_wrapped
```

### 欧拉角运算

```python
euler1 = mathutils.Euler((0.0, 0.0, 0.0))
euler2 = mathutils.Euler((0.0, 0.0, math.radians(45)))

# 欧拉角加法
result = euler1 + euler2

# 欧拉角减法
result = euler1 - euler2

# 欧拉角复制
copied = euler1.copy()
```

### 欧拉角方法

```python
euler = mathutils.Euler((0.0, 0.0, 0.0))

# 包装角度
euler.make_compatible(other_euler)

# 归一化
euler.normalize()

# 转换为四元数
quat = euler.to_quaternion()

# 转换为矩阵
matrix = euler.to_matrix()

# 旋转向量
vec = mathutils.Vector((1.0, 0.0, 0.0))
rotated = euler.to_quaternion() @ vec
```

## Color (颜色)

颜色用于表示 RGBA 颜色。

### 创建颜色

```python
import mathutils

# 创建颜色
color = mathutils.Color((1.0, 0.0, 0.0))

# 从十六进制创建
color = mathutils.Color((1.0, 0.0, 0.0))
color.from_hex('#FF0000')

# 从 HSV 创建
color = mathutils.Color()
color.from_hsv((0.0, 1.0, 1.0))
```

### 颜色属性

```python
color = mathutils.Color((1.0, 0.0, 0.0))

# 颜色分量
r = color.r
g = color.g
b = color.b

# 颜色复制
copied = color.copy()
```

### 颜色运算

```python
color1 = mathutils.Color((1.0, 0.0, 0.0))
color2 = mathutils.Color((0.0, 1.0, 0.0))

# 颜色加法
result = color1 + color2

# 颜色减法
result = color1 - color2

# 颜色乘法 (标量)
result = color1 * 2.0

# 颜色除法 (标量)
result = color1 / 2.0

# 颜色混合
result = color1.blend(color2, 0.5)
```

### 颜色方法

```python
color = mathutils.Color((1.0, 0.0, 0.0))

# 转换为 HSV
hsv = color.hsv

# 从 HSV 设置
color.hsv = (0.0, 1.0, 1.0)

# 转换为十六进制
hex_str = color.to_hex()

# 从十六进制设置
color.from_hex('#FF0000')

# 饱和度
color.saturate(0.5)

# 反转颜色
color.invert()

# 复制
copied = color.copy()
```

## 常用数学函数

```python
import mathutils

# 线性插值
result = mathutils.lerp(0.0, 10.0, 0.5)

# 球面线性插值
result = mathutils.slerp(0.0, 10.0, 0.5)

# 限制值
result = mathutils.clamp(5.0, 0.0, 10.0)

# 向下取整
result = mathutils.floor(5.7)

# 向上取整
result = mathutils.ceil(5.3)

# 四舍五入
result = mathutils.round(5.5)

# 模运算
result = mathutils.modulo(5.0, 2.0)

# 吸附
result = mathutils.snap(5.7, 1.0)
```

## 几何运算

```python
import mathutils

# 点到线的距离
point = mathutils.Vector((0.0, 0.0, 0.0))
line_p1 = mathutils.Vector((1.0, 0.0, 0.0))
line_p2 = mathutils.Vector((2.0, 0.0, 0.0))
distance = mathutils.geometry.point_line_distance(point, line_p1, line_p2)

# 点到面的距离
plane_co = mathutils.Vector((0.0, 0.0, 0.0))
plane_no = mathutils.Vector((0.0, 0.0, 1.0))
distance = mathutils.geometry.distance_point_to_plane(point, plane_co, plane_no)

# 线段相交
intersect = mathutils.geometry.intersect_line_line_2d(
    line_a_p1, line_a_p2,
    line_b_p1, line_b_p2
)

# 三角形重心
barycentric = mathutils.geometry.barycentric_transform(
    point,
    tri_a, tri_b, tri_c,
    tri_a_new, tri_b_new, tri_c_new
)

# 多边形面积
area = mathutils.geometry.area_tri(p1, p2, p3)
```

## 实用示例

### 计算两点距离

```python
import mathutils

point1 = mathutils.Vector((0.0, 0.0, 0.0))
point2 = mathutils.Vector((1.0, 1.0, 1.0))
distance = (point1 - point2).length
```

### 计算两个向量夹角

```python
import mathutils

vec1 = mathutils.Vector((1.0, 0.0, 0.0))
vec2 = mathutils.Vector((0.0, 1.0, 0.0))
angle = vec1.angle(vec2)
```

### 旋转向量

```python
import mathutils

vec = mathutils.Vector((1.0, 0.0, 0.0))
euler = mathutils.Euler((0.0, 0.0, math.radians(45)))
rotated = vec.rotate(euler)
```

### 创建变换矩阵

```python
import mathutils

# 平移
translation = mathutils.Matrix.Translation((1.0, 2.0, 3.0))

# 旋转
rotation = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')

# 缩放
scale = mathutils.Matrix.Scale(2.0, 4)

# 组合变换
transform = translation @ rotation @ scale
```

### 颜色混合

```python
import mathutils

color1 = mathutils.Color((1.0, 0.0, 0.0))
color2 = mathutils.Color((0.0, 1.0, 0.0))
blended = color1.blend(color2, 0.5)
```

### 欧拉角转四元数

```python
import mathutils

euler = mathutils.Euler((0.0, 0.0, math.radians(45)))
quat = euler.to_quaternion()
```

### 四元数转矩阵

```python
import mathutils

quat = mathutils.Quaternion((0.0, 0.0, 1.0), math.radians(45))
matrix = quat.to_matrix().to_4x4()
```


---
## mathutils_bvhtree

# mathutils.bvhtree - BVHTree Utilities Module

BVHTree模块提供了包围体层次树（Bounding Volume Hierarchy Tree）的创建和查询功能，用于高效的碰撞检测、射线投射和最近点查找。

## 概述

BVHTree是一种空间数据结构，将几何体组织成层次化的包围体，以便快速进行空间查询。它在物理模拟、碰撞检测、光线追踪和视觉选择等场景中非常有用。

## 核心功能

### BVHTree创建

```python
import bpy
import mathutils
from mathutils.bvhtree import BVHTree

def create_bvh_from_mesh(mesh):
    """从网格创建BVHTree"""
    verts = [v.co for v in mesh.vertices]
    edges = [e.vertices for e in mesh.edges]
    return BVHTree.FromPolygons(verts, edges)

def create_bvh_from_object(obj):
    """从对象创建BVHTree"""
    if obj.type == 'MESH':
        depsgraph = bpy.context.evaluated_depsgraph_get()
        eval_obj = obj.evaluated_get(depsgraph)
        mesh = eval_obj.to_mesh()
        try:
            return create_bvh_from_mesh(mesh)
        finally:
            eval_obj.to_mesh_clear()

def create_bvh_from_faces(vertices, face_indices):
    """从顶点和面索引创建BVHTree"""
    return BVHTree.FromPolygons(vertices, face_indices)
```

### 查找最近点

```python
def find_nearest_point(bvh_tree, point):
    """查找BVHTree中最近的点"""
    nearest_co, normal, index, distance = bvh_tree.find_nearest(point)
    return {
        'point': nearest_co,
        'normal': normal,
        'face_index': index,
        'distance': distance
    }

def find_nearest_point_on_mesh(obj, point):
    """在对象表面查找最近点"""
    bvh = create_bvh_from_object(obj)
    if bvh:
        return find_nearest_point(bvh, point)
    return None

def find_all_nearby_points(bvh_tree, point, max_distance):
    """查找一定距离内的所有点"""
    results = []
    bvh_tree.find_nearest_range(point, max_distance, lambda co, no, index, dist: results.append({
        'point': co,
        'normal': no,
        'face_index': index,
        'distance': dist
    }))
    return results
```

### 射线投射

```python
def ray_cast(bvh_tree, origin, direction, distance=1.70141e+38):
    """从原点沿方向发射射线"""
    location, normal, index, distance = bvh_tree.ray_cast(origin, direction, distance)
    if location is not None:
        return {
            'location': location,
            'normal': normal,
            'face_index': index,
            'distance': distance
        }
    return None

def ray_cast_from_mouse(bvh_tree, mouse_pos, camera_matrix, distance=1000):
    """从鼠标位置发射射线"""
    region = bpy.context.region
    rv3d = bpy.context.space_data.region_3d
    view_vector = rv3d.view_vector_from_point(mouse_pos)
    return ray_cast(bvh_tree, mouse_pos, view_vector, distance)

def ray_cast_all_intersections(bvh_tree, origin, direction):
    """获取射线的所有交点"""
    results = []
    def callback(loc, nor, idx, dist):
        results.append({'location': loc, 'normal': nor, 'index': idx, 'distance': dist})
    bvh_tree.ray_cast(origin, direction, lambda loc, nor, idx, dist: callback(loc, nor, idx, dist) or dist)
    return results
```

### 碰撞检测

```python
def check_collision(bvh1, bvh2):
    """检查两个BVHTree是否相交"""
    overlapping = bvh1.overlap(bvh2)
    return len(overlapping) > 0

def get_colliding_faces(bvh1, bvh2):
    """获取相交的面对"""
    return bvh1.overlap(bvh2)

def get_collision_pairs(trees_dict):
    """获取所有碰撞的物体对"""
    objects = list(trees_dict.keys())
    collisions = []
    for i, obj1 in enumerate(objects):
        for obj2 in objects[i+1:]:
            if check_collision(trees_dict[obj1], trees_dict[obj2]):
                collisions.append((obj1, obj2))
    return collisions

def distance_between_objects(obj1, obj2):
    """计算两个对象之间的最短距离"""
    bvh1 = create_bvh_from_object(obj1)
    bvh2 = create_bvh_from_object(obj2)
    if bvh1 and bvh2:
        pairs = bvh1.overlap(bvh2)
        if pairs:
            min_dist = float('inf')
            for pair in pairs:
                for p1 in pair[0]:
                    for p2 in pair[1]:
                        dist = (p1 - p2).length
                        if dist < min_dist:
                            min_dist = dist
            return min_dist
    return None
```

## 实用函数

### 简单封装

```python
def build_bvh(obj, depsgraph=None):
    """快速构建对象的BVH树"""
    if depsgraph is None:
        depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    try:
        verts = [v.co for v in mesh.vertices]
        polys = [p.vertices for p in mesh.polygons]
        return BVHTree.FromPolygons(verts, polys)
    finally:
        eval_obj.to_mesh_clear()

def update_scene_bvh():
    """更新场景中所有对象的BVH"""
    bvh_trees = {}
    depsgraph = bpy.context.evaluated_depsgraph_get()
    for obj in bpy.context.scene.objects:
        if obj.type == 'MESH':
            bvh_trees[obj.name] = build_bvh(obj, depsgraph)
    return bvh_trees
```

### 高级查询

```python
def find_points_in_sphere(bvh_tree, center, radius):
    """查找球体内的所有点"""
    points = []
    bvh_tree.find_nearest_range(center, radius, 
        lambda co, no, idx, dist: points.append({'point': co, 'index': idx}))
    return points

def get_face_center(bvh_tree, face_index):
    """获取指定面的中心点"""
    for vert in bvh_tree.polygons[face_index]:
        pass
    return None

def project_point_to_surface(bvh_tree, point):
    """将点投影到BVH表面"""
    result = bvh_tree.find_nearest(point)
    if result[0] is not None:
        return result[0], result[1], result[3]
    return None, None, None
```

## 常见问题排查

### 性能问题

BVHTree查询很慢：
- 确保BVHTree只创建一次并复用
- 对于动态对象，使用eval对象创建
- 使用较低的精度设置可以提高速度

内存占用过高：
- 及时清理不再使用的BVHTree
- 对于大型场景，分区域创建BVHTree

### 射线投射无结果

射线方向错误：
- 确保方向向量是归一化的
- 检查原点和方向是否在正确的坐标系中

距离值太小：
- 增大ray_cast的distance参数
- 默认距离可能不足以到达目标

### 碰撞检测不准确

顶点位置不匹配：
- 确保使用evaluated对象获取网格数据
- 检查对象的世界矩阵变换

面索引无效：
- 检查返回的face_index是否在有效范围内
- 网格数据可能已更改，需要重新创建BVHTree


---
## mathutils_geometry

# mathutils.geometry 模块

## 模块概述

mathutils.geometry 模块提供了一系列用于几何计算的函数，这些函数是进行 3D 几何操作和空间分析的基础工具。该模块包含了向量、矩阵之外的专门用于几何关系的计算功能，如点线距离、射线检测、面法线计算、曲线插值等。这些函数在游戏开发、动画制作、几何处理等场景中有着广泛的应用。

与其他 mathutils 子模块不同，geometry 模块不包含类定义，而是纯粹提供静态函数。这种设计使得这些几何计算函数可以直接调用，无需创建对象实例。所有的函数都经过优化，能够高效处理大规模的几何计算任务。函数的输入输出通常使用 Vector 对象，方便与其他 mathutils 模块配合使用。

该模块中的函数可以分为几个主要类别：距离计算函数用于测量空间中的各种距离关系，射线检测函数用于光线追踪和拾取操作，插值函数用于生成平滑的曲线路径，面积和法线函数用于网格几何分析。理解这些函数的功能和使用场景对于编写高效的 3D 脚本至关重要。

## 距离计算函数

### 点到点的距离

点到点距离是最基本的几何计算，用于测量两个三维空间点之间的直线距离。这个函数在碰撞检测、路径规划、相似度比较等场景中非常有用。mathutils.geometry.distance_point_to_point 函数接收两个 Vector 对象作为输入，返回它们之间的欧几里得距离。

除了距离值，有时还需要获取距离向量，即从一个点指向另一个点的向量。使用两个点的坐标相减即可得到距离向量，这个向量包含了方向信息，可以用于进一步的计算。

```python
from mathutils.geometry import distance_point_to_point
from mathutils import Vector

# 计算两点之间的距离
point_a = Vector((0.0, 0.0, 0.0))
point_b = Vector((3.0, 4.0, 0.0))

distance = distance_point_to_point(point_a, point_b)
print(f"两点距离: {distance:.4f}")  # 输出: 5.0000

# 使用勾股定理验证
expected = (3.0**2 + 4.0**2) ** 0.5
print(f"验证结果: {expected:.4f}")

# 计算距离向量
distance_vector = point_b - point_a
print(f"距离向量: {distance_vector}")

# 批量计算点之间的距离
points = [
    Vector((0, 0, 0)),
    Vector((1, 2, 3)),
    Vector((4, 5, 6)),
    Vector((7, 8, 9))
]

print("\n点对距离矩阵:")
for i, p1 in enumerate(points):
    for j, p2 in enumerate(points):
        if i < j:
            dist = distance_point_to_point(p1, p2)
            print(f"  点{i} 到 点{j}: {dist:.4f}")
```

### 点到直线的距离

点到直线的距离计算返回空间中任意一点到给定直线的最短距离。这在碰撞检测、光线投射、几何约束等场景中非常重要。直线由直线上的一点和方向向量定义，函数会计算目标点到这条直线的垂直距离。

计算原理是找到直线上距离目标点最近的点，然后计算这两点之间的距离。这个最近点就是目标点在直线上的投影。函数同时返回距离值和投影点的位置，便于进行后续计算。

```python
from mathutils.geometry import distance_point_to_line
from mathutils import Vector

# 定义直线（通过点+方向向量）
line_point = Vector((0.0, 0.0, 0.0))
line_direction = Vector((1.0, 0.0, 0.0))

# 目标点
target_point = Vector((5.0, 3.0, 0.0))

# 计算点到直线的距离
distance, closest_point = distance_point_to_line(target_point, line_point, line_direction)

print(f"目标点: {target_point}")
print(f"直线上的最近点: {closest_point}")
print(f"点到直线的距离: {distance:.4f}")

# 验证：最近点到目标点的向量应该与直线方向垂直
vector_to_target = target_point - closest_point
print(f"垂直验证 (点积应接近0): {line_direction.dot(vector_to_target):.6f}")

# 三维空间中的点到直线距离
line_point_3d = Vector((0.0, 0.0, 0.0))
line_direction_3d = Vector((1.0, 1.0, 1.0)).normalized()
target_3d = Vector((3.0, 0.0, 0.0))

distance_3d, closest_3d = distance_point_to_line(target_3d, line_point_3d, line_direction_3d)
print(f"\n三维示例:")
print(f"  距离: {distance_3d:.4f}")
print(f"  最近点: {closest_3d}")
```

### 点到平面的距离

点到平面的距离计算返回空间中任意一点到给定平面的最短距离。平面由平面上的一点和法线向量定义。这个函数在平面检测、切割操作、投影计算等场景中非常有用。距离的符号表示点在平面的哪一侧，正值表示法线方向一侧，负值表示相反方向。

```python
from mathutils.geometry import distance_point_to_plane
from mathutils import Vector

# 定义平面（通过点+法线向量）
plane_point = Vector((0.0, 0.0, 0.0))
plane_normal = Vector((0.0, 0.0, 1.0))

# 目标点
target_point = Vector((2.0, 3.0, 5.0))

# 计算点到平面的距离
distance = distance_point_to_plane(target_point, plane_point, plane_normal)

print(f"平面法线: {plane_normal}")
print(f"目标点: {target_point}")
print(f"点到平面的距离: {distance:.4f}")
print(f"点是否在平面上方: {distance > 0}")

# 任意方向的平面
plane_point_2 = Vector((1.0, 1.0, 1.0))
plane_normal_2 = Vector((1.0, 1.0, 1.0)).normalized()
target_point_2 = Vector((3.0, 3.0, 3.0))

distance_2 = distance_point_to_plane(target_point_2, plane_point_2, plane_normal_2)
print(f"\n任意平面示例:")
print(f"  距离: {distance_2:.4f}")

# 分类空间中的点
points_to_classify = [
    Vector((0, 0, 2)),
    Vector((0, 0, -1)),
    Vector((1, 1, 0))
]

print("\n点分类:")
for point in points_to_classify:
    dist = distance_point_to_plane(point, plane_point, plane_normal)
    side = "上方" if dist > 0 else ("下方" if dist < 0 else "平面上")
    print(f"  {point}: {side} (距离: {dist:.4f})")
```

### 球体之间的距离

两个球体之间的距离计算用于判断球体是否相交以及相交的程度。这个函数接收两个球心位置和半径作为输入，返回球心之间的距离。通过比较距离和半径之和，可以判断球体是否相交、分离或包含。

```python
from mathutils.geometry import intersect_sphere_sphere
from mathutils import Vector

# 定义两个球体
sphere1_center = Vector((0.0, 0.0, 0.0))
sphere1_radius = 1.0

sphere2_center = Vector((2.5, 0.0, 0.0))
sphere2_radius = 1.0

# 计算球体相交情况
intersection = intersect_sphere_sphere(sphere1_center, sphere1_radius,
                                        sphere2_center, sphere2_radius)

print(f"球体1: 中心 {sphere1_center}, 半径 {sphere1_radius}")
print(f"球体2: 中心 {sphere2_center}, 半径 {sphere2_radius}")
print(f"相交结果: {intersection}")

# 判断球体关系
center_distance = (sphere2_center - sphere1_center).length
combined_radius = sphere1_radius + sphere2_radius

if center_distance > combined_radius:
    print("球体分离")
elif center_distance < abs(sphere1_radius - sphere2_radius):
    print("一个球体包含另一个")
else:
    print("球体相交")

# 批量检测球体碰撞
spheres = [
    {"center": Vector((0, 0, 0)), "radius": 1.0},
    {"center": Vector((1.5, 0, 0)), "radius": 0.8},
    {"center": Vector((5, 0, 0)), "radius": 1.0},
    {"center": Vector((0, 2, 0)), "radius": 0.5}
]

print("\n球体碰撞检测:")
for i, s1 in enumerate(spheres):
    for j, s2 in enumerate(spheres):
        if i < j:
            result = intersect_sphere_sphere(s1["center"], s1["radius"],
                                              s2["center"], s2["radius"])
            collision = "相交" if result else "分离"
            print(f"  球{i} 和 球{j}: {collision}")
```

## 射线检测函数

### 射线与平面的交点

射线与平面的交点计算是光线追踪和拾取操作的基础。射线由起点和方向定义，平面由点和法线定义。函数返回交点的位置，如果没有交点则返回 None。这个函数在实现鼠标拾取、视线检测、阴影计算等方面非常有用。

```python
from mathutils.geometry import intersect_ray_plane
from mathutils import Vector

# 定义射线
ray_origin = Vector((0.0, 0.0, 0.0))
ray_direction = Vector((0.0, 0.0, 1.0)).normalized()

# 定义平面
plane_origin = Vector((0.0, 0.0, 5.0))
plane_normal = Vector((0.0, 0.0, 1.0))

# 计算交点
intersection = intersect_ray_plane(ray_origin, ray_direction, plane_origin, plane_normal)

print(f"射线起点: {ray_origin}")
print(f"射线方向: {ray_direction}")
print(f"平面位置: {plane_origin}")
print(f"平面法线: {plane_normal}")

if intersection:
    print(f"交点: {intersection}")
else:
    print("射线与平面无交点")

# 射线与多个平面的交点
planes = [
    {"origin": Vector((0, 0, 2)), "normal": Vector((0, 0, 1))},
    {"origin": Vector((0, 0, 5)), "normal": Vector((0, 0, 1))},
    {"origin": Vector((0, 0, 10)), "normal": Vector((0, 0, 1))}
]

print("\n射线与多个平面的交点:")
for i, plane in enumerate(planes):
    point = intersect_ray_plane(ray_origin, ray_direction,
                                plane["origin"], plane["normal"])
    if point:
        print(f"  平面{i}: {point}")
```

### 射线与球体的交点

射线与球体的交点计算用于检测光线是否与球体相交，并返回交点信息。函数返回一个包含所有交点的列表，如果没有交点则返回空列表。这个功能在实现球体光照、高光、碰撞检测等方面非常有用。

```python
from mathutils.geometry import intersect_ray_sphere
from mathutils import Vector

# 定义射线
ray_origin = Vector((0.0, 0.0, -5.0))
ray_direction = Vector((0.0, 0.0, 1.0)).normalized()

# 定义球体
sphere_center = Vector((0.0, 0.0, 0.0))
sphere_radius = 2.0

# 计算交点
intersections = intersect_ray_sphere(ray_origin, ray_direction,
                                     sphere_center, sphere_radius)

print(f"射线起点: {ray_origin}")
print(f"射线方向: {ray_direction}")
print(f"球心: {sphere_center}, 半径: {sphere_radius}")
print(f"交点数量: {len(intersections)}")

for i, point in enumerate(intersections):
    print(f"  交点{i}: {point}")

    # 计算从射线起点到交点的距离
    distance = (point - ray_origin).length
    print(f"    距离: {distance:.4f}")

# 视线检测示例
def check_visibility(from_point, to_point, obstacles):
    """检查两点之间是否有障碍物"""
    direction = (to_point - from_point).normalized()
    distance = (to_point - from_point).length

    for obstacle in obstacles:
        hits = intersect_ray_sphere(from_point, direction,
                                    obstacle["center"], obstacle["radius"])
        if hits:
            for hit in hits:
                hit_distance = (hit - from_point).length
                if hit_distance < distance:
                    return False, hit
    return True, None

# 使用示例
obstacles = [
    {"center": Vector((0, 0, 2)), "radius": 1.0},
    {"center": Vector((3, 0, 5)), "radius": 0.5}
]
visible, hit_point = check_visibility(Vector((0, 0, 0)), Vector((5, 0, 10)), obstacles)
print(f"\n可见性检测: {'可见' if visible else f'被阻挡，阻挡点: {hit_point}'}")
```

### 射线与三角形的交点

射线与三角形的交点计算是实现精确几何拾取的基础。三角形由三个顶点定义，函数检测射线是否与三角形相交，并返回交点信息。这个功能在实现网格拾取、光线追踪、碰撞检测等方面非常重要。

```python
from mathutils.geometry import intersect_ray_triangle
from mathutils import Vector

# 定义射线
ray_origin = Vector((0.0, 0.0, -3.0))
ray_direction = Vector((0.0, 0.0, 1.0)).normalized()

# 定义三角形
triangle = [
    Vector((-1.0, -1.0, 0.0)),
    Vector((1.0, -1.0, 0.0)),
    Vector((0.0, 1.0, 0.0))
]

# 计算交点
intersection = intersect_ray_triangle(ray_origin, ray_direction, triangle)

print(f"射线起点: {ray_origin}")
print(f"射线方向: {ray_direction}")
print(f"三角形顶点: {triangle}")

if intersection:
    print(f"交点: {intersection}")

    # 计算重心坐标
    barycentric = calculate_barycentric(intersection, triangle)
    print(f"重心坐标: {barycentric}")
else:
    print("射线与三角形无交点")

# 批量检测射线与多个三角形的交点
def ray_mesh_intersection(ray_origin, ray_direction, mesh):
    """检测射线与网格的交点"""
    closest_hit = None
    min_distance = float('inf')

    for polygon in mesh.polygons:
        triangle = [mesh.vertices[v].co for v in polygon.vertices]
        hit = intersect_ray_triangle(ray_origin, ray_direction, triangle)

        if hit:
            distance = (hit - ray_origin).length
            if distance < min_distance:
                min_distance = distance
                closest_hit = hit

    return closest_hit, min_distance

def calculate_barycentric(point, triangle):
    """计算重心坐标"""
    v0, v1, v2 = [Vector(v) for v in triangle]
    p = Vector(point)

    v0v1 = v1 - v0
    v0v2 = v2 - v0
    v0p = p - v0

    d00 = v0v1.dot(v0v1)
    d01 = v0v1.dot(v0v2)
    d11 = v0v2.dot(v0v2)
    d20 = v0p.dot(v0v1)
    d21 = v0p.dot(v0v2)

    denom = d00 * d11 - d01 * d01
    if abs(denom) < 1e-10:
        return (1.0, 0.0, 0.0)

    v = (d11 * d20 - d01 * d21) / denom
    w = (d00 * d21 - d01 * d20) / denom
    u = 1.0 - v - w

    return (u, v, w)
```

### 直线相交检测

直线与直线、直线与球体的相交检测用于判断几何元素之间的空间关系。这些函数在实现碰撞检测、几何约束、路径规划等方面非常有用。

```python
from mathutils.geometry import (
    intersect_line_line, intersect_line_sphere, intersect_line_plane
)
from mathutils import Vector

# 直线与直线的交点
line1_a = Vector((0.0, 0.0, 0.0))
line1_b = Vector((1.0, 0.0, 0.0))

line2_a = Vector((0.5, -1.0, 0.0))
line2_b = Vector((0.5, 1.0, 0.0))

intersection = intersect_line_line(line1_a, line1_b, line2_a, line2_b)

print("直线与直线的交点:")
print(f"  直线1: {line1_a} -> {line1_b}")
print(f"  直线2: {line2_a} -> {line2_b}")

if intersection:
    print(f"  交点: {intersection}")
else:
    print("  直线平行或不相交")

# 直线与球体的交点
line_a = Vector((-3.0, 0.0, 0.0))
line_b = Vector((3.0, 0.0, 0.0))
sphere_center = Vector((0.0, 0.0, 0.0))
sphere_radius = 1.0

intersections = intersect_line_sphere(line_a, line_b, sphere_center, sphere_radius)

print(f"\n直线与球体的交点:")
print(f"  直线: {line_a} -> {line_b}")
print(f"  球心: {sphere_center}, 半径: {sphere_radius}")
print(f"  交点数量: {len(intersections)}")

for i, point in enumerate(intersections):
    print(f"  交点{i}: {point}")

# 直线与平面的交点
plane_point = Vector((0.0, 0.0, 0.0))
plane_normal = Vector((0.0, 1.0, 0.0))

intersection_plane = intersect_line_plane(line_a, line_b, plane_point, plane_normal)

print(f"\n直线与平面的交点:")
print(f"  交点: {intersection_plane}")
```

## 插值函数

### 贝塞尔曲线插值

贝塞尔曲线插值函数用于在控制点之间生成平滑的曲线。这个函数接收多个控制点和一个插值因子（0到1之间），返回曲线上对应位置的点。三次贝塞尔曲线使用四个控制点，通过调整这些点的位置可以控制曲线的形状。

```python
from mathutils.geometry import interpolate_bezier
from mathutils import Vector

# 三次贝塞尔曲线控制点
knots = [
    Vector((0.0, 0.0, 0.0)),
    Vector((1.0, 3.0, 0.0)),
    Vector((3.0, 3.0, 0.0)),
    Vector((4.0, 0.0, 0.0))
]

# 生成曲线路径
resolution = 20
path_points = []

for i in range(resolution + 1):
    t = i / resolution
    point = interpolate_bezier(knots[0], knots[1], knots[2], knots[3], t)
    path_points.append(point)

print("贝塞尔曲线路径点:")
for i, point in enumerate(path_points):
    print(f"  t={i/resolution:.2f}: {point}")

# 可视化控制点和曲线
print(f"\n控制点:")
for i, knot in enumerate(knots):
    print(f"  P{i}: {knot}")

# 插值点与切线计算
def bezier_tangent(knots, t):
    """计算贝塞尔曲线的切线方向"""
    from mathutils.geometry import interpolate_bezier

    # 使用接近的点来近似切线
    delta = 0.001
    p1 = interpolate_bezier(knots[0], knots[1], knots[2], knots[3], t - delta)
    p2 = interpolate_bezier(knots[0], knots[1], knots[2], knots[3], t + delta)

    return (p2 - p1).normalized()

print("\n曲线切线方向:")
for i in range(0, resolution + 1, 5):
    t = i / resolution
    point = interpolate_bezier(knots[0], knots[1], knots[2], knots[3], t)
    tangent = bezier_tangent(knots, t)
    print(f"  t={t:.2f}: 位置={point}, 切线={tangent}")
```

### NURBS 曲线插值

NURBS 曲线插值函数用于生成更复杂的自由曲线。这种插值方法支持更多的控制点，可以创建更复杂的曲线形状。函数使用三阶样条插值，通过给定的控制点序列生成平滑曲线。

```python
from mathutils.geometry import interpolate_nurbs
from mathutils import Vector

# NURBS 控制点
control_points = [
    Vector((0.0, 0.0, 0.0)),
    Vector((1.0, 2.0, 0.0)),
    Vector((2.0, 1.0, 0.0)),
    Vector((3.0, 3.0, 0.0)),
    Vector((4.0, 0.0, 0.0)),
    Vector((5.0, 2.0, 0.0))
]

# 生成 NURBS 曲线路径
resolution = 50
nurbs_path = []

for i in range(resolution + 1):
    t = i / resolution
    point = interpolate_nurbs(control_points, t)
    nurbs_path.append(point)

print("NURBS 曲线路径点:")
print(f"  共 {len(nurbs_path)} 个点")
print(f"  起点: {nurbs_path[0]}")
print(f"  终点: {nurbs_path[-1]}")

# 线性插值与 NURBS 插值对比
def linear_interpolate(points, t):
    """线性插值"""
    i = t * (len(points) - 1)
    i0 = int(i)
    i1 = min(i0 + 1, len(points) - 1)
    local_t = i - i0
    return points[i0].lerp(points[i1], local_t)

print("\n线性插值 vs NURBS 插值对比:")
for i in [0, int(resolution/4), int(resolution/2), int(resolution*3/4), resolution]:
    t = i / resolution
    linear = linear_interpolate(control_points, t)
    nurbs_point = nurbs_path[i]
    print(f"  t={t:.2f}: 线性={linear}, NURBS={nurbs_point}")
```

## 几何属性计算

### 三角形面积

三角形面积计算函数用于确定三角形的表面积。这个函数在网格分析、碰撞体积计算、资源优化等方面非常有用。函数接收三个顶点作为输入，返回三角形的面积值。

```python
from mathutils.geometry import area_triangle
from mathutils import Vector

# 定义三角形顶点
vertex_a = Vector((0.0, 0.0, 0.0))
vertex_b = Vector((1.0, 0.0, 0.0))
vertex_c = Vector((0.0, 1.0, 0.0))

# 计算面积
area = area_triangle(vertex_a, vertex_b, vertex_c)

print(f"三角形顶点: A={vertex_a}, B={vertex_b}, C={vertex_c}")
print(f"三角形面积: {area:.4f}")

# 验证：直角三角形的面积应该是两直角边的乘积除以2
expected_area = 0.5 * 1.0 * 1.0
print(f"验证结果: {expected_area:.4f}")

# 计算网格的总表面积
def calculate_mesh_area(mesh):
    """计算网格的总表面积"""
    total_area = 0.0

    for polygon in mesh.polygons:
        vertices = [mesh.vertices[v].co for v in polygon.vertices]
        if len(vertices) == 3:
            area = area_triangle(vertices[0], vertices[1], vertices[2])
        elif len(vertices) == 4:
            area1 = area_triangle(vertices[0], vertices[1], vertices[2])
            area2 = area_triangle(vertices[0], vertices[2], vertices[3])
            area = area1 + area2
        else:
            # 对于多边形，使用三角形分割法
            area = 0.0
            for i in range(1, len(vertices) - 1):
                area += area_triangle(vertices[0], vertices[i], vertices[i + 1])

        total_area += area

    return total_area

# 使用示例
if bpy.context.active_object and bpy.context.active_object.type == 'MESH':
    mesh = bpy.context.active_object.data
    total_area = calculate_mesh_area(mesh)
    print(f"\n网格 '{mesh.name}' 的总表面积: {total_area:.4f}")
```

### 面法线计算

面法线计算函数用于确定面的垂直方向。这个函数在光照计算、碰撞检测、法线贴图生成等方面非常重要。函数接收三个顶点作为输入，返回面的单位法向量。

```python
from mathutils.geometry import normal
from mathutils import Vector

# 定义三角形顶点
vertex_a = Vector((0.0, 0.0, 0.0))
vertex_b = Vector((1.0, 0.0, 0.0))
vertex_c = Vector((0.0, 1.0, 0.0))

# 计算法线
face_normal = normal(vertex_a, vertex_b, vertex_c)

print(f"三角形顶点: A={vertex_a}, B={vertex_b}, C={vertex_c}")
print(f"面法线: {face_normal}")
print(f"法线长度: {face_normal.length:.4f}")

# 验证：结果应该是 Z 轴正方向
print(f"期望方向: (0, 0, 1)")

# 计算网格的法线
def calculate_mesh_normals(mesh):
    """计算网格的所有面法线"""
    normals = []

    for polygon in mesh.polygons:
        vertices = [mesh.vertices[v].co for v in polygon.vertices]
        if len(vertices) >= 3:
            poly_normal = normal(vertices[0], vertices[1], vertices[2])
            normals.append({
                'polygon_index': polygon.index,
                'normal': poly_normal,
                'area': polygon.area
            })

    return normals

# 使用示例
if bpy.context.active_object and bpy.context.active_object.type == 'MESH':
    mesh = bpy.context.active_object.data
    normals = calculate_mesh_normals(mesh)

    print(f"\n网格 '{mesh.name}' 的面法线:")
    for n in normals[:5]:  # 只显示前5个
        print(f"  面{n['polygon_index']}: 法线={n['normal']}, 面积={n['area']:.4f}")

    # 检查法线一致性
    avg_normal = sum((n['normal'] * n['area'] for n in normals), Vector((0, 0, 0)))
    avg_normal = avg_normal.normalized()
    print(f"\n面积加权平均法线: {avg_normal}")
```

## 实践示例

### 网格碰撞检测器

这个示例展示了如何使用几何计算函数实现简单的碰撞检测系统。

```python
from mathutils.geometry import (
    distance_point_to_point, intersect_sphere_sphere, intersect_ray_sphere
)
from mathutils import Vector

class CollisionDetector:
    def __init__(self):
        self.colliders = []

    def add_collider(self, center, radius, name="Collider"):
        """添加碰撞体"""
        self.colliders.append({
            "center": Vector(center),
            "radius": radius,
            "name": name
        })

    def check_collision(self, point, radius=0.0):
        """检查点是否与任何碰撞体碰撞"""
        for collider in self.colliders:
            center = collider["center"]
            collider_radius = collider["radius"]

            if radius > 0:
                # 点+半径 vs 球体
                distance = distance_point_to_point(point, center)
                if distance < radius + collider_radius:
                    return True, collider
            else:
                # 点 vs 球体
                if distance_point_to_point(point, center) < collider_radius:
                    return True, collider

        return False, None

    def check_sphere_collision(self, sphere_center, sphere_radius):
        """检查两个球体是否碰撞"""
        for i, collider1 in enumerate(self.colliders):
            for collider2 in self.colliders[i + 1:]:
                collision = intersect_sphere_sphere(
                    collider1["center"], collider1["radius"],
                    collider2["center"], collider2["radius"]
                )
                if collision:
                    return True, (collider1, collider2)
        return False, None

    def ray_cast(self, ray_origin, ray_direction, max_distance=float('inf')):
        """射线投射检测"""
        direction = Vector(ray_direction).normalized()

        for collider in self.colliders:
            hits = intersect_ray_sphere(ray_origin, direction,
                                        collider["center"], collider["radius"])
            for hit in hits:
                distance = (hit - ray_origin).length
                if distance <= max_distance:
                    return hit, collider, distance

        return None, None, None

# 使用示例
detector = CollisionDetector()
detector.add_collider(Vector((0, 0, 0)), 1.0, "球体1")
detector.add_collider(Vector((2, 0, 0)), 0.8, "球体2")
detector.add_collider(Vector((0, 3, 0)), 1.2, "球体3")

# 测试点碰撞
test_point = Vector((0.5, 0.5, 0.5))
collision, hit_collider = detector.check_collision(test_point)
print(f"点 {test_point} 碰撞检测: {'碰撞' if collision else '无碰撞'}")

# 射线投射
ray_origin = Vector((-2, 0, 0))
ray_direction = Vector((1, 0, 0))
hit, collider, distance = detector.ray_cast(ray_origin, ray_direction)
if hit:
    print(f"射线击中: {collider['name']}, 距离: {distance:.4f}")
```

### 几何测量工具

这个示例展示了如何使用几何计算函数创建一个实用的几何测量工具。

```python
from mathutils.geometry import (
    distance_point_to_point, distance_point_to_line, distance_point_to_plane,
    area_triangle, normal
)
from mathutils import Vector

class GeometryMeasurer:
    def __init__(self):
        self.unit = "米"

    def set_unit(self, unit_name):
        """设置测量单位"""
        self.unit = unit_name

    def measure_distance(self, point1, point2):
        """测量两点之间的距离"""
        distance = distance_point_to_point(Vector(point1), Vector(point2))
        return {"type": "点对距离", "value": distance, "unit": self.unit}

    def measure_point_to_line(self, point, line_point, line_direction):
        """测量点到直线的距离"""
        distance, closest = distance_point_to_line(
            Vector(point), Vector(line_point), Vector(line_direction)
        )
        return {
            "type": "点到直线距离",
            "value": distance,
            "closest_point": closest,
            "unit": self.unit
        }

    def measure_point_to_plane(self, point, plane_point, plane_normal):
        """测量点到平面的距离"""
        distance = distance_point_to_plane(
            Vector(point), Vector(plane_point), Vector(plane_normal)
        )
        return {
            "type": "点到平面距离",
            "value": distance,
            "unit": self.unit
        }

    def measure_triangle_area(self, vertex1, vertex2, vertex3):
        """测量三角形面积"""
        area = area_triangle(
            Vector(vertex1), Vector(vertex2), Vector(vertex3)
        )
        return {"type": "三角形面积", "value": area, "unit": self.unit + "²"}

    def measure_face_normal(self, vertex1, vertex2, vertex3):
        """测量面法线"""
        normal_vec = normal(
            Vector(vertex1), Vector(vertex2), Vector(vertex3)
        )
        return {"type": "面法线", "value": normal_vec}

    def generate_report(self, measurements):
        """生成测量报告"""
        report = []
        report.append("=" * 40)
        report.append("几何测量报告")
        report.append("=" * 40)
        report.append(f"测量单位: {self.unit}")
        report.append("-" * 40)

        for i, measurement in enumerate(measurements, 1):
            report.append(f"\n测量 {i}:")
            for key, value in measurement.items():
                if isinstance(value, float):
                    report.append(f"  {key}: {value:.4f}")
                else:
                    report.append(f"  {key}: {value}")

        return "\n".join(report)

# 使用示例
measurer = GeometryMeasurer()
measurer.set_unit("米")

measurements = [
    measurer.measure_distance((0, 0, 0), (3, 4, 0)),
    measurer.measure_point_to_line(
        (5, 3, 0),
        (0, 0, 0),
        (1, 0, 0)
    ),
    measurer.measure_triangle_area(
        (0, 0, 0),
        (1, 0, 0),
        (0, 1, 0)
    ),
    measurer.measure_face_normal(
        (0, 0, 0),
        (1, 0, 0),
        (0, 1, 0)
    )
]

report = measurer.generate_report(measurements)
print(report)
```

## 注意事项

### 数值精度

几何计算函数返回的浮点数值可能存在精度误差。在进行等值比较时，应该使用容差范围而不是精确相等判断。例如，判断两个距离是否相等时，可以使用 `abs(distance1 - distance2) < 1e-6`。

### 向量标准化

许多几何计算函数假设输入向量已经标准化，特别是方向向量。使用未标准化的方向向量可能导致计算结果不正确。如果不确定向量是否标准化，应该先调用 `.normalized()` 方法。

### 退化情况处理

某些几何配置可能导致退化情况，如零长度向量、共线点、重合顶点等。这些情况可能导致除零错误或无意义的结果。在进行几何计算之前，应该检查输入数据的有效性。

---
## mathutils_interpolate

# mathutils.interpolate - Interpolation Utilities Module

interpolate模块提供了各种插值功能，包括曲线插值、面片插值和散乱数据插值。

## 概述

mathutils.interpolate提供了在Blender中进行数据插值的实用工具。这些函数支持不同类型的插值需求，从简单的线性插值到复杂的三维曲面插值。

## 核心功能

### 曲线插值

```python
import bpy
import mathutils
from mathutils.interpolate import bezier_curve_easing
```
def linear_interpolate(start, end, factor):
    """线性插值"""
    return start + (end - start) * factor

def ease_in_quad(t):
    """二次缓动进入"""
    return t * t

def ease_out_quad(t):
    """二次缓动退出"""
    return t * (2 - t)

def ease_in_out_cubic(t):
    """三次缓动进出"""
    return 4 * t * t * t if t < 0.5 else 1 - pow(-2 * t + 2, 3) / 2

def bezier_ease(t, easing=0):
    """贝塞尔曲线缓动"""
    return bezier_curve_easing(t, easing)
```

### 三维插值

```python
def interpolate_vector_list(vectors, factor):
    """插值向量列表"""
    result = []
    for i, v in enumerate(vectors):
        result.append(linear_interpolate(vectors[0], vectors[-1], factor))
    return result

def interpolate_between_points(points, num_steps):
    """在多个点之间插值生成路径"""
    if len(points) < 2:
        return points
    path = []
    for i in range(len(points) - 1):
        for j in range(num_steps):
            t = j / (num_steps - 1)
            path.append(linear_interpolate(points[i], points[i+1], t))
    return path

def interpolate_surface(grid, u, v):
    """双线性表面插值"""
    x00 = grid[0][0]
    x10 = grid[1][0]
    x01 = grid[0][1]
    x11 = grid[1][1]
    top = linear_interpolate(x00, x10, u)
    bottom = linear_interpolate(x01, x11, u)
    return linear_interpolate(top, bottom, v)
```

### 散乱数据插值

```python
def interpolate_from_scatter(coords, values, target_point, kernel='shepard'):
    """散乱数据插值"""
    weights = []
    total_weight = 0
    for coord, value in zip(coords, values):
        dist = (coord - target_point).length
        if dist > 0:
            weight = 1.0 / (dist ** 2)
            weights.append(value * weight)
            total_weight += weight
    if total_weight > 0:
        return sum(weights) / total_weight
    return values[0] if values else None

def idw_interpolation(points, values, target, power=2):
    """反距离加权插值"""
    numerator = 0
    denominator = 0
    for p, v in zip(points, values):
        dist = (p - target).length
        if dist > 0:
            weight = 1.0 / (dist ** power)
            numerator += v * weight
            denominator += weight
    return numerator / denominator if denominator > 0 else 0
```

## 实用函数

### 动画路径插值

```python
def create_smooth_path(points, num_interpolations=10):
    """创建平滑动画路径"""
    if len(points) < 2:
        return points
    path = []
    for i in range(len(points) - 1):
        segment = create_bezier_segment(
            points[i],
            points[i],
            points[i+1],
            points[i+1],
            num_interpolations
        )
        path.extend(segment[1:])
    return path

def create_bezier_segment(p0, p1, p2, p3, num_points):
    """创建三次贝塞尔曲线段"""
    t_values = [i / (num_points - 1) for i in range(num_points)]
    curve = []
    for t in t_values:
        curve.append(bezier(p0, p1, p2, p3, t))
    return curve

def bezier(p0, p1, p2, p3, t):
    """三次贝塞尔曲线公式"""
    mt = 1 - t
    return (mt**3 * p0 + 
            3 * mt**2 * t * p1 + 
            3 * mt * t**2 * p2 + 
            t**3 * p3)
```

### 颜色插值

```python
def interpolate_color(color1, color2, factor):
    """插值颜色"""
    return type(color1)(
        color1[0] + (color2[0] - color1[0]) * factor,
        color1[1] + (color2[1] - color1[1]) * factor,
        color1[2] + (color2[2] - color1[2]) * factor,
        color1[3] + (color2[3] - color1[3]) * factor if len(color1) > 3 else 1.0
    )

def gradient_colors(colors, num_steps):
    """生成渐变颜色序列"""
    gradient = []
    segment_length = num_steps // (len(colors) - 1)
    for i in range(len(colors) - 1):
        for j in range(segment_length):
            factor = j / segment_length
            gradient.append(interpolate_color(colors[i], colors[i+1], factor))
    return gradient
```

### 权重插值

```python
def interpolate_vertex_weights(weights1, weights2, factor):
    """插值顶点权重"""
    return [w1 + (w2 - w1) * factor for w1, w2 in zip(weights1, weights2)]

def blend_weights(weight_dict1, weight_dict2, factor):
    """混合两组权重"""
    all_vertices = set(weight_dict1.keys()) | set(weight_dict2.keys())
    result = {}
    for v in all_vertices:
        w1 = weight_dict1.get(v, 0)
        w2 = weight_dict2.get(v, 0)
        result[v] = w1 + (w2 - w1) * factor
    return result

def normalize_weights(weights):
    """归一化权重"""
    total = sum(weights.values())
    if total > 0:
        return {k: v / total for k, v in weights.items()}
    return weights
```

## 常见问题排查

### 插值不稳定

结果出现NaN或无穷大：
- 检查输入值是否有效
- 避免除以零的距离值
- 使用epsilon值防止除零

### 性能问题

插值计算太慢：
- 对于实时计算，减少插值点数量
- 预计算常用插值曲线
- 使用向量化操作代替循环

### 边界问题

插值超出预期范围：
- 确保factor在0到1之间
- 使用clamp函数限制factor范围
- 检查输入数据的有效性

### 颜色插值异常

颜色值超出0-1范围：
- 插值后使用clamp限制范围
- 确保输入颜色值有效
- 使用正确的颜色空间


---
## mathutils_kdtree

# mathutils.kdtree - KDTree Utilities Module

KDTree模块提供了K维树（K-Dimensional Tree）的创建和最近邻搜索功能，用于高效的空间点查询。

## 概述

KDTree是一种空间分割数据结构，将k维空间分成嵌套的超矩形区域。它特别适合在大量点中进行最近邻搜索、范围查询和聚类分析。

## 核心功能

### KDTree创建

```python
import bpy
import mathutils
from mathutils.kdtree import KDTree

def create_kdtree_from_vertices(mesh):
    """从网格顶点创建KDTree"""
    verts = [v.co for v in mesh.vertices]
    tree = KDTree(len(verts))
    for i, v in enumerate(verts):
        tree.insert(v, i)
    tree.balance()
    return tree

def create_kdtree_from_points(points):
    """从点列表创建KDTree"""
    tree = KDTree(len(points))
    for i, p in enumerate(points):
        tree.insert(p, i)
    tree.balance()
    return tree

def create_kdtree_with_data(points, data_list):
    """创建带数据的KDTree"""
    tree = KDTree(len(points))
    for i, (p, d) in enumerate(zip(points, data_list)):
        tree.insert(p, i, data=d)
    tree.balance()
    return tree
```

### 最近邻搜索

```python
def find_nearest(tree, point):
    """查找最近的点"""
    co, index, dist = tree.find(point)
    return {
        'coordinate': co,
        'index': index,
        'distance': dist
    }

def find_n_nearest(tree, point, n):
    """查找最近的N个点"""
    return [(tree.find_nearest(point, n=i)) for i in range(n)]

def find_nearest_in_range(tree, point, radius):
    """查找半径内的所有点"""
    return [ (co, index, dist) for co, index, dist in tree.find_range(point, radius) ]

def find_all_nearest(tree, point, num):
    """获取最近的多个点"""
    results = []
    for co, index, dist in tree.find_nearest(point, num):
        results.append({
            'coordinate': co,
            'index': index,
            'distance': dist
        })
    return results
```

### 范围查询

```python
def find_points_in_sphere(tree, center, radius):
    """查找球体内的所有点"""
    return list(tree.find_range(center, radius))

def find_points_in_box(tree, center, half_size):
    """查找立方体内的所有点"""
    from mathutils import Vector
    min_point = center - Vector(half_size)
    max_point = center + Vector(half_size)
    return list(tree.find_range_aabb(min_point, max_point))

def count_points_in_sphere(tree, center, radius):
    """统计球体内的点数"""
    return len(list(tree.find_range(center, radius)))
```

### 数据关联

```python
def find_nearest_with_data(tree, point):
    """查找最近的点及其关联数据"""
    co, index, dist, data = tree.find(point)
    return {
        'coordinate': co,
        'index': index,
        'distance': dist,
        'data': data
    }

def create_kdtree_with_vertex_data(mesh):
    """创建带顶点索引的KDTree"""
    verts = [v.co for v in mesh.vertices]
    tree = KDTree(len(verts))
    for i, v in enumerate(verts):
        tree.insert(v, i)
    tree.balance()
    return tree

def find_nearest_vertex(tree, mesh, point):
    """在网格中查找最近的顶点"""
    co, index, dist = tree.find(point)
    if index < len(mesh.vertices):
        return {
            'vertex': mesh.vertices[index],
            'coordinate': co,
            'distance': dist
        }
    return None
```

## 实用函数

### 批量操作

```python
def build_scene_kdtree():
    """构建场景中所有对象顶点的KDTree"""
    all_verts = []
    vert_map = []
    for obj in bpy.context.scene.objects:
        if obj.type == 'MESH':
            for v in obj.data.vertices:
                world_co = obj.matrix_world @ v.co
                all_verts.append(world_co)
                vert_map.append((obj.name, v.index))
    tree = KDTree(len(all_verts))
    for i, v in enumerate(all_verts):
        tree.insert(v, i)
    tree.balance()
    return tree, vert_map

def find_nearest_vertex_in_scene(point):
    """在场景中查找最近的顶点"""
    tree, vert_map = build_scene_kdtree()
    co, index, dist = tree.find(point)
    if index < len(vert_map):
        obj_name, vert_index = vert_map[index]
        return {
            'object': bpy.data.objects[obj_name],
            'vertex_index': vert_index,
            'coordinate': co,
            'distance': dist
        }
    return None
```

### 高级功能

```python
def find_k_nearest_neighbors(tree, point, k):
    """查找K个最近邻"""
    results = []
    for co, index, dist in tree.find_nearest(point, k):
        results.append({
            'coordinate': co,
            'index': index,
            'distance': dist
        })
    return results

def cluster_points_kdtree(points, radius):
    """使用KDTree进行点云聚类"""
    tree = create_kdtree_from_points(points)
    visited = set()
    clusters = []
    for i, point in enumerate(points):
        if i not in visited:
            cluster = [point]
            visited.add(i)
            nearby = tree.find_range(point, radius)
            for co, idx, dist in nearby:
                if idx not in visited:
                    cluster.append(points[idx])
                    visited.add(idx)
            clusters.append(cluster)
    return clusters

def find_duplicates(points, threshold):
    """查找重复点"""
    tree = create_kdtree_from_points(points)
    duplicates = []
    for i, point in enumerate(points):
        for co, idx, dist in tree.find_range(point, threshold):
            if idx > i:
                duplicates.append((i, idx, dist))
    return duplicates
```

## 常见问题排查

### 树未平衡

搜索结果不准确：
- 创建KDTree后必须调用balance()方法
- 如果点分布不均匀，可能需要多次平衡

### 搜索性能问题

搜索速度慢：
- 确保树已平衡
- 避免在循环中重复创建树
- 使用find_nearest代替多次find调用

### 索引越界

index超出范围：
- 确保插入时的索引有效
- 插入后不要修改原始数据列表
- 使用find_range时检查返回的索引

### 内存使用

内存占用过高：
- 对于非常大的点云，考虑分块创建多个KDTree
- 及时释放不再使用的树对象
- 使用生成器表达式处理结果


---
## mathutils_noise

# mathutils.noise 模块

## 模块概述

mathutils.noise 模块提供了一系列程序化噪波生成函数，这些函数是程序化内容生成的核心工具。噪波函数能够生成看似随机的但实际上具有连续性的数值分布，这种特性使得它们非常适合用于模拟自然界中的纹理、地形、云层等复杂现象。与纯随机数不同，噪波函数生成的数值在空间中具有相关性，相邻的点会产生相似的数值。

该模块实现了多种经典的噪波算法，每种算法都有其独特的特性适用场景。Blender 噪波提供基础的随机性分布，Cellular 噪波产生细胞状的图案，Voronoi 噪波生成基于距离场的特征，Musgrave 噪波系列则专门用于地形和表面纹理的程序化生成。这些函数的输出值通常在 -1 到 1 或 0 到 1 的范围内，可以通过参数调整频率、幅度、偏移等属性。

在 Blender 中，噪波函数被广泛应用于材质节点、几何节点、雕刻工具等各个模块。通过 Python API 直接使用这些函数可以实现更灵活的自动化处理，如批量生成纹理、程序化地形建模、动画噪波效果等。理解各种噪波算法的特性对于选择合适的工具解决特定问题至关重要。

## 基础噪波类型

### Blender 噪波

Blender 噪波是模块中最基础的随机噪波函数，它生成连续的随机值分布。这种噪波类似于 Perlin 噪波，但使用不同的算法实现。Blender 噪波适合用于创建基础的随机纹理、颜色变化、材质表面的微小变化等效果。通过调整输入坐标可以控制噪波的空间分布。

```python
from mathutils.noise import noise
from mathutils import Vector

# 一维 Blender 噪波
print("一维 Blender 噪波:")
for i in range(10):
    x = i * 0.1
    value = noise(x)
    print(f"  x={x:.1f}: {value:.4f}")

# 二维 Blender 噪波
print("\n二维 Blender 噪波:")
for i in range(5):
    for j in range(5):
        x, y = i * 0.5, j * 0.5
        value = noise(Vector((x, y)))
        print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")

# 三维 Blender 噪波（用于动画）
print("\n三维 Blender 噪波（时间维度）:")
for t in range(5):
    value = noise(Vector((0.5, 0.5, t * 0.2)))
    print(f"  t={t}: {value:.4f}")

# 生成一维噪波序列
def generate_noise_sequence(start, end, steps, scale=1.0):
    """生成一维噪波序列"""
    values = []
    for i in range(steps):
        x = start + (end - start) * i / (steps - 1)
        values.append(scale * noise(x))
    return values

sequence = generate_noise_sequence(0, 10, 100, scale=0.5)
print(f"\n噪波序列统计:")
print(f"  长度: {len(sequence)}")
print(f"  最小值: {min(sequence):.4f}")
print(f"  最大值: {max(sequence):.4f}")
print(f"  平均值: {sum(sequence) / len(sequence):.4f}")
```

### 基础坐标变换

噪波函数的行为很大程度上取决于输入坐标的取值和变换。通过对坐标进行缩放、平移、旋转等变换，可以控制噪波的频率、相位和方向。这些变换对于获得期望的噪波效果至关重要。

```python
from mathutils.noise import noise
from mathutils import Vector, Matrix

# 频率缩放控制噪波的密度
def scaled_noise(x, y, scale):
    """缩放坐标后的噪波"""
    return noise(Vector((x * scale, y * scale)))

# 多尺度叠加（分形布朗运动基础）
def fbm_base(x, y, octaves=4, persistence=0.5, lacunarity=2.0):
    """基础的分形布朗运动"""
    total = 0.0
    frequency = 1.0
    amplitude = 1.0
    max_value = 0.0

    for _ in range(octaves):
        total += noise(Vector((x * frequency, y * frequency))) * amplitude
        max_value += amplitude
        amplitude *= persistence
        frequency *= lacunarity

    return total / max_value

# 测试不同频率的效果
print("不同频率的噪波对比:")
for scale in [0.5, 1.0, 2.0, 4.0]:
    value = scaled_noise(1.0, 1.0, scale)
    print(f"  频率 {scale}: {value:.4f}")

# 测试分形叠加
print("\n分形布朗运动测试:")
for octaves in [1, 2, 4, 8]:
    value = fbm_base(1.0, 1.0, octaves=octaves)
    print(f"  {octaves} 层叠加: {value:.4f}")
```

### 坐标偏移应用

通过在噪波调用前对坐标进行预处理，可以实现各种效果控制。添加时间偏移可以创建动画噪波，添加随机偏移可以生成多个不相关的噪波层，添加空间偏移可以移动噪波图案。

```python
from mathutils.noise import noise
from mathutils import Vector

def offset_noise(x, y, offset_x, offset_y):
    """带偏移的噪波"""
    return noise(Vector((x + offset_x, y + offset_y)))

def animated_noise(x, y, time, time_speed=1.0):
    """带时间动画的噪波"""
    return noise(Vector((x, y, time * time_speed)))

def layered_noise(x, y, layers):
    """多层噪波叠加"""
    total = 0.0
    for i, (offset, scale, weight) in enumerate(layers):
        layer_value = offset_noise(x, y, offset[0], offset[1])
        layer_value = offset_noise(x * scale, y * scale, 0, 0)
        total += layer_value * weight
    return total

# 时间动画示例
print("噪波动画帧:")
for frame in range(10):
    value = animated_noise(0.5, 0.5, frame, time_speed=0.1)
    print(f"  帧 {frame}: {value:.4f}")

# 多层噪波示例
layers = [
    ((0, 0), 1.0, 1.0),      # 基础层
    ((5.2, 1.3), 2.0, 0.5),  # 高频细节层
    ((3.4, 2.1), 4.0, 0.25)  # 更高频细节层
]
value = layered_noise(1.0, 1.0, layers)
print(f"\n多层噪波叠加结果: {value:.4f}")
```

## Cellular 噪波

### Cellular 噪波基础

Cellular 噪波生成基于细胞分裂模式的特征图案，它在每个输入点周围查找最近的细胞中心点，并计算到该中心的距离。这种噪波产生细胞状、斑点状的图案，非常适合创建皮肤、鳞片、细胞组织等自然纹理。

```python
from mathutils.noise import cellular
from mathutils import Vector

# 二维 Cellular 噪波
print("二维 Cellular 噪波:")
for i in range(5):
    for j in range(5):
        x, y = i * 0.5, j * 0.5
        value = cellular(Vector((x, y)))
        print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")

# 三维 Cellular 噪波
print("\n三维 Cellular 噪波:")
for i in range(3):
    for j in range(3):
        x, y, z = i * 0.5, j * 0.5, 0.0
        value = cellular(Vector((x, y, z)))
        print(f"  ({x:.1f}, {y:.1f}, {z:.1f}): {value:.4f}")

# 生成 Cellular 纹理图
def generate_cellular_map(width, height, scale=1.0):
    """生成 Cellular 纹理图"""
    texture = []
    for y in range(height):
        row = []
        for x in range(width):
            nx = x / width * scale
            ny = y / height * scale
            value = cellular(Vector((nx, ny)))
            row.append(value)
        texture.append(row)
    return texture

texture = generate_cellular_map(32, 32, scale=2.0)
print(f"\n纹理图统计:")
print(f"  尺寸: {len(texture)} x {len(texture[0])}")
print(f"  值范围: {min(min(row) for row in texture):.4f} - {max(max(row) for row in texture):.4f}")
```

### Cellular 噪波变体

Cellular 噪波有多种变体，通过传入不同的维度参数可以生成不同空间维度的细胞图案。一维 Cellular 噪波产生间隔性的条纹，二维产生斑点图案，三维产生更加复杂的三维细胞结构。

```python
from mathutils.noise import cellular
from mathutils import Vector

# 一维 Cellular 噪波（条纹状）
print("一维 Cellular 噪波:")
for i in range(10):
    x = i * 0.2
    value = cellular(x)
    print(f"  x={x:.1f}: {value:.4f}")

# 二维 Cellular 噪波（斑点状）
print("\n二维 Cellular 噪波:")
for i in range(5):
    for j in range(5):
        value = cellular(Vector((i * 0.3, j * 0.3)))
        bar = "█" * int(abs(value) * 20)
        print(f"  ({i}, {j}): {value:.3f} {bar}")

# 三维 Cellular 噪波
print("\n三维 Cellular 噪波:")
for z in range(3):
    print(f"  Z={z}:")
    for y in range(3):
        values = []
        for x in range(3):
            value = cellular(Vector((x * 0.3, y * 0.3, z * 0.3)))
            values.append(f"{value:.2f}")
        print(f"    Y={y}: {', '.join(values)}")
```

## Voronoi 噪波

### Voronoi 基础

Voronoi 噪波基于 Voronoi 图算法，它计算输入点到一组随机特征点的最近距离。这种噪波产生独特的细胞状边界图案，非常适合创建鳞片、皮肤、细胞组织、裂纹等效果。与 Cellular 噪波类似但具有不同的特征点分布模式。

```python
from mathutils.noise import voronoi
from mathutils import Vector

# 二维 Voronoi 噪波
print("二维 Voronoi 噪波:")
for i in range(5):
    for j in range(5):
        x, y = i * 0.5, j * 0.5
        value = voronoi(Vector((x, y)))
        print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")

# 生成 Voronoi 纹理
def generate_voronoi_texture(width, height, scale=1.0):
    """生成 Voronoi 纹理"""
    texture = []
    for y in range(height):
        row = []
        for x in range(width):
            nx = x / width * scale
            ny = y / height * scale
            value = voronoi(Vector((nx, ny)))
            row.append(value)
        texture.append(row)
    return texture

texture = generate_voronoi_texture(20, 20, scale=2.0)
print(f"\nVoronoi 纹理统计:")
print(f"  最小值: {min(min(row) for row in texture):.4f}")
print(f"  最大值: {max(max(row) for row in texture):.4f}")

# 可视化 Voronoi 图案
def visualize_voronoi(scale=1.0, resolution=20):
    """可视化 Voronoi 图案"""
    print(f"\nVoronoi 图案可视化 (scale={scale}):")
    for y in range(resolution):
        line = ""
        for x in range(resolution):
            value = voronoi(Vector((x / resolution * scale, y / resolution * scale)))
            char = "██" if value > 0.8 else "▒▒" if value > 0.4 else "  "
            line += char
        print(f"  {line}")

visualize_voronoi(scale=1.0)
visualize_voronoi(scale=2.0)
```

### Voronoi 距离场

Voronoi 噪波的输出是距离值，表示到最近特征点的距离。这个距离场可以用于创建各种效果，如距离边缘的距离、特征点密度等。通过分析距离场可以提取 Voronoi 单元的边界和特征点位置。

```python
from mathutils.noise import voronoi
from mathutils import Vector
import math

def analyze_voronoi_cell(center, radius, samples=100):
    """分析 Voronoi 单元特征"""
    center_distance = voronoi(Vector(center))

    # 采样周围区域
    min_dist = float('inf')
    max_dist = 0
    avg_dist = 0

    for i in range(samples + 1):
        angle = 2 * math.pi * i / samples
        dx = math.cos(angle) * radius
        dy = math.sin(angle) * radius
        point = (center[0] + dx, center[1] + dy)
        dist = voronoi(Vector(point))
        min_dist = min(min_dist, dist)
        max_dist = max(max_dist, dist)
        avg_dist += dist

    avg_dist /= samples

    return {
        "center_distance": center_distance,
        "min_distance": min_dist,
        "max_distance": max_dist,
        "avg_distance": avg_dist,
        "range": max_dist - min_dist
    }

# 分析多个位置
print("Voronoi 距离场分析:")
for x in [0.0, 0.5, 1.0]:
    for y in [0.0, 0.5, 1.0]:
        analysis = analyze_voronoi_cell((x, y), 0.1, samples=50)
        print(f"\n  位置 ({x}, {y}):")
        print(f"    中心距离: {analysis['center_distance']:.4f}")
        print(f"    距离范围: {analysis['min_distance']:.4f} - {analysis['max_distance']:.4f}")
```

## Musgrave 噪波类型

### Musgrave 噪波基础

Musgrave 噪波是一系列程序化地形生成算法的统称，包括 Musgrave Fractal、Musgrave Ridged Multifractal、Musgrave Hybrid Multifractal、Musgrave Hetero Terrain 等变体。这些算法专门设计用于生成自然地貌般的地形纹理。

```python
from mathutils.noise import musgrave_fractal, musgrave_ridgedmf, musgrave_hetero, musgrave_hybridmf, musgrave_multifractal

# Musgrave Fractal - 基础分形噪波
print("Musgrave Fractal 噪波:")
for i in range(5):
    x, y = i * 0.2, i * 0.2
    value = musgrave_fractal(x, y, noise_basis='PERLIN_ORIGINAL')
    print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")

# Musgrave Ridged Multifractal - 岭状多重分形
print("\nMusgrave Ridged Multifractal 噪波:")
for i in range(5):
    x, y = i * 0.2, i * 0.2
    value = musgrave_ridgedmf(x, y, noise_basis='PERLIN_ORIGINAL')
    print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")

# Musgrave Hetero Terrain - 异质地貌
print("\nMusgrave Hetero Terrain 噪波:")
for i in range(5):
    x, y = i * 0.2, i * 0.2
    value = musgrave_hetero(x, y, noise_basis='PERLIN_ORIGINAL')
    print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")

# Musgrave Hybrid Multifractal - 混合多重分形
print("\nMusgrave Hybrid Multifractal 噪波:")
for i in range(5):
    x, y = i * 0.2, i * 0.2
    value = musgrave_hybridmf(x, y, noise_basis='PERLIN_ORIGINAL')
    print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")

# Musgrave Multifractal - 多重分形
print("\nMusgrave Multifractal 噪波:")
for i in range(5):
    x, y = i * 0.2, i * 0.2
    value = musgrave_multifractal(x, y, noise_basis='PERLIN_ORIGINAL')
    print(f"  ({x:.1f}, {y:.1f}): {value:.4f}")
```

### Musgrave 参数对比

每种 Musgrave 噪波算法都有特定的参数可以调整，包括基噪波类型、H 值（分形维数）、拉卡尼蒂（频率倍增）、倍频程数等。理解这些参数的作用对于获得期望的地形效果至关重要。

```python
from mathutils.noise import musgrave_fractal, musgrave_ridgedmf

def test_musgrave_params():
    """测试 Musgrave 噪波参数"""
    x, y = 1.0, 1.0

    # 测试不同的基噪波
    noise_bases = ['PERLIN_ORIGINAL', 'PERLIN_NEW', 'SIMPLEX', 'CELLULAR', 'VORONOI', 'VORONOI_F1']

    print("不同基噪波类型:")
    for basis in noise_bases:
        try:
            value = musgrave_fractal(x, y, noise_basis=basis)
            print(f"  {basis}: {value:.4f}")
        except Exception as e:
            print(f"  {basis}: 错误 - {e}")

    # 测试不同 H 值（分形维数）
    print("\n不同 H 值 (Fractal):")
    for h in [0.1, 0.5, 1.0, 1.5, 2.0]:
        value = musgrave_fractal(x, y, h=h)
        print(f"  H={h}: {value:.4f}")

    # 测试不同倍频程
    print("\n不同倍频郎数:")
    for octaves in [1, 2, 4, 8, 16]:
        value = musgrave_fractal(x, y, octaves=octaves)
        print(f"  octaves={octaves}: {value:.4f}")

    # 测试拉卡尼蒂
    print("\n不同拉卡尼蒂:")
    for lacunarity in [1.5, 2.0, 2.5, 3.0]:
        value = musgrave_fractal(x, y, lacunarity=lacunarity)
        print(f"  lacunarity={lacunarity}: {value:.4f}")

test_musgrave_params()
```

### 地形生成示例

使用 Musgrave 噪波生成程序化地形高度图是常见的应用场景。通过组合多种噪波类型和参数调整，可以创建各种地貌特征。

```python
from mathutils.noise import musgrave_fractal, musgrave_ridgedmf, noise
from mathutils import Vector

def generate_heightmap(width, height, scale=1.0, height_scale=1.0):
    """生成地形高度图"""
    heightmap = []

    for y in range(height):
        row = []
        for x in range(width):
            nx = x / width * scale
            ny = y / height * scale

            # 基础地形（低频）
            base = musgrave_fractal(nx, ny, noise_basis='PERLIN_ORIGINAL',
                                   h=1.0, lacunarity=2.0, octaves=4)

            # 细节（高频）
            detail = musgrave_fractal(nx, ny, noise_basis='PERLIN_ORIGINAL',
                                     h=0.5, lacunarity=2.0, octaves=8)

            # 组合
            height_value = base * 0.6 + detail * 0.4
            row.append(height_value * height_scale)
        heightmap.append(row)

    return heightmap

def generate_terrain_texture(width, height, scale=2.0):
    """生成复杂的地形纹理"""
    texture = []

    for y in range(height):
        row = []
        for x in range(width):
            nx = x / width * scale
            ny = y / height * scale

            # 主地形特征
            main_terrain = musgrave_fractal(nx, ny, noise_basis='PERLIN_ORIGINAL',
                                           h=0.8, lacunarity=2.0, octaves=6)

            # 岭状特征（山脉）
            ridges = musgrave_ridgedmf(nx, ny, noise_basis='PERLIN_ORIGINAL',
                                      h=1.5, lacunarity=2.0, octaves=6)

            # 异质地貌特征
            hetero = musgrave_hetero(nx, ny, noise_basis='PERLIN_ORIGINAL',
                                    h=0.5, lacunarity=2.5, octaves=4)

            # 组合
            value = main_terrain * 0.4 + ridges * 0.4 + hetero * 0.2
            row.append(value)
        texture.append(row)

    return texture

# 生成高度图
heightmap = generate_heightmap(64, 64, scale=2.0, height_scale=10.0)
print("地形高度图统计:")
print(f"  尺寸: {len(heightmap)} x {len(heightmap[0])}")
print(f"  高度范围: {min(min(row) for row in heightmap):.2f} - {max(max(row) for row in heightmap):.2f}")

# 生成地形纹理
terrain = generate_terrain_texture(32, 32, scale=2.0)
print(f"\n地形纹理统计:")
print(f"  尺寸: {len(terrain)} x {len(terrain[0])}")
print(f"  值范围: {min(min(row) for row in terrain):.4f} - {max(max(row) for row in terrain):.4f}")
```

## 实践示例

### 程序化纹理生成器

这个示例展示了如何组合各种噪波函数创建复杂的程序化纹理。

```python
from mathutils.noise import (
    noise, cellular, voronoi,
    musgrave_fractal, musgrave_ridgedmf, musgrave_hetero
)
from mathutils import Vector
import math

class ProceduralTextureGenerator:
    def __init__(self):
        self.scale = 1.0

    def set_scale(self, scale):
        """设置纹理缩放"""
        self.scale = scale

    def generate_wood_texture(self, width, height):
        """生成木纹纹理"""
        texture = []
        for y in range(height):
            row = []
            for x in range(width):
                nx = x / width * self.scale
                ny = y / height * self.scale

                # 基础噪波
                base = noise(Vector((nx, ny)))

                # 添加年轮效果
                rings = abs(cellular(Vector((nx, ny))))

                # 组合
                value = base * 0.3 + rings * 0.7
                row.append(value)
            texture.append(row)
        return texture

    def generate_marble_texture(self, width, height):
        """生成大理石纹理"""
        texture = []
        for y in range(height):
            row = []
            for x in range(width):
                nx = x / width * self.scale
                ny = y / height * self.scale

                # 分形噪波（用于裂纹）
                cracks = musgrave_fractal(nx, ny, noise_basis='PERLIN_ORIGINAL',
                                         h=1.5, lacunarity=2.0, octaves=4)

                # 扭曲效果
                warped_x = nx + noise(Vector((nx * 2, ny * 2))) * 0.1
                warped_y = ny + noise(Vector((nx * 2 + 1, ny * 2 + 1))) * 0.1
                base = noise(Vector((warped_x, warped_y)))

                # 组合（使用正弦函数模拟大理石纹路）
                value = math.sin((cracks + base) * 20.0)
                value = (value + 1) / 2  # 归一化到 0-1
                row.append(value)
            texture.append(row)
        return texture

    def generate_cloud_texture(self, width, height):
        """生成云层纹理"""
        texture = []
        for y in range(height):
            row = []
            for x in range(width):
                nx = x / width * self.scale
                ny = y / height * self.scale

                # 多尺度叠加
                cloud = 0.0
                for i in range(5):
                    scale = self.scale * (2 ** i)
                    weight = 0.5 ** i
                    cloud += noise(Vector((nx * scale, ny * scale, i * 0.5))) * weight

                # 阈值处理（使云更清晰）
                cloud = max(0, cloud - 0.2) / 0.8
                row.append(cloud)
            texture.append(row)
        return texture

    def generate_camo_texture(self, width, height):
        """生成迷彩纹理"""
        texture = []
        for y in range(height):
            row = []
            for x in range(width):
                nx = x / width * self.scale
                ny = y / height * self.scale

                # 使用 Voronoi 创建斑块
                voron = voronoi(Vector((nx, ny)))

                # 添加噪波扰动
                noise_val = noise(Vector((nx * 3, ny * 3)))

                # 组合
                value = voron + noise_val * 0.3
                row.append(value)
            texture.append(row)
        return texture

# 使用示例
generator = ProceduralTextureGenerator()
generator.set_scale(2.0)

print("程序化纹理生成:")
wood = generator.generate_wood_texture(32, 32)
marble = generator.generate_marble_texture(32, 32)
cloud = generator.generate_cloud_texture(32, 32)
camo = generator.generate_camo_texture(32, 32)

for name, texture in [("木纹", wood), ("大理石", marble), ("云层", cloud), ("迷彩", camo)]:
    flat = [v for row in texture for v in row]
    print(f"  {name}: 范围 {min(flat):.4f} - {max(flat):.4f}")
```

### 噪波动画生成器

这个示例展示了如何创建噪波动画效果。

```python
from mathutils.noise import noise, voronoi, cellular
from mathutils import Vector

class NoiseAnimator:
    def __init__(self):
        self.speed = 1.0
        self.scale = 1.0

    def set_speed(self, speed):
        """设置动画速度"""
        self.speed = speed

    def set_scale(self, scale):
        """设置空间缩放"""
        self.scale = scale

    def animate_2d(self, frames, width, height, noise_type='blender'):
        """生成 2D 噪波动画帧"""
        frames_data = []

        for frame in range(frames):
            frame_data = []
            for y in range(height):
                row = []
                for x in range(width):
                    nx = x / width * self.scale
                    ny = y / height * self.scale
                    nz = frame / frames * self.speed

                    if noise_type == 'blender':
                        value = noise(Vector((nx, ny, nz)))
                    elif noise_type == 'voronoi':
                        value = voronoi(Vector((nx, ny, nz)))
                    elif noise_type == 'cellular':
                        value = cellular(Vector((nx, ny, nz)))
                    else:
                        value = noise(Vector((nx, ny)))

                    row.append(value)
                frame_data.append(row)
            frames_data.append(frame_data)

        return frames_data

    def get_frame_stats(self, frames_data, frame_idx):
        """获取指定帧的统计信息"""
        frame = frames_data[frame_idx]
        flat = [v for row in frame for v in row]
        return {
            'frame': frame_idx,
            'min': min(flat),
            'max': max(flat),
            'avg': sum(flat) / len(flat)
        }

# 使用示例
animator = NoiseAnimator()
animator.set_speed(2.0)
animator.set_scale(3.0)

# 生成动画
frames = animator.animate_2d(frames=10, width=20, height=20, noise_type='blender')

print("噪波动画统计:")
for i in [0, 4, 9]:
    stats = animator.get_frame_stats(frames, i)
    print(f"  帧 {stats['frame']}: 最小={stats['min']:.4f}, 最大={stats['max']:.4f}, 平均={stats['avg']:.4f}")

# 对比不同噪波类型
print("\n不同噪波类型动画帧对比:")
for noise_type in ['blender', 'voronoi', 'cellular']:
    frames = animator.animate_2d(frames=5, width=10, height=10, noise_type=noise_type)
    first_frame = [v for row in frames[0] for v in row]
    print(f"  {noise_type}: 范围 {min(first_frame):.4f} - {max(first_frame):.4f}")
```

## 注意事项

### 坐标范围

噪波函数的输出值范围因具体函数而异，但通常在 -1 到 1 或 0 到 1 之间。某些 Musgrave 函数的输出范围可能更大，在使用时应该注意进行适当的缩放和偏移以获得期望的效果。

### 性能考虑

复杂的噪波计算，特别是高倍频郎数的分形叠加，会消耗较多的计算资源。在实时应用或处理大量数据时，应该考虑使用适当的分辨率和缓存策略。对于静态纹理，可以预先计算并存储结果。

### 种子变化

默认情况下，噪波函数使用固定的内部种子，每次运行产生相同的结果。如果需要每次运行产生不同的结果，可以通过偏移坐标或使用外部种子生成器来实现随机化。

