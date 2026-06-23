# bpy_ops_object_transform

---
## bpy_ops_object

# bpy.ops.object - Object Operations Module

Blender 对象操作符，用于创建、选择、变换、复制和管理 3D 对象。

## Overview

`bpy.ops.object` 模块是 Blender Python API 中最核心的操作符模块之一，包含了场景中对象的所有基本操作命令。这个模块涵盖了对象的创建、选择、变换、复制、删除、分组、对齐等常用功能。

## 核心功能

### 对象选择操作

```python
import bpy

# 全选
bpy.ops.object.select_all(action='SELECT')

# 全不选
bpy.ops.object.select_all(action='DESELECT')

# 反选
bpy.ops.object.select_all(action='INVERT')

# 选择活动对象
bpy.ops.object.select_active(type='MESH')

# 选择边框
bpy.ops.object.select_border(gesture_mode=0, xmin=100, xmax=300, ymin=100, ymax=300)

# 选择圆形
bpy.ops.object.select_circle(x=200, y=200, radius=50)

# 选择多边形
bpy.ops.object.select_lasso(path=[(100, 100), (200, 100), (200, 200), (100, 200)])

# 选择更多
bpy.ops.object.select_more(use_face_step=True, use_connected=False)

# 选择更少
bpy.ops.object.select_less(use_face_step=True)

# 选择相似
bpy.ops.object.select_hierarchy(direction='CHILD', extend=True)
bpy.ops.object.select_hierarchy(direction='PARENT', extend=True)

# 选择类型
bpy.ops.object.select_by_type(type='MESH')
bpy.ops.object.select_by_type(type='CURVE')
bpy.ops.object.select_by_type(type='ARMATURE')
bpy.ops.object.select_by_type(type='CAMERA')
bpy.ops.object.select_by_type(type='LIGHT')

def select_all_objects():
    """选择所有对象"""
    bpy.ops.object.select_all(action='SELECT')
    return bpy.context.selected_objects

def deselect_all():
    """取消所有选择"""
    bpy.ops.object.select_all(action='DESELECT')

def select_by_name(name):
    """按名称选择对象"""
    deselect_all()
    if name in bpy.data.objects:
        obj = bpy.data.objects[name]
        obj.select_set(True)
        bpy.context.view_layer.objects.active = obj

def select_by_prefix(prefix):
    """按前缀选择对象"""
    deselect_all()
    for obj in bpy.data.objects:
        if obj.name.startswith(prefix):
            obj.select_set(True)
```

### 对象变换操作

```python
# 变换模式
bpy.ops.object.mode_set(mode='OBJECT')
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.object.mode_set(mode='POSE')
bpy.ops.object.mode_set(mode='SCULPT')
bpy.ops.object.mode_set(mode='VERTEX_PAINT')
bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
bpy.ops.object.mode_set(mode='TEXTURE_PAINT')

# 应用变换
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
bpy.ops.object.transform_apply(location=True, rotation=False, scale=False)
bpy.ops.object.transform_apply(location=False, rotation=True, scale=False)
bpy.ops.object.transform_apply(location=False, rotation=False, scale=True)

# 重置变换
bpy.ops.object.location_clear(clear_delta=False)
bpy.ops.object.rotation_clear(clear_delta=False)
bpy.ops.object.scale_clear(clear_delta=False)
bpy.ops.object.transform_clear(type='ALL')

# 变换到原点
bpy.ops.object.origin_set(type='ORIGIN_GEOMETRY', center='MEDIAN')
bpy.ops.object.origin_set(type='ORIGIN_CURSOR', center='MEDIAN')
bpy.ops.object.origin_set(type='ORIGIN_CENTER_OF_MASS', center='MEDIAN')
bpy.ops.object.origin_set(type='ORIGIN_3D_CURSOR', center='MEDIAN')

# 父级设置
bpy.ops.object.parent_set(type='OBJECT')
bpy.ops.object.parent_set(type='BONE_RELATIVE')
bpy.ops.object.parent_set(type='BONE_SELECTED')
bpy.ops.object.parent_clear(type='CLEAR_KEEP_TRANSFORM')
bpy.ops.object.parent_clear(type='CLEAR_INVERSE')

# 约束添加
bpy.ops.object.constraint_add(type='COPY_LOCATION')
bpy.ops.object.constraint_add(type='COPY_ROTATION')
bpy.ops.object.constraint_add(type='COPY_SCALE')
bpy.ops.object.constraint_add(type='TRACK_TO')
bpy.ops.object.constraint_add(type='LOOK_AT')
bpy.ops.object.constraint_add(type='IK')
bpy.ops.object.constraint_add(type='FOLLOW_PATH')

def apply_all_transforms():
    """应用所有变换"""
    bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)

def reset_to_origin(obj):
    """重置对象到原点"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.location_clear()
    bpy.ops.object.rotation_clear()
    bpy.ops.object.scale_clear()

def set_parent(child, parent):
    """设置父级"""
    bpy.ops.object.select_all(action='DESELECT')
    child.select_set(True)
    parent.select_set(True)
    bpy.context.view_layer.objects.active = child
    bpy.ops.object.parent_set(type='OBJECT')
```

### 对象创建操作

```python
import bpy
import math

# 添加空对象
bpy.ops.object.empty_add(type='PLAIN_AXES')
bpy.ops.object.empty_add(type='ARROWS')
bpy.ops.object.empty_add(type='SINGLE_ARROW')
bpy.ops.object.empty_add(type='CIRCLE')
bpy.ops.object.empty_add(type='CUBE')
bpy.ops.object.empty_add(type='SPHERE')
bpy.ops.object.empty_add(type='CONE')

# 添加摄像机
bpy.ops.object.camera_add(location=(0, 0, 5))
bpy.ops.object.camera_add(rotation=(math.radians(90), 0, 0))

# 添加灯光
bpy.ops.object.light_add(type='POINT')
bpy.ops.object.light_add(type='SUN')
bpy.ops.object.light_add(type='SPOT')
bpy.ops.object.light_add(type='AREA')

# 添加网格
bpy.ops.object.mesh_add()
bpy.ops.object.primitive_cube_add()
bpy.ops.object.primitive_uv_sphere_add()
bpy.ops.object.primitive_ico_sphere_add()
bpy.ops.object.primitive_cylinder_add()
bpy.ops.object.primitive_cone_add()
bpy.ops.object.primitive_torus_add()
bpy.ops.object.primitive_circle_add()
bpy.ops.object.primitive_cube_add(size=2, location=(0, 0, 0))
bpy.ops.object.primitive_grid_add(x_subdivisions=10, y_subdivisions=10)
bpy.ops.object.primitive_monkey_add()

# 添加曲线
bpy.ops.object.curve_add()
bpy.ops.object.primitive_bezier_curve_add()
bpy.ops.object.primitive_bezier_circle_add()
bpy.ops.object.primitive_nurbs_path_add()
bpy.ops.object.primitive_nurbs_circle_add()

# 添加曲面
bpy.ops.object.surface_add()
bpy.ops.object.primitive_nurbs_surface_circle_add()
bpy.ops.object.primitive_nurbs_surface_path_add()

# 添加文字
bpy.ops.object.text_add()
bpy.ops.object.text_add(enter_editmode=True)

# 添加骨骼
bpy.ops.object.armature_add()
bpy.ops.object.armature_add(enter_editmode=True)

# 添加粒子系统
bpy.ops.object.particle_system_add()

# 添加毛发
bpy.ops.object.hair_add()

# 添加 Metaball
bpy.ops.object.metaball_add(type='BALL')
bpy.ops.object.metaball_add(type='CAPSULE')
bpy.ops.object.metaball_add(type='PLANE')
bpy.ops.object.metaball_add(type='ELLIPSOID')

def create_cube(name="Cube", size=1.0, location=(0, 0, 0)):
    """创建立方体"""
    bpy.ops.mesh.primitive_cube_add(size=size, location=location)
    obj = bpy.context.active_object
    obj.name = name
    return obj

def create_sphere(name="Sphere", radius=1.0, segments=32, rings=16, location=(0, 0, 0)):
    """创建球体"""
    bpy.ops.mesh.primitive_uv_sphere_add(radius=radius, segments=segments, ring_count=rings, location=location)
    obj = bpy.context.active_object
    obj.name = name
    return obj

def create_cylinder(name="Cylinder", radius=1.0, depth=2.0, vertices=32, location=(0, 0, 0)):
    """创建圆柱体"""
    bpy.ops.mesh.primitive_cylinder_add(radius=radius, depth=depth, vertices=vertices, location=location)
    obj = bpy.context.active_object
    obj.name = name
    return obj

def create_light(name="Light", type='POINT', energy=100, location=(0, 0, 5)):
    """创建灯光"""
    bpy.ops.object.light_add(type=type, location=location)
    obj = bpy.context.active_object
    obj.name = name
    obj.data.energy = energy
    return obj

def create_camera(name="Camera", location=(0, 0, 5), rotation=(0, 0, 0)):
    """创建摄像机"""
    bpy.ops.object.camera_add(location=location, rotation=rotation)
    obj = bpy.context.active_object
    obj.name = name
    bpy.context.scene.camera = obj
    return obj
```

### 对象复制操作

```python
# 复制
bpy.ops.object.duplicate(linked=False, mode='TRANSLATION')
bpy.ops.object.duplicate(linked=True, mode='TRANSLATION')

# 复制并移动
bpy.ops.object.duplicate_move(OBJECT_OT_duplicate={"linked": False, "mode": 'TRANSLATION'}, TRANSFORM_OT_translate={"value": (1, 0, 0)})

# 实例化
bpy.ops.object.duplicates_make_real()

# 实例化集合
bpy.ops.object.duplicates_upstream()

# 复制层级
bpy.ops.object.duplicate_hierarchy(mode='CHILD')

# 浅复制
bpy.ops.object.shade_flat()
bpy.ops.object.shade_smooth()

def duplicate_object(obj, offset=(1, 0, 0)):
    """复制对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.duplicate()
    new_obj = bpy.context.active_object
    new_obj.location = (obj.location[0] + offset[0], obj.location[1] + offset[1], obj.location[2] + offset[2])
    return new_obj

def duplicate_linked(obj):
    """浅复制对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.duplicate(linked=True)
    return bpy.context.active_object

def create_array(obj, count=5, offset=(1, 0, 0)):
    """创建线性阵列"""
    objects = []
    for i in range(count):
        new_obj = duplicate_object(obj, (offset[0] * i, offset[1] * i, offset[2] * i))
        objects.append(new_obj)
    return objects
```

### 对象删除操作

```python
# 删除
bpy.ops.object.delete(use_global=False)
bpy.ops.object.delete(use_global=True)

# 删除未使用的数据
bpy.ops.object.data_objects_delete()

# 清除未使用的数据块
bpy.ops.object.data_gpencil_delete()

def delete_object(obj, use_global=False):
    """删除对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.delete(use_global=use_global)

def delete_selected():
    """删除选中的对象"""
    bpy.ops.object.delete(use_global=False)

def delete_all_objects():
    """删除所有对象"""
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete(use_global=False)

def delete_by_type(obj_type):
    """按类型删除对象"""
    for obj in bpy.data.objects:
        if obj.type == obj_type:
            bpy.data.objects.remove(obj)
```

### 对象分组操作

```python
# 添加到集合
bpy.ops.object.collection_link(collection="Collection")

# 从集合取消链接
bpy.ops.object.collection_unlink(collection="Collection")

# 添加新集合
bpy.ops.object.collection_add()

# 移出主集合
bpy.ops.object.move_to_collection(collection_index=0, is_new=True, new_collection_name="New Collection")

def add_to_collection(obj, collection_name):
    """添加对象到集合"""
    if collection_name not in bpy.data.collections:
        bpy.ops.object.collection_add()
        bpy.context.scene.active_collection.name = collection_name
    
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.collection_link(collection=collection_name)

def move_to_collection(obj, collection_name):
    """移动对象到集合"""
    if collection_name not in bpy.data.collections:
        bpy.ops.object.collection_add()
    
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    for i, coll in enumerate(bpy.data.collections):
        if coll.name == collection_name:
            bpy.ops.object.move_to_collection(collection_index=i)
            break

def create_collection(name, objects=None):
    """创建集合并添加对象"""
    bpy.ops.object.collection_add()
    new_collection = bpy.context.scene.active_collection
    new_collection.name = name
    
    if objects:
        for obj in objects:
            move_to_collection(obj, name)
    
    return new_collection
```

## 对象变换操作

### 移动操作

```python
import bpy
import math

# 移动
bpy.ops.transform.translate(value=(1, 0, 0))

# 移动到原点
bpy.ops.object.location_clear()

# 设置位置
def set_location(obj, x, y, z):
    """设置对象位置"""
    obj.location = (x, y, z)
    obj.keyframe_insert(data_path="location")

def move_object(obj, dx, dy, dz):
    """移动对象"""
    obj.location.x += dx
    obj.location.y += dy
    obj.location.z += dz

def move_to_origin(obj):
    """移动对象到原点"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.location_clear()

def animate_move(obj, start_pos, end_pos, start_frame=1, end_frame=30):
    """创建移动动画"""
    obj.location = start_pos
    obj.keyframe_insert(data_path="location", frame=start_frame)
    
    obj.location = end_pos
    obj.keyframe_insert(data_path="location", frame=end_frame)

def move_along_path(obj, path_points, frames_per_point=10):
    """沿路径移动对象"""
    for i, point in enumerate(path_points):
        frame = i * frames_per_point + 1
        obj.location = point
        obj.keyframe_insert(data_path="location", frame=frame)
```

### 旋转操作

```python
# 旋转
bpy.ops.transform.rotate(value=math.radians(45), orient_axis='Z')

# 旋转模式
bpy.ops.object.rotation_clear(clear_delta=False)

# 设置旋转
def set_rotation(obj, euler=None, quaternion=None):
    """设置对象旋转"""
    if euler:
        obj.rotation_euler = euler
    elif quaternion:
        obj.rotation_quaternion = quaternion

def rotate_object(obj, angle, axis='Z'):
    """旋转对象"""
    if axis == 'X':
        obj.rotation_euler.x += math.radians(angle)
    elif axis == 'Y':
        obj.rotation_euler.y += math.radians(angle)
    elif axis == 'Z':
        obj.rotation_euler.z += math.radians(angle)

def reset_rotation(obj):
    """重置旋转"""
    obj.rotation_euler = (0, 0, 0)

def animate_rotation(obj, start_angle, end_angle, axis='Z', start_frame=1, end_frame=30):
    """创建旋转动画"""
    if axis == 'X':
        obj.rotation_euler.x = math.radians(start_angle)
        obj.keyframe_insert(data_path="rotation_euler", frame=start_frame)
        obj.rotation_euler.x = math.radians(end_angle)
        obj.keyframe_insert(data_path="rotation_euler", frame=end_frame)
    elif axis == 'Y':
        obj.rotation_euler.y = math.radians(start_angle)
        obj.keyframe_insert(data_path="rotation_euler", frame=start_frame)
        obj.rotation_euler.y = math.radians(end_angle)
        obj.keyframe_insert(data_path="rotation_euler", frame=end_frame)
    elif axis == 'Z':
        obj.rotation_euler.z = math.radians(start_angle)
        obj.keyframe_insert(data_path="rotation_euler", frame=start_frame)
        obj.rotation_euler.z = math.radians(end_angle)
        obj.keyframe_insert(data_path="rotation_euler", frame=end_frame)

def spin_object(obj, angle, axis='Z', steps=36):
    """旋转扫描创建几何体"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    for i in range(1, steps):
        bpy.ops.object.duplicate()
        bpy.ops.transform.rotate(value=angle / steps, orient_axis=axis)
    
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.join()
```

