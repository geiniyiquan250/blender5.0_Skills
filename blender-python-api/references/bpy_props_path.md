# bpy_props_path

---
## bpy_props

# bpy.props 属性定义模块

定义用于扩展 Blender 内部数据的属性。这些函数的返回值用于给 Blender 注册的类分配属性，不能直接在其他上下文中使用。

**重要提示：** 所有参数必须作为关键字参数传递。

## 数值属性

### FloatProperty

```python
import bpy

bpy.types.Material.custom_float = bpy.props.FloatProperty(
    name="浮点属性",
    description="这是一个浮点数值属性",
    default=0.5,
    min=0.0,
    max=1.0,
    step=1,
    precision=2,
    unit='LENGTH',
    update=None,
    get=None,
    set=None
)
```

定义浮点类型属性。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `default` (float): 默认值。
- `min` (float): 最小值。
- `max` (float): 最大值。
- `soft_min` (float): 软限制最小值。
- `soft_max` (float): 软限制最大值。
- `step` (int): 步进值。
- `precision` (int): 小数位数。
- `unit` (str): 单位类型，可选 'LENGTH'、'AREA'、'VOLUME'、'ROTATION'、'TIME'、'VELOCITY'、'ACCELERATION'。
- `update` (Callable): 值更新时的回调函数。
- `get` (Callable): 获取属性值的函数。
- `set` (Callable): 设置属性值的函数。

---

### IntProperty

```python
import bpy

bpy.types.Object.custom_int = bpy.props.IntProperty(
    name="整型属性",
    description="这是一个整型数值属性",
    default=10,
    min=0,
    max=100,
    step=1,
    update=None
)
```

定义整型属性。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `default` (int): 默认值。
- `min` (int): 最小值。
- `max` (int): 最大值。
- `soft_min` (int): 软限制最小值。
- `soft_max` (int): 软限制最大值。
- `step` (int): 步进值。
- `update` (Callable): 值更新时的回调函数。

---

### BoolProperty

```python
import bpy

bpy.types.Object.custom_bool = bpy.props.BoolProperty(
    name="布尔属性",
    description="这是一个布尔类型属性",
    default=True,
    update=None
)
```

定义布尔类型属性（复选框形式）。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `default` (bool): 默认值。
- `update` (Callable): 值更新时的回调函数。

---

## 字符串属性

### StringProperty

```python
import bpy

bpy.types.Object.custom_string = bpy.props.StringProperty(
    name="字符串属性",
    description="这是一个字符串属性",
    default="默认值",
    maxlen=255,
    subtype='FILE_PATH',
    update=None
)
```

定义字符串类型属性。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `default` (str): 默认值。
- `maxlen` (int): 最大长度限制。
- `subtype` (str): 子类型，可选 'NONE'、'FILE_PATH'、'DIR_PATH'、'FILE_NAME'、'BYTE_STRING'、'PASSWORD'。
- `update` (Callable): 值更新时的回调函数。

---

## 枚举属性

### EnumProperty

```python
import bpy

items = [
    ("OPTION_A", "选项A", "这是选项A的描述"),
    ("OPTION_B", "选项B", "这是选项B的描述"),
    ("OPTION_C", "选项C", "这是选项C的描述"),
]

bpy.types.Object.custom_enum = bpy.props.EnumProperty(
    name="枚举属性",
    description="这是一个枚举选择属性",
    items=items,
    default="OPTION_A",
    update=None
)
```

定义枚举类型属性（下拉菜单形式）。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `items` (Sequence): 枚举项列表，每项为元组 (identifier, name, description, icon, number)。
- `default` (str | int | Sequence): 默认值。
- `update` (Callable): 值更新时的回调函数。

**items 格式说明：**

- `identifier` (str): 枚举项的唯一标识符。
- `name` (str): 显示名称。
- `description` (str): 描述信息。
- `icon` (str | int): 图标名称或图标常量。
- `number` (int): 枚举值（通常与 identifier 相同）。

---

## 指针属性

### PointerProperty

```python
import bpy

class MaterialSettings(bpy.types.PropertyGroup):
    my_float: bpy.props.FloatProperty(name="浮点值", default=0.5)
    my_string: bpy.props.StringProperty(name="字符串", default="")

bpy.utils.register_class(MaterialSettings)

bpy.types.Material.custom_settings = bpy.props.PointerProperty(
    name="设置组",
    description="指向自定义设置组的指针",
    type=MaterialSettings
)
```

定义指向其他类型的指针属性。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `type` (type): 指向的类型，必须是 PropertyGroup 或其他 bpy_struct 子类。

---

### CollectionProperty

```python
import bpy

class SceneItem(bpy.types.PropertyGroup):
    name: bpy.props.StringProperty(name="名称", default="")
    value: bpy.props.IntProperty(name="值", default=0)

bpy.utils.register_class(SceneItem)

bpy.types.Scene.custom_collection = bpy.props.CollectionProperty(
    name="集合",
    description="自定义项目集合",
    type=SceneItem
)
```

定义项目集合属性。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `type` (type): 集合元素的类型，必须是 PropertyGroup 子类。

---

## 浮点向量属性

### FloatVectorProperty

```python
import bpy

bpy.types.Object.custom_vector = bpy.props.FloatVectorProperty(
    name="向量属性",
    description="这是一个三维向量属性",
    size=3,
    default=(0.0, 0.0, 0.0),
    min=0.0,
    max=1.0,
    step=1,
    precision=2,
    unit='LENGTH',
    update=None
)
```

定义浮点向量属性。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `size` (int): 向量维度（2、3 或 4）。
- `default` (Sequence[float]): 默认值。
- `min` (float): 最小值。
- `max` (float): 最大值。
- `soft_min` (float): 软限制最小值。
- `soft_max` (float): 软限制最大值。
- `step` (int): 步进值。
- `precision` (int): 小数位数。
- `unit` (str): 单位类型。
- `update` (Callable): 值更新时的回调函数。

---

### IntVectorProperty

```python
import bpy

bpy.types.Object.custom_int_vector = bpy.props.IntVectorProperty(
    name="整型向量",
    description="这是整型向量属性",
    size=3,
    default=(0, 0, 0),
    min=0,
    max=255
)
```

定义整型向量属性。

**参数：**

- `name` (str): 属性显示名称。
- `description` (str): 属性描述信息。
- `size` (int): 向量维度。
- `default` (Sequence[int]): 默认值。
- `min` (int): 最小值。
- `max` (int): 最大值。
- `soft_min` (int): 软限制最小值。
- `soft_max` (int): 软限制最大值。

---

## 使用示例

### 操作符属性定义

```python
import bpy

class OBJECT_OT_custom_operator(bpy.types.Operator):
    bl_idname = "object.custom_operator"
    bl_label = "自定义操作符"
    bl_options = {'REGISTER', 'UNDO'}

    my_float: bpy.props.FloatProperty(
        name="浮点值",
        default=0.5,
        min=0.0,
        max=1.0
    )
    
    my_bool: bpy.props.BoolProperty(
        name="启用选项",
        default=True
    )
    
    my_string: bpy.props.StringProperty(
        name="字符串",
        default="文本"
    )
    
    my_enum: bpy.props.EnumProperty(
        name="选择模式",
        items=[
            ('MODE_A', "模式A", "模式A描述"),
            ('MODE_B', "模式B", "模式B描述"),
            ('MODE_C', "模式C", "模式C描述"),
        ],
        default='MODE_A'
    )

    def execute(self, context):
        print(f"浮点值: {self.my_float}")
        print(f"布尔值: {self.my_bool}")
        print(f"字符串: {self.my_string}")
        print(f"枚举值: {self.my_enum}")
        return {'FINISHED'}

    def invoke(self, context, event):
        return self.execute(context)

bpy.utils.register_class(OBJECT_OT_custom_operator)

bpy.ops.object.custom_operator(
    my_float=0.8,
    my_bool=False,
    my_string="测试文本",
    my_enum='MODE_B'
)
```

### 属性组定义

```python
import bpy

class ToolSettings(bpy.types.PropertyGroup):
    strength: bpy.props.FloatProperty(
        name="强度",
        default=0.5,
        min=0.0,
        max=1.0
    )
    
    radius: bpy.props.IntProperty(
        name="半径",
        default=20,
        min=1,
        max=100
    )
    
    enabled: bpy.props.BoolProperty(
        name="启用",
        default=True
    )

bpy.utils.register_class(ToolSettings)

bpy.types.Object.tool_settings = bpy.props.PointerProperty(
    name="工具设置",
    type=ToolSettings
)

mat = bpy.data.materials[0]
mat.tool_settings.strength = 0.75
mat.tool_settings.radius = 50
mat.tool_settings.enabled = False
```

### 集合属性定义

