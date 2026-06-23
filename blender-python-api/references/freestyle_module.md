# freestyle_module

---
## freestyle

# freestyle 线条渲染模块

freestyle 模块提供了 Freestyle 非真实感渲染引擎的数据类型和工具，用于创建风格化的线条渲染效果。

```python
import freestyle
```

## 模块概述

Freestyle 模块包含以下子模块：

- `freestyle.types`：视图映射组件的数据类型（0D 和 1D 元素）
- `freestyle.predicates`：线条选择谓词
- `freestyle.functions`：线条处理函数
- `freestyle.chainingiterators`：线条链化迭代器
- `freestyle.shaders`：线条着色器
- `freestyle.utils`：样式模块编写辅助函数

## 核心概念

### 线条渲染工作流

```python
import freestyle

# 创建样式模块
def create_style_module():
    from freestyle.predicates import (
        NOT, GT, INTERSECTS, PROJECTED_X, PROJECTED_Y,
        TrueBP1D, BackTesselationBP1D, Face2DAreaBP1D,
        Length2DBP1D, MaterialBP2D, NormalBP1D, Quantity1DPredicate,
        TrueBP1D, UV2DCoordBP1D
    )
    from freestyle.functions import (
        Curvature2DF0D, Curvature2DGradientF0D, Normal2DF0D,
        ObjectAndFaceIdF0D, SquareRootGradientF0D
    )
    from freestyle.chainingiterators import (
        ChainSilhouetteIterator, ChainBoundaryIterator,
        ChainContourIterator, ChainCreaseAngleIterator
    )
    from freestyle.shaders import (
        ConstantColorShader, ConstantThicknessShader,
        PolygonalizationShader, SamplingShader,
        StrokeShader, ThicknessShader, ColorShader
    )
```

### 谓词（Predicates）

谓词用于选择要渲染的线条，基于几何属性和材质属性进行过滤。

```python
from freestyle.predicates import (
    GT, LT, EQ, NOT, AND, OR,
    INTERSECTS, ProjectedX, ProjectedY, ProjectedZ,
    NormalBP1D, Curvature2DBP1D, MaterialBP2D,
    Length2DBP1D, BackTesselationBP1D, TrueBP1D
)

# 选择长度大于 100 的线条
length_predicate = GT(Length2DBP1D(), 100)

# 选择法线向上的面
normal_predicate = GT(NormalBP1D(), 0.0)

# 组合谓词
combined = AND(length_predicate, normal_predicate)
```

### 函数（Functions）

函数用于计算线条的各种属性值，供着色器使用。

```python
from freestyle.functions import (
    Curvature2DF0D, Curvature2DGradientF0D,
    Normal2DF0D, ObjectAndFaceIdF0D,
    SquareRootGradientF0D, DistanceToCameraF0D
)

# 获取曲率值
curvature_func = Curvature2DF0D()

# 获取法线
normal_func = Normal2DF0D()
```

### 链化迭代器（Chaining Iterators）

链化迭代器决定如何将相邻的边连接成连续的线条。

```python
from freestyle.chainingiterators import (
    ChainSilhouetteIterator, ChainBoundaryIterator,
    ChainContourIterator, ChainCreaseAngleIterator
)

# 创建轮廓链化迭代器
silhouette = ChainSilhouetteIterator()

# 创建边界链化迭代器
boundary = ChainBoundaryIterator()

# 创建折痕角度链化迭代器
crease = ChainCreaseAngleIterator(angle_threshold=1.0)
```

### 着色器（Shaders）

着色器定义线条的外观，包括颜色、厚度、样式等。

```python
from freestyle.shaders import (
    ConstantColorShader, ConstantThicknessShader,
    PolygonalizationShader, SamplingShader,
    StrokeShader, ThicknessShader, ColorShader,
    SmoothingShader, GuidingLinesShader
)

# 创建常量颜色着色器
color_shader = ConstantColorShader((0.0, 0.0, 0.0, 1.0))

# 创建常量厚度着色器
thickness_shader = ConstantThicknessShader(2.0)
```

## 样式模块示例

### 简单轮廓线

```python
from freestyle.predicates import (
    NOT, GT, INTERSECTS, TrueBP1D, BackTesselationBP1D
)
from freestyle.chainingiterators import ChainSilhouetteIterator
from freestyle.shaders import (
    ConstantColorShader, ConstantThicknessShader,
    StrokeShader
)

def create_silhouette_style():
    # 选择背对观察者的边
    predicate = NOT(BackTesselationBP1D())
    
    # 使用轮廓链化
    chaining = ChainSilhouetteIterator()
    
    # 定义着色器
    shaders = [
        ConstantColorShader((0.0, 0.0, 0.0, 1.0)),
        ConstantThicknessShader(1.5),
        StrokeShader()
    ]
    
    return predicate, chaining, shaders
```

### 艺术风格线条

```python
from freestyle.predicates import (
    GT, NormalBP1D, Length2DBP1D, MaterialBP2D
)
from freestyle.chainingiterators import ChainContourIterator
from freestyle.shaders import (
    ConstantColorShader, ThicknessShader,
    PolygonalizationShader, SmoothingShader
)

def create_artistic_style():
    # 根据法线和长度选择线条
    predicate = AND(
        GT(NormalBP1D(), 0.2),
        GT(Length2DBP1D(), 50.0)
    )
    
    # 使用轮廓链化
    chaining = ChainContourIterator()
    
    # 定义着色器序列
    shaders = [
        ConstantColorShader((0.1, 0.1, 0.1, 1.0)),
        ThicknessShader(3.0, 1.0),
        PolygonalizationShader(10.0),
        SmoothingShader(3)
    ]
    
    return predicate, chaining, shaders
```

### 手绘风格线条

```python
from freestyle.predicates import GT, Length2DBP1D
from freestyle.chainingiterators import ChainCreaseAngleIterator
from freestyle.shaders import (
    ConstantColorShader, ThicknessShader,
    SamplingShader, GuidingLinesShader
)

def create_sketch_style():
    predicate = GT(Length2DBP1D(), 30.0)
    
    chaining = ChainCreaseAngleIterator(angle_threshold=0.5)
    
    shaders = [
        ConstantColorShader((0.0, 0.0, 0.0, 1.0)),
        ThicknessShader(4.0, 0.5),
        SamplingShader(5.0),
        GuidingLinesShader(5.0)
    ]
    
    return predicate, chaining, shaders
```