### 缩放操作

```python
# 缩放
bpy.ops.transform.resize(value=(2, 2, 2))

# 缩放重置
bpy.ops.object.scale_clear(clear_delta=False)

# 设置缩放
def set_scale(obj, x, y, z):
    """设置对象缩放"""
    obj.scale = (x, y, z)

def scale_object(obj, factor):
    """缩放对象"""
    obj.scale.x *= factor
    obj.scale.y *= factor
    obj.scale.z *= factor

def reset_scale(obj):
    """重置缩放"""
    obj.scale = (1, 1, 1)

def animate_scale(obj, start_scale, end_scale, start_frame=1, end_frame=30):
    """创建缩放动画"""
    obj.scale = start_scale
    obj.keyframe_insert(data_path="scale", frame=start_frame)
    obj.scale = end_scale
    obj.keyframe_insert(data_path="scale", frame=end_frame)

def scale_along_axis(obj, factor, axis='Z'):
    """沿轴缩放"""
    if axis == 'X':
        obj.scale.x *= factor
    elif axis == 'Y':
        obj.scale.y *= factor
    elif axis == 'Z':
        obj.scale.z *= factor
    elif axis == 'ALL':
        scale_object(obj, factor)
```

## 对象对齐和分布

```python
# 对齐视图
bpy.ops.object.align(bb_quality=True, align_mode='OPT_2', relative_to='OPT_1')

# 对齐到活动对象
bpy.ops.object.align(align_mode='OPT_1', relative_to='OPT_3')

# 分布
bpy.ops.object.distribute()

# 收集到光标
bpy.ops.object.cursor_on_selected()

def align_objects(objects, alignment='CENTER'):
    """对齐多个对象"""
    if not objects:
        return
    
    bpy.ops.object.select_all(action='DESELECT')
    for obj in objects:
        obj.select_set(True)
    bpy.context.view_layer.objects.active = objects[0]
    
    if alignment == 'CENTER':
        bpy.ops.object.align(align_mode='OPT_1', relative_to='OPT_4')
    elif alignment == 'MIN':
        bpy.ops.object.align(align_mode='OPT_4', relative_to='OPT_4')
    elif alignment == 'MAX':
        bpy.ops.object.align(align_mode='OPT_3', relative_to='OPT_4')

def distribute_horizontally(objects):
    """水平分布对象"""
    if not objects:
        return
    
    bpy.ops.object.select_all(action='DESELECT')
    for obj in objects:
        obj.select_set(True)
    bpy.context.view_layer.objects.active = objects[0]
    
    bpy.ops.object.distribute()

def center_objects(objects):
    """将对象居中"""
    for obj in objects:
        obj.location = (0, 0, 0)
```

## 对象可见性和显示

```python
# 隐藏
bpy.ops.object.hide_view_clear()
bpy.ops.object.hide_view_set(unselected=False)

# 选择可见性
bpy.ops.object.select大明王朝

# 可渲染性
bpy.ops.object.render_deny_clear()
bpy.ops.object.render_deny_set()

# 显示类型
bpy.ops.object.shade_flat()
bpy.ops.object.shade_smooth()

# 线框显示
bpy.ops.object.display_type_set(type='WIRE')
bpy.ops.object.display_type_set(type='SOLID')
bpy.ops.object.display_type_set(type='BOUNDS')

def hide_selected():
    """隐藏选中的对象"""
    bpy.ops.object.hide_view_set(unselected=False)

def hide_unselected():
    """隐藏未选中的对象"""
    bpy.ops.object.hide_view_set(unselected=True)

def show_all_hidden():
    """显示所有隐藏的对象"""
    bpy.ops.object.hide_view_clear()

def set_display_type(obj, display_type='SOLID'):
    """设置显示类型"""
    obj.display_type = display_type

def set_shading(obj, smooth=True):
    """设置平滑着色"""
    if smooth:
        bpy.ops.object.shade_smooth()
    else:
        bpy.ops.object.shade_flat()
```

## 高级对象操作

### 骨骼操作

```python
import bpy

# 进入编辑模式
bpy.ops.object.mode_set(mode='EDIT')

# 进入姿态模式
bpy.ops.object.mode_set(mode='POSE')

# 选择骨骼
bpy.ops.pose.select_all(action='SELECT')
bpy.ops.pose.select_all(action='DESELECT')
bpy.ops.pose.select_hierarchy(direction='PARENT', extend=True)
bpy.ops.pose.select_hierarchy(direction='CHILD', extend=True)

# 应用姿态
bpy.ops.pose.armature_apply()
bpy.ops.pose.apply_pose_use_current()

# 复制姿态
bpy.ops.pose.copy()

# 粘贴姿态
bpy.ops.pose.paste()

# 镜像姿态
bpy.ops.pose.flip(left_side=True, right_side=True)

# 重置姿态
bpy.ops.pose.rot_clear()
bpy.ops.pose.loc_clear()
bpy.ops.pose.scale_clear()

# 关键帧
bpy.ops.pose.keyframe_insert()
bpy.ops.pose.keyframe_delete()

def select_all_bones():
    """选择所有骨骼"""
    bpy.ops.pose.select_all(action='SELECT')

def deselect_all_bones():
    """取消选择所有骨骼"""
    bpy.ops.pose.select_all(action='DESELECT')

def select_bone_chain(start_bone, direction='CHILD'):
    """选择骨骼链"""
    bpy.ops.pose.select_all(action='DESELECT')
    bpy.context.active_pose_bone = start_bone
    bpy.ops.pose.select_hierarchy(direction=direction, extend=True)

def copy_pose(source_pose_obj, target_pose_obj):
    """复制姿态"""
    bpy.ops.object.select_all(action='DESELECT')
    source_pose_obj.select_set(True)
    bpy.context.view_layer.objects.active = source_pose_obj
    bpy.ops.pose.copy()
    
    bpy.ops.object.select_all(action='DESELECT')
    target_pose_obj.select_set(True)
    bpy.context.view_layer.objects.active = target_pose_obj
    bpy.ops.pose.paste()

def mirror_pose(pose_obj):
    """镜像姿态"""
    bpy.ops.object.select_all(action='DESELECT')
    pose_obj.select_set(True)
    bpy.context.view_layer.objects.active = pose_obj
    bpy.ops.pose.flip()
```

### 约束操作

```python
import bpy

def add_constraint(obj, constraint_type, name="Constraint"):
    """添加约束"""
    constraint = obj.constraints.new(type=constraint_type)
    constraint.name = name
    return constraint

def add_copy_location(obj, target, name="CopyLocation"):
    """添加位置复制约束"""
    constraint = add_constraint(obj, 'COPY_LOCATION', name)
    constraint.target = target
    constraint.influence = 1.0
    return constraint

def add_copy_rotation(obj, target, name="CopyRotation"):
    """添加旋转复制约束"""
    constraint = add_constraint(obj, 'COPY_ROTATION', name)
    constraint.target = target
    constraint.influence = 1.0
    return constraint

def add_copy_scale(obj, target, name="CopyScale"):
    """添加缩放复制约束"""
    constraint = add_constraint(obj, 'COPY_SCALE', name)
    constraint.target = target
    constraint.influence = 1.0
    return constraint

def add_track_to(obj, target, name="TrackTo"):
    """添加朝向约束"""
    constraint = add_constraint(obj, 'TRACK_TO', name)
    constraint.target = target
    constraint.track = 'Z'
    constraint.up = 'Y'
    return constraint

def add_look_at(obj, target, name="LookAt"):
    """添加注视约束"""
    constraint = add_constraint(obj, 'LOOK_AT', name)
    constraint.target = target
    constraint.track_axis = 'TRACK_Z'
    constraint.up_axis = 'UP_Y'
    return constraint

def add_follow_path(obj, path, name="FollowPath"):
    """添加路径跟随约束"""
    constraint = add_constraint(obj, 'FOLLOW_PATH', name)
    constraint.target = path
    constraint.use_curve_follow = True
    constraint.use_curve_radius = True
    return constraint

def add_ik(obj, target, name="IK", chain_count=2):
    """添加反向动力学约束"""
    constraint = add_constraint(obj, 'IK', name)
    constraint.target = target
    constraint.chain_count = chain_count
    constraint.use_tail = False
    return constraint

def remove_constraint(obj, constraint_name):
    """移除约束"""
    for constraint in obj.constraints:
        if constraint.name == constraint_name:
            obj.constraints.remove(constraint)
            return True
    return False

def list_constraints(obj):
    """列出所有约束"""
    for constraint in obj.constraints:
        print(f"约束: {constraint.name}, 类型: {constraint.type}")
```

## 修饰器操作

```python
import bpy

def add_modifier(obj, modifier_type, name="Modifier"):
    """添加修饰器"""
    modifier = obj.modifiers.new(name=name, type=modifier_type)
    return modifier

def add_subdivision(obj, name="Subdivision", levels=1):
    """添加细分表面修饰器"""
    modifier = add_modifier(obj, 'SUBSURF', name)
    modifier.levels = levels
    modifier.render_levels = levels
    return modifier

def add_array(obj, name="Array", count=5):
    """添加数组修饰器"""
    modifier = add_modifier(obj, 'ARRAY', name)
    modifier.count = count
    return modifier

def add_bevel(obj, name="Bevel", width=0.1):
    """添加倒角修饰器"""
    modifier = add_modifier(obj, 'BEVEL', name)
    modifier.width = width
    return modifier

def add_solidify(obj, name="Solidify", thickness=0.1):
    """添加实体化修饰器"""
    modifier = add_modifier(obj, 'SOLIDIFY', name)
    modifier.thickness = thickness
    return modifier

def add_screw(obj, name="Screw", angle=360):
    """添加螺旋修饰器"""
    modifier = add_modifier(obj, 'SCREW', name)
    modifier.angle = math.radians(angle)
    return modifier

def add_skin(obj, name="Skin"):
    """添加皮肤修饰器"""
    modifier = add_modifier(obj, 'SKIN', name)
    return modifier

def add_decimate(obj, name="Decimate", ratio=0.5):
    """添加三角面精简修饰器"""
    modifier = add_modifier(obj, 'DECIMATE', name)
    modifier.ratio = ratio
    return modifier

def add_cast(obj, name="Cast", factor=1.0):
    """添加投射修饰器"""
    modifier = add_modifier(obj, 'CAST', name)
    modifier.factor = factor
    return modifier

def add_simple_deform(obj, name="SimpleDeform", angle=math.radians(45)):
    """添加简单变形修饰器"""
    modifier = add_modifier(obj, 'SIMPLE_DEFORM', name)
    modifier.angle = angle
    return modifier

def add_displace(obj, texture=None, name="Displace", strength=0.5):
    """添加置换修饰器"""
    modifier = add_modifier(obj, 'DISPLACE', name)
    modifier.texture = texture
    modifier.strength = strength
    return modifier

def add_wireframe(obj, name="Wireframe", thickness=0.05):
    """添加线框修饰器"""
    modifier = add_modifier(obj, 'WIREFRAME', name)
    modifier.thickness = thickness
    return modifier

def apply_modifiers(obj):
    """应用所有修饰器"""
    mesh = obj.to_mesh()
    obj.data = mesh

def remove_modifier(obj, modifier_name):
    """移除修饰器"""
    for modifier in obj.modifiers:
        if modifier.name == modifier_name:
            obj.modifiers.remove(modifier)
            return True
    return False

def list_modifiers(obj):
    """列出所有修饰器"""
    for modifier in obj.modifiers:
        print(f"修饰器: {modifier.name}, 类型: {modifier.type}")
```

## 对象工程面板

```python
class ObjectEngineeringPanel(bpy.types.Panel):
    """对象工程面板"""
    bl_label = "对象工具"
    bl_idname = "OBJECT_PT_engineering_tools"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '对象'
    
    def draw(self, context):
        layout = self.layout
        obj = context.object
        
        if not obj:
            layout.label(text="请选择对象")
            return
        
        layout.label(text=f"对象: {obj.name}")
        layout.label(text=f"类型: {obj.type}")
        layout.label(f"位置: ({obj.location.x:.2f}, {obj.location.y:.2f}, {obj.location.z:.2f})")
        
        layout.separator()
        layout.label(text="选择:")
        
        row = layout.row(align=True)
        row.operator("object.select_all", text="全选").action='SELECT'
        row.operator("object.select_all", text="反选").action='INVERT'
        
        row = layout.row()
        row.operator("object.select_more", text="更多")
        row.operator("object.select_less", text="更少")
        
        layout.separator()
        layout.label(text="变换:")
        
        row = layout.row(align=True)
        row.operator("object.transform_apply", text="应用").location=True
        row.operator("object.location_clear", text="清除位置")
        
        row = layout.row()
        row.operator("object.rotation_clear", text="清除旋转")
        row.operator("object.scale_clear", text="清除缩放")
        
        layout.separator()
        layout.label(text="显示:")
        
        row = layout.row()
        row.operator("object.shade_smooth", text="平滑")
        row.operator("object.shade_flat", text="平滑")
        
        layout.separator()
        layout.label(text="删除:")
        
        row = layout.row()
        row.operator("object.delete", text="删除").use_global=False
        row.operator("object.delete", text="全局删除").use_global=True

class ObjectCollectionPanel(bpy.types.Panel):
    """对象集合面板"""
    bl_label = "集合管理"
    bl_idname = "OBJECT_PT_collection_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '对象'
    
    def draw(self, context):
        layout = self.layout
        obj = context.object
        
        if not obj:
            layout.label(text="请选择对象")
            return
        
        layout.label(text=f"当前集合: {context.scene.active_collection.name}")
        
        layout.separator()
        layout.label(text="集合操作:")
        
        row = layout.row()
        row.operator("object.collection_add", text="添加集合")
        row.operator("object.move_to_collection", text="移动到")
```

## 最佳实践

### 1. 对象命名规范

