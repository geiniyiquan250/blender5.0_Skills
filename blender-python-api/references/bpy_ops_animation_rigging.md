# bpy_ops_animation_rigging

---
## bpy_ops_armature

# bpy.ops.armature 模块

骨骼操作模块，用于创建和编辑骨骼系统。

## 概述

bpy.ops.armature 模块包含用于创建、编辑和管理骨骼系统的运算符。这些运算符控制骨骼的创建、选择、变换和父子关系。

## 常用操作

### 骨骼创建

```python
import bpy

# 添加单根骨骼
bpy.ops.armature.bone_primitive_add(name='Bone', enter_editmode=True)

# 添加骨骼
bpy.ops.armature.bone_primitive_add()

# 添加骨骼并进入编辑模式
bpy.ops.armature.bone_primitive_add(enter_editmode=True)

# 复制骨骼
bpy.ops.armature.duplicate()

# 删除骨骼
bpy.ops.armature.delete()

# 重命名骨骼
bpy.ops.armature.rename(name='NewBone')
```

### 选择操作

```python
# 全选
bpy.ops.armature.select_all(action='SELECT')
bpy.ops.armature.select_all(action='DESELECT')
bpy.ops.armature.select_all(action='INVERT')

# 选择父子
bpy.ops.armature.select_hierarchy(direction='PARENT', extend=False)
bpy.ops.armature.select_hierarchy(direction='CHILD', extend=False)
bpy.ops.armature.select_hierarchy(direction='PARENT', extend=True)
bpy.ops.armature.select_hierarchy(direction='CHILD', extend=True)

# 选择同类型
bpy.ops.armature.select_similar(type='TYPE', compare='EQUAL')

# 选择连接
bpy.ops.armature.select_linked()

# 选择mirrored
bpy.ops.armature.select_mirror(only_active=False)
```

### 骨骼编辑

```pytho

# 细分骨骼
bpy.ops.armature.subdivide(number_cuts=1)

# 细分多级
bpy.ops.armature.subdivide(number_cuts=1, number_of_cuts=3)

# 合并骨骼
bpy.ops.armature.merge(type='AT_CURSOR')

# 分离骨骼
bpy.ops.armature.separate()

# 填充
bpy.ops.armature.fill()

# 对称
bpy.ops.armature.symmetric_make(use_flip_names=False)
```

### 骨骼变换

```python
# 移动
bpy.ops.transform.translate(value=(0, 0, 0))

# 旋转
bpy.ops.transform.rotate(value=0)

# 缩放
bpy.ops.transform.resize(value=(1, 1, 1))

# 平滑
bpy.ops.armature_smooth.smooth()
```

### 骨骼关系

```python
# 创建父子关系
bpy.ops.armature.parent_set(type='BONE')

# 清除父子关系
bpy.ops.armature.parent_clear(type='CLEAR')

# 设置为父级
bpy.ops.armature.parent_set(type='OBJECT')

# 设置为骨关联
bpy.ops.armature.parent_set(type='BONE_RELATIVE')
```

### 姿态模式

```python
# 进入姿态模式
bpy.ops.object.mode_set(mode='POSE')

# 退出姿态模式
bpy.ops.object.mode_set(mode='EDIT')

# 重置姿态
bpy.ops.pose.rot_clear()
bpy.ops.pose.loc_clear()
bpy.ops.pose.scale_clear()
bpy.ops.pose.transforms_clear()

# 应用姿态为姿态
bpy.ops.pose.visual_transform_apply()

# 应用姿态为绑定姿态
bpy.ops.pose.armature_apply()
```

### 约束操作

```python
# 添加约束
bpy.ops.pose.constraint_add(type='COPY_LOCATION')

# 复制约束
bpy.ops.pose.constraints_copy()

# 粘贴约束
bpy.ops.pose.constraints_paste()
```

### 驱动操作

```python
# 添加驱动
bpy.ops.pose.driver_add()

# 删除驱动
bpy.ops.pose.driver_remove()

# 添加变量
bpy.ops.pose.driver_add_target()

# 显示驱动
bpy.ops.pose.autoside_names_axials()
```

## 实际应用示例

### 创建基础骨骼链

```python
def create_basic_armature(name='BasicArmature'):
    """创建基础骨骼链"""
    # 创建空物体作为骨架容器
    bpy.ops.object.armature_add()
    armature = bpy.context.active_object
    armature.name = name
    
    # 进入编辑模式
    bpy.ops.object.mode_set(mode='EDIT')
    
    # 获取骨骼
    bones = armature.data.edit_bones
    
    # 创建骨骼链
    bone_count = 5
    bone_length = 0.5
    
    for i in range(bone_count):
        if i == 0:
            bone = bones[0]
            bone.name = f'Bone_{i}'
            bone.head = (0, 0, 0)
            bone.tail = (0, 0, bone_length)
        else:
            prev_bone = bones[f'Bone_{i-1}']
            bone = bones.new(f'Bone_{i}')
            bone.head = prev_bone.tail
            bone.tail = (0, 0, bone_length * (i + 2))
            bone.parent = prev_bone
    
    # 退出编辑模式
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return armature
```

### 创建两足动物骨骼

```python
def create_biped_armature(scale=1.0):
    """创建两足动物骨骼"""
    bpy.ops.object.armature_add()
    armature = bpy.context.active_object
    armature.name = 'Biped'
    
    bpy.ops.object.mode_set(mode='EDIT')
    bones = armature.data.edit_bones
    
    # 脊柱骨骼
    spine_len = 0.5 * scale
    
    # 骨盆
    pelvis = bones.new('Pelvis')
    pelvis.head = (0, 0, spine_len * 4)
    pelvis.tail = (0, 0, spine_len * 5)
    
    # 脊柱
    spine = bones.new('Spine')
    spine.head = pelvis.tail
    spine.tail = (0, 0, spine_len * 7)
    spine.parent = pelvis
    
    # 脖子
    neck = bones.new('Neck')
    neck.head = spine.tail
    neck.tail = (0, 0, spine_len * 8)
    neck.parent = spine
    
    # 头部
    head = bones.new('Head')
    head.head = neck.tail
    head.tail = (0, 0, spine_len * 9.5)
    head.parent = neck
    
    # 手臂（左）
    shoulder_l = bones.new('Shoulder_L')
    shoulder_l.head = spine.tail + (0.2 * scale, 0, 0)
    shoulder_l.tail = shoulder_l.head + (0.3 * scale, 0, 0)
    shoulder_l.parent = spine
    
    arm_l = bones.new('Arm_L')
    arm_l.head = shoulder_l.tail
    arm_l.tail = arm_l.head + (0, 0, -0.4 * scale)
    arm_l.parent = shoulder_l
    
    # 手臂（右）
    shoulder_r = bones.new('Shoulder_R')
    shoulder_r.head = spine.tail + (-0.2 * scale, 0, 0)
    shoulder_r.tail = shoulder_r.head + (-0.3 * scale, 0, 0)
    shoulder_r.parent = spine
    
    arm_r = bones.new('Arm_R')
    arm_r.head = shoulder_r.tail
    arm_r.tail = arm_r.head + (0, 0, -0.4 * scale)
    arm_r.parent = shoulder_r
    
    # 腿（左）
    hip_l = bones.new('Hip_L')
    hip_l.head = pelvis.head + (0.1 * scale, 0, 0)
    hip_l.tail = hip_l.head + (0, 0, -0.4 * scale)
    hip_l.parent = pelvis
    
    leg_l = bones.new('Leg_L')
    leg_l.head = hip_l.tail
    leg_l.tail = leg_l.head + (0, 0, -0.5 * scale)
    leg_l.parent = hip_l
    
    # 腿（右）
    hip_r = bones.new('Hip_R')
    hip_r.head = pelvis.head + (-0.1 * scale, 0, 0)
    hip_r.tail = hip_r.head + (0, 0, -0.4 * scale)
    hip_r.parent = pelvis
    
    leg_r = bones.new('Leg_R')
    leg_r.head = hip_r.tail
    leg_r.tail = leg_r.head + (0, 0, -0.5 * scale)
    leg_r.parent = hip_r
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return armature
```

### 创建程序化手指骨骼

```python
def create_finger_bones(finger_name, start_pos, length, segments=3):
    """创建手指骨骼"""
    armature = bpy.context.active_object
    
    bpy.ops.object.mode_set(mode='EDIT')
    bones = armature.data.edit_bones
    
    segment_length = length / segments
    
    for i in range(segments):
        bone = bones.new(f'{finger_name}_{i}')
        
        if i == 0:
            bone.head = start_pos
            bone.tail = (start_pos[0], start_pos[1], start_pos[2] + segment_length)
        else:
            prev_bone = bones[f'{finger_name}_{i-1}']
            bone.head = prev_bone.tail
            bone.tail = (bone.head[0], bone.head[1], bone.head[2] + segment_length)
            bone.parent = prev_bone
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return bones
```

### 添加IK约束

```python
def add_ik_constraint(pole_target, end_bone, chain_length=2):
    """添加IK约束"""
    # 进入姿态模式
    bpy.ops.object.mode_set(mode='POSE')
    
    # 获取末端骨骼
    pose_bones = bpy.context.active_object.pose.bones
    end_pose_bone = pose_bones[end_bone]
    
    # 添加IK约束
    ik_constraint = end_pose_bone.constraints.new(type='IK')
    ik_constraint.target = pole_target
    ik_constraint.chain_count = chain_length
    
    # 设置极向
    ik_constraint.pole_target = pole_target
    ik_constraint.pole_angle = 0
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return ik_constraint
```

### 批量应用约束

```python
def batch_add_constraints(constraints_config):
    """批量添加约束"""
    bpy.ops.object.mode_set(mode='POSE')
    
    armature = bpy.context.active_object
    
    for bone_name, constraint_type in constraints_config.items():
        pose_bone = armature.pose.bones.get(bone_name)
        if pose_bone:
            constraint = pose_bone.constraints.new(type=constraint_type)
            print(f"已添加 {constraint_type} 到 {bone_name}")
    
    bpy.ops.object.mode_set(mode='OBJECT')
```

### 创建程序化行走动画

```python
def create_walk_cycle(armature, frames=30):
    """创建程序化行走循环"""
    bpy.ops.object.mode_set(mode='POSE')
    
    # 设置帧
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = frames
    
    # 获取姿态骨骼
    pose_bones = armature.pose.bones
    
    # 设置关键帧
    for frame in range(1, frames + 1):
        bpy.context.scene.frame_set(frame)
        
        # 简单的正弦波动画
        t = (frame - 1) / (frames - 1)
        angle = math.sin(t * 2 * math.pi) * 0.5
        
        # 设置腿部旋转
        if 'Leg_L' in pose_bones:
            pose_bones['Leg_L'].rotation_euler = (angle, 0, 0)
            pose_bones['Leg_L'].keyframe_insert(data_path='rotation_euler')
        
        if 'Leg_R' in pose_bones:
            pose_bones['Leg_R'].rotation_euler = (-angle, 0, 0)
            pose_bones['Leg_R'].keyframe_insert(data_path='rotation_euler')
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return armature
```

### 创建骨骼镜像

```python
def mirror_armature_arm(left_arm_name, right_arm_name):
    """镜像骨骼手臂"""
    bpy.ops.object.mode_set(mode='EDIT')
    
    # 选择左臂
    left_bone = armature.data.edit_bones.get(left_arm_name)
    if left_bone:
        bpy.ops.armature.select_all(action='DESELECT')
        left_bone.select = True
        bpy.context.active_object.data.edit_bones.active = left_bone
        
        # 执行镜像
        bpy.ops.armature.select_mirror(only_active=True)
    
    bpy.ops.object.mode_set(mode='OBJECT')
```

## 使用场景

- **角色绑定**：创建角色骨骼系统
- **动画制作**：创建和控制角色动画
- **IK系统**：添加反向动力学约束
- **FK系统**：正向动力学控制
- **程序化动画**：自动生成动画
- **骨骼镜像**：快速创建对称骨骼

## 注意事项

- 骨骼需要正确的父子关系
- IK链长度影响运动范围
- 骨骼名称用于引用和约束
- 姿态模式用于动画
- 编辑模式用于绑定
- 镜像需要正确命名


---
## bpy_ops_pose

# bpy.ops.pose - Pose Operations Module

Blender 姿态操作符，用于在姿态模式下进行骨骼姿态的编辑、关键帧和动画控制。

## Overview

`bpy.ops.pose` 模块包含姿态编辑器中的所有操作命令，涵盖骨骼选择、姿态编辑、关键帧操作、约束管理和动画控制等功能。这个模块主要用于角色绑定和骨骼动画编辑。

## 核心功能

### 骨骼选择操作

```python
import bpy
import math

# 全选
bpy.ops.pose.select_all(action='SELECT')

# 全不选
bpy.ops.pose.select_all(action='DESELECT')

# 反选
bpy.ops.pose.select_all(action='INVERT')

# 选择层级
bpy.ops.pose.select_hierarchy(direction='PARENT', extend=False)
bpy.ops.pose.select_hierarchy(direction='CHILD', extend=False)
bpy.ops.pose.select_hierarchy(direction='PARENT', extend=True)
bpy.ops.pose.select_hierarchy(direction='CHILD', extend=True)

# 选择对称
bpy.ops.pose.select_mirror(only_active=False)

# 选择链接
bpy.ops.pose.select_linked(extend=False)

# 选择键选择
bpy.ops.pose.select_keyed()

# 选择保护
bpy.ops.pose.select_protected(extend=False)

# 选择约束
bpy.ops.pose.select_constrainted()

def select_all_bones():
    """选择所有骨骼"""
    bpy.ops.pose.select_all(action='SELECT')
    return [bone for bone in bpy.context.active_object.pose.bones if bone.bone.select]

def deselect_all_bones():
    """取消选择所有骨骼"""
    bpy.ops.pose.select_all(action='DESELECT')

def select_bone_chain(start_bone, direction='CHILD', extend=True):
    """选择骨骼链"""
    bpy.context.active_pose_bone = start_bone
    bpy.ops.pose.select_hierarchy(direction=direction, extend=extend)

def select_child_bones(bone, recursive=True):
    """选择子骨骼"""
    bpy.ops.pose.select_all(action='DESELECT')
    bone.bone.select = True
    bpy.context.active_pose_bone = bone
    
    if recursive:
        for child in bone.children:
            select_child_bones(child, True)

def select_parent_bones(bone, recursive=True):
    """选择父骨骼"""
    bpy.ops.pose.select_all(action='DESELECT')
    bone.bone.select = True
    bpy.context.active_pose_bone = bone
    
    if recursive and bone.parent:
        select_parent_bones(bone.parent, True)

def select_symmetric_bones():
    """选择对称骨骼"""
    bpy.ops.pose.select_mirror(only_active=False)
```

### 姿态变换操作

```python
# 清除变换
bpy.ops.pose.rot_clear(delta=False)
bpy.ops.pose.loc_clear(delta=False)
bpy.ops.pose.scale_clear(delta=False)
bpy.ops.pose.transform_clear(type='ALL')

# 应用姿态
bpy.ops.pose.armature_apply()
bpy.ops.pose.apply_pose_use_current()

# 复制姿态
bpy.ops.pose.copy()

# 粘贴姿态
bpy.ops.pose.paste(flip=False)

# 镜像姿态
bpy.ops.pose.flip(left_side=True, right_side=True)

# 复制定位
bpy.ops.pose.copy_location()

# 粘贴定位
bpy.ops.pose.paste_location(flip=False)

# 复制旋转
bpy.ops.pose.copy_rotation()

# 粘贴旋转
bpy.ops.pose.paste_rotation(flip=False)

# 复制缩放
bpy.ops.pose.copy_scale()

# 粘贴缩放
bpy.ops.pose.paste_scale(flip=False)

def clear_all_transforms():
    """清除所有变换"""
    bpy.ops.pose.rot_clear()
    bpy.ops.pose.loc_clear()
    bpy.ops.pose.scale_clear()

def reset_pose_to_rest():
    """重置姿态到休息姿态"""
    bpy.ops.pose.select_all(action='SELECT')
    bpy.ops.pose.transform_clear(type='ALL')

def copy_pose_to_clipboard():
    """复制姿态到剪贴板"""
    bpy.ops.pose.copy()

def paste_pose_from_clipboard(flip=False):
    """从剪贴板粘贴姿态"""
    bpy.ops.pose.paste(flip=flip)

def mirror_pose(left_side=True, right_side=True):
    """镜像姿态"""
    bpy.ops.pose.flip(left_side=left_side, right_side=right_side)

def copy_location_only():
    """仅复制位置"""
    bpy.ops.pose.copy_location()

def paste_location_only(flip=False):
    """仅粘贴位置"""
    bpy.ops.pose.paste_location(flip=flip)

def copy_rotation_only():
    """仅复制旋转"""
    bpy.ops.pose.copy_rotation()

def paste_rotation_only(flip=False):
    """仅粘贴旋转"""
    bpy.ops.pose.paste_rotation(flip=flip)

def copy_scale_only():
    """仅复制缩放"""
    bpy.ops.pose.copy_scale()

def paste_scale_only(flip=False):
    """仅粘贴缩放"""
    bpy.ops.pose.paste_scale(flip=flip)

def apply_current_pose():
    """应用当前姿态"""
    bpy.ops.pose.armature_apply()
```

### 关键帧操作

```python
# 插入关键帧
bpy.ops.pose.keyframe_insert()

# 删除关键帧
bpy.ops.pose.keyframe_delete()

# 清除关键帧
bpy.ops.pose.keyframe_clear()

# 插入所有关键帧
bpy.ops.pose.keyframe_insert(data_path='location')
bpy.ops.pose.keyframe_insert(data_path='rotation_euler')
bpy.ops.pose.keyframe_insert(data_path='rotation_quaternion')
bpy.ops.pose.keyframe_insert(data_path='scale')

def insert_keyframe_all():
    """为所有选中的骨骼插入关键帧"""
    bpy.ops.pose.select_all(action='SELECT')
    bpy.ops.pose.keyframe_insert()

def insert_keyframe_location():
    """为位置插入关键帧"""
    bpy.ops.pose.keyframe_insert(data_path='location')

def insert_keyframe_rotation():
    """为旋转插入关键帧"""
    bpy.ops.pose.keyframe_insert(data_path='rotation_euler')

def insert_keyframe_scale():
    """为缩放插入关键帧"""
    bpy.ops.pose.keyframe_insert(data_path='scale')

def delete_all_keyframes():
    """删除所有关键帧"""
    bpy.ops.pose.keyframe_delete()

def clear_all_keyframes():
    """清除所有关键帧"""
    bpy.ops.pose.keyframe_clear()

def keyframe_selected_bones():
    """为选中的骨骼插入关键帧"""
    bpy.ops.pose.keyframe_insert()

def keyframe_protected_bones():
    """为受保护的骨骼插入关键帧"""
    bpy.ops.pose.select_protected(extend=False)
    bpy.ops.pose.keyframe_insert()
```

### 约束操作

```python
# 添加约束
bpy.ops.pose.constraint_add(type='COPY_LOCATION')
bpy.ops.pose.constraint_add(type='COPY_ROTATION')
bpy.ops.pose.constraint_add(type='COPY_SCALE')
bpy.ops.pose.constraint_add(type='IK')
bpy.ops.pose.constraint_add(type='STRETCH_TO')
bpy.ops.pose.constraint_add(type='TRACK_TO')
bpy.ops.pose.constraint_add(type='LOOK_AT')
bpy.ops.pose.constraint_add(type='FOLLOW_PATH')
bpy.ops.pose.constraint_add(type='LIMIT_ROTATION')

# 约束添加（带目标）
bpy.ops.pose.constraint_add_with_targets(type='COPY_LOCATION')

# 复制约束
bpy.ops.pose.constraint_copy()

# 粘贴约束
bpy.ops.pose.constraint_paste()

# 删除约束
bpy.ops.pose.constraint_remove()

# 移动约束
bpy.ops.pose.constraint_move(type='COPY_LOCATION', direction='UP')
bpy.ops.pose.constraint_move(type='COPY_LOCATION', direction='DOWN')

def add_copy_location_constraint(target, bone, name="CopyLocation"):
    """添加位置复制约束"""
    constraint = bone.constraints.new(type='COPY_LOCATION')
    constraint.target = target
    constraint.subtarget = ""
    constraint.influence = 1.0
    return constraint

def add_copy_rotation_constraint(target, bone, name="CopyRotation"):
    """添加旋转复制约束"""
    constraint = bone.constraints.new(type='COPY_ROTATION')
    constraint.target = target
    constraint.subtarget = ""
    constraint.influence = 1.0
    return constraint

def add_ik_constraint(target, bone, chain_count=2):
    """添加反向动力学约束"""
    constraint = bone.constraints.new(type='IK')
    constraint.target = target
    constraint.chain_count = chain_count
    constraint.use_tail = False
    return constraint

def add_stretch_to_constraint(target, bone):
    """添加伸展约束"""
    constraint = bone.constraints.new(type='STRETCH_TO')
    constraint.target = target
    constraint.rest_length = bone.length
    constraint.gradient = 1.0
    return constraint

def add_track_to_constraint(target, bone):
    """添加朝向约束"""
    constraint = bone.constraints.new(type='TRACK_TO')
    constraint.target = target
    constraint.track_axis = 'TRACK_Z'
    constraint.up_axis = 'UP_Y'
    return constraint

def remove_all_constraints(bone):
    """移除所有约束"""
    for constraint in list(bone.constraints):
        bone.constraints.remove(constraint)

def copy_constraint(source_bone, dest_bone, constraint_name):
    """复制约束"""
    for constraint in source_bone.constraints:
        if constraint.name == constraint_name:
            new_constraint = constraint.copy()
            dest_bone.constraints.append(new_constraint)
            return new_constraint
    return None
```

### 骨骼变换操作

