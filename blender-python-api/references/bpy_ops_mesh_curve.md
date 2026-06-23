# bpy_ops_mesh_curve

---
## bpy_ops_mesh

# bpy.ops.mesh - Mesh Operations Module

Blender 网格操作符，用于在编辑模式下进行网格顶点的创建、编辑、变换和管理。

## Overview

`bpy.ops.mesh` 模块包含网格编辑器中的所有操作命令，涵盖顶点、边、面的选择、创建、删除、变换等核心功能。这个模块主要用于编辑模式下的网格建模操作。

## 核心功能

### 网格选择操作

```python
import bpy

# 全选
bpy.ops.mesh.select_all(action='SELECT')

# 全不选
bpy.ops.mesh.select_all(action='DESELECT')

# 反选
bpy.ops.mesh.select_all(action='INVERT')

# 选择更多
bpy.ops.mesh.select_more(use_face_step=True)

# 选择更少
bpy.ops.mesh.select_less(use_face_step=True)

# 选择相似
bpy.ops.mesh.select_similar(type='COPLANAR', threshold=0.01)
bpy.ops.mesh.select_similar(type='NORMAL', threshold=0.01)
bpy.ops.mesh.select_similar(type='MATERIAL', threshold=0.01)
bpy.ops.mesh.select_similar(type='FACE', threshold=0.01)
bpy.ops.mesh.select_similar(type='VIRTUAL', threshold=0.01)

# 选择循环边
bpy.ops.mesh.loop_multi_select(ring=False)
bpy.ops.mesh.loop_multi_select(ring=True)

# 选择边循环
bpy.ops.mesh.edge_loop_select()
bpy.ops.mesh.select_vertex_path()

# 选择面循环
bpy.ops.mesh.faces_select_linked()

# 选择连接面
bpy.ops.mesh.select_linked()

# 边框选择
bpy.ops.mesh.select_border(gesture_mode=0, xmin=100, xmax=300, ymin=100, ymax=300)

# 圆形选择
bpy.ops.mesh.select_circle(x=200, y=200, radius=50, mode='ADD')

# 套索选择
bpy.ops.mesh.select_lasso(path=[(100, 100), (200, 100), (200, 200), (100, 200)])

# 切变选择
bpy.ops.mesh.select_mode(type='VERT')
bpy.ops.mesh.select_mode(type='EDGE')
bpy.ops.mesh.select_mode(type='FACE')

def select_all_vertices():
    """选择所有顶点"""
    bpy.ops.mesh.select_mode(type='VERT')
    bpy.ops.mesh.select_all(action='SELECT')
    return [v for v in bpy.context.active_object.data.vertices if v.select]

def select_all_edges():
    """选择所有边"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.select_all(action='SELECT')
    return [e for e in bpy.context.active_object.data.edges if e.select]

def select_all_faces():
    """选择所有面"""
    bpy.ops.mesh.select_mode(type='FACE')
    bpy.ops.mesh.select_all(action='SELECT')
    return [f for f in bpy.context.active_object.data.polygons if f.select]

def deselect_all():
    """取消所有选择"""
    bpy.ops.mesh.select_all(action='DESELECT')
```

### 顶点操作

```python
# 创建顶点
bpy.ops.mesh.duplicates_extrude()

# 删除顶点
bpy.ops.mesh.delete(type='VERT')

# 合并顶点
bpy.ops.mesh.merge(type='CENTER', uvs=False)
bpy.ops.mesh.merge(type='CURSOR', uvs=False)
bpy.ops.mesh.merge(type='COLLAPSE', uvs=False)
bpy.ops.mesh.merge(type='AT_CURSOR', uvs=False)

# 细分顶点
bpy.ops.mesh.subdivide(number_cuts=1)
bpy.ops.mesh.subdivide(number_cuts=2)

# 细分边
bpy.ops.mesh.subdivide_edgeloop(number_cuts=1)

# 倒角顶点
bpy.ops.mesh.bevel_vertex(offset=0.1, segments=3)

# 极点切变
bpy.ops.mesh.poke()

# 分离顶点
bpy.ops.mesh.separate(type='SELECTED')
bpy.ops.mesh.separate(type='MATERIAL')
bpy.ops.mesh.separate(type='LOOSE')

def create_vertex(location):
    """创建顶点"""
    bpy.ops.mesh.primitive_plane_add(size=0, location=location)
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.merge(type='CENTER')

def delete_selected_vertices():
    """删除选中的顶点"""
    bpy.ops.mesh.select_mode(type='VERT')
    bpy.ops.mesh.delete(type='VERT')

def merge_vertices_to_center():
    """合并顶点到中心"""
    bpy.ops.mesh.select_mode(type='VERT')
    bpy.ops.mesh.merge(type='CENTER')

def subdivide_edges(iterations=1):
    """细分边"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.select_all(action='SELECT')
    for _ in range(iterations):
        bpy.ops.mesh.subdivide(number_cuts=1)

def bevel_vertex(obj, offset=0.05, segments=3):
    """倒角顶点"""
    bpy.ops.mesh.select_mode(type='VERT')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.bevel_vertex(offset=offset, segments=segments)
```

### 边操作

```python
# 创建边
bpy.ops.mesh.edge_vert_add()

# 删除边
bpy.ops.mesh.delete(type='EDGE')

# 旋转边
bpy.ops.mesh.rotate_edges(edge_mode='ADJ')

# 折叠边
bpy.ops.mesh.edge_collapse()

# 细分边
bpy.ops.mesh.subdivide(number_cuts=1)

# 倒角边
bpy.ops.mesh.bevel(offset=0.02, segments=3, affect='EDGES')

# 切变边
bpy.ops.mesh.shear(angle=math.radians(45))

# 创建边环
bpy.ops.mesh.insert_loop()

# 延伸边
bpy.ops.mesh.extrude_region_move()

# 旋转边
bpy.ops.mesh.rotate()

# 标记接缝
bpy.ops.mesh.mark_seam(clear=False)
bpy.ops.mesh.mark_seam(clear=True)

# 标记锐边
bpy.ops.mesh.mark_sharp(clear=False)
bpy.ops.mesh.mark_sharp(clear=True)

# 平滑边
bpy.ops.mesh.set_edges_visible()

def create_edge_from_verts(vert1, vert2):
    """从两个顶点创建边"""
    bpy.ops.mesh.select_all(action='DESELECT')
    vert1.select = True
    vert2.select = True
    bpy.ops.mesh.edge_vert_add()

def delete_selected_edges():
    """删除选中的边"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.delete(type='EDGE')

def rotate_selected_edges():
    """旋转选中的边"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.rotate_edges(edge_mode='ADJ')

def bevel_selected_edges(offset=0.02, segments=3):
    """倒角选中的边"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.bevel(offset=offset, segments=segments, affect='EDGES')

def add_edge_loop(count=1):
    """添加边循环"""
    bpy.ops.mesh.select_all(action='SELECT')
    for _ in range(count):
        bpy.ops.mesh.insert_loop()

def mark_seam_selected():
    """标记接缝"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.mark_seam(clear=False)

def clear_seam_selected():
    """清除接缝"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.mark_seam(clear=True)

def mark_sharp_selected():
    """标记锐边"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.mark_sharp(clear=False)

def clear_sharp_selected():
    """清除锐边"""
    bpy.ops.mesh.select_mode(type='EDGE')
    bpy.ops.mesh.mark_sharp(clear=True)
```

### 面操作

```python
# 创建面
bpy.ops.mesh.fill()

# 删除面
bpy.ops.mesh.delete(type='FACE')

# 三角化面
bpy.ops.mesh.quads_convert_to_tris(quad_method='BEAUTY', ngon_method='BEAUTY')

# 三角面转四边面
bpy.ops.mesh.tris_convert_to_quads(limit_mode='ANGLE', angle_limit=math.radians(40))

# 重新三角化
bpy.ops.mesh.recalc_face_normals()

# 翻转法线
bpy.ops.mesh.flip_normals()

# 分离面
bpy.ops.mesh.split()

# 填充孔洞
bpy.ops.mesh.fill_holes(sides=4)

# 凸包
bpy.ops.mesh.convex_hull()

# 内部面
bpy.ops.mesh.select_non_manifold(extend=True)

# 溶解面
bpy.ops.mesh.dissolve_faces(use_verts=True, use_face_split=True)

# 三角化
bpy.ops.mesh.primitive_triangulate()

def create_face_from_vertices(vertices):
    """从顶点创建面"""
    bpy.ops.mesh.select_all(action='DESELECT')
    for v in vertices:
        v.select = True
    bpy.ops.mesh.fill()

def delete_selected_faces():
    """删除选中的面"""
    bpy.ops.mesh.select_mode(type='FACE')
    bpy.ops.mesh.delete(type='FACE')

def triangulate_faces():
    """三角化所有面"""
    bpy.ops.mesh.select_mode(type='FACE')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.quads_convert_to_tris()

def quads_convert_from_tris():
    """三角面转四边面"""
    bpy.ops.mesh.select_mode(type='FACE')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.tris_convert_to_quads()

def flip_normals_selected():
    """翻转选中面的法线"""
    bpy.ops.mesh.select_mode(type='FACE')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.flip_normals()

def fill_holes(max_sides=4):
    """填充孔洞"""
    bpy.ops.mesh.fill_holes(sides=max_sides)

def dissolve_selected_faces():
    """溶解选中的面"""
    bpy.ops.mesh.select_mode(type='FACE')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.dissolve_faces()

def select_non_manifold():
    """选择非流形几何"""
    bpy.ops.mesh.select_non_manifold(extend=True)
```

### 网格变换操作

```python
# 挤出
bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value": (0, 0, 1)})
bpy.ops.mesh.extrude_region_shrink_fatten(
    TRANSFORM_OT_shrink_fatten={"value": 0.1, "use_even_offset": True}
)

# 挤出个别
bpy.ops.mesh.extrude_vertex_move()
bpy.ops.mesh.extrude_edge_move()
bpy.ops.mesh.extrude_face_move()

# 旋转
bpy.ops.mesh.rotate(angle=math.radians(45))

# 缩放
bpy.ops.mesh.scale(scale_factor=0.5)

# 切变
bpy.ops.mesh.shear(angle=math.radians(45))

# 弯曲
bpy.ops.mesh.bend(angle=math.radians(90))

# 推拉
bpy.ops.mesh.push_pull(value=0.5)

# 球形化
bpy.ops.mesh.spherize(factor=1.0)

# 变形
bpy.ops.mesh.deform(calc_length=True)

# 平滑
bpy.ops.mesh.vertices_smooth(factor=0.5, repeat=1)
bpy.ops.mesh.laplacian_smooth(factor=0.5, repeat=1)

def extrude_region(axis='Z', distance=1.0):
    """挤出区域"""
    if axis == 'X':
        bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value": (distance, 0, 0)})
    elif axis == 'Y':
        bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value": (0, distance, 0)})
    elif axis == 'Z':
        bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value": (0, 0, distance)})

def extrude_individual():
    """挤出个别面"""
    bpy.ops.mesh.extrude_face_move()

def scale_selected(factor):
    """缩放选中的元素"""
    bpy.ops.transform.resize(value=(factor, factor, factor))

def rotate_selected(angle, axis='Z'):
    """旋转选中的元素"""
    bpy.ops.transform.rotate(value=angle, orient_axis=axis)

def smooth_vertices(iterations=1, factor=0.5):
    """平滑顶点"""
    for _ in range(iterations):
        bpy.ops.mesh.vertices_smooth(factor=factor)

def spherize(factor=1.0):
    """球形化"""
    bpy.ops.mesh.spherize(factor=factor)

def push_pull(value=0.5):
    """推拉"""
    bpy.ops.mesh.push_pull(value=value)

def shear_mesh(angle=math.radians(45), direction=(1, 0, 0)):
    """切变网格"""
    bpy.ops.mesh.shear(angle=angle)
```

### 网格编辑操作

```python
# 细分
bpy.ops.mesh.subdivide(number_cuts=1, use_grid_fill=True)

# 切割
bpy.ops.mesh.knife_tool(confirm=True)
bpy.ops.mesh.knife_select(extend=False, deselect=False, select=True)

# 循环切割
bpy.ops.mesh.loopcut_slide(
    TRANSFORM_OT_edge_slide={"value": 0},
    MESH_OT_loopcut={"number_cuts": 1, "smoothness": 0, "falloff": 'INVERSE_SQUARE', "object_index": 0, "edge_index": 0}
)

# 滑移边
bpy.ops.mesh.edge_slide(TRANSFORM_OT_edge_slide={"value": 0})

# 滑移顶点
bpy.ops.mesh.vert_slide(TRANSFORM_OT_vert_slide={"value": 0})

# 对称
bpy.ops.mesh.symmetrize(direction='NEGATIVE_X')

# 移除三角形
bpy.ops.mesh.dissolve_degenerate(threshold=0.001)

# 移除边线
bpy.ops.mesh.dissolve_limited(angle_limit=math.radians(5))

# 有限溶解
bpy.ops.mesh.dissolve_limit(angle_limit=math.radians(5), use_dissolve_boundaries=False)

# 溶解顶点
bpy.ops.mesh.dissolve_verts(use_verts=True, use_face_split=True)

def subdivide_mesh(levels=1):
    """细分网格"""
    bpy.ops.mesh.select_all(action='SELECT')
    for _ in range(levels):
        bpy.ops.mesh.subdivide(number_cuts=1)

def cut_mesh(start, end, direction=(0, 0, 1)):
    """切割网格"""
    bpy.ops.mesh.knife_tool(confirm=True)

def add_loop_cuts(count=1):
    """添加循环切割"""
    bpy.ops.mesh.loopcut_slide(
        MESH_OT_loopcut={"number_cuts": count, "smoothness": 0}
    )

def slide_edge(value=0.5):
    """滑移边"""
    bpy.ops.mesh.edge_slide(TRANSFORM_OT_edge_slide={"value": value})

def symmetrize_mesh(direction='NEGATIVE_X'):
    """对称网格"""
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.symmetrize(direction=direction)

def dissolve_degenerate(threshold=0.001):
    """溶解退化几何"""
    bpy.ops.mesh.dissolve_degenerate(threshold=threshold)

def dissolve_limited(angle_limit=math.radians(5)):
    """有限溶解"""
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.dissolve_limited(angle_limit=angle_limit)
```

### 网格清理操作

```python
# 填充三角面
bpy.ops.mesh.fill_triangles()

# 移除双面
bpy.ops.mesh.decimate(ratio=0.5)

# 重投影
bpy.ops.mesh.project_paint(stroke=None)

# 重新映射 UV
bpy.ops.mesh.uvs_resize()

# 重新计算法线
bpy.ops.mesh.normals_make_consistent(inside=False)

def decimate_mesh(ratio=0.5):
    """精简网格"""
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.decimate(ratio=ratio)

def recalculate_normals():
    """重新计算法线"""
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.normals_make_consistent(inside=False)

def make_normals_consistent(inside=False):
    """使法线一致"""
    bpy.ops.mesh.normals_make_consistent(inside=inside)

def remove_doubles(threshold=0.0001):
    """移除重复顶点"""
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.remove_doubles(threshold=threshold)

def intersect_faces():
    """相交面"""
    bpy.ops.mesh.intersect(mode='SELECT_UNSELECT')

def intersect_faces_selected():
    """相交选中面"""
    bpy.ops.mesh.intersect(mode='SELECT')

def boolean_operation(operation='DIFFERENCE'):
    """布尔运算"""
    bpy.ops.mesh.boolean(operation=operation)
```

## 实用网格操作

### 网格创建工具

```python
import bpy
import math

def create_grid(x_subdivisions=10, y_subdivisions=10, size=2):
    """创建网格"""
    bpy.ops.mesh.primitive_grid_add(x_subdivisions=x_subdivisions, y_subdivisions=y_subdivisions, size=size)
    return bpy.context.active_object

def create_circle(vertices=32, radius=1, fill_type='NOTHING'):
    """创建圆环"""
    bpy.ops.mesh.primitive_circle_add(vertices=vertices, radius=radius, fill_type=fill_type)
    return bpy.context.active_object

def create_cone(vertices=32, radius1=1, radius2=0, depth=2):
    """创建圆锥"""
    bpy.ops.mesh.primitive_cone_add(vertices=vertices, radius1=radius1, radius2=radius2, depth=depth)
    return bpy.context.active_object

def create_torus(major_segments=48, minor_segments=12, major_radius=1, minor_radius=0.25):
    """创建圆环"""
    bpy.ops.mesh.primitive_torus_add(major_segments=major_segments, minor_segments=minor_segments, 
                                     major_radius=major_radius, minor_radius=minor_radius)
    return bpy.context.active_object

def create_ico_sphere(subdivisions=2, radius=1):
    """创建二十面球体"""
    bpy.ops.mesh.primitive_ico_sphere_add(subdivisions=subdivisions, radius=radius)
    return bpy.context.active_object

def create_suzanne():
    """创建猴子"""
    bpy.ops.mesh.primitive_monkey_add()
    return bpy.context.active_object
```

### 网格变换工具

```python
def extrude_mesh(obj, direction=(0, 0, 1), distance=1.0, steps=1):
    """挤出网格"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.mode_set(mode='EDIT')
    
    bpy.ops.mesh.select_all(action='SELECT')
    for i in range(steps):
        offset = (direction[0] * distance / steps, direction[1] * distance / steps, direction[2] * distance / steps)
        bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value": offset})
    
    bpy.ops.object.mode_set(mode='OBJECT')

def create_array(obj, count=5, offset=(1, 0, 0), axis='X'):
    """创建阵列"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    objects = [obj]
    for i in range(1, count):
        bpy.ops.object.duplicate()
        new_obj = bpy.context.active_object
        
        if axis == 'X':
            new_obj.location.x = obj.location.x + offset[0] * i
        elif axis == 'Y':
            new_obj.location.y = obj.location.y + offset[1] * i
        elif axis == 'Z':
            new_obj.location.z = obj.location.z + offset[2] * i
        
        objects.append(new_obj)
    
    return objects

def create_radial_array(obj, count=8, angle=360):
    """创建环形阵列"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    objects = [obj]
    angle_step = math.radians(angle / count)
    
    for i in range(1, count):
        bpy.ops.object.duplicate()
        new_obj = bpy.context.active_object
        new_obj.rotation_euler.z += angle_step * i
        objects.append(new_obj)
    
    return objects

def create_staircase(obj, steps=10, rise=0.2, tread=0.3):
    """创建楼梯"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.mode_set(mode='EDIT')
    
    for i in range(steps):
        offset = (tread * i, 0, rise * i)
        bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value": offset})
    
    bpy.ops.object.mode_set(mode='OBJECT')
```

### 网格修饰器应用

```python
def apply_subdivision(obj, levels=2):
    """应用细分修饰器"""
    modifier = obj.modifiers.new(name="Subdivision", type='SUBSURF')
    modifier.levels = levels
    modifier.render_levels = levels
    bpy.ops.object.modifier_apply(modifier="Subdivision")

def apply_array(obj, offset=(1, 0, 0)):
    """应用数组修饰器"""
    modifier = obj.modifiers.new(name="Array", type='ARRAY')
    modifier.count = 5
    modifier.relative_offset_displace = (offset[0], offset[1], offset[2])
    bpy.ops.object.modifier_apply(modifier="Array")

def apply_bevel(obj, width=0.1):
    """应用倒角修饰器"""
    modifier = obj.modifiers.new(name="Bevel", type='BEVEL')
    modifier.width = width
    modifier.segments = 3
    bpy.ops.object.modifier_apply(modifier="Bevel")

def apply_solidify(obj, thickness=0.1):
    """应用实体化修饰器"""
    modifier = obj.modifiers.new(name="Solidify", type='SOLIDIFY')
    modifier.thickness = thickness
    bpy.ops.object.modifier_apply(modifier="Solidify")

def apply_decimate(obj, ratio=0.5):
    """应用精简修饰器"""
    modifier = obj.modifiers.new(name="Decimate", type='DECIMATE')
    modifier.ratio = ratio
    bpy.ops.object.modifier_apply(modifier="Decimate")
```

## 网格工程面板

```python
class MeshEngineeringPanel(bpy.types.Panel):
    """网格工程面板"""
    bl_label = "网格工具"
    bl_idname = "MESH_PT_engineering_tools"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '网格'
    
    def draw(self, context):
        layout = self.layout
        obj = context.object
        
        if not obj or obj.type != 'MESH':
            layout.label(text="请选择网格对象")
            return
        
        layout.label(text="选择:")
        
        row = layout.row(align=True)
        row.operator("mesh.select_all", text="全选").action='SELECT'
        row.operator("mesh.select_all", text="反选").action='INVERT'
        
        row = layout.row()
        row.operator("mesh.select_more", text="更多")
        row.operator("mesh.select_less", text="更少")
        
        row = layout.row()
        row.operator("mesh.select_similar", text="相似").type='NORMAL'
        
        layout.separator()
        layout.label(text="编辑:")
        
        row = layout.row(align=True)
        row.operator("mesh.subdivide", text="细分").number_cuts=1
        row.operator("mesh.merge", text="合并").type='CENTER'
        
        row = layout.row()
        row.operator("mesh.delete", text="删除顶点").type='VERT'
        row.operator("mesh.delete", text="删除边").type='EDGE'
        row.operator("mesh.delete", text="删除面").type='FACE'
        
        row = layout.row()
        row.operator("mesh.bevel", text="倒角").offset=0.02
        row.operator("mesh.bevel_vertex", text="顶点倒角").offset=0.02
        
        layout.separator()
        layout.label(text="变换:")
        
        row = layout.row()
        row.operator("mesh.extrude_region_move", text="挤出")
        row.operator("mesh.inset", text="内缩")
        
        row = layout.row()
        row.operator("mesh.loopcut_slide", text="循环切割")
        row.operator("mesh.knife_tool", text="切割")
        
        layout.separator()
        layout.label(text="清理:")
        
        row = layout.row()
        row.operator("mesh.tris_convert_to_quads", text="转四边")
        row.operator("mesh.quads_convert_to_tris", text="转三角")
        
        row = layout.row()
        row.operator("mesh.dissolve_degenerate", text="清除退化")
        row.operator("mesh.remove_doubles", text="移除重复")
        
        row = layout.row()
        row.operator("mesh.normals_make_consistent", text="法线一致")
        row.operator("mesh.fill_holes", text="填充孔洞")
```

## 最佳实践

### 1. 网格优化

```python
def optimize_mesh(obj, threshold=0.0001):
    """优化网格"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.mode_set(mode='EDIT')
    
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.remove_doubles(threshold=threshold)
    
    bpy.ops.mesh.dissolve_degenerate(threshold=threshold)
    
    bpy.ops.object.mode_set(mode='OBJECT')

def triangulate_all():
    """三角化所有面"""
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.quads_convert_to_tris()

def quadrize_all():
    """四边化所有面"""
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.tris_convert_to_quads()

def clean_non_manifold():
    """清理非流形几何"""
    bpy.ops.mesh.select_non_manifold(extend=True)
    bpy.ops.mesh.delete(type='VERT')
```

### 2. 网格验证

```python
def validate_mesh(obj):
    """验证网格"""
    issues = []
    mesh = obj.data
    
    # 检查孤立顶点
    for v in mesh.vertices:
        if not v.select:
            continue
        connected = False
        for edge in mesh.edges:
            if v.index in edge.vertices:
                connected = True
                break
        if not connected:
            issues.append(f"顶点 {v.index} 是孤立的")
    
    # 检查双面
    for poly in mesh.polygons:
        if len(poly.vertices) < 3:
            issues.append(f"面 {poly.index} 顶点数不足")
    
    # 检查法线
    for poly in mesh.polygons:
        if poly.normal.length < 0.001:
            issues.append(f"面 {poly.index} 法线异常")
    
    return issues

def check_topology():
    """检查拓扑"""
    obj = bpy.context.active_object
    if not obj or obj.type != 'MESH':
        return
    
    mesh = obj.data
    
    # 计算统计数据
    vertex_count = len(mesh.vertices)
    edge_count = len(mesh.edges)
    face_count = len(mesh.polygons)
    
    print(f"顶点数: {vertex_count}")
    print(f"边数: {edge_count}")
    print(f"面数: {face_count}")
    
    # 检查拓扑是否合理
    expected_edges = vertex_count * 3 - 3
    if edge_count > expected_edges * 1.5:
        print("警告: 边数可能过多")
```

### 3. 网格备份

```python
def backup_mesh(obj):
    """备份网格数据"""
    return obj.data.copy()

def restore_mesh(obj, backup):
    """恢复网格数据"""
    obj.data = backup
```

## 常见问题

### Q: 无法选择顶点
A: 确保在编辑模式下，选择模式设置为顶点。

### Q: 细分后网格变形
A: 检查法线方向，确保网格是封闭的。

### Q: 布尔运算失败
A: 确保网格是有效的，尝试修复法线和移除重复顶点。

### Q: 网格有破面
A: 使用修复非流形几何功能，检查网格连通性。

## 相关模块

- [bpy.ops.object](bpy_ops_object_module.md) - 对象操作
- [bpy.ops.curve](bpy_ops_curve_module.md) - 曲线操作
- [bmesh.types](bmesh_types_detailed_module.md) - BMesh 类型
- [bpy.types](bpy_types_detailed_module.md) - 数据类型


---
## bpy_ops_curve

# bpy.ops.curve 模块

曲线操作模块，用于创建和编辑Bézier曲线和NURBS曲线。

## 概述

bpy.ops.curve 模块包含用于创建、编辑和操作各种类型曲线的运算符。这些运算符支持Bézier曲线、NURBS曲线和路径的创建与修改。

## 常用操作

### 曲线创建

```python
import bpy

# 添加Bézier圆
bpy.ops.primitive_bezier_circle_add(radius=1.0, location=(0, 0, 0))

# 添加Bézier曲线
bpy.ops.primitive_bezier_curve_add(radius=1.0, location=(0, 0, 0))

# 添加NURBS圆
bpy.ops.primitive_nurbs_circle_add(radius=1.0, location=(0, 0, 0))

# 添加NURBS路径
bpy.ops.primitive_nurbs_path_add(radius=1.0, location=(0, 0, 0))

# 添加NURBS曲线
bpy.ops.primitive_nurbs_curve_add(radius=1.0, location=(0, 0, 0))
```

### 曲线编辑

```python
# 细分曲线
bpy.ops.curve.subdivide(number_cuts=1)

# 倒角
bpy.ops.curve.bevel_depth_set()

# 设置倒角深度
bpy.ops.curve.bevel_depth_set()

# 设置分辨率
bpy.ops.curve.resolution_u_set()

# 清除倒角
bpy.ops.curve.bevel_depth_clear()

# 平滑曲线
bpy.ops.curve.smooth()
```

### 选择操作

```python
# 全选
bpy.ops.curve.select_all(action='SELECT')
bpy.ops.curve.select_all(action='DESELECT')
bpy.ops.curve.select_all(action='INVERT')

# 选择同类型
bpy.ops.curve.select_same_type()

# 选择连接
bpy.ops.curve.select_linked()

# 选择相似
bpy.ops.curve.select_similar(type='TYPE', compare='EQUAL')
```

### 曲线操作

```python
# 复制曲线
bpy.ops.curve.duplicate()

# 删除曲线
bpy.ops.curve.delete(type='SEGMENT')

# 删除首段
bpy.ops.curve.delete(type='VERT')

# 删除选中点
bpy.ops.curve.delete(type='EDGE')

# 分离曲线
bpy.ops.curve.separate()

# 连接曲线
bpy.ops.curve.make_regular()

# 闭合曲线
bpy.ops.curve.cyclic_toggle(direction='CYCLIC_U')

# 切换循环
bpy.ops.curve.cyclic_toggle(direction='CYCLIC_V')
```

### 点操作

```python
# 添加点
bpy.ops.curve.subdivide(number_cuts=1)

# 删除点
bpy.ops.curve.delete(type='VERT')

# 移动点
bpy.ops.transform.translate(value=(0, 0, 0))

# 旋转点
bpy.ops.transform.rotate(value=0)

# 缩放点
bpy.ops.transform.resize(value=(1, 1, 1))

# 滑动点
bpy.ops.curve.slide()
```

### 曲线类型

```python
# 设置曲线类型
bpy.ops.curve.curve_type_set(type='POLY')
bpy.ops.curve.curve_type_set(type='BEZIER')
bpy.ops.curve.curve_type_set(type='NURBS')

# 转换为多段线
bpy.ops.curve.curve_type_set(type='POLY')

# 转换为Bézier
bpy.ops.curve.curve_type_set(type='BEZIER')

# 转换为NURBS
bpy.ops.curve.curve_type_set(type='NURBS')
```

### 曲线变形

```python
# 细分
bpy.ops.curve.subdivide(number_cuts=1)

# 倒角
bpy.ops.curve.bevel_start_set()

# 结束倒角
bpy.ops.curve.bevel_end_set()

# 倒角深度
bpy.ops.curve.bevel_depth_set()

# 倒角分辨率
bpy.ops.curve.bevel_resolution_set()
```

### 曲线投射

```python
# 投射到网格
bpy.ops.curve.project()

# 投射到曲面
bpy.ops.curve.project()

# 曲线间插值
bpy.ops.curve.interpolate()
```

## 实际应用示例

### 创建程序化圆环

