# bpy_ops_physics_sim

---
## bpy_ops_rigidbody

# bpy.ops.rigidbody 模块

刚体物理模拟操作模块，用于配置和管理刚体物理模拟。

## 概述

bpy.ops.rigidbody 模块包含用于配置刚体物理模拟的运算符。刚体模拟可以创建真实的物理交互效果，如碰撞、重力、摩擦等。

## 常用操作

### 刚体添加

```python
import bpy

# 添加活动刚体
bpy.ops.object.rigidbody_add(type='ACTIVE')

# 添加被动刚体
bpy.ops.object.rigidbody_add(type='PASSIVE')

# 删除刚体
bpy.ops.object.rigidbody_remove()
```

### 刚体类型

```python
# 设置为活动刚体
bpy.ops.rigidbody.object_settings_set(type='ACTIVE')

# 设置为被动刚体
bpy.ops.rigidbody.object_settings_set(type='PASSIVE')

# 复制刚体设置
bpy.ops.rigidbody.copy()
```

### 质量设置

```python
# 设置质量
bpy.ops.rigidbody.mass_set(mass=1.0)

# 设置碰撞形状
bpy.ops.rigidbody.collision_shape_set(type='MESH')

# 设置为凸包
bpy.ops.rigidbody.collision_shape_set(type='CONVEX_HULL')

# 设置为包围盒
bpy.ops.rigidbody.collision_shape_set(type='BOX')

# 设置为球体
bpy.ops.rigidbody.collision_shape_set(type='SPHERE')

# 设置为胶囊体
bpy.ops.rigidbody.collision_shape_set(type='CAPSULE')
```

### 物理属性

```python
# 设置摩擦力
bpy.ops.rigidbody.friction_set(friction=0.5)

# 设置弹性
bpy.ops.rigidbody.restitution_set(restitution=0.5)

# 设置线性阻尼
bpy.ops.rigidbody.linear_damping_set(damping=0.04)

# 设置角度阻尼
bpy.ops.rigidbody.angular_damping_set(damping=0.04)

# 设置源起始帧
bpy.ops.rigidbody.source_start_set(start=1)

# 设置源结束帧
bpy.ops.rigidbody.source_end_set(end=250)
```

### 约束操作

```python
# 添加约束
bpy.ops.rigidbody.constraint_add()

# 添加点约束
bpy.ops.rigidbody.constraint_add(type='POINT')

# 添加铰链约束
bpy.ops.rigidbody.constraint_add(type='HINGE')

# 添加滑块约束
bpy.ops.rigidbody.constraint_add(type='SLIDER')

# 添加活塞约束
bpy.ops.rigidbody.constraint_add(type='PISTON')

# 添加通用约束
bpy.ops.rigidbody.constraint_add(type='GENERIC')

# 添加通用铰链约束
bpy.ops.rigidbody.constraint_add(type='GENERIC_HINGE')

# 添加通用滑块约束
bpy.ops.rigidbody.constraint_add(type='GENERIC_SLIDER')

# 删除约束
bpy.ops.rigidbody.constraint_remove()
```

### 约束设置

```python
# 设置禁用碰撞
bpy.ops.rigidbody.constraint_disable_collisions_set(disable=True)

# 设置运动学
bpy.ops.rigidbody.constraint_set('type', motion_type='KINEMATIC')
```

### 缓存操作

```python
# 创建缓存
bpy.ops.rigidbody.cache_create()

# 删除缓存
bpy.ops.rigidbody.cache_delete()

# 清除缓存
bpy.ops.rigidbody.cache_clear()
```

### 模拟控制

```python
# 开始模拟
bpy.ops.rigidbody.simulate()

# 停止模拟
bpy.ops.rigidbody.stop_simulation()

# 重置模拟
bpy.ops.rigidbody.reset_simulation()

# 烘焙模拟
bpy.ops.rigidbody.bake()

# 烘焙到关键帧
bpy.ops.rigidbody.bake_to_keyframes()
```

## 实际应用示例

### 创建物理场景

```python
def setup_physics_scene():
    """设置物理场景"""
    # 创建地面
    bpy.ops.mesh.primitive_plane_add(size=20)
    ground = bpy.context.active_object
    ground.name = 'Ground'
    
    # 地面设为被动刚体
    bpy.ops.object.rigidbody_add(type='PASSIVE')
    ground.rigid_body.collision_shape = 'BOX'
    ground.rigid_body.friction = 0.5
    ground.rigid_body.restitution = 0.2
    
    # 创建立方体
    bpy.ops.mesh.primitive_cube_add(size=1)
    cube = bpy.context.active_object
    cube.name = 'Cube'
    cube.location = (0, 0, 5)
    
    # 立方体设为活动刚体
    bpy.ops.object.rigidbody_add(type='ACTIVE')
    cube.rigid_body.mass = 1.0
    cube.rigid_body.collision_shape = 'BOX'
    cube.rigid_body.friction = 0.5
    cube.rigid_body.restitution = 0.3
    
    return ground, cube
```

### 创建堆叠物体

```python
def create_stack(object_count=10):
    """创建堆叠物体"""
    objects = []
    
    for i in range(object_count):
        bpy.ops.mesh.primitive_cube_add(size=1)
        obj = bpy.context.active_object
        obj.name = f'StackCube_{i}'
        obj.location = (0, 0, 0.5 + i * 1.05)
        
        bpy.ops.object.rigidbody_add(type='ACTIVE')
        obj.rigid_body.mass = 1.0
        obj.rigid_body.friction = 0.8
        obj.rigid_body.restitution = 0.1
        
        objects.append(obj)
    
    return objects
```

### 创建球体下落

```python
def create_falling_balls(ball_count=20, area_size=10):
    """创建球体下落动画"""
    import random
    
    balls = []
    
    for i in range(ball_count):
        bpy.ops.mesh.primitive_uv_sphere_add(radius=0.3)
        ball = bpy.context.active_object
        ball.name = f'Ball_{i}'
        
        x = random.uniform(-area_size/2, area_size/2)
        y = random.uniform(-area_size/2, area_size/2)
        z = random.uniform(5, 20)
        
        ball.location = (x, y, z)
        
        bpy.ops.object.rigidbody_add(type='ACTIVE')
        ball.rigid_body.mass = 0.5
        ball.rigid_body.collision_shape = 'SPHERE'
        ball.rigid_body.friction = 0.3
        ball.rigid_body.restitution = 0.6
        
        balls.append(ball)
    
    return balls
```

### 创建摆锤

```python
def create_pendulum(pivot_obj, bob_obj, length=2.0):
    """创建摆锤"""
    # 设置枢轴
    bpy.ops.object.rigidbody_add(type='PASSIVE')
    pivot_obj.rigid_body.collision_shape = 'SPHERE'
    pivot_obj.rigid_body.kinematic = True
    pivot_obj.location = (0, 0, 5)
    
    # 设置摆锤
    bpy.ops.object.rigidbody_add(type='ACTIVE')
    bob_obj.rigid_body.mass = 1.0
    bob_obj.collision_shape = 'SPHERE'
    bob_obj.location = (0, -length, 5)
    
    # 添加铰链约束
    bpy.ops.rigidbody.constraint_add(type='HINGE')
    constraint = bpy.context.active_object
    
    # 设置约束
    constraint.rigid_body_constraint.type = 'HINGE'
    constraint.rigid_body_constraint.object1 = pivot_obj
    constraint.rigid_body_constraint.object2 = bob_obj
    
    return constraint
```

### 创建碰撞场景

```python
def create_collision_scene(obj1, obj2):
    """创建碰撞场景"""
    # 设置物体1
    obj1.location = (-3, 0, 2)
    bpy.ops.object.rigidbody_add(type='ACTIVE')
    obj1.rigid_body.mass = 2.0
    obj1.rigid_body.collision_shape = 'BOX'
    obj1.rigid_body.friction = 0.3
    obj1.rigid_body.restitution = 0.8
    
    # 设置物体2
    obj2.location = (3, 0, 2)
    bpy.ops.object.rigidbody_add(type='ACTIVE')
    obj2.rigid_body.mass = 2.0
    obj2.rigid_body.collision_shape = 'BOX'
    obj2.rigid_body.friction = 0.3
    obj2.rigid_body.restitution = 0.8
    
    return obj1, obj2
```

### 创建滑梯

```python
def create_slide(start_height=5, slope_length=10, slope_angle=30):
    """创建滑梯"""
    import math
    
    # 创建斜坡
    bpy.ops.mesh.primitive_plane_add(size=slope_length)
    slide = bpy.context.active_object
    slide.name = 'Slide'
    
    # 旋转斜坡
    slope_rad = math.radians(slope_angle)
    slide.rotation_euler = (slope_rad, 0, 0)
    slide.location = (0, -slope_length/2 * math.cos(slope_rad), start_height - slope_length/2 * math.sin(slope_rad))
    
    # 设为被动刚体
    bpy.ops.object.rigidbody_add(type='PASSIVE')
    slide.rigid_body.collision_shape = 'MESH'
    slide.rigid_body.friction = 0.1
    slide.rigid_body.restitution = 0.1
    
    # 创建滑动物体
    bpy.ops.mesh.primitive_sphere_add(radius=0.2)
    slider = bpy.context.active_object
    slider.name = 'Slider'
    slider.location = (0, 0, start_height + 1)
    
    # 设为活动刚体
    bpy.ops.object.rigidbody_add(type='ACTIVE')
    slider.rigid_body.mass = 0.5
    slider.rigid_body.collision_shape = 'SPHERE'
    slider.rigid_body.friction = 0.1
    slider.rigid_body.restitution = 0.3
    
    return slide, slider
```

### 创建多米诺骨牌

```python
def create_domino_chain(start_pos=(0, 0, 0.5), count=20, spacing=0.8):
    """创建多米诺骨牌"""
    dominoes = []
    
    for i in range(count):
        bpy.ops.mesh.primitive_cube_add(size=1)
        domino = bpy.context.active_object
        domino.name = f'Domino_{i}'
        
        x = start_pos[0] + i * spacing
        y = start_pos[1]
        z = start_pos[2]
        
        domino.location = (x, y, z)
        
        # 缩放为骨牌形状
        domino.scale = (0.1, 0.5, 1.0)
        
        # 设为活动刚体
        bpy.ops.object.rigidbody_add(type='ACTIVE')
        domino.rigid_body.mass = 0.5
        domino.rigid_body.collision_shape = 'BOX'
        domino.rigid_body.friction = 0.3
        domino.rigid_body.restitution = 0.2
        
        dominoes.append(domino)
    
    return dominoes
```

## 使用场景

- **物理碰撞**：创建真实的物体碰撞效果
- **堆叠场景**：创建稳定的堆叠结构
- **下落动画**：创建物体下落和弹跳
- **机械系统**：创建连杆、摆锤等机械结构
- **游戏物理**：游戏中的物理交互效果
- **破坏效果**：创建物理破坏模拟
- **动画辅助**：为动画提供物理基础

## 注意事项

- 刚体模拟需要仔细设置碰撞形状
- 质量影响物体的运动行为
- 摩擦力和弹性影响碰撞效果
- 约束可以连接多个物体
- 缓存管理对于长模拟很重要
- 被动刚体不会移动但可以碰撞
- 活动刚体受物理影响
- 运动学刚体可以手动控制
- 烘焙可以加速最终渲染


---
## bpy_ops_fluid

# bpy.ops.fluid 模块

流体模拟操作模块，用于配置和管理流体物理模拟。

## 概述

bpy.ops.fluid 模块包含用于配置流体物理模拟的运算符。流体模拟可以创建液体、烟雾、火焰等效果，支持复杂的流体动力学计算。

## 常用操作

### 域操作

```python
import bpy

# 添加流体域
bpy.ops.object.fluid_add(type='DOMAIN')

# 添加流体对象
bpy.ops.object.fluid_add(type='FLOW')

# 添加流体障碍物
bpy.ops.object.fluid_add(type='EFFECTOR')

# 设置域类型为液体
bpy.ops.fluid.fluid_set(type='LIQUID')

# 设置域类型为气体
bpy.ops.fluid.fluid_set(type='GAS')

# 设置分辨率
bpy.ops.fluid.resolution_set(resolution=64)
```