## freestyle.utils 工具函数

```python
import freestyle.utils as utils

# 加载样式模块
style_module = utils.LoadStyleModule("//styles/my_style.py")

# 获取当前场景的视图映射
view_map = utils.GetViewMap()

# 获取 0D 元素
svertices = view_map.svertices

# 获取 1D 元素
edges = view_map.edges

# 获取线条
strokes = view_map.strokes
```

### 常用工具函数

```python
import freestyle.utils as utils

# 获取当前渲染的图像尺寸
width, height = utils.GetRenderSize()

# 获取像素比例
pixel_ratio = utils.GetPixelSize()

# 获取线条颜色
color = utils.GetStrokeColor()

# 设置线条颜色
utils.SetStrokeColor((1.0, 0.0, 0.0, 1.0))
```

## 高级应用

### 多层线条效果

```python
from freestyle.predicates import GT, LT, AND, NormalBP1D, Length2DBP1D
from freestyle.chainingiterators import ChainSilhouetteIterator, ChainContourIterator
from freestyle.shaders import ConstantColorShader, ConstantThicknessShader

def create_layered_style():
    layers = []
    
    # 粗轮廓层
    layer1_predicate = GT(Length2DBP1D(), 200.0)
    layer1_chaining = ChainSilhouetteIterator()
    layer1_shaders = [
        ConstantColorShader((0.0, 0.0, 0.0, 1.0)),
        ConstantThicknessShader(4.0)
    ]
    layers.append((layer1_predicate, layer1_chaining, layer1_shaders))
    
    # 细轮廓层
    layer2_predicate = AND(
        GT(Length2DBP1D(), 50.0),
        LT(Length2DBP1D(), 200.0)
    )
    layer2_chaining = ChainContourIterator()
    layer2_shaders = [
        ConstantColorShader((0.2, 0.2, 0.2, 1.0)),
        ConstantThicknessShader(2.0)
    ]
    layers.append((layer2_predicate, layer2_chaining, layer2_shaders))
    
    return layers
```

### 基于材质的线条样式

```python
from freestyle.predicates import MaterialBP2D, GT
from freestyle.chainingiterators import ChainContourIterator
from freestyle.shaders import ConstantColorShader, ConstantThicknessShader

def create_material_style():
    layers = []
    
    # 金属材质层
    metal_predicate = MaterialBP2D("Metal")
    metal_chaining = ChainContourIterator()
    metal_shaders = [
        ConstantColorShader((0.7, 0.7, 0.8, 1.0)),
        ConstantThicknessShader(2.5)
    ]
    layers.append((metal_predicate, metal_chaining, metal_shaders))
    
    # 塑料材质层
    plastic_predicate = MaterialBP2D("Plastic")
    plastic_chaining = ChainContourIterator()
    plastic_shaders = [
        ConstantColorShader((0.3, 0.3, 0.3, 1.0)),
        ConstantThicknessShader(1.5)
    ]
    layers.append((plastic_predicate, plastic_chaining, plastic_shaders))
    
    return layers
```

### 动态线条效果

```python
from freestyle.predicates import GT, NormalBP1D
from freestyle.chainingiterators import ChainSilhouetteIterator
from freestyle.shaders import ConstantColorShader, ThicknessShader

def create_dyanmic_style(time_factor):
    predicate = GT(NormalBP1D(), 0.0)
    
    chaining = ChainSilhouetteIterator()
    
    # 根据时间动态调整厚度
    thickness = 1.0 + 0.5 * time_factor
    
    shaders = [
        ConstantColorShader((0.0, 0.0, 0.0, 1.0)),
        ThicknessShader(thickness, thickness * 0.5)
    ]
    
    return predicate, chaining, shaders
```

## 调试技巧

```python
import freestyle.utils as utils

# 打印视图映射信息
view_map = utils.GetViewMap()
print(f"0D 元素数量: {len(view_map.svertices)}")
print(f"1D 元素数量: {len(view_map.edges)}")
print(f"线条数量: {len(view_map.strokes)}")

# 遍历所有线条
for stroke in view_map.strokes:
    print(f"线条点数: {len(stroke)}")
    print(f"线条长度: {stroke.length}")
    
    # 遍历线条上的点
    for vertex in stroke:
        print(f"位置: {vertex.point}")
        print(f"法线: {vertex.normal}")
```

## 性能优化建议

1. **使用适当的谓词**：精确的谓词可以减少处理的线条数量
2. **合理设置链化参数**：如折痕角度阈值
3. **优化着色器序列**：避免不必要的计算
4. **使用采样**：对于复杂线条，适当采样可以提高性能

```python
# 采样示例
from freestyle.shaders import SamplingShader

# 减少线条上的点数
sampling_shader = SamplingShader(5.0)  # 最小点间距

# 在着色器列表中使用
shaders = [
    sampling_shader,
    ConstantColorShader((0.0, 0.0, 0.0, 1.0))
]
```


---
## freestyle_chainingiterators

# freestyle.chainingiterators - 线条链化迭代器模块

freestyle.chainingiterators模块提供了Freestyle线条渲染中的链化迭代器，用于决定如何将相邻的边连接成连续的线条。

## 概述

链化迭代器是Freestyle线条渲染的核心组件之一。它们定义了从一条边到相邻边的连接规则，决定了最终生成线条的走向和连续性。不同的链化策略会产生不同风格的线条效果。

## 核心功能

### 基础迭代器