```python
def create_procedural_torus_curve(major_radius=2.0, minor_radius=0.5, resolution=64):
    """创建程序化圆环曲线"""
    import math
    
    # 创建Bézier圆
    bpy.ops.primitive_bezier_circle_add(radius=major_radius)
    torus = bpy.context.active_object
    torus.name = 'TorusCurve'
    
    # 获取曲线数据
    curve = torus.data
    
    # 设置分辨率
    curve.resolution_u = resolution
    
    # 添加细分
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.curve.subdivide(number_cuts=resolution * 2)
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return torus
```

### 创建程序化螺旋

```python
def create_spiral_curve(turns=5, radius=1.0, height=3.0, points=100):
    """创建螺旋曲线"""
    import math
    
    # 创建NURBS路径
    bpy.ops.primitive_nurbs_path_add(radius=1.0)
    spiral = bpy.context.active_object
    spiral.name = 'SpiralCurve'
    
    # 获取曲线数据
    curve = spiral.data
    
    # 清理默认点
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.curve.select_all(action='SELECT')
    bpy.ops.curve.delete(type='VERT')
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 添加螺旋点
    for i in range(points):
        t = i / (points - 1)
        angle = t * turns * 2 * math.pi
        current_radius = radius * (1 - t * 0.5)
        x = current_radius * math.cos(angle)
        y = current_radius * math.sin(angle)
        z = height * t - height / 2
        
        curve.points.add(1)
        curve.points[-1].co = (x, y, z, 1)
    
    return spiral
```

### 创建文字路径

```python
def create_text_path(text, font_obj=None, resolution=12):
    """将文字转换为曲线"""
    # 创建文字对象
    bpy.ops.object.text_add()
    text_obj = bpy.context.active_object
    text_obj.data.body = text
    
    # 设置分辨率
    text_obj.data.resolution_u = resolution
    
    # 转换为曲线
    bpy.ops.object.convert(target='CURVE')
    
    return text_obj
```

### 创建程序化形状键曲线

```python
def create_curve_shape_keys(curve_obj, key_names, key_data):
    """为曲线创建形状键"""
    # 切换到对象模式
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 添加形状键基础
    basis_key = curve_obj.shape_key_add(name='Basis')
    
    # 创建每个形状键
    for key_name, data in zip(key_names, key_data):
        key = curve_obj.shape_key_add(name=key_name)
        
        # 设置关键帧
        for i, point in enumerate(data):
            key.data[i].co = point
    
    return curve_obj.shape_key_blocks
```

### 创建路径动画

```python
def create_path_animation(obj, curve_obj, frames=100):
    """创建沿路径动画"""
    # 添加跟随路径约束
    constraint = obj.constraints.new(type='FOLLOW_PATH')
    constraint.target = curve_obj
    
    # 设置曲线
    constraint.use_curve_follow = True
    constraint.up_axis = 'UP_Y'
    constraint.forward_axis = 'FORWARD_X'
    
    # 约束路径
    bpy.ops.constraint.followpath_path_animate(
        constraint='Follow Path',
        owner='OBJECT',
        frame_start=1,
        length=frames
    )
    
    return obj
```

### 创建程序化齿轮曲线

```python
def create_gear_curve(teeth=12, outer_radius=1.0, inner_radius=0.8, resolution=24):
    """创建齿轮形状曲线"""
    import math
    
    # 创建Bézier曲线
    bpy.ops.primitive_bezier_curve_add(radius=1.0)
    gear = bpy.context.active_object
    gear.name = 'GearCurve'
    
    curve = gear.data
    
    # 计算齿轮点
    points = []
    for i in range(teeth * 2):
        angle = (i / (teeth * 2)) * 2 * math.pi
        is_outer = i % 2 == 0
        
        radius = outer_radius if is_outer else inner_radius
        x = radius * math.cos(angle)
        y = radius * math.sin(angle)
        points.append((x, y, 0))
    
    # 设置点
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.curve.select_all(action='SELECT')
    bpy.ops.curve.delete(type='VERT')
    
    for point in points:
        curve.points.add(1)
        curve.points[-1].co = (*point, 1)
    
    # 闭合曲线
    bpy.ops.curve.cyclic_toggle(direction='CYCLIC_U')
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return gear
```

### 创建程序化花形曲线

```python
def create_flower_curve(petals=5, radius=1.0, resolution=100):
    """创建花瓣形状曲线"""
    import math
    
    # 创建Bézier曲线
    bpy.ops.primitive_bezier_curve_add(radius=1.0)
    flower = bpy.context.active_object
    flower.name = 'FlowerCurve'
    
    curve = flower.data
    
    # 清理默认点
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.curve.select_all(action='SELECT')
    bpy.ops.curve.delete(type='VERT')
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 计算花瓣点
    for i in range(resolution):
        t = i / (resolution - 1)
        angle = t * 2 * math.pi
        
        # 花瓣公式
        r = radius * (0.5 + 0.5 * math.sin(petals * angle))
        
        x = r * math.cos(angle)
        y = r * math.sin(angle)
        
        curve.points.add(1)
        curve.points[-1].co = (x, y, 1, 1)
    
    # 闭合曲线
    bpy.ops.curve.cyclic_toggle(direction='CYCLIC_U')
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return flower
```

### 创建程序化心形曲线

```python
def create_heart_curve(size=1.0, resolution=100):
    """创建心形曲线"""
    import math
    
    # 创建Bézier曲线
    bpy.ops.primitive_bezier_curve_add(radius=1.0)
    heart = bpy.context.active_object
    heart.name = 'HeartCurve'
    
    curve = heart.data
    
    # 清理默认点
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.curve.select_all(action='SELECT')
    bpy.ops.curve.delete(type='VERT')
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 计算心形点
    for i in range(resolution):
        t = i / (resolution - 1) * 2 * math.pi
        
        # 心形公式
        x = 16 * math.pow(math.sin(t), 3)
        y = -(13 * math.cos(t) - 5 * math.cos(2*t) - 2 * math.cos(3*t) - math.cos(4*t))
        
        x = x * size / 16
        y = y * size / 16
        
        curve.points.add(1)
        curve.points[-1].co = (x, y, 0, 1)
    
    # 闭合曲线
    bpy.ops.curve.cyclic_toggle(direction='CYCLIC_U')
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return heart
```

## 使用场景

- **路径动画**：创建沿路径的运动
- **程序化形状**：创建数学定义的曲线
- **文字转换**：将文字转为曲线
- **模型生成**：从曲线生成管状模型
- **装饰设计**：创建装饰性曲线
- **机械设计**：创建齿轮等机械形状

## 注意事项

- 曲线需要足够的点来表达形状
- NURBS曲线需要控制点
- Bézier曲线有句柄
- 曲线可以转换为网格
- 路径动画需要正确设置
- 曲线分辨率影响平滑度


---
## bpy_ops_surface

# bpy.ops.surface - Surface Operations Module

Blender 曲面操作符，用于创建和编辑 NURBS 曲面和曲面对象。

## Overview

`bpy.ops.surface` 模块包含曲面对象的操作命令，支持 NURBS 曲面的创建、编辑和参数调整。这个模块主要用于高级建模中的曲面创建。

## 核心功能

```python
import bpy

# 添加 NURBS 曲面
bpy.ops.surface.primitive_nurbs_surface_surface_add(location=(0, 0, 0))

# 添加 NURBS 圆柱
bpy.ops.surface.primitive_nurbs_surface_cylinder_add(location=(0, 0, 0))

# 添加 NURBS 球体
bpy.ops.surface.primitive_nurbs_surface_sphere_add(location=(0, 0, 0))

# 添加 NURBS 圆环
bpy.ops.surface.primitive_nurbs_surface_torus_add(location=(0, 0, 0))
```

## 实际应用示例

### 曲面管理器

```python
import bpy
import math

class SurfaceManager:
    """曲面管理器"""
    
    def __init__(self):
        self.surfaces = {}
    
    def create_nurbs_surface(self, name, location=(0, 0, 0),
                             u_size=1, v_size=1,
                             u_resolution=4, v_resolution=4):
        """创建 NURBS 曲面"""
        bpy.ops.surface.primitive_nurbs_surface_surface_add(
            location=location
        )
        surface = bpy.context.active_object
        surface.name = name
        
        # 设置分辨率
        surface.data.u_resolution = u_resolution
        surface.data.v_resolution = v_resolution
        
        self.surfaces[name] = surface
        return surface
    
    def create_nurbs_cylinder(self, name, location=(0, 0, 0),
                              radius=1, depth=2,
                              resolution_u=8, resolution_v=4):
        """创建 NURBS 圆柱"""
        bpy.ops.surface.primitive_nurbs_surface_cylinder_add(
            location=location
        )
        cylinder = bpy.context.active_object
        cylinder.name = name
        
        # 设置参数
        cylinder.data.radius = radius
        cylinder.data.depth = depth
        cylinder.data.resolution_u = resolution_u
        cylinder.data.resolution_v = resolution_v
        
        self.surfaces[name] = cylinder
        return cylinder
    
    def create_nurbs_sphere(self, name, location=(0, 0, 0),
                           radius=1, resolution_u=16, resolution_v=12):
        """创建 NURBS 球体"""
        bpy.ops.surface.primitive_nurbs_surface_sphere_add(
            location=location
        )
        sphere = bpy.context.active_object
        sphere.name = name
        
        # 设置参数
        sphere.data.radius = radius
        sphere.data.resolution_u = resolution_u
        sphere.data.resolution_v = resolution_v
        
        self.surfaces[name] = sphere
        return sphere
    
    def create_nurbs_torus(self, name, location=(0, 0, 0),
                          major_radius=2, minor_radius=0.5,
                          resolution_u=16, resolution_v=8):
        """创建 NURBS 圆环"""
        bpy.ops.surface.primitive_nurbs_surface_torus_add(
            location=location
        )
        torus = bpy.context.active_object
        torus.name = name
        
        # 设置参数
        torus.data.major_radius = major_radius
        torus.data.minor_radius = minor_radius
        torus.data.resolution_u = resolution_u
        torus.data.resolution_v = resolution_v
        
        self.surfaces[name] = torus
        return torus
    
    def add_surface_primitive(self, surface_type, name, **kwargs):
        """添加曲面图元"""
        if surface_type == 'surface':
            return self.create_nurbs_surface(name, **kwargs)
        elif surface_type == 'cylinder':
            return self.create_nurbs_cylinder(name, **kwargs)
        elif surface_type == 'sphere':
            return self.create_nurbs_sphere(name, **kwargs)
        elif surface_type == 'torus':
            return self.create_nurbs_torus(name, **kwargs)
    
    def convert_to_mesh(self, surface_name):
        """转换为网格"""
        surface = self.surfaces.get(surface_name)
        if surface:
            bpy.context.view_layer.objects.active = surface
            bpy.ops.object.convert(target='MESH')
    
    def convert_to_curve(self, surface_name):
        """转换为曲线"""
        surface = self.surfaces.get(surface_name)
        if surface:
            bpy.context.view_layer.objects.active = surface
            bpy.ops.object.convert(target='CURVE')
    
    def set_resolution(self, surface_name, u_res, v_res):
        """设置分辨率"""
        surface = self.surfaces.get(surface_name)
        if surface:
            surface.data.u_resolution = u_res
            surface.data.v_resolution = v_res
    
    def set_u_knots(self, surface_name, knots):
        """设置 U 方向节点"""
        surface = self.surfaces.get(surface_name)
        if surface:
            surface.data.u_knots = knots
    
    def set_v_knots(self, surface_name, knots):
        """设置 V 方向节点"""
        surface = self.surfaces.get(surface_name)
        if surface:
            surface.data.v_knots = knots
    
    def get_surface_info(self, surface_name):
        """获取曲面信息"""
        surface = self.surfaces.get(surface_name)
        if not surface:
            return None
        
        data = surface.data
        return {
            'name': surface.name,
            'type': data.type,
            'u_resolution': data.u_resolution,
            'v_resolution': data.v_resolution,
            'u_knots': len(data.u_knots),
            'v_knots': len(data.v_knots)
        }
    
    def duplicate_surface(self, surface_name, new_name):
        """复制曲面"""
        surface = self.surfaces.get(surface_name)
        if surface:
            bpy.context.view_layer.objects.active = surface
            bpy.ops.object.duplicate()
            duplicate = bpy.context.active_object
            duplicate.name = new_name
            self.surfaces[new_name] = duplicate
            return duplicate
    
    def delete_surface(self, surface_name):
        """删除曲面"""
        surface = self.surfaces.get(surface_name)
        if surface:
            bpy.data.objects.remove(surface, do_unlink=True)
            del self.surfaces[surface_name]


def setup_surface_workflow():
    """设置曲面工作流程"""
    manager = SurfaceManager()
    
    print("曲面管理器已设置")
    print("可用功能:")
    print("- create_nurbs_surface: 创建 NURBS 曲面")
    print("- create_nurbs_cylinder: 创建 NURBS 圆柱")
    print("- convert_to_mesh: 转换为网格")
    
    return manager
```

### 曲面建模工具

```python
import bpy
import math

class SurfaceModelingTool:
    """曲面建模工具"""
    
    def __init__(self):
        self.control_points = {}
    
    def create_surface_from_points(self, points, u_segments, v_segments,
                                   name="Surface"):
        """从点创建曲面"""
        # 创建 NURBS 曲面
        surface = bpy.ops.surface.primitive_nurbs_surface_surface_add()
        surface = bpy.context.active_object
        surface.name = name
        
        # 设置控制点
        data = surface.data
        
        # 调整分辨率以匹配点网格
        data.u_resolution = u_segments + 1
        data.v_resolution = v_segments + 1
        
        # 设置点位置
        for i in range(u_segments + 1):
            for j in range(v_segments + 1):
                if i * (v_segments + 1) + j < len(points):
                    point = points[i * (v_segments + 1) + j]
                    # 设置控制点位置
                    pass
        
        return surface
    
    def create_ruled_surface(self, curve1, curve2, name="RuledSurface"):
        """创建直纹面"""
        # 创建两个 NURBS 曲线
        # 然后创建直纹面
        pass
    
    def create_revolved_surface(self, curve, axis=(0, 0, 1), angle=360,
                                name="RevolvedSurface"):
        """创建旋转面"""
        # 旋转曲线创建曲面
        pass
    
    def create_lofted_surface(self, curves, name="LoftedSurface"):
        """创建放样面"""
        # 通过多个曲线放样创建曲面
        pass
    
    def create_bezier_patch(self, name, resolution=4, size=2):
        """创建贝塞尔曲面"""
        surface = self.create_nurbs_surface(
            name,
            u_resolution=resolution,
            v_resolution=resolution
        )
        
        # 设置为贝塞尔类型
        surface.data.nurbs_type = 'BEZIER'
        surface.data.u_order = 4
        surface.data.v_order = 4
        
        return surface
    
    def add_control_point(self, surface_name, u, v, location):
        """添加控制点"""
        surface = self.surfaces.get(surface_name)
        if not surface:
            return
        
        # 获取控制点层
        pass
    
    def move_control_point(self, surface_name, u, v, offset):
        """移动控制点"""
        surface = self.surfaces.get(surface_name)
        if not surface:
            return
        
        # 移动控制点
        pass
    
    def insert_knot_u(self, surface_name, u_param):
        """插入 U 方向节点"""
        surface = self.surfaces.get(surface_name)
        if surface:
            surface.data.u_knots.append(u_param)
    
    def insert_knot_v(self, surface_name, v_param):
        """插入 V 方向节点"""
        surface = self.surfaces.get(surface_name)
        if surface:
            surface.data.v_knots.append(v_param)
    
    def increase_resolution(self, surface_name, u_increase=1, v_increase=1):
        """增加分辨率"""
        surface = self.surfaces.get(surface_name)
        if surface:
            surface.data.u_resolution += u_increase
            surface.data.v_resolution += v_increase
    
    def decrease_resolution(self, surface_name, u_decrease=1, v_decrease=1):
        """降低分辨率"""
        surface = self.surfaces.get(surface_name)
        if surface:
            surface.data.u_resolution = max(1, surface.data.u_resolution - u_decrease)
            surface.data.v_resolution = max(1, surface.data.v_resolution - v_decrease)
    
    def match_surface_to_mesh(self, surface_name, mesh_name):
        """匹配曲面到网格"""
        surface = self.surfaces.get(surface_name)
        mesh = bpy.data.objects.get(mesh_name)
        
        if not surface or not mesh:
            return
        
        # 设置曲面边界框匹配网格
        pass
    
    def create_patch_from_mesh_face(self, mesh_name, face_index,
                                   resolution=4):
        """从网格面创建曲面"""
        mesh = bpy.data.objects.get(mesh_name)
        if not mesh:
            return
        
        # 获取面顶点
        pass
    
    def export_surface_to_obj(self, surface_name, filepath):
        """导出曲面到 OBJ"""
        surface = self.surfaces.get(surface_name)
        if surface:
            # 转换为网格后导出
            self.convert_to_mesh(surface_name)
            
            bpy.ops.wm.obj_export(
                filepath=filepath,
                export_selected=True
            )


def setup_surface_modeling():
    """设置曲面建模"""
    tool = SurfaceModelingTool()
    
    print("曲面建模工具已设置")
    print("可用功能:")
    print("- create_surface_from_points: 从点创建")
    print("- create_bezier_patch: 创建贝塞尔曲面")
    print("- increase_resolution: 增加分辨率")
    
    return tool
```

### NURBS 曲线工具

```python
import bpy
import math

class NurbsCurveTool:
    """NURBS 曲线工具"""
    
    def __init__(self):
        self.curves = {}
    
    def create_nurbs_path(self, name, points, resolution=4):
        """创建 NURBS 路径"""
        bpy.ops.curve.primitive_nurbs_path_add()
        curve = bpy.context.active_object
        curve.name = name
        
        # 设置点
        data = curve.data
        data.resolution_u = resolution
        
        self.curves[name] = curve
        return curve
    
    def create_nurbs_circle(self, name, radius=1, resolution=16):
        """创建 NURBS 圆"""
        bpy.ops.curve.primitive_nurbs_circle_add(radius=radius)
        curve = bpy.context.active_object
        curve.name = name
        curve.data.resolution_u = resolution
        
        self.curves[name] = curve
        return curve
    
    def set_curve_resolution(self, curve_name, resolution):
        """设置曲线分辨率"""
        curve = self.curves.get(curve_name)
        if curve:
            curve.data.resolution_u = resolution
    
    def add_curve_point(self, curve_name, location, index=-1):
        """添加曲线点"""
        curve = self.curves.get(curve_name)
        if curve:
            bpy.ops.curve.duplicate()
    
    def remove_curve_point(self, curve_name, index):
        """移除曲线点"""
        curve = self.curves.get(curve_name)
        if curve and index < len(curve.data.splines[0].points):
            curve.data.splines[0].points.remove(
                curve.data.splines[0].points[index]
            )
    
    def set_curve_order(self, curve_name, order):
        """设置曲线阶数"""
        curve = self.curves.get(curve_name)
        if curve:
            curve.data.nurbs_type = 'POLY' if order == 1 else 'NURBS'
    
    def close_curve(self, curve_name):
        """闭合曲线"""
        curve = self.curves.get(curve_name)
        if curve:
            curve.data.splines[0].use_cyclic_u = True
    
    def open_curve(self, curve_name):
        """打开曲线"""
        curve = self.curves.get(curve_name)
        if curve:
            curve.data.splines[0].use_cyclic_u = False
    
    def create_curve_from_mesh(self, mesh_name, edge_indices,
                               name="Curve"):
        """从网格边创建曲线"""
        mesh = bpy.data.objects.get(mesh_name)
        if not mesh:
            return None
        
        # 选择边
        bpy.context.view_layer.objects.active = mesh
        bpy.ops.object.mode_set(mode='EDIT')
        bpy.ops.mesh.select_all(action='DESELECT')
        
        # 创建曲线
        pass
    
    def convert_to_nurbs(self, curve_name):
        """转换为 NURBS"""
        curve = self.curves.get(curve_name)
        if curve:
            curve.data.nurbs_type = 'NURBS'
    
    def convert_to_poly(self, curve_name):
        """转换为多边形"""
        curve = self.curves.get(curve_name)
        if curve:
            curve.data.nurbs_type = 'POLY'
    
    def get_curve_length(self, curve_name):
        """获取曲线长度"""
        curve = self.curves.get(curve_name)
        if curve:
            return curve.data.splines[0].calc_length()
        return 0
    
    def sample_curve(self, curve_name, num_samples=10):
        """采样曲线"""
        curve = self.curves.get(curve_name)
        if not curve:
            return []
        
        points = []
        for i in range(num_samples):
            t = i / (num_samples - 1)
            point = curve.data.splines[0].point_at_index(t)
            points.append(point)
        
        return points
    
    def create_offset_curve(self, curve_name, offset, name="Offset"):
        """创建偏移曲线"""
        curve = self.curves.get(curve_name)
        if not curve:
            return None
        
        # 创建偏移曲线
        pass
    
    def export_curve_to_svg(self, curve_name, filepath):
        """导出曲线到 SVG"""
        curve = self.curves.get(curve_name)
        if not curve:
            return
        
        # 生成 SVG
        svg_content = '<svg xmlns="http://www.w3.org/2000/svg">'
        svg_content += '<path d="'
        
        # 添加路径数据
        pass
        
        svg_content += '"/>'
        svg_content += '</svg>'
        
        with open(filepath, 'w') as f:
            f.write(svg_content)


def setup_nurbs_workflow():
    """设置 NURBS 工作流程"""
    tool = NurbsCurveTool()
    
    print("NURBS 曲线工具已设置")
    print("可用功能:")
    print("- create_nurbs_path: 创建路径")
    print("- create_nurbs_circle: 创建圆")
    print("- sample_curve: 采样曲线")
    
    return tool
```


---
## bpy_ops_lattice

# bpy.ops.lattice - Lattice Operations Module

Blender 晶格操作符，用于创建和编辑晶格变形对象。

## Overview

`bpy.ops.lattice` 模块包含晶格（Lattice）对象的操作命令，支持晶格的创建、编辑和变形应用。这个模块主要用于网格变形和动画控制。

## 核心功能

```python
import bpy

# 创建晶格
bpy.ops.object.lattice_add(location=(0, 0, 0))

# 晶格设置
bpy.ops.lattice.select_all(action='SELECT')
bpy.ops.lattice.select_more()
bpy.ops.lattice.select_less()
bpy.ops.lattice.select_ungrouped()

# 变形操作
bpy.ops.lattice.make_regular(type='BONE')
bpy.ops.lattice.flip(axis='X')
```

## 实际应用示例

### 晶格管理器

```python
import bpy
import math

class LatticeManager:
    """晶格管理器"""
    
    def __init__(self):
        self.lattices = {}
    
    def create_lattice(self, name, location=(0, 0, 0), 
                       dimensions=(2, 2, 2),
                       size=2.0):
        """创建晶格"""
        bpy.ops.object.lattice_add(location=location)
        lattice = bpy.context.active_object
        lattice.name = name
        
        # 设置晶格参数
        lattice.data.points_u = dimensions[0]
        lattice.data.points_v = dimensions[1]
        lattice.data.points_w = dimensions[2]
        lattice.data.size = size
        
        self.lattices[name] = lattice
        return lattice
    
    def create_adaptive_lattice(self, target_obj, resolution=2, size=1.0):
        """创建自适应晶格"""
        # 基于目标对象大小创建晶格
        bbox = target_obj.bound_box
        min_x = min(v[0] for v in bbox)
        max_x = max(v[0] for v in bbox)
        min_y = min(v[1] for v in bbox)
        max_y = max(v[1] for v in bbox)
        min_z = min(v[2] for v in bbox)
        max_z = max(v[2] for v in bbox)
        
        center = target_obj.location
        
        lattice = self.create_lattice(
            f"{target_obj.name}_Lattice",
            location=center,
            dimensions=(resolution, resolution, resolution),
            size=size * max(
                max_x - min_x,
                max_y - min_y,
                max_z - min_z
            )
        )
        
        return lattice
    
    def apply_lattice_to_object(self, lattice_name, target_name):
        """应用晶格到对象"""
        lattice = bpy.data.objects.get(lattice_name)
        target = bpy.data.objects.get(target_name)
        
        if not lattice or not target:
            return
        
        # 添加晶格修改器
        mod = target.modifiers.new(name="Lattice", type='LATTICE')
        mod.object = lattice
        
        return mod
    
    def create_deformation_lattice(self, target_obj, resolution=3, 
                                   influence=1.0, location_offset=(0, 0, 0)):
        """创建变形晶格"""
        lattice = self.create_adaptive_lattice(target_obj, resolution)
        lattice.location = (
            target_obj.location.x + location_offset[0],
            target_obj.location.y + location_offset[1],
            target_obj.location.z + location_offset[2]
        )
        
        # 应用晶格
        mod = self.apply_lattice_to_object(lattice.name, target_obj.name)
        mod.strength = influence
        
        return lattice, mod
    
    def set_lattice_resolution(self, lattice_name, u, v, w):
        """设置晶格分辨率"""
        lattice = bpy.data.objects.get(lattice_name)
        if lattice:
            lattice.data.points_u = u
            lattice.data.points_v = v
            lattice.data.points_w = w
    
    def get_lattice_info(self, lattice_name):
        """获取晶格信息"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return None
        
        return {
            'name': lattice.name,
            'resolution': (
                lattice.data.points_u,
                lattice.data.points_v,
                lattice.data.points_w
            ),
            'size': lattice.data.size,
            'point_count': (
                lattice.data.points_u * 
                lattice.data.points_v * 
                lattice.data.points_w
            )
        }
    
    def export_lattice_points(self, lattice_name):
        """导出晶格点"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return []
        
        points = []
        for point in lattice.data.points:
            points.append(point.co_deform.copy())
        
        return points
    
    def import_lattice_points(self, lattice_name, points):
        """导入晶格点"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        for i, point in enumerate(points):
            if i < len(lattice.data.points):
                lattice.data.points[i].co_deform = point


def setup_lattice_workflow():
    """设置晶格工作流程"""
    manager = LatticeManager()
    
    print("晶格管理器已设置")
    print("可用功能:")
    print("- create_lattice: 创建晶格")
    print("- apply_lattice_to_object: 应用晶格")
    print("- set_lattice_resolution: 设置分辨率")
    
    return manager
```

### 晶格动画系统

```python
import bpy
import math

class LatticeAnimationSystem:
    """晶格动画系统"""
    
    def __init__(self):
        self.animated_lattices = {}
    
    def create_squash_stretch_animation(self, lattice_name, target_name,
                                        start_frame=1, end_frame=30,
                                        squash_amount=0.5, stretch_amount=1.5):
        """创建挤压拉伸动画"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return None
        
        # 挤压
        bpy.context.scene.frame_set(start_frame)
        lattice.scale = (1, 1, 1)
        lattice.keyframe_insert(data_path='scale', frame=start_frame)
        
        bpy.context.scene.frame_set(start_frame + (end_frame - start_frame) // 3)
        lattice.scale = (stretch_amount, squash_amount, stretch_amount)
        lattice.keyframe_insert(data_path='scale', frame=start_frame + (end_frame - start_frame) // 3)
        
        # 拉伸
        bpy.context.scene.frame_set(start_frame + 2 * (end_frame - start_frame) // 3)
        lattice.scale = (squash_amount, stretch_amount, squash_amount)
        lattice.keyframe_insert(data_path='scale', frame=start_frame + 2 * (end_frame - start_frame) // 3)
        
        # 恢复
        bpy.context.scene.frame_set(end_frame)
        lattice.scale = (1, 1, 1)
        lattice.keyframe_insert(data_path='scale', frame=end_frame)
        
        return lattice
    
    def create_wave_deformation(self, lattice_name, start_frame=1,
                                amplitude=0.5, frequency=2.0):
        """创建波浪变形动画"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return None
        
        for frame in range(start_frame, start_frame + 60):
            bpy.context.scene.frame_set(frame)
            
            time = (frame - start_frame) / 30.0
            
            for i, point in enumerate(lattice.data.points):
                z = point.co_deform.z
                offset = i % lattice.data.points_u
                wave = amplitude * math.sin(2 * math.pi * frequency * time + offset)
                
                new_z = z + wave
                point.co_deform.z = new_z
            
            lattice.data.update()
            
            if frame == start_frame:
                point.co_deform.keyframe_insert(data_path='co_deform', index=2)
        
        return lattice
    
    def create_morph_targets(self, lattice_name, target_shapes, start_frame=1):
        """创建变形目标"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return {}
        
        shapes = {}
        
        for shape_name, shape_points in target_shapes.items():
            # 保存原始形状
            original_points = [(p.co_deform.copy(), p.co_latt) 
                             for p in lattice.data.points]
            
            # 应用新形状
            for i, point in enumerate(lattice.data.points):
                if i < len(shape_points):
                    point.co_deform = shape_points[i]
            
            # 插入关键帧
            bpy.context.scene.frame_set(start_frame)
            for point in lattice.data.points:
                point.co_deform.keyframe_insert(data_path='co_deform', index=-1)
            
            # 恢复原始形状
            for i, (orig_co, orig_cl) in enumerate(original_points):
                lattice.data.points[i].co_deform = orig_co
                lattice.data.points[i].co_latt = orig_cl
            
            shapes[shape_name] = lattice
        
        return shapes
    
    def create_puppet_deformation(self, target_name, control_bones,
                                  lattice_resolution=3):
        """创建木偶变形"""
        # 创建晶格
        target = bpy.data.objects.get(target_name)
        if not target:
            return None
        
        manager = LatticeManager()
        lattice = manager.create_adaptive_lattice(target, lattice_resolution)
        manager.apply_lattice_to_object(lattice.name, target_name)
        
        return lattice
    
    def keyframe_point_movement(self, lattice_name, point_index,
                                keyframes):
        """关键帧点移动"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice or point_index >= len(lattice.data.points):
            return
        
        point = lattice.data.points[point_index]
        
        for frame, position in keyframes.items():
            bpy.context.scene.frame_set(frame)
            point.co_deform = position
            point.co_deform.keyframe_insert(data_path='co_deform', index=-1)
    
    def create_point_constraint(self, lattice_name, point_index, target):
        """创建点约束"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice or point_index >= len(lattice.data.points):
            return None
        
        # 创建空对象作为控制器
        bpy.ops.object.empty_add(type='ARROWS', location=target.location)
        controller = bpy.context.active_object
        controller.name = f"{lattice.name}_Point{point_index}_Controller"
        
        return controller


def setup_lattice_animation():
    """设置晶格动画"""
    system = LatticeAnimationSystem()
    
    print("晶格动画系统已设置")
    print("可用功能:")
    print("- create_squash_stretch_animation: 挤压拉伸")
    print("- create_wave_deformation: 波浪变形")
    print("- create_morph_targets: 变形目标")
    
    return system
```

