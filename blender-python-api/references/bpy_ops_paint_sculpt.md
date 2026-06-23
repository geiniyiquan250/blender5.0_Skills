# bpy_ops_paint_sculpt

---
## bpy_ops_paint

# bpy.ops.paint 模块

纹理绘制操作模块，用于在顶点绘制、权重绘制和纹理绘制模式下进行绘制操作。

## 概述

bpy.ops.paint 模块包含用于在各种绘制模式下执行绘制操作的运算符。这些运算符支持顶点颜色绘制、权重绘制和图像纹理绘制。

## 常用操作

### 通用绘制操作

```python
import bpy

# 执行绘制笔触
bpy.ops.paint.brush_stroke(
    stroke=stroke_data,
    mode='NORMAL',
    ignore_background_click=False
)

# 笔触数据格式
stroke_data = [
    {"name": "", "location": (x, y, z), "mouse": (mx, my), "pressure": 1.0, "time": 0.0, "pen_flip": False, "active": True},
]
```

### 顶点颜色绘制

```python
# 切换到顶点颜色绘制模式
bpy.ops.paint.vertex_paint_toggle()

# 设置顶点颜色
bpy.ops.paint.vertex_color_set()

# 反转顶点颜色
bpy.ops.paint.vertex_color_symmetry_totrace()

# 混合顶点颜色
bpy.ops.paint.vertex_color_from_weight()

# 显示顶点颜色
bpy.ops.paint.vertex_paint_dof(
    mode='SHOW'
)

# 隐藏顶点颜色
bpy.ops.paint.vertex_paint_dof(
    mode='HIDE'
)
```

### 权重绘制

```python
# 切换到权重绘制模式
bpy.ops.paint.weight_paint_toggle()

# 设置权重值
bpy.ops.paint.weight_set()

# 从顶点组名称设置权重
bpy.ops.paint.weight_paint_from_bone_envelopes()

# 对称权重
bpy.ops.paint.weight_symmetry_totrace()

# 清理权重
bpy.ops.paint.weight_clean(threshold=0.01)

# 归一化权重
bpy.ops.paint.normalize_weight_groups()

# 混合权重
bpy.ops.paint.blend_from_mesh()
```

### 图像绘制

```python
# 切换到图像绘制模式
bpy.ops.paint.image_paint_toggle()

# 克隆源设置
bpy.ops.paint.clone_location()
bpy.ops.paint.clone_image_add()

# 投影绘制
bpy.ops.paint.project_image(image_id=0)

# 绘制到岛屿
bpy.ops.paint.image_paint_with_image_id(image_id=0)

# 清除绘制
bpy.ops.paint.image_paint_stroke(
    stroke=stroke_data,
    mode='NORMAL'
)
```

### 选择操作

```python
# 顶点选择
bpy.ops.paint.select_all(action='SELECT')
bpy.ops.paint.select_all(action='DESELECT')
bpy.ops.paint.select_all(action='INVERT')

# 选择连接
bpy.ops.paint.select_linked()
bpy.ops.paint.select_linked_pick(unselected=False)

# 选择更多/更少
bpy.ops.paint.select_more()
bpy.ops.paint.select_less()

# 框选
bpy.ops.paint.border_select()
bpy.ops.paint.circle_select()

# 路径选择
bpy.ops.paint.gradient_select()
```

### 可见性操作

```python
# 隐藏选定
bpy.ops.paint.hide_show(action='HIDE', areas='SELECTED')

# 显示所有
bpy.ops.paint.hide_show(action='SHOW', areas='ALL')

# 反转可见性
bpy.ops.paint.hide_show(action='INVERT', areas='ALL')
```

## 实际应用示例

### 自动化顶点颜色绘制

```python
def apply_vertex_colors(obj, color_data, blend_mode='MIX'):
    """应用顶点颜色数据"""
    # 切换到顶点颜色绘制模式
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.paint.vertex_paint_toggle()
    
    # 获取顶点颜色层
    if not obj.data.vertex_colors:
        obj.data.vertex_colors.new(name="Col")
    
    color_layer = obj.data.vertex_colors.active
    
    # 应用颜色
    for poly in obj.data.polygons:
        for loop_index in poly.loop_indices:
            vertex_index = obj.data.loops[loop_index].vertex_index
            if vertex_index in color_data:
                color = color_data[vertex_index]
                color_layer.data[loop_index].color = (*color, 1.0)
    
    bpy.ops.object.mode_set(mode='OBJECT')
```

### 批量权重转移

```python
def transfer_weights(source_obj, target_obj, vert_indices=None):
    """将权重从源物体转移到目标物体"""
    # 确保两个物体都在权重绘制模式
    bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
    
    # 选择目标物体
    bpy.ops.object.select_all(action='DESELECT')
    target_obj.select_set(True)
    bpy.context.view_layer.objects.active = target_obj
    
    # 设置顶点索引
    if vert_indices is None:
        vert_indices = range(len(target_obj.data.vertices))
    
    # 创建笔触数据
    stroke = []
    for i in vert_indices:
        v = target_obj.data.vertices[i]
        stroke.append({
            "name": f"weight_{i}",
            "location": v.co,
            "mouse": (0, 0),
            "pressure": 1.0,
            "time": i * 0.01,
            "pen_flip": False,
            "active": True
        })
    
    # 执行权重转移
    bpy.ops.paint.weight_paint_stroke(
        stroke=stroke,
        mode='INVERT'
    )
```

### 顶点颜色渐变

```python
def create_vertex_color_gradient(obj, axis='Z', color_start=(1, 0, 0), color_end=(0, 0, 1)):
    """创建沿轴向的顶点颜色渐变"""
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.paint.vertex_paint_toggle()
    
    # 获取顶点颜色层
    if not obj.data.vertex_colors:
        obj.data.vertex_colors.new(name="Col")
    color_layer = obj.data.vertex_colors.active
    
    # 获取轴向范围
    coords = [v.co[{'X': 0, 'Y': 1, 'Z': 2}[axis]] for v in obj.data.vertices]
    min_coord = min(coords)
    max_coord = max(coords)
    range_size = max_coord - min_coord if max_coord != min_coord else 1.0
    
    # 应用渐变颜色
    for loop in obj.data.loops:
        v = obj.data.vertices[loop.vertex_index]
        coord = v.co[{'X': 0, 'Y': 1, 'Z': 2}[axis]]
        t = (coord - min_coord) / range_size
        
        r = color_start[0] + (color_end[0] - color_start[0]) * t
        g = color_start[1] + (color_end[1] - color_start[1]) * t
        b = color_start[2] + (color_end[2] - color_start[2]) * t
        
        color_layer.data[loop.index].color = (r, g, b, 1.0)
    
    bpy.ops.object.mode_set(mode='OBJECT')
```

### 权重清理

```python
def clean_weights(obj, threshold=0.01):
    """清理小于阈值的权重"""
    bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
    
    # 获取所有顶点组
    vgs = obj.vertex_groups
    
    for vg in vgs:
        for v in obj.data.vertices:
            try:
                weight = vg.weight(v.index)
                if weight < threshold:
                    vg.remove([v])
            except Exception:
                pass
    
    bpy.ops.object.mode_set(mode='OBJECT')
```