```python
import bpy

class ConfigItem(bpy.types.PropertyGroup):
    key: bpy.props.StringProperty(name="键", default="")
    value: bpy.props.FloatProperty(name="值", default=0.0)
    enabled: bpy.props.BoolProperty(name="启用", default=True)

bpy.utils.register_class(ConfigItem)

bpy.types.Scene.config = bpy.props.CollectionProperty(
    name="配置项",
    type=ConfigItem
)

scene = bpy.context.scene

item = scene.config.add()
item.key = "渲染质量"
item.value = 1.0
item.enabled = True

item = scene.config.add()
item.key = "采样数"
item.value = 128.0
item.enabled = True

for idx, item in enumerate(scene.config):
    print(f"[{idx}] {item.key} = {item.value} (启用: {item.enabled})")

scene.config.remove(0)
```

### 动态枚举

```python
import bpy

def get_object_names(self):
    return [(obj.name, obj.name, f"选择{obj.name}")
            for obj in bpy.data.objects]

def update_object_selection(self, context):
    if self.selected_object:
        obj = bpy.data.objects.get(self.selected_object)
        if obj:
            print(f"选中对象: {obj.name}")
            print(f"位置: {obj.location}")

class ObjectSelector(bpy.types.PropertyGroup):
    selected_object: bpy.props.EnumProperty(
        name="选择对象",
        description="从场景中选择一个对象",
        items=get_object_names,
        update=update_object_selection
    )

bpy.utils.register_class(ObjectSelector)

bpy.types.Scene.selector = bpy.props.PointerProperty(
    name="对象选择器",
    type=ObjectSelector
)
```

### 更新回调

```python
import bpy

def update_length(self, context):
    old_value = getattr(self, "length", 0)
    new_value = self.length
    
    self.volume = new_value ** 3
    
    print(f"长度从 {old_value} 变为 {new_value}")
    print(f"计算体积: {self.volume}")

def update_width(self, context):
    self.length = self.width * 2

class BoxSettings(bpy.types.PropertyGroup):
    length: bpy.props.FloatProperty(
        name="长度",
        default=1.0,
        min=0.1,
        update=update_length
    )
    
    width: bpy.props.FloatProperty(
        name="宽度",
        default=1.0,
        min=0.1,
        update=update_width
    )
    
    volume: bpy.props.FloatProperty(
        name="体积",
        default=1.0,
        min=0.0,
        precision=3
    )

bpy.utils.register_class(BoxSettings)

bpy.types.Object.box = bpy.props.PointerProperty(
    name="盒子设置",
    type=BoxSettings
)

obj = bpy.context.active_object
obj.box.length = 2.0
```

### 属性动画支持

```python
import bpy

class AnimatedProperty(bpy.types.PropertyGroup):
    value: bpy.props.FloatProperty(
        name="动画值",
        default=0.0,
        min=-10.0,
        max=10.0
    )
    
    @property
    def animated_value(self):
        return self.value

bpy.utils.register_class(AnimatedProperty)

bpy.types.Object.animated = bpy.props.PointerProperty(
    name="动画属性",
    type=AnimatedProperty
)

obj = bpy.context.active_object

obj.animated.value = 0.0

if not obj.animation_data:
    obj.animation_data_create()

obj.animated.value = 10.0
obj.keyframe_insert(data_path='animated.value', frame=10)

obj.animated.value = -10.0
obj.keyframe_insert(data_path='animated.value', frame=50)
```

---

## 注意事项

1. **参数传递：** 所有属性函数的参数必须使用关键字参数传递。

2. **注册顺序：** PropertyGroup 必须先注册才能用于 PointerProperty 或 CollectionProperty。

3. **线程安全：** 更新回调可能在多线程环境中执行，不应修改其他数据块或非线程安全的数据。

4. **操作符中的特殊处理：** 如果属性属于 Operator，update 回调的第一个参数是 OperatorProperties 实例，而非操作符实例本身。

5. **属性动画：** 自定义属性可以被动画系统驱动，支持关键帧插入和驱动程序。

6. **ID 类型限制：** 自定义属性可以添加到 ID、Bone 和 PoseBone 的子类中。

7. **访问方式：** 自定义属性可以通过用户界面、Python 代码访问，也可以被动画系统驱动。

---
## bpy_path

# bpy.path 路径模块

提供与 Blender 路径处理相关的实用工具函数，作用范围与 os.path 类似，专门处理 Blender 中的文件路径操作。

## 函数

### abspath

```python
import bpy

absolute_path = bpy.path.abspath("//textures/wood.png")
print(absolute_path)

absolute_path = bpy.path.abspath("//../assets/model.fbx", start="/project")
print(absolute_path)
```

返回相对于当前 blend 文件的绝对路径，支持使用「//」前缀表示相对于当前文件目录。

**参数：**

- `start` (str | bytes): 相对路径的起始目录，未设置时使用当前文件名所在目录。
- `library` (Library): 路径所属的库文件，当库不为 None 时，其路径会替换 start。

**返回：**

- str: 绝对路径。

**说明：** 「//」前缀是 Blender 特有的相对路径表示方式，表示相对于当前保存的 blend 文件位置。

---

### basename

```python
import bpy
from bpy import path

result = path.basename("/home/user/project/textures/wood.png")
print(result)

result = path.basename("//textures/wood.png")
print(result)
```

等价于 os.path.basename 函数，但会跳过「//」前缀。确保 Windows 平台兼容性。

**返回：**

- str: 给定路径的基本名称（文件名部分）。

---

### clean_name

```python
import bpy

clean = bpy.path.clean_name("My Texture Map 1")
print(clean)

clean = bpy.path.clean_name("Texture@#$%", replace="-")
print(clean)
```

返回经过清理的名称，用于处理可能导致问题的特殊字符。所有除 A-Z、a-z、0-9 之外的字符都会被替换为下划线或指定的替换字符。

**参数：**

- `name` (str | bytes): 需要清理的路径名或文件名。
- `replace` (str): 用于替换非法字符的字符，默认为下划线「_」。

**返回：**

- str: 清理后的安全名称。

**应用场景：** 文件写入、材质命名、对象命名等需要避免特殊字符的场景。

---

### display_name

```python
import bpy

display = bpy.path.display_name("wood_texture.png")
print(display)

display = bpy.path.display_name("wood_texture.png", has_ext=False)
print(display)

display = bpy.path.display_name("wood_texture.png", title_case=False)
print(display)
```

根据名称创建用于菜单和用户界面的显示字符串，专为文件名和模块名设计。

**参数：**

- `name` (str): 用于界面显示的名称。
- `has_ext` (bool): 是否从名称中移除文件扩展名，默认为 True。
- `title_case` (bool): 是否将小写名称转换为标题格式，默认为 True。

**返回：**

- str: 用于界面显示的字符串。

**示例效果：**

- `wood_texture.png` → `Wood Texture`
- `wood_texture.png`（has_ext=False）→ `Wood Texture`
- `wood_texture.png`（title_case=False）→ `wood texture`

---

### display_name_to_filepath

```python
import bpy

filepath = bpy.path.display_name_to_filepath("My File_001")
print(filepath)
```

执行 display_name 的逆操作，将显示名称转换回可用于文件路径的格式，处理文件中不支持的特殊字符的字面量版本。

**参数：**

- `name` (str): 需要转换的显示名称。

**返回：**

- str: 文件路径字符串。

---

### display_name_from_filepath

```python
import bpy

display = bpy.path.display_name_from_filepath("/home/user/textures/wood.png")
print(display)
```

从文件路径中提取文件名（不含目录和扩展名），确保 UTF-8 兼容性。

**参数：**

- `name` (str): 需要转换的文件路径。

**返回：**

- str: 用于显示的名称。

---

### ensure_ext

```python
import bpy

filepath = bpy.path.ensure_ext("/path/to/file", ".png")
print(filepath)

filepath = bpy.path.ensure_ext("/path/to/file.png", ".jpg")
print(filepath)

filepath = bpy.path.ensure_ext("/path/to/file", ".tar.gz")
print(filepath)
```

如果文件路径未包含指定扩展名，则添加该扩展名。

**参数：**

- `filepath` (str): 文件路径。
- `ext` (str): 需要检查的扩展名，可以是复合扩展名（如 .tar.gz），必须以点号开头。
- `case_sensitive` (bool): 比较扩展名时是否区分大小写，默认为 False。

**返回：**

- str: 带有指定扩展名的文件路径。

---

### is_subdir

```python
import bpy

result = bpy.path.is_subdir("/project/assets/textures", "/project/assets")
print(result)

result = bpy.path.is_subdir("/project/assets", "/project/textures")
print(result)
```

检查给定路径是否是指定目录的子目录。两个路径都必须是绝对路径。

**参数：**

- `path` (str | bytes): 需要检查的绝对路径。
- `directory` (str | bytes): 父目录的绝对路径。

