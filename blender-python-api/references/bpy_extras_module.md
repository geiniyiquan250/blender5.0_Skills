# bpy_extras_module

---
## bpy_extras

# bpy_extras Module Reference

bpy_extras 模块提供 Blender Python API 的扩展工具函数，包括节点、对象、网格、图像和资源的实用工具。

## bpy_extras 概述

bpy_extras 包含多个子模块，每个子模块提供特定领域的实用函数：
- node_utils: 节点工具
- object_utils: 对象工具
- mesh_utils: 网格工具
- image_utils: 图像工具
- asset_utils: 资源工具
- anim_utils: 动画工具
- view3d_utils: 3D 视图工具

### 何时使用 bpy_extras

- 查找节点输入输出
- 坐标转换
- 网格分析
- 图像处理
- 资源管理

## node_utils - 节点工具

### find_node_input

查找节点的输入插槽。

```python
from bpy_extras import node_utils

# 查找输入插槽
input_socket = node_utils.find_node_input(node, identifier)

# 参数
# node: 目标节点
# identifier: 插槽标识符（如 'Surface', 'Color'）

# 返回值
# NodeSocket: 找到的输入插槽，未找到返回 None
```

### find_node_output

查找节点的输出插槽。

```python
from bpy_extras import node_utils

# 查找输出插槽
output_socket = node_utils.find_node_output(node, identifier)

# 参数
# node: 目标节点
# identifier: 插槽标识符

# 返回值
# NodeSocket: 找到的输出插槽
```

### socket_id_to_index

将插槽 ID 转换为索引。

```python
from bpy_extras import node_utils

# 转换为索引
index = node_utils.socket_id_to_index(node, identifier, is_input)

# 参数
# node: 目标节点
# identifier: 插槽标识符
# is_input: 是否为输入插槽

# 返回值
# int: 插槽索引
```

### socket_index_to_id

将插槽索引转换为 ID。

```python
from bpy_extras import node_utils

# 转换为 ID
identifier = node_utils.socket_index_to_id(node, index, is_input)

# 参数
# node: 目标节点
# index: 插槽索引
# is_input: 是否为输入插槽

# 返回值
# str: 插槽标识符
```

## object_utils - 对象工具

### object_data_add

将数据添加到对象。

```python
from bpy_extras import object_utils

# 添加网格数据
mesh = bpy.data.meshes.new("MyMesh")
object_utils.object_data_add(bpy.context, mesh)

# 添加曲线数据
curve = bpy.data.curves.new("MyCurve", type='CURVE')
object_utils.object_data_add(bpy.context, curve)
```

### object_data_link

将数据链接到对象。

```python
from bpy_extras import object_utils

# 链接数据
object_utils.object_data_link(data, scene, collection, object_name)
```

### add_object_align_init

初始化对象对齐。

```python
from bpy_extras import object_utils

# 初始化对齐
init_matrix = object_utils.add_object_align_init(
    bpy.context,
    operator,
    align_type='WORLD'
)

# 参数
# context: 上下文
# operator: 操作符
# align_type: 对齐类型 ('WORLD', 'VIEW', 'CURSOR')
```

### world_to_camera_view

将世界坐标转换为相机视图坐标。

```python
from bpy_extras import object_utils

# 转换坐标
x, y = object_utils.world_to_camera_view(
    bpy.context,
    scene,
    camera_obj,
    world_co
)

# 参数
# context: 上下文
# scene: 场景
# camera: 相机对象
# world_co: 世界坐标 (Vector)

# 返回值
# x, y: 视图坐标 (0-1 范围)
```

### camera_view_to_world

将相机视图坐标转换为世界坐标。

```python
from bpy_extras import object_utils

# 转换坐标
world_co, direction = object_utils.camera_view_to_world(
    scene,
    camera_obj,
    x,
    y,
    distance
)

# 参数
# scene: 场景
# camera: 相机对象
# x, y: 视图坐标 (0-1 范围)
# distance: 距离

# 返回值
# world_co: 世界坐标
# direction: 方向向量
```

## mesh_utils - 网格工具

### mesh_linked_uv_islands

获取链接的 UV 岛屿。

```python
from bpy_extras import mesh_utils

# 获取 UV 岛屿
uv_islands = mesh_utils.mesh_linked_uv_islands(mesh)

# 参数
# mesh: 网格对象

# 返回值
# list: UV 岛屿列表，每个岛屿是顶点索引列表
```

### mesh_linked_uv_islands_to_mesh

将 UV 岛屿转换为独立网格。

```python
from bpy_extras import mesh_utils

# 转换为网格
island_meshes = mesh_utils.mesh_linked_uv_islands_to_mesh(mesh)

# 参数
# mesh: 源网格

# 返回值
# list: 独立网格列表
```

## image_utils - 图像工具

### load_image

加载图像文件。

```python
from bpy_extras import image_utils

# 加载图像
image = image_utils.load_image(image_path)

# 参数
# image_path: 图像文件路径

# 返回值
# Image: 加载的图像对象
```

### load_image_sequence

加载图像序列。

```python
from bpy_extras import image_utils

# 加载图像序列
images = image_utils.load_image_sequence(
    base_path,
    base_name,
    start_frame,
    end_frame,
    extension
)

# 参数
# base_path: 基础路径
# base_name: 基础名称
# start_frame: 起始帧
# end_frame: 结束帧
# extension: 文件扩展名

# 返回值
# list: 图像对象列表
```

### new_image

创建新图像。

```python
from bpy_extras import image_utils

# 创建图像
image = image_utils.new_image(
    name,
    width,
    height,
    alpha=False,
    float_buffer=False,
    stereo3d=False
)

# 参数
# name: 图像名称
# width: 宽度
# height: 高度
# alpha: 是否包含 Alpha 通道
# float_buffer: 是否使用浮点缓冲区
# stereo3d: 是否为立体图像

# 返回值
# Image: 创建的图像对象
```

## asset_utils - 资源工具

### SpaceAssetInfo

资源信息空间。

```python
from bpy_extras import asset_utils

# 获取资源信息
asset_info = asset_utils.SpaceAssetInfo

# 属性
# active_index: 活动资源索引
# catalog_id: 目录 ID
# filter_type: 筛选类型
# sort_method: 排序方法
```

### activate_asset_by_id

按 ID 激活资源。

```python
from bpy_extras import asset_utils

# 激活资源
asset_utils.activate_asset_by_id(
    asset_identifier,
    library_id=None,
    deferred=False
)

# 参数
# asset_identifier: 资源标识符
# library_id: 库 ID
# deferred: 是否延迟激活
```

### asset_mark

标记资源。

```python
from bpy_extras import asset_utils

# 标记资源
asset_utils.asset_mark(asset_identifier, library_id=None)
```

### asset_unmark

取消标记资源。

```python
from bpy_extras import asset_utils

# 取消标记
asset_utils.asset_unmark(asset_identifier, library_id=None)
```

## anim_utils - 动画工具

### get_fcurve_channels

获取 F-Curve 通道。

```python
from bpy_extras import anim_utils

# 获取通道
channels = anim_utils.get_fcurve_channels(action, data_path)

# 参数
# action: 动作
# data_path: 数据路径

# 返回值
# list: F-Curve 通道列表
```

### get_action_groups

获取动作组。

```python
from bpy_extras import anim_utils

# 获取组
groups = anim_utils.get_action_groups(action)

# 参数
# action: 动作

# 返回值
# list: 动作组列表
```

## view3d_utils - 3D 视图工具

### location_3d_to_region_2d

将 3D 坐标转换为区域 2D 坐标。

```python
from bpy_extras import view3d_utils

# 转换坐标
region_co = view3d_utils.location_3d_to_region_2d(
    region,
    region_3d,
    coord
)

# 参数
# region: 区域
# region_3d: 3D 区域
# coord: 3D 坐标

# 返回值
# Vector: 2D 区域坐标
```

### location_3d_to_region_2d_with_depth

将 3D 坐标转换为区域 2D 坐标（带深度）。

```python
from bpy_extras import view3d_utils

# 转换坐标
region_co, depth = view3d_utils.location_3d_to_region_2d_with_depth(
    region,
    region_3d,
    coord
)

# 返回值
# region_co: 2D 区域坐标
# depth: 深度值
```

### region_2d_to_location_3d

将区域 2D 坐标转换为 3D 坐标。

```python
from bpy_extras import view3d_utils

# 转换坐标
world_co = view3d_utils.region_2d_to_location_3d(
    region,
    region_3d,
    coord,
    depth_location
)

# 参数
# region: 区域
# region_3d: 3D 区域
# coord: 2D 坐标
# depth_location: 深度位置

# 返回值
# Vector: 3D 世界坐标
```

### region_2d_to_origin_3d

将区域 2D 坐标转换为 3D 原点。

```python
from bpy_extras import view3d_utils

# 转换原点
origin, direction = view3d_utils.region_2d_to_origin_3d(
    region,
    region_3d,
    coord
)

# 返回值
# origin: 3D 原点
# direction: 方向向量
```

### region_2d_to_vector_3d

将区域 2D 向量转换为 3D 向量。

```python
from bpy_extras import view3d_utils

# 转换向量
vector_3d = view3d_utils.region_2d_to_vector_3d(
    region,
    region_3d,
    coord_a,
    coord_b
)

# 参数
# coord_a: 起始 2D 坐标
# coord_b: 结束 2D 坐标

# 返回值
# Vector: 3D 向量
```

## 完整示例

### 查找节点连接

```python
import bpy
from bpy_extras import node_utils

def find_bsdf_input(mat):
    """查找材质的 BSDF 输入"""
    if not mat.use_nodes:
        return None
    
    node_tree = mat.node_tree
    output = node_tree.nodes.get("Material Output")
    
    if output:
        return node_utils.find_node_input(output, "Surface")
    
    return None

# 使用示例
mat = bpy.data.materials.get("MyMaterial")
if mat:
    input_socket = find_bsdf_input(mat)
    if input_socket:
        print(f"Found input: {input_socket.name}")
```

### 世界坐标转相机视图

```python
import bpy
from bpy_extras import object_utils

def get_object_screen_position(obj, camera):
    """获取对象在相机视图中的屏幕位置"""
    scene = bpy.context.scene
    
    # 获取对象中心的世界坐标
    world_co = obj.location
    
    # 转换为视图坐标
    x, y = object_utils.world_to_camera_view(
        bpy.context,
        scene,
        camera,
        world_co
    )
    
    # 获取视口尺寸
    viewport = bpy.context.region
    screen_x = int(x * viewport.width)
    screen_y = int(y * viewport.height)
    
    return screen_x, screen_y

# 使用示例
camera = bpy.context.scene.camera
obj = bpy.context.active_object
if camera and obj:
    pos = get_object_screen_position(obj, camera)
    print(f"Screen position: {pos}")
```