```python
from freestyle.chainingiterators import (
    ChainSilhouetteIterator,
    ChainBoundaryIterator,
    ChainCreaseAngleIterator,
    ChainContourIterator,
    ChainUVSinuousIterator
)

def create_silhouette_iterator(predicate=None):
    """创建轮廓链化迭代器"""
    return ChainSilhouetteIterator(predicate)

def create_boundary_iterator(predicate=None):
    """创建边界链化迭代器"""
    return ChainBoundaryIterator(predicate)

def create_crease_angle_iterator(angle_threshold=math.radians(45)):
    """创建折痕角度链化迭代器"""
    return ChainCreaseAngleIterator(angle_threshold)

def create_contour_iterator(view_map, predicate=None):
    """创建轮廓链化迭代器"""
    return ChainContourIterator(view_map, predicate)
```

### 迭代器配置

```python
def configure_silhouette_iterator(
    iterator,
    chaining_distance=10,
    initial_chaining_distance=10,
    orientation_flag=True,
    prob_flag=True
):
    """配置轮廓链化迭代器"""
    iterator.chaining_distance = chaining_distance
    iterator.initial_chaining_distance = initial_chaining_distance
    iterator.orientation_flag = orientation_flag
    iterator.prob_flag = prob_flag
    return iterator

def configure_crease_iterator(
    iterator,
    angle_threshold=math.radians(30),
    absolute=False
):
    """配置折痕链化迭代器"""
    iterator.angle_threshold = angle_threshold
    iterator.absolute = absolute
    return iterator
```

### 自定义迭代器

```python
class CustomChainingIterator:
    """自定义链化迭代器"""
    def __init__(self, predicate=None):
        self.predicate = predicate
        self.chaining_distance = 10
    
    def iterate(self, edge, stroke):
        """迭代方法"""
        pass
    
    def reset(self):
        """重置迭代器"""
        pass

def create_custom_iterator(predicate=None):
    """创建自定义链化迭代器"""
    return CustomChainingIterator(predicate)
```

## 实用函数

### 线条生成

```python
def generate_strokes(view_map, chain_iterator, predicate=None):
    """生成线条"""
    from freestyle import stroke
    
    strokes = []
    chain_iterator.reset()
    
    for stroke in chain_iterator.iterate():
        if stroke:
            strokes.append(stroke)
    
    return strokes

def generate_silhouette_strokes(view_map, predicate=None):
    """生成轮廓线条"""
    iterator = ChainSilhouetteIterator(predicate)
    return generate_strokes(view_map, iterator)

def generate_contour_strokes(view_map, predicate=None):
    """生成轮廓线条"""
    iterator = ChainContourIterator(view_map, predicate)
    return generate_strokes(view_map, iterator)
```

### 迭代器组合

```python
def chain_iterators(iter1, iter2):
    """组合两个迭代器"""
    class ChainedIterator:
        def __init__(self, it1, it2):
            self.it1 = it1
            self.it2 = it2
        
        def iterate(self, edge, stroke):
            result = self.it1.iterate(edge, stroke)
            if not result:
                result = self.it2.iterate(edge, stroke)
            return result
    
    return ChainedIterator(iter1, iter2)

def priority_chaining(iterator, priority_edges):
    """优先链化"""
    class PriorityIterator:
        def __init__(self, iterator, priorities):
            self.iterator = iterator
            self.priorities = priorities
        
        def iterate(self, edge, stroke):
            for edge_type in self.priorities:
                if edge.type == edge_type:
                    return self.iterator.iterate(edge, stroke)
            return None
    
    return PriorityIterator(iterator, priority_edges)
```

### 性能优化

```python
def optimize_chaining(strokes, max_length=100):
    """优化链化长度"""
    for stroke in strokes:
        while len(stroke.points) > max_length:
            stroke.points = stroke.points[:max_length]
    return strokes

def merge_short_strokes(strokes, min_length=5):
    """合并短线条"""
    merged = []
    current_stroke = None
    
    for stroke in strokes:
        if stroke.length() < min_length:
            if current_stroke:
                current_stroke.extend(stroke.points)
        else:
            if current_stroke:
                merged.append(current_stroke)
            current_stroke = stroke
    
    if current_stroke:
        merged.append(current_stroke)
    
    return merged
```

## 常见问题排查

### 线条不连续

链化规则问题：
- 调整链化距离参数
- 检查谓词过滤条件
- 尝试不同的迭代器类型

### 性能问题

线条数量过多：
- 增加链化距离减少线条数量
- 使用谓词过滤不必要的边
- 限制最大线条长度

### 线条丢失

视图映射问题：
- 确认view_map正确构建
- 检查迭代器初始化参数
- 验证场景对象可见性


---
## freestyle_functions

# freestyle.functions - 线条函数模块

freestyle.functions模块提供了Freestyle线条渲染中的函数工具，用于定义线条样式和效果。

## 概述

线条函数模块是Freestyle样式系统的核心组件，提供了各种函数来定义线条的行为和外观。这些函数可以组合使用来创建复杂的线条效果。

## 核心功能

### 曲线函数

```python
import math
from freestyle.functions import (
    curveLength,
    curveU,
    curveTi,
    curveTsi,
    curveNi,
    getParameter,
    getNormalizedParameter
)

def calculate_curve_length(stroke):
    """计算曲线长度"""
    return curveLength(stroke)

def get_u_at_point(stroke, point):
    """获取点的U参数"""
    return curveU(stroke, point)

def get_tangent_at_index(stroke, index):
    """获取指定索引处的切线"""
    return curveTi(stroke, index)

def get_smooth_tangent(stroke, index):
    """获取平滑切线"""
    return curveTsi(stroke, index)

def get_curvature(stroke, index):
    """获取曲率"""
    return curveNi(stroke, index)
```

### 参数函数

```python
def get_absolute_parameter(stroke, normalized_value):
    """获取绝对参数"""
    return getParameter(stroke, normalized_value)

def get_normalized_parameter(stroke, absolute_value):
    """获取归一化参数"""
    return getNormalizedParameter(stroke, absolute_value)

def map_range(value, in_min, in_max, out_min, out_max):
    """映射范围"""
    if in_max - in_min == 0:
        return out_min
    normalized = (value - in_min) / (in_max - in_min)
    return out_min + normalized * (out_max - out_min)
```

### 距离函数