```python
# 变换
bpy.ops.transform.rotate(value=math.radians(45), orient_axis='Z')

# 平移
bpy.ops.transform.translate(value=(1, 0, 0))

# 缩放
bpy.ops.transform.resize(value=(1.5, 1.5, 1.5))

# 缩放骨骼
bpy.ops.transform.bbone_resize(value=(1, 1, 1))

# 变换到活动
bpy.ops.pose.transforms_to_deltas(mode='ALL')

# 增量为零
bpy.ops.pose.locrotmode_toggle()

# 自动 IK
bpy.ops.pose.auto_ik_clear()

def rotate_bone(bone, angle, axis='Z'):
    """旋转骨骼"""
    if axis == 'X':
        bone.rotation_euler.x = math.radians(angle)
    elif axis == 'Y':
        bone.rotation_euler.y = math.radians(angle)
    elif axis == 'Z':
        bone.rotation_euler.z = math.radians(angle)

def set_bone_location(bone, x, y, z):
    """设置骨骼位置"""
    bone.location = (x, y, z)

def set_bone_scale(bone, x, y, z):
    """设置骨骼缩放"""
    bone.scale = (x, y, z)

def reset_bone_transform(bone):
    """重置骨骼变换"""
    bone.location = (0, 0, 0)
    bone.rotation_euler = (0, 0, 0)
    bone.scale = (1, 1, 1)

def transform_to_deltas(mode='ALL'):
    """变换到增量"""
    bpy.ops.pose.transforms_to_deltas(mode=mode)
```

### 骨骼编辑操作

```python
# 骨骼显示
bpy.ops.pose.bone_layers(layers=[True, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False, False])

# 切换可见性
bpy.ops.pose.collada_export()

# 骨骼命名
bpy.ops.pose.autoside_names(axis='X')

# 对称命名
bpy.ops.pose.flip_names()

def set_bone_layer(bone, layer_index):
    """设置骨骼层"""
    for i in range(32):
        bone.bone.layers[i] = (i == layer_index)

def show_bone_layer(layer_index):
    """显示骨骼层"""
    bpy.ops.pose.bone_layers(layers=[i == layer_index for i in range(32)])

def hide_other_layers(current_layer):
    """隐藏其他层"""
    layers = [i == current_layer for i in range(32)]
    bpy.ops.pose.bone_layers(layers=layers)

def auto_rename_bones(axis='X'):
    """自动重命名骨骼"""
    bpy.ops.pose.autoside_names(axis=axis)

def flip_bone_names():
    """翻转骨骼名称"""
    bpy.ops.pose.flip_names()
```

## 实用姿态操作

### 姿态管理

```python
import bpy

class PoseManager:
    """姿态管理器"""
    
    @staticmethod
    def save_pose(name):
        """保存当前姿态"""
        pose = bpy.context.active_object.pose
        saved_pose = {}
        
        for bone in pose.bones:
            if bone.bone.select:
                saved_pose[bone.name] = {
                    'location': bone.location.copy(),
                    'rotation_euler': bone.rotation_euler.copy(),
                    'rotation_quaternion': bone.rotation_quaternion.copy(),
                    'scale': bone.scale.copy(),
                }
        
        return saved_pose
    
    @staticmethod
    def load_pose(pose_data, frame=None):
        """加载姿态"""
        pose = bpy.context.active_object.pose
        
        for bone_name, transform in pose_data.items():
            if bone_name in pose:
                bone = pose[bone_name]
                bone.location = transform['location']
                bone.rotation_euler = transform['rotation_euler']
                bone.rotation_quaternion = transform['rotation_quaternion']
                bone.scale = transform['scale']
                
                if frame is not None:
                    bone.keyframe_insert(data_path='location', frame=frame)
                    bone.keyframe_insert(data_path='rotation_euler', frame=frame)
                    bone.keyframe_insert(data_path='scale', frame=frame)
    
    @staticmethod
    def blend_poses(pose1, pose2, factor):
        """混合两个姿态"""
        blended = {}
        
        for bone_name in pose1:
            if bone_name in pose2:
                blended[bone_name] = {
                    'location': pose1[bone_name]['location'].lerp(pose2[bone_name]['location'], factor),
                    'rotation_euler': pose1[bone_name]['rotation_euler'].lerp(pose2[bone_name]['rotation_euler'], factor),
                    'scale': pose1[bone_name]['scale'].lerp(pose2[bone_name]['scale'], factor),
                }
        
        return blended
    
    @staticmethod
    def mirror_pose_data(pose_data):
        """镜像姿态数据"""
        mirrored = {}
        
        for bone_name, transform in pose_data.items():
            mirrored_name = bone_name.replace('.L', '.R').replace('.R', '.L')
            mirrored[mirrored_name] = {
                'location': (-transform['location'][0], transform['location'][1], transform['location'][2]),
                'rotation_euler': (-transform['rotation_euler'][0], -transform['rotation_euler'][1], transform['rotation_euler'][2]),
                'rotation_quaternion': transform['rotation_quaternion'].copy(),
                rotation_quaternion[0] *= -1
                rotation_quaternion[1] *= -1
                mirrored[mirrored_name] = {
                    'location': mirrored_location,
                    'rotation_euler': mirrored_rotation,
                    'scale': transform['scale'],
                }
        
        return mirrored

def save_current_pose(name):
    """保存当前姿态"""
    return PoseManager.save_pose(name)

def load_saved_pose(pose_data):
    """加载保存的姿态"""
    PoseManager.load_pose(pose_data)

def blend_poses_animated(pose1_data, pose2_data, start_frame, end_frame):
    """动画混合两个姿态"""
    frames = end_frame - start_frame
    for i in range(frames + 1):
        factor = i / frames
        blended = PoseManager.blend_poses(pose1_data, pose2_data, factor)
        PoseManager.load_pose(blended, frame=start_frame + i)
```

### IK 和 FK 控制

```python
import bpy
import math

class IKFKController:
    """IK/FK 控制器"""
    
    def __init__(self, armature):
        self.armature = armature
        self.pose = armature.pose
    
    def set_ik_fk_blend(self, ik_fk_value, bone_chain):
        """设置 IK/FK 混合值"""
        # ik_fk_value: 0 = FK, 1 = IK
        for bone in bone_chain:
            if "ik_fk" in bone:
                bone["ik_fk"] = ik_fk_value
    
    def switch_to_ik(self, end_bone, pole_target):
        """切换到 IK 模式"""
        # 添加 IK 约束
        ik_bone = end_bone
        constraint = ik_bone.constraints.new(type='IK')
        constraint.target = pole_target
        constraint.chain_length = 2
        
        # 隐藏 FK 控制
        for bone in self.pose.bones:
            if bone != end_bone and bone != pole_target:
                bone.bone.hide = True
    
    def switch_to_fk(self, bone_chain):
        """切换到 FK 模式"""
        # 移除 IK 约束
        for bone in bone_chain:
            for constraint in list(bone.constraints):
                if constraint.type == 'IK':
                    bone.constraints.remove(constraint)
    
    def create_pole_target(self, name, location):
        """创建极目标"""
        bpy.ops.object.empty_add(type='SPHERE', location=location)
        pole = bpy.context.active_object
        pole.name = name
        return pole

def setup_ik_chain(armature_obj, end_bone_name, pole_name="PoleTarget"):
    """设置 IK 链"""
    end_bone = armature_obj.pose.bones[end_bone_name]
    
    # 创建极目标
    pole_location = (end_bone.matrix @ bpy.types.PoseBone.location).to_3d()
    pole_location.y -= 1.0  # 放在后面
    
    pole = IKFKController(armature_obj).create_pole_target(pole_name, pole_location)
    
    # 添加 IK 约束
    constraint = end_bone.constraints.new(type='IK')
    constraint.target = armature_obj
    constraint.subtarget = pole.name
    constraint.chain_length = 2
    
    return pole

def toggle_ik_fk(armature, bone_name, value):
    """切换 IK/FK"""
    bone = armature.pose.bones[bone_name]
    if "ik_fk" not in bone:
        bone["ik_fk"] = 0.0
    
    bone["ik_fk"] = max(0, min(1, bone["ik_fk"] + value))
    return bone["ik_fk"]
```

### 动画姿态创建

```python
import bpy
import math

def create_walk_cycle(armature, frames_per_step=12):
    """创建行走循环动画"""
    # 设置帧率
    bpy.context.scene.render.fps = 30
    
    # 初始姿态
    set_neutral_pose(armature)
    
    # 关键帧
    for step in range(4):
        frame = step * frames_per_step
        
        if step % 2 == 0:
            set_walk_left_pose(armature, frame)
        else:
            set_walk_right_pose(armature, frame)
    
    # 循环
    armature.animation_data.action.use_cyclic = True

def set_neutral_pose(armature, frame=1):
    """设置中性姿态"""
    for bone in armature.pose.bones:
        bone.rotation_euler = (0, 0, 0)
        bone.location = (0, 0, 0)
        bone.scale = (1, 1, 1)
        bone.keyframe_insert(data_path='rotation_euler', frame=frame)
        bone.keyframe_insert(data_path='location', frame=frame)
        bone.keyframe_insert(data_path='scale', frame=frame)

def set_walk_left_pose(armature, frame):
    """设置左脚行走姿态"""
    # 左腿向前
    left_leg = armature.pose.bones.get("Thigh.L")
    if left_leg:
        left_leg.rotation_euler.x = math.radians(30)
        left_leg.keyframe_insert(data_path='rotation_euler', frame=frame)
    
    # 右腿向后
    right_leg = armature.pose.bones.get("Thigh.R")
    if right_leg:
        right_leg.rotation_euler.x = math.radians(-30)
        right_leg.keyframe_insert(data_path='rotation_euler', frame=frame)
    
    # 左臂向后
    left_arm = armature.pose.bones.get("UpperArm.L")
    if left_arm:
        left_arm.rotation_euler.x = math.radians(-30)
        left_arm.keyframe_insert(data_path='rotation_euler', frame=frame)
    
    # 右臂向前
    right_arm = armature.pose.bones.get("UpperArm.R")
    if right_arm:
        right_arm.rotation_euler.x = math.radians(30)
        right_arm.keyframe_insert(data_path='rotation_euler', frame=frame)

def set_walk_right_pose(armature, frame):
    """设置右脚行走姿态"""
    # 左腿向后
    left_leg = armature.pose.bones.get("Thigh.L")
    if left_leg:
        left_leg.rotation_euler.x = math.radians(-30)
        left_leg.keyframe_insert(data_path='rotation_euler', frame=frame)
    
    # 右腿向前
    right_leg = armature.pose.bones.get("Thigh.R")
    if right_leg:
        right_leg.rotation_euler.x = math.radians(30)
        right_leg.keyframe_insert(data_path='rotation_euler', frame=frame)
    
    # 左臂向前
    left_arm = armature.pose.bones.get("UpperArm.L")
    if left_arm:
        left_arm.rotation_euler.x = math.radians(30)
        left_arm.keyframe_insert(data_path='rotation_euler', frame=frame)
    
    # 右臂向后
    right_arm = armature.pose.bones.get("UpperArm.R")
    if right_arm:
        right_arm.rotation_euler.x = math.radians(-30)
        right_arm.keyframe_insert(data_path='rotation_euler', frame=frame)

def create_bounce_animation(armature, bounce_height=0.1, duration=30):
    """创建弹跳动画"""
    for frame in range(duration + 1):
        t = frame / duration
        
        # 正弦波
        y_offset = math.sin(t * 2 * math.pi) * bounce_height
        
        # 应用于根骨骼
        root = armature.pose.bones[0]
        root.location.y = y_offset
        root.keyframe_insert(data_path='location', frame=frame + 1)
```

## 姿态工程面板

```python
class PoseEngineeringPanel(bpy.types.Panel):
    """姿态工程面板"""
    bl_label = "姿态工具"
    bl_idname = "POSE_PT_pose_tools"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '姿态'
    
    def draw(self, context):
        layout = self.layout
        obj = context.object
        
        if not obj or obj.type != 'ARMATURE':
            layout.label(text="请选择骨架对象")
            return
        
        layout.label(text="骨骼操作:")
        
        row = layout.row(align=True)
        row.operator("pose.select_all", text="全选").action='SELECT'
        row.operator("pose.select_all", text="反选").action='INVERT'
        
        row = layout.row()
        row.operator("pose.select_hierarchy", text="子骨骼").direction='CHILD'
        row.operator("pose.select_hierarchy", text="父骨骼").direction='PARENT'
        
        layout.separator()
        layout.label(text="姿态:")
        
        row = layout.row(align=True)
        row.operator("pose.copy", text="复制")
        row.operator("pose.paste", text="粘贴")
        
        row = layout.row()
        row.operator("pose.flip", text="镜像")
        row.operator("pose.apply_pose_use_current", text="应用")
        
        row = layout.row()
        row.operator("pose.rot_clear", text="清除旋转")
        row.operator("pose.loc_clear", text="清除位置")
        row.operator("pose.scale_clear", text="清除缩放")
        
        layout.separator()
        layout.label(text="关键帧:")
        
        row = layout.row(align=True)
        row.operator("pose.keyframe_insert", text="插入")
        row.operator("pose.keyframe_delete", text="删除")
        
        layout.separator()
        layout.label(text="约束:")
        
        row = layout.row()
        row.operator("pose.constraint_add", text="添加").type='COPY_LOCATION'
        row.operator("pose.constraint_remove", text="移除")
        
        row = layout.row()
        row.operator("pose.copy_constraint", text="复制")
        row.operator("pose.paste_constraint", text="粘贴")
```

## 最佳实践

### 1. 骨骼命名规范

```python
def apply_bone_naming(prefix, armature):
    """应用骨骼命名规范"""
    for i, bone in enumerate(armature.data.bones):
        bone.name = f"{prefix}_{i:02d}"

def create_bone_pairs(left_bone, right_bone):
    """创建骨骼对"""
    # 检查是否是左右对
    left_suffixes = ['.L', '_L', '_left', '.left']
    right_suffixes = ['.R', '_R', '_right', '.right']
    
    if any(left_bone.name.endswith(s) for s in left_suffixes):
        counterpart = left_bone.name
        for s in left_suffixes:
            counterpart = counterpart.replace(s, '')
        for s in right_suffixes:
            if left_bone.name.endswith(s.replace('_', '')):
                counterpart += s
            else:
                counterpart += s
        return counterpart
    return None
```

### 2. 姿态优化

```python
def optimize_keyframes(armature, threshold=0.001):
    """优化关键帧"""
    if not armature.animation_data or not armature.animation_data.action:
        return
    
    action = armature.animation_data.action
    
    for fcurve in action.fcurves:
        keyframe_points = list(fcurve.keyframe_points)
        
        for i in range(len(keyframe_points) - 1):
            curr = keyframe_points[i]
            next_kp = keyframe_points[i + 1]
            
            if abs(curr.co.y - next_kp.co.y) < threshold:
                fcurve.keyframe_points.remove(next_kp)
```

### 3. 姿态验证

```python
def validate_pose(armature):
    """验证姿态"""
    issues = []
    
    for bone in armature.pose.bones:
        # 检查旋转
        for i, angle in enumerate(bone.rotation_euler):
            if abs(angle) > math.pi * 2:
                issues.append(f"骨骼 {bone.name} 旋转超出范围")
        
        # 检查缩放
        for i, scale in enumerate(bone.scale):
            if scale < 0 or scale > 10:
                issues.append(f"骨骼 {bone.name} 缩放超出范围")
        
        # 检查约束
        for constraint in bone.constraints:
            if constraint.target is None:
                issues.append(f"骨骼 {bone.name} 的约束 {constraint.name} 没有目标")
    
    return issues
```

## 常见问题

### Q: 姿态不显示
A: 确保在姿态模式下，检查骨骼是否在正确层级。

### Q: 关键帧不工作
A: 检查是否在正确的场景中，确保动作已分配给骨架。

### Q: IK 不工作
A: 检查目标是否存在，确保链条长度正确设置。

### Q: 约束冲突
A: 检查约束顺序，确保没有冲突的约束。

## 相关模块

- [bpy.ops.armature](bpy_ops_armature_module.md) - 骨骼操作
- [bpy.ops.object](bpy_ops_object_module.md) - 对象操作
- [bpy.ops.animation](bpy_ops_animation_module.md) - 动画操作
- [bpy.types](bpy_types_detailed_module.md) - 数据类型


---
## bpy_ops_action

# bpy.ops.action 模块

动作编辑器操作模块，提供时间轴和动作数据编辑功能。

## 概述

bpy.ops.action 模块包含用于操作动作编辑器（Action Editor）和时间轴（Timeline）的运算符。这些运算符主要用于编辑动画关键帧、动作数据块和时间轴范围。

## 常用操作

### 关键帧操作

```python
import bpy

# 选择关键帧
bpy.ops.action.select_all(action='SELECT')
bpy.ops.action.select_all(action='DESELECT')
bpy.ops.action.select_all(action='INVERT')

# 选择关键帧范围
bpy.ops.action.select_lefthand()
bpy.ops.action.select_righthand()

# 选择关键帧列（同一帧上的所有关键帧）
bpy.ops.action.select_column(mode='KEYS')
bpy.ops.action.select_column(mode='CFRA')

# 选择同类型关键帧
bpy.ops.action.select_same_type()

# 关键帧外推类型切换
bpy.ops.action.extrapolation_type(type='MAKE_CYCLIC')
```

### 关键帧编辑

```python
# 复制关键帧到剪贴板
bpy.ops.action.copy()

# 粘贴关键帧
bpy.ops.action.paste(offset='START')
bpy.ops.action.paste(offset='CURRENT')

# 删除关键帧
bpy.ops.action.delete()

# 移动关键帧
bpy.ops.action.move(type='KEYS')
bpy.ops.action.move(type='FRAME')

# 滑动关键帧
bpy.ops.action.slide(value=5.0)

# 插入关键帧
bpy.ops.action.keyframe_insert(type='ALL')
bpy.ops.action.keyframe_insert(type='SELECTED')

# 转换为关键帧
bpy.ops.action.keyframe(type='REPLACE')
bpy.ops.action.keyframe(type='INSERT')

# 清除关键帧
bpy.ops.action.keyframe_clear(type='ALL')
bpy.ops.action.keyframe_clear(type='SELECTED')
```

### 通道操作

```python
# 选择通道
bpy.ops.action.select_all(action='SELECT')

# 通道类型选择
bpy.ops.action.select_type(type='FCOURSE')
bpy.ops.action.select_type(type='OBJECT')
bpy.ops.action.select_type(type='GROUP')
bpy.ops.action.select_type(type='MATERIAL')
bpy.ops.action.select_type(type='CONSTRAINT')

# 切换通道激活状态
bpy.ops.action.toggle_active()

# 移动通道
bpy.ops.action.move_down()
bpy.ops.action.move_up()

# 通道独奏
bpy.ops.action.solo_selected()
```

### 范围和帧操作

```python
# 设置预览范围
bpy.ops.action.view_previewrange()

# 设置场景范围
bpy.ops.action.view_framename(name='name')

# 跳转帧
bpy.ops.action.frame_jump()

# 推送帧到范围
bpy.ops.action.push_down()

# 替换动作
bpy.ops.action.replacement_add()

# 快照
bpy.ops.action.snapshot()
bpy.ops.action.snapshot_clear()
```

## 实际应用示例

### 批量调整动画时间

```python
import bpy

def retime_animation(obj, start_frame, speed_factor):
    """调整动画时间和速度"""
    # 切换到物体模式
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 选择物体
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 进入物体动画编辑
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 调整时间
    if speed_factor != 1.0:
        bpy.ops.action.scale_length(value=speed_factor)
    
    # 移动到新起始帧
    bpy.ops.action.offset_set(offset=start_frame - obj.animation_data.action.frame_range[0])

# 使用示例
cube = bpy.data.objects['Cube']
retime_animation(cube, frame=1, speed_factor=0.5)
```

### 复制动画数据

```python
def copy_animation(source_obj, target_obj):
    """复制动画从源物体到目标物体"""
    if not source_obj.animation_data or not source_obj.animation_data.action:
        return
    
    action = source_obj.animation_data.action
    
    # 创建新动作
    new_action = action.copy()
    new_action.name = f"{target_obj.name}_Action"
    new_action.use_fake_user = True
    
    # 分配到目标
    if not target_obj.animation_data:
        target_obj.animation_data_create()
    target_obj.animation_data.action = new_action
    
    return new_action
```

## 使用场景

- **动画时间轴编辑**：调整关键帧位置、范围
- **动作库管理**：复制、粘贴、推送动作数据
- **动画重定时**：调整动画速度和时序
- **通道编辑**：调整动画通道顺序和激活状态
- **关键帧批量操作**：选择、删除、移动多个关键帧

## 注意事项

- 操作前确保选中正确的动作编辑器区域
- 某些操作需要在物体模式和姿态模式间切换
- 关键帧操作会影响所有选中的通道


---
## bpy_ops_anim

# bpy.ops.anim - 动画操作模块

Blender动画操作符，用于管理动画数据和关键帧操作。

## 概述

`bpy.ops.anim` 模块提供用于操作Blender动画系统的核心功能，包括关键帧管理、动画数据操作和动画播放控制。

## 核心功能

### 关键帧操作

```python
import bpy

# 插入关键帧
bpy.ops.anim.keyframe_insert(
    type='OBJECT'
)

# 插入所有关键帧
bpy.ops.anim.keyframe_insert(
    type='ALL'
)

# 插入可用关键帧
bpy.ops.anim.keyframe_insert(
    type='AVAILABLE'
)

# 删除关键帧
bpy.ops.anim.keyframe_delete(
    type='OBJECT'
)

# 删除所有关键帧
bpy.ops.anim.keyframe_delete(
    type='ALL'
)

# 删除可用关键帧
bpy.ops.anim.keyframe_delete(
    type='AVAILABLE'
)

# 清除关键帧
bpy.ops.anim.keyframe_clear(
    type='OBJECT'
)
```

### 动画数据操作

```python
# 复制动画数据
bpy.ops.anim.copy_fcurves()

# 粘贴动画数据
bpy.ops.anim.paste_fcurves()

# 清除动画数据
bpy.ops.anim.clear_fcurves()

# 推送动画
bpy.ops.anim.push_down()

# 推送所有到NLA
bpy.ops.anim.push_down(
    type='ALL'
)

# 合并到动作
bpy.ops.anim.merge_to_nla()

# 合并所有到NLA
bpy.ops.anim.merge_to_nla(
    type='ALL'
)
```

### 动作操作

```python
# 新建动作
bpy.ops.anim.action_new(
    name='NewAction'
)

# 删除动作
bpy.ops.anim.action_delete()

# 复制动作
bpy.ops.anim.action_copy()

# 粘贴动作
bpy.ops.anim.action_paste()

# 粘贴到新动作
bpy.ops.anim.action_paste(
    overwrite=True
)

# 清除动作
bpy.ops.anim.action_clear()
```

### 插值设置