```python
def apply_object_naming(prefix, objects):
    """应用命名规范"""
    for i, obj in enumerate(objects):
        obj.name = f"{prefix}_{i:03d}"

def get_object_by_name(name):
    """按名称获取对象"""
    return bpy.data.objects.get(name)

def get_objects_by_type(obj_type):
    """按类型获取对象"""
    return [obj for obj in bpy.data.objects if obj.type == obj_type]

def get_selected_objects():
    """获取选中的对象"""
    return bpy.context.selected_objects

def get_active_object():
    """获取活动对象"""
    return bpy.context.active_object
```

### 2. 对象选择优化

```python
def batch_select(objects):
    """批量选择对象"""
    bpy.ops.object.select_all(action='DESELECT')
    for obj in objects:
        obj.select_set(True)
    if objects:
        bpy.context.view_layer.objects.active = objects[0]

def select_objects_in_viewport():
    """选择视口中的对象"""
    visible_objects = []
    for obj in bpy.data.objects:
        if obj.visible_get():
            visible_objects.append(obj)
    batch_select(visible_objects)
```

### 3. 对象变换优化

```python
def transform_objects(objects, location=None, rotation=None, scale=None):
    """批量变换对象"""
    for obj in objects:
        if location:
            obj.location = location
        if rotation:
            obj.rotation_euler = rotation
        if scale:
            obj.scale = scale

def keyframe_objects(objects, data_path, frame):
    """批量关键帧"""
    for obj in objects:
        obj.keyframe_insert(data_path=data_path, frame=frame)
```

## 常见问题

### Q: 对象无法选择
A: 检查对象是否被隐藏或排除于视口，确保在正确的集合中。

### Q: 变换不生效
A: 检查对象是否有父级约束或修饰器影响，尝试应用变换。

### Q: 修饰器不显示
A: 检查修饰器是否被禁用，切换到正确模式查看效果。

### Q: 约束不工作
A: 检查约束目标是否存在且正确设置，确保约束未被禁用。

## 相关模块

- [bpy.ops.mesh](bpy_ops_detailed_module.md) - 网格操作
- [bpy.ops.armature](bpy_ops_armature_module.md) - 骨骼操作
- [bpy.ops.curve](bpy_ops_curve_module.md) - 曲线操作
- [bpy.ops.pose](bpy_ops_pose_module.md) - 姿态操作
- [bpy.types](bpy_types_detailed_module.md) - 数据类型


---
## bpy_ops_transform

# bpy.ops.transform 模块

变换操作模块，用于执行物体的变换操作（移动、旋转、缩放）。

## 概述

bpy.ops.transform 模块包含用于执行物体变换的运算符。这些运算符提供与变换操作符（Transform Operator）相同的功能，可以通过脚本控制物体的位置、旋转和缩放。

## 常用操作

### 通用变换参数

```python
import bpy

# 所有变换操作共享的参数
# value: 变换值（移动距离、旋转角度、缩放因子）
# orient_type: 变换轴向（'GLOBAL', 'LOCAL', 'NORMAL', 'GIMBAL', 'VIEW')
# orient_matrix_type: 方向矩阵类型
# constraint_axis: 约束轴 (x, y, z)
# constraint_orientation: 约束方向
# mirror: 镜像
# correct_aspect: 修正长宽比
# proportional: 使用比例编辑
# proportional_size: 比例编辑大小
# snap: 启用吸附
# snap_target: 吸附目标
# snap_point: 自定义吸附点
# snap_align: 对齐到吸附点
# use_transform_skip_children: 跳过子物体变换
```

### 平移操作

```python
# 沿指定轴移动
bpy.ops.transform.translate(
    value=(x, y, z),
    orient_type='GLOBAL',
    constraint_axis=(False, True, False)  # 仅Y轴
)

# 使用变换上下文移动
bpy.ops.transform.transform(
    mode='TRANSLATE',
    value=(1.0, 0.0, 0.0)
)
```

### 旋转操作

```python
# 旋转
bpy.ops.transform.rotate(
    value=radians,  # 弧度值
    orient_axis='Z',  # 旋转轴
    orient_type='GLOBAL'
)

# 自旋
bpy.ops.transform.spin(
    angle=radians,
    center=(0, 0, 0),
    axis='Z'
)

# 缩放
bpy.ops.transform.resize(
    value=(x, y, z),  # 缩放因子
    orient_type='GLOBAL',
    constraint_axis=(False, False, True)  # 仅Z轴
)
```

### 特殊变换

```python
# 倾斜
bpy.ops.transform.shear(
    value=(x, y, z),
    orient_axis_ortho='X',
    orient_axis='Z'
)

# 弯曲
bpy.ops.transform.bend(
    angle=radians,
    direction=(1, 0, 0),
    center=(0, 0, 0)
)

# 变形
bpy.ops.transform.deform(
    factor=1.0,
    mirror=True
)

# 推送
bpy.ops.transform.push_pull(
    value=1.0,
    mirror=True
)
```

### 变换应用

```python
# 应用旋转变换
bpy.ops.transform.rotate(value=0, mode='QUATERNION')

# 应用缩放变换
bpy.ops.transform.resize(value=(1, 1, 1))

# 应用所有变换
bpy.ops.transform.transform(mode='TRANSLATION')

# 重置变换
bpy.ops.transform.resize(value=(0, 0, 0))
```

## 实际应用示例

### 精确移动物体

```python
def precise_move(obj, target_location, axis_orient='GLOBAL'):
    """精确移动物体到指定位置"""
    # 计算移动值
    if axis_orient == 'GLOBAL':
        delta = (
            target_location[0] - obj.location.x,
            target_location[1] - obj.location.y,
            target_location[2] - obj.location.z
        )
    else:
        delta = target_location
    
    # 执行移动
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    bpy.ops.transform.translate(
        value=delta,
        orient_type=axis_orient
    )

# 使用示例
cube = bpy.data.objects['Cube']
precise_move(cube, (5.0, 0.0, 2.0), 'GLOBAL')
```

### 物体环绕动画

```python
def create_orbit_animation(obj, center_point, radius, num_frames=100):
    """创建物体环绕动画"""
    import math
    
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    for i in range(num_frames):
        angle = (2 * math.pi * i) / num_frames
        
        x = center_point[0] + radius * math.cos(angle)
        y = center_point[1] + radius * math.sin(angle)
        z = center_point[2]
        
        bpy.context.scene.frame_set(i)
        
        bpy.ops.transform.translate(
            value=(x - obj.location.x, y - obj.location.y, z - obj.location.z),
            orient_type='GLOBAL'
        )
        
        obj.keyframe_insert(data_path="location", frame=i)
```

### 批量缩放调整

```python
def batch_scale_objects(scale_factor, selected_only=True):
    """批量缩放选中物体"""
    if selected_only:
        objects = bpy.context.selected_objects
    else:
        objects = bpy.context.scene.objects
    
    bpy.ops.object.select_all(action='DESELECT')
    
    for obj in objects:
        obj.select_set(True)
    
    if not selected_only:
        bpy.ops.object.select_hierarchy(direction='CHILD', extend=True)
    
    bpy.ops.transform.resize(
        value=(scale_factor, scale_factor, scale_factor),
        orient_type='GLOBAL'
    )
```

### 沿法线挤出

```python
def extrude_along_normal(obj, distance, iterations=1):
    """沿法线方向挤出"""
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='SELECT')
    
    for _ in range(iterations):
        bpy.ops.mesh.extrude_region_move(
            TRANSFORM_OT_translate={
                "value": (0, 0, distance),
                "orient_type": 'NORMAL'
            }
        )
    
    bpy.ops.object.mode_set(mode='OBJECT')
```

## 使用场景

- **物体定位**：通过脚本精确控制物体位置
- **动画创建**：批量生成变换动画关键帧
- **批量操作**：同时变换多个物体
- **几何编辑**：在编辑模式下变换几何元素
- **精确控制**：使用约束轴和方向进行精确变换

## 注意事项

- 变换操作影响当前选中的物体
- 在编辑模式下变换顶点、边、面
- 使用约束轴可以限制变换方向
- 比例编辑可以创建平滑变形效果


---
## bpy_ops_empty

# bpy.ops.empty - 空对象操作模块

Blender空对象操作符，用于创建和管理空对象，常用作占位符、父子关系锚点或自定义属性载体。

## 概述

`bpy.ops.empty` 模块提供用于操作Blender中空对象的功能。空对象是不可见的对象，可用于组织场景层级、设置变换中心点或作为脚本的占位符。

## 核心功能

### 空对象创建

```python
import bpy

# 添加普通空对象
bpy.ops.object.empty_add(
    type='PLAIN_AXES',
    location=(0, 0, 0),
    rotation=(0, 0, 0)
)

# 添加箭头空对象
bpy.ops.object.empty_add(
    type='ARROWS',
    location=(2.0, 0, 0)
)

# 添加单十字空对象
bpy.ops.object.empty_add(
    type='SINGLE_ARROW',
    location=(0, 2.0, 0)
)

# 添加球体空对象
bpy.ops.object.empty_add(
    type='SPHERE',
    location=(0, 0, 2.0)
)

# 添加立方体空对象
bpy.ops.object.empty_add(
    type='CUBE',
    location=(1.0, 1.0, 1.0)
)

# 添加圆锥体空对象
bpy.ops.object.empty_add(
    type='CONE',
    location=(3.0, 3.0, 0)
)

# 添加图像空对象
bpy.ops.object.empty_add(
    type='IMAGE',
    location=(0, 0, 0)
)
```

### 空对象显示设置

```python
# 设置空对象显示类型
empty = bpy.context.active_object
empty.empty_display_type = 'PLAIN_AXES'
empty.empty_display_type = 'ARROWS'
empty.empty_display_type = 'SINGLE_ARROW'
empty.empty_display_type = 'SPHERE'
empty.empty_display_type = 'CUBE'
empty.empty_display_type = 'CONE'
empty.empty_display_type = 'CIRCLE'
empty.empty_display_type = 'DIAMOND'
empty.empty_display_type = 'THICK_CROSS'
empty.empty_display_type = 'SLIM_CROSS'
empty.empty_display_type = 'IMAGE'

# 设置显示大小
empty.empty_display_size = 0.5

# 设置图像
if empty.empty_display_type == 'IMAGE':
    empty.data.source = '//reference.png'

# 始终显示图像
empty.data.use_alpha = True
```

### 空对象变换

```python
# 移动空对象
empty.location = (5.0, 0, 0)

# 旋转空对象
empty.rotation_euler = (0, 0, math.radians(45))

# 缩放空对象
empty.scale = (1.0, 2.0, 1.0)

# 应用变换
bpy.ops.object.transform_apply(
    location=True,
    rotation=True,
    scale=True
)

# 设置父对象
child = bpy.data.objects['Cube']
empty.select_set(True)
child.select_set(True)
bpy.context.view_layer.objects.active = empty
bpy.ops.object.parent_set()

# 清除父对象
bpy.ops.object.parent_clear(
    type='CLEAR_KEEP_TRANSFORM'
)
```

### 空对象作为占位符

```python
# 创建网格占位符
bpy.ops.object.empty_add(type='SPHERE')
placeholder = bpy.context.active_object
placeholder.name = 'Placeholder_Cube'
placeholder.empty_display_size = 1.0
placeholder.empty_display_type = 'CUBE'

# 创建相机目标空对象
bpy.ops.object.empty_add(type='PLAIN_AXES')
camera_target = bpy.context.active_object
camera_target.name = 'CameraTarget'
bpy.context.scene.camera.data.target = camera_target

# 创建空对象组
bpy.ops.object.empty_add(type='SPHERE')
group_root = bpy.context.active_object
group_root.name = 'Group_Root'

# 将多个对象设为子对象
for obj in ['Cube', 'Sphere', 'Cylinder']:
    bpy.ops.object.empty_add(type='SPHERE')
    child = bpy.context.active_object
    child.name = f'Child_{obj}'
    child.parent = group_root
    child.location = (i * 2, 0, 0)
```

## 实用函数

```python
def create_placeholder(name, location, display_type='CUBE', size=1.0):
    """创建占位符空对象"""
    bpy.ops.object.empty_add(type=display_type, location=location)
    empty = bpy.context.active_object
    empty.name = name
    empty.empty_display_size = size
    return empty

def create_empty_group(name, location, children_names=None):
    """创建空对象组"""
    bpy.ops.object.empty_add(type='SPHERE', location=location)
    group = bpy.context.active_object
    group.name = name
    
    if children_names:
        for child_name in children_names:
            child = bpy.data.objects.get(child_name)
            if child:
                child.parent = group
    
    return group

def setup_camera_target(camera_name, target_name, offset=(0, 0, 0)):
    """设置相机目标空对象"""
    camera = bpy.data.objects.get(camera_name)
    target = bpy.data.objects.get(target_name)
    
    if camera and target:
        bpy.context.view_layer.objects.active = camera
        camera.select_set(True)
        target.select_set(True)
        bpy.ops.object.constraint_add(type='TRACK_TO')
        camera.constraints['Track To'].target = target
        camera.constraints['Track To'].track_axis = 'TRACK_NEGATIVE_Z'
        camera.constraints['Track To'].up_axis = 'UP_Y'

def create_rig_pivot(name, location, axis='Z'):
    """创建骨骼绑定枢轴空对象"""
    bpy.ops.object.empty_add(type='SPHERE', location=location)
    pivot = bpy.context.active_object
    pivot.name = name
    pivot.empty_display_size = 0.2
    
    if axis == 'X':
        pivot.empty_display_type = 'SINGLE_ARROW'
        pivot.rotation_euler = (0, math.radians(90), 0)
    elif axis == 'Y':
        pivot.empty_display_type = 'SINGLE_ARROW'
        pivot.rotation_euler = (math.radians(90), 0, 0)
    else:
        pivot.empty_display_type = 'SINGLE_ARROW'
    
    return pivot

def create_image_reference(image_path, location, scale=1.0):
    """创建图像参考空对象"""
    bpy.ops.object.empty_add(type='IMAGE', location=location)
    ref = bpy.context.active_object
    ref.name = 'ImageReference'
    ref.data.source = image_path
    ref.empty_display_size = scale
    return ref

def batch_create_waypoints(names, locations, display_type='SPHERE'):
    """批量创建路径点空对象"""
    waypoints = []
    for name, loc in zip(names, locations):
        bpy.ops.object.empty_add(type=display_type, location=loc)
        wp = bpy.context.active_object
        wp.name = name
        waypoints.append(wp)
    return waypoints
```

## 常见问题排查

### 空对象不可见
```python
# 检查显示类型
empty = bpy.context.active_object
print(f"显示类型: {empty.empty_display_type}")

# 检查显示大小
print(f"显示大小: {empty.empty_display_size}")

# 检查隐藏状态
print(f"隐藏: {empty.hide_viewport}")

# 设置可见
empty.hide_viewport = False

# 增大显示大小
empty.empty_display_size = 1.0

# 切换显示类型
empty.empty_display_type = 'CUBE'
```