```python
def get_distance_2d(point1, point2):
    """计算2D距离"""
    dx = point1.x - point2.x
    dy = point1.y - point2.y
    return math.sqrt(dx * dx + dy * dy)

def get_distance_3d(point1, point2):
    """计算3D距离"""
    dx = point1.x - point2.x
    dy = point1.y - point2.y
    dz = point1.z - point2.z
    return math.sqrt(dx * dx + dy * dy + dz * dz)
```

## 实用函数

### 角度函数

```python
def get_angle(vector1, vector2):
    """计算两个向量之间的角度"""
    dot = vector1.dot(vector2)
    len1 = vector1.length
    len2 = vector2.length
    if len1 == 0 or len2 == 0:
        return 0
    cos_angle = dot / (len1 * len2)
    cos_angle = max(-1, min(1, cos_angle))
    return math.degrees(math.acos(cos_angle))

def get_direction_angle(vector):
    """获取向量方向角度"""
    return math.degrees(math.atan2(vector.y, vector.x))
```

### 插值函数

```python
def lerp(start, end, factor):
    """线性插值"""
    return start + (end - start) * factor

def ease_in_out(t):
    """缓入缓出"""
    return t * t * (3 - 2 * t)

def smooth_step(edge0, edge1, x):
    """平滑阶梯"""
    t = clamp((x - edge0) / (edge1 - edge0), 0, 1)
    return t * t * (3 - 2 * t)

def clamp(x, min_val, max_val):
    """限制范围"""
    return max(min_val, min(x, max_val))
```

### 曲线评估

```python
def evaluate_bezier(t, control_points):
    """评估贝塞尔曲线"""
    n = len(control_points) - 1
    result = Vector((0, 0, 0))
    for i, point in enumerate(control_points):
        bernstein = binomial(n, i) * (1 - t) ** (n - i) * t ** i
        result += point * bernstein
    return result

def binomial(n, k):
    """计算二项式系数"""
    if k < 0 or k > n:
        return 0
    result = 1
    for i in range(1, min(k, n - k) + 1):
        result = result * (n - i + 1) // i
    return result
```

## 常见问题排查

### 计算错误

参数问题：
- 确认输入值在有效范围
- 检查向量长度非零
- 验证曲线点数量

### 性能问题

计算复杂：
- 缓存常用计算结果
- 减少不必要的函数调用
- 使用近似计算


---
## freestyle_predicates

# freestyle.predicates - 线条选择谓词模块

freestyle.predicates模块提供了Freestyle线条渲染中的谓词函数，用于根据几何属性和材质属性选择要渲染的边。

## 概述

谓词是Freestyle样式模块的核心组件之一，它们定义了选择线条的条件。通过组合不同的谓词，可以精确控制哪些边被渲染成线条，实现丰富的艺术效果。

## 核心功能

### 比较谓词

```python
from freestyle.predicates import (
    GT, LT, EQ, NOT, AND, OR,
    GreaterThan, LessThan, EqualTo,
    Not, And, Or
)

def create_length_predicate(threshold, greater=True):
    """创建长度谓词"""
    from freestyle.predicates import Length2DBP1D
    if greater:
        return GT(Length2DBP1D(), threshold)
    else:
        return LT(Length2DBP1D(), threshold)

def create_curvature_predicate(threshold, greater=True):
    """创建曲率谓词"""
    from freestyle.predicates import Curvature2DBP1D
    if greater:
        return GT(Curvature2DBP1D(), threshold)
    else:
        return LT(Curvature2DBP1D(), threshold)

def create_normal_predicate(axis, threshold, greater=True):
    """创建法线谓词"""
    from freestyle.predicates import NormalBP1D
    normal_pred = NormalBP1D()
    if greater:
        return GT(normal_pred, threshold) if axis == 'Z' else None
    return normal_pred
```

### 几何谓词

```python
from freestyle.predicates import (
    Length2DBP1D,
    Curvature2DBP1D,
    NormalBP1D,
    MaterialBP2D,
    BackTesselationBP1D,
    ProjectedX,
    ProjectedY,
    ProjectedZ
)

def create_face_area_predicate(threshold, greater=True):
    """创建面面积谓词"""
    from freestyle.predicates import Face2DAreaBP1D
    if greater:
        return GT(Face2DAreaBP1D(), threshold)
    else:
        return LT(Face2DAreaBP1D(), threshold)

def create_projected_x_predicate(threshold, greater=True):
    """创建投影X谓词"""
    pred = ProjectedX()
    if greater:
        return GT(pred, threshold)
    else:
        return LT(pred, threshold)

def create_projected_y_predicate(threshold, greater=True):
    """创建投影Y谓词"""
    pred = ProjectedY()
    if greater:
        return GT(pred, threshold)
    else:
        return LT(pred, threshold)

def create_projected_z_predicate(threshold, greater=True):
    """创建投影Z谓词"""
    pred = ProjectedZ()
    if greater:
        return GT(pred, threshold)
    else:
        return LT(pred, threshold)
```

### 材质谓词

```python
def create_material_predicate(material_name):
    """创建材质谓词"""
    return MaterialBP2D() == material_name

def create_material_compare_predicate(material_name, operator='=='):
    """创建材质比较谓词"""
    pred = MaterialBP2D()
    if operator == '==':
        return pred == material_name
    elif operator == '!=':
        return pred != material_name
    return None

def create_material_name_contains_predicate(substring):
    """创建材质名称包含谓词"""
    def contains_material():
        pass
    return contains_material
```

### 组合谓词

```python
def combine_predicates(pred1, pred2, operator='AND'):
    """组合谓词"""
    if operator == 'AND':
        return And(pred1, pred2)
    elif operator == 'OR':
        return Or(pred1, pred2)
    elif operator == 'NOT':
        return Not(pred1)
    return None

def create_complex_predicate(length_thresh, curvature_thresh):
    """创建复合谓词"""
    length_pred = GT(Length2DBP1D(), length_thresh)
    curvature_pred = GT(Curvature2DBP1D(), curvature_thresh)
    return And(length_pred, curvature_pred)

def negate_predicate(pred):
    """取反谓词"""
    return Not(pred)
```

## 实用函数