### 晶格变形工具

```python
import bpy
import math

class LatticeDeformationTool:
    """晶格变形工具"""
    
    def __init__(self):
        self.deformation_types = ['affine', 'bilinear', 'barycentric']
    
    def apply_bend_deformation(self, lattice_name, angle, axis='z',
                               center=(0, 0, 0)):
        """应用弯曲变形"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        angle_rad = math.radians(angle)
        
        for point in lattice.data.points:
            x, y, z = point.co_deform
            
            if axis == 'z':
                # Z轴弯曲
                radius = (z - center[2]) if z != center[2] else 1.0
                new_x = radius * math.sin(angle_rad * (x / radius)) + center[0]
                new_y = radius * (1 - math.cos(angle_rad * (x / radius))) + center[1]
                point.co_deform = (new_x, new_y, z)
            
            elif axis == 'y':
                # Y轴弯曲
                radius = (y - center[1]) if y != center[1] else 1.0
                new_x = radius * math.sin(angle_rad * (x / radius)) + center[0]
                new_z = radius * (1 - math.cos(angle_rad * (x / radius))) + center[2]
                point.co_deform = (new_x, y, new_z)
            
            elif axis == 'x':
                # X轴弯曲
                radius = (x - center[0]) if x != center[0] else 1.0
                new_y = radius * math.sin(angle_rad * (y / radius)) + center[1]
                new_z = radius * (1 - math.cos(angle_rad * (y / radius))) + center[2]
                point.co_deform = (x, new_y, new_z)
        
        lattice.data.update()
    
    def apply_twist_deformation(self, lattice_name, angle, axis='z'):
        """应用扭曲变形"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        angle_rad = math.radians(angle)
        
        for point in lattice.data.points:
            x, y, z = point.co_deform
            
            if axis == 'z':
                factor = z
            elif axis == 'y':
                factor = y
            else:
                factor = x
            
            rotation = angle_rad * factor
            
            new_x = x * math.cos(rotation) - y * math.sin(rotation)
            new_y = x * math.sin(rotation) + y * math.cos(rotation)
            
            point.co_deform = (new_x, new_y, z)
        
        lattice.data.update()
    
    def apply_taper_deformation(self, lattice_name, amount, axis='z',
                                factor=1.0):
        """应用锥化变形"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        for point in lattice.data.points:
            x, y, z = point.co_deform
            
            if axis == 'z':
                factor_z = z * amount
                point.co_deform = (x * (1 + factor_z * factor),
                                 y * (1 + factor_z * factor), z)
            elif axis == 'y':
                factor_y = y * amount
                point.co_deform = (x * (1 + factor_y * factor), y,
                                 z * (1 + factor_y * factor))
            else:
                factor_x = x * amount
                point.co_deform = (x, y * (1 + factor_x * factor),
                                 z * (1 + factor_x * factor))
        
        lattice.data.update()
    
    def apply_shear_deformation(self, lattice_name, amount, axis='z',
                                direction='x'):
        """应用剪切变形"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        for point in lattice.data.points:
            x, y, z = point.co_deform
            
            if axis == 'z':
                if direction == 'x':
                    point.co_deform = (x + amount * z, y, z)
                else:
                    point.co_deform = (x, y + amount * z, z)
            elif axis == 'y':
                if direction == 'x':
                    point.co_deform = (x + amount * y, y, z)
                else:
                    point.co_deform = (x, y, z + amount * y)
            else:
                if direction == 'y':
                    point.co_deform = (x, y + amount * x, z)
                else:
                    point.co_deform = (x, y, z + amount * x)
        
        lattice.data.update()
    
    def apply_jitter_deformation(self, lattice_name, amount=0.1):
        """应用抖动变形"""
        import random
        
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        for point in lattice.data.points:
            point.co_deform = (
                point.co_deform[0] + random.uniform(-amount, amount),
                point.co_deform[1] + random.uniform(-amount, amount),
                point.co_deform[2] + random.uniform(-amount, amount)
            )
        
        lattice.data.update()
    
    def apply_noise_deformation(self, lattice_name, scale=1.0, amplitude=0.1):
        """应用噪声变形"""
        import random
        
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        for i, point in enumerate(lattice.data.points):
            noise = random.random() * 2 - 1
            point.co_deform = (
                point.co_deform[0] + noise * amplitude,
                point.co_deform[1] + noise * amplitude,
                point.co_deform[2] + noise * amplitude
            )
        
        lattice.data.update()
    
    def create_symmetrical_lattice(self, lattice_name, axis='x'):
        """创建对称晶格"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        # 获取点数据
        points = [(p.co_deform.copy(), p.co_latt) for p in lattice.data.points]
        
        # 镜像点
        for i, (orig_co, orig_cl) in enumerate(points):
            if axis == 'x':
                mirrored = (-orig_co[0], orig_co[1], orig_co[2])
            elif axis == 'y':
                mirrored = (orig_co[0], -orig_co[1], orig_co[2])
            else:
                mirrored = (orig_co[0], orig_co[1], -orig_co[2])
            
            # 找到对称点并应用
            if i + len(points) // 2 < len(lattice.data.points):
                lattice.data.points[i + len(points) // 2].co_deform = mirrored
        
        lattice.data.update()
    
    def reset_lattice_shape(self, lattice_name):
        """重置晶格形状"""
        lattice = bpy.data.objects.get(lattice_name)
        if not lattice:
            return
        
        for point in lattice.data.points:
            point.co_deform = point.co_latt.copy()
        
        lattice.data.update()


def setup_deformation_tool():
    """设置变形工具"""
    tool = LatticeDeformationTool()
    
    print("晶格变形工具已设置")
    print("可用变形:")
    print("- bend: 弯曲")
    print("- twist: 扭曲")
    print("- taper: 锥化")
    print("- shear: 剪切")
    print("- jitter: 抖动")
    print("- noise: 噪声")
    
    return tool
```


---
## bpy_ops_mball

# bpy.ops.mball 模块

元球操作模块，用于创建和管理元球。

## 概述

bpy.ops.mball 模块包含用于创建、编辑和操作元球的运算符。这些元球是程序化定义的曲面，可用于创建有机形状和流体效果。

## 常用操作

### 元球创建

```python
import bpy

# 添加球体元球
bpy.ops.mesh.primitive_uv_sphere_add(radius=1.0, location=(0, 0, 0))

# 添加胶囊元球
bpy.ops.mesh.primitive_capule_add(radius=0.5, depth=1.0)

# 转换为元球
bpy.ops.object.convert(target='META')

# 添加元球数据
bpy.ops.object.metaball_add(type='BALL', radius=1.0)

# 添加平面元球
bpy.ops.object.metaball_add(type='PLANE', radius=1.0)

# 添加椭圆体元球
bpy.ops.object.metaball_add(type='ELLIPSOID', radius=1.0)

# 添加立方体元球
bpy.ops.object.metaball_add(type='CUBE', radius=1.0)
```

### 元球类型

```python
# 添加球体
bpy.ops.object.metaball_add(type='BALL')

# 添加平面
bpy.ops.object.metaball_add(type='PLANE')

# 添加椭圆体
bpy.ops.object.metaball_add(type='ELLIPSOID')

# 添加立方体
bpy.ops.object.metaball_add(type='CUBE')

# 添加圆柱体
bpy.ops.object.metaball_add(type='CYLINDER')
```

### 元球编辑

```python
# 复制元球
bpy.ops.object.duplicate()

# 删除元球
bpy.ops.object.delete()

# 重命名元球
bpy.ops.object.rename(name='Metaball')

# 设置分辨率
bpy.ops.object.metaball_add(type='BALL', resolution=0.5)

# 更新元球
bpy.ops.object.update()
```

### 选择操作

```python
# 全选
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.select_all(action='DESELECT')
bpy.ops.object.select_all(action='INVERT')

# 选择同类型
bpy.ops.object.select_same_type()
```

### 元球设置

```python
# 设置阈值
bpy.ops.object.metaball_add(type='BALL', threshold=1.0)

# 设置半径
bpy.ops.object.metaball_add(type='BALL', radius=1.0)

# 设置刚度
bpy.ops.object.metaball_add(type='BALL', stiffness=0.5)

# 设置副本
bpy.ops.object.metaball_add(type='BALL', copy_radius=False)
```

### 元球转换

```python
# 转换为网格
bpy.ops.object.convert(target='MESH')

# 转换为曲线
bpy.ops.object.convert(target='CURVE')

# 转换为曲面
bpy.ops.object.convert(target='SURFACE')
```

## 实际应用示例

### 创建基础元球

```python
def create_basic_metaball(name='BasicMetaball'):
    """创建基础元球"""
    bpy.ops.object.metaball_add(type='BALL', radius=1.0, location=(0, 0, 0))
    metaball = bpy.context.active_object
    metaball.name = name
    
    # 设置分辨率
    metaball.data.resolution = 0.2
    
    return metaball
```

### 创建程序化水滴

```python
def create_dripping_effect(count=10, center=(0, 0, 0), radius=1.0):
    """创建水滴效果"""
    import random
    import math
    
    # 创建中心元球
    bpy.ops.object.metaball_add(type='BALL', radius=radius, location=center)
    center_ball = bpy.context.active_object
    center_ball.name = 'DripCenter'
    
    # 添加滴落元球
    for i in range(count):
        x = center[0] + random.uniform(-2, 2)
        y = center[1] + random.uniform(-2, 2)
        z = center[2] + random.uniform(-1, 3)
        
        bpy.ops.object.metaball_add(
            type='BALL',
            radius=random.uniform(0.2, 0.5),
            location=(x, y, z)
        )
        
        ball = bpy.context.active_object
        ball.name = f'Drip_{i}'
    
    return bpy.context.active_object
```

### 创建有机形状

```python
def create_organic_shape(seed=42):
    """创建有机形状"""
    import random
    random.seed(seed)
    
    # 创建主元球
    bpy.ops.object.metaball_add(type='ELLIPSOID', radius=2.0, location=(0, 0, 0))
    main_ball = bpy.context.active_object
    main_ball.name = 'OrganicMain'
    
    # 添加子元球
    for i in range(15):
        x = random.uniform(-3, 3)
        y = random.uniform(-3, 3)
        z = random.uniform(-3, 3)
        
        radius = random.uniform(0.3, 1.0)
        
        bpy.ops.object.metaball_add(
            type='BALL',
            radius=radius,
            location=(x, y, z)
        )
        
        ball = bpy.context.active_object
        ball.name = f'OrganicPart_{i}'
    
    # 设置阈值
    main_ball.data.threshold = 1.0
    
    return main_ball
```

### 创建细胞结构

```python
def create_cell_structure(cells=20, cluster_centers=3):
    """创建细胞结构"""
    import random
    
    # 创建聚类中心
    centers = []
    for i in range(cluster_centers):
        centers.append((
            random.uniform(-5, 5),
            random.uniform(-5, 5),
            random.uniform(-5, 5)
        ))
    
    # 创建元球
    for i in range(cells):
        center = random.choice(centers)
        
        x = center[0] + random.uniform(-2, 2)
        y = center[1] + random.uniform(-2, 2)
        z = center[2] + random.uniform(-2, 2)
        
        radius = random.uniform(0.3, 0.8)
        
        bpy.ops.object.metaball_add(
            type='BALL',
            radius=radius,
            location=(x, y, z)
        )
        
        cell = bpy.context.active_object
        cell.name = f'Cell_{i}'
    
    return bpy.context.active_object
```

### 创建流体球效果

```python
def create_fluid_balls(ball_count=8, container_size=3):
    """创建流体球效果"""
    import random
    
    for i in range(ball_count):
        x = random.uniform(-container_size, container_size)
        y = random.uniform(-container_size, container_size)
        z = random.uniform(-container_size, container_size)
        
        radius = random.uniform(0.5, 1.5)
        
        bpy.ops.object.metaball_add(
            type='BALL',
            radius=radius,
            location=(x, y, z)
        )
        
        ball = bpy.context.active_object
        ball.name = f'Fluid_{i}'
    
    return bpy.context.active_object
```

### 创建复杂拓扑

```python
def create_complex_topology():
    """创建复杂拓扑元球"""
    import math
    
    # 创建中心椭圆体
    bpy.ops.object.metaball_add(type='ELLIPSOID', radius=2.0, location=(0, 0, 0))
    center = bpy.context.active_object
    center.data.stiffness = 0.8
    
    # 创建环形元球
    ring_count = 12
    ring_radius = 3.0
    
    for i in range(ring_count):
        angle = (i / ring_count) * 2 * math.pi
        x = ring_radius * math.cos(angle)
        y = ring_radius * math.sin(angle)
        
        bpy.ops.object.metaball_add(
            type='BALL',
            radius=0.5,
            location=(x, y, 0)
        )
        
        ball = bpy.context.active_object
        ball.name = f'Ring_{i}'
    
    # 创建垂直元球
    for i in range(4):
        z = (i - 1.5) * 2
        bpy.ops.object.metaball_add(
            type='BALL',
            radius=0.6,
            location=(0, 0, z)
        )
    
    return center
```

### 创建元球动画

```python
def create_metaball_animation(metaball_obj, frames=60, amplitude=1.0):
    """创建元球动画"""
    import math
    
    # 设置帧范围
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = frames
    
    # 动画参数
    for frame in range(1, frames + 1):
        bpy.context.scene.frame_set(frame)
        
        t = (frame - 1) / frames * 2 * math.pi
        
        # 脉动效果
        scale = 1.0 + amplitude * math.sin(t)
        metaball_obj.scale = (scale, scale, scale)
        metaball_obj.keyframe_insert(data_path='scale')
    
    return metaball_obj
```

### 将元球转换为网格

```python
def convert_metaball_to_mesh(metaball_obj, name='MetaballMesh'):
    """将元球转换为网格"""
    # 选择元球
    bpy.ops.object.select_all(action='DESELECT')
    metaball_obj.select_set(True)
    bpy.context.view_layer.objects.active = metaball_obj
    
    # 转换为网格
    bpy.ops.object.convert(target='MESH')
    
    # 重命名
    mesh_obj = bpy.context.active_object
    mesh_obj.name = name
    
    return mesh_obj
```

## 使用场景

- **有机形状**：创建自然有机的形状
- **水滴效果**：创建流体和液体效果
- **细胞结构**：模拟生物细胞
- **软体效果**：创建软体和变形效果
- **程序化动画**：创建动态变化的有机形状
- **拓扑生成**：生成复杂表面

## 注意事项

- 元球需要转换为网格才能渲染
- 分辨率影响质量和性能
- 阈值控制融合效果
- 刚度控制形状硬度
- 大量元球影响性能
- 元球不支持细分曲面


---
## bpy_ops_text

# bpy.ops.text - Text Operations Module

Blender 文本操作符，用于在文本编辑器中创建、编辑和管理文本数据块。

## Overview

`bpy.ops.text` 模块包含文本编辑器中的所有操作命令，涵盖文本的创建、编辑、选择、查找替换等核心功能。这个模块主要用于处理 Blender 的文本数据块和脚本文件。

## 核心功能

### 文本选择操作

```python
import bpy

# 全选
bpy.ops.text.select_all(select='SELECT')

# 全不选
bpy.ops.text.select_all(select='DESELECT')

# 反选
bpy.ops.text.select_all(select='INVERT')

# 选择行
bpy.ops.text.select_line()

# 移动光标到文件开头
bpy.ops.text.move(btype='LINE_BEGIN', select=False)

# 移动光标到文件末尾
bpy.ops.text.move(btype='LINE_END', select=False)

# 向上移动
bpy.ops.text.move(btype='PREVIOUS_LINE', select=False)

# 向下移动
bpy.ops.text.move(btype='NEXT_LINE', select=False)

# 移动到上一个字符
bpy.ops.text.move(btype='PREVIOUS_CHARACTER', select=False)

# 移动到下一个字符
bpy.ops.text.move(btype='NEXT_CHARACTER', select=False)

# 移动到上一个单词
bpy.ops.text.move(btype='PREVIOUS_WORD', select=False)

# 移动到下一个单词
bpy.ops.text.move(btype='NEXT_WORD', select=False)

# 跳转到指定行
bpy.ops.text.jump(line_number=10)

# 滚动到光标位置
bpy.ops.text.scroll(lines=-5)

def select_all_text():
    """全选文本"""
    text = bpy.context.space_data.text
    if text:
        bpy.ops.text.select_all(select='SELECT')
        return text.as_string()
    return ""

def select_current_line():
    """选择当前行"""
    bpy.ops.text.select_line()
    text = bpy.context.space_data.text
    if text and text.current_line:
        return text.current_line.body
    return ""

def go_to_line(line_num):
    """跳转到指定行"""
    text = bpy.context.space_data.text
    if text and 1 <= line_num <= len(text.lines):
        bpy.ops.text.jump(line_number=line_num)
        return True
    return False
```

### 文本编辑操作

```python
# 剪切选中文本
bpy.ops.text.cut()

# 复制选中文本
bpy.ops.text.copy()

# 粘贴文本
bpy.ops.text.paste()

# 删除选中文本
bpy.ops.text.delete(type='DELETE')

# 删除前一个字符
bpy.ops.text.delete(type='PREVIOUS_CHARACTER')

# 删除到行末
bpy.ops.text.delete(type='NEXT_CHARACTER')

# 插入新行
bpy.ops.text.insert()

# 缩进
bpy.ops.text.indent()

# 取消缩进
bpy.ops.text.unindent()

# 注释
bpy.ops.text.comment()

# 取消注释
bpy.ops.text.uncomment()

# 转换为大写
bpy.ops.text.case(case='UPPER')

# 转换为小写
bpy.ops.text.case(case='LOWER')

# 首字母大写
bpy.ops.text.case(case='TITLE')

# 反转大小写
bpy.ops.text.case(case='SWAP')

def insert_text(text_obj, content):
    """在光标位置插入文本"""
    bpy.context.space_data.text = text_obj
    bpy.ops.text.insert(text=content)

def delete_selection():
    """删除选中的文本"""
    bpy.ops.text.delete(type='DELETE')

def comment_lines(line_start, line_end):
    """注释指定范围的行"""
    bpy.ops.text.select_line()
    # 注意：实际实现需要更复杂的逻辑
    bpy.ops.text.comment()
```

### 文本查找替换

```python
# 查找文本
bpy.ops.text.find(find_text="def ", flags=REGEX=False, wrap=True)

# 查找并替换
bpy.ops.text.find_replace(find_text="old_text", replace_text="new_text", 
                          flags=REGEX, all=True)

# 清除查找高亮
bpy.ops.text.find_set_cursor(flag=False)

def find_all_occurrences(text_obj, search_text):
    """查找所有匹配项"""
    bpy.context.space_data.text = text_obj
    results = []
    for i, line in enumerate(text_obj.lines):
        if search_text in line.body:
            results.append({
                'line_number': i + 1,
                'content': line.body
            })
    return results

def replace_all(text_obj, old_text, new_text):
    """替换所有匹配项"""
    bpy.context.space_data.text = text_obj
    bpy.ops.text.find_replace(
        find_text=old_text,
        replace_text=new_text,
        flags=REGEX,
        all=True
    )
```

### 文本运行操作

```python
# 运行脚本
bpy.ops.text.run_script()

# 运行选中的脚本
bpy.ops.text.run_script()

# 检查语法
bpy.ops.text.check_globals()

# 自动格式化
bpy.ops.text.auto_indent(action='INSERT', type='SPACE')

def execute_script():
    """执行当前文本块中的脚本"""
    text = bpy.context.space_data.text
    if text:
        bpy.ops.text.run_script()

def execute_selection():
    """执行选中的代码"""
    bpy.ops.text.copy()
    # 执行复制的代码
    exec(bpy.context.window_manager.clipboard)
```

### 文本块操作

```python
# 新建文本块
bpy.ops.text.new(filepath="")

# 打开文本文件
bpy.ops.text.open(filepath="C:/path/to/script.py")

# 保存文本块
bpy.ops.text.save()

# 另存为
bpy.ops.text.save_as(filepath="C:/path/to/save_as.py")

# 重新加载
bpy.ops.text.reload()

# 拆分文本块
bpy.ops.text.split()

# 合并文本块
bpy.ops.text.merge()

def create_new_script(name, content=""):
    """创建新的脚本文件"""
    bpy.ops.text.new(filepath="")
    text = bpy.context.space_data.text
    text.name = name
    if content:
        bpy.ops.text.insert(text=content)
    return text

def save_script(text_obj, filepath):
    """保存脚本到文件"""
    bpy.context.space_data.text = text_obj
    bpy.ops.text.save_as(filepath=filepath)

def load_script(filepath):
    """加载脚本文件"""
    bpy.ops.text.open(filepath=filepath)
    return bpy.context.space_data.text
```

## 实际应用示例

### 脚本管理器

```python
import bpy
import os

class ScriptManager:
    """脚本管理器"""
    
    def __init__(self):
        self.script_dir = bpy.app.tempdir
    
    def create_script(self, name, code):
        """创建脚本"""
        bpy.ops.text.new(filepath="")
        text = bpy.context.space_data.text
        text.name = name
        bpy.ops.text.insert(text=code)
        return text
    
    def load_script(self, filepath):
        """加载脚本"""
        if os.path.exists(filepath):
            bpy.ops.text.open(filepath=filepath)
            return bpy.context.space_data.text
        return None
    
    def save_script(self, text, filepath=None):
        """保存脚本"""
        bpy.context.space_data.text = text
        if filepath:
            bpy.ops.text.save_as(filepath=filepath)
        else:
            bpy.ops.text.save()
    
    def run_script(self, text):
        """运行脚本"""
        bpy.context.space_data.text = text
        bpy.ops.text.run_script()
    
    def list_scripts(self):
        """列出所有文本块"""
        return [text.name for text in bpy.data.texts]
    
    def delete_script(self, name):
        """删除脚本"""
        if name in bpy.data.texts:
            bpy.data.texts.remove(bpy.data.texts[name])
    
    def duplicate_script(self, name, new_name):
        """复制脚本"""
        if name in bpy.data.texts:
            old_text = bpy.data.texts[name]
            bpy.ops.text.new(filepath="")
            new_text = bpy.context.space_data.text
            new_text.name = new_name
            new_text.write(old_text.as_string())
            return new_text
        return None


def create_script_library():
    """创建脚本库"""
    manager = ScriptManager()
    
    # 创建常用脚本
    manager.create_script("utils.py", '''
def hello_world():
    print("Hello, World!")

def calculate_area(width, height):
    return width * height
''')
    
    manager.create_script("setup_scene.py", '''
import bpy

def setup_basic_scene():
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # 创建相机
    bpy.ops.object.camera_add(location=(0, -10, 5))
    camera = bpy.context.active_object
    camera.rotation_euler = (1.2, 0, 0)
    bpy.context.scene.camera = camera
    
    # 创建光源
    bpy.ops.object.light_add(location=(5, -5, 10))
    light = bpy.context.active_object
    light.data.energy = 1000
    
    return camera, light
''')
    
    print("脚本库创建完成")
    print(f"脚本列表: {manager.list_scripts()}")
    return manager
```

### 代码片段库

```python
import bpy

class CodeSnippetLibrary:
    """代码片段库"""
    
    snippets = {
        "create_cube": '''
bpy.ops.mesh.primitive_cube_add(size=1, location=(0, 0, 0))
cube = bpy.context.active_object
cube.name = "MyCube"
''',
        "create_sphere": '''
bpy.ops.mesh.primitive_uv_sphere_add(radius=1, location=(0, 0, 0))
sphere = bpy.context.active_object
sphere.name = "MySphere"
''',
        "create_material": '''
mat = bpy.data.materials.new(name="NewMaterial")
mat.use_nodes = True
obj.active_material = mat
''',
        "add_driver": '''
def add_driver(source, prop, target, path):
    driver = source.driver_add(prop)
    var = driver.variables.new()
    var.targets[0].id = target
    var.targets[0].data_path = path
    driver.modifiers[0].mode = 'Averaged'
'''
    }
    
    def __init__(self, library_name="Snippets"):
        self.library_name = library_name
        self._ensure_library()
    
    def _ensure_library(self):
        """确保库存在"""
        if self.library_name not in bpy.data.texts:
            bpy.ops.text.new(filepath="")
            text = bpy.context.space_data.text
            text.name = self.library_name
    
    def add_snippet(self, name, code):
        """添加代码片段"""
        self.snippets[name] = code
        text = bpy.data.texts[self.library_name]
        text.write(f"\n# {name}\n{code}\n")
    
    def insert_snippet(self, name):
        """插入代码片段"""
        if name in self.snippets:
            bpy.ops.text.insert(text=self.snippets[name])
            return True
        return False
    
    def get_snippet(self, name):
        """获取代码片段"""
        return self.snippets.get(name, "")
    
    def list_snippets(self):
        """列出所有代码片段"""
        return list(self.snippets.keys())
    
    def load_snippets_from_text(self, text_name):
        """从文本块加载代码片段"""
        if text_name in bpy.data.texts:
            text = bpy.data.texts[text_name]
            content = text.as_string()
            # 解析代码片段（假设格式为 # snippet_name\ncode）
            lines = content.split('\n')
            current_name = None
            current_code = []
            
            for line in lines:
                if line.startswith('# '):
                    if current_name and current_code:
                        self.snippets[current_name] = '\n'.join(current_code)
                    current_name = line[2:].strip()
                    current_code = []
                elif current_name:
                    current_code.append(line)
            
            if current_name and current_code:
                self.snippets[current_name] = '\n'.join(current_code)
    
    def save_snippets_to_text(self, text_name):
        """保存代码片段到文本块"""
        if text_name not in bpy.data.texts:
            bpy.ops.text.new(filepath="")
            text = bpy.context.space_data.text
            text.name = text_name
        else:
            bpy.context.space_data.text = bpy.data.texts[text_name]
            bpy.ops.text.select_all(select='SELECT')
            bpy.ops.text.delete(type='DELETE')
        
        for name, code in self.snippets.items():
            bpy.ops.text.insert(text=f"# {name}\n{code}\n")


def setup_snippet_library():
    """设置代码片段库"""
    library = CodeSnippetLibrary("CodeSnippets")
    
    print("代码片段库已创建")
    print(f"可用片段: {library.list_snippets()}")
    
    return library
```


---
## bpy_ops_text_editor

# bpy.ops.text_editor - 文本编辑器操作模块

Blender文本编辑器操作符，用于在Blender内置文本编辑器中管理和编辑文本。

## 概述

`bpy.ops.text_editor` 模块提供用于操作Blender内置文本编辑器的功能，包括文本选择、编辑、搜索替换、格式化等操作。

## 核心功能

### 文本选择操作

```python
import bpy

# 全选文本
bpy.ops.text_editor.select_all(action='SELECT')

# 全不选
bpy.ops.text_editor.select_all(action='DESELECT')

# 反选
bpy.ops.text_editor.select_all(action='INVERT')

# 选择行
bpy.ops.text_editor.select_line()

# 选择单词
bpy.ops.text_editor.select_word()

# 选择括号内容
bpy.ops.text_editor.select_parentheses()

# 选择到行首
bpy.ops.text_editor.select_start_of_line()

# 选择到行尾
bpy.ops.text_editor.select_end_of_line()

# 选择到文件开头
bpy.ops.text_editor.select_start_of_file()

# 选择到文件结尾
bpy.ops.text_editor.select_end_of_file()

# 扩展选择到下一个单词
bpy.ops.text_editor.select_word_end(extend=True)

# 扩展选择到上一个单词
bpy.ops.text_editor.select_word_start(extend=True)

# 扩展选择到下一行
bpy.ops.text_editor.select_next_line(extend=True)

# 扩展选择到上一行
bpy.ops.text_editor.select_prev_line(extend=True)
```

### 文本编辑操作