```python
# 设置插值类型
bpy.ops.anim.keyframe_insert(
    type='OBJECT',
    confirm_success=False
)

# 设置为常量插值
bpy.ops.graph.interpolation_type(
    type='CONSTANT'
)

# 设置为线性插值
bpy.ops.graph.interpolation_type(
    type='LINEAR'
)

# 设置为贝塞尔插值
bpy.ops.graph.interpolation_type(
    type='BEZIER'
)

# 设置为弹性插值
bpy.ops.graph.extrapolation_type(
    type='MAKE_CYCLIC'
)

# 设置为外推
bpy.ops.graph.extrapolation_type(
    type='CYCLIC'
)
```

### 播放控制

```python
# 播放动画
bpy.ops.anim.play(
    forward=True
)

# 反向播放
bpy.ops.anim.play(
    forward=False
)

# 停止播放
bpy.ops.anim.play(
    speed=0
)

# 跳转到开始
bpy.ops.anim.frame_jump(
    end=False
)

# 跳转到结束
bpy.ops.anim.frame_jump(
    end=True
)

# 步进一帧
bpy.ops.anim.step_frame(
    forward=True
)

# 步退一帧
bpy.ops.anim.step_frame(
    forward=False
)
```

## 实用函数

```python
def create_action_with_keyframes(action_name, obj, keyframes):
    """创建带有关键帧的动作"""
    action = bpy.data.actions.new(name=action_name)
    obj.animation_data.action = action
    
    for frame, transform in keyframes.items():
        bpy.context.scene.frame_set(frame)
        obj.location = transform.get('location', obj.location)
        obj.rotation_euler = transform.get('rotation', obj.rotation_euler)
        obj.scale = transform.get('scale', obj.scale)
        bpy.ops.anim.keyframe_insert(type='OBJECT')
    
    return action

def copy_animation_to_objects(source_obj, target_objs):
    """复制动画到多个对象"""
    if not source_obj.animation_data:
        return
    
    source_action = source_obj.animation_data.action
    if not source_obj.animation_data.action:
        return
    
    for target in target_objs:
        if not target.animation_data:
            target.animation_data_create()
        
        target.animation_data.action = source_action.copy()
        target.animation_data.action.name = f'{source_action.name}_{target.name}'

def batch_keyframe_insert(objects, frame):
    """批量插入关键帧"""
    bpy.context.scene.frame_set(frame)
    
    for obj in objects:
        bpy.context.view_layer.objects.active = obj
        obj.select_set(True)
        bpy.ops.anim.keyframe_insert(type='OBJECT')
        obj.select_set(False)

def get_keyframe_range(action):
    """获取动作的帧范围"""
    if not action:
        return (0, 0)
    
    min_frame = float('inf')
    max_frame = float('-inf')
    
    for fcurve in action.fcurves:
        for keyframe in fcurve.keyframe_points:
            if keyframe.co[0] < min_frame:
                min_frame = keyframe.co[0]
            if keyframe.co[0] > max_frame:
                max_frame = keyframe.co[0]
    
    return (int(min_frame), int(max_frame)) if min_frame != float('inf') else (0, 0)

def offset_action_frames(action, offset):
    """偏移动作帧"""
    for fcurve in action.fcurves:
        for keyframe in fcurve.keyframe_points:
            keyframe.co[0] += offset
            keyframe.handle_left[0] += offset
            keyframe.handle_right[0] += offset

def scale_action_duration(action, scale_factor):
    """缩放动作时长"""
    for fcurve in action.fcurves:
        for keyframe in fcurve.keyframe_points:
            keyframe.co[0] *= scale_factor
            keyframe.handle_left[0] *= scale_factor
            keyframe.handle_right[0] *= scale_factor
```

## 常见问题排查

### 关键帧不生效
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")
print(f"动作: {obj.animation_data.action if obj.animation_data else None}")

# 检查关键帧
if obj.animation_data and obj.animation_data.action:
    print(f"曲线数: {len(obj.animation_data.action.fcurves)}")
    for curve in obj.animation_data.action.fcurves:
        print(f"曲线: {curve.data_path}, 关键帧数: {len(curve.keyframe_points)}")

# 手动插入关键帧
bpy.context.scene.frame_set(1)
bpy.ops.anim.keyframe_insert(type='OBJECT')

# 检查数据路径
print(f"数据路径: {obj.path_from_id()}")
```

### 动画不播放
```python
# 检查播放状态
print(f"正在播放: {bpy.context.scene.is_playing}")

# 检查帧范围
print(f"开始帧: {bpy.context.scene.frame_start}")
print(f"结束帧: {bpy.context.scene.frame_end}")
print(f"当前帧: {bpy.context.scene.frame_current}")

# 设置帧范围
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 250

# 重新开始播放
bpy.ops.anim.play(forward=True)
```

### 动作丢失
```python
# 检查所有动作
print("所有动作:")
for action in bpy.data.actions:
    print(f"  {action.name}: {len(action.fcurves)} 曲线")

# 检查未使用动作
unused_actions = [a for a in bpy.data.actions if not a.users]
print(f"未使用动作: {len(unused_actions)}")

# 恢复动作
action = bpy.data.actions.get('LostAction')
if action:
    obj.animation_data.action = action
```


---
## bpy_ops_animation

# bpy.ops.animation - Animation Operations Module

Blender 动画操作符，用于创建、编辑和管理动画关键帧、动画曲线和动画播放。

## Overview

`bpy.ops.animation` 模块包含动画编辑器中的所有操作命令，涵盖关键帧操作、动画曲线编辑、时间轴控制、播放和导出等功能。

## 核心功能

### 关键帧操作

```python
import bpy

# 插入关键帧
bpy.ops.anim.keyframe_insert(type='Location')
bpy.ops.anim.keyframe_insert(type='Rotation')
bpy.ops.anim.keyframe_insert(type='Scaling')
bpy.ops.anim.keyframe_insert(type='Location, Rotation, Scaling')

# 删除关键帧
bpy.ops.anim.keyframe_delete(type='Location')
bpy.ops.anim.keyframe_delete(type='Rotation')
bpy.ops.anim.keyframe_delete(type='Scaling')

# 删除所有关键帧
bpy.ops.anim.keyframe_delete_v3d()

# 清除关键帧
bpy.ops.anim.keyframe_clear(type='Location')
bpy.ops.anim.keyframe_clear(type='Rotation')
bpy.ops.anim.keyframe_clear(type='Scaling')

def insert_keyframe_at_frame(obj, frame, data_path, value):
    """在指定帧插入关键帧"""
    bpy.context.scene.frame_set(frame)
    
    # 设置值
    if '.' in data_path:
        parts = data_path.split('.')
        if parts[0] == 'location':
            obj.location = eval(value)
        elif parts[0] == 'rotation_euler':
            obj.rotation_euler = eval(value)
        elif parts[0] == 'rotation_quaternion':
            obj.rotation_quaternion = eval(value)
        elif parts[0] == 'scale':
            obj.scale = eval(value)
    else:
        setattr(obj, data_path, value)
    
    # 插入关键帧
    bpy.ops.anim.keyframe_insert(data_path=data_path)
    
    return True

def insert_keyframe_for_object(obj, frame):
    """为对象的所有变换插入关键帧"""
    bpy.context.scene.frame_set(frame)
    
    # 插入位置
    obj.keyframe_insert(data_path="location", frame=frame)
    obj.keyframe_insert(data_path="rotation_euler", frame=frame)
    obj.keyframe_insert(data_path="scale", frame=frame)
    
    return True
```

### 动画曲线操作

```python
# 选择所有关键帧
bpy.ops.anim.select_all(action='SELECT')

# 取消选择
bpy.ops.anim.select_all(action='DESELECT')

# 反选
bpy.ops.anim.select_all(action='INVERT')

# 选择关键帧
bpy.ops.anim.select_border(gesture_mode=0, xmin=0, xmax=100, ymin=0, ymax=100)

# 关键帧类型
bpy.ops.anim.keyframe_type(type='KEYFRAME')
bpy.ops.anim.keyframe_type(type='BREAKDOWN')
bpy.ops.anim.keyframe_type(type='EXTREME')
bpy.ops.anim.keyframe_type(type='JITTER')

# 添加衰减
bpy.ops.anim.keyframe_insert(type='Location')

# 清洁关键帧
bpy.ops.anim.cleankeyframes(threshold=0.001, channels=False)

# 清洁曲线
bpy.ops.anim.channels_clean_empty()

# 展开所有
bpy.ops.anim.channels_expand()

# 折叠所有
bpy.ops.anim.channels_collapse()

# 切换选择
bpy.ops.anim.channels_select_all(action='TOGGLE')

# 反转选择
bpy.ops.anim.channels_select_all(action='INVERT')

# 选择层级
bpy.ops.anim.channels_select_hierarchy(direction='UP')
bpy.ops.anim.channels_select_hierarchy(direction='DOWN')
```

### 时间轴操作

```python
# 播放
bpy.ops.screen.animation_play(reverse=False, speed=1.0)

# 暂停
bpy.ops.screen.animation_play(reverse=True)

# 跳转到开始
bpy.ops.screen.frame_jump(end=False)

# 跳转到结束
bpy.ops.screen.frame_jump(end=True)

# 跳转到前一关键帧
bpy.ops.screen.keyframe_jump(next=False)

# 跳转到下一关键帧
bpy.ops.screen.keyframe_jump(next=True)

# 插入帧
bpy.ops.screen.frame_offset(delta=1)

# 删除帧
bpy.ops.screen.delete()

# 拆分帧
bpy.ops.screen.split()

# 添加标记
bpy.ops.marker.add()

# 转到标记
bpy.ops.marker.jump_to_next()
bpy.ops.marker.jump_to_prev()

def goto_frame(frame):
    """跳转到指定帧"""
    bpy.context.scene.frame_set(frame)

def play_animation():
    """播放动画"""
    bpy.ops.screen.animation_play(reverse=False)

def stop_animation():
    """停止动画"""
    bpy.ops.screen.animation_play(reverse=True)
```

### 动画选择

```python
# 选择可见动画
bpy.ops.anim.select_filter_by_valid_data(data_filter=True)

# 清除选择过滤器
bpy.ops.anim.clear_poseleec()

# 转到活动对象
bpy.ops.anim.go_to_node(action='FIRST')
bpy.ops.anim.go_to_node(action='LAST')
bpy.ops.anim.go_to_node(action='LEFT')
bpy.ops.anim.go_to_node(action='RIGHT')
bpy.ops.anim.go_to_node(action='UP')
bpy.ops.anim.go_to_node(action='DOWN')
```

## 动画编辑

### 关键帧编辑

```python
import bpy

def copy_keyframes(source_obj, dest_obj, data_path="location"):
    """复制关键帧"""
    # 获取源对象的关键帧
    source_fcurves = []
    if source_obj.animation_data:
        for fcurve in source_obj.animation_data.action.fcurves:
            if data_path in fcurve.data_path:
                source_fcurves.append(fcurve)
    
    if not source_fcurves:
        return False
    
    # 确保目标有动画数据
    if not dest_obj.animation_data:
        dest_obj.animation_data_create()
    
    # 获取或创建动作
    if not dest_obj.animation_data.action:
        dest_obj.animation_data.action = bpy.data.actions.new(name=f"{dest_obj.name}_Action")
    
    action = dest_obj.animation_data.action
    
    # 复制关键帧
    for src_curve in source_fcurves:
        dst_curve = None
        for dst_fcurve in action.fcurves:
            if dst_fcurve.data_path == src_curve.data_path:
                dst_curve = dst_fcurve
                break
        
        if not dst_curve:
            dst_curve = action.fcurves.new(data_path=src_curve.data_path)
        
        # 复制关键帧点
        dst_curve.keyframe_points.add(len(src_curve.keyframe_points))
        for i, kp in enumerate(src_curve.keyframe_points):
            dst_kp = dst_curve.keyframe_points[i]
            dst_kp.co = kp.co
            dst_kp.handle_left = kp.handle_left
            dst_kp.handle_right = kp.handle_right
            dst_kp.interpolation = kp.interpolation
            dst_kp.easing = kp.easing
    
    return True

def move_keyframes(obj, data_path, frame_offset):
    """移动关键帧"""
    if not obj.animation_data or not obj.animation_data.action:
        return False
    
    for fcurve in obj.animation_data.action.fcurves:
        if data_path in fcurve.data_path:
            for kp in fcurve.keyframe_points:
                kp.co_ui.x += frame_offset
    
    return True

def scale_keyframes(obj, data_path, scale_factor, pivot_frame=None):
    """缩放关键帧"""
    if not obj.animation_data or not obj.animation_data.action:
        return False
    
    for fcurve in obj.animation_data.action.fcurves:
        if data_path in fcurve.data_path:
            for kp in fcurve.keyframe_points:
                if pivot_frame is None:
                    kp.co_ui.x *= scale_factor
                else:
                    # 以 pivot_frame 为中心缩放
                    distance = kp.co_ui.x - pivot_frame
                    kp.co_ui.x = pivot_frame + distance * scale_factor
    
    return True

def delete_keyframes_in_range(obj, data_path, start_frame, end_frame):
    """删除指定范围内的关键帧"""
    if not obj.animation_data or not obj.animation_data.action:
        return False
    
    for fcurve in obj.animation_data.action.fcurves:
        if data_path in fcurve.data_path:
            keyframe_points_to_remove = []
            for kp in fcurve.keyframe_points:
                if start_frame <= kp.co.x <= end_frame:
                    keyframe_points_to_remove.append(kp)
            
            for kp in keyframe_points_to_remove:
                fcurve.keyframe_points.remove(kp)
    
    return True
```

### 插值和外推

```python
import bpy

def set_interpolation(obj, data_path, interpolation='BEZIER'):
    """设置插值类型"""
    if not obj.animation_data or not obj.animation_data.action:
        return False
    
    inter_map = {
        'CONSTANT': 'CONSTANT',
        'LINEAR': 'LINEAR',
        'BEZIER': 'BEZIER',
        'SINE': 'SINE',
        'QUAD': 'QUAD',
        'CUBIC': 'CUBIC',
        'QUART': 'QUART',
        'EXPO': 'EXPO',
        'CIRC': 'CIRC',
        'BACK': 'BACK',
        'BOUNCE': 'BOUNCE',
        'ELASTIC': 'ELASTIC',
    }
    
    for fcurve in obj.animation_data.action.fcurves:
        if data_path in fcurve.data_path:
            for kp in fcurve.keyframe_points:
                if interpolation in inter_map:
                    kp.interpolation = inter_map[interpolation]
    
    return True

def set_easing(obj, data_path, easing='AUTO'):
    """设置缓动类型"""
    if not obj.animation_data or not obj.animation_data.action:
        return False
    
    easing_map = {
        'AUTO': 'AUTO',
        'EASE_IN': 'EASE_IN',
        'EASE_OUT': 'EASE_OUT',
        'EASE_IN_OUT': 'EASE_IN_OUT',
    }
    
    for fcurve in obj.animation_data.action.fcurves:
        if data_path in fcurve.data_path:
            for kp in fcurve.keyframe_points:
                if easing in easing_map:
                    kp.easing = easing_map[easing]
    
    return True

def set_extrapolation(obj, data_path, extrapolation='CONSTANT'):
    """设置外推类型"""
    if not obj.animation_data or not obj.animation_data.action:
        return False
    
    extrap_map = {
        'CONSTANT': 'CONSTANT',
        'LINEAR': 'LINEAR',
        'MAKE_CYCLIC': 'MAKE_CYCLIC',
        'MAKE_CYCLIC_OFFSET': 'MAKE_CYCLIC_OFFSET',
    }
    
    for fcurve in obj.animation_data.action.fcurves:
        if data_path in fcurve.data_path:
            if extrapolation in extrap_map:
                fcurve.extrapolation = extrap_map[extrapolation]
    
    return True

def make_cyclic(obj, data_path):
    """使动画循环"""
    set_extrapolation(obj, data_path, 'MAKE_CYCLIC')
```

### 关键帧操作符

```python
import bpy

class KeyframeOperator:
    """关键帧操作器"""
    
    @staticmethod
    def insert_keyframes(obj, frame, keyable_properties=None):
        """为对象插入关键帧"""
        bpy.context.scene.frame_set(frame)
        
        if keyable_properties is None:
            keyable_properties = ['location', 'rotation_euler', 'rotation_quaternion', 'scale']
        
        for prop in keyable_properties:
            if hasattr(obj, prop):
                obj.keyframe_insert(data_path=prop, frame=frame)
    
    @staticmethod
    def insert_keyframe_at_value(obj, frame, data_path, value):
        """在指定值处插入关键帧"""
        bpy.context.scene.frame_set(frame)
        
        # 设置值
        parts = data_path.split('.')
        if len(parts) > 1:
            if parts[0] == 'location':
                obj.location = value
            elif parts[0] == 'rotation_euler':
                obj.rotation_euler = value
            elif parts[0] == 'scale':
                obj.scale = value
        else:
            setattr(obj, data_path, value)
        
        # 插入关键帧
        obj.keyframe_insert(data_path=data_path, frame=frame)
    
    @staticmethod
    def delete_keyframes(obj, data_path=None, frame_range=None):
        """删除关键帧"""
        if not obj.animation_data:
            return
        
        action = obj.animation_data.action
        if not action:
            return
        
        fcurves = action.fcurves if data_path is None else [f for f in action.fcurves if data_path in f.data_path]
        
        for fcurve in fcurves:
            if frame_range is None:
                # 删除所有关键帧
                for kp in list(fcurve.keyframe_points):
                    fcurve.keyframe_points.remove(kp)
            else:
                # 删除范围内的关键帧
                for kp in list(fcurve.keyframe_points):
                    if frame_range[0] <= kp.co.x <= frame_range[1]:
                        fcurve.keyframe_points.remove(kp)
    
    @staticmethod
    def offset_keyframes(obj, data_path, frame_offset, value_offset=None):
        """偏移关键帧"""
        if not obj.animation_data or not obj.animation_data.action:
            return
        
        for fcurve in obj.animation_data.action.fcurves:
            if data_path in fcurve.data_path:
                for kp in fcurve.keyframe_points:
                    kp.co.x += frame_offset
                    if value_offset is not None:
                        kp.co.y += value_offset
    
    @staticmethod
    def mirror_keyframes(obj, data_path, pivot_frame=None):
    镜像关键帧
        if pivot_frame is None:
            scene = bpy.context.scene
            pivot_frame = (scene.frame_end + scene.frame_start) / 2
        
        if not obj.animation_data or not obj.animation_data.action:
            return
        
        for fcurve in obj.animation_data.action.fcurves:
            if data_path in fcurve.data_path:
                for kp in fcurve.keyframe_points:
                    distance = kp.co.x - pivot_frame
                    kp.co.x = pivot_frame - distance
    
    @staticmethod
    def blend_keyframes(obj, start_frame, end_frame, blend_type='MIX'):
        """混合关键帧"""
        # 这个功能通常需要更复杂的实现
        pass
```

## 动画播放控制

```python
import bpy

class AnimationPlayer:
    """动画播放器"""
    
    def __init__(self):
        self.is_playing = False
    
    def play(self, reverse=False, speed=1.0):
        """播放动画"""
        bpy.ops.screen.animation_play(reverse=reverse)
        self.is_playing = True
    
    def pause(self):
        """暂停动画"""
        if self.is_playing:
            bpy.ops.screen.animation_play()
            self.is_playing = False
    
    def stop(self):
        """停止动画"""
        self.pause()
        bpy.context.scene.frame_set(bpy.context.scene.frame_current)
    
    def goto_frame(self, frame):
        """跳转到指定帧"""
        bpy.context.scene.frame_set(frame)
    
    def goto_start(self):
        """跳转到开始"""
        bpy.context.scene.frame_set(bpy.context.scene.frame_start)
    
    def goto_end(self):
        """跳转到结束"""
        bpy.context.scene.frame_set(bpy.context.scene.frame_end)
    
    def step_forward(self, steps=1):
        """前进"""
        bpy.context.scene.frame_set(bpy.context.scene.frame_current + steps)
    
    def step_backward(self, steps=1):
        """后退"""
        bpy.context.scene.frame_set(bpy.context.scene.frame_current - steps)
    
    def goto_previous_keyframe(self):
        """跳转到前一关键帧"""
        bpy.ops.screen.keyframe_jump(next=False)
    
    def goto_next_keyframe(self):
        """跳转到下一关键帧"""
        bpy.ops.screen.keyframe_jump(next=True)
```

## 动画数据操作

### 动作管理

```python
import bpy

class ActionManager:
    """动作管理器"""
    
    @staticmethod
    def create_action(name, obj=None):
        """创建新动作"""
        action = bpy.data.actions.new(name=name)
        
        if obj and obj.animation_data:
            obj.animation_data.action = action
        
        return action
    
    @staticmethod
    def assign_action(obj, action):
        """为对象分配动作"""
        if not obj.animation_data:
            obj.animation_data_create()
        
        obj.animation_data.action = action
    
    @staticmethod
    def push_action(obj, action):
        """推送动作到 NLA"""
        if not obj.animation_data:
            obj.animation_data_create()
        
        if not obj.animation_data.nla_track:
            track = obj.animation_data.nla_tracks.new()
            track.name = f"{obj.name}_Track"
        
        strip = track.strips.new(action.name, action, 0)
        return strip
    
    @staticmethod
    def push_down_action(obj):
        """将动作推送到 NLA"""
        if not obj.animation_data or not obj.animation_data.action:
            return None
        
        action = obj.animation_data.action
        
        bpy.ops.nla.push_down()
        
        return action
    
    @staticmethod
    def find_unused_actions():
        """查找未使用的动作"""
        unused = []
        for action in bpy.data.actions:
            used = False
            for obj in bpy.data.objects:
                if obj.animation_data:
                    if obj.animation_data.action == action:
                        used = True
                        break
                    if obj.animation_data.nla_tracks:
                        for track in obj.animation_data.nla_tracks:
                            for strip in track.strips:
                                if strip.action == action:
                                    used = True
                                    break
                            if used:
                                break
                        if used:
                            break
            if not used:
                unused.append(action)
        
        return unused
    
    @staticmethod
    def delete_unused_actions():
        """删除所有未使用的动作"""
        unused = ActionManager.find_unused_actions()
        for action in unused:
            bpy.data.actions.remove(action)
        print(f"已删除 {len(unused)} 个未使用的动作")
    
    @staticmethod
    def merge_actions(source_action, target_action):
        """合并动作"""
        # 复制源动作的关键帧到目标动作
        for src_fcurve in source_action.fcurves:
            dst_fcurve = None
            for dst_fcurve in target_action.fcurves:
                if dst_fcurve.data_path == src_fcurve.data_path:
                    dst_fcurve = dst_fcurve
                    break
            
            if not dst_fcurve:
                dst_fcurve = target_action.fcurves.new(data_path=src_fcurve.data_path)
            
            # 复制关键帧
            dst_fcurve.keyframe_points.add(len(src_fcurve.keyframe_points))
            for i, kp in enumerate(src_fcurve.keyframe_points):
                dst_kp = dst_fcurve.keyframe_points[i]
                dst_kp.co = kp.co
                dst_kp.handle_left = kp.handle_left
                dst_kp.handle_right = kp.handle_right
    
    @staticmethod
    def copy_action(source_obj, dest_obj):
        """复制动作"""
        if not source_obj.animation_data or not source_obj.animation_data.action:
            return None
        
        source_action = source_obj.animation_data.action
        new_action = source_action.copy()
        new_action.name = f"{source_action.name}_{dest_obj.name}"
        
        ActionManager.assign_action(dest_obj, new_action)
        
        return new_action