### 谓词构建器

```python
def build_predicate_from_config(config):
    """从配置构建谓词"""
    pred_type = config.get('type')
    if pred_type == 'length':
        return GT(Length2DBP1D(), config['threshold'])
    elif pred_type == 'curvature':
        return GT(Curvature2DBP1D(), config['threshold'])
    elif pred_type == 'normal':
        return GT(NormalBP1D(), config['threshold'])
    elif pred_type == 'compound':
        preds = [build_predicate_from_config(c) for c in config['conditions']]
        if config.get('operator') == 'AND':
            return And(*preds)
        elif config.get('operator') == 'OR':
            return Or(*preds)
    return None

def predicates_to_string(predicate, indent=0):
    """将谓词转换为字符串"""
    prefix = '  ' * indent
    pred_type = type(predicate).__name__
    if hasattr(predicate, 'predicates'):
        children = [predicates_to_string(p, indent + 1) for p in predicate.predicates]
        return f'{prefix}{pred_type}(\n{"".join(children)}\n{prefix})'
    elif hasattr(predicate, 'threshold'):
        return f'{prefix}{pred_type}(threshold={predicate.threshold})'
    return f'{prefix}{pred_type}'
```

### 谓词测试

```python
def test_predicate(predicate, view_map):
    """测试谓词"""
    from freestyle import operators
    
    results = []
    for edge in view_map.edges:
        if predicate(view_map, edge):
            results.append(edge)
    return results

def count_predicate_matches(predicate, view_map):
    """统计谓词匹配数量"""
    count = 0
    for edge in view_map.edges:
        if predicate(view_map, edge):
            count += 1
    return count

def get_predicate_statistics(predicate, view_map):
    """获取谓词统计信息"""
    total = len(view_map.edges)
    matched = count_predicate_matches(predicate, view_map)
    return {
        'total_edges': total,
        'matched_edges': matched,
        'match_ratio': matched / total if total > 0 else 0
    }
```

### 常用谓词组合

```python
def create_silhouette_predicate():
    """创建轮廓线谓词"""
    return Not(BackTesselationBP1D())

def create_contour_predicate():
    """创建轮廓线谓词"""
    pred = Not(BackTesselationBP1D())
    return pred

def create_edge_predicate(threshold=5):
    """创建边缘线谓词"""
    length_pred = GT(Length2DBP1D(), threshold)
    back_pred = Not(BackTesselationBP1D())
    return And(length_pred, back_pred)

def create_skin_ridge_predicate(curvature_threshold=0.5):
    """创建皮肤褶皱谓词"""
    curvature_pred = GT(Curvature2DBP1D(), curvature_threshold)
    back_pred = Not(BackTesselationBP1D())
    return And(curvature_pred, back_pred)

def create_ridge_predicate(angle_threshold=math.radians(45)):
    """创建脊线谓词"""
    crease_pred = ChainCreaseAngleIterator(angle_threshold)
    return crease_pred
```

## 常见问题排查

### 谓词不生效

谓词类型错误：
- 确认谓词类导入正确
- 检查谓词参数类型
- 验证谓词组合逻辑

### 谓词组合复杂

嵌套层级过深：
- 使用具名谓词提高可读性
- 限制组合深度
- 分解复杂谓词为简单谓词

### 性能问题

谓词计算开销大：
- 避免重复计算相同属性
- 使用缓存优化
- 简化谓词逻辑


---
## freestyle_shaders

# freestyle.shaders - 线条着色器模块

freestyle.shaders模块提供了Freestyle线条渲染中的着色器函数，用于控制线条的视觉属性，包括颜色、宽度、不透明度等。

## 概述

着色器是Freestyle样式模块的核心组件之一，它们定义了线条的渲染外观。通过应用不同的着色器，可以创建各种艺术风格的线条效果，如墨水渲染、铅笔画、水彩等。

## 核心功能

### 颜色着色器

```python
from freestyle.shaders import (
    ConstantColorShader,
    GradientColorShader,
    TextureColorShader,
    MaterialColorShader
)

def create_constant_color_shader(color=(0, 0, 0, 1)):
    """创建常量颜色着色器"""
    return ConstantColorShader(color)

def create_gradient_color_shader(
    colors, 
    direction='X', 
    interpolation='LINEAR'
):
    """创建渐变颜色着色器"""
    return GradientColorShader(colors, direction, interpolation)

def create_material_color_shader():
    """创建材质颜色着色器"""
    return MaterialColorShader()

def create_texture_color_shader(texture_image, uv_layer=None):
    """创建纹理颜色着色器"""
    return TextureColorShader(texture_image, uv_layer)
```

### 宽度着色器

```python
from freestyle.shaders import (
    ConstantThicknessShader,
    VariableThicknessShader,
    FwidthThicknessShader,
    CurvatureWidthShader
)

def create_constant_thickness_shader(thickness=2.0):
    """创建常量宽度着色器"""
    return ConstantThicknessShader(thickness)

def create_variable_thickness_shader(
    thickness_base=1.0,
    thickness_min=0.5,
    thickness_max=5.0
):
    """创建可变宽度着色器"""
    return VariableThicknessShader(
        thickness_base, 
        thickness_min, 
        thickness_max
    )

def create_curvature_width_shader(
    base_thickness=1.0,
    curvature_scale=2.0,
    min_thickness=0.5,
    max_thickness=4.0
):
    """创建曲率宽度着色器"""
    return CurvatureWidthShader(
        base_thickness,
        curvature_scale,
        min_thickness,
        max_thickness
    )

def create_fwidth_thickness_shader(thickness_base=2.0):
    """创建Fwidth宽度着色器"""
    return FwidthThicknessShader(thickness_base)
```

### 不透明度着色器