### UV 岛屿分析

```python
import bpy
from bpy_extras import mesh_utils

def analyze_uv_islands(mesh):
    """分析网格的 UV 岛屿"""
    if not mesh.uv_layers:
        return []
    
    uv_layer = mesh.uv_layers.active
    
    # 获取 UV 岛屿
    islands = mesh_utils.mesh_linked_uv_islands(mesh)
    
    result = []
    for i, island in enumerate(islands):
        # 计算岛屿面积
        area = 0.0
        for loop_idx in island:
            uv = uv_layer.data[loop_idx].uv
            area += uv.x  # 简化计算
        
        result.append({
            'index': i,
            'vertex_count': len(island),
            'area': area
        })
    
    return result

# 使用示例
obj = bpy.context.active_object
if obj and obj.type == 'MESH':
    islands = analyze_uv_islands(obj.data)
    for island in islands:
        print(f"Island {island['index']}: {island['vertex_count']} vertices")
```

### 射线投射

```python
import bpy
from bpy_extras import view3d_utils

def raycast_from_mouse(mouse_x, mouse_y):
    """从鼠标位置发射射线"""
    region = bpy.context.region
    region_3d = bpy.context.space_data.region_3d
    
    # 坐标归一化
    coord = (mouse_x / region.width, mouse_y / region.height)
    
    # 获取射线
    origin, direction = view3d_utils.region_2d_to_origin_3d(
        region, region_3d, coord
    )
    
    # 射线检测
    result, location, normal, index, object_, matrix = bpy.context.scene.ray_cast(
        origin, direction
    )
    
    if result:
        return {
            'location': location,
            'normal': normal,
            'object': object_,
            'hit': True
        }
    
    return {'hit': False}

# 使用示例
# mouse_x, mouse_y = 400, 300
# hit = raycast_from_mouse(mouse_x, mouse_y)
```

### 加载图像序列

```python
import bpy
from bpy_extras import image_utils

def load_image_sequence_opengl():
    """加载 OpenGL 渲染的图像序列"""
    base_path = "//render/output_"
    base_name = "frame_"
    start_frame = 1
    end_frame = 100
    extension = ".png"
    
    images = image_utils.load_image_sequence(
        base_path,
        base_name,
        start_frame,
        end_frame,
        extension
    )
    
    return images

# 创建图像序列编辑器条
def create_image_strip(images):
    seq_editor = bpy.context.scene.sequence_editor
    
    for i, image in enumerate(images):
        strip = seq_editor.sequences.new_image(
            name=f"Frame {i + 1}",
            filepath=image.filepath,
            channel=1,
            frame_start=i + 1
        )
    
    return True
```

## 最佳实践

1. **错误处理**：工具函数可能返回 None，检查返回值
2. **上下文要求**：某些函数需要有效的上下文
3. **坐标空间**：注意区分世界坐标、视图坐标和屏幕坐标
4. **性能优化**：大量操作时使用批处理
5. **兼容性**：检查 Blender 版本兼容性


---
## bpy_extras_anim_utils

# bpy_extras.anim_utils - 动画工具模块

bpy_extras.anim_utils模块提供了Blender动画相关的实用工具函数，用于简化动画数据的创建和操作。

## 概述

动画工具模块包含用于创建动画关键帧、驱动路径、权重渐变等动画相关操作的函数。这些函数封装了常见的动画任务，让动画开发更加便捷。

## 核心功能

### 关键帧操作

```python
import bpy
from bpy_extras.anim_utils import (
    animation_step,
    make_weighted_deform_path
)

def insert_keyframe(obj, data_path, frame, value):
    """在指定帧插入关键帧"""
    obj.keyframe_insert(data_path=data_path, frame=frame)
    if data_path in obj:
        obj[data_path] = value

def set_keyframe_at_frame(obj, path, frame, value):
    """在指定帧设置关键帧值"""
    obj.path_from_id(path)
    obj[path] = value
    obj.keyframe_insert(data_path=path, frame=frame)

def batch_insert_keyframes(obj, data_path, keyframes):
    """批量插入关键帧"""
    for frame, value in keyframes:
        obj[data_path] = value
        obj.keyframe_insert(data_path=data_path, frame=frame)

def create_keyframe_anim(obj, property_name, values, frames):
    """创建关键帧动画"""
    if len(values) != len(frames):
        return
    for value, frame in zip(values, frames):
        obj[property_name] = value
        obj.keyframe_insert(data_path=property_name, frame=frame)
```

### 驱动路径

```python
def create_driver_path(obj, target_obj, target_prop, data_path, var_name='var'):
    """创建驱动路径"""
    fcurves = obj.animation_data.drivers
    driver = fcurves.new(data_path=data_path)
    driver.driver.expression = var_name
    var = driver.driver.variables.new()
    var.name = var_name
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    return driver

def create_distance_driver(obj, target_obj, driver_var='dist'):
    """创建距离驱动"""
    return create_driver_path(
        obj, target_obj, 
        'location', 
        'location',
        driver_var
    )

def create_rotation_driver(obj, target_obj, axis='X', driver_var='rot'):
    """创建旋转驱动"""
    prop_path = f'rotation_euler.{axis.lower()}'
    return create_driver_path(obj, target_obj, prop_path, prop_path, driver_var)

def remove_all_drivers(obj):
    """移除所有驱动"""
    if obj.animation_data and obj.animation_data.drivers:
        for fcurve in list(obj.animation_data.drivers):
            obj.animation_data.drivers.remove(fcurve)
```

### 动画数据管理

```python
def copy_anim_data(source, target, replace=False):
    """复制动画数据"""
    if source.animation_data and source.animation_data.action:
        if not target.animation_data:
            target.animation_data_create()
        if replace or not target.animation_data.action:
            target.animation_data.action = source.animation_data.action.copy()

def merge_animations(target_action, source_action, start_frame=1):
    """合并动画"""
    if not source_action:
        return
    for fcurve in source_action.fcurves:
        for keyframe in fcurve.keyframe_points:
            keyframe.co[0] += start_frame - 1
        target_action.fcurves.new(fcurve.data_path)
        target_action.fcurves[-1].keyframe_points.add(len(fcurve.keyframe_points))
        for i, kp in enumerate(fcurve.keyframe_points):
            target_action.fcurves[-1].keyframe_points[i].co = kp.co
            target_action.fcurves[-1].keyframe_points[i].handle_left = kp.handle_left
            target_action.fcurves[-1].keyframe_points[i].handle_right = kp.handle_right

def clear_anim_data(obj, preserve_actions=True):
    """清除动画数据"""
    if not obj.animation_data:
        return
    if preserve_actions:
        actions = [fc.action for fc in obj.animation_data.drivers]
        for action in actions:
            if action and action.users == 1:
                bpy.data.actions.remove(action)
    obj.animation_data_clear()
```

## 实用函数

### 动画曲线分析

```python
def get_action_range(action):
    """获取动作帧范围"""
    if not action:
        return None, None
    start_frame = float('inf')
    end_frame = float('-inf')
    for fcurve in action.fcurves:
        for keyframe in fcurve.keyframe_points:
            if keyframe.co[0] < start_frame:
                start_frame = keyframe.co[0]
            if keyframe.co[0] > end_frame:
                end_frame = keyframe.co[0]
    return int(start_frame), int(end_frame) if end_frame != float('-inf') else None

def get_anim_length(obj):
    """获取动画长度"""
    action = None
    if obj.animation_data and obj.animation_data.action:
        action = obj.animation_data.action
    elif obj.animation_data and obj.animation_data.nla_tracks:
        for track in obj.animation_data.nla_tracks:
            for strip in track.strips:
                if strip.action:
                    action = strip.action
                    break
    if action:
        return get_action_range(action)
    return None, None

def count_keyframes(obj):
    """统计关键帧数量"""
    count = 0
    if obj.animation_data:
        if obj.animation_data.action:
            for fcurve in obj.animation_data.action.fcurves:
                count += len(fcurve.keyframe_points)
    return count
```

### 权重路径

```python
def create_weighted_deform_path(obj, vertex_group, points, frames, easing='AUTO'):
    """创建加权变形路径"""
    make_weighted_deform_path(
        obj, 
        vertex_group, 
        points, 
        frames, 
        easing=easing
    )

def create_warp_path(obj, vgroup, path_points, start_frame=1):
    """创建变形动画路径"""
    points = [p.co for p in path_points]
    frames = list(range(start_frame, start_frame + len(points)))
    create_weighted_deform_path(obj, vgroup, points, frames)
```

### 动画工具

```python
def bake_action_to_objects(action, objects, frame_start, frame_end):
    """将动作烘焙到多个对象"""
    for obj in objects:
        if not obj.animation_data:
            obj.animation_data_create()
        obj.animation_data.action = action.copy()
        bpy.context.view_layer.objects.active = obj
        bpy.ops.nla.bake(frame_start=frame_start, frame_end=frame_end)

def retarget_action(source_obj, target_obj, target_rig=None):
    """重定向动作"""
    if not source_obj.animation_data or not source_obj.animation_data.action:
        return
    action = source_obj.animation_data.action
    if not target_obj.animation_data:
        target_obj.animation_data_create()
    target_obj.animation_data.action = action.copy()
```

## 常见问题排查

### 关键帧不生效

数据路径错误：
- 确保使用正确的属性路径，如'location'、'rotation_euler'
- 使用obj.path_from_id()验证路径
- 检查属性是否可动画化

### 驱动不更新

依赖关系问题：
- 确保驱动目标对象存在
- 检查目标属性是否正确
- 尝试手动更新：bpy.context.view_layer.update()

### 动画丢失

数据清除问题：
- 在复制操作前检查动画数据是否存在
- 使用copy()而非直接赋值
- 及时保存未使用的数据块


---
## bpy_extras_asset_utils

# bpy_extras.asset_utils - 资源工具模块

bpy_extras.asset_utils模块提供了Blender资源库和资源管理的实用工具函数。

## 概述

资源工具模块支持资源库的浏览、资源的查找和过滤、以及资源数据的访问等操作。适用于开发资源浏览器插件和资源批量处理工具。

## 核心功能

### 资源库操作

```python
import bpy
from bpy_extras.asset_utils import (
    SpaceFileReferences,
    AssetLibrary,
    iterate_asset_files
)

def get_all_asset_libraries():
    """获取所有资源库"""
    return bpy.context.preferences.filepaths.asset_libraries

def get_active_asset_library():
    """获取当前活动资源库"""
    return bpy.context.window_manager.active_asset_library

def refresh_asset_library(library_path):
    """刷新资源库"""
    for library in bpy.context.preferences.filepaths.asset_libraries:
        if library.path == library_path:
            library.reload()
            break

def add_asset_library(name, path):
    """添加资源库"""
    prefs = bpy.context.preferences.filepaths
    new_lib = prefs.asset_libraries.add()
    new_lib.name = name
    new_lib.path = path
    return new_lib

def remove_asset_library(library_path):
    """移除资源库"""
    prefs = bpy.context.preferences.filepaths
    for i, lib in enumerate(prefs.asset_libraries):
        if lib.path == library_path:
            prefs.asset_libraries.remove(i)
            break
```