**返回：**

- bool: 路径是否为子目录。

---

### module_names

```python
import bpy

modules = bpy.path.module_names("/scripts")
print(modules)

modules = bpy.path.module_names("/scripts", recursive=True)
print(modules)

modules = bpy.path.module_names("/scripts", package="my_package")
print(modules)
```

返回可以从指定路径导入的模块列表。

**参数：**

- `path` (str): 要扫描的目录路径。
- `recursive` (bool): 是否递归返回包的子模块名称，默认为 False。
- `package` (str): 可选字符串，用作模块名称的前缀（不带尾部点号）。

**返回：**

- list[tuple[str, str]]: 模块名称和模块文件路径的字符串对列表。

---

### native_pathsep

```python
import bpy

path = bpy.path.native_pathsep("/home/user/project/file.txt")
print(path)
```

将路径分隔符替换为系统原生的分隔符（os.sep）。

**参数：**

- `path` (str): 需要替换分隔符的路径。

**返回：**

- str: 使用系统原生分隔符的路径。

**示例：** 在 Windows 上会将 `/` 替换为 `\`，在 Linux/macOS 上保持不变。

---

### reduce_dirs

```python
import bpy

dirs = [
    "/project",
    "/project/src",
    "/project/src/utils",
    "/project/src",
    "/project/assets",
    "/project/assets/textures",
]

unique_dirs = bpy.path.reduce_dirs(dirs)
print(unique_dirs)
```

给定一系列目录路径，移除重复项和嵌套在其他路径中的目录。

**参数：**

- `dirs` (Sequence[str]): 目录路径序列。

**返回：**

- list[str]: 去重后的唯一路径列表。

**用途：** 递归路径搜索时避免重复搜索同一目录。

---

### relpath

```python
import bpy

relative = bpy.path.relpath("/project/textures/wood.png")
print(relative)

relative = bpy.path.relpath("/project/textures/wood.png", start="/project")
print(relative)
```

返回相对于当前 blend 文件的路径，使用「//」前缀表示。

**参数：**

- `path` (str | bytes): 绝对路径。
- `start` (str | bytes): 相对路径的起始目录，未设置时使用当前文件名所在目录。

**返回：**

- str: 相对路径。

**说明：** 与 abspath 互为逆操作，abspath 将相对路径转为绝对路径，relpath 将绝对路径转为相对路径。

---

### resolve_ncase

```python
import bpy

path = bpy.path.resolve_ncase("/home/User/Project/file.txt")
print(path)
```

在大小写敏感的系统上解析大小写不敏感的路径，如果找到则返回正确大小写的路径，否则返回原始路径。

**参数：**

- `path` (str | bytes): 需要解析的路径。

**返回：**

- str: 正确大小写的路径或原始路径。

**用途：** 确保在大小写敏感的文件系统上访问 Windows 风格的大小写混合路径时能找到正确文件。

---

## 使用示例

### 文件导出路径处理

```python
import bpy
import os

def export_with_correct_path(obj, export_dir, filename, extension=".obj"):
    clean_filename = bpy.path.clean_name(filename)
    full_filename = bpy.path.ensure_ext(clean_filename, extension)
    full_path = os.path.join(export_dir, full_filename)
    
    abs_path = bpy.path.abspath("//" + os.path.join(export_dir, full_filename))
    
    print(f"导出路径: {abs_path}")
    
    return abs_path

export_path = export_with_correct_path(
    bpy.context.active_object,
    "exported_models",
    "My Model 001"
)
```

### 批量处理纹理路径

```python
import bpy
import os

def batch_fix_texture_paths(material, new_textures_dir):
    for node in material.node_tree.nodes:
        if node.type == 'IMAGE' and node.image:
            old_path = node.image.filepath
            
            display_name = bpy.path.display_name_from_filepath(old_path)
            clean_name = bpy.path.clean_name(display_name)
            
            new_filename = bpy.path.ensure_ext(clean_name, ".png")
            new_path = os.path.join(new_textures_dir, new_filename)
            
            relative_path = bpy.path.relpath("//" + new_path)
            
            print(f"旧路径: {old_path}")
            print(f"新相对路径: {relative_path}")

batch_fix_texture_paths(bpy.context.active_object.active_material, "textures")
```

### 插件模块加载

```python
import bpy
import os

def load_plugin_modules(plugin_dir):
    if not os.path.exists(plugin_dir):
        print(f"目录不存在: {plugin_dir}")
        return []
    
    module_list = bpy.path.module_names(plugin_dir, recursive=True)
    
    valid_modules = []
    for module_name, module_file in module_list:
        if bpy.path.is_subdir(module_file, plugin_dir):
            valid_modules.append((module_name, module_file))
    
    print(f"找到 {len(valid_modules)} 个可用模块:")
    for name, path in valid_modules:
        print(f"  - {name}: {path}")
    
    return valid_modules

modules = load_plugin_modules("//plugins")
```

### 相对路径管理

```python
import bpy
import os

class PathManager:
    def __init__(self, base_path=None):
        self.base_path = base_path or bpy.path.abspath("//")
    
    def to_relative(self, absolute_path):
        rel = bpy.path.relpath(absolute_path, start=self.base_path)
        return f"//{rel}" if not rel.startswith("//") else rel
    
    def to_absolute(self, relative_path):
        if relative_path.startswith("//"):
            return bpy.path.abspath(relative_path)
        return os.path.join(self.base_path, relative_path)
    
    def is_in_project(self, path):
        abs_path = self.to_absolute(path)
        return bpy.path.is_subdir(abs_path, self.base_path)

manager = PathManager("/project/assets")

print(manager.to_relative("/project/textures/wood.png"))
print(manager.to_absolute("//textures/wood.png"))
print(manager.is_in_project("//textures"))
```

### 路径清理和规范化

```python
import bpy
import os

def sanitize_export_paths(export_items):
    cleaned_paths = []
    
    for item in export_items:
        display = bpy.path.display_name(item["name"])
        clean = bpy.path.clean_name(display)
        final = bpy.path.ensure_ext(clean, item.get("ext", ".obj"))
        
        full_path = os.path.join(item["dir"], final)
        native = bpy.path.native_pathsep(full_path)
        
        cleaned_paths.append({
            "original": item["name"],
            "cleaned": final,
            "path": native,
            "relative": bpy.path.relpath(native)
        })
    
    reduced = bpy.path.reduce_dirs([p["path"] for p in cleaned_paths])
    print(f"规范化后目录: {reduced}")
    
    return cleaned_paths
```

---

## 注意事项

1. **「//」前缀：** 在 Blender 中，「//」前缀表示相对于当前 blend 文件的路径，在文件未保存时可能无法正确解析。

2. **路径分隔符：** 建议使用 bpy.path 模块的函数处理路径，而非直接操作字符串，以确保跨平台兼容性。

3. **UTF-8 编码：** display_name_from_filepath 确保返回的字符串是 UTF-8 兼容的，避免编码问题。

4. **大小写敏感性：** 在 Windows 上文件名通常不区分大小写，但在 Linux/macOS 上区分。resolve_ncase 可帮助处理跨平台路径问题。

5. **库文件路径：** abspath 和 relpath 的 library 参数可以正确处理来自外部库文件的相对路径。

6. **模块搜索：** module_names 返回的模块需要位于 Python 的模块搜索路径中才能成功导入。

---
## bpy_file_data

# 文件与数据管理模块

## 文件系统概述

每个blend文件包含一个数据库，该数据库包含文件中的所有场景、对象、网格、纹理等。文件可以包含多个场景，每个场景可以包含多个对象，对象可以包含多个材质，材质可以包含许多纹理。还可以创建不同对象之间的链接或在对象之间共享数据，文件可以从其他Blender文件链接数据。

```python
# Blender文件结构
# blend文件是自包含的数据库
# 包含所有场景、对象、网格、纹理等
# 支持多场景管理
# 支持数据链接和共享

# 文件管理核心概念
# 数据块 - 基本数据单元
# 链接 - 从其他文件引用数据
# 追加 - 复制数据到当前文件
# 打包 - 将外部文件嵌入blend文件

# 文件操作方式
# 新建 - 创建新文件
# 打开 - 加载现有文件
# 保存 - 保存当前状态
# 另存为 - 保存为新文件
# 恢复 - 恢复到自动保存版本
```

## 数据块系统

### 数据块基础

数据块是任何Blender项目的基本单元。数据块的例子包括：网格、对象、材质、纹理、节点树、场景、文本、画笔，甚至是工作区。数据块是不同种类数据的通用抽象，具有一组常见的基本特征、属性和行为。

```python
# 数据块通用特征
# 是blend文件的主要内容
# 可以相互引用以实现重用和实例化
# 在给定类型内名称唯一
# 可以添加、移除、编辑、复制
# 可以在文件之间链接
# 可以有自己的动画数据
# 可以有自定义属性