```

### NLA 操作

```python
import bpy

class NLAManager:
    """NLA 管理器"""
    
    @staticmethod
    def create_track(obj, name):
        """创建 NLA 轨道"""
        if not obj.animation_data:
            obj.animation_data_create()
        
        track = obj.animation_data.nla_tracks.new()
        track.name = name
        
        return track
    
    @staticmethod
    def add_strip(track, action, frame_start, frame_end, name=None):
        """在轨道上添加片段"""
        if name is None:
            name = action.name
        
        strip = track.strips.new(name, action, frame_start)
        strip.frame_end = frame_end
        
        return strip
    
    @staticmethod
    def apply_action_as_nla(obj, action):
        """将动作应用为 NLA"""
        if not obj.animation_data:
            obj.animation_data_create()
        
        # 创建轨道
        track = NLAManager.create_track(obj, f"{action.name}_Track")
        
        # 添加片段
        NLAManager.add_strip(track, action, 0, action.frame_end)
        
        return track
    
    @staticmethod
    def mute_track(track, mute=True):
        """静音轨道"""
        track.mute = mute
    
    @staticmethod
    def delete_track(track):
        """删除轨道"""
        obj = track.id_data
        if obj.animation_data:
            obj.animation_data.nla_tracks.remove(track)
    
    @staticmethod
    def move_strip(strip, frame_offset):
        """移动片段"""
        strip.frame_start += frame_offset
        strip.frame_end += frame_offset
    
    @staticmethod
    def trim_strip(strip, new_start, new_end):
        """裁剪片段"""
        strip.frame_start = new_start
        strip.frame_end = new_end
    
    @staticmethod
    def blend_strips(track, blend_type='CROSS'):
        """混合片段"""
        # 复杂的混合功能
        pass
```

## 动画工程面板

```python
class AnimationPanel(bpy.types.Panel):
    """动画面板"""
    bl_label = "动画工具"
    bl_idname = "ANIMATION_PT_animation_tools"
    bl_space_type = 'DOPESHEET_EDITOR'
    bl_region_type = 'UI'
    
    def draw(self, context):
        layout = self.layout
        scene = context.scene
        
        layout.label(text="时间轴:")
        
        row = layout.row()
        row.operator("screen.frame_jump", text="开始").end=False
        row.operator("screen.frame_jump", text="结束").end=True
        
        row = layout.row()
        row.operator("screen.keyframe_jump", text="上一帧").next=False
        row.operator("screen.keyframe_jump", text="下一帧").next=True
        
        row = layout.row()
        row.operator("screen.animation_play", text="播放").reverse=False
        row.operator("screen.animation_play", text="暂停").reverse=True
        
        layout.separator()
        layout.label(text="关键帧:")
        
        row = layout.row(align=True)
        row.operator("anim.keyframe_insert", text="插入").type='Location, Rotation, Scaling'
        row.operator("anim.keyframe_delete", text="删除").type='Location, Rotation, Scaling'
        
        layout.separator()
        layout.label(text="选择:")
        
        row = layout.row()
        row.operator("anim.select_all", text="全选").action='SELECT'
        row.operator("anim.select_all", text="反选").action='INVERT'
        
        row = layout.row()
        row.operator("anim.keyframe_type", text="关键帧").type='KEYFRAME'
        row.operator("anim.keyframe_type", text="衰减").type='BREAKDOWN'

class KeyframeEngineeringPanel(bpy.types.Panel):
    """关键帧工程面板"""
    bl_label = "关键帧工程"
    bl_idname = "ANIMATION_PT_keyframe_engineering"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '动画'
    
    def draw(self, context):
        layout = self.layout
        obj = context.object
        
        if not obj:
            layout.label(text="请选择对象")
            return
        
        layout.label(text=f"对象: {obj.name}")
        
        layout.separator()
        layout.label(text="关键帧操作:")
        
        row = layout.row(align=True)
        row.operator("keyframe_engineering.insert_all", text="插入全部")
        row.operator("keyframe_engineering.delete_all", text="删除全部")
        
        row = layout.row()
        row.operator("keyframe_engineering.copy", text="复制")
        row.operator("keyframe_engineering.paste", text="粘贴")
        
        layout.separator()
        layout.label(text="插值:")
        
        row = layout.row()
        row.operator("keyframe_engineering.set_linear", text="线性")
        row.operator("keyframe_engineering.set_bezier", text="贝塞尔")
        
        row = layout.row()
        row.operator("keyframe_engineering.set_constant", text="常数")
        row.operator("keyframe_engineering.set_cyclic", text="循环")
```

## 最佳实践

### 1. 动画优化

```python
def optimize_keyframes(obj, threshold=0.001):
    """优化关键帧"""
    if not obj.animation_data or not obj.animation_data.action:
        return
    
    action = obj.animation_data.action
    
    for fcurve in action.fcurves:
        keyframe_points = list(fcurve.keyframe_points)
        
        for i in range(len(keyframe_points) - 1):
            curr = keyframe_points[i]
            next_kp = keyframe_points[i + 1]
            
            # 检查是否可以合并
            if abs(curr.co.y - next_kp.co.y) < threshold:
                # 计算中间帧
                mid_frame = (curr.co.x + next_kp.co.x) / 2
                mid_value = (curr.co.y + next_kp.co.y) / 2
                
                # 删除中间关键帧
                # 这里需要更复杂的逻辑
                pass
```

### 2. 动画备份

```python
def backup_animation(obj):
    """备份动画"""
    if not obj.animation_data:
        return None
    
    action = obj.animation_data.action
    if not action:
        return None
    
    # 复制动作
    backup_action = action.copy()
    backup_action.name = f"{action.name}_backup"
    
    return backup_action

def restore_animation(obj, backup_action):
    """恢复动画"""
    if not obj.animation_data:
        obj.animation_data_create()
    
    obj.animation_data.action = backup_action
```

### 3. 动画验证

```python
def validate_animation(obj):
    """验证动画"""
    issues = []
    
    if not obj.animation_data:
        issues.append("对象没有动画数据")
        return issues
    
    action = obj.animation_data.action
    if not action:
        issues.append("对象没有动作")
        return issues
    
    # 检查关键帧
    for fcurve in action.fcurves:
        if len(fcurve.keyframe_points) < 2:
            issues.append(f"曲线 {fcurve.data_path} 关键帧不足")
        
        # 检查排序
        frames = [kp.co.x for kp in fcurve.keyframe_points]
        if frames != sorted(frames):
            issues.append(f"曲线 {fcurve.data_path} 关键帧未排序")
    
    return issues
```

## 常见问题

### Q: 关键帧不工作
A: 检查对象是否有动画数据，确保在正确的帧插入关键帧。

### Q: 动画不流畅
A: 调整插值类型，使用贝塞尔或弹性缓动。

### Q: 动画无法循环
A: 设置外推类型为 `MAKE_CYCLIC`，确保首尾关键帧值一致。

## 相关模块

- [bpy.ops](bpy_ops_detailed_module.md) - Blender 操作符
- [bpy.types](bpy_types_detailed_module.md) - 数据类型
- [bpy.data](bpy_data_module.md) - 数据访问
- [bpy.context](bpy_context_module.md) - 上下文访问


---
## bpy_ops_nla

# bpy.ops.nla 模块

非线性动画操作模块，用于管理动画条、轨道和动画混合。

## 概述

bpy.ops.nla 模块包含用于在非线性动画编辑器中操作动画的运算符。NLA编辑器用于管理多个动画片段的叠加和混合，支持复杂的动画层级结构。

## 常用操作

### 轨道操作

```python
import bpy

# 添加轨道
bpy.ops.nla.track_add()

# 删除轨道
bpy.ops.nla.track_delete()

# 移动轨道
bpy.ops.nla.track_move(direction='UP')
bpy.ops.nla.track_move(direction='DOWN')

# 选择轨道
bpy.ops.nla.track_select(extend=False)

# 取消选择轨道
bpy.ops.nla.tracks_deselect()
```

### 动画条操作

```python
# 添加动画条
bpy.ops.nla.strip_add()

# 删除动画条
bpy.ops.nla.strip_delete()

# 移动动画条
bpy.ops.nla.strip_move(direction='LEFT')
bpy.ops.nla.strip_move(direction='RIGHT')

# 调整动画条大小
bpy.ops.nla.strip_modal()

# 拆分动画条
bpy.ops.nla.strip_split()

# 标记切片
bpy.ops.nla.strip_jump()

# 选择动画条
bpy.ops.nla.select(extend=False)
bpy.ops.nla.select(extend=True, align=True)

# 切换选择
bpy.ops.nla.select_border(extension=False)
```

### 动作操作

```python
# 推行动作到NLA
bpy.ops.nla.push_down()

# 弹出动作
bpy.ops.nla.pop()

# 应用混合
bpy.ops.nla.apply_as()

# 创建动作
bpy.ops.nla.action_new()

# 转换到场景范围
bpy.ops.nla.toscenens()

# 合并动作
bpy.ops.nla.action_sync_length()
```

### 混合操作

```python
# 设置混合类型
bpy.ops.nla.blend_type(type='REPLACE')

# 设置混合强度
bpy.ops.nla.blend_down()

# 混合滑块
bpy.ops.nla.mixdown()

# 混合插值
bpy.ops.nla.mix_interp(type='AUTO')
```

### 关键帧操作

```python
# 插入关键帧
bpy.ops.nla.keyframe_insert(type='ALL')

# 删除关键帧
bpy.ops.nla.keyframe_delete(type='ALL')

# 清除关键帧
bpy.ops.nla.keyframe_clear(type='ALL')

# 添加所有重要关键帧
bpy.ops.nla.keyframe_insert(type='WHOLE_CHARACTER')
```

### 视图操作

```python
# 跳转到前一个关键帧
bpy.ops.nla.previewrange_set()

# 跳转到下一个关键帧
bpy.ops.nla.view_selected()

# 视图所有
bpy.ops.nla.view_all()

# 跳转到开始
bpy.ops.nla.frame_jump()

# 跳转到原点
bpy.ops.nla.origin_set()
```

### 复制粘贴操作

```python
# 复制
bpy.ops.nla.copy()

# 粘贴
bpy.ops.nla.paste()

# 粘贴到选中
bpy.ops.nla.paste(flipped=False)
```

## 实际应用示例

### 创建动画混合系统

```python
def create_animation_blend_system(obj, base_action, overlay_action, blend_frames=10):
    """创建动画混合系统"""
    # 推入基础动作到NLA
    bpy.context.view_layer.objects.active = obj
    obj.animation_data.action = base_action
    bpy.ops.nla.push_down()
    
    # 获取NLA轨道
    nla_tracks = obj.animation_data.nla_tracks
    
    # 创建覆盖动画轨道
    overlay_track = nla_tracks.new()
    overlay_track.name = 'OverlayAnim'
    
    # 设置动作
    obj.animation_data.action = overlay_action
    obj.animation_data.nla_tracks.active = overlay_track
    bpy.ops.nla.push_down()
    
    # 设置混合
    for strip in overlay_track.strips:
        strip.blend_type = 'ADD'
        strip.blend_in = blend_frames
        strip.blend_out = blend_frames
    
    return overlay_track
```

### 创建层级动画系统

```python
def create_layered_animation(obj, layers=3):
    """创建层级动画系统"""
    # 创建NLA轨道层级
    tracks = []
    
    for i in range(layers):
        # 添加轨道
        track = obj.animation_data.nla_tracks.new()
        track.name = f'AnimLayer_{i}'
        tracks.append(track)
        
        # 设置活动轨道
        obj.animation_data.nla_tracks.active = track
    
    return tracks

def add_animation_to_layer(obj, layer_index, action, start_frame=1, end_frame=30):
    """添加动画到指定层"""
    # 获取轨道
    tracks = obj.animation_data.nla_tracks
    if layer_index >= len(tracks):
        return None
    
    track = tracks[layer_index]
    
    # 设置动作
    obj.animation_data.action = action
    
    # 创建动画条
    strip = track.strips.new(action.name, start_frame, action)
    strip.frame_start = start_frame
    strip.frame_end = end_frame
    
    return strip
```

### 创建程序化动画序列

```python
def create_animated_sequence(obj, actions, frame_ranges):
    """创建动画序列"""
    current_frame = 1
    
    for i, action in enumerate(actions):
        # 创建轨道
        track = obj.animation_data.nla_tracks.new()
        track.name = f'Seq_{i}'
        
        # 设置动作
        obj.animation_data.action = action
        
        # 获取帧范围
        start_frame = frame_ranges[i][0]
        end_frame = frame_ranges[i][1]
        
        # 创建动画条
        strip = track.strips.new(action.name, start_frame, action)
        strip.frame_start = start_frame
        strip.frame_end = end_frame
        
        # 设置转换
        if i > 0:
            prev_strip = tracks[i-1].strips[-1]
            prev_strip.blend_out = 5
        
        strip.blend_in = 5
        strip.blend_type = 'REPLACE'
```

### 创建程序化行走循环

```python
def setup_procedural_walk_cycle(character, walk_actions, cycle_count=4):
    """设置程序化行走循环"""
    current_frame = 1
    total_frames = 0
    
    # 计算总帧数
    for action in walk_actions:
        if action.frame_range[1] > total_frames:
            total_frames = action.frame_range[1]
    
    # 创建主轨道
    main_track = character.animation_data.nla_tracks.new()
    main_track.name = 'WalkCycle'
    
    # 添加每个动作
    for i, action in enumerate(walk_actions):
        action_start = current_frame
        action_end = current_frame + (action.frame_range[1] - action.frame_range[0])
        
        # 设置动作
        character.animation_data.action = action
        
        # 创建动画条
        strip = main_track.strips.new(action.name, action_start, action)
        strip.frame_start = action_start
        strip.frame_end = action_end
        strip.repeat = cycle_count
        
        current_frame = action_end + 1
    
    return main_track
```

### 创建动画过渡系统

```python
def create_animation_transitions(obj, actions, transition_length=10):
    """创建动画过渡系统"""
    tracks = obj.animation_data.nla_tracks
    transition_track = tracks.new()
    transition_track.name = 'Transitions'
    
    for i in range(len(actions) - 1):
        current_action = actions[i]
        next_action = actions[i + 1]
        
        # 创建过渡动画条
        start_frame = int(current_action.frame_range[1])
        end_frame = start_frame + transition_length
        
        # 设置过渡动作
        obj.animation_data.action = current_action
        bpy.ops.nla.push_down()
        
        # 设置混合
        for strip in transition_track.strips:
            if strip.name == current_action.name:
                strip.blend_out = transition_length
        
        obj.animation_data.action = next_action
        bpy.ops.nla.push_down()
        
        for strip in transition_track.strips:
            if strip.name == next_action.name:
                strip.blend_in = transition_length
                strip.blend_type = 'ADD'
```

### 创建随机动画选择器

```python
def create_random_animation_selector(obj, actions, select_frame=1):
    """创建随机动画选择器"""
    tracks = obj.animation_data.nla_tracks
    
    # 创建随机选择轨道
    selector_track = tracks.new()
    selector_track.name = 'RandomSelector'
    
    # 添加所有动作到轨道
    for i, action in enumerate(actions):
        # 设置动作
        obj.animation_data.action = action
        
        # 随机起始帧
        import random
        random_start = random.randint(1, int(action.frame_range[1]))
        
        # 创建动画条
        strip = selector_track.strips.new(
            action.name,
            select_frame + i * 10,
            action
        )
        
        # 设置条件
        strip.use_reverse = False
        strip.repeat = 1
    
    return selector_track
```

## 使用场景

- **角色动画**：管理复杂的角色动画系统
- **动画混合**：混合多个动画片段
- **动画序列**：创建动画播放列表
- **动画分层**：创建动画层级系统
- **程序化动画**：程序化生成复杂动画
- **动画过渡**：创建平滑的动画过渡

## 注意事项

- NLA操作需要正确的活动对象
- 动画条不能重叠
- 混合类型影响最终动画效果
- 轨道顺序影响动画叠加
- 关键帧插入需要正确的时间范围
- 动作推入会创建新的动画条


---
## bpy_ops_graph

# bpy.ops.graph 模块

曲线编辑器操作模块，用于编辑动画曲线和关键帧。

## 概述

bpy.ops.graph 模块包含用于在曲线编辑器中操作动画曲线（F曲线）的运算符。曲线编辑器用于编辑动画的关键帧、曲线形状、插值方式等。

## 常用操作

### 关键帧操作

```python
import bpy

# 添加关键帧
bpy.ops.graph.keyframe_insert(type='ALL')

# 添加指定类型关键帧
bpy.ops.graph.keyframe_insert(type='LOC')
bpy.ops.graph.keyframe_insert(type='ROT')
bpy.ops.graph.keyframe_insert(type='SCALE')

# 删除关键帧
bpy.ops.graph.keyframe_delete(type='ALL')

# 删除选择的关键帧
bpy.ops.graph.delete()

# 清除所有关键帧
bpy.ops.graph.clean()

# 均化关键帧
bpy.ops.graph.euler_filter()
```

### 曲线选择操作

```python
# 选择所有曲线
bpy.ops.graph.select_all(action='SELECT')

# 取消选择
bpy.ops.graph.select_all(action='DESELECT')

# 反选
bpy.ops.graph.select_all(action='INVERT')

# 选择左边
bpy.ops.graph.select_border(extension=False, include_handles=False)

# 选择右边
bpy.ops.graph.select_border(extension=False, include_handles=False, left=False)

# 扩展选择
bpy.ops.graph.select_border(extension=True)

# 选择更少
bpy.ops.graph.select_less()

# 选择更多
bpy.ops.graph.select_more()
```

### 曲线编辑操作

```python
# 复制曲线
bpy.ops.graph.copy()

# 粘贴曲线
bpy.ops.graph.paste(offset='START')

# 粘贴到当前位置
bpy.ops.graph.paste(offset='CURRENT')

# 偏移粘贴
bpy.ops.graph.paste(offset='YES')

# 剪切曲线
bpy.ops.graph.cut()

# 删除曲线
bpy.ops.graph.delete(type='CURVE')

# 移除中间关键帧
bpy.ops.graph.decimate(type='SELECTED', ratio=0.1)
```

### 曲线平滑操作

```python
# 平滑曲线
bpy.ops.graph.smooth()

# 简化曲线
bpy.ops.graph.decimate(type='SELECTED', ratio=0.5)

# 采样关键帧
bpy.ops.graph.sample(type='SELECTED')

# 步进插值
bpy.ops.graph.handle_type(type='AUTO')

# 设置为自动
bpy.ops.graph.handle_type(type='AUTO')

# 设置为向量
bpy.ops.graph.handle_type(type='VECTOR')

# 设置为锚点
bpy.ops.graph.handle_type(type='ALIGNED')

# 设置为自由
bpy.ops.graph.handle_type(type='FREE')
```

### 插值类型操作

```python
# 设置插值类型
bpy.ops.graph.interpolation_type(type='BEZIER')

# 线性插值
bpy.ops.graph.interpolation_type(type='LINEAR')

# 常量插值
bpy.ops.graph.interpolation_type(type='CONSTANT')

# 弹性插值
bpy.ops.graph.interpolation_type(type='ELASTIC')

# 正弦插值
bpy.ops.graph.interpolation_type(type='SINE')

# 缓动插值
bpy.ops.graph.interpolation_type(type='EASE')

# 指数插值
bpy.ops.graph.interpolation_type(type='EXPO')
```

### 曲线外推

```python
# 循环外推
bpy.ops.graph.extrapolation_type(type='CYCLIC')

# 循环偏移外推
bpy.ops.graph.extrapolation_type(type='CYCLIC_OFFSET')

# 线性外推
bpy.ops.graph.extrapolation_type(type='LINEAR')

# 常量外推
bpy.ops.graph.extrapolation_type(type='CONSTANT')
```

### 曲线修饰器

```python
# 添加修饰器
bpy.ops.graph.modifier_add(type='CYCLES')

# 添加噪声修饰器
bpy.ops.graph.modifier_add(type='NOISE')

# 添加函数修饰器
bpy.ops.graph.modifier_add(type='FNGENERATOR')

# 添加限制范围修饰器
bpy.ops.graph.modifier_add(type='LIMITS')

# 添加Envelope修饰器
bpy.ops.graph.modifier_add(type='ENVELOPE')

# 应用修饰器
bpy.ops.graph.modifier_apply(modifier='Cycles')

# 删除修饰器
bpy.ops.graph.modifier_remove(modifier='Cycles')

# 移动修饰器
bpy.ops.graph.modifier_move_up(modifier='Noise')
bpy.ops.graph.modifier_move_down(modifier='Noise')
```

### 快照操作

```python
# 创建快照
bpy.ops.graph.snapshot(type='START')

# 创建完整快照
bpyops.graph.snapshot(type='FULL')

# 清除快照
bpy.ops.graph.snapshot_clear(type='START')

# 恢复到快照
bpy.ops.graph.snapshot_apply(type='START')

# 恢复到原始状态
bpy.ops.graph.snapshot_apply_original(type='START')
```

### 视图操作

```python
# 居中选区
bpy.ops.graph.view_selected()

# 居中全部
bpy.ops.graph.view_all()

# 跳转到关键帧
bpy.ops.graph.frame_jump()

# 跳转到第一个关键帧
bpy.ops.graph.first_frame()

# 跳转到最后一个关键帧
bpy.ops.graph.last_frame()

# 放大
bpy.ops.graph.zoom_in()

# 缩小
bpy.ops.graph.zoom_out()