### 资源浏览

```python
def iterate_local_assets(directory):
    """遍历本地资源文件"""
    for item in iterate_asset_files(bpy.context, directory):
        yield item

def get_blend_assets(filepath):
    """获取blend文件中的资源"""
    with bpy.data.libraries.load(filepath) as (data_from, data_to):
        for collection in data_from.collections:
            yield {
                'name': collection,
                'type': 'collection',
                'filepath': filepath
            }
        for obj in data_from.objects:
            yield {
                'name': obj,
                'type': 'object',
                'filepath': filepath
            }
        for mat in data_from.materials:
            yield {
                'name': mat,
                'type': 'material',
                'filepath': filepath
            }

def find_assets_by_type(asset_library, asset_type):
    """按类型查找资源"""
    results = []
    for asset in iterate_asset_files(bpy.context, asset_library):
        if hasattr(asset, 'asset_type'):
            if asset.asset_type == asset_type:
                results.append(asset)
    return results
```

### 资源属性

```python
def get_asset_identifier(obj):
    """获取资源标识符"""
    if not obj.asset_data:
        return None
    return obj.asset_data.name

def set_asset_name(obj, name):
    """设置资源名称"""
    if obj.asset_data:
        obj.asset_data.name = name

def get_asset_tags(obj):
    """获取资源标签"""
    if not obj.asset_data:
        return []
    return [tag.name for tag in obj.asset_data.tags]

def add_asset_tag(obj, tag_name):
    """添加资源标签"""
    if obj.asset_data:
        obj.asset_data.tags.new(name=tag_name)

def remove_asset_tag(obj, tag_name):
    """移除资源标签"""
    if obj.asset_data:
        for tag in obj.asset_data.tags:
            if tag.name == tag_name:
                obj.asset_data.tags.remove(tag)
                break

def get_asset_author(obj):
    """获取资源作者"""
    if obj.asset_data:
        return obj.asset_data.author
    return None

def set_asset_author(obj, author):
    """设置资源作者"""
    if obj.asset_data:
        obj.asset_data.author = author
```

## 实用函数

### 资源导入

```python
def import_asset(filepath, asset_type='auto', link=False):
    """导入资源"""
    if asset_type == 'auto':
        if filepath.endswith('.blend'):
            asset_type = 'blend'
        elif filepath.endswith('.fbx'):
            asset_type = 'fbx'
        elif filepath.endswith('.gltf'):
            asset_type = 'gltf'
    
    if asset_type == 'blend':
        with bpy.data.libraries.load(filepath, link=link) as (src, dst):
            for item in src.objects:
                dst.objects.append(item)
        return dst.objects[-1] if dst.objects else None
    
    return None

def append_asset(filepath, asset_name, asset_type='collection'):
    """追加资源"""
    directory = filepath
    if asset_type == 'collection':
        directory = filepath + '/Collection/'
    elif asset_type == 'object':
        directory = filepath + '/Object/'
    elif asset_type == 'material':
        directory = filepath + '/Material/'
    
    bpy.ops.wm.append(
        filepath=filepath,
        directory=directory,
        files=[{'name': asset_name}],
        link=False
    )
```

### 资源导出

```python
def export_as_asset(obj, filepath, include_data=True):
    """导出为资源"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        use_selection=True,
        export_include=[] if include_data else ['DATA']
    )

def batch_export_assets(objects, output_dir, format='GLTF'):
    """批量导出资源"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for obj in objects:
        filepath = os.path.join(output_dir, f"{obj.name}.gltf")
        export_as_asset(obj, filepath)
```

### 资源管理

```python
class AssetManager:
    """资源管理器"""
    def __init__(self):
        self.assets = []
    
    def add_asset(self, obj):
        """添加资源"""
        if obj.asset_data:
            self.assets.append(obj)
    
    def remove_asset(self, obj):
        """移除资源"""
        if obj in self.assets:
            self.assets.remove(obj)
    
    def clear(self):
        """清空管理器"""
        self.assets.clear()
    
    def filter_by_tag(self, tag_name):
        """按标签过滤"""
        return [a for a in self.assets if tag_name in get_asset_tags(a)]
    
    def filter_by_type(self, obj_type):
        """按类型过滤"""
        return [a for a in self.assets if a.type == obj_type]
    
    def export_all(self, output_dir, format='GLTF'):
        """导出所有资源"""
        for obj in self.assets:
            export_as_asset(obj, os.path.join(output_dir, f"{obj.name}.{format.lower()}"))
```

## 常见问题排查

### 资源无法标记

数据块类型不支持：
- 只有ID数据块可以标记为资源
- 确保对象类型是可标记的
- 检查asset_data属性是否存在

### 资源库不显示

路径问题：
- 确认资源库路径存在
- 检查路径权限
- 尝试刷新资源库

### 导入资源失败

文件格式问题：
- 确认文件格式被支持
- 检查blend文件版本
- 验证资源名称正确性


---
## bpy_extras_id_map_utils

# bpy_extras.id_map_utils - ID映射工具模块

bpy_extras.id_map_utils模块提供了ID数据块与名称之间的映射转换功能。

## 概述

ID映射工具用于在不同数据块之间建立名称与实际数据对象的映射关系。这在处理资源导入导出、数据迁移等场景中非常有用。

## 核心功能

### ID映射创建

```python
import bpy
from bpy_extras.id_map_utils import (
    get_data_from_id,
    get_name_from_id
)

def create_id_map(id_block):
    """创建ID映射字典"""
    id_map = {}
    if hasattr(id_block, 'items'):
        for name, item in id_block.items():
            id_map[name] = item
    return id_map

def create_object_name_map(scene=None):
    """创建对象名称映射"""
    if scene is None:
        scene = bpy.context.scene
    name_map = {}
    for obj in scene.objects:
        name_map[obj.name] = obj
    return name_map

def create_material_name_map():
    """创建材质名称映射"""
    mat_map = {}
    for mat in bpy.data.materials:
        mat_map[mat.name] = mat
    return mat_map
```

### ID查询

```python
def get_object_by_name(name, scene=None):
    """通过名称获取对象"""
    if scene is None:
        scene = bpy.context.scene
    return scene.objects.get(name)

def get_material_by_name(name):
    """通过名称获取材质"""
    return bpy.data.materials.get(name)

def get_texture_by_name(name):
    """通过名称获取纹理"""
    return bpy.data.textures.get(name)

def get_image_by_name(name):
    """通过名称获取图像"""
    return bpy.data.images.get(name)

def get_collection_by_name(name, scene=None):
    """通过名称获取集合"""
    if scene is None:
        scene = bpy.context.scene
    return scene.collection.children.get(name) or bpy.data.collections.get(name)

def get_armature_by_name(name):
    """通过名称获取骨架"""
    return bpy.data.armatures.get(name)
```

### ID列表操作

```python
def get_all_object_names(scene=None):
    """获取所有对象名称"""
    if scene is None:
        scene = bpy.context.scene
    return [obj.name for obj in scene.objects]

def get_all_material_names():
    """获取所有材质名称"""
    return [mat.name for mat in bpy.data.materials]

def filter_objects_by_type(obj_type, scene=None):
    """按类型过滤对象"""
    if scene is None:
        scene = bpy.context.scene
    return {obj.name: obj for obj in scene.objects if obj.type == obj_type}

def filter_objects_by_layer(scene=None, only_visible=True):
    """按图层过滤对象"""
    if scene is None:
        scene = bpy.context.scene
    result = {}
    for obj in scene.objects:
        if only_visible and not obj.visible_get():
            continue
        result[obj.name] = obj
    return result
```

## 实用函数

### 数据迁移

```python
def remap_object_references(
    obj, 
    old_name_map, 
    new_name_map, 
    data_path='collection'
):
    """重新映射对象引用"""
    if data_path not in obj:
        return
    collection = obj[data_path]
    if isinstance(collection, bpy.types.Collection):
        new_collection = new_name_map.get(collection.name)
        if new_collection:
            obj[data_path] = new_collection

def remap_material_references(obj, old_mat_map, new_mat_map):
    """重新映射材质引用"""
    if obj.type != 'MESH':
        return
    for i, slot in enumerate(obj.material_slots):
        new_mat = new_mat_map.get(slot.material.name)
        if new_mat:
            obj.material_slots[i].material = new_mat

def batch_remap_references(
    objects, 
    old_name_map, 
    new_name_map, 
    reference_type='object'
):
    """批量重新映射引用"""
    for obj in objects:
        if reference_type == 'collection':
            remap_object_references(obj, old_name_map, new_name_map, 'collection')
        elif reference_type == 'material':
            remap_material_references(obj, old_name_map, new_name_map)
```

### 验证工具

```python
def validate_object_references(obj, name_map):
    """验证对象引用有效性"""
    issues = []
    for attr in ['active_material', 'data']:
        if hasattr(obj, attr):
            ref = getattr(obj, attr)
            if ref and hasattr(ref, 'name'):
                if ref.name not in name_map:
                    issues.append(f"{attr}: {ref.name}")
    return issues

def find_broken_references(scene=None):
    """查找断开的引用"""
    if scene is None:
        scene = bpy.context.scene
    broken = []
    for obj in scene.objects:
        issues = validate_object_references(obj, scene.objects)
        if issues:
            broken.append((obj.name, issues))
    return broken

def cleanup_unused_materials():
    """清理未使用的材质"""
    used_mats = set()
    for obj in bpy.context.scene.objects:
        if obj.type == 'MESH':
            for slot in obj.material_slots:
                if slot.material:
                    used_mats.add(slot.material)
    unused = [mat for mat in bpy.data.materials if mat not in used_mats]
    for mat in unused:
        bpy.data.materials.remove(mat)
    return len(unused)
```

## 常见问题排查

### 名称冲突

多个对象同名：
- Blender允许同名对象在不同集合中
- 使用完整的对象路径进行区分
- 使用object.name_full获取全名

### 引用丢失

数据块已删除：
- 检查是否所有引用都指向有效对象
- 使用try-except处理可能的空引用
- 在操作前验证名称映射

### 映射不完整

数据未加载：
- 确保相关数据块已加载到内存
- 检查collection层级结构
- 验证引用路径的正确性


---
## bpy_extras_image_utils

# bpy_extras.image_utils - 图像工具模块