# 数据块类型
# 动作 - 存储动画F曲线
# 骨架 - 用于变形网格的骨骼
# 画笔 - 雕刻和绘制模式中的画笔资产
# 缓存文件 - 用于网格缓存修饰符
# 相机 - 相机对象的数据
# 集合 - 在场景中组织和分组对象
# 曲线 - 曲线、字体和曲面对象的数据
# 曲线新版本 - 新的曲线数据类型
# 字体 - 引用字体文件
# 图像 - 图像文件
# 键 - 形状键几何体存储
# 晶格 - 基于网格的晶格变形
# 库 - 引用外部blend文件

# 非数据块数据
# 骨骼 - 属于骨架类型
# 序列条带 - 属于场景类型
# 顶点组 - 属于网格类型
```

### 数据块操作

```python
# 创建数据块
mesh = bpy.data.meshes.new(name="NewMesh")
obj = bpy.data.objects.new(name="NewObject", object_data=mesh)

# 访问数据块
for mesh in bpy.data.meshes:
    print(f"网格: {mesh.name}")

# 重命名数据块
mesh.name = "RenamedMesh"

# 删除数据块
bpy.data.meshes.remove(mesh)

# 复制数据块
mesh_copy = mesh.copy()
mesh_copy.name = "MeshCopy"

# 检查数据块使用情况
for user in mesh.users:
    print(f"使用者: {user.name}")
```

### 数据块类型详情

Blender支持多种类型的数据块，每种都有特定的用途和链接能力。

```python
# 动作数据块
# 存储动画F曲线
# 用于数据块动画数据和NLA编辑器
# 支持链接但不支持打包

# 骨架数据块
# 用于变形网格的骨骼
# 用作骨架对象的数据
# 由骨架修饰符使用
# 支持链接但不支持打包

# 相机数据块
# 用作相机对象的数据
# 存储相机参数
# 支持链接但不支持打包

# 集合数据块
# 在场景中组织和分组对象
# 用于实例化对象
# 在库链接中使用
# 支持链接但不支持打包

# 曲线数据块
# 用于曲线、字体和曲面对象
# 支持多种曲线类型
# 支持链接但不支持打包

# 图像数据块
# 图像文件
# 用于着色器节点和纹理
# 支持链接和打包

# 材质数据块
# 材质定义
# 用于网格、曲面等
# 支持链接但不支持打包

# 场景数据块
# 场景定义
# 包含所有场景内容
# 支持链接但不支持打包

# 纹理数据块
# 纹理定义
# 用于材质着色器
# 支持链接但不支持打包
```

## 文件打开与保存

### 打开文件

```python
# 打开blend文件
import bpy
bpy.ops.wm.open_mainfile(filepath="//path/to/file.blend")

# 打开最近文件
bpy.ops.wm.open_mainfile(filepath=bpy.utils.resource_path('USER') + "/config/recover.blend")

# 打开并恢复
bpy.ops.wm.open_mainfile(filepath="//autosave.blend")

# 文件浏览器打开
bpy.ops.wm.path_open(filepath="//")

# 追加文件内容
bpy.ops.wm.append(
    filepath="//source.blend/Collection/MyCollection",
    directory="//source.blend/Collection/",
    filename="MyCollection"
)
```

### 保存文件

```python
# 保存当前文件
bpy.ops.wm.save_mainfile()

# 另存为
bpy.ops.wm.save_as_mainfile(filepath="//new_file.blend")

# 保存副本
bpy.ops.wm.save_mainfile(filepath="//backup.blend")

# 保存用户默认
bpy.ops.wm.save_homefile()

# 保存启动文件
bpy.ops.wm.save_userpref()

# 自动保存恢复
bpy.ops.wm.recover_auto_save(filepath="//autosave.blend")
```

### 文件打包

```python
# 打包所有外部数据
bpy.ops.file.pack_all()

# 打包选定对象的数据
bpy.ops.file.pack_selected()

# 取消打包所有数据
bpy.ops.file.unpack_all()

# 取消打包选定数据
bpy.ops.file.unpack_selected()

# 重新打包所有数据
bpy.ops.file.pack_all()

# 打包模式
# 使用空名称 - 打包但不使用文件名
# 写入本地文件 - 打包到blend文件所在目录
# 保留当前目录 - 保持当前文件路径
# 使用绝对路径 - 使用文件原始绝对路径
```

## 链接与追加

### 库链接基础

链接允许您从其他blend文件引用数据，而无需将数据复制到当前文件。这使得多用户协作和资产管理更加高效。

```python
# 链接数据块
import bpy
bpy.ops.wm.link(
    filepath="//library.blend/Collection/MyCollection",
    directory="//library.blend/Collection/",
    filename="MyCollection"
)

# 链接集合实例
collection = bpy.data.collections.new("LinkedCollection")
instance = bpy.context.scene.collection.objects.link(
    bpy.data.objects.new("Instance", None)
)
instance.instance_type = 'COLLECTION'
instance.instance_collection = collection

# 链接对象数据
obj = bpy.data.objects.new("LinkedObject", bpy.data.meshes["SourceMesh"])
bpy.context.collection.objects.link(obj)

# 追加数据块
bpy.ops.wm.append(
    filepath="//library.blend/Mesh/SourceMesh",
    directory="//library.blend/Mesh/",
    filename="SourceMesh"
)
```

### 库覆盖

库覆盖允许您覆盖链接数据的特定属性，同时保持与原始库的链接。

```python
# 创建库覆盖
import bpy
obj = bpy.data.objects["LinkedObject"]
override = obj.make_override_library()
override.location = (1.0, 0.0, 0.0)

# 编辑库覆盖
bpy.ops.object.liboverride_apply_property(
    property="location",
    value="(2.0, 0.0, 0.0)"
)

# 同步库覆盖
bpy.ops.object.liboverride_sync()

# 删除库覆盖
bpy.ops.object.liboverride_delete()

# 查看库覆盖
for obj in bpy.data.objects:
    if obj.override_library:
        print(f"覆盖对象: {obj.name}")
```

### 追加操作

```python
# 追加场景
bpy.ops.wm.append(
    filepath="//source.blend/Scene/MyScene",
    directory="//source.blend/Scene/",
    filename="MyScene",
    autoselect=True,
    active_collection=True
)

# 追加对象
bpy.ops.wm.append(
    filepath="//source.blend/Object/Cube",
    directory="//source.blend/Object/",
    filename="Cube",
    autoselect=True,
    link=False
)

# 追加材质
bpy.ops.wm.append(
    filepath="//source.blend/Material/MyMaterial",
    directory="//source.blend/Material/",
    filename="MyMaterial"
)

# 追加并链接
bpy.ops.wm.append(
    filepath="//source.blend/Collection/MyCollection",
    directory="//source.blend/Collection/",
    filename="MyCollection",
    link=True
)
```

## 资产管理

### 资产系统

Blender的资产系统允许您将数据块标记为资产，便于在不同项目间共享和重用。

```python
# 将数据块标记为资产
import bpy
material = bpy.data.materials["MyMaterial"]
material.asset_mark()

# 清除资产标记
material.asset_clear()

# 设置资产元数据
material.asset_info.name = "My Material"
material.asset_info.author = "Artist Name"
material.asset_info.description = "A custom material"

# 批量标记资产
for mat in bpy.data.materials:
    if not mat.asset_data:
        mat.asset_mark()

# 查看资产信息
for mat in bpy.data.materials:
    if mat.asset_data:
        print(f"资产: {mat.asset_data.name}")
        print(f"作者: {mat.asset_info.author}")
```

### 资产库

资产库允许您组织和共享资产集合。

```python
# 创建资产库
bpy.ops.preferences.asset_library_add(
    directory="//assets/"
)

# 配置资产库
prefs = bpy.context.preferences
asset_lib = prefs.filepaths.asset_libraries["MyAssets"]
asset_lib.path = "//assets/"
asset_lib.import_method = 'APPEND'

# 浏览资产库
bpy.ops.editors.asset_browser(
    library_name="MyAssets",
    asset_type='MATERIALS'
)

# 导入资产
bpy.ops.editors.asset_browser_expand_catalog()
bpy.ops.editors.asset_browser_catalog_apply()

# 同步资产库
bpy.ops.editors.asset_library_refresh()
```

### 资源目录

资源目录用于在资产库内组织资产。

```python
# 创建目录
catalog = bpy.data.simulation_type["MyAssets"].catalogs.new("Materials")
catalog.name = "Materials"

# 添加到目录
bpy.ops.editors.asset_browser_catalog_apply(
    catalog_id=catalog.catalog_id
)

# 从目录移除
bpy.ops.editors.asset_browser_catalog_remove(
    catalog_id=catalog.catalog_id
)

# 重命名目录
catalog.name = "New Catalog Name"