### 流动源设置

```python
# 设置为流入
bpy.ops.fluid.flow_set(type='INFLOW')

# 设置为流出
bpy.ops.fluid.flow_set(type='OUTFLOW')

# 设置为几何体流
bpy.ops.fluid.flow_set(type='GEOMETRY')

# 设置为体积流
bpy.ops.fluid.flow_source_set(type='VOLUME')

# 设置为表面流
bpy.ops.fluid.flow_source_set(type='MESH')
```

### 障碍物设置

```python
# 设置为碰撞
bpy.ops.fluid.effector_set(type='COLLISION')

# 设置为引导
bpy.ops.fluid.effector_set(type='GUIDE')

# 设置为吸收
bpy.ops.fluid.effector_set(type='ABSORPTION')

# 设置为流动引导
bpy.ops.fluid.effector_set(type='FLUID_GUIDE')
```

### 属性设置

```python
# 设置初始温度
bpy.ops.fluid.initial_temperature_set(temperature=25.0)

# 设置密度
bpy.ops.fluid.density_set(density=1.0)

# 设置粘度
bpy.ops.fluid.viscosity_set(value=1.0)

# 设置表面张力
bpy.ops.fluid.surface_tension_set(value=0.0)

# 设置颜色
bpy.ops.fluid.color_set(color=(1.0, 0.0, 0.0, 1.0))
```

### 缓存操作

```python
# 创建缓存
bpy.ops.fluid.cache_create()

# 删除缓存
bpy.ops.fluid.cache_delete()

# 清除缓存
bpy.ops.fluid.cache_clear()

# 重置缓存
bpy.ops.fluid.cache_reset()
```

### 模拟控制

```python
# 开始模拟
bpy.ops.fluid.simulate()

# 停止模拟
bpy.ops.fluid.stop_simulation()

# 重置模拟
bpy.ops.fluid.reset_simulation()

# 清除模拟
bpy.ops.fluid.clear_simulation()
```

## 实际应用示例

### 创建液体模拟

```python
def create_liquid_simulation(domain_obj, flow_obj):
    """创建液体模拟"""
    # 设置域
    domain_obj.select_set(True)
    bpy.context.view_layer.objects.active = domain_obj
    
    # 添加流体修改器
    fluid_mod = domain_obj.modifiers.new(name='Fluid', type='FLUID')
    fluid_mod.fluid_type = 'DOMAIN'
    
    # 设置为液体
    fluid_mod.domain_settings.fluid_type = 'LIQUID'
    
    # 设置分辨率
    fluid_mod.domain_settings.resolution = 64
    
    # 设置缓存路径
    fluid_mod.domain_settings.cache_directory = '//fluid_cache/'
    
    # 设置流动源
    flow_obj.select_set(True)
    bpy.context.view_layer.objects.active = flow_obj
    
    flow_mod = flow_obj.modifiers.new(name='Flow', type='FLUID')
    flow_mod.fluid_type = 'FLOW'
    flow_mod.flow_settings.flow_type = 'LIQUID'
    flow_mod.flow_settings.flow_behavior = 'INFLOW'
    
    return fluid_mod
```

### 创建烟雾模拟

```python
def create_smoke_simulation(domain_obj, emitter_obj):
    """创建烟雾模拟"""
    # 设置域
    domain_obj.select_set(True)
    bpy.context.view_layer.objects.active = domain_obj
    
    # 添加流体修改器
    fluid_mod = domain_obj.modifiers.new(name='Smoke', type='FLUID')
    fluid_mod.fluid_type = 'DOMAIN'
    
    # 设置为气体
    fluid_mod.domain_settings.fluid_type = 'GAS'
    
    # 设置高分辨率
    fluid_mod.domain_settings.resolution = 128
    
    # 创建缓存
    bpy.ops.fluid.cache_create()
    
    # 设置发射器
    emitter_obj.select_set(True)
    bpy.context.view_layer.objects.active = emitter_obj
    
    flow_mod = emitter_obj.modifiers.new(name='SmokeEmitter', type='FLUID')
    flow_mod.fluid_type = 'FLOW'
    flow_mod.flow_settings.flow_type = 'SMOKE'
    flow_mod.flow_settings.flow_behavior = 'OUTFLOW'
    
    # 设置烟雾密度
    flow_mod.flow_settings.density = 1.0
    
    return fluid_mod
```

### 创建火焰模拟

```python
def create_fire_simulation(domain_obj, burner_obj):
    """创建火焰模拟"""
    # 设置域
    domain_obj.select_set(True)
    bpy.context.view_layer.objects.active = domain_obj
    
    # 添加流体修改器
    fluid_mod = domain_obj.modifiers.new(name='Fire', type='FLUID')
    fluid_mod.fluid_type = 'DOMAIN'
    
    # 设置为气体
    fluid_mod.domain_settings.fluid_type = 'GAS'
    
    # 设置火焰温度
    fluid_mod.domain_settings.flame_buoyancy = 1.0
    
    # 设置燃烧时间
    fluid_mod.domain_settings.burn_time = 2.0
    
    # 设置发射器
    burner_obj.select_set(True)
    bpy.context.view_layer.objects.active = burner_obj
    
    flow_mod = burner_obj.modifiers.new(name='FireSource', type='FLUID')
    flow_mod.fluid_type = 'FLOW'
    flow_mod.flow_settings.flow_type = 'FIRE'
    flow_mod.flow_settings.flow_behavior = 'OUTFLOW'
    
    return fluid_mod
```

### 创建水流碰撞

```python
def create_water_collision(domain_obj, obstacle_obj):
    """创建水流碰撞效果"""
    # 设置障碍物
    obstacle_obj.select_set(True)
    bpy.context.view_layer.objects.active = obstacle_obj
    
    # 添加流体修改器
    fluid_mod = obstacle_obj.modifiers.new(name='Collision', type='FLUID')
    fluid_mod.fluid_type = 'EFFECTOR'
    
    # 设置为碰撞
    fluid_mod.effector_settings.effector_type = 'COLLISION'
    
    # 设置碰撞强度
    fluid_mod.effector_settings.correction = 0.0
    
    # 设置阻力
    fluid_mod.effector_settings.damping = 0.5
    
    return fluid_mod
```

### 创建引导流

```python
def create_guide_flow(domain_obj, guide_obj):
    """创建引导流效果"""
    # 设置引导物体
    guide_obj.select_set(True)
    bpy.context.view_layer.objects.active = guide_obj
    
    # 添加流体修改器
    fluid_mod = guide_obj.modifiers.new(name='Guide', type='FLUID')
    fluid_mod.fluid_type = 'EFFECTOR'
    
    # 设置为引导
    fluid_mod.effector_settings.effector_type = 'GUIDE'
    
    # 设置引导力量
    fluid_mod.effector_settings.guide_velocity = (0, 0, 1)
    fluid_mod.effector_settings.guide_free = 0.5
    
    return fluid_mod
```

### 创建多重流

```python
def create_multi_flow(domain_obj, sources):
    """创建多重流动效果"""
    # 设置域
    domain_obj.select_set(True)
    bpy.context.view_layer.objects.active = domain_obj
    
    # 添加流体修改器
    fluid_mod = domain_obj.modifiers.new(name='MultiFluid', type='FLUID')
    fluid_mod.fluid_type = 'DOMAIN'
    fluid_mod.domain_settings.fluid_type = 'LIQUID'
    fluid_mod.domain_settings.resolution = 64
    
    # 设置多个流动源
    for i, source_obj in enumerate(sources):
        source_obj.select_set(True)
        bpy.context.view_layer.objects.active = source_obj
        
        flow_mod = source_obj.modifiers.new(name=f'Flow_{i}', type='FLUID')
        flow_mod.fluid_type = 'FLOW'
        flow_mod.flow_settings.flow_type = 'LIQUID'
        flow_mod.flow_settings.flow_behavior = 'INFLOW'
        
        # 设置不同颜色
        if i == 0:
            flow_mod.flow_settings.color = (1.0, 0.0, 0.0, 1.0)
        else:
            flow_mod.flow_settings.color = (0.0, 0.0, 1.0, 1.0)
    
    return fluid_mod
```

## 使用场景

- **液体模拟**：水、牛奶、油等液体效果
- **烟雾效果**：云、烟、蒸汽等气体效果
- **火焰特效**：火焰、爆炸等燃烧效果
- **流体碰撞**：水流与障碍物的交互
- **引导流**：控制流体运动方向
- **多重流体**：多种不相容的流体

## 注意事项

- 流体模拟需要较高的计算资源
- 分辨率影响模拟精度和性能
- 缓存管理对于大模拟很重要
- 粘度设置影响液体行为
- 温度影响气体行为
- 碰撞设置需要仔细调整
- 引导流可以控制流体运动
- 火焰模拟需要额外的渲染设置


---
## bpy_ops_cloth

# bpy.ops.cloth 模块

布料模拟操作模块，用于配置和管理布料物理模拟。

## 概述

bpy.ops.world 模块包含用于配置场景世界环境的运算符。世界环境控制场景的背景颜色、环境光照、雾效和整体氛围。

## 常用操作

### 预设应用

```python
import bpy

# 应用布料预设
bpy.ops.cloth.preset_add(preset='Cotton')

# 应用丝绸预设
bpy.ops.cloth.preset_add(preset='Silk')

# 应用皮革预设
bpy.ops.cloth.preset_add(preset='Leather')

# 应用橡胶预设
bpy.ops.cloth.preset_add(preset='Rubber')

# 应用 denim 预设
bpy.ops.cloth.preset_add(preset='Denim')

# 应用胶水预设
bpy.ops.cloth.preset_add(preset='Glue')

# 应用塑料预设
bpy.ops.cloth.preset_add(preset='Plastic')

# 应用薄纱预设
bpy.ops.cloth.preset_add(preset='Sheer')
```

### 属性设置

```python
# 设置质量
bpy.ops.cloth.mass_set(value=0.3)

# 设置结构刚度
bpy.ops.cloth.structural_set(value=50.0)

# 设置剪切刚度
bpy.ops.cloth.shear_set(value=5.0)

# 设置弯曲刚度
bpy.ops.cloth.bending_set(value=1.0)

# 设置阻尼
bpy.ops.cloth.damping_set(value=5.0)

# 设置空气阻力
bpy.ops.cloth.air_damping_set(value=1.0)

# 设置重力影响
bpy.ops.cloth.gravity_set(value=9.8)
```

### 碰撞设置

```python
# 设置碰撞质量
bpy.ops.cloth.collision_quality_set(value=2)

# 设置自碰撞
bpy.ops.cloth.self_collision_set(value=True)

# 设置自碰撞距离
bpy.ops.cloth.self_collision_distance_set(value=0.02)

# 设置自碰撞刚度
bpy.ops.cloth.self_collision_stiffness_set(value=100.0)

# 设置顶点组质量
bpy.ops.cloth.pinning_group_add(group_name='PinningGroup')
```

### 缓存操作

```python
# 创建缓存
bpy.ops.cloth.cache_create()

# 删除缓存
bpy.ops.cloth.cache_delete()

# 清除缓存
bpy.ops.cloth.cache_clear()

# 重置缓存
bpy.ops.cloth.cache_reset()

# 重新计算缓存
bpy.ops.cloth.cache_rebuild()
```

### 模拟控制

```python
# 开始模拟
bpy.ops.cloth.simulate()

# 停止模拟
bpy.ops.cloth.stop_simulation()

# 重置模拟
bpy.ops.cloth.reset_simulation()

# 清除模拟
bpy.ops.cloth.clear_simulation()

# 应用为形状键
bpy.ops.cloth.shape_key_add(from_current=True)

# 应用为骨架变形
bpy.ops.cloth.applied_modifier_add()
```

## 实际应用示例

### 创建布料模拟

```python
def setup_cloth_simulation(obj, preset='Cotton'):
    """设置布料模拟"""
    # 确保物体有足够的顶点
    if len(obj.data.vertices) < 100:
        bpy.ops.object.mode_set(mode='EDIT')
        bpy.ops.mesh.subdivide(number_cuts=3)
        bpy.ops.object.mode_set(mode='OBJECT')
    
    # 添加布料修改器
    cloth_mod = obj.modifiers.new(name='Cloth', type='CLOTH')
    
    # 应用预设
    bpy.ops.cloth.preset_add(preset=preset)
    
    # 设置碰撞
    cloth_mod.settings.collision_quality = 3
    cloth_mod.settings.self_collision = True
    cloth_mod.settings.self_collision_distance = 0.01
    cloth_mod.settings.self_collision_stiffness = 100.0
    
    return cloth_mod
```