## 使用场景

- **顶点颜色绘制**：为网格添加顶点颜色数据
- **权重绘制**：编辑顶点权重用于蒙皮
- **纹理绘制**：在3D模型上绘制纹理
- **自动化着色**：批量应用颜色和权重数据
- **数据转移**：将颜色和权重数据转移到其他模型
- **权重清理**：清理无效的权重数据

## 注意事项

- 绘制操作需要先切换到相应的绘制模式
- 顶点颜色需要先创建颜色层
- 权重绘制需要顶点组存在
- 笔触数据需要精确的位置信息
- 批量操作可能影响性能


---
## bpy_ops_sculpt

# bpy.ops.sculpt 模块

雕塑操作模块，用于在雕塑模式下编辑网格几何体。

## 概述

bpy.ops.sculpt 模块包含用于雕塑模式下编辑网格的运算符。这些运算符提供各种雕塑工具和操作，包括绘制笔触、雕刻工具和几何体修改。

## 常用操作

### 笔触操作

```python
import bpy

# 执行笔触
bpy.ops.sculpt.brush_stroke(
    stroke=stroke_data,
    mode='NORMAL',
    weight=0.5
)

# 笔触数据格式
stroke_data = [
    {"name": "", "location": (x, y, z), "mouse": (mx, my), "pressure": 1.0, "time": 0.0, "pen_flip": False, "active": True},
]
```

### 工具切换

```python
# 切换到绘制工具
bpy.ops.sculpt.set_tool(tool='DRAW')

# 切换到平滑工具
bpy.ops.sculpt.set_tool(tool='SMOOTH')

# 切换到抓取工具
bpy.ops.sculpt.set_tool(tool='GRAB')

# 切换到膨胀工具
bpy.ops.sculpt.set_tool(tool='INFLATE')

# 切换到折痕工具
bpy.ops.sculpt.set_tool(tool='CREASE')

# 切换到层工具
bpy.ops.sculpt.set_tool(tool='LAYER')

# 切换到折痕工具
bpy.ops.sculpt.set_tool(tool='BELD')

# 切换到滑动工具
bpy.ops.sculpt.set_tool(tool='SLIDE')

# 切换到旋转工具
bpy.ops.sculpt.set_tool(tool='ROTATE')

# 切换到细节工具
bpy.ops.sculpt.set_tool(tool='DETAIL')

# 切换到简化工具
bpy.ops.sculpt.set_tool(tool='SIMPLIFY')
```

### 细节操作

```python
# Dyntopo细节
bpy.ops.sculpt.dynamic_topology_toggle()

# 重新计算法线
bpy.ops.sculpt.recalculate_normals(mode='ORIGINAL')

# 对称切换
bpy.ops.sculpt.symmetry_set(sym_axis='X', toggle=True)

# 锁定表面
bpy.ops.sculpt.dyntopo_detail_size(size=6, mode='ABSOLUTE')

# 优化细节
bpy.ops.sculpt.detail_flood_fill()

# 细分网格
bpy.ops.sculpt.subdivide(

stroke_mode='DRAG')
```

### 遮罩操作

```python
# 全选遮罩
bpy.ops.sculpt.mask_flood_fill(mode='VALUE', value=1.0)

# 反转遮罩
bpy.ops.sculpt.mask_flood_fill(mode='INVERT', value=0.0)

# 清除遮罩
bpy.ops.sculpt.mask_flood_fill(mode='VALUE', value=0.0)

# 扩大遮罩
bpy.ops.sculpt.mask_expand(
    offset=2,
    iterations=1,
    mask_mode='GROW',
    smooth_mask=True
)

# 收缩遮罩
bpy.ops.sculpt.mask_expand(
    offset=2,
    iterations=1,
    mask_mode='SHRINK',
    smooth_mask=True
)

# 绘制遮罩
bpy.ops.sculpt.mask_draw(
    value=1.0,
    auto_normalize=True,
    symmetric=False
)

# 清除绘制遮罩
bpy.ops.sculpt.mask_delete(type='MASK')
```

### 对称和镜像

```python
# X轴对称
bpy.ops.sculpt.symmetrize(direction='NEGATIVE_X')

# Y轴对称
bpy.ops.sculpt.symmetrize(direction='NEGATIVE_Y')

# Z轴对称
bpy.ops.sculpt.symmetrize(direction='NEGATIVE_Z')
```

### 可见性操作

```python
# 隐藏选定
bpy.ops.sculpt.hide_show(action='HIDE', areas='MASKED')

# 显示所有
bpy.ops.sculpt.hide_show(action='SHOW', areas='ALL')

# 反转可见性
bpy.ops.sculpt.hide_show(action='INVERT', areas='ALL')

# 框选可见
bpy.ops.sculpt._border_gesture_2d(mode='HIDE', value=0.0)

# 框选遮罩
bpy.ops.sculpt.border_gesture_2d(mode='MASK', value=1.0)
```

## 实际应用示例

### 自动化雕塑笔触

```python
def apply_sculpt_stroke(obj, locations, tool='DRAW', strength=1.0):
    """在指定位置应用雕塑笔触"""
    # 切换到雕塑模式
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.mode_set(mode='SCULPT')
    
    # 设置工具
    bpy.ops.sculpt.set_tool(tool=tool)
    
    # 创建笔触数据
    stroke = []
    for i, loc in enumerate(locations):
        stroke.append({
            "name": f"stroke_{i}",
            "location": loc,
            "mouse": (0, 0),
            "pressure": strength,
            "time": i * 0.1,
            "pen_flip": False,
            "active": True
        })
    
    # 执行笔触
    bpy.ops.sculpt.brush_stroke(
        stroke=stroke,
        mode='NORMAL',
        weight=0.5
    )
```

### 批量遮罩处理

```python
def mask_object_edges(obj, distance=0.5, value=1.0):
    """遮罩物体边缘"""
    import bmesh
    from mathutils import Vector
    
    bpy.ops.object.mode_set(mode='EDIT')
    bm = bmesh.from_edit_mesh(obj.data)
    
    # 获取边缘顶点
    edge_verts = set()
    for edge in bm.edges:
        for v in edge.verts:
            edge_verts.add(v)
    
    # 遮罩边缘顶点
    mask_layer = bm.verts.layers.paint_mask.verify()
    
    for v in bm.verts:
        if v in edge_verts:
            v[mask_layer] = value
    
    bmesh.update_edit_mesh(obj.data)
    bpy.ops.object.mode_set(mode='OBJECT')

def grow_mask(obj, iterations=2):
    """扩大遮罩"""
    for _ in range(iterations):
        bpy.ops.sculpt.mask_expand(
            offset=1,
            iterations=1,
            mask_mode='GROW',
            smooth_mask=True
        )
```

### 对称雕刻脚本