# 查看目录结构
for catalog in bpy.data.simulation_type["MyAssets"].catalogs:
    print(f"目录: {catalog.name}")
```

## 自定义属性

### 自定义属性基础

自定义属性允许您向数据块添加任意数据，这些数据可以用于各种目的，包括存储元数据、配置数据或第三方插件数据。

```python
# 添加自定义属性
import bpy
obj = bpy.context.active_object

# 添加简单属性
obj["my_integer"] = 42
obj["my_float"] = 3.14
obj["my_string"] = "Hello"
obj["my_boolean"] = True

# 使用Python属性
obj["scale_factor"] = 1.0

# 验证属性存在
if "custom_key" in obj:
    print(f"值: {obj['custom_key']}")

# 删除属性
if "old_key" in obj:
    del obj["old_key"]

# 获取属性
value = obj.get("key", "default")
```

### 自定义属性类型

```python
# 数值属性
obj["int_value"] = 10
obj["float_value"] = 2.5
obj["vector_value"] = (1.0, 2.0, 3.0)

# 字符串属性
obj["name"] = "ObjectName"
obj["description"] = "A test object"

# 数组属性
obj["rotation_euler"] = (0.0, 0.0, 0.0)
obj["scale_vector"] = (1.0, 1.0, 1.0)

# 嵌套属性
obj["custom_data"] = {"key": "value"}
```

### 属性动画

```python
# 创建属性动画
obj["custom_property"] = 0.0
obj.keyframe_insert(data_path='["custom_property"]', frame=1)

obj["custom_property"] = 10.0
obj.keyframe_insert(data_path='["custom_property"]', frame=10)

# 读取属性动画
for fcurve in obj.animation_data.action.fcurves:
    if fcurve.data_path == '["custom_property"]':
        for keyframe in fcurve.keyframe_points:
            print(f"帧: {keyframe.co.x}, 值: {keyframe.co.y}")
```

## 文件路径配置

### 路径设置

```python
# 获取文件路径
blend_file_path = bpy.data.filepath
if blend_file_path:
    print(f"当前文件: {blend_file_path}")

# 获取目录
import os
file_dir = os.path.dirname(bpy.data.filepath)
file_name = os.path.basename(bpy.data.filepath)

# 相对路径
relative_path = bpy.path.relpath("//textures/image.png")

# 绝对路径
absolute_path = bpy.path.abspath("//textures/image.png")

# 检查文件存在
file_exists = os.path.exists(absolute_path)

# 路径归一化
normalized = bpy.path.cleanup(absolute_path)
```

### 配置路径

```python
# Blender配置目录
config_dir = bpy.utils.resource_path('USER') + "/config/"

# 用户默认文件
user_default = config_dir + "startup.blend"

# 用户密钥映射
keymap_file = config_dir + "keymap.py"

# 临时目录
import tempfile
temp_dir = tempfile.gettempdir()

# 项目目录
project_dir = bpy.path.abspath("//")
```

## 大纲管理器

### 大纲管理

大纲编辑器显示blend文件中的所有数据，允许您管理对象、集合和数据块。

```python
# 访问大纲视图
outliner = bpy.context.area.spaces[0]

# 设置筛选类型
outliner.filter_id_type = 'OBJECT'

# 设置搜索筛选
outliner.search_query = "Cube"

# 设置显示模式
outliner.display_mode = 'DATA'

# 刷新大纲
bpy.ops.outliner.operatingspace_refresh()

# 选择所有可见
bpy.ops.outliner.select_hierarchy(direction='CHILD', extend=True)
```

### 大纲操作

```python
# 选择对象
bpy.ops.outliner.select_walk(direction='DOWN')

# 展开所有
bpy.ops.outliner.expanded_toggle()

# 展开到层级
bpy.ops.outliner.show_hierarchy()

# 刷新库
bpy.ops.outliner.lib_refresh()

# 同步选择
outliner.use_sync_select = True

# 高亮更新
outliner.use_highlight_update = True
```

## 性能与优化

### 文件优化

```python
# 清理未使用数据
bpy.ops.outliner.orphans_purge()

# 清理所有未使用
bpy.ops.outliner.orphans_purge(do_local_ids=True, do_linked_ids=True)

# 压缩文件
bpy.ops.wm.save_mainfile(compress=True)

# 打包图像
for image in bpy.data.images:
    if not image.packed_file:
        bpy.ops.image.pack(image=image)

# 优化材质
for material in bpy.data.materials:
    if not material.users:
        bpy.data.materials.remove(material)
```

### 数据清理

```python
# 清理空集合
for collection in bpy.data.collections:
    if len(collection.objects) == 0:
        bpy.data.collections.remove(collection)

# 清理空顶点组
obj = bpy.context.active_object
for vg in obj.vertex_groups:
    if len(vg.weight(I) == 0.0 for I in range(len(obj.data.vertices))) == len(obj.data.vertices):
        obj.vertex_groups.remove(vg)

# 清理无效约束
for obj in bpy.data.objects:
    for constraint in obj.constraints:
        if not constraint.is_valid:
            obj.constraints.remove(constraint)
```

## 最佳实践

### 项目组织

```python
# 规范文件结构
# project/
#   ├── assets/
#   │   ├── materials/
#   │   ├── textures/
#   │   └── models/
#   ├── blends/
#   │   ├── master.blend
#   │   ├── chars/
#   │   └── props/
#   └── output/

# 使用集合组织
root_collection = bpy.data.collections.new("ProjectRoot")
bpy.context.scene.collection.children.link(root_collection)

# 使用前缀命名
char_collection = bpy.data.collections.new("CH_Character")
root_collection.children.link(char_collection)

# 使用资产库
bpy.ops.preferences.asset_library_add(directory="//assets/")
```

### 数据管理策略

```python
# 链接优先
# 对于共享资产使用链接
# 减少文件大小
# 保持更新同步

# 追加特定数据
# 对于需要修改的数据使用追加
# 复制数据到当前文件
# 避免意外影响源文件

# 使用集合组织
# 按功能分组对象
# 便于管理可见性
# 支持分层结构

# 定期清理
# 清理未使用数据
# 减少文件大小
# 提高性能
```

### 版本控制

```python
# 增量保存
import os
base_path = bpy.data.filepath
dir_path = os.path.dirname(base_path)
name = os.path.splitext(os.path.basename(base_path))[0]
for i in range(1, 100):
    version_path = f"{dir_path}/{name}_v{i:02d}.blend"
    if not os.path.exists(version_path):
        bpy.ops.wm.save_as_mainfile(filepath=version_path)
        break

# 保存检查点
bpy.ops.wm.save_mainfile(filepath="//checkpoints/checkpoint.blend")

# 记录变更
with open("//changelog.txt", "a") as f:
    f.write(f"Version {version}: Added new character rig\n")
```

## 常见问题解决

### 文件问题

```python
# 文件无法打开
# 检查文件路径是否正确
# 验证文件未被其他程序锁定
# 检查Blender版本兼容性

# 文件过大
# 打包图像和纹理
# 清理未使用数据
# 使用链接替代复制

# 链接丢失
# 检查库路径设置
# 重新链接缺失数据
# 使用相对路径

# 恢复崩溃
# 查找自动保存文件
# 使用恢复菜单
# 检查临时目录
```

### 数据问题

```python
# 数据块冲突
# 重命名冲突的数据块
# 使用唯一命名
# 清理重复数据

# 循环引用
# 检查约束链
# 避免循环父子关系
# 使用单一数据源

# 内存不足
# 清理未使用数据
# 减少视口复杂度
# 使用代理对象

# 数据损坏
# 尝试从备份恢复
# 使用恢复自动保存
# 重新导入外部数据
```


---
## idprop

# idprop ID属性模块

提供对 Blender ID 属性的类型访问，用于处理自定义属性数据的序列化和反序列化。

## 类

### IDPropertyArray

```python
import bpy

obj = bpy.data.objects[0]
if "custom_array" in obj:
    arr = obj["custom_array"]
    if isinstance(arr, idprop.types.IDPropertyArray):
        print(f"类型代码: {arr.typecode}")
        python_list = arr.to_list()
```

Blender ID 属性数组类型，支持数值数组的存储和操作。

**方法：**

- `to_list()`: 将数组转换为 Python 列表返回。
- `typecode`: 属性，数组数据类型代码，'f' 表示 float，'d' 表示 double，'i' 表示 int，'b' 表示 bool。

---

### IDPropertyGroup

```python
import bpy
import idprop

obj = bpy.data.objects[0]
if "custom_group" in obj:
    group = obj["custom_group"]
    if isinstance(group, idprop.types.IDPropertyGroup):
        print(f"组名: {group.name}")
        keys = group.keys()
        for key in keys:
            print(f"{key}: {group.get(key)}")