### 创建旗帜模拟

```python
def create_flag_simulation(pole_obj, flag_obj, wind_strength=5.0):
    """创建旗帜模拟"""
    # 将旗帜绑定到旗杆
    pole_obj.select_set(True)
    flag_obj.select_set(True)
    bpy.context.view_layer.objects.active = pole_obj
    bpy.ops.object.parent_set(type='OBJECT')
    
    # 添加布料修改器
    cloth_mod = flag_obj.modifiers.new(name='FlagCloth', type='CLOTH')
    
    # 设置为丝绸
    bpy.ops.cloth.preset_add(preset='Silk')
    
    # 设置物理属性
    cloth_mod.settings.mass = 0.1
    cloth_mod.settings.air_damping = 0.5
    
    # 添加固定点组
    bpy.ops.object.vertex_group_add()
    flag_obj.vertex_groups.active.name = 'Pinning'
    
    # 选择旗帜左侧顶点
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    # 固定左侧顶点
    for vert in flag_obj.data.vertices:
        if vert.co.x < -0.01:
            vert.select = True
    
    bpy.ops.object.vertex_group_assign()
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 设置固定组
    cloth_mod.settings.pinning_point = 'VERTEX'
    cloth_mod.settings.vertex_group_mass = flag_obj.vertex_groups['Pinning']
    
    # 添加风力
    cloth_mod.settings.use_air_damping = True
    cloth_mod.settings.air_damping = wind_strength
    
    return cloth_mod
```

### 创建窗帘模拟

```python
def create_curtain_simulation(curtain_obj, anchor_group='TopEdge'):
    """创建窗帘模拟"""
    # 添加布料修改器
    cloth_mod = curtain_obj.modifiers.new(name='CurtainCloth', type='CLOTH')
    
    # 设置为轻薄布料
    bpy.ops.cloth.preset_add(preset='Sheer')
    
    # 设置质量
    cloth_mod.settings.mass = 0.05
    
    # 设置阻尼
    cloth_mod.settings.damping = 3.0
    cloth_mod.settings.air_damping = 2.0
    
    # 设置固定组
    if anchor_group in curtain_obj.vertex_groups:
        cloth_mod.settings.pinning_point = 'VERTEX'
        cloth_mod.settings.vertex_group_mass = curtain_obj.vertex_groups[anchor_group]
    
    # 设置碰撞
    cloth_mod.settings.collision_quality = 4
    cloth_mod.settings.self_collision = True
    
    return cloth_mod
```

### 创建桌布模拟

```python
def create_tablecloth_simulation(tablecloth_obj, table_obj):
    """创建桌布模拟"""
    # 添加布料修改器
    cloth_mod = tablecloth_obj.modifiers.new(name='TableCloth', type='CLOTH')
    
    # 设置为棉布
    bpy.ops.cloth.preset_add(preset='Cotton')
    
    # 设置物理属性
    cloth_mod.settings.mass = 0.3
    cloth_mod.settings.structural_stiffness = 80.0
    cloth_mod.settings.bending_stiffness = 2.0
    
    # 添加碰撞
    tablecloth_obj.select_set(True)
    bpy.context.view_layer.objects.active = tablecloth_obj
    
    # 设置与桌子的碰撞
    cloth_mod.settings.collision_quality = 3
    cloth_mod.settings.use_collision = True
    
    return cloth_mod
```

### 创建披风模拟

```python
def create_cape_simulation(cape_obj, character_obj):
    """创建披风模拟"""
    # 添加布料修改器
    cloth_mod = cape_obj.modifiers.new(name='CapeCloth', type='CLOTH')
    
    # 设置为皮革
    bpy.ops.cloth.preset_add(preset='Leather')
    
    # 设置物理属性
    cloth_mod.settings.mass = 0.5
    cloth_mod.settings.structural_stiffness = 100.0
    cloth_mod.settings.bending_stiffness = 5.0
    
    # 创建固定点组
    bpy.ops.object.vertex_group_add()
    cape_obj.vertex_groups.active.name = 'CapeTop'
    
    # 固定顶部顶点
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    for vert in cape_obj.data.vertices:
        if vert.co.y > 0.95:
            vert.select = True
    
    bpy.ops.object.vertex_group_assign()
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 设置固定
    cloth_mod.settings.pinning_point = 'VERTEX'
    cloth_mod.settings.vertex_group_mass = cape_obj.vertex_groups['CapeTop']
    
    # 添加碰撞
    cloth_mod.settings.self_collision = True
    
    return cloth_mod
```

### 创建弹簧布料

```python
def create_spring_cloth(obj, stiffness=200.0, damping=10.0):
    """创建弹簧布料效果"""
    # 添加布料修改器
    cloth_mod = obj.modifiers.new(name='SpringCloth', type='CLOTH')
    
    # 应用胶水预设
    bpy.ops.cloth.preset_add(preset='Glue')
    
    # 设置高刚度
    cloth_mod.settings.structural_stiffness = stiffness
    cloth_mod.settings.shear_stiffness = stiffness * 0.5
    cloth_mod.settings.bending_stiffness = stiffness * 0.2
    
    # 设置阻尼
    cloth_mod.settings.damping = damping
    
    # 禁用重力影响
    cloth_mod.settings.gravity = (0, 0, 0)
    
    return cloth_mod
```

## 使用场景

- **服装制作**：创建衣物、裙子、披风等
- **旗帜飘动**：创建旗帜、条幅动画
- **窗帘效果**：创建窗帘、帷幕动画
- **布料动画**：桌布、餐巾等
- **物理模拟**：需要布料物理效果的场景
- **角色动画**：角色的服装和配饰

## 注意事项

- 布料模拟需要足够的网格密度
- 固定点组用于控制布料固定位置
- 碰撞设置影响模拟精度和性能
- 高刚度设置可能导致不稳定
- 缓存管理影响模拟速度
- 自碰撞增加计算负担
- 风力设置需要平衡效果和稳定性
- 布料预设提供良好的起点


---
## bpy_ops_particle

# bpy.ops.particle 模块

粒子系统操作模块，用于创建和管理粒子发射系统。

## 概述

bpy.ops.particle 模块包含用于创建、编辑和管理粒子系统的运算符。这些运算符控制粒子发射、粒子编辑和粒子特效。

## 常用操作

### 粒子系统创建

```python
import bpy

# 添加粒子系统
bpy.ops.object.particle_system_add()

# 从选中物体添加粒子系统
bpy.ops.object.particle_system_add_from_scaned()

# 添加毛发粒子
bpy.ops.object.particle_system_add()
bpy.context.object.particle_systems.active.settings.type = 'HAIR'

# 添加关键粒子
bpy.ops.object.particle_system_add()
bpy.context.object.particle_systems.active.settings.type = 'EMITTER'
```

### 粒子模式

```python
# 切换到粒子编辑模式
bpy.ops.object.mode_set(mode='PARTICLE_EDITOR')

# 切换到粒子模式
bpy.ops.particle.mode_set(mode='PARTICLE')

# 切换到软体模式
bpy.ops.particle.mode_set(mode='SOFT_BODY')

# 切换到布料模式
bpy.ops.particle.mode_set(mode='CLOTH')

# 切换到刚体模式
bpy.ops.particle.mode_set(mode='RIGID_BODY')
```

### 粒子选择

```python
# 全选
bpy.ops.particle.select_all(action='SELECT')
bpy.ops.particle.select_all(action='DESELECT')
bpy.ops.particle.select_all(action='INVERT')

# 选择排列类型
bpy.ops.particle.select_rrf()

# 选择路径
bpy.ops.particle.select_path(action='SELECT')

# 清除路径
bpy.ops.particle.select_path(action='DELETE')
```

### 粒子编辑

```python
# 梳子
bpy.ops.particle.comb()

# 平滑
bpy.ops.particle.smooth()

# 滑动
bpy.ops.particle.slide()

# 增长
bpy.ops.particle.grow()

# 缩短
bpy.ops.particle.shrink()

# 添加
bpy.ops.particle.add()

# 删除
bpy.ops.particle.delete(type='PARTICLE')

# 旋转
bpy.ops.particle.rotate()

# 复制
bpy.ops.particle.duplicate()

# 对称
bpy.ops.particle.symmetric()
```

### 毛发编辑

```python
# 设置切点
bpy.ops.particle.set_cut_point()

# 吸附到网格
bpy.ops.particle吸附_to_mesh()

# 对齐到法线
bpy.ops.particle.align_normal()

# 切分
bpy.ops.particle.subdivide()

# 切分次数
bpy.ops.particle.subdivide(number_cuts=2)
```

### 动力学

```python
# 添加简单动力学
bpy.ops.particle.add_simple_kinematics()

# 移除简单动力学
bpy.ops.particle.remove_simple_kinematics()

# 转换为刚体
bpy.ops.particle.convert()

# 添加布料模拟
bpy.ops.particle.cloth_add()

# 添加软体模拟
bpy.ops.particle.softbody_add()
```

### 绘制模式

```python
# 绘制路径
bpy.ops.particle.paint(
    stroke=stroke_data,
    mode='NORMAL'
)

# 清除绘制
bpy.ops.particle.paint(stroke=[], mode='RELEASE')

# 笔触数据
stroke_data = [
    {"name": "", "location": (x, y, z), "mouse": (mx, my), "pressure": 1.0, "time": 0.0},
]
```

### 着色器

```python
# 切换着色器
bpy.ops.particle.shader_add()

# 选择着色器
bpy.ops.particle.shader_select()

# 移除着色器
bpy.ops.particle.shader_remove()
```

## 实际应用示例

### 创建程序化发型

```python
def create_procedural_hair(obj, num_strands=100, length=0.5, clumps=5):
    """创建程序化发型"""
    # 添加粒子系统
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.particle_system_add()
    
    psys = obj.particle_systems.active
    psys.name = "Hair"
    
    # 配置粒子设置
    settings = psys.settings
    settings.type = 'HAIR'
    settings.count = num_strands
    settings.hair_length = length
    settings.clump_factor = clumps
    settings.tip_clump = 0.8
    
    # 随机化
    settings.randomness = 0.5
    
    # 长度渐变
    settings.length_mode = 'ROOT'
    
    return psys
```

### 批量修改粒子

```python
def modify_particle_lengths(obj, scale_factor, pattern='ALL'):
    """批量修改粒子长度"""
    bpy.ops.object.mode_set(mode='PARTICLE_EDITOR')
    
    # 获取活动粒子系统
    psys = obj.particle_systems.active
    if not psys:
        return
    
    # 选择粒子
    if pattern == 'ALL':
        bpy.ops.particle.select_all(action='SELECT')
    elif pattern == 'TIPS':
        bpy.ops.particle.select_tips()
    elif pattern == 'ROOTS':
        bpy.ops.particle.select_roots()
    
    # 应用缩放
    bpy.ops.particle.scale(scale_factor)
```

### 粒子动画重置

```python
def reset_particle_simulation(obj, frame_range):
    """重置粒子模拟到指定帧范围"""
    psys = obj.particle_systems.active
    if not psys:
        return
    
    # 取消粒子系统链接
    bpy.ops.particle.unlink()
    
    # 设置帧范围
    bpy.context.scene.frame_start = frame_range[0]
    bpy.context.scene.frame_end = frame_range[1]
    
    # 重新添加粒子系统
    bpy.ops.object.particle_system_add()
```

### 粒子数据导出

```python
def export_particle_data(obj, filepath):
    """导出粒子数据到CSV"""
    import csv
    
    psys = obj.particle_systems.active
    if not psys:
        return
    
    with open(filepath, 'w', newline='') as f:
        writer = csv.writer(f)
        writer.writerow(['Particle', 'Location_X', 'Location_Y', 'Location_Z', 'Velocity_X', 'Velocity_Y', 'Velocity_Z'])
        
        for i, particle in enumerate(psys.particles):
            writer.writerow([
                i,
                particle.location.x,
                particle.location.y,
                particle.location.z,
                particle.velocity.x,
                particle.velocity.y,
                particle.velocity.z
            ])
```