bpy_extras.image_utils模块提供了图像文件加载、图像序列处理和图像转换的实用工具函数。

## 概述

图像工具模块封装了常见的图像处理任务，包括加载图像、生成缩略图、图像序列管理等。适用于开发材质浏览器、纹理管理工具和图像批处理插件。

## 核心功能

### 图像加载

```python
import bpy
import os
from bpy_extras.image_utils import load_image

def load_single_image(filepath, force_reload=False):
    """加载单个图像"""
    img = bpy.data.images.load(filepath, force_reload=force_reload)
    return img

def load_image_check(filepath):
    """加载图像并检查是否存在"""
    img = bpy.data.images.get(filepath)
    if img is None:
        img = bpy.data.images.load(filepath)
    return img

def load_image_with_name(filepath, name=None):
    """加载图像并指定名称"""
    img = bpy.data.images.load(filepath)
    if name:
        img.name = name
    return img

def load_image_relative(filepath, base_path):
    """从相对路径加载图像"""
    if os.path.isabs(filepath):
        full_path = filepath
    else:
        full_path = os.path.join(base_path, filepath)
    if os.path.exists(full_path):
        return load_single_image(full_path)
    return None
```

### 图像序列

```python
def load_image_sequence(directory, file_pattern, start_frame=1, end_frame=None):
    """加载图像序列"""
    images = []
    if end_frame is None:
        files = sorted(os.listdir(directory))
        end_frame = len(files)
    
    for frame in range(start_frame, end_frame + 1):
        filename = file_pattern % frame
        filepath = os.path.join(directory, filename)
        if os.path.exists(filepath):
            img = load_single_image(filepath)
            images.append(img)
    return images

def load_image_strip(filepath, num_images, name=None):
    """加载图像条"""
    img = bpy.data.images.load(filepath)
    if name:
        img.name = name
    img.source = 'STRIP'
    img.frame_start = 0
    img.frame_end = num_images - 1
    return img

def generate_image_sequence(preview_directory):
    """生成图像序列"""
    if not os.path.exists(preview_directory):
        os.makedirs(preview_directory)
    files = sorted(os.listdir(preview_directory))
    return [os.path.join(preview_directory, f) for f in files if f.endswith(('.png', '.jpg', '.jpeg'))]
```

### 缩略图生成

```python
def create_thumbnail(image, size=(128, 128)):
    """创建缩略图"""
    img = image.copy()
    img.scale(size)
    return img

def generate_thumbnails(source_dir, output_dir, size=(128, 128)):
    """批量生成缩略图"""
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for filename in os.listdir(source_dir):
        if filename.endswith(('.png', '.jpg', '.jpeg')):
            filepath = os.path.join(source_dir, filename)
            img = load_single_image(filepath)
            thumb = create_thumbnail(img, size)
            thumb.filepath_raw = os.path.join(output_dir, f"thumb_{filename}")
            thumb.file_format = 'PNG'
            thumb.save()
            img.user_clear()
    return output_dir
```

## 实用函数

### 图像管理

```python
def find_image_by_name(name):
    """通过名称查找图像"""
    return bpy.data.images.get(name)

def find_images_by_pattern(pattern):
    """通过模式查找图像"""
    results = []
    for img in bpy_data.images:
        if pattern in img.name:
            results.append(img)
    return results

def find_images_by_size(min_width=0, min_height=0):
    """按尺寸查找图像"""
    results = []
    for img in bpy.data.images:
        if img.size[0] >= min_width and img.size[1] >= min_height:
            results.append(img)
    return results

def cleanup_unused_images():
    """清理未使用的图像"""
    unused = [img for img in bpy.data.images if img.users == 0]
    for img in unused:
        bpy.data.images.remove(img)
    return len(unused)

def group_images_by_size():
    """按尺寸分组图像"""
    groups = {}
    for img in bpy.data.images:
        size_key = (img.size[0], img.size[1])
        if size_key not in groups:
            groups[size_key] = []
        groups[size_key].append(img)
    return groups
```

### 图像处理

```python
def resize_image(image, width, height):
    """调整图像大小"""
    image.scale((width, height))
    return image

def crop_image(image, x, y, width, height):
    """裁剪图像"""
    if hasattr(image, 'crop'):
        image.crop((x, y, x + width, y + height))
    return image

def rotate_image(image, angle):
    """旋转图像"""
    if hasattr(image, 'rotate'):
        image.rotate(angle)
    return image

def convert_image_format(image, target_format='PNG'):
    """转换图像格式"""
    image.file_format = target_format
    return image

def duplicate_image(source, name=None):
    """复制图像"""
    new_img = source.copy()
    if name:
        new_img.name = name
    bpy.data.images.append(new_img)
    return new_img
```

### 批处理工具

```python
class ImageBatchProcessor:
    """图像批处理器"""
    def __init__(self):
        self.images = []
        self.errors = []
    
    def add_from_directory(self, directory, extensions=('.png', '.jpg', '.jpeg')):
        """从目录添加图像"""
        for filename in os.listdir(directory):
            if filename.endswith(extensions):
                filepath = os.path.join(directory, filename)
                try:
                    img = load_single_image(filepath)
                    self.images.append(img)
                except Exception as e:
                    self.errors.append((filename, str(e)))
    
    def add_by_pattern(self, pattern):
        """按模式添加图像"""
        for img in bpy.data.images:
            if pattern in img.name:
                self.images.append(img)
    
    def resize_all(self, width, height):
        """批量调整大小"""
        for img in self.images:
            resize_image(img, width, height)
    
    def convert_all(self, target_format):
        """批量转换格式"""
        for img in self.images:
            convert_image_format(img, target_format)
    
    def export_all(self, output_dir, format='PNG'):
        """批量导出"""
        if not os.path.exists(output_dir):
            os.makedirs(output_dir)
        for img in self.images:
            filename = os.path.splitext(img.name)[0] + f'.{format.lower()}'
            filepath = os.path.join(output_dir, filename)
            export_image(img, filepath, format)
    
    def clear(self):
        """清空处理器"""
        self.images.clear()
        self.errors.clear()
```

## 常见问题排查

### 图像加载失败

文件路径问题：
- 确认文件路径正确且存在
- 检查文件权限
- 验证图像文件格式被支持

### 内存占用过高

图像数据过大：
- 及时清理未使用的图像
- 使用缩略图替代大图像
- 限制同时加载的图像数量

### 图像序列不同步

帧范围不匹配：
- 确保所有图像尺寸一致
- 检查帧编号连续性
- 验证文件命名模式正确


---
## bpy_extras_io_utils

# bpy_extras.io_utils - 输入输出工具模块

bpy_extras.io_utils模块提供了文件路径处理、数据导入导出和资源路径管理的实用工具函数。

## 概述

输入输出工具模块封装了Blender中常见的文件操作和路径处理任务。包括相对路径转换、文件查找、数据块导入导出等功能。

## 核心功能

### 路径处理

```python
import bpy
import os
from bpy_extras.io_utils import (
    path_reference_target,
    path_resolve
)

def blender_path_to_system(blender_path):
    """将Blender路径转换为系统路径"""
    if blender_path.startswith('//'):
        blend_dir = bpy.path.abspath('//')
        return os.path.join(blend_dir, blender_path[2:])
    return blender_path

def system_path_to_blender(system_path):
    """将系统路径转换为Blender相对路径"""
    blend_file = bpy.data.filepath
    if not blend_file:
        return system_path
    blend_dir = os.path.dirname(blend_file)
    if system_path.startswith(blend_dir):
        return '//' + os.path.relpath(system_path, blend_dir)
    return system_path

def resolve_path(path, absolute=True):
    """解析路径"""
    if path.startswith('//'):
        resolved = bpy.path.abspath(path)
    else:
        resolved = os.path.abspath(path)
    if not absolute:
        resolved = os.path.relpath(resolved, bpy.path.abspath('//'))
    return resolved

def get_relative_path(from_path, to_path):
    """获取相对路径"""
    from_abs = blender_path_to_system(from_path) if from_path.startswith('//') else from_path
    to_abs = blender_path_to_system(to_path) if to_path.startswith('//') else to_path
    return os.path.relpath(to_abs, os.path.dirname(from_abs))
```

### 文件操作

```python
def find_file(directory, filename):
    """在目录中查找文件"""
    for root, dirs, files in os.walk(directory):
        if filename in files:
            return os.path.join(root, filename)
    return None

def find_files(directory, pattern):
    """查找匹配模式的文件"""
    results = []
    for root, dirs, files in os.walk(directory):
        for f in files:
            if pattern in f or f.endswith(pattern):
                results.append(os.path.join(root, f))
    return results

def list_directory(directory, extensions=None):
    """列出目录内容"""
    if not os.path.exists(directory):
        return []
    items = os.listdir(directory)
    if extensions:
        items = [i for i in items if any(i.endswith(ext) for ext in extensions)]
    return items

def create_directory(path):
    """创建目录"""
    if not os.path.exists(path):
        os.makedirs(path)
    return path
```

### 数据块导出

```python
def export_data_block(data_block, filepath, data_type='auto'):
    """导出数据块"""
    if data_type == 'auto':
        if isinstance(data_block, bpy.types.Mesh):
            data_type = 'mesh'
        elif isinstance(data_block, bpy.types.Material):
            data_type = 'material'
        elif isinstance(data_block, bpy.types.Object):
            data_type = 'object'
    
    if data_type == 'object':
        export_object_to_obj(data_block, filepath)
    elif data_type == 'mesh':
        export_mesh_to_file(data_block, filepath)
    
    return filepath

def export_object_to_obj(obj, filepath):
    """将对象导出为OBJ"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.export_scene.obj(
        filepath=filepath,
        use_selection=True,
        use_normals=True,
        use_triangles=True
    )

def export_objects_to_gltf(objects, filepath):
    """将多个对象导出为GLTF"""
    bpy.ops.object.select_all(action='DESELECT')
    for obj in objects:
        obj.select_set(True)
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        use_selection=True,
        export_apply=True
    )
```

## 实用函数

### 数据块导入

```python
def import_data_block(filepath, data_type, link=False):
    """导入数据块"""
    if data_type == 'object':
        return import_object(filepath, link)
    elif data_type == 'material':
        return import_material(filepath, link)
    return None

def import_object(filepath, link=False):
    """导入对象"""
    if filepath.endswith('.obj'):
        bpy.ops.import_scene.obj(filepath=filepath)
    elif filepath.endswith('.gltf') or filepath.endswith('.glb'):
        bpy.ops.import_scene.gltf(filepath=filepath)
    elif filepath.endswith('.fbx'):
        bpy.ops.import_scene.fbx(filepath=filepath)
    return bpy.context.selected_objects[-1] if bpy.context.selected_objects else None

def import_material(filepath):
    """导入材质"""
    if filepath.endswith('.blend'):
        bpy.ops.wm.append(
            filepath=filepath,
            directory=os.path.dirname(filepath) + '/Material/',
            files=[{'name': os.path.splitext(os.path.basename(filepath))[0]}]
        )
    return bpy.data.materials[-1] if bpy.data.materials else None
```