```python
# 复制文本
bpy.ops.text_editor.copy()

# 剪切文本
bpy.ops.text_editor.cut()

# 粘贴文本
bpy.ops.text_editor.paste()

# 删除选中
bpy.ops.text_editor.delete()

# 删除行
bpy.ops.text_editor.delete_line()

# 删除到行尾
bpy.ops.text_editor.delete_to_end_of_line()

# 删除到行首
bpy.ops.text_editor.delete_to_start_of_line()

# 删除单词
bpy.ops.text_editor.delete_word()

# 删除前一个字符
bpy.ops.text_editor.delete_previous_character()

# 删除下一个字符
bpy.ops.text_editor.delete_next_character()

# 撤销
bpy.ops.text_editor.undo()

# 重做
bpy.ops.text_editor.redo()

# 重复行
bpy.ops.text_editor.duplicate_line()

# 合并行
bpy.ops.text_editor.merge_lines()
```

### 插入操作

```python
# 插入新行
bpy.ops.text_editor.insert_line()

# 在上方插入行
bpy.ops.text_editor.insert_line_above()

# 在下方插入行
bpy.ops.text_editor.insert_line_below()

# 插入制表符
bpy.ops.text_editor.insert_tab()

# 插入日期
bpy.ops.text_editor.insert_date()

# 插入时间
bpy.ops.text_editor.insert_time()

# 插入文件路径
bpy.ops.text_editor.insert_filepath()

# 插入对象名
bpy.ops.text_editor.insert_object_name()

# 插入当前属性
bpy.ops.text_editor.insert_property()
```

### 搜索和替换

```python
# 查找文本
bpy.ops.text_editor.find(text='search_text')

# 查找下一个
bpy.ops.text_editor.find_next()

# 查找上一个
bpy.ops.text_editor.find_previous()

# 替换文本
bpy.ops.text_editor.replace(text='search_text', replace='new_text')

# 替换所有
bpy.ops.text_editor.replace_all(text='search_text', replace='new_text')

# 使用正则表达式查找
bpy.ops.text_editor.find_regex(pattern='regex_pattern')

# 高亮全部
bpy.ops.text_editor.find_all(text='search_text')

# 清除高亮
bpy.ops.text_editor.find_clear()
```

### 文本格式化

```python

# 自动缩进
bpy.ops.text_editor.indent()

# 取消缩进
bpy.ops.text_editor.unindent()

# 注释代码
bpy.ops.text_editor.comment()

# 取消注释
bpy.ops.text_editor.uncomment()

# 区块注释
bpy.ops.text_editor.block_comment()

# 取消区块注释
bpy.ops.text_editor.unblock_comment()

# 格式化代码
bpy.ops.text_editor.format()

# 整理格式
bpy.ops.text_editor.reformat()

# 转换制表符为空格
bpy.ops.text_editorTabs_to_spaces()

# 转换空格为制表符
bpy.ops.text_editor.spaces_to_tabs()

# 大写转换
bpy.ops.text_editor.upper_case()

# 小写转换
bpy.ops.text_editor.lower_case()

# 首字母大写
bpy.ops.text_editor.capitalize()

# 交换大小写
bpy.ops.text_editor.swap_case()
```

### 文本移动

```python
# 移动到行首
bpy.ops.text_editor.move_to_start_of_line()

# 移动到行尾
bpy.ops.text_editor.move_to_end_of_line()

# 移动到文件开头
bpy.ops.text_editor.move_to_start_of_file()

# 移动到文件结尾
bpy.ops.text_editor.move_to_end_of_file()

# 移动到上一个单词
bpy.ops.text_editor.move_word_left()

# 移动到下一个单词
bpy.ops.text_editor.move_word_right()

# 移动到匹配括号
bpy.ops.text_editor.move_to_matching_bracket()

# 滚动到行
bpy.ops.text_editor.scroll_to_line(line_number=100)

# 滚动到选中
bpy.ops.text_editor.scroll_to_selection()
```

### 书签和跳转

```python
# 切换书签
bpy.ops.text_editor.toggle_bookmark()

# 添加书签
bpy.ops.text_editor.add_bookmark()

# 删除书签
bpy.ops.text_editor.clear_bookmark()

# 清除所有书签
bpy.ops.text_editor.clear_all_bookmarks()

# 跳转到下一个书签
bpy.ops.text_editor.next_bookmark()

# 跳转到上一个书签
bpy.ops.text_editor.prev_bookmark()

# 添加位置标记
bpy.ops.text_editor.add_position_marker()

# 跳转到位置标记
bpy.ops.text_editor.jump_to_marker(index=0)

# 清除位置标记
bpy.ops.text_editor.clear_position_markers()
```

### 文本转换

```python
# 转换为大写
bpy.ops.text_editor.upper_case()

# 转换为小写
bpy.ops.text_editor.lower_case()

# 转换为标题
bpy.ops.text_editor.title_case()

# 转换为换行
bpy.ops.text_editor.to_endoffile_as_newline()

# 转换换行符
bpy.ops.text_editor.convert_line_endings(type='UNIX')

# 排序行
bpy.ops.text_editor.sort_lines()

# 反向排序
bpy.ops.text_editor.sort_lines_reverse()

# 移除空行
bpy.ops.text_editor.remove_empty_lines()

# 移除重复行
bpy.ops.text_editor.remove_duplicate_lines()

# 合并行
bpy.ops.text_editor.join_lines()
```

### 文件操作

```python
# 新建文本
bpy.ops.text_editor.new()

# 打开文件
bpy.ops.text_editor.open(filepath='//script.py')

# 保存文件
bpy.ops.text_editor.save()

# 另存为
bpy.ops.text_editor.save_as(filepath='//new_script.py')

# 重新加载
bpy.ops.text_editor.reload()

# 导入文本
bpy.ops.text_editor.import_()

# 导出文本
bpy.ops.text_editor.export()
```

### 编码和语言

```python
# 设置编码
bpy.ops.text_editor.set_encoding(encoding='UTF-8')

# 检测编码
bpy.ops.text_editor.detect_encoding()

# 设置语言
bpy.ops.text_editor.set_language(language='Python')

# 设置语法高亮
bpy.ops.text_editor.set_syntax(syntax='Python')

# 设置换行方式
bpy.ops.text_editor.set_line_wrapping(wrap=True)

# 设置自动换行
bpy.ops.text_editor.set_auto_wrap()
```

## 实用函数

```python
def get_selected_text():
    """获取选中的文本"""
    text = bpy.context.space_data
    return text.selected_lines

def get_current_line_text():
    """获取当前行文本"""
    text = bpy.context.space_data
    return text.current_line.body

def get_line_number():
    """获取当前行号"""
    return bpy.context.space_data.current_line_index

def replace_in_file(old_text, new_text):
    """替换文件中的文本"""
    bpy.ops.text_editor.select_all(action='SELECT')
    bpy.ops.text_editor.replace(text=old_text, replace=new_text)

def insert_at_cursor(text):
    """在光标位置插入文本"""
    bpy.ops.text_editor.insert(text)

def get_text_stats():
    """获取文本统计信息"""
    text = bpy.context.space_data
    return {
        'lines': len(text.lines),
        'characters': sum(len(l.body) for l in text.lines),
        'words': sum(len(l.body.split()) for l in text.lines)
    }

def format_python_code():
    """格式化Python代码"""
    bpy.ops.text_editor.select_all(action='SELECT')
    bpy.ops.text_editor.format()

def toggle_bookmarks_all():
    """切换所有行的书签"""
    for i in range(len(bpy.context.space_data.lines)):
        bpy.ops.text_editor.move(line=i)
        bpy.ops.text_editor.toggle_bookmark()

def find_in_all_open():
    """在所有打开的文本中查找"""
    search_term = 'search_text'
    results = []
    for text in bpy.data.texts:
        if search_term in text.as_string():
            results.append(text.name)
    return results
```

## 常见问题排查

### 文本不显示
```python
# 检查当前文本
print(f"当前文本: {bpy.context.space_data.text}")

# 检查行数
print(f"行数: {len(bpy.context.space_data.lines)}")

# 重新加载
bpy.ops.text_editor.reload()

# 检查显示设置
text = bpy.context.space_data
print(f"显示行号: {text.show_line_numbers}")
print(f"显示空白: {text.show_whitespace}")
```

### 选择操作问题
```python
# 清除选择
bpy.ops.text_editor.select_all(action='DESELECT')

# 重置选择
bpy.ops.text_editor.move_to_start_of_file()
bpy.ops.text_editor.select_end_of_file()

# 检查选择模式
print(f"选择模式: {bpy.context.space_data.select_end}")
```

### 编码问题
```python
# 检查当前编码
print(f"当前编码: {bpy.context.space_data.encoding}")

# 尝试重新打开
bpy.ops.text_editor.reload()

# 设置正确编码
bpy.ops.text_editor.set_encoding(encoding='UTF-8')
```


---
## bpy_ops_ed

# bpy.ops.ed - 编辑器操作模块

Blender编辑器通用操作符，用于各种编辑器中的编辑操作。

## 概述

`bpy.ops.ed` 模块提供用于在Blender各种编辑器中进行通用编辑操作的功能，包括撤销、重做、复制、粘贴等。

## 核心功能

### 撤销重做

```python
import bpy

# 撤销
bpy.ops.ed.undo()

# 重做
bpy.ops.ed.redo()

# 撤销历史
bpy.ops.ed.undo_history(
    step=1
)

# 撤销消息
bpy.ops.ed.undo_push(
    message='自定义操作'
)

# 撤销重新引发
bpy.ops.ed.undo_redo()
```

### 撤销重做设置

```python
# 启用撤销
bpy.context.preferences.edit.use_global_undo = True

# 禁用撤销
bpy.context.preferences.edit.use_global_undo = False

# 设置撤销步数
bpy.context.preferences.edit.undo_steps = 256

# 设置撤销内存
bpy.context.preferences.edit.undo_memory_limit = 64
```

### 复制粘贴

```python
# 复制
bpy.ops.ed.copy(
    buffer_name='BUFFER'
)

# 粘贴
bpy.ops.ed.paste(
    buffer_name='BUFFER'
)

# 粘贴历史
bpy.ops.ed.paste_buffer(
    flip=True,
    offset=(0, 0, 0)
)

# 复制选择
bpy.ops.ed.copy_selected()
```

### 选择操作

```python
# 全选
bpy.ops.ed.select_all(
    action='SELECT'
)

# 取消全选
bpy.ops.ed.select_all(
    action='DESELECT'
)

# 反选
bpy.ops.ed.select_all(
    action='INVERT'
)

# 选择相似
bpy.ops.ed.select_all(
    action='SIMILAR'
)
```

### 删除操作

```python
# 删除
bpy.ops.ed.delete()

# 删除行
bpy.ops.ed.delete_line()

# 删除字符
bpy.ops.ed.delete_char(
    direction='FORWARD'
)

# 删除单词
bpy.ops.ed.delete_word(
    direction='FORWARD'
)
```

### 插入操作

```python
# 插入换行
bpy.ops.ed.insert()

# 插入字符
bpy.ops.ed.insert_text(
    text='a'
)

# 插入换行符
bpy.ops.ed.insert_line_break()
```

### 移动操作

```python
# 移动行
bpy.ops.ed.move_lines(
    direction='DOWN'
)

# 移动字符
bpy.ops.ed.move_char(
    direction='FORWARD'
)

# 移动单词
bpy.ops.ed.move_word(
    direction='FORWARD'
)

# 移动到行首
bpy.ops.ed.move_line_start()

# 移动到行尾
bpy.ops.ed.move_line_end()
```

### 查找替换

```python
# 查找
bpy.ops.ed.find()

# 查找下一个
bpy.ops.ed.find_next()

# 查找上一个
bpy.ops.ed.find_previous()

# 替换
bpy.ops.ed.replace()

# 替换全部
bpy.ops.ed.replace_all()
```

### 缩进操作

```python
# 增加缩进
bpy.ops.ed.indent(
    insert=True
)

# 减少缩进
bpy.ops.ed.indent(
    insert=False
)

# 自动缩进
bpy.ops.ed.auto_indent()

# 转换缩进
bpy.ops.ed.indentation_convert()
```

### 注释操作

```python
# 切换注释
bpy.ops.ed.comment_toggle()

# 添加注释
bpy.ops.ed.comment_add()

# 删除注释
bpy.ops.ed.comment_remove()
```

## 实用函数

```python
def select_all_in_editor():
    """全选"""
    bpy.ops.ed.select_all(action='SELECT')

def deselect_all_in_editor():
    """取消全选"""
    bpy.ops.ed.select_all(action='DESELECT')

def invert_selection():
    """反选"""
    bpy.ops.ed.select_all(action='INVERT')

def undo_operation():
    """撤销"""
    bpy.ops.ed.undo()

def redo_operation():
    """重做"""
    bpy.ops.ed.redo()

def copy_selection():
    """复制选择内容"""
    bpy.ops.ed.copy()

def paste_buffer():
    """粘贴"""
    bpy.ops.ed.paste()

def find_text(text):
    """查找文本"""
    bpy.ops.ed.find()
    # 在查找框中输入文本
    for char in text:
        bpy.ops.ed.insert_text(text=char)

def replace_text(old_text, new_text):
    """替换文本"""
    bpy.ops.ed.find()
    bpy.ops.ed.replace()
    # 在替换框中输入文本
    for char in old_text:
        bpy.ops.ed.delete_char(direction='FORWARD')
    for char in new_text:
        bpy.ops.ed.insert_text(text=char)

def move_line_down():
    """下移行"""
    bpy.ops.ed.move_lines(direction='DOWN')

def move_line_up():
    """上移行"""
    bpy.ops.ed.move_lines(direction='UP')

def indent_selected():
    """增加选中缩进"""
    bpy.ops.ed.indent(insert=True)

def dedent_selected():
    """减少选中缩进"""
    bpy.ops.ed.indent(insert=False)
```

## 常见问题排查

### 撤销不工作
```python
# 检查撤销设置
print(f"全局撤销: {bpy.context.preferences.edit.use_global_undo}")
print(f"撤销步数: {bpy.context.preferences.edit.undo_steps}")

# 启用撤销
bpy.context.preferences.edit.use_global_undo = True

# 检查撤销历史
print(f"撤销历史: {bpy.context.scene.undo_stack}")

# 手动触发撤销
bpy.ops.ed.undo()
```

### 选择不工作
```python
# 检查编辑器类型
print(f"区域类型: {bpy.context.area.type}")

# 确保在可编辑区域
if bpy.context.area.type in ['TEXT_EDITOR', 'CONSOLE', 'NODE_EDITOR']:
    print("支持选择操作")
else:
    print("不支持选择操作")

# 重置选择
bpy.ops.ed.select_all(action='DESELECT')
```

### 复制粘贴问题
```python
# 检查剪贴板
import os
print(f"剪贴板内容: {os.popen('powershell Get-Clipboard').read()}")

# 检查缓冲区
print(f"缓冲区: {bpy.context.scene.tool_settings.buffer}")

# 清除缓冲区
bpy.ops.ed.copy(buffer_name='BUFFER')

# 重新复制
bpy.ops.ed.copy()
```


---
## bpy_ops_geometry

# bpy.ops.geometry - 几何操作模块

Blender几何操作符，用于在3D视口中进行几何计算和变换。

## 概述

`bpy.ops.geometry` 模块提供用于执行几何计算、空间变换和几何数据分析的操作功能。这些操作对于程序化建模、几何处理和数学计算非常有用。

## 核心功能

### 距离测量

```python
import bpy
import math

# 计算两点之间的距离
bpy.ops.geometry.distance_measure(
    mode='POINTS',
    source_point=(0, 0, 0),
    target_point=(1, 1, 1)
)

# 测量选中对象之间的距离
bpy.ops.geometry.distance_measure(mode='OBJECTS')

# 测量边长
bpy.ops.geometry.distance_measure(mode='EDGES')

# 测量曲线的长度
bpy.ops.geometry.measure_length(
    target='ACTIVE_OBJECT'
)

# 测量表面积
bpy.ops.geometry.measure_area(
    target='ACTIVE_OBJECT'
)

# 测量体积
bpy.ops.geometry.measure_volume(
    target='ACTIVE_OBJECT'
)

# 测量角度
bpy.ops.geometry.measure_angle(
    vertex=(0, 0, 0),
    point1=(1, 0, 0),
    point2=(0, 1, 0)
)
```

### 空间变换

```python
# 绕任意轴旋转
bpy.ops.geometry.rotate(
    input_vector=(1, 0, 0),
    center=(0, 0, 0),
    axis=(0, 0, 1),
    angle=math.radians(45)
)

# 沿法线方向挤出
bpy.ops.geometry.extrude_along_normal(
    distance=1.0,
    steps=1
)

# 沿曲线挤出
bpy.ops.geometry.extrude_along_curve(
    curve='CurvePath'
)

# 沿路径变形
bpy.ops.geometry.deform_along_path(
    path='CurvePath'
)

# 镜像几何体
bpy.ops.geometry.mirror(
    mirror_axis='X',
    center=(0, 0, 0)
)

# 缩放沿轴
bpy.ops.geometry.scale_along_axis(
    factor=2.0,
    axis='Z'
)

# 投影到平面
bpy.ops.geometry.project_to_plane(
    plane_point=(0, 0, 0),
    plane_normal=(0, 0, 1)
)

# 投影到球面
bpy.ops.geometry.project_to_sphere(
    center=(0, 0, 0),
    radius=1.0
)
```

### 布尔运算

```python
# 几何体交集
bpy.ops.geometry.boolean_intersect(
    solver='FAST',
    tolerance=0.001
)

# 几何体并集
bpy.ops.geometry.boolean_union(
    solver='FAST'
)

# 几何体差集
bpy.ops.geometry.boolean_difference(
    solver='FAST',
    tolerance=0.001
)

# 切片操作
bpy.ops.geometry.slice(
    plane_point=(0, 0, 0),
    plane_normal=(0, 0, 1),
    fill_holes=True
)

# 切割几何体
bpy.ops.geometry.cut(
   切割_object='CutObject',
    use_fill=False
)

# 分离重叠
bpy.ops.geometry.split_overlap(
    precision=0.001
)

# 合并接近的顶点
bpy.ops.geometry.merge_vertices(
    distance=0.001
)
```

### 曲率计算

```python
# 计算平均曲率
bpy.ops.geometry.curvature(
    curvature_type='MEAN'
)

# 计算高斯曲率
bpy.ops.geometry.curvature(
    curvature_type='GAUSS'
)

# 计算主曲率
bpy.ops.geometry.curvature(
    curvature_type='PRINCIPAL'
)

# 计算法线
bpy.ops.geometry.recalculate_normals(
    inside=False,
    sharp_edges=True
)

# 计算切线
bpy.ops.geometry.recalculate_tangents(
    uv_layer='UVMap'
)

# 计算包围盒
bpy.ops.geometry.bounding_box(
    calc_center=True
)
```

### 三角化

```python
# 三角化网格
bpy.ops.geometry.triangulate(
    ngon_method='BEAUTY',
    quad_method='SHORTEST_DIAGONAL'
)

# 转换为三角面
bpy.ops.geometry.quads_convert_to_tris()

# 反三角化
bpy.ops.geometry.tris_to_quads()
```

### 细分曲面

```python
# 细分网格
bpy.ops.geometry.subdivide(
    number_cuts=1,
    smoothness=0.0,
    quadtri=False
)

# 细分边
bpy.ops.geometry.subdivide_edges(
    number_cuts=1,
    edge_percents={}
)

# 连线细分
bpy.ops.geometry.subdivide_edgeloop(
    number_cuts=1
)

# 冠层细分
bpy.ops.geometry.compactness(
    distance=1.0
)
```

### 修复操作

```python

# 移除孤立顶点
bpy.ops.geometry.remove_isolated_vertices()

# 移除零点
bpy.ops.geometry.remove_zero_size()

# 填充孔洞
bpy.ops.geometry.fill_holes(
    sides=4
)

# 修复法线方向
bpy.ops.geometry.fix_normals()

# 修复非流形
bpy.ops.geometry.fix_non_manifold(
    method='DISSOLVE'
)

# 修复自交
bpy.ops.geometry.fix_self_intersections()
```

### 网格分析

```python
# 检查网格质量
bpy.ops.geometry.mesh_quality(
    threshold=0.001
)

# 检查非流形
bpy.ops.geometry.find_non_manifold()

# 检查零点
bpy.ops.geometry.find_zero_size()

# 检查自交
bpy.ops.geometry.find_self_intersections()

# 检查非平面面
bpy.ops.geometry.find_non_planar_faces(
    threshold=0.001
)

# 检查细长面
bpy.ops.geometry.find_thin_faces(
    threshold=0.1
)

# 统计信息
bpy.ops.geometry.mesh_stats()
```

### 顶点组操作

```python
# 根据几何体选择创建顶点组
bpy.ops.geometry.select_to_vertex_group()

# 根据顶点组选择几何体
bpy.ops.geometry.vertex_group_to_select()

# 顶点组反转选择
bpy.ops.geometry.vertex_group_invert()

# 顶点组扩展选择
bpy.ops.geometry.vertex_group_expand(
    factor=1
)

# 顶点组收缩选择
bpy.ops.geometry.vertex_group_shrink(
    factor=1
)

# 顶点组平滑
bpy.ops.geometry.vertex_group_smooth(
    factor=0.5
)

# 顶点组复制
bpy.ops.geometry.vertex_group_copy()

# 顶点组从权重创建
bpy.ops.geometry.vertex_group_from_weights(
    threshold=0.5
)
```

## 实用函数

```python
def get_mesh_center(obj):
    """获取网格中心点"""
    if obj.type == 'MESH':
        return obj.location + obj.data.vertices[0].co
    return obj.location

def get_total_surface_area(obj):
    """计算总表面积"""
    if obj.type == 'MESH':
        return sum(p.area for p in obj.data.polygons)
    return 0.0

def get_total_volume(obj):
    """计算总体积"""
    if obj.type == 'MESH':
        return obj.data.volume
    return 0.0

def get_bounding_box(obj):
    """获取包围盒"""
    return obj.bound_box

def normalize_vector(vec):
    """归一化向量"""
    length = math.sqrt(sum(v**2 for v in vec))
    if length > 0:
        return tuple(v/length for v in vec)
    return (0, 0, 0)

def distance_between_points(p1, p2):
    """计算两点间距离"""
    return math.sqrt(sum((a-b)**2 for a, b in zip(p1, p2)))

def angle_between_vectors(v1, v2):
    """计算两向量夹角"""
    dot = sum(a*b for a, b in zip(v1, v2))
    mag1 = math.sqrt(sum(v**2 for v in v1))
    mag2 = math.sqrt(sum(v**2 for v in v2))
    if mag1 * mag2 > 0:
        return math.acos(dot / (mag1 * mag2))
    return 0.0

def project_point_to_plane(point, plane_point, plane_normal):
    """将点投影到平面"""
    v = tuple(p - pp for p, pp in zip(point, plane_point))
    d = sum(v * n for v, n in zip(v, plane_normal))
    return tuple(p - d * n for p, n in zip(point, plane_normal))
```

## 常见问题排查

### 布尔运算失败
```python
# 检查对象是否有有效几何体
obj = bpy.context.active_object
print(f"顶点数: {len(obj.data.vertices)}")
print(f"面数: {len(obj.data.polygons)}")

# 确保对象是网格类型
print(f"对象类型: {obj.type}")

# 尝试不同的求解器
bpy.ops.geometry.boolean_intersect(solver='EXACT')

# 简化几何体
bpy.ops.geometry.decimate(ratio=0.5)
```

### 曲率计算问题
```python
# 确保网格有足够的细分
print(f"面数: {len(bpy.context.active_object.data.polygons)}")

# 重新计算法线
bpy.ops.geometry.recalculate_normals()

# 检查网格质量
bpy.ops.geometry.mesh_quality(threshold=0.0001)
```

### 测量数据不准确
```python
# 确保单位正确
print(f"场景单位: {bpy.context.scene.unit_settings.system}")

# 使用正确的缩放
obj = bpy.context.active_object
scale = obj.scale
print(f"对象缩放: {scale}")

# 应用变换
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
```


---
## bpy_ops_curves

# bpy.ops.curves - Curves Operations Module

Blender 曲线操作符，用于创建和编辑曲线对象。

## Overview

`bpy.ops.curves` 模块包含曲线对象的操作命令，支持曲线笔刷、曲线编辑和曲线属性设置。这个模块主要用于 Grease Pencil 和曲线绘制。

## 核心功能

```python
import bpy

# 曲线选择
bpy.ops.curves.select_all(action='SELECT')
bpy.ops.curves.select_less()
bpy.ops.curves.select_more()

# 曲线编辑
bpy.ops.curves.subdivide()
bpy.ops.curves.delete()
bpy.ops.curves.duplicate()

# 曲线笔刷
bpy.ops.curves.draw()
bpy.ops.curves.stroke(
    stroke=[{"name": "Empty", "location": (0, 0, 0), "pressure": 1, "time": 0}]
)
```

## 实际应用示例

### 曲线管理器

```python
import bpy
import math

class CurvesManager:
    """曲线管理器"""
    
    def __init__(self):
        self.curves = {}
    
    def select_all_curves(self, action='SELECT'):
        """选择所有曲线"""
        bpy.ops.curves.select_all(action=action)
    
    def deselect_all(self):
        """取消选择"""
        self.select_all_curves(action='DESELECT')
    
    def select_more(self):
        """扩大选择"""
        bpy.ops.curves.select_more()
    
    def select_less(self):
        """缩小选择"""
        bpy.ops.curves.select_less()
    
    def select_linked(self):
        """选择连接"""
        bpy.ops.curves.select_linked()
    
    def select_random(self, percent=50, seed=0):
        """随机选择"""
        bpy.ops.curves.select_random(percent=percent, seed=seed)
    
    def subdivide_curves(self, number_of_cuts=1):
        """细分曲线"""
        bpy.ops.curves.subdivide(number_of_cuts=number_of_cuts)
    
    def delete_curves(self, type='SELECTED'):
        """删除曲线"""
        bpy.ops.curves.delete(type=type)
    
    def delete_points(self, type='POINTS'):
        """删除点"""
        bpy.ops.curves.delete(type=type)
    
    def duplicate_curves(self):
        """复制曲线"""
        bpy.ops.curves.duplicate()
    
    def duplicate_linked(self):
        """复制连接"""
        bpy.ops.curves.duplicate_linked()
    
    def merge_points(self, distance=0.1, mode='AT_LAST'):
        """合并点"""
        bpy.ops.curves.merge_points(distance=distance, mode=mode)
    
    def select_first(self):
        """选择第一个"""
        bpy.ops.curves.select_first()
    
    def select_last(self):
        """选择最后一个"""
        bpy.ops.curves.select_last()
    
    def set_selection_radius(self, radius=0.1):
        """设置选择半径"""
        bpy.ops.curves.selection_set(radius=radius)


def setup_curves_manager():
    """设置曲线管理器"""
    manager = CurvesManager()
    
    print("曲线管理器已设置")
    print("可用功能:")
    print("- select_all_curves: 全选")
    print("- subdivide_curves: 细分")
    print("- merge_points: 合并点")
    
    return manager
```

### 曲线笔刷工具

```python
import bpy

class CurvesBrushTool:
    """曲线笔刷工具"""
    
    def __init__(self):
        self.brushes = {}
    
    def set_brush_size(self, size):
        """设置笔刷大小"""
        bpy.context.scene.tool_settings.curves_paint.brush.size = size
    
    def set_brush_strength(self, strength):
        """设置笔刷强度"""
        bpy.context.scene.tool_settings.curves_paint.brush.strength = strength
    
    def set_brush_color(self, color):
        """设置笔刷颜色"""
        bpy.context.scene.tool_settings.curves_paint.brush.color = color
    
    def set_curve_mode(self, mode='ADD'):
        """设置曲线模式"""
        bpy.context.scene.tool_settings.curves_paint.mode = mode
    
    def use_curve_sculpting(self, use=True):
        """启用曲线雕刻"""
        bpy.context.scene.tool_settings.use_curves_sculpt = use
    
    def apply_brush_stroke(self, stroke_points):
        """应用笔刷笔触"""
        bpy.ops.curves.stroke(
            stroke=stroke_points,
            mode='REPLACE'
        )
    
    def draw_curve(self, start_point, end_point):
        """绘制曲线"""
        stroke = [
            {"name": "Empty", "location": start_point, "pressure": 1, "time": 0},
            {"name": "Empty", "location": end_point, "pressure": 1, "time": 1}
        ]
        
        self.apply_brush_stroke(stroke)
    
    def create_smooth_curve(self, points, smooth_strength=1.0):
        """创建平滑曲线"""
        for i, point in enumerate(points):
            if i == 0:
                continue
            
            stroke = [
                {"name": "Empty", "location": points[i-1], "pressure": 1, "time": 0},
                {"name": "Empty", "location": point, "pressure": 1, "time": 1}
            ]
            
            bpy.ops.curves.stroke(
                stroke=stroke,
                mode='SMOOTH'
            )
    
    def create_thick_curve(self, points, thickness):
        """创建粗曲线"""
        for i, point in enumerate(points):
            if i == 0:
                continue
            
            stroke = [
                {"name": "Empty", "location": points[i-1], "pressure": thickness, "time": 0},
                {"name": "Empty", "location": point, "pressure": thickness, "time": 1}
            ]
            
            bpy.ops.curves.stroke(
                stroke=stroke,
                mode='REPLACE'
            )
    
    def create_tapered_curve(self, points, start_thickness=1.0, end_thickness=0.1):
        """创建锥形曲线"""
        for i, point in enumerate(points):
            if i == 0:
                continue
            
            t = i / len(points)
            thickness = start_thickness + (end_thickness - start_thickness) * t
            
            stroke = [
                {"name": "Empty", "location": points[i-1], "pressure": thickness, "time": 0},
                {"name": "Empty", "location": point, "pressure": thickness, "time": 1}
            ]
            
            bpy.ops.curves.stroke(
                stroke=stroke,
                mode='REPLACE'
            )
    
    def smooth_selected_curves(self, strength=1.0):
        """平滑选中曲线"""
        bpy.ops.curves.smooth(strength=strength)
    
    def thin_selected_curves(self, factor=0.5):
        """变细选中曲线"""
        bpy.ops.curves.thin(factor=factor)
    
    def thick_selected_curves(self, factor=2.0):
        """变粗选中曲线"""
        bpy.ops.curves.thin(factor=factor, inverse=True)


def setup_brush_tool():
    """设置笔刷工具"""
    tool = CurvesBrushTool()
    
    print("曲线笔刷工具已设置")
    print("可用功能:")
    print("- set_brush_size: 设置大小")
    print("- apply_brush_stroke: 应用笔触")
    print("- smooth_selected_curves: 平滑")
    
    return tool
```