### 空对象显示问题
```python
# 检查显示比例
print(f"比例: {empty.scale}")

# 重置显示设置
empty.empty_display_type = 'PLAIN_AXES'
empty.empty_display_size = 0.5

# 同步显示设置到所有空对象
for obj in bpy.data.objects:
    if obj.type == 'EMPTY':
        obj.empty_display_type = empty.empty_display_type
        obj.empty_display_size = empty.empty_display_size
```

### 父子关系问题
```python
# 检查父对象
child = bpy.context.active_object
print(f"父对象: {child.parent}")

# 检查子对象
parent = bpy.context.active_object
children = [obj for obj in bpy.data.objects if obj.parent == parent]
print(f"子对象数: {len(children)}")

# 设置父对象
child.parent = parent
bpy.ops.object.parent_set()

# 清除父对象
bpy.ops.object.parent_clear(type='CLEAR_KEEP_TRANSFORM')
```


---
## bpy_ops_group

# bpy.ops.group - 组操作模块

Blender对象组操作符，用于创建和管理对象集合组。

## 概述

`bpy.ops.group` 模块提供用于在Blender中创建、编辑和管理对象组的功能。对象组允许您组织和批量操作多个对象。

## 核心功能

### 组创建

```python
import bpy

# 创建新组
bpy.ops.group.create(
    name='MyGroup'
)

# 创建空组
bpy.ops.group.create(
    name='EmptyGroup'
)

# 追加到现有组
bpy.ops.group.objects_add(
    collection='MyGroup'
)

# 从选择创建组
bpy.ops.group.create(
    name='SelectedObjects'
)
```

### 组操作

```python
# 添加选中对象到组
bpy.ops.group.objects_add_active(
    collection='MyGroup'
)

# 从组中移除对象
bpy.ops.group.objects_remove(
    collection='MyGroup'
)

# 移除活动对象
bpy.ops.group.objects_remove_active(
    collection='MyGroup'
)

# 清除所有组
bpy.ops.group.objects_remove_all()

# 链接组到场景
bpy.ops.group.link(
    collection='MyGroup'
)

# 取消链接组
bpy.ops.group.unlink(
    collection='MyGroup'
)
```

### 组管理

```python
# 重命名组
bpy.ops.group.rename(
    name='OldName',
    new_name='NewName'
)

# 删除组
bpy.ops.group.delete(
    unlink=True
)

# 复制组
bpy.ops.group.duplicate(
    collection='MyGroup'
)

# 合并组
bpy.ops.group.merge(
    collection='TargetGroup'
)

# 选择组内对象
bpy.ops.group.select(
    collection='MyGroup'
)

# 反选组内对象
bpy.ops.group.inverse_select(
    collection='MyGroup'
)

# 选择组
bpy.ops.group.select_all(
    collection='MyGroup',
    action='SELECT'
)
```

### 批量组操作

```python
# 创建并填充组
def create_filled_group(group_name, object_names):
    bpy.ops.group.create(name=group_name)
    for obj_name in object_names:
        obj = bpy.data.objects.get(obj_name)
        if obj:
            bpy.context.view_layer.objects.active = obj
            bpy.ops.group.objects_add_active(collection=group_name)

# 批量创建组
def batch_create_groups(base_name, count, prefix=''):
    groups = []
    for i in range(count):
        group_name = f'{base_name}_{i}'
        bpy.ops.group.create(name=group_name)
        groups.append(group_name)
    return groups

# 移动对象到组
def move_to_group(object_name, target_group):
    obj = bpy.data.objects.get(object_name)
    if obj:
        for group in obj.users_group:
            bpy.ops.group.objects_remove(collection=group.name)
        bpy.context.view_layer.objects.active = obj
        bpy.ops.group.objects_add_active(collection=target_group)

# 复制组内容
def duplicate_group_contents(source_group, target_name):
    source = bpy.data.collections.get(source_group)
    if source:
        bpy.ops.group.create(name=target_name)
        for obj in source.objects:
            obj.select_set(True)
        bpy.ops.group.objects_add_active(collection=target_name)
        bpy.ops.object.select_all(action='DESELECT')
```

## 实用函数

```python
def get_group_objects(group_name):
    """获取组内所有对象"""
    group = bpy.data.collections.get(group_name)
    if group:
        return list(group.objects)
    return []

def is_in_group(object_name, group_name):
    """检查对象是否在组中"""
    obj = bpy.data.objects.get(object_name)
    group = bpy.data.collections.get(group_name)
    if obj and group:
        return obj in group.objects
    return False

def get_object_groups(object_name):
    """获取对象所属的所有组"""
    obj = bpy.data.objects.get(object_name)
    if obj:
        return [group.name for group in obj.users_group]
    return []

def create_layer_collection(group_name):
    """为组创建层集合"""
    layer_collection = bpy.context.view_layer.layer_collection
    bpy.ops.collection.create(name=group_name)
    return layer_collection.children.get(group_name)

def link_to_all_scenes(group_name):
    """将组链接到所有场景"""
    group = bpy.data.collections.get(group_name)
    if group:
        for scene in bpy.data.scenes:
            if group.name not in scene.collection.children:
                scene.collection.children.link(group)

def export_group_to_file(group_name, filepath):
    """导出组内容到文件"""
    group = bpy.data.collections.get(group_name)
    if group:
        for obj in group.objects:
            obj.select_set(True)
        bpy.ops.export_scene.fbx(
            filepath=filepath,
            use_selection=True
        )
        bpy.ops.object.select_all(action='DESELECT')

def batch_assign_materials(group_name, material_name):
    """批量为组内对象分配材质"""
    group = bpy.data.collections.get(group_name)
    material = bpy.data.materials.get(material_name)
    if group and material:
        for obj in group.objects:
            if obj.data and hasattr(obj.data, 'materials'):
                obj.data.materials.append(material)
```

## 常见问题排查

### 对象不在组中
```python
# 检查对象所属组
obj = bpy.context.active_object
print(f"对象: {obj.name}")
print(f"所属组: {[g.name for g in obj.users_group]}")

# 检查组内对象
group = bpy.data.collections.get('MyGroup')
print(f"组内对象: {[obj.name for obj in group.objects]}")

# 重新添加对象
bpy.context.view_layer.objects.active = obj
bpy.ops.group.objects_add_active(collection='MyGroup')
```

### 组不可见
```python
# 检查组可见性
group = bpy.data.collections.get('MyGroup')
print(f"隐藏: {group.hide_viewport}")
print(f"渲染隐藏: {group.hide_render}")

# 显示组
group.hide_viewport = False
group.hide_render = False

# 检查层集合
layer_collection = bpy.context.view_layer.layer_collection
for child in layer_collection.children:
    if child.name == group.name:
        print(f"层集合隐藏: {child.hide_viewport}")
        child.hide_viewport = False
```

### 批量操作问题
```python
# 检查所有组
print("所有组:")
for group in bpy.data.collections:
    print(f"  {group.name}: {len(group.objects)} 对象")

# 检查场景中的组
scene = bpy.context.scene
print("场景组:")
for child in scene.collection.children:
    print(f"  {child.name}")

# 同步组和场景
for group in bpy.data.collections:
    if group.name not in scene.collection.children:
        scene.collection.children.link(group)
```


---
## bpy_ops_collection

# bpy.ops.collection - Collection Operations Module

集合操作模块，用于创建、管理和组织Blender集合。

## 概述

`bpy.ops.collection` 模块提供了Blender集合系统的操作功能，包括集合的创建、删除、对象管理、批量导出等。集合是Blender组织场景数据的核心机制，支持复杂的层级结构和灵活的对象管理。

## 核心功能

### 集合创建

```python
import bpy

# 创建新集合
bpy.ops.collection.create(name='')
```

### 对象管理

```python
# 添加活动对象到集合
bpy.ops.collection.objects_add_active(collection='')

# 从集合移除对象
bpy.ops.collection.objects_remove(collection='')

# 从集合移除活动对象
bpy.ops.collection.objects_remove_active(collection='')

# 从所有集合移除对象
bpy.ops.collection.objects_remove_all()
```

### 批量导出

```python
# 添加导出器
bpy.ops.collection.exporter_add(name='')

# 执行导出
bpy.ops.collection.exporter_export(index=0)

# 移动导出器
bpy.ops.collection.exporter_move(direction='UP')

# 移除导出器
bpy.ops.collection.exporter_remove(index=0)

# 导出所有
bpy.ops.collection.export_all()
```

## 实用函数

```python
# 创建集合并添加选中对象
def create_collection_with_selected(name):
    # 创建集合
    bpy.ops.collection.create(name=name)
    # 添加选中对象
    bpy.ops.collection.objects_add_active(collection=name)

# 批量创建多个集合
def create_multiple_collections(names):
    for name in names:
        bpy.ops.collection.create(name=name)

# 清空对象的集合引用
def clear_all_collections(obj):
    bpy.ops.collection.objects_remove_all()

# 移动对象到新集合
def move_to_collection(obj_name, collection_name):
    # 选中对象
    bpy.ops.object.select_all(action='DESELECT')
    bpy.data.objects[obj_name].select_set(True)
    bpy.context.view_layer.objects.active = bpy.data.objects[obj_name]
    # 移除所有集合
    bpy.ops.collection.objects_remove_all()
    # 添加到新集合
    bpy.ops.collection.objects_add_active(collection=collection_name)

# 批量导出集合中的对象
def export_collection_objects(collection_name, filepath):
    # 设置导出参数
    # 执行导出操作
    pass
```

## 常见问题排查

**问题：创建集合失败**

- 检查名称是否重复
- 确认名称格式正确
- 验证权限是否足够

**问题：对象无法添加到集合**

- 确保对象存在
- 检查集合是否有效
- 确认对象类型支持集合

**问题：导出失败**

- 检查导出器配置
- 确认文件路径可写
- 验证导出格式支持

**问题：集合引用混乱**

- 使用`objects_remove_all()`清理引用
- 重新组织集合结构
- 避免循环引用

**问题：批量导出无响应**

- 检查导出队列
- 确认资源充足
- 尝试分批导出


---
## bpy_ops_scene

# bpy.ops.scene 模块

场景操作模块，用于管理Blender场景和场景数据。

## 概述

bpy.ops.scene 模块包含用于创建、删除和管理Blender场景的运算符。这些运算符控制场景的渲染设置、物理模拟设置和场景级操作。

## 常用操作

### 场景管理

```python
import bpy

# 创建新场景
bpy.ops.scene.new(type='NEW')

# 复制当前场景
bpy.ops.scene.new(type='COPY')

# 将选中的物体复制到新场景
bpy.ops.scene.new(type='LINK_OBJECTS')

# 删除场景
bpy.ops.scene.delete()

# 重命名场景
bpy.context.scene.name = "NewSceneName"
```

### 帧设置

```python
# 设置当前帧
bpy.context.scene.frame_set(50)

# 设置起始帧
bpy.context.scene.frame_start = 1

# 设置结束帧
bpy.context.scene.frame_end = 250

# 设置预览范围
bpy.context.scene.frame_preview_start = 1
bpy.context.scene.frame_preview_end = 100

# 设置步长
bpy.context.scene.frame_step = 1
```

### 单位设置

```python
# 设置单位系统
bpy.context.scene.unit_settings.system = 'METRIC'
bpy.context.scene.unit_settings.system = 'IMPERIAL'
bpy.context.scene.unit_settings.system = 'NONE'

# 设置单位精度
bpy.context.scene.unit_settings.scale_length = 1.0

# 设置角度单位
bpy.context.scene.unit_settings.system_rotation = 'DEGREES'
bpy.context.scene.unit_settings.system_rotation = 'RADIANS'
```

### 场景视口显示

```python
# 设置显示模式
bpy.context.scene.display_settings.display_device = 'sRGB'

# 设置渲染引擎
bpy.context.scene.render.engine = 'CYCLES'
bpy.context.scene.render.engine = 'EEVEE'

# 设置显示更新
bpy.context.scene.display.render_aa = 'FXAA'
bpy.context.scene.display.shadow_cube_size = '2048'
```

### 场景特效

```python
# 场景色彩管理
bpy.context.scene.view_settings.view_transform = 'Filmic'
bpy.context.scene.view_settings.look = 'High Contrast'

# 伽马校正
bpy.context.scene.view_settings.gamma = 1.0
bpy.context.scene.view_settings.exposure = 0.0
```

## 实际应用示例

### 批量设置场景帧范围

```python
def setup_scene_frame_range(start, end, step=1):
    """设置场景帧范围"""
    scene = bpy.context.scene
    scene.frame_start = start
    scene.frame_end = end
    scene.frame_step = step
    
    # 同时设置预览范围
    scene.frame_preview_start = start
    scene.frame_preview_end = end

def copy_frame_range_to_all_scenes():
    """将当前场景的帧范围复制到所有场景"""
    current_frame_range = {
        'start': bpy.context.scene.frame_start,
        'end': bpy.context.scene.frame_end,
        'step': bpy.context.scene.frame_step
    }
    
    for scene in bpy.data.scenes:
        scene.frame_start = current_frame_range['start']
        scene.frame_end = current_frame_range['end']
        scene.frame_step = current_frame_range['step']
```

### 场景批量渲染配置

```python
def setup_batch_rendering(resolution_x=1920, resolution_y=1080, engine='CYCLES'):
    """配置批量渲染"""
    for scene in bpy.data.scenes:
        scene.render.resolution_x = resolution_x
        scene.render.resolution_y = resolution_y
        scene.render.engine = engine
        
        # 设置像素比
        scene.render.pixel_aspect_x = 1.0
        scene.render.pixel_aspect_y = 1.0
        
        # 设置输出格式
        scene.render.image_settings.file_format = 'PNG'
        scene.render.image_settings.color_mode = 'RGBA'

def setup_scene_colormanagement(scene_name, view_transform='Filmic'):
    """配置场景色彩管理"""
    scene = bpy.data.scenes.get(scene_name)
    if scene:
        scene.view_settings.view_transform = view_transform
        scene.view_settings.look = 'Medium Contrast'
        scene.view_settings.exposure = 0.0
        scene.view_settings.gamma = 1.0
```

### 场景单位配置

```python
def configure_scene_units(system='METRIC', scale=1.0):
    """配置场景单位系统"""
    scene = bpy.context.scene
    scene.unit_settings.system = system
    scene.unit_settings.scale_length = scale
    
    if system == 'METRIC':
        scene.unit_settings.length_unit = 'METERS'
    elif system == 'IMPERIAL':
        scene.unit_settings.length_unit = 'FEET'

def export_scene_info(scene_name):
    """导出场景信息"""
    scene = bpy.data.scenes.get(scene_name)
    if not scene:
        return None
    
    info = {
        'name': scene.name,
        'frame_range': (scene.frame_start, scene.frame_end, scene.frame_step),
        'unit_system': scene.unit_settings.system,
        'unit_scale': scene.unit_settings.scale_length,
        'render_engine': scene.render.engine,
        'resolution': (scene.render.resolution_x, scene.render.resolution_y),
        'objects_count': len(scene.objects),
        'collections_count': len(scene.collection.children),
    }
    
    return info
```