# 平移
bpy.ops.graph.pan()
```

## 实际应用示例

### 动画曲线清理

```python
def clean_animation_curves():
    """清理动画曲线"""
    # 均化欧拉角
    bpy.ops.graph.euler_filter()
    
    # 清理多余关键帧
    bpy.ops.graph.clean()
    
    # 平滑选中的曲线
    bpy.ops.graph.select_all(action='SELECT')
    bpy.ops.graph.smooth()
    
    # 设置合适的插值
    for fcurve in bpy.context.active_object.animation_data.action.fcurves:
        bpy.ops.graph.select_all(action='DESELECT')
        fcurve.select = True
        bpy.context.area.type = 'GRAPH_EDITOR'
        bpy.ops.graph.interpolation_type(type='AUTO')
```

### 创建程序化动画曲线

```python
def create_bounce_animation(obj, frames=60, bounce_height=2.0):
    """创建弹跳动画曲线"""
    # 设置物体位置
    obj.location = (0, 0, 0)
    
    # 插入起始关键帧
    obj.keyframe_insert(data_path='location', frame=1)
    
    # 设置弹跳位置
    obj.location = (0, 0, bounce_height)
    obj.keyframe_insert(data_path='location', frame=15)
    
    obj.location = (0, 0, 0)
    obj.keyframe_insert(data_path='location', frame=30)
    
    obj.location = (0, 0, bounce_height * 0.6)
    obj.keyframe_insert(data_path='location', frame=45)
    
    obj.location = (0, 0, 0)
    obj.keyframe_insert(data_path='location', frame=60)
    
    # 编辑曲线插值
    bpy.context.area.type = 'GRAPH_EDITOR'
    bpy.ops.graph.select_all(action='SELECT')
    
    # 设置为弹性插值
    bpy.ops.graph.interpolation_type(type='ELASTIC')
    
    # 设置出点为自动
    bpy.ops.graph.handle_type(type='AUTO')
```

### 批量设置动画曲线

```python
def batch_set_curve_interpolation(interp_type='LINEAR'):
    """批量设置动画曲线插值类型"""
    # 确保在曲线编辑器中
    bpy.context.area.type = 'GRAPH_EDITOR'
    
    # 获取当前动作
    action = bpy.context.active_object.animation_data.action
    
    for fcurve in action.fcurves:
        # 选择曲线
        bpy.ops.graph.select_all(action='DESELECT')
        fcurve.select = True
        bpy.context.active_object.active_rotation_keyframe = fcurve
        
        # 设置插值
        bpy.ops.graph.interpolation_type(type=interp_type)
```

### 创建循环动画

```python
def setup_looping_animation(obj, loop_frames=30):
    """设置循环动画"""
    # 选中所有曲线
    bpy.context.area.type = 'GRAPH_EDITOR'
    bpy.ops.graph.select_all(action='SELECT')
    
    # 设置循环外推
    bpy.ops.graph.extrapolation_type(type='CYCLIC')
    
    # 设置曲线为循环
    for fcurve in obj.animation_data.action.fcurves:
        # 确保首尾关键帧匹配
        first_key = fcurve.keyframe_points[0]
        last_key = fcurve.keyframe_points[-1]
        
        if first_key.co[0] != 0:
            bpy.ops.graph.keyframe_insert(type='ALL')
```

### 曲线噪声效果

```python
def add_noise_to_curves(obj, noise_strength=0.1, noise_scale=1.0):
    """为曲线添加噪声效果"""
    bpy.context.area.type = 'GRAPH_EDITOR'
    bpy.ops.graph.select_all(action='SELECT')
    
    for fcurve in obj.animation_data.action.fcurves:
        # 添加噪声修饰器
        bpy.ops.graph.modifier_add(type='NOISE')
        
        modifier = fcurve.modifiers[-1]
        modifier.strength = noise_strength
        modifier.scale = noise_scale
        modifier.phase = 0.0
        modifier.depth = 0.0
        
        # 更新视图
        bpy.ops.graph.update()
```

### 简化动画曲线

```python
def simplify_curves(obj, threshold=0.1):
    """简化动画曲线关键帧"""
    bpy.context.area.type = 'GRAPH_EDITOR'
    
    for fcurve in obj.animation_data.action.fcurves:
        # 选择曲线
        bpy.ops.graph.select_all(action='DESELECT')
        fcurve.select = True
        
        # 简化曲线
        bpy.ops.graph.decimate(type='SELECTED', ratio=threshold)
```

## 使用场景

- **动画编辑**：编辑和调整动画曲线
- **曲线清理**：清理多余的关键帧和平滑曲线
- **插值控制**：设置关键帧之间的插值方式
- **循环动画**：创建循环动画曲线
- **曲线特效**：使用修饰器添加噪声等效果
- **动画烘焙**：烘焙动画数据

## 注意事项

- 曲线编辑需要正确的区域类型
- 删除操作不可逆
- 外推设置影响循环动画
- 修饰器会实时影响曲线
- 插值类型影响动画感觉
- 关键帧清洁不会影响动画结果


---
## bpy_ops_dopesheet

# bpy.ops.dopesheet - 动画摄影表操作模块

bpy.ops.dopesheet模块提供了动画摄影表（ dope sheet）编辑器中的操作功能，用于管理和编辑关键帧动画数据。

## 概述

动画摄影表是Blender中用于查看和编辑动画数据的重要编辑器。这个模块提供了在摄影表环境中进行选择、编辑和时间管理的操作。

## 核心功能

### 关键帧选择

```python
import bpy

def select_all_keyframes():
    """选择所有关键帧"""
    bpy.ops.dopesheet.select_all(action='SELECT')

def deselect_all_keyframes():
    """取消选择所有关键帧"""
    bpy.ops.dopesheet.select_all(action='DESELECT')

def select_keyframes_in_range(start_frame, end_frame):
    """选择范围内的关键帧"""
    bpy.context.scene.frame_start = start_frame
    bpy.context.scene.frame_end = end_frame
    bpy.ops.dopsheet.select_border(region_filter=('DOPESHEET',))
```

### 关键帧编辑

```python
def delete_selected_keyframes():
    """删除选中的关键帧"""
    bpy.ops.dopesheet.delete()

def duplicate_keyframes():
    """复制关键帧"""
    bpy.ops.dopesheet.duplicate()

def copy_keyframes():
    """复制关键帧"""
    bpy.ops.dopesheet.copy()

def paste_keyframes():
    """粘贴关键帧"""
    bpy.ops.dopesheet.paste()
```

### 时间操作

```python
def set_preview_range(start, end):
    """设置预览范围"""
    bpy.context.scene.frame_preview_start = start
    bpy.context.scene.frame_preview_end = end

def jump_to_frame(frame):
    """跳转到指定帧"""
    bpy.context.scene.frame_current = frame

def frame_jump(end=True):
    """帧跳转"""
    bpy.ops.dopesheet.frame_jump(end=end)

def frame_ping_pong():
    """帧往复"""
    bpy.ops.dopesheet.ping_pong()
```

## 实用函数

### 通道操作

```python
def expand_all_channels():
    """展开所有通道"""
    bpy.ops.dopesheet.channels_expand()

def collapse_all_channels():
    """折叠所有通道"""
    bpy.ops.dopesheet.channels_collapse()

def select_channel_hierarchy():
    """选择通道层级"""
    bpy.ops.dopesheet.select_hierarchy(direction='CHILDREN')
```

### 关键帧操作

```python
def set_keyframe_on_selected():
    """在选中属性上设置关键帧"""
    bpy.ops.dopesheet.keyframe_insert()

def delete_keyframe_at_cursor():
    """在光标处删除关键帧"""
    bpy.ops.dopesheet.keyframe_delete()

def clean_keyframes():
    """清理关键帧"""
    bpy.ops.dopesheet.clean()
```

## 常见问题排查

### 选择问题

选择不工作：
- 确认编辑器为激活状态
- 检查是否有可选择对象
- 验证选择模式设置

### 复制粘贴问题

剪贴板为空：
- 先执行复制操作
- 检查目标位置是否有效
- 确认动画数据类型匹配


---
## bpy_ops_keying_set

# bpy.ops.keying_set - 关键帧设置操作模块

Blender关键帧设置操作符，用于管理和批量设置关键帧。

## 概述

`bpy.ops.keying_set` 模块提供用于创建、编辑和管理关键帧设置的功能。关键帧设置允许您批量为多个属性设置关键帧。

## 核心功能

### 关键帧设置创建

```python
import bpy

# 创建新关键帧设置
bpy.ops.keying_set.new(
    name='MyKeyingSet',
    type='INSERT'
)

# 创建插入关键帧设置
bpy.ops.keying_set.new(
    name='InsertAll',
    type='INSERT'
)

# 创建覆盖关键帧设置
bpy.ops.keying_set.new(
    name='OverrideAll',
    type='OVERRIDE'
)

# 创建清除关键帧设置
bpy.ops.keying_set.new(
    name='ClearAll',
    type='CLEAR'
)
```

### 关键帧设置操作

```python
# 删除关键帧设置
bpy.ops.keying_set.remove()

# 重命名关键帧设置
bpy.ops.keying_set.rename(
    name='NewName'
)

# 复制关键帧设置
bpy.ops.keying_set.duplicate()

# 添加路径到关键帧设置
bpy.ops.keying_set.path_add(
    data_path='location'
)

# 添加路径变体
bpy.ops.keying_set.path_add(
    data_path='rotation_euler',
    index=0
)

# 移除路径
bpy.ops.keying_set.path_remove()

# 移除所有路径
bpy.ops.keying_set.path_remove_all()
```

### 关键帧操作

```python
# 插入关键帧
bpy.ops.keyframe_insert()

# 插入活动对象关键帧
bpy.ops.keyframe_insert(
    type='ACTIVEOBJ'
)

# 插入选择对象关键帧
bpy.ops.keyframe_insert(
    type='SELECTED'
)

# 插入可见对象关键帧
bpy.ops.keyframe_insert(
    type='VISIBLE'
)

# 清除关键帧
bpy.ops.keyframe_delete()

# 清除所有关键帧
bpy.ops.keyframe_delete(
    type='ALL'
)

# 清除插值
bpy.ops.keyframe_clear_viable()
```

### 批量关键帧设置

```python
# 创建完整动画关键帧设置
def create_full_animation_keying_set():
    bpy.ops.keying_set.new(name='FullAnimation', type='INSERT')
    keying_set = bpy.context.scene.keying_sets.active
    
    paths = [
        'location',
        'rotation_euler',
        'scale',
        'rotation_quaternion',
        'delta_location',
        'delta_rotation_euler',
        'delta_scale'
    ]
    
    for path in paths:
        keying_set.paths.add(
            data_path=path,
            index=-1,
            group_method='NONE',
            group_name=''
        )
    
    return keying_set

# 创建材质关键帧设置
def create_material_keying_set():
    bpy.ops.keying_set.new(name='MaterialAnim', type='INSERT')
    keying_set = bpy.context.scene.keying_sets.active
    
    material_paths = [
        'diffuse_color',
        'specular_color',
        'specular_intensity',
        'metallic',
        'roughness',
        'alpha',
        'emission_color',
        'emission_strength'
    ]
    
    for path in material_paths:
        keying_set.paths.add(
            data_path=f'materials[""].{path}',
            index=-1
        )
    
    return keying_set

# 创建约束关键帧设置
def create_constraint_keying_set():
    bpy.ops.keying_set.new(name='ConstraintAnim', type='INSERT')
    keying_set = bpy.context.scene.keying_sets.active
    
    for i in range(32):
        keying_set.paths.add(
            data_path=f'constraints[""].influence',
            index=i
        )
    
    return keying_set

# 创建修改器关键帧设置
def create_modifier_keying_set():
    bpy.ops.keying_set.new(name='ModifierAnim', type='INSERT')
    keying_set = bpy.context.scene.keying_sets.active
    
    for i in range(32):
        keying_set.paths.add(
            data_path=f'modifiers[""].factor',
            index=i
        )
    
    return keying_set
```

## 实用函数

```python
def get_keying_set_paths(keying_set_name):
    """获取关键帧设置的所有路径"""
    keying_set = bpy.context.scene.keying_sets.get(keying_set_name)
    if keying_set:
        return [(p.data_path, p.array_index) for p in keying_set.paths]
    return []

def add_object_properties_to_keying_set(keying_set_name, obj):
    """添加对象属性到关键帧设置"""
    keying_set = bpy.context.scene.keying_sets.get(keying_set_name)
    if keying_set:
        for prop in obj.bl_rna.properties:
            if prop.is_animable:
                keying_set.paths.add(
                    data_path=f'objects["{obj.name}"].{prop.identifier}',
                    index=-1
                )

def batch_keyframe_objects(object_names, frame):
    """批量为对象设置关键帧"""
    bpy.context.scene.frame_set(frame)
    
    for obj_name in object_names:
        obj = bpy.data.objects.get(obj_name)
        if obj:
            bpy.context.view_layer.objects.active = obj
            bpy.ops.keyframe_insert(type='ACTIVEOBJ')

def create_visibility_keying_set():
    """创建可见性关键帧设置"""
    bpy.ops.keying_set.new(name='Visibility', type='INSERT')
    keying_set = bpy.context.scene.keying_sets.active
    
    keying_set.paths.add(
        data_path='hide_viewport',
        index=-1
    )
    keying_set.paths.add(
        data_path='hide_render',
        index=-1
    )
    keying_set.paths.add(
        data_path='hide_select',
        index=-1
    )
    
    return keying_set

def sync_keying_sets_to_action(action_name):
    """同步关键帧设置到动作"""
    action = bpy.data.actions.get(action_name)
    if action:
        for keying_set in bpy.context.scene.keying_sets:
            for path in keying_set.paths:
                if path.id_type == 'OBJECT':
                    path.id = bpy.context.active_object

def export_keying_set(keying_set_name, filepath):
    """导出关键帧设置"""
    import json
    
    keying_set = bpy.context.scene.keying_sets.get(keying_set_name)
    if keying_set:
        data = {
            'name': keying_set.bl_label,
            'type': keying_set.bl_idname,
            'paths': [(p.data_path, p.array_index) for p in keying_set.paths]
        }
        
        with open(filepath, 'w') as f:
            json.dump(data, f, indent=2)
```

## 常见问题排查

### 关键帧不生效
```python
# 检查关键帧设置
keying_set = bpy.context.scene.keying_sets.active
print(f"关键帧设置: {keying_set}")
print(f"路径数: {len(keying_set.paths) if keying_set else 0}")

# 检查路径
if keying_set:
    for path in keying_set.paths:
        print(f"路径: {path.data_path}, 索引: {path.array_index}")

# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")
print(f"动作: {obj.animation_data.action if obj.animation_data else None}")
```

### 批量关键帧问题
```python
# 检查帧范围
print(f"开始帧: {bpy.context.scene.frame_start}")
print(f"结束帧: {bpy.context.scene.frame_end}")
print(f"当前帧: {bpy.context.scene.frame_current}")

# 设置正确的帧
bpy.context.scene.frame_set(1)

# 检查对象是否被锁定
obj = bpy.context.active_object
print(f"锁定: {obj.lock_location}")
print(f"旋转锁定: {obj.lock_rotation}")
print(f"缩放锁定: {obj.lock_scale}")
```

### 关键帧设置丢失
```python
# 检查所有关键帧设置
print("所有关键帧设置:")
for ks in bpy.context.scene.keying_sets:
    print(f"  {ks.name}: {len(ks.paths)} 路径")

# 重新创建关键帧设置
bpy.ops.keying_set.new(name='Recovery', type='INSERT')

# 从备份恢复
import json
with open('//keying_set_backup.json', 'r') as f:
    data = json.load(f)
    bpy.ops.keying_set.new(name=data['name'], type='INSERT')
    ks = bpy.context.scene.keying_sets.active
    for path_data in data['paths']:
        ks.paths.add(data_path=path_data[0], index=path_data[1])
```


---
## bpy_ops_poselib

# bpy.ops.poselib 模块

姿态库操作模块，用于管理、保存和应用骨骼姿态。

## 概述

bpy.ops.poselib 模块包含用于创建和管理姿态库的运算符。姿态库允许你保存和重用骨骼的各种姿态，方便动画制作和角色动作管理。

## 常用操作

### 姿态库管理

```python
import bpy

# 读取姿态库
bpy.ops.poselib.read(filepath="path/to/pose_library.blend")

# 写入姿态库
bpy.ops.poselib.write(filepath="path/to/pose_library.blend")

# 读取所有姿态
bpy.ops.poselib.read_internally(filepath="path/to/file.blend")

# 导出姿态
bpy.ops.poselib.pose_export(filepath="path/to/pose.json")

# 导入姿态
bpy.ops.poselib.pose_import(filepath="path/to/pose.json")
```

### 姿态操作

```python
# 添加当前姿态
bpy.ops.poselib.pose_add(name='NewPose')

# 删除姿态
bpy.ops.poselib.pose_remove(pose_index=0)

# 重命名姿态
bpy.ops.poselib.pose_rename(old_name='OldName', new_name='NewName')

# 复制姿态
bpy.ops.poselib.pose_copy()

# 粘贴姿态
bpy.ops.poselib.pose_paste()

# 应用姿态
bpy.ops.poselib.apply_pose(pose_index=0)
```

### 姿态插值

```python
# 插值到姿态
bpy.ops.poselib.pose_blend(pose_index=0, factor=0.5)

# 混合姿态
bpy.ops.poselib.pose_mix(pose1_index=0, pose2_index=1, factor=0.5)

# 姿态循环
bpy.ops.poselib.pose_cyclic(factor=1.0)
```

## 实际应用示例

### 创建姿态库

```python
def create_pose_library(armature_obj, pose_names):
    """创建姿态库"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    armature_obj.select_set(True)
    
    # 进入姿态模式
    bpy.ops.object.mode_set(mode='POSE')
    
    # 为每个名称创建姿态
    for i, pose_name in enumerate(pose_names):
        # 设置姿态
        set_specific_pose(armature_obj, i)
        
        # 添加到姿态库
        bpy.ops.poselib.pose_add(name=pose_name)
    
    return armature_obj.data.poselib

def set_specific_pose(armature_obj, pose_index):
    """设置特定姿态"""
    # 根据索引设置不同的姿态
    if pose_index == 0:
        # T字姿态
        set_t_pose(armature_obj)
    elif pose_index == 1:
        # 休息姿态
        set_rest_pose(armature_obj)
    elif pose_index == 2:
        # 行走姿态
        set_walk_pose(armature_obj)

def set_t_pose(armature_obj):
    """设置T字姿态"""
    for bone in armature_obj.pose.bones:
        bone.rotation_mode = 'XYZ'
        bone.rotation_euler = (0, 0, 0)
        bone.location = (0, 0, 0)

def set_rest_pose(armature_obj):
    """设置休息姿态"""
    for bone in armature_obj.pose.bones:
        bone.rotation_mode = 'XYZ'
        bone.rotation_euler = (0, 0, 0)
        if 'arm' in bone.name.lower():
            bone.rotation_euler = (0, 0, -0.3)
        if 'leg' in bone.name.lower():
            bone.rotation_euler = (0, 0, 0.1)
```

### 保存所有姿态

```python
def save_all_poses(armature_obj, filepath):
    """保存所有姿态到文件"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    
    # 保存姿态库
    bpy.ops.poselib.write(filepath=filepath)
```

### 加载姿态库

```python
def load_pose_library(filepath):
    """加载姿态库"""
    # 读取姿态库文件
    bpy.ops.poselib.read(filepath=filepath)
    
    # 返回姿态库
    return bpy.context.scene.poselib
```

### 应用姿态动画

```python
def apply_pose_sequence(armature_obj, pose_indices, frame_duration=10):
    """应用姿态序列动画"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    armature_obj.select_set(True)
    
    # 进入姿态模式
    bpy.ops.object.mode_set(mode='POSE')
    
    current_frame = 1
    
    for pose_index in pose_indices:
        # 应用姿态
        bpy.ops.poselib.apply_pose(pose_index=pose_index)
        
        # 插入关键帧
        for bone in armature_obj.pose.bones:
            bone.keyframe_insert(data_path='rotation_euler', frame=current_frame)
            bone.keyframe_insert(data_path='location', frame=current_frame)
        
        # 移动到下一帧
        current_frame += frame_duration
```

### 创建姿态过渡

```python
def create_pose_transition(armature_obj, from_pose_index, to_pose_index, duration=10):
    """创建姿态过渡动画"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    armature_obj.select_set(True)
    
    # 进入姿态模式
    bpy.ops.object.mode_set(mode='POSE')
    
    # 设置起始姿态
    bpy.ops.poselib.apply_pose(pose_index=from_pose_index)
    
    start_frame = bpy.context.scene.frame_current
    
    # 设置结束姿态
    bpy.ops.poselib.apply_pose(pose_index=to_pose_index)
    
    end_frame = start_frame + duration
    
    # 插值中间帧
    bpy.ops.poselib.pose_blend(pose_index=to_pose_index, factor=0.5)
    
    # 插入关键帧
    for bone in armature_obj.pose.bones:
        bone.keyframe_insert(data_path='rotation_euler', frame=start_frame)
        bone.keyframe_insert(data_path='location', frame=start_frame)
        bone.keyframe_insert(data_path='rotation_euler', frame=end_frame)
        bone.keyframe_insert(data_path='location', frame=end_frame)
```

### 批量处理姿态

```python
def batch_process_poses(armature_obj, process_func):
    """批量处理姿态"""
    # 获取姿态库
    poselib = armature_obj.data.poselib
    
    # 保存原始姿态
    save_current_pose(armature_obj)
    
    # 处理每个姿态
    for i in range(len(poselib.pose_markers)):
        # 应用姿态
        bpy.ops.poselib.apply_pose(pose_index=i)
        
        # 应用处理函数
        process_func(armature_obj)
        
        # 重新保存姿态
        poselib.pose_markers[i].pose_library.pose_markers[i].name = poselib.pose_markers[i].name
        save_current_pose(armature_obj)

def save_current_pose(armature_obj):
    """保存当前姿态"""
    # 在当前帧保存姿态
    for bone in armature_obj.pose.bones:
        bone.keyframe_insert(data_path='rotation_euler')
        bone.keyframe_insert(data_path='location')