### 曲线动画工具

```python
import bpy

class CurvesAnimationTool:
    """曲线动画工具"""
    
    def __init__(self):
        self.keyframes = {}
    
    def insert_keyframe(self, data_path, index=-1, frame=None):
        """插入关键帧"""
        if frame is None:
            frame = bpy.context.scene.frame_current
        
        bpy.ops.anim.keyframe_insert(data_path=data_path, type='DEFAULT', index=index)
        bpy.context.scene.frame_set(frame)
    
    def animate_curve_thickness(self, curve_name, keyframes):
        """动画曲线粗细"""
        curve = bpy.data.objects.get(curve_name)
        if not curve:
            return
        
        for frame, thickness in keyframes.items():
            bpy.context.scene.frame_set(frame)
            curve.data.bevel_depth = thickness
            curve.data.keyframe_insert(data_path='bevel_depth', frame=frame)
    
    def animate_curve_color(self, curve_name, keyframes):
        """动画曲线颜色"""
        curve = bpy.data.objects.get(curve_name)
        if not curve:
            return
        
        for frame, color in keyframes.items():
            bpy.context.scene.frame_set(frame)
            curve.data.materials[0].node_tree.nodes["Principled BSDF"].inputs['Base Color'].default_value = color
            curve.data.materials[0].node_tree.nodes["Principled BSDF"].inputs['Base Color'].keyframe_insert(
                data_path='default_value', frame=frame
            )
    
    def create_path_animation(self, curve_name, target_object, frames):
        """创建路径动画"""
        curve = bpy.data.objects.get(curve_name)
        target = bpy.data.objects.get(target_object)
        
        if not curve or not target:
            return
        
        # 添加跟随路径约束
        constraint = target.constraints.new(type='FOLLOW_PATH')
        constraint.target = curve
        constraint.use_curve_follow = True
        
        # 插入关键帧
        for i, frame in enumerate(frames):
            bpy.context.scene.frame_set(frame)
            constraint.offset_factor = i / len(frames)
            constraint.keyframe_insert(data_path='offset_factor', frame=frame)
    
    def animate_along_curve(self, object_name, curve_name, start_frame=1, end_frame=100):
        """沿曲线动画"""
        obj = bpy.data.objects.get(object_name)
        curve = bpy.data.objects.get(curve_name)
        
        if not obj or not curve:
            return
        
        # 创建路径约束
        constraint = obj.constraints.new(type='FOLLOW_PATH')
        constraint.target = curve
        constraint.use_curve_follow = True
        
        # 设置动画
        for frame in range(start_frame, end_frame + 1):
            bpy.context.scene.frame_set(frame)
            constraint.offset_factor = (frame - start_frame) / (end_frame - start_frame)
            constraint.keyframe_insert(data_path='offset_factor', frame=frame)
    
    def create_wiggle_effect(self, curve_name, amplitude=0.1, frequency=1.0, frames=60):
        """创建摆动效果"""
        curve = bpy.data.objects.get(curve_name)
        if not curve:
            return
        
        import math
        
        for frame in range(frames):
            bpy.context.scene.frame_set(frame + 1)
            
            time = frame / 30.0
            
            # 移动曲线点
            for point in curve.data.splines[0].points:
                point.co = (
                    point.co[0] + amplitude * math.sin(frequency * time),
                    point.co[1] + amplitude * math.cos(frequency * time),
                    point.co[2],
                    point.co[3]
                )
            
            curve.data.splines[0].keyframe_insert(data_path='points', frame=frame + 1)
    
    def export_curve_animation(self, curve_name, output_path):
        """导出曲线动画"""
        curve = bpy.data.objects.get(curve_name)
        if not curve:
            return None
        
        # 导出为关键帧数据
        frames = []
        
        for frame in range(
            bpy.context.scene.frame_start,
            bpy.context.scene.frame_end + 1
        ):
            bpy.context.scene.frame_set(frame)
            
            frame_data = {
                'frame': frame,
                'points': []
            }
            
            for point in curve.data.splines[0].points:
                frame_data['points'].append(point.co.copy())
            
            frames.append(frame_data)
        
        # 保存数据
        import json
        
        with open(output_path, 'w') as f:
            json.dump(frames, f, indent=2)
        
        return output_path


def setup_curves_animation():
    """设置曲线动画"""
    tool = CurvesAnimationTool()
    
    print("曲线动画工具已设置")
    print("可用功能:")
    print("- animate_curve_thickness: 粗细动画")
    print("- animate_along_curve: 沿曲线动画")
    print("- create_wiggle_effect: 摆动效果")
    
    return tool
```


---
## bpy_ops_pointcloud

# bpy.ops.pointcloud 模块

点云操作模块，用于处理和操作3D点云数据。

## 概述

bpy.ops.pointcloud 模块包含用于处理点云数据的运算符。点云是由大量离散点组成的三维数据，常用于三维扫描、激光雷达数据和科学可视化等领域。

## 常用操作

### 点云创建

```python
import bpy

# 从网格创建点云
bpy.ops.object.pointcloud_add_from_mesh()

# 添加空点云
bpy.ops.object.pointcloud_add()

# 从文件导入点云
bpy.ops.import_mesh.pointcloud(filepath="path/to/file.ply")
```

### 点云编辑

```python
# 选择所有点
bpy.ops.pointcloud.select_all(action='SELECT')

# 取消选择
bpy.ops.pointcloud.select_all(action='DESELECT')

# 反选
bpy.ops.pointcloud.select_all(action='INVERT')

# 删除选中点
bpy.ops.pointcloud.delete()

# 复制点云
bpy.ops.pointcloud.duplicate()
```

### 点选择

```python
# 框选点
bpy.ops.pointcloud.select_border(gesture_mode=0, xmin=100, xmax=200, ymin=50, ymax=150)

# 选择随机点
bpy.ops.pointcloud.select_random(percent=50)

# 选择法向一致点
bpy.ops.pointcloud.select_normal()

# 选择最近点
bpy.ops.pointcloud.select_nearest()

# 扩展选择
bpy.ops.pointcloud.select_expand()
```

### 点属性

```python
# 设置点颜色
bpy.ops.pointcloud.color_set(color=(1, 0, 0, 1))

# 设置点大小
bpy.ops.pointcloud.set_radius(radius=0.01)

# 设置点法向
bpy.ops.pointcloud.set_normal(vector=(0, 0, 1))

# 重新计算法向
bpy.ops.pointcloud.recalc_normals()
```

### 过滤器

```python
# 应用统计过滤器
bpy.ops.pointcloud.statistical_outlier_filter(mean_k=10, std_dev=2.0)

# 应用半径过滤器
bpy.ops.pointcloud.radius_outlier_filter(min_points=10, radius=0.05)

# 应用Voxel网格过滤器
bpy.ops.pointcloud.voxel_grid_filter(voxel_size=0.01)
```

## 实际应用示例

### 从网格创建点云

```python
def create_pointcloud_from_mesh(mesh_obj, point_count=10000):
    """从网格创建点云"""
    # 选中网格对象
    bpy.context.view_layer.objects.active = mesh_obj
    mesh_obj.select_set(True)
    
    # 创建点云
    bpy.ops.object.pointcloud_add_from_mesh(
        mesh_obj.name,
        point_count=point_count
    )
    
    # 获取点云对象
    pointcloud_obj = bpy.context.active_object
    pointcloud_obj.name = f'{mesh_obj.name}_PointCloud'
    
    return pointcloud_obj
```

### 创建随机点云

```python
def create_random_pointcloud(count=1000, bounds=((-1, -1, -1), (1, 1, 1))):
    """创建随机点云"""
    import random
    
    # 创建空点云对象
    bpy.ops.object.pointcloud_add()
    pc_obj = bpy.context.active_object
    pc_obj.name = 'RandomPointCloud'
    
    # 添加点数据
    pc = pc_obj.data
    pc.points.add(count)
    
    # 设置随机点位置
    for i in range(count):
        x = random.uniform(bounds[0][0], bounds[1][0])
        y = random.uniform(bounds[0][1], bounds[1][1])
        z = random.uniform(bounds[0][2], bounds[1][2])
        pc.points[i].co = (x, y, z)
    
    return pc_obj
```

### 创建球形点云

```python
def create_spherical_pointcloud(radius=1.0, point_count=1000, hollow=False):
    """创建球形点云"""
    import math
    import random
    
    # 创建空点云
    bpy.ops.object.pointcloud_add()
    pc_obj = bpy.context.active_object
    pc_obj.name = 'SphericalPointCloud'
    
    # 添加点数据
    pc = pc_obj.data
    pc.points.add(point_count)
    
    # 生成球面点
    for i in range(point_count):
        # 均匀分布在球面上
        phi = random.uniform(0, 2 * math.pi)
        costheta = random.uniform(-1, 1)
        theta = math.acos(costheta)
        
        if hollow:
            # 实心球体
            r = radius * random.random() ** (1/3)
        else:
            r = radius
        
        x = r * math.sin(theta) * math.cos(phi)
        y = r * math.sin(theta) * math.sin(phi)
        z = r * math.cos(theta)
        
        pc.points[i].co = (x, y, z)
    
    return pc_obj
```

### 创建圆柱形点云

```python
def create_cylindrical_pointcloud(height=2.0, radius=1.0, point_count=1000):
    """创建圆柱形点云"""
    import math
    import random
    
    # 创建空点云
    bpy.ops.object.pointcloud_add()
    pc_obj = bpy.context.active_object
    pc_obj.name = 'CylindricalPointCloud'
    
    # 添加点数据
    pc = pc_obj.data
    pc.points.add(point_count)
    
    for i in range(point_count):
        theta = random.uniform(0, 2 * math.pi)
        r = radius * math.sqrt(random.random())
        z = random.uniform(-height/2, height/2)
        
        x = r * math.cos(theta)
        y = r * math.sin(theta)
        
        pc.points[i].co = (x, y, z)
    
    return pc_obj
```

### 点云着色

```python
def color_pointcloud_by_height(pc_obj):
    """根据高度着色点云"""
    pc = pc_obj.data
    
    # 获取高度范围
    min_z = min(p.co[2] for p in pc.points)
    max_z = max(p.co[2] for p in pc.points)
    z_range = max_z - min_z or 1
    
    # 设置颜色渐变
    for point in pc.points:
        z = point.co[2]
        ratio = (z - min_z) / z_range
        
        # 蓝色到红色渐变
        point.color = (
            ratio,           # R
            0,               # G
            1 - ratio,       # B
            1.0              # A
        )
```

### 点云着色

```python
def color_pointcloud_by_distance(pc_obj, center=(0, 0, 0)):
    """根据距离着色点云"""
    pc = pc_obj.data
    
    # 计算最大距离
    max_dist = 0
    for point in pc.points:
        dist = ((point.co[0] - center[0])**2 +
                (point.co[1] - center[1])**2 +
                (point.co[2] - center[2])**2)**0.5
        max_dist = max(max_dist, dist)
    
    max_dist = max_dist or 1
    
    # 设置颜色渐变
    for point in pc.points:
        dist = ((point.co[0] - center[0])**2 +
                (point.co[1] - center[1])**2 +
                (point.co[2] - center[2])**2)**0.5
        ratio = dist / max_dist
        
        point.color = (
            ratio,
            1 - ratio,
            0.5,
            1.0
        )
```

### 点云下采样

```python
def downsample_pointcloud(pc_obj, ratio=0.1):
    """点云下采样"""
    pc = pc_obj.data
    
    # 随机选择保留的点
    import random
    total_points = len(pc.points)
    keep_count = int(total_points * ratio)
    
    keep_indices = set(random.sample(range(total_points), keep_count))
    
    # 删除其他点
    for i in range(total_points - 1, -1, -1):
        if i not in keep_indices:
            pc.points.remove(pc.points[i])
```

### 点云过滤

```python
def filter_points_by_bounds(pc_obj, bounds_min, bounds_max):
    """根据边界过滤点云"""
    pc = pc_obj.data
    
    for i in range(len(pc.points) - 1, -1, -1):
        point = pc.points[i]
        
        # 检查是否在边界内
        if (point.co[0] < bounds_min[0] or point.co[0] > bounds_max[0] or
            point.co[1] < bounds_min[1] or point.co[1] > bounds_max[1] or
            point.co[2] < bounds_min[2] or point.co[2] > bounds_max[2]):
            pc.points.remove(point)
```

### 点云转换为网格

```python
def pointcloud_to_mesh(pc_obj):
    """点云转换为网格"""
    # 选中点云对象
    bpy.context.view_layer.objects.active = pc_obj
    pc_obj.select_set(True)
    
    # 转换为网格
    bpy.ops.object.convert(target='MESH')
    
    # 返回新网格对象
    return bpy.context.active_object
```

### 创建点云动画

```python
def create_pointcloud_animation(pc_obj, frame_count=60):
    """创建点云动画"""
    pc = pc_obj.data
    
    for frame in range(frame_count):
        # 设置当前帧
        bpy.context.scene.frame_set(frame)
        
        # 更新点位置
        for i, point in enumerate(pc.points):
            # 简单的波动效果
            x = point.co.x + 0.01 * math.sin(frame * 0.1 + i * 0.1)
            y = point.co.y + 0.01 * math.cos(frame * 0.1 + i * 0.1)
            point.co = (x, y, point.co.z)
        
        # 插入关键帧
        pc.points.foreach_keyframe_insert(data_path='co', frame=frame)
```

### 点云配准模拟

```python
def simulate_pointcloud_registration(pc_static, pc_moving, steps=10):
    """模拟点云配准"""
    # 初始变换
    current_matrix = pc_moving.matrix_world.copy()
    
    for step in range(steps):
        # 计算变换（简化版）
        bpy.context.scene.frame_set(step)
        
        # 简单的插值变换
        progress = step / steps
        new_matrix = current_matrix.copy()
        
        # 这里应该是实际的配准算法
        # 简化示例：逐渐对齐到静态点云位置
        new_matrix.translation = current_matrix.translation * (1 - progress)
        
        # 应用变换
        pc_moving.matrix_world = new_matrix
        
        # 插入关键帧
        pc_moving.matrix_world.keyframe_insert(data_path='location', frame=step)
        pc_moving.matrix_world.keyframe_insert(data_path='rotation_euler', frame=step)
        pc_moving.matrix_world.keyframe_insert(data_path='scale', frame=step)
```

## 使用场景

- **3D扫描数据处理**：处理激光扫描获得的点云数据
- **激光雷达可视化**：处理和可视化LiDAR数据
- **科学数据可视化**：显示三维测量数据
- **计算机视觉**：点云分割和识别
- **逆向工程**：从实物创建数字模型
- **地形建模**：创建地形表面点云
- **建筑测量**：建筑和基础设施测量
- **文化遗产保护**：文物数字化记录

## 注意事项

- 点云数据量可能很大，注意内存使用
- 渲染大量点可能影响性能
- 正确的点大小设置对可视化很重要
- 法向计算影响光照效果
- 定期保存工作进度
- 导入导出时注意文件格式
- 大规模点云需要分块处理
- 注意数据精度和噪声处理
- 配准前需要预处理数据
- 选择合适的过滤器优化数据


---
## bpy_ops_uv

# bpy.ops.uv - UV Operations Module

Blender UV 操作符，用于在 UV 编辑器中管理和操作网格的 UV 坐标。

## Overview

`bpy.ops.uv` 模块包含 UV 编辑器中的所有操作命令，涵盖 UV 的选择、编辑、对齐、投影等核心功能。这个模块主要用于纹理映射和 UV 展开的管理。

## 核心功能

### UV 选择操作

```python
import bpy

# 全选 UV
bpy.ops.uv.select_all(action='SELECT')

# 全不选
bpy.ops.uv.select_all(action='DESELECT')

# 反选
bpy.ops.uv.select_all(action='INVERT')

# 选择相似
bpy.ops.uv.select_similar(type='AREA', threshold=0.01)
bpy.ops.uv.select_similar(type='LENGTH', threshold=0.01)
bpy.ops.uv.select_similar(type='PERIMETER', threshold=0.01)

# 选择循环
bpy.ops.uv.select_loop()

# 选择边环
bpy.ops.uv.select_edge_ring()

# 边框选择
bpy.ops.uv.select_border(gesture_mode=0, xmin=100, xmax=300, ymin=100, ymax=300)

# 圆形选择
bpy.ops.uv.select_circle(x=200, y=200, radius=50)

# 套索选择
bpy.ops.uv.select_lasso(path=[(100, 100), (200, 100), (200, 200), (100, 200)])

# 取消选择连接的
bpy.ops.uv.select_less()

# 扩大选择
bpy.ops.uv.select_more()

# 同步选择
bpy.ops.uv.select_sync_from_mode()

def select_uv_by_material_slot(material_slot_name):
    """按材质槽选择 UV"""
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    obj = bpy.context.active_object
    for i, slot in enumerate(obj.material_slots):
        if slot.name == material_slot_name:
            bpy.ops.object.material_slot_select(slot_index=i)
            break

def select_uv_faces_by_size(threshold=0.01):
    """选择小于阈值的 UV 面"""
    bpy.ops.uv.select_all(action='SELECT')
    bpy.ops.uv.select_similar(type='AREA', threshold=threshold)
    bpy.ops.uv.select_all(action='INVERT')
```

### UV 编辑操作

```python
# 移动 UV
bpy.ops.uv.translate(delta_x=0.1, delta_y=0.1)

# 旋转 UV
bpy.ops.uv.rotate(value=0.785398)  # 45度

# 缩放 UV
bpy.ops.uv.scale(value=1.5)

# 剪切 UV
bpy.ops.uv.cut(cut_through=True)

# 复制 UV
bpy.ops.uv.duplicate()

# 粘贴 UV
bpy.ops.uv.paste()

# 清除 UV
bpy.ops.uv.delete(selected=True, type='VERTEX')

# 排列 UV
bpy.ops.uv.align(axis='X')
bpy.ops.uv.align(axis='Y')

# 修正 UV 宽高比
bpy.ops.uv.reveal(select=True)

# 最小化拉伸
bpy.ops.uv.minimize_stretch(iterations=10)

# 焊接 UV 顶点
bpy.ops.uv.weld()

# 合并 UV 顶点
bpy.ops.uv.stitch(use_limit=False, limit=0.01, static_thresh=0.1)

# 细分 UV
bpy.ops.uv.subdivide()

# 排序 UV
bpy.ops.uv.sort(type='TOP_TO_BOTTOM')
bpy.ops.uv.sort(type='LEFT_TO_RIGHT')

def move_uv_selection(x, y):
    """移动选中的 UV"""
    bpy.ops.uv.translate(delta_x=x, delta_y=y)

def rotate_uv_90_degrees(direction='CW'):
    """旋转 UV 90 度"""
    if direction == 'CW':
        bpy.ops.uv.rotate(value=-1.5708)  # -90度
    else:
        bpy.ops.uv.rotate(value=1.5708)   # 90度

def scale_uv_uniform(factor):
    """统一缩放 UV"""
    bpy.ops.uv.scale(value=factor)
```

### UV 投影操作

```python
# 智能 UV 投影
bpy.ops.uv.smart_project(
    angle_limit=66,
    island_margin=0.08,
    area_weight=0.75,
    correct_aspect=True
)

# 立方体投影
bpy.ops.uv.cube_project(
    cube_size=1,
    gap=0.02
)

# 圆柱投影
bpy.ops.uv.cylinder_project(
    direction='ALIGN_TO_OBJECT',
    align_pole='UP',
    area_weight=0.75
)

# 球形投影
bpy.ops.uv.sphere_project(
    direction='ALIGN_TO_OBJECT',
    area_weight=0.75
)

# 从视图投影
bpy.ops.uv.project_from_view(
    scale_to_bounds=False,
    correct_aspect=True
)

# 遵循活动面
bpy.ops.uv.follow_active_quads(mode='LENGTH_AVERAGE')

def smart_project_selected(angle=66, margin=0.08):
    """对选中的面进行智能投影"""
    bpy.ops.mesh.select_all(action='DESELECT')
    bpy.ops.object.mode_set(mode='EDIT')
    
    bpy.ops.uv.smart_project(
        angle_limit=angle,
        island_margin=margin,
        area_weight=0.75,
        correct_aspect=True
    )

def project_from_view_scaled():
    """从视图投影并缩放到边界"""
    bpy.ops.uv.project_from_view(
        scale_to_bounds=True,
        correct_aspect=True
    )
```

### UV 展开操作

```python
# 展开
bpy.ops.uv.unwrap(
    method='ANGLE_BASED',
    fill_holes=True,
    correct_aspect=True,
    use_subsurf_data=False,
    margin=0.001
)

# 收缩包裹
bpy.ops.uv.shrinkwrap(
    target=bpy.data.objects['TargetObject'],
    offset=0.001
)

def unwrap_with_margin(margin=0.05):
    """带边距展开"""
    bpy.ops.uv.unwrap(
        method='ANGLE_BASED',
        fill_holes=True,
        correct_aspect=True,
        margin=margin
    )

def reset_uv():
    """重置 UV"""
    bpy.ops.uv.reset()
```

## 实际应用示例

### UV 布局管理器

```python
import bpy
import math

class UVLayoutManager:
    """UV 布局管理器"""
    
    def __init__(self, texel_density=1024):
        self.texel_density = texel_density
    
    def get_uv_area(self, uv_layer):
        """计算 UV 面积"""
        mesh = bpy.context.active_object.data
        area = 0
        
        for poly in mesh.polygons:
            if not poly.select:
                continue
            
            verts = [mesh.vertices[v].co for v in poly.vertices]
            
            if len(verts) == 4:
                # 四边形分解为两个三角形
                area += self._triangle_area(verts[0], verts[1], verts[2])
                area += self._triangle_area(verts[0], verts[2], verts[3])
            else:
                # 三角形
                area += self._triangle_area(verts[0], verts[1], verts[2])
        
        return area
    
    def _triangle_area(self, a, b, c):
        """计算三角形面积"""
        ab = (b[0] - a[0], b[1] - a[1], b[2] - a[2])
        ac = (c[0] - a[0], c[1] - a[1], c[2] - a[2])
        
        cross = (
            ab[1] * ac[2] - ab[2] * ac[1],
            ab[2] * ac[0] - ab[0] * ac[2],
            ab[0] * ac[1] - ab[1] * ac[0]
        )
        
        return 0.5 * math.sqrt(sum(x**2 for x in cross))
    
    def calculate_texel_density(self, obj, uv_layer_name="UVMap"):
        """计算纹理密度"""
        mesh = obj.data
        uv_layer = mesh.uv_layers[uv_layer_name]
        
        total_uv_area = 0
        total_mesh_area = 0
        
        for poly in mesh.polygons:
            uv_area = 0
            mesh_area = 0
            
            verts = [mesh.vertices[v].co for v in poly.vertices]
            
            # 计算网格面积
            if len(verts) == 4:
                mesh_area = self._triangle_area(verts[0], verts[1], verts[2])
                mesh_area += self._triangle_area(verts[0], verts[2], verts[3])
            else:
                mesh_area = self._triangle_area(verts[0], verts[1], verts[2])
            
            # 计算 UV 面积
            uvs = [uv_layer.data[loop].uv for loop in poly.loop_indices]
            if len(uvs) == 4:
                uv_area = self._triangle_area_uv(uvs[0], uvs[1], uvs[2])
                uv_area += self._triangle_area_uv(uvs[0], uvs[2], uvs[3])
            else:
                uv_area = self._triangle_area_uv(uvs[0], uvs[1], uvs[2])
            
            total_uv_area += uv_area
            total_mesh_area += mesh_area
        
        # 计算纹理密度 (pixels per unit)
        texture_size = self.texel_density
        texel_density = math.sqrt(uv_area / mesh_area) * texture_size if mesh_area > 0 else 0
        
        return texel_density
    
    def _triangle_area_uv(self, a, b, c):
        """计算 UV 三角形面积"""
        ab = (b[0] - a[0], b[1] - a[1])
        ac = (c[0] - a[0], c[1] - a[1])
        
        return 0.5 * abs(ab[0] * ac[1] - ab[1] * ac[0])
    
    def normalize_texel_density(self, target_density=1024):
        """标准化纹理密度"""
        obj = bpy.context.active_object
        
        # 获取当前密度
        current_density = self.calculate_texel_density(obj)
        
        if current_density > 0:
            scale_factor = target_density / current_density
            bpy.ops.uv.scale(value=math.sqrt(scale_factor))
    
    def pack_uv_islands(self, margin=0.02, rotate=True, scale=True):
        """打包 UV 岛屿"""
        bpy.ops.uv.select_all(action='SELECT')
        bpy.ops.uv.pack_islands(
            margin=margin,
            rotate=rotate,
            scale=scale
        )
    
    def export_uv_layout(self, filepath, resolution=(2048, 2048)):
        """导出 UV 布局"""
        # 创建渲染设置
        bpy.ops.object.mode_set(mode='OBJECT')
        
        scene = bpy.context.scene
        old_engine = scene.render.engine
        scene.render.engine = 'CYCLES'
        
        # 设置渲染分辨率
        scene.render.resolution_x = resolution[0]
        scene.render.resolution_y = resolution[1]
        
        # 启用 UV 同步选择
        bpy.context.scene.tool_settings.use_uv_select_sync = True
        
        # 渲染 UV 布局
        scene.render.filepath = filepath
        bpy.ops.render.render(write_still=True)
        
        # 恢复设置
        scene.render.engine = old_engine
        
        return filepath
    
    def layout_uv_grid(self, cols=4):
        """将 UV 排列成网格"""
        mesh = bpy.context.active_object.data
        
        # 获取所有 UV 面
        uv_layer = mesh.uv_layers.active
        faces = [poly for poly in mesh.polygons if poly.select]
        
        # 排列
        for i, poly in enumerate(faces):
            row = i // cols
            col = i % cols
            
            # 移动 UV 到网格位置
            for loop_idx in poly.loop_indices:
                uv = uv_layer.data[loop_idx].uv
                uv[0] = col + uv[0] / cols
                uv[1] = 1 - row - uv[1] / cols


def setup_uv_layout():
    """设置 UV 布局"""
    manager = UVLayoutManager(texel_density=1024)
    
    # 智能投影
    bpy.ops.uv.smart_project(
        angle_limit=66,
        island_margin=0.08,
        area_weight=0.75,
        correct_aspect=True
    )
    
    # 打包
    manager.pack_uv_islands(margin=0.02)
    
    return manager
```

### UV 检查工具