### 资源管理

```python
def get_resource_path(resource_type):
    """获取资源类型路径"""
    prefs = bpy.context.preferences
    if resource_type == 'texture':
        return prefs.filepaths.texture_directory
    elif resource_type == 'script':
        return prefs.filepaths.script_directory
    elif resource_type == 'font':
        return prefs.filepaths.font_directory
    return ''

def set_resource_path(resource_type, path):
    """设置资源类型路径"""
    prefs = bpy.context.preferences
    if resource_type == 'texture':
        prefs.filepaths.texture_directory = path
    elif resource_type == 'script':
        prefs.filepaths.script_directory = path
    elif resource_type == 'font':
        prefs.filepaths.font_directory = path

def find_resources(resource_type, pattern):
    """查找资源"""
    resource_dir = get_resource_path(resource_type)
    if resource_dir and os.path.exists(resource_dir):
        return find_files(resource_dir, pattern)
    return []
```

### 文件验证

```python
def validate_file_exists(filepath):
    """验证文件存在"""
    if filepath.startswith('//'):
        abs_path = blender_path_to_system(filepath)
    else:
        abs_path = filepath
    return os.path.exists(abs_path)

def validate_blend_file(filepath):
    """验证blend文件"""
    if not validate_file_exists(filepath):
        return False
    with open(filepath, 'rb') as f:
        header = f.read(7)
        return header == b'BLENDER'
    
def get_file_size(filepath):
    """获取文件大小"""
    abs_path = blender_path_to_system(filepath)
    if os.path.exists(abs_path):
        return os.path.getsize(abs_path)
    return 0

def get_file_mtime(filepath):
    """获取文件修改时间"""
    abs_path = blender_path_to_system(filepath)
    if os.path.exists(abs_path):
        return os.path.getmtime(abs_path)
    return 0
```

## 常见问题排查

### 相对路径无效

文件未保存：
- 相对路径需要先保存blend文件
- 使用绝对路径进行测试
- 检查路径是否正确

### 导入导出失败

格式不支持：
- 确认文件格式被Blender支持
- 安装必要的导入导出插件
- 检查文件是否损坏

### 路径分隔符

跨平台问题：
- 使用os.path.join构建路径
- 避免硬编码分隔符
- 使用bpy.path模块处理Blender路径


---
## bpy_extras_keyconfig_utils

# bpy_extras.keyconfig_utils - 键位配置工具模块

bpy_extras.keyconfig_utils模块提供了键位配置管理、快捷键操作和自定义键位映射的实用工具函数。

## 概述

键位配置工具模块用于管理Blender的快捷键配置，支持快捷键的添加、移除、查询和导出等操作。适用于开发自定义键位映射和键位管理工具。

## 核心功能

### 键位配置管理

```python
import bpy
from bpy_extras.keyconfig_utils import (
    keyconfig_init_from_data,
    keyconfig_test
)

def get_all_keyconfigs():
    """获取所有键位配置"""
    return bpy.context.window_manager.keyconfigs

def get_user_keyconfig():
    """获取用户键位配置"""
    return bpy.context.window_manager.keyconfigs.user

def get_addon_keyconfigs():
    """获取插件键位配置"""
    return bpy.context.window_manager.keyconfigs.addon

def get_prefs_keyconfig():
    """获取默认键位配置"""
    return bpy.context.window_manager.keyconfigs.default

def get_keyconfig_by_name(name):
    """通过名称获取键位配置"""
    for kc in bpy.context.window_manager.keyconfigs:
        if kc.name == name:
            return kc
    return None
```

### 快捷键操作

```python
def add_shortcut(keyconfig, idname, type, value, shift=False, ctrl=False, alt=False, oskey=False):
    """添加快捷键"""
    if not keyconfig:
        return None
    keymap = keyconfig.keymaps.new(name='Object Mode', space_type='EMPTY')
    return keymap.keymap_items.new(
        idname=idname,
        type=type,
        value=value,
        shift=shift,
        ctrl=ctrl,
        alt=alt,
        oskey=oskey
    )

def remove_shortcut(keyconfig, idname, type=None, value=None):
    """移除快捷键"""
    for keymap in keyconfig.keymaps:
        for item in list(keymap.keymap_items):
            if item.idname == idname:
                if type is None or (item.type == type and (value is None or item.value == value)):
                    keymap.keymap_items.remove(item)

def find_shortcut(keyconfig, idname):
    """查找快捷键"""
    results = []
    for keymap in keyconfig.keymaps:
        for item in keymap.keymap_items:
            if item.idname == idname:
                results.append({
                    'keymap': keymap.name,
                    'type': item.type,
                    'value': item.value,
                    'shift': item.shift,
                    'ctrl': item.ctrl,
                    'alt': item.alt
                })
    return results

def get_shortcut_string(keymap_item):
    """获取快捷键字符串"""
    parts = []
    if keymap_item.ctrl:
        parts.append('Ctrl')
    if keymap_item.shift:
        parts.append('Shift')
    if keymap_item.alt:
        parts.append('Alt')
    if keymap_item.oskey:
        parts.append('OS')
    parts.append(keymap_item.type)
    return '+'.join(parts)
```

### 键位配置导出导入

```python
def export_keyconfig(keyconfig, filepath):
    """导出键位配置"""
    import json
    data = {}
    for keymap in keyconfig.keymaps:
        keymap_data = []
        for item in keymap.keymap_items:
            keymap_data.append({
                'idname': item.idname,
                'type': item.type,
                'value': item.value,
                'shift': item.shift,
                'ctrl': item.ctrl,
                'alt': item.alt,
                'oskey': item.oskey,
                'properties': dict(item.properties)
            })
        data[keymap.name] = keymap_data
    
    with open(filepath, 'w', encoding='utf-8') as f:
        json.dump(data, f, indent=2, ensure_ascii=False)

def import_keyconfig(filepath, keyconfig_name='Custom'):
    """导入键位配置"""
    import json
    with open(filepath, 'r', encoding='utf-8') as f:
        data = json.load(f)
    
    kc = bpy.context.window_manager.keyconfigs.addon.new(keyconfig_name)
    for keymap_name, items in data.items():
        keymap = kc.keymaps.new(name=keymap_name, space_type='EMPTY')
        for item_data in items:
            item = keymap.keymap_items.new(
                idname=item_data['idname'],
                type=item_data['type'],
                value=item_data['value'],
                shift=item_data.get('shift', False),
                ctrl=item_data.get('ctrl', False),
                alt=item_data.get('alt', False),
                oskey=item_data.get('oskey', False)
            )
            for prop_name, prop_value in item_data.get('properties', {}).items():
                setattr(item.properties, prop_name, prop_value)
    return kc
```

## 实用函数

### 键位查询

```python
def get_keymap_items_for_operator(operator_id):
    """获取操作符的所有键位项"""
    results = []
    for kc in bpy.context.window_manager.keyconfigs:
        for keymap in kc.keymaps:
            for item in keymap.keymap_items:
                if item.idname == operator_id:
                    results.append({
                        'keyconfig': kc.name,
                        'keymap': keymap.name,
                        'item': item
                    })
    return results

def get_assigned_keys(idname):
    """获取操作符的已分配键位"""
    all_assignments = find_shortcut(
        bpy.context.window_manager.keyconfigs.user,
        idname
    )
    return all_assignments

def search_keymaps(search_term):
    """搜索键位映射"""
    results = []
    for kc in bpy.context.window_manager.keyconfigs:
        for keymap in kc.keymaps:
            for item in keymap.keymap_items:
                if (search_term in item.idname or 
                    search_term in keymap.name or
                    search_term in item.type):
                    results.append({
                        'keyconfig': kc.name,
                        'keymap': keymap.name,
                        'idname': item.idname,
                        'type': item.type,
                        'value': item.value
                    })
    return results
```

### 键位冲突检测

```python
def find_key_conflicts(keyconfig=None):
    """查找键位冲突"""
    if keyconfig is None:
        keyconfig = bpy.context.window_manager.keyconfigs.user
    
    key_map = {}
    conflicts = []
    
    for keymap in keyconfig.keymaps:
        for item in keymap.keymap_items:
            key = (item.type, item.value, item.shift, item.ctrl, item.alt, item.oskey)
            if key in key_map:
                conflicts.append({
                    'key': key,
                    'first': key_map[key],
                    'second': {
                        'keymap': keymap.name,
                        'idname': item.idname
                    }
                })
            else:
                key_map[key] = {
                    'keymap': keymap.name,
                    'idname': item.idname
                }
    
    return conflicts

def check_key_available(key_type, key_value='PRESS', shift=False, ctrl=False, alt=False):
    """检查键位是否可用"""
    for kc in bpy.context.window_manager.keyconfigs:
        for keymap in kc.keymaps:
            for item in keymap.keymap_items:
                if (item.type == key_type and 
                    item.value == key_value and
                    item.shift == shift and
                    item.ctrl == ctrl and
                    item.alt == alt):
                    return False
    return True
```

### 键位重置

```python
def reset_keyconfig(keyconfig_name):
    """重置键位配置"""
    kc = get_keyconfig_by_name(keyconfig_name)
    if kc:
        bpy.context.window_manager.keyconfigs.remove(kc)

def reset_to_default():
    """重置为默认键位"""
    bpy.ops.wm.keyconfig_preset_add(name='Blender')

def restore_factory_keyconfig():
    """恢复出厂设置"""
    bpy.ops.wm.read_factory_keyconfig()
```

## 常见问题排查

### 快捷键不生效

键位配置未激活：
- 确认键位配置已启用
- 检查快捷键是否被覆盖
- 重启Blender使更改生效

### 键位冲突

多个操作符使用相同键位：
- 使用find_key_conflicts查找冲突
- 修改其中一个快捷键
- 使用修饰键区分

### 导入失败

配置格式错误：
- 确认JSON格式正确
- 检查必要的字段是否存在
- 验证操作符ID有效


---
## bpy_extras_mesh_utils

# bpy_extras.mesh_utils - 网格工具模块

bpy_extras.mesh_utils模块提供了网格数据操作、几何分析和网格转换的实用工具函数。

## 概述

网格工具模块封装了常见的网格处理任务，包括顶点查找、边面操作、网格分析和几何变换等。适用于开发网格处理工具和几何分析插件。

## 核心功能

### 顶点操作