```

### 创建随机姿态

```python
def create_random_poses(armature_obj, count=10):
    """创建随机姿态库"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    armature_obj.select_set(True)
    
    # 进入姿态模式
    bpy.ops.object.mode_set(mode='POSE')
    
    import random
    
    for i in range(count):
        # 随机设置每个骨骼的姿态
        for bone in armature_obj.pose.bones:
            if not bone.lock_rotation[0]:
                bone.rotation_euler.x = random.uniform(-0.5, 0.5)
            if not bone.lock_rotation[1]:
                bone.rotation_euler.y = random.uniform(-0.5, 0.5)
            if not bone.lock_rotation[2]:
                bone.rotation_euler.z = random.uniform(-0.5, 0.5)
        
        # 添加到姿态库
        bpy.ops.poselib.pose_add(name=f'RandomPose_{i}')
```

### 镜像姿态

```python
def mirror_pose_library(armature_obj):
    """镜像姿态库"""
    # 获取姿态库
    poselib = armature_obj.data.poselib
    
    # 遍历所有姿态
    for marker in poselib.pose_markers:
        # 应用姿态
        bpy.ops.poselib.apply_pose(pose_index=marker.index)
        
        # 镜像每个骨骼
        for bone in armature_obj.pose.bones:
            # 检查是否是对称骨骼
            if bone.name.endswith('.L') or bone.name.endswith('.L'):
                # 获取对称骨骼
                mirror_name = bone.name.replace('.L', '.R').replace('.R', '.L')
                mirror_bone = armature_obj.pose.bones.get(mirror_name)
                
                if mirror_bone:
                    # 镜像旋转
                    mirror_bone.rotation_euler.x = bone.rotation_euler.x
                    mirror_bone.rotation_euler.y = bone.rotation_euler.y
                    mirror_bone.rotation_euler.z = -bone.rotation_euler.z
        
        # 添加镜像姿态
        bpy.ops.poselib.pose_add(name=f'{marker.name}_Mirror')
```

### 导出姿态为JSON

```python
def export_poses_to_json(armature_obj, filepath):
    """导出姿态为JSON文件"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    
    # 导出姿态
    bpy.ops.poselib.pose_export(filepath=filepath)

def import_poses_from_json(armature_obj, filepath):
    """从JSON文件导入姿态"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    armature_obj.select_set(True)
    
    # 导入姿态
    bpy.ops.poselib.pose_import(filepath=filepath)
```

### 创建姿态预览动画

```python
def create_pose_preview_animation(armature_obj, fps=12, frames_per_pose=8):
    """创建姿态预览动画"""
    # 确保选中骨架
    bpy.context.view_layer.objects.active = armature_obj
    armature_obj.select_set(True)
    
    # 进入姿态模式
    bpy.ops.object.mode_set(mode='POSE')
    
    # 获取姿态库
    poselib = armature_obj.data.poselib
    pose_count = len(poselib.pose_markers)
    
    # 设置帧率
    bpy.context.scene.render.fps = fps
    
    current_frame = 1
    
    for i, marker in enumerate(poselib.pose_markers):
        # 应用姿态
        bpy.ops.poselib.apply_pose(pose_index=marker.index)
        
        # 在每帧插入关键帧
        for j in range(frames_per_pose):
            frame = current_frame + j
            bpy.context.scene.frame_set(frame)
            
            for bone in armature_obj.pose.bones:
                bone.keyframe_insert(data_path='rotation_euler', frame=frame)
                bone.keyframe_insert(data_path='location', frame=frame)
        
        current_frame += frames_per_pose
    
    # 设置场景帧范围
    bpy.context.scene.frame_end = current_frame - 1
```

## 使用场景

- **角色动画**：保存和重用角色姿态
- **动作捕捉**：处理动作捕捉数据
- **游戏开发**：导出姿态用于游戏引擎
- **动画制作**：快速创建姿态转换
- **角色设计**：探索不同的姿态选项
- **批量处理**：自动化姿态处理流程
- **姿态预览**：创建姿态预览动画
- **数据交换**：与其他软件交换姿态数据
- **版本管理**：管理不同版本姿态
- **团队协作**：共享姿态库

## 注意事项

- 姿态库需要在骨架数据中创建
- 应用姿态前需要进入姿态模式
- 骨骼名称必须一致才能正确应用
- 镜像姿态需要对称骨骼命名
- 大型姿态库需要组织管理
- 导出前应验证姿态完整性
- 定期备份姿态库文件
- 注意骨骼约束对姿态的影响
- 锁定骨骼不会被姿态影响
- 插值动画需要平滑的关键帧


---
## bpy_ops_constraint

# bpy.ops.constraint 模块

约束操作模块，用于创建和管理物体间的约束关系。

## 概述

bpy.ops.constraint 模块包含用于在物体上添加、编辑和删除约束的运算符。这些约束控制物体之间的运动关系，如跟随目标、复制变换、父子关系等。

## 常用操作

### 约束添加

```python
import bpy

# 添加约束到选中物体
bpy.ops.constraint.add(type='COPY_LOCATION')

# 添加目标物体类型的约束
bpy.ops.constraint.add(type='TRACK_TO')

# 添加影响特定轴的约束
bpy.ops.constraint.add(type='COPY_LOCATION', influence=0.5)
```

### 约束类型

```python
# 复制位置
bpy.ops.constraint.add(type='COPY_LOCATION')
bpy.ops.constraint.add(type='COPY_LOCATION', use_x=True, use_y=True, use_z=True)

# 复制旋转
bpy.ops.constraint.add(type='COPY_ROTATION')
bpy.ops.constraint.add(type='COPY_ROTATION', use_x=True, use_y=True, use_z=True)

# 复制缩放
bpy.ops.constraint.add(type='COPY_SCALE')
bpy.ops.constraint.add(type='COPY_SCALE', use_x=True, use_y=True, use_z=True)

# 复制变换
bpy.ops.constraint.add(type='COPY_TRANSFORMS')

# 跟随路径
bpy.ops.constraint.add(type='FOLLOW_PATH')
bpy.ops.constraint.add(type='FOLLOW_PATH', follow_curve=True, use_curve_follow=True)

# 变换到变换
bpy.ops.constraint.add(type='TRANSFORM_TO')

# 变换来自
bpy.ops.constraint.add(type='TRANSFORM_FROM')

# 变换偏移
bpy.ops.constraint.add(type='TRANSFORM_OFFSET')

# 限制位置
bpy.ops.constraint.add(type='LIMIT_LOCATION')
bpy.ops.constraint.add(type='LIMIT_LOCATION', use_min_x=True, use_max_x=True)

# 限制旋转
bpy.ops.constraint.add(type='LIMIT_ROTATION')
bpy.ops.constraint.add(type='LIMIT_ROTATION', use_min_x=True, use_max_x=True)

# 限制缩放
bpy.ops.constraint.add(type='LIMIT_SCALE')
bpy.ops.constraint.add(type='LIMIT_SCALE', use_min_x=True, use_max_x=True)

# 维护体积
bpy.ops.constraint.add(type='MAINTAIN_VOLUME')

# 变换混合
bpy.ops.constraint.add(type='TRANSFORM_MIX')
bpy.ops.constraint.add(type='TRANSFORM_MIX', use_location=True, use_rotation=True)
```

### 约束操作

```python
# 选中约束
bpy.ops.constraint.select(action='SELECT')
bpy.ops.constraint.select(action='DESELECT')
bpy.ops.constraint.select(action='INVERT')

# 移动约束
bpy.ops.constraint.move(type='UP')
bpy.ops.constraint.move(type='DOWN')

# 删除约束
bpy.ops.constraint.delete()

# 应用约束
bpy.ops.constraint.apply(constraint='Constraint')

# 禁用约束
bpy.ops.constraint.disable(constraint='Constraint')
bpy.ops.constraint.enable(constraint='Constraint')

# 设置目标
bpy.ops.constraint.set_target(target='TargetObject')

# 设置子物体
bpy.ops.constraint.set_subtarget(subtarget='BoneName')

# 复制约束
bpy.ops.constraint.copy')

# 粘贴约束
bpy.ops.constraint.paste')
```

### 约束目标操作

```python
# 从目标创建空物体
bpy.ops.constraint.object_bake_offsets(type='OFFSET')

# 清除偏移
bpy.ops.constraint.object_bake_offsets(type='CLEAR')

# 设置目标偏移
bpy.ops.constraint.object_bake_offsets(type='OFFSET')

# 重置目标
bpy.ops.constraint.reset_target()

# 更新目标
bpy.ops.constraint.update_target()
```

## 实际应用示例

### 创建相机跟随系统

```python
def setup_camera_follow(target_obj, camera_obj):
    """设置相机跟随目标物体"""
    # 确保相机被选中
    bpy.ops.object.select_all(action='DESELECT')
    camera_obj.select_set(True)
    bpy.context.view_layer.objects.active = camera_obj
    
    # 添加跟踪约束
    constraint = camera_obj.constraints.new(type='TRACK_TO')
    constraint.target = target_obj
    constraint.track_axis = 'TRACK_NEGATIVE_Z'
    constraint.up_axis = 'UP_Y'
    
    # 添加位置约束（带偏移）
    pos_constraint = camera_obj.constraints.new(type='COPY_LOCATION')
    pos_constraint.target = target_obj
    pos_constraint.influence = 0.0
    
    return [constraint, pos_constraint]
```

### 创建父子关系链

```python
def create_rig_chain(objects):
    """创建物体链条的父子关系"""
    for i, obj in enumerate(objects):
        if i == 0:
            continue
        
        # 清除现有约束
        for constraint in obj.constraints:
            obj.constraints.remove(constraint)
        
        # 添加跟随约束
        follow = obj.constraints.new(type='COPY_TRANSFORMS')
        follow.target = objects[i-1]
        follow.influence = 1.0
        
        # 使用子物体模式
        follow.subtarget = ''
    
    return objects
```

### 创建行走循环动画

```python
def setup_walk_cycle(character_root, foot_r, foot_l):
    """设置角色行走循环"""
    # 右脚位置约束
    foot_r_constraint = foot_r.constraints.new(type='COPY_LOCATION')
    foot_r_constraint.target = character_root
    
    # 右脚旋转约束
    foot_r_rot = foot_r.constraints.new(type='COPY_ROTATION')
    foot_r_rot.target = character_root
    foot_r_rot.influence = 0.0
    
    # 左脚位置约束
    foot_l_constraint = foot_l.constraints.new(type='COPY_LOCATION')
    foot_l_constraint.target = character_root
    
    # 左脚旋转约束
    foot_l_rot = foot_l.constraints.new(type='COPY_ROTATION')
    foot_l_rot.target = character_root
    foot_l_rot.influence = 0.0
    
    return [foot_r_constraint, foot_r_rot, foot_l_constraint, foot_l_rot]
```

### 创建眼睛注视系统

```python
def setup_eye_tracking(eye_l, eye_r, target):
    """设置双眼注视目标"""
    for eye in [eye_l, eye_r]:
        # 清除现有约束
        eye.constraints.clear()
        
        # 添加轨道约束
        track = eye.constraints.new(type='TRACK_TO')
        track.target = target
        track.track_axis = 'TRACK_NEGATIVE_Z'
        track.up_axis = 'UP_Y'
        
        # 添加限制旋转约束
        limit = eye.constraints.new(type='LIMIT_ROTATION')
        limit.use_min_x = True
        limit.min_x = -0.5
        limit.use_max_x = True
        limit.max_x = 0.5
        limit.use_min_y = True
        limit.min_y = -0.5
        limit.use_max_y = True
        limit.max_y = 0.5
```

## 使用场景

- **角色绑定**：创建骨骼和物体间的运动关系
- **相机控制**：设置相机跟随和注视目标
- **动画系统**：建立复杂的运动链和约束网络
- **机械动画**：创建齿轮、连杆等机械运动
- **特效控制**：控制粒子发射器和力场的行为

## 注意事项

- 约束按顺序执行，后添加的约束覆盖前面的效果
- 目标物体必须在场景中存在
- 子物体（subtarget）只对骨骼约束有效
- 父子关系约束和变换约束会冲突
- 循环约束会导致无限递归


---
## bpy_ops_marker

# bpy.ops.marker - Timeline Marker Operations Module

Blender 时间线标记操作符，用于管理场景中的时间线标记。

## Overview

`bpy.ops.marker` 模块包含时间线标记的操作命令，支持标记的创建、删除、移动和选择。这个模块主要用于动画时间线的组织和管理。

## 核心功能

```python
import bpy

# 添加标记
bpy.ops.marker.add()

# 移动标记
bpy.ops.marker.move(frames=10)

# 删除标记
bpy.ops.marker.delete()

# 选择标记
bpy.ops.marker.select(action='SELECT')

# 重命名标记
bpy.ops.marker.rename(name="NewName")

# 绑定相机
bpy.ops.marker.camera_bind()

# 解除相机绑定
bpy.ops.marker.camera_bind_clear()
```

## 实际应用示例

### 标记管理器

```python
import bpy

class MarkerManager:
    """标记管理器"""
    
    def __init__(self):
        self.markers = {}
    
    def add_marker(self, name=None, frame=None, camera=None):
        """添加标记"""
        if frame is None:
            frame = bpy.context.scene.frame_current
        
        bpy.ops.marker.add()
        marker = bpy.context.scene.timeline_markers[-1]
        
        if name:
            marker.name = name
        
        marker.frame = frame
        
        if camera:
            marker.camera = camera
        
        self.markers[marker.name] = marker
        return marker
    
    def add_markers_from_list(self, marker_list):
        """从列表添加标记"""
        for item in marker_list:
            self.add_marker(
                name=item.get('name'),
                frame=item.get('frame', bpy.context.scene.frame_current),
                camera=item.get('camera')
            )
    
    def move_marker(self, marker_name, frame):
        """移动标记"""
        marker = self.markers.get(marker_name)
        if marker:
            marker.frame = frame
    
    def move_marker_by_frames(self, marker_name, frames):
        """按帧数移动标记"""
        marker = self.markers.get(marker_name)
        if marker:
            marker.frame += frames
    
    def delete_marker(self, marker_name):
        """删除标记"""
        marker = self.markers.get(marker_name)
        if marker:
            bpy.context.scene.timeline_markers.active = marker
            bpy.ops.marker.delete()
            del self.markers[marker_name]
    
    def delete_all_markers(self):
        """删除所有标记"""
        for name in list(self.markers.keys()):
            self.delete_marker(name)
    
    def rename_marker(self, marker_name, new_name):
        """重命名标记"""
        marker = self.markers.get(marker_name)
        if marker:
            old_name = marker.name
            marker.name = new_name
            self.markers[new_name] = self.markers.pop(old_name)
    
    def select_marker(self, marker_name, select=True):
        """选择标记"""
        marker = self.markers.get(marker_name)
        if marker:
            marker.select = select
    
    def select_all_markers(self):
        """全选标记"""
        for marker in bpy.context.scene.timeline_markers:
            marker.select = True
    
    def deselect_all_markers(self):
        """取消选择"""
        for marker in bpy.context.scene.timeline_markers:
            marker.select = False
    
    def get_marker_at_frame(self, frame):
        """获取指定帧的标记"""
        for marker in bpy.context.scene.timeline_markers:
            if marker.frame == frame:
                return marker
        return None
    
    def get_next_marker(self, frame=None):
        """获取下一个标记"""
        if frame is None:
            frame = bpy.context.scene.frame_current
        
        next_markers = [m for m in bpy.context.scene.timeline_markers 
                       if m.frame > frame]
        
        return min(next_markers, key=lambda m: m.frame) if next_markers else None
    
    def get_previous_marker(self, frame=None):
        """获取上一个标记"""
        if frame is None:
            frame = bpy.context.scene.frame_current
        
        prev_markers = [m for m in bpy.context.scene.timeline_markers 
                       if m.frame < frame]
        
        return max(prev_markers, key=lambda m: m.frame) if prev_markers else None
    
    def jump_to_marker(self, marker_name):
        """跳转到标记"""
        marker = self.markers.get(marker_name)
        if marker:
            bpy.context.scene.frame_set(marker.frame)
    
    def jump_to_next_marker(self):
        """跳转到下一个标记"""
        marker = self.get_next_marker()
        if marker:
            self.jump_to_marker(marker.name)
    
    def jump_to_previous_marker(self):
        """跳转到上一个标记"""
        marker = self.get_previous_marker()
        if marker:
            self.jump_to_marker(marker.name)
    
    def set_camera_for_marker(self, marker_name, camera):
        """为标记设置相机"""
        marker = self.markers.get(marker_name)
        if marker:
            marker.camera = camera
    
    def bind_camera_to_marker(self, marker_name):
        """绑定相机到标记"""
        marker = self.markers.get(marker_name)
        if marker:
            bpy.context.scene.timeline_markers.active = marker
            bpy.ops.marker.camera_bind()
    
    def clear_camera_binding(self, marker_name):
        """清除相机绑定"""
        marker = self.markers.get(marker_name)
        if marker:
            bpy.context.scene.timeline_markers.active = marker
            bpy.ops.marker.camera_bind_clear()
    
    def duplicate_marker(self, marker_name):
        """复制标记"""
        marker = self.markers.get(marker_name)
        if marker:
            return self.add_marker(
                name=f"{marker.name}_copy",
                frame=marker.frame + 1,
                camera=marker.camera
            )
    
    def get_marker_info(self, marker_name):
        """获取标记信息"""
        marker = self.markers.get(marker_name)
        if not marker:
            return None
        
        return {
            'name': marker.name,
            'frame': marker.frame,
            'camera': marker.camera.name if marker.camera else None,
            'select': marker.select
        }
    
    def list_markers(self):
        """列出所有标记"""
        return [m.name for m in bpy.context.scene.timeline_markers]
    
    def export_markers(self, filepath):
        """导出标记"""
        import json
        
        marker_data = []
        for marker in bpy.context.scene.timeline_markers:
            marker_data.append({
                'name': marker.name,
                'frame': marker.frame,
                'camera': marker.camera.name if marker.camera else None
            })
        
        with open(filepath, 'w', encoding='utf-8') as f:
            json.dump(marker_data, f, indent=2, ensure_ascii=False)
        
        return filepath
    
    def import_markers(self, filepath):
        """导入标记"""
        import json
        
        with open(filepath, 'r', encoding='utf-8') as f:
            marker_data = json.load(f)
        
        for item in marker_data:
            camera = bpy.data.objects.get(item['camera']) if item.get('camera') else None
            self.add_marker(
                name=item['name'],
                frame=item['frame'],
                camera=camera
            )
        
        return len(marker_data)


def setup_marker_workflow():
    """设置标记工作流程"""
    manager = MarkerManager()
    
    print("标记管理器已设置")
    print(f"当前标记: {manager.list_markers()}")
    
    return manager
```

### 动画场景管理器

```python
import bpy

class SceneAnimationManager:
    """场景动画管理器"""
    
    def __init__(self):
        self.scenes = {}
    
    def create_scene_for_marker(self, marker_name, scene_name=None):
        """为标记创建场景"""
        if scene_name is None:
            scene_name = f"{marker_name}_Scene"
        
        scene = bpy.data.scenes.new(name=scene_name)
        
        # 复制当前场景内容
        current_scene = bpy.context.scene
        
        for obj in current_scene.objects:
            if obj.users > 0:
                obj.users_collection[0].objects.link(obj)
        
        self.scenes[marker_name] = scene
        return scene
    
    def setup_scene_sequence(self, marker_list, scene_prefix="Scene"):
        """设置场景序列"""
        for i, marker_data in enumerate(marker_list):
            scene_name = f"{scene_prefix}_{i+1}"
            
            # 创建场景
            scene = bpy.data.scenes.new(name=scene_name)
            
            # 复制对象
            current_scene = bpy.context.scene
            for obj in current_scene.objects:
                if obj.name in marker_data.get('objects', []):
                    obj.users_collection[0].objects.link(obj)
            
            # 创建标记
            manager = MarkerManager()
            manager.add_marker(
                name=f"Scene_{i+1}",
                frame=marker_data.get('frame', i * 30)
            )
            
            self.scenes[scene_name] = scene
        
        return self.scenes
    
    def create_cut_points(self, frame_list):
        """创建剪辑点"""
        manager = MarkerManager()
        
        for i, frame in enumerate(frame_list):
            manager.add_marker(
                name=f"Cut_{i+1}",
                frame=frame
            )
        
        return len(frame_list)
    
    def create_chapter_markers(self, chapters):
        """创建章节标记"""
        manager = MarkerManager()
        
        for chapter in chapters:
            manager.add_marker(
                name=chapter['name'],
                frame=chapter['frame']
            )
        
        return len(chapters)
    
    def setup_camera_switches(self, camera_list):
        """设置相机切换"""
        manager = MarkerManager()
        
        for i, (frame, camera_name) in enumerate(camera_list):
            camera = bpy.data.objects.get(camera_name)
            if camera:
                manager.add_marker(
                    name=f"Camera_{i+1}",
                    frame=frame,
                    camera=camera
                )
        
        return len(camera_list)
    
    def animate_transitions(self, transition_list):
        """设置过渡动画"""
        for transition in transition_list:
            start_frame = transition['start_frame']
            end_frame = transition['end_frame']
            transition_type = transition.get('type', 'fade')
            
            # 创建标记
            manager = MarkerManager()
            manager.add_marker(
                name=f"{transition_type}_in",
                frame=start_frame
            )
            manager.add_marker(
                name=f"{transition_type}_out",
                frame=end_frame
            )
            
            # 创建过渡动画
            # 这里可以添加实际的过渡效果代码
    
    def create_keyframe_markers(self, object_name, keyframe_frames):
        """创建关键帧标记"""
        manager = MarkerManager()
        
        for i, frame in enumerate(keyframe_frames):
            manager.add_marker(
                name=f"{object_name}_Key_{i+1}",
                frame=frame
            )
        
        return len(keyframe_frames)
    
    def organize_animation_timeline(self, sections):
        """组织动画时间线"""
        manager = MarkerManager()
        
        for section in sections:
            manager.add_marker(
                name=section['name'],
                frame=section['start_frame']
            )
        
        return True
    
    def export_timeline_layout(self, filepath):
        """导出时间线布局"""
        import json
        
        layout = {
            'markers': [],
            'scenes': list(self.scenes.keys())
        }
        
        for marker in bpy.context.scene.timeline_markers:
            layout['markers'].append({
                'name': marker.name,
                'frame': marker.frame,
                'camera': marker.camera.name if marker.camera else None
            })
        
        with open(filepath, 'w', encoding='utf-8') as f:
            json.dump(layout, f, indent=2, ensure_ascii=False)
        
        return filepath


def setup_animation_workflow():
    """设置动画工作流程"""
    manager = SceneAnimationManager()
    
    print("场景动画管理器已设置")
    print("可用功能:")
    print("- create_cut_points: 创建剪辑点")
    print("- create_chapter_markers: 创建章节标记")
    print("- setup_camera_switches: 设置相机切换")
    print("- animate_transitions: 设置过渡动画")
    
    return manager
```

### 音频同步工具

```python
import bpy

class AudioSyncTool:
    """音频同步工具"""
    
    def __init__(self):
        self.audio_events = {}
    
    def add_audio_marker(self, name, frame, audio_path=None):
        """添加音频事件标记"""
        marker = MarkerManager().add_marker(
            name=name,
            frame=frame
        )
        
        self.audio_events[name] = {
            'frame': frame,
            'audio_path': audio_path
        }
        
        return marker
    
    def import_audio_events(self, audio_path, event_frames):
        """导入音频事件"""
        for i, frame in enumerate(event_frames):
            self.add_audio_marker(
                name=f"AudioEvent_{i+1}",
                frame=frame,
                audio_path=audio_path
            )
    
    def sync_to_audio_peaks(self, audio_path, peak_frames):
        """同步到音频峰值"""
        manager = MarkerManager()
        
        for i, frame in enumerate(peak_frames):
            manager.add_marker(
                name=f"Peak_{i+1}",
                frame=frame
            )
        
        return len(peak_frames)
    
    def create_audio_guide_track(self, audio_path, interval=30):
        """创建音频引导轨道"""
        manager = MarkerManager()
        
        # 获取音频长度
        sound = bpy.data.sounds.get(audio_path)
        if not sound:
            return 0
        
        total_frames = int(sound.length * bpy.context.scene.render.fps)
        
        # 每隔 interval 帧添加标记
        for frame in range(0, total_frames, interval):
            manager.add_marker(
                name=f"Guide_{frame}",
                frame=frame
            )
        
        return total_frames // interval
    
    def sync_animations_to_audio(self, object_name, animation_data, fps=30):
        """同步动画到音频"""
        manager = MarkerManager()
        
        for event in animation_data:
            manager.add_marker(
                name=f"{object_name}_{event['name']}",
                frame=int(event['time'] * fps)
            )
        
        return len(animation_data)
    
    def create_lip_sync_markers(self, phoneme_data, fps=24):
        """创建口型同步标记"""
        manager = MarkerManager()
        
        for phoneme in phoneme_data:
            start_frame = int(phoneme['start_time'] * fps)
            end_frame = int(phoneme['end_time'] * fps)
            
            manager.add_marker(
                name=f"LipSync_{phoneme['phoneme']}_{start_frame}",
                frame=start_frame
            )
        
        return len(phoneme_data)
    
    def create_beat_markers(self, bpm, start_frame=1, duration_bars=4,
                           beats_per_bar=4):
        """创建节拍标记"""
        manager = MarkerManager()
        
        fps = bpy.context.scene.render.fps
        seconds_per_beat = 60 / bpm
        frames_per_beat = int(seconds_per_beat * fps)
        frames_per_bar = frames_per_beat * beats_per_bar
        
        for bar in range(duration_bars):
            for beat in range(beats_per_bar):
                frame = start_frame + bar * frames_per_bar + beat * frames_per_beat
                
                name = f"Bar{bar+1}_Beat{beat+1}"
                manager.add_marker(name=name, frame=frame)
        
        return duration_bars * beats_per_bar
    
    def export_audio_sync_data(self, filepath):
        """导出音频同步数据"""
        import json
        
        sync_data = {
            'events': [],
            'total_duration_frames': bpy.context.scene.frame_end
        }
        
        for name, data in self.audio_events.items():
            sync_data['events'].append({
                'name': name,
                'frame': data['frame'],
                'audio_path': data['audio_path']
            })
        
        with open(filepath, 'w', encoding='utf-8') as f:
            json.dump(sync_data, f, indent=2, ensure_ascii=False)
        
        return filepath


def setup_audio_sync():
    """设置音频同步"""
    tool = AudioSyncTool()
    
    print("音频同步工具已设置")
    print("可用功能:")
    print("- create_beat_markers: 创建节拍标记")
    print("- create_lip_sync_markers: 创建口型同步")
    print("- sync_animations_to_audio: 同步动画到音频")
    
    return tool
```


---
## bpy_ops_palette

# bpy.ops.palette - Palette Operations Module

Blender 调色板操作符，用于管理和操作颜色调色板。

## Overview

`bpy.ops.palette` 模块包含调色板的操作命令，支持颜色的添加、删除、排序和管理。这个模块主要用于culpt 绘画和纹理绘画中的颜色管理。

## 核心功能

```python
import bpy

# 添加颜色
bpy.ops.palette.add()

# 移除颜色
bpy.ops.palette.remove()

# 排序颜色
bpy.ops.palette.sort()

# 收缩调色板
bpy.ops.palette.shrink()

# 扩展调色板
bpy.ops.palette.expand()
```

## 实际应用示例

### 调色板管理器

```python
import bpy
import colorsys

class PaletteManager:
    """调色板管理器"""
    
    def __init__(self):
        self.palettes = {}
    
    def get_active_palette(self):
        """获取当前调色板"""
        return bpy.context.scene.tool_settings.sculpt_palette
    
    def create_palette(self, name, colors=None):
        """创建调色板"""
        palette = bpy.data.palettes.new(name=name)
        
        if colors:
            for color in colors:
                self.add_color(palette, color)
        
        self.palettes[name] = palette
        return palette
    
    def add_color(self, palette, color, position=-1):
        """添加颜色"""
        color_slot = palette.colors.add()
        color_slot.color = color
        
        if position >= 0:
            # 移动到指定位置
            palette.colors.move(position, len(palette.colors) - 1)
        
        return color_slot
    
    def add_color_from_hex(self, palette, hex_color, position=-1):
        """从十六进制添加颜色"""
        hex_color = hex_color.lstrip('#')
        r = int(hex_color[0:2], 16) / 255.0
        g = int(hex_color[2:4], 16) / 255.0
        b = int(hex_color[4:6], 16) / 255.0
        
        return self.add_color(palette, (r, g, b, 1.0), position)
    
    def remove_color(self, palette, index):
        """移除颜色"""
        if index < len(palette.colors):
            palette.colors.remove(palette.colors[index])
    
    def remove_color_by_name(self, palette, name):
        """按名称移除颜色"""
        for i, color in enumerate(palette.colors):
            if color.name == name:
                self.remove_color(palette, i)
                return True
        return False
    
    def clear_palette(self, palette):
        """清空调色板"""
        while len(palette.colors) > 0:
            palette.colors.remove(palette.colors[0])
    
    def get_color(self, palette, index):
        """获取颜色"""
        if index < len(palette.colors):
            return palette.colors[index]
        return None
    
    def set_color(self, palette, index, color):
        """设置颜色"""
        if index < len(palette.colors):
            palette.colors[index].color = color
    
    def swap_colors(self, palette, index1, index2):
        """交换颜色"""
        if (index1 < len(palette.colors) and 
            index2 < len(palette.colors)):
            color1 = palette.colors[index1].color.copy()
            color2 = palette.colors[index2].color.copy()
            
            palette.colors[index1].color = color2
            palette.colors[index2].color = color1
    
    def duplicate_color(self, palette, index):
        """复制颜色"""
        color = self.get_color(palette, index)
        if color:
            return self.add_color(palette, color.color)
    
    def sort_by_hue(self, palette, reverse=False):
        """按色相排序"""
        colors = [(i, c.color) for i, c in enumerate(palette.colors)]
        
        def get_hue(c):
            if c[3] < 0.5:  # 跳过透明
                return -1
            return colorsys.rgb_to_hsv(c[0], c[1], c[2])[0]
        
        colors.sort(key=get_hue, reverse=reverse)
        
        # 重新排序
        for new_index, (old_index, color) in enumerate(colors):
            if new_index != old_index:
                palette.colors.move(old_index, new_index)
    
    def sort_by_saturation(self, palette, reverse=False):
        """按饱和度排序"""
        colors = [(i, c.color) for i, c in enumerate(palette.colors)]
        
        def get_saturation(c):
            if c[3] < 0.5:
                return -1
            return colorsys.rgb_to_hsv(c[0], c[1], c[2])[1]
        
        colors.sort(key=get_saturation, reverse=reverse)
        
        for new_index, (old_index, color) in enumerate(colors):
            if new_index != old_index:
                palette.colors.move(old_index, new_index)
    
    def sort_by_brightness(self, palette, reverse=False):
        """按亮度排序"""
        colors = [(i, c.color) for i, c in enumerate(palette.colors)]
        
        def get_brightness(c):
            if c[3] < 0.5:
                return -1
            return colorsys.rgb_to_hsv(c[0], c[1], c[2])[2]
        
        colors.sort(key=get_brightness, reverse=reverse)
        
        for new_index, (old_index, color) in enumerate(colors):
            if new_index != old_index:
                palette.colors.move(old_index, new_index)
    
    def create_gradient(self, palette, start_color, end_color, steps):
        """创建渐变"""
        colors = []
        for i in range(steps):
            t = i / (steps - 1)
            r = start_color[0] + (end_color[0] - start_color[0]) * t
            g = start_color[1] + (end_color[1] - start_color[1]) * t
            b = start_color[2] + (end_color[2] - start_color[2]) * t
            colors.append((r, g, b, 1.0))
        
        for color in colors:
            self.add_color(palette, color)
        
        return colors
    
    def create_rainbow(self, palette, steps=12):
        """创建彩虹色"""
        colors = []
        for i in range(steps):
            hue = i / steps
            rgb = colorsys.hsv_to_rgb(hue, 1.0, 1.0)
            colors.append((rgb[0], rgb[1], rgb[2], 1.0))
        
        for color in colors:
            self.add_color(palette, color)
        
        return colors
    
    def create_grayscale(self, palette, steps=10):
        """创建灰度"""
        colors = []
        for i in range(steps):
            gray = i / (steps - 1)
            colors.append((gray, gray, gray, 1.0))
        
        for color in colors:
            self.add_color(palette, color)
        
        return colors
    
    def export_palette(self, palette, filepath):
        """导出调色板"""
        with open(filepath, 'w') as f:
            for color in palette.colors:
                r, g, b, a = color.color
                f.write(f"{r:.4f} {g:.4f} {b:.4f} {a:.4f}\n")
        
        return filepath
    
    def import_palette(self, filepath, palette_name="Imported"):
        """导入调色板"""
        colors = []
        
        with open(filepath, 'r') as f:
            for line in f:
                parts = line.strip().split()
                if len(parts) >= 3:
                    colors.append((float(parts[0]), float(parts[1]), 
                                   float(parts[2]), 1.0))
        
        return self.create_palette(palette_name, colors)
    
    def duplicate_palette(self, palette, new_name):
        """复制调色板"""
        new_palette = self.create_palette(new_name)
        
        for color in palette.colors:
            self.add_color(new_palette, color.color)
        
        return new_palette
    
    def get_palette_info(self, palette):
        """获取调色板信息"""
        return {
            'name': palette.name,
            'color_count': len(palette.colors),
            'colors': [(c.color[0], c.color[1], c.color[2]) 
                      for c in palette.colors]
        }


def setup_palette_manager():
    """设置调色板管理器"""
    manager = PaletteManager()
    
    print("调色板管理器已设置")
    print("可用功能:")
    print("- create_palette: 创建调色板")
    print("- add_color: 添加颜色")
    print("- sort_by_hue: 按色相排序")
    print("- create_gradient: 创建渐变")
    
    return manager
```

### 颜色选择器工具

```python
import bpy
import colorsys

class ColorPickerTools:
    """颜色选择器工具"""
    
    def __init__(self):
        self.color_history = []
    
    def rgb_to_hsv(self, r, g, b):
        """RGB 转 HSV"""
        return colorsys.rgb_to_hsv(r, g, b)
    
    def hsv_to_rgb(self, h, s, v):
        """HSV 转 RGB"""
        return colorsys.hsv_to_rgb(h, s, v)
    
    def rgb_to_hex(self, r, g, b):
        """RGB 转十六进制"""
        return '#{:02x}{:02x}{:02x}'.format(
            int(r * 255), int(g * 255), int(b * 255)
        )
    
    def hex_to_rgb(self, hex_color):
        """十六进制转 RGB"""
        hex_color = hex_color.lstrip('#')
        return (
            int(hex_color[0:2], 16) / 255.0,
            int(hex_color[2:4], 16) / 255.0,
            int(hex_color[4:6], 16) / 255.0
        )
    
    def get_complementary(self, color):
        """获取互补色"""
        r, g, b = color[0], color[1], color[2]
        h, s, v = self.rgb_to_hsv(r, g, b)
        h = (h + 0.5) % 1.0
        rgb = self.hsv_to_rgb(h, s, v)
        return (rgb[0], rgb[1], rgb[2], color[3] if len(color) > 3 else 1.0)
    
    def get_analogous(self, color, angle=30):
        """获取类似色"""
        r, g, b = color[0], color[1], color[2]
        h, s, v = self.rgb_to_hsv(r, g, b)
        
        analogous = []
        for offset in [-angle, angle]:
            new_h = (h + offset / 360.0) % 1.0
            rgb = self.hsv_to_rgb(new_h, s, v)
            analogous.append((rgb[0], rgb[1], rgb[2], color[3] if len(color) > 3 else 1.0))
        
        return analogous
    
    def get_triadic(self, color):
        """获取三色"""
        r, g, b = color[0], color[1], color[2]
        h, s, v = self.rgb_to_hsv(r, g, b)
        
        triadic = []
        for offset in [1/3, 2/3]:
            new_h = (h + offset) % 1.0
            rgb = self.hsv_to_rgb(new_h, s, v)
            triadic.append((rgb[0], rgb[1], rgb[2], color[3] if len(color) > 3 else 1.0))
        
        return triadic
    
    def get_tetradic(self, color):
        """获取四色"""
        r, g, b = color[0], color[1], color[2]
        h, s, v = self.rgb_to_hsv(r, g, b)
        
        tetradic = []
        for offset in [1/4, 1/2, 3/4]:
            new_h = (h + offset) % 1.0
            rgb = self.hsv_to_rgb(new_h, s, v)
            tetradic.append((rgb[0], rgb[1], rgb[2], color[3] if len(color) > 3 else 1.0))
        
        return tetradic
    
    def adjust_brightness(self, color, factor):
        """调整亮度"""
        r, g, b = color[0], color[1], color[2]
        h, s, v = self.rgb_to_hsv(r, g, b)
        v = max(0, min(1, v * factor))
        rgb = self.hsv_to_rgb(h, s, v)
        return (rgb[0], rgb[1], rgb[2], color[3] if len(color) > 3 else 1.0)
    
    def adjust_saturation(self, color, factor):
        """调整饱和度"""
        r, g, b = color[0], color[1], color[2]
        h, s, v = self.rgb_to_hsv(r, g, b)
        s = max(0, min(1, s * factor))
        rgb = self.hsv_to_rgb(h, s, v)
        return (rgb[0], rgb[1], rgb[2], color[3] if len(color) > 3 else 1.0)
    
    def adjust_hue(self, color, offset):
        """调整色相"""
        r, g, b = color[0], color[1], color[2]
        h, s, v = self.rgb_to_hsv(r, g, b)
        h = (h + offset) % 1.0
        rgb = self.hsv_to_rgb(h, s, v)
        return (rgb[0], rgb[1], rgb[2], color[3] if len(color) > 3 else 1.0)
    
    def desaturate(self, color, factor=1.0):
        """去饱和"""
        r, g, b = color[0], color[1], color[2]
        gray = 0.299 * r + 0.587 * g + 0.114 * b
        return (
            r + (gray - r) * factor,
            g + (gray - g) * factor,
            b + (gray - b) * factor,
            color[3] if len(color) > 3 else 1.0
        )
    
    def create_color_scheme(self, base_color, scheme_type='complementary'):
        """创建配色方案"""
        if scheme_type == 'complementary':
            return [base_color, self.get_complementary(base_color)]
        elif scheme_type == 'analogous':
            return [base_color] + self.get_analogous(base_color)
        elif scheme_type == 'triadic':
            return [base_color] + self.get_triadic(base_color)
        elif scheme_type == 'tetradic':
            return [base_color] + self.get_tetradic(base_color)
        elif scheme_type == 'monochromatic':
            colors = []
            for i in range(5):
                factor = 0.5 + i * 0.125
                colors.append(self.adjust_brightness(base_color, factor))
            return colors
        
        return [base_color]
    
    def add_to_history(self, color):
        """添加到历史"""
        self.color_history.append(color)
        if len(self.color_history) > 10:
            self.color_history.pop(0)
    
    def get_history(self):
        """获取历史"""
        return self.color_history.copy()


def setup_color_tools():
    """设置颜色工具"""
    tools = ColorPickerTools()
    
    print("颜色选择器工具已设置")
    print("可用功能:")
    print("- get_complementary: 互补色")
    print("- get_analogous: 类似色")
    print("- get_triadic: 三色")
    print("- create_color_scheme: 创建配色方案")
    
    return tools
```


---
## bpy_ops_clipboard

# bpy.ops.clipboard - 剪贴板操作模块

Blender剪贴板操作符，用于复制和粘贴数据。

## 概述

`bpy.ops.clipboard` 模块提供用于在Blender中进行复制、粘贴和剪贴操作的功能。这些操作符可以跨不同对象和编辑器使用。

## 核心功能

### 复制操作

```python
import bpy

# 复制选中的对象
bpy.ops.object.copy()

# 复制选中的数据
bpy.ops.data_copy()

# 复制属性
bpy.ops.clipboard.copy(
    type='PROPERTIES'
)

# 复制变换
bpy.ops.transform.copy()

# 复制材质
bpy.ops.object.material_copy()

# 复制顶点组
bpy.ops.object.vertex_group_copy()

# 复制修改器
bpy.ops.object.modifier_copy()

# 复制约束
bpy.ops.object.constraint_copy()

# 复制动画数据
bpy.ops.action.copy()

# 复制驱动
bpy.ops.graph.copy_driver()
```

### 粘贴操作

```python
# 粘贴对象
bpy.ops.object.paste()

# 粘贴数据
bpy.ops.data_paste()

# 粘贴属性
bpy.ops.clipboard.paste(
    type='PROPERTIES'
)

# 粘贴变换
bpy.ops.transform.paste()

# 粘贴材质
bpy.ops.object.material_paste(
    replace=True
)

# 粘贴顶点组
bpy.ops.object.vertex_group_paste()

# 粘贴修改器
bpy.ops.object.modifier_paste()

# 粘贴约束
bpy.ops.object.constraint_paste()

# 粘贴动画数据
bpy.ops.action.paste()

# 粘贴驱动
bpy.ops.graph.paste_driver()
```

### 特殊粘贴操作

```python
# 粘贴到所有选中对象
bpy.ops.object.paste_to_all()

# 原地粘贴
bpy.ops.object.paste_in_place()

# 反向粘贴
bpy.ops.object.paste_in_reverse()

# 混合粘贴
bpy.ops.object.paste_mix(
    factor=0.5
)

# 偏移粘贴
bpy.ops.object.paste_offset(
    offset=(1.0, 0.0, 0.0)
)

# 镜像粘贴
bpy.ops.object.paste_mirror(
    axis='X'
)
```

### 剪贴操作

```python
# 剪贴选中对象
bpy.ops.object.cut(
    clipboard=True
)

# 剪贴并链接
bpy.ops.object.cut(
    clipboard=True,
    link=True
)

# 删除到剪贴板
bpy.ops.object.delete(
    use_global=False
)

# 全局删除到剪贴板
bpy.ops.object.delete(
    use_global=True
)
```

## 实用函数

```python
def copy_object_with_children(obj):
    """复制对象及其子对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.select_hierarchy(
        direction='CHILD',
        extend=True
    )
    bpy.ops.object.copy()