```python
import bpy
import math

class UVChecker:
    """UV 检查工具"""
    
    def check_overlapping_faces(self):
        """检查重叠面"""
        mesh = bpy.context.active_object.data
        uv_layer = mesh.uv_layers.active
        
        overlapping = []
        
        for i, poly1 in enumerate(mesh.polygons):
            if not poly1.select:
                continue
            
            uv1 = [uv_layer.data[loop].uv for loop in poly1.loop_indices]
            
            for j, poly2 in enumerate(mesh.polygons):
                if i >= j or not poly2.select:
                    continue
                
                uv2 = [uv_layer.data[loop].uv for loop in poly2.loop_indices]
                
                if self._uvs_overlap(uv1, uv2):
                    overlapping.append((i, j))
        
        # 高亮重叠面
        bpy.ops.uv.select_all(action='DESELECT')
        for i, j in overlapping:
            mesh.polygons[i].select = True
            mesh.polygons[j].select = True
        
        return overlapping
    
    def _uvs_overlap(self, uvs1, uvs2):
        """检查 UV 是否重叠"""
        bounds1 = self._get_uv_bounds(uvs1)
        bounds2 = self._get_uv_bounds(uvs2)
        
        # 简单的边界框检查
        return not (
            bounds1[0] >= bounds2[2] or
            bounds1[2] <= bounds2[0] or
            bounds1[1] >= bounds2[3] or
            bounds1[3] <= bounds2[1]
        )
    
    def _get_uv_bounds(self, uvs):
        """获取 UV 边界"""
        min_u = min(uv[0] for uv in uvs)
        max_u = max(uv[0] for uv in uvs)
        min_v = min(uv[1] for uv in uvs)
        max_v = max(uv[1] for uv in uvs)
        
        return (min_u, min_v, max_u, max_v)
    
    def check_stretched_faces(self, threshold=0.1):
        """检查拉伸面"""
        mesh = bpy.context.active_object.data
        uv_layer = mesh.uv_layers.active
        
        stretched = []
        
        for poly in mesh.polygons:
            if not poly.select:
                continue
            
            # 获取 UV 和 3D 面积比
            uvs = [uv_layer.data[loop].uv for loop in poly.loop_indices]
            verts = [mesh.vertices[v].co for v in poly.vertices]
            
            uv_area = self._triangle_area_uv(uvs[0], uvs[1], uvs[2])
            mesh_area = self._triangle_area_verts(verts[0], verts[1], verts[2])
            
            if mesh_area > 0:
                ratio = uv_area / mesh_area
                if ratio < threshold:
                    stretched.append(poly)
        
        return stretched
    
    def _triangle_area_uv(self, a, b, c):
        """计算 UV 三角形面积"""
        ab = (b[0] - a[0], b[1] - a[1])
        ac = (c[0] - a[0], c[1] - a[1])
        return 0.5 * abs(ab[0] * ac[1] - ab[1] * ac[0])
    
    def _triangle_area_verts(self, a, b, c):
        """计算 3D 三角形面积"""
        ab = (b[0] - a[0], b[1] - a[1], b[2] - a[2])
        ac = (c[0] - a[0], c[1] - a[1], c[2] - a[2])
        
        cross = (
            ab[1] * ac[2] - ab[2] * ac[1],
            ab[2] * ac[0] - ab[0] * ac[2],
            ab[0] * ac[1] - ab[1] * ac[0]
        )
        
        return 0.5 * math.sqrt(sum(x**2 for x in cross))
    
    def check_uv_bounds(self):
        """检查 UV 边界"""
        mesh = bpy.context.active_object.data
        uv_layer = mesh.uv_layers.active
        
        min_u = float('inf')
        max_u = float('-inf')
        min_v = float('inf')
        max_v = float('-inf')
        
        for poly in mesh.polygons:
            for loop_idx in poly.loop_indices:
                uv = uv_layer.data[loop_idx].uv
                min_u = min(min_u, uv[0])
                max_u = max(max_u, uv[0])
                min_v = min(min_v, uv[1])
                max_v = max(max_v, uv[1])
        
        return {
            'min_u': min_u,
            'max_u': max_u,
            'min_v': min_v,
            'max_v': max_v,
            'width': max_u - min_u,
            'height': max_v - min_v,
            'total_area': (max_u - min_u) * (max_v - min_v)
        }
    
    def select_distorted_faces(self, distortion_threshold=1.5):
        """选择变形面"""
        mesh = bpy.context.active_object.data
        uv_layer = mesh.uv_layers.active
        
        bpy.ops.uv.select_all(action='DESELECT')
        
        for poly in mesh.polygons:
            if not poly.select:
                continue
            
            uvs = [uv_layer.data[loop].uv for loop in poly.loop_indices]
            verts = [mesh.vertices[v].co for v in poly.vertices]
            
            uv_area = self._triangle_area_uv(uvs[0], uvs[1], uvs[2])
            mesh_area = self._triangle_area_verts(verts[0], verts[1], verts[2])
            
            if mesh_area > 0:
                ratio = mesh_area / uv_area if uv_area > 0 else float('inf')
                if ratio > distortion_threshold:
                    poly.select = True


def check_uv_quality():
    """检查 UV 质量"""
    checker = UVChecker()
    
    # 检查重叠
    overlapping = checker.check_overlapping_faces()
    print(f"发现 {len(overlapping)} 对重叠面")
    
    # 检查拉伸
    stretched = checker.check_stretched_faces(threshold=0.05)
    print(f"发现 {len(stretched)} 个拉伸面")
    
    # 检查边界
    bounds = checker.check_uv_bounds()
    print(f"UV 边界: U [{bounds['min_u']:.2f}, {bounds['max_u']:.2f}], "
          f"V [{bounds['min_v']:.2f}, {bounds['max_v']:.2f}]")
    
    return {
        'overlapping': overlapping,
        'stretched': stretched,
        'bounds': bounds
    }
```

### UV 自动化工具

```python
import bpy

class UVAutomationTool:
    """UV 自动化工具"""
    
    def setup_texture_atlas(self, objects, atlas_size=4096, padding=16):
        """创建纹理图集"""
        # 收集所有 UV
        all_uvs = []
        total_area = 0
        
        for obj in objects:
            if obj.type != 'MESH':
                continue
            
            mesh = obj.data
            
            for poly in mesh.polygons:
                uv_area = 0
                for loop_idx in poly.loop_indices:
                    uv = mesh.uv_layers.active.data[loop_idx].uv
                    all_uvs.append(uv)
        
        # 计算每个对象的 UV 区域
        object_areas = {}
        for obj in objects:
            if obj.type != 'MESH':
                continue
            
            mesh = obj.data
            area = sum(poly.area for poly in mesh.polygons)
            object_areas[obj.name] = area
        
        total_area = sum(object_areas.values())
        
        # 计算缩放比例
        scale_factor = math.sqrt(total_area) / atlas_size * (1 - padding / atlas_size)
        
        # 重新缩放 UV
        for obj in objects:
            if obj.type != 'MESH':
                continue
            
            mesh = obj.data
            
            for poly in mesh.polygons:
                for loop_idx in poly.loop_indices:
                    uv = mesh.uv_layers.active.data[loop_idx].uv
                    uv[0] *= scale_factor
                    uv[1] *= scale_factor
    
    def batch_unwrap(self, objects, method='smart', **kwargs):
        """批量展开"""
        for obj in objects:
            if obj.type != 'MESH':
                continue
            
            bpy.context.view_layer.objects.active = obj
            bpy.ops.object.mode_set(mode='EDIT')
            bpy.ops.mesh.select_all(action='SELECT')
            
            if method == 'smart':
                bpy.ops.uv.smart_project(**kwargs)
            elif method == 'cube':
                bpy.ops.uv.cube_project(**kwargs)
            elif method == 'cylinder':
                bpy.ops.uv.cylinder_project(**kwargs)
            elif method == 'sphere':
                bpy.ops.uv.sphere_project(**kwargs)
            
            bpy.ops.object.mode_set(mode='OBJECT')
    
    def transfer_uv_from_object(self, source, target):
        """从源对象传递 UV 到目标对象"""
        bpy.context.view_layer.objects.active = target
        
        # 确保两个对象都在编辑模式
        bpy.ops.object.mode_set(mode='EDIT')
        
        # 选择源对象
        bpy.ops.mesh.select_all(action='DESELECT')
        source.select_set(True)
        
        # 传递 UV
        bpy.ops.object.join_uvs()
        
        # 分离
        bpy.ops.mesh.separate(type='SELECTED')
    
    def create_uv_template(self, obj, output_path):
        """创建 UV 模板"""
        # 临时设置所有面为单一材质
        temp_mat = bpy.data.materials.new(name="UVTemplate")
        temp_mat.use_nodes = True
        nodes = temp_mat.node_tree.nodes
        
        # 创建纯色材质
        principled = nodes.get('Principled BSDF')
        if principled:
            principled.inputs['Base Color'].default_value = (1, 1, 1, 1)
        
        # 应用材质
        if obj.data.materials:
            obj.data.materials[0] = temp_mat
        else:
            obj.data.materials.append(temp_mat)
        
        # 导出 UV 布局
        manager = UVLayoutManager()
        manager.export_uv_layout(output_path, resolution=(4096, 4096))
        
        # 恢复
        bpy.data.materials.remove(temp_mat)


def automate_uv_workflow(objects):
    """自动化 UV 工作流程"""
    tool = UVAutomationTool()
    
    # 批量展开
    tool.batch_unwrap(
        objects,
        method='smart',
        angle_limit=75,
        island_margin=0.03,
        area_weight=1.0,
        correct_aspect=True
    )
    
    # 选择并优化
    checker = UVChecker()
    checker.select_distorted_faces(distortion_threshold=2.0)
    
    # 重新展开选中的面
    bpy.context.active_object
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.uv.unwrap(
        method='ANGLE_BASED',
        fill_holes=True,
        correct_aspect=True,
        margin=0.05
    )
    
    # 打包
    manager = UVLayoutManager()
    manager.pack_uv_islands(margin=0.02)
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return True
```


---
## bpy_ops_mask

# bpy.ops.mask 模块

遮罩操作模块，用于创建和管理图像合成遮罩。

## 概述

bpy.ops.mask 模块包含用于在遮罩编辑器中创建和编辑遮罩的运算符。遮罩用于在合成阶段隔离图像的特定区域，支持形状绘制、动画和羽化效果。

## 常用操作

### 遮罩创建

```python
import bpy

# 创建新遮罩
bpy.ops.mask.new(name='MyMask')

# 从选中图像创建遮罩
bpy.ops.mask.new(name='NewMask')

# 复制遮罩
bpy.ops.mask.duplicate(mask='MyMask')
```

### 遮罩选择

```python
# 选择遮罩
bpy.ops.mask.select(mask='MyMask', extend=False)

# 扩展选择
bpy.ops.mask.select(mask='MyMask', extend=True)

# 选择所有
bpy.ops.mask.select_all(action='SELECT')

# 取消选择
bpy.ops.mask.select_all(action='DESELECT')

# 反选
bpy.ops.mask.select_all(action='INVERT')
```

### 图层操作

```python
# 添加图层
bpy.ops.mask.layer_new(name='NewLayer')

# 删除图层
bpy.ops.mask.layer_remove()

# 重命名图层
bpy.ops.mask.layer_rename(name='RenamedLayer')

# 设置活动图层
bpy.ops.mask.layer_set_active(name='LayerName')
```

### 样条操作

```python
# 添加新样条
bpy.ops.mask.primitive_add()

# 添加椭圆
bpy.ops.mask.primitive_add(type='ELLIPSE')

# 添加矩形
bpy.ops.mask.primitive_add(type='BOX')

# 添加直线
bpy.ops.mask.primitive_add(type='LINE')

# 关闭样条
bpy.ops.mask.cyclic_toggle(direction='CYCLIC_U')

# 切换方向
bpy.ops.mask.cyclic_toggle(direction='CYCLIC_V')

# 细分样条
bpy.ops.mask.subdivide(number_cuts=1)

# 合并样条
bpy.ops.mask.merge(type='LAST')

# 分割样条
bpy.ops.mask.split()
```

### 点操作

```python
# 选择点
bpy.ops.mask.select_border(extension=False)

# 选择更多点
bpy.ops.mask.select_more()

# 选择更少点
bpy.ops.mask.select_less()

# 添加点
bpy.ops.mask.cyclic_toggle(direction='CYCLIC_U')

# 删除点
bpy.ops.mask.delete(type='POINTS')

# 滑动点
bpy.ops.mask.slide(point='EVEN')

# 滑动长度
bpy.ops.mask.slide(length='EVEN')
```

### 形状操作

```python
# 插值形状
bpy.ops.mask.shape_key_insert()

# 清除形状键
bpy.ops.mask.shape_key_clear()

# 插入形状关键帧
bpy.ops.mask.shape_key_feather_insert()

# 羽化插值
bpy.ops.mask.feather_weight_clear()

# 对称
bpy.ops.mask.symmetrize(direction='NEGATIVE_X')

# 最小化距离
bpy.ops.mask.minimum_distance()

# 转换到线条
bpy.ops.mask.convert(type='OUTLINE')

# 转换到羽化
bpy.ops.mask.convert(type='RADIUS_ADVANCED')
```

### 动画操作

```python
# 插入关键帧
bpy.ops.mask.keyframe_insert(type='ALL')

# 删除关键帧
bpy.ops.mask.keyframe_delete(type='ALL')

# 清除关键帧
bpy.ops.mask.keyframe_clear(type='ALL')

# 跳转到关键帧
bpy.ops.mask.frame_jump()

# 插入形状关键帧
bpy.ops.mask.shape_key_insert()
```

### 视口操作

```python
# 放大
bpy.ops.mask.zoom_in()

# 缩小
bpy.ops.mask.zoom_out()

# 平移
bpy.ops.mask.view_pan()

# 居中
bpy.ops.mask.view_all()

# 翻转
bpy.ops.mask.flip()

# 旋转
bpy.ops.mask.rotate(value=0.785398)
```

## 实际应用示例

### 创建程序化遮罩系统

```python
def create_procedural_mask(name='ProceduralMask'):
    """创建程序化遮罩"""
    # 创建新遮罩
    bpy.ops.mask.new(name=name)
    mask = bpy.data.masks[name]
    
    # 创建基础图层
    layer = mask.layers.new(name='BaseLayer')
    mask.layers.active = layer
    
    # 创建遮罩形状
    for i in range(8):
        angle = i * (2 * 3.14159 / 8)
        x = 0.5 + 0.3 * (i % 2)
        y = 0.5 + 0.3 * ((i + 1) % 2)
        
        bpy.ops.mask.primitive_add(type='ELLIPSE')
        active_spline = layer.splines.active
        active_spline.points[0].co = (x, y, 0)
    
    return mask
```

### 创建动画遮罩

```python
def animate_mask_expansion(mask_name, start_frame=1, end_frame=60):
    """创建遮罩扩展动画"""
    mask = bpy.data.masks[mask_name]
    layer = mask.layers.active
    
    # 设置起始帧
    bpy.context.scene.frame_set(start_frame)
    
    # 初始大小
    for spline in layer.splines:
        for point in spline.points:
            point.radius = 0.1
        spline.keyframe_insert(data_path='points')
    
    # 设置结束帧
    bpy.context.scene.frame_set(end_frame)
    
    # 最终大小
    for spline in layer.splines:
        for point in spline.points:
            point.radius = 1.0
        spline.keyframe_insert(data_path='points')
```

### 创建复杂遮罩形状

```python
def create_complex_mask_shape():
    """创建复杂遮罩形状"""
    # 创建遮罩
    bpy.ops.mask.new(name='ComplexShape')
    mask = bpy.data.masks['ComplexShape']
    layer = mask.layers.new(name='MainLayer')
    mask.layers.active = layer
    
    # 创建星形遮罩
    points = 8
    outer_radius = 0.4
    inner_radius = 0.2
    
    for i in range(points):
        angle = i * (2 * 3.14159 / points)
        
        # 外点
        x_outer = 0.5 + outer_radius * math.cos(angle)
        y_outer = 0.5 + outer_radius * math.sin(angle)
        
        # 内点
        angle_inner = angle + 3.14159 / points
        x_inner = 0.5 + inner_radius * math.cos(angle_inner)
        y_inner = 0.5 + inner_radius * math.sin(angle_inner)
        
        # 添加点
        bpy.ops.mask.primitive_add(type='FREEHAND')
        active_spline = layer.splines.active
        
        # 设置点位置
        for j, point in enumerate(active_spline.points):
            if j % 2 == 0:
                point.co = (x_outer, y_outer, 0)
            else:
                point.co = (x_inner, y_inner, 0)
```

### 创建羽化遮罩

```python
def create_feathered_mask():
    """创建羽化遮罩"""
    # 创建遮罩
    bpy.ops.mask.new(name='FeatheredMask')
    mask = bpy.data.masks['FeatheredMask']
    layer = mask.layers.new(name='FeatherLayer')
    mask.layers.active = layer
    
    # 创建圆形遮罩
    bpy.ops.mask.primitive_add(type='ELLIPSE')
    spline = layer.splines.active
    
    # 设置羽化
    for point in spline.points:
        point.radius = 0.5
        point.feather_weight = 0.5
    
    # 插入羽化关键帧
    bpy.ops.mask.shape_key_feather_insert()
```

### 创建对称遮罩

```python
def create_symmetric_mask():
    """创建对称遮罩"""
    # 创建遮罩
    bpy.ops.mask.new(name='SymmetricMask')
    mask = bpy.data.masks['SymmetricMask']
    layer = mask.layers.new(name='SymLayer')
    mask.layers.active = layer
    
    # 创建一半遮罩
    bpy.ops.mask.primitive_add(type='FREEHAND')
    spline = layer.splines.active
    
    # 添加点
    points = [(0.3, 0.2), (0.6, 0.5), (0.3, 0.8)]
    for i, (x, y) in enumerate(points):
        spline.points[i].co = (x, y, 0)
    
    # 对称遮罩
    bpy.ops.mask.symmetrize(direction='POSITIVE_X')
```

## 使用场景

- **合成工作流**：在合成阶段隔离图像区域
- **动态遮罩**：创建动画遮罩用于跟踪
- **形状编辑**：创建复杂的遮罩形状
- **羽化效果**：创建柔和边缘的遮罩
- **批处理**：自动化遮罩创建流程
- **特效制作**：用于运动模糊、景深等效果

## 注意事项

- 遮罩需要在合成节点中使用
- 动画遮罩需要关键帧支持
- 羽化效果影响合成质量
- 点数量影响性能
- 对称操作需要正确设置方向
- 遮罩坐标系是归一化的（0-1范围）


---
## bpy_ops_grease_pencil

# bpy.ops.grease_pencil 模块

石墨铅笔动画操作模块，用于创建和管理2D绘画动画。

## 概述

bpy.ops.grease_pencil 模块包含用于创建和编辑石墨铅笔（Grease Pencil）对象的运算符。石墨铅笔是Blender中强大的2D绘图工具，可以用于动画制作、故事板绘制、概念设计等多种场景。

## 常用操作

### 基础操作

```python
import bpy

# 添加石墨铅笔对象
bpy.ops.object.grease_pencil_add(type='EMPTY')

# 添加空白石墨铅笔
bpy.ops.object.grease_pencil_add(type='EMPTY')

# 添加指定类型的石墨铅笔
bpy.ops.object.grease_pencil_add(type='DRAWING')

# 转换为石墨铅笔
bpy.ops.object.convert(target='GPENCIL')
```

### 绘制模式

```python
# 进入绘制模式
bpy.ops.gpencil.draw(mode='DRAW')

# 进入绘制模式
bpy.ops.gpencil.draw(mode='DRAW', stroke=stroke_data)

# 进入线条模式
bpy.ops.gpencil.draw(mode='LINE')

# 进入多边形模式
bpy.ops.gpencil.draw(mode='POLY')

# 进入弧线模式
bpy.ops.gpencil.draw(mode='ARC')

# 进入曲线模式
bpy.ops.gpencil.draw(mode='CURVE')

### 图层操作

```python
# 添加新图层
bpy.ops.gpencil.layer_add()

# 删除图层
bpy.ops.gpencil.layer_remove()

# 重命名图层
bpy.ops.gpencil.layer_rename(name='NewLayer')

# 复制图层
bpy.ops.gpencil.layer_duplicate()

# 移动图层
bpy.ops.gpencil.layer_move(type='UP')

# 隐藏图层
bpy.ops.gpencil.layer_hide()

# 显示图层
bpy.ops.gpencil.layer_reveal()

# 锁定图层
bpy.ops.gpencil.layer_lock()

# 解锁图层
bpy.ops.gpencil.layer_unlock()

# 设置图层透明度
bpy.ops.gpencil.layer_opacity(opacity=0.5)

# 设置图层混合模式
bpy.ops.gpencil.layer_blend(mode='NORMAL')
```

### 帧操作

```python
# 添加新帧
bpy.ops.gpencil.frame_add()

# 删除帧
bpy.ops.gpencil.frame_delete()

# 复制帧
bpy.ops.gpencil.frame_duplicate()

# 移动帧
bpy.ops.gpencil.frame_move(type='NEXT')

# 跳转到帧
bpy.ops.gpencil.frame_jump(frame=10)

# 选择帧
bpy.ops.gpencil.frame_select()

# 设置关键帧
bpy.ops.gpencil.interpolate()
```

### 绘制操作

```python
# 绘制线条
bpy.ops.gpencil.draw()

# 绘制点
bpy.ops.gpencil.draw(mode='DOT')

# 擦除线条
bpy.ops.gpencil.erase()

# 平滑线条
bpy.ops.gpencil.smooth()

# 简化线条
bpy.ops.gpencil.simplify()

# 复制到粘贴板
bpy.ops.gpencil.copy()

# 从粘贴板粘贴
bpy.ops.gpencil.paste()

# 删除选中
bpy.ops.gpencil.delete(type='STROKE')
```

### 选择操作

```python
# 全选
bpy.ops.gpencil.select_all(action='SELECT')

# 取消全选
bpy.ops.gpencil.select_all(action='DESELECT')

# 反选
bpy.ops.gpencil.select_all(action='INVERT')

# 选择同层
bpy.ops.gpencil.select_layer()

# 选择同帧
bpy.ops.gpencil.select_frame()

# 选择相同颜色
bpy.ops.gpencil.select_color()

# 选择相似
bpy.ops.gpencil.select_similar()
```

### 编辑操作

```python
# 进入编辑模式
bpy.ops.gpencil.editmode_toggle()

# 移动选中
bpy.ops.gpencil.move(delta_x=10, delta_y=0)

# 旋转选中
bpy.ops.gpencil.rotate(angle=45)

# 缩放选中
bpy.ops.gpencil.scale(factor=1.5)

# 复制选中
bpy.ops.gpencil.duplicate_move()

# 删除选中
bpy.ops.gpencil.delete(type='POINTS')

# 合并选中
bpy.ops.gpencil.merge(type='LAST')
```

## 实际应用示例

### 创建基本绘图

```python
def create_grease_pencil_drawing():
    """创建石墨铅笔绘图"""
    # 添加石墨铅笔对象
    bpy.ops.object.grease_pencil_add(type='EMPTY')
    gp_obj = bpy.context.active_object
    
    # 创建新图层
    bpy.ops.gpencil.layer_add()
    layer = bpy.context.active_object
    
    # 创建新帧
    bpy.ops.gpencil.frame_add()
    frame = bpy.context.active_object
    
    # 绘制简单线条
    stroke = [
        {"co": (0, 0, 0), "pressure": 1, "time": 0},
        {"co": (1, 1, 0), "pressure": 1, "time": 1},
        {"co": (2, 0, 0), "pressure": 1, "time": 2},
    ]
    
    # 添加笔画
    bpy.ops.gpencil.draw(stroke=stroke, mode='DRAW')
    
    return gp_obj
```

### 创建逐帧动画

```python
def create_frame_by_frame_animation(gp_obj, frame_count=24):
    """创建逐帧动画"""
    # 确保选中石墨铅笔对象
    bpy.context.view_layer.objects.active = gp_obj
    gp_obj.select_set(True)
    
    # 创建图层
    bpy.ops.gpencil.layer_add()
    layer = gp_obj.data.layers[0]
    
    # 为每一帧创建内容
    for i in range(frame_count):
        # 添加新帧
        bpy.ops.gpencil.frame_add()
        
        # 在当前帧绘制
        create_frame_content(i)
        
        # 移动到下一帧
        bpy.ops.gpencil.frame_move(type='NEXT')

def create_frame_content(frame_index):
    """创建帧内容"""
    import math
    
    # 创建圆形动画
    angle = frame_index * (2 * math.pi / 24)
    radius = 2
    
    stroke = []
    for i in range(36):
        theta = i * (2 * math.pi / 36)
        x = radius * math.cos(theta)
        y = radius * math.sin(theta)
        stroke.append({
            "co": (x, y, 0),
            "pressure": 1,
            "time": i
        })
    
    bpy.ops.gpencil.draw(stroke=stroke, mode='DRAW')
```

### 创建动画特效

```python
def create_glow_effect(gp_obj, layer_name):
    """创建发光效果"""
    layer = gp_obj.data.layers.get(layer_name)
    
    if layer:
        # 复制图层
        bpy.ops.gpencil.layer_duplicate()
        glow_layer = gp_obj.data.layers[-1]
        
        # 设置发光属性
        for frame in glow_layer.active_frame.strokes:
            for point in stroke.points:
                point.color = (1, 1, 0, 1)  # 黄色
            
        # 设置混合模式为叠加
        glow_layer.blend_mode = 'ADD'
        
        # 设置透明度
        bpy.ops.gpencil.layer_opacity(opacity=0.5)
```

### 创建洋葱皮效果

```python
def create_onion_skin(gp_obj, layer_name, past_frames=3, future_frames=3):
    """创建洋葱皮效果"""
    layer = gp_obj.data.layers.get(layer_name)
    
    if layer:
        # 启用洋葱皮
        layer.use_onion_skinning = True
        
        # 设置显示帧数
        layer.grease_pencil_onion_user_data.past_frames = past_frames
        layer.grease_pencil_onion_user_data.future_frames = future_frames
        
        # 设置洋葱皮透明度
        layer.grease_pencil_onion_user_data.opacity = 0.3
```

### 创建墨迹效果

```python
def create_ink_effect(gp_obj, layer_name):
    """创建墨迹效果"""
    layer = gp_obj.data.layers.get(layer_name)
    
    if layer:
        # 添加线条平滑修改器
        bpy.ops.gpencil.stroke_smooth()
        
        # 添加线条模糊效果
        for stroke in layer.active_frame.strokes:
            for point in stroke.points:
                point.draw_mode = '3DSPACE'
```

### 创建填充效果

```python
def create_fill_effect(gp_obj, layer_name):
    """创建填充效果"""
    layer = gp_obj.data.layers.get(layer_name)
    
    if layer:
        # 选择封闭区域
        bpy.ops.gpencil.select_all(action='DESELECT')
        
        # 使用填充工具
        for frame in layer.frames:
            for stroke in frame.strokes:
                if stroke.is_closed:
                    stroke.fill_opacity = 1.0
                    stroke.material_index = 1
```

### 创建蒙版效果

```python
def create_mask_layer(gp_obj, mask_layer_name):
    """创建蒙版图层"""
    # 添加新图层
    bpy.ops.gpencil.layer_add()
    mask_layer = bpy.context.active_object
    mask_layer.name = mask_layer_name
    
    # 设置为蒙版
    mask_layer.use_mask_layer = True
    
    # 绘制蒙版区域
    bpy.ops.gpencil.draw()
    
    return mask_layer
```

### 导出为SVG

```python
def export_to_svg(gp_obj, filepath):
    """导出为SVG文件"""
    # 选择石墨铅笔对象
    bpy.context.view_layer.objects.active = gp_obj
    gp_obj.select_set(True)
    
    # 导出为SVG
    bpy.ops.export_scene.gpencil(
        filepath=filepath,
        export_type='SVG'
    )
```

### 导出为PNG序列

```python
def export_to_png_sequence(gp_obj, output_dir, frame_start=1, frame_end=24):
    """导出为PNG序列"""
    # 设置渲染输出路径
    bpy.context.scene.render.filepath = output_dir
    
    # 逐帧渲染
    for frame in range(frame_start, frame_end + 1):
        bpy.context.scene.frame_set(frame)
        
        # 渲染当前帧
        bpy.ops.render.render(
            write_still=True,
            use_viewport=True
        )
```

### 创建故事板

```python
def create_storyboard(gp_obj, panels, frame_duration=8):
    """创建故事板"""
    layer = gp_obj.data.layers[0]
    
    for i, panel_content in enumerate(panels):
        # 计算帧范围
        start_frame = i * frame_duration + 1
        end_frame = (i + 1) * frame_duration
        
        # 跳转到帧
        bpy.ops.gpencil.frame_jump(frame=start_frame)
        
        # 绘制面板内容
        draw_panel_content(panel_content)
        
        # 复制到所有帧
        for frame in range(start_frame + 1, end_frame + 1):
            bpy.ops.gpencil.frame_jump(frame=frame)
            bpy.ops.gpencil.frame_duplicate()

def draw_panel_content(content):
    """绘制面板内容"""
    # 根据内容类型绘制不同图形
    if content['type'] == 'box':
        draw_rectangle(content['position'], content['size'])
    elif content['type'] == 'circle':
        draw_circle(content['position'], content['radius'])
    elif content['type'] == 'text':
        draw_text(content['text'], content['position'])
```

## 使用场景

- **2D动画制作**：创建手绘风格的动画
- **故事板绘制**：快速绘制动画分镜
- **概念设计**：草图和快速设计
- **注释和标记**：在3D场景中添加2D注释
- **动态特效**：创建线条动画和特效
- **角色动画**：2D角色动作设计
- **场景布局**：快速布局和构图
- **教学演示**：创建说明性动画

## 注意事项

- 石墨铅笔绘图需要切换到绘图区域
- 图层管理对于复杂项目很重要
- 帧操作需要确保在正确的图层上
- 绘制操作前需要选择正确的笔刷
- 导出前应检查图层和帧的完整性
- 大型项目需要注意性能优化
- 定期保存工作进度
- 使用蒙版可以创建复杂的叠加效果
- 洋葱皮功能有助于动画参考
- 结合3D场景可以创建独特的视觉效果


---
## bpy_ops_gpencil

# bpy.ops.grease_pencil - 画笔操作模块

Blender画笔操作符，用于在画笔编辑器中管理画笔设置。

## 概述

`bpy.ops.gpencil` 模块提供用于操作画笔编辑器和画笔设置的功能。这与 grease_pencil 模块不同，这是专门用于画笔本身的编辑操作。

## 核心功能

### 画笔选择

```python
import bpy

# 选择画笔
bpy.ops.gpencil.select(
    extend=False,
    deselect=False,
    toggle=False,
    deselect_all=True
)

# 选择所有画笔
bpy.ops.gpencil.select_all(
    action='SELECT'
)

# 取消选择所有画笔
bpy.ops.gpencil.select_all(
    action='DESELECT'
)

