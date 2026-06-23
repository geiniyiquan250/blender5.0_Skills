# bpy_physics_materials

---
## bpy_physics_simulation

# 物理模拟模块

## 物理系统概述

Blender 的物理系统允许您模拟许多不同的真实世界物理现象。您可以使用这些系统创建各种静态和动态效果，如头发、草和羊群、雨、烟雾和灰尘、水、布料、果冻等。

```python
# 物理系统类型
# 刚体 - 刚性物体碰撞和坠落
# 布料 - 织物和柔性材料
# 软体 - 弹性变形物体
# 流体 - 液体和气体
# 粒子 - 头发、草、烟雾
# 动态绘画 - 绘画影响模拟
# 力场 - 力和影响区域
# 碰撞 - 物体间交互

# 快速效果
# Object --> Quick Effects 菜单
# 设置包含所选对象的基本模拟场景或效果
# 工具将添加基本对象如域或粒子系统
# 带有预定义设置
# 以便立即查看结果
```

## 快速效果工具

### 快速毛发

快速毛发在所选对象上添加毛发设置。毛发设置基于几何节点构建，使用 Blender 作为捆绑资产附带的毛发节点组。

```python
# 快速毛发参数
Density - 生成的毛发曲线的表面密度
Length - 生成的毛发曲线的长度
Hair Radius - 毛发的宽度，用于渲染
View Percentage - 视口密度的因子
Apply Hair Curves - 应用使用毛发生成节点组的修饰符
Noise - 使用噪声纹理变形毛发
Frizz - 使用每点随机向量使毛发卷曲

# 毛发节点组
Generate Hair Curves - 生成毛发曲线
Hair Curves Noise - 毛发曲线噪声
Frizz Hair Curves - 卷曲毛发曲线
```

## 粒子系统

粒子系统是创建头发、草、烟雾、雨和其他效果的核心工具。

### 粒子系统基础

粒子系统由三个主要部分组成：发射器、粒子设置和渲染。

```python
# 发射器
# 发射粒子的对象
# 可以是网格、面、顶点或体积
# 决定粒子产生位置

# 粒子设置
# 定义粒子行为
# 数量、大小、速度
# 生命周期、物理类型
# 渲染方式

# 渲染
# 渲染粒子为对象
# 渲染为线条
# 渲染为广告牌
# 渲染为毛发
```

### 发射设置

```python
# 发射数量
Number - 粒子总数
Seed - 随机种子
Jitter - 发射器上的抖动

# 发射源
Emitter - 发射对象
Volume - 体积发射
Faces - 面发射
Vertices - 顶点发射
Bounding Box - 边界框发射

# 分布
Random - 随机分布
Grid - 网格分布
Jittered - 抖动网格

# 时间
Start Frame - 开始帧
End Frame - 结束帧
Lifetime - 生命周期
Lifetime Randomness - 生命周期随机性
```

### 物理类型

```python
# 牛顿物理学
# 标准粒子物理
# 受重力影响
# 有速度和加速度

# 关键帧物理
# 粒子沿关键帧路径
# 可控运动
# 精确控制

# 流体物理
# 流体模拟集成
# 真实流体行为
# 湍流效果

# 群行为
# 模拟群体行为
# 规则和目标
# 交互影响
```

### 毛发粒子

毛发粒子用于创建头发、胡须、草等效果。

```python
# 毛发发射
从网格表面发射
控制密度和分布
指定长度和粗细

# 毛发形状
Guid hairs - 引导毛发
Children - 子毛发
Clumping - 聚集
Kinking - 弯曲
Twisting - 扭曲

# 毛发动力学
Dynamics - 动力学
Collision - 碰撞
Constraints - 约束
```

### 粒子渲染

```python
# 渲染类型
None - 不渲染
Object - 对象渲染
Collection - 集合渲染
Billboard - 广告牌渲染
Line - 线条渲染

# 对象渲染
重复对象实例化
随机旋转和缩放
实例属性

# 广告牌渲染
总是面向相机
使用纹理
动画支持

# 毛发渲染
毛发曲线
体积渲染
程序化着色
```

### 粒子力场

力场影响粒子的运动和行为。

```python
# 力场类型
Force - 力场
Vortex - 漩涡
Magnet - 磁场
Wind - 风力
Texture - 纹理力场
Boid - 群行为力场
Harmonic - 谐波力场
Charge - 电荷力场

# 力场参数
Strength - 强度
Falloff - 衰减
Flow - 流动
Noise - 噪声
```

## 布料模拟

布料模拟用于创建衣物、窗帘、旗帜等织物效果。

### 布料基础

```python
# 布料修饰符
添加到网格对象
自动计算织物行为
支持碰撞和约束

# 布料属性
质量 - 织物质量
刚度 - 织物刚度
阻尼 - 运动阻尼
```

### 布料物理属性

```python
# 质量
质量影响重力效果
轻质布料飘动更多
重质布料更稳定

# 刚度
结构刚度 - 保持形状
剪切刚度 - 抗剪切变形
弯曲刚度 - 抗弯曲

# 阻尼
空气阻尼 - 空气阻力
内部阻尼 - 内部摩擦
```

### 布料碰撞

```python
# 自碰撞
检测布料自身碰撞
防止穿透
保持体积

# 碰撞对象
指定碰撞对象
碰撞距离
摩擦系数

# 碰撞参数
Distance - 碰撞距离
Friction - 摩擦力
Repulsion - 排斥力
```

### 布料约束

```python
# 针点
固定布料特定点
保持位置不变
创建悬挂效果

# 折痕
添加折痕记忆
控制折痕深度
创建预设折痕

# 皮带约束
沿路径约束布料
控制布料运动
创建引导效果
```

## 软体模拟

软体模拟用于创建果冻、气球、凝胶等弹性物体。

### 软体基础

```python
# 软体对象
具有弹性变形
受力和碰撞影响
保持总体积

# 软体参数
弹性 - 恢复形状
塑性 - 永久变形
阻尼 - 能量损失
```

### 软体力

```python
# 外力
重力 - 持续向下的力
风力 - 空气流动影响
力场 - 自定义力

# 内力
压力 - 内部压力
体积保持 - 保持体积
弹簧力 - 连接弹性
```

### 软体碰撞

```python
# 碰撞检测
软体与刚体碰撞
软体间碰撞
自碰撞

# 碰撞响应
变形响应
反弹效果
摩擦影响
```

### 软体设置

```python
# 求解器设置
迭代次数 - 精度控制
误差阈值 - 收敛精度
时间步长 - 模拟稳定性

# 边缘设置
边缘弹簧 - 保持边缘长度
边缘刚度 - 边缘硬度
边缘阻尼 - 边缘能量损失

# 目标设置
目标权重 - 目标影响
目标弹性 - 目标吸引力
保持形状 - 目标形状记忆
```

## 流体模拟

流体模拟用于创建水、烟雾、火等效果。

### 流体基础

```python
# 域对象
定义模拟空间
包含流体模拟
设置分辨率和边界

# 流体类型
液体 - 水和其他液体
气体 - 烟雾和火
自适应域 - 动态调整域大小

# 流体设置
分辨率 - 模拟精度
时间步长 - 模拟速度
迭代次数 - 求解精度
```

### 域设置

```python
# 通用设置
Resolution Divisions - 分辨率
Time Scale - 时间缩放
Fluid - 流体
End Time - 结束时间

# 内存管理
Cache - 缓存
Cache Type - 缓存类型
Resolution - 分辨率控制

# 显示设置
Viewport Display - 视口显示
Render Display - 渲染显示
Slice - 切片显示
```

### 流体类型

```python
# 入口
Inflow - 流入
从对象流入流体
控制流量和速度

# 出口
Outflow - 流出
从区域流出流体
清除流体

# 障碍物
Effector - 障碍物
流体与之交互
控制碰撞行为

# 引导
Guides - 引导
控制流体方向
添加湍流和旋转
```

### 液体设置

```python
# 液体参数
Surface - 表面
SurfaceNarrowing - 表面变窄
Tracer - 示踪粒子

# 泡沫
Foam - 泡沫
Bubble - 气泡
Spray - 水雾
Particles - 粒子

# 渲染
Mesh - 网格
Flip Particles - FLIP粒子
Preview - 预览
```

### 气体设置

```python
# 气体参数
Density - 密度
Dissolve - 消散
Buoyancy - 浮力

# 火焰
Fire - 火焰
Color - 颜色
Temperature - 温度
Flames - 火焰

# 烟雾
Smoke - 烟雾
Color - 颜色
Density - 密度
Turbulence - 湍流
```

## 刚体模拟

刚体模拟用于创建碰撞、坠落、堆叠等效果。

### 刚体基础

```python
# 刚体类型
Active - 主动刚体
受力影响
可运动

Passive - 被动刚体
不受力影响
固定不动

# 刚体形状
Mesh - 网格形状
Primitive - 基本形状
Convex Hull - 凸包
Compound - 复合形状
```

### 刚体属性

```python
# 物理属性
Mass - 质量
Friction - 摩擦力
Restitution - 弹性

# 运动学
Animation - 动画控制
Kinematic - 运动学
Deactivation - 失活
```

### 刚体约束

```python
# 约束类型
Fixed - 固定约束
Hinge - 铰链约束
Slider - 滑块约束
Motor - 电机约束
Generic - 通用约束
Generic Spring - 通用弹簧
Point - 点约束

# 约束参数
Limits - 限制
Motor - 电机
Spring - 弹簧
```

### 刚体世界

```python
# 世界设置
Gravity - 重力
Time Scale - 时间缩放
Steps per Second - 每秒步数
Solver - 求解器

# 碰撞检测
Collision Margin - 碰撞边距
Collision Collection - 碰撞集合
```

## 动态绘画

动态绘画允许使用对象作为画笔在另一对象上绘画，绘画影响物理模拟。

### 动态绘画基础

```python
# 画笔
在表面上绘画
影响画布属性
控制绘画效果

# 画布
接收绘画的表面
存储绘画数据
影响后续效果
```

### 画笔设置

```python
# 画笔类型
Blur - 模糊
Displace - 置换
Dry - 干燥
Emission - 发射
Paint - 绘画
Smear - 涂抹
Strength - 强度
Velocities - 速度

# 笔刷属性
Radius - 半径
Hardness - 硬度
Opacity - 不透明度
Color - 颜色
```

### 画布设置

```python
# 画布类型
Mesh - 网格画布
Wave - 波浪
Weight - 权重

# 画布属性
Influence - 影响
Surface - 表面
Displace - 置换
Color - 颜色
```

### 动态绘画影响

```python
# 绘画影响
颜色变化
权重变化
几何体变形
影响物理模拟
```

## 力场

力场定义空间中影响其他物理对象的区域。

### 力场类型

```python
# 力场类型
Force - 力场
Vortex - 漩涡
Magnet - 磁场
Wind - 风力
Texture - 纹理力场
Boid - 群行为力场
Harmonic - 谐波力场
Lennard-Jones - 伦纳德-琼斯力场
Charge - 电荷力场
Drag - 阻力
Turbulence - 湍流
Fluid Flow - 流体流动
```

### 力场参数

```python
# 通用参数
Strength - 强度
Falloff - 衰减
Falloff Power - 衰减功率
Radial Falloff - 径向衰减

# 空间参数
Sphere - 球形
Cylinder - 圆柱形
Plane - 平面
Point - 点
Line - 线条

# 方向参数
Z - Z轴
Radial - 径向
Normal - 法向
Tangent - 切向
```

## 碰撞

碰撞处理物理对象之间的交互。

### 碰撞基础

```python
# 碰撞对象
主动碰撞
被动碰撞
碰撞几何体

# 碰撞类型
Mesh - 网格碰撞
Primitive - 基本碰撞
Convex Hull - 凸包
```

### 碰撞参数

```python
# 碰撞设置
Friction - 摩擦力
Restitution - 弹性
Collision Margin - 碰撞边距

# 碰撞组
碰撞组
碰撞掩码
过滤碰撞
```

## 物理模拟最佳实践

### 性能优化

```python
# 网格优化
使用适当的多边形数量
简化碰撞几何体
使用代理对象

# 模拟优化
使用适当的求解器设置
限制模拟范围
使用缓存

# 渲染优化
使用适当的渲染设置
烘焙静态模拟
使用代理几何体
```

### 稳定性和收敛

```python
# 时间步长
适当的时间步长
固定时间步长
自适应时间步长

# 求解器设置
迭代次数
收敛精度
约束求解

# 常见问题解决
穿透问题 - 增加子步
不稳定 - 减少时间步长
爆炸 - 检查碰撞设置
```

### 工作流程

```python
# 模拟设置
1. 准备几何体
2. 添加物理修饰符
3. 设置物理参数
4. 添加碰撞和约束
5. 测试模拟

# 迭代改进
1. 预览低分辨率
2. 调整关键参数
3. 增加分辨率
4. 最终烘焙

# 导出和重用
1. 烘焙模拟
2. 导出缓存
3. 重用设置
```

## 模拟节点

Blender 支持使用几何节点进行物理模拟的节点式方法。

```python
# 模拟节点
Simulation Input - 模拟输入
Simulation Output - 模拟输出
Simulation Zone - 模拟区域

# 物理节点
Rigid Body - 刚体
Cloth - 布料
Fluid - 流体
Particles - 粒子

# 控制节点
Set Position - 设置位置
Set Rotation - 设置旋转
Apply Force - 应用力
```

## 物理模拟缓存

```python
# 缓存类型
Folder - 文件夹
Multifile - 多文件
Object - 对象

# 缓存设置
Start Frame - 开始帧
End Frame - 结束帧
Cache Limit - 缓存限制

# 缓存操作
Bake - 烘焙
Free - 释放
Replay - 重放
```


---
## bpy_modifiers_tools

# Blender 修饰器、雕刻与高级工具指南

本模块涵盖 Blender 修饰器系统、雕刻绘画工具、约束系统和其他高级功能的完整技能，帮助开发者创建复杂的几何处理流程和专业级工具。

## 概述

Blender 提供了强大的非破坏性编辑工具链，包括修饰器堆栈、约束系统、雕刻工具和物理模拟。理解这些系统的程序化操作对于开发高级插件和自动化工作流程至关重要。

**核心概念：**
- 修饰器类型和应用顺序
- 雕刻模式与笔刷系统
- 约束类型和目标控制
- 物理模拟配置
- 变换操作和坐标系统

## 核心功能

### 修饰器系统

修饰器是一种非破坏性的几何处理方式，可以堆叠应用。

**修饰器类型：**

```python
import bpy

# 修饰器类型映射
MODIFIER_TYPES = {
    'ARRAY': '阵列',
    'BEVEL': '倒角',
    'BOOLEAN': '布尔',
    'BUILD': '构建',
    'CAST': '投射',
    'CLOTH': '布料',
    'COLLISION': '碰撞',
    'CORRECTIVE_SMOOTH': '校正平滑',
    'CURVE': '曲线',
    'DECIMATE': '精简',
    'DISPLACE': '置换',
    'DYNAMIC_PAINT': '动态绘画',
    'EDGE_SPLIT': '边分割',
    'EMISSION': '发射',
    'FLUID': '流体',
    'GREASE_PENCIL': '涂鸦铅笔',
    'HOOK': '钩子',
    'INSTANCE': '实例',
    'LAPLACIANSMOOTH': '拉普拉斯平滑',
    'LATTICE': '晶格',
    'LEVEL_OF_DETAIL': '细节层次',
    'MASK': '遮罩',
    'MESH_CACHE': '网格缓存',
    'MESH_SEQUENCE_CACHE': '网格序列缓存',
    'MIRROR': '镜像',
    'MULTIRES': '多级细分',
    'NORMAL_EDIT': '法线编辑',
    'NORMALISE': '标准化',
    'OBJECT_ATTRACTS': '对象吸引',
    'OCEAN': '海洋',
    'PARTICLE_INSTANCE': '粒子实例',
    'PARTICLE_SYSTEM': '粒子系统',
    'REMESH': '重网格化',
    'SCREW': '螺旋',
    'SIMPLE_DEFORM': '简单形变',
    'SMOOTH': '平滑',
    'SOFT_BODY': '软体',
    'SOLIDIFY': '加厚',
    'SUBSURF': '细分表面',
    'SURFACE': '表面',
    'SURFACE_DEFORM': '表面形变',
    'UV_PROJECT': 'UV 投影',
    'VERTEX_WEIGHT_EDIT': '顶点权重编辑',
    'VERTEX_WEIGHT_MIX': '顶点权重混合',
    'VERTEX_WEIGHT_PROXIMITY': '顶点权重接近',
    'WIREFRAME': '线框',
    'WILD_CARVE': '野性雕刻',
}
```

**添加和应用修饰器：**

```python
def add_modifier(obj, modifier_type, name=None):
    """为对象添加修饰器"""
    modifier = obj.modifiers.new(
        name=name or f"{modifier_type}_Modifier",
        type=modifier_type
    )
    return modifier

def apply_modifier(obj, modifier_name):
    """应用修饰器（转换为实际几何体）"""
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.modifier_apply(modifier=modifier_name)

def apply_all_modifiers(obj):
    """应用所有修饰器"""
    # 从上到下应用
    for modifier in list(obj.modifiers):
        apply_modifier(obj, modifier.name)

def remove_modifier(obj, modifier_name):
    """移除修饰器"""
    modifier = obj.modifiers.get(modifier_name)
    if modifier:
        obj.modifiers.remove(modifier)

def copy_modifiers(source_obj, target_obj):
    """复制修饰器到另一对象"""
    # 清除目标对象的现有修饰器
    for modifier in list(target_obj.modifiers):
        target_obj.modifiers.remove(modifier)
    
    # 复制修饰器
    for modifier in source_obj.modifiers:
        new_mod = target_obj.modifiers.new(
            name=modifier.name,
            type=modifier.type
        )
        # 复制属性
        copy_modifier_properties(modifier, new_mod)
```

**常用修饰器配置：**