```python
from freestyle.shaders import (
    ConstantAlphaShader,
    GradientAlphaShader,
    CurvatureAlphaShader
)

def create_constant_alpha_shader(alpha=1.0):
    """创建常量不透明度着色器"""
    return ConstantAlphaShader(alpha)

def create_gradient_alpha_shader(
    alphas, 
    direction='X', 
    interpolation='LINEAR'
):
    """创建渐变不透明度着色器"""
    return GradientAlphaShader(alphas, direction, interpolation)

def create_curvature_alpha_shader(
    base_alpha=1.0,
    curvature_scale=1.0,
    min_alpha=0.2,
    max_alpha=1.0
):
    """创建曲率不透明度着色器"""
    return CurvatureAlphaShader(
        base_alpha,
        curvature_scale,
        min_alpha,
        max_alpha
    )
```

## 实用函数

### 着色器链

```python
def chain_shaders(shader1, shader2):
    """链化两个着色器"""
    class ChainedShader:
        def __init__(self, s1, s2):
            self.s1 = s1
            self.s2 = s2
        
        def shade(self, stroke):
            self.s1.shade(stroke)
            self.s2.shade(stroke)
    
    return ChainedShader(shader1, shader2)

def create_stroke_shader_pipeline(shaders):
    """创建着色器管线"""
    def apply_shaders(stroke):
        for shader in shaders:
            shader.shade(stroke)
    return apply_shaders

def apply_multiple_shaders(stroke, shaders):
    """应用多个着色器"""
    for shader in shaders:
        shader.shade(stroke)
```

### 高级着色器

```python
def create_inkscape_shader(
    stroke_color=(0, 0, 0, 1),
    fill_color=(1, 1, 1, 1),
    thickness=2.0
):
    """创建墨水风格着色器"""
    color_shader = ConstantColorShader(stroke_color)
    thickness_shader = ConstantThicknessShader(thickness)
    return chain_shaders(color_shader, thickness_shader)

def create_pencil_shader(
    base_thickness=1.5,
    opacity=0.8,
    jitter_strength=0.3
):
    """创建铅笔风格着色器"""
    thickness_shader = VariableThicknessShader(
        base_thickness * 0.5,
        base_thickness * 0.5,
        base_thickness * 1.5
    )
    alpha_shader = ConstantAlphaShader(opacity)
    return chain_shaders(thickness_shader, alpha_shader)

def create_watercolor_shader(
    wetness=0.5,
    thickness_range=(0.5, 3.0)
):
    """创建水彩风格着色器"""
    from freestyle.shaders import StrokeTextureShader
    
    texture_shader = StrokeTextureShader()
    thickness_shader = VariableThicknessShader(
        thickness_range[0],
        thickness_range[0],
        thickness_range[1]
    )
    return chain_shaders(thickness_shader, texture_shader)
```

### 着色器参数调整

```python
def adjust_shader_parameter(shader, parameter, value):
    """调整着色器参数"""
    if hasattr(shader, parameter):
        setattr(shader, parameter, value)
        return True
    return False

def get_shader_parameters(shader):
    """获取着色器参数"""
    params = {}
    for attr in dir(shader):
        if not attr.startswith('_') and not callable(getattr(shader, attr)):
            params[attr] = getattr(shader, attr)
    return params

def clone_shader(shader):
    """克隆着色器"""
    params = get_shader_parameters(shader)
    shader_class = type(shader)
    return shader_class(**params)
```

## 常见问题排查

### 着色效果不明显

参数范围问题：
- 调整着色器参数范围
- 检查输入数据是否有效
- 确认着色器顺序正确

### 渲染错误

着色器不兼容：
- 确认着色器类型匹配
- 检查参数类型正确
- 验证纹理文件存在

### 性能问题

着色器计算复杂：
- 减少着色器数量
- 简化着色器逻辑
- 使用缓存优化


---
## freestyle_types

# freestyle.types - 类型定义模块

freestyle.types模块定义了Freestyle线条渲染框架中使用的核心数据类型，包括边、线条、点等基础数据结构。

## 概述

类型模块定义了Freestyle系统的数据结构，这些类型是构建线条渲染管线的基础。理解这些类型对于开发自定义样式和工具至关重要。

## 核心类型

### 边类型

```python
from freestyle.types import (
    Edge,
    OrientedEdge,
    ViewEdge,
    FEdge,
    FEdgeSilhouette,
    FEdgeCrease,
    FEdgeBorder,
    FEdgeRidge,
    FEdgeValence1,
    FEdgeValence2
)

class Edge:
    """基础边类型"""
    def __init__(self, vertices=None, id=None):
        self.vertices = vertices or []
        self.id = id
        self.length = 0.0
    
    def compute_length(self):
        """计算边长度"""
        if len(self.vertices) >= 2:
            self.length = (self.vertices[0] - self.vertices[1]).length
        return self.length

class ViewEdge(Edge):
    """视图边类型"""
    def __init__(self, edge=None, viewpoint=None):
        super().__init__()
        self.edge = edge
        self.viewpoint = viewpoint
        self.view2d = None
        self.is_silhouette = False
        self.is_border = False
    
    def compute_projection(self, viewpoint):
        """计算投影"""
        self.view2d = self.edge.project(viewpoint)

class OrientedEdge:
    """有向边类型"""
    def __init__(self, view_edge, orientation):
        self.view_edge = view_edge
        self.orientation = orientation
        self.tvertices = []
    
    def get_opposite(self):
        """获取相反方向的有向边"""
        return OrientedEdge(self.view_edge, not self.orientation)
```

### 线条类型

```python
from freestyle.types import (
    Stroke,
    StrokeVertex,
    StrokeAttribute
)

class StrokeVertex:
    """线条顶点类型"""
    def __init__(
        self, 
        point=None, 
        normal=None, 
        curvature=0.0,
        attributes=None
    ):
        self.point = point or Vector((0, 0, 0))
        self.normal = normal or Vector((0, 0, 1))
        self.curvature = curvature
        self.attributes = attributes or StrokeAttribute()
    
    def compute_curvature(self):
        """计算曲率"""
        pass

class Stroke:
    """线条类型"""
    def __init__(self, vertices=None):
        self.vertices = vertices or []
        self.attributes = StrokeAttribute()
        self.length = 0.0
        self.id = None
    
    def __len__(self):
        return len(self.vertices)
    
    def __iter__(self):
        return iter(self.vertices)
    
    def compute_length(self):
        """计算线条长度"""
        self.length = sum(
            (self.vertices[i].point - self.vertices[i-1].point).length
            for i in range(1, len(self.vertices))
        )
        return self.length
    
    def get_point_at(self, distance):
        """获取指定距离的点"""
        pass

class StrokeAttribute:
    """线条属性类型"""
    def __init__(
        self, 
        color=None, 
        thickness=None, 
        alpha=None,
        visibility=True
    ):
        self.color = color or (0, 0, 0, 1)
        self.thickness = thickness or 1.0
        self.alpha = alpha or 1.0
        self.visibility = visibility
        self.texture_id = None
    
    def __eq__(self, other):
        if not isinstance(other, StrokeAttribute):
            return False
        return (self.color == other.color and
                self.thickness == other.thickness and
                self.alpha == other.alpha)
```