```python
def symmetric_sculpt(obj, axis='X', num_iterations=5):
    """执行对称雕刻"""
    bpy.ops.object.mode_set(mode='SCULPT')
    
    # 确保对称开启
    for ax in ['X', 'Y', 'Z']:
        bpy.ops.sculpt.symmetry_set(sym_axis=ax, toggle=(ax == axis))
    
    # 应用多次雕刻笔触
    for i in range(num_iterations):
        bpy.ops.sculpt.brush_stroke(
            stroke=[{
                "name": f"stroke_{i}",
                "location": obj.location,
                "mouse": (100 + i * 10, 100),
                "pressure": 1.0,
                "time": i * 0.1,
                "pen_flip": False,
                "active": True
            }],
            mode='NORMAL',
            weight=0.5
        )
```

## 使用场景

- **数字雕刻**：创建有机形状和角色模型
- **细节雕刻**：添加表面细节和纹理
- **拓扑编辑**：使用Dyntopo动态调整网格密度
- **遮罩管理**：控制雕刻影响区域
- **对称操作**：快速创建对称模型
- **自动化脚本**：批量处理和脚本化雕刻操作

## 注意事项

- 雕塑操作需要高密度网格或Dyntopo
- Dyntopo影响性能和细节级别
- 遮罩可以保护已完成的区域
- 对称雕刻需要正确对齐模型
- 笔触数据需要精确的位置和压力值


---
## bpy_ops_paint_weight

# bpy.ops.paint.weight - 权重绘画操作模块

bpy.ops.paint.weight模块提供了权重绘画（weight painting）模式下的操作功能，用于编辑顶点组的权重值。

## 概述

权重绘画是Blender中为顶点分配权重值的重要工具，用于骨骼绑定和变形。这个模块提供了在权重绘画模式下的选择、绘制和编辑操作。

## 核心功能

### 权重选择

```python
import bpy

def select_all_vertices():
    """选择所有顶点"""
    bpy.ops.paint.select_all(action='SELECT')

def deselect_all_vertices():
    """取消选择所有顶点"""
    bpy.ops.paint.select_all(action='DESELECT')

def invert_selection():
    """反选"""
    bpy.ops.paint.select_all(action='INVERT')

def select_by_weight(min_weight, threshold=0.01):
    """按权重选择"""
    bpy.ops.paint.weight_select(
        action='SELECT',
        min_weight=min_weight,
        threshold=threshold
    )
```

### 权重操作

```python
def assign_default_weight():
    """分配默认权重"""
    bpy.ops.object.vertex_group_assign_default()

def assign_weight(value=1.0):
    """分配权重"""
    bpy.ops.object.vertex_group_assign(
        weight=value
    )

def remove_weight():
    """移除权重"""
    bpy.ops.object.vertex_group_remove_from()

def normalize_weights():
    """归一化权重"""
    bpy.ops.object.vertex_group_normalize()

def invert_weights():
    """反转权重"""
    bpy.ops.object.vertex_group_invert()
```

### 绘制操作

```python
def set_weight(value):
    """设置权重"""
    bpy.ops.paint.weight_paint_toggle()
    bpy.context.tool_settings.weight_paint.brush.weight = value

def sample_weight():
    """采样权重"""
    bpy.ops.paint.weight_sample()

def sample_group():
    """采样组"""
    bpy.ops.paint.weight_sample_group()
```

## 实用函数

### 选择工具

```python
def grow_selection():
    """扩展选择"""
    bpy.ops.paint.select_border(extend=True)

def shrink_selection():
    """收缩选择"""
    bpy.ops.paint.select_less()

def select_linked():
    """选择连接"""
    bpy.ops.paint.select_linked()

def deselect_hierarchy():
    """取消选择层级"""
    bpy.ops.paint.select_hierarchy(
        action='DESELECT',
        direction='PARENT'
    )
```

### 权重工具

```python
def blend_weights(factor=0.5):
    """混合权重"""
    bpy.ops.object.vertex_group_blend(factor=factor)

def clean_weights(threshold=0.01):
    """清理权重"""
    bpy.ops.object.vertex_group_clean(
        limit=threshold
    )

def quantize_weights(steps=4):
    """量化权重"""
    bpy.ops.object.vertex_group_quantize(
        steps=steps
    )
```

## 常见问题排查

### 选择问题

选择不准确：
- 调整选择阈值
- 检查顶点组是否存在
- 确认权重模式正确

### 权重分配失败

组问题：
- 确认选择了正确的顶点组
- 检查对象是否有顶点组
- 验证权重值范围有效


---
## bpy_ops_paint_texture

# bpy.ops.paint.texture - 纹理绘画操作模块

bpy.ops.paint.texture模块提供了纹理绘制（ texture paint）模式下的操作功能，用于直接在模型上绘制纹理。

## 概述

纹理绘制是Blender中直接在3D模型表面绘制颜色和细节的功能。这个模块提供了笔刷控制、颜色选择和绘制操作。

## 核心功能

### 笔刷设置

```python
import bpy

def set_brush_size(size):
    """设置笔刷大小"""
    bpy.context.tool_settings.image_paint.brush.size = size

def set_brush_strength(strength):
    """设置笔刷强度"""
    bpy.context.tool_settings.image_paint.brush.strength = strength

def set_brush_color(color):
    """设置笔刷颜色"""
    bpy.context.tool_settings.image_paint.brush.color = color

def set_brush_blur(blur):
    """设置笔刷模糊"""
    bpy.context.tool_settings.image_paint.brush.blur = blur
```

### 绘制操作

```python
def paint_brush():
    """绘制笔刷"""
    bpy.ops.paint.texture_paint_toggle()
    bpy.ops.paint.brush_colors_flip()

def sample_color():
    """采样颜色"""
    bpy.ops.paint.sample_brush_color(
        location=bpy.context.region.view2d.region_to_view(
            bpy.context.region.x,
            bpy.context.region.y
        )
    )

def clone_source():
    """设置克隆源"""
    bpy.ops.paint.clone_image_set()
```

### 选择操作

```python
def select_all():
    """全选"""
    bpy.ops.paint.select_all(action='SELECT')

def deselect_all():
    """取消全选"""
    bpy.ops.paint.select_all(action='DESELECT')

def invert_selection():
    """反选"""
    bpy.ops.paint.select_all(action='INVERT')

def select_circle(x, y, radius):
    """圆形选择"""
    bpy.ops.paint.face_select_border(
        x=x,
        y=y,
        radius=radius,
        mode='SET'
    )
```

## 实用函数

### 遮罩操作

```python
def add_mask():
    """添加遮罩"""
    bpy.ops.paint.add_orco_mask()

def clear_mask():
    """清除遮罩"""
    bpy.ops.paint.clear_orco_mask()

def invert_mask():
    """反相遮罩"""
    bpy.ops.paint.invert_orco_mask()
```

### 绘制工具

```python
def grab_tool():
    """抓取工具"""
    bpy.ops.paint.grab_clone()

def smudge_tool():
    """涂抹工具"""
    bpy.context.tool_settings.image_paint.brush.mask_tool = 'SMEAR'

def deformer_tool():
    """变形工具"""
    bpy.context.tool_settings.image_paint.brush.mask_tool = 'DEFORM'
```

## 常见问题排查

### 绘制问题

笔刷无响应：
- 确认纹理可编辑
- 检查UV是否正确
- 验证笔刷设置

### 颜色问题

颜色不准确：
- 检查颜色空间设置
- 确认纹理格式支持
- 验证颜色管理设置