```python
# 细分表面修饰器
def setup_subdivision(obj, levels=2, render_levels=2):
    """设置细分表面修饰器"""
    subsurf = add_modifier(obj, 'SUBSURF', 'Subdivision')
    subsurf.levels = levels
    subsurf.render_levels = render_levels
    subsurf.subdivision_type = 'CATMULL_CLARK'  # CATMULL_CLARK, SIMPLE
    subsurf.use_subsurf_uv = True
    subsurf.use_openvr = True
    return subsurf

# 镜像修饰器
def setup_mirror(obj, axis={'X'}, merge_threshold=0.01):
    """设置镜像修饰器"""
    mirror = add_modifier(obj, 'MIRROR', 'Mirror')
    mirror.use_axis = {
        'X': 'X' in axis,
        'Y': 'Y' in axis,
        'Z': 'Z' in axis
    }
    mirror.use_mirror_merge = True
    mirror.merge_threshold = merge_threshold
    mirror.clip_center = True
    return mirror

# 布尔修饰器
def setup_boolean(obj, target, operation='DIFFERENCE'):
    """设置布尔修饰器"""
    bool_mod = add_modifier(obj, 'BOOLEAN', 'Boolean')
    bool_mod.object = target
    bool_mod.operation = operation  # DIFFERENCE, UNION, INTERsect
    bool_mod.solver = 'FAST'  # FAST, EXACT
    return bool_mod

# 阵列修饰器
def setup_array(obj, count=5, offset=(2.0, 0.0, 0.0), relative_offset=(1.0, 0.0, 0.0)):
    """设置阵列修饰器"""
    array = add_modifier(obj, 'ARRAY', 'Array')
    array.count = count
    array.use_relative_offset = True
    array.relative_offset_displace = relative_offset
    array.use_constant_offset = False
    array.constant_offset_displace = offset
    array.use_object_offset = False
    return array

# 倒角修饰器
def setup_bevel(obj, width=0.1, segments=3, limit_method='ANGLE'):
    """设置倒角修饰器"""
    bevel = add_modifier(obj, 'BEVEL', 'Bevel')
    bevel.width = width
    bevel.segments = segments
    bevel.limit_method = limit_method  # NONE, ANGLE, WEIGHT
    bevel.angle_limit = 1.0  # 弧度
    bevel.use_clamp_overlap = True
    bevel.miter_outer = 'MITER'  # MITER, SHARP, BEVEL
    return bevel

# 加厚修饰器
def setup_solidify(obj, thickness=0.1, offset=0):
    """设置加厚修饰器"""
    solidify = add_modifier(obj, 'SOLIDIFY', 'Solidify')
    solidify.thickness = thickness
    solidify.thickness_clamp = 0.1
    solidify.offset = offset
    solidify.use_even_offset = True
    solidify.use_relative_offset = True
    solidify.use_flip_normals = False
    solidify.material_offset = 0
    return solidify

# 线框修饰器
def setup_wireframe(obj, thickness=0.05, use_replace=True):
    """设置线框修饰器"""
    wireframe = add_modifier(obj, 'WIREFRAME', 'Wireframe')
    wireframe.thickness = thickness
    wireframe.use_replace = use_replace
    wireframe.use_boundary = True
    wireframe.use_even_offset = True
    wireframe.use_relative_offset = True
    wireframe.material_offset = 0
    return wireframe

# 置换修饰器
def setup_displace(obj, texture, strength=1.0):
    """设置置换修饰器"""
    displace = add_modifier(obj, 'DISPLACE', 'Displace')
    displace.texture = texture
    displace.strength = strength
    displace.mid_level = 0.5
    displace.direction = 'NORMAL'  # NORMAL, X, Y, Z, RGB
    return displace

# 简单形变修饰器
def setup_simple_deform(obj, mode='TWIST', angle=1.57, axis='Z'):
    """设置简单形变修饰器"""
    deform = add_modifier(obj, 'SIMPLE_DEFORM', 'SimpleDeform')
    deform.deform_method = mode  # TWIST, BEND, STRETCH, TAPER
    deform.angle = angle
    deform.deform_axis = axis
    deform.limits = (0.0, 1.0)
    return deform

# 精简修饰器
def setup_decimate(obj, ratio=0.5, triangulate=True):
    """设置精简修饰器"""
    decimate = add_modifier(obj, 'DECIMATE', 'Decimate')
    decimate.ratio = ratio
    decimate.decimate_type = 'COLLAPSE'  # COLLAPSE, UNSUBDIV, DIST
    decimate.use_triangulate = triangulate
    decimate.delimit = {'NORMAL', 'UV'}  # 边界
    return decimate
```

### 约束系统

约束用于控制对象的变换和运动关系。

**约束类型：**

```python
CONSTRAINT_TYPES = {
    'CAMERA_SOLVER': '相机求解',
    'COPY_LOCATION': '复制位置',
    'COPY_ROTATION': '复制旋转',
    'COPY_TRANSFORMS': '复制变换',
    'DAMPED_TRACK': '阻尼跟踪',
    'IK': '反向动力学',
    'LIMIT_DISTANCE': '限制距离',
    'LIMIT_LOCATION': '限制位置',
    'LIMIT_ROTATION': '限制旋转',
    'LIMIT_SCALE': '限制缩放',
    'MAINTAIN_VOLUME': '保持体积',
    'NORMAL_DIRECTION': '法线方向',
    'PYTHON': 'Python',
    'SPLINE_IK': '样条线 IK',
    'STRETCH_TO': '拉伸到',
    'TRACK_TO': '跟踪到',
    'TRANSFORM': '变换',
    'TRANSFORM_CACHE': '变换缓存',
    'clamp_to': '夹紧到',
}
```

**约束基本操作：**

```python
def add_constraint(obj, constraint_type, name=None):
    """添加约束"""
    constraint = obj.constraints.new(
        name=name or f"{constraint_type}_Constraint",
        type=constraint_type
    )
    return constraint

def remove_constraint(obj, constraint_name):
    """移除约束"""
    constraint = obj.constraints.get(constraint_name)
    if constraint:
        obj.constraints.remove(constraint)

def copy_constraints(source_obj, target_obj):
    """复制约束"""
    for constraint in source_obj.constraints:
        new_constraint = target_obj.constraints.new(type=constraint.type)
        new_constraint.name = constraint.name
        copy_constraint_properties(constraint, new_constraint)
```

**常用约束配置：**

```python
# 复制位置约束
def setup_copy_location(obj, target, space='WORLD', invert=False):
    """设置复制位置约束"""
    constraint = add_constraint(obj, 'COPY_LOCATION', 'CopyLocation')
    constraint.target = target
    constraint.subtarget = ''
    constraint.use_x = not invert
    constraint.use_y = not invert
    constraint.use_z = not invert
    constraint.invert_x = invert
    constraint.invert_y = invert
    constraint.invert_z = invert
    constraint.target_space = space  # WORLD, POSE, LOCAL_WITH_PARENT, LOCAL
    constraint.owner_space = space
    return constraint

# 复制旋转约束
def setup_copy_rotation(obj, target, space='WORLD'):
    """设置复制旋转约束"""
    constraint = add_constraint(obj, 'COPY_ROTATION', 'CopyRotation')
    constraint.target = target
    constraint.use_x = True
    constraint.use_y = True
    constraint.use_z = True
    constraint.target_space = space
    constraint.owner_space = space
    return constraint

# 复制变换约束
def setup_copy_transforms(obj, target, space='WORLD'):
    """设置复制变换约束"""
    constraint = add_constraint(obj, 'COPY_TRANSFORMS', 'CopyTransforms')
    constraint.target = target
    constraint.target_space = space
    constraint.owner_space = space
    return constraint

# 跟踪约束
def setup_track_to(obj, target, track_axis='TRACK_NEGATIVE_Z', up_axis='Y'):
    """设置跟踪约束"""
    constraint = add_constraint(obj, 'TRACK_TO', 'TrackTo')
    constraint.target = target
    constraint.track_axis = track_axis  # TRACK_X, TRACK_Y, TRACK_Z, TRACK_-X, TRACK_-Y, TRACK_-Z
    constraint.up_axis = up_axis  # X, Y, Z
    return constraint

# 阻尼跟踪约束
def setup_damped_track(obj, target, track_axis='TRACK_Z'):
    """设置阻尼跟踪约束"""
    constraint = add_constraint(obj, 'DAMPED_TRACK', 'DampedTrack')
    constraint.target = target
    constraint.track_axis = track_axis
    return constraint

# 拉伸到约束
def setup_stretch_to(obj, target, bulge=1.0):
    """设置拉伸到约束"""
    constraint = add_constraint(obj, 'STRETCH_TO', 'StretchTo')
    constraint.target = target
    constraint.rest_length = obj.location.length
    constraint.bulge = bulge
    constraint.use_bulge_min = False
    constraint.use_bulge_max = False
    return constraint

# 限制变换约束
def setup_limit_transform(obj, location=(0, 0, 0), rotation=(0, 0, 0), scale=(1, 1, 1)):
    """设置限制变换约束"""
    constraint = add_constraint(obj, 'TRANSFORM', 'LimitTransform')
    
    # 位置限制
    constraint.min_x = location[0]
    constraint.max_x = location[0]
    constraint.min_y = location[1]
    constraint.max_y = location[1]
    constraint.min_z = location[2]
    constraint.max_z = location[2]
    
    # 旋转限制（弧度）
    from math import radians
    constraint.min_angles_x = radians(rotation[0])
    constraint.max_angles_x = radians(rotation[0])
    constraint.min_angles_y = radians(rotation[1])
    constraint.max_angles_y = radians(rotation[1])
    constraint.min_angles_z = radians(rotation[2])
    constraint.max_angles_z = radians(rotation[2])
    
    # 缩放限制
    constraint.min_scale_x = scale[0]
    constraint.max_scale_x = scale[0]
    constraint.min_scale_y = scale[1]
    constraint.max_scale_y = scale[1]
    constraint.min_scale_z = scale[2]
    constraint.max_scale_z = scale[2]
    
    return constraint

# 反向动力学约束
def setup_ik(obj, target, chain_count=2, pole_target=None, pole_angle=0):
    """设置 IK 约束"""
    constraint = add_constraint(obj, 'IK', 'IK')
    constraint.target = target
    constraint.subtarget = ''
    constraint.pole_target = pole_target
    constraint.pole_subtarget = ''
    constraint.chain_count = chain_count
    constraint.use_tail = True
    constraint.ik_type = 'COPY_POSE'  # COPY_POSE, DISTANCE
    constraint.ik_angle = pole_angle
    return constraint

# 样条线 IK 约束
def setup_spline_ik(obj, target, chain_count=4):
    """设置样条线 IK 约束"""
    constraint = add_constraint(obj, 'SPLINE_IK', 'SplineIK')
    constraint.target = target
    constraint.chain_count = chain_count
    constraint.use_even_divergence = True
    constraint.use_stretch = True
    constraint.bulge = 1.0
    return constraint
```

### 雕刻和绘画

Blender 的雕刻模式提供了丰富的笔刷和工具。

**雕刻模式基本操作：**

```python
# 进入雕刻模式
bpy.ops.object.mode_set(mode='SCULPT')

# 获取雕刻工具设置
tool_settings = bpy.context.scene.tool_settings

# 笔刷设置
brush = tool_settings.sculpt.brush
brush.size = 50  # 笔刷大小
brush.strength = 1.0  # 笔刷强度
brush.auto_smooth_factor = 0.5  # 自动平滑因子

# 笔刷类型
SCULPT_BRUSHES = {
    'DRAW': '绘制',
    'DRAW_SHARP': '锐利绘制',
    'CLAW': '爪形',
    'CLAY': '黏土',
    'CLAY_STRIPS': '黏土条',
    'CREASE': '折痕',
    'ELASTIC_DEFORM': '弹性形变',
    'FILLET': '圆角',
    'FLATTEN': '扁平化',
    'GRAB': '抓取',
    'INFLATE': '充气',
    'LAYER': '层',
    'MASK': '遮罩',
    'NUDGE': '推挤',
    'PINCH': '捏合',
    'ROTATE': '旋转',
    'SAND': '沙化',
    'SMOOTH': '平滑',
    'SNAKE_HOOK': '蛇形钩',
    'THUMB': '拇指',
}
```

**笔刷配置函数：**

```python
def set_sculpt_brush(brush_name, size=50, strength=1.0, auto_smooth=0.0):
    """设置雕刻笔刷参数"""
    tool_settings = bpy.context.scene.tool_settings
    brush = tool_settings.sculpt.brush
    
    brush.name = brush_name
    brush.size = size
    brush.strength = strength
    brush.auto_smooth_factor = auto_smooth
    
    return brush

def set_brush_color(color):
    """设置绘制颜色（用于顶点绘制）"""
    tool_settings = bpy.context.scene.tool_settings
    brush = tool_settings.vertex_paint.brush
    brush.color = color
    brush.strength = 1.0

def set_brush_weight(weight):
    """设置权重绘制权重"""
    tool_settings = bpy.context.scene.tool_settings
    brush = tool_settings.weight_paint.brush
    brush.weight = weight
    brush.strength = 1.0

def set_sculpt_symmetry(axis_enable={'X', 'Y', 'Z'}):
    """设置雕刻对称轴"""
    tool_settings = bpy.context.scene.tool_settings
    sculpt = tool_settings.sculpt
    
    sculpt.use_symmetry_x = 'X' in axis_enable
    sculpt.use_symmetry_y = 'Y' in axis_enable
    sculpt.use_symmetry_z = 'Z' in axis_enable
    sculpt.radial_symmetry = 1
    sculpt.symmetry_path = 'NEGATIVE'  # POSITIVE, NEGATIVE, both
```

**遮罩操作：**

```python
def invert_mask():
    """反转遮罩"""
    bpy.ops.paint.mask_invert()

def clear_mask():
    """清除遮罩"""
    bpy.ops.paint.mask_clear(action='ALL')

def mask_all():
    """遮罩全部"""
    bpy.ops.paint.mask_filter(mask_value=1.0)

def fill_mask():
    """填充遮罩"""
    bpy.ops.paint.mask_fill()

def grow_mask(iterations=1):
    """扩大遮罩"""
    for _ in range(iterations):
        bpy.ops.paint.mask_grow()

def shrink_mask(iterations=1):
    """缩小遮罩"""
    for _ in range(iterations):
        bpy.ops.paint.mask_shrink()
```

### 顶点权重

顶点权重在绑定和修饰器中广泛使用。

```python
# 顶点组操作
def create_vertex_group(obj, name):
    """创建顶点组"""
    return obj.vertex_groups.new(name=name)

def delete_vertex_group(obj, name):
    """删除顶点组"""
    vg = obj.vertex_groups.get(name)
    if vg:
        obj.vertex_groups.remove(vg)

def assign_weight(obj, vertex_group, weight, vertices):
    """为顶点分配权重"""
    vg = obj.vertex_groups.get(vertex_group)
    if vg:
        for v in vertices:
            vg.add([v.index], weight, 'REPLACE')

def set_vertex_weights_from_distance(obj, vertex_group, target_obj, max_distance=1.0):
    """根据距离设置顶点权重"""
    import math
    
    vg = obj.vertex_groups.get(vertex_group)
    if not vg:
        vg = create_vertex_group(obj, vertex_group)
    
    mesh = obj.data
    
    for v in mesh.vertices:
        dist = (v.co - target_obj.location).length
        weight = max(0.0, 1.0 - dist / max_distance)
        vg.add([v.index], weight, 'REPLACE')

def smooth_weights(obj, vertex_group, iterations=3):
    """平滑顶点权重"""
    bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
    
    # 使用权重工具
    bpy.ops.paint.weight_smooth(factor=0.5, repeat=iterations)

def blend_weights(obj, vertex_group, target_group, blend_factor=0.5):
    """混合两组权重"""
    bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
    
    # 选择源和目标组
    obj.vertex_groups.active_index = obj.vertex_groups[vertex_group].index
    obj.vertex_groups[vertex_group].select = True
    obj.vertex_groups[target_group].select = True
```

### 物理模拟

Blender 支持多种物理模拟类型。

**物理模拟类型：**

```python
PHYSICS_TYPES = {
    'CLOTH': '布料',
    'SOFT_BODY': '软体',
    'RIGID_BODY': '刚体',
    'DYNAMIC_PAINT': '动态绘画',
    'FLUID': '流体',
    'PARTICLE_SYSTEM': '粒子系统',
    'COLLISION': '碰撞',
}
```

**刚体物理：**

```python
def setup_rigid_body(obj, type='ACTIVE', mass=1.0):
    """设置刚体"""
    bpy.ops.object.rigid_body_add()
    rigid_body = obj.rigid_body
    
    rigid_body.type = type  # ACTIVE, PASSIVE
    rigid_body.mass = mass
    
    return rigid_body

def setup_rigid_body_constraint(obj_a, obj_b, type='GENERIC', pivot='CENTER'):
    """设置刚体约束"""
    constraint = bpy.ops.object.rigid_body_constraint_add()
    rigid_constraint = constraint
    
    rigid_constraint.type = type  # GENERIC, GENERIC_SPRING, POINT, HINGE, SLIDER, PISTON
    rigid_constraint.object1 = obj_a
    rigid_constraint.object2 = obj_b
    
    return rigid_constraint

def convert_to_rigid_bodies(collection):
    """将集合中的对象转换为刚体"""
    for obj in collection.objects:
        if obj.type in ['MESH', 'CURVE', 'FONT']:
            setup_rigid_body(obj, type='ACTIVE')
```

**布料模拟：**

```python
def setup_cloth(obj, presets='DEFAULT'):
    """设置布料"""
    bpy.ops.object.modifier_add(type='CLOTH')
    cloth = obj.modifiers[-1]
    
    # 预设配置
    presets = {
        'DEFAULT': {},
        'COTTON': {'mass': 0.3, 'shear': 0.5},
        'SILK': {'mass': 0.1, 'shear': 0.1},
        'LEATHER': {'mass': 0.8, 'shear': 0.8},
        'DENIM': {'mass': 0.5, 'shear': 0.3},
    }
    
    if presets:
        settings = presets.get(presets, {})
        for key, value in settings.items():
            setattr(cloth.settings, key, value)
    
    return cloth

def pin_cloth_vertices(obj, vertex_group_name):
    """固定布料顶点"""
    cloth = obj.modifiers[-1]
    vg = obj.vertex_groups.get(vertex_group_name)
    
    if vg and cloth:
        cloth.settings.vertex_group_mass = vg.name
        cloth.settings.pin_stiffness = 1.0
```

**粒子系统：**

```python
def add_particle_system(obj, name='ParticleSystem'):
    """添加粒子系统"""
    bpy.ops.object.particle_system_add()
    particle_system = obj.particle_systems[-1]
    particle_system.name = name
    return particle_system

def setup_emission(obj, count=1000, lifetime=50, size=0.1):
    """设置发射器"""
    particle_system = add_particle_system(obj)
    settings = particle_system.settings
    
    settings.type = 'EMITTER'
    settings.count = count
    settings.lifetime = lifetime
    settings.frame_start = 1
    settings.frame_end = 100
    settings.lifetime_random = 0.2
    settings.size = size
    settings.size_random = 0.1
    
    # 发射源设置
    settings.emit_from = 'FACE'  # VERT, FACE, VOLUME
    settings.distribution = 'RAND'  # RAND, JITTER, GRID
    
    return settings

def setup_hair(obj, length=1.0, density=10):
    """设置毛发"""
    particle_system = add_particle_system(obj)
    settings = particle_system.settings
    
    settings.type = 'HAIR'
    settings.hair_length = length
    settings.hair_density = density
    settings.clump_factor = 0.0
    settings.roughness_end = 0.2
    
    return settings

def convert_particles_to_mesh(obj, particle_system_name, target_name):
    """将粒子转换为网格"""
    bpy.ops.object.particle_system_convert(
        object=obj,
        particle_system=particle_system_name
    )
```

### 变换操作

对象的变换操作包括位置、旋转和缩放。

```python
def reset_transform(obj):
    """重置变换"""
    obj.location = (0, 0, 0)
    obj.rotation_euler = (0, 0, 0)
    obj.scale = (1, 1, 1)

def set_location(obj, x=0, y=0, z=0):
    """设置位置"""
    obj.location = (x, y, z)

def set_rotation(obj, euler=(0, 0, 0), order='XYZ'):
    """设置旋转（欧拉角）"""
    obj.rotation_mode = order
    obj.rotation_euler = euler

def set_rotation_degrees(obj, x=0, y=0, z=0, order='XYZ'):
    """设置旋转（角度）"""
    import math
    obj.rotation_mode = order
    obj.rotation_euler = (math.radians(x), math.radians(y), math.radians(z))

def set_rotation_quaternion(obj, w=1, x=0, y=0, z=0):
    """设置旋转（四元数）"""
    obj.rotation_mode = 'QUATERNION'
    obj.rotation_quaternion = (w, x, y, z)

def set_scale(obj, x=1, y=1, z=1):
    """设置缩放"""
    obj.scale = (x, y, z)

def set_uniform_scale(obj, factor):
    """设置统一缩放"""
    obj.scale = (factor, factor, factor)

def apply_transform(obj, location=True, rotation=True, scale=True):
    """应用变换"""
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.transform_apply(
        location=location,
        rotation=rotation,
        scale=scale
    )
```

### 坐标空间转换

```python
def world_to_local(obj, world_vector):
    """世界坐标转局部坐标"""
    return obj.matrix_world.inverted() @ world_vector

def local_to_world(obj, local_vector):
    """局部坐标转世界坐标"""
    return obj.matrix_world @ local_vector

def world_to_object(obj, target):
    """获取对象在另一对象空间中的位置"""
    inv_matrix = target.matrix_world.inverted()
    return inv_matrix @ obj.location

def get_global_position(obj):
    """获取对象的全局位置"""
    return obj.matrix_world.translation

def get_global_rotation(obj):
    """获取对象的全局旋转"""
    return obj.matrix_world.to_euler()

def get_global_scale(obj):
    """获取对象的全局缩放"""
    return obj.matrix_world.to_scale()
```