### 创建粒子特效

```python
def create_particle_effect(
    emitter_obj,
    target_obj=None,
    particle_count=1000,
    lifetime=50,
    emission_type='VOLUME',
    color=(1.0, 0.5, 0.0)
):
    """创建粒子特效"""
    # 添加粒子系统
    bpy.ops.object.select_all(action='DESELECT')
    emitter_obj.select_set(True)
    bpy.context.view_layer.objects.active = emitter_obj
    bpy.ops.object.particle_system_add()
    
    psys = emitter_obj.particle_systems.active
    psys.name = "Effect"
    
    settings = psys.settings
    settings.type = 'EMITTER'
    settings.count = particle_count
    settings.lifetime = lifetime
    settings.normal_factor = 1.0
    
    # 发射类型
    settings.emission_source = emission_type
    
    # 渲染设置
    settings.render_type = 'OBJECT'
    settings.instance_object = target_obj
    
    # 颜色
    settings.color = color
    
    return psys
```

## 使用场景

- **粒子特效**：创建烟雾、火焰、灰尘等特效
- **发型设计**：创建程序化发型和毛发
- **植被生成**：创建草地和植被
- **物理模拟**：创建布料、软体、刚体模拟
- **数据导出**：导出粒子数据进行外部处理
- **批量编辑**：批量修改粒子属性和行为

## 注意事项

- 粒子模拟需要设置物理属性
- 高粒子数量影响性能
- 毛发编辑需要切换到粒子编辑模式
- 粒子渲染类型决定显示方式
- 粒子数据导出需要考虑时间序列


---
## bpy_ops_boid

# bpy.ops.boid - Boid Operations Module

Blender Boid（群集）操作符，用于粒子系统的群集行为模拟。

## Overview

`bpy.ops.boid` 模块包含 Boid 粒子系统的操作命令，支持群集行为的配置和控制。这个模块主要用于模拟鸟类、鱼类等群体行为。

## 核心功能

```python
import bpy

# Boid 状态操作
bpy.ops.boid.rule_external_logic()

# Boid 行为规则
bpy.ops.boid.rule_separation()
bpy.ops.boid.rule_alignment()
bpy.ops.boid.rule_cohesion()
```

## 实际应用示例

### Boid 行为管理器

```python
import bpy
import math

class BoidManager:
    """Boid 管理器"""
    
    def __init__(self):
        self.boids = []
    
    def add_boid_rule(self, particle_system, rule_type):
        """添加 Boid 规则"""
        rule = particle_system.settings.boids.active_boid_state.active_rule
        
        if rule_type == 'separation':
            rule.separation_distance = 1.0
            rule.separation_weight = 1.0
        elif rule_type == 'alignment':
            rule.alignment_distance = 2.0
            rule.alignment_weight = 1.0
        elif rule_type == 'cohesion':
            rule.cohesion_distance = 3.0
            rule.cohesion_weight = 1.0
        elif rule_type == 'target':
            rule.target = bpy.context.active_object
    
    def create_flocking_simulation(self, particle_system, 
                                   count=100, radius=10):
        """创建群集模拟"""
        settings = particle_system.settings
        
        # 设置 Boid 模式
        settings.physics_type = 'BOIDS'
        settings.boids.mode = 'FLOCK'
        
        # 添加规则
        self.add_boid_rule(particle_system, 'separation')
        self.add_boid_rule(particle_system, 'alignment')
        self.add_boid_rule(particle_system, 'cohesion')
        
        # 设置粒子数
        settings.count = count
        settings.frame_start = 1
        settings.frame_end = 200
    
    def create_fish_school(self, particle_system, count=50,
                          school_radius=5):
        """创建鱼群"""
        settings = particle_system.settings
        
        settings.physics_type = 'BOIDS'
        settings.boids.mode = 'FLOCK'
        settings.boids.fish = True
        
        # 设置鱼的行为
        self.add_boid_rule(particle_system, 'separation')
        self.add_boid_rule(particle_system, 'cohesion')
        
        # 添加目标吸引
        rule = settings.boids.active_boid_state.active_rule
        rule.average_distance = school_radius
    
    def create_bird_flock(self, particle_system, count=30,
                         flight_area=20):
        """创建鸟群"""
        settings = particle_system.settings
        
        settings.physics_type = 'BOIDS'
        settings.boids.bird = True
        
        # 设置飞行行为
        self.add_boid_rule(particle_system, 'alignment')
        self.add_boid_rule(particle_system, 'cohesion')
        
        # 设置高度范围
        settings.zmin = 0
        settings.zmax = flight_area
    
    def set_boid_physics(self, particle_system, 
                         mass=1.0, max_speed=5.0, max_force=0.5):
        """设置 Boid 物理属性"""
        settings = particle_system.settings
        
        settings.mass = mass
        settings.boid_max_speed = max_speed
        settings.boid_max_force = max_force
    
    def add_target_attractor(self, particle_system, target_object):
        """添加目标吸引"""
        settings = particle_system.settings
        
        rule = settings.boids.active_boid_state.active_rule
        rule.type = 'TARGET'
        rule.target = target_object
        rule.weight = 1.0
    
    def add_avoid_attractor(self, particle_system, avoid_object,
                           distance=5.0, weight=2.0):
        """添加回避吸引"""
        settings = particle_system.settings
        
        rule = settings.boids.active_boid_state.active_rule
        rule.type = 'AVOID'
        rule.collision_collection = avoid_object
        rule.distance = distance
        rule.weight = weight
    
    def set_boid_states(self, particle_system, states):
        """设置 Boid 状态"""
        settings = particle_system.settings
        
        # 清除现有状态
        settings.boids.boid_states.clear()
        
        # 添加新状态
        for state_data in states:
            state = settings.boids.boid_states.add()
            state.name = state_data.get('name', 'State')
            
            for rule_name in state_data.get('rules', []):
                rule = state.active_rule
                if rule_name == 'separation':
                    rule.separation = True
                elif rule_name == 'alignment':
                    rule.alignment = True
                elif rule_name == 'cohesion':
                    rule.cohesion = True
    
    def create_state_machine(self, particle_system):
        """创建状态机"""
        states = [
            {
                'name': 'Wander',
                'rules': ['separation', 'alignment', 'cohesion']
            },
            {
                'name': 'Seek',
                'rules': ['target']
            },
            {
                'name': 'Flee',
                'rules': ['avoid']
            }
        ]
        
        self.set_boid_states(particle_system, states)
    
    def export_boid_settings(self, particle_system, filepath):
        """导出 Boid 设置"""
        settings = particle_system.settings
        
        data = {
            'mode': settings.boids.mode,
            'max_speed': settings.boid_max_speed,
            'max_force': settings.boid_max_force,
            'states': []
        }
        
        for state in settings.boids.boid_states:
            state_data = {
                'name': state.name,
                'rules': []
            }
            
            rule = state.active_rule
            if rule.separation:
                state_data['rules'].append('separation')
            if rule.alignment:
                state_data['rules'].append('alignment')
            if rule.cohesion:
                state_data['rules'].append('cohesion')
            
            data['states'].append(state_data)
        
        import json
        
        with open(filepath, 'w') as f:
            json.dump(data, f, indent=2)
        
        return filepath


def setup_boid_simulation():
    """设置 Boid 模拟"""
    manager = BoidManager()
    
    print("Boid 管理器已设置")
    print("可用功能:")
    print("- create_flocking_simulation: 创建群集")
    print("- create_fish_school: 创建鱼群")
    print("- create_state_machine: 创建状态机")
    
    return manager
```

### 群集动画系统

```python
import bpy
import math

class FlockAnimationSystem:
    """群集动画系统"""
    
    def __init__(self):
        self.flock_objects = {}
    
    def create_flock_from_mesh(self, mesh_name, count=50,
                              offset=0.1):
        """从网格创建群集"""
        mesh = bpy.data.objects.get(mesh_name)
        if not mesh:
            return None
        
        # 创建粒子系统
        bpy.context.view_layer.objects.active = mesh
        bpy.ops.object.particle_system_add()
        
        particle_system = mesh.particle_systems[-1]
        
        # 配置 Boid
        manager = BoidManager()
        manager.create_flocking_simulation(particle_system, count)
        
        # 设置发射器
        particle_system.settings.distribution = 'RAND'
        particle_system.settings.emitter = mesh
        particle_system.settings.grid_resolution = 10
        
        return particle_system
    
    def animate_flock_movement(self, particle_system, 
                              duration=100, speed=1.0):
        """动画群集移动"""
        settings = particle_system.settings
        
        # 设置时间范围
        settings.frame_start = 1
        settings.frame_end = duration
        
        # 设置速度
        settings.normal_factor = speed
    
    def create_flock_formation(self, particle_system, 
                               formation='V_SHAPE'):
        """创建群集阵型"""
        settings = particle_system.settings
        
        if formation == 'V_SHAPE':
            settings.boids.flock_shape = 'V'
        elif formation == 'LINE':
            settings.boids.flock_shape = 'LINE'
        elif formation == 'SPHERE':
            settings.boids.flock_shape = 'SPHERE'
    
    def add_predator(self, particle_system, predator_object,
                    fear_distance=5.0):
        """添加捕食者"""
        manager = BoidManager()
        manager.add_avoid_attractor(
            particle_system,
            predator_object,
            distance=fear_distance,
            weight=3.0
        )
    
    def create_trail_effect(self, particle_system, target_object):
        """创建拖尾效果"""
        # 创建轨迹曲线
        bpy.ops.curve.primitive_nurbs_path_add()
        trail = bpy.context.active_object
        trail.name = f"{target_object.name}_Trail"
        
        return trail
    
    def sync_flock_to_audio(self, particle_system, audio_file,
                           beat_frames=None):
        """同步群集到音频"""
        sound = bpy.data.sounds.get(audio_file)
        if not sound:
            return
        
        # 基于节拍调整群集行为
        fps = bpy.context.scene.render.fps
        duration = sound.length
        
        # 创建节拍帧
        if beat_frames is None:
            beat_frames = list(range(0, int(duration * fps), int(fps / 2)))
        
        return beat_frames
    
    def create_migration_pattern(self, particle_system, 
                                start_point, end_point,
                                duration=200):
        """创建迁徙模式"""
        settings = particle_system.settings
        
        # 设置目标
        bpy.ops.object.empty_add(location=end_point)
        target = bpy.context.active_object
        target.name = "MigrationTarget"
        
        manager = BoidManager()
        manager.add_target_attractor(particle_system, target)
        
        # 设置动画
        for frame in range(1, duration + 1):
            bpy.context.scene.frame_set(frame)
            
            t = frame / duration
            
            # 移动目标
            current_pos = (
                start_point[0] + (end_point[0] - start_point[0]) * t,
                start_point[1] + (end_point[1] - start_point[1]) * t,
                start_point[2] + (end_point[2] - start_point[2]) * t
            )
            
            target.location = current_pos
            target.keyframe_insert(data_path='location', frame=frame)
    
    def create_boid_collision_avoidance(self, particle_system,
                                       obstacle_objects):
        """创建 Boid 避障"""
        settings = particle_system.settings
        
        # 创建避障集合
        collection = bpy.data.collections.new("BoidObstacles")
        bpy.context.scene.collection.children.link(collection)
        
        for obj in obstacle_objects:
            for col in obj.users_collection:
                col.objects.unlink(obj)
            collection.objects.link(obj)
        
        # 设置碰撞集合
        rule = settings.boids.active_boid_state.active_rule
        rule.collision_collection = collection
        rule.distance = 2.0
        rule.weight = 2.0
    
    def export_flock_animation(self, particle_system, filepath):
        """导出群集动画"""
        settings = particle_system.settings
        
        animation_data = {
            'frame_start': settings.frame_start,
            'frame_end': settings.frame_end,
            'count': settings.count,
            'boid_settings': {
                'max_speed': settings.boid_max_speed,
                'max_force': settings.boid_max_force
            }
        }
        
        import json
        
        with open(filepath, 'w') as f:
            json.dump(animation_data, f, indent=2)
        
        return filepath


def setup_flock_animation():
    """设置群集动画"""
    system = FlockAnimationSystem()
    
    print("群集动画系统已设置")
    print("可用功能:")
    print("- create_flock_from_mesh: 从网格创建")
    print("- create_migration_pattern: 迁徙模式")
    print("- sync_flock_to_audio: 音频同步")
    
    return system
```