## 使用场景

- **多场景管理**：创建、删除、复制场景
- **渲染配置**：设置渲染引擎、分辨率、输出格式
- **帧范围控制**：管理动画的时间范围
- **单位系统**：配置测量单位和精度
- **色彩管理**：设置场景的色彩显示和输出
- **批量处理**：在多个场景中执行统一设置

## 注意事项

- 场景删除会移除所有关联的数据
- 单位设置影响测量和显示
- 帧范围更改影响所有关联的动画


---
## bpy_ops_workspace

# bpy.ops.workspace 模块

工作区操作模块，用于管理Blender的工作区和界面布局。

## 概述

bpy.ops.workspace 模块包含用于操作工作区的运算符。工作区定义了Blender的界面布局，包括不同的编辑器区域、工具栏和面板配置。

## 常用操作

### 工作区管理

```python
import bpy

# 切换到工作区
bpy.ops.workspace.workspace_duplicate(new_workspace_name='NewWorkspace')

# 切换工作区
bpy.ops.workspace.append_activate(idname='Modeling')

# 删除工作区
bpy.ops.workspace.delete()
```

### 编辑器操作

```python
# 分割区域
bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)

# 合并区域
bpy.ops.screen.area_join(min_x=0, min_y=0, max_x=500, max_y=500)

# 切换编辑器类型
bpy.ops.screen.area_type_swap_or_assign(type='VIEW_3D')

# 全屏切换
bpy.ops.screen.fullscreen()
```

### 区域操作

```python
# 最大化区域
bpy.ops.screen.screen_full_area(use_hide_panels=False)

# 恢复区域
bpy.ops.screen.back_to_previous()

# 区域循环
bpy.ops.screen.area_next()

# 区域上一个
bpy.ops.screen.area_previous()
```

### 布局操作

```python
# 添加面板
bpy.ops.screen.area_add()

# 删除面板
bpy.ops.screen.area_delete()

# 面板复制
bpy.ops.screen.area_dupli()

# 面板循环
bpy.ops.screen.area_move()
```

### 用户界面

```python
# 显示工具设置
bpy.ops.wm.tool_set_by_id(name='builtin.select')

# 切换侧边栏
bpy.ops.wm.context_toggle(data_path='space_data.show_region_ui')

# 切换工具栏
bpy.ops.wm.context_toggle(data_path='space_data.show_region_toolbar')

# 切换属性面板
bpy.ops.wm.context_toggle(data_path='space_data.show_region_hud')
```

## 实际应用示例

### 创建自定义工作区

```python
def create_modeling_workspace():
    """创建建模工作区"""
    # 切换到默认工作区
    bpy.ops.workspace.append_activate(idname='Modeling')
    
    # 获取当前工作区
    workspace = bpy.context.workspace
    
    # 重命名工作区
    workspace.name = 'CustomModeling'
    
    # 配置编辑器布局
    configure_editors(workspace)
    
    return workspace

def configure_editors(workspace):
    """配置编辑器布局"""
    for area in workspace.screens[0].areas:
        for space in area.spaces:
            if space.type == 'VIEW_3D':
                # 配置3D视图
                space.show_region_toolbar = True
                space.show_region_ui = True
                space.overlay.show_overlays = True
            elif space.type == 'PROPERTIES':
                # 配置属性面板
                space.context = 'MODIFIER'
```

### 快速建模布局

```python
def setup_quick_modeling_layout():
    """设置快速建模布局"""
    # 创建水平分割
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.7)
    
    # 在左侧设置3D视图
    bpy.ops.screen.area_type_swap_or_assign(type='VIEW_3D')
    
    # 在右侧设置属性面板
    bpy.ops.screen.area_type_swap_or_assign(type='PROPERTIES')
    
    # 设置底部区域为时间线
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.8)
    bpy.ops.screen.area_type_swap_or_assign(type='TIMELINE')
```

### 动画工作区

```python
def setup_animation_workspace():
    """设置动画工作区"""
    # 清除当前布局
    bpy.ops.screen.delete()
    
    # 创建主3D视图
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.6)
    
    # 左侧3D视图
    bpy.ops.screen.area_type_swap_or_assign(type='VIEW_3D')
    
    # 右侧分割
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.5)
    
    # 右上动画曲线
    bpy.ops.screen.area_type_swap_or_assign(type='GRAPH_EDITOR')
    
    # 右下时间线
    bpy.ops.screen.area_type_swap_or_assign(type='TIMELINE')
    
    # 底部添加NLA编辑器
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.8)
    bpy.ops.screen.area_type_swap_or_assign(type='NLA_EDITOR')
```

### 绑定工作区

```python
def setup_rigging_workspace():
    """设置绑定工作区"""
    # 垂直分割
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)
    
    # 左侧3D视图
    bpy.ops.screen.area_type_swap_or_assign(type='VIEW_3D')
    
    # 右侧分割
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.3)
    
    # 右上大纲视图
    bpy.ops.screen.area_type_swap_or_assign(type='OUTLINER')
    
    # 右下属性面板
    bpy.ops.screen.area_type_swap_or_assign(type='PROPERTIES')
```

### 渲染工作区

```python
def setup_rendering_workspace():
    """设置渲染工作区"""
    # 创建渲染预览布局
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.4)
    
    # 左侧渲染结果
    bpy.ops.screen.area_type_swap_or_assign(type='IMAGE_EDITOR')
    
    # 右侧分割
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.5)
    
    # 右上节点编辑器
    bpy.ops.screen.area_type_swap_or_assign(type='NODE_EDITOR')
    
    # 右下属性面板
    bpy.ops.screen.area_type_swap_or_assign(type='PROPERTIES')
    
    # 底部添加控制台
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.8)
    bpy.ops.screen.area_type_swap_or_assign(type='CONSOLE')
```

### 视频编辑工作区

```python
def setup_video_editing_workspace():
    """设置视频编辑工作区"""
    # 底部时间线
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.3)
    bpy.ops.screen.area_type_swap_or_assign(type='SEQUENCE_EDITOR')
    
    # 左侧视频预览
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)
    bpy.ops.screen.area_type_swap_or_assign(type='IMAGE_EDITOR')
    
    # 右侧分割
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.5)
    
    # 右上属性
    bpy.ops.screen.area_type_swap_or_assign(type='PROPERTIES')
    
    # 右下音频混合器
    bpy.ops.screen.area_type_swap_or_assign(type='DOPESHEET_EDITOR')
```

### 切换快捷键设置

```python
def setup_quick_layouts():
    """设置快速布局切换"""
    # 创建几个预设布局
    
    # 布局1：仅3D视图
    bpy.ops.workspace.workspace_duplicate(new_workspace_name='Full3D')
    
    # 布局2：建模布局
    bpy.ops.workspace.workspace_duplicate(new_workspace_name='Modeling')
    setup_quick_modeling_layout()
    
    # 布局3：动画布局
    bpy.ops.workspace.workspace_duplicate(new_workspace_name='Animation')
    setup_animation_workspace()
    
    # 布局4：渲染布局
    bpy.ops.workspace.workspace_duplicate(new_workspace_name='Rendering')
    setup_rendering_workspace()
```

### 保存当前布局

```python
def save_current_layout_as_workspace(name):
    """保存当前布局为工作区"""
    # 复制当前工作区
    bpy.ops.workspace.workspace_duplicate(new_workspace_name=name)
    
    # 获取新工作区
    workspace = bpy.context.workspace
    
    # 锁定工作区防止误删
    if workspace.id_data:
        workspace.id_data.use_fake_user = True
    
    return workspace
```

### 恢复默认布局

```python
def reset_to_default_layout():
    """恢复默认布局"""
    # 切换到默认工作区
    bpy.ops.workspace.append_activate(idname='Layout')
    
    # 如果需要完全重置
    bpy.ops.screen.user_preset_warp()
```

### 批量配置工作区

```python
def configure_all_workspaces():
    """批量配置所有工作区"""
    # 获取所有工作区
    workspaces = bpy.data.workspaces
    
    for workspace in workspaces:
        try:
            # 为每个工作区设置统一配置
            configure_workspace_defaults(workspace)
        except Exception as e:
            print(f"配置工作区 {workspace.name} 失败: {e}")

def configure_workspace_defaults(workspace):
    """配置工作区默认设置"""
    for screen in workspace.screens:
        for area in screen.areas:
            for space in area.spaces:
                if space.type == 'VIEW_3D':
                    # 统一3D视图设置
                    space.shading.type = 'MATERIAL'
                    space.overlay.show_overlays = True
                elif space.type == 'PROPERTIES':
                    space.context = 'MATERIAL'
```

### 创建工作室环境

```python
def create_studio_workspace():
    """创建专业工作室工作区"""
    # 主工作区
    bpy.ops.workspace.workspace_duplicate(new_workspace_name='Studio')
    
    # 4视图布局
    for area in bpy.context.workspace.screens[0].areas:
        if area.type == 'VIEW_3D':
            space = area.spaces[0]
            space.region_quadviews[0].show = True
            space.region_quadviews[1].show = True
            space.region_quadviews[2].show = True
    
    # 辅助面板
    bpy.ops.screen.area_add()
    bpy.ops.screen.area_type_swap_or_assign(type='FILE_BROWSER')
```

## 使用场景

- **界面管理**：自定义Blender界面布局
- **工作流优化**：创建针对特定任务的工作区
- **团队协作**：共享统一的工作区配置
- **多任务处理**：快速切换不同工作模式
- **教学演示**：创建清晰的教学界面
- **专业制作**：配置专业制作环境
- **快捷操作**：设置快速布局切换
- **界面定制**：自定义编辑器显示
- **性能优化**：调整界面提高性能
- **远程协作**：通过工作区配置共享工作流

## 注意事项

- 工作区修改可能影响用户体验
- 大量编辑器会影响性能
- 工作区配置保存在用户设置中
- 共享工作区时注意相对路径
- 复杂布局可能不适合所有屏幕
- 定期备份工作区配置
- 使用快捷键可以提高切换效率
- 注意工作区与文件路径的关联
- 大纲视图有助于复杂场景管理
- 多屏用户需要特殊配置


---
## bpy_ops_screen

# bpy.ops.screen 模块

屏幕操作模块，用于管理编辑器区域、视图和屏幕布局。

## 概述

bpy.ops.screen 模块包含用于管理Blender界面布局、编辑器区域和屏幕视图的运算符。这些运算符控制界面的显示、区域分割和视图导航。

## 常用操作

### 屏幕管理

```python
import bpy

# 新建屏幕
bpy.ops.screen.new()

# 删除屏幕
bpy.ops.screen.delete()

# 重命名屏幕
bpy.ops.screen.rename(name='CustomLayout')

# 切换全屏
bpy.ops.screen.fullscreen_toggle()

# 切换装饰
bpy.ops.screen.decorator_toggle()

# 最大化区域
bpy.ops.screen.region_quadview()

# 切换到下一个屏幕
bpy.ops.screen.next_user_flip()

# 切换到上一个屏幕
bpy.ops.screen.previous_user_flip()

# 显示用户界面
bpy.ops.screen.userpref_show()
```

### 区域操作

```python
# 分割区域
bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.5)

# 水平分割
bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.5)

# 垂直分割
bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)

# 删除区域
bpy.ops.screen.area_delete(type='COLLAPSE')

# 折叠区域
bpy.ops.screen.area_flip()

# 交换区域
bpy.ops.screen.area_swap()

# 扩大区域
bpy.ops.screen.area_doubling()

# 最大化区域
bpy.ops.screen.maximise_border()
```

### 视图操作

```python
# 放大视图
bpy.ops.screen.zoom_in()

# 缩小视图
bpy.ops.screen.zoom_out()

# 放大边框
bpy.ops.screen.zoom_border(gesture_mode=0)

# 放大选择
bpy.ops.screen.zoom_to_selection()

# 全部视图
bpy.ops.screen.zoom_camera_1_to_1()

# 局部放大
bpy.ops.screen.zoom_in(factor=1.2)
bpy.ops.screen.zoom_out(factor=1.2)
```

### 帧操作

```python
# 播放动画
bpy.ops.screen.animation_play(reverse=False, sync=False)

# 反向播放
bpy.ops.screen.animation_play(reverse=True)

# 暂停
bpy.ops.screen.animation_play()

# 逐帧前进
bpy.ops.screen.frame_jump(end=False)

# 逐帧后退
bpy.ops.screen.frame_offset_backward(frame=1)

# 跳转到开始
bpy.ops.screen.frame_jump(end=False)

# 跳转到结束
bpy.ops.screen.frame_jump(end=True)
```

### 关键帧操作

```python
# 插入关键帧
bpy.ops.screen.keyframe_jump(next=True)
bpy.ops.screen.keyframe_jump(next=False)

# 跳转到关键帧
bpy.ops.screen.keyframe_jump(next=True)

# 上一个关键帧
bpy.ops.screen.keyframe_jump(next=False)
```

### 编辑器操作

```python
# 区域类型
bpy.ops.screen.area_duplicator()

# 复制区域
bpy.ops.screen.area_duplicator()

# 改变区域类型
bpy.ops.screen.change_area_type(region_type='VIEW_3D')

# 切换区域类型
bpy.ops.screen.change_area_type(region_type='GRAPH_EDITOR')

# 切换到时间线
bpy.ops.screen.change_area_type(region_type='TIMELINE')

# 切换到属性
bpy.ops.screen.change_area_type(region_type='PROPERTIES')

# 切换到大纲
bpy.ops.screen.change_area_type(region_type='OUTLINER')

# 切换到文件浏览器
bpy.ops.screen.change_area_type(region_type='FILE_BROWSER')
```

### 工具设置

```python
# 工具栏切换
bpy.ops.screen.toolbar_prompt(tool_idname='builtin.select')

# 侧边栏切换
bpy.ops.screen.sidebar_ui_show()
```

## 实际应用示例

### 创建自定义屏幕布局

```python
def create_custom_layout():
    """创建自定义屏幕布局"""
    # 新建屏幕
    bpy.ops.screen.new()
    screen = bpy.context.screen
    
    # 设置屏幕名称
    screen.name = 'CustomLayout'
    
    # 返回新屏幕
    return screen
```

### 设置四视图布局