## 实用函数

### 修饰器工具函数

```python
def copy_modifier_properties(source, target):
    """复制修饰器属性"""
    # 获取源修饰器的所有属性
    for attr in dir(source):
        if attr.startswith('_') or attr in ['bl_rna', 'rna_type']:
            continue
        try:
            value = getattr(source, attr)
            if not callable(value):
                setattr(target, attr, value)
        except (AttributeError, TypeError):
            pass

def get_modifier_stack(obj):
    """获取修饰器栈"""
    return [mod.name for mod in obj.modifiers]

def reorder_modifiers(obj, new_order):
    """重新排序修饰器"""
    for i, mod_name in enumerate(new_order):
        mod = obj.modifiers.get(mod_name)
        if mod:
            for _ in range(i):
                bpy.ops.object.modifier_move_down(modifier=mod.name)

def get_modifier_by_type(obj, modifier_type):
    """获取指定类型的修饰器"""
    for mod in obj.modifiers:
        if mod.type == modifier_type:
            return mod
    return None

def has_modifier_type(obj, modifier_type):
    """检查是否有指定类型的修饰器"""
    return get_modifier_by_type(obj, modifier_type) is not None
```

### 约束工具函数

```python
def copy_constraint_properties(source, target):
    """复制约束属性"""
    for attr in dir(source):
        if attr.startswith('_') or attr in ['bl_rna', 'rna_type', 'type']:
            continue
        try:
            value = getattr(source, attr)
            if not callable(value):
                setattr(target, attr, value)
        except (AttributeError, TypeError):
            pass

def get_constraint_stack(obj):
    """获取约束栈"""
    return [con.name for con in obj.constraints]

def enable_constraint(obj, constraint_name, enabled=True):
    """启用或禁用约束"""
    constraint = obj.constraints.get(constraint_name)
    if constraint:
        constraint.mute = not enabled

def get_constraints_by_type(obj, constraint_type):
    """获取指定类型的约束"""
    return [con for con in obj.constraints if con.type == constraint_type]

def set_constraint_influence(obj, constraint_name, influence):
    """设置约束影响度"""
    constraint = obj.constraints.get(constraint_name)
    if constraint:
        constraint.influence = influence
```

### 几何处理函数

```python
def get_vertex_count(obj):
    """获取顶点数"""
    if obj.type == 'MESH':
        return len(obj.data.vertices)
    return 0

def get_edge_count(obj):
    """获取边数"""
    if obj.type == 'MESH':
        return len(obj.data.edges)
    return 0

def get_face_count(obj):
    """获取面数"""
    if obj.type == 'MESH':
        return len(obj.data.polygons)
    return 0

def get_center_of_mass(obj):
    """获取质心"""
    if obj.type == 'MESH':
        mesh = obj.data
        total = sum((v.co for v in mesh.vertices), Vector())
        return total / len(mesh.vertices)
    return Vector()

def get_bounding_box(obj):
    """获取包围盒"""
    if obj.type == 'MESH':
        mesh = obj.data
        min_x = min(v.co.x for v in mesh.vertices)
        min_y = min(v.co.y for v in mesh.vertices)
        min_z = min(v.co.z for v in mesh.vertices)
        max_x = max(v.co.x for v in mesh.vertices)
        max_y = max(v.co.y for v in mesh.vertices)
        max_z = max(v.co.z for v in mesh.vertices)
        return ((min_x, min_y, min_z), (max_x, max_y, max_z))
    return ((0, 0, 0), (0, 0, 0))

def get_surface_area(obj):
    """获取表面积"""
    if obj.type == 'MESH':
        import bmesh
        bm = bmesh.new()
        bm.from_mesh(obj.data)
        area = sum(f.calc_area() for f in bm.faces)
        bm.free()
        return area
    return 0

def get_volume(obj):
    """获取体积"""
    if obj.type == 'MESH':
        import bmesh
        bm = bmesh.new()
        bm.from_mesh(obj.data)
        volume = bm.calc_volume()
        bm.free()
        return volume
    return 0
```

### 批处理函数

```python
def batch_add_modifiers(objects, modifier_type, **kwargs):
    """批量添加修饰器"""
    results = []
    for obj in objects:
        try:
            mod = add_modifier(obj, modifier_type, **kwargs)
            results.append((obj, 'success', mod))
        except Exception as e:
            results.append((obj, 'error', str(e)))
    return results

def batch_add_constraints(objects, constraint_type, **kwargs):
    """批量添加约束"""
    results = []
    for obj in objects:
        try:
            con = add_constraint(obj, constraint_type, **kwargs)
            results.append((obj, 'success', con))
        except Exception as e:
            results.append((obj, 'error', str(e)))
    return results

def batch_reset_transform(objects):
    """批量重置变换"""
    for obj in objects:
        reset_transform(obj)

def batch_apply_modifiers(objects):
    """批量应用修饰器"""
    for obj in objects:
        apply_all_modifiers(obj)
```

## 常见问题排查

### 问题：修饰器不生效

**可能原因：**
1. 修饰器被禁用
2. 对象类型不支持该修饰器
3. 几何体不足

**解决方案：**

```python
# 检查修饰器是否启用
def check_modifier_enabled(obj, mod_name):
    mod = obj.modifiers.get(mod_name)
    if mod:
        if mod.mute:
            mod.mute = False  # 启用
        if not mod.show_viewport:
            mod.show_viewport = True
        if not mod.show_render:
            mod.show_render = True
        return True
    return False

# 检查对象类型
def check_modifier_compatibility(obj, mod_type):
    if mod_type in ['SUBSURF', 'DECIMATE', 'REMESH']:
        return obj.type == 'MESH'
    if mod_type in ['ARRAY', 'BEVEL', 'SOLIDIFY']:
        return obj.type in ['MESH', 'CURVE']
    return True
```

### 问题：约束导致循环依赖

**解决方案：**

```python
def check_constraint_cycles(obj):
    """检查约束是否造成循环依赖"""
    checked = set()
    
    def traverse(current, path):
        if current in path:
            return True
        if current in checked:
            return False
        
        path.add(current)
        checked.add(current)
        
        for constraint in current.constraints:
            if constraint.type == 'PYTHON':
                continue
            target = getattr(constraint, 'target', None)
            if target:
                if traverse(target, path.copy()):
                    return True
        
        return False
    
    return traverse(obj, set())

def resolve_constraint_cycles(obj):
    """解决约束循环"""
    for constraint in obj.constraints:
        if constraint.type in ['COPY_LOCATION', 'COPY_ROTATION', 'COPY_TRANSFORMS']:
            if constraint.target == obj:
                constraint.target = None
```

### 问题：物理模拟不稳定

**解决方案：**

```python
def optimize_cloth_simulation(obj):
    """优化布料模拟稳定性"""
    cloth = obj.modifiers[-1]
    settings = cloth.settings
    
    # 增加质量减少抖动
    settings.mass = max(settings.mass, 0.1)
    
    # 增加结构刚度
    settings.spring_k = 100.0
    settings.shear_k = 50.0
    
    # 增加迭代次数
    settings.solver_iterations = 10
    
    # 启用子步
    settings.substeps = 3

def optimize_rigid_body_simulation(objects):
    """优化刚体模拟"""
    for obj in objects:
        if obj.rigid_body:
            # 增加质量
            obj.rigid_body.mass = max(obj.rigid_body.mass, 0.1)
            
            # 减少碰撞容差
            obj.rigid_body.collision_margin = 0.01
            
            # 启用连续碰撞检测
            obj.rigid_body.use_ccd = True
```

### 问题：雕刻笔刷不工作

**解决方案：**

```python
def check_sculpt_mode():
    """检查雕刻模式"""
    obj = bpy.context.active_object
    if not obj:
        print("没有活动对象")
        return False
    
    if bpy.context.mode != 'SCULPT':
        print(f"当前模式: {bpy.context.mode}，需要雕刻模式")
        return False
    
    if obj.type != 'MESH':
        print(f"对象类型: {obj.type}，需要网格类型")
        return False
    
    return True

def reset_brush_settings():
    """重置笔刷设置"""
    tool_settings = bpy.context.scene.tool_settings
    brush = tool_settings.sculpt.brush
    
    brush.size = 50
    brush.strength = 1.0
    brush.auto_smooth_factor = 0.0
    brush.use_pressure_strength = False
    brush.use_pressure_size = False
    
    # 重置雕刻对称
    tool_settings.sculpt.use_symmetry_x = False
    tool_settings.sculpt.use_symmetry_y = False
    tool_settings.sculpt.use_symmetry_z = False
```

### 问题：顶点权重不正确

**解决方案：**

```python
def validate_vertex_weights(obj, vertex_group):
    """验证顶点权重"""
    vg = obj.vertex_groups.get(vertex_group)
    if not vg:
        return False
    
    mesh = obj.data
    for v in mesh.vertices:
        for g in v.groups:
            if g.group == vg.index:
                if g.weight < 0 or g.weight > 1:
                    print(f"顶点 {v.index} 权重无效: {g.weight}")
                    return False
    
    return True

def normalize_vertex_weights(obj, vertex_group):
    """归一化顶点权重"""
    vg = obj.vertex_groups.get(vertex_group)
    if not vg:
        return
    
    mesh = obj.data
    for v in mesh.vertices:
        total_weight = sum(g.weight for g in v.groups)
        if total_weight > 0:
            for g in v.groups:
                if g.group == vg.index:
                    normalized = g.weight / total_weight
                    vg.add([v.index], normalized, 'REPLACE')
```

## 设计最佳实践

### 1. 修饰器栈优化

```python
def optimize_modifier_stack(obj):
    """优化修饰器栈性能"""
    modifiers = list(obj.modifiers)
    
    # 1. 合并连续的同类修饰器
    merged = []
    for mod in modifiers:
        if merged and mod.type == merged[-1].type:
            # 合并逻辑
            pass
        else:
            merged.append(mod)
    
    # 2. 移动成本高的修饰器到最后
    heavy_types = ['SUBSURF', 'REMESH', 'MULTIRES']
    light_mods = [m for m in merged if m.type not in heavy_types]
    heavy_mods = [m for m in merged if m.type in heavy_types]
    
    # 3. 移除不必要的修饰器
    for mod in light_mods + heavy_mods:
        if is_redundant_modifier(mod):
            obj.modifiers.remove(mod)

def is_redundant_modifier(mod):
    """检查修饰器是否冗余"""
    if mod.type == 'SUBSURF' and mod.levels == 0 and mod.render_levels == 0:
        return True
    if mod.type == 'MIRROR' and not any(mod.use_axis.values()):
        return True
    if mod.type == 'ARRAY' and mod.count == 1:
        return True
    return False
```

### 2. 约束链优化

```python
def build_constraint_chain(root_obj, end_effector, constraint_type='COPY_TRANSFORMS'):
    """构建约束链"""
    chain = []
    current = end_effector
    
    while current != root_obj and current:
        parent = current.parent
        if parent:
            constraint = add_constraint(current, constraint_type)
            constraint.target = parent
            constraint.head_tail = 0
            chain.append(constraint)
        current = parent
    
    return chain

def disable_constraint_chain(chain):
    """禁用约束链"""
    for constraint in chain:
        constraint.mute = True
```

### 3. 物理模拟最佳实践

```python
def setup_physical_properties(obj, density=1000, friction=0.5, restitution=0.5):
    """设置物理属性"""
    if not obj.rigid_body:
        bpy.ops.object.rigid_body_add()
    
    obj.rigid_body.mass = obj.dimensions.x * obj.dimensions.y * obj.dimensions.z * density
    obj.rigid_body.friction = friction
    obj.rigid_body.restitution = restitution

def create_physical_protection_zone(center, radius, name="ProtectionZone"):
    """创建物理保护区域"""
    # 创建碰撞对象
    bpy.ops.mesh.primitive_uv_sphere_add(radius=radius, location=center)
    zone = bpy.context.active_object
    zone.name = name
    
    # 设置为被动刚体
    bpy.ops.object.rigid_body_add(type='PASSIVE')
    zone.rigid_body.collision_shape = 'MESH'
    zone.rigid_body.friction = 0.0
    
    return zone
```

### 4. 变形工具最佳实践

```python
def create_blend_shape(obj, name="BlendShape", relative_key=True):
    """创建形状键"""
    if not obj.data.shape_keys:
        obj.shape_key_add(name="Basis")
    
    shape_key = obj.shape_key_add(name=name)
    return shape_key

def update_blend_shape(obj, shape_name, factor):
    """更新形状键权重"""
    if obj.data.shape_keys:
        key_block = obj.data.shape_keys.key_blocks.get(shape_name)
        if key_block:
            key_block.value = factor

def create_morph_target(obj, target_obj, name="MorphTarget"):
    """创建变形目标"""
    # 基于目标对象创建形状键
    shape_key = create_blend_shape(obj, name)
    
    # 复制顶点位置
    for i, v in enumerate(obj.data.vertices):
        if i < len(target_obj.data.vertices):
            shape_key.data[i].co = target_obj.data.vertices[i].co
    
    return shape_key
```

## 高级主题

### 批处理修饰器应用

```python
def batch_apply_modifiers_with_bmesh(objects, modifier_types=None):
    """使用 BMesh 批量应用修饰器"""
    import bmesh
    
    results = []
    
    for obj in objects:
        try:
            # 保存原始网格
            original_mesh = obj.data.copy()
            
            # 临时应用修饰器
            depsgraph = bpy.context.evaluated_depsgraph_get()
            eval_obj = obj.evaluated_get(depsgraph)
            eval_mesh = eval_obj.to_mesh()
            
            # 创建新网格
            new_mesh = bpy.data.meshes.new(obj.name + "_applied")
            new_mesh.from_pydata(eval_mesh.vertices, eval_mesh.edges, eval_mesh.polygons)
            new_mesh.update()
            
            # 清除修饰器
            for mod in list(obj.modifiers):
                if not modifier_types or mod.type in modifier_types:
                    obj.modifiers.remove(mod)
            
            # 替换网格
            obj.data = new_mesh
            
            results.append((obj, 'success'))
            
        except Exception as e:
            results.append((obj, 'error', str(e)))
    
    return results
```

### 约束动画系统

```python
def create_rigging_system(root, end_effector, segments=3):
    """创建 IK 绑定系统"""
    bones = []
    
    # 创建骨骼链
    for i in range(segments):
        bpy.ops.object.armature_add(enter_editmode=True, location=(0, 0, 0))
        armature = bpy.context.object
        
        edit_bones = armature.data.edit_bones
        bones.append(edit_bones[0])
        
        # 设置骨骼长度和位置
        if i > 0:
            bones[i].head = bones[i-1].tail
            bones[i].tail = bones[i].head + Vector((0, 1, 0))
        
        bpy.ops.object.mode_set(mode='OBJECT')
    
    # 添加 IK 约束
    pose_bones = end_effector.pose.bones
    for i, bone in enumerate(bones):
        if i == len(bones) - 1:  # 末端
            ik_constraint = pose_bones[bone.name].constraints.new('IK')
            ik_constraint.target = end_effector
            ik_constraint.chain_count = len(bones) - i
    
    return bones
```

### 动态程序化几何体

```python
def create_procedural_mesh(name, generator_func, *args, **kwargs):
    """创建程序化网格"""
    mesh = bpy.data.meshes.new(name)
    obj = bpy.data.objects.new(name, mesh)
    
    # 添加到场景
    bpy.context.collection.objects.link(obj)
    
    # 生成几何体
    generator_func(mesh, *args, **kwargs)
    
    return obj

def generate_grid_mesh(mesh, size=10, divisions=10):
    """生成网格几何体"""
    from math import pi
    
    vertices = []
    edges = []
    faces = []
    
    step = size / divisions
    
    # 创建顶点
    for z in range(divisions + 1):
        for x in range(divisions + 1):
            vertices.append((x * step - size/2, 0, z * step - size/2))
    
    # 创建边和面
    for z in range(divisions):
        for x in range(divisions):
            v1 = z * (divisions + 1) + x
            v2 = v1 + 1
            v3 = (z + 1) * (divisions + 1) + x + 1
            v4 = v3 - 1
            
            edges.append((v1, v2))
            edges.append((v2, v3))
            edges.append((v3, v4))
            edges.append((v4, v1))
            faces.append((v1, v2, v3, v4))
    
    mesh.from_pydata(vertices, edges, faces)
```

### 自定义笔刷系统

```python
def create_custom_sculpt_brush(name, sculpt_func):
    """创建自定义雕刻笔刷"""
    brush = bpy.data.brushes.new(name)
    brush.sculpt_capabilities.sculpt = True
    
    # 设置笔刷着色器
    brush.color = (1.0, 0.2, 0.2)
    brush.secondary_color = (0.2, 0.2, 1.0)
    
    return brush

def setup_stroke_methods():
    """配置笔画方法"""
    tool_settings = bpy.context.scene.tool_settings
    sculpt = tool_settings.sculpt
    
    # 笔画类型
    sculpt.stroke_method = 'DRAG'  # DRAG, SPACE, CONTINUOUS
    
    # 笔触间隔
    sculpt.stroke_method = 'SPACE'
    sculpt.unified_paint_settings.distance_min = 10
    
    # 随机化
    sculpt.brush.use_rabbit_pressure = False
    sculpt.brush.use_smooth_stroke = False
    sculpt.brush.use_airbrush = True
```


---
## bpy_sculpt_paint

# 雕刻与绘制模块

## 雕刻与绘制概述

雕刻和绘制提供了一种通过画笔进行更自由形式编辑的工作流程。有几种模式可以做到这一点，每种都有其自己的用途。

```python
# 雕刻模式
# 更改和变换网格的拓扑
# 使用画笔更改模型的区域
# 而不是处理单个元素

# 顶点绘制
# 将颜色直接绘制到对象上
# 通过直接操纵顶点的颜色
# 而非纹理

# 权重绘制
# 更改活动顶点组中顶点的权重
# 用于绑定网格
# 控制粒子发射、毛发密度

# 纹理绘制
# 更改活动图像纹理的像素
# 在模型上绘制纹理

# 进入模式
# 从 3D 视口的模式菜单访问
# 或使用 Ctrl-Tab 饼图菜单
```

## 雕刻模式

### 雕刻基础

雕刻模式类似于编辑模式，都用于改变模型的形状，但雕刻模式使用非常不同的工作流程：不处理单个元素（顶点、边和面），而是主要使用画笔更改模型的区域。

```python
# 雕刻模式特点
# 使用画笔而非顶点编辑
# 更自由形式的建模
# 适合有机形状和细节

# 笔刷光标
# 进入雕刻模式后
# 光标将变为圆形
# 指示画笔的大小

# 警告
# 为确保可预测的画笔行为
# 请确保应用网格的缩放

# 工具栏和工具设置
# 进入雕刻模式后
# 3D 视口的工具栏和工具设置
# 将变为雕刻模式特定面板
```

### 动态拓扑雕刻

```python
# 动态拓扑
# 在雕刻时动态添加和移除几何体
# 允许无限细节级别
# 适合有机形状

# 启用动态拓扑
# 在工具设置中启用
# 选择要使用的细分方法
# 确定雕刻分辨率

# 细分级别
# 更高细节级别需要更多内存
# 根据需要平衡细节和性能

# 雕刻分辨率
# 控制雕刻时添加的细节量
# 更高分辨率 = 更多顶点
# 更精细的细节
```

### 细分表面雕刻

```python
# 细分曲面修饰符
# 使用细分曲面修饰符雕刻
# 在基础网格上雕刻
# 动态查看细分结果

# 雕刻多层
# 在不同细分级别雕刻
# 低多边形用于大型形状
# 高多边形用于细节

# 置换绘制
# 在高分辨率上绘制置换
# 使用多分辨率修饰符
# 传递细节到基础网格
```

## 雕刻画笔

### 基础画笔

