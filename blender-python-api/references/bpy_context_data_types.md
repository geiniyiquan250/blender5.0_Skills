# bpy_context_data_types

---
## bpy

# bpy Module Reference

Complete reference for the main Blender Python API module.

## bpy.context

Context-aware access to Blender data.

### Properties

- `active_object` - Currently selected object
- `selected_objects` - All selected objects
- `scene` - Current scene
- `view_layer` - Current view layer
- `window` - Current window
- `screen` - Current screen
- `area` - Current area
- `region` - Current region
- `space_data` - Current space data
- `region_data` - Current region data
- `mode` - Current mode (OBJECT', EDIT', SCULPT', etc.)

### Methods

- `copy()` - Create a copy of context
- `temp_override()` - Create temporary override context

### Context Usage Examples

```python
import bpy

# 获取当前上下文
context = bpy.context

# 获取活动对象
active_obj = context.active_object

# 获取所有选中对象
selected_objs = context.selected_objects

# 获取当前场景
current_scene = context.scene

# 创建临时上下文覆盖
with context.temp_override(selected_objects=[obj1, obj2]):
    # 在此上下文中执行操作
    bpy.ops.object.select_all(action='SELECT')
```

## bpy.data

Access to all blend file data.

### Collections

- `bpy.data.objects` - All objects
- `bpy.data.meshes` - All meshes
- `bpy.data.materials` - All materials
- `bpy.data.textures` - All textures
- `bpy.data.images` - All images
- `bpy.data.lamps` - All lamps (deprecated, use lights)
- `bpy.data.lights` - All lights
- `bpy.data.cameras` - All cameras
- `bpy.data.scenes` - All scenes
- `bpy.data.worlds` - All worlds
- `bpy.data.curves` - All curves
- `bpy.data.surfaces` - All surfaces
- `bpy.data.metaballs` - All metaballs
- `bpy.data.lattices` - All lattices
- `bpy.data.armatures` - All armatures
- `bpy.data.actions` - All actions
- `bpy.data.speakers` - All speakers
- `bpy.data.sounds` - All sounds
- `bpy.data.particles` - All particle systems
- `bpy.data.node_groups` - All node groups
- `bpy.data.texts` - All text blocks
- `bpy.data.collections` - All collections
- `bpy.data.movieclips` - All movie clips
- `bpy.data.masks` - All masks
- `bpy.data.grease_pencils` - All grease pencils
- `bpy.data.palettes` - All color palettes
- `bpy.data.workspaces` - All workspaces
- `bpy.data.screens` - All screens
- `bpy.data.window_managers` - All window managers
- `bpy.data.libraries` - All linked libraries
- `bpy.data.fonts` - All fonts
- `bpy.data.cache_files` - All cache files
- `bpy.data.hair_curves` - All hair curves
- `bpy.data.pointclouds` - All point clouds
- `bpy.data.volumes` - All volumes
- `bpy.data.paint_curves` - All paint curves
- `bpy.data.line_styles` - All line styles
- `bpy.data.brushes` - All brushes
- `bpy.data.node_trees` - All node trees
- `bpy.data.probes` - All probes

### Methods

- `new()` - Create new data block
- `remove()` - Remove data block
- `user_map()` - Get user map of data blocks

### Data Usage Examples

```python
import bpy

# 创建新网格
mesh = bpy.data.meshes.new("MyMesh")

# 创建新对象
obj = bpy.data.objects.new("MyObject", mesh)

# 将对象添加到场景
bpy.context.scene.collection.objects.link(obj)

# 遍历所有对象
for obj in bpy.data.objects:
    print(f"Object: {obj.name}")

# 删除对象
bpy.data.objects.remove(obj)

# 获取用户映射
user_map = bpy.data.user_map()

# 批量操作数据块
for mesh in bpy.data.meshes:
    if mesh.users == 0:
        bpy.data.meshes.remove(mesh)
```

## bpy.ops

Operators for executing Blender commands.

### Usage Pattern

```python
bpy.ops.category.operator_name(
    parameter1=value1,
    parameter2=value2
)
```

### Return Values

- `{'FINISHED'}` - Operation completed successfully
- `{'CANCELLED'}` - Operation was cancelled
- `{'RUNNING_MODAL'}` - Modal operator is running

### Common Parameters

- `override_context` - Override context for operation
- `confirm` - Confirm destructive operations
- `skip_save` - Skip saving before operation

### Operator Categories

#### Object Operations
```python
# 创建立方体
bpy.ops.mesh.primitive_cube_add(location=(0, 0, 0))

# 选择对象
bpy.ops.object.select_all(action='SELECT')

# 删除对象
bpy.ops.object.delete(use_global=False, confirm=False)

# 复制对象
bpy.ops.object.duplicate_move()

# 变换对象
bpy.ops.transform.translate(value=(1, 0, 0))
```

#### Mesh Operations
```python
# 进入编辑模式
bpy.ops.object.mode_set(mode='EDIT')

# 挤出顶点
bpy.ops.mesh.extrude_region_move()

# 细分网格
bpy.ops.mesh.subdivide(number_cuts=2)

# 合并顶点
bpy.ops.mesh.merge(type='CENTER')

# 退出编辑模式
bpy.ops.object.mode_set(mode='OBJECT')
```

#### Scene Operations
```python
# 设置当前帧
bpy.context.scene.frame_set(10)

# 插入关键帧
bpy.ops.anim.keyframe_insert(type='Location')

# 渲染场景
bpy.ops.render.render(write_still=True)

# 保存文件
bpy.ops.wm.save_as_mainfile(filepath="/path/to/file.blend")
```

## bpy.types

Type definitions for all Blender data.

### Common Base Classes

- `bpy.types.ID` - Base class for all data blocks
- `bpy.types.Struct` - Base class for RNA structs
- `bpy.types.Property` - Base class for properties

### Key Properties

All ID types have these properties:
- `name` - Name of the data block
- `users` - Number of users
- `use_fake_user` - Fake user flag
- `tag` - Tag for selection
- `library` - Library reference
- `library_weak_reference` - Weak library reference

## bpy.props

Property definitions for creating custom properties.

### Property Types

#### BoolProperty

```python
bpy.props.BoolProperty(
    name="My Bool",
    description="Description",
    default=False,
    options={'HIDDEN', 'SKIP_SAVE'}
)
```

#### IntProperty

```python
bpy.props.IntProperty(
    name="My Int",
    description="Description",
    default=0,
    min=0,
    max=100,
    soft_min=0,
    soft_max=100,
    step=1,
    options={'ANIMATABLE', 'PROPORTIONAL'}
)
```

#### FloatProperty

```python
bpy.props.FloatProperty(
    name="My Float",
    description="Description",
    default=0.0,
    min=0.0,
    max=1.0,
    precision=3,
    subtype='PERCENTAGE',
    unit='NONE',
    options={'ANIMATABLE', 'PROPORTIONAL'}
)
```

#### StringProperty

```python
bpy.props.StringProperty(
    name="My String",
    description="Description",
    default="",
    maxlen=1024,
    subtype='FILE_PATH',
    options={'HIDDEN', 'TEXTEDIT_UPDATE'}
)
```

#### EnumProperty

```python
bpy.props.EnumProperty(
    name="My Enum",
    description="Description",
    items=[
        ('A', "Option A", "Description A"),
        ('B', "Option B", "Description B"),
        ('C', "Option C", "Description C"),
    ],
    default='A',
    options={'ENUM_FLAG'}
)
```

#### CollectionProperty

```python
bpy.props.CollectionProperty(
    type=MyCustomType,
    name="My Collection",
    description="Collection of items"
)
```

#### PointerProperty

```python
bpy.props.PointerProperty(
    type=bpy.types.Object,
    name="Target Object",
    description="Reference to another object"
)
```

### Property Options

- `'HIDDEN'` - Hide from UI
- `'SKIP_SAVE'` - Don't save to file
- `'ANIMATABLE'` - Can be animated
- `'LIBRARY_EDITABLE'` - Editable in linked libraries
- `'PROPORTIONAL'` - Proportional editing
- `'TEXTEDIT_UPDATE'` - Update on text edit
- `'ENUM_FLAG'` - Multiple selection for enums

### Property Subtypes

- `'PIXEL'` - Pixel values
- `'UNSIGNED'` - Unsigned integers
- `'PERCENTAGE'` - Percentage values
- `'FACTOR'` - Factor values (0.0-1.0)
- `'ANGLE'` - Angle values
- `'TIME'` - Time values
- `'DISTANCE'` - Distance values
- `'FILE_PATH'` - File paths
- `'DIR_PATH'` - Directory paths
- `'FILE_NAME'` - File names
- `'BYTE_STRING'` - Byte strings

### Property Units

- `'NONE'` - No unit
- `'LENGTH'` - Length unit
- `'AREA'` - Area unit
- `'VOLUME'` - Volume unit
- `'ROTATION'` - Rotation unit
- `'TIME'` - Time unit
- `'VELOCITY'` - Velocity unit
- `'ACCELERATION'` - Acceleration unit
- `'MASS'` - Mass unit
- `'CAMERA'` - Camera unit

### Property Usage Example

```python
import bpy

class MyProperties(bpy.types.PropertyGroup):
    my_bool: bpy.props.BoolProperty(
        name="Enable Feature",
        description="Enable this feature",
        default=True
    )
    
    my_int: bpy.props.IntProperty(
        name="Count",
        description="Number of items",
        default=5,
        min=1,
        max=100
    )
    
    my_enum: bpy.props.EnumProperty(
        name="Mode",
        items=[
            ('A', "Mode A", "First mode"),
            ('B', "Mode B", "Second mode"),
            ('C', "Mode C", "Third mode")
        ],
        default='A'
    )

# 注册属性组
bpy.utils.register_class(MyProperties)

# 添加到场景
bpy.types.Scene.my_props = bpy.props.PointerProperty(type=MyProperties)

# 使用属性
scene = bpy.context.scene
scene.my_props.my_bool = True
scene.my_props.my_int = 10
scene.my_props.my_enum = 'B'
)
```

#### CollectionProperty

```python
bpy.props.CollectionProperty(
    name="My Collection",
    description="Description",
    type=bpy.types.PropertyGroup
)
```

#### PointerProperty

```python
bpy.props.PointerProperty(
    name="My Pointer",
    description="Description",
    type=bpy.types.Object,
    poll=lambda self, obj: obj and obj.type == 'MESH'
)
```

#### FloatVectorProperty

```python
bpy.props.FloatVectorProperty(
    name="My Vector",
    description="Description",
    default=(0.0, 0.0, 0.0),
    size=3,
    min=0.0,
    max=1.0,
    subtype='COLOR',
    options={'ANIMATABLE', 'PROPORTIONAL'}
)
```

#### FloatColorProperty

```python
bpy.props.FloatColorProperty(
    name="My Color",
    description="Description",
    default=(0.8, 0.8, 0.8, 1.0),
    min=0.0,
    max=1.0,
    subtype='COLOR_GAMMA',
    options={'ANIMATABLE', 'PROPORTIONAL'}
)
```

### Property Options

- `HIDDEN` - Property is hidden in UI
- `SKIP_SAVE` - Property is not saved
- `ANIMATABLE` - Property can be animated
- `LIBRARY_EDITABLE` - Property can be edited in library
- `PROPORTIONAL` - Property is proportional
- `TEXTEDIT_UPDATE` - Update on text edit
- `ENUM_FLAG` - Enum is a flag set

### Property Subtypes

#### String Subtypes

- `FILE_PATH` - File path
- `DIR_PATH` - Directory path
- `FILE_NAME` - File name only
- `BYTE_STRING` - Byte string
- `PASSWORD` - Password field
- `NONE` - No subtype

#### Float Subtypes

- `PIXEL` - Pixel value
- `UNSIGNED` - Unsigned value
- `PERCENTAGE` - Percentage value
- `FACTOR` - Factor value (0-1)
- `ANGLE` - Angle value
- `TIME` - Time value
- `DISTANCE` - Distance value
- `NONE` - No subtype

#### FloatVector Subtypes

- `XYZ` - XYZ vector
- `COLOR` - RGBA color
- `COLOR_GAMMA` - RGBA color with gamma
- `TRANSLATION` - Translation vector
- `DIRECTION` - Direction vector
- `VELOCITY` - Velocity vector
- `ACCELERATION` - Acceleration vector
- `EULER` - Euler rotation
- `QUATERNION` - Quaternion rotation
- `NONE` - No subtype

## bpy.path

File path utilities for Blender.

### Functions

#### abspath

```python
path = bpy.path.abspath(path)
```

Get absolute path from relative path.

#### basename

```python
name = bpy.path.basename(path)
```

Get basename from path.

#### dirname

```python
dir = bpy.path.dirname(path)
```

Get directory name from path.

#### display_name

```python
name = bpy.path.display_name(path)
```

Get display name from filepath.

#### display_name_to_filepath

```python
path = bpy.path.display_name_to_filepath(name)
```

Get filepath from display name.

#### ensure_ext

```python
path = bpy.path.ensure_ext(filepath, ext)
```

Ensure file has extension.

#### is_subdir

```python
is_sub = bpy.path.is_subdir(directory, path)
```

Check if path is subdirectory.

#### native_pathsep

```python
path = bpy.path.native_pathsep(path)
```

Get path with native path separator.

#### resolve_ncase

```python
path = bpy.path.resolve_ncase(path)
```

Resolve path case-insensitively.

## bpy.app

Application information and utilities.

### Properties

- `version` - Blender version tuple
- `version_string` - Blender version string
- `binary_path` - Blender binary path
- `background` - Background mode flag
- `tempdir` - Temporary directory
- `translations` - Translations module

### Methods

- `timers.register()` - Register timer
- `timers.unregister()` - Unregister timer

## bpy.msgbus

Message bus for inter-module communication.

### Functions

#### clear_by_owner

```python
bpy.msgbus.clear_by_owner(owner)
```

Clear all messages by owner.

#### publish_rna

```python
bpy.msgbus.publish_rna(key, value)
```

Publish RNA message.

#### subscribe_rna

```python
bpy.msgbus.subscribe_rna(
    key,
    owner,
    args,
    notify
)
```

Subscribe to RNA message.

## Registration

### Register Functions

```python
import bpy

def register():
    bpy.utils.register_class(MyClass)
    bpy.types.VIEW3D_MT_mesh_add.append(menu_func)

def unregister():
    bpy.types.VIEW3D_MT_mesh_add.remove(menu_func)
    bpy.utils.unregister_class(MyClass)

if __name__ == "__main__":
    register()
```

### Class Registration

```python
bpy.utils.register_class(MyClass)
```

Register a single class.

```python
bpy.utils.register_classes([Class1, Class2, Class3])
```

Register multiple classes.

### Unregistration

```python
bpy.utils.unregister_class(MyClass)
```

Unregister a single class.

```python
bpy.utils.unregister_classes([Class1, Class2, Class3])
```

Unregister multiple classes.

## Utilities

### bpy.utils

#### import_test

```python
if bpy.utils.import_test("numpy"):
    import numpy
```

Test if module can be imported.

#### register_class

```python
bpy.utils.register_class(MyClass)
```

Register a class.

#### unregister_class

```python
bpy.utils.unregister_class(MyClass)
```

Unregister a class.

#### register_manual_map

```python
bpy.utils.register_manual_map("my_operator_id", "path/to/manual.html")
```

Register manual map entry.

#### unregister_manual_map

```python
bpy.utils.unregister_manual_map("my_operator_id")
```

Unregister manual map entry.

#### smpte_from_frame

```python
timecode = bpy.utils.smpte_from_frame(frame, fps=None, fps_base=None)
```

Convert frame to SMPTE timecode.

#### smpte_to_frame

```python
frame = bpy.utils.smpte_to_frame(timecode_string, fps=None, fps_base=None)
```

Convert SMPTE timecode to frame.

### bpy_extras

#### node_utils

```python
from bpy_extras import node_utils

node_utils.find_node_input(node, identifier)
node_utils.find_node_output(node, identifier)
node_utils.socket_id_to_index(node, identifier, is_input)
node_utils.socket_index_to_id(node, index, is_input)
```

Node utility functions.

#### object_utils

```python
from bpy_extras import object_utils

object_utils.object_data_group(context)
object_utils.add_object_align_init(context, operator)
object_utils.world_to_camera_view(context, operator)
```

Object utility functions.

#### mesh_utils

```python
from bpy_extras import mesh_utils

mesh_utils.mesh_linked_uv_islands(mesh)
mesh_utils.mesh_linked_uv_islands_to_mesh(mesh)
```

Mesh utility functions.

#### image_utils

```python
from bpy_extras import image_utils

image_utils.load_image(image_path)
image_utils.new_image(name, width, height, alpha=False, float_buffer=False)
```

Image utility functions.

#### asset_utils

```python
from bpy_extras import asset_utils

asset_utils.SpaceAssetInfo
asset_utils.activate_asset_by_id(asset_identifier, library_id=None, deferred=False)
asset_utils.asset_mark(asset_identifier, library_id=None)
```

Asset utility functions.


---
## bpy_context

# bpy.context 模块

## 模块概述

bpy.context 模块提供对 Blender 当前上下文的访问接口，通过该模块可以获取当前用户界面状态、选中的对象、活动元素以及各种编辑状态。该模块是编写交互式脚本和操作符的基础，几乎所有需要与用户操作交互的代码都需要通过它来获取当前状态。上下文信息是动态的，会根据用户的选择和操作实时变化，因此在编写插件和脚本时需要特别注意上下文的使用时机。

上下文对象是一个特殊的字典-like 对象，它聚合了多个层面的信息。顶层属性提供最常用的对象和数据的快速访问，如 scene、object、active_object 等核心属性。区域特定属性则根据当前激活的区域返回对应的数据，如 view_layer、region、area、space_data 等。这些属性在不同类型的编辑器窗口中可能返回不同的值，例如在 3D 视图中 space_data 返回 SpaceView3D，而在图像编辑器中返回 SpaceImageEditor。

在多窗口和多屏幕的工作环境中，同一个 Blender 实例可能存在多个上下文窗口。bpy.context 总是返回当前执行上下文窗口的状态，这与 bpy.data 的全局数据不同，后者包含的是整个 .blend 文件的数据。因此，当编写需要在多个窗口中正确运行的代码时，需要特别注意上下文的动态特性。

## 核心属性

### 活动对象属性

活动对象属性提供对当前编辑焦点对象的快速访问，这些属性是编写操作脚本时最常使用的信息来源。active_object 属性返回当前活动的物体，该物体通常带有操作焦点，如变换操作的目标。object 属性是 active_object 的别名，两者在功能上完全相同。active_object_base 属性返回活动物体所在的集合基元，这在处理复杂场景层次时特别有用。

在物体模式下，active_object 返回当前选中的单个物体。在编辑模式下，active_object 返回正在编辑的物体，而 active_editable_object 则返回当前可编辑的物体，这在多选编辑时尤为重要。selected_objects 列表包含所有当前选中的物体，无论这些物体处于什么模式。selected_editable_objects 列表则只包含可编辑的选中物体，过滤掉被保护或锁定的物体。

```python
import bpy

# 获取当前活动物体
obj = bpy.context.active_object
if obj:
    print(f"活动物体: {obj.name}")
    print(f"物体类型: {obj.type}")
    print(f"物体位置: {obj.location}")

# 获取所有选中物体
for obj in bpy.context.selected_objects:
    print(f"选中物体: {obj.name}")

# 检查当前是否处于编辑模式
if bpy.context.active_object and bpy.context.active_object.mode == 'EDIT':
    print("当前处于编辑模式")
    # 获取编辑选中的顶点
    edit_obj = bpy.context.active_object
    mesh = edit_obj.data
    if mesh.total_vert_sel > 0:
        print(f"选中了 {mesh.total_vert_sel} 个顶点")
```

### 场景相关属性

scene 属性返回当前场景对象，这是访问场景级别数据的入口点。通过 scene 属性可以访问场景中的所有物体、集合、相机、灯光等元素。view_layer 属性返回当前视图层，视图层用于组织场景中的物体可见性和渲染设置。在 Blender 2.8 之后的版本中，view_layer 取代了原来的 layer 属性，提供了更灵活的层管理功能。

window 属性返回当前窗口管理器对象，screen 属性返回当前屏幕对象。area 属性返回当前激活的区域，region 属性返回当前区域中的特定区域（如 3D 视口中的面板区域）。这些属性在编写需要精确定位界面元素的脚本时非常有用。

```python
import bpy

# 获取当前场景
scene = bpy.context.scene

# 访问场景物体
print(f"场景中的物体数量: {len(scene.objects)}")

# 获取当前视图层
view_layer = bpy.context.view_layer

# 获取当前选中的集合
active_layer_collection = bpy.context.layer_collection
print(f"当前集合: {active_layer_collection.name}")

# 访问当前屏幕和区域
screen = bpy.context.screen
area = bpy.context.area
print(f"当前区域类型: {area.type if area else '无'}")

# 遍历屏幕区域
for region in screen.areas[0].regions:
    if region.type == 'WINDOW':
        print(f"窗口区域: {region.type}")
```

### 编辑器区域属性

space_data 属性返回当前活动空间的数据，具体类型取决于当前激活的区域类型。在 3D 视图中，它返回 SpaceView3D 对象，包含视图视角、渲染属性等信息。在图像编辑器中，它返回 SpaceImageEditor 对象，包含图像显示和编辑相关的信息。在节点编辑器中，它返回 SpaceNodeEditor 对象，包含节点树和合成设置的信息。

region_3d 属性返回当前 3D 区域的数据，这是一个 RegionView3D 对象，包含视图的相机变换、透视/正交设置等信息。当需要在 3D 视图中进行坐标计算或视图操作时，这个属性是必不可少的。

```python
import bpy
from mathutils import Vector

# 获取 3D 视图信息
space = bpy.context.space_data
if space and hasattr(space, 'region_3d'):
    r3d = space.region_3d
    print(f"视图类型: {'透视' if r3d.is_perspective else '正交'}")
    print(f"视图旋转: {r3d.view_rotation}")
    print(f"视图距离: {r3d.view_distance}")

    # 获取视图矩阵
    view_matrix = r3d.view_matrix
    print(f"视图矩阵:\n{view_matrix}")

    # 将世界坐标转换为视图坐标
    world_point = Vector((1.0, 2.0, 3.0))
    view_point = world_point @ r3d.view_matrix.inverted()
    print(f"世界点 {world_point} 在视图中的位置: {view_point}")
```

### 工具设置属性

tool_settings 属性返回当前工具的设置，这是一个 ToolSettings 对象，包含各种编辑工具的共享设置。不同的编辑模式有不同的工具设置，如编辑顶点时的吸附设置、变换时的变换轴心点设置等。这些设置可以用于在脚本中模拟用户的工具操作行为。

```python
import bpy

# 获取工具设置
tool_settings = bpy.context.tool_settings

# 获取变换设置
transform = tool_settings.transform_pivot_point
print(f"变换轴心点: {transform}")

# 获取吸附设置
snap = tool_settings.use_snap
print(f"吸附启用: {snap}")
if snap:
    snap_target = tool_settings.snap_target
    print(f"吸附目标: {snap_target}")

# 获取雕刻设置（如果处于雕刻模式）
if bpy.context.mode.startswith('SCULPT'):
    sculpt = tool_settings.sculpt
    print(f"雕刻工具: {sculpt.current_tool}")
    print(f"雕刻笔刷大小: {sculpt.brush.size}")
```

### 偏好设置属性

preferences 属性返回用户偏好设置对象，这是一个 PreferencesPreferences 对象，包含 Blender 的各种用户配置。这些设置影响整个应用程序的行为，包括主题颜色、快捷键、默认文件路径等。在编写需要访问用户配置的脚本时，可以通过这个属性获取相关设置。

```python
import bpy

# 获取偏好设置
prefs = bpy.context.preferences

# 获取文件路径设置
file_paths = prefs.filepaths
print(f"默认保存目录: {file_paths.default_directory}")
print(f"最近文件数量: {file_paths.recent_files}")

# 获取主题设置
theme = prefs.themes['Default']
print(f"主题背景颜色: {theme.view_3d.space.colors.back}")

# 获取编辑偏好
edits = prefs.editPreferences
print(f"撤销步数: {edits.undo_steps}")
print(f"自动保存间隔: {edits.auto_save_time}")
```

## 上下文方法

### 复制属性方法

copy 方法用于复制当前上下文中的属性字典，返回一个包含所有上下文属性的字典。这个方法在需要保存和恢复上下文状态时非常有用，例如在操作前后保存当前选择状态和活动物体，操作完成后再恢复。

```python
import bpy

# 保存当前上下文状态
saved_context = bpy.context.copy()

# 执行一些操作
bpy.ops.object.select_all(action='SELECT')

# 恢复上下文状态
for key, value in saved_context.items():
    try:
        bpy.context[key] = value
    except (KeyError, TypeError):
        pass  # 某些属性可能无法设置，忽略错误

print("上下文状态已恢复")
```

### 深度复制属性方法

deep_copy 方法是 copy 方法的深拷贝版本，它会递归复制所有嵌套的对象。这个方法比 copy 更消耗资源，但在需要完整隔离原始数据的情况下是必要的。例如，当需要修改上下文中的对象而不影响原始状态时，应该使用 deep_copy。

```python
import bpy

# 使用深度复制保存完整上下文
saved_context = bpy.context.deep_copy()

# 修改活动物体的位置
if bpy.context.active_object:
    bpy.context.active_object.location.x += 1.0

# 深度复制的内容不会受到修改影响
original_obj = saved_context['active_object']
print(f"原始物体位置: {original_obj.location.x}")
print(f"当前物体位置: {bpy.context.active_object.location.x}")
```

## 常用上下文模式

### 物体模式

物体模式是 Blender 的默认编辑模式，在此模式下可以对整个物体进行操作，包括选择、移动、旋转、缩放等变换操作。bpy.context.mode 返回 'OBJECT' 字符串。selected_objects 列表包含所有选中的物体，active_object 是活动物体。

```python
import bpy

def process_object_mode():
    """处理物体模式下的操作"""
    if bpy.context.mode != 'OBJECT':
        print("不在物体模式下")
        return

    # 获取选中的网格物体
    mesh_objects = [obj for obj in bpy.context.selected_objects
                   if obj.type == 'MESH']

    print(f"选中的网格物体数量: {len(mesh_objects)}")

    for obj in mesh_objects:
        # 计算物体包围盒中心
        if obj.bound_box:
            center = sum((Vector(b) for b in obj.bound_box), Vector((0, 0, 0))) / 8
            world_center = obj.matrix_world @ center
            print(f"物体 {obj.name} 的世界中心: {world_center}")

    # 设置活动物体
    if mesh_objects:
        bpy.context.view_layer.objects.active = mesh_objects[0]
        print(f"设置 {mesh_objects[0].name} 为活动物体")
```

### 编辑模式

编辑模式允许直接编辑物体的几何数据，根据编辑对象类型的不同，可以进一步分为顶点编辑、边编辑和面编辑三种子模式。bpy.context.mode 返回 'EDIT'，而 bpy.context.tool_settings.mesh_select_mode 返回一个布尔元组，表示当前启用的选择模式（顶点、边、面）。

在编辑模式下，active_object 返回正在编辑的物体，edit_object 是其别名。selected_editable_objects 返回当前可编辑的选中物体列表。edit_object.data 返回物体的网格数据，包含所有几何信息。

```python
import bpy

def process_edit_mode():
    """处理编辑模式下的操作"""
    if bpy.context.mode != 'EDIT':
        print("不在编辑模式下")
        return

    obj = bpy.context.active_object
    mesh = obj.data

    # 检查当前选择模式
    select_mode = bpy.context.tool_settings.mesh_select_mode
    print(f"选择模式 - 顶点: {select_mode[0]}, 边: {select_mode[1]}, 面: {select_mode[2]}")

    if select_mode[0]:  # 顶点选择模式
        selected_verts = [v for v in mesh.vertices if v.select]
        print(f"选中的顶点数量: {len(selected_verts)}")

        # 计算选中顶点的平均位置
        if selected_verts:
            avg_pos = sum((v.co for v in selected_verts), Vector((0, 0, 0))) / len(selected_verts)
            print(f"选中顶点的平均位置: {avg_pos}")

    if select_mode[2]:  # 面选择模式
        selected_faces = [p for p in mesh.polygons if p.select]
        print(f"选中的面数量: {len(selected_faces)}")

        # 计算选中面的总面积
        total_area = sum(p.area for p in selected_faces)
        print(f"选中面的总面积: {total_area:.2f}")
```

### 雕刻模式

雕刻模式提供数字雕刻工具，用于对网格进行细节雕刻。bpy.context.mode 返回 'SCULPT'。sculpt 属性提供对雕刻工具设置的访问，包括当前工具、笔刷、大小、强度等参数。

```python
import bpy

def process_sculpt_mode():
    """处理雕刻模式下的操作"""
    if not bpy.context.mode.startswith('SCULPT'):
        print("不在雕刻模式下")
        return

    sculpt = bpy.context.tool_settings.sculpt

    # 获取当前雕刻工具信息
    print(f"当前工具: {sculpt.current_tool}")
    print(f"当前笔刷: {sculpt.brush.name}")

    # 获取笔刷设置
    brush = sculpt.brush
    print(f"笔刷大小: {brush.size}")
    print(f"笔刷强度: {brush.strength}")
    print(f"笔刷角度: {brush.angle}")

    # 获取雕刻模式设置
    print(f"镜像雕刻: {sculpt.use_symmetry}")
    print(f"Dyntopo 启用: {sculpt.dyntopo.detail}")
```

### 顶点绘画模式

顶点绘画模式用于直接对网格顶点应用颜色，bpy.context.mode 返回 'PAINT_VERTEX'。vertex_paint 属性提供对顶点绘画工具设置的访问。

```python
import bpy

def process_vertex_paint_mode():
    """处理顶点绘画模式下的操作"""
    if bpy.context.mode != 'PAINT_VERTEX':
        print("不在顶点绘画模式下")
        return

    vp = bpy.context.tool_settings.vertex_paint

    # 获取顶点绘画设置
    print(f"当前笔刷: {vp.brush.name}")
    print(f"混合模式: {vp.brush.blend}")

    # 获取当前颜色
    color = vp.brush.color
    print(f"当前颜色: RGB({color[0]:.2f}, {color[1]:.2f}, {color[2]:.2f})")
```

### 纹理绘画模式

纹理绘画模式允许直接在网格表面绘制纹理，bpy.context.mode 返回 'PAINT_TEXTURE'。image_paint 属性提供对纹理绘画工具的访问，包括笔刷、蒙版、图像等设置。

```python
import bpy

def process_texture_paint_mode():
    """处理纹理绘画模式下的操作"""
    if bpy.context.mode != 'PAINT_TEXTURE':
        print("不在纹理绘画模式下")
        return

    ip = bpy.context.tool_settings.image_paint

    # 获取纹理绘画设置
    print(f"当前笔刷: {ip.brush.name}")
    print(f"镜像绘画: {ip.brush.use_rake}")
    print(f"角度锁定: {ip.brush.use_angle}")

    # 获取当前图像
    if ip.canvas:
        print(f"当前画布图像: {ip.canvas.name}")
        print(f"图像尺寸: {ip.canvas.size[0]} x {ip.canvas.size[1]}")
```

### 权重绘画模式

权重绘画模式用于为网格顶点分配顶点组权重，bpy.context.mode 返回 'PAINT_WEIGHT'。weight_paint 属性提供对权重绘画工具的访问。

```python
import bpy

def process_weight_paint_mode():
    """处理权重绘画模式下的操作"""
    if bpy.context.mode != 'PAINT_WEIGHT':
        print("不在权重绘画模式下")
        return

    wp = bpy.context.tool_settings.weight_paint

    # 获取权重绘画设置
    print(f"当前笔刷: {wp.brush.name}")
    print(f"权重: {wp.brush.weight}")

    # 获取活动顶点组
    obj = bpy.context.active_object
    if obj.vertex_groups.active:
        print(f"活动顶点组: {obj.vertex_groups.active.name}")
```

## 上下文与数据访问

### 实时获取选中状态

在编写操作脚本时，经常需要实时获取当前的选中状态。由于上下文是动态变化的，每次访问 selected_objects 等属性都会返回最新的状态。因此，在执行操作前应该先获取需要的信息，而不是依赖之前保存的状态。

```python
import bpy

def get_selected_mesh_info():
    """获取当前选中的网格物体信息"""
    selected_meshes = []

    for obj in bpy.context.selected_objects:
        if obj.type == 'MESH':
            mesh_info = {
                'name': obj.name,
                'location': obj.location.copy(),
                'rotation': obj.rotation_euler.copy(),
                'scale': obj.scale.copy(),
                'vertex_count': len(obj.data.vertices),
                'edge_count': len(obj.data.edges),
                'face_count': len(obj.data.polygons)
            }
            selected_meshes.append(mesh_info)

    return selected_meshes

# 使用示例
infos = get_selected_mesh_info()
for info in infos:
    print(f"物体: {info['name']}")
    print(f"  顶点数: {info['vertex_count']}")
    print(f"  面数: {info['face_count']}")
```

### 获取活动元素

活动元素包括活动物体、活动材质、活动纹理、活动图像等。这些元素通常用于确定操作的目标，例如在材质槽中设置材质时，操作的是活动材质槽中的材质。

```python
import bpy

def get_active_elements():
    """获取当前活动的各种元素"""
    elements = {}

    # 活动物体
    elements['object'] = bpy.context.active_object

    # 活动材质
    if elements['object'] and elements['object'].active_material:
        elements['material'] = elements['object'].active_material

    # 活动纹理（如果存在）
    if elements.get('material') and elements['material'].active_texture:
        elements['texture'] = elements['material'].active_texture

    # 活动图像
    if 'texture' in elements and elements['texture'].type == 'IMAGE':
        elements['image'] = elements['texture'].image

    # 活动节点（如果在节点编辑器中）
    if bpy.context.area and bpy.context.area.type == 'NODE_EDITOR':
        space = bpy.context.space_data
        if space.edit_tree:
            elements['node_tree'] = space.edit_tree

    return elements

# 使用示例
elements = get_active_elements()
for key, value in if value:
        elements.items():
    print(f"活动{key}: {value.name if hasattr(value, 'name') else value}")
```

### 上下文切换

虽然 Blender 的操作符通常会自动切换到适当的上下文，但有时需要手动切换上下文模式。bpy.ops.object.mode_set 操作符可以用于切换模式。

```python
import bpy

def switch_to_edit_mode():
    """切换到编辑模式"""
    bpy.ops.object.mode_set(mode='EDIT')
    print("已切换到编辑模式")

def switch_to_object_mode():
    """切换到物体模式"""
    bpy.ops.object.mode_set(mode='OBJECT')
    print("已切换到物体模式")

def switch_to_sculpt_mode():
    """切换到雕刻模式"""
    bpy.ops.object.mode_set(mode='SCULPT')
    print("已切换到雕刻模式")

def ensure_object_mode(func):
    """确保在物体模式下执行函数的装饰器"""
    def wrapper(*args, **kwargs):
        if bpy.context.mode != 'OBJECT':
            bpy.ops.object.mode_set(mode='OBJECT')
        return func(*args, **kwargs)
    return wrapper

@ensure_object_mode
def delete_selected_objects():
    """删除选中的物体（确保在物体模式下执行）"""
    bpy.ops.object.delete(use_global=False)
    print("已删除选中的物体")
```

## 实践示例

### 批量重命名选中物体

这个示例展示了如何使用上下文信息批量重命名选中的物体，结合选中的数量自动编号。

```python
import bpy

def batch_rename_selected(prefix="Cube", start=1):
    """批量重命名选中的物体"""
    selected = bpy.context.selected_objects

    if not selected:
        print("没有选中的物体")
        return

    for i, obj in enumerate(selected):
        new_name = f"{prefix}_{start + i:03d}"
        obj.name = new_name
        print(f"已将 {obj.name} 重命名为 {new_name}")

    print(f"共重命名 {len(selected)} 个物体")

# 使用示例
# batch_rename_selected("Cube", 1)
```

### 选中物体导出信息

这个示例展示了如何收集选中物体的详细信息并输出为报告格式。

```python
import bpy

def export_selected_report():
    """导出选中物体的报告"""
    selected = bpy.context.selected_objects
    report = []

    report.append("=" * 50)
    report.append("选中物体报告")
    report.append("=" * 50)
    report.append(f"选中数量: {len(selected)}")
    report.append("-" * 50)

    for i, obj in enumerate(selected, 1):
        report.append(f"[{i}] {obj.name}")
        report.append(f"    类型: {obj.type}")
        report.append(f"    位置: {obj.location.to_tuple()}")
        report.append(f"    旋转: {obj.rotation_euler.to_tuple()}")
        report.append(f"    缩放: {obj.scale.to_tuple()}")

        if obj.type == 'MESH':
            report.append(f"    顶点数: {len(obj.data.vertices)}")
            report.append(f"    边数: {len(obj.data.edges)}")
            report.append(f"    面数: {len(obj.data.polygons)}")

        report.append("")

    report_text = "\n".join(report)
    print(report_text)

    return report_text

# 使用示例
# export_selected_report()
```

### 快速选择同类物体

这个示例展示了如何根据条件快速选择场景中的同类物体。

```python
import bpy

def select_objects_by_type(obj_type, clear_first=True):
    """选择指定类型的所有物体"""
    if clear_first:
        bpy.ops.object.select_all(action='DESELECT')

    scene = bpy.context.scene

    for obj in scene.objects:
        if obj.type == obj_type:
            obj.select_set(True)
            print(f"已选中: {obj.name}")

    # 确保至少有一个物体被选中（保持活动物体）
    matching = [obj for obj in scene.objects if obj.type == obj_type]
    if matching:
        bpy.context.view_layer.objects.active = matching[0]
        print(f"共选中 {len(matching)} 个 {obj_type} 类型物体")

# 使用示例
# select_objects_by_type('MESH')
# select_objects_by_type('LIGHT')
# select_objects_by_type('CAMERA')
```

## 注意事项

### 上下文时效性

bpy.context 返回的上下文信息是实时变化的，在脚本执行过程中上下文可能因为用户操作或其他脚本而改变。因此，在关键操作前应该重新获取上下文信息，而不是依赖之前保存的状态。对于需要保存上下文以便后续恢复的场景，应该使用 deep_copy 方法。

### 区域依赖性

某些上下文属性依赖于当前激活的区域类型。例如，space_data 在 3D 视图中返回 SpaceView3D，在节点编辑器中返回 SpaceNodeEditor。在编写通用脚本时，应该先检查区域的类型再访问对应的属性。

### 模式依赖性

不同的编辑模式有不同的可用属性和操作。在编辑模式下可以访问网格的几何数据，而在物体模式下则不能。编写模式相关的脚本时，应该先检查当前模式再执行相应的操作。

### 多窗口环境

在多窗口环境中，bpy.context 返回当前执行上下文窗口的状态。如果脚本需要在所有窗口中都能正确运行，需要特别注意上下文的使用，必要时应该遍历所有窗口分别处理。

---
## bpy_data

# bpy.data 模块

## 模块概述

bpy.data 模块提供对 Blender 当前 .blend 文件中所有数据的全局访问接口，通过该模块可以访问场景、物体、材质、纹理、图像、几何节点树等各种数据。这个模块是 Blender Python API 的核心组成部分，它将 .blend 文件中的所有数据组织成易于访问的集合形式。与 bpy.context 提供运行时上下文信息不同，bpy.data 包含的是文件的持久化数据，这些数据在文件保存和加载时保持不变。

数据访问遵循标准的 Python 集合接口，可以通过索引、名称或键来访问具体的数据项。每个数据项都是 Blender 内部数据结构的 Python 表示，如 Scene、Object、Material、Texture、Image、NodeTree 等。bpy.data 中的数据在脚本运行过程中会被实时更新，例如当用户创建新物体时，bpy.data.objects 集合会自动包含新物体的引用。

该模块还提供了数据管理的常用方法，包括创建新数据项、删除不需要的数据、查找重复项、批量更新属性等。由于 bpy.data 包含整个项目的数据，在编写脚本时应该注意数据操作的影响范围，避免意外修改或删除重要的项目数据。

## 数据集合结构

### 集合属性

bpy.data 中的每个数据集合都继承自 bpy.props.CollectionProperty 和 bpy.types.ID 的组合，具有统一的访问接口。collections 属性返回所有数据集合的字典，可以按名称或索引访问每个集合。所有的集合都支持整数索引、字符串键和迭代操作，这提供了极大的灵活性。

每个数据项都有一些共同的基本属性。name 属性是数据项的显示名称，可以在用户界面中修改。users 属性表示有多少其他数据引用了这个数据项，用于判断是否可以安全删除。use_fake_user 属性设置是否为数据项创建一个虚引用，防止在没有用户引用时被垃圾回收。tag 属性用于在脚本中临时标记数据项。

```python
import bpy

# 访问所有数据集合
print("数据集合列表:")
for name, collection in bpy.data.collections.items():
    print(f"  {name}: {len(collection)} 项")

# 访问常用集合
scenes = bpy.data.scenes
objects = bpy.data.objects
materials = bpy.data.materials
meshes = bpy.data.meshes

print(f"\n场景数量: {len(scenes)}")
print(f"物体数量: {len(objects)}")
print(f"材质数量: {len(materials)}")
print(f"网格数量: {len(meshes)}")

# 通过名称访问数据项
cube = bpy.data.objects.get("Cube")
if cube:
    print(f"\n找到物体: {cube.name}")
    print(f"物体类型: {cube.type}")
    print(f"引用计数: {cube.users}")

# 通过索引访问数据项
if len(objects) > 0:
    first_obj = objects[0]
    print(f"\n第一个物体: {first_obj.name}")
```

### 数据生命周期

理解数据生命周期对于正确管理 Blender 项目数据至关重要。当创建一个新的数据项时，它的 users 计数从 0 开始。如果 users 变为 0 且没有设置 use_fake_user，该数据项会在下一次垃圾回收时从文件中删除。垃圾回收可以通过 bpy.data.remove 函数手动触发，也可以让 Blender 自动处理。

数据项的删除应该使用 bpy.data.remove 函数，而不是直接从集合中弹出或删除。这确保了所有引用和数据关联被正确清理。如果删除的数据项仍被其他数据引用，Blender 会阻止删除操作并抛出异常。

```python
import bpy

def safe_remove_data(data_item):
    """安全地删除数据项"""
    if data_item is None:
        return

    # 检查引用计数
    if data_item.users > 0:
        print(f"警告: {data_item.name} 仍有 {data_item.users} 个引用，无法删除")
        return False

    # 获取数据项类型名称
    data_type = type(data_item).__name__
    data_name = data_item.name

    # 删除数据项
    bpy.data.remove(data_item)
    print(f"已删除 {data_type}: {data_name}")
    return True

def create_fake_user_for_orphans():
    """为所有孤立数据项创建虚引用"""
    removed_count = 0

    for collection_name in ['objects', 'materials', 'meshes', 'textures', 'images']:
        collection = getattr(bpy.data, collection_name)
        for item in collection:
            if item.users == 0:
                item.use_fake_user = True
                removed_count += 1

    print(f"为 {removed_count} 个孤立数据项创建了虚引用")

# 使用示例
# create_fake_user_for_orphans()
```

## 场景数据访问

### 场景集合

scenes 集合包含项目中的所有场景，每个场景是一个完整的场景定义，包含自己的物体集合、渲染设置、时间线等。通过这个集合可以创建新场景、切换活动场景、复制现有场景等操作。活动场景存储在 bpy.context.scene 中，但可以通过 bpy.data.scenes 访问所有场景。

```python
import bpy

# 获取所有场景
for scene in bpy.data.scenes:
    print(f"场景: {scene.name}")

# 创建新场景
new_scene = bpy.data.scenes.new(name="渲染场景")
print(f"创建新场景: {new_scene.name}")

# 复制现有场景
if bpy.data.scenes:
    copied_scene = bpy.data.scenes.new(name="复制场景", from_scene=bpy.data.scenes[0])
    print(f"复制场景: {copied_scene.name}")

# 删除场景（如果有引用则无法删除）
scene_to_delete = bpy.data.scenes.get("删除场景")
if scene_to_delete and scene_to_delete.users == 0:
    bpy.data.scenes.remove(scene_to_delete)

# 设置活动场景
if len(bpy.data.scenes) > 1:
    bpy.context.window.scene = bpy.data.scenes[1]
    print(f"切换到场景: {bpy.context.scene.name}")
```

### 场景属性访问

每个场景对象包含丰富的属性，涵盖了场景的各个方面。objects 集合包含场景中所有物体的引用。view_layers 集合包含视图层的定义，用于组织物体的可见性和渲染。render 属性包含渲染设置，如分辨率、采样、输出格式等。timeline_cursor 表示时间线光标的位置。

```python
import bpy

scene = bpy.context.scene

# 访问场景中的物体
print(f"场景 '{scene.name}' 中的物体:")
for obj in scene.objects:
    print(f"  - {obj.name} ({obj.type})")

# 访问视图层
print(f"\n视图层:")
for layer in scene.view_layers:
    layer_info = {
        'name': layer.name,
        'objects': len(layer.objects),
        'use': layer.use
    }
    print(f"  - {layer_info}")

# 访问渲染设置
render = scene.render
print(f"\n渲染设置:")
print(f"  分辨率: {render.resolution_x} x {render.resolution_y}")
print(f"  帧范围: {render.frame_start} - {render.frame_end}")
print(f"  FPS: {render.fps}")

# 访问节点树
if scene.use_nodes:
    tree = scene.node_tree
    print(f"\n合成节点树:")
    for node in tree.nodes:
        print(f"  - {node.name} ({node.type})")
```

## 物体数据访问

### 物体集合

objects 集合包含项目中所有物体的引用，无论这些物体属于哪个场景。每个物体都有一个唯一的数据块引用，指向其几何数据（如 Mesh、Curve、Surface 等）。物体的位置、旋转、缩放等变换信息存储在物体本身，而几何数据则存储在独立的数据块中。

```python
import bpy

# 遍历所有物体
print("所有物体:")
for obj in bpy.data.objects:
    obj_type = obj.type
    data_block = obj.data

    print(f"  {obj.name}:")
    print(f"    类型: {obj_type}")
    print(f"    数据块: {data_block.name if data_block else '无'}")

    # 获取变换信息
    print(f"    位置: {obj.location.to_tuple()}")
    print(f"    旋转: {obj.rotation_euler.to_tuple()}")
    print(f"    缩放: {obj.scale.to_tuple()}")

    # 获取物体矩阵
    print(f"    世界矩阵:\n{obj.matrix_world}")

# 按类型筛选物体
def get_objects_by_type(obj_type):
    """获取指定类型的所有物体"""
    return [obj for obj in bpy.data.objects if obj.type == obj_type]

mesh_objects = get_objects_by_type('MESH')
light_objects = get_objects_by_type('LIGHT')
camera_objects = get_objects_by_type('CAMERA')

print(f"\n网格物体: {len(mesh_objects)}")
print(f"灯光物体: {len(light_objects)}")
print(f"相机物体: {len(camera_objects)}")
```

### 物体数据块

物体的几何数据存储在独立的数据块中，可以通过物体的 data 属性访问。不同的物体类型对应不同的数据块类型：网格物体对应 Mesh 数据块，曲线物体对应 Curve 数据块，曲面物体对应 Surface 数据块，字体物体对应 Text 数据块等。

```python
import bpy

# 访问网格数据
for obj in bpy.data.objects:
    if obj.type == 'MESH':
        mesh = obj.data
        print(f"\n物体 {obj.name} 的网格数据:")
        print(f"  网格名称: {mesh.name}")
        print(f"  顶点数: {len(mesh.vertices)}")
        print(f"  边数: {len(mesh.edges)}")
        print(f"  面数: {len(mesh.polygons)}")

        # 访问顶点坐标
        if mesh.vertices:
            first_vertex = mesh.vertices[0]
            print(f"  第一个顶点: {first_vertex.co}")

        # 访问面法线
        if mesh.polygons:
            first_poly = mesh.polygons[0]
            print(f"  第一个面法线: {first_poly.normal}")

# 访问曲线数据
for obj in bpy.data.objects:
    if obj.type == 'CURVE':
        curve = obj.data
        print(f"\n物体 {obj.name} 的曲线数据:")
        print(f"  曲线名称: {curve.name}")
        print(f"  样条数量: {len(curve.splines)}")

        for spline in curve.splines:
            spline_type = type(spline).__name__
            points_count = len(spline.points) if hasattr(spline, 'points') else len(spline.bezier_points)
            print(f"    样条类型: {spline_type}, 点数: {points_count}")
```

### 创建和删除物体

通过 bpy.data.objects.new 可以创建新物体，这个方法需要指定物体名称和数据块。数据块可以是 None（创建空物体）或现有的数据块引用。创建物体后，需要将其添加到场景中才能在视图中显示。

```python
import bpy
import math

def create_new_mesh(name, vertices, edges, faces):
    """创建新的网格数据"""
    mesh = bpy.data.meshes.new(name)
    mesh.from_pydata(vertices, edges, faces)
    mesh.update()

    # 创建物体
    obj = bpy.data.objects.new(name, mesh)
    
    # 添加到当前场景
    bpy.context.scene.collection.objects.link(obj)
    
    return obj

def create_cube(name="新立方体"):
    """创建立方体"""
    # 定义立方体顶点
    vertices = [
        (-1, -1, -1), (1, -1, -1), (1, 1, -1), (-1, 1, -1),
        (-1, -1, 1), (1, -1, 1), (1, 1, 1), (-1, 1, 1)
    ]

    # 定义边
    edges = [
        (0, 1), (1, 2), (2, 3), (3, 0),
        (4, 5), (5, 6), (6, 7), (7, 4),
        (0, 4), (1, 5), (2, 6), (3, 7)
    ]

    # 定义面
    faces = [
        (0, 1, 2, 3), (4, 7, 6, 5),
        (0, 4, 5, 1), (1, 5, 6, 2),
        (2, 6, 7, 3), (3, 7, 4, 0)
    ]

    return create_new_mesh(name, vertices, edges, faces)

# 使用示例
# cube = create_cube("我的立方体")
# cube.location = (0, 0, 0)

def duplicate_object(obj, new_name=None):
    """复制物体"""
    if new_name is None:
        new_name = f"{obj.name}_复制"

    # 复制数据块
    new_data = obj.data.copy()
    new_data.name = new_name

    # 创建新物体
    new_obj = bpy.data.objects.new(new_name, new_data)

    # 复制变换属性
    new_obj.location = obj.location.copy()
    new_obj.rotation_euler = obj.rotation_euler.copy()
    new_obj.scale = obj.scale.copy()

    # 复制其他属性
    new_obj.matrix_world = obj.matrix_world.copy()

    # 添加到场景
    bpy.context.scene.collection.objects.link(new_obj)

    return new_obj

# 使用示例
# if bpy.context.active_object:
#     duplicated = duplicate_object(bpy.context.active_object)
```

## 材质数据访问

### 材质集合

materials 集合包含项目中所有材质的引用。每个材质可以应用到多个物体，材质的使用计数（users）表示有多少物体或数据块使用了该材质。材质包含多个属性组，如表面属性、体积属性、置换属性等。

```python
import bpy

# 遍历所有材质
for material in bpy.data.materials:
    print(f"\n材质: {material.name}")
    print(f"  引用计数: {material.users}")

    # 检查是否使用
    is_used = material.users > 0
    print(f"  正在使用: {is_used}")
    print(f"  虚引用: {material.use_fake_user}")

    # 获取材质类型
    material_type = "体积" if material.use_nodes and material.node_tree and any(n.type == 'BSDF_VOLUME' for n in material.node_tree.nodes) else "表面"
    print(f"  类型: {material_type}")

    # 如果使用节点，遍历节点
    if material.use_nodes and material.node_tree:
        print(f"  节点树:")
        for node in material.node_tree.nodes:
            print(f"    - {node.name} ({node.type})")

# 创建新材质
def create_new_material(name, color=(0.5, 0.5, 0.5, 1.0)):
    """创建新的基础材质"""
    material = bpy.data.materials.new(name=name)
    material.diffuse_color = color

    # 禁用节点，使用简单材质
    material.use_nodes = False
    material.diffuse_color = color[:3]

    return material

# 使用示例
# mat = create_new_material("我的材质", (0.8, 0.2, 0.2, 1.0))
# bpy.context.active_object.data.materials.append(mat)
```

### 材质属性访问

材质对象包含丰富的属性，覆盖了表面着色、体积渲染、置换等多个方面。每个属性组都有独立的配置选项，可以通过材质对象的属性直接访问。

```python
import bpy

def analyze_material(material):
    """分析材质的详细信息"""
    info = {
        '名称': material.name,
        '引用计数': material.users,
        '使用节点': material.use_nodes,
        '虚引用': material.use_fake_user
    }

    # 表面属性
    if hasattr(material, 'surface'):
        surface = material.surface
        info['表面着色器'] = surface.shader_type if hasattr(surface, 'shader_type') else "无"

    # 体积属性
    if hasattr(material, 'volume'):
        volume = material.volume
        info['体积着色器'] = volume.shader_type if hasattr(volume, 'shader_type') else "无"

    return info

# 分析所有材质
print("材质分析报告:")
for material in bpy.data.materials:
    info = analyze_material(material)
    print(f"\n{info['名称']}:")
    for key, value in info.items():
        print(f"  {key}: {value}")
```

## 纹理和图像数据访问

### 纹理集合

textures 集合包含项目中所有纹理的引用。纹理可以是图像纹理、程序纹理（如云层、噪波、 Musgrave 等）。每个纹理都有类型属性，标识纹理的种类。Blender 4.0 之后，传统的纹理系统逐渐被节点系统取代，但纹理集合仍然保留用于向后兼容。

```python
import bpy

# 遍历所有纹理
for texture in bpy.data.textures:
    print(f"\n纹理: {texture.name}")
    print(f"  类型: {texture.type}")
    print(f"  引用计数: {texture.users}")

    # 根据类型显示不同信息
    if texture.type == 'CLOUDS':
        print(f"  云层纹理 - 大小: {texture.noise_scale}")
    elif texture.type == 'WOOD':
        print(f"  木纹纹理 - 宽度: {texture.noise_width}")
    elif texture.type == 'MARBLE':
        print(f"  大理石纹理 - 深度: {texture.noise_depth}")
    elif texture.type == 'IMAGE':
        if texture.image:
            print(f"  图像纹理: {texture.image.name}")
            print(f"  图像尺寸: {texture.image.size[0]} x {texture.image.size[1]}")

# 统计纹理类型
texture_counts = {}
for texture in bpy.data.textures:
    texture_type = texture.type
    texture_counts[texture_type] = texture_counts.get(texture_type, 0) + 1

print("\n纹理类型统计:")
for tex_type, count in texture_counts.items():
    print(f"  {tex_type}: {count}")
```

### 图像集合

images 集合包含项目中加载的所有图像。每个图像可以是外部文件引用或嵌入式数据。图像有尺寸、通道数、颜色空间等属性。对于外部文件，可以检查文件是否丢失或需要重新加载。

```python
import bpy
import os

def analyze_image(image):
    """分析图像信息"""
    info = {
        '名称': image.name,
        '尺寸': f"{image.size[0]} x {image.size[1]}",
        '通道数': image.channels if hasattr(image, 'channels') else "未知",
        '深度': f"{image.depth} 位" if hasattr(image, 'depth') else "未知",
        '文件路径': image.filepath if hasattr(image, 'filepath') else "无",
        '已打包': image.packed_file is not None if hasattr(image, 'packed_file') else False,
        '引用计数': image.users,
        '分辨率': f"{image.dpi[0]} DPI" if hasattr(image, 'dpi') else "未知"
    }

    return info

# 分析所有图像
print("图像分析报告:")
for image in bpy.data.images:
    info = analyze_image(image)
    print(f"\n{info['名称']}:")
    for key, value in info.items():
        print(f"  {key}: {value}")

    # 检查文件是否存在（对于未打包的图像）
    if info['文件路径'] and info['文件路径'] != "无" and not info['已打包']:
        filepath = bpy.path.abspath(info['文件路径'])
        if not os.path.exists(filepath):
            print(f"  警告: 文件丢失!")

# 创建新图像
def create_new_image(name, width, height, color=(0, 0, 0, 1)):
    """创建新图像"""
    image = bpy.data.images.new(name=name, width=width, height=height, alpha='A' in image.color_mode if hasattr(image, 'color_mode') else True)

    # 填充颜色
    pixels = [c for c in color for _ in range(width * height)]
    image.pixels = pixels

    return image

# 使用示例
# img = create_new_image("我的纹理", 512, 512, (1, 1, 1, 1))
```

## 节点树数据访问

### 节点树集合

node_trees 集合包含项目中所有节点树的引用，包括材质节点树、合成节点树、几何节点树等。每种节点树都有特定的根节点类型和用途。材质节点树用于定义材质外观，合成节点树用于后期处理，几何节点树用于几何体生成和修改。

```python
import bpy

# 遍历所有节点树
for node_tree in bpy.data.node_groups:
    print(f"\n节点树: {node_tree.name}")
    print(f"  类型: {node_tree.type}")
    print(f"  节点数: {len(node_tree.nodes)}")
    print(f"  链接数: {len(node_tree.links)}")

    # 统计节点类型
    node_counts = {}
    for node in node_tree.nodes:
        node_type = node.type
        node_counts[node_type] = node_counts.get(node_type, 0) + 1

    print(f"  节点类型统计:")
    for ntype, count in node_counts.items():
        print(f"    {ntype}: {count}")

# 按类型筛选节点树
def get_node_trees_by_type(tree_type):
    """获取指定类型的节点树"""
    return [tree for tree in bpy.data.node_groups if tree.type == tree_type]

material_trees = get_node_trees_by_type('SHADER')
geometry_trees = get_node_trees_by_type('GEOMETRY')
compositor_trees = get_node_trees_by_type('COMPOSITE')

print(f"\n材质节点树: {len(material_trees)}")
print(f"几何节点树: {len(geometry_trees)}")
print(f"合成节点树: {len(compositor_trees)}")
```

### 节点和链接访问

每个节点树由多个节点和链接组成。节点是执行特定功能的单元，如 Principled BSDF 节点、混合节点、数学节点等。链接连接节点的输出和输入端口，建立数据流。可以通过遍历 nodes 和 links 集合来访问和修改节点树的结构。

```python
import bpy

def analyze_node_tree(node_tree):
    """分析节点树结构"""
    print(f"\n节点树: {node_tree.name}")
    print(f"  节点数: {len(node_tree.nodes)}")
    print(f"  链接数: {len(node_tree.links)}")

    print("  节点列表:")
    for i, node in enumerate(node_tree.nodes):
        print(f"    [{i}] {node.name} ({node.type})")

        # 显示输入端口
        inputs = [f"{inp.identifier}:{inp.type}" for inp in node.inputs if inp.enabled]
        if inputs:
            print(f"        输入: {', '.join(inputs)}")

        # 显示输出端口
        outputs = [f"{out.identifier}:{out.type}" for out in node.outputs if out.enabled]
        if outputs:
            print(f"        输出: {', '.join(outputs)}")

    print("  链接列表:")
    for link in node_tree.links:
        from_node = link.from_node.name
        from_socket = link.from_socket.identifier
        to_node = link.to_node.name
        to_socket = link.to_socket.identifier
        print(f"    {from_node}.{from_socket} -> {to_node}.{to_socket}")

# 分析当前材质的节点树
if bpy.context.active_object and bpy.context.active_object.active_material:
    material = bpy.context.active_object.active_material
    if material.use_nodes and material.node_tree:
        analyze_node_tree(material.node_tree)
```

### 创建节点和链接

通过 Python API 可以创建新节点并建立连接来构建节点树。这是程序化生成材质和几何节点设置的基础。

```python
import bpy

def create_simple_material(name="程序材质"):
    """创建简单的 Principled BSDF 材质"""
    # 创建材质
    material = bpy.data.materials.new(name=name)
    material.use_nodes = True

    # 获取节点树
    tree = material.node_tree

    # 清除默认节点
    tree.nodes.clear()

    # 创建输出节点
    output = tree.nodes.new('ShaderNodeOutputMaterial')
    output.location = (400, 0)

    # 创建 Principled BSDF 节点
    principled = tree.nodes.new('ShaderNodeBsdfPrincipled')
    principled.location = (0, 0)

    # 创建颜色渐变节点（可选）
    gradient = tree.nodes.new('ShaderNodeValToRGB')
    gradient.location = (-400, 0)
    gradient.color_ramp.elements[0].color = (0, 0, 1, 1)
    gradient.color_ramp.elements[1].color = (1, 0, 0, 1)

    # 创建链接
    tree.links.new(principled.outputs['BSDF'], output.inputs['Surface'])

    return material

# 使用示例
# mat = create_simple_material("我的程序材质")

def create_geometry_nodes_tree(name="几何节点"):
    """创建几何节点树"""
    # 创建节点树
    tree = bpy.data.node_groups.new(name=name, type='GEOMETRY')

    # 创建节点
    input_node = tree.nodes.new('NodeGroupInput')
    output_node = tree.nodes.new('NodeGroupOutput')
    cube_node = tree.nodes.new('GeometryNodeMeshCube')

    # 设置位置
    input_node.location = (-200, 0)
    output_node.location = (400, 0)
    cube_node.location = (0, 0)

    # 创建链接
    tree.links.new(cube_node.outputs['Mesh'], output_node.inputs['Geometry'])

    return tree

# 使用示例
# geo_tree = create_geometry_nodes_tree("我的几何节点")
```

## 常用操作方法

### 查找重复数据

项目中经常会产生重复的数据项，如复制物体时连带复制的数据块。使用 find_duplicates 方法可以找出可以合并的数据项，这有助于优化文件大小和减少内存使用。

```python
import bpy

def find_duplicate_meshes():
    """查找重复的网格数据"""
    mesh_dict = {}

    for mesh in bpy.data.meshes:
        key = (len(mesh.vertices), len(mesh.polygons))
        if key not in mesh_dict:
            mesh_dict[key] = []
        mesh_dict[key].append(mesh)

    duplicates = {k: v for k, v in mesh_dict.items() if len(v) > 1}
    return duplicates

def find_similar_materials(threshold=0.9):
    """查找相似的材质"""
    # 基于节点类型和数量比较
    materials = list(bpy.data.materials)
    similar_pairs = []

    for i, mat1 in enumerate(materials):
        for mat2 in materials[i + 1:]:
            if mat1.use_nodes and mat2.use_nodes:
                if mat1.node_tree and mat2.node_tree:
                    nodes1 = len(mat1.node_tree.nodes)
                    nodes2 = len(mat2.node_tree.nodes)
                    if nodes1 == nodes2:
                        similar_pairs.append((mat1, mat2))

    return similar_pairs

# 使用示例
# duplicates = find_duplicate_meshes()
# for key, meshes in duplicates.items():
#     print(f"重复网格 (顶点数: {key[0]}, 面数: {key[1]}):")
#     for mesh in meshes:
#         print(f"  - {mesh.name}")
```

### 批量更新属性

当需要更新多个数据项的共同属性时，可以遍历数据集合进行批量操作。这比逐个处理更高效，也更容易维护。

```python
import bpy

def batch_set_material_color(color, material_names=None):
    """批量设置材质颜色"""
    materials = bpy.data.materials

    if material_names:
        target_materials = [mat for mat in materials if mat.name in material_names]
    else:
        target_materials = list(materials)

    for mat in target_materials:
        if not mat.use_nodes:
            mat.diffuse_color = color

    print(f"已更新 {len(target_materials)} 个材质")

def batch_rename_objects(prefix, obj_type=None):
    """批量重命名物体"""
    objects = bpy.data.objects

    if obj_type:
        target_objects = [obj for obj in objects if obj.type == obj_type]
    else:
        target_objects = list(objects)

    for i, obj in enumerate(target_objects):
        obj.name = f"{prefix}_{i:03d}"

    print(f"已重命名 {len(target_objects)} 个物体")

def batch_set_object_location(location, obj_names=None):
    """批量设置物体位置"""
    objects = bpy.data.objects

    if obj_names:
        target_objects = [obj for obj in objects if obj.name in obj_names]
    else:
        target_objects = list(objects)

    for obj in target_objects:
        obj.location = location

    print(f"已更新 {len(target_objects)} 个物体的位置")

# 使用示例
# batch_set_material_color((0.8, 0.2, 0.2, 1.0))
# batch_rename_objects("Cube", 'MESH')
# batch_set_object_location((0, 0, 0))
```

### 数据验证和清理

定期验证和清理项目数据可以保持文件的整洁和性能。使用各种 API 方法可以检查数据的有效性并清理不需要的项目。

```python
import bpy

def validate_all_data():
    """验证所有数据项的有效性"""
    issues = []

    # 检查孤立物体
    for obj in bpy.data.objects:
        if obj.users == 0:
            issues.append(f"孤立物体: {obj.name}")

    # 检查孤立材质
    for mat in bpy.data.materials:
        if mat.users == 0 and not mat.use_fake_user:
            issues.append(f"孤立材质: {mat.name}")

    # 检查孤立网格
    for mesh in bpy.data.meshes:
        if mesh.users == 0:
            issues.append(f"孤立网格: {mesh.name}")

    # 检查丢失的图像引用
    for image in bpy.data.images:
        if image.packed_file is None and not image.filepath:
            issues.append(f"无源图像: {image.name}")

    return issues

def cleanup_orphaned_data():
    """清理孤立数据项"""
    removed = {'objects': 0, 'materials': 0, 'meshes': 0, 'textures': 0, 'images': 0}

    # 清理孤立物体
    for obj in list(bpy.data.objects):
        if obj.users == 0:
            bpy.data.objects.remove(obj)
            removed['objects'] += 1

    # 清理孤立材质
    for mat in list(bpy.data.materials):
        if mat.users == 0:
            bpy.data.materials.remove(mat)
            removed['materials'] += 1

    # 清理孤立网格
    for mesh in list(bpy.data.meshes):
        if mesh.users == 0:
            bpy.data.meshes.remove(mesh)
            removed['meshes'] += 1

    return removed

# 使用示例
# issues = validate_all_data()
# for issue in issues:
#     print(issue)
```

## 实践示例

### 导出数据报告

这个示例展示了如何生成项目数据的详细报告。

```python
import bpy

def generate_data_report():
    """生成项目数据报告"""
    report = []
    report.append("=" * 60)
    report.append("Blender 项目数据报告")
    report.append("=" * 60)

    # 场景信息
    report.append(f"\n场景数量: {len(bpy.data.scenes)}")
    for scene in bpy.data.scenes:
        report.append(f"  - {scene.name}")

    # 物体统计
    report.append(f"\n物体总数: {len(bpy.data.objects)}")
    obj_counts = {}
    for obj in bpy.data.objects:
        obj_counts[obj.type] = obj_counts.get(obj.type, 0) + 1
    for obj_type, count in sorted(obj_counts.items()):
        report.append(f"  - {obj_type}: {count}")

    # 材质统计
    report.append(f"\n材质总数: {len(bpy.data.materials)}")
    used_materials = sum(1 for m in bpy.data.materials if m.users > 0)
    unused_materials = len(bpy.data.materials) - used_materials
    report.append(f"  - 使用中: {used_materials}")
    report.append(f"  - 未使用: {unused_materials}")

    # 网格统计
    report.append(f"\n网格总数: {len(bpy.data.meshes)}")
    total_vertices = sum(len(m.vertices) for m in bpy.data.meshes)
    total_polygons = sum(len(m.polygons) for m in bpy.data.meshes)
    report.append(f"  - 总顶点数: {total_vertices}")
    report.append(f"  - 总面数: {total_polygons}")

    # 图像统计
    report.append(f"\n图像总数: {len(bpy.data.images)}")
    total_pixels = sum(i.size[0] * i.size[1] for i in bpy.data.images)
    report.append(f"  - 总像素数: {total_pixels}")

    # 节点树统计
    report.append(f"\n节点树总数: {len(bpy.data.node_groups)}")

    return "\n".join(report)

# 使用示例
# report = generate_data_report()
# print(report)
```

### 资源管理器

这个示例展示了一个简单的资源管理器功能，用于查看和管理项目数据。

```python
import bpy

class DataManager:
    def __init__(self):
        self.data_types = [
            ('scenes', '场景', bpy.data.scenes),
            ('objects', '物体', bpy.data.objects),
            ('materials', '材质', bpy.data.materials),
            ('meshes', '网格', bpy.data.meshes),
            ('textures', '纹理', bpy.data.textures),
            ('images', '图像', bpy.data.images),
            ('node_groups', '节点树', bpy.data.node_groups)
        ]

    def list_data_items(self, data_type):
        """列出指定类型的所有数据项"""
        for type_key, type_name, collection in self.data_types:
            if type_key == data_type:
                print(f"\n{type_name} ({len(collection)} 项):")
                for item in collection:
                    users = item.users
                    fake_user = getattr(item, 'use_fake_user', False)
                    status = "使用中" if users > 0 else ("虚引用" if fake_user else "孤立")
                    print(f"  - {item.name} ({status}, 引用: {users})")
                return

    def get_statistics(self):
        """获取数据统计信息"""
        stats = {}
        for type_key, _, collection in self.data_types:
            stats[type_key] = len(collection)
        return stats

    def find_data_by_name(self, name, data_type=None):
        """按名称查找数据项"""
        results = []

        if data_type:
            target_types = [(dt, tn, c) for dt, tn, c in self.data_types if dt == data_type]
        else:
            target_types = self.data_types

        for type_key, _, collection in target_types:
            for item in collection:
                if name.lower() in item.name.lower():
                    results.append((type_key, item))

        return results

    def remove_unused_data(self, data_type=None):
        """删除未使用的数据项"""
        removed_count = 0

        if data_type:
            target_collections = [c for dt, _, c in self.data_types if dt == data_type]
        else:
            target_collections = [c for _, _, c in self.data_types]

        for collection in target_collections:
            collection_name = type(collection).__name__
            for item in list(collection):
                if item.users == 0 and not getattr(item, 'use_fake_user', False):
                    collection.remove(item)
                    removed_count += 1
                    print(f"已删除: {item.name} ({collection_name})")

        return removed_count

# 使用示例
# manager = DataManager()
# manager.list_data_items('materials')
# results = manager.find_data_by_name('Cube')
# removed = manager.remove_unused_data()
```

## 注意事项

### 数据引用管理

在操作 bpy.data 中的数据时，需要特别注意引用关系。当删除一个数据项时，确保没有其他数据引用它，否则会抛出异常。可以使用 users 属性检查引用计数。对于需要保留但暂时未使用的数据，可以设置 use_fake_user 属性来防止被垃圾回收。

### 文件性能影响

bpy.data 中的数据量直接影响文件的加载、保存和内存使用。大型项目应该定期清理未使用的数据项，使用批量操作代替循环操作可以提高性能。在编写处理大量数据的脚本时，应该注意内存使用，避免创建不必要的数据副本。

### 实时更新机制

bpy.data 中的集合是实时更新的，这意味着在遍历集合时如果创建或删除数据项，可能会影响遍历的正确性。如果需要在遍历时修改集合，建议先创建数据项的副本列表。

---
## bpy_types.md

# bpy.types Reference

bpy.types 模块定义了 Blender 中所有数据类型及其属性。

## ID 基类

所有 Blender 数据块都继承自 ID 基类。

### 通用属性

```python
# 数据块名称
name = data_block.name

# 用户数量
users = data_block.users

# 是否使用假用户
use_fake_user = data_block.use_fake_user

# 数据块标签
tag = data_block.tag

# 库引用
library = data_block.library

# 是否是本地数据块
is_local = data_block.library is None

# 是否是链接数据块
is_library_indirect = data_block.library is not None
```

## Scene (场景)

场景包含所有场景数据。

```python
# 获取当前场景
scene = bpy.context.scene

# 场景属性
scene.name  # 场景名称
scene.frame_start  # 起始帧
scene.frame_end  # 结束帧
scene.frame_current  # 当前帧
scene.render  # 渲染设置
scene.camera  # 活动相机
scene.world  # 世界设置
scene.collection  # 主集合

# 遍历场景中的对象
for obj in scene.objects:
    print(obj.name)

# 获取活动对象
active_obj = scene.active_object

# 设置活动对象
scene.active_object = obj

# 更新场景
scene.update()
```

## Object (对象)

对象是 Blender 中的 3D 实体。

```python
# 获取对象
obj = bpy.context.active_object

# 对象属性
obj.name  # 对象名称
obj.type  # 对象类型 ('MESH', 'CURVE', 'ARMATURE', 等)
obj.location  # 位置
obj.rotation_euler  # 欧拉旋转
obj.rotation_quaternion  # 四元数旋转
obj.scale  # 缩放
obj.matrix_world  # 世界矩阵
obj.matrix_local  # 本地矩阵
obj.parent  # 父对象
obj.parent_type  # 父类型
obj.lock_location  # 锁定位置
obj.lock_rotation  # 锁定旋转
obj.lock_scale  # 锁定缩放
obj.hide_viewport  # 在视口中隐藏
obj.hide_render  # 在渲染中隐藏
obj.select  # 选择状态
obj.active_material  # 活动材质
obj.active_material_index  # 活动材质索引

# 对象操作
# 设置位置
obj.location = (1.0, 2.0, 3.0)

# 设置旋转
obj.rotation_euler = (0.0, 0.0, math.radians(45))

# 设置缩放
obj.scale = (2.0, 2.0, 2.0)

# 应用变换
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)

# 设置父对象
obj.parent = parent_obj
obj.parent_type = 'OBJECT'

# 清除父对象
obj.parent = None

# 复制对象
new_obj = obj.copy()
bpy.context.collection.objects.link(new_obj)

# 删除对象
bpy.data.objects.remove(obj, do_unlink=True)
```

## Mesh (网格)

网格定义了对象的几何形状。

```python
# 获取网格
mesh = obj.data

# 网格属性
mesh.name  # 网格名称
mesh.vertices  # 顶点列表
mesh.edges  # 边列表
mesh.polygons  # 面列表
mesh.materials  # 材质列表
mesh.uv_layers  # UV 层
mesh.vertex_colors  # 顶点颜色
mesh.vertex_groups  # 顶点组
mesh.shape_keys  # 形状键
mesh.modifiers  # 修改器

# 遍历顶点
for vert in mesh.vertices:
    print(f"Vertex {vert.index}: {vert.co}")

# 遍历边
for edge in mesh.edges:
    print(f"Edge {edge.index}: {edge.vertices}")

# 遍历面
for poly in mesh.polygons:
    print(f"Face {poly.index}: {poly.vertices}")

# 访问顶点坐标
for vert in mesh.vertices:
    x, y, z = vert.co

# 访问面法线
for poly in mesh.polygons:
    normal = poly.normal

# 访问 UV 坐标
if mesh.uv_layers.active:
    uv_layer = mesh.uv_layers.active.data
    for poly in mesh.polygons:
        for loop_idx in poly.loop_indices:
            uv = uv_layer[loop_idx].uv
            print(f"UV: {uv.x}, {uv.y}")

# 更新网格
mesh.update()

# 重新计算法线
mesh.calc_normals()

# 重新计算切线
mesh.calc_tangents()
```

## Material (材质)

材质定义了对象的表面属性。

```python
# 创建材质
mat = bpy.data.materials.new(name="MyMaterial")

# 材质属性
mat.name  # 材质名称
mat.use_nodes  # 是否使用节点
mat.node_tree  # 节点树
mat.diffuse_color  # 漫反射颜色
mat.metallic  # 金属度
mat.roughness  # 粗糙度
mat.specular_intensity  # 镜面强度
mat.alpha  # 透明度
mat.blend_method  # 混合方法

# 使用节点
mat.use_nodes = True
nodes = mat.node_tree.nodes
links = mat.node_tree.links

# 获取输出节点
output = nodes.get("Material Output")

# 获取 BSDF 节点
bsdf = nodes.get("Principled BSDF")

# 设置颜色
bsdf.inputs['Base Color'].default_value = (1.0, 0.0, 0.0, 1.0)

# 设置金属度
bsdf.inputs['Metallic'].default_value = 1.0

# 设置粗糙度
bsdf.inputs['Roughness'].default_value = 0.5

# 将材质分配给对象
obj = bpy.context.active_object
if obj.material_slots:
    obj.material_slots[0].material = mat
else:
    obj.data.materials.append(mat)
```

## Texture (纹理)

纹理定义了图像或程序纹理。

```python
# 创建纹理
tex = bpy.data.textures.new(name="MyTexture", type='IMAGE')

# 纹理属性
tex.name  # 纹理名称
tex.type  # 纹理类型
tex.image  # 图像
tex.use_interpolation  # 使用插值
tex.use_mipmap  # 使用 mipmap
tex.extension  # 扩展方式

# 加载图像
tex.image = bpy.data.images.load(filepath="path/to/image.png")
```

## Image (图像)

图像用于纹理和渲染。

```python
# 加载图像
img = bpy.data.images.load(filepath="path/to/image.png")

# 图像属性
img.name  # 图像名称
img.filepath  # 文件路径
img.size  # 图像尺寸
img.pixels  # 像素数据
img.channels  # 通道数
img.alpha_mode  # Alpha 模式
img.colorspace_settings  # 色彩空间设置

# 创建新图像
img = bpy.data.images.new(
    name="NewImage",
    width=512,
    height=512,
    alpha=True,
    float_buffer=False
)

# 访问像素数据
pixels = img.pixels
for i in range(0, len(pixels), 4):
    r = pixels[i]
    g = pixels[i + 1]
    b = pixels[i + 2]
    a = pixels[i + 3]

# 保存图像
img.save()
```

## Camera (相机)

相机定义了视图和渲染视角。

```python
# 创建相机
cam = bpy.data.cameras.new(name="MyCamera")

# 相机属性
cam.name  # 相机名称
cam.type  # 相机类型 ('PERSP', 'ORTHO')
cam.lens  # 焦距
cam.sensor_width  # 传感器宽度
cam.sensor_height  # 传感器高度
cam.ortho_scale  # 正交缩放
cam.clip_start  # 近裁剪距离
cam.clip_end  # 远裁剪距离
cam.dof  # 景深设置

# 创建相机对象
cam_obj = bpy.data.objects.new(name="Camera", object_data=cam)
bpy.context.collection.objects.link(cam_obj)
bpy.context.scene.camera = cam_obj

# 设置相机位置
cam_obj.location = (0.0, -10.0, 2.0)
cam_obj.rotation_euler = (math.radians(90), 0.0, 0.0)
```

## Light (灯光)

灯光定义了场景中的光源。

```python
# 创建灯光
light = bpy.data.lights.new(name="MyLight", type='POINT')

# 灯光属性
light.name  # 灯光名称
light.type  # 灯光类型 ('POINT', 'SUN', 'SPOT', 'AREA')
light.color  # 灯光颜色
light.energy  # 灯光强度
light.distance  # 灯光距离
light.specular_factor  # 镜面因子
light.shadow_soft_size  # 阴影软大小

# 创建灯光对象
light_obj = bpy.data.objects.new(name="Light", object_data=light)
bpy.context.collection.objects.link(light_obj)

# 设置灯光位置
light_obj.location = (0.0, 0.0, 5.0)
```

## World (世界)

世界定义了场景的背景和环境。

```python
# 获取世界
world = bpy.context.scene.world

# 创建世界
world = bpy.data.worlds.new(name="MyWorld")
bpy.context.scene.world = world

# 世界属性
world.name  # 世界名称
world.use_nodes  # 是否使用节点
world.node_tree  # 节点树
world.color  # 背景颜色

# 使用节点
world.use_nodes = True
nodes = world.node_tree.nodes
bg_node = nodes.get("Background")
bg_node.inputs['Color'].default_value = (0.1, 0.1, 0.2, 1.0)
bg_node.inputs['Strength'].default_value = 1.0
```

## Armature (骨架)

骨架用于角色动画。

```python
# 创建骨架
arm = bpy.data.armatures.new(name="MyArmature")

# 骨架属性
arm.name  # 骨架名称
arm.display_type  # 显示类型
arm.show_names  # 显示名称

# 创建骨架对象
arm_obj = bpy.data.objects.new(name="Armature", object_data=arm)
bpy.context.collection.objects.link(arm_obj)
```

## Bone (骨骼)

骨骼是骨架的组成部分。

```python
# 获取骨骼
arm = bpy.context.active_object.data
bone = arm.bones.get("Bone")

# 骨骼属性
bone.name  # 骨骼名称
bone.head  # 头部位置
bone.tail  # 尾部位置
bone.roll  # 滚动角度
bone.parent  # 父骨骼
bone.use_connect  # 连接到父骨骼
bone.use_deform  # 是否变形
bone.hide  # 隐藏
bone.select  # 选择
bone.select_head  # 选择头部
bone.select_tail  # 选择尾部

# 设置骨骼位置
bone.head = (0.0, 0.0, 0.0)
bone.tail = (0.0, 0.0, 1.0)
```

## Curve (曲线)

曲线用于创建 2D 和 3D 曲线。

```python
# 创建曲线
curve = bpy.data.curves.new(name="MyCurve", type='CURVE')

# 曲线属性
curve.name  # 曲线名称
curve.dimensions  # 维度 ('2D', '3D')
curve.resolution_u  # U 分辨率
curve.resolution_v  # V 分辨率
curve.bevel_depth  # 倒角深度
curve.bevel_resolution  # 倒角分辨率
curve.extrude  # 挤出深度
curve.taper_radius  # 锥化半径

# 添加样条
spline = curve.splines.new(type='BEZIER')
spline.bezier_points.add(1)

# 设置点位置
spline.bezier_points[0].co = (0.0, 0.0, 0.0)
spline.bezier_points[0].handle_left = (-1.0, 0.0, 0.0)
spline.bezier_points[0].handle_right = (1.0, 0.0, 0.0)
spline.bezier_points[1].co = (2.0, 0.0, 0.0)
spline.bezier_points[1].handle_left = (1.0, 0.0, 0.0)
spline.bezier_points[1].handle_right = (3.0, 0.0, 0.0)

# 创建曲线对象
curve_obj = bpy.data.objects.new(name="Curve", object_data=curve)
bpy.context.collection.objects.link(curve_obj)
```

## Modifier (修改器)

修改器用于修改对象的几何形状。

```python
# 获取修改器
obj = bpy.context.active_object
modifier = obj.modifiers.get("MyModifier")

# 创建修改器
modifier = obj.modifiers.new(name="Subsurf", type='SUBSURF')

# 修改器类型
modifier.type  # 修改器类型

# 细分表面修改器
modifier.levels  # 细分级别
modifier.render_levels  # 渲染细分级别

# 镜像修改器
modifier.use_mirror_x  # 镜像 X 轴
modifier.use_mirror_y  # 镜像 Y 轴
modifier.use_mirror_z  # 镜像 Z 轴
modifier.use_clip  # 裁剪

# 阵列修改器
modifier.count  # 数量
modifier.relative_offset_displace  # 相对偏移
modifier.constant_offset_displace  # 常量偏移

# 倒角修改器
modifier.width  # 宽度
modifier.segments  # 段数
modifier.profile  # 轮廓

# 实体化修改器
modifier.thickness  # 厚度
modifier.offset  # 偏移

# 应用修改器
bpy.ops.object.modifier_apply(modifier=modifier.name)

# 删除修改器
obj.modifiers.remove(modifier)
```

## NodeTree (节点树)

节点树用于着色器、合成和纹理。

```python
# 获取节点树
mat = bpy.data.materials.get("MyMaterial")
node_tree = mat.node_tree

# 节点树属性
node_tree.name  # 节点树名称
node_tree.nodes  # 节点列表
node_tree.links  # 连接列表

# 创建节点
node = node_tree.nodes.new(type='ShaderNodeBsdfPrincipled')
node.location = (200, 200)

# 获取节点
node = node_tree.nodes.get("Principled BSDF")

# 节点属性
node.name  # 节点名称
node.type  # 节点类型
node.location  # 位置
node.width  # 宽度
node.height  # 高度
node.inputs  # 输入列表
node.outputs  # 输出列表

# 访问输入
input = node.inputs['Base Color']
input.default_value = (1.0, 0.0, 0.0, 1.0)

# 访问输出
output = node.outputs['BSDF']

# 连接节点
node1 = node_tree.nodes.new(type='ShaderNodeBsdfPrincipled')
node2 = node_tree.nodes.new(type='ShaderNodeOutputMaterial')
node_tree.links.new(node1.outputs['BSDF'], node2.inputs['Surface'])

# 删除节点
node_tree.nodes.remove(node)
```

## Collection (集合)

集合用于组织对象。

```python
# 创建集合
collection = bpy.data.collections.new(name="MyCollection")
bpy.context.scene.collection.children.link(collection)

# 集合属性
collection.name  # 集合名称
collection.objects  # 对象列表
collection.children  # 子集合列表
collection.hide_viewport  # 在视口中隐藏
collection.hide_render  # 在渲染中隐藏

# 添加对象到集合
collection.objects.link(obj)

# 从集合移除对象
collection.objects.unlink(obj)

# 遍历集合中的对象
for obj in collection.objects:
    print(obj.name)

# 遍历子集合
for child in collection.children:
    print(child.name)
```

## RenderSettings (渲染设置)

渲染设置控制场景的渲染。

```python
# 获取渲染设置
render = bpy.context.scene.render

# 渲染属性
render.engine  # 渲染引擎 ('BLENDER_EEVEE', 'CYCLES')
render.resolution_x  # 宽度
render.resolution_y  # 高度
render.resolution_percentage  # 分辨率百分比
render.fps  # 帧率
render.filepath  # 输出路径
render.image_settings.file_format  # 文件格式
render.image_settings.color_mode  # 颜色模式
render.image_settings.color_depth  # 颜色深度
render.film_transparent  # 透明背景
render.use_compositing  # 使用合成
render.use_sequencer  # 使用序列编辑器
```

## SpaceView3D (3D 视图空间)

3D 视图空间控制 3D 视图的显示。

```python
# 获取 3D 视图空间
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        space = area.spaces.active
        break

# 3D 视图属性
space.shading.type  # 着色类型 ('WIREFRAME', 'SOLID', 'MATERIAL', 'RENDERED')
space.shading.show_xray  # 显示 X 射线
space.shading.xray_alpha  # X 射线透明度
space.overlay.show_overlays  # 显示覆盖层
space.overlay.show_floor  # 显示地面
space.overlay.show_axis  # 显示轴
space.region_3d.view_location  # 视图位置
space.region_3d.view_rotation  # 视图旋转
space.region_3d.view_distance  # 视图距离
```

## WindowManager (窗口管理器)

窗口管理器控制 Blender 窗口。

```python
# 获取窗口管理器
wm = bpy.context.window_manager

# 窗口管理器属性
wm.windows  # 窗口列表
wm.keyconfigs  # 键配置
wm.operators  # 操作符

# 添加操作符
wm.operators.add(op_name="my.operator")

# 移除操作符
wm.operators.remove(op_name="my.operator")
```


---
## bpy_types_detailed

# bpy.types - Blender Data Types Reference

Blender Python API 所有数据类型的完整参考，涵盖核心对象、场景元素和UI组件。

## Overview

`bpy.types` 模块定义了 Blender 中所有数据结构的类。这些类型继承自 `bpy_struct`，提供了访问和修改 Blender 数据的接口。

## 核心数据类型分类

### 场景与对象管理

```python
import bpy

# Scene - 场景数据容器
scene = bpy.context.scene
print(f"场景名称: {scene.name}")
print(f"当前帧: {scene.frame_current}")
print(f"帧范围: {scene.frame_start} - {scene.frame_end}")

# 设置场景属性
scene.render.engine = 'CYCLES'
scene.unit_settings.system = 'METRIC'

# Object - 对象是场景中的基本元素
obj = bpy.context.active_object
print(f"对象名称: {obj.name}")
print(f"对象类型: {obj.type}")  # MESH, CURVE, ARMATURE, etc.
print(f"对象位置: {obj.location}")
print(f"对象旋转: {obj.rotation_euler}")
print(f"对象缩放: {obj.scale}")

# 对象变换操作
obj.location = (1.0, 2.0, 3.0)
obj.rotation_euler = (0.0, 0.0, 0.785)  # 45度
obj.scale = (1.5, 1.5, 1.5)

# 对象层级关系
print(f"父对象: {obj.parent}")
print(f"子对象数量: {len(obj.children)}")

# Collection - 集合用于组织对象
collection = bpy.data.collections.new("MyCollection")
bpy.context.scene.collection.children.link(collection)
collection.objects.link(obj)
```

### 网格数据类型

```python
# Mesh - 网格几何数据
mesh = bpy.context.active_object.data
print(f"顶点数: {len(mesh.vertices)}")
print(f"边数: {len(mesh.edges)}")
print(f"面数: {len(mesh.polygons)}")

# 访问顶点数据
for i, vert in enumerate(mesh.vertices):
    print(f"顶点 {i}: {vert.co}")
    print(f"  法线: {vert.normal}")

# 访问边数据
for edge in mesh.edges:
    print(f"边: {edge.vertices}")

# 访问面数据
for poly in mesh.polygons:
    print(f"面 {poly.index}: 顶点 {poly.vertices}")
    print(f"  法线: {poly.normal}")
    print(f"  面积: {poly.area}")

# Mesh 属性
mesh.use_auto_smooth = True
mesh.shade_smooth()

# UV层
uv_layer = mesh.uv_layers.new(name="UVMap")
for loop in mesh.loops:
    uv = uv_layer.data[loop.index].uv
    print(f"UV: {uv}")

# 顶点颜色层
vcol_layer = mesh.vertex_colors.new(name="Col")
for poly in mesh.polygons:
    for loop_idx in poly.loop_indices:
        color = vcol_layer.data[loop_idx].color
        print(f"颜色: {color}")
```

### 材质与着色

```python
# Material - 材质定义
mat = bpy.data.materials.new(name="MyMaterial")
mat.use_nodes = True

# 获取节点树
nodes = mat.node_tree.nodes
links = mat.node_tree.links

# 创建 Principled BSDF 材质
principled = nodes.get("Principled BSDF")
if principled:
    principled.inputs["Base Color"].default_value = (1.0, 0.2, 0.2, 1.0)
    principled.inputs["Metallic"].default_value = 0.5
    principled.inputs["Roughness"].default_value = 0.3

# 创建新材质槽
obj.data.materials.append(mat)

# 获取活动材质
if obj.active_material:
    print(f"活动材质: {obj.active_material.name}")

# 纹理
tex = bpy.data.textures.new(name="MyTexture", type='CLOUDS')
tex.noise_scale = 2.0
```

### 曲线与曲面

```python
# Curve - 曲线数据
curve = bpy.context.active_object.data
print(f"曲线类型: {curve.type}")  # BEZIER, POLY, SURFACE, etc.

# Bezier曲线
if curve.type == 'BEZIER':
    for spline in curve.splines:
        print(f"点数: {len(spline.points)}")
        for point in spline.points:
            print(f"  坐标: {point.co}")
            print(f"  句柄: {point.handle_type}")

# NURBS曲线
if curve.type == 'NURBS':
    for spline in curve.splines:
        print(f"阶数: {spline.order_u}")
        print(f"闭合: {spline.use_cyclic_u}")

# Surface - 曲面
surface = bpy.context.active_object.data
print(f"U向点数: {surface.point_count_u}")
print(f"V向点数: {surface.point_count_v}")
```

### 骨骼与动画

```python
# Armature - 骨骼系统
armature = bpy.context.active_object.data
print(f"骨骼数量: {len(armature.bones)}")

# 遍历骨骼
for bone in armature.bones:
    print(f"骨骼: {bone.name}")
    print(f"  头部: {bone.head}")
    print(f"  尾部: {bone.tail}")
    print(f"  长度: {bone.length}")

# Edit Bones (编辑模式)
if armature.edit_bones:
    for bone in armature.edit_bones:
        print(f"编辑骨骼: {bone.name}")

# Pose - 姿态数据
pose = bpy.context.active_object.pose
for pose_bone in pose.bones:
    print(f"姿态骨骼: {pose_bone.name}")
    print(f"  位置: {pose_bone.location}")
    print(f"  旋转: {pose_bone.rotation_quaternion}")
    print(f"  缩放: {pose_bone.scale}")

# 约束
for constraint in pose_bone.constraints:
    print(f"约束: {constraint.name}")
    print(f"  类型: {constraint.type}")
```

### 相机与灯光

```python
# Camera - 相机
camera = bpy.context.active_object.data
print(f"相机类型: {camera.type}")
print(f"焦距: {camera.lens}")
print(f"传感器尺寸: {camera.sensor_width}")

# 相机变换
camera.shift_x = 0.1
camera.shift_y = -0.05

# 景深设置
camera.dof.use_dof = True
camera.dof.focus_distance = 5.0
camera.dof.aperture_fstop = 2.8

# Light - 灯光
light = bpy.context.active_object.data
print(f"灯光类型: {light.type}")  # POINT, SUN, SPOT, AREA
print(f"功率: {light.energy}")
print(f"颜色: {light.color}")

# 设置灯光属性
if light.type == 'SPOT':
    light.spot_size = 0.5  # 弧度
    light.spot_blend = 0.5
    light.falloff_type = 'INVERSE_LINEAR'
```

### 节点与合成

```python
# NodeTree - 节点树
node_tree = bpy.context.active_object.data.node_tree
print(f"节点数: {len(node_tree.nodes)}")

# 遍历节点
for node in node_tree.nodes:
    print(f"节点: {node.name}")
    print(f"  类型: {node.type}")
    print(f"  位置: {node.location}")

# 创建节点
new_node = node_tree.nodes.new(type='ShaderNodeMixRGB')
new_node.location = (200, 300)
new_node.blend_type = 'MIX'
new_node.inputs[0].default_value = 0.5

# 连接节点
links = node_tree.links
link = links.new(output_node.outputs[0], input_node.inputs[0])

# 合成节点树
comp_tree = bpy.context.scene.node_tree
render_layers = comp_tree.nodes.get("Render Layers")
composite = comp_tree.nodes.get("Composite")
if render_layers and composite:
    links.new(render_layers.outputs[0], composite.inputs[0])
```

### UI 组件类型

```python
# Panel - UI面板
class MyPanel(bpy.types.Panel):
    bl_label = "我的面板"
    bl_idname = "OBJECT_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '工具'

    def draw(self, context):
        layout = self.layout
        layout.label(text="Hello World")

# Menu - UI菜单
class MyMenu(bpy.types.Menu):
    bl_idname = "OBJECT_MT_my_menu"
    bl_label = "我的菜单"

    def draw(self, context):
        layout = self.layout
        layout.operator("object.select_all", text="全选")
        layout.operator("object.delete", text="删除")

# Operator - 操作符
class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "我的操作符"

    def execute(self, context):
        print("操作执行")
        return {'FINISHED'}

# UIList - UI列表
class MyList(bpy.types.UIList):
    def draw_item(self, context, layout, data, item, icon, active_data, active_propname):
        layout.label(text=item.name)
```

### 动画数据类型

```python
# Action - 动画动作
action = bpy.data.actions.new(name="MyAction")
print(f"帧范围: {action.frame_range}")

# F-Curves
for fcurve in action.fcurves:
    print(f"数据路径: {fcurve.data_path}")
    print(f"数组索引: {fcurve.array_index}")
    
    # 关键帧
    for keyframe in fcurve.keyframe_points:
        print(f"  帧: {keyframe.co.x}, 值: {keyframe.co.y}")

# AnimData - 动画数据
anim_data = obj.animation_data
if anim_data:
    print(f"动作: {anim_data.action}")
    print(f"驱动: {len(anim_data.drivers)}")

# Drivers - 驱动
for driver in anim_data.drivers:
    print(f"驱动数据路径: {driver.data_path}")
    print(f"数组索引: {driver.array_index}")

# NLA Tracks
for track in anim_data.nla_tracks:
    print(f"NLA轨道: {track.name}")
    for strip in track.strips:
        print(f"  条带: {strip.name}")
        print(f"  帧范围: {strip.frame_start} - {strip.frame_end}")
```

## 重要类型详细说明

### ID 类型层次

```python
# ID 是所有可识别数据的基础类型
# 所有 bpy.types.* 都继承自 bpy_struct -> ID

# ID 属性
mat = bpy.data.materials[0]
print(f"名称: {mat.name}")
print(f"库: {mat.library}")
print(f"用户数: {mat.users}")
print(f"fake用户: {mat.use_fake_user}")

# 复制数据
new_mat = mat.copy()
new_mat.name = mat.name + ".001"

# 用户管理
mat.use_fake_user = True
bpy.data.materials.remove(mat)

# 标签
mat.tag = True
mat.tags.clear()
```

### 集合管理

```python
# 场景集合
scene_collection = bpy.context.scene.collection
print(f"子集合数: {len(scene_collection.children)}")
print(f"直接对象数: {len(scene_collection.objects)}")

# 遍历所有集合
for collection in bpy.data.collections:
    print(f"集合: {collection.name}")
    print(f"  对象数: {len(collection.objects)}")
    for obj in collection.objects:
        print(f"    - {obj.name}")

# 链接和取消链接
collection.objects.link(obj)
collection.objects.unlink(obj)

# 集合操作
new_collection = bpy.data.collections.new("Test")
scene.collection.children.link(new_collection)
bpy.data.collections.remove(new_collection)
```

## 常见模式

### 安全的数据访问

```python
def safe_get_material(obj, slot=0):
    """安全获取材质"""
    if obj and obj.data and obj.data.materials:
        return obj.data.materials[slot]
    return None

def get_active_object_data():
    """获取活动对象的数据"""
    obj = bpy.context.active_object
    if obj and obj.data:
        return obj.data
    return None
```

### 批量修改数据

```python
def batch_rename_objects(prefix):
    """批量重命名对象"""
    for obj in bpy.context.selected_objects:
        obj.name = f"{prefix}_{obj.name}"

def batch_apply_modifiers():
    """批量应用修改器"""
    for obj in bpy.context.selected_objects:
        if obj.type == 'MESH':
            for modifier in obj.modifiers:
                bpy.context.view_layer.objects.active = obj
                bpy.ops.object.modifier_apply(modifier=modifier.name)
```

## 性能注意事项

1. **避免频繁的 ID 查找**：使用引用而不是重复查找
2. **批量操作**：使用 bmesh 进行大量网格操作
3. **及时清理**：删除不再需要的数据
4. **使用 eval_depsgraph**：正确处理依赖图

## 相关模块

- [bpy.data](bpy_data_module.md) - 数据访问接口
- [bpy.context](bpy_context_module.md) - 上下文访问
- [bpy.ops](bpy_ops.md) - 操作符系统
- [bpy.props](bpy_props_module.md) - 属性定义


---
## bpy_data_inspection

# 数据检查与调试模块

## 数据检查概述

Blender提供了多种数据检查和调试工具，帮助开发者分析场景数据、调试几何节点、查看属性值。这些工具对于理解Blender内部数据结构、排查问题和优化性能非常重要。

```python
# 数据检查工具
# 大纲编辑器 - 查看场景层级结构
# 属性编辑器 - 查看和编辑对象属性
# 电子表格编辑器 - 查看几何体属性
# Python控制台 - 交互式代码执行
# 信息编辑器 - 查看操作日志

# 调试方法
# 打印变量值
# 使用断点
# 查看堆栈跟踪
# 分析性能瓶颈

# 数据访问方式
# Python API访问
# 内置工具查看
# 导出数据分析
```

## 大纲编辑器

### 大纲视图操作

大纲编辑器显示blend文件中的所有数据，允许您管理对象、集合和数据块。

```python
# 访问大纲编辑器
space = bpy.context.area.spaces[0]
if space.type == 'OUTLINER':
    outliner = space

# 设置显示模式
outliner.display_mode = 'DATA'  # 数据模式
outliner.display_mode = 'OBJECT'  # 对象模式
outliner.display_mode = 'COLLECTIONS'  # 集合模式
outliner.display_mode = 'VIEW_LAYER'  # 视图层模式
outliner.display_mode = 'LIBRARY_OVERRIDES'  # 库覆盖模式
outliner.display_mode = '_ORPHANS'  # 未使用数据

# 设置筛选类型
outliner.filter_id_type = 'OBJECT'  # 对象
outliner.filter_id_type = 'MESH'  # 网格
outliner.filter_id_type = 'MATERIAL'  # 材质
outliner.filter_id_type = 'TEXTURE'  # 纹理
outliner.filter_id_type = 'ACTION'  # 动作

# 搜索筛选
outliner.search_query = "Cube"
outliner.use_filter_case_sensitive = True
outliner.use_filter_complete = False

# 展开/折叠操作
bpy.ops.outliner.expanded_toggle()
bpy.ops.outliner.show_hierarchy()

# 刷新大纲
bpy.ops.outliner.operatingspace_refresh()
```

### 大纲数据访问

```python
# 遍历场景对象
for obj in bpy.context.scene.objects:
    print(f"对象: {obj.name}, 类型: {obj.type}")

# 遍历集合
for collection in bpy.data.collections:
    print(f"集合: {collection.name}")
    for obj in collection.objects:
        print(f"  - {obj.name}")

# 遍历视图层
for view_layer in bpy.context.scene.view_layers:
    print(f"视图层: {view_layer.name}")

# 遍历数据块
for mesh in bpy.data.meshes:
    print(f"网格: {mesh.name}, 顶点数: {len(mesh.vertices)}")

for material in bpy.data.materials:
    print(f"材质: {material.name}, 用户数: {material.users}")

for texture in bpy.data.textures:
    print(f"纹理: {texture.name}, 类型: {texture.type}")
```

## 电子表格编辑器

### 电子表格基础

电子表格编辑器用于检查活动对象的几何属性，通常用于调试几何节点。

```python
# 访问电子表格编辑器
for area in bpy.context.screen.areas:
    if area.type == 'SPREADSHEET':
        spreadsheet = area.spaces[0]
        break

# 设置显示选项
spreadsheet.show_only_selected = True  # 只显示选定元素
spreadsheet.use_filter = False  # 使用筛选
spreadsheet.show_internal_attributes = False  # 显示内部属性

# 设置显示的数据集
# 几何节点评估后的数据
spreadsheet.context_attr = 'geometry'

# 设置显示的数据域
spreadsheet.attribute_domain = 'POINT'  # 点域
spreadsheet.attribute_domain = 'EDGE'  # 边域
spreadsheet.attribute_domain = 'FACE'  # 面域
spreadsheet.attribute_domain = 'CORNER'  # 角域
spreadsheet.attribute_domain = 'SPLINE'  # 样条域
spreadsheet.attribute_domain = 'INSTANCE'  # 实例域

# 选择属性显示
spreadsheet.pin = True  # 固定当前视图
```

### 电子表格操作

```python
# 调整列宽
bpy.ops.spreadsheet.resize_column(column_index=0, width=100)

# 自动调整列宽
bpy.ops.spreadsheet.fit_column(column_index=0)

# 重新排列列
bpy.ops.spreadsheet.reorder_columns(
    column_indices=[0, 2, 1, 3]
)

# 获取显示的数据
# 电子表格显示的是评估后的几何数据
# 需要通过几何节点上下文访问

# 使用Python获取类似数据
import bpy
obj = bpy.context.active_object

# 获取几何数据
if obj.mode == 'EDIT':
    geo = obj.data
else:
    # 在对象模式下，需要通过依赖图获取评估后的数据
    depsgraph = bpy.context.evaluated_depsgraph_get()
    eval_obj = obj.evaluated_get(depsgraph)
    geo = eval_obj.to_mesh()
```

### 查看几何属性

```python
# 查看顶点数据
for i, vert in enumerate(geo.vertices):
    print(f"顶点 {i}: 坐标={vert.co}, 法线={vert.normal}")

# 查看边数据
for i, edge in enumerate(geo.edges):
    print(f"边 {i}: 顶点={edge.vertices}, 长度={edge.length}")

# 查看面数据
for i, poly in enumerate(geo.polygons):
    print(f"面 {i}: 顶点={poly.vertices}, 法线={poly.normal}, 面积={poly.area}")

# 查看UV层
for uv_layer in geo.uv_layers:
    print(f"UV层: {uv_layer.name}")
    for i, loop in enumerate(uv_layer.data):
        print(f"  角 {i}: UV={loop.uv}")

# 查看顶点颜色层
for color_layer in geo.vertex_colors:
    print(f"顶点颜色层: {color_layer.name}")
    for i, col in enumerate(color_layer.data):
        print(f"  角 {i}: 颜色={col.color}")
```

## 属性检查

### 属性访问

```python
# 获取活动对象属性
obj = bpy.context.active_object

# 获取变换属性
print(f"位置: {obj.location}")
print(f"旋转: {obj.rotation_euler}")
print(f"缩放: {obj.scale}")

# 获取对象数据属性
if obj.type == 'MESH':
    print(f"网格顶点数: {len(obj.data.vertices)}")
    print(f"网格面数: {len(obj.data.polygons)}")
elif obj.type == 'CAMERA':
    print(f"焦距: {obj.data.lens}")
    print(f"传感器: {obj.data.sensor_width}")
elif obj.type == 'LIGHT':
    print(f"灯光类型: {obj.data.type}")
    print(f"灯光功率: {obj.data.energy}")

# 获取材质属性
for i, slot in enumerate(obj.material_slots):
    print(f"材质槽 {i}: {slot.name}")
    mat = slot.material
    if mat:
        print(f"  使用节点: {mat.use_nodes}")
        if mat.use_nodes:
            for node in mat.node_tree.nodes:
                print(f"    节点: {node.name}, 类型: {node.type}")
```

### 属性搜索

```python
# 搜索特定属性
def find_properties(obj, search_term):
    results = []
    for prop_name in dir(obj):
        if search_term.lower() in prop_name.lower():
            try:
                value = getattr(obj, prop_name)
                if not callable(value):
                    results.append((prop_name, value))
            except:
                pass
    return results

# 使用示例
results = find_properties(bpy.context.active_object, "scale")
for name, value in results:
    print(f"{name}: {value}")

# 搜索所有数据块的属性
def search_all_properties(search_term):
    results = []
    for collection in [bpy.data.objects, bpy.data.meshes, 
                       bpy.data.materials, bpy.data.textures]:
        for item in collection:
            props = find_properties(item, search_term)
            for prop_name, value in props:
                results.append((item.name, prop_name, value))
    return results
```

## Python控制台

### 控制台操作

```python
# 执行Python代码
# 在Python控制台中直接输入代码
# 结果会立即显示

# 使用bpy模块
import bpy
bpy.context.scene.frame_current

# 使用快捷属性
C = bpy.context
D = bpy.data
S = bpy.context.scene

# 执行多行代码
# 使用Ctrl+Enter换行

# 历史命令
# 上/下箭头浏览历史

# 自动补全
# Tab键自动补全属性和方法

# 复制粘贴
# Ctrl+C复制
# Ctrl+V粘贴
```

### 控制台脚本

```python
# 批量获取信息
def get_scene_info():
    info = {
        "对象数量": len(bpy.data.objects),
        "网格数量": len(bpy.data.meshes),
        "材质数量": len(bpy.data.materials),
        "纹理数量": len(bpy.data.textures),
        "场景名称": bpy.context.scene.name,
        "当前帧": bpy.context.scene.frame_current
    }
    return info

# 打印对象层次结构
def print_hierarchy(obj, indent=0):
    print("  " * indent + obj.name)
    for child in obj.children:
        print_hierarchy(child, indent + 1)

# 分析网格数据
def analyze_mesh(mesh):
    stats = {
        "顶点数": len(mesh.vertices),
        "边数": len(mesh.edges),
        "面数": len(mesh.polygons),
        "三角形数": sum(1 for p in mesh.polygons if len(p.vertices) == 3),
        "四边形数": sum(1 for p in mesh.polygons if len(p.vertices) == 4),
        "多边形数": sum(1 for p in mesh.polygons if len(p.vertices) > 4)
    }
    return stats
```

## 信息编辑器

### 查看操作日志

```python
# 查看信息编辑器内容
for area in bpy.context.screen.areas:
    if area.type == 'INFO':
        for space in area.spaces:
            if space.type == 'INFO':
                info_space = space
                break

# 获取操作历史
# 信息编辑器显示历史操作
# 可以查看错误消息和警告

# 获取警告
warnings = [line for line in info_space.lines if "Warning" in line]
for warning in warnings:
    print(warning.body)

# 获取错误
errors = [line for line in info_space.lines if "Error" in line]
for error in errors:
    print(error.body)
```

### 操作统计

```python
# 统计操作类型
def count_operations():
    op_counts = {}
    for line in info_space.lines:
        if line.op_type:
            op_type = line.op_type
            op_counts[op_type] = op_counts.get(op_type, 0) + 1
    return op_counts

# 获取最近操作
def get_recent_operations(count=10):
    recent = []
    for line in info_space.lines[-count:]:
        if line.op_type:
            recent.append((line.op_type, line.body))
    return recent
```

## 调试技术

### 打印调试

```python
# 打印变量
value = 42
print(f"值: {value}")

# 打印对象信息
obj = bpy.context.active_object
print(f"对象名称: {obj.name}")
print(f"对象类型: {obj.type}")
print(f"对象位置: {obj.location}")

# 打印列表
items = [1, 2, 3, 4, 5]
print(f"列表: {items}")

# 打印字典
data = {"key1": "value1", "key2": "value2"}
print(f"字典: {data}")

# 格式化输出
print(f"帧: {bpy.context.scene.frame_current}/{bpy.context.scene.frame_end}")
print(f"多边形: {len(bpy.context.active_object.data.polygons)}")
```

### 断点调试

```python
# 使用pdb进行断点调试
import pdb

def my_function():
    # 设置断点
    pdb.set_trace()
    
    # 代码执行到这里会暂停
    result = some_calculation()
    return result

# 设置条件断点
# 在pdb中使用:
# (pdb) condition <breakpoint_number> <condition>

# 查看变量
# (pdb) pp variable_name

# 继续执行
# (pdb) c (continue)

# 单步执行
# (pdb) s (step)

# 继续到下一个断点
# (pdb) n (next)
```

### 异常捕获

```python
# 捕获异常
try:
    # 可能出错的代码
    result = obj.data.vertices[1000].co
except IndexError as e:
    print(f"索引错误: {e}")
except AttributeError as e:
    print(f"属性错误: {e}")
except Exception as e:
    print(f"其他错误: {e}")

# 记录异常
import traceback

try:
    risky_operation()
except Exception as e:
    print("发生异常:")
    traceback.print_exc()

# 重新抛出异常
try:
    operation()
except:
    print("处理异常")
    raise
```

## 性能分析

### 时间测量

```python
import time

# 简单时间测量
start = time.time()
# 执行操作
result = heavy_calculation()
end = time.time()
print(f"耗时: {end - start:.3f}秒")

# 使用context manager
class Timer:
    def __enter__(self):
        self.start = time.time()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.end = time.time()
        print(f"耗时: {self.end - self.start:.3f}秒")

# 使用
with Timer():
    heavy_operation()

# 更精确的时间测量
import timeit

def my_function():
    return sum(range(1000))

# 多次执行取平均
time = timeit.timeit(my_function, number=1000)
print(f"平均耗时: {time/1000:.6f}秒")
```

### 内存分析

```python
import sys

# 获取对象大小
obj = bpy.context.active_object
size = sys.getsizeof(obj)
print(f"对象大小: {size}字节")

# 获取更详细的大小信息
def get_size(obj):
    if hasattr(obj, '__dict__'):
        return sys.getsizeof(obj.__dict__)
    return sys.getsizeof(obj)

# 检查内存使用
import psutil
process = psutil.Process()
memory_info = process.memory_info()
print(f"内存使用: {memory_info.rss / 1024 / 1024:.2f}MB")
print(f"虚拟内存: {memory_info.vms / 1024 / 1024:.2f}MB")

# 跟踪内存增长
def track_memory(func):
    def wrapper(*args, **kwargs):
        before = process.memory_info().rss
        result = func(*args, **kwargs)
        after = process.memory_info().rss
        print(f"内存增长: {(after - before) / 1024 / 1024:.2f}MB")
        return result
    return wrapper
```

### 对象统计

```python
# 统计场景中的对象
def count_scene_objects():
    counts = {
        "总对象": len(bpy.context.scene.objects),
        "网格对象": sum(1 for o in bpy.context.scene.objects if o.type == 'MESH'),
        "曲线对象": sum(1 for o in bpy.context.scene.objects if o.type == 'CURVE'),
        "相机对象": sum(1 for o in bpy.context.scene.objects if o.type == 'CAMERA'),
        "灯光对象": sum(1 for o in bpy.context.scene.objects if o.type == 'LIGHT'),
        "空对象": sum(1 for o in bpy.context.scene.objects if o.type == 'EMPTY'),
        "骨架对象": sum(1 for o in bpy.context.scene.objects if o.type == 'ARMATURE')
    }
    return counts

# 统计数据块
def count_data_blocks():
    counts = {
        "网格": len(bpy.data.meshes),
        "材质": len(bpy.data.materials),
        "纹理": len(bpy.data.textures),
        "图像": len(bpy.data.images),
        "动作": len(bpy.data.actions),
        "曲线": len(bpy.data.curves),
        "骨骼": len(bpy.data.armatures),
        "相机": len(bpy.data.cameras),
        "灯光": len(bpy.data.lights)
    }
    return counts

# 查找孤立数据块
def find_orphaned():
    orphaned = []
    for mesh in bpy.data.meshes:
        if mesh.users == 0:
            orphaned.append(mesh)
    return orphaned
```

## 日志记录

### 配置日志

```python
import logging

# 配置日志
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler("blender_debug.log"),
        logging.StreamHandler()
    ]
)

# 获取日志记录器
logger = logging.getLogger(__name__)

# 使用日志
logger.debug("调试信息")
logger.info("普通信息")
logger.warning("警告")
logger.error("错误")

# 设置级别
logger.setLevel(logging.WARNING)
```

### Blender日志

```python
# 启用Blender调试输出
import bpy

# 设置日志级别
bpy.app.debug = True
bpy.app.debug_wm = True
bpy.app.debug_python = True

# 使用--debug参数启动
# blender --debug --debug-python

# 查看控制台输出
# 所有print语句和日志都会显示在控制台

# 自定义日志处理
def custom_log(message):
    print(f"[Custom] {message}")

# 注册日志回调
# bpy.app.handlers.log_post.append(custom_log)
```

## 数据导出分析

### 导出数据

```python
# 导出场景统计到CSV
import csv

def export_scene_stats(filepath):
    with open(filepath, 'w', newline='') as f:
        writer = csv.writer(f)
        writer.writerow(['名称', '类型', '顶点数', '面数', '材质'])
        
        for obj in bpy.context.scene.objects:
            if obj.type == 'MESH':
                mesh = obj.data
                writer.writerow([
                    obj.name,
                    obj.type,
                    len(mesh.vertices),
                    len(mesh.polygons),
                    len(obj.material_slots)
                ])

# 导出属性到JSON
import json

def export_properties(obj, filepath):
    data = {
        'name': obj.name,
        'type': obj.type,
        'location': list(obj.location),
        'rotation': list(obj.rotation_euler),
        'scale': list(obj.scale),
        'dimensions': list(obj.dimensions)
    }
    
    with open(filepath, 'w') as f:
        json.dump(data, f, indent=2)
```

### 导入数据分析

```python
# 读取CSV文件
import csv

def import_scene_stats(filepath):
    with open(filepath, 'r') as f:
        reader = csv.DictReader(f)
        for row in reader:
            print(f"对象: {row['名称']}, 类型: {row['类型']}")

# 读取JSON文件
def load_properties(filepath):
    with open(filepath, 'r') as f:
        data = json.load(f)
    return data
```

## 最佳实践

### 调试工作流

```python
# 系统化调试步骤
def debug_scene():
    print("=== 场景调试开始 ===")
    
    # 1. 打印基本信息
    print(f"帧范围: {bpy.context.scene.frame_start}-{bpy.context.scene.frame_end}")
    print(f"当前帧: {bpy.context.scene.frame_current}")
    print(f"对象数量: {len(bpy.context.scene.objects)}")
    
    # 2. 检查活动对象
    obj = bpy.context.active_object
    if obj:
        print(f"活动对象: {obj.name}")
        print(f"对象类型: {obj.type}")
        
        if obj.type == 'MESH':
            print(f"顶点数: {len(obj.data.vertices)}")
            print(f"面数: {len(obj.data.polygons)}")
    
    # 3. 检查材质
    for slot in obj.material_slots:
        if slot.material:
            print(f"材质: {slot.material.name}")
    
    print("=== 场景调试结束 ===")

# 使用调试函数
debug_scene()
```

### 文档记录

```python
# 记录场景状态
def document_scene_state(filepath="scene_report.txt"):
    with open(filepath, 'w') as f:
        f.write("场景状态报告\n")
        f.write("=" * 50 + "\n\n")
        
        f.write("基本信息\n")
        f.write("-" * 20 + "\n")
        f.write(f"场景名称: {bpy.context.scene.name}\n")
        f.write(f"帧范围: {bpy.context.scene.frame_start}-{bpy.context.scene.frame_end}\n")
        f.write(f"当前帧: {bpy.context.scene.frame_current}\n\n")
        
        f.write("对象列表\n")
        f.write("-" * 20 + "\n")
        for obj in bpy.context.scene.objects:
            f.write(f"- {obj.name} ({obj.type})\n")
            if obj.type == 'MESH':
                f.write(f"  顶点数: {len(obj.data.vertices)}\n")
                f.write(f"  面数: {len(obj.data.polygons)}\n")
        
        f.write("\n材质列表\n")
        f.write("-" * 20 + "\n")
        for mat in bpy.data.materials:
            f.write(f"- {mat.name} (用户数: {mat.users})\n")
```

### 版本控制

```python
# 场景版本信息
def get_scene_version():
    info = {
        "blender_version": bpy.app.version_string,
        "file_path": bpy.data.filepath or "未保存",
        "save_date": None,
        "scene_objects": len(bpy.context.scene.objects),
        "data_meshes": len(bpy.data.meshes),
        "data_materials": len(bpy.data.materials)
    }
    
    if bpy.data.filepath:
        import os
        info["save_date"] = os.path.getmtime(bpy.data.filepath)
    
    return info

# 比较场景变化
def compare_scenes(old_info, new_info):
    changes = []
    
    if old_info["scene_objects"] != new_info["scene_objects"]:
        changes.append(f"对象数量: {old_info['scene_objects']} -> {new_info['scene_objects']}")
    
    if old_info["data_meshes"] != new_info["data_meshes"]:
        changes.append(f"网格数量: {old_info['data_meshes']} -> {new_info['data_meshes']}")
    
    return changes
```

## 常见问题解决

### 访问问题

```python
# 对象不存在
try:
    obj = bpy.data.objects["NonExistent"]
except KeyError:
    print("对象不存在")

# 使用get方法
obj = bpy.data.objects.get("NonExistent")
if obj:
    print(f"找到对象: {obj.name}")
else:
    print("对象不存在")

# 检查对象类型
obj = bpy.context.active_object
if obj and obj.type == 'MESH':
    print(f"网格顶点数: {len(obj.data.vertices)}")
else:
    print("活动对象不是网格")

# 检查模式
if bpy.context.active_object:
    if bpy.context.active_object.mode == 'EDIT':
        print("在编辑模式")
    else:
        print("在对象模式")
```

### 性能问题

```python
# 识别性能瓶颈
def profile_operations():
    operations = []
    
    for i in range(10):
        start = time.time()
        
        # 执行测试操作
        for obj in bpy.context.scene.objects:
            if obj.type == 'MESH':
                _ = len(obj.data.vertices)
        
        end = time.time()
        operations.append(end - start)
    
    avg_time = sum(operations) / len(operations)
    print(f"平均耗时: {avg_time:.4f}秒")
    print(f"最快: {min(operations):.4f}秒")
    print(f"最慢: {max(operations):.4f}秒")

# 优化建议
# 减少对象数量
# 使用修饰符减少几何体
# 批量操作代替循环
# 使用引用而非复制
```