---
## bpy_ops_paint_vertex

# bpy.ops.paint_vertex - 顶点绘制操作模块

Blender顶点绘制操作符，用于在顶点级别进行颜色和权重绘制。

## 概述

`bpy.ops.paint_vertex` 模块提供用于顶点颜色绘制和顶点权重编辑的功能。这是在顶点级别操作网格颜色和权重的核心工具。

## 核心功能

### 顶点颜色绘制

```python
import bpy

# 切换到顶点颜色模式
bpy.ops.paint.vertex_paint_toggle()

# 切换到权重绘制模式
bpy.ops.paint.weight_paint_toggle()

# 设置绘制颜色
bpy.context.tool_settings.vertex_paint.brush.color = (1.0, 0.0, 0.0)

# 设置绘制强度
bpy.context.tool_settings.vertex_paint.brush.strength = 0.8

# 设置绘制混合模式
bpy.context.tool_settings.vertex_paint.brush.blend = 'MIX'

# 设置画笔大小
bpy.context.tool_settings.vertex_paint.brush.size = 30

# 设置画笔硬度
bpy.context.tool_settings.vertex_paint.brush.hardness = 0.5
```

### 顶点颜色操作

```python
# 填充颜色
bpy.ops.paint.vertex_color_set(
    color=(0.0, 1.0, 0.0, 1.0)
)

# 混合颜色
bpy.ops.paint.vertex_color_brightness_contrast(
    brightness=0.0,
    contrast=0.0
)

# 调整色相
bpy.ops.paint.vertex_color_hsv(
    h=0.5,
    s=1.0,
    v=1.0
)

# 反转颜色
bpy.ops.paint.vertex_color_invert()

# 平滑颜色
bpy.ops.paint.vertex_color_smooth()

# 混合到目标
bpy.ops.paint.vertex_color_from_weight()
```

### 顶点权重操作

```python
# 设置权重
bpy.context.tool_settings.weight_paint.brush.weight = 0.5

# 添加权重
bpy.ops.paint.weight_grow_blur(
    factor=0.1
)

# 减少权重
bpy.ops.paint.weight_decrease(
    factor=0.1
)

# 镜像权重
bpy.ops.paint.weight_mirror(
    flip_names=True
)

# 标准化权重
bpy.ops.paint.weight_normalize_all()

# 清除未使用的权重
bpy.ops.paint.weight_clean()

# 从顶点组填充
bpy.ops.paint.weight_set()

# 混合权重
bpy.ops.paint.weight_blend(
    blend_type='MIX'
)
```

### 选择操作

```python
# 全选
bpy.ops.paint.select_all(action='SELECT')

# 取消全选
bpy.ops.paint.select_all(action='DESELECT')

# 反选
bpy.ops.paint.select_all(action='INVERT')

# 选择相似
bpy.ops.paint.select_similar(
    threshold=0.1,
    type='WEIGHT'
)

# 扩展选择
bpy.ops.paint.select_lasso(
    path=[(x, y) for x, y in [(100, 100), (200, 100), (200, 200), (100, 200)]]
)

# 选择连通的
bpy.ops.paint.select_linked()

# 取消选择连通的
bpy.ops.paint.select_linked_pick(toggle=True)
```

### 顶点组操作

```python
# 新建顶点组
bpy.ops.object.vertex_group_add()

# 重命名顶点组
bpy.ops.object.vertex_group_rename(
    name='NewName'
)

# 删除顶点组
bpy.ops.object.vertex_group_remove()

# 复制顶点组
bpy.ops.object.vertex_group_copy()

# 设置活动顶点组
bpy.context.active_object.vertex_groups.active_index = 0

# 分配到顶点组
bpy.ops.object.vertex_group_assign()

# 从顶点组移除
bpy.ops.object.vertex_group_remove_from()

# 选择顶点组
bpy.ops.object.vertex_group_select()

# 反选顶点组
bpy.ops.object.vertex_group_deselect()
```

### 权重工具

```python
# 绘制权重
bpy.ops.paint.weight_paint()

# 绘制渐变权重
bpy.ops.paint.weight_gradient(
    type='LINEAR'
)

# 绘制径向权重
bpy.ops.paint.weight_gradient(
    type='RADIAL'
)

# 刷权重
bpy.ops.paint.weight_brush()

# 克隆权重
bpy.ops.paint.weight_clone()

# 采样权重
bpy.ops.paint.weight_sample()

# 对称权重
bpy.ops.paint.weight_symmetrize()
```

## 实用函数

```python
def get_vertex_colors(mesh):
    """获取网格的所有顶点颜色层"""
    return [layer.name for layer in mesh.vertex_colors]

def get_active_vertex_color(mesh):
    """获取活动顶点颜色层"""
    if mesh.vertex_colors.active:
        return mesh.vertex_colors.active.name
    return None

def create_vertex_color_layer(mesh, layer_name):
    """创建顶点颜色层"""
    return mesh.vertex_colors.new(name=layer_name)

def set_vertex_color(mesh, color_layer, vertex_indices, color):
    """设置顶点颜色"""
    layer = mesh.vertex_colors.get(color_layer)
    if layer:
        for i in vertex_indices:
            layer.data[i].color = color

def get_vertex_weights(obj, vertex_group_name):
    """获取顶点的权重"""
    group = obj.vertex_groups.get(vertex_group_name)
    if group:
        weights = []
        for i, v in enumerate(obj.data.vertices):
            try:
                weight = group.weight(i)
                weights.append(weight)
            except:
                weights.append(0.0)
        return weights
    return []

def set_vertex_weights(obj, vertex_group_name, weights):
    """设置顶点权重"""
    group = obj.vertex_groups.get(vertex_group_name)
    if group:
        for i, weight in enumerate(weights):
            if weight > 0:
                group.add([i], weight, 'REPLACE')
            else:
                group.remove([i])

def normalize_vertex_weights(obj, vertex_group_name):
    """标准化顶点权重"""
    group = obj.vertex_groups.get(vertex_group_name)
    if group:
        weights = get_vertex_weights(obj, group.name)
        max_weight = max(weights) if weights else 1.0
        if max_weight > 0:
            normalized = [w / max_weight for w in weights]
            set_vertex_weights(obj, group.name, normalized)

def smooth_vertex_weights(obj, vertex_group_name, iterations=3):
    """平滑顶点权重"""
    for _ in range(iterations):
        weights = get_vertex_weights(obj, vertex_group_name)
        smoothed = []
        for i in range(len(weights)):
            neighbors = []
            for edge in obj.data.edges:
                if i in edge.vertices:
                    neighbor = edge.vertices[0] if edge.vertices[1] == i else edge.vertices[1]
                    if neighbor < len(weights):
                        neighbors.append(weights[neighbor])
            if neighbors:
                smoothed.append(sum(neighbors) / len(neighbors))
            else:
                smoothed.append(weights[i])
        set_vertex_weights(obj, vertex_group_name, smoothed)

def copy_vertex_colors(src_obj, dst_obj, src_layer, dst_layer):
    """复制顶点颜色"""
    src_mesh = src_obj.data
    dst_mesh = dst_obj.data
    
    src_color = src_mesh.vertex_colors.get(src_layer)
    dst_color = dst_mesh.vertex_colors.get(dst_layer)
    
    if src_color and dst_color:
        for src_loop, dst_loop in zip(src_color.data, dst_color.data):
            dst_loop.color = src_loop.color
```