```

Blender ID 属性组类型，支持字典-like 的键值对存储。

**方法：**

- `clear()`: 清除组中的所有成员。
- `get(key, default=None)`: 获取指定键的值，如果键不存在则返回默认值。
- `items()`: 返回组中的键值对迭代器。
- `keys()`: 返回组中所有键的列表。
- `pop(key, default)`: 从组中移除指定键并返回其值，如果键不存在则返回默认值。
- `to_dict()`: 将组转换为纯 Python 字典。
- `update(other)`: 使用另一个 IDPropertyGroup 或字典更新当前组。
- `values()`: 返回组中所有值的迭代器。

**属性：**

- `name`: 组的名称。

---

### 迭代器类

用于支持 IDPropertyGroup 的迭代操作。

- `IDPropertyGroupIterItems`: 键值对迭代器。
- `IDPropertyGroupIterKeys`: 键迭代器。
- `IDPropertyGroupIterValues`: 值迭代器。
- `IDPropertyGroupViewItems`: 键值对视图。
- `IDPropertyGroupViewKeys`: 键视图。
- `IDPropertyGroupViewValues`: 值视图。

---

## 使用示例

### 创建 ID 属性组

```python
import bpy

obj = bpy.context.active_object

if "MyCustomProps" not in obj:
    obj["MyCustomProps"] = {}

group = obj["MyCustomProps"]
group["name"] = "Test Object"
group["enabled"] = True
group["count"] = 42
group["scale_factor"] = 1.5
```

### 从 Python 字典创建

```python
import bpy
import idprop
from idprop.types import IDPropertyGroup

data = {
    "string_val": "Hello",
    "int_val": 100,
    "float_val": 3.14,
    "bool_val": False,
}

group = IDPropertyGroup()
group.update(data)

obj = bpy.context.active_object
obj["DynamicProps"] = group
```

### 读取和修改属性

```python
import bpy
import idprop

obj = bpy.context.active_object

if "Settings" in obj:
    settings = obj["Settings"]
    
    if isinstance(settings, idprop.types.IDPropertyGroup):
        old_value = settings.get("intensity", 1.0)
        settings["intensity"] = old_value * 1.5
        
        if "deprecated_key" in settings.keys():
            value = settings.pop("deprecated_key", 0)
            print(f"移除旧属性: {value}")
        
        print("所有设置:")
        for key, value in settings.items():
            print(f"  {key}: {value}")
```

### 导出属性数据

```python
import bpy
import idprop

obj = bpy.context.active_object

if "Config" in obj:
    config = obj["Config"]
    
    if isinstance(config, idprop.types.IDPropertyGroup):
        config_dict = config.to_dict()
        
        import json
        json_str = json.dumps(config_dict, indent=2)
        print(json_str)
```

### 处理数组属性

```python
import bpy
import idprop

obj = bpy.context.active_object

if "VertexColors" in obj:
    colors = obj["VertexColors"]
    
    if isinstance(colors, idprop.types.IDPropertyArray):
        print(f"数组类型: {colors.typecode}")
        
        python_list = colors.to_list()
        print(f"数组长度: {len(python_list)}")
        print(f"前3个元素: {python_list[:3]}")
```

### 属性组嵌套

```python
import bpy
import idprop
from idprop.types import IDPropertyGroup

obj = bpy.context.active_object

outer_group = IDPropertyGroup()
outer_group["name"] = "Outer"

inner_group = IDPropertyGroup()
inner_group["value"] = 123
inner_group["enabled"] = True

outer_group["nested"] = inner_group
obj["ComplexProps"] = outer_group

if "ComplexProps" in obj:
    complex_data = obj["ComplexProps"]
    if isinstance(complex_data, idprop.types.IDPropertyGroup):
        nested = complex_data.get("nested")
        if isinstance(nested, idprop.types.IDPropertyGroup):
            print(f"嵌套值: {nested.get('value')}")
```

### 批量更新属性

```python
import bpy
import idprop

obj = bpy.context.active_object

if "BatchUpdate" in obj:
    batch = obj["BatchUpdate"]
    
    if isinstance(batch, idprop.types.IDPropertyGroup):
        updates = {
            "param1": 10,
            "param2": 20,
            "param3": 30,
        }
        
        batch.update(updates)
        
        print("更新后的键:", batch.keys())
        print("更新后的值:", [batch.get(k) for k in batch.keys()])
```

---

## 注意事项

1. **类型检查**: 使用 isinstance() 检查属性是否为 IDPropertyGroup 或 IDPropertyArray 类型。

2. **数据转换**: to_dict() 方法将 IDPropertyGroup 转换为纯 Python 字典，便于序列化。

3. **数组类型**: IDPropertyArray 的 typecode 属性指示数据类型，'f' 为单精度浮点，'d' 为双精度浮点，'i' 为整型，'b' 为布尔型。

4. **迭代器生命周期**: 迭代器类在遍历过程中使用，注意不要在迭代过程中修改组结构。

5. **属性嵌套**: IDPropertyGroup 可以嵌套其他 IDPropertyGroup，形成复杂的数据结构。

6. **文件保存**: ID 属性会自动随 .blend 文件保存和加载。

7. **Python 表示**: pop() 和 get() 方法返回 Python 原生类型，而不是 ID 属性类型。

---
## idprop_types

# idprop.types 模块

idprop.types 模块定义了 Blender ID 属性系统的核心类型，用于在数据块中存储自定义属性。

## ID 属性类型概述

ID 属性是 Blender 中用于在各种数据块（对象、材质、场景等）中存储自定义数据的关键机制。该模块定义了属性类型和工具函数。

### 何时使用 ID 属性

- 存储自定义对象数据
- 保存插件配置
- 实现属性动画
- 在数据块之间传递数据

## 属性类型

### IDPropertyGroup

属性组容器，用于组织多个相关属性。

```python
import bpy
from idprop.types import IDPropertyGroup

# 访问对象的属性组
obj = bpy.context.active_object

# 检查是否有属性组
if "custom_props" not in obj:
    # 创建新的属性组
    obj["custom_props"] = {}

# 访问属性组
props = obj["custom_props"]

# 设置属性
props["my_int"] = 42
props["my_float"] = 3.14
props["my_string"] = "Hello"
props["my_vector"] = [1.0, 2.0, 3.0]

# 读取属性
print(props.get("my_int"))
print(props.get("my_float"))
```

### IDPropertyArray

属性数组，用于存储数组类型的属性。

```python
import bpy

# 创建数组属性
obj = bpy.context.active_object

# 整数数组
obj["int_array"] = [1, 2, 3, 4, 5]

# 浮点数数组
obj["float_array"] = [1.1, 2.2, 3.3, 4.4]

# 向量（3元素数组）
obj["position"] = [0.0, 0.0, 0.0]

# 矩阵（16元素数组，用于 4x4 矩阵）
obj["transform"] = [1.0] * 16

# 访问数组
array = obj["int_array"]
print(f"数组长度: {len(array)}")
print(f"第一个元素: {array[0]}")
print(f"数组类型: {type(array)}")
```

## 属性类型函数

### to_idprop

将 Python 对象转换为 ID 属性。

```python
from idprop.types import to_idprop

# 转换各种类型
int_prop = to_idprop(42)
float_prop = to_idprop(3.14)
string_prop = to_idprop("hello")
list_prop = to_idprop([1, 2, 3])
dict_prop = to_idprop({"key": "value"})
```

### idprop_to_py

将 ID 属性转换回 Python 对象。

```python
from idprop.types import idprop_to_py

# 假设从属性中获取的值
value = bpy.context.active_object.get("my_property")

# 转换为 Python 对象
py_value = idprop_to_py(value)
```

## 实际应用示例

### 自定义属性管理器

```python
import bpy
from idprop.types import IDPropertyGroup
from typing import Any, Dict, List, Optional