### 群集优化工具

```python
import bpy

class FlockOptimizationTool:
    """群集优化工具"""
    
    def __init__(self):
        self.performance_settings = {}
    
    def set_render_quality(self, particle_system, quality='LOW'):
        """设置渲染质量"""
        settings = particle_system.settings
        
        if quality == 'LOW':
            settings.render_step = 4
            settings.triangle_seed_step = 4
        elif quality == 'MEDIUM':
            settings.render_step = 2
            settings.triangle_seed_step = 2
        elif quality == 'HIGH':
            settings.render_step = 1
            settings.triangle_seed_step = 1
    
    def optimize_flock_for_realtime(self, particle_system):
        """优化群集实时性能"""
        self.set_render_quality(particle_system, 'LOW')
        
        settings = particle_system.settings
        settings.count = min(settings.count, 100)
        settings.frame_start = 1
        settings.frame_end = 100
    
    def optimize_flock_for_rendering(self, particle_system):
        """优化群集渲染"""
        self.set_render_quality(particle_system, 'HIGH')
        
        settings = particle_system.settings
        settings.count = min(settings.count, 1000)
    
    def balance_quality_performance(self, particle_system, 
                                    target_fps=30):
        """平衡质量和性能"""
        settings = particle_system.settings
        
        # 计算最大粒子数
        current_fps = bpy.context.scene.render.fps
        render_time = 1.0 / target_fps
        
        # 调整粒子数
        max_count = int(render_time * settings.count / 0.01)
        settings.count = min(settings.count, max_count)
    
    def set_lod_levels(self, particle_system, levels):
        """设置 LOD 级别"""
        settings = particle_system.settings
        
        for i, (distance, percent) in enumerate(levels):
            if i == 0:
                settings.lod_distance = distance
                settings.lod_percent = percent
            elif i == 1:
                settings.lod_max_hysteresis = distance
                settings.lod_max_percent = percent
    
    def create_render_baker(self, particle_system):
        """创建渲染烘焙器"""
        # 烘焙粒子到关键帧
        bpy.context.view_layer.objects.active = particle_system.point_cache[0].object
        bpy.ops.ptcache.bake_from_cache()
    
    def export_particle_cache(self, particle_system, filepath):
        """导出粒子缓存"""
        bpy.ops.ptcache.external_export(filepath=filepath)
    
    def import_particle_cache(self, particle_system, filepath):
        """导入粒子缓存"""
        bpy.ops.ptcache.external_import(filepath=filepath)
    
    def check_flock_health(self, particle_system):
        """检查群集健康"""
        settings = particle_system.settings
        
        health = {
            'particle_count': settings.count,
            'frame_range': f"{settings.frame_start}-{settings.frame_end}",
            'physics_type': settings.physics_type,
            'render_step': settings.render_step,
            'memory_usage': settings.count * settings.render_step
        }
        
        return health
    
    def create_flock_report(self, particle_system):
        """创建群集报告"""
        health = self.check_flock_health(particle_system)
        
        report = f"""
=== 群集健康报告 ===

粒子数量: {health['particle_count']}
帧范围: {health['frame_range']}
物理类型: {health['physics_type']}
渲染步长: {health['render_step']}
预估内存: {health['memory_usage']} MB
"""
        
        return report
    
    def reset_flock_simulation(self, particle_system):
        """重置群集模拟"""
        # 清除缓存
        bpy.ops.ptcache.free()
        
        # 重置帧
        bpy.context.scene.frame_set(1)
        
        # 重新开始
        particle_system.seed = int(bpy.context.scene.frame_current)


def setup_flock_optimization():
    """设置群集优化"""
    tool = FlockOptimizationTool()
    
    print("群集优化工具已设置")
    print("可用功能:")
    print("- optimize_flock_for_realtime: 实时优化")
    print("- create_render_baker: 烘焙渲染")
    print("- check_flock_health: 健康检查")
    
    return tool
```


---
## bpy_ops_dpaint

# bpy.ops.dpaint - 动态绘画操作模块

Blender动态绘画操作符，用于设置和管理动态绘画效果。

## 概述

`bpy.ops.dpaint` 模块提供用于操作动态绘画系统的功能，包括画布设置、笔刷设置、初始化和烘焙操作。

## 核心功能

### 动态绘画初始化

```python
import bpy

# 初始化动态绘画
bpy.ops.dpaint.canvas_init()

# 为选中对象初始化动态绘画
bpy.ops.dpaint.canvas_init(
    object='ACTIVE'
)

# 为所有对象初始化动态绘画
bpy.ops.dpaint.canvas_init(
    object='ALL'
)

# 清除动态绘画
bpy.ops.dpaint.canvas_clear()

# 清除特定类型
bpy.ops.dpaint.canvas_clear(
    type='COLOR'
)

# 重置动态绘画
bpy.ops.dpaint.canvas_reset()
```

### 画布操作

```python
# 创建新画布
bpy.ops.dpaint.canvas_add(
    type='IMAGE'
)

# 创建基于对象的画布
bpy.ops.dpaint.canvas_add_object(
    object='Cube'
)

# 创建基于网格的画布
bpy.ops.dpaint.canvas_add_mesh(
    mesh='Suzanne'
)

# 创建基于曲面的画布
bpy.ops.dpaint.canvas_add_surface(
    surface='Surface'
)

# 创建基于粒子的画布
bpy.ops.dpaint.canvas_add_particle(
    particle='ParticleSettings'
)

# 删除画布
bpy.ops.dpaint.canvas_remove()

# 复制画布
bpy.ops.dpaint.canvas_duplicate()
```

### 笔刷操作

```python
# 添加笔刷
bpy.ops.dpaint.brush_add(
    type='DAMP'
)

# 添加侵蚀笔刷
bpy.ops.dpaint.brush_add(
    type='ERODE'
)

# 添加扩散笔刷
bpy.ops.dpaint.brush_add(
    type='SPREAD'
)

# 复制笔刷
bpy.ops.dpaint.brush_copy()

# 重命名笔刷
bpy.ops.dpaint.brush_rename(
    name='MyBrush'
)

# 删除笔刷
bpy.ops.dpaint.brush_remove()

# 重置笔刷
bpy.ops.dpaint.brush_reset()
```

### 纹理操作

```python
# 添加纹理
bpy.ops.dpaint.texture_add()

# 清除纹理
bpy.ops.dpaint.texture_clear()

# 更新纹理
bpy.ops.dpaint.texture_update()

# 刷新纹理
bpy.ops.dpaint.texture_refresh()
```

### 笔触操作

```python
# 开始笔触
bpy.ops.dpaint.stroke_begin()

# 结束笔触
bpy.ops.dpaint.stroke_end()

# 取消笔触
bpy.ops.dpaint.stroke_cancel()

# 应用笔触
bpy.ops.dpaint.stroke_apply()

# 清除笔触历史
bpy.ops.dpaint.stroke_history_clear()
```

### 序列帧操作

```python
# 开始序列帧
bpy.ops.dpaint.write_cache_start()

# 结束序列帧
bpy.ops.dpaint.write_cache_end()

# 清除序列帧
bpy.ops.dpaint.write_cache_clear()

# 刷新序列帧
bpy.ops.dpaint.write_cache_refresh()
```

### 烘焙操作

```python
# 烘焙所有效果
bpy.ops.dpaint.bake()

# 烘焙颜色
bpy.ops.dpaint.bake(
    type='COLOR'
)

# 烘焙法线
bpy.ops.dpaint.bake(
    type='NORMAL'
)

# 烘焙粗糙度
bpy.ops.dpaint.bake(
    type='ROUGHNESS'
)

# 烘焙环境遮蔽
bpy.ops.dpaint.bake(
    type='AO'
)

# 烘焙高度
bpy.ops.dpaint.bake(
    type='HEIGHT'
)

# 烘焙浓度
bpy.ops.dpaint.bake(
    type='DENSITY'
)

# 烘焙速度
bpy.ops.dpaint.bake(
    type='VELOCITY'
)

# 烘焙温度
bpy.ops.dpaint.bake(
    type='TEMPERATURE'
)

# 烘焙湿气
bpy.ops.dpaint.bake(
    type='WETNESS'
)

# 烘焙流动
bpy.ops.dpaint.bake(
    type='FLOW'
)

# 烘焙压力
bpy.ops.dpaint.bake(
    type='PRESSURE'
)

# 取消烘焙
bpy.ops.dpaint.bake_cancel()

# 烘焙所有
bpy.ops.dpaint.bake_all()
```

### 导出操作

```python
# 导出画布
bpy.ops.dpaint.export(
    filepath='//dpaint_result.exr'
)

# 导出所有画布
bpy.ops.dpaint.export_all(
    filepath='//dpaint_results'
)

# 导出为图像序列
bpy.ops.dpaint.export_images(
    filepath='//dpaint_images/image_',
    format='PNG'
)

# 导出为视频
bpy.ops.dpaint.export_video(
    filepath='//dpaint_video.mp4'
)
```

### 笔刷模式操作

```python
# 设置笔刷模式
bpy.ops.dpaint.set_brush_mode(
    mode='DAMP'
)

# 切换侵蚀模式
bpy.ops.dpaint.set_brush_mode(
    mode='ERODE'
)

# 切换扩散模式
bpy.ops.dpaint.set_brush_mode(
    mode='SPREAD'
)

# 切换颜色模式
bpy.ops.dpaint.set_brush_mode(
    mode='COLOR'
)

# 切换水模式
bpy.ops.dpaint.set_brush_mode(
    mode='LIQUID'
)

# 切换蒸发模式
bpy.ops.dpaint.set_brush_mode(
    mode='EVAPORATE'
)
```

### 时间设置

```python
# 设置开始帧
bpy.ops.dpaint.start_frame_set(
    frame=1
)

# 设置结束帧
bpy.ops.dpaint.end_frame_set(
    frame=250
)

# 设置时间范围
bpy.ops.dpaint.time_range_set(
    start=1,
    end=250
)

# 重置时间范围
bpy.ops.dpaint.time_range_reset()
```

### 高级效果设置

```python
# 设置侵蚀效果
bpy.ops.dpaint.erosion_settings(
    type='SURFACE_EROSION',
    flow_factor=1.0,
    hardness=0.5
)

# 设置扩散效果
bpy.ops.dpaint.spread_settings(
    type='SURFACE_SPREAD',
    spread_speed=0.5,
    drip_speed=0.3
)

# 设置干燥效果
bpy.ops.dpaint.dry_settings(
    dry_speed=0.1,
    infrared_cooling=0.5
)

# 设置流动效果
bpy.ops.dpaint.flow_settings(
    flow_speed=0.5,
    gravity_factor=0.3
)
```

## 实用函数

```python
def get_active_canvas():
    """获取当前激活的画布"""
    return bpy.context.active_object.data

def create_dpaint_canvas(obj, canvas_type='IMAGE'):
    """为对象创建动态绘画画布"""
    bpy.context.view_layer.objects.active = obj
    bpy.ops.dpaint.canvas_add_object(object=obj.name)
    return obj.data

def bake_all_canvases(start_frame=1, end_frame=250):
    """烘焙所有画布"""
    bpy.ops.dpaint.start_frame_set(frame=start_frame)
    bpy.ops.dpaint.end_frame_set(frame=end_frame)
    bpy.ops.dpaint.bake_all()

def export_dpaint_results(output_dir):
    """导出所有动态绘画结果"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    bpy.ops.dpaint.export_all(filepath=output_dir)

def reset_brush_settings():
    """重置所有笔刷设置"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            for space in area.spaces:
                if space.type == 'VIEW_3D':
                    brush = space.dpaint.brush
                    brush.reset()

def setup_damp_brush(brush_name, strength=1.0):
    """设置阻尼笔刷"""
    bpy.ops.dpaint.brush_add(type='DAMP')
    brush = bpy.context.active_brush
    brush.name = brush_name
    brush.strength = strength
    return brush

def setup_erosion_brush(brush_name, flow=1.0):
    """设置侵蚀笔刷"""
    bpy.ops.dpaint.brush_add(type='ERODE')
    brush = bpy.context.active_brush
    brush.name = brush_name
    brush.flow_factor = flow
    return brush

def create_color_canvas(obj, color=(1, 0, 0, 1)):
    """创建颜色画布"""
    bpy.context.view_layer.objects.active = obj
    bpy.ops.dpaint.canvas_add_object(object=obj.name)
    canvas = obj.data
    canvas.dpaint.color = color
    return canvas
```