## 常见问题排查

### 顶点颜色不显示
```python
# 检查顶点颜色层
mesh = bpy.context.active_object.data
print(f"顶点颜色层: {[l.name for l in mesh.vertex_colors]}")

# 检查活动层
print(f"活动层: {mesh.vertex_colors.active}")

# 启用顶点颜色显示
for poly in mesh.polygons:
    poly.use_smooth = True

# 检查材质
mat = bpy.context.active_object.active_material
if mat:
    print(f"使用顶点颜色: {mat.use_vertex_color_paint}")
    mat.use_vertex_color_paint = True
```

### 权重绘制无效果
```python
# 检查活动顶点组
obj = bpy.context.active_object
print(f"活动顶点组: {obj.vertex_groups.active}")

# 检查权重
group = obj.vertex_groups.get(obj.vertex_groups.active.name)
if group:
    weights = get_vertex_weights(obj, group.name)
    print(f"权重范围: {min(weights)} - {max(weights)}")

# 检查绘制模式
print(f"绘制模式: {bpy.context.mode}")

# 切换到权重绘制模式
bpy.ops.paint.weight_paint_toggle()
```

### 选择问题
```python
# 检查选择模式
print(f"选择模式: {bpy.context.tool_settings.select_mode}")

# 检查是否可以编辑
obj = bpy.context.active_object
print(f"可编辑: {not obj.library}")

# 重置选择
bpy.ops.paint.select_all(action='DESELECT')

# 手动选择顶点
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.select_all(action='DESELECT')
bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
```


---
## bpy_ops_paint_curve

# bpy.ops.paint_curve - 笔触曲线操作模块

Blender笔触曲线操作符，用于创建和管理绘画笔触路径。

## 概述

`bpy.ops.paint_curve` 模块提供用于在绘画工具中创建和操作笔触曲线的功能。笔触曲线定义了绘画时的笔触形状和动态效果。

## 核心功能

### 笔触曲线创建

```python
import bpy

# 添加笔触曲线点
bpy.ops.paintcurve.add(
    location=(0, 0, 0)
)

# 添加多个点
for i in range(10):
    bpy.ops.paintcurve.add(
        location=(i * 0.1, 0, 0)
    )

# 关闭笔触曲线
bpy.ops.paintcurve.close()

# 完成笔触曲线
bpy.ops.paintcurve.record()

# 取消笔触曲线
bpy.ops.paintcurve.cancel()
```

### 笔触曲线选择

```python
# 选择所有点
bpy.ops.paintcurve.select_all(
    action='SELECT'
)

# 取消选择所有点
bpy.ops.paintcurve.select_all(
    action='DESELECT'
)

# 反选
bpy.ops.paintcurve.select_all(
    action='INVERT'
)

# 选择相似的点
bpy.ops.paintcurve.select_similar(
    type='DISTANCE',
    threshold=0.1
)

# 选择区域内的点
bpy.ops.paintcurve.select_box(
    xmin=100,
    ymin=100,
    xmax=200,
    ymax=200
)

# 选择圆内的点
bpy.ops.paintcurve.select_circle(
    x=150,
    y=150,
    radius=50
)

# 套索选择
bpy.ops.paintcurve.select_lasso(
    path=[(x, y) for x, y in [(100, 100), (200, 100), (200, 200), (100, 200)]]
)
```

### 笔触曲线编辑

```python
# 删除选中点
bpy.ops.paintcurve.delete()

# 移除点
bpy.ops.paintcurve.remove()

# 分离点
bpy.ops.paintcurve.separate()

# 连接点
bpy.ops.paintcurve.make_segment()

# 断开连接
bpy.ops.paintcurve.break_segment()

# 滑动点
bpy.ops.paintcurve.slide(
    factor=0.5
)

# 平滑点
bpy.ops.paintcurve.smooth()

# 简化曲线
bpy.ops.paintcurve.simplify(
    factor=0.1
)
```

### 笔触曲线变换

```python
# 移动选中点
bpy.ops.paintcurve.move(
    delta=(1.0, 0.0, 0.0)
)

# 旋转
bpy.ops.paintcurve.rotate(
    angle=45
)

# 缩放
bpy.ops.paintcurve.scale(
    factor=1.5
)

# 应用变换
bpy.ops.paintcurve.transform_apply()

# 重置变换
bpy.ops.paintcurve.transform_reset()
```

### 笔触曲线属性

```python
# 设置点权重
bpy.ops.paintcurve.set_weight(
    value=1.0
)

# 设置点半径
bpy.ops.paintcurve.set_radius(
    radius=10
)

# 设置点颜色
bpy.ops.paintcurve.set_color(
    color=(1.0, 0.0, 0.0, 1.0)
)

# 设置点硬度
bpy.ops.paintcurve.set_hardness(
    hardness=0.8
)

# 设置点时间
bpy.ops.paintcurve.set_time(
    time=0.0
)

# 随机化点属性
bpy.ops.paintcurve.randomize(
    factor=0.1
)
```

### 笔触曲线工具

```python
# 插值点
bpy.ops.paintcurve.subdivide(
    cuts=1
)

# 细分曲线
bpy.ops.paintcurve.subdivide_curve(
    number_cuts=2
)

# 倒角点
bpy.ops.paintcurve.bevel_point(
    radius=0.1
)

# 倒角曲线
bpy.ops.paintcurve.bevel_curve(
    radius=0.1
)

# 转换点类型
bpy.ops.paintcurve.convert_point_type(
    type='AUTO'
)

# 切换切线
bpy.ops.paintcurve.toggle_handle_type()
```

## 实用函数

```python
def create_brush_stroke_curve(name, points):
    """创建笔触曲线"""
    bpy.ops.paintcurve.new(name=name)
    curve = bpy.context.active_object
    
    for point in points:
        bpy.ops.paintcurve.add(location=point)
    
    bpy.ops.paintcurve.close()
    
    return curve

def add_noise_to_curve(curve, amount=0.1):
    """为曲线添加噪点"""
    import random
    
    for i, point in enumerate(curve.data.points):
        if point.select:
            point.location += (
                random.uniform(-amount, amount),
                random.uniform(-amount, amount),
                random.uniform(-amount, amount)
            )

def smooth_curve_points(curve, iterations=3):
    """平滑曲线点"""
    for _ in range(iterations):
        points = curve.data.points
        for i in range(len(points)):
            if i > 0 and i < len(points) - 1:
                prev_point = points[i - 1]
                next_point = points[i + 1]
                
                avg_location = (
                    (prev_point.location.x + next_point.location.x) / 2,
                    (prev_point.location.y + next_point.location.y) / 2,
                    (prev_point.location.z + next_point.location.z) / 2
                )
                
                points[i].location = avg_location

def sample_curve_to_keyframes(curve, num_samples=10):
    """将曲线采样为关键帧"""
    points = curve.data.points
    step = max(1, len(points) // num_samples)
    
    for i in range(0, len(points), step):
        point = points[i]
        bpy.context.scene.frame_set(i)
        point.select_set(True)
    
    return [i for i in range(0, len(points), step)]

def create_radial_stroke(name, center, radius, num_points=12):
    """创建放射状笔触曲线"""
    import math
    
    bpy.ops.paintcurve.new(name=name)
    curve = bpy.context.active_object
    
    for i in range(num_points):
        angle = (2 * math.pi * i) / num_points
        x = center[0] + radius * math.cos(angle)
        y = center[1] + radius * math.sin(angle)
        z = center[2]
        
        bpy.ops.paintcurve.add(location=(x, y, z))
    
    bpy.ops.paintcurve.close()
    
    return curve

def create_spiral_stroke(name, center, max_radius, turns, points_per_turn):
    """创建螺旋笔触曲线"""
    import math
    
    bpy.ops.paintcurve.new(name=name)
    curve = bpy.context.active_object
    
    total_points = turns * points_per_turn
    
    for i in range(total_points):
        angle = (2 * math.pi * i) / points_per_turn
        radius = (max_radius * i) / total_points
        
        x = center[0] + radius * math.cos(angle)
        y = center[1] + radius * math.sin(angle)
        z = center[2]
        
        bpy.ops.paintcurve.add(location=(x, y, z))
    
    bpy.ops.paintcurve.close()
    
    return curve
```