```python
# 绘制画笔
# 基本推拉画笔
# 沿法线推拉网格
# 用于添加体积和形状

# 绘制尖锐
# 类似于绘制但带有锐利边缘
# 用于创建锐利特征
# 如岩石或机械零件

# 粘土
# 添加类似粘土的材料
# 创建均匀的厚度
# 用于建立基本形状

# 粘土条
# 类似于粘土但形状更窄
# 用于添加长条
# 如肌肉或脊

# 粘土拇指
# 使用拇指动作平滑粘土
# 用于融合和平滑表面
# 去除指纹和不规则
```

### 变形画笔

```python
# 平滑画笔
# 平滑网格表面
# 减少噪点和瑕疵
# 用于平滑过渡

# 捏画笔
# 捏合区域向内
# 创建折痕和皱褶
# 用于锐利特征和边缘

# 膨胀画笔
# 膨胀区域向外
# 增加体积
# 用于添加肉或填充

# 抓住画笔
# 抓住并移动区域
# 类似于拖拽顶点
# 用于拉伸和定位

# 蛇形钩
# 像钩子一样抓住区域
# 用于拉出长特征
# 如头发、角、尾巴

# 弹性变形
# 弹性变形网格
# 拉伸和回弹
# 用于有机形状

# 滑动放松
# 滑动顶点同时放松
# 减少拉伸伪影
# 用于细节雕刻
```

### 高级画笔

```python
# 折痕画笔
# 创建折痕和锐利边缘
# 沿边缘推入
# 用于衣服褶皱或锐利特征

# 层面画笔
# 添加新几何体层
# 不会移动底层网格
# 用于添加细节而不影响形状

# 多平面刮刀
# 使用多个平面刮擦
# 创建平坦表面
# 用于硬表面建模

# 旋转画笔
# 旋转网格区域
# 围绕笔刷中心
# 用于扭曲和卷曲

# 边界画笔
# 沿着边界雕刻
# 创建清晰边缘
# 用于硬表面细节

# 布料画笔
# 模拟布料行为
# 用于创建布料褶皱
# 雕刻服装和布料
```

### 实用画笔

```python
# 遮罩画笔
# 绘制遮罩区域
# 保护区域免受进一步雕刻
# 用于固定某些区域

# 简化画笔
# 减少网格细节
# 移除小特征
# 用于优化和清理

# 绘制面组
# 创建和编辑面组
# 用于组织和隔离区域
```

### 笔刷属性

```python
# 半径
# 笔刷的大小
# 控制影响区域

# 强度
# 笔刷的效果强度
# 控制变形量

# 衰减
# 笔刷效果的衰减曲线
# 控制笔刷边缘的柔和度

# 角度
# 笔刷旋转角度
# 用于控制笔刷方向

# 纹理
# 笔刷的纹理图案
# 控制笔刷如何影响表面

# 着色器
# 笔刷如何变形网格
# 不同着色器用于不同效果
```

## 顶点绘制

### 顶点绘制基础

顶点绘制是一种将颜色直接绘制到对象上的简单方法，直接操纵顶点的颜色而非纹理。顶点绘制将颜色信息存储为颜色属性，可用于不同的渲染引擎。

```python
# 顶点绘制特点
# 直接修改顶点颜色
# 颜色渐变到相邻顶点
# 不修改遮挡面的颜色

# 颜色属性
# 存储为颜色属性数据块
# 可以在材质节点树中使用
# 使用顶点颜色节点

# 视图颜色属性
# 使用工作台渲染引擎
# 在 3D 视图中查看
# 设置为实体着色
# 选择属性颜色选项

# 颜色属性管理
# 在标题中间的调色板弹出菜单中
# 创建和管理颜色属性
```

### 顶点绘制工具

```python
# 绘制
# 在顶点上绘制颜色

# 平滑
# 平滑顶点颜色
# 减少颜色过渡

# 模糊
# 模糊顶点颜色
# 创建柔和过渡

# 混合模式
# 正常 - 正常混合
# 乘法 - 变暗颜色
# 屏幕 - 变亮颜色
# 叠加 - 增强对比度
# 减淡 - 变亮区域
# 加深 - 变暗区域
```

## 权重绘制

### 权重绘制基础

顶点组可能有大量关联的顶点，因此有大量权重（每个分配的顶点一个权重）。权重绘制是一种以非常直观的方式维护大量权重信息的方法。

```python
# 主要用途
# 绑定网格
# 顶点组定义骨骼对网格的相对影响

# 其他用途
# 控制粒子发射
# 控制毛发密度
# 控制修饰符
# 控制形状键

# 权重颜色代码
# 权重可视化使用冷/热颜色系统
# 低值区域（权重接近 0.0）显示为蓝色
# 高值区域（权重接近 1.0）显示为红色
# 中间值显示为彩虹色

# 未引用顶点
# 显示为黑色
# 可以看到引用区域和未引用区域
# 用于查找权重错误
```

### 权重绘制工具

```python
# 绘制
# 在顶点上绘制权重

# 平滑
# 平滑顶点权重
# 在相邻顶点之间混合权重

# 模糊
# 模糊权重值
# 创建柔和过渡

# 添加
# 添加权重到区域

# 减去
# 从区域减去权重

# 乘以
# 乘以权重值
```

### 归一化权重工作流

```python
# 归一化权重
# 用于变形时
# 权重通常必须归一化
# 使得分配给单个顶点的所有变形权重相加为 1
# 骨架修饰符自动执行此操作

# 归一化全部
# 首先对现有权重进行归一化
# 使用归一化全部工具

# 自动归一化
# 启用后自动维护归一化
# 在绘制时保持权重正确

# 顶点组锁定
# 任何顶点组都可以锁定
# 防止对其更改
# 在归一化工作流中有更重要的含义

# 多重绘制
# 将多个选定的骨骼视为一个骨骼
# 绘制操作改变组合权重
# 保留组内的比率
```

## 纹理绘制

### 纹理绘制基础

纹理绘制是一种直接在网格上绘制纹理的方法。它允许您添加细节、更改颜色和创建纹理，而无需外部软件。

```python
# 纹理绘制特点
# 直接在模型上绘制
# 实时预览效果
# 支持图层和混合模式

# 进入纹理绘制
# 对象必须有材质
# 材质必须有图像纹理
# 切换到纹理绘制模式

# 笔刷设置
# 笔刷大小
# 笔刷硬度
# 笔刷不透明度
# 笔刷颜色

# 纹理槽
# 控制绘制纹理的位置
# 在纹理空间中放置纹理
```

### 纹理绘制工具

```python
# 绘制
# 在纹理上绘制颜色

# 平滑
# 平滑纹理颜色
# 减少笔触痕迹

# 模糊
# 模糊纹理像素
# 创建柔和过渡

# 克隆
# 从源区域克隆到目标
# 用于复制细节

# 橡皮擦
# 移除绘制的内容
# 显示底层纹理
```

### 笔刷纹理

```python
# 笔刷纹理
# 定义笔刷如何影响纹理
# 可以是图像或程序纹理

# 纹理混合
# 混合模式定义如何混合颜色
# 正常 - 正常混合
# 乘法 - 变暗
# 屏幕 - 变亮
# 叠加 - 增强对比度
# 减去 - 变暗

# 纹理投射
# 从图像投射纹理
# 使用屏幕投影
# 用于详细工作
```

## 曲线雕刻

### 曲线雕刻基础

曲线雕刻模式允许使用画笔雕刻和编辑曲线对象。它专门用于曲线几何体，提供控制和修改曲线的工具。

```python
# 曲线雕刻画笔
# 添加曲线 - 添加新曲线
# 梳理曲线 - 梳理和定向毛发
# 删除曲线 - 删除曲线
# 密度曲线 - 控制曲线密度
# 生长收缩曲线 - 生长或收缩曲线
# 捏曲线 - 捏合曲线
# 膨胀曲线 - 膨胀曲线
# 选择绘制 - 选择曲线
# 滑动曲线 - 滑动曲线
# 平滑曲线 - 平滑曲线
# 蛇形钩 - 抓取和拉伸曲线
```

### 曲线工具设置

```python
# 曲线设置
# 曲线半径
# 曲线插值
# 曲线分辨率

# 曲线类型
# 贝塞尔曲线
# NURBS 曲线
# 多边形曲线
```

## 雕刻工具

### 常用工具

```python
# 笔刷工具
# 主雕刻工具
# 使用不同画笔变形网格

# 遮罩工具
# 绘制遮罩
# 编辑遮罩
# 反转遮罩
# 清除遮罩

# 面组工具
# 创建面组
# 编辑面组
# 切换面组可见性

# 隐藏工具
# 隐藏选定区域
# 隐藏未选定区域
# 显示隐藏的区域

# 变形工具
# 移动
# 旋转
# 缩放
```

### 筛选工具

```python
# 网格筛选
# 应用筛选到整个网格
# 整体缩放
# 平滑
# 置换

# 颜色筛选
# 应用颜色操作
# 饱和度
# 色调
# 亮度

# 布纹筛选
# 模拟布料行为
# 添加布纹褶皱
# 雕刻服装
```

### 投影工具

```python
# 线条投影
# 从线条投影几何体
# 创建脊和山谷
# 用于硬表面细节

# 修剪工具
# 使用修剪曲线修剪几何体
# 添加或移除材料
# 用于精确切割
```

## 工具设置

### 笔刷设置

```python
# 笔刷属性
# 半径 - 笔刷大小
# 强度 - 效果强度
# 角度 - 笔刷旋转
# 衰减 - 衰减曲线

# 高级设置
# 压力影响半径
# 压力影响强度
# 随机化

# 着色器设置
# 笔刷着色器
# 衰减曲线
# 柔化边缘
```

### 动态拓扑设置

```python
# 动态拓扑
# 细分类型 - 四边形或三角形
# 细节类型 - 恒定或细节到大小
# 细节大小 - 雕刻细节级别
# 禁用边缘折叠
```

### 对称设置

```python
# 对称
# X 轴对称
# Y 轴对称
# Z 轴对称
# 径向对称
# 旋转对称

# 镜像
# 镜像笔刷
# 镜像选择
```

### 可见性设置

```python
# 可见性
# 隐藏未选定
# 隐藏网格边界
# 框架所有

# 面组可见性
# 面组边界
# 面组颜色
# 显示所有面组
```

## 最佳实践

### 雕刻工作流

```python
# 从低多边形开始
# 创建基础形状
# 不要过早添加细节

# 分层雕刻
# 从大形状开始
# 然后添加中等细节
# 最后添加小细节

# 使用对称
# 使用对称雕刻身体
# 然后添加不对称细节

# 备份工作
# 定期保存版本
# 使用撤销功能
```

### 笔刷技术

```python
# 笔刷方向
# 沿法线推拉
# 使用参考图像
# 注意光线方向

# 笔刷强度
# 从低强度开始
# 逐步增加
# 避免过度雕刻

# 笔刷半径
# 根据细节大小调整
# 大区域用大半径
# 小细节用小半径

# 平滑过渡
# 使用平滑画笔混合
# 创建自然过渡
```

### 性能优化

```python
# 优化几何体
# 保持合理的顶点数量
# 使用多分辨率修饰符
# 雕刻后简化网格

# 使用 LOD
# 使用细节级别
# 低细节用于雕刻
# 高细节用于最终细节

# 隐藏区域
# 隐藏不需要雕刻的区域
# 减少视口负担
```

## Python脚本

### 雕刻操作

```python
# 切换到雕刻模式
import bpy
obj = bpy.context.active_object

bpy.ops.object.mode_set(mode='SCULPT')

# 选择画笔
bpy.ops.wm.tool_set_by_id(name="builtin_brush.Draw")

# 设置画笔属性
context = bpy.context
brush = context.tool_settings.sculpt.brush

brush.size = 50  # 半径
brush.strength = 0.5  # 强度
brush.auto_smooth_factor = 0.5  # 自动平滑
```

### 顶点颜色操作

```python
# 设置顶点颜色
import bpy
import random

obj = bpy.context.active_object

# 确保有颜色属性
if not obj.data.color_attributes:
    obj.data.color_attributes.new(name="Col", domain='CORNER')

color_attr = obj.data.color_attributes.active

# 随机化顶点颜色
for i, color in enumerate(color_attr.data):
    color.color = (random.random(), random.random(), random.random(), 1.0)

# 绘制顶点颜色
bpy.ops.object.mode_set(mode='VERTEX_PAINT')
```

### 权重操作

```python
# 设置顶点权重
import bpy

obj = bpy.context.active_object

# 创建顶点组
vertex_group = obj.vertex_groups.new(name="TestGroup")

# 设置顶点权重
for i in range(len(obj.data.vertices)):
    weight = i / len(obj.data.vertices)
    vertex_group.add([i], weight, 'REPLACE')

# 切换到权重绘制
bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
```

### 批量操作

```python
# 批量设置画笔
import bpy

# 设置所有画笔的属性
for brush in bpy.data.brushes:
    if brush.sculpt:
        brush.sculpt.size = 50
        brush.sculpt.strength = 0.5

# 禁用对称
context = bpy.context
context.tool_settings.sculpt.use_symmetry_x = False
context.tool_settings.sculpt.use_symmetry_y = False
context.tool_settings.sculpt.use_symmetry_z = False
```

## 常见问题解决

### 雕刻问题

```python
# 笔刷无反应
# 检查是否在雕刻模式
# 检查对象是否被锁定
# 检查可见性设置

# 笔刷变形异常
# 检查网格缩放
# 应用缩放
# 检查笔刷方向

# 动态拓扑问题
# 检查动态拓扑是否启用
# 检查细节级别
# 检查是否有足够内存
```

### 绘制问题

```python
# 颜色不显示
# 检查颜色属性是否创建
# 检查材质是否使用顶点颜色
# 检查视图着色模式

# 权重不生效
# 检查顶点组是否存在
# 检查权重值范围
# 检查修饰符设置

# 纹理不更新
# 检查纹理是否加载
# 检查纹理槽设置
# 检查图像是否保存
```


---
## bpy_grease_pencil

# Grease Pencil模块

## Grease Pencil概述

Grease Pencil是Blender中的一种对象类型，它接受来自鼠标或压感 stylus 的绘图信息，并将其作为点集合放置在3D空间中，这些点被定义为笔划。Grease Pencil对象可用于制作传统2D动画、剪纸动画、运动图形，或用作分镜工具等。

```python
# Grease Pencil特点
# 接受鼠标或压感输入
# 在3D空间中创建点
# 支持2D和3D绘图
# 支持动画制作

# 应用场景
# 传统2D动画
# 剪纸动画
# 运动图形
# 分镜工具
# 3D空间中的插图
# 概念艺术
# 动画预览

# 与网格的区别
# 点相当于网格的顶点
# 编辑线是不可见的辅助线
# 笔划是最终渲染结果
```

## Grease Pencil结构

Grease Pencil对象有三个主要基本组件：点、编辑线和笔划。

```python
# 点（Points）
# 是编辑Grease Pencil对象的主要元素
# 点代表3D空间中的单个点
# 每个点存储定义笔划最终外观的所有属性
# 包括：位置、厚度、Alpha、权重、UV旋转

# 编辑线（Edit Lines）
# 点始终由直线连接
# 在编辑模式或线框视图中可见
# 在渲染图像中不可见
# 用于构建最终笔划

# 笔划（Strokes）
# 是点和编辑线的渲染图像
# 使用特定的Grease Pencil材质
# 材质在笔划级别链接

# 数据结构
# Grease Pencil对象包含多个图层
# 每个图层包含多个帧
# 每帧包含多个笔划
# 每个笔划包含多个点
```

## 创建和管理

### 创建Grease Pencil对象

```python
# 创建空白的Grease Pencil对象
import bpy
bpy.ops.object.gpencil_add(
    type='EMPTY',
    location=(0, 0, 0)
)

# 从模板创建
# 使用2D动画模板
bpy.ops.wm.read_homefile(
    filepath=bpy.utils.resource_path('USER') + "/templates/starter.blend",
    ui_tab='PROJECTS',
    project='2D Animation'
)

# 创建指定类型的Grease Pencil
bpy.ops.object.gpencil_add(
    type='STROKE',  # 单笔划
    location=(0, 0, 0)
)

# 复制Grease Pencil对象
gp_obj = bpy.context.active_object
bpy.ops.object.duplicate({"selected_objects": [gp_obj]})
```

### 管理图层

```python
# 访问Grease Pencil数据
gp_data = bpy.context.active_object.data

# 创建新图层
layer = gp_data.layers.new(name="LayerName", set_active=True)

# 删除图层
gp_data.layers.remove(layer)

# 重命名图层
layer.name = "NewLayerName"

# 获取活动图层
active_layer = gp_data.layers.active

# 设置图层可见性
layer.hide = False
layer.lock = False

# 设置图层透明度
layer.opacity = 0.5

# 设置图层混合模式
layer.blend_mode = 'REGULAR'  # 普通
layer.blend_mode = 'ADD'  # 叠加
layer.blend_mode = 'MULTIPLY'  # 正片叠底
layer.blend_mode = 'SUBTRACT'  # 减去
layer.blend_mode = 'DIVIDE'  # 划分

# 图层排序
gp_data.layers.move(layer, -1)  # 上移
gp_data.layers.move(layer, 1)  # 下移

# 锁定图层
layer.lock = True
layer.lock_value = True  # 锁定位置
layer.lock_parents = True  # 锁定父级
```

### 管理帧

```python
# 在活动图层中创建帧
frame = active_layer.frames.new(frame_number=1)

# 删除帧
active_layer.frames.remove(frame)

# 获取帧
frame = active_layer.frames[0]

# 复制帧
import copy
new_frame = active_layer.frames.new(frame.frame_number + 1)
for stroke in frame.strokes:
    new_stroke = new_frame.strokes.new(stroke.material.name)
    new_stroke.points.add(len(stroke.points))
    new_stroke.points.foreach_set('co', stroke.points.foreach_get('co'))
    new_stroke.points.foreach_set('pressure', stroke.points.foreach_get('pressure'))
    new_stroke.points.foreach_set('strength', stroke.points.foreach_get('strength'))

# 帧关键属性
frame.frame_number  # 帧编号
frame.duration  # 持续时间

# 清空帧内容
frame.strokes.clear()
```

## 笔划操作

### 创建笔划

```python
# 创建新笔划
stroke = frame.strokes.new(material_name="MaterialName")

# 设置笔划点
stroke.points.add(count=5)

# 设置点坐标
stroke.points[0].co = (1.0, 0.0, 0.0)  # X, Y, Z
stroke.points[1].co = (0.5, 0.5, 0.0)
stroke.points[2].co = (0.0, 1.0, 0.0)
stroke.points[3].co = (-0.5, 0.5, 0.0)
stroke.points[4].co = (-1.0, 0.0, 0.0)

# 设置压力值（用于可变宽度）
stroke.points[0].pressure = 1.0
stroke.points[1].pressure = 0.8
stroke.points[2].pressure = 0.5

# 设置强度值（用于不透明度）
stroke.points[0].strength = 1.0
stroke.points[1].strength = 0.9

# 设置UV旋转
stroke.points[0].uv_rotation = 0.0
```

### 笔划属性

```python
# 笔划基本属性
stroke.material.name  # 材质名称
stroke.draw_mode  # 绘制模式
stroke.show_fill  # 显示填充
stroke.show_stroke  # 显示描边
stroke.thickness  # 厚度

# 笔划几何属性
stroke.is_closed  # 是否闭合
stroke.use_cyclic  # 使用循环
stroke.start_endpoint  # 起点
stroke.end_endpoint  # 终点

# 访问所有点
for point in stroke.points:
    print(f"位置: {point.co}")
    print(f"压力: {point.pressure}")
    print(f"强度: {point.strength}")

# 点数量
num_points = len(stroke.points)

# 更新笔划
stroke.update()
```

### 笔划编辑