# 反选
bpy.ops.gpencil.select_all(
    action='INVERT'
)

# 选择相似的画笔
bpy.ops.gpencil.select_similar(
    type='TYPE',
    threshold=0.1
)
```

### 画笔编辑

```python
# 新建画笔
bpy.ops.gpencil.brush_add(
    name='NewBrush'
)

# 复制画笔
bpy.ops.gpencil.brush_duplicate()

# 重命名画笔
bpy.ops.gpencil.brush_rename(
    name='MyBrush'
)

# 删除画笔
bpy.ops.gpencil.brush_delete()

# 重置画笔
bpy.ops.gpencil.brush_reset()

# 清除画笔
bpy.ops.gpencil.brush_clear()
```

### 画笔设置

```python
# 设置画笔类型
bpy.ops.gpencil.brush_type_set(
    type='DRAW'
)

# 设置画笔颜色
bpy.ops.gpencil.brush_color_set(
    color=(1.0, 0.0, 0.0, 1.0)
)

# 设置画笔大小
bpy.ops.gpencil.brush_size_set(
    size=20
)

# 设置画笔强度
bpy.ops.gpencil.brush_strength_set(
    strength=0.5
)

# 设置画笔混合模式
bpy.ops.gpencil.brush_blend_set(
    mode='MIX'
)

# 应用画笔预设
bpy.ops.gpencil.brush_preset_apply(
    preset='Basic'
)

# 保存画笔预设
bpy.ops.gpencil.brush_preset_save(
    name='MyPreset'
)

# 删除画笔预设
bpy.ops.gpencil.brush_preset_delete(
    name='MyPreset'
)
```

### 画笔纹理

```python
# 添加纹理
bpy.ops.gpencil.texture_add()

# 清除纹理
bpy.ops.gpencil.texture_remove()

# 设置纹理缩放
bpy.ops.gpencil.texture_scale_set(
    scale=1.0
)

# 设置纹理旋转
bpy.ops.gpencil.texture_rotate_set(
    angle=0.0
)
```

### 画笔曲线

```python
# 设置曲线
bpy.ops.gpencil.curve_preset(
    preset='SHARP'
)

# 编辑曲线
bpy.ops.gpencil.curve_edit()

# 重置曲线
bpy.ops.gpencil.curve_reset()

# 设置曲线点
bpy.ops.gpencil.curve_point_add()
```

### 画笔模板

```python
# 添加模板
bpy.ops.gpencil.stencil_add()

# 移除模板
bpy.ops.gpencil.stencil_remove()

# 显示模板
bpy.ops.gpencil.stencil_show()

# 隐藏模板
bpy.ops.gpencil.stencil_hide()

# 旋转模板
bpy.ops.gpencil.stencil_rotate(
    angle=45
)

# 缩放模板
bpy.ops.gpencil.stencil_scale(
    scale=1.5
)

# 移动模板
bpy.ops.gpencil.stencil_offset(
    factor=(0.1, 0.1)
)
```

### 画笔变换

```python
# 变换画笔
bpy.ops.gpencil.brush_transform()

# 旋转画笔
bpy.ops.gpencil.brush_rotate(
    angle=45
)

# 缩放画笔
bpy.ops.gpencil.brush_scale(
    factor=1.5
)

# 移动画笔
bpy.ops.gpencil.brush_offset(
    factor=(0.1, 0.1)
)
```

### 画笔工具

```python
# 采样颜色
bpy.ops.gpencil.sample_color()

# 采样活动颜色
bpy.ops.gpencil.sample_active()

# 切换画笔编辑器
bpy.ops.gpencil.toggle_toolshelf()

# 切换画笔属性
bpy.ops.gpencil.toggle_brush_panel()
```

### 画笔库操作

```python
# 导入画笔库
bpy.ops.gpencil.brush_library_import(
    filepath='//brushes.blend'
)

# 导出画笔库
bpy.ops.gpencil.brush_library_export(
    filepath='//brushes.blend'
)

# 添加画笔到库
bpy.ops.gpencil.brush_to_library()

# 从库中移除画笔
bpy.ops.gpencil.brush_from_library()
```

## 实用函数

```python
def get_all_brushes():
    """获取所有画笔"""
    return list(bpy.data.gpencil_brushes)

def get_brush_by_name(name):
    """根据名称获取画笔"""
    return bpy.data.gpencil_brushes.get(name)

def get_active_brush():
    """获取当前画笔"""
    return bpy.context.tool_settings.gpencil_brush

def set_active_brush(brush_name):
    """设置当前画笔"""
    brush = bpy.data.gpencil_brushes.get(brush_name)
    if brush:
        bpy.context.tool_settings.gpencil_brush = brush

def create_brush(name, brush_type='DRAW'):
    """创建新画笔"""
    brush = bpy.data.gpencil_brushes.new(name=name)
    brush.gpencil_brush_type = brush_type
    return brush

def duplicate_brush(brush_name, new_name):
    """复制画笔"""
    brush = get_brush_by_name(brush_name)
    if brush:
        new_brush = brush.copy()
        new_brush.name = new_name
        return new_brush
    return None

def delete_brush(brush_name):
    """删除画笔"""
    brush = get_brush_by_name(brush_name)
    if brush:
        bpy.data.gpencil_brushes.remove(brush)

def reset_all_brushes():
    """重置所有画笔"""
    for brush in bpy.data.gpencil_brushes:
        brush.reset()

def batch_set_brush_size(brushes, size):
    """批量设置画笔大小"""
    for brush_name in brushes:
        brush = get_brush_by_name(brush_name)
        if brush:
            brush.size = size

def batch_set_brush_color(brushes, color):
    """批量设置画笔颜色"""
    for brush_name in brushes:
        brush = get_brush_by_name(brush_name)
        if brush:
            brush.color = color

def compare_brushes(brush1_name, brush2_name):
    """比较两个画笔设置"""
    brush1 = get_brush_by_name(brush1_name)
    brush2 = get_brush_by_name(brush2_name)
    if brush1 and brush2:
        return {
            'size': brush1.size == brush2.size,
            'strength': brush1.strength == brush2.strength,
            'color': brush1.color == brush2.color
        }
    return False
```

## 常见问题排查

### 画笔不生效
```python
# 检查当前画笔
brush = bpy.context.tool_settings.gpencil_brush
print(f"当前画笔: {brush}")

# 检查画笔类型
print(f"画笔类型: {brush.gpencil_brush_type}")

# 确保在正确的工作区
print(f"当前区域类型: {bpy.context.area.type}")

# 检查画笔是否被锁定
print(f"锁定: {brush.locked}")

# 解锁画笔
brush.locked = False
```

### 画笔设置无法保存
```python
# 检查画笔是否来自库
brush = bpy.context.tool_settings.gpencil_brush
print(f"库: {brush.library}")

# 复制到本地
if brush.library:
    local_brush = brush.copy()
    local_brush.name = brush.name + '_local'
    bpy.context.tool_settings.gpencil_brush = local_brush

# 保存为预设
bpy.ops.gpencil.brush_preset_save(name='MySettings')
```

### 画笔纹理不显示
```python
# 检查纹理
brush = bpy.context.tool_settings.gpencil_brush
print(f"纹理: {brush.texture}")

# 检查纹理图像
if brush.texture:
    print(f"纹理图像: {brush.texture.image}")

# 检查纹理混合
print(f"纹理混合: {brush.texture_mode}")

# 重新加载纹理
if brush.texture and brush.texture.image:
    brush.texture.image.reload()
```


---
## bpy_ops_font

# bpy.ops.font - Font Operations Module

Blender 字体操作符，用于创建和编辑文本对象。

## Overview

`bpy.ops.font` 模块包含文本对象（Text Object）的所有操作命令，涵盖文本的创建、编辑、样式设置等功能。这个模块主要用于处理 Blender 中的 3D 文字对象。

## 核心功能

### 文本对象创建

```python
import bpy

# 创建文本对象
bpy.ops.object.text_add(location=(0, 0, 0))
text_obj = bpy.context.active_object
text_obj.data.body = "Hello World"

# 创建内联文本对象
bpy.ops.font.text_add(text="New Text", cursor=0)

# 在光标处插入文本
bpy.ops.font.insert(text=" inserted text")

# 打开文本文件
bpy.ops.font.open(filepath="C:/fonts/text.txt")

# 替换选中的文本
bpy.ops.font.replace(replace="replacement text")
```

### 文本编辑操作

```python
# 选择所有文本
bpy.ops.font.select_all(select='SELECT')

# 全不选
bpy.ops.font.select_all(select='DESELECT')

# 移动光标
bpy.ops.font.move(type='LINE_BEGIN')
bpy.ops.font.move(type='LINE_END')
bpy.ops.font.move(type='PREVIOUS_CHARACTER')
bpy.ops.font.move(type='NEXT_CHARACTER')
bpy.ops.font.move(type='PREVIOUS_WORD')
bpy.ops.font.move(type='NEXT_WORD')
bpy.ops.font.move(type='PREVIOUS_LINE')
bpy.ops.font.move(type='NEXT_LINE')

# 删除
bpy.ops.font.delete(type='DELETE')
bpy.ops.font.delete(type='BACKSPACE')
bpy.ops.font.delete(type='WORD_DELETE')

# 剪切/复制/粘贴
bpy.ops.font.cut()
bpy.ops.font.copy()
bpy.ops.font.paste()
```

### 文本样式设置

```python
# 设置字体
bpy.ops.font.font_select.invoke()

# 设置字体大小
text_obj.data.size = 1.0

# 设置行距
text_obj.data.line_height = 1.5

# 设置字间距
text_obj.data.space_character = 0.2

# 设置单词间距
text_obj.data.space_word = 0.5

# 设置段落缩进
text_obj.data.tab_width = 1.0

# 设置对齐方式
text_obj.data.align_x = 'LEFT'  # LEFT, CENTER, RIGHT, JUSTIFY, FLUSH
text_obj.data.align_y = 'TOP'   # TOP, MIDDLE, BOTTOM

# 设置自动换行
text_obj.data.wrap_width = 0.0  # 0 为禁用

# 设置粗体
text_obj.data.font = bold_font
text_obj.data.bold_font = bold_font

# 设置斜体
text_obj.data.italic_font = italic_font
text_obj.data.shear = 0.2

# 设置下划线
text_obj.data.underline_position = -0.1
text_obj.data.underline_height = 0.02
```

### 文本效果

```python
# 设置阴影
text_obj.data.shadow = True
text_obj.data.shadow_offset_x = 0.05
text_obj.data.shadow_offset_y = -0.05
text_obj.data.shadow_color = (0, 0, 0, 0.5)

# 设置轮廓
text_obj.data.outline = True
text_obj.data.outline_width = 0.02
text_obj.data.outline_color = (1, 1, 1, 1)

# 设置模糊
text_obj.data.blur_radius = 0.0
```

## 实际应用示例

### 文本生成器

```python
import bpy
import os

class TextGenerator:
    """文本生成器"""
    
    def __init__(self):
        self.default_size = 1.0
        self.default_depth = 0.0
        self.default_resolution = 12
    
    def create_text(self, text, location=(0, 0, 0), size=1.0, depth=0.0):
        """创建 3D 文本"""
        bpy.ops.object.text_add(location=location)
        text_obj = bpy.context.active_object
        
        # 设置文本内容
        text_obj.data.body = text
        
        # 设置大小
        text_obj.data.size = size
        
        # 设置挤出深度
        text_obj.data.extrude = depth
        
        # 设置分辨率
        text_obj.data.resolution_u = self.default_resolution
        text_obj.data.bevel_resolution = self.default_resolution
        
        return text_obj
    
    def create_3d_title(self, text, location=(0, 0, 0)):
        """创建 3D 标题"""
        title = self.create_text(text, location=location, size=1.0, depth=0.1)
        title.name = "Title"
        return title
    
    def create_subtitle(self, text, location=(0, -1, 0)):
        """创建副标题"""
        subtitle = self.create_text(text, location=location, size=0.5, depth=0.05)
        subtitle.name = "Subtitle"
        return subtitle
    
    def create_styled_text(self, text, location=(0, 0, 0), 
                           size=1.0, depth=0.1,
                           bold=False, italic=False,
                           shadow=True, outline=False):
        """创建样式化文本"""
        text_obj = self.create_text(text, location=location, size=size, depth=depth)
        
        # 设置阴影
        if shadow:
            text_obj.data.shadow = True
            text_obj.data.shadow_offset_x = 0.02
            text_obj.data.shadow_offset_y = -0.02
            text_obj.data.shadow_color = (0, 0, 0, 0.8)
        
        # 设置轮廓
        if outline:
            text_obj.data.outline = True
            text_obj.data.outline_width = 0.01
            text_obj.data.outline_color = (1, 1, 1, 1)
        
        return text_obj
    
    def create_multiline_text(self, lines, location=(0, 0, 0), size=1.0):
        """创建多行文本"""
        text = '\n'.join(lines)
        text_obj = self.create_text(text, location=location, size=size)
        text_obj.data.align_x = 'CENTER'
        return text_obj


def create_styled_title_scene(title, subtitle):
    """创建标题场景"""
    generator = TextGenerator()
    
    # 主标题
    main_title = generator.create_3d_title(
        title,
        location=(0, 0, 0)
    )
    
    # 副标题
    sub = generator.create_subtitle(
        subtitle,
        location=(0, -1.2, 0)
    )
    
    # 添加材质
    for obj in [main_title, sub]:
        if obj.data.materials:
            mat = obj.data.materials[0]
        else:
            mat = bpy.data.materials.new(name=f"{obj.name}_Material")
            obj.data.materials.append(mat)
        
        mat.use_nodes = True
        nodes = mat.node_tree.nodes
        principled = nodes.get('Principled BSDF')
        if principled:
            principled.inputs['Base Color'].default_value = (1, 1, 1, 1)
            principled.inputs['Metallic'].default_value = 0.3
            principled.inputs['Roughness'].default_value = 0.4
    
    return main_title, sub
```

### 标题动画系统

```python
import bpy

class TitleAnimationSystem:
    """标题动画系统"""
    
    def __init__(self):
        self.frame_rate = bpy.context.scene.render.fps
    
    def animate_text_appearance(self, text_obj, start_frame, duration=30):
        """动画：文字出现效果"""
        # 缩放动画
        text_obj.scale = (0, 0, 0)
        text_obj.keyframe_insert(data_path='scale', frame=start_frame)
        
        text_obj.scale = (1, 1, 1)
        text_obj.keyframe_insert(data_path='scale', frame=start_frame + duration)
        
        # 旋转动画
        text_obj.rotation_euler = (0, 0, -0.5)
        text_obj.keyframe_insert(data_path='rotation_euler', frame=start_frame)
        
        text_obj.rotation_euler = (0, 0, 0)
        text_obj.keyframe_insert(data_path='rotation_euler', frame=start_frame + duration)
    
    def animate_text_wave(self, text_obj, start_frame, duration=60):
        """动画：波浪效果"""
        for i in range(len(text_obj.data.body)):
            char_index = text_obj.data.body.index(text_obj.data.body[i]) if i > 0 else 0
            # 注意：逐字符动画需要更复杂的实现
            pass
    
    def animate_letter_by_letter(self, text_obj, start_frame, letter_duration=5):
        """动画：逐字显示"""
        text_obj.data.body = ""
        total_duration = len(text_obj.data.body) * letter_duration
        
        for i in range(total_duration):
            frame = start_frame + i * letter_duration
            # 设置可见字符数
            # 注意：此功能需要使用表达式或驱动实现
    
    def create_typewriter_effect(self, text_obj, start_frame, char_delay=3):
        """打字机效果"""
        original_text = text_obj.data.body
        text_obj.data.body = ""
        
        for i, char in enumerate(original_text):
            text_obj.data.body += char
            text_obj.data.body_eval = text_obj.data.body  # 强制更新
            text_obj.keyframe_insert(data_path='[""]', frame=start_frame + i * char_delay)
        
        # 恢复完整文本
        text_obj.data.body = original_text
    
    def create_fade_in_out(self, text_obj, fade_in_frames=15, fade_out_frames=15, 
                          hold_frames=60, start_frame=1):
        """淡入淡出效果"""
        mat = text_obj.data.materials[0] if text_obj.data.materials else None
        if not mat or not mat.use_nodes:
            return
        
        nodes = mat.node_tree.nodes
        principled = nodes.get('Principled BSDF')
        if not principled:
            return
        
        alpha_input = principled.inputs.get('Alpha')
        if not alpha_input:
            return
        
        # 淡入
        alpha_input.default_value = 0
        alpha_input.keyframe_insert(data_path='default_value', frame=start_frame)
        alpha_input.default_value = 1
        alpha_input.keyframe_insert(data_path='default_value', frame=start_frame + fade_in_frames)
        
        # 保持
        alpha_input.default_value = 1
        alpha_input.keyframe_insert(data_path='default_value', frame=start_frame + fade_in_frames + hold_frames)
        
        # 淡出
        alpha_input.default_value = 0
        alpha_input.keyframe_insert(data_path='default_value', frame=start_frame + fade_in_frames + hold_frames + fade_out_frames)
    
    def setup_title_sequence(self, text_objects, start_frame=1):
        """设置标题序列动画"""
        current_frame = start_frame
        
        for obj in text_objects:
            duration = 30
            self.animate_text_appearance(obj, current_frame, duration=duration)
            self.create_fade_in_out(obj, start_frame=current_frame, hold_frames=30)
            current_frame += duration + 60
        
        return current_frame


def create_animated_title(title_text, subtitle_text):
    """创建带动画的标题"""
    system = TitleAnimationSystem()
    generator = TextGenerator()
    
    # 创建文本对象
    title = generator.create_styled_text(
        title_text,
        location=(0, 0, 0),
        size=1.5,
        depth=0.1,
        shadow=True
    )
    title.name = "AnimatedTitle"
    
    subtitle = generator.create_styled_text(
        subtitle_text,
        location=(0, -2, 0),
        size=0.8,
        depth=0.05,
        shadow=True
    )
    subtitle.name = "AnimatedSubtitle"
    
    # 设置动画
    system.animate_text_appearance(title, start_frame=1, duration=30)
    system.create_fade_in_out(title, start_frame=1, hold_frames=60)
    
    system.animate_text_appearance(subtitle, start_frame=60, duration=20)
    system.create_fade_in_out(subtitle, start_frame=60, hold_frames=60)
    
    return title, subtitle
```

### 文本路径工具

```python
import bpy
import math

class TextOnPathTool:
    """文本沿路径工具"""
    
    def create_text_on_curve(self, text_obj, curve_obj):
        """创建沿曲线分布的文本"""
        # 将文本添加到曲线
        text_obj.data.align_x = 'CENTER'
        
        # 创建路径约束
        constraint = text_obj.constraints.new(type='FOLLOW_PATH')
        constraint.target = curve_obj
        constraint.offset = 0
        constraint.use_curve_follow = True
        constraint.use_curve_radius = True
        
        return constraint
    
    def create_circular_text(self, text, radius=3, location=(0, 0, 0)):
        """创建圆形文本"""
        # 创建圆形曲线
        bpy.ops.mesh.primitive_circle_add(
            vertices=64,
            radius=radius,
            fill_type='NOTHING',
            location=location
        )
        curve = bpy.context.active_object
        curve.name = "TextPath"
        
        # 转换为曲线
        bpy.ops.object.convert(target='CURVE')
        
        # 创建文本
        bpy.ops.object.text_add(location=(radius, 0, 0))
        text_obj = bpy.context.active_object
        text_obj.data.body = text
        text_obj.data.size = 0.5
        text_obj.data.align_x = 'CENTER'
        
        # 设置沿路径
        constraint = text_obj.constraints.new(type='FOLLOW_PATH')
        constraint.target = curve
        constraint.use_curve_follow = True
        constraint.use_curve_radius = False
        
        # 调整方向
        text_obj.rotation_euler = (0, math.pi / 2, 0)
        
        return text_obj, curve
    
    def create_spiral_text(self, text, loops=3, height=2, radius=1):
        """创建螺旋文本"""
        # 创建螺旋曲线
        bpy.ops.curve.primitive_nurbs_path_add(radius1=radius)
        curve = bpy.context.active_object
        curve.name = "SpiralPath"
        
        curve_data = curve.data
        points = []
        
        for i in range(64):
            t = i / 63
            angle = t * loops * 2 * math.pi
            x = radius - t * radius * 0.8
            y = math.cos(angle) * x * 0.1
            z = t * height
            points.append((x, y, z))
        
        # 设置曲线点
        curve_data.splines[0].points.foreach_set('co', 
            [coord for point in points for coord in point + (1,)])
        
        # 创建文本
        bpy.ops.object.text_add(location=(0, 0, 0))
        text_obj = bpy.context.active_object
        text_obj.data.body = text
        text_obj.data.size = 0.3
        text_obj.data.align_x = 'CENTER'
        
        # 设置沿路径
        constraint = text_obj.constraints.new(type='FOLLOW_PATH')
        constraint.target = curve
        constraint.use_curve_follow = True
        constraint.offset_factor = -0.5
        
        return text_obj, curve


def create_text_scene():
    """创建文本场景"""
    tool = TextOnPathTool()
    generator = TextGenerator()
    
    # 直线标题
    title = generator.create_3d_title(
        "Blender Python",
        location=(0, 2, 0)
    )
    
    # 圆形文本
    circular_text, path = tool.create_circular_text(
        "SCRIPTING",
        radius=4,
        location=(0, 0, 0)
    )
    
    return title, circular_text, path
```

### 文本材质系统

```python
import bpy

class TextMaterialSystem:
    """文本材质系统"""
    
    materials = {}
    
    @classmethod
    def create_neon_material(cls, name, color=(0, 1, 1, 1), glow_strength=2.0):
        """创建霓虹材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        nodes = mat.node_tree.nodes
        links = mat.node_tree.links
        
        # 清除默认节点
        nodes.clear()
        
        # 创建节点
        output = nodes.new(type='ShaderNodeOutputMaterial')
        output.location = (400, 0)
        
        emission = nodes.new(type='ShaderNodeEmission')
        emission.location = (0, 0)
        emission.inputs['Color'].default_value = color
        emission.inputs['Strength'].default_value = glow_strength
        
        links.new(emission.outputs['Emission'], output.inputs['Surface'])
        
        cls.materials[name] = mat
        return mat
    
    @classmethod
    def create_metal_material(cls, name, color=(0.8, 0.8, 0.8, 1), roughness=0.3, metallic=1.0):
        """创建金属材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        nodes = mat.node_tree.nodes
        
        principled = nodes.get('Principled BSDF')
        if principled:
            principled.inputs['Base Color'].default_value = color
            principled.inputs['Roughness'].default_value = roughness
            principled.inputs['Metallic'].default_value = metallic
        
        cls.materials[name] = mat
        return mat
    
    @classmethod
    def create_glass_material(cls, name, color=(1, 1, 1, 1), transmission=1.0):
        """创建玻璃材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        mat.blend_method = 'BLEND'
        nodes = mat.node_tree.nodes
        
        principled = nodes.get('Principled BSDF')
        if principled:
            principled.inputs['Base Color'].default_value = color
            principled.inputs['Transmission'].default_value = transmission
            principled.inputs['Roughness'].default_value = 0.0
            principled.inputs['IOR'].default_value = 1.45
        
        cls.materials[name] = mat
        return mat
    
    @classmethod
    def create_wood_material(cls, name, color=(0.4, 0.25, 0.1, 1)):
        """创建木质材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        nodes = mat.node_tree.nodes
        links = mat.node_tree.links
        
        principled = nodes.get('Principled BSDF')
        if principled:
            principled.inputs['Base Color'].default_value = color
            principled.inputs['Roughness'].default_value = 0.7
        
        cls.materials[name] = mat
        return mat
    
    @classmethod
    def apply_material(cls, text_obj, material_name):
        """应用材质到文本"""
        mat = cls.materials.get(material_name)
        if not mat:
            return False
        
        if not text_obj.data.materials:
            text_obj.data.materials.append(mat)
        else:
            text_obj.data.materials[0] = mat
        
        return True
    
    @classmethod
    def list_materials(cls):
        """列出所有创建的材质"""
        return list(cls.materials.keys())


def setup_text_materials():
    """设置文本材质"""
    system = TextMaterialSystem()
    
    system.create_neon_material("NeonCyan", color=(0, 1, 1, 1), glow_strength=3.0)
    system.create_neon_material("NeonPink", color=(1, 0, 1, 1), glow_strength=3.0)
    system.create_metal_material("Gold", color=(1.0, 0.8, 0.2, 1))
    system.create_metal_material("Silver", color=(0.9, 0.9, 0.9, 1))
    system.create_glass_material("Glass", color=(1, 1, 1, 0.3))
    system.create_wood_material("Wood", color=(0.4, 0.25, 0.1, 1))
    
    print("材质已创建:")
    print(system.list_materials())
    
    return system
```


---
## bpy_ops_image

# bpy.ops.image - Image Operations Module

Blender 图像编辑器操作符，用于图像编辑、UV 编辑和纹理处理。

## Overview

`bpy.ops.image` 模块包含图像编辑器中的所有操作命令，包括图像加载、保存、编辑和 UV 映射操作。

## 核心功能

### 图像管理

```python
import bpy
import os

# 新建图像
bpy.ops.image.new(name="Untitled", width=1024, height=1024, color=(1, 1, 1, 1), alpha=True)

# 打开图像文件
bpy.ops.image.open(filepath="//textures/model.png", directory="//textures/", files=[{"name": "model.png", "name": "model.png"}], relative=True)

# 打开图像序列
bpy.ops.image.open(filepath="//render/frame_001.png", directory="//render/", files=[{"name": "frame_001.png", "name": "frame_001.png"}, {"name": "frame_002.png", "name": "frame_002.png"}], relative=True)

# 重新加载当前图像
bpy.ops.image.reload()

# 保存图像
bpy.ops.image.save()

# 另存为
bpy.ops.image.save_as(filepath="//exports/texture.png", copy=True)

# 外部保存
bpy.ops.image.external_save(filepath="//exports/texture.png")

# 外部编辑（用外部程序打开）
bpy.ops.image.external_edit(filepath="//textures/model.png")
```

### 图像编辑

```python
# 切换到绘画模式
bpy.ops.image.paint()

# 切换到 UV 编辑模式
bpy.ops.image.uv_sculpt()

# 切换到遮罩模式
bpy.ops.image.view_mask()

# 切换到样条线模式
bpy.ops.image.view_curve()

# 反转图像颜色
bpy.ops.image.invert(invert_r=False, invert_g=False, invert_b=False, invert_a=False)

# 调整图像大小
bpy.ops.image.resize(size=(2048, 2048), interpolation='BICUBIC')

# 裁剪图像
bpy.ops.image.crop(x=0, y=0, width=1024, height=1024)

# 旋转图像
bpy.ops.image.rotate(angle=1.570796, interpolation='BICUBIC')

# 平移图像
bpy.ops.image.offset(x=100, y=50)

# 清除图像
bpy.ops.image.clear()
```

### 图像填充

```python
# 用颜色填充
bpy.ops.image.fill(color=(1.0, 0.0, 0.0, 1.0), threshold=0, paint_mode='COLOR', channel='COLOR')

# 用渐变填充
bpy.ops.image.gradient_fill(gradient_type='DIAGONAL', steps=5, color1=(0, 0, 0, 1), color2=(1, 1, 1, 1), angle=0, width=1)

# 用检查板图案填充
bpy.ops.image.checker_fill(distance=10, intensity1=1, intensity2=0)
```

### 图像绘制

```python
# 绘制点
bpy.ops.image.draw_cursor_set(x=100, y=100)
bpy.ops.image.draw_brush(size=20, x=100, y=100, stroke=[{"name": "", "location": (0, 0), "pressure": 1, "time": 0, "pen_flip": False}])

# 绘制线条
bpy.ops.image.draw_brush(stroke=[{"name": "", "location": (0, 0), "pressure": 1, "time": 0, "pen_flip": False}, {"name": "", "location": (100, 100), "pressure": 1, "time": 0.1, "pen_flip": False}])

# 绘制曲线
bpy.ops.image.draw_curve(curve_mode='FREE', stroke=[{"name": "", "location": (0, 0), "pressure": 1, "time": 0, "pen_flip": False}])

# 绘制圆
bpy.ops.image.draw_brush(size=30, x=100, y=100)

# 绘制矩形
bpy.ops.image.draw_brush(size=30, x=100, y=100)
```

### 图像滤镜

```python
# 高斯模糊
bpy.ops.image.filter_gaussian(threshold=0.5)

# 模糊
bpy.ops.image.filter_blur(factor=1.0)

# 锐化
bpy.ops.image.filter_sharpen(factor=1.0)

# 边缘检测
bpy.ops.image.filter_sobel()

# Laplacian 滤镜
bpy.ops.image.filter_laplacian()

#  emboss 滤镜
bpy.ops.image.filter_emboss(factor=1.0)

# 发现的边缘
bpy.ops.image.filter_sobel()

# 自定义滤镜
bpy.ops.image.filter_custom(use=True)
bpy.ops.image.filter_custom_cancel()

# 应用滤镜
bpy.ops.image.filter_apply()
bpy.ops.image.filter_apply_next()
```

### 图像混合

```python
# 混合
bpy.ops.image.blend_color(color=(1.0, 0.0, 0.0, 1.0))