```python
import bpy
import bmesh
from mathutils import Vector
from bpy_extras.mesh_utils import (
    mesh_linked_tri_lookup,
    edge_face_count
)

def get_vertex_neighbors(mesh, vert_index):
    """获取顶点的相邻顶点"""
    vert = mesh.vertices[vert_index]
    neighbors = []
    for edge in vert.link_edges:
        for v in edge.verts:
            if v.index != vert_index:
                neighbors.append(v.index)
    return list(set(neighbors))

def get_vertex_faces(mesh, vert_index):
    """获取顶点的关联面"""
    vert = mesh.vertices[vert_index]
    faces = []
    for loop in vert.link_loops:
        faces.append(loop.face.index)
    return list(set(faces))

def find_vertex_by_co(mesh, co, tolerance=0.001):
    """通过坐标查找顶点"""
    for vert in mesh.vertices:
        if (vert.co - co).length < tolerance:
            return vert
    return None

def find_vertices_in_sphere(mesh, center, radius):
    """查找球体内的顶点"""
    vertices = []
    for vert in mesh.vertices:
        if (vert.co - center).length <= radius:
            vertices.append(vert)
    return vertices

def sort_vertices_by_distance(mesh, center):
    """按距离排序顶点"""
    verts = [(v, (v.co - center).length) for v in mesh.vertices]
    return [v for v, d in sorted(verts, key=lambda x: x[1])]
```

### 边操作

```python
def get_edge_vertices(mesh, edge_index):
    """获取边的两个顶点"""
    edge = mesh.edges[edge_index]
    return edge.vertices[0], edge.vertices[1]

def get_edge_length(mesh, edge_index):
    """获取边长度"""
    v1, v2 = get_edge_vertices(mesh, edge_index)
    return (mesh.vertices[v1].co - mesh.vertices[v2].co).length

def get_edge_faces(mesh, edge_index):
    """获取边关联的面"""
    edge = mesh.edges[edge_index]
    faces = []
    for poly in mesh.polygons:
        if edge_index in [e for e in poly.edge_keys]:
            faces.append(poly.index)
    return faces

def find_short_edges(mesh, threshold):
    """查找短边"""
    short_edges = []
    for i, edge in enumerate(mesh.edges):
        if get_edge_length(mesh, i) < threshold:
            short_edges.append(i)
    return short_edges

def find_long_edges(mesh, threshold):
    """查找长边"""
    long_edges = []
    for i, edge in enumerate(mesh.edges):
        if get_edge_length(mesh, i) > threshold:
            long_edges.append(i)
    return long_edges
```

### 面操作

```python
def get_face_vertices(mesh, face_index):
    """获取面的顶点索引"""
    face = mesh.polygons[face_index]
    return list(face.vertices)

def get_face_edges(mesh, face_index):
    """获取面的边索引"""
    face = mesh.polygons[face_index]
    return list(face.edge_indices)

def get_face_center(mesh, face_index):
    """获取面中心"""
    verts = get_face_vertices(mesh, face_index)
    center = Vector((0, 0, 0))
    for v_idx in verts:
        center += mesh.vertices[v_idx].co
    return center / len(verts)

def get_face_normal(mesh, face_index):
    """获取面法线"""
    return mesh.polygons[face_index].normal

def get_face_area(mesh, face_index):
    """获取面面积"""
    return mesh.polygons[face_index].area

def find_large_faces(mesh, threshold):
    """查找大面积面"""
    large_faces = []
    for i in range(len(mesh.polygons)):
        if get_face_area(mesh, i) > threshold:
            large_faces.append(i)
    return large_faces

def find_small_faces(mesh, threshold):
    """查找小面积面"""
    small_faces = []
    for i in range(len(mesh.polygons)):
        if get_face_area(mesh, i) < threshold:
            small_faces.append(i)
    return small_faces
```

## 实用函数

### 网格分析

```python
def calculate_mesh_volume(mesh):
    """计算网格体积"""
    volume = 0.0
    for poly in mesh.polygons:
        verts = [mesh.vertices[v].co for v in poly.vertices]
        if len(verts) >= 3:
            v0 = verts[0]
            for v1, v2 in zip(verts[1:-1], verts[2:]):
                volume += v0.cross(v1).dot(v2)
    return abs(volume) / 6.0

def calculate_surface_area(mesh):
    """计算表面积"""
    area = 0.0
    for poly in mesh.polygons:
        area += poly.area
    return area

def get_bounding_box(mesh):
    """获取包围盒"""
    min_co = Vector((float('inf'), float('inf'), float('inf')))
    max_co = Vector((float('-inf'), float('-inf'), float('-inf')))
    for vert in mesh.vertices:
        for i in range(3):
            if vert.co[i] < min_co[i]:
                min_co[i] = vert.co[i]
            if vert.co[i] > max_co[i]:
                max_co[i] = vert.co[i]
    return min_co, max_co

def get_mesh_dimensions(mesh):
    """获取网格尺寸"""
    min_co, max_co = get_bounding_box()
    return max_co - min_co

def analyze_mesh_topology(mesh):
    """分析网格拓扑"""
    stats = {
        'vertices': len(mesh.vertices),
        'edges': len(mesh.edges),
        'faces': len(mesh.polygons),
        'face_vertices_distribution': {},
        'ngons': 0
    }
    for poly in mesh.polygons:
        num_verts = len(poly.vertices)
        stats['face_vertices_distribution'][num_verts] = \
            stats['face_vertices_distribution'].get(num_verts, 0) + 1
        if num_verts > 4:
            stats['ngons'] += 1
    return stats
```

### 网格转换

```python
def triangulate_mesh(mesh):
    """三角化网格"""
    bm = bmesh.new()
    bm.from_mesh(mesh)
    bmesh.ops.triangulate(bm, faces=bm.faces)
    result = bpy.data.meshes.new(mesh.name + '_tri')
    bm.to_mesh(result)
    bm.free()
    return result

def quadify_mesh(mesh):
    """四边形化网格"""
    bm = bmesh.new()
    bm.from_mesh(mesh)
    bmesh.ops.tris_convert_to_quads(bm, faces=bm.faces)
    result = bpy.data.meshes.new(mesh.name + '_quad')
    bm.to_mesh(result)
    bm.free()
    return result

def subdivide_mesh(mesh, cuts=1):
    """细分网格"""
    bm = bmesh.new()
    bm.from_mesh(mesh)
    bmesh.ops.subdivide_edges(
        bm, 
        edges=bm.edges, 
        cuts=cuts, 
        use_grid_fill=True
    )
    result = bpy.data.meshes.new(mesh.name + '_sub')
    bm.to_mesh(result)
    bm.free()
    return result

def decimate_mesh(mesh, ratio):
    """简化网格"""
    bm = bmesh.new()
    bm.from_mesh(mesh)
    bmesh.ops.decimate(
        bm, 
        geom=bm.faces + bm.edges + bm.verts, 
        face_dest=len(bm.faces) * ratio
    )
    result = bpy.data.meshes.new(mesh.name + '_dec')
    bm.to_mesh(result)
    bm.free()
    return result
```

### 网格数据

```python
def get_uv_coordinates(mesh, face_index):
    """获取面的UV坐标"""
    if not mesh.uv_layers:
        return []
    uv_layer = mesh.uv_layers.active.data
    face = mesh.polygons[face_index]
    return [uv_layer[loop_index].uv for loop_index in face.loop_indices]

def get_vertex_colors(mesh, face_index):
    """获取面的顶点颜色"""
    if not mesh.vertex_colors:
        return []
    color_layer = mesh.vertex_colors.active.data
    face = mesh.polygons[face_index]
    return [color_layer[loop_index].color for loop_index in face.loop_indices]

def create_attribute(mesh, name, data, domain='POINT'):
    """创建属性"""
    if domain == 'POINT':
        mesh.attributes.new(name=name, type='FLOAT', domain='POINT')
        attr = mesh.attributes[name].data
        for i, value in enumerate(data):
            attr[i].value = value if isinstance(value, (int, float)) else value[0]
```

## 常见问题排查

### 顶点查找失败

坐标不精确：
- 使用适当的容差值
- 考虑网格变换
- 检查坐标系是否一致

### 网格操作缓慢

数据量过大：
- 使用bmesh进行批量操作
- 分块处理大型网格
- 减少不必要的遍历

### 网格变形异常

变换顺序问题：
- 应用所有变换
- 考虑局部和世界坐标系
- 使用正确的矩阵变换


---
## bpy_extras_node_utils

# bpy_extras.node_utils - 节点工具模块

bpy_extras.node_utils模块提供了节点编辑、节点连接和节点树操作的实用工具函数。

## 概述

节点工具模块封装了Blender节点编辑器中的常见操作，包括节点输入输出查找、节点连接管理、节点树遍历等功能。适用于开发节点处理工具和材质编辑器插件。

## 核心功能

### 节点插槽查找

```python
import bpy
from bpy_extras.node_utils import (
    find_node_input,
    find_node_output,
    socket_id_to_index,
    socket_index_to_id
)

def get_node_input_socket(node, identifier):
    """查找节点的输入插槽"""
    return find_node_input(node, identifier)

def get_node_output_socket(node, identifier):
    """查找节点的输出插槽"""
    return find_node_output(node, identifier)

def get_socket_index(node, identifier, is_input):
    """将插槽ID转换为索引"""
    return socket_id_to_index(node, identifier, is_input)

def get_socket_id(node, index, is_input):
    """将插槽索引转换为ID"""
    return socket_index_to_id(node, index, is_input)

def list_all_inputs(node):
    """列出所有输入插槽"""
    inputs = []
    for input in node.inputs:
        inputs.append({
            'name': input.name,
            'identifier': input.identifier,
            'type': input.type,
            'default_value': input.default_value if hasattr(input, 'default_value') else None
        })
    return inputs

def list_all_outputs(node):
    """列出所有输出插槽"""
    outputs = []
    for output in node.outputs:
        outputs.append({
            'name': output.name,
            'identifier': output.identifier,
            'type': output.type
        })
    return outputs
```

### 节点查找

```python
def find_node_by_type(node_tree, node_type):
    """按类型查找节点"""
    for node in node_tree.nodes:
        if node.type == node_type:
            return node
    return None

def find_nodes_by_type(node_tree, node_type):
    """按类型查找所有节点"""
    return [node for node in node_tree.nodes if node.type == node_type]

def find_node_by_name(node_tree, name):
    """按名称查找节点"""
    return node_tree.nodes.get(name)

def find_nodes_by_name_pattern(node_tree, pattern):
    """按名称模式查找节点"""
    return [node for node in node_tree.nodes if pattern in node.name]

def find_node_with_input_connected(node_tree, node_type, input_name):
    """查找输入已连接的节点"""
    for node in node_tree.nodes:
        if node.type == node_type:
            input_socket = node.inputs.get(input_name)
            if input_socket and input_socket.is_linked:
                return node
    return None

def find_node_with_output_connected(node_tree, node_type, output_name):
    """查找输出已连接的节点"""
    for node in node_tree.nodes:
        if node.type == node_type:
            output_socket = node.outputs.get(output_name)
            if output_socket and output_socket.is_linked:
                return node
    return None
```