```python
# 删除笔划
frame.strokes.remove(stroke)

# 复制笔划
new_stroke = frame.strokes.new(stroke.material.name)
num_points = len(stroke.points)
new_stroke.points.add(num_points)

# 复制点数据
coords = [c for p in stroke.points for c in p.co]
pressures = [p.pressure for p in stroke.points]
strengths = [p.strength for p in stroke.points]

new_stroke.points.foreach_set('co', coords)
new_stroke.points.foreach_set('pressure', pressures)
new_stroke.points.foreach_set('strength', strengths)

# 转换笔划坐标
from mathutils import Matrix
transform_matrix = Matrix.Translation((1, 0, 0))
for point in stroke.points:
    point.co = transform_matrix @ point.co

# 平滑笔划
for _ in range(5):
    for i in range(1, len(stroke.points) - 1):
        prev_pt = stroke.points[i-1].co
        curr_pt = stroke.points[i].co
        next_pt = stroke.points[i+1].co
        stroke.points[i].co = (prev_pt + curr_pt + next_pt) / 3
```

## 绘图模式

### 进入绘图模式

```python
# 切换到绘图模式
bpy.ops.object.mode_set(mode='PAINT_GPENCIL')

# 活动Grease Pencil对象
obj = bpy.context.active_object

# 活动图层
layer = obj.data.layers.active

# 活动帧
frame = layer.active_frame

# 切换到编辑模式
bpy.ops.object.mode_set(mode='EDIT_GPENCIL')

# 切换到雕刻模式
bpy.ops.object.mode_set(mode='SCULPT_GPENCIL')

# 切换到顶点绘制模式
bpy.ops.object.mode_set(mode='VERTEX_GPENCIL')

# 切换到权重绘制模式
bpy.ops.object.mode_set(mode='WEIGHT_GPENCIL')
```

### 绘图工具

```python
# 选择绘图工具
bpy.ops.wm.tool_set_by_id(name="gpencil_draw")

# 画笔类型
# 画笔 - 基本绘制
# 墨水 - 精确线条
# 标记 - 粗线条
# 填充 - 区域填充
# 橡皮擦 - 擦除

# 设置画笔属性
brush = bpy.context.tool_settings.gpencil_paint.brush
brush.size = 20  # 画笔大小
brush.gpencil_tool = 'DRAW'  # 工具类型
brush.gpencil_paint.thickness  # 厚度
brush.gpencil_paint.color  # 颜色
brush.gpencil_paint.alpha  # Alpha值

# 设置绘制模式
bpy.context.tool_settings.gpencil_paint.brush.gpencil_paint.paint_mode = 'DRAW'
bpy.context.tool_settings.gpencil_paint.brush.gpencil_paint.paint_mode = 'ERASE'
bpy.context.tool_settings.gpencil_paint.brush.gpencil_paint.paint_mode = 'FILL'
```

### 绘制设置

```python
# 绘制设置
settings = bpy.context.tool_settings.gpencil_paint

# 自动创建关键帧
settings.use_autokeyframe_gpencil = True

# 新帧类型
settings.new_frame_type = 'KEYFRAME'  # 关键帧
settings.new_frame_TYPE = 'INTERPOLATED'  # 插值帧

# 绘制平面
settings.gpencil_stroke_placement_view3d = 'SURFACE'  # 表面
settings.gpencil_stroke_placement_view3d = 'CURSOR'  # 游标
settings.gpencil_stroke_placement_view3d = 'VIEW'  # 视图
settings.gpencil_stroke_placement_view3d = 'ZAXIS'  # Z轴
settings.gpencil_stroke_placement_view3d = 'XYAXIS'  # XY轴
settings.gpencil_stroke_placement_view3d = 'XZAXIS'  # XZ轴

# 捕捉设置
settings.use_snap_gpencil = True
settings.snap_gpencil_target = 'CENTER'  # 中心
settings.snap_gpencil_target = 'CLOSEST'  # 最近点
```

## 材质

### 创建材质

```python
# 创建Grease Pencil材质
mat = bpy.data.materials.new(name="GP_Material")
mat.use_gpencil = True

# 设置材质属性
gp_mat = mat.gpencil_settings

# 设置颜色
gp_mat.color = (0.0, 0.5, 1.0, 1.0)  # RGBA

# 设置不透明度
gp_mat.alpha = 1.0

# 设置线宽
gp_mat.line_width = 3.0

# 设置填充颜色
gp_mat.fill_color = (1.0, 1.0, 1.0, 1.0)
gp_mat.use_fill = True

# 设置材质混合模式
gp_mat.blend_mode = 'REGULAR'  # 普通
gp_mat.blend_mode = 'ADD'  # 叠加
gp_mat.blend_mode = 'MULTIPLY'  # 正片叠底
gp_mat.blend_mode = 'SUBTRACT'  # 减去
gp_mat.blend_mode = 'DIVIDE'  # 划分
```

### 材质纹理

```python
# 添加纹理到材质
gp_mat.use_texture = True

# 创建纹理
tex = bpy.data.textures.new(name="GP_Texture", type='IMAGE')
tex.image = bpy.data.images.load("//texture.png")

# 设置纹理混合
gp_mat.texture_mask = 'STROKE'  # 笔划
gp_mat.texture_mask = 'FILL'  # 填充
gp_mat.texture_mask = 'BOTH'  # 两者

# 设置纹理混合模式
gp_mat.texture_blend = 'MULTIPLY'  # 正片叠底
gp_mat.texture_blend = 'ADD'  # 叠加
gp_mat.texture_blend = 'MIX'  # 混合
```

### 分配材质

```python
# 分配材质到对象
gp_obj.data.materials.append(mat)

# 为笔划设置材质
stroke.material = mat

# 获取材质的笔划
for frame in layer.frames:
    for stroke in frame.strokes:
        if stroke.material == mat:
            print(f"帧 {frame.frame_number} 使用该材质")

# 批量更改材质
for frame in layer.frames:
    for stroke in frame.strokes:
        stroke.material = new_mat
```

## 修饰符

### 添加修饰符

```python
# 添加修饰符
mod = gp_obj.modifiers.new(name="ModifierName", type='GP_MODIFIER_TYPE')

# 修饰符类型
# GP_MODIFIER_TYPE = 'gp_modifier_type'

# 可用修饰符
# 生成修饰符
mod = gp_obj.modifiers.new(name="Array", type='GP_ARRAY')
mod = gp_obj.modifiers.new(name="Build", type='GP_BUILD')
mod = gp_obj.modifiers.new(name="Dash", type='GP_DASH')
mod = gp_obj.modifiers.new(name="Envelope", type='GP_ENVELOPE')
mod = gp_obj.modifiers.new(name="LineArt", type='GP_LINEART')
mod = gp_obj.modifiers.new(name="Mirror", type='GP_MIRROR')
mod = gp_obj.modifiers.new(name="Simplify", type='GP_SIMPLIFY')
mod = gp_obj.modifiers.new(name="Subdivide", type='GP_SUBDIVIDE')

# 变形修饰符
mod = gp_obj.modifiers.new(name="Armature", type='GP_ARMATURE')
mod = gp_obj.modifiers.new(name="Hook", type='GP_HOOK')
mod = gp_obj.modifiers.new(name="Lattice", type='GP_LATTICE')
mod = gp_obj.modifiers.new(name="Noise", type='GP_NOISE')
mod = gp_obj.modifiers.new(name="Offset", type='GP_OFFSET')
mod = gp_obj.modifiers.new(name="Shrinkwrap", type='GP_SHRINKWRAP')
mod = gp_obj.modifiers.new(name="Smooth", type='GP_SMOOTH')
mod = gp_obj.modifiers.new(name="Thickness", type='GP_THICKNESS')

# 颜色修饰符
mod = gp_obj.modifiers.new(name="HueSaturation", type='GP_HUE_SATURATION')
mod = gp_obj.modifiers.new(name="Opacity", type='GP_OPACITY')
mod = gp_obj.modifiers.new(name="Tint", type='GP_TINT')

# 编辑修饰符
mod = gp_obj.modifiers.new(name="TextureMapping", type='GP_TEXTURE_MAPPING')
mod = gp_obj.modifiers.new(name="TimeOffset", type='GP_TIME_OFFSET')
mod = gp_obj.modifiers.new(name="WeightAngle", type='GP_WEIGHT_ANGLE')
mod = gp_obj.modifiers.new(name="WeightProximity", type='GP_WEIGHT_PROXIMITY')
```

### 修饰符属性

```python
# 获取修饰符
mod = gp_obj.modifiers["ModifierName"]

# 启用/禁用
mod.show_viewport = True
mod.show_render = True
mod.show_in_editmode = True

# 删除修饰符
gp_obj.modifiers.remove(mod)

# 复制修饰符
new_mod = gp_obj.modifiers.new(name="Copy", type=mod.type)
for attr in dir(mod):
    if not attr.startswith('_') and attr != 'type' and attr != 'name':
        setattr(new_mod, attr, getattr(mod, attr))

# 重新排序
for i, mod in enumerate(gp_obj.modifiers):
    mod = gp_obj.modifiers.move(i, i+1)
```

### 常用修饰符设置

```python
# 镜像修饰符
mod = gp_obj.modifiers.new(name="Mirror", type='GP_MIRROR')
mod.x_axis = True
mod.y_axis = False
mod.z_axis = False
mod.merge_threshold = 0.01

# 简化修饰符
mod = gp_obj.modifiers.new(name="Simplify", type='GP_SIMPLIFY')
mod.factor = 0.1
mod.step = 1

# 厚度修饰符
mod = gp_obj.modifiers.new(name="Thickness", type='GP_THICKNESS')
mod.thickness = 2.0
mod.factor = 1.0

# 颜色修饰符
mod = gp_obj.modifiers.new(name="Opacity", type='GP_OPACITY')
mod.opacity = 0.5
mod.factor = 1.0

# 构建修饰符
mod = gp_obj.modifiers.new(name="Build", type='GP_BUILD')
mod.frame_start = 1
mod.frame_end = 30
mod.mode = 'REPLACE'
```

## 动画

### 关键帧操作

```python
# 插入关键帧
bpy.ops.gpencil.keyframe_insert()

# 删除关键帧
bpy.ops.gpencil.keyframe_delete()

# 清除关键帧
bpy.ops.gpencil.keyframe_clear()

# 复制到其他帧
bpy.ops.gpencil.copy()
bpy.ops.gpencil.paste(frame_number=10)

# 设置活动关键帧
layer.active_frame.frame_number = 10

# 关键帧属性
frame.keyframe_type = 'KEYFRAME'  # 关键帧
frame.keyframe_type = 'BREAKDOWN'  # 中间帧
frame.keyframe_type = 'EXTREME'  # 极值帧
frame.keyframe_type = 'JITTER'  # 抖动帧
```

### 插值

```python
# 设置插值类型
bpy.context.scene.tool_settings.gpencil_interpolate_all_layers = True
bpy.context.scene.tool_settings.gpencil_interpolate_all_keyframes = True

# 执行插值
bpy.ops.gpencil.interpolate()

# 插值模式
bpy.context.scene.tool_settings.gpencil_s_interpolation = 'LINEAR'  # 线性
bpy.context.scene.tool_settings.gpencil_s_interpolation = 'BEZIER'  # 贝塞尔
bpy.context.scene.tool_settings.gpencil_s_interpolation = 'ELASTIC'  # 弹性
bpy.context.scene.tool_settings.gpencil_s_interpolation = 'EXPO'  # 指数

# 插值Easing
bpy.context.scene.tool_settings.gpencil_easing = 'EASE_IN_OUT'  # 入出
bpy.context.scene.tool_settings.gpencil_easing = 'EASE_IN'  # 入
bpy.context.scene.tool_settings.gpencil_easing = 'EASE_OUT'  # 出
bpy.context.scene.tool_settings.gpencil_easing = 'EXTREME'  # 极值
```

### 时间偏移

```python
# 创建时间偏移修饰符
mod = gp_obj.modifiers.new(name="TimeOffset", type='GP_TIME_OFFSET')
mod.offset_type = 'CUSTOM'  # 自定义
mod.frame_offset = 5  # 帧偏移

mod.offset_type = 'CYCLE'  # 循环
mod.cycles = 2  # 循环次数

mod.offset_type = 'REVERSE'  # 翻转

# 随机时间偏移
mod.use_random = True
mod.random_seed = 42
mod.random_strength = 0.5
```

## 视觉特效

### 添加视觉特效

```python
# 创建视觉特效修饰符
# 模糊
mod = gp_obj.modifiers.new(name="Blur", type='GP_BLUR')
mod.radius = 5.0
mod.iterations = 3

# 着色
mod = gp_obj.modifiers.new(name="Colorize", type='GP_COLORIZE')
mod.color_mode = 'HUE'  # 色相
mod.color = (1.0, 0.0, 0.0, 1.0)

# 翻转
mod = gp_obj.modifiers.new(name="Flip", type='GP_FLIP')
mod.x_axis = False
mod.y_axis = True

# 发光
mod = gp_obj.modifiers.new(name="Glow", type='GP_GLOW')
mod.threshold = 0.5
mod.strength = 1.0
mod.radius = 10.0

# 像素化
mod = gp_obj.modifiers.new(name="Pixelate", type='GP_PIXELATE')
mod.size = 8

# 边缘
mod = gp_obj.modifiers.new(name="Rim", type='GP_RIM')
mod.thickness = 2.0
mod.opacity = 0.5

# 阴影
mod = gp_obj.modifiers.new(name="Shadow", type='GP_SHADOW')
mod.offset_x = 2.0
mod.offset_y = -2.0
mod.blur = 5.0

# 旋涡
mod = gp_obj.modifiers.new(name="Swirl", type='GP_SWIRL')
mod.radius = 20.0
mod.angle = 180.0

# 波浪失真
mod = gp_obj.modifiers.new(name="WaveDistortion", type='GP_WAVE_DISTORTION')
mod.distortion_type = 'SINE'
mod.amplitude = 0.5
mod.frequency = 2.0
```

## 多帧编辑

### 多帧概述

```python
# 多帧编辑允许同时编辑多个帧
# 对于创建逐帧动画非常有用
# 可以同时查看和编辑多个帧

# 启用多帧编辑
bpy.context.scene.tool_settings.use_gpencil_multiframe = True

# 设置多帧范围
bpy.context.scene.tool_settings.gpencil_multiframe_start = 1
bpy.context.scene.tool_settings.gpencil_multiframe_end = 30

# 多帧类型
# ALL - 所有帧
# RANGE - 帧范围
# SELECTED - 选定帧
```

## 雕刻模式

### 雕刻工具

```python
# 进入雕刻模式
bpy.ops.object.mode_set(mode='SCULPT_GPENCIL')

# 选择雕刻工具
bpy.ops.wm.tool_set_by_id(name="gpencil_sculpt")

# 雕刻工具类型
# Grab - 抓取
# Push - 推
# Pinch - 捏
# Smooth - 平滑
# Thick - 加厚
# Thin - 变细
# Randomize - 随机化
# Twist - 扭曲

# 雕刻设置
settings = bpy.context.tool_settings.gpencil_sculpt

# 强度
settings.strength = 0.5

# 半径
settings.radius = 20

# 雕刻模式
settings.sculpt_tool = 'GRAB'
settings.sculpt_tool = 'PUSH'
settings.sculpt_tool = 'PINCH'
settings.sculpt_tool = 'SMOOTH'
```

## 顶点绘制模式

### 顶点颜色

```python
# 进入顶点绘制模式
bpy.ops.object.mode_set(mode='VERTEX_GPENCIL')

# 选择顶点绘制工具
bpy.ops.wm.tool_set_by_id(name="gpencil_vertex")

# 顶点绘制工具
# Draw - 绘制
# Fill - 填充
# Smear - 涂抹

# 设置颜色
settings = bpy.context.tool_settings.gpencil_vertex_paint
settings.color = (1.0, 0.0, 0.0, 1.0)
settings.alpha = 1.0

# 混合模式
settings.vertex_color_method = 'MIX'  # 混合
settings.vertex_color_method = 'ADD'  # 叠加
settings.vertex_color_method = 'SUBTRACT'  # 减去
settings.vertex_color_method = 'MULTIPLY'  # 正片叠底
```

## 权重绘制模式

### 权重控制

```python
# 进入权重绘制模式
bpy.ops.object.mode_set(mode='WEIGHT_GPENCIL')

# 选择权重绘制工具
bpy.ops.wm.tool_set_by_id(name="gpencil_weight")

# 权重绘制工具
# Draw - 绘制
# Gradient - 渐变
# Sample - 采样

# 设置权重属性
settings = bpy.context.tool_settings.gpencil_weight_paint
settings.weight = 0.5

# 自动归一化
settings.use_auto_normalize = True

# 锁定保持
settings.use_lock_relative = True
```

## Python脚本

### 批量创建

```python
# 批量创建Grease Pencil动画帧
def create_gp_animation(gp_obj, num_frames, points_per_stroke=10):
    layer = gp_obj.data.layers.new(name="AnimationLayer", set_active=True)
    
    for i in range(num_frames):
        frame = layer.frames.new(frame_number=i+1)
        stroke = frame.strokes.new(material_name="Default")
        stroke.points.add(points_per_stroke)
        
        # 创建圆形路径
        import math
        for j in range(points_per_stroke):
            angle = 2 * math.pi * j / points_per_stroke
            x = math.cos(angle) * (1.0 + i * 0.01)
            y = math.sin(angle) * (1.0 + i * 0.01)
            stroke.points[j].co = (x, y, 0.0)
            stroke.points[j].pressure = 1.0 - i * 0.01

# 使用函数
create_gp_animation(bpy.context.active_object, 60)
```

### 导入导出

```python
# 导出为SVG
bpy.ops.export_annotation(filepath="//drawing.svg")

# 导出为PNG
bpy.ops.render.gpencil_export(
    filepath="//frame.png",
    start_frame=1,
    end_frame=1
)

# 导出为动画
bpy.ops.render.gpencil_export(
    filepath="//animation",
    start_frame=1,
    end_frame=60,
    duration=1/24
)

# 导入SVG
bpy.ops.import_annotation(filepath="//drawing.svg")
```

### 动画播放控制

```python
# 播放Grease Pencil动画
bpy.ops.animation.play()

# 设置播放速度
bpy.context.scene.render.fps = 24

# 设置帧范围
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 60

# 跳转到帧
bpy.context.scene.frame_set(30)
```

## 最佳实践

### 性能优化

```python
# 使用合适的点数量
# 避免过多的点
# 使用简化修饰符

# 合理使用图层
# 按内容分组
# 控制图层数量

# 使用修饰符
# 使用镜像减少绘制
# 使用构建修饰符创建动画

# 优化渲染
# 降低预览分辨率
# 禁用不必要的修饰符
```

### 工作流程建议

```python
# 规划动画
# 创建分镜
# 规划关键帧

# 使用2D动画模板
# 预设工作区
# 预设图层结构

# 定期保存
# 使用版本控制
# 备份重要文件

# 使用快捷键
# 熟悉常用操作
# 自定义快捷键
```

## 常见问题解决

### 绘制问题

```python
# 笔划不可见
# 检查图层可见性
# 检查材质设置
# 检查渲染设置

# 无法绘制
# 检查是否在绘图模式
# 检查活动图层
# 检查帧是否锁定

# 点太密集
# 使用更少的点
# 启用压力敏感
# 调整画笔设置
```

### 动画问题

```python
# 关键帧丢失
# 检查自动关键帧设置
# 手动插入关键帧
# 检查图层锁定状态

# 插值不正确
# 检查插值设置
# 调整关键帧位置
# 使用更多关键帧

# 动画卡顿
# 减少帧数量
# 简化笔划
# 降低预览质量
```


---
## bpy_raycasting_spatial

# 射线投射与空间查询模块

## 射线投射概述

射线投射是Blender中实现交互式工具的核心技术，通过发射射线检测场景中的对象，可以实现选择工具、放置物体、测量距离等功能。射线投射在3D视口中拾取对象、计算视线相交、确定点击位置等场景中广泛应用。