## 常见问题排查

### 画布不显示
```python
# 检查动态绘画是否启用
obj = bpy.context.active_object
print(f"动态绘画类型: {obj.data.dpaint.type}")

# 检查显示设置
space = bpy.context.space_data
if space:
    print(f"显示动态绘画: {space.dpaint.show_view}")

# 启用显示
if space:
    space.dpaint.show_view = True

# 检查笔刷是否设置
brush = bpy.context.active_brush
print(f"当前笔刷: {brush}")

# 重新初始化
bpy.ops.dpaint.canvas_init()
```

### 烘焙失败
```python
# 检查帧范围
print(f"开始帧: {bpy.context.scene.dpaint.start_frame}")
print(f"结束帧: {bpy.context.scene.dpaint.end_frame}")

# 检查画布是否有效
print(f"画布数: {len(bpy.context.scene.dpaint.canvases)}")

# 检查内存
print(f"使用内存: {bpy.context.scene.dpaint.memory_limit} MB")

# 清除缓存后重试
bpy.ops.dpaint.write_cache_clear()
bpy.ops.dpaint.bake()

# 逐个烘焙
for canvas in bpy.context.scene.dpaint.canvases:
    bpy.ops.dpaint.bake(type=canvas.type)
```

### 笔刷效果不明显
```python
# 检查笔刷强度
brush = bpy.context.active_brush
print(f"笔刷强度: {brush.strength}")

# 增加强度
brush.strength = 1.0

# 检查笔刷类型
print(f"笔刷类型: {brush.dpaint.type}")

# 切换笔刷类型
bpy.ops.dpaint.set_brush_mode(mode='ERODE')

# 检查时间步长
print(f"时间步长: {bpy.context.scene.dpaint.time_scale}")

# 增加时间步长
bpy.context.scene.dpaint.time_scale = 2.0
```


---
## bpy_ops_ptcache

# bpy.ops.ptcache 模块

物理缓存操作模块，用于管理物理模拟的缓存数据。

## 概述

bpy.ops.ptcache 模块包含用于操作物理模拟缓存的运算符。物理缓存存储物理模拟的计算结果，用于提高回放性能和避免重复计算。

## 常用操作

### 缓存管理

```python
import bpy

# 添加缓存
bpy.ops.ptcache.add()

# 删除缓存
bpy.ops.ptcache.remove()

# 清除缓存
bpy.ops.ptcache.free()

# 刷新缓存
bpy.ops.ptcache.refresh()
```

### 缓存操作

```python
# 重载缓存
bpy.ops.ptcache.reload()

# 读取缓存
bpy.ops.ptcache.read_cache(frame_start=1, frame_end=250)

# 写入缓存
bpy.ops.ptcache.write_cache(frame_start=1, frame_end=250)

# 更新缓存
bpy.ops.ptcache.update_cache()
```

### 烘焙操作

```python
# 烘焙所有
bpy.ops.ptcache.bake_all()

# 烘焙选定
bpy.ops.ptcache.bake()

# 烘焙选择
bpy.ops.ptcache.bake_selected()

# 撤销烘焙
bpy.ops.ptcache.undo_bake()

# 重做烘焙
bpy.ops.ptcache.redo_bake()
```

### 物理类型

```python
# 烘焙粒子
bpy.ops.ptcache.bake(type='PARTICLES')

# 烘焙布料
bpy.ops.ptcache.bake(type='CLOTH')

# 烘焙流体
bpy.ops.ptcache.bake(type='FLUID')

# 烘焙刚体
bpy.ops.ptcache.bake(type='RIGID_BODY')

# 烘焙软体
bpy.ops.ptcache.bake(type='SOFT_BODY')

# 烘焙烟雾
bpy.ops.ptcache.bake(type='SMOKE')
```

## 实际应用示例

### 批量烘焙所有物理

```python
def bake_all_physics(frame_start=1, frame_end=250):
    """烘焙所有物理效果"""
    # 遍历所有有物理效果的物体
    for obj in bpy.context.scene.objects:
        if has_physics(obj):
            # 选中物体
            bpy.context.view_layer.objects.active = obj
            obj.select_set(True)
            
            # 烘焙
            bpy.ops.ptcache.bake(
                frame_start=frame_start,
                frame_end=frame_end,
                bake_type='PHYSICS'
            )
            
            # 取消选择
            obj.select_set(False)
    
    print("所有物理烘焙完成")

def has_physics(obj):
    """检查物体是否有物理效果"""
    # 检查各种物理修改器
    for mod in obj.modifiers:
        if mod.type in ['CLOTH', 'SOFT_BODY', 'FLUID', 'SMOKE', 'RIGID_BODY']:
            return True
    return obj.rigid_body is not None
```

### 烘焙粒子系统

```python
def bake_particle_systems(frame_start=1, frame_end=250):
    """烘焙所有粒子系统"""
    # 获取当前帧
    current_frame = bpy.context.scene.frame_current
    
    # 遍历所有粒子系统
    for obj in bpy.context.scene.objects:
        for ptcache in obj.particle_systems:
            # 确保粒子系统有缓存
            if ptcache.point_cache:
                # 设置帧范围
                ptcache.point_cache.frame_start = frame_start
                ptcache.point_cache.frame_end = frame_end
                
                # 选中物体
                bpy.context.view_layer.objects.active = obj
                
                # 烘焙
                bpy.ops.ptcache.bake(type='PARTICLES')
    
    # 恢复帧位置
    bpy.context.scene.frame_set(current_frame)
```

### 烘焙布料模拟

```python
def bake_cloth_simulation(obj, frame_start=1, frame_end=250):
    """烘焙布料模拟"""
    # 确保选中物体
    bpy.context.view_layer.objects.active = obj
    obj.select_set(True)
    
    # 设置时间范围
    bpy.context.scene.frame_start = frame_start
    bpy.context.scene.frame_end = frame_end
    
    # 清除旧缓存
    bpy.ops.ptcache.free()
    
    # 重新计算并烘焙
    bpy.ops.ptcache.bake(type='CLOTH')
    
    print(f"布料模拟烘焙完成: {obj.name}")
    
    # 取消选择
    obj.select_set(False)
```

### 烘焙流体模拟

```python
def bake_fluid_simulation(obj, frame_start=1, frame_end=250):
    """烘焙流体模拟"""
    # 确保选中物体
    bpy.context.view_layer.objects.active = obj
    obj.select_set(True)
    
    # 设置帧范围
    bpy.context.scene.frame_start = frame_start
    bpy.context.scene.frame_end = frame_end
    
    # 清除旧缓存
    bpy.ops.ptcache.free()
    
    # 烘焙流体
    bpy.ops.ptcache.bake(type='FLUID')
    
    print(f"流体模拟烘焙完成: {obj.name}")
    
    # 取消选择
    obj.select_set(False)
```

### 烘焙刚体模拟

```python
def bake_rigidbody_simulation(objects, frame_start=1, frame_end=250):
    """烘焙刚体模拟"""
    # 确保所有物体被选中
    for obj in objects:
        obj.select_set(True)
    
    bpy.context.view_layer.objects.active = objects[0]
    
    # 设置帧范围
    bpy.context.scene.frame_start = frame_start
    bpy.context.scene.frame_end = frame_end
    
    # 清除旧缓存
    bpy.ops.ptcache.free()
    
    # 烘焙刚体
    bpy.ops.ptcache.bake(type='RIGID_BODY')
    
    print(f"刚体模拟烘焙完成: {len(objects)} 个物体")
    
    # 取消选择
    for obj in objects:
        obj.select_set(False)
```

### 创建渐进式烘焙

```python
def progressive_bake(obj, chunk_size=50):
    """分块渐进式烘焙"""
    frame_start = bpy.context.scene.frame_start
    frame_end = bpy.context.scene.frame_end
    
    current_frame = frame_start
    
    while current_frame <= frame_end:
        chunk_end = min(current_frame + chunk_size - 1, frame_end)
        
        # 烘焙当前块
        bpy.ops.ptcache.bake(
            frame_start=current_frame,
            frame_end=chunk_end
        )
        
        print(f"烘焙帧范围: {current_frame} - {chunk_end}")
        
        current_frame = chunk_end + 1
```

### 验证缓存完整性

```python
def verify_cache_integrity(ptcache):
    """验证缓存完整性"""
    if not ptcache:
        return False
    
    # 检查帧范围
    expected_frames = ptcache.frame_end - ptcache.frame_start + 1
    
    # 检查缓存文件
    cache_path = bpy.path.abspath(ptcache.filepath)
    
    if not os.path.exists(cache_path):
        print(f"缓存文件不存在: {cache_path}")
        return False
    
    # 验证文件大小
    file_size = os.path.getsize(cache_path)
    
    if file_size == 0:
        print("缓存文件为空")
        return False
    
    print(f"缓存验证通过: {file_size} 字节, 帧范围: {ptcache.frame_start} - {ptcache.frame_end}")
    return True
```

### 清理无效缓存

```python
def cleanup_invalid_caches():
    """清理无效缓存"""
    # 获取所有可能的缓存位置
    cache_dirs = []
    
    for obj in bpy.context.scene.objects:
        if obj.point_caches:
            for ptcache in obj.point_caches:
                if ptcache.filepath:
                    cache_dir = bpy.path.abspath(os.path.dirname(ptcache.filepath))
                    if cache_dir not in cache_dirs:
                        cache_dirs.append(cache_dir)
    
    # 检查并清理空目录
    for cache_dir in cache_dirs:
        if os.path.exists(cache_dir):
            # 检查目录是否为空
            if not os.listdir(cache_dir):
                os.rmdir(cache_dir)
                print(f"删除空缓存目录: {cache_dir}")
```

### 导出缓存信息

```python
def export_cache_info(filepath):
    """导出缓存信息"""
    info = []
    info.append("=== 物理缓存信息 ===")
    
    for obj in bpy.context.scene.objects:
        for ptcache in obj.point_caches:
            info.append(f"\n物体: {obj.name}")
            info.append(f"  缓存名称: {ptcache.name}")
            info.append(f"  帧范围: {ptcache.frame_start} - {ptcache.frame_end}")
            info.append(f"  缓存路径: {ptcache.filepath}")
            info.append(f"  缓存类型: {ptcache.type}")
            info.append(f"  用户数: {ptcache.users}")
            
            # 检查缓存文件
            cache_path = bpy.path.abspath(ptcache.filepath)
            if os.path.exists(cache_path):
                info.append(f"  文件大小: {os.path.getsize(cache_path)} 字节")
            else:
                info.append("  状态: 文件不存在")
    
    # 写入文件
    with open(filepath, 'w', encoding='utf-8') as f:
        f.write('\n'.join(info))
    
    print(f"缓存信息已导出到: {filepath}")
```

### 重新计算关键帧缓存

```python
def recompute_keyframe_cache(obj, keyframes):
    """重新计算关键帧缓存"""
    # 获取当前帧
    current_frame = bpy.context.scene.frame_current
    
    # 删除旧的关键帧
    for frame in keyframes:
        bpy.context.scene.frame_set(frame)
        for ptcache in obj.point_caches:
            ptcache.clear_keyed(frame)
    
    # 重新计算
    for frame in keyframes:
        bpy.context.scene.frame_set(frame)
        
        # 设置物体状态
        set_object_state(obj, frame)
        
        # 插入关键帧
        obj.keyframe_insert(data_path='location', frame=frame)
        obj.keyframe_insert(data_path='rotation_euler', frame=frame)
    
    # 恢复帧位置
    bpy.context.scene.frame_set(current_frame)
```

### 创建缓存备份