## 常见问题排查

### 笔触曲线不可见
```python
# 检查曲线是否存在
print(f"笔触曲线数: {len(bpy.data.paint_curves)}")

# 检查活动曲线
print(f"活动曲线: {bpy.context.active_object}")

# 检查显示设置
curve = bpy.context.active_object
print(f"显示: {curve.show_viewport}")
print(f"渲染: {curve.show_render}")

# 启用显示
curve.show_viewport = True
curve.show_render = True

# 检查点数量
print(f"点数: {len(curve.data.points)}")
```

### 点无法选择
```python
# 检查选择模式
print(f"选择模式: {bpy.context.tool_settings.select_mode}")

# 重置选择
bpy.ops.paintcurve.select_all(action='DESELECT')

# 切换到点选择模式
bpy.ops.paintcurve.select_mode(type='POINT')

# 检查点是否被锁定
for point in curve.data.points:
    print(f"锁定: {point.lock}")
```

### 曲线编辑无效果
```python
# 检查曲线数据
curve = bpy.context.active_object
print(f"点数: {len(curve.data.points)}")
print(f"段数: {len(curve.data.splines)}")

# 重新创建曲线
bpy.ops.paintcurve.new(name='NewCurve')

# 清除历史
bpy.ops.paintcurve.clear()

# 检查曲线类型
print(f"曲线类型: {curve.data.splines[0].type if curve.data.splines else 'N/A'}")
```


---
## bpy_ops_paintcurve

# bpy.ops.paintcurve - 绘制曲线操作模块

Blender绘制曲线操作符，用于管理绘画曲线预设和工作流程。

## 概述

`bpy.ops.paintcurve` 模块提供用于创建、编辑和应用绘制曲线预设的功能。绘制曲线用于控制绘画工具的笔触特性，如笔刷大小、不透明度等。

## 核心功能

### 曲线选择操作

```python
import bpy

# 全选绘制曲线
bpy.ops.paintcurve.select_all(action='SELECT')

# 全不选
bpy.ops.paintcurve.select_all(action='DESELECT')

# 反选
bpy.ops.paintcurve.select_all(action='INVERT')

# 选择激活的绘制曲线
bpy.ops.paintcurve.select_active()

# 根据名称选择
bpy.ops.paintcurve.select_by_name(name='curve_name')

# 选择相似的绘制曲线
bpy.ops.paintcurve.select_similar(type='CURVE')
```

### 绘制曲线创建

```python
# 创建新的绘制曲线
bpy.ops.paintcurve.new(name='New Paint Curve')

# 从曲线创建
bpy.ops.paintcurve.duplicate()

# 复制绘制曲线
bpy.ops.paintcurve.copy()

# 粘贴绘制曲线
bpy.ops.paintcurve.paste()

# 从默认曲线创建
bpy.ops.paintcurve.reset_to_default()

# 创建对称曲线
bpy.ops.paintcurve.symmetrize()
```

### 绘制曲线编辑

```python
# 删除绘制曲线
bpy.ops.paintcurve.delete()

# 重命名绘制曲线
bpy.ops.paintcurve.rename(name='new_name')

# 编辑绘制曲线
bpy.ops.paintcurve.edit()

# 应用绘制曲线
bpy.ops.paintcurve.apply()

# 重置绘制曲线
bpy.ops.paintcurve.reset()

# 反转曲线
bpy.ops.paintcurve.invert()

# 翻转曲线
bpy.ops.paintcurve.flip()

# 平滑曲线
bpy.ops.paintcurve.smooth(iterations=5)

# 简化曲线点
bpy.ops.paintcurve.simplify(threshold=0.01)
```

### 曲线点操作

```python
# 添加曲线点
bpy.ops.paintcurve.add_point(x=0.5, y=0.5)

# 删除曲线点
bpy.ops.paintcurve.delete_point()

# 选择曲线点
bpy.ops.paintcurve.select_point(index=0)

# 移动曲线点
bpy.ops.paintcurve.move_point(index=0, x=0.6, y=0.6)

# 设置曲线点句柄
bpy.ops.paintcurve.set_handle(index=0, handle_type='AUTO')

# 设置切线
bpy.ops.paintcurve.set_tangent(index=0, angle=45.0)
```

### 曲线变换

```python
# 移动曲线
bpy.ops.paintcurve.translate(x=0.1, y=0.1)

# 缩放曲线
bpy.ops.paintcurve.scale(factor=1.5)

# 旋转曲线
bpy.ops.paintcurve.rotate(angle=45.0))

# 曲线重采样
bpy.ops.paintcurve.resample(number_points=10))

# 曲线插值
bpy.ops.paintcurve.interpolate(type='BEZIER')
```

### 绘制曲线类型

```python
# 设置为线性曲线
bpy.ops.paintcurve.set_type(type='LINEAR')

# 设置为贝塞尔曲线
bpy.ops.paintcurve.set_type(type='BEZIER')

# 设置为自定义曲线
bpy.ops.paintcurve.set_type(type='CUSTOM')

# 设置插值模式
bpy.ops.paintcurve.set_interpolation(mode='LINEAR')

# 设置平滑模式
bpy.ops.paintcurve.set_smoothing(mode='CARDINAL')
```

### 导出导入

```python
# 导出绘制曲线
bpy.ops.paintcurve.export(filepath='//paintcurve.json')

# 导入绘制曲线
bpy.ops.paintcurve.import_(filepath='//paintcurve.json')

# 导出所有曲线
bpy.ops.paintcurve.export_all(filepath='//all_curves.json')

# 批量导入
bpy.ops.paintcurve.import_batch(filepaths=['//curve1.json', '//curve2.json'])
```

### 预设管理