### 节点连接

```python
def get_node_connections(node_tree):
    """获取所有连接"""
    connections = []
    for link in node_tree.links:
        connections.append({
            'from_node': link.from_node.name,
            'from_socket': link.from_socket.name,
            'to_node': link.to_node.name,
            'to_socket': link.to_socket.name
        })
    return connections

def connect_nodes(node_tree, from_node, from_socket, to_node, to_socket):
    """连接两个节点"""
    socket_from = from_node.outputs.get(from_socket)
    socket_to = to_node.inputs.get(to_socket)
    if socket_from and socket_to:
        return node_tree.links.new(socket_from, socket_to)
    return None

def disconnect_node_input(node, input_name):
    """断开节点的输入连接"""
    socket = node.inputs.get(input_name)
    if socket and socket.is_linked:
        link = socket.links[0]
        node_tree = node.id_data
        node_tree.links.remove(link)

def disconnect_node_output(node, output_name):
    """断开节点的输出连接"""
    socket = node.outputs.get(output_name)
    if socket:
        for link in list(socket.links):
            node_tree = node.id_data
            node_tree.links.remove(link)

def remove_all_connections(node):
    """移除节点的所有连接"""
    for input in list(node.inputs):
        for link in list(input.links):
            node.id_data.links.remove(link)
    for output in list(node.outputs):
        for link in list(output.links):
            node.id_data.links.remove(link)
```

## 实用函数

### 节点树遍历

```python
def traverse_node_tree(node_tree, include_hidden=True):
    """遍历节点树"""
    nodes = []
    for node in node_tree.nodes:
        if not include_hidden and node.hide:
            continue
        nodes.append(node)
    return nodes

def find_source_node(node_tree):
    """查找源节点"""
    for node in node_tree.nodes:
        if node.type == 'GROUP':
            return node
        if hasattr(node, 'inputs') and len(node.inputs) == 0:
            return node
    return None

def find_output_node(node_tree):
    """查找输出节点"""
    for node in node_tree.nodes:
        if node.type == 'OUTPUT_MATERIAL':
            return node
        if hasattr(node, 'inputs') and len(node.inputs) > 0 and node.type == 'OUTPUT':
            return node
    return None

def find_dependent_nodes(node_tree, start_node):
    """查找依赖节点"""
    dependent = []
    stack = [start_node]
    while stack:
        current = stack.pop()
        for input in current.inputs:
            if input.is_linked:
                link = input.links[0]
                from_node = link.from_node
                if from_node not in dependent:
                    dependent.append(from_node)
                    stack.append(from_node)
    return dependent

def find_dependency_chain(node_tree, end_node, max_depth=10):
    """查找依赖链"""
    chain = []
    current = end_node
    depth = 0
    while depth < max_depth:
        for input in current.inputs:
            if input.is_linked:
                link = input.links[0]
                chain.append(link.from_node)
                current = link.from_node
                break
        else:
            break
        depth += 1
    return chain
```

### 节点创建

```python
def create_node(node_tree, node_type, name=None, location=(0, 0)):
    """创建节点"""
    node = node_tree.nodes.new(type=node_type)
    if name:
        node.name = name
    node.location = location
    return node

def duplicate_node(node_tree, node):
    """复制节点"""
    new_node = node_tree.nodes.new(type=node.type)
    new_node.name = node.name + '.001'
    new_node.location = (node.location.x + 300, node.location.y)
    new_node.inputs['Strength'].default_value = node.inputs['Strength'].default_value
    return new_node

def create_ principled_bsdf_setup(node_tree, location=(-200, 0)):
    """创建Principled BSDF设置"""
    output = find_node_by_type(node_tree, 'OUTPUT_MATERIAL')
    if not output:
        output = create_node(node_tree, 'ShaderNodeOutputMaterial', location=(400, 0))
    
    principled = create_node(node_tree, 'ShaderNodeBsdfPrincipled', location=(0, 0))
    
    connect_nodes(node_tree, principled, 'BSDF', output, 'Surface')
    return principled

def create_texture_coordinate_chain(node_tree):
    """创建纹理坐标链"""
    coord = create_node(node_tree, 'ShaderNodeTexCoord', location=(-600, 0))
    mapping = create_node(node_tree, 'ShaderNodeMapping', location=(-300, 0))
    image = create_node(node_tree, 'ShaderNodeTexImage', location=(0, 0))
    
    connect_nodes(node_tree, coord, 'Generated', mapping, 'Vector')
    connect_nodes(node_tree, mapping, 'Vector', image, 'Vector')
    return coord, mapping, image
```

### 节点属性

```python
def copy_node_attributes(source, target):
    """复制节点属性"""
    for input in source.inputs:
        if input.is_linked:
            continue
        if hasattr(input, 'default_value'):
            target_input = target.inputs.get(input.name)
            if target_input and hasattr(target_input, 'default_value'):
                target_input.default_value = input.default_value

def get_node_type_description(node_type):
    """获取节点类型描述"""
    descriptions = {
        'TEX_IMAGE': '图像纹理节点',
        'BSDF_PRINCIPLED': 'Principled BSDF节点',
        'OUTPUT_MATERIAL': '材质输出节点',
        'TEX_COORD': '纹理坐标节点',
        'MAPPING': '映射节点'
    }
    return descriptions.get(node_type, '未知节点类型')

def get_socket_type_description(socket_type):
    """获取插槽类型描述"""
    descriptions = {
        'VALUE': '浮点值',
        'RGBA': '颜色',
        'VECTOR': '向量',
        'SHADER': '着色器'
    }
    return descriptions.get(socket_type, '未知类型')
```

## 常见问题排查

### 节点未找到

节点不存在：
- 确认节点类型名称正确
- 检查节点树是否正确
- 使用find_nodes_by_type获取所有匹配节点

### 连接失败

插槽类型不匹配：
- 确认两个插槽类型兼容
- 检查节点是否已存在连接
- 验证节点是否在同一个节点树中

### 属性访问错误

属性不存在：
- 使用hasattr检查属性
- 确认节点类型支持该属性
- 检查输入输出名称是否正确


---
## bpy_extras_object_utils

# bpy_extras.object_utils - 对象工具模块

bpy_extras.object_utils模块提供了对象创建、对象变换和对象数据管理的实用工具函数。

## 概述

对象工具模块封装了Blender中常见的对象操作，包括对象的创建、数据添加、坐标转换等功能。适用于开发自动化建模工具和对象管理插件。

## 核心功能

### 对象创建

```python
import bpy
import mathutils
from mathutils import Vector, Matrix
from bpy_extras.object_utils import (
    object_data_add,
    object_data_link,
    add_object_align_init
)

def create_mesh_object(name, vertices, edges=None, faces=None, location=(0, 0, 0)):
    """创建网格对象"""
    mesh = bpy.data.meshes.new(name)
    mesh.from_pydata(vertices, edges if edges else [], faces if faces else [])
    mesh.update()
    
    obj = bpy.data.objects.new(name, mesh)
    bpy.context.collection.objects.link(obj)
    obj.location = location
    
    return obj

def create_curve_object(name, points, location=(0, 0, 0)):
    """创建曲线对象"""
    curve_data = bpy.data.curves.new(name, type='CURVE')
    curve_data.dimensions = '3D'
    
    spline = curve_data.splines.new('BEZIER')
    spline.bezier_points.add(len(points) - 1)
    for i, point in enumerate(points):
        spline.bezier_points[i].co = point
    
    obj = bpy.data.objects.new(name, curve_data)
    bpy.context.collection.objects.link(obj)
    obj.location = location
    
    return obj

def add_primitive_cube(location=(0, 0, 0), size=2):
    """添加立方体"""
    bpy.ops.mesh.primitive_cube_add(size=size, location=location)
    return bpy.context.active_object

def add_primitive_sphere(location=(0, 0, 0), radius=1, segments=32, ring_count=16):
    """添加球体"""
    bpy.ops.mesh.primitive_uv_sphere_add(
        radius=radius, 
        segments=segments, 
        ring_count=ring_count,
        location=location
    )
    return bpy.context.active_object

def add_primitive_cylinder(location=(0, 0, 0), radius=1, depth=2, vertices=32):
    """添加圆柱体"""
    bpy.ops.mesh.primitive_cylinder_add(
        radius=radius, 
        depth=depth, 
        vertices=vertices,
        location=location
    )
    return bpy.context.active_object
```

### 对象数据添加

```python
def add_mesh_data(context, mesh):
    """添加网格数据到新对象"""
    return object_data_add(context, mesh)

def add_curve_data(context, curve):
    """添加曲线数据到新对象"""
    return object_data_add(context, curve)

def link_object_data(data, scene=None, collection=None, object_name=None):
    """链接数据到对象"""
    return object_data_link(data, scene, collection, object_name)

def add_object_with_alignment(context, operator, align_type='WORLD'):
    """添加带对齐的对齐"""
    matrix = add_object_align_init(context, operator, align_type=align_type)
    return matrix
```

### 坐标转换

```python
def world_to_local(obj, world_co):
    """世界坐标转局部坐标"""
    return obj.matrix_world.inverted() @ world_co

def local_to_world(obj, local_co):
    """局部坐标转世界坐标"""
    return obj.matrix_world @ local_co

def world_to_object(obj, point):
    """将点转换到对象空间"""
    return obj.matrix_world.inverted() @ point

def object_to_world(point):
    """将点转换到世界空间"""
    return bpy.context.active_object.matrix_world @ point

def transform_point_by_matrix(point, matrix):
    """通过矩阵变换点"""
    return matrix @ point

def transform_vector_by_matrix(vector, matrix):
    """通过矩阵变换向量"""
    return (matrix.to_3x3() @ vector)

def get_object_bounds(obj, apply_modifiers=True):
    """获取对象边界框"""
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    mesh = eval_obj.to_mesh()
    try:
        coords = [v.co for v in mesh.vertices]
        if not coords:
            return None, None
        min_co = Vector(min(c[i] for c in coords) for i in range(3))
        max_co = Vector(max(c[i] for c in coords) for i in range(3))
        return min_co, max_co
    finally:
        eval_obj.to_mesh_clear()
```

## 实用函数

### 对象变换