## 扩展类型

### 谓词结果

```python
from freestyle.types import (
    PredType,
    STROKE,
    EDGE,
    VERTEX1D,
    VERTEX2D
)

class PredType:
    """谓词结果类型"""
    def __init__(self, value=True):
        self.value = value
    
    def __bool__(self):
        return self.value
    
    def __and__(self, other):
        return PredType(self.value and other.value)
    
    def __or__(self, other):
        return PredType(self.value or other.value)
    
    def __invert__(self):
        return PredType(not self.value)
```

### 视图映射

```python
from freestyle.types import ViewMap

class ViewMap:
    """视图映射类型"""
    def __init__(self):
        self.edges = []
        self.fedges = []
        self.stroke_list = None
    
    def __len__(self):
        return len(self.edges)
    
    def __iter__(self):
        return iter(self.edges)
    
    def add_edge(self, edge):
        """添加边"""
        self.edges.append(edge)
    
    def add_fedge(self, fedge):
        """添加特征边"""
        self.fedges.append(fedge)
    
    def build(self):
        """构建视图映射"""
        pass
    
    def clear(self):
        """清空视图映射"""
        self.edges.clear()
        self.fedges.clear()
```

### 样式模块

```python
from freestyle.types import (
    StrokeShader,
    ChainPredicateIterator,
    StrokeIterator
)

class StrokeShader:
    """线条着色器类型"""
    def __init__(self):
        self.name = None
    
    def shade(self, stroke):
        """着色方法"""
        pass

class ChainPredicateIterator:
    """链化谓词迭代器类型"""
    def __init__(self, predicate=None, it=None):
        self.predicate = predicate
        self.iterator = it
    
    def iterate(self, stroke):
        """迭代方法"""
        pass
    
    def reset(self):
        """重置迭代器"""
        pass

class StrokeIterator:
    """线条迭代器类型"""
    def __init__(self, chain_predicate_iterator=None, stroke_shader=None):
        self.chain_predicate_iterator = chain_predicate_iterator
        self.stroke_shader = stroke_shader
    
    def __iter__(self):
        return self
    
    def __next__(self):
        """获取下一条线条"""
        pass
```

## 实用函数

### 类型转换

```python
def edge_to_viewedge(edge, viewpoint):
    """将边转换为视图边"""
    return ViewEdge(edge, viewpoint)

def points_to_stroke(points):
    """将点转换为线条"""
    vertices = [StrokeVertex(p) for p in points]
    stroke = Stroke(vertices)
    stroke.compute_length()
    return stroke

def stroke_to_polylines(stroke):
    """将线条转换为多段线"""
    return [v.point for v in stroke.vertices]
```

### 类型检查

```python
def is_fedge_silhouette(fedge):
    """检查是否为轮廓特征边"""
    return isinstance(fedge, FEdgeSilhouette)

def is_fedge_crease(fedge):
    """检查是否为折痕特征边"""
    return isinstance(fedge, FEdgeCrease)

def is_fedge_ridge(fedge):
    """检查是否为脊特征边"""
    return isinstance(fedge, FEdgeRidge)

def is_fedge_border(fedge):
    """检查是否为边界特征边"""
    return isinstance(fedge, FEdgeBorder)
```

## 常见问题排查

### 类型不匹配

类型使用错误：
- 确认使用正确的类型导入
- 检查对象实例类型
- 使用isinstance验证

### 属性访问错误

属性不存在：
- 使用hasattr检查属性
- 确认对象已正确初始化
- 检查属性名称拼写

### 内存问题

对象数量过多：
- 及时清理不需要的对象
- 使用弱引用管理对象
- 分批处理大量数据


---
## freestyle_utils

# freestyle.utils - 工具函数模块

freestyle.utils模块提供了Freestyle线条渲染框架中的通用工具函数，包括几何计算、视图处理和样式操作等辅助功能。

## 概述

工具函数模块封装了Freestyle系统中常用的辅助功能，涵盖几何计算、线条处理、样式管理等方面。这些函数可以帮助开发者快速实现自定义功能。

## 核心功能

### 几何计算

```python
from freestyle.utils import (
    compute_polygon_smoothness,
    compute_line_smoothness,
    compute_viewedge_density,
    compute_fedge_length
)

def calculate_curvature(points):
    """计算曲率"""
    if len(points) < 3:
        return 0.0
    
    total_curvature = 0.0
    for i in range(1, len(points) - 1):
        v1 = points[i] - points[i-1]
        v2 = points[i+1] - points[i]
        if v1.length > 0 and v2.length > 0:
            v1.normalize()
            v2.normalize()
            dot = max(-1.0, min(1.0, v1.dot(v2)))
            angle = math.acos(dot)
            total_curvature += angle
    
    return total_curvature / max(1, len(points) - 2)

def calculate_total_length(points):
    """计算总长度"""
    total = 0.0
    for i in range(1, len(points)):
        total += (points[i] - points[i-1]).length
    return total

def calculate_polygon_area(points):
    """计算多边形面积"""
    if len(points) < 3:
        return 0.0
    
    area = 0.0
    n = len(points)
    for i in range(n):
        j = (i + 1) % n
        area += points[i].x * points[j].y
        area -= points[j].x * points[i].y
    return abs(area) / 2.0

def calculate_bounding_box(points):
    """计算边界框"""
    if not points:
        return None, None
    
    min_x = min(p.x for p in points)
    min_y = min(p.y for p in points)
    min_z = min(p.z for p in points)
    max_x = max(p.x for p in points)
    max_y = max(p.y for p in points)
    max_z = max(p.z for p in points)
    
    return Vector((min_x, min_y, min_z)), Vector((max_x, max_y, max_z))
```