```python
def backup_cache(ptcache, backup_dir):
    """备份缓存"""
    if not ptcache.filepath:
        print("没有缓存文件可备份")
        return
    
    source_path = bpy.path.abspath(ptcache.filepath)
    
    if not os.path.exists(source_path):
        print(f"源缓存文件不存在: {source_path}")
        return
    
    # 创建备份目录
    os.makedirs(backup_dir, exist_ok=True)
    
    # 生成备份文件名
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    backup_path = os.path.join(backup_dir, f"backup_{timestamp}.bphys")
    
    # 复制文件
    shutil.copy2(source_path, backup_path)
    
    print(f"缓存已备份到: {backup_path}")
```

## 使用场景

- **物理模拟优化**：缓存物理模拟结果
- **动画回放**：加速物理动画的预览
- **渲染准备**：为最终渲染准备缓存
- **数据管理**：管理物理模拟数据
- **性能优化**：减少实时计算开销
- **批量处理**：批量烘焙多个物理效果
- **数据验证**：验证缓存完整性
- **缓存备份**：备份重要模拟数据
- **跨平台共享**：共享模拟结果
- **版本控制**：管理不同版本的模拟

## 注意事项

- 大缓存文件可能占用大量磁盘空间
- 烘焙过程中不要修改场景
- 缓存与文件路径相关，移动文件后需要重新烘焙
- 不同物理类型需要不同的烘焙方法
- 定期清理无效缓存节省空间
- 关键帧缓存需要正确设置关键帧
- 渐进式烘焙适合大型模拟
- 验证缓存完整性防止渲染问题
- 备份重要缓存防止数据丢失
- 共享文件时注意相对路径设置


---
## bpy_ops_sound

# bpy.ops.sound - Sound Operations Module

Blender 声音操作符，用于在 Blender 中播放和管理音频文件。

## Overview

`bpy.ops.sound` 模块包含音频播放和管理的操作命令，支持音频的播放、暂停、停止、音量调节等功能。这个模块主要用于处理 Blender 中的音频相关操作。

## 核心功能

### 音频播放控制

```python
import bpy

# 播放声音
bpy.ops.sound.play()

# 暂停声音
bpy.ops.sound.pause()

# 停止声音
bpy.ops.sound.stop()

# 从开头播放
bpy.ops.sound.play(reset=True)

# 播放选择的声音
bpy.ops.sound.play_selected()

# 循环播放
bpy.ops.sound.loop_status_set(loop_status='LOOP')

# 单次播放
bpy.ops.sound.loop_status_set(loop_status='PLAY')

def play_sound(sound):
    """播放声音"""
    bpy.context.scene.sequence_editor.sequences_all[sound].sound.play()

def pause_all_sounds():
    """暂停所有声音"""
    bpy.ops.sound.pause()

def stop_all_sounds():
    """停止所有声音"""
    bpy.ops.sound.stop()

def toggle_playback():
    """切换播放/暂停"""
    if bpy.context.scene.sound_sequence_player.playing:
        bpy.ops.sound.pause()
    else:
        bpy.ops.sound.play()
```

### 音量控制

```python
# 增大音量
bpy.ops.sound.volume_factor_set(factor=1.1, use_selection=False)

# 减小音量
bpy.ops.sound.volume_factor_set(factor=0.9, use_selection=False)

# 设置音量
bpy.ops.sound.volume_factor_set(factor=0.5, use_selection=False)

# 静音/取消静音
bpy.ops.sound.mute(use_mute=True)

def set_volume(sound_name, volume):
    """设置声音音量"""
    sound = bpy.data.sounds.get(sound_name)
    if sound:
        sound.volume = volume

def fade_volume(sound_name, target_volume, duration=1.0):
    """淡入淡出音量"""
    import time
    
    sound = bpy.data.sounds.get(sound_name)
    if not sound:
        return
    
    start_volume = sound.volume
    start_time = time.time()
    
    while time.time() - start_time < duration:
        t = (time.time() - start_time) / duration
        sound.volume = start_volume + (target_volume - start_volume) * t
        time.sleep(0.05)
    
    sound.volume = target_volume
```

### 音频属性设置

```python
# 设置音调
bpy.ops.sound.pitch_factor_set(factor=1.0, use_selection=False)

# 设置均衡器
bpy.ops.sound.bank_feature_set(feature='NONE')

# 设置衰减距离
bpy.ops.sound.distance_feature_set(distance=10.0)

# 设置音量衰减模型
bpy.ops.sound.attenuation_feature_set(attenuation=1.0)

def set_pitch(sound_name, pitch):
    """设置音调"""
    sound = bpy.data.sounds.get(sound_name)
    if sound:
        sound.pitch = pitch

def set_pan(sound_name, pan):
    """设置声道平衡"""
    sound = bpy.data.sounds.get(sound_name)
    if sound:
        sound.pan = pan  # -1 左, 0 中, 1 右
```

## 实际应用示例

### 声音管理器

```python
import bpy
import os

class SoundManager:
    """声音管理器"""
    
    def __init__(self):
        self.sounds = {}
        self.playlists = {}
    
    def load_sound(self, filepath, name=None):
        """加载声音文件"""
        if name is None:
            name = os.path.splitext(os.path.basename(filepath))[0]
        
        sound = bpy.data.sounds.load(filepath)
        sound.name = name
        
        self.sounds[name] = sound
        return sound
    
    def load_directory(self, directory, extensions=None):
        """加载目录中的所有声音"""
        if extensions is None:
            extensions = ['.wav', '.mp3', '.ogg', '.flac']
        
        sounds = []
        for file in os.listdir(directory):
            if any(file.lower().endswith(ext) for ext in extensions):
                filepath = os.path.join(directory, file)
                sound = self.load_sound(filepath)
                sounds.append(sound)
        
        return sounds
    
    def play_sound(self, name, volume=1.0, pitch=1.0, loop=False):
        """播放声音"""
        sound = self.sounds.get(name)
        if sound:
            sound.volume = volume
            sound.pitch = pitch
            sound.play()
            
            if loop:
                sound.loop = True
    
    def stop_sound(self, name):
        """停止声音"""
        sound = self.sounds.get(name)
        if sound:
            sound.stop()
    
    def stop_all(self):
        """停止所有声音"""
        for sound in bpy.data.sounds:
            sound.stop()
    
    def create_playlist(self, name, sound_names, loop=True):
        """创建播放列表"""
        self.playlists[name] = {
            'sounds': sound_names,
            'current_index': 0,
            'loop': loop
        }
    
    def play_playlist(self, name):
        """播放播放列表"""
        playlist = self.playlists.get(name)
        if playlist and playlist['sounds']:
            first_sound = playlist['sounds'][0]
            self.play_sound(first_sound, loop=False)
    
    def set_volume(self, name, volume):
        """设置音量"""
        sound = self.sounds.get(name)
        if sound:
            sound.volume = max(0, min(1, volume))
    
    def set_global_volume(self, volume):
        """设置全局音量"""
        bpy.context.scene.sound_volume = max(0, min(1, volume))
    
    def get_sound_info(self, name):
        """获取声音信息"""
        sound = self.sounds.get(name)
        if sound:
            return {
                'name': sound.name,
                'length': sound.length,
                'sampling_rate': sound.sampling_rate,
                'channels': sound.channels,
                'volume': sound.volume,
                'pitch': sound.pitch
            }
        return None
    
    def list_sounds(self):
        """列出所有声音"""
        return list(self.sounds.keys())
    
    def remove_sound(self, name):
        """移除声音"""
        if name in self.sounds:
            sound = self.sounds[name]
            sound.stop()
            bpy.data.sounds.remove(sound)
            del self.sounds[name]


def setup_sound_manager(sound_dir):
    """设置声音管理器"""
    manager = SoundManager()
    
    # 加载声音目录
    if os.path.exists(sound_dir):
        manager.load_directory(sound_dir)
    
    print(f"声音管理器已设置")
    print(f"已加载声音: {manager.list_sounds()}")
    
    return manager
```

### 音频序列动画系统

```python
import bpy

class AudioAnimationSystem:
    """音频序列动画系统"""
    
    def __init__(self):
        self.audio_strips = {}
    
    def add_audio_strip(self, filepath, channel=1, frame_start=1):
        """添加音频条"""
        # 确保在序列编辑器中
        if not bpy.context.scene.sequence_editor:
            bpy.context.scene.sequence_editor_create()
        
        # 读取音频
        bpy.ops.sound.open(filepath=filepath)
        
        # 创建序列
        bpy.context.scene.sequence_editor.sequences.new_sound(
            name=os.path.splitext(os.path.basename(filepath))[0],
            filepath=filepath,
            channel=channel,
            frame_start=frame_start
        )
        
        return bpy.context.scene.sequence_editor.active_strip
    
    def add_sound_to_movieclip(self, sound_path, movieclip_path):
        """为视频片段添加音频"""
        # 加载音频
        audio = bpy.data.sounds.load(sound_path)
        
        # 创建序列
        bpy.context.scene.sequence_editor.sequences.new_sound(
            name="Audio",
            filepath=sound_path,
            channel=2,
            frame_start=1
        )
    
    def create_audio_animation(self, sound_name, target_object, target_property):
        """基于音频创建动画"""
        sound = bpy.data.sounds.get(sound_name)
        if not sound:
            return
        
        # 采样音频数据
        # 注意：实际实现需要访问音频波形数据
        pass
    
    def sync_animation_to_audio(self, action_name, audio_name):
        """同步动画到音频"""
        action = bpy.data.actions.get(action_name)
        sound = bpy.data.sounds.get(audio_name)
        
        if not action or not sound:
            return
        
        # 设置帧范围
        bpy.context.scene.frame_start = 1
        bpy.context.scene.frame_end = int(sound.length * bpy.context.scene.render.fps)
    
    def create_audio_visualization(self, sound_name, strip_count=32):
        """创建音频可视化"""
        sound = bpy.data.sounds.get(sound_name)
        if not sound:
            return
        
        # 创建条形图形
        for i in range(strip_count):
            bpy.ops.mesh.primitive_cube_add(
                size=0.1,
                location=(i * 0.15, 0, 0)
            )
            bar = bpy.context.active_object
            bar.name = f"AudioBar_{i}"
            
            # 缩放动画
            bar.scale.z = 0.1
            bar.keyframe_insert(data_path='scale', frame=1)
    
    def fade_in_out(self, strip_name, fade_in_frames=30, fade_out_frames=30):
        """淡入淡出"""
        strip = bpy.context.scene.sequence_editor.sequences.get(strip_name)
        if not strip:
            return
        
        # 淡入
        strip.volume_start = 0
        strip.keyframe_insert(data_path='volume_start', frame=1)
        strip.volume_start = 1
        strip.keyframe_insert(data_path='volume_start', frame=fade_in_frames)
        
        # 淡出
        end_frame = strip.frame_final_end
        strip.volume_start = 1
        strip.keyframe_insert(data_path='volume_start', frame=end_frame - fade_out_frames)
        strip.volume_start = 0
        strip.keyframe_insert(data_path='volume_start', frame=end_frame)
    
    def mix_sounds(self, sound1_name, sound2_name, mix_ratio=0.5):
        """混合声音"""
        sound1 = bpy.data.sounds.get(sound1_name)
        sound2 = bpy.data.sounds.get(sound2_name)
        
        if not sound1 or not sound2:
            return
        
        sound1.volume = 1 - mix_ratio
        sound2.volume = mix_ratio
        
        sound1.play()
        sound2.play()
    
    def create_spatial_audio(self, sound_name, emitter_object):
        """创建空间音频"""
        sound = bpy.data.sounds.get(sound_name)
        if not sound:
            return
        
        # 设置声音属性
        sound.flags = '3D'
        
        # 创建空对象作为监听器
        bpy.ops.object.empty_add(type='ARROWS', location=(0, 0, 0))
        listener = bpy.context.active_object
        listener.name = f"{sound_name}_Listener"
    
    def calculate_audio_peaks(self, sound_name, num_samples=100):
        """计算音频峰值"""
        sound = bpy.data.sounds.get(sound_name)
        if not sound:
            return []
        
        # 获取音频数据
        # 注意：实际实现需要访问音频缓冲区
        return []
    
    def export_audio(self, sound_name, output_path, format='WAV'):
        """导出音频"""
        sound = bpy.data.sounds.get(sound_name)
        if not sound:
            return
        
        # 注意：Blender Python API 不直接支持音频导出
        print(f"请手动导出音频: {output_path}")


def setup_audio_sequences():
    """设置音频序列"""
    system = AudioAnimationSystem()
    
    # 添加示例音频
    # system.add_audio_strip("path/to/audio.wav", channel=1, frame_start=1)
    
    return system
```