```python
# 射线投射应用场景
# 对象选择与拾取 - 检测鼠标点击位置的对象
# 放置物体 - 确定新物体的放置位置
# 视线检测 - 检测两点间的可视性
# 碰撞检测 - 检测射线与网格的相交
# 表面测量 - 获取交点处的法线、UV等信息

# 核心类
# Ray - 射线类，包含起点和方向
# bpy_extras.view3d_utils - 3D视图工具模块
# region_2d_to_location_3d - 2D转3D坐标转换
# region_3d_to_location_2d - 3D转2D坐标转换

# 坐标空间
# 屏幕空间 - 2D坐标，像素单位
# 视图空间 - 相对于相机的坐标
# 世界空间 - 场景全局坐标
# 本地空间 - 相对于对象的坐标
```

## 射线投射基础

### 创建射线

```python
import bpy
import bpy_extras
from mathutils import Vector, Matrix

def create_ray_from_mouse(region, rv3d, mouse_x, mouse_y):
    """从鼠标位置创建射线"""
    # 获取射线起点和方向
    ray_start = bpy_extras.view3d_utils.region_2d_to_origin_3d(region, rv3d, (mouse_x, mouse_y))
    ray_end = bpy_extras.view3d_utils.region_2d_to_vector_3d(region, rv3d, (mouse_x, mouse_y))
    ray_direction = ray_end - ray_start
    
    return ray_start, ray_direction

def create_ray_to_point(region, rv3d, mouse_x, mouse_y, target_point):
    """创建指向目标点的射线"""
    start, direction = create_ray_from_mouse(region, rv3d, mouse_x, mouse_y)
    
    # 归一化方向
    direction = direction.normalized()
    
    return start, direction

def create_orthographic_ray(region, rv3d, mouse_x, mouse_y):
    """从正交视图创建射线"""
    ray_start = bpy_extras.view3d_utils.region_2d_to_origin_3d(region, rv3d, (mouse_x, mouse_y))
    ray_direction = rv3d.view_matrix.to_quaternion() @ Vector((0, 0, -1))
    
    return ray_start, ray_direction

def create_perspective_ray(region, rv3d, mouse_x, mouse_y):
    """从透视视图创建射线"""
    ray_start = bpy_extras.view3d_utils.region_2d_to_origin_3d(region, rv3d, (mouse_x, mouse_y))
    ray_end = bpy_extras.view3d_utils.region_2d_to_vector_3d(region, rv3d, (mouse_x, mouse_y))
    ray_direction = (ray_end - ray_start).normalized()
    
    return ray_start, ray_direction
```

### 射线相交检测

```python
def ray_cast_single(scene, ray_start, ray_direction, objects=None):
    """射线投射检测单个相交"""
    # 使用场景的射线投射
    result = scene.ray_cast(ray_start, ray_direction)
    
    if result[0]:
        return {
            'hit': True,
            'location': result[1],
            'normal': result[2],
            'object': result[3],
            'face_index': result[4]
        }
    
    return {'hit': False}

def ray_cast_all(scene, ray_start, ray_direction, objects=None):
    """射线投射检测所有相交"""
    hits = []
    
    if objects is None:
        objects = scene.objects
    
    for obj in objects:
        if obj.type != 'MESH':
            continue
        
        # 获取评估后的网格
        depsgraph = bpy.context.evaluated_depsgraph_get()
        eval_obj = obj.evaluated_get(depsgraph)
        mesh = eval_obj.to_mesh()
        
        # 射线转换到本地空间
        inverse_matrix = obj.matrix_world.inverted()
        local_start = inverse_matrix @ ray_start
        local_end = inverse_matrix @ (ray_start + ray_direction * 1000)
        local_direction = (local_end - local_start).normalized()
        
        # 射线投射到网格
        hit, location, normal, face_index = mesh.ray_cast(local_start, local_direction)
        
        if hit:
            # 转换回世界空间
            world_location = obj.matrix_world @ location
            world_normal = (obj.matrix_world.to_3x3() @ normal).normalized()
            
            hits.append({
                'hit': True,
                'location': world_location,
                'normal': world_normal,
                'object': obj,
                'face_index': face_index,
                'distance': (world_location - ray_start).length
            })
        
        eval_obj.to_mesh_clear()
    
    # 按距离排序
    hits.sort(key=lambda x: x['distance'])
    
    return hits

def ray_cast_closest(scene, ray_start, ray_direction, objects=None):
    """获取最近的相交点"""
    hits = ray_cast_all(scene, ray_start, ray_direction, objects)
    
    if hits:
        return hits[0]
    
    return {'hit': False}
```

### 视图辅助函数

```python
def get_view3d_context():
    """获取3D视图上下文"""
    context = bpy.context
    
    # 获取活动区域
    for area in context.screen.areas:
        if area.type == 'VIEW_3D':
            for space in area.spaces:
                if space.type == 'VIEW_3D':
                    region = None
                    for r in area.regions:
                        if r.type == 'WINDOW':
                            region = r
                            break
                    
                    if region and space.region_3d:
                        return {
                            'area': area,
                            'space': space,
                            'region': region,
                            'rv3d': space.region_3d
                        }
    
    return None

def get_mouse_position(event):
    """获取鼠标在区域中的位置"""
    region = event.region
    mouse_x = event.mouse_region_x
    mouse_y = event.mouse_region_y
    
    return mouse_x, mouse_y

def is_in_view3d(event):
    """检查事件是否在3D视口中"""
    return event.area and event.area.type == 'VIEW_3D'

def convert_to_view3d(event):
    """转换事件到3D视图坐标"""
    if not is_in_view3d(event):
        return None
    
    return {
        'x': event.mouse_region_x,
        'y': event.mouse_region_y
    }
```

## 对象拾取

### 基础拾取

```python
def pick_object(context, mouse_x, mouse_y):
    """拾取鼠标位置的对象"""
    view3d = get_view3d_context()
    if not view3d:
        return None
    
    region = view3d['region']
    rv3d = view3d['rv3d']
    
    # 创建射线
    ray_start, ray_direction = create_ray_from_mouse(region, rv3d, mouse_x, mouse_y)
    
    # 射线投射
    scene = context.scene
    result = scene.ray_cast(ray_start, ray_direction)
    
    if result[0]:
        return result[3]  # 返回对象
    
    return None

def pick_object_and_face(context, mouse_x, mouse_y):
    """拾取对象和面"""
    view3d = get_view3d_context()
    if not view3d:
        return None
    
    region = view3d['region']
    rv3d = view3d['rv3d']
    
    ray_start, ray_direction = create_ray_from_mouse(region, rv3d, mouse_x, mouse_y)
    scene = context.scene
    
    result = scene.ray_cast(ray_start, ray_direction)
    
    if result[0]:
        return {
            'object': result[3],
            'location': result[1],
            'normal': result[2],
            'face_index': result[4]
        }
    
    return None

def pick_vertex(context, mouse_x, mouse_y, threshold=0.1):
    """拾取最近的顶点"""
    pick_result = pick_object_and_face(context, mouse_x, mouse_y)
    if not pick_result:
        return None
    
    obj = pick_result['object']
    hit_point = pick_result['location']
    
    # 获取评估后的网格
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    # 查找最近顶点
    closest_vertex = None
    min_distance = threshold
    
    for vertex in mesh.vertices:
        world_co = obj.matrix_world @ vertex.co
        distance = (world_co - hit_point).length
        
        if distance < min_distance:
            closest_vertex = vertex
            min_distance = distance
    
    eval_obj.to_mesh_clear()
    
    if closest_vertex:
        return {
            'vertex': closest_vertex,
            'object': obj,
            'location': obj.matrix_world @ closest_vertex.co
        }
    
    return None

def pick_edge(context, mouse_x, mouse_y, threshold=0.1):
    """拾取最近的边"""
    pick_result = pick_object_and_face(context, mouse_x, mouse_y)
    if not pick_result:
        return None
    
    obj = pick_result['object']
    hit_point = pick_result['location']
    
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    closest_edge = None
    min_distance = threshold
    
    for i, edge in enumerate(mesh.edges):
        v1 = mesh.vertices[edge.vertices[0]]
        v2 = mesh.vertices[edge.vertices[1]]
        
        p1 = obj.matrix_world @ v1.co
        p2 = obj.matrix_world @ v2.co
        
        # 计算点到线段的距离
        edge_vec = p2 - p1
        edge_length = edge_vec.length
        
        if edge_length < 0.0001:
            continue
        
        t = ((hit_point - p1) @ edge_vec) / (edge_length ** 2)
        t = max(0, min(1, t))
        
        closest_point = p1 + edge_vec * t
        distance = (hit_point - closest_point).length
        
        if distance < min_distance:
            closest_edge = edge
            min_distance = distance
            closest_point_on_edge = closest_point
    
    eval_obj.to_mesh_clear()
    
    if closest_edge:
        return {
            'edge': closest_edge,
            'object': obj,
            'location': closest_point_on_edge
        }
    
    return None
```

### 高级拾取

```python
def pick_with_filter(context, mouse_x, mouse_y, object_types=None, custom_filter=None):
    """带过滤器的拾取"""
    if object_types is None:
        object_types = ['MESH']
    
    view3d = get_view3d_context()
    if not view3d:
        return None
    
    region = view3d['region']
    rv3d = view3d['rv3d']
    
    ray_start, ray_direction = create_ray_from_mouse(region, rv3d, mouse_x, mouse_y)
    scene = context.scene
    
    # 获取所有可能的对象
    objects = [obj for obj in scene.objects if obj.type in object_types]
    
    # 射线投射
    hits = ray_cast_all(scene, ray_start, ray_direction, objects)
    
    # 应用自定义过滤
    for hit in hits:
        if custom_filter:
            if custom_filter(hit['object']):
                return hit
        else:
            return hit
    
    return None

def pick_nearest_to_cursor(context, mouse_x, mouse_y, objects=None):
    """拾取离光标最近的对象"""
    view3d = get_view3d_context()
    if not view3d:
        return None
    
    region = view3d['region']
    rv3d = view3d['rv3d']
    
    # 获取屏幕坐标对应的世界坐标
    cursor_3d = bpy_extras.view3d_utils.region_2d_to_location_3d(
        region, rv3d, (mouse_x, mouse_y), depth=0
    )
    
    if objects is None:
        objects = context.scene.objects
    
    # 找到最近的对象
    closest_obj = None
    min_distance = float('inf')
    
    for obj in objects:
        # 使用对象的中心点
        center = obj.matrix_world.to_translation()
        distance = (center - cursor_3d).length
        
        if distance < min_distance:
            min_distance = distance
            closest_obj = obj
    
    return closest_obj

def pick_by_name_prefix(context, mouse_x, mouse_y, prefix):
    """按名称前缀拾取"""
    def name_filter(obj):
        return obj.name.startswith(prefix)
    
    return pick_with_filter(context, mouse_x, mouse_y, custom_filter=name_filter)
```

## 表面查询

### 法线与切线

```python
def get_surface_normal(context, mouse_x, mouse_y):
    """获取表面法线"""
    result = pick_object_and_face(context, mouse_x, mouse_y)
    
    if result:
        return result['normal']
    
    return None

def get_surface_tangent(context, mouse_x, mouse_y, uv_channel=0):
    """获取表面切线"""
    result = pick_object_and_face(context, mouse_x, mouse_y)
    
    if not result:
        return None
    
    obj = result['object']
    face_index = result['face_index']
    uv_name = obj.data.uv_layers[uv_channel].name if obj.data.uv_layers else None
    
    if not uv_name:
        return None
    
    # 获取评估后的网格
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    # 计算切线
    face = mesh.polygons[face_index]
    uv_layer = mesh.uv_layers[uv_name]
    
    # 简化切线计算
    tangent = Vector((1, 0, 0))
    
    eval_obj.to_mesh_clear()
    
    # 转换到世界空间
    world_tangent = obj.matrix_world.to_3x3() @ tangent
    
    return world_tangent.normalized()

def get_surface_uv(context, mouse_x, mouse_y, uv_channel=0):
    """获取表面UV坐标"""
    result = pick_object_and_face(context, mouse_x, mouse_y)
    
    if not result:
        return None
    
    obj = result['object']
    hit_local = obj.matrix_world.inverted() @ result['location']
    face_index = result['face_index']
    
    if not obj.data.uv_layers:
        return None
    
    uv_name = obj.data.uv_layers[uv_channel].name
    
    # 获取评估后的网格
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    # 在面上找到交点
    face = mesh.polygons[face_index]
    uv_layer = mesh.uv_layers[uv_name]
    
    # 简化UV计算
    uv_coords = [uv_layer.data[loop_index].uv for loop_index in face.loop_indices]
    
    # 重心坐标计算UV
    v0 = mesh.vertices[face.vertices[0]].co
    v1 = mesh.vertices[face.vertices[1]].co
    v2 = mesh.vertices[face.vertices[2]].co
    
    # 简化：返回第一个UV
    uv = uv_coords[0] if uv_coords else (0, 0)
    
    eval_obj.to_mesh_clear()
    
    return uv
```

### 表面属性

```python
def get_surface_material(context, mouse_x, mouse_y):
    """获取表面材质"""
    result = pick_object_and_face(context, mouse_x, mouse_y)
    
    if not result:
        return None
    
    obj = result['object']
    face_index = result['face_index']
    
    if not obj.data.materials:
        return None
    
    # 确定材质索引
    face = obj.data.polygons[face_index]
    material_index = face.material_index
    
    if material_index < len(obj.data.materials):
        return obj.data.materials[material_index]
    
    return None

def get_surface_color(context, mouse_x, mouse_y):
    """获取表面顶点颜色"""
    result = pick_object_and_face(context, mouse_x, mouse_y)
    
    if not result:
        return None
    
    obj = result['object']
    hit_local = obj.matrix_world.inverted() @ result['location']
    
    if not obj.data.vertex_colors:
        return None
    
    color_layer = obj.data.vertex_colors.active
    face_index = result['face_index']
    
    # 获取评估后的网格
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    face = mesh.polygons[face_index]
    
    # 简化：返回第一个顶点颜色
    color = color_layer.data[face.loop_indices[0]].color
    
    eval_obj.to_mesh_clear()
    
    return color

def get_object_under_cursor(context, mouse_x, mouse_y):
    """获取光标下的对象（不进行射线投射）"""
    view3d = get_view3d_context()
    if not view3d:
        return None
    
    space = view3d['space']
    
    # 使用视图选择
    if space.region_3d.view_perspective == 'ORTHO':
        depth = 0
    else:
        depth = None
    
    # 获取3D位置
    location_3d = bpy_extras.view3d_utils.region_2d_to_location_3d(
        view3d['region'], view3d['rv3d'], (mouse_x, mouse_y), depth
    )
    
    # 查找包含该点的对象
    for obj in context.scene.objects:
        if obj.type != 'MESH':
            continue
        
        if obj.instance_type != 'NONE':
            continue
        
        # 简单检查：点是否在包围盒内
        if obj.bound_box:
            local_point = obj.matrix_world.inverted() @ location_3d
            
            # 检查包围盒
            bb = obj.bound_box
            if (bb[0][0] <= local_point.x <= bb[6][0] and
                bb[0][1] <= local_point.y <= bb[6][1] and
                bb[0][2] <= local_point.z <= bb[6][2]):
                return obj
    
    return None
```

## 距离计算

### 空间距离

```python
def distance_point_to_point(p1, p2):
    """两点间距离"""
    return (p1 - p2).length

def distance_point_to_line(point, line_p1, line_p2):
    """点到直线的距离"""
    line_vec = line_p2 - line_p1
    point_vec = point - line_p1
    
    if line_vec.length < 0.0001:
        return point_vec.length
    
    # 投影长度
    t = (point_vec @ line_vec) / (line_vec @ line_vec)
    t = max(0, min(1, t))
    
    # 直线上最近的点
    closest = line_p1 + line_vec * t
    
    return (point - closest).length

def distance_point_to_segment(point, seg_p1, seg_p2):
    """点到线段的距离"""
    line_vec = seg_p2 - seg_p1
    point_vec = point - seg_p1
    
    if line_vec.length < 0.0001:
        return point_vec.length
    
    t = (point_vec @ line_vec) / (line_vec @ line_vec)
    t = max(0, min(1, t))
    
    closest = seg_p1 + line_vec * t
    
    return (point - closest).length

def distance_point_to_plane(point, plane_point, plane_normal):
    """点到平面的距离"""
    plane_normal = plane_normal.normalized()
    diff = point - plane_point
    
    return abs(diff @ plane_normal)

def distance_point_to_triangle(point, v0, v1, v2):
    """点到三角形的距离"""
    # 简化为点到平面的距离
    edge1 = v1 - v0
    edge2 = v2 - v0
    
    normal = (edge1.cross(edge2)).normalized()
    distance = distance_point_to_plane(point, v0, normal)
    
    # 检查点是否在三角形投影范围内
    # 简化：总是返回投影距离
    
    return distance

def distance_object_to_object(obj1, obj2):
    """对象间的最小距离"""
    if obj1.type == 'MESH' and obj2.type == 'MESH':
        return distance_mesh_to_mesh(obj1, obj2)
    else:
        # 使用中心点距离
        return distance_point_to_point(
            obj1.matrix_world.to_translation(),
            obj2.matrix_world.to_translation()
        )

def distance_mesh_to_mesh(mesh_obj1, mesh_obj2, samples=100):
    """两个网格间的最小距离（近似）"""
    # 简化版本：采样网格顶点
    min_distance = float('inf')
    
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj1 = mesh_obj1.evaluated_get(depsgraph)
    eval_obj2 = mesh_obj2.evaluated_get(depsgraph)
    
    mesh1 = eval_obj1.to_mesh()
    mesh2 = eval_obj2.to_mesh()
    
    vertices1 = [mesh_obj1.matrix_world @ v.co for v in mesh1.vertices[:samples]]
    vertices2 = [mesh_obj2.matrix_world @ v.co for v in mesh2.vertices[:samples]]
    
    for v1 in vertices1:
        for v2 in vertices2:
            dist = distance_point_to_point(v1, v2)
            if dist < min_distance:
                min_distance = dist
    
    eval_obj1.to_mesh_clear()
    eval_obj2.to_mesh_clear()
    
    return min_distance if min_distance != float('inf') else None
```

### 角度计算

```python
def angle_between_vectors(v1, v2):
    """两向量间角度"""
    dot = v1.normalized() @ v2.normalized()
    return math.acos(max(-1, min(1, dot)))

def angle_point_to_point(view_point, p1, p2):
    """从视角点看两点的角度"""
    v1 = p1 - view_point
    v2 = p2 - view_point
    
    return angle_between_vectors(v1, v2)

def get_view_angle(obj, camera):
    """获取对象相对于相机的视角"""
    obj_pos = obj.matrix_world.to_translation()
    cam_pos = camera.matrix_world.to_translation()
    
    # 视线方向
    view_dir = (obj_pos - cam_pos).normalized()
    
    # 相机朝向
    cam_dir = camera.matrix_world.to_quaternion() @ Vector((0, 0, -1))
    
    return angle_between_vectors(view_dir, cam_dir)

def is_in_frustum(obj, camera):
    """检查对象是否在相机视锥体内"""
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_camera = camera.evaluated_get(depsgraph)
    
    # 简化检查：对象中心是否在视野内
    obj_pos = obj.matrix_world.to_translation()
    cam_pos = camera.matrix_world.to_translation()
    
    view_angle = get_view_angle(obj, camera)
    
    # 假设90度视野
    return view_angle < math.radians(45)
```

## 放置与对齐

### 表面放置