# 混合因子
bpy.ops.image.curves_point_set(curve_index=0, value=0.5)

# 色阶调整
bpy.ops.image.levels(min=0.0, max=1.0, gamma=1.0)

# 色相/饱和度调整
bpy.ops.image.hue_rotate(hue=0.0)

# 亮度/对比度调整
bpy.ops.image.brightness_contrast(brightness=0, contrast=0)
```

### 图像插图

```python
# 切换插图显示
bpy.ops.image.cycle_uv_tiles()

# 渲染插图
bpy.ops.image.render_border(xmin=0, xmax=100, ymin=0, ymax=100)

# 清除插图
bpy.ops.image.clear_render_border()
```

### 采样

```python
# 采样颜色
bpy.ops.image.sample(x=100, y=100)

# 采样线性
bpy.ops.image.sample_line(xstart=0, ystart=0, xend=100, yend=100, cursor=1002)

# 绘制样本
bpy.ops.image.sample_brush(size=20, x=100, y=100)
```

### 选择操作

```python
# 全选
bpy.ops.image.select_all(action='SELECT')

# 全不选
bpy.ops.image.select_all(action='DESELECT')

# 反选
bpy.ops.image.select_all(action='INVERT')

# 选择边框
bpy.ops.image.select_border(xmin=0, xmax=100, ymin=0, ymax=100, extend=True)

# 选择圆圈
bpy.ops.image.select_circle(x=100, y=100, radius=20, gesture_mode=0)

# 选择套索
bpy.ops.image.select_lasso(path=[(0, 0), (100, 0), (100, 100), (0, 100)], deselect=False, extend=True)
```

### 图像变换

```python
# 变换
bpy.ops.image.transform(mode='TRANSLATE', value=(10.0, 0.0))

# 旋转
bpy.ops.image.transform(mode='ROTATE', value=0.785398)

# 缩放
bpy.ops.image.transform(mode='SCALE', value=(1.5, 1.5))

# 裁剪
bpy.ops.image.transform(mode='CROP', value=(0, 0, 100, 100))

# 变形
bpy.ops.image.transform(mode='WARP', value=(0, 0))
```

### 图像投影

```python
# 投影相机
bpy.ops.image.project_edit(camera=None)

# 投影相机清除
bpy.ops.image.project_view_all()
```

### 图像模板

```python
# 新建模板
bpy.ops.image.new_color(name="Template", width=1024, height=1024, color=(0, 0, 0, 1), alpha=True)

# 编辑模板
bpy.ops.image.save_dirty()
```

## 实际应用示例

### 批量处理纹理

```python
import bpy
import os

def process_textures(input_dir, output_dir, operation='resize', size=(1024, 1024)):
    """批量处理纹理"""
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for filename in os.listdir(input_dir):
        if filename.endswith(('.png', '.jpg', '.jpeg', '.tga')):
            filepath = os.path.join(input_dir, filename)
            
            # 打开图像
            bpy.ops.image.open(filepath=filepath, img = bpy relative=True)
           .context.active_image
            
            if img:
                if operation == 'resize':
                    # 调整大小
                    bpy.ops.image.resize(size=size)
                elif operation == 'crop':
                    # 裁剪到中心
                    width = img.size[0]
                    height = img.size[1]
                    min_dim = min(width, height)
                    x = (width - min_dim) // 2
                    y = (height - min_dim) // 2
                    bpy.ops.image.crop(x=x, y=y, width=min_dim, height=min_dim)
                elif operation == 'invert':
                    # 反转颜色
                    bpy.ops.image.invert(invert_r=True, invert_g=True, invert_b=True)
                
                # 保存
                output_path = os.path.join(output_dir, filename)
                bpy.ops.image.save_as(filepath=output_path, copy=True)
                
                print(f"处理完成: {filename}")

# 使用示例
# process_textures("//textures/input", "//textures/output", 'resize', (2048, 2048))
```

### 创建 UV 贴图

```python
import bpy

def create_uv_map_texture(obj, texture_name="UVMap"):
    """为对象创建 UV 贴图纹理"""
    if not obj.data.uv_layers:
        bpy.ops.mesh.uv_texture_add()
    
    uv_layer = obj.data.uv_layers.active
    
    # 获取 UV 坐标
    uv_coords = []
    for poly in obj.data.polygons:
        for loop_idx in poly.loop_indices:
            uv_coord = obj.data.uv_layers.active.data[loop_idx].uv
            uv_coords.append(uv_coord)
    
    # 创建新图像
    bpy.ops.image.new(name=texture_name, width=1024, height=1024, color=(0.5, 0.5, 0.5, 1.0))
    img = bpy.context.active_image
    
    # 将图像添加到材质
    mat = bpy.data.materials.new(name=f"{obj.name}_UV")
    mat.use_nodes = True
    nodes = mat.node_tree.nodes
    links = mat.node_tree.links
    
    # 创建图像节点
    img_node = nodes.new(type='ShaderNodeTexImage')
    img_node.image = img
    img_node.width_hidden = 200
    
    # 获取主原理 BSDF
    principled = nodes.get("Principled BSDF")
    if principled:
        links.new(img_node.outputs[0], principled.inputs["Base Color"])
    
    # 添加到对象
    if obj.data.materials:
        obj.data.materials[0] = mat
    else:
        obj.data.materials.append(mat)
    
    return img

# 使用示例
# obj = bpy.context.active_object
# img = create_uv_map_texture(obj)
```

### 图像序列动画

```python
import bpy
import os

class ImageSequenceOperator(bpy.types.Operator):
    """图像序列操作符"""
    bl_idname = "image.sequence_animation"
    bl_label = "创建图像序列动画"
    
    directory: bpy.props.StringProperty(subtype="FILE_PATH")
    image_extension: bpy.props.StringProperty(default=".png")
    
    def execute(self, context):
        # 获取目录中的所有图像文件
        files = [f for f in os.listdir(self.directory) 
                 if f.endswith(self.image_extension)]
        files.sort()
        
        if not files:
            self.report({'WARNING'}, "未找到图像文件")
            return {'CANCELLED'}
        
        # 创建图像列表
        image_list = []
        for filename in files:
            filepath = os.path.join(self.directory, filename)
            bpy.ops.image.open(filepath=filepath, relative=True)
            image_list.append(bpy.context.active_image)
        
        # 设置活动图像为第一张
        if image_list:
            context.space_data.image = image_list[0]
            print(f"已加载 {len(image_list)} 张图像")
        
        return {'FINISHED'}
    
    def invoke(self, context, event):
        context.window_manager.fileselect_add(self)
        return {'RUNNING_MODAL'}
```

### 图像批处理操作器

```python
class ImageBatchProcessor(bpy.types.Operator):
    """图像批处理器"""
    bl_idname = "image.batch_processor"
    bl_label = "批处理图像"
    
    operation: bpy.props.EnumProperty(
        name="操作",
        items=[
            ('RESIZE', "调整大小", "调整图像大小"),
            ('CROP', "裁剪", "裁剪图像"),
            ('INVERT', "反色", "反转图像颜色"),
            ('BLUR', "模糊", "应用高斯模糊"),
        ]
    )
    
    width: bpy.props.IntProperty(name="宽度", default=1024, min=1, max=8192)
    height: bpy.props.IntProperty(name="高度", default=1024, min=1, max=8192)
    blur_amount: bpy.props.FloatProperty(name="模糊量", default=0.5, min=0.0, max=10.0)
    
    def execute(self, context):
        # 获取选中的图像
        selected_images = []
        for img in bpy.data.images:
            if img.users > 0:
                selected_images.append(img)
        
        if not selected_images:
            self.report({'WARNING'}, "未找到使用的图像")
            return {'CANCELLED'}
        
        original_image = context.space_data.image
        
        for img in selected_images:
            context.space_data.image = img
            
            if self.operation == 'RESIZE':
                bpy.ops.image.resize(size=(self.width, self.height))
            elif self.operation == 'CROP':
                bpy.ops.image.crop(x=0, y=0, width=self.width, height=self.height)
            elif self.operation == 'INVERT':
                bpy.ops.image.invert(invert_r=True, invert_g=True, invert_b=True)
            elif self.operation == 'BLUR':
                bpy.ops.image.filter_gaussian(threshold=self.blur_amount)
        
        # 恢复活动图像
        context.space_data.image = original_image
        
        self.report({'INFO'}, f"已处理 {len(selected_images)} 张图像")
        return {'FINISHED'}
    
    def invoke(self, context, event):
        return context.window_manager.invoke_props_dialog(self)
    
    def draw(self, context):
        layout = self.layout
        layout.prop(self, "operation")
        
        if self.operation == 'RESIZE':
            layout.prop(self, "width")
            layout.prop(self, "height")
        elif self.operation == 'BLUR':
            layout.prop(self, "blur_amount")
```

### UV 编辑面板

```python
class UVEditPanel(bpy.types.Panel):
    """UV 编辑面板"""
    bl_label = "UV 编辑工具"
    bl_idname = "IMAGE_PT_uv_tools"
    bl_space_type = 'IMAGE_EDITOR'
    bl_region_type = 'UI'
    bl_category = '工具'
    
    def draw(self, context):
        layout = self.layout
        space = context.space_data
        
        # UV 层管理
        row = layout.row()
        row.operator("uv.uv_texture_add", text="添加 UV 层")
        row.operator("uv.uv_texture_remove", text="移除 UV 层")
        
        # UV 选择模式
        layout.label(text="选择模式:")
        row = layout.row(heading="模式")
        row.prop_enum(space, "uv_select_mode", 'VERTEX')
        row.prop_enum(space, "uv_select_mode", 'EDGE')
        row.prop_enum(space, "uv_select_mode", 'FACE')
        row.prop_enum(space, "uv_select_mode", 'ISLAND')
        
        # 对齐工具
        layout.separator()
        layout.label(text="对齐工具:")
        row = layout.row(align=True)
        row.operator("uv.align", text="左对齐").align='LEFT'
        row.operator("uv.align", text="水平居中").align='CENTER_X'
        row.operator("uv.align", text="右对齐").align='RIGHT'
        
        row = layout.row(align=True)
        row.operator("uv.align", text="底对齐").align='BOTTOM'
        row.operator("uv.align", text="垂直居中").align='CENTER_Y'
        row.operator("uv.align", text="顶对齐").align='TOP'
        
        # 分布工具
        layout.separator()
        layout.label(text="分布工具:")
        row = layout.row(align=True)
        row.operator("uv.distribute", text="水平分布").distribute='DISTRIBUTE'
        row.operator("uv.distribute", text="垂直分布").distribute='DISTRIBUTEY'
        
        # 缝合工具
        layout.separator()
        layout.label(text="缝合工具:")
        row = layout.row(align=True)
        row.operator("uv.stitch", text="缝合")
        row.operator("uv.snap_selected_to_cursor", text="到光标")
```

## 最佳实践

### 1. 图像资源管理

```python
def manage_image_resources():
    """管理图像资源"""
    # 查找未使用的图像
    unused_images = [img for img in bpy.data.images if img.users == 0]
    
    # 清理未使用的图像
    for img in unused_images:
        bpy.data.images.remove(img)
        print(f"已清理: {img.name}")
    
    # 打印统计
    print(f"总图像数: {len(bpy.data.images)}")
    print(f"使用中: {len(bpy.data.images) - len(unused_images)}")
```

### 2. 图像路径处理

```python
def relink_images(new_base_dir):
    """重新链接图像"""
    for img in bpy.data.images:
        if img.filepath and not os.path.exists(bpy.path.abspath(img.filepath)):
            # 尝试在新目录中查找
            filename = os.path.basename(img.filepath)
            new_path = os.path.join(new_base_dir, filename)
            
            if os.path.exists(new_path):
                img.filepath = bpy.path.relpath(new_path)
                print(f"已重新链接: {img.name}")
```

### 3. 图像尺寸优化

```python
def optimize_images(max_size=2048):
    """优化图像尺寸"""
    for img in bpy.data.images:
        if img.size[0] > max_size or img.size[1] > max_size:
            # 计算新尺寸
            scale = min(max_size / img.size[0], max_size / img.size[1])
            new_size = (int(img.size[0] * scale), int(img.size[1] * scale))
            
            # 保存原始尺寸
            original_size = img.size
            
            # 调整大小
            bpy.context.space_data.image = img
            bpy.ops.image.resize(size=new_size)
            
            print(f"优化 {img.name}: {original_size} -> {new_size}")
```

## 常见问题

### Q: 图像不显示
A: 检查图像是否已加载，确认 UV 编辑器中选择了正确的图像。

### Q: UV 映射丢失
A: 确保网格有 UV 层，使用 `uv_texture_add` 添加。

### Q: 图像编辑不保存
A: 使用 `save()` 或 `save_as()` 保存更改。

## 相关模块

- [bpy.ops](bpy_ops_detailed_module.md) - Blender 操作符
- [bpy.data](bpy_data_module.md) - 数据访问
- [imbuf](imbuf_module.md) - 图像缓冲操作
- [bpy_extras](bpy_extras_module.md) - 额外工具函数


---
## bpy_ops_uilist

# bpy.ops.uilist - UI List Operations Module

Blender UI 列表操作符，用于管理用户界面列表的选择和操作。

## Overview

`bpy.ops.uilist` 模块包含 UI 列表的操作命令，支持列表项的选择、移动、删除等操作。这个模块主要用于自定义界面和列表控件的交互。

## 核心功能

```python
import bpy

# 选择列表项
bpy.ops.uilist.select_item(index=0, extend=False)

# 选择范围
bpy.ops.uilist.select_item_range(start=0, end=10)

# 激活列表项
bpy.ops.uilist.active_item(index=0)

# 删除列表项
bpy.ops.uilist.delete_item(index=0)

# 移动列表项
bpy.ops.uilist.move_item(direction='UP')

# 复制列表项
bpy.ops.uilist.duplicate_item(index=0)
```

## 实际应用示例

### UI 列表管理器

```python
import bpy

class UIListManager:
    """UI 列表管理器"""
    
    def __init__(self):
        self.lists = {}
    
    def select_item(self, list_id, index, extend=False):
        """选择列表项"""
        bpy.ops.uilist.select_item(
            list_id=list_id,
            index=index,
            extend=extend
        )
    
    def select_range(self, list_id, start, end):
        """选择范围"""
        bpy.ops.uilist.select_item_range(
            list_id=list_id,
            start=start,
            end=end
        )
    
    def select_all(self, list_id):
        """全选"""
        bpy.ops.uilist.select_item(
            list_id=list_id,
            index=-1
        )
    
    def deselect_all(self, list_id):
        """取消选择"""
        bpy.ops.uilist.select_item(
            list_id=list_id,
            index=-2
        )
    
    def invert_selection(self, list_id):
        """反选"""
        bpy.ops.uilist.select_item(
            list_id=list_id,
            index=-3
        )
    
    def activate_item(self, list_id, index):
        """激活列表项"""
        bpy.ops.uilist.active_item(
            list_id=list_id,
            index=index
        )
    
    def delete_item(self, list_id, index):
        """删除列表项"""
        bpy.ops.uilist.delete_item(
            list_id=list_id,
            index=index
        )
    
    def delete_selected(self, list_id):
        """删除选中项"""
        bpy.ops.uilist.delete_item(
            list_id=list_id,
            index=-1
        )
    
    def move_item_up(self, list_id, index):
        """上移"""
        bpy.ops.uilist.move_item(
            list_id=list_id,
            direction='UP',
            index=index
        )
    
    def move_item_down(self, list_id, index):
        """下移"""
        bpy.ops.uilist.move_item(
            list_id=list_id,
            direction='DOWN',
            index=index
        )
    
    def duplicate_item(self, list_id, index):
        """复制"""
        bpy.ops.uilist.duplicate_item(
            list_id=list_id,
            index=index
        )
    
    def move_to_top(self, list_id, index):
        """移到顶部"""
        if index > 0:
            for i in range(index):
                self.move_item_up(list_id, 0)
    
    def move_to_bottom(self, list_id, index):
        """移到底部"""
        # 需要先获取列表长度
        pass
    
    def refresh_list(self, list_id):
        """刷新列表"""
        bpy.ops.uilist.select_item(
            list_id=list_id,
            index=-4
        )
    
    def filter_list(self, list_id, filter_text):
        """过滤列表"""
        bpy.ops.uilist.filter(
            list_id=list_id,
            filter_text=filter_text
        )
    
    def clear_filter(self, list_id):
        """清除过滤"""
        bpy.ops.uilist.filter(
            list_id=list_id,
            filter_text=""
        )
    
    def expand_all(self, list_id):
        """展开所有"""
        bpy.ops.uilist.expand_or_collapse(
            list_id=list_id,
            action='EXPAND'
        )
    
    def collapse_all(self, list_id):
        """折叠所有"""
        bpy.ops.uilist.expand_or_collapse(
            list_id=list_id,
            action='COLLAPSE'
        )


def setup_uilist_manager():
    """设置 UI 列表管理器"""
    manager = UIListManager()
    
    print("UI 列表管理器已设置")
    print("可用功能:")
    print("- select_item: 选择项")
    print("- delete_item: 删除项")
    print("- move_item: 移动项")
    print("- filter_list: 过滤列表")
    
    return manager
```

### 集合列表管理器

```python
import bpy

class CollectionListManager:
    """集合列表管理器"""
    
    def __init__(self):
        self.collections = []
    
    def get_collections(self):
        """获取所有集合"""
        self.collections = [col.name for col in bpy.data.collections]
        return self.collections
    
    def select_collection(self, collection_name):
        """选择集合"""
        collection = bpy.data.collections.get(collection_name)
        if collection:
            bpy.context.view_layer.active_collection = collection
    
    def create_collection(self, name, parent=None):
        """创建集合"""
        if parent:
            parent_col = bpy.data.collections.get(parent)
            if parent_col:
                collection = bpy.data.collections.new(name)
                parent_col.children.link(collection)
                return collection
        
        return bpy.data.collections.new(name)
    
    def delete_collection(self, collection_name):
        """删除集合"""
        collection = bpy.data.collections.get(collection_name)
        if collection:
            # 移除所有对象
            for obj in collection.objects:
                for col in obj.users_collection:
                    col.objects.unlink(obj)
            
            bpy.data.collections.remove(collection)
    
    def duplicate_collection(self, collection_name, new_name):
        """复制集合"""
        collection = bpy.data.collections.get(collection_name)
        if not collection:
            return None
        
        new_collection = bpy.data.collections.new(new_name)
        
        # 复制对象
        for obj in collection.objects:
            new_obj = obj.copy()
            new_obj.data = obj.data.copy() if obj.data else None
            new_collection.objects.link(new_obj)
        
        return new_collection
    
    def move_to_collection(self, object_name, collection_name):
        """移动对象到集合"""
        obj = bpy.data.objects.get(object_name)
        collection = bpy.data.collections.get(collection_name)
        
        if obj and collection:
            # 从旧集合移除
            for col in obj.users_collection:
                col.objects.unlink(obj)
            
            # 添加到新集合
            collection.objects.link(obj)
    
    def add_to_collection(self, object_names, collection_name):
        """添加对象到集合"""
        collection = bpy.data.collections.get(collection_name)
        if not collection:
            return
        
        for obj_name in object_names:
            obj = bpy.data.objects.get(obj_name)
            if obj and obj not in collection.objects:
                collection.objects.link(obj)
    
    def remove_from_collection(self, object_name, collection_name):
        """从集合移除对象"""
        obj = bpy.data.objects.get(object_name)
        collection = bpy.data.collections.get(collection_name)
        
        if obj and collection and obj in collection.objects:
            collection.objects.unlink(obj)
    
    def list_objects_in_collection(self, collection_name):
        """列出集合中的对象"""
        collection = bpy.data.collections.get(collection_name)
        if collection:
            return [obj.name for obj in collection.objects]
        return []
    
    def find_object_collections(self, object_name):
        """查找对象所在的集合"""
        obj = bpy.data.objects.get(object_name)
        if obj:
            return [col.name for col in obj.users_collection]
        return []
    
    def export_collection(self, collection_name, filepath):
        """导出集合"""
        collection = bpy.data.collections.get(collection_name)
        if not collection:
            return None
        
        # 导出到 GLTF
        bpy.ops.wm.gltf_export(
            filepath=filepath,
            export_format='GLTF_SEPARATE',
            selected=True,
            use_selection=True
        )
        
        return filepath
    
    def import_collection(self, filepath, collection_name="Imported"):
        """导入集合"""
        # 导入 GLTF
        bpy.ops.wm.gltf_import(filepath=filepath)
        
        # 获取导入的集合
        if bpy.context.selected_objects:
            collection = bpy.data.collections.get(collection_name)
            if not collection:
                collection = bpy.data.collections.new(collection_name)
            
            for obj in bpy.context.selected_objects:
                if obj not in collection.objects:
                    collection.objects.link(obj)
        
        return bpy.data.collections.get(collection_name)


def setup_collection_manager():
    """设置集合管理器"""
    manager = CollectionListManager()
    
    manager.get_collections()
    print("集合列表管理器已设置")
    print(f"当前集合: {manager.collections}")
    
    return manager
```

### 材质列表管理器

```python
import bpy

class MaterialListManager:
    """材质列表管理器"""
    
    def __init__(self):
        self.materials = []
    
    def get_materials(self):
        """获取所有材质"""
        self.materials = [mat.name for mat in bpy.data.materials]
        return self.materials
    
    def select_material(self, material_name):
        """选择材质"""
        material = bpy.data.materials.get(material_name)
        if material:
            bpy.context.active_object.active_material = material
    
    def create_material(self, name, base_color=(0.5, 0.5, 0.5, 1)):
        """创建材质"""
        material = bpy.data.materials.new(name=name)
        material.use_nodes = True
        
        # 设置基础颜色
        nodes = material.node_tree.nodes
        principled = nodes.get('Principled BSDF')
        if principled:
            principled.inputs['Base Color'].default_value = base_color
        
        return material
    
    def duplicate_material(self, material_name, new_name):
        """复制材质"""
        material = bpy.data.materials.get(material_name)
        if not material:
            return None
        
        new_material = material.copy()
        new_material.name = new_name
        return new_material
    
    def delete_material(self, material_name):
        """删除材质"""
        material = bpy.data.materials.get(material_name)
        if material:
            bpy.data.materials.remove(material)
    
    def assign_material(self, object_name, material_name, material_index=0):
        """分配材质"""
        obj = bpy.data.objects.get(object_name)
        material = bpy.data.materials.get(material_name)
        
        if obj and material:
            # 确保材质槽存在
            while len(obj.material_slots) <= material_index:
                obj.data.materials.append(None)
            
            obj.material_slots[material_index].material = material
    
    def get_object_material(self, object_name, slot_index=0):
        """获取对象材质"""
        obj = bpy.data.objects.get(object_name)
        if obj and slot_index < len(obj.material_slots):
            return obj.material_slots[slot_index].material
        return None
    
    def create_material_override(self, material_name, object_name):
        """创建材质覆盖"""
        material = bpy.data.materials.get(material_name)
        obj = bpy.data.objects.get(object_name)
        
        if material and obj:
            override_mat = material.copy()
            override_mat.name = f"{material_name}_Override"
            return self.assign_material(object_name, override_mat.name)
    
    def link_material_to_all_objects(self, material_name):
        """链接材质到所有对象"""
        material = bpy.data.materials.get(material_name)
        if not material:
            return
        
        for obj in bpy.data.objects:
            if obj.type in ['MESH', 'CURVE', 'SURFACE', 'FONT']:
                if material not in obj.data.materials:
                    obj.data.materials.append(material)
    
    def export_materials(self, filepath, format='MTL'):
        """导出材质"""
        if format == 'MTL':
            # 导出为 MTL 格式
            content = ""
            for mat in bpy.data.materials:
                content += f"newmtl {mat.name}\n"
                nodes = mat.node_tree.nodes if mat.use_nodes else []
                principled = nodes.get('Principled BSDF')
                if principled:
                    color = principled.inputs['Base Color'].default_value
                    content += f"Kd {color[0]} {color[1]} {color[2]}\n"
                content += "\n"
            
            with open(filepath, 'w') as f:
                f.write(content)
        
        return filepath
    
    def import_materials(self, filepath):
        """导入材质"""
        # 导入 MTL 格式
        pass
    
    def list_unused_materials(self):
        """列出未使用的材质"""
        unused = []
        for mat in bpy.data.materials:
            used = False
            for obj in bpy.data.objects:
                if obj.data and hasattr(obj.data, 'materials'):
                    if mat in obj.data.materials:
                        used = True
                        break
            if not used:
                unused.append(mat.name)
        return unused
    
    def remove_unused_materials(self):
        """移除未使用的材质"""
        unused = self.list_unused_materials()
        for name in unused:
            self.delete_material(name)
        return len(unused)


def setup_material_manager():
    """设置材质管理器"""
    manager = MaterialListManager()
    
    manager.get_materials()
    print("材质列表管理器已设置")
    print(f"当前材质: {manager.materials}")
    
    return manager
```

### 数据块列表工具

```python
import bpy

class DataBlockListTool:
    """数据块列表工具"""
    
    data_types = [
        'ACTIONS', 'ARMATURES', 'BRUSHES', 'CAMERAS', 'CURVES',
        'FONTS', 'GREASEPENCIL', 'IMAGES', 'LIGHT_PROBES',
        'LIGHTS', 'LINESTYLES', 'MASKS', 'MATERIALS', 'MESHES',
        'METABALLS', 'NODETREES', 'OBJECTS', 'PAINT_CURVES',
        'PALETTES', 'PARTICLES', 'SCENES', 'SOUNDS', 'SPEAKERS',
        'TEXTURES', 'WORKSPACES', 'WORLDS'
    ]
    
    def get_data_blocks(self, data_type):
        """获取数据块"""
        data_type = data_type.upper()
        
        if data_type == 'ACTIONS':
            return bpy.data.actions
        elif data_type == 'ARMATURES':
            return bpy.data.armatures
        elif data_type == 'BRUSHES':
            return bpy.data.brushes
        elif data_type == 'CAMERAS':
            return bpy.data.cameras
        elif data_type == 'CURVES':
            return bpy.data.curves
        elif data_type == 'IMAGES':
            return bpy.data.images
        elif data_type == 'LIGHTS':
            return bpy.data.lights
        elif data_type == 'MATERIALS':
            return bpy.data.materials
        elif data_type == 'MESHES':
            return bpy.data.meshes
        elif data_type == 'OBJECTS':
            return bpy.data.objects
        elif data_type == 'SCENES':
            return bpy.data.scenes
        elif data_type == 'SOUNDS':
            return bpy.data.sounds
        elif data_type == 'TEXTURES':
            return bpy.data.textures
        elif data_type == 'WORLDS':
            return bpy.data.worlds
        
        return None
    
    def list_data_blocks(self, data_type):
        """列出数据块"""
        blocks = self.get_data_blocks(data_type)
        if blocks:
            return [block.name for block in blocks]
        return []
    
    def get_data_block_count(self, data_type):
        """获取数据块数量"""
        blocks = self.get_data_blocks(data_type)
        return len(blocks) if blocks else 0
    
    def delete_data_block(self, data_type, name):
        """删除数据块"""
        blocks = self.get_data_blocks(data_type)
        if blocks:
            block = blocks.get(name)
            if block:
                blocks.remove(block)
                return True
        return False
    
    def rename_data_block(self, data_type, old_name, new_name):
        """重命名数据块"""
        blocks = self.get_data_blocks(data_type)
        if blocks:
            block = blocks.get(old_name)
            if block:
                block.name = new_name
                return True
        return False
    
    def duplicate_data_block(self, data_type, name, new_name):
        """复制数据块"""
        blocks = self.get_data_blocks(data_type)
        if blocks:
            block = blocks.get(name)
            if block:
                new_block = block.copy()
                new_block.name = new_name
                return new_block
        return None
    
    def find_data_block_usage(self, data_type, name):
        """查找数据块使用情况"""
        blocks = self.get_data_blocks(data_type)
        if blocks:
            block = blocks.get(name)
            if block:
                users = []
                for obj in bpy.data.objects:
                    if data_type == 'MATERIALS':
                        if block in obj.data.materials:
                            users.append(obj.name)
                    elif data_type == 'MESHES':
                        if obj.data == block:
                            users.append(obj.name)
                return users
        return []
    
    def clear_unused_data_blocks(self, data_type):
        """清理未使用的数据块"""
        blocks = self.get_data_blocks(data_type)
        if not blocks:
            return 0
        
        count = 0
        for block in list(blocks):
            if block.users == 0:
                blocks.remove(block)
                count += 1
        
        return count
    
    def export_data_blocks(self, data_type, filepath):
        """导出数据块"""
        blocks = self.get_data_blocks(data_type)
        if not blocks:
            return None
        
        import json
        
        data = []
        for block in blocks:
            data.append({
                'name': block.name,
                'users': block.users
            })
        
        with open(filepath, 'w') as f:
            json.dump(data, f, indent=2)
        
        return filepath
    
    def get_all_data_stats(self):
        """获取所有数据统计"""
        stats = {}
        for data_type in self.data_types:
            count = self.get_data_block_count(data_type)
            if count > 0:
                stats[data_type] = count
        return stats


def setup_datablock_tool():
    """设置数据块工具"""
    tool = DataBlockListTool()
    
    stats = tool.get_all_data_stats()
    print("数据块列表工具已设置")
    print(f"统计数据: {stats}")
    
    return tool
```