```python
# 应用预设
bpy.ops.paintcurve.preset_apply(preset='Soft Brush')

# 保存为预设
bpy.ops.paintcurve.preset_save(name='My Preset')

# 删除预设
bpy.ops.paintcurve.preset_delete(name='My Preset')

# 重置预设
bpy.ops.paintcurve.preset_reset()

# 列出所有预设
bpy.ops.paintcurve.preset_list()
```

### 绘制曲线应用

```python
# 应用到当前笔刷
bpy.ops.paintcurve.apply_to_brush()

# 应用到所有笔刷
bpy.ops.paintcurve.apply_to_all_brushes()

# 从笔刷读取
bpy.ops.paintcurve.read_from_brush()

# 设置笔刷曲线
bpy.ops.paintcurve.set_brush_curve(curve_name='curve_name')

# 清除笔刷曲线
bpy.ops.paintcurve.clear_brush_curve()
```

## 实用函数

```python
def get_all_paint_curves():
    """获取所有绘制曲线"""
    return list(bpy.data.paint_curves)

def get_paint_curve_by_name(name):
    """根据名称获取绘制曲线"""
    return bpy.data.paint_curves.get(name)

def get_active_paint_curve():
    """获取当前激活的绘制曲线"""
    return bpy.context.paint_curve

def save_curve_to_file(curve, filepath):
    """保存曲线到文件"""
    bpy.ops.paintcurve.select_all(action='DESELECT')
    curve.select = True
    bpy.context.paint_curve = curve
    bpy.ops.paintcurve.export(filepath=filepath)

def load_curve_from_file(filepath):
    """从文件加载曲线"""
    bpy.ops.paintcurve.import_(filepath=filepath)
    return bpy.context.paint_curve

def copy_curve_settings(source, target):
    """复制曲线设置"""
    target.data = source.data.copy()
    target.name = target.name or source.name

def create_brush_curve(brush_name, curve_type='SOFT'):
    """为笔刷创建预设曲线"""
    brush = bpy.data.brushes.get(brush_name)
    if not brush:
        return None
    
    if curve_type == 'SOFT':
        points = [(0.0, 0.0), (0.2, 0.1), (0.5, 0.5), (0.8, 0.9), (1.0, 1.0)]
    elif curve_type == 'HARD':
        points = [(0.0, 0.0), (0.0, 1.0), (1.0, 1.0)]
    else:
        points = [(0.0, 0.0), (0.5, 0.5), (1.0, 1.0)]
    
    curve = bpy.data.paint_curves.new(name=f"{brush_name}_Curve")
    curve.data = points
    return curve

def batch_apply_curve_to_brushes(curve, brush_names):
    """批量应用曲线到多个笔刷"""
    for brush_name in brush_names:
        brush = bpy.data.brushes.get(brush_name)
        if brush:
            bpy.ops.paintcurve.select_all(action='DESELECT')
            curve.select = True
            bpy.context.paint_curve = curve
            bpy.ops.paintcurve.apply_to_brush()

def compare_curves(curve1, curve2):
    """比较两条曲线是否相同"""
    if len(curve1.data) != len(curve2.data):
        return False
    return all(abs(p1 - p2) < 0.001 
               for p1, p2 in zip(curve1.data, curve2.data))
```

## 常见问题排查

### 曲线不生效
```python
# 检查曲线是否激活
print(f"当前曲线: {bpy.context.paint_curve}")

# 检查笔刷设置
brush = bpy.context.brush
print(f"笔刷曲线: {brush.curve}")

# 重新应用曲线
bpy.ops.paintcurve.apply_to_brush()

# 检查曲线类型
print(f"曲线类型: {bpy.context.paint_curve.type}")
```

### 曲线点编辑问题
```python
# 确保在编辑模式
print(f"当前模式: {bpy.context.paint_curve_editor}")

# 重置曲线
bpy.ops.paintcurve.reset()

# 重新创建
bpy.ops.paintcurve.new(name='Reset Curve')

# 手动设置点
curve = bpy.context.paint_curve
curve.data = [(0.0, 0.0), (0.5, 0.5), (1.0, 1.0)]
```

### 导出导入问题
```python
# 检查文件路径
import os
print(f"文件存在: {os.path.exists('//paintcurve.json')}")

# 尝试不同的导出格式
bpy.ops.paintcurve.export(filepath='//paintcurve.json', format='JSON')

# 检查数据完整性
curve = bpy.context.paint_curve
print(f"曲线点数: {len(curve.data)}")
print(f"曲线类型: {curve.type}")
```


---
## bpy_ops_sculpt_curves

# bpy.ops.sculpt_curves - 雕刻曲线操作模块

Blender雕刻曲线操作符，用于在雕刻模式下创建和编辑曲线。

## 概述

`bpy.ops.sculpt_curves` 模块提供用于在Blender雕刻工作区中创建、编辑和操作曲线的功能。这是Blender 4.0+版本中新增的曲线雕刻系统。

## 核心功能

### 曲线选择操作

```python
import bpy

# 全选曲线
bpy.ops.sculpt_curves.select_all(action='SELECT')

# 全不选
bpy.ops.sculpt_curves.select_all(action='DESELECT')

# 反选
bpy.ops.sculpt_curves.select_all(action='INVERT')

# 选择更多
bpy.ops.sculpt_curves.select_more()

# 选择更少
bpy.ops.sculpt_curves.select_less()

# 选择相似曲线
bpy.ops.sculpt_curves.select_similar(type='LENGTH')

# 选择连接的曲线
bpy.ops.sculpt_curves.select_linked()

# 边框选择
bpy.ops.sculpt_curves.select_border(gesture_mode=0, xmin=100, xmax=300, ymin=100, ymax=300)

# 圆形选择
bpy.ops.sculpt_curves.select_circle(x=200, y=200, radius=50, mode='ADD')

# 套索选择
bpy.ops.sculpt_curves.select_lasso(path=[(100, 100), (200, 100), (200, 200), (100, 200)])

# 根据长度选择
bpy.ops.sculpt_curves.select_by_length(min_length=0.1, max_length=10.0)

# 根据方向选择
bpy.ops.sculpt_curves.select_by_direction(direction='UP', threshold=0.5)
```

### 曲线创建

```python
# 从点创建曲线
bpy.ops.sculpt_curves.create_curve_from_points()

# 创建新曲线
bpy.ops.sculpt_curves.primitive_curve_add()

# 从现有几何体创建曲线
bpy.ops.sculpt_curves.duplicate_curves()

# 从网格边创建曲线
bpy.ops.sculpt_curves.create_from_edges()

# 从选中点创建曲线
bpy.ops.sculpt_curves.create_curve_from_selection()

# 添加曲线点
bpy.ops.sculpt_curves.add_point(location=(0, 0, 0))

# 添加多个点
bpy.ops.sculpt_curves.add_point_chain(count=10)

# 闭合曲线
bpy.ops.sculpt_curves.close_curve()

# 断开曲线
bpy.ops.sculpt_curves.split_curve()
```

### 曲线编辑