def duplicate_object(obj, linked=False):
    """复制对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.duplicate(
        linked=linked,
        mode='DUPLICATE'
    )
    return bpy.context.active_object

def batch_copy_objects(object_names, offset=(2.0, 0, 0)):
    """批量复制对象"""
    copies = []
    for obj_name in object_names:
        obj = bpy.data.objects.get(obj_name)
        if obj:
            copy = duplicate_object(obj)
            copy.location += offset
            copies.append(copy)
    return copies

def mirror_object_across_axis(obj, axis='X'):
    """沿轴镜像对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.duplicate()
    bpy.ops.transform.mirror(
        orient_type='GLOBAL',
        constraint_axis=(axis == 'X', axis == 'Y', axis == 'Z'),
        orient_matrix=((1, 0, 0), (0, 1, 0), (0, 0, 1))
    )

def offset_duplicate_and_chain(base_obj, count, offset):
    """链式偏移复制"""
    result = [base_obj]
    for i in range(count):
        prev = result[-1]
        bpy.ops.object.select_all(action='DESELECT')
        prev.select_set(True)
        bpy.active = prev
.context.view_layer.objects        bpy.ops.object.duplicate()
        new_obj = bpy.context.active_object
        new_obj.location += offset * (i + 1)
        result.append(new_obj)
    return result

def copy_material_to_objects(material_name, object_names):
    """将材质复制到多个对象"""
    material = bpy.data.materials.get(material_name)
    if material:
        for obj_name in object_names:
            obj = bpy.data.objects.get(obj_name)
            if obj and obj.data and hasattr(obj.data, 'materials'):
                if material not in obj.data.materials:
                    obj.data.materials.append(material)

def transfer_vertex_groups(source_obj, target_objs, replace=False):
    """传输顶点组"""
    for target_name in target_objs:
        target = bpy.data.objects.get(target_name)
        if target and target.type == source_obj.type:
            for vg in source_obj.vertex_groups:
                if vg.name not in [tvg.name for tvg in target.vertex_groups] or replace:
                    if replace:
                        target.vertex_groups.remove(target.vertex_groups.get(vg.name))
                    new_vg = target.vertex_groups.new(name=vg.name)
                    new_vg.lock_weight = vg.lock_weight
```

## 常见问题排查

### 复制粘贴无响应
```python
# 检查是否有选中对象
selected = [obj for obj in bpy.context.selected_objects]
print(f"选中对象数: {len(selected)}")

# 检查活动对象
print(f"活动对象: {bpy.context.active_object}")

# 检查对象是否在视图中
for obj in selected:
    print(f"对象 {obj.name}: 隐藏={obj.hide_viewport}, 可选={obj.hide_select}")

# 确保对象可见且可选
for obj in selected:
    obj.hide_viewport = False
    obj.hide_select = False