```python
def setup_quadview():
    """设置四视图布局"""
    # 水平分割
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.5)
    
    # 获取区域列表
    areas = bpy.context.screen.areas
    
    # 垂直分割第一个区域
    if len(areas) > 0:
        bpy.context.area = areas[0]
        bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)
    
    # 垂直分割第二个区域
    if len(areas) > 1:
        bpy.context.area = areas[1]
        bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)
    
    # 切换区域类型
    for area in areas:
        if area.type == 'VIEW_3D':
            bpy.context.area = area
            bpy.ops.screen.change_area_type(region_type='VIEW_3D')
```

### 创建动画工作区

```python
def setup_animation_workspace():
    """设置动画工作区"""
    # 删除默认区域
    while len(bpy.context.screen.areas) > 1:
        bpy.ops.screen.area_delete(type='COLLAPSE')
    
    # 分割出时间线
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.7)
    
    # 设置左侧为3D视图
    bpy.context.screen.areas[0].type = 'VIEW_3D'
    
    # 设置右侧上半部分为曲线编辑器
    bpy.context.screen.areas[1].type = 'GRAPH_EDITOR'
    
    # 设置右侧下半部分为时间线
    bpy.ops.screen.area_split(direction='HORIZONTAL', factor=0.5)
```

### 创建材质工作区

```python
def setup_material_workspace():
    """设置材质工作区"""
    # 删除多余区域
    while len(bpy.context.screen.areas) > 1:
        bpy.ops.screen.area_delete(type='COLLAPSE')
    
    # 分割出属性面板
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.75)
    
    # 设置左侧为节点编辑器
    bpy.context.screen.areas[0].type = 'NODE_EDITOR'
    
    # 设置右侧为属性面板
    bpy.context.screen.areas[1].type = 'PROPERTIES'
```

### 创建脚本工作区

```python
def setup_scripting_workspace():
    """设置脚本工作区"""
    # 删除多余区域
    while len(bpy.context.screen.areas) > 1:
        bpy.ops.screen.area_delete(type='COLLAPSE')
    
    # 垂直分割
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)
    
    # 左侧上半部分为文本编辑器
    bpy.context.screen.areas[0].type = 'TEXT_EDITOR'
    
    # 左侧下半部分为系统控制台
    bpy.context.screen.areas[1].type = 'CONSOLE'
```

### 快速切换编辑器

```python
def switch_editor_type(editor_type):
    """快速切换编辑器类型"""
    # 设置当前区域为指定类型
    bpy.context.area.type = editor_type

# 使用示例
# switch_editor_type('VIEW_3D')  # 切换到3D视图
# switch_editor_type('NODE_EDITOR')  # 切换到节点编辑器
# switch_editor_type('GRAPH_EDITOR')  # 切换到曲线编辑器
```

### 创建渲染预览布局

```python
def setup_render_preview():
    """设置渲染预览布局"""
    # 删除多余区域
    while len(bpy.context.screen.areas) > 1:
        bpy.ops.screen.area_delete(type='COLLAPSE')
    
    # 分割出图像编辑器
    bpy.ops.screen.area_split(direction='VERTICAL', factor=0.7)
    
    # 设置左侧为图像编辑器
    bpy.context.screen.areas[0].type = 'IMAGE_EDITOR'
    
    # 设置右侧为属性面板
    bpy.context.screen.areas[1].type = 'PROPERTIES'
```

### 恢复默认布局

```python
def reset_to_default():
    """恢复到默认布局"""
    # 加载默认用户设置
    bpy.ops.wm.read_factory_settings(use_empty=True)
```

## 使用场景

- **界面布局**：创建自定义编辑器排列
- **工作区管理**：保存和切换工作区
- **视图控制**：缩放和导航视图
- **动画播放**：控制动画预览
- **区域管理**：分割和合并区域
- **编辑器切换**：快速切换不同编辑器

## 注意事项

- 区域操作会影响当前屏幕布局
- 分割操作需要指定方向和比例
- 屏幕布局可以保存为用户预设
- 全屏模式隐藏所有UI元素
- 恢复设置会影响所有屏幕
- 区域类型可以动态切换


---
## bpy_ops_outliner

# bpy.ops.outliner 模块

大纲视图操作模块，用于管理场景数据块的层级结构。

## 概述

bpy.ops.outliner 模块包含用于在大纲视图中操作和管理场景数据块的运算符。大纲视图显示场景中所有物体的层级结构，包括物体、集合、材质、纹理等数据块。

## 常用操作

### 选择操作

```python
import bpy

# 选中大纲视图中的项目
bpy.ops.outliner.select(extend=False)

# 扩展选择
bpy.ops.outliner.select(extend=True)

# 选中子项
bpy.ops.outliner.select(walk=True, item='CHILD')

# 选中父项
bpy.ops.outliner.select(walk=True, item='PARENT')

# 全选
bpy.ops.outliner.select_all(action='SELECT')

# 取消全选
bpy.ops.outliner.select_all(action='DESELECT')

# 反选
bpy.ops.outliner.select_all(action='INVERT')

# 选择可渲染物体
bpy.ops.outliner.select_all(action='SELECT')
```

### 展开折叠操作

```python
# 展开选中项
bpy.ops.outliner.expanded_toggle(expand=True)

# 折叠选中项
bpy.ops.outliner.expanded_toggle(expand=False)

# 展开所有
bpy.ops.outliner.expand_tree()

# 折叠所有
bpy.ops.outliner.collapse_tree()

# 递归展开
bpy.ops.outliner.show_hierarchy()
```

### 显示隐藏操作

```python
# 隐藏选中项
bpy.ops.outliner.hide(clear=False)

# 显示选中项
bpy.ops.outliner.hide(clear=True)

# 切换可见性
bpy.ops.outliner.visibility_toggle()

# 设置为可渲染
bpy.ops.outliner.render_toggle()

# 设置为不可渲染
bpy.ops.outliner.render_toggle(clear=True)

# 切换选择状态
bpy.ops.outliner.selectability_toggle()
```

### 删除操作

```python
# 删除选中项
bpy.ops.outliner.delete()

# 删除未使用的数据
bpy.ops.outliner.orphans_purge()

# 清除未链接数据
bpy.ops.outliner.unused_data_id()
```

### 搜索过滤操作

```python
# 搜索
bpy.ops.outliner.search(name='Cube')

# 清除搜索
bpy.ops.outliner.search(clear=True)

# 设置过滤模式
bpy.ops.outliner.filter_set(name='MESH')

# 切换过滤器
bpy.ops.outliner.toggle_filter()
```

### 拖拽操作

```python
# 拖拽到集合
bpy.ops.outliner.drag_and_drop(osource_path=())

# 移动到集合
bpy.ops.outliner.collection_drop(clear=False)
```

### 复制粘贴操作

```python
# 复制
bpy.ops.outliner.copy_rna_path()

# 粘贴
bpy.ops.outliner.paste_rna_path(action='COPY')
```

## 实际应用示例

### 清理未使用数据

```python
def purge_unused_data():
    """清理所有未使用的数据块"""
    # 清除未链接的数据
    bpy.ops.outliner.orphans_purge()
    
    # 清除未使用的图像
    for img in bpy.data.images:
        if not img.users:
            bpy.data.images.remove(img)
    
    # 清除未使用的材质
    for mat in bpy.data.materials:
        if not mat.users:
            bpy.data.materials.remove(mat)
    
    # 清除未使用的纹理
    for tex in bpy.data.textures:
        if not tex.users:
            bpy.data.textures.remove(tex)
    
    # 清除未使用的网格
    for mesh in bpy.data.meshes:
        if not mesh.users:
            bpy.data.meshes.remove(mesh)
```

### 批量显示隐藏集合

```python
def toggle_collections_visibility(visible=True):
    """批量设置集合可见性"""
    for collection in bpy.data.collections:
        collection.hide_viewport = not visible
        collection.hide_render = not visible

def hide_all_except(collection_names):
    """隐藏除指定集合外的所有集合"""
    for collection in bpy.data.collections:
        if collection.name in collection_names:
            collection.hide_viewport = False
            collection.hide_render = False
        else:
            collection.hide_viewport = True
            collection.hide_render = True
```

### 展开完整层级结构

```python
def expand_full_hierarchy():
    """展开完整的大纲层级结构"""
    bpy.ops.outliner.expand_tree()

def collapse_to_level(max_depth=2):
    """折叠到指定层级"""
    # 先展开所有
    bpy.ops.outliner.expand_tree()
    
    # 遍历所有集合并折叠深层级
    def collapse_recursive(collection, current_depth):
        if current_depth >= max_depth:
            for child in collection.children:
                for obj in child.objects:
                    pass
            return
        
        for child in collection.children:
            collapse_recursive(child, current_depth + 1)
    
    for collection in bpy.data.collections:
        collapse_recursive(collection, 0)
```

### 搜索特定类型物体

```python
def search_objects_by_type(obj_type='MESH'):
    """搜索特定类型的物体"""
    # 设置过滤器
    bpy.ops.outliner.filter_set(name=obj_type)
    
    # 执行搜索
    results = []
    for obj in bpy.context.scene.objects:
        if obj.type == obj_type:
            results.append(obj)
    
    return results

def search_materials_by_name(keyword):
    """按名称搜索材质"""
    bpy.ops.outliner.filter_set(name='MATERIAL')
    
    results = []
    for mat in bpy.data.materials:
        if keyword.lower() in mat.name.lower():
            results.append(mat)
    
    return results
```

### 管理场景层级结构

```python
def organize_scene_hierarchy():
    """组织场景层级结构"""
    # 创建主集合
    main_col = bpy.ops.collection.create(name='SceneMain')[1]
    
    # 按类型分组
    type_groups = {}
    for obj in bpy.context.scene.objects:
        if obj.type not in type_groups:
            type_col = bpy.ops.collection.create(name=f'Type_{obj.type}')[1]
            type_groups[obj.type] = type_col
            main_col.children.link(type_col)
        
        # 从其他集合移除
        for col in obj.users_collection:
            col.objects.unlink(obj)
        
        # 链接到类型集合
        type_groups[obj.type].objects.link(obj)
    
    return main_col
```

### 重命名层级项目

```python
def rename_with_prefix(prefix):
    """为选中物体添加前缀"""
    bpy.ops.outliner.select(extend=True)
    
    for obj in bpy.context.selected_objects:
        if not obj.name.startswith(prefix):
            obj.name = f'{prefix}_{obj.name}'

def rename_with_suffix(suffix):
    """为选中物体添加后缀"""
    bpy.ops.outliner.select(extend=True)
    
    for obj in bpy.context.selected_objects:
        if not obj.name.endswith(suffix):
            obj.name = f'{obj.name}_{suffix}'
```

## 使用场景

- **场景管理**：组织和清理场景数据
- **层级导航**：在大纲视图中导航复杂的层级结构
- **数据清理**：删除未使用的数据块
- **批量操作**：批量显示/隐藏、选择物体
- **搜索查找**：快速找到特定的物体或数据块
- **集合管理**：管理集合层级结构

## 注意事项

- 大纲视图操作需要正确的上下文
- 删除操作不可逆
- 过滤器设置会影响可见项目
- 层级展开/折叠状态会保存到文件
- 搜索结果受过滤器影响
- 拖拽操作需要正确的拖拽目标


---
## bpy_ops_area

# bpy.ops.area - 区域操作模块

Blender编辑器区域操作符，用于管理编辑器和区域布局。

## 概述

`bpy.ops.area` 模块提供用于操作Blender用户界面区域的功能，包括分割、合并、调整大小和切换编辑器类型。

## 核心功能

### 区域操作

```python
import bpy

# 垂直分割区域
bpy.ops.screen.area_split(
    direction='VERTICAL',
    factor=0.5
)

# 水平分割区域
bpy.ops.screen.area_split(
    direction='HORIZONTAL',
    factor=0.5
)

# 交换区域
bpy.ops.screen.area_swap()

# 最大化区域
bpy.ops.screen.screen_full_area()

# 退出全屏
bpy.ops.screen.screen_full_area(
    use_restore=True
)

# 删除区域
bpy.ops.screen.area_delete()

# 合并区域
bpy.ops.screen.area_join(
    min_x=0,
    min_y=0,
    max_x=500,
    max_y=500
)
```

### 编辑器类型切换

```python
# 切换到3D视图
bpy.ops.screen.screen_set(
    delta=1
)

# 设置当前区域为3D视图
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        area.ui_type = 'VIEW_3D'

# 切换到时间线
bpy.context.area.ui_type = 'TIMELINE'

# 切换到图像编辑器
bpy.context.area.ui_type = 'IMAGE_EDITOR'

# 切换到节点编辑器
bpy.context.area.ui_type = 'NODE_EDITOR'

# 切换到文本编辑器
bpy.context.area.ui_type = 'TEXT_EDITOR'

# 切换到Python控制台
bpy.context.area.ui_type = 'CONSOLE'

# 切换到属性编辑器
bpy.context.area.ui_type = 'PROPERTIES'

# 切换到大纲视图
bpy.context.area.ui_type = 'OUTLINER'

# 切换到文件浏览器
bpy.context.area.ui_type = 'FILE_BROWSER'

# 切换到序列编辑器
bpy.context.area.ui_type = 'SEQUENCE_EDITOR'

# 切换到动作编辑器
bpy.context.area.ui_type = 'DOPESHEET_EDITOR'

# 切换到图形编辑器
bpy.context.area.ui_type = 'GRAPH_EDITOR'

# 切换到钳编辑器
bpy.context.area.ui_type = 'CLIP_EDITOR'

# 切换到表面
bpy.context.area.ui_type = 'SPREADSHEET'

# 切换到信息
bpy.context.area.ui_type = 'INFO'
```

### 区域导航

```python
# 放大
bpy.ops.screen.zoom_in(
    delta=1
)

# 缩小
bpy.ops.screen.zoom_out(
    delta=1
)

# 放大到区域
bpy.ops.screen.zoom_to_fit()

# 重置缩放
bpy.ops.screen.zoom_border(
    action='RESET'
)

# 居中视图
bpy.ops.screen.view_center_cursor()

# 居中到选中的
bpy.ops.screen.view_center_pick()
```

### 视图操作

```python
# 顶部
bpy.ops.screen.view_top()

# 底部
bpy.ops.screen.view_bottom()

# 左侧
bpy.ops.screen.view_left()

# 右侧
bpy.ops.screen.view_right()

# 正面
bpy.ops.screen.view_front()

# 背面
bpy.ops.screen.view_back()

# 摄像机
bpy.ops.screen.view_camera()

# 用户视图
bpy.ops.screen.view_user()

# 切换正交视图
bpy.ops.screen.view_persportho()

# 切换透视视图
bpy.ops.screen.view_persportho()

# 切换渲染边界
bpy.ops.screen.render_border()
```

## 实用函数