class CustomPropertyManager:
    """自定义属性管理器"""
    
    def __init__(self, data_block):
        self.data_block = data_block
        self.group_name = "_custom_props"
        self._ensure_group()
    
    def _ensure_group(self):
        """确保属性组存在"""
        if self.group_name not in self.data_block:
            self.data_block[self.group_name] = {}
    
    @property
    def group(self):
        """获取属性组"""
        return self.data_block[self.group_name]
    
    def set_int(self, key: str, value: int) -> None:
        """设置整数属性"""
        self.group[key] = int(value)
    
    def get_int(self, key: str, default: int = 0) -> int:
        """获取整数属性"""
        return int(self.group.get(key, default))
    
    def set_float(self, key: str, value: float) -> None:
        """设置浮点数属性"""
        self.group[key] = float(value)
    
    def get_float(self, key: str, default: float = 0.0) -> float:
        """获取浮点数属性"""
        return float(self.group.get(key, default))
    
    def set_string(self, key: str, value: str) -> None:
        """设置字符串属性"""
        self.group[key] = str(value)
    
    def get_string(self, key: str, default: str = "") -> str:
        """获取字符串属性"""
        return str(self.group.get(key, default))
    
    def set_vector(self, key: str, value: List[float], size: int = 3) -> None:
        """设置向量属性"""
        vector = list(value)
        while len(vector) < size:
            vector.append(0.0)
        self.group[key] = vector[:size]
    
    def get_vector(self, key: str, default: List[float] = None) -> List[float]:
        """获取向量属性"""
        if default is None:
            default = [0.0, 0.0, 0.0]
        return list(self.group.get(key, default))
    
    def set_bool(self, key: str, value: bool) -> None:
        """设置布尔属性"""
        self.group[key] = bool(value)
    
    def get_bool(self, key: str, default: bool = False) -> bool:
        """获取布尔属性"""
        return bool(self.group.get(key, default))
    
    def set_color(self, key: str, r: float, g: float, b: float, a: float = 1.0) -> None:
        """设置颜色属性"""
        self.group[key] = [float(r), float(g), float(b), float(a)]
    
    def get_color(self, key: str, default: List[float] = None) -> List[float]:
        """获取颜色属性"""
        if default is None:
            default = [0.0, 0.0, 0.0, 1.0]
        return list(self.group.get(key, default))
    
    def set_matrix(self, key: str, matrix, size: int = 16) -> None:
        """设置矩阵属性"""
        flat = []
        for row in matrix:
            flat.extend(list(row))
        self.group[key] = flat[:size]
    
    def get_matrix(self, key: str) -> List[float]:
        """获取矩阵属性"""
        return list(self.group.get(key, [1.0] * 16))
    
    def remove(self, key: str) -> bool:
        """删除属性"""
        if key in self.group:
            del self.group[key]
            return True
        return False
    
    def clear(self) -> None:
        """清除所有属性"""
        self.group.clear()
    
    def has(self, key: str) -> bool:
        """检查属性是否存在"""
        return key in self.group
    
    def keys(self) -> List[str]:
        """获取所有属性键"""
        return list(self.group.keys())
    
    def values(self) -> List[Any]:
        """获取所有属性值"""
        return list(self.group.values())
    
    def items(self) -> Dict[str, Any]:
        """获取所有属性项"""
        return dict(self.group.items())
    
    def get_all(self) -> Dict[str, Any]:
        """获取所有属性"""
        return self.items()


class ObjectPropertyManager(CustomPropertyManager):
    """对象属性管理器"""
    
    def __init__(self, obj):
        super().__init__(obj)
        self.obj = obj
    
    def set_work_mode(self, mode: str) -> None:
        """设置工作模式"""
        valid_modes = ['EDIT', 'POSE', 'SCULPT', 'PAINT']
        if mode not in valid_modes:
            mode = valid_modes[0]
        self.set_string("work_mode", mode)
    
    def get_work_mode(self) -> str:
        """获取工作模式"""
        return self.get_string("work_mode", "EDIT")
    
    def set_selection_priority(self, priority: int) -> None:
        """设置选择优先级"""
        self.set_int("selection_priority", max(0, min(10, priority)))
    
    def get_selection_priority(self) -> int:
        """获取选择优先级"""
        return self.get_int("selection_priority", 0)
    
    def set_custom_color(self, r: float, g: float, b: float) -> None:
        """设置自定义颜色"""
        self.set_color("custom_color", r, g, b, 1.0)
    
    def get_custom_color(self) -> List[float]:
        """获取自定义颜色"""
        return self.get_color("custom_color", [0.0, 0.0, 0.0, 1.0])
    
    def mark_as_important(self, important: bool = True) -> None:
        """标记为重要"""
        self.set_bool("is_important", important)
    
    def is_important(self) -> bool:
        """检查是否重要"""
        return self.get_bool("is_important", False)
    
    def save_transform_data(self) -> None:
        """保存变换数据"""
        self.set_vector("saved_location", self.obj.location)
        self.set_vector("saved_rotation", self.obj.rotation_euler)
        self.set_vector("saved_scale", self.obj.scale)
    
    def restore_transform_data(self) -> None:
        """恢复变换数据"""
        if self.has("saved_location"):
            self.obj.location = self.get_vector("saved_location")
        if self.has("saved_rotation"):
            self.obj.rotation_euler = self.get_vector("saved_rotation")
        if self.has("saved_scale"):
            self.obj.scale = self.get_vector("saved_scale")


class MaterialPropertyManager(CustomPropertyManager):
    """材质属性管理器"""
    
    def __init__(self, mat):
        super().__init__(mat)
        self.mat = mat
    
    def set_shader_type(self, shader_type: str) -> None:
        """设置着色器类型"""
        valid_types = ['PRINCIPLED', 'EMISSION', 'TOON', 'GLOSSY']
        if shader_type not in valid_types:
            shader_type = valid_types[0]
        self.set_string("shader_type", shader_type)
    
    def get_shader_type(self) -> str:
        """获取着色器类型"""
        return self.get_string("shader_type", "PRINCIPLED")
    
    def set_render_weight(self, weight: float) -> None:
        """设置渲染权重"""
        self.set_float("render_weight", max(0.0, min(1.0, weight)))
    
    def get_render_weight(self) -> float:
        """获取渲染权重"""
        return self.get_float("render_weight", 1.0)
    
    def set_subsurface_color(self, r: float, g: float, b: float) -> None:
        """设置次表面颜色"""
        self.set_color("subsurface_color", r, g, b, 1.0)
    
    def get_subsurface_color(self) -> List[float]:
        """获取次表面颜色"""
        return self.get_color("subsurface_color", [0.8, 0.2, 0.2, 1.0])


def create_property_managers():
    """创建属性管理器"""
    obj = bpy.context.active_object
    
    if obj:
        obj_manager = ObjectPropertyManager(obj)
        print(f"对象属性管理器已创建: {obj.name}")
        return obj_manager
    
    mat = bpy.context.active_object.active_material if bpy.context.active_object else None
    
    if mat:
        mat_manager = MaterialPropertyManager(mat)
        print(f"材质属性管理器已创建: {mat.name}")
        return mat_manager
    
    print("未选中对象或材质")
    return None
```

### 插件配置系统

```python
import bpy
from idprop.types import IDPropertyGroup
import json
import os

class PluginConfigManager:
    """插件配置管理器"""
    
    def __init__(self, plugin_name: str):
        self.plugin_name = plugin_name
        self.config_group = "_plugin_config"
        self._ensure_config()
    
    def _ensure_config(self):
        """确保配置组存在"""
        if self.config_group not in bpy.context.scene:
            bpy.context.scene[self.config_group] = {}
    
    @property
    def config(self):
        """获取配置"""
        return bpy.context.scene[self.config_group].get(self.plugin_name, {})
    
    def _get_or_create_config(self):
        """获取或创建配置"""
        if self.plugin_name not in bpy.context.scene[self.config_group]:
            bpy.context.scene[self.config_group][self.plugin_name] = {}
        return bpy.context.scene[self.config_group][self.plugin_name]
    
    def set(self, key: str, value) -> None:
        """设置配置值"""
        config = self._get_or_create_config()
        
        # 类型检查和转换
        if isinstance(value, bool):
            config[key] = bool(value)
        elif isinstance(value, int):
            config[key] = int(value)
        elif isinstance(value, float):
            config[key] = float(value)
        elif isinstance(value, str):
            config[key] = str(value)
        elif isinstance(value, (list, tuple)):
            config[key] = list(value)
        elif isinstance(value, dict):
            config[key] = dict(value)
        else:
            config[key] = value
    
    def get(self, key: str, default=None):
        """获取配置值"""
        config = self._get_or_create_config()
        return config.get(key, default)
    
    def remove(self, key: str) -> bool:
        """删除配置项"""
        config = self._get_or_create_config()
        if key in config:
            del config[key]
            return True
        return False
    
    def clear(self) -> None:
        """清除所有配置"""
        config = self._get_or_create_config()
        config.clear()
    
    def export_config(self, filepath: str) -> bool:
        """导出配置到文件"""
        config = self._get_or_create_config()
        
        try:
            with open(filepath, 'w', encoding='utf-8') as f:
                json.dump(dict(config), f, indent=2, ensure_ascii=False)
            print(f"配置已导出到: {filepath}")
            return True
        except Exception as e:
            print(f"导出配置失败: {e}")
            return False
    
    def import_config(self, filepath: str) -> bool:
        """从文件导入配置"""
        try:
            with open(filepath, 'r', encoding='utf-8') as f:
                imported_config = json.load(f)
            
            config = self._get_or_create_config()
            config.clear()
            config.update(imported_config)
            
            print(f"配置已从 {filepath} 导入")
            return True
        except Exception as e:
            print(f"导入配置失败: {e}")
            return False
    
    def reset_to_defaults(self) -> None:
        """重置为默认配置"""
        defaults = {
            'auto_save': True,
            'save_interval': 300,
            'debug_mode': False,
            'language': 'en',
            'theme': 'DARK',
            'recent_files': []
        }
        
        config = self._get_or_create_config()
        config.clear()
        config.update(defaults)
        print("配置已重置为默认值")