```python
def place_on_surface(obj, context, mouse_x, mouse_y, offset=0):
    """将对象放置在表面上"""
    result = pick_object_and_face(context, mouse_x, mouse_y)
    
    if not result:
        return False
    
    # 设置位置
    obj.location = result['location']
    
    # 设置方向：使Z轴指向法线
    normal = result['normal']
    
    # 简化的向上向量计算
    up = Vector((0, 0, 1))
    
    if abs(normal @ up) > 0.99:
        # 法线垂直
        up = Vector((0, 1, 0))
    
    # 计算旋转
    rotation_axis = up.cross(normal).normalized()
    rotation_angle = math.acos(max(-1, min(1, up @ normal)))
    
    if rotation_axis.length > 0.0001:
        obj.rotation_euler = rotation_axis.to_track_quat('-Z', 'Y').to_euler('XYZ')
    else:
        # 法线与上向量平行或相反
        if normal @ up < 0:
            obj.rotation_euler = (math.pi, 0, 0)
    
    # 应用偏移
    obj.location += normal * offset
    
    return True

def align_to_normal(obj, normal, up_axis='Z'):
    """使对象对齐到法线"""
    # 目标方向
    if up_axis == 'Z':
        target_up = Vector((0, 0, 1))
    elif up_axis == 'Y':
        target_up = Vector((0, 1, 0))
    else:
        target_up = Vector((1, 0, 0))
    
    # 计算旋转
    rotation_axis = target_up.cross(normal).normalized()
    rotation_angle = math.acos(max(-1, min(1, target_up @ normal)))
    
    if rotation_axis.length > 0.0001:
        obj.rotation_euler = rotation_axis.to_track_quat('Z', 'Y').to_euler('XYZ')
    
    return obj.rotation_euler

def place_along_curve(obj, curve, position, up_axis='Z'):
    """沿曲线放置对象"""
    # 获取曲线上位置点的切线
    point = curve.data.splines[0].evaluate(position)
    tangent = curve.data.splines[0].evaluate(position + 0.01) - point
    
    # 归一化切线
    tangent = tangent.normalized()
    
    # 设置位置
    obj.location = point
    
    # 设置方向
    if up_axis == 'Z':
        up = Vector((0, 0, 1))
    else:
        up = Vector((0, 1, 0))
    
    # 计算旋转
    rotation_axis = up.cross(tangent).normalized()
    rotation_angle = math.acos(max(-1, min(1, up @ tangent)))
    
    if rotation_axis.length > 0.0001:
        obj.rotation_euler = rotation_axis.to_track_quat('Z', 'Y').to_euler('XYZ')
```

### 对齐操作

```python
def align_objects(objects, axis='X', method='CENTER'):
    """沿轴对齐多个对象"""
    if not objects:
        return
    
    if method == 'CENTER':
        center = sum((obj.location for obj in objects), Vector((0, 0, 0))) / len(objects)
        for obj in objects:
            if axis == 'X':
                obj.location.x = center.x
            elif axis == 'Y':
                obj.location.y = center.y
            elif axis == 'Z':
                obj.location.z = center.z
    
    elif method == 'MIN':
        min_val = min(getattr(obj.location, axis) for obj in objects)
        for obj in objects:
            setattr(obj.location, axis, min_val)
    
    elif method == 'MAX':
        max_val = max(getattr(obj.location, axis) for obj in objects)
        for obj in objects:
            setattr(obj.location, axis, max_val)

def distribute_objects(objects, axis='X'):
    """均匀分布多个对象"""
    if len(objects) < 2:
        return
    
    sorted_objs = sorted(objects, key=lambda obj: getattr(obj.location, axis))
    
    min_val = getattr(sorted_objs[0].location, axis)
    max_val = getattr(sorted_objs[-1].location, axis)
    
    step = (max_val - min_val) / (len(objects) - 1)
    
    for i, obj in enumerate(sorted_objs):
        setattr(obj.location, axis, min_val + step * i)

def align_to_selection(context, target_obj, source_objs=None):
    """将对象对齐到选择"""
    if source_objs is None:
        source_objs = context.selected_objects.copy()
    
    if target_obj not in source_objs:
        source_objs.append(target_obj)
    
    for obj in source_objs:
        if obj == target_obj:
            continue
        
        obj.location = target_obj.location
        obj.rotation_euler = target_obj.rotation_euler
        obj.scale = target_obj.scale
```

## 体积查询

### 包围盒

```python
def get_world_bounding_box(obj):
    """获取对象的世界包围盒"""
    if not obj.bound_box:
        return None
    
    matrix = obj.matrix_world
    corners = []
    
    for corner in obj.bound_box:
        world_corner = matrix @ Vector(corner)
        corners.append(world_corner)
    
    return corners

def get_bounding_box_center(obj):
    """获取包围盒中心"""
    corners = get_world_bounding_box(obj)
    if not corners:
        return obj.location
    
    center = sum(corners, Vector((0, 0, 0))) / len(corners)
    return center

def get_bounding_box_size(obj):
    """获取包围盒尺寸"""
    corners = get_world_bounding_box(obj)
    if not corners:
        return Vector((0, 0, 0))
    
    min_coords = Vector([float('inf')] * 3)
    max_coords = Vector([-float('inf')] * 3)
    
    for corner in corners:
        for i in range(3):
            min_coords[i] = min(min_coords[i], corner[i])
            max_coords[i] = max(max_coords[i], corner[i])
    
    return max_coords - min_coords

def is_point_in_bounding_box(point, obj):
    """检查点是否在包围盒内"""
    corners = get_world_bounding_box(obj)
    if not corners:
        return False
    
    # 简化：检查是否在轴对齐包围盒内
    center = get_bounding_box_center(obj)
    size = get_bounding_box_size(obj)
    
    for i in range(3):
        if abs(point[i] - center[i]) > size[i] / 2:
            return False
    
    return True

def bounding_box_intersects(obj1, obj2):
    """检查两个对象的包围盒是否相交"""
    corners1 = get_world_bounding_box(obj1)
    corners2 = get_world_bounding_box(obj2)
    
    if not corners1 or not corners2:
        return False
    
    # 检查每个轴
    for i in range(3):
        min1 = min(c[i] for c in corners1)
        max1 = max(c[i] for c in corners1)
        min2 = min(c[i] for c in corners2)
        max2 = max(c[i] for c in corners2)
        
        if max1 < min2 or max2 < min1:
            return False
    
    return True
```

### 体积计算

```python
def get_object_volume(obj):
    """获取对象体积"""
    if obj.type != 'MESH':
        return 0
    
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    volume = 0
    for poly in mesh.polygons:
        # 简化体积计算
        volume += poly.area
    
    eval_obj.to_mesh_clear()
    
    return volume

def get_center_of_mass(obj):
    """获取对象质心"""
    if obj.type != 'MESH':
        return obj.location
    
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    center = Vector((0, 0, 0))
    total_area = 0
    
    for poly in mesh.polygons:
        poly_center = Vector((0, 0, 0))
        for vi in poly.vertices:
            poly_center += mesh.vertices[vi].co
        poly_center /= len(poly.vertices)
        
        center += poly_center * poly.area
        total_area += poly.area
    
    eval_obj.to_mesh_clear()
    
    if total_area > 0:
        center /= total_area
        return obj.matrix_world @ center
    
    return obj.location

def is_point_inside_mesh(point, obj):
    """检查点是否在网格内部"""
    if obj.type != 'MESH':
        return False
    
    # 从点向任意方向发射射线
    test_dir = Vector((1, 0, 0))
    
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    
    local_point = obj.matrix_world.inverted() @ point
    
    # 射线投射
    hit, _, _, _ = mesh.ray_cast(local_point, test_dir)
    
    eval_obj.to_mesh_clear()
    
    # 奇数次相交表示在内部
    return hit
```

## 性能优化

### 射线投射优化

```python
class RaycastOptimizer:
    """射线投射优化器"""
    
    def __init__(self):
        self._object_cache = {}
        self._mesh_cache = {}
    
    def get_cached_mesh(self, obj):
        """获取缓存的网格"""
        if obj.name not in self._mesh_cache:
            depsgraph = bpy.context.evaluated_depsgraph_get()
            eval_obj = obj.evaluated_get(depsgraph)
            self._mesh_cache[obj.name] = eval_obj.to_mesh()
            self._object_cache[obj.name] = obj
        
        return self._mesh_cache[obj.name]
    
    def clear_cache(self):
        """清除缓存"""
        for mesh in self._mesh_cache.values():
            # 注意：不能直接删除评估后的网格
            pass
        self._mesh_cache.clear()
        self._object_cache.clear()

def optimize_raycast_for_objects(objects):
    """优化对指定对象集的射线投射"""
    # 预先评估所有对象
    depsgraph = bpy.context.evaluated_depsgraph_get()
    
    evaluated_objects = {}
    meshes = {}
    
    for obj in objects:
        if obj.type == 'MESH':
            eval_obj = obj.evaluated_get(depsgraph)
            meshes[obj] = eval_obj.to_mesh()
            evaluated_objects[obj.name] = eval_obj
    
    return evaluated_objects, meshes

def use_octree_for_raycast(mesh, max_depth=8):
    """为网格创建八叉树加速射线投射"""
    # 简化版本：不创建实际八叉树
    
    # 优化：预先计算面法线
    face_normals = {}
    for poly in mesh.polygons:
        face_normals[poly.index] = poly.normal
    
    return face_normals
```

### 批量查询优化

```python
class SpatialQuery:
    """空间查询类"""
    
    def __init__(self):
        self._objects = []
        self._aabb_tree = None
    
    def build(self, objects=None):
        """构建空间索引"""
        if objects is None:
            objects = bpy.context.scene.objects
        
        self._objects = [obj for obj in objects if obj.type == 'MESH']
        
        # 简化：不创建实际AABB树
        # 可以使用BVH模块
    
    def query_point(self, point, max_distance=float('inf')):
        """查询点附近的网格"""
        results = []
        
        for obj in self._objects:
            if is_point_in_bounding_box(point, obj):
                dist = distance_point_to_object(point, obj)
                if dist < max_distance:
                    results.append((obj, dist))
        
        results.sort(key=lambda x: x[1])
        return results
    
    def query_ray(self, ray_start, ray_direction, max_distance=float('inf')):
        """查询射线穿过的网格"""
        results = []
        
        for obj in self._objects:
            hit = ray_cast_single(bpy.context.scene, ray_start, ray_direction, [obj])
            if hit['hit']:
                dist = (hit['location'] - ray_start).length
                if dist < max_distance:
                    results.append((obj, hit, dist))
        
        results.sort(key=lambda x: x[2])
        return results

def distance_point_to_object(point, obj):
    """点到对象的最小距离"""
    if obj.type != 'MESH':
        return (point - obj.location).length
    
    # 简化：点到包围盒的距离
    corners = get_world_bounding_box(obj)
    if not corners:
        return float('inf')
    
    # 简化为中心点距离
    return distance_point_to_point(point, get_bounding_box_center(obj))
```


---
## bpy_video_sequencer

# 视频序列编辑器模块

## 视频序列编辑器概述

视频序列编辑器（Video Sequence Editor，VSE）是Blender中用于视频编辑、特效合成和音频处理的强大工具。通过Python API，开发者可以自动化视频编辑任务、创建自定义效果和处理多媒体内容。VSE支持多种类型的片段，包括视频剪辑、图像序列、音频文件、文本标题和色彩校正效果。

```python
# VSE核心概念
# 序列（Strip）- 时间线上的基本编辑单位
# 通道（Channel）- 垂直轨道，用于叠加多个序列
# 帧（Frame）- 时间轴上的最小单位
# 片段类型
#   Movie - 视频文件
#   Image - 图像或图像序列
#   Scene - Blender场景
#   MovieClip - 视频片段对象
#   Sound - 音频文件
#   Text - 文字标题
#   Color - 色彩条
#   Effect - 效果条
#   Adjust - 调整层
```

## 访问视频序列编辑器

### 场景与序列数据

```python
# 获取序列编辑器数据
scene = bpy.context.scene
sequence_editor = scene.sequence_editor

# 检查是否启用序列编辑器
if sequence_editor:
    print("序列编辑器已启用")
    print(f"总序列数: {len(sequence_editor.sequences)}")
else:
    print("序列编辑器未启用")

# 启用序列编辑器
if not scene.sequence_editor:
    scene.sequence_editor = bpy.data.scenes[0].sequence_editor
    scene.sequence_editor_create()

# 遍历所有序列
for seq in sequence_editor.sequences:
    print(f"序列: {seq.name}, 类型: {seq.type}, 帧范围: {seq.frame_start}-{seq.frame_end}")

# 获取活动序列
active_seq = sequence_editor.active_strip
if active_seq:
    print(f"活动序列: {active_seq.name}")

# 获取选中的序列
selected_seqs = [s for s in sequence_editor.sequences if s.select]
print(f"选中序列数: {len(selected_seqs)}")
```

### 序列属性

```python
# 通用序列属性
def print_sequence_info(seq):
    print(f"名称: {seq.name}")
    print(f"类型: {seq.type}")
    print(f"通道: {seq.channel}")
    print(f"开始帧: {seq.frame_start}")
    print(f"结束帧: {seq.frame_end}")
    print(f"时长: {seq.frame_duration}")
    print(f"帧偏移: {seq.frame_offset_start} - {seq.frame_offset_end}")
    print(f"速度因子: {seq.speed_factor}")
    print(f"淡入淡出: {seq.fade_in}, {seq.fade_out}")
    print(f"静音: {seq.mute}")

# 修改序列位置
def move_sequence(seq, new_start, new_channel=None):
    seq.frame_start = new_start
    if new_channel:
        seq.channel = new_channel

# 修改序列时长
def trim_sequence(seq, start_offset, end_offset):
    seq.frame_offset_start = start_offset
    seq.frame_offset_end = end_offset
```

## 创建序列

### 视频剪辑序列

```python
# 导入视频文件
def create_movie_strip(filepath, frame_start=1, channel=1):
    scene = bpy.context.scene
    
    # 确保序列编辑器存在
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    # 创建视频序列
    new_strip = scene.sequence_editor.sequences.new_movie(
        name=bpy.path.basename(filepath),
        filepath=filepath,
        frame_start=frame_start,
        channel=channel
    )
    
    return new_strip

# 使用
strip = create_movie_strip(
    filepath="D:/videos/my_video.mp4",
    frame_start=1,
    channel=2
)

# 从文件浏览器导入
filepath = "D:/videos/clip.mp4"
bpy.ops.sequencer.movie_strip_add(
    filepath=filepath,
    frame_start=10,
    channel=1,
    replace_sel=True
)
```

### 图像序列

```python
# 创建图像序列
def create_image_sequence(filepaths, frame_start=1, channel=1):
    scene = bpy.context.scene
    
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    # 创建图像序列
    seq = scene.sequence_editor.sequences.new_image(
        name="Image Sequence",
        filepaths=filepaths,
        frame_start=frame_start,
        channel=channel
    )
    
    return seq

# 示例
images = [
    "D:/images/frame001.png",
    "D:/images/frame002.png",
    "D:/images/frame003.png"
]
strip = create_image_sequence(images)

# 导入目录中的图像序列
import os

def import_image_sequence_from_dir(directory, channel=1):
    scene = bpy.context.scene
    
    # 获取目录中所有图像文件
    valid_exts = ['.png', '.jpg', '.jpeg', '.tif', '.tiff', '.exr']
    files = sorted([
        os.path.join(directory, f) for f in os.listdir(directory)
        if os.path.splitext(f)[1].lower() in valid_exts
    ])
    
    if not files:
        return None
    
    # 创建图像序列
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    seq = scene.sequence_editor.sequences.new_image(
        name=os.path.basename(directory),
        filepaths=files,
        frame_start=1,
        channel=channel
    )
    
    return seq

# 使用
seq = import_image_sequence_from_dir("D:/animation/frames/", channel=2)
```

### 音频序列

```python
# 创建音频序列
def create_audio_strip(filepath, frame_start=1, channel=1):
    scene = bpy.context.scene
    
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    # 创建音频序列
    seq = scene.sequence_editor.sequences.new_sound(
        name=bpy.path.basename(filepath),
        filepath=filepath,
        frame_start=frame_start,
        channel=channel
    )
    
    return seq

# 使用
audio = create_audio_strip("D:/audio/bgm.mp3", frame_start=1, channel=4)

# 音频属性设置
def set_audio_properties(audio_seq, volume=1.0, pan=0.0):
    audio_seq.sound.volume = volume
    audio_seq.sound.pan = pan

# 修改音量淡入淡出
def set_audio_fades(audio_seq, fade_in=0.0, fade_out=0.0):
    audio_seq.animation_offset_end = fade_in
    audio_seq.animation_offset_start = fade_out

# 同步音频
def sync_audio_to_video(audio_seq, video_seq):
    audio_seq.frame_start = video_seq.frame_start
    audio_seq.frame_duration = video_seq.frame_duration
```

### 文字标题序列

```python
# 创建文字序列
def create_text_strip(text, frame_start=1, channel=1, duration=60):
    scene = bpy.context.scene
    
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    # 创建文字序列
    seq = scene.sequence_editor.sequences.new_text(
        name="Text",
        text=text,
        frame_start=frame_start,
        channel=channel,
        duration=duration
    )
    
    return seq

# 使用
text = create_text_strip("Hello, World!", frame_start=10, channel=3)

# 设置文字属性
def set_text_properties(text_seq, font_size=60, font_path=None, align='CENTER'):
    text_seq.text = text_seq.text  # 触发更新
    text_seq.font_size = font_size
    
    if font_path:
        text_seq.font_path = font_path
    
    text_seq.align_x = align

# 文字样式设置
def style_text_seq(text_seq, color=(1, 1, 1, 1), outline_color=(0, 0, 0, 1)):
    text_seq.color = color
    text_seq.outline_color = outline_color
    text_seq.outline_width = 2
    text_seq.shadow = True
    text_seq.shadow_offset = (2, 2)
```

### 色彩序列

```python
# 创建色彩条（背景色）
def create_color_strip(color, frame_start=1, channel=1, duration=60):
    scene = bpy.context.scene
    
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    # 创建色彩序列
    seq = scene.sequence_editor.sequences.new_color(
        name="Color",
        color=color,
        frame_start=frame_start,
        channel=channel,
        duration=duration
    )
    
    return seq

# 使用
color = (0.1, 0.1, 0.1, 1.0)  # 黑色背景
bg = create_color_strip(color, frame_start=1, channel=1)
```

## 场景序列

```python
# 导入Blender场景作为序列
def create_scene_strip(scene_name, frame_start=1, channel=1):
    scene = bpy.context.scene
    
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    # 创建场景序列
    seq = scene.sequence_editor.sequences.new_scene(
        name=scene_name,
        scene=bpy.data.scenes[scene_name],
        frame_start=frame_start,
        channel=channel
    )
    
    return seq

# 使用
scene_seq = create_scene_strip("CameraAnimation", frame_start=1, channel=2)

# 设置场景序列选项
def set_scene_strip_options(scene_seq, use_grease_pencil=True, show_front=True, show_back=True):
    scene_seq.show_grease_pencil = use_grease_pencil
    scene_seq.show_frontfaces = show_front
    scene_seq.show_backfaces = show_back

# 从其他文件链接场景
def link_scene_strip(filepath, scene_name, frame_start=1, channel=1):
    bpy.ops.wm.append(
        filepath=os.path.join(filepath, "Scene", scene_name),
        directory=filepath
    )
    
    scene = bpy.context.scene
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    seq = scene.sequence_editor.sequences.new_scene(
        name=scene_name,
        scene=bpy.data.scenes[scene_name],
        frame_start=frame_start,
        channel=channel
    )
    
    return seq
```

## 效果序列

### 基础效果

```python
# 创建效果序列
def create_effect_strip(effect_type, input1, frame_start=None, channel=1):
    scene = bpy.context.scene
    
    if not scene.sequence_editor:
        scene.sequence_editor_create()
    
    # 确定开始帧
    if frame_start is None:
        frame_start = input1.frame_start
    
    # 创建效果
    if effect_type in ['CROSS', 'DARKEN', 'LIGHTEN', 'ADD', 'SUBTRACT', 'ALPHA_OVER', 'ALPHA_UNDER', 'OVER_DROP']:
        # 需要两个输入的效果
        seq = scene.sequence_editor.sequences.new_effect(
            name=f"{effect_type} Effect",
            type=effect_type,
            frame_start=frame_start,
            channel=channel,
            input1=input1
        )
    else:
        # 单输入效果
        seq = scene.sequence_editor.sequences.new_effect(
            name=f"{effect_type} Effect",
            type=effect_type,
            frame_start=frame_start,
            channel=channel,
            input1=input1
        )
    
    return seq

# 转场效果
cross_strip = create_effect_strip('CROSS', video_strip1, frame_start=video_strip1.frame_end, channel=3)
cross_strip.input2 = video_strip2
```