```python
def get_current_area():
    """获取当前区域"""
    return bpy.context.area

def get_all_areas():
    """获取所有区域"""
    return list(bpy.context.screen.areas)

def get_areas_by_type(area_type):
    """获取指定类型的所有区域"""
    return [area for area in bpy.context.screen.areas if area.type == area_type]

def create_editor_split(screen, area_type, direction='VERTICAL', factor=0.5):
    """创建分割编辑器"""
    for area in screen.areas:
        if area.type == 'VIEW_3D':
            override = bpy.context.copy()
            override['area'] = area
            override['screen'] = screen
            bpy.ops.screen.area_split(
                override,
                direction=direction,
                factor=factor
            )
            break

def switch_area_type(area, new_type):
    """切换区域类型"""
    area.ui_type = new_type

def maximize_area(area):
    """最大化区域"""
    override = bpy.context.copy()
    override['area'] = area
    bpy.ops.screen.screen_full_area(override)

def get_region(area, region_type):
    """获取区域中的特定区域"""
    for region in area.regions:
        if region.type == region_type:
            return region
    return None

def setup_workspace(name):
    """设置工作区"""
    for workspace in bpy.data.workspaces:
        if workspace.name == name:
            bpy.context.window.workspace = workspace
            break

def save_workspace(name):
    """保存工作区"""
    bpy.ops.wm.save_homefile()
    workspace = bpy.context.workspace
    workspace.name = name

def reset_workspace():
    """重置工作区"""
    bpy.ops.wm.read_homefile(use_factory_settings=True)
```

## 常见问题排查

### 区域操作无响应
```python
# 检查当前区域
area = bpy.context.area
print(f"区域类型: {area.type}")
print(f"区域索引: {area.x}, {area.y}")
print(f"区域大小: {area.width}, {area.height}")

# 检查区域类型
print(f"UI类型: {area.ui_type}")

# 重置区域
bpy.ops.screen.area_delete()

# 检查所有区域
print("所有区域:")
for area in bpy.context.screen.areas:
    print(f"  类型: {area.type}, 大小: {area.width}x{area.height}")
```

### 编辑器类型切换失败
```python
# 检查支持的编辑器类型
supported_types = [
    'VIEW_3D', 'TIMELINE', 'GRAPH_EDITOR', 'DOPESHEET_EDITOR',
    'NODE_EDITOR', 'IMAGE_EDITOR', 'TEXT_EDITOR', 'CONSOLE',
    'PROPERTIES', 'OUTLINER', 'FILE_BROWSER', 'SEQUENCE_EDITOR',
    'CLIP_EDITOR', 'SPREADSHEET', 'INFO'
]

print(f"目标类型: {area.ui_type}")
print(f"支持类型: {supported_types}")

if area.ui_type in supported_types:
    print("类型有效")
else:
    print("类型无效")
    area.ui_type = 'VIEW_3D'
```

### 视图导航问题
```python
# 检查当前视图
space = bpy.context.area.spaces.active
print(f"空间类型: {space.type}")
print(f"视图类型: {space.shading.type}")

# 重置视图
bpy.ops.screen.view_all()

# 检查视口边界
print(f"边界: {space.region_3d.view_matrix}")

# 重新计算视图
bpy.ops.screen.view_center_cursor()
```


---
## bpy_ops_view3d

# bpy.ops.view3d 模块

3D视图操作模块，用于控制3D视口显示和交互。

## 概述

bpy.ops.view3d 模块包含用于控制3D视口显示、视角切换、视图导航和交互式操作的运算符。这些运算符控制3D视口的显示方式和用户交互。

## 常用操作

### 视图切换

```python
import bpy

# 切换到顶视图
bpy.ops.view3d.view_axis(type='TOP', align_active=False)

# 切换到底视图
bpy.ops.view3d.view_axis(type='BOTTOM', align_active=False)

# 切换到前视图
bpy.ops.view3d.view_axis(type='FRONT', align_active=False)

# 切换到后视图
bpy.ops.view3d.view_axis(type='BACK', align_active=False)

# 切换到左视图
bpy.ops.view3d.view_axis(type='LEFT', align_active=False)

# 切换到右视图
bpy.ops.view3d.view_axis(type='RIGHT', align_active=False)

# 切换到相机视图
bpy.ops.view3d.view_camera()

# 正交/透视切换
bpy.ops.view3d.view_persportho()
```

### 视图导航

```python
# 缩放
bpy.ops.view3d.zoom(delta=1)

# 放大
bpy.ops.view3d.zoom_in()

# 缩小
bpy.ops.view3d.zoom_out()

# 平移
bpy.ops.view3d.view_pan(type='PANLEFT')
bpy.ops.view3d.view_pan(type='PANRIGHT')
bpy.ops.view3d.view_pan(type='PANUP')
bpy.ops.view3d.view_pan(type='PANDOWN')

# 环绕旋转
bpy.ops.view3d.rotate()

# 旋转视角
bpy.ops.view3d.view_roll(angle=0.7854)

# 聚焦到选中物体
bpy.ops.view3d.view_selected()

# 聚焦到光标位置
bpy.ops.view3d.view_center_cursor()

# 聚焦到原点
bpy.ops.view3d.view_center_lock()

# 平滑缩放
bpy.ops.view3d.zoom_camera_1_to_1()
```

### 视口显示

```python
# 切换X射线显示
bpy.ops.view3d.toggle_xray()

# 切换轮廓线
bpy.ops.view3d.toggle_render_border()

# 切换对称显示
bpy.ops.view3d.toggle_symmetry()

# 切换线框显示
bpy.ops.view3d.toggle_wireframe()

# 切换物体/编辑模式
bpy.ops.view3d.mode_set(mode='EDIT')

# 切换到物体模式
bpy.ops.view3d.mode_set(mode='OBJECT')

# 设置着色方式
bpy.ops.view3d.shade(selected_wire=False)

# 线框着色
bpy.ops.view3d.shade(type='WIREFRAME')

# 实体着色
bpy.ops.view3d.shade(type='SOLID')

# 材质预览
bpy.ops.view3d.shade(type='MATERIAL')
```

### 栅格和参考平面

```python
# 切换栅格显示
bpy.ops.view3d.toggle_grid()

# 切换主栅格线
bpy.ops.view3d.toggle_main_grid()

# 栅格细分级别
bpy.ops.view3d.grid_lines(lines=16)

# 栅格底座
bpy.ops.view3d.toggle_floor()
```

### 3D游标操作

```python
# 设置3D游标位置
bpy.ops.view3d.cursor3d(
    orientation='WORLD',
    use_depth=True
)

# 将选中物体对齐到游标
bpy.ops.view3d.snap_selected_to_cursor(use_offset=True)
bpy.ops.view3d.snap_selected_to_cursor(use_offset=False)

# 将游标对齐到选中物体
bpy.ops.view3d.snap_cursor_to_selected()

# 将游标对齐到原点
bpy.ops.view3d.snap_cursor_to_origin()

# 将游标对齐到中心
bpy.ops.view3d.snap_cursor_to_active()

# 游标旋转
bpy.ops.view3d.rotate_cursor_rotate()

# 游标滑动
bpy.ops.view3d.cursor_warp()
```

## 实际应用示例

### 自动聚焦系统

```python
def focus_on_object(obj, zoom_factor=1.0):
    """聚焦到指定物体"""
    # 选中物体
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 聚焦到选中物体
    bpy.ops.view3d.view_selected(use_all_regions=False)
    
    # 调整缩放
    if zoom_factor != 1.0:
        for _ in range(abs(int(zoom_factor))):
            if zoom_factor > 0:
                bpy.ops.view3d.zoom_in()
            else:
                bpy.ops.view3d.zoom_out()

def focus_cycle(objects):
    """循环聚焦到多个物体"""
    current_index = 0
    
    for i, obj in enumerate(objects):
        if obj.select_get():
            current_index = i
            break
    
    # 移动到下一个
    next_index = (current_index + 1) % len(objects)
    focus_on_object(objects[next_index])
```

### 视图预设保存

```python
def save_view_preset(name, area=None):
    """保存当前视图为预设"""
    if area is None:
        area = bpy.context.area
    
    # 获取当前3D区域的空间
    space = None
    for space in area.spaces:
        if space.type == 'VIEW_3D':
            break
    
    preset = {
        'name': name,
        'pivot_point': str(space.pivot_point),
        'view_matrix': list(space.view_matrix),
        'lens': space.lens,
        'clip_start': space.clip_start,
        'clip_end': space.clip_end,
    }
    
    return preset

def apply_view_preset(preset, area=None):
    """应用视图预设"""
    if area is None:
        area = bpy.context.area
    
    space = None
    for s in area.spaces:
        if s.type == 'VIEW_3D':
            space = s
            break
    
    if space and preset:
        space.pivot_point = preset.get('pivot_point', 'BOUNDING_BOX_CENTER')
        space.lens = preset.get('lens', 50.0)
        space.clip_start = preset.get('clip_start', 0.1)
        space.clip_end = preset.get('clip_end', 1000.0)
```

### 批量视图调整

```python
def set_all_viewports_solid():
    """将所有3D视口设置为实体着色"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            for space in area.spaces:
                if space.type == 'VIEW_3D':
                    bpy.ops.view3d.shade(type='SOLID')

def reset_all_viewports():
    """重置所有视口设置"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            for space in area.spaces:
                if space.type == 'VIEW_3D':
                    space.pivot_point = 'BOUNDING_BOX_CENTER'
                    space.lens = 50.0
                    space.clip_start = 0.1
                    space.clip_end = 1000.0
```

## 使用场景

- **视图导航**：缩放、平移、旋转3D视口
- **视角管理**：保存和恢复视图预设
- **视口显示**：切换着色模式、显示选项
- **3D游标控制**：精确定位和对齐
- **自动化操作**：在脚本中控制视图行为
- **多视口编辑**：同时管理多个3D视口

## 注意事项

- 某些操作需要先激活3D视口区域
- 视图矩阵操作可能影响导航体验
- 着色模式影响显示性能
- 3D游标位置影响许多操作的行为


---
## bpy_ops_view2d

# bpy.ops.view2d - 二维视图操作模块

Blender二维视图操作符，用于操作图形编辑器、时间线等二维视图区域的导航和显示。

## 概述

`bpy.ops.view2d` 模块提供用于在Blender的各种二维视图区域中进行平移、缩放和其他视图操作的功能。

## 核心功能

### 视图平移

```python
import bpy

# 向左平移
bpy.ops.view2d.pan(
    dx=-50,
    dy=0
)

# 向右平移
bpy.ops.view2d.pan(
    dx=50,
    dy=0
)

# 向上平移
bpy.ops.view2d.pan(
    dx=0,
    dy=50
)

# 向下平移
bpy.ops.view2d.pan(
    dx=0,
    dy=-50
)

# 平移到中心
bpy.ops.view2d.pan(
    dx='CENTER'
)
```

### 视图缩放

```python
# 放大
bpy.ops.view2d.zoom(
    factor=1.1
)

# 缩小
bpy.ops.view2d.zoom(
    factor=0.9
)

# 放大到鼠标位置
bpy.ops.view2d.zoom(
    factor=1.1,
    mx=200,
    my=300
)

# 缩小到鼠标位置
bpy.ops.view2d.zoom(
    factor=0.9,
    mx=200,
    my=300
)

# 放大到区域
bpy.ops.view2d.zoom(
    factor=1.0,
    xmin=100,
    ymin=100,
    xmax=300,
    ymax=300
)

# 重置缩放
bpy.ops.view2d.zoom(
    factor=1.0,
    mx=0,
    my=0
)

# 放大到适应
bpy.ops.view2d.zoom_out(
    factor=1.0
)

# 缩小到适应
bpy.ops.view2d.zoom_in(
    factor=1.0
)

# 适应所有曲线
bpy.ops.view2d.zoom_border(
    action='FIT'
)
```

### 区域选择

```python
# 框选
bpy.ops.view2d.select_box(
    xmin=100,
    ymin=100,
    xmax=300,
    ymax=300,
    gesture_mode=3
)

# 框选添加
bpy.ops.view2d.select_box(
    xmin=100,
    ymin=100,
    xmax=300,
    ymax=300,
    gesture_mode=1
)

# 框选减去
bpy.ops.view2d.select_box(
    xmin=100,
    ymin=100,
    xmax=300,
    ymax=300,
    gesture_mode=2
)

# 圆形选择
bpy.ops.view2d.select_circle(
    x=200,
    y=200,
    radius=50,
    gesture_mode=3
)

# 套索选择
bpy.ops.view2d.select_lasso(
    path=[(x, y) for x, y in [(100, 100), (200, 100), (200, 200), (100, 200)]]
)
```

### 关键帧导航

```python
# 跳转到开始帧
bpy.ops.view2d.go_to_first()

# 跳转到结束帧
bpy.ops.view2d.go_to_last()

# 跳转到下一帧
bpy.ops.view2d.frame_jump(
    forward=True
)

# 跳转到上一帧
bpy.ops.view2d.frame_jump(
    forward=False
)

# 跳转到指定帧
bpy.ops.view2d.frame_jump(
    frame=100
)

# 播放
bpy.ops.view2d.play(
    speed=1.0
)

# 播放反向
bpy.ops.view2d.play(
    speed=-1.0
)

# 停止播放
bpy.ops.view2d.play(
    speed=0.0
)
```

### 曲线导航

```python
# 跳转到曲线开始
bpy.ops.view2d.curve_start()

# 跳转到曲线结束
bpy.ops.view2d.curve_end()

# 跳转到下一个关键帧
bpy.ops.view2d.nla_next()

# 跳转到上一个关键帧
bpy.ops.view2d.nla_prev()

# 选择范围
bpy.ops.view2d.select_border(
    gesture_mode=3
)
```

## 实用函数

```python
def get_view2d_bounds():
    """获取当前视图边界"""
    area = bpy.context.area
    space = area.spaces.active
    
    if hasattr(space, 'show_region_ui'):
        # 考虑UI区域
        total_width = area.width - area.regions['UI'].width
        return {
            'left': 0,
            'top': 0,
            'right': total_width,
            'bottom': area.height
        }
    else:
        return {
            'left': 0,
            'top': 0,
            'right': area.width,
            'bottom': area.height
        }

def center_view_on_cursor():
    """将视图中心移动到光标位置"""
    bpy.ops.view2d.pan(dx='CENTER')

def zoom_to_selection():
    """缩放到选择"""
    bpy.ops.view2d.zoom_border(action='FIT')

def zoom_to_frame(frame_number):
    """缩放到指定帧"""
    bpy.context.scene.frame_set(frame_number)
    bpy.ops.view2d.frame_jump(frame=frame_number)

def scroll_to_left():
    """滚动到最左边"""
    bpy.ops.view2d.pan(dx='LEFT_MAX')

def scroll_to_right():
    """滚动到最右边"""
    bpy.ops.view2d.pan(dx='RIGHT_MAX')

def scroll_to_top():
    """滚动到最顶部"""
    bpy.ops.view2d.pan(dy='TOP_MAX')

def scroll_to_bottom():
    """滚动到最底部"""
    bpy.ops.view2d.pan(dy='BOTTOM_MAX')

def smooth_zoom(factor):
    """平滑缩放"""
    bpy.ops.view2d.zoom(factor=factor)

def batch_scroll(direction, amount):
    """批量滚动"""
    if direction == 'left':
        for _ in range(amount):
            bpy.ops.view2d.pan(dx=-10)
    elif direction == 'right':
        for _ in range(range):
            bpy.ops.view2d.pan(dx=10)
    elif direction == 'up':
        for _ in range(amount):
            bpy.ops.view2d.pan(dy=10)
    elif direction == 'down':
        for _ in range(amount):
            bpy.ops.view2d.pan(dy=-10)
```