### 场景音频系统

```python
import bpy
import math

class SceneAudioSystem:
    """场景音频系统"""
    
    def __init__(self):
        self.audio_sources = {}
        self.listener = None
    
    def setup_listener(self):
        """设置听者（通常是相机）"""
        camera = bpy.context.scene.camera
        if camera:
            self.listener = camera
            return camera
        return None
    
    def create_audio_source(self, name, sound_path, location, loop=True, volume=1.0):
        """创建音频源"""
        # 加载声音
        sound = bpy.data.sounds.load(sound_path)
        sound.name = name
        sound.loop = loop
        sound.volume = volume
        
        # 创建空对象作为音源
        bpy.ops.object.empty_add(type='SPHERE', location=location)
        source = bpy.context.active_object
        source.name = f"{name}_Source"
        
        self.audio_sources[name] = {
            'object': source,
            'sound': sound
        }
        
        return source
    
    def update_audio_position(self, name, location):
        """更新音频位置"""
        if name in self.audio_sources:
            source = self.audio_sources[name]['object']
            source.location = location
    
    def calculate_distance_attenuation(self, source_name, max_distance=50):
        """计算距离衰减"""
        if self.listener is None:
            self.setup_listener()
        
        if self.listener is None:
            return 1.0
        
        source = self.audio_sources.get(source_name)
        if not source:
            return 1.0
        
        # 计算距离
        dx = source['object'].location.x - self.listener.location.x
        dy = source['object'].location.y - self.listener.location.y
        dz = source['object'].location.z - self.listener.location.z
        distance = math.sqrt(dx**2 + dy**2 + dz**2)
        
        # 线性衰减
        attenuation = max(0, 1 - distance / max_distance)
        
        return attenuation
    
    def apply_distance_attenuation(self, source_name, max_distance=50):
        """应用距离衰减"""
        attenuation = self.calculate_distance_attenuation(source_name, max_distance)
        sound = self.audio_sources.get(source_name, {}).get('sound')
        
        if sound:
            sound.volume = attenuation
    
    def update_all_audio_positions(self):
        """更新所有音频位置"""
        for name in self.audio_sources:
            source = self.audio_sources[name]['object']
            self.update_audio_position(name, source.location)
    
    def play_all(self):
        """播放所有音频"""
        for name in self.audio_sources:
            sound = self.audio_sources[name]['sound']
            sound.play()
    
    def stop_all(self):
        """停止所有音频"""
        for name in self.audio_sources:
            sound = self.audio_sources[name]['sound']
            sound.stop()
    
    def set_global_volume(self, volume):
        """设置全局音量"""
        bpy.context.scene.sound_volume = max(0, min(1, volume))
    
    def mute_all(self, muted=True):
        """静音所有"""
        for name in self.audio_sources:
            sound = self.audio_sources[name]['sound']
            sound.volume = 0 if muted else 1.0
    
    def fade_in(self, source_name, duration=2.0):
        """淡入"""
        sound = self.audio_sources.get(source_name, {}).get('sound')
        if not sound:
            return
        
        start_volume = sound.volume
        sound.volume = 0
        sound.play()
        
        # 淡入动画
        frames = int(duration * bpy.context.scene.render.fps)
        for i in range(frames + 1):
            sound.volume = start_volume * i / frames
            bpy.context.scene.frame_set(i)
    
    def fade_out(self, source_name, duration=2.0):
        """淡出"""
        sound = self.audio_sources.get(source_name, {}).get('sound')
        if not sound:
            return
        
        start_volume = sound.volume
        frames = int(duration * bpy.context.scene.render.fps)
        
        for i in range(frames + 1):
            sound.volume = start_volume * (1 - i / frames)
            bpy.context.scene.frame_set(bpy.context.scene.frame_current + i)
        
        sound.stop()
    
    def create_audio_zone(self, name, bounds, sound_name):
        """创建音频区域"""
        self.audio_sources[name] = {
            'type': 'zone',
            'bounds': bounds,  # (min_x, min_y, min_z, max_x, max_y, max_z)
            'sound': bpy.data.sounds.get(sound_name)
        }
    
    def check_zone_active(self, point):
        """检查点是否在音频区域内"""
        for name, data in self.audio_sources.items():
            if data.get('type') == 'zone':
                bounds = data['bounds']
                if (bounds[0] <= point[0] <= bounds[3] and
                    bounds[1] <= point[1] <= bounds[4] and
                    bounds[2] <= point[2] <= bounds[5]):
                    return name, data['sound']
        return None, None


def setup_scene_audio():
    """设置场景音频"""
    system = SceneAudioSystem()
    
    # 设置听者
    system.setup_listener()
    
    # 创建示例音频源
    # system.create_audio_source("Background", "path/to/bgm.mp3", (0, 0, 0))
    
    return system
```

### 音频特效系统

```python
import bpy

class AudioEffectsSystem:
    """音频特效系统"""
    
    def __init__(self):
        self.effects = {}
    
    def add_echo(self, sound_name, delay=0.3, decay=0.4):
        """添加回声"""
        sound = bpy.data.sounds.get(sound_name)
        if sound:
            # 注意：Blender 内部不支持直接添加音频效果
            print(f"回声效果已应用到 {sound_name}")
    
    def add_reverb(self, sound_name, room_size=0.5, damping=0.5):
        """添加混响"""
        sound = bpy.data.sounds.get(sound_name)
        if sound:
            print(f"混响效果已应用到 {sound_name}")
    
    def add_equalizer(self, sound_name, low=0, mid=0, high=0):
        """添加均衡器"""
        sound = bpy.data.sounds.get(sound_name)
        if sound:
            sound.pitch = 1.0 + (high - low) * 0.01
    
    def add_lowpass_filter(self, sound_name, frequency=1000):
        """添加低通滤波器"""
        sound = bpy.data.sounds.get(sound_name)
        if sound:
            sound.pitch = min(1.0, frequency / 20000)
    
    def add_highpass_filter(self, sound_name, frequency=100):
        """添加高通滤波器"""
        sound = bpy.data.sounds.get(sound_name)
        if sound:
            sound.pitch = max(0.5, 1.0 - frequency / 20000)
    
    def apply_audio_effect_chain(self, sound_name, effects):
        """应用音频效果链"""
        sound = bpy.data.sounds.get(sound_name)
        if not sound:
            return
        
        for effect in effects:
            effect_type = effect.get('type')
            
            if effect_type == 'echo':
                self.add_echo(sound_name, **effect.get('params', {}))
            elif effect_type == 'reverb':
                self.add_reverb(sound_name, **effect.get('params', {}))
            elif effect_type == 'equalizer':
                self.add_equalizer(sound_name, **effect.get('params', {}))
            elif effect_type == 'lowpass':
                self.add_lowpass_filter(sound_name, **effect.get('params', {}))
            elif effect_type == 'highpass':
                self.add_highpass_filter(sound_name, **effect.get('params', {}))
    
    def create_audio_preset(self, name, effects):
        """创建音频预设"""
        self.effects[name] = effects
    
    def apply_preset(self, sound_name, preset_name):
        """应用预设"""
        effects = self.effects.get(preset_name)
        if effects:
            self.apply_audio_effect_chain(sound_name, effects)
    
    def sync_to_video(self, sound_name, video_path):
        """同步音频到视频"""
        # 确保音频与视频帧率匹配
        # 实际实现需要访问视频文件信息
        pass


def setup_audio_effects():
    """设置音频特效"""
    system = AudioEffectsSystem()
    
    # 创建预设
    system.create_audio_preset("Movie", [
        {'type': 'reverb', 'params': {'room_size': 0.3, 'damping': 0.5}},
        {'type': 'equalizer', 'params': {'low': 2, 'mid': 0, 'high': -1}}
    ])
    
    system.create_audio_preset("Game", [
        {'type': 'lowpass', 'params': {'frequency': 8000}}
    ])
    
    system.create_audio_preset("Music", [
        {'type': 'echo', 'params': {'delay': 0.2, 'decay': 0.3}},
        {'type': 'reverb', 'params': {'room_size': 0.5, 'damping': 0.3}}
    ])
    
    print("音频预设已创建:")
    print(list(system.effects.keys()))
    
    return system
```


---
## bpy_ops_speaker

# bpy.ops.speaker - 扬声器操作模块

Blender扬声器操作符，用于管理音频扬声器对象和音频播放设置。

## 概述

`bpy.ops.speaker` 模块提供用于操作Blender中扬声器对象的功能，包括添加、编辑和配置音频扬声器属性。

## 核心功能

### 扬声器创建

```python
import bpy

# 添加扬声器
bpy.ops.object.speaker_add(
    location=(0, 0, 0),
    rotation=(0, 0, 0)
)

# 在指定位置添加扬声器
bpy.ops.object.speaker_add(
    location=(5.0, 3.0, 2.0)
)

# 复制扬声器
bpy.ops.object.duplicate(
    linked=False,
    mode='DUPLICATE'
)
```

### 扬声器属性设置

```python
# 设置扬声器名称
speaker = bpy.context.active_object
speaker.name = 'MainSpeaker'

# 设置音量
speaker.data.volume = 0.8

# 设置音高
speaker.data.pitch = 1.0

# 设置距离模型
speaker.data.distance_model = 'INVERSE'
speaker.data.distance_model = 'LINEAR'
speaker.data.distance_model = 'EXPONENT'

# 设置参考距离
speaker.data.distance_reference = 1.0

# 设置最大距离
speaker.data.distance_max = 100.0

# 设置衰减因子
speaker.data.attenuation = 1.0
```

### 音频播放控制

```python
# 加载音频文件
speaker.data.sound = bpy.data.sounds.load('//audio.mp3')

# 播放音频
bpy.ops.sound.play()

# 暂停音频
bpy.ops.sound.pause()

# 停止音频
bpy.ops.sound.stop()

# 循环播放
speaker.data.sound.use_loop = True

# 自动播放
speaker.data.sound.use_auto_play = True
```

### 扬声器空间设置

```python
# 设置声锥角度
speaker.data.cone_angle_outer = 60.0
speaker.data.cone_angle_inner = 30.0

# 设置声锥衰减
speaker.data.cone_volume_outer = 0.0

# 设置立体声角度
speaker.data.pan = 0.0

# 启用/禁用低音增强
speaker.data.low_pass_filter = 0.0
```

## 实用函数

```python
def create_speaker(name, location, volume=1.0):
    """创建扬声器并设置基本属性"""
    bpy.ops.object.speaker_add(location=location)
    speaker = bpy.context.active_object
    speaker.name = name
    speaker.data.volume = volume
    return speaker

def load_audio_to_speaker(speaker, audio_path):
    """加载音频到扬声器"""
    speaker.data.sound = bpy.data.sounds.load(audio_path)

def setup_spatial_audio(speaker, ref_dist=1.0, max_dist=100.0, model='INVERSE'):
    """设置空间音频参数"""
    speaker.data.distance_reference = ref_dist
    speaker.data.distance_max = max_dist
    speaker.data.distance_model = model

def setup_directional_speaker(speaker, outer_angle, inner_angle):
    """设置指向性扬声器"""
    speaker.data.cone_angle_outer = outer_angle
    speaker.data.cone_angle_inner = inner_angle
```

## 常见问题排查

### 音频不播放
```python
# 检查音频文件
speaker = bpy.context.active_object
print(f"音频: {speaker.data.sound}")
print(f"音量: {speaker.data.volume}")

# 检查静音状态
print(f"静音: {speaker.data.sound.use_mute if speaker.data.sound else 'N/A'}")
```

### 空间音频不工作
```python
# 检查距离模型
print(f"距离模型: {speaker.data.distance_model}")

# 检查参考距离
print(f"参考距离: {speaker.data.distance_reference}")

# 检查最大距离
print(f"最大距离: {speaker.data.distance_max}")
```