```python
def set_location(obj, location):
    """设置位置"""
    obj.location = location

def set_rotation(obj, rotation_euler=None, rotation_axis_angle=None):
    """设置旋转"""
    if rotation_euler:
        obj.rotation_euler = rotation_euler
    elif rotation_axis_angle:
        obj.rotation_axis_angle = rotation_axis_angle

def set_scale(obj, scale):
    """设置缩放"""
    obj.scale = scale

def reset_transform(obj):
    """重置变换"""
    obj.location = (0, 0, 0)
    obj.rotation_euler = (0, 0, 0)
    obj.scale = (1, 1, 1)

def apply_transform(obj, location=True, rotation=True, scale=True):
    """应用变换"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.transform_apply(
        location=location, 
        rotation=rotation, 
        scale=scale
    )

def duplicate_object(obj, linked=False):
    """复制对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.duplicate(linked=linked)
    return bpy.context.active_object
```

### 对象选择

```python
def select_object(obj, deselect_others=True):
    """选择对象"""
    if deselect_others:
        bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj

def deselect_all():
    """取消所有选择"""
    bpy.ops.object.select_all(action='DESELECT')

def select_by_type(obj_type):
    """按类型选择"""
    bpy.ops.object.select_all(action='DESELECT')
    for obj in bpy.context.scene.objects:
        if obj.type == obj_type:
            obj.select_set(True)

def select_by_name_pattern(pattern):
    """按名称模式选择"""
    bpy.ops.object.select_all(action='DESELECT')
    for obj in bpy.context.scene.objects:
        if pattern in obj.name:
            obj.select_set(True)

def get_selected_objects():
    """获取选中的对象"""
    return bpy.context.selected_objects

def get_active_object():
    """获取活动对象"""
    return bpy.context.active_object
```

### 对象层级

```python
def get_all_children(obj):
    """获取所有子对象"""
    children = []
    for child in obj.children:
        children.append(child)
        children.extend(get_all_children(child))
    return children

def get_parent_chain(obj):
    """获取父对象链"""
    chain = []
    current = obj
    while current:
        chain.append(current)
        current = current.parent
    return chain

def set_parent(obj, parent, keep_transform=True):
    """设置父对象"""
    if keep_transform:
        bpy.ops.object.select_all(action='DESELECT')
        obj.select_set(True)
        parent.select_set(True)
        bpy.context.view_layer.objects.active = obj
        bpy.ops.object.parent_set(type='OBJECT', keep_transform=True)
    else:
        obj.parent = parent

def clear_parent(obj, keep_transform=True):
    """清除父对象"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.object.parent_clear(type='CLEAR_KEEP_TRANSFORM' if keep_transform else 'CLEAR')
```

## 常见问题排查

### 变换不生效

对象未激活：
- 确保对象被选中
- 设置活动对象
- 检查对象是否在视图中

### 坐标转换错误

矩阵不可逆：
- 检查对象是否有零缩放
- 确认矩阵变换有效
- 使用try-except处理异常

### 对象未找到

名称冲突：
- 使用完整的对象名称
- 检查对象是否存在
- 考虑不同集合中的同名对象


---
## bpy_extras_view3d_utils

# bpy_extras.view3d_utils - 3D视图工具模块

bpy_extras.view3d_utils模块提供了3D视图坐标转换、射线投射和视图投影的实用工具函数。

## 概述

3D视图工具模块封装了3D视图中常用的坐标操作，包括屏幕坐标与3D坐标的相互转换、射线投射、视图向量计算等功能。适用于开发交互式工具和3D视口插件。

## 核心功能

### 坐标转换

```python
import bpy
import bpy_extras
from mathutils import Vector, Matrix
from bpy_extras.view3d_utils import (
    location_3d_to_region_2d,
    location_3d_to_region_2d_multiple,
    region_2d_to_location_3d,
    region_2d_to_origin_3d,
    region_2d_to_vector_3d
)

def world_to_screen(co, region=None, rv3d=None):
    """将3D坐标转换为屏幕坐标"""
    if region is None:
        region = bpy.context.region
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return location_3d_to_region_2d(co, region, rv3d)

def screen_to_world_3d(x, y, depth=0, region=None, rv3d=None):
    """将屏幕坐标转换为3D坐标"""
    if region is None:
        region = bpy.context.region
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return region_2d_to_location_3d(x, y, depth, region, rv3d)

def screen_to_world_origin(x, y, region=None, rv3d=None):
    """将屏幕坐标转换为世界原点射线"""
    if region is None:
        region = bpy.context.region
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    origin = region_2d_to_origin_3d(x, y, region, rv3d)
    direction = region_2d_to_vector_3d(x, y, region, rv3d)
    return origin, direction

def get_viewport_center(region=None, rv3d=None):
    """获取视口中心"""
    if region is None:
        region = bpy.context.region
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    
    center_x = region.width / 2
    center_y = region.height / 2
    return (center_x, center_y)

def convert_3d_point_to_view(point, matrix=None, region=None, rv3d=None):
    """转换3D点到视图空间"""
    if matrix is None:
        matrix = bpy.context.space_data.region_3d.view_matrix
    return matrix @ point
```

### 射线投射

```python
def cast_ray_from_mouse(region=None, rv3d=None, mouse_coords=None):
    """从鼠标位置发射射线"""
    if region is None:
        region = bpy.context.region
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    if mouse_coords is None:
        mouse_coords = bpy.context.region_data.mouse
    
    origin = region_2d_to_origin_3d(
        mouse_coords.x, 
        mouse_coords.y, 
        region, 
        rv3d
    )
    direction = region_2d_to_vector_3d(
        mouse_coords.x, 
        mouse_coords.y, 
        region, 
        rv3d
    )
    
    return origin, direction

def ray_cast_scene(origin, direction, distance=1000):
    """在场景中投射射线"""
    result = bpy.context.scene.ray_cast(
        bpy.context.evaluated_depsgraph_get(),
        origin,
        direction,
        distance=distance
    )
    return result

def ray_cast_objects(objects, origin, direction, distance=1000):
    """在指定对象上投射射线"""
    results = []
    for obj in objects:
        if obj.type != 'MESH':
            continue
        depsgraph = bpy.context.evaluated_depsgraph_get()
        eval_obj = obj.evaluated_get(depsgraph)
        mesh = eval_obj.to_mesh()
        try:
            from mathutils.bvhtree import BVHTree
            bvh = BVHTree.FromPolygons(
                [v.co for v in mesh.vertices],
                [p.vertices for p in mesh.polygons]
            )
            location, normal, index, dist = bvh.ray_cast(origin, direction, distance)
            if location is not None:
                results.append({
                    'object': obj,
                    'location': location,
                    'normal': normal,
                    'face_index': index,
                    'distance': dist
                })
        finally:
            eval_obj.to_mesh_clear()
    return results

def get_point_under_mouse(region=None, rv3d=None, mouse_coords=None):
    """获取鼠标下的点"""
    origin, direction = cast_ray_from_mouse(region, rv3d, mouse_coords)
    hit, location, normal, index, obj, matrix = ray_cast_scene(origin, direction)
    if hit:
        return {
            'location': location,
            'normal': normal,
            'face_index': index,
            'object': obj
        }
    return None
```

### 视图操作

```python
def get_view_distance(region=None, rv3d=None):
    """获取视图距离"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return rv3d.view_distance

def set_view_distance(distance, region=None, rv3d=None):
    """设置视图距离"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    rv3d.view_distance = distance

def get_view_rotation(region=None, rv3d=None):
    """获取视图旋转"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return rv3d.view_rotation

def set_view_rotation(rotation, region=None, rv3d=None):
    """设置视图旋转"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    rv3d.view_rotation = rotation

def get_view_perspective(region=None, rv3d=None):
    """获取视图投影类型"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return rv3d.view_perspective

def set_view_perspective(perspective, region=None, rv3d=None):
    """设置视图投影类型"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    rv3d.view_perspective = perspective
```

## 实用函数

### 视口坐标

```python
def get_viewport_bounds(region=None, rv3d=None):
    """获取视口边界"""
    if region is None:
        region = bpy.context.region
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    
    corners = [
        Vector((0, 0, 0)),
        Vector((region.width, 0, 0)),
        Vector((region.width, region.height, 0)),
        Vector((0, region.height, 0))
    ]
    
    world_corners = []
    view_matrix_inv = rv3d.view_matrix.inverted()
    for corner in corners:
        screen_co = location_3d_to_region_2d(corner, region, rv3d)
        world_corners.append(screen_co)
    
    return world_corners

def point_in_viewport(point, region=None, rv3d=None):
    """检查点是否在视口内"""
    screen = world_to_screen(point, region, rv3d)
    if screen is None:
        return False
    
    if region is None:
        region = bpy.context.region
    
    return (0 <= screen.x <= region.width and 
            0 <= screen.y <= region.height)

def get_mouse_distance_from_view_center(mouse_coords, region=None):
    """计算鼠标到视图中心的距离"""
    if region is None:
        region = bpy.context.region
    center_x, center_y = get_viewport_center(region)
    dx = mouse_coords.x - center_x
    dy = mouse_coords.y - center_y
    return (dx * dx + dy * dy) ** 0.5
```

### 视图对齐

```python
def align_view_to_object(obj, align_axis='Z', region=None, rv3d=None):
    """对齐视图到对象"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    
    bpy.ops.view3d.view_axis(
        type=align_axis,
        align_active=True
    )

def center_view_on_object(obj, region=None, rv3d=None):
    """将视图中心对准对象"""
    bpy.ops.view3d.view_selected(use_all_regions=False)

def view_from_direction(direction, region=None, rv3d=None):
    """从指定方向查看"""
    rot_quat = direction.to_track_quat('-Z', 'Y').to_euler()
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    rv3d.view_rotation = rot_quat.to_quaternion()

def set_orbit_target(point, region=None, rv3d=None):
    """设置旋转目标点"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    bpy.ops.view3d.view_orbit(pivot_point=point)
```

### 投影计算

```python
def project_world_to_view_matrix(region=None, rv3d=None):
    """获取世界到视图的投影矩阵"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return rv3d.view_matrix

def get_projection_matrix(region=None, rv3d=None):
    """获取投影矩阵"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return rv3d.perspective_matrix

def get_view_matrix_inverted(region=None, rv3d=None):
    """获取视图矩阵的逆矩阵"""
    if rv3d is None:
        rv3d = bpy.context.space_data.region_3d
    return rv3d.view_matrix.inverted()

def calculate_screen_aspect(region=None):
    """计算屏幕宽高比"""
    if region is None:
        region = bpy.context.region
    return region.width / region.height
```

## 常见问题排查

### 坐标转换失败

视口数据无效：
- 确保在3D视图中运行
- 检查region和rv3d是否有效
- 确认视图已正确初始化

### 射线投射无结果

距离或方向问题：
- 增加射线投射距离
- 检查射线方向是否正确
- 确认对象在场景中可见

### 视图操作无响应

视口类型不支持：
- 确认当前是3D视图
- 检查视图是否被锁定
- 尝试切换视图类型