class AddonPreferencesManager:
    """插件偏好设置管理器"""
    
    def __init__(self, addon_name: str):
        self.addon_name = addon_name
        self.prefs_name = "_addon_preferences"
    
    def get_preferences(self):
        """获取偏好设置"""
        if self.prefs_name not in bpy.context.scene:
            bpy.context.scene[self.prefs_name] = {}
        
        return bpy.context.scene[self.prefs_name].get(self.addon_name, {})
    
    def set_hotkey(self, hotkey_name: str, key: str, ctrl: bool = False, 
                   shift: bool = False, alt: bool = False) -> None:
        """设置快捷键"""
        prefs = self.get_preferences()
        
        if 'hotkeys' not in prefs:
            prefs['hotkeys'] = {}
        
        prefs['hotkeys'][hotkey_name] = {
            'key': key,
            'ctrl': ctrl,
            'shift': shift,
            'alt': alt
        }
    
    def get_hotkey(self, hotkey_name: str) -> dict:
        """获取快捷键"""
        prefs = self.get_preferences()
        hotkeys = prefs.get('hotkeys', {})
        return hotkeys.get(hotkey_name, {})
    
    def set_ui_scale(self, scale: float) -> None:
        """设置界面缩放"""
        prefs = self.get_preferences()
        prefs['ui_scale'] = max(0.5, min(2.0, scale))
    
    def get_ui_scale(self) -> float:
        """获取界面缩放"""
        return self.get_preferences().get('ui_scale', 1.0)
    
    def set_cache_size(self, size_mb: int) -> None:
        """设置缓存大小"""
        prefs = self.get_preferences()
        prefs['cache_size_mb'] = max(100, min(10000, size_mb))
    
    def get_cache_size(self) -> int:
        """获取缓存大小"""
        return self.get_preferences().get('cache_size_mb', 500)
    
    def enable_feature(self, feature_name: str) -> None:
        """启用功能"""
        prefs = self.get_preferences()
        
        if 'features' not in prefs:
            prefs['features'] = {}
        
        prefs['features'][feature_name] = True
    
    def disable_feature(self, feature_name: str) -> None:
        """禁用功能"""
        prefs = self.get_preferences()
        
        if 'features' in prefs:
            prefs['features'][feature_name] = False
    
    def is_feature_enabled(self, feature_name: str) -> bool:
        """检查功能是否启用"""
        prefs = self.get_preferences()
        features = prefs.get('features', {})
        return features.get(feature_name, False)


def setup_plugin_configuration():
    """设置插件配置"""
    config_manager = PluginConfigManager("MyBlenderPlugin")
    
    # 设置默认配置
    config_manager.set('version', '1.0.0')
    config_manager.set('auto_save', True)
    config_manager.set('save_interval', 300)
    config_manager.set('debug_mode', False)
    config_manager.set('max_history', 20)
    
    prefs_manager = AddonPreferencesManager("MyBlenderPlugin")
    prefs_manager.set_hotkey("open_panel", 'P', ctrl=True)
    prefs_manager.set_ui_scale(1.0)
    prefs_manager.set_cache_size(500)
    
    print("插件配置已设置")
    
    return config_manager, prefs_manager
```

### 场景状态保存系统

```python
import bpy
from idprop.types import IDPropertyGroup
from typing import Dict, List, Any
import json

class SceneStateManager:
    """场景状态管理器"""
    
    def __init__(self):
        self.state_group = "_scene_states"
        self._ensure_state_group()
    
    def _ensure_state_group(self):
        """确保状态组存在"""
        if self.state_group not in bpy.context.scene:
            bpy.context.scene[self.state_group] = {}
    
    @property
    def states(self):
        """获取所有状态"""
        return bpy.context.scene[self.state_group]
    
    def save_current_state(self, name: str) -> bool:
        """保存当前场景状态"""
        scene = bpy.context.scene
        
        state = {
            'frame': scene.frame_current,
            'frame_start': scene.frame_start,
            'frame_end': scene.frame_end,
            'render_resolution': (scene.render.resolution_x, scene.render.resolution_y),
            'object_states': self._save_object_states(),
            'collection_states': self._save_collection_states()
        }
        
        self.states[name] = state
        print(f"已保存状态: {name}")
        return True
    
    def _save_object_states(self) -> Dict[str, Dict]:
        """保存对象状态"""
        object_states = {}
        
        for obj in bpy.data.objects:
            object_states[obj.name] = {
                'location': list(obj.location),
                'rotation_euler': list(obj.rotation_euler),
                'scale': list(obj.scale),
                'visible': obj.visible_get(),
                'select': obj.select_get()
            }
        
        return object_states
    
    def _save_collection_states(self) -> Dict[str, Dict]:
        """保存集合状态"""
        collection_states = {}
        
        for coll in bpy.data.collections:
            collection_states[coll.name] = {
                'hide_viewport': coll.hide_viewport,
                'hide_render': coll.hide_render,
                'objects': [obj.name for obj in coll.objects]
            }
        
        return collection_states
    
    def restore_state(self, name: str) -> bool:
        """恢复场景状态"""
        if name not in self.states:
            print(f"未找到状态: {name}")
            return False
        
        state = self.states[name]
        scene = bpy.context.scene
        
        # 恢复帧设置
        scene.frame_current = state.get('frame', 1)
        scene.frame_start = state.get('frame_start', 1)
        scene.frame_end = state.get('frame_end', 250)
        
        # 恢复渲染分辨率
        res = state.get('render_resolution', (1920, 1080))
        scene.render.resolution_x = res[0]
        scene.render.resolution_y = res[1]
        
        # 恢复对象状态
        if 'object_states' in state:
            self._restore_object_states(state['object_states'])
        
        # 恢复集合状态
        if 'collection_states' in state:
            self._restore_collection_states(state['collection_states'])
        
        print(f"已恢复状态: {name}")
        return True
    
    def _restore_object_states(self, object_states: Dict[str, Dict]) -> None:
        """恢复对象状态"""
        for obj_name, obj_state in object_states.items():
            obj = bpy.data.objects.get(obj_name)
            if obj:
                obj.location = obj_state.get('location', obj.location)
                obj.rotation_euler = obj_state.get('rotation_euler', obj.rotation_euler)
                obj.scale = obj_state.get('scale', obj.scale)
    
    def _restore_collection_states(self, collection_states: Dict[str, Dict]) -> None:
        """恢复集合状态"""
        for coll_name, coll_state in collection_states.items():
            coll = bpy.data.collections.get(coll_name)
            if coll:
                coll.hide_viewport = coll_state.get('hide_viewport', False)
                coll.hide_render = coll_state.get('hide_render', False)
    
    def delete_state(self, name: str) -> bool:
        """删除状态"""
        if name in self.states:
            del self.states[name]
            print(f"已删除状态: {name}")
            return True
        return False
    
    def list_states(self) -> List[str]:
        """列出所有保存的状态"""
        return list(self.states.keys())
    
    def export_states(self, filepath: str) -> bool:
        """导出所有状态到文件"""
        try:
            with open(filepath, 'w', encoding='utf-8') as f:
                json.dump(dict(self.states), f, indent=2, ensure_ascii=False)
            print(f"状态已导出到: {filepath}")
            return True
        except Exception as e:
            print(f"导出失败: {e}")
            return False
    
    def import_states(self, filepath: str) -> bool:
        """从文件导入状态"""
        try:
            with open(filepath, 'r', encoding='utf-8') as f:
                imported_states = json.load(f)
            
            self.states.clear()
            self.states.update(imported_states)
            
            print(f"状态已从 {filepath} 导入")
            return True
        except Exception as e:
            print(f"导入失败: {e}")
            return False


def manage_scene_states():
    """管理场景状态"""
    manager = SceneStateManager()
    
    # 保存当前状态
    manager.save_current_state("initial")
    
    # 执行一些操作...
    
    # 恢复初始状态
    manager.restore_state("initial")
    
    # 列出所有状态
    print(f"已保存的状态: {manager.list_states()}")
    
    return manager
```

## 最佳实践

1. **命名规范**：使用前缀或特定命名空间组织自定义属性，避免冲突
2. **类型安全**：始终进行类型检查和转换，确保属性类型正确
3. **版本控制**：在属性组中包含版本信息，便于未来迁移和升级
4. **数据验证**：设置属性时进行范围和有效性验证
5. **备份恢复**：实现状态的保存和恢复机制，保护用户数据