### 效果类型

```python
# 转场效果
def create_transition(input1, input2, trans_type='CROSS', duration=24):
    scene = bpy.context.scene
    
    trans_strip = scene.sequence_editor.sequences.new_effect(
        name=f"{trans_type} Transition",
        type=trans_type,
        frame_start=input1.frame_end,
        channel=max(input1.channel, input2.channel) + 1
    )
    trans_strip.input1 = input1
    trans_strip.input2 = input2
    trans_strip.transition_duration = duration
    
    return trans_strip

# 溶解效果
def create_dissolve(input_seq, duration=24):
    dissolve = create_effect_strip('DISSOLVE', input_seq)
    dissolve.transition_duration = duration
    return dissolve

# 色彩校正效果
def create_color_balance(input_seq):
    cb = create_effect_strip('COLOR_BALANCE', input_seq)
    return cb

# 亮度对比度
def create_brightness_contrast(input_seq):
    bc = create_effect_strip('BRIGHTCONTRAST', input_seq)
    return bc

# 模糊效果
def create_blur(input_seq):
    blur = create_effect_strip('BLUR', input_seq)
    blur.size_x = 10
    blur.size_y = 10
    return blur
```

### 合成效果

```python
# Alpha叠加
def alpha_over(background, foreground, frame_start=None):
    alpha = create_effect_strip('ALPHA_OVER', background, frame_start, channel=background.channel + 1)
    alpha.input2 = foreground
    return alpha

# Alpha混合
def alpha_under(background, foreground, frame_start=None):
    alpha = create_effect_strip('ALPHA_UNDER', background, frame_start, channel=background.channel + 1)
    alpha.input2 = foreground
    return alpha

# 叠加混合
def add_blend(background, foreground, frame_start=None):
    add = create_effect_strip('ADD', background, frame_start, channel=background.channel + 1)
    add.input2 = foreground
    return add

# 变暗效果
def darken_blend(background, foreground, frame_start=None):
    darken = create_effect_strip('DARKEN', background, frame_start, channel=background.channel + 1)
    darken.input2 = foreground
    return darken

# 变亮效果
def lighten_blend(background, foreground, frame_start=None):
    lighten = create_effect_strip('LIGHTEN', background, frame_start, channel=background.channel + 1)
    lighten.input2 = foreground
    return lighten
```

## 修饰符

### 修饰符基础

```python
# 获取序列的修饰符
seq = sequence_editor.active_strip
modifiers = seq.modifiers

# 列出所有修饰符
for mod in modifiers:
    print(f"修饰符: {mod.name}, 类型: {mod.type}")

# 添加修饰符
def add_modifier(seq, mod_type, name=None):
    mod = seq.modifiers.new(name=f"{mod_type} Modifier", type=mod_type)
    return mod

# 移除修饰符
def remove_modifier(seq, mod_name):
    mod = seq.modifiers.get(mod_name)
    if mod:
        seq.modifiers.remove(mod)

# 重命名修饰符
def rename_modifier(seq, old_name, new_name):
    mod = seq.modifiers.get(old_name)
    if mod:
        mod.name = new_name
```

### 颜色修饰符

```python
# 色彩校正修饰符
def add_color_balance(seq):
    mod = seq.modifiers.new(name="Color Balance", type='COLOR_BALANCE')
    return mod

def set_color_balance(mod, lift=(0,0,0), gamma=(1,1,1), gain=(1,1,1)):
    mod.color_balance.lift_factor = lift
    mod.color_balance.gamma_factor = gamma
    mod.color_balance.gain_factor = gain

# 亮度对比度修饰符
def add_brightness_contrast(seq):
    mod = seq.modifiers.new(name="Brightness Contrast", type='BRIGHTCONTRAST')
    return mod

def set_brightness_contrast(mod, brightness=0, contrast=0):
    mod.brightness = brightness
    mod.contrast = contrast

# 色调曲线修饰符
def add_curves(seq):
    mod = seq.modifiers.new(name="Curves", type='CURVES')
    return mod

# 设置色调曲线
def set_curves(mod, points=[(0,0), (0.25, 0.25), (0.75, 0.75), (1,1)]):
    mod.mapping.curves = points

# 饱和度修饰符
def add_saturation(seq, factor=1.0):
    mod = seq.modifiers.new(name="Saturation", type='SATURATION')
    mod.factor = factor
    return mod
```

### 变换修饰符

```python
# 位置偏移修饰符
def add_transform(seq):
    mod = seq.modifiers.new(name="Transform", type='TRANSFORM')
    return mod

def set_transform(mod, offset_x=0, offset_y=0, scale_x=1.0, scale_y=1.0, rotation=0):
    mod.offset_x = offset_x
    mod.offset_y = offset_y
    mod.scale_x = scale_x
    mod.scale_y = scale_y
    mod.rotation = rotation

# 裁剪修饰符
def add_crop(seq):
    mod = seq.modifiers.new(name="Crop", type='CROP')
    return mod

def set_crop(mod, crop_left=0, crop_right=0, crop_top=0, crop_bottom=0):
    mod.crop_left = crop_left
    mod.crop_right = crop_right
    mod.crop_top = crop_top
    mod.crop_bottom = crop_bottom

# 文本不透明度修饰符
def add_text_opacity(seq):
    mod = seq.modifiers.new(name="Text Opacity", type='TEXT_OPACITY')
    return mod

def set_text_opacity(mod, opacity=1.0):
    mod.opacity = opacity
```

### 效果修饰符

```python
# 模糊修饰符
def add_blur_mod(seq):
    mod = seq.modifiers.new(name="Blur", type='BLUR')
    return mod

def set_blur(mod, size_x=10, size_y=10, fit_method='FIT', blur_method='GAUSS'):
    mod.size_x = size_x
    mod.size_y = size_y
    mod.fit_method = fit_method
    mod.blur_method = blur_method

# 残影修饰符
def add_ghosting(seq, intensity=0.5, decay=0.95):
    mod = seq.modifiers.new(name="Ghosting", type='GHOSTING')
    mod.intensity = intensity
    mod.decay = decay
    return mod

# 蒙版修饰符
def add_mask(seq, mask_strip):
    mod = seq.modifiers.new(name="Mask", type='MASK')
    mod.mask_type = 'SECONDARY'
    mod.mask_strip = mask_strip
    return mod

# 运动模糊修饰符
def add_motion_blur(seq):
    mod = seq.modifiers.new(name="Motion Blur", type='MOTION_BLUR')
    return mod

def set_motion_blur(mod, intensity=1.0):
    mod.intensity = intensity
```

## 多通道编辑

### 通道管理

```python
# 创建新通道
def create_channel(channel_type='VIDEO', above=None):
    scene = bpy.context.scene
    seq_editor = scene.sequence_editor
    
    # 获取最高通道
    max_channel = 0
    for seq in seq_editor.sequences:
        if seq.channel > max_channel:
            max_channel = seq.channel
    
    new_channel = max_channel + 1
    
    return new_channel

# 移动序列到通道
def move_to_channel(seq, new_channel):
    seq.channel = new_channel

# 交换两个序列的通道
def swap_channels(seq1, seq2):
    channel1 = seq1.channel
    channel2 = seq2.channel
    seq1.channel = channel2
    seq2.channel = channel1

# 垂直排列序列
def vertical_arrange(sequences):
    for i, seq in enumerate(sequences):
        seq.channel = i + 1
```

### 同步序列

```python
# 同步多个序列的开始帧
def sync_start_frames(sequences, reference_frame=None):
    if reference_frame is None:
        reference_frame = min(s.frame_start for s in sequences)
    
    for seq in sequences:
        seq.frame_start = reference_frame

# 同步时长
def sync_durations(sequences, reference_seq=None):
    if reference_seq is None:
        max_duration = max(s.frame_duration for s in sequences)
    else:
        max_duration = reference_seq.frame_duration
    
    for seq in sequences:
        if seq.frame_duration < max_duration:
            seq.frame_end = seq.frame_start + max_duration

# 对齐序列末端
def align_end_frames(sequences, reference_seq=None):
    if reference_seq is None:
        reference_end = max(s.frame_end for s in sequences)
    else:
        reference_end = reference_seq.frame_end
    
    for seq in sequences:
        offset = reference_end - seq.frame_end
        seq.frame_start += offset
```

## 时间线操作

### 播放控制

```python
# 播放视频
def play_sequence():
    bpy.ops.sequencer.view_all_preview()

# 逐帧前进
def frame_step_forward():
    scene = bpy.context.scene
    scene.frame_set(scene.frame_current + 1)

# 逐帧后退
def frame_step_backward():
    scene = bpy.context.scene
    scene.frame_set(scene.frame_current - 1)

# 跳转到序列开头
def jump_to_start(seq):
    scene = bpy.context.scene
    scene.frame_set(seq.frame_start)

# 跳转到序列结尾
def jump_to_end(seq):
    scene = bpy.context.scene
    scene.frame_set(seq.frame_end)

# 跳转到下一个切割点
def jump_to_next_cut():
    bpy.ops.sequencer.jump(next=True, center=True)

# 跳转到上一个切割点
def jump_to_prev_cut():
    bpy.ops.sequencer.jump(next=False, center=True)
```

### 标记和时间轴

```python
# 创建时间标记
def create_marker(name, frame, channel=0):
    scene = bpy.context.scene
    
    # 检查是否已存在
    marker = scene.timeline_markers.get(name)
    if marker:
        marker.frame = frame
    else:
        marker = scene.timeline_markers.new(name, frame=frame)
    
    return marker

# 创建场景标记
def create_scene_marker(scene_obj, name, frame):
    marker = scene_obj.timeline_markers.get(name)
    if marker:
        marker.frame = frame
    else:
        marker = scene_obj.timeline_markers.new(name, frame=frame)
    
    return marker

# 删除标记
def delete_marker(name):
    scene = bpy.context.scene
    marker = scene.timeline_markers.get(name)
    if marker:
        scene.timeline_markers.remove(marker)

# 列出所有标记
def list_markers():
    for marker in bpy.context.scene.timeline_markers:
        print(f"标记: {marker.name}, 帧: {marker.frame}")
```

## 渲染输出

### 序列渲染

```python
# 渲染序列到视频文件
def render_sequence_to_file(output_filepath, start_frame=None, end_frame=None):
    scene = bpy.context.scene
    
    # 设置帧范围
    if start_frame is None:
        start_frame = scene.frame_start
    if end_frame is None:
        end_frame = scene.frame_end
    
    scene.frame_start = start_frame
    scene.frame_end = end_frame
    
    # 设置输出路径
    scene.render.filepath = output_filepath
    scene.render.image_settings.file_format = 'FFMPEG'
    scene.render.ffmpeg.format = 'MPEG4'
    scene.render.ffmpeg.codec = 'H264'
    
    # 开始渲染
    bpy.ops.render.render(animation=True)

# 渲染设置
def setup_render_settings(engine='BLENDER_WORKBENCH', resolution_x=1920, resolution_y=1080):
    scene = bpy.context.scene
    
    scene.render.engine = engine
    scene.render.resolution_x = resolution_x
    scene.render.resolution_y = resolution_y
    scene.render.resolution_percentage = 100
    
    # 设置像素宽高比
    scene.render.pixel_aspect_x = 1.0
    scene.render.pixel_aspect_y = 1.0

# 设置输出格式
def set_output_format(format='MPEG4', codec='H264', quality='HIGH'):
    scene = bpy.context.scene
    
    scene.render.image_settings.file_format = 'FFMPEG'
    scene.render.ffmpeg.format = format
    scene.render.ffmpeg.codec = codec
    
    if quality == 'HIGH':
        scene.render.ffmpeg.video_bitrate = 8000
    elif quality == 'MEDIUM':
        scene.render.ffmpeg.video_bitrate = 4000
    else:
        scene.render.ffmpeg.video_bitrate = 2000
```

### 音频导出

```python
# 导出音频
def export_audio(output_filepath, start_frame=None, end_frame=None):
    scene = bpy.context.scene
    
    if start_frame is None:
        start_frame = scene.frame_start
    if end_frame is None:
        end_frame = scene.frame_end
    
    scene.render.filepath = output_filepath
    scene.render.image_settings.file_format = 'FFMPEG'
    scene.render.ffmpeg.audio_codec = 'AAC'
    scene.render.ffmpeg.audio_bitrate = 192
    
    # 只渲染音频
    bpy.ops.render.render(animation=True, write_still=False)
```

## 高级操作

### 批处理

```python
# 批量处理序列
def batch_process_sequences(sequences, process_func):
    results = []
    for seq in sequences:
        result = process_func(seq)
        results.append(result)
    return results

# 应用效果到多个序列
def apply_effect_to_all(effect_type, sequences, channel_offset=1):
    results = []
    for i, seq in enumerate(sequences):
        effect = create_effect_strip(effect_type, seq, channel=seq.channel + channel_offset)
        results.append(effect)
    return results

# 批量导入视频
def batch_import_videos(video_files, start_channel=1):
    strips = []
    current_frame = 1
    
    for i, filepath in enumerate(video_files):
        channel = start_channel + i
        strip = create_movie_strip(filepath, frame_start=current_frame, channel=channel)
        strips.append(strip)
        
        # 更新下一个视频的开始帧
        current_frame += strip.frame_duration + 1
    
    return strips
```

### 序列复制

```python
# 复制序列
def duplicate_sequence(seq, offset_frame=0, offset_channel=0):
    scene = bpy.context.scene
    
    # 创建新序列
    if seq.type == 'MOVIE':
        new_seq = scene.sequence_editor.sequences.new_movie(
            name=f"{seq.name} Copy",
            filepath=seq.filepath,
            frame_start=seq.frame_start + offset_frame,
            channel=seq.channel + offset_channel
        )
    elif seq.type == 'SOUND':
        new_seq = scene.sequence_editor.sequences.new_sound(
            name=f"{seq.name} Copy",
            filepath=seq.sound.filepath,
            frame_start=seq.frame_start + offset_frame,
            channel=seq.channel + offset_channel
        )
    else:
        # 通用复制方法
        new_seq = seq.copy()
        new_seq.frame_start = seq.frame_start + offset_frame
        new_seq.channel = seq.channel + offset_channel
        scene.sequence_editor.sequences.append(new_seq)
    
    return new_seq

# 批量复制序列
def duplicate_all_sequences(offset_frame=0, offset_channel=0):
    scene = bpy.context.scene
    original_sequences = list(scene.sequence_editor.sequences)
    
    for seq in original_sequences:
        duplicate_sequence(seq, offset_frame, offset_channel)
```

### 序列分析

```python
# 分析序列信息
def analyze_sequences():
    scene = bpy.context.scene
    seq_editor = scene.sequence_editor
    
    stats = {
        'total_count': len(seq_editor.sequences),
        'by_type': {},
        'by_channel': {},
        'total_duration': 0
    }
    
    for seq in seq_editor.sequences:
        # 按类型统计
        if seq.type not in stats['by_type']:
            stats['by_type'][seq.type] = 0
        stats['by_type'][seq.type] += 1
        
        # 按通道统计
        channel = seq.channel
        if channel not in stats['by_channel']:
            stats['by_channel'][channel] = 0
        stats['by_channel'][channel] += 1
        
        # 累计时长
        if seq.frame_end > stats['total_duration']:
            stats['total_duration'] = seq.frame_end
    
    return stats

# 获取序列使用率
def get_sequence_usage(seq):
    return {
        'duration': seq.frame_duration,
        'offset_start': seq.frame_offset_start,
        'offset_end': seq.frame_offset_end,
        'effective_duration': seq.frame_duration - seq.frame_offset_start - seq.frame_offset_end,
        'speed_factor': seq.speed_factor,
        'actual_duration': seq.frame_duration / seq.speed_factor
    }
```

### 场景转换动画

```python
# 创建场景切换效果
def create_scene_transition(scene1_seq, scene2_seq, trans_duration=24, trans_type='CROSS'):
    # 调整序列长度
    scene1_seq.frame_end = scene1_seq.frame_start + scene1_seq.frame_duration - trans_duration
    scene2_seq.frame_start = scene1_seq.frame_end
    
    # 创建转场
    trans = create_transition(scene1_seq, scene2_seq, trans_type, trans_duration)
    
    return trans

# 创建定格动画效果
def create_freeze_frame(video_seq, freeze_frame, duration=24):
    # 复制一帧
    freeze = duplicate_sequence(video_seq, offset_frame=0, offset_channel=1)
    freeze.name = "Freeze Frame"
    
    # 设置淡入效果
    freeze.fade_in = 0
    freeze.fade_out = 0
    
    # 偏移到定格帧
    freeze.frame_offset_start = freeze_frame - video_seq.frame_start
    freeze.frame_offset_end = freeze.frame_duration - freeze.frame_offset_start - duration
    
    return freeze

# 创建慢动作效果
def create_slow_motion(seq, start_frame, duration, slow_factor=2.0):
    # 创建slow motion段
    slow = duplicate_sequence(seq, offset_frame=start_frame - seq.frame_start)
    slow.name = "Slow Motion"
    slow.frame_start = start_frame
    slow.frame_duration = duration * slow_factor
    slow.speed_factor = slow_factor
    slow.frame_offset_end = duration
    
    return slow
```

## 最佳实践

### 性能优化

```python
# 禁用实时预览更新
def batch_edit(disable_preview=True):
    scene = bpy.context.scene
    seq_editor = scene.sequence_editor
    
    old_state = scene.sequence_editor_show_overlay
    if disable_preview:
        scene.sequence_editor_show_overlay = False
    
    try:
        # 执行批量操作
        pass
    finally:
        scene.sequence_editor_show_overlay = old_state

# 使用临时通道
def temp_channel_operation(channel, operation):
    scene = bpy.context.scene
    
    # 获取当前序列
    original_seqs = [s for s in scene.sequence_editor.sequences if s.channel == channel]
    
    # 临时移除
    for seq in original_seqs:
        seq.mute = True
    
    try:
        result = operation()
        return result
    finally:
        # 恢复
        for seq in original_seqs:
            seq.mute = False

# 延迟更新
def deferred_update():
    import bpy
    
    if hasattr(deferred_update, 'timer'):
        return
    
    deferred_update.timer = bpy.app.timers.register(
        lambda: process_updates() or None,
        first_interval=0.1
    )
```

### 项目管理

```python
# 清理未使用的资源
def cleanup_unused():
    scene = bpy.context.scene
    
    # 清理未使用的图像
    for img in list(bpy.data.images):
        if img.users == 0:
            bpy.data.images.remove(img)
    
    # 清理未使用的音频
    for sound in list(bpy.data.sounds):
        if sound.users == 0:
            bpy.data.sounds.remove(sound)
    
    # 清理未使用的场景
    for scn in list(bpy.data.scenes):
        if scn != scene and scn.users == 0:
            bpy.data.scenes.remove(scn)

# 收集所有资源路径
def collect_resource_paths():
    paths = set()
    
    for seq in bpy.context.scene.sequence_editor.sequences:
        if hasattr(seq, 'filepath') and seq.filepath:
            paths.add(bpy.path.abspath(seq.filepath))
        if hasattr(seq, 'sound') and seq.sound:
            paths.add(bpy.path.abspath(seq.sound.filepath))
    
    return paths

# 项目备份
def backup_project(backup_dir):
    import shutil
    import os
    
    # 保存当前文件
    if bpy.data.filepath:
        shutil.copy(bpy.data.filepath, os.path.join(backup_dir, "backup.blend"))
    
    # 收集并复制资源
    for path in collect_resource_paths():
        if os.path.exists(path):
            dest = os.path.join(backup_dir, os.path.basename(path))
            shutil.copy(path, dest)
```