### 线条处理

```python
def simplify_stroke(points, tolerance=0.1):
    """简化线条"""
    if len(points) < 3:
        return points
    
    simplified = [points[0]]
    last_point = points[0]
    
    for point in points[1:]:
        if (point - last_point).length > tolerance:
            simplified.append(point)
            last_point = point
    
    if simplified[-1] != points[-1]:
        simplified.append(points[-1])
    
    return simplified

def resample_stroke(points, num_points):
    """重采样线条"""
    if len(points) < 2:
        return points
    
    total_length = calculate_total_length(points)
    segment_length = total_length / (num_points - 1)
    
    result = [points[0]]
    current_length = 0
    target_length = segment_length
    
    for i in range(1, len(points)):
        while current_length + (points[i] - points[i-1]).length >= target_length:
            t = (target_length - current_length) / (points[i] - points[i-1]).length
            new_point = points[i-1] + (points[i] - points[i-1]) * t
            result.append(new_point)
            target_length += segment_length
        current_length += (points[i] - points[i-1]).length
    
    while len(result) < num_points:
        result.append(points[-1])
    
    return result[:num_points]

def smooth_stroke(points, strength=0.5, iterations=1):
    """平滑线条"""
    if len(points) < 3:
        return points
    
    for _ in range(iterations):
        new_points = [points[0]]
        for i in range(1, len(points) - 1):
            smoothed = points[i] * (1 - strength) + (points[i-1] + points[i+1]) / 2 * strength
            new_points.append(smoothed)
        new_points.append(points[-1])
        points = new_points
    
    return points
```

### 视图工具

```python
def get_view_edge_density(view_map, region):
    """获取视图边密度"""
    density_map = {}
    for edge in view_map.edges:
        if edge.view2d:
            center = edge.view2d.get_center()
            x, y = int(center.x), int(center.y)
            if 0 <= x < region.width and 0 <= y < region.height:
                density_map[(x, y)] = density_map.get((x, y), 0) + 1
    return density_map

def project_points_to_view(points, view_matrix, projection_matrix):
    """将点投影到视图"""
    projected = []
    for point in points:
        clip = projection_matrix @ (view_matrix @ point)
        ndc = Vector((clip.x / clip.w, clip.y / clip.w))
        screen = Vector((
            (ndc.x + 1) / 2 * region.width,
            (ndc.y + 1) / 2 * region.height
        ))
        projected.append(screen)
    return projected

def calculate_view_coverage(view_map, region):
    """计算视图覆盖率"""
    covered_pixels = set()
    for edge in view_map.edges:
        if edge.view2d:
            for point in edge.view2d.points:
                x, y = int(point.x), int(point.y)
                if 0 <= x < region.width and 0 <= y < region.height:
                    covered_pixels.add((x, y))
    return len(covered_pixels) / (region.width * region.height)
```

## 实用函数

### 样式管理

```python
def load_style_file(filepath):
    """加载样式文件"""
    with open(filepath, 'r') as f:
        return json.load(f)

def save_style_file(style_data, filepath):
    """保存样式文件"""
    with open(filepath, 'w') as f:
        json.dump(style_data, f, indent=2)

def apply_style(stroke, style_config):
    """应用样式配置"""
    if 'color' in style_config:
        stroke.attributes.color = tuple(style_config['color'])
    if 'thickness' in style_config:
        stroke.attributes.thickness = style_config['thickness']
    if 'alpha' in style_config:
        stroke.attributes.alpha = style_config['alpha']

def extract_style(stroke):
    """提取样式配置"""
    return {
        'color': list(stroke.attributes.color),
        'thickness': stroke.attributes.thickness,
        'alpha': stroke.attributes.alpha
    }
```

### 批量处理

```python
def process_strokes(strokes, processor_func):
    """批量处理线条"""
    results = []
    for stroke in strokes:
        result = processor_func(stroke)
        results.append(result)
    return results

def filter_strokes(strokes, filter_func):
    """过滤线条"""
    return [s for s in strokes if filter_func(s)]

def sort_strokes(strokes, key_func, reverse=False):
    """排序线条"""
    return sorted(strokes, key=key_func, reverse=reverse)

def group_strokes(strokes, group_func):
    """分组线条"""
    groups = {}
    for stroke in strokes:
        key = group_func(stroke)
        if key not in groups:
            groups[key] = []
        groups[key].append(stroke)
    return groups
```

### 性能优化

```python
def batch_compute_lengths(strokes):
    """批量计算长度"""
    for stroke in strokes:
        stroke.compute_length()
    return strokes

def optimize_stroke_density(strokes, max_points=100):
    """优化线条密度"""
    for stroke in strokes:
        if len(stroke.vertices) > max_points:
            stroke.vertices = resample_stroke(
                [v.point for v in stroke.vertices],
                max_points
            )

def cull_invisible_strokes(strokes, view_bounds):
    """剔除不可见线条"""
    visible = []
    for stroke in strokes:
        if is_visible(stroke, view_bounds):
            visible.append(stroke)
    return visible

def is_visible(stroke, view_bounds):
    """检查线条是否可见"""
    min_x, min_y, max_x, max_y = view_bounds
    for vertex in stroke.vertices:
        if not (min_x <= vertex.point.x <= max_x and
                min_y <= vertex.point.y <= max_y):
            return False
    return True
```

## 常见问题排查

### 线条处理错误

输入数据问题：
- 确认点列表有效
- 检查点数量是否足够
- 验证坐标范围正确

### 性能问题

处理时间过长：
- 使用批量处理优化
- 减少不必要的计算
- 使用缓存机制

### 样式应用失败

配置格式错误：
- 确认JSON格式正确
- 检查配置参数类型
- 验证属性名称存在