## 常见问题排查

### 视图无法缩放
```python
# 检查区域类型
area = bpy.context.area
print(f"区域类型: {area.type}")
print(f"UI类型: {area.ui_type}")

# 检查空间
space = area.spaces.active
print(f"空间类型: {space.type}")

# 检查缩放限制
print(f"最小缩放: {space.zoom_min}")
print(f"最大缩放: {space.zoom_max}")

# 手动设置缩放
space.zoom = (1.0, 1.0)

# 重置缩放
space.zoom = (1.0, 1.0)
```

### 选择框不显示
```python
# 检查选择模式
print(f"选择模式: {bpy.context.tool_settings.select_mode}")

# 检查选择类型
space = bpy.context.area.spaces.active
print(f"选择类型: {space.ui_mode}")

# 重置选择
bpy.ops.view2d.select_box(
    xmin=0,
    ymin=0,
    xmax=0,
    ymax=0,
    gesture_mode=0
)

# 检查选择事件
print(f"选择手势: {bpy.context.scene.tool_settings.gesture_selection}")
```

### 播放无响应
```python
# 检查播放状态
print(f"正在播放: {bpy.context.scene.is_playing}")

# 检查帧范围
print(f"开始帧: {bpy.context.scene.frame_start}")
print(f"结束帧: {bpy.context.scene.frame_end}")
print(f"当前帧: {bpy.context.scene.frame_current}")

# 手动设置帧
bpy.context.scene.frame_set(100)

# 重新开始播放
bpy.ops.view2d.play(speed=1.0)

# 停止播放
bpy.ops.view2d.play(speed=0.0)
```


---
## bpy_ops_view3d_ruler

# bpy.ops.view3d - 标尺测量操作模块

Blender 3D视口标尺测量操作符，用于在3D视图中测量距离和角度。

## 概述

`bpy.ops.view3d.ruler` 模块提供用于在Blender 3D视图中进行距离和角度测量的功能，支持精确的尺寸标注和角度计算。

## 核心功能

### 标尺工具

```python
import bpy

# 切换标尺工具
bpy.ops.view3d.ruler(
    mode='RULER',
    xmin=0,
    xmax=0,
    ymin=0,
    ymax=0,
    cursor_transform=False,
    texture_space_correction=True,
    remove_on_action=True
)

# 开始测量
bpy.ops.view3d.ruler(
    mode='RULER',
    xmin=100,
    ymin=100
)

# 添加测量点
bpy.ops.view3d.ruler(
    mode='RULER',
    xmin=200,
    ymin=200
)
```

### 角度测量

```python
# 切换到角度测量
bpy.ops.view3d.ruler(
    mode='ANGULAR'
)

# 测量角度
bpy.ops.view3d.ruler(
    mode='ANGULAR',
    xmin=100,
    ymin=100
)
```

### 删除测量

```python
# 删除测量
bpy.ops.view3d.ruler(
    mode='DELETE'
)

# 删除所有测量
def clear_all_measurements():
    # 清除所有标注对象
    for obj in bpy.data.objects:
        if obj.type == 'FONT':
            bpy.data.objects.remove(obj, do_unlink=True)
    
    # 清除所有线条对象
    for obj in bpy.data.objects:
        if obj.type == 'CURVE':
            if 'ruler' in obj.name.lower() or 'measurement' in obj.name.lower():
                bpy.data.objects.remove(obj, do_unlink=True)
```

### 测量模式

```python
# 距离测量模式
bpy.ops.view3d.ruler(
    mode='RULER'
)

# 角度测量模式
bpy.ops.view3d.ruler(
    mode='ANGULAR'
)

# 面积测量模式
bpy.ops.view3d.ruler(
    mode='AREA'
)
```

## 实用函数

```python
def start_measurement():
    """开始测量"""
    bpy.ops.view3d.ruler(mode='RULER')

def start_angle_measurement():
    """开始角度测量"""
    bpy.ops.view3d.ruler(mode='ANGULAR')

def clear_measurements():
    """清除所有测量"""
    bpy.ops.view3d.ruler(mode='DELETE')

def measure_distance(point1, point2):
    """测量两点距离"""
    import math
    
    x1, y1, z1 = point1
    x2, y2, z2 = point2
    
    distance = math.sqrt((x2 - x1)**2 + (y2 - y1)**2 + (z2 - z1)**2)
    return distance

def measure_object_dimensions(obj):
    """测量对象尺寸"""
    if obj.type == 'MESH':
        # 获取边界框尺寸
        dimensions = obj.dimensions
        print(f"对象 {obj.name} 尺寸:")
        print(f"  X: {dimensions.x:.4f}")
        print(f"  Y: {dimensions.y:.4f}")
        print(f"  Z: {dimensions.z:.4f}")
        return dimensions
    else:
        print(f"对象 {obj.name} 不是网格类型")
        return None

def measure_selection_bounds():
    """测量选中对象边界"""
    # 获取选中对象的并集边界框
    min_x = min_y = min_z = float('inf')
    max_x = max_y = max_z = float('-inf')
    
    for obj in bpy.context.selected_objects:
        if obj.type == 'MESH':
            # 获取世界坐标边界框
            bbox_corners = [obj.matrix_world @ Vector(corner) for corner in obj.bound_box]
            
            for corner in bbox_corners:
                min_x = min(min_x, corner.x)
                min_y = min(min_y, corner.y)
                min_z = min(min_z, corner.z)
                max_x = max(max_x, corner.x)
                max_y = max(max_y, corner.y)
                max_z = max(max_z, corner.z)
    
    if min_x != float('inf'):
        width = max_x - min_x
        depth = max_y - min_y
        height = max_z - min_z
        
        print(f"选中对象边界:")
        print(f"  宽度: {width:.4f}")
        print(f"  深度: {depth:.4f}")
        print(f"  高度: {height:.4f}")
        print(f"  总体积: {width * depth * height:.4f}")
        
        return (width, depth, height)
    
    return None

def measure_vertex_distance(obj, vert_index1, vert_index2):
    """测量顶点间距离"""
    if obj.type == 'MESH':
        v1 = obj.data.vertices[vert_index1]
        v2 = obj.data.vertices[vert_index2]
        
        # 获取世界坐标
        p1 = obj.matrix_world @ v1.co
        p2 = obj.matrix_world @ v2.co
        
        distance = (p1 - p2).length
        print(f"顶点 {vert_index1} 到 {vert_index2}: {distance:.4f}")
        return distance
    
    return None

def measure_edge_length(obj, edge_index):
    """测量边长"""
    if obj.type == 'MESH':
        edge = obj.data.edges[edge_index]
        v1 = obj.data.vertices[edge.vertices[0]]
        v2 = obj.data.vertices[edge.vertices[1]]
        
        p1 = obj.matrix_world @ v1.co
        p2 = obj.matrix_world @ v2.co
        
        length = (p1 - p2).length
        print(f"边 {edge_index} 长度: {length:.4f}")
        return length
    
    return None

def measure_face_area(obj, face_index):
    """测量面面积"""
    if obj.type == 'MESH':
        face = obj.data.polygons[face_index]
        area = face.area
        print(f"面 {face_index} 面积: {area:.4f}")
        return area
    
    return None

def measure_total_surface_area(obj):
    """测量总表面积"""
    if obj.type == 'MESH':
        total_area = sum(face.area for face in obj.data.polygons)
        print(f"对象 {obj.name} 总表面积: {total_area:.4f}")
        return total_area
    
    return None

def measure_volume(obj):
    """测量体积"""
    if obj.type == 'MESH':
        volume = obj.volume
        print(f"对象 {obj.name} 体积: {volume:.4f}")
        return volume
    
    return None

def calculate_angle(v1, v2):
    """计算两个向量之间的角度"""
    import math
    
    dot = v1.dot(v2)
    length1 = v1.length
    length2 = v2.length
    
    if length1 > 0 and length2 > 0:
        cos_angle = dot / (length1 * length2)
        angle = math.degrees(math.acos(max(-1, min(1, cos_angle))))
        print(f"角度: {angle:.2f}度")
        return angle
    
    return None
```

## 常见问题排查

### 标尺工具不工作
```python
# 检查区域类型
print(f"区域类型: {bpy.context.area.type}")

# 确保在3D视图中
if bpy.context.area.type != 'VIEW_3D':
    print("请切换到3D视图使用标尺工具")

# 检查工具设置
space = bpy.context.area.spaces.active
print(f"工具: {space.show_measure_panel}")

# 启用测量面板
space.show_measure_panel = True
```

### 测量结果不准确
```python
# 检查单位设置
print(f"单位系统: {bpy.context.scene.unit_settings.system}")
print(f"比例: {bpy.context.scene.unit_settings.scale_length}")

# 检查测量精度
print(f"小数位数: {bpy.context.scene.unit_settings.system_precision}")

# 确保正确的单位
if bpy.context.scene.unit_settings.system != 'METRIC':
    bpy.context.scene.unit_settings.system = 'METRIC'

# 设置正确的比例
bpy.context.scene.unit_settings.scale_length = 1.0
```

### 测量单位问题
```python
# 检查当前单位
print(f"长度单位: {bpy.context.scene.unit_settings.length_unit}")

# 设置单位
bpy.context.scene.unit_settings.length_unit = 'METERS'

# 转换测量值
def convert_distance(distance, from_unit, to_unit):
    """转换距离单位"""
    conversions = {
        'METERS': 1.0,
        'CENTIMETERS': 0.01,
        'MILLIMETERS': 0.001,
        'FEET': 0.3048,
        'INCHES': 0.0254
    }
    
    if from_unit in conversions and to_unit in conversions:
        return distance * conversions[from_unit] / conversions[to_unit]
    
    return distance
```

### 标注不显示
```python
# 检查标注对象
print(f"对象数: {len(bpy.data.objects)}")

# 检查标注类型
annotation_count = 0
for obj in bpy.data.objects:
    if obj.type == 'FONT':
        annotation_count += 1
        print(f"标注: {obj.name}")

# 检查标注可见性
for obj in bpy.context.scene.objects:
    if obj.type == 'FONT':
        print(f"{obj.name}: 隐藏={obj.hide_viewport}")
        obj.hide_viewport = False

# 检查渲染设置
space = bpy.context.area.spaces.active
print(f"标注显示: {space.show_annotation}")
```

### 角度测量问题
```python
# 检查角度测量模式
bpy.ops.view3d.ruler(mode='ANGULAR')

# 测量三点角度
def measure_three_point_angle(point1, point2, point3):
    """测量三点形成的角度"""
    from mathutils import Vector
    
    v1 = point2 - point1
    v2 = point3 - point2
    
    return calculate_angle(v1, v2)

# 测量面法线角度
def measure_face_normal_angle(obj, face_index):
    """测量面法线角度"""
    from mathutils import Vector
    
    if obj.type == 'MESH':
        face = obj.data.polygons[face_index]
        normal = Vector(face.normal)
        
        # 计算与Z轴的角度
        z_axis = Vector((0, 0, 1))
        angle = calculate_angle(normal, z_axis)
        
        return angle
    
    return None
```


---
## bpy_ops_viewport

# bpy.ops.view2d - 2D视图操作模块

bpy.ops.view2d模块提供了2D视图（如Timeline、Graph Editor等）中的导航和操作功能。

## 概述

2D视图是Blender中用于查看时间轴、动画曲线等2D数据的编辑器。这个模块提供了视图导航、缩放和平移操作。

## 核心功能

### 视图导航

```python
import bpy

def view_all():
    """查看全部"""
    bpy.ops.view2d.view_all()

def view_selected():
    """查看选中"""
    bpy.ops.view2d.view_selected()

def view_center():
    """视图居中"""
    bpy.ops.view2d.center()

def view_center_cursor():
    """视图居中到光标"""
    bpy.ops.view2d.center_cursor()
```

### 缩放操作

```python
def zoom_in():
    """放大"""
    bpy.ops.view2d.zoom_in()

def zoom_out():
    """缩小"""
    bpy.ops.view2d.zoom_out()

def zoom_in_10():
    """放大10%"""
    bpy.ops.view2d.zoom_in(factor=1.1)

def zoom_out_10():
    """缩小10%"""
    bpy.ops.view2d.zoom_out(factor=0.9)

def zoom_border(xmin, ymin, xmax, ymax):
    """缩放到边框"""
    bpy.ops.view2d.zoom_border(
        xmin=xmin,
        ymin=ymin,
        xmax=xmax,
        ymax=ymax
    )
```

### 平移操作

```python
def pan_left():
    """左移"""
    bpy.ops.view2d.pan(direction='LEFT')

def pan_right():
    """右移"""
    bpy.ops.view2d.pan(direction='RIGHT')

def pan_up():
    """上移"""
    bpy.ops.view2d.pan(direction='UP')

def pan_down():
    """下移"""
    bpy.ops.view2d.pan(direction='DOWN')
```

## 实用函数

### 滚动操作

```python
def scroll_up():
    """向上滚动"""
    bpy.ops.view2d.scroll_up()

def scroll_down():
    """向下滚动"""
    bpy.ops.view2d.scroll_down()

def scroll_left():
    """向左滚动"""
    bpy.ops.view2d.scroll_left()

def scroll_right():
    """向右滚动"""
    bpy.ops.view2d.scroll_right()

def scroll_page_up():
    """向上翻页"""
    bpy.ops.view2d.scroll_page(
        up=True,
        page=True
    )

def scroll_page_down():
    """向下翻页"""
    bpy.ops.view2d.scroll_page(
        up=False,
        page=True
    )
```

### 区域操作

```python
def scroller_drag():
    """拖动滚动条"""
    bpy.ops.view2d.scroller_drag()

def scroll_match():
    """滚动匹配"""
    bpy.ops.view2d.scroll_match()
```

## 常见问题排查

### 视图问题

视图无响应：
- 确认编辑器激活
- 检查鼠标位置
- 验证视图类型

### 缩放问题

缩放失效：
- 检查缩放限制
- 确认视口焦点
- 验证数据范围