```

### 粘贴位置错误
```python
# 检查原点位置
obj = bpy.context.active_object
print(f"原点: {obj.location}")

# 检查世界原点
print(f"世界原点: {bpy.context.scene.cursor.location}")

# 设置正确的粘贴位置
bpy.context.scene.cursor.location = (0, 0, 0)
bpy.ops.object.paste_location()

# 检查吸附设置
print(f"吸附: {bpy.context.scene.tool_settings.use_snap}")
```

### 材质复制问题
```python
# 检查材质
obj = bpy.context.active_object
print(f"对象材质: {obj.data.materials if obj.data else 'N/A'}")

# 检查源材质
source_mat = bpy.context.scene.clipboard
if isinstance(source_mat, bpy.types.Material):
    print(f"源材质: {source_mat.name}")

# 重新分配材质
if obj.data:
    obj.data.materials.clear()
    obj.data.materials.append(source_mat)
```


---
## bpy_ops_import_anim

# bpy.ops.import_anim - Animation Import Module

动画导入模块，用于导入BVH动作捕捉数据。

## 概述

`bpy.ops.import_anim` 模块提供了Blender中导入BVH（BioVision Hierarchy）动作捕捉文件的功能。BVH是一种广泛使用的动作数据格式，包含骨骼层级和动画关键帧信息，常用于角色动画制作。

## 核心功能

### BVH文件导入

```python
import bpy

# 导入BVH文件
bpy.ops.import_anim.bvh(
    filepath='',
    filter_glob='*.bvh',
    target='ARMATURE',
    global_scale=1.0,
    frame_start=1,
    use_fps_scale=False,
    update_scene_fps=False,
    update_scene_duration=False,
    use_cyclic=False,
    rotate_mode='NATIVE',
    axis_forward='-Z',
    axis_up='Y'
)
```

## 参数说明

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| filepath | string | '' | BVH文件路径 |
| filter_glob | string | '*.bvh' | 文件过滤器 |
| target | string | 'ARMATURE' | 导入目标类型（ARMATURE, OBJECT） |
| global_scale | float | 1.0 | 全局缩放比例 |
| frame_start | int | 1 | 起始帧 |
| use_fps_scale | bool | False | 是否缩放帧率 |
| update_scene_fps | bool | False | 是否更新场景帧率 |
| update_scene_duration | bool | False | 是否更新场景持续时间 |
| use_cyclic | bool | False | 是否循环动画 |
| rotate_mode | string | 'NATIVE' | 旋转模式（NATIVE, QUATERNION, XYZ） |
| axis_forward | string | '-Z' | 向前轴向 |
| axis_up | string | 'Y' | 向上轴向 |

## 实用函数

```python
# 导入BVH文件并自动命名
def import_bvh_auto_name(filepath, target='ARMATURE'):
    # 获取文件名作为骨骼名称
    import os
    name = os.path.splitext(os.path.basename(filepath))[0]
    
    # 导入
    bpy.ops.import_anim.bvh(
        filepath=filepath,
        target=target,
        global_scale=1.0,
        frame_start=1
    )
    
    return name

# 批量导入多个BVH文件
def import_multiple_bvh(filepaths, target='ARMATURE'):
    for filepath in filepaths:
        import_bvh_auto_name(filepath, target)

# 导入并调整动画速度
def import_bvh_with_speed(filepath, speed_factor=1.0):
    bpy.ops.import_anim.bvh(
        filepath=filepath,
        target='ARMATURE',
        global_scale=1.0,
        use_fps_scale=True,
        frame_start=1
    )
    
    if speed_factor != 1.0:
        # 缩放动作数据
        # 注意：需要在导入后手动缩放关键帧

# 导入BVH并应用循环
def import_bvh_cyclic(filepath):
    bpy.ops.import_anim.bvh(
        filepath=filepath,
        target='ARMATURE',
        use_cyclic=True,
        global_scale=1.0
    )
```

## 常见问题排查

**问题：导入后骨骼方向错误**

- 调整`axis_forward`和`axis_up`参数
- 尝试不同的旋转模式（'NATIVE', 'QUATERNION', 'XYZ'）
- 在导入后使用`bpy.ops.object.transform_apply()`应用变换

**问题：动画速度不正确**

- 设置`use_fps_scale=True`来缩放帧率
- 调整`global_scale`参数
- 检查BVH文件中的原始帧率设置

**问题：骨骼无法正确绑定**

- 确认`target`参数设置为'ARMATURE'
- 检查场景中是否已有所需的骨骼
- 尝试使用OBJECT目标类型

**问题：动画帧数不对**

- 检查`frame_start`参数
- 设置`update_scene_duration=True`
- 验证BVH文件的原始帧范围

**问题：文件无法识别**

- 确认文件扩展名为.bvh
- 检查文件编码是否为ASCII或UTF-8
- 验证BVH文件格式是否完整

**问题：导入后骨骼缩放异常**

- 调整`global_scale`参数
- 尝试不同的缩放值（如0.01或100）
- 在导入后应用缩放变换


---
## bpy_ops_export_anim

# bpy.ops.export_anim 模块

动画导出操作模块，用于将Blender动画导出为各种格式。

## 概述

bpy.ops.export_anim 模块包含用于将动画数据导出为外部格式的运算符。这些运算符支持导出骨骼动画、形状键动画和约束动画。

## 常用操作

### 基础导出

```python
import bpy

# 导出动画
bpy.ops.export_anim.animation_export(
    filepath='//animation.fbx',
    export_format='FBX'
)

# 导出选定动画
bpy.ops.export_anim.animation_export(
    filepath='//animation.fbx',
    use_selection=True
)

# 导出所有动画
bpy.ops.export_anim.animation_export(
    filepath='//animation.fbx',
    use_selection=False
)
```

### FBX导出

```python
# FBX格式导出
bpy.ops.export_anim.fbx_export(
    filepath='//animation.fbx',
    export_format='FBX',
    use_selection=False,
    export_animations=True,
    export_frame_range=True,
    export_nla_strips=True
)

# FBX骨骼导出
bpy.ops.export_anim.fbx_export(
    filepath='//character.fbx',
    export_format='FBX',
    export_armature=True,
    export_bone_animations=True
)

# FBX形状键导出
bpy.ops.export_anim.fbx_export(
    filepath='//morph.fbx',
    export_format='FBX',
    export_morph=True,
    export_morph_normal=True
)
```

### glTF导出

```python
# glTF格式导出
bpy.ops.export_anim.gltf_export(
    filepath='//animation.glb',
    export_format='GLB',
    use_selection=False,
    export_animations=True,
    export_lights=True,
    export_cameras=True
)

# glTF骨骼动画
bpy.ops.export_anim.gltf_export(
    filepath='//character.glb',
    export_format='GLB',
    export_animations=True,
    export_skins=True,
    export_morph=True
)

# glTF NLA导出
bpy.ops.export_anim.gltf_export(
    filepath='//nla.glb',
    export_format='GLB',
    export_animations=True,
    export_nla_strips=True
)
```

### ABC导出

```python
# Alembic格式导出
bpy.ops.export_anim.alembic_export(
    filepath='//simulation.abc',
    start=1,
    end=250,
    export_animations=True,
    export_frame_range=True
)

# ABC几何导出
bpy.ops.export_anim.alembic_export(
    filepath='//geometry.abc',
    start=1,
    end=100,
    export_animations=True,
    export_uv=True,
    export_normals=True
)
```

### OBJ导出

```python
# OBJ格式导出（带动画）
bpy.ops.export_anim.obj_export(
    filepath='//model.obj',
    export_animated=True,
    export_frame_start=1,
    export_frame_end=100
)
```

### 设置导出

```python
# 导出范围
bpy.ops.export_anim.animation_export(
    filepath='//animation.fbx',
    frame_start=1,
    frame_end=250
)

# 导出采样
bpy.ops.export_anim.animation_export(
    filepath='//animation.fbx',
    export_frame_step=1
)

# 导出烘焙
bpy.ops.export_anim.animation_export(
    filepath='//animation.fbx',
    export_bake_all_animations=True,
    export_bake_use_nla_strips=True
)
```

## 实际应用示例

### 导出角色动画到FBX

```python
def export_character_animation(character, output_path, start_frame=1, end_frame=250):
    """导出角色动画到FBX"""
    # 选中角色
    bpy.ops.object.select_all(action='DESELECT')
    character.select_set(True)
    bpy.context.view_layer.objects.active = character
    
    # 导出动画
    bpy.ops.export_anim.fbx_export(
        filepath=output_path,
        export_format='FBX',
        use_selection=True,
        export_animations=True,
        export_frame_range=True,
        export_bake_all_animations=True,
        export_armature=True,
        export_bone_animations=True,
        export_nla_strips=True,
        export_current_frame=start_frame
    )
    
    return output_path
```

### 导出glTF动画

```python
def export_gltf_animation(output_path, start_frame=1, end_frame=250):
    """导出glTF动画"""
    bpy.ops.export_anim.gltf_export(
        filepath=output_path,
        export_format='GLB',
        use_selection=False,
        export_animations=True,
        export_frame_range=True,
        export_nla_strips=True,
        export_current_frame=start_frame
    )
    
    return output_path
```

### 导出NLA动画

```python
def export_nla_animation(nla_track, output_path):
    """导出NLA轨道动画"""
    # 设置当前帧为动画开始
    bpy.context.scene.frame_set(nla_track.strips[0].frame_start)
    
    # 导出
    bpy.ops.export_anim.animation_export(
        filepath=output_path,
        export_format='FBX',
        use_selection=True,
        export_nla_strips=True
    )
    
    return output_path
```

### 批量导出动画

```python
def batch_export_animations(export_config):
    """批量导出多个动画"""
    import os
    
    results = []
    
    for config in export_config:
        character = config['character']
        output_dir = config['output_dir']
        animations = config['animations']
        
        for anim_name, anim_config in animations.items():
            output_path = os.path.join(output_dir, f'{anim_name}.fbx')
            
            # 设置帧范围
            bpy.context.scene.frame_start = anim_config['start']
            bpy.context.scene.frame_end = anim_config['end']
            
            # 选中角色
            bpy.ops.object.select_all(action='DESELECT')
            character.select_set(True)
            bpy.context.view_layer.objects.active = character
            
            # 导出
            bpy.ops.export_anim.fbx_export(
                filepath=output_path,
                export_format='FBX',
                use_selection=True,
                export_animations=True,
                export_frame_range=True
            )
            
            results.append({
                'animation': anim_name,
                'path': output_path,
                'frames': f"{anim_config['start']}-{anim_config['end']}"
            })
    
    return results
```

### 导出形状键动画

```python
def export_shape_key_animation(mesh, output_path, start_frame=1, end_frame=100):
    """导出形状键动画"""
    # 选中网格
    bpy.ops.object.select_all(action='DESELECT')
    mesh.select_set(True)
    bpy.context.view_layer.objects.active = mesh
    
    # 设置帧范围
    bpy.context.scene.frame_start = start_frame
    bpy.context.scene.frame_end = end_frame
    
    # 导出
    bpy.ops.export_anim.fbx_export(
        filepath=output_path,
        export_format='FBX',
        use_selection=True,
        export_morph=True,
        export_morph_normal=True
    )
    
    return output_path
```

### 导出Alembic模拟

```python
def export_alembic_simulation(domain_obj, output_path, start_frame=1, end_frame=250):
    """导出Alembic模拟"""
    # 选中域对象
    bpy.ops.object.select_all(action='DESELECT')
    domain_obj.select_set(True)
    bpy.context.view_layer.objects.active = domain_obj
    
    # 导出
    bpy.ops.export_anim.alembic_export(
        filepath=output_path,
        start=start_frame,
        end=end_frame,
        export_animations=True,
        export_frame_range=True
    )
    
    return output_path
```

### 导出程序化动画

```python
def export_procedural_animation(obj, output_path, frames=250):
    """导出程序化动画"""
    # 确保对象已选中
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 设置帧范围
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = frames
    
    # 烘焙动画
    bpy.ops.nla.bake(
        frame_start=1,
        frame_end=frames,
        visual_keying=True,
        clear_constraints=True
    )
    
    # 导出
    bpy.ops.export_anim.animation_export(
        filepath=output_path,
        export_format='GLB',
        use_selection=True,
        export_bake_all_animations=True
    )
    
    return output_path
```

## 使用场景

- **游戏开发**：导出角色和动画到游戏引擎
- **Web3D**：导出模型和动画到Web平台
- **后期合成**：导出分层渲染和模拟
- **跨平台共享**：在不同软件间传递动画
- **存档备份**：保存动画项目
- **渲染农场**：在外部渲染

## 注意事项

- 不同格式支持不同功能
- FBX适合大多数游戏引擎
- glTF适合Web和实时应用
- Alembic适合特效和模拟
- 烘焙动画确保兼容性
- 帧范围影响导出大小


---
## bpy_ops_export_animation

# bpy.ops.export_animation - 动画导出操作模块

Blender动画导出操作符，用于将动画数据导出为各种动画文件格式。

## 概述

`bpy.ops.export_animation` 模块提供用于将Blender中的动画数据导出为外部文件格式的功能，包括FBX、ABC、USD等。

## 核心功能

### FBX导出

```python
import bpy

# 导出为FBX
bpy.ops.export_animation.fbx(
    filepath='//animation.fbx',
    export_selected=True,
    apply_scale_settings='FBX_SCALE_ALL',
    apply_unit_scale=True,
    axis_forward='-Z',
    axis_up='Y',
    bake_space_transform=False,
    use_selection=True,
    use_visible=True,
    use_active_collection=False,
    export_animation=True,
    export_nla_strips=True,
    export_lights=True,
    export_cameras=True,
    export_mesh_modifiers=True,
    export_armature=True,
    export_skins=True,
    export_all_influences=False,
    deform_bones_only=False,
    sampling_rate=0,
    export_bake=False,
    export_anim=True,
    export_anim_action='ACTIVE',
    export_anim_action_all=True,
    export_anim_use_nla=True,
    export_anim_use_all_bones=True,
    export_anim_optimize=True,
    export_anim_sampling_rate=1.0,
    export_anim_opacity=True
)

# 导出选中对象动画
bpy.ops.export_animation.fbx(
    filepath='//selected_anim.fbx',
    export_selected=True,
    use_selection=True
)

# 导出所有动画
bpy.ops.export_animation.fbx(
    filepath='//all_anim.fbx',
    export_selected=False
)

# 导出骨骼动画
bpy.ops.export_animation.fbx(
    filepath='//armature_anim.fbx',
    export_armature=True,
    export_skins=True
)

# 导出带NLA的动画
bpy.ops.export_animation.fbx(
    filepath='//nla_anim.fbx',
    export_animation=True,
    export_nla_strips=True
)
```

### Alembic导出

```python
# 导出为Alembic
bpy.ops.export_animation.alembic(
    filepath='//animation.abc',
    export_selected=True,
    apply_subdiv=False,
    export_visible=True,
    export_hair=True,
    export_particles=True,
    export_deforum=False,
    export_uv=True,
    export_normals=True,
    export_vcol=False,
    export_material_names=False,
    export_vertex_groups=False,
    export_triangulated=False,
    export_yup=True,
    renderable_only=False,
    visible_only=False,
    use_selection=True,
    end_frame=250,
    start_frame=1,
    stride=1,
    shutter_open=0.0,
    shutter_close=1.0,
    compression='ogawa'
)

# 导出所有帧
bpy.ops.export_animation.alembic(
    filepath='//full_anim.abc',
    start_frame=bpy.context.scene.frame_start,
    end_frame=bpy.context.scene.frame_end
)

# 导出粒子系统
bpy.ops.export_animation.alembic(
    filepath='//particles.abc',
    export_particles=True
)

# 导出毛发
bpy.ops.export_animation.alembic(
    filepath='//hair.abc',
    export_hair=True
)

# 高压缩导出
bpy.ops.export_animation.alembic(
    filepath='//compressed.abc',
    compression='hdf5'
)
```

### USD导出

```python
# 导出为USD
bpy.ops.export_animation.usd(
    filepath='//animation.usd',
    export_selected=True,
    export_visible=True,
    export_cameras=True,
    export_lights=True,
    export_materials=True,
    export_modifiers=True,
    export_type='MESH',
    export_pbr_material=True,
    export_textures=True,
    export_frame_range=True,
    export_frame_step=1,
    export_uv=True,
    export_normals=True,
    export_compression=False,
    export_hair=True,
    export_particles=True
)

# 导出带材质的USD
bpy.ops.export_animation.usd(
    filepath='//with_materials.usd',
    export_materials=True,
    export_pbr_material=True
)

# 导出特定帧范围
bpy.ops.export_animation.usd(
    filepath='//frame_range.usd',
    export_frame_range=True,
    start_frame=1,
    end_frame=100
)

# 导出为USDZ
bpy.ops.export_animation.usd(
    filepath='//animation.usdz',
    export_compression=True
)
```

### GLTF导出（动画相关）

```python
# 导出为GLTF（包含动画）
bpy.ops.export_animation.gltf(
    filepath='//animation.glb',
    export_selected=True,
    export_format='GLB',
    export_cameras=True,
    export_lights=True,
    export_animations=True,
    export_frame_range=True,
    export_frame_step=1,
    export_skins=True,
    export_all_influences=False,
    export_morph=True,
    export_yup=True,
    export_apply=True
)

# 导出动画为GLTF
bpy.ops.export_animation.gltf(
    filepath='//anim_only.glb',
    export_format='GLB',
    export_cameras=False,
    export_lights=False,
    export_animations=True
)

# 导出为GLTF动画片段
bpy.ops.export_animation.gltf(
    filepath='//animation.gltf',
    export_format='GLTF_SEPARATE',
    export_animations=True
)
```

### 动画烘焙导出

```python
# 烘焙所有动画
bpy.ops.export_animation.bake(
    start_frame=1,
    end_frame=250,
    step=1,
    apply_scale=True,
    apply_transforms=True,
    clear_constraints=True,
    clear_parents=False,
    export_global_objects=True,
    export_selected_objects=True
)

# 烘焙选中的骨骼动画
bpy.ops.export_animation.bake(
    object_type='ARMATURE',
    export_selected=True,
    bake_anim=True,
    bake_anim_action_all=True
)

# 烘焙变形目标动画
bpy.ops.export_animation.bake(
    object_type='MESH',
    bake_anim=True,
    bake_morph=True
)
```

## 实用函数

```python
def export_animation_fbx(filepath, selected_only=True):
    """导出动画为FBX格式"""
    bpy.ops.export_animation.fbx(
        filepath=filepath,
        export_selected=selected_only,
        export_anim=True,
        export_anim_action='ACTIVE'
    )

def export_animation_alembic(filepath, start_frame=1, end_frame=250):
    """导出动画为Alembic格式"""
    bpy.ops.export_animation.alembic(
        filepath=filepath,
        start_frame=start_frame,
        end_frame=end_frame,
        export_anim=True
    )

def export_animation_usd(filepath, start_frame=1, end_frame=250):
    """导出动画为USD格式"""
    bpy.ops.export_animation.usd(
        filepath=filepath,
        export_frame_range=True,
        start_frame=start_frame,
        end_frame=end_frame,
        export_animations=True
    )

def batch_export_animations(output_dir):
    """批量导出多个动画"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    # 导出FBX
    export_animation_fbx(os.path.join(output_dir, 'animation.fbx'))
    
    # 导出Alembic
    export_animation_alembic(os.path.join(output_dir, 'animation.abc'))
    
    # 导出USD
    export_animation_usd(os.path.join(output_dir, 'animation.usd'))

def export_selected_armature_anim(filepath):
    """导出选中的骨骼动画"""
    bpy.ops.export_animation.fbx(
        filepath=filepath,
        export_selected=True,
        export_armature=True,
        export_skins=True,
        export_animation=True,
        export_anim_action='ACTIVE'
    )

def export_all_actions(filepath):
    """导出所有动作"""
    bpy.ops.export_animation.fbx(
        filepath=filepath,
        export_selected=False,
        export_anim=True,
        export_anim_action='ALL'
    )
```

## 常见问题排查

###
```python
 动画丢失# 检查对象动画
obj = bpy.context.active_object
print(f"对象类型: {obj.type}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")
    if obj.animation_data.action:
        print(f"帧范围: {obj.animation_data.action.frame_range}")

# 检查NLA轨道
for nla_track in obj.animation_data.nla_tracks:
    for strip in nla_track.strips:
        print(f"NLA轨道: {nla_track.name}, 片段: {strip.name}")

# 检查约束
for constraint in obj.constraints:
    print(f"约束类型: {constraint.type}")

# 确保动画已启用导出
bpy.ops.export_animation.fbx(
    filepath='//test.fbx',
    export_anim=True
)
```

### 骨骼动画问题
```python
# 检查骨骼
armature = bpy.context.active_object
if armature.type == 'ARMATURE':
    print(f"骨骼数: {len(armature.data.bones)}")
    
    # 检查骨骼变换
    for bone in armature.data.bones:
        print(f"骨骼: {bone.name}, 位置: {bone.head_local}")

# 检查权重
mesh = [obj for obj in bpy.context.selected_objects if obj.type == 'MESH'][0]
if mesh.vertex_groups:
    print(f"顶点组数: {len(mesh.vertex_groups)}")

# 导出时包含所有骨骼
bpy.ops.export_animation.fbx(
    filepath='//armature.fbx',
    export_armature=True,
    export_skins=True,
    export_anim_use_all_bones=True
)
```

### 烘焙问题
```python
# 检查帧范围
print(f"开始帧: {bpy.context.scene.frame_start}")
print(f"结束帧: {bpy.context.scene.frame_end}")

# 检查帧率
print(f"帧率: {bpy.context.scene.render.fps}")

# 设置正确的帧范围
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 250

# 烘焙前应用所有修改
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.visual_transform_apply()

# 清除约束后烘焙
bpy.ops.export_animation.bake(
    export_anim=True,
    bake_anim=True,
    clear_constraints=True
)
```

### 导出格式不支持
```python
# 检查可用的导出操作
print("可用的导出操作:")
for op in dir(bpy.ops.export_animation):
    if not op.startswith('_'):
        print(f"  - {op}")

# 检查导出选项
bpy.ops.export_animation.fbx('//test.fbx', export_selected=True)
print("FBX导出可用")

# 尝试Alembic
try:
    bpy.ops.export_animation.alembic('//test.abc', export_selected=True)
    print("Alembic导出可用")
except:
    print("Alembic导出不可用")
```