```python
# 删除选中曲线
bpy.ops.sculpt_curves.delete(type='SELECTED')

# 删除点
bpy.ops.sculpt_curves.delete_points()

# 删除曲线段
bpy.ops.sculpt_curves.delete_segment()

# 细分曲线
bpy.ops.sculpt_curves.subdivide(number_cuts=1)

# 简化曲线
bpy.ops.sculpt_curves.simplify(threshold=0.01)

# 合并曲线
bpy.ops.sculpt_curves.merge()

# 合并到点
bpy.ops.sculpt_curves.merge_at_cursor()

# 倒角曲线点
bpy.ops.sculpt_curves.bevel_point(radius=0.1)

# 改变曲线类型
bpy.ops.sculpt_curves.change_curve_type(type='POLY')

# 平滑曲线
bpy.ops.sculpt_curves.smooth(iterations=5)

# 重插值曲线
bpy.ops.sculpt_curves.resample(number_points=20)
```

### 曲线变换

```python
# 移动曲线
bpy.ops.sculpt_curves.translate(value=(1.0, 0.0, 0.0))

# 旋转曲线
bpy.ops.sculpt_curves.rotate(angle=45.0, axis='Z')

# 缩放曲线
bpy.ops.sculpt_curves.scale(value=(1.5, 1.5, 1.5))

# 变换曲线
bpy.ops.sculpt_curves.transform(mode='TRANSLATION')

# 吸附到网格
bpy.ops.sculpt_curves.snap_to_grid()

# 吸附到原点
bpy.ops.sculpt_curves.snap_to_origin()

# 吸附到选中对象
bpy.ops.sculpt_curves.snap_to_selected()

# 应用变换
bpy.ops.sculpt_curves.apply_transform()

# 重置变换
bpy.ops.sculpt_curves.reset_transform()

# 复制曲线
bpy.ops.sculpt_curves.duplicate()

# 对称曲线
bpy.ops.sculpt_curves.symmetry(axis='X')
```

### 曲线雕刻工具

```python
# 使用雕刻笔刷
bpy.ops.sculpt_curves.brush_stroke()

# 拉伸手柄
bpy.ops.sculpt_curves.grow_shrink(handle='LEFT', factor=0.1)

# 调整曲线粗细
bpy.ops.sculpt_curves.change_radius(factor=1.5)

# 调整所有曲线粗细
bpy.ops.sculpt_curves.change_radius_all(factor=1.5)

# 曲线吸附
bpy.ops.sculpt_curves.sculpt_stroke()

# 曲线推挤
bpy.ops.sculpt_curves.push_stroke()

# 曲线平滑
bpy.ops.sculpt_curves.smooth_stroke()

# 曲线添加点
bpy.ops.sculpt_curves.add_points_stroke()

# 曲线抽吸
bpy.ops.sculpt_curves.suck_stroke()
```

### 曲线样式设置

```python
# 设置曲线颜色
bpy.ops.sculpt_curves.set_color(color=(1.0, 0.0, 0.0, 1.0))

# 设置曲线材质
bpy.ops.sculpt_curves.set_material(material_name='CurveMaterial')

# 设置曲线粗细
bpy.ops.sculpt_curves.set_radius(radius=0.05)

# 设置点大小
bpy.ops.sculpt_curves.set_point_size(size=0.1)

# 设置曲线插值
bpy.ops.sculpt_curves.set_interpolation(type='BSPLINE')

# 应用材质到所有曲线
bpy.ops.sculpt_curves.set_material_all(material_name='CurveMaterial')

# 清除材质
bpy.ops.sculpt_curves.clear_material()
```

### 曲线UV和纹理

```python
# 设置曲线UV
bpy.ops.sculpt_curves.uv_set(uv_layer='UVMap')

# 应用UV
bpy.ops.sculpt_curves.uv_apply()

# 设置曲线纹理
bpy.ops.sculpt_curves.texture_set(texture_name='CurveTexture')

# 应用纹理到所有曲线
bpy.ops.sculpt_curves.texture_set_all(texture_name='CurveTexture')

# 清除纹理
bpy.ops.sculpt_curves.texture_clear()
```

### 曲线动画

```python
# 插入关键帧
bpy.ops.sculpt_curves.insert_keyframe()

# 删除关键帧
bpy.ops.sculpt_curves.delete_keyframe()

# 清除动画
bpy.ops.sculpt_curves.clear_keyframe()

# 设置曲线驱动的形状键
bpy.ops.sculpt_curves.shape_key_add()

# 移除形状键
bpy.ops.sculpt_curves.shape_key_remove()
```

### 曲线数据操作

```python
# 转换为网格
bpy.ops.sculpt_curves.convert_to_mesh()

# 转换为曲面
bpy.ops.sculpt_curves.convert_to_surface()

# 导出曲线数据
bpy.ops.sculpt_curves.export_data(filepath='//curves.json')

# 导入曲线数据
bpy.ops.sculpt_curves.import_data(filepath='//curves.json')

# 重新计算法线
bpy.ops.sculpt_curves.recalculate_normals()

# 重新计算切线
bpy.ops.sculpt_curves.recalculate_tangents()
```

## 实用函数

```python
def get_selected_curves():
    """获取所有选中的曲线"""
    return [c for c in bpy.context.active_object.data.curves if c.select]

def get_curve_points(curve):
    """获取曲线上的所有点"""
    return list(curve.points)

def get_curve_length(curve):
    """计算曲线长度"""
    return sum(p.co.distance_to(p_next.co) 
               for p, p_next in zip(curve.points[:-1], curve.points[1:]))

def create_curve_from_points(points, name='NewCurve'):
    """从点列表创建曲线"""
    obj = bpy.context.active_object
    curve_data = obj.data
    curve_data.curves[0].points.foreach_set('co', [c for p in points for c in p])
    curve_data.curves[0].name = name
    return curve_data.curves[0]

def batch_apply_material(mat_name):
    """批量应用材质到所有曲线"""
    for curve in bpy.context.active_object.data.curves:
        curve.material_index = bpy.data.materials.find(mat_name)

def select_curves_by_length(min_len, max_len):
    """按长度选择曲线"""
    for curve in bpy.context.active_object.data.curves:
        length = get_curve_length(curve)
        curve.select = (min_len <= length <= max_len)

def normalize_curve(curve):
    """归一化曲线"""
    center = get_curve_center(curve)
    for point in curve.points:
        point.co -= center
```

## 常见问题排查

### 曲线不显示
```python
# 检查曲线数据是否存在
print(f"曲线数量: {len(bpy.context.active_object.data.curves)}")

# 检查渲染属性
obj = bpy.context.active_object
print(f"渲染: {obj.display.rendered_type}")

# 检查视口显示
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        for space in area.spaces:
            if space.type == 'VIEW_3D':
                print(f"显示曲线: {space.show_curves}")
```

### 选择问题
```python
# 重置选择
bpy.ops.sculpt_curves.select_all(action='DESELECT')

# 手动选择曲线
for curve in bpy.context.active_object.data.curves:
    curve.select = True

# 检查选择模式
print(f"选择模式: {bpy.context.scene.tool_settings.sculpt_curves_select_mode}")
```

### 性能问题
```python
# 减少曲线点数量
for curve in bpy.context.active_object.data.curves:
    bpy.ops.sculpt_curves.simplify(threshold=0.1)

# 降低显示精度
import bpy
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        for space in area.spaces:
            if space.type == 'VIEW_3D':
                space.curves_resolution = 8
```


