# bpy_scripting

---
## bpy_python_scripting

# Python脚本与扩展开发模块

## Python脚本概述

Python 是一种解释型、交互式、面向对象的编程语言。它结合了模块、异常、动态类型、非常高级的动态数据结构和类。Python 将卓越的能力与非常清晰的语法相结合。

Python 脚本是扩展 Blender 功能的通用方法。Blender 的大多数领域都可以编写脚本，包括动画、渲染、导入导出、对象创建和自动化重复任务。

```python
# Blender Python API
# API 是与 Blender 交互的主要方式
# 提供对所有 Blender 功能的编程访问

# API 文档链接
# https://docs.blender.org/api/current/
# 官方 API 文档，用于编写脚本时参考

# 快速入门
# https://docs.blender.org/api/current/info_quickstart.html
# 简短介绍，包含示例

# 脚本编写有用链接
# Python.org - Python 通用信息
# Blender Python API - 官方 API 文档
# API Introduction - 快速入门指南
# Blender Artists - Python 支持论坛
```

## 扩展Blender

### 插件系统

插件是使 Blender 获得额外功能的脚本，可以从扩展中启用。Blender 可执行文件之外，有数百个由许多人编写的插件。

```python
# 插件类型
# 官方支持的插件与 Blender 捆绑
# 其他插件由 Blender Extensions 提供
# 这些不是官方发布的一部分
# 许多工作可靠且非常有用

# 插件文档
# 查看 /addons/index 了解 Blender 附带的插件文档

# 插件安装
# 通过 Blender 首选项中的扩展安装
# 单击安装按钮并选择 .zip 文件

# 插件文件位置
# 所有脚本从 scripts 文件夹加载
# 本地、系统和用户路径
# 可以在首选项中设置额外的搜索路径
```

### 脚本类型

除了插件外，还有几种其他类型的脚本可以扩展 Blender 的功能：

```python
# 模块
# 实用程序库，用于导入其他脚本

# 预设
# Blender 工具和键配置的设置

# 启动文件
# 这些文件在启动 Blender 时导入
# 定义大部分 Blender 的 UI
# 以及一些额外的核心操作符

# 自定义脚本
# 与插件相比，它们通常用于通过文本编辑器一次性执行
```

## Blender Python API 基础

### 核心模块

```python
# bpy 模块
# Blender Python API 的顶层模块
# 所有 Blender 功能通过 bpy 访问

# 常用别名
C  # 快速访问 bpy.context
D  # 快速访问 bpy.data
bpy  # 顶层 Blender Python API 模块

# 主要子模块
bpy.context  # 当前上下文
bpy.data  # 所有数据块的访问
bpy.ops  # 操作符
bpy.types  # 类型定义
bpy.utils  # 实用工具函数
bpy.app  # 应用程序信息
```

### 上下文访问

```python
# bpy.context 提供对当前状态的访问
# 包括场景、选中对象、当前对象模式等

# 上下文属性
context.scene  # 当前场景
context.selected_objects  # 选中的对象列表
context.active_object  # 活动对象
context.mode  # 当前模式
context.region  # 当前区域
context.space_data  # 当前空间数据
context.tool_settings  # 工具设置

# 示例
import bpy
obj = bpy.context.active_object
print(obj.name)
```

### 数据访问

```python
# bpy.data 提供对所有数据块的访问

# 数据集合
bpy.data.objects  # 所有对象
bpy.data.meshes  # 所有网格数据
bpy.data.materials  # 所有材质
bpy.data.scenes  # 所有场景
bpy.data.cameras  # 所有相机
bpy.data.lights  # 所有灯光
bpy.data.texts  # 所有文本数据块
bpy.data.armatures  # 所有骨架
bpy.data.curves  # 所有曲线
bpy.data.collections  # 所有集合

# 遍历数据
for obj in bpy.data.objects:
    print(obj.name)

# 通过名称访问
obj = bpy.data.objects["Cube"]
```

### 操作符

```python
# bpy.ops 包含所有操作符
# 操作符是 Blender 中可执行的动作

# 操作符调用
bpy.ops.object.select_all(action='SELECT')
bpy.ops.mesh.primitive_cube_add(size=2, location=(0, 0, 0))
bpy.ops.object.delete()

# 操作符属性
# 操作符有上下文要求
# 必须在正确的模式和区域调用
# 返回包含结果信息的集合

# 错误处理
result = bpy.ops.object.delete()
if result {'FINISHED'}:
    print("删除成功")
```

## 脚本开发工具

### 文本编辑器

文本编辑器可用于编写 Python 脚本、基于脚本的着色器（如 OSL 和 GLSL）或纯文本注释。

```python
# 打开文本编辑器
# 切换到 Scripting 工作区
# 或按 Shift-F11 替换当前编辑器

# 文本编辑器功能
# 新建文本 - Alt+N
# 打开文件 - Alt+O
# 重新加载 - Alt+R
# 外部编辑
# 保存 - Alt+S
# 另存为 - Shift+Ctrl+Alt+S

# 运行脚本
# Alt+P 运行脚本

# 显示选项
# 行号显示
# 自动换行
# 语法高亮
# 强调当前行

# 外部冲突解决
# 当外部文本文件从另一个程序更新时
# 重新从磁盘加载
# 使文本内部
# 忽略
```

### Python控制台

Python 控制台提供了一种快速测试代码片段和探索 Blender API 的方法。它执行您在 >>> 提示符下键入的任何内容，并具有命令历史和自动完成功能。

```python
# 控制台别名
C  # 快速访问 bpy.context
D  # 快速访问 bpy.data
bpy  # 顶层 Blender Python API 模块

# 控制台操作
# 清除所有 - 刷新控制台
# 清除行 - Shift+Return
# 复制为脚本 - Shift+Ctrl+C
# 粘贴 - Ctrl+V
# 自动完成 - Tab

# 历史导航
# 上箭头 - 历史中的上一个命令
# 下箭头 - 历史中的下一个命令

# 查看环境
# 输入 dir() 并按 Return 执行
# 查看全局函数和变量列表

# 自动完成
# 键入 bpy. 并按 Tab
# 显示模块的可用成员
# 子模块以绿色列出
# 属性和方法以相同方式列出
# 方法以尾随 ( 指示
```

## 插件开发

### 插件结构

```python
# 传统插件结构
bl_info = {
    "name": "My Add-on",
    "author": "Your Name",
    "version": (1, 0, 0),
    "blender": (3, 0, 0),
    "location": "View3D > Add-ons",
    "description": "Description of the add-on",
    "warning": "",
    "doc_url": "",
    "category": "Add-on",
}

import bpy

class MyOperator(bpy.types.Operator):
    """My Operator Description"""
    bl_idname = "my.operator"
    bl_label = "My Operator"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        # 执行操作
        return {'FINISHED'}

def register():
    bpy.utils.register_class(MyOperator)

def unregister():
    bpy.utils.unregister_class(MyOperator)

if __name__ == "__main__":
    register()
```

### 插件注册

```python
# 注册类
# 操作符
# 面板
# 属性组
# 菜单
# 工具

# 注册函数
def register():
    bpy.utils.register_class(MyOperator)
    bpy.utils.register_class(MyPanel)

# 注销函数
def unregister():
    bpy.utils.unregister_class(MyPanel)
    bpy.utils.unregister_class(MyOperator)

# 检查是否作为主程序运行
if __name__ == "__main__":
    register()
```

### 面板创建

```python
# 创建面板
class MyPanel(bpy.types.Panel):
    """My Panel Description"""
    bl_label = "My Panel"
    bl_idname = "PT_MyPanel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'My Add-on'

    def draw(self, context):
        layout = self.layout
        layout.operator("my.operator")
```

### 操作符创建

```python
# 操作符类型
# 执行一次性操作
# 有用户界面
# 支持撤销

# 基本操作符
class MyOperator(bpy.types.Operator):
    bl_idname = "my.operator"
    bl_label = "My Operator"
    bl_options = {'REGISTER', 'UNDO'}

    # 属性
    my_int: bpy.props.IntProperty()
    my_float: bpy.props.FloatProperty()
    my_string: bpy.props.StringProperty()
    my_enum: bpy.props.EnumProperty(
        items=[
            ('OPT1', "Option 1", "Description 1"),
            ('OPT2', "Option 2", "Description 2"),
        ]
    )

    def execute(self, context):
        # 执行主要操作
        return {'FINISHED'}

    def invoke(self, context, event):
        # 处理鼠标交互
        return {'RUNNING_MODAL'}

    def draw(self, context):
        # 在弹出面板中绘制UI
        layout = self.layout
```

### 插件转换为扩展

Blender 4.2 引入了扩展系统，旧的插件创建方式被视为已弃用。转换过程如下：

```python
# 创建清单文件 manifest.toml
# 移除 bl_info 信息
# 将所有模块名称引用替换为 __package__
# 使所有模块导入使用相对导入
# 使用 Python Wheels 打包外部 Python 依赖

# 扩展命名空间
# 传统插件使用模块名访问首选项
# 扩展使用仓库名称作为命名空间的一部分
# 例如：bl_ext.{repository_module_name}.kitsu

# 使用 __package__
# 插件可以定义自己的首选项
# 使用包名称作为标识符
# 使用 __package__ 访问
```

## 脚本安全

### 安全风险

在 blend-file 中包含 Python 脚本的能力对于绑定和自动化等高级任务很有价值。但是，它带来了安全风险，因为 Python 不限制脚本可以执行的操作。

```python
# 安全风险
# Python 不限制脚本可以执行的操作
# 恶意脚本可能执行有害操作
# 只运行来自您知道和信任的来源的脚本

# 默认设置
# 自动执行是默认禁用的
# 某些 blend-file 需要此功能才能正常工作
```

### 脚本执行方式

```python
# 自动执行
# 注册的文本块 - 文本数据块的注册选项启用
# 动画驱动程序 - Python 表达式用于驱动值

# 手动执行
# 在文本编辑器中运行脚本
# 使用 Freestyle 渲染
# 这些需要用户交互

# Trusted Source
# 文件浏览器有 Trusted Source 选项
# 可以逐案控制自动执行
```

### 控制脚本执行

```python
# 首选项设置
# 自动运行 Python 脚本
# 这意味着 Trusted Source 选项将默认启用
# 脚本可以在加载 blend-file 时运行

# 命令行参数
# -y 或 --enable-autoexec 启用
# -Y 或 --disable-autoexec 禁用

# 示例
blender --background --enable-autoexec my_movie.blend --render-anim
```

### 安全最佳实践

```python
# 只运行可信脚本
# 避免运行来源不明的脚本
# 检查脚本内容
# 了解脚本的功能

# 隔离测试
# 在独立环境中测试脚本
# 使用沙盒或虚拟机
# 限制脚本权限

# 定期更新
# 保持 Blender 和插件更新
# 应用安全补丁
# 监控已知漏洞
```

## 网络访问

```python
# 如果插件需要使用互联网
# 必须检查 bpy.app.online_access（只读属性）
# 此选项由首选项控制
# 可以通过命令行覆盖（--offline-mode / --online-mode）

# 检查在线访问
if bpy.app.online_access:
    # 执行网络请求

# 错误消息处理
# 检查 bpy.app.online_access_override
# 确定用户是否可以打开在线访问
```

## 本地存储

```python
# 插件不能假设自己的目录可写
# 系统仓库可能不可写
# 写入插件目录也有缺点
# 升级扩展将删除所有文件

# 使用用户目录
extension_directory = bpy.utils.extension_path_user(
    __package__, path="", create=True
)

# 创建子目录
# 使用 path 参数
# 目录将在升级之间保留
# 但在扩展卸载时将被删除
```

## 依赖管理

### 捆绑依赖

```python
# 扩展必须自包含
# 必须自带所有依赖项
# 特别是第三方 Python 模块

# 选项
# 1. 使用 Python Wheels 捆绑
#    这是推荐的捆绑依赖方式

# 2. 捆绑其他插件
#    如果插件依赖另一个插件
#    确保单独和组合的插件
#    检查已注册的类型

# 3. 使用 Vendorize
#    可以将纯 Python 依赖作为子模块捆绑
#    避免版本冲突
```

## API使用最佳实践

### 性能优化

```python
# 避免不必要的更新
# 使用低层级 API
# 批处理操作
# 避免在循环中访问属性

# 正确示例
for obj in objects:
    obj.location.x += 1  # 每次更新

# 高效示例
for obj in objects:
    obj.location.x += 1
# 批量更新
bpy.context.view_layer.update()

# 使用适当的循环
# for 循环比 while 循环更高效
# 使用列表推导式
# 避免不必要的迭代
```

### 错误处理

```python
# 异常处理
try:
    bpy.ops.object.delete()
except RuntimeError as e:
    print(f"错误: {e}")

# 检查前置条件
if not bpy.context.selected_objects:
    return {'CANCELLED'}

# 返回适当的结果
# {'FINISHED'} - 操作成功
# {'CANCELLED'} - 操作取消
# {'RUNNING_MODAL'} - 正在运行模态
# {'PASS_THROUGH'} - 传递事件
```

### 代码组织

```python
# 模块化代码
# 将功能分组到模块中
# 使用类和函数
# 避免全局变量

# 命名约定
# 类名使用 PascalCase
# 函数和变量使用 snake_case
# 常量使用 UPPER_SNAKE_CASE

# 文档字符串
# 为所有公共函数和类添加文档
# 说明参数和返回值
# 提供使用示例
```

## 常见开发任务

### 创建自定义操作符

```python
# 1. 定义操作符类
class OBJECT_OT_my_operator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "My Custom Operator"
    bl_description = "Does something useful"
    bl_options = {'REGISTER', 'UNDO'}

    # 属性定义
    my_property: bpy.props.FloatProperty(
        name="My Property",
        description="Description",
        default=1.0,
        min=0.0,
        max=10.0
    )

    # 执行方法
    def execute(self, context):
        # 实现功能
        return {'FINISHED'}

    # 绘制方法（可选）
    def draw(self, context):
        layout = self.layout
        layout.prop(self, "my_property")

    # 调用方法（可选）
    def invoke(self, context, event):
        return context.window_manager.invoke_props_dialog(self)
```

### 创建自定义面板

```python
# 1. 定义面板类
class VIEW3D_PT_my_panel(bpy.types.Panel):
    bl_label = "My Panel"
    bl_idname = "VIEW3D_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'My Add-on'

    def draw(self, context):
        layout = self.layout
        col = layout.column()
        col.operator("object.my_operator")
        col.prop(context.scene, "my_scene_property")
```

### 创建菜单

```python
# 1. 定义菜单类
class VIEW3D_MT_my_menu(bpy.types.Menu):
    bl_label = "My Menu"
    bl_idname = "VIEW3D_MT_my_menu"

    def draw(self, context):
        layout = self.layout
        layout.operator("object.my_operator")
        layout.separator()
        layout.operator("object.another_operator")

# 2. 将菜单添加到现有菜单
def menu_func(self, context):
    self.layout.separator()
    self.layout.menu("VIEW3D_MT_my_menu")

# 3. 注册菜单和绘制函数
bpy.utils.register_class(VIEW3D_MT_my_menu)
bpy.types.VIEW3D_MT_add.prepend(menu_func)
```

## 调试技巧

```python
# 打印调试
print("调试信息")
print(f"变量值: {variable}")

# 异常追踪
import traceback
try:
    # 代码
except Exception as e:
    traceback.print_exc()

# API 探索
# 在 Python 控制台中使用 dir()
# 查看对象的属性和方法
# 使用 help() 获取文档

# 性能分析
import time
start = time.time()
# 代码
end = time.time()
print(f"执行时间: {end - start}")
```

## 插件发布

```python
# 准备发布
# 清理代码
# 添加文档
# 测试所有功能
# 创建用户指南

# 版本控制
# 遵循语义版本控制
# 更新版本号
# 记录更改

# 扩展平台
# 共享到 extensions.blender.org
# 遵循扩展指南
# 确保兼容性

# 社区支持
# 提供支持渠道
# 响应问题报告
# 更新插件
```


---
## bpy_scripting_animation

# Blender 脚本开发与动画系统指南

本模块整合 Blender 脚本开发、Python 控制台使用、动画驱动和节点编辑器操作的高级技能，帮助开发者创建专业的 Blender 插件和自动化工具。

## 概述

Blender 提供了强大的脚本开发环境，包括 Python 控制台、文本编辑器、动画驱动系统和节点编辑器。理解这些工具的使用方法对于高效开发 Blender 插件和自动化工作流程至关重要。

**核心概念：**
- Python 控制台快速测试和探索 API
- 文本编辑器的脚本开发和运行功能
- 动画驱动的数学表达式和自定义属性
- 节点编辑器的程序化操作

## 核心功能

### Python 控制台

Python 控制台提供了快速测试代码片段和探索 Blender API 的方式。

**别名系统：**

```python
# 控制台可用的便捷别名
C  # 等同于 bpy.context，快速访问当前上下文
D  # 等同于 bpy.data，访问所有 blend 文件数据
bpy  # Blender Python API 主模块

# 使用示例
C.object  # 获取活动对象
C.scene   # 获取当前场景
D.objects # 获取所有对象列表
```

**自动补全：**

```python
# 输入模块名后按 Tab 键查看可用成员
bpy.  # 显示 bpy 模块的所有子模块、属性和方法
bpy.ops.  # 显示所有操作符
bpy.data.  # 显示所有数据块类型

# 智能感知示例
# 输入 bpy.context. 后按 Tab 键
# 显示: active_object, selected_objects, mode, scene 等
```

**执行和历史：**

```python
# 执行当前行
Ctrl + Return  # 执行命令

# 添加到历史但不执行
Shift + Return

# 历史导航
Up/Down  # 上一条/下一条命令

# 复制为脚本
Shift + Ctrl + C  # 将完整历史缓冲区复制到剪贴板
```

### 文本编辑器

Blender 文本编辑器支持 Python 脚本开发、OSL/GLSL 着色器编写和普通文本注释。

**基本操作：**

```python
# 创建新文本
bpy.ops.text.new()

# 打开外部文件
bpy.ops.text.open(filepath="C:/path/to/script.py")

# 保存文件
bpy.ops.text.save()
bpy.ops.text.save_as(filepath="C:/path/to/new_script.py")

# 重新加载
bpy.ops.text.reload()
```

**脚本执行：**

```python
# 运行当前脚本
bpy.ops.text.run_script()

# 外部编辑
bpy.ops.text.jump_to_file_at_point()

# 冲突解决
bpy.ops.text.resolve_conflict(action='RELOAD')  # 从磁盘重新加载
bpy.ops.text.resolve_conflict(action='MAKE_INTERNAL')  # 转为内部文本
bpy.ops.text.resolve_conflict(action='IGNORE')  # 忽略
```

**自动运行：**

```python
# 启用 Register 选项后，文本数据块将在 blend 文件加载时自动执行
text_block.use_module = True

# Live Edit - 每次修改后自动运行
space = bpy.context.space_text
space.use_live_edit = True
```

**视图控制：**

```python
# 语法高亮和显示选项
bpy.ops.text.toggle('INVOKE_DEFAULT')  # 切换边栏
bpy.ops.text.move(type='LINE_BEGIN')   # 移动到行首
bpy.ops.text.move(type='LINE_END')     # 移动到行尾
bpy.ops.text.move(type='FILE_TOP')     # 文件顶部
bpy.ops.text.move(type='FILE_BOTTOM')  # 文件底部
bpy.ops.text.jump(line_number=100)     # 跳转到指定行
```

### 动画驱动

驱动是一种通过函数或数学表达式控制属性值的方式。

**驱动基础：**

```python
# 创建驱动
obj = bpy.context.object

# 添加驱动到属性
obj.driver_add("rotation_euler", 0)  # 为 X 轴旋转添加驱动

# 手动设置驱动表达式（通过 API）
driver = obj.driver_remap(driver_index=0)
driver.type = 'AVERAGE'  # AVG, SUM, MIN, MAX, SCRIPTED
```

**驱动变量：**

```python
# 创建驱动变量
var = driver.variables.new()
var.name = "var_name"  # 变量名，用于表达式中引用

# 设置变量类型和目标
var.type = 'TRANSFORMS'  # TRANSFORMS, SINGLE_PROP, LOC_DIFF, ROT_DIFF, etc.

# 配置变换目标
target = var.targets[0]
target.id = bpy.data.objects["TargetObject"]
target.data_path = "scale"  # 目标属性的数据路径
target.transform_type = 'SCALE'
target.transform_space = 'LOCAL'
```

**脚本表达式：**

```python
# 设置脚本表达式
driver.type = 'SCRIPTED'
driver.expression = "var_name * 2 + 1"

# 简单表达式（可原生评估，无需 Python）
# 支持: +, -, *, /, //, %, @, ** 等
# 函数: sin, cos, tan, radians, degrees, abs, floor, ceil, round, etc.
# 变量引用: 使用驱动变量名

# 示例：旋转跟随缩放
driver.expression = "var_scale * 90"  # 缩放值为 1 时旋转 90 度

# 复杂表达式（需要 Python 解释器）
driver.expression = "sin(var * 3.14159 / 180)"
```

**F-Curve 操作：**

```python
# 获取驱动的 F-Curve
fcurve = obj.animation_data.drivers[0].fcurves[0]

# 添加修改器
modifier = fcurve.modifiers.new('CYCLES')

# 设置关键帧
fcurve.keyframe_points.add(count=2)
fcurve.keyframe_points[0].co = (0, 0)
fcurve.keyframe_points[1].co = (100, 360)
```

### 节点编辑器操作

Blender 的节点编辑器用于材质、几何节点和合成节点的程序化操作。

**几何节点编辑器：**

```python
# 访问几何节点编辑器
space = bpy.context.space_data
space.node_tree_type = 'GeometryNodeTree'

# 获取节点树
node_group = bpy.data.node_groups["GeometryNodes"]

# 添加节点
node = node_group.nodes.new(type='GeometryNodeMeshCube')
node.location = (0, 0)

# 连接节点
links = node_group.links
input_socket = node_group.nodes["Cube"].outputs["Mesh"]
output_socket = node_group.nodes["Group Output"].inputs["Geometry"]
links.new(input_socket, output_socket)

# 设置属性
node.inputs["Size"].default_value = 2.0
node.inputs["Resolution"].default_value = 64

# 工具上下文设置
tree = bpy.data.node_groups["MyTree"]
tree.is_type_mesh = True   # 支持网格对象
tree.is_type_curve = True  # 支持曲线对象
tree.is_mode_object = True  # 支持对象模式
tree.is_mode_edit = True    # 支持编辑模式
```

**着色器编辑器：**

```python
# 访问着色器编辑器
space = bpy.context.space_data
space.node_tree_type = 'ShaderNodeTree'
space.shader_type = 'OBJECT'  # OBJECT, WORLD, LINE_STYLE

# 获取材质节点树
material = bpy.data.materials["MyMaterial"]
material.use_nodes = True
node_tree = material.node_tree

# 添加着色节点
node = node_tree.nodes.new(type='ShaderNodeBsdfPrincipled')
node.location = (0, 0)

# 设置属性
node.inputs["Base Color"].default_value = (1, 0, 0, 1)
node.inputs["Metallic"].default_value = 0.5
node.inputs["Roughness"].default_value = 0.2

# 创建输出
node_tree.nodes.new(type='ShaderNodeOutputMaterial')

# 链接节点
links = node_tree.links
links.new(bsdf.outputs["BSDF"], output.inputs["Surface"])
```

**节点操作：**

```python
# 选择节点
node.select = True

# 自动布局
bpy.ops.node.autoposition()

# 链接和断开
bpy.ops.node.link('INVOKE_DEFAULT')
bpy.ops.node.links_cut('INVOKE_DEFAULT')

# 删除未使用节点
bpy.ops.node.delete_replaced()

# 复制粘贴
bpy.ops.node.duplicate('INVOKE_DEFAULT')
bpy.ops.node.paste('INVOKE_DEFAULT')
```

## 实用函数

### 快速 API 探索

```python
def explore_module(module, depth=0):
    """探索模块的所有成员"""
    for attr_name in dir(module):
        if attr_name.startswith('_'):
            continue
        try:
            attr = getattr(module, attr_name)
            print('  ' * depth + f"{attr_name}: {type(attr).__name__}")
        except Exception as e:
            print('  ' * depth + f"{attr_name}: ERROR - {e}")

def get_all_operators():
    """获取所有可用操作符"""
    import operator
    ops = []
    for attr_name in dir(bpy.ops):
        op = getattr(bpy.ops, attr_name)
        if hasattr(op, 'poll'):
            ops.append(attr_name)
    return ops

def get_property_path(obj, prop):
    """获取属性的数据路径"""
    if hasattr(obj, prop):
        return f'["{prop}"]'
    else:
        parts = prop.split('.')
        # 递归查找嵌套属性
        return '.'.join(parts)
```

### 驱动工具函数

```python
def create_driver_expression(driver, variables, expression):
    """创建带变量的驱动表达式"""
    for var_data in variables:
        var = driver.variables.new()
        var.name = var_data['name']
        var.type = var_data['type']
        
        target = var.targets[0]
        target.id = var_data['target']
        if 'data_path' in var_data:
            target.data_path = var_data['data_path']
    
    driver.type = 'SCRIPTED'
    driver.expression = expression

def add_simple_driver(obj, prop_path, target_obj, target_prop, factor=1.0):
    """添加简单驱动：obj.prop = target_obj.target_prop * factor"""
    driver = obj.driver_add(prop_path)
    
    var = driver.variables.new()
    var.name = "var"
    var.type = 'TRANSFORMS'
    
    target = var.targets[0]
    target.id = target_obj
    target.data_path = f'["{target_prop}"]' if target_prop.startswith('_') else target_prop
    target.transform_type = 'SCALE'
    target.transform_space = 'LOCAL'
    
    driver.type = 'AVERAGE'
    return driver

def setup_ramp_driver(driver_func):
    """创建带 Ramp 的驱动函数"""
    # 添加 Generator 修改器
    modifier = driver.fcurves[0].modifiers.new('GENERATOR')
    modifier.order = 2
    # 配置多项式系数
```

### 节点树工具

```python
def create_node_group(node_type='GeometryNodeTree', name="NewGroup"):
    """创建新节点组"""
    if node_type == 'GeometryNodeTree':
        node_group = bpy.data.node_groups.new(name, 'GeometryNodeTree')
    else:
        node_group = bpy.data.node_groups.new(name, 'ShaderNodeTree')
    
    # 添加输入输出节点
    input_node = node_group.nodes.new('NodeGroupInput')
    output_node = node_group.nodes.new('NodeGroupOutput')
    
    input_node.location = (-200, 0)
    output_node.location = (200, 0)
    
    return node_group

def connect_nodes(node_tree, output_node, output_name, input_node, input_name):
    """连接两个节点"""
    link = node_tree.links.new
    socket_out = output_node.outputs[output_name]
    socket_in = input_node.inputs[input_name]
    link(socket_out, socket_in)

def set_node_input_value(node, input_name, value):
    """设置节点输入值"""
    if input_name in node.inputs:
        node.inputs[input_name].default_value = value

def duplicate_node_tree(source_tree, new_name):
    """复制节点树"""
    new_tree = source_tree.copy()
    new_tree.name = new_name
    return new_tree
```

### 动画工具

```python
def insert_keyframe(obj, data_path, frame, value=None):
    """插入关键帧"""
    if value is not None:
        setattr(obj, data_path, value)
    obj.keyframe_insert(data_path=data_path, frame=frame)

def copy_action(source_obj, target_obj):
    """复制动作到另一对象"""
    if source_obj.animation_data and source_obj.animation_data.action:
        action = source_obj.animation_data.action.copy()
        if target_obj.animation_data is None:
            target_obj.animation_data_create()
        target_obj.animation_data.action = action

def batch_keyframe(objects, data_path, start_frame, end_frame, step=10):
    """批量为多个对象添加关键帧"""
    for obj in objects:
        for frame in range(start_frame, end_frame + 1, step):
            obj.keyframe_insert(data_path=data_path, frame=frame)

def create_cyclic_motion(obj, data_path, period=100, amplitude=1.0):
    """创建循环动画"""
    driver = obj.driver_add(data_path)
    var = driver.variables.new()
    var.name = "t"
    var.type = 'TRANSFORMS'
    var.targets[0].data_path = "frame"
    var.targets[0].transform_type = 'TIME'
    driver.expression = f"{amplitude} * sin(t * 2 * 3.14159 / {period})"
```

## 常见问题排查

### 问题：驱动表达式不生效

**可能原因：**
1. 变量名与表达式中引用不一致
2. 目标数据路径错误
3. 表达式包含复杂函数

**排查步骤：**

```python
# 1. 检查变量名
for var in driver.variables:
    print(f"变量名: {var.name}, 类型: {var.type}")
    for target in var.targets:
        print(f"  目标ID: {target.id}, 数据路径: {target.data_path}")

# 2. 检查表达式
print(f"驱动类型: {driver.type}")
print(f"表达式: {driver.expression}")

# 3. 测试简单表达式
driver.expression = "var * 1"  # 最小化测试
```

**解决方案：**

```python
# 确保变量名在表达式中正确引用
# 变量名为 "my_var"，表达式中使用 "my_var"

# 使用简单表达式避免 Python 评估
# 简单: "var * 2 + 1"
# 复杂: "math.sin(var)" (需要 import math)

# 检查数据路径是否正确
# 对象属性: "location.x", "rotation_euler[0]"
# 自定义属性: '["my_prop"]'
```

### 问题：节点连接失败

**可能原因：**
1. 插座类型不匹配
2. 节点未初始化完成
3. 节点树被锁定

**解决方案：**

```python
# 1. 检查插座类型
def get_socket_type(socket):
    return type(socket).__name__

# 2. 确保节点已添加到节点树
if node.name not in node_tree.nodes:
    node_tree.nodes.link(node)

# 3. 使用正确的连接方法
link = node_tree.links.new
link(output_socket, input_socket)
```

### 问题：控制台别名不可用

**解决方案：**

```python
# 控制台别名在脚本中不可用，需要使用完整路径
# 脚本中
bpy.context  # 而不是 C
bpy.data     # 而不是 D

# 使用上下文快捷方式
ctx = bpy.context
obj = ctx.active_object
```

### 问题：脚本修改后不生效

**解决方案：**

```python
# 1. 重新运行脚本
# 在文本编辑器中按 Alt+P

# 2. 启用 Live Edit
space = bpy.context.space_text
space.use_live_edit = True

# 3. 重新注册类
bpy.utils.unregister_class(MyClass)
bpy.utils.register_class(MyClass)

# 4. 刷新 UI
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        for region in area.regions:
            if region.type == 'UI':
                bpy.ops.region.tag_redraw(region)
```

### 问题：动画曲线插值错误

**解决方案：**

```python
# 1. 检查关键帧插值类型
for keyframe in fcurve.keyframe_points:
    print(f"插值类型: {keyframe.interpolation}")

# 2. 设置插值类型
keyframe.interpolation = 'BEZIER'  # BEZIER, LINEAR, CONSTANT
keyframe.easing = 'EASE_IN_OUT'    # EASE_IN, EASE_OUT, EASE_IN_OUT

# 3. 设置缓动
keyframe.handle_left_type = 'AUTO'
keyframe.handle_right_type = 'AUTO'
```

## 设计最佳实践

### 1. 模块化代码结构

```python
# 将复杂功能拆分为多个函数和类
class MyAddon:
    def __init__(self):
        self.setup_operators()
        self.setup_panels()
        self.setup_properties()
    
    def setup_operators(self):
        # 注册操作符
        pass
    
    def setup_panels(self):
        # 注册面板
        pass
    
    def setup_properties(self):
        # 注册属性
        pass

# 主入口点
def register():
    MyAddon()
```

### 2. 错误处理和日志

```python
import logging

# 设置日志
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

def my_function():
    try:
        # 操作代码
        result = do_something()
        logger.info(f"操作成功: {result}")
        return result
    except Exception as e:
        logger.error(f"操作失败: {e}")
        raise
```

### 3. 性能优化

```python
# 1. 批量操作
with bpy.context.temp_override(object=obj):
    bpy.ops.object.select_all(action='SELECT')

# 2. 禁用更新
for obj in bpy.data.objects:
    obj.select_set(True)
bpy.ops.object.delete()

# 3. 使用适当的数据结构
# 使用集合而非列表进行成员检查
objects_set = set(bpy.data.objects)

# 4. 避免频繁的上下文切换
ctx = bpy.context
scene = ctx.scene
for obj in scene.objects:
    if obj.type == 'MESH':
        # 处理网格
        pass
```

### 4. 版本兼容性

```python
def check_version(major, minor=0):
    """检查 Blender 版本"""
    version = bpy.app.version
    return version >= (major, minor)

# 根据版本使用不同 API
if check_version(4, 2):
    # 使用扩展系统
    from . import manifest
else:
    # 使用传统插件
    bl_info = {...}
```

### 5. 文档和注释

```python
class MyOperator(bpy.types.Operator):
    """简短描述（用于工具提示）"""
    
    bl_idname = "object.my_operator"
    bl_label = "操作名称"
    bl_options = {'REGISTER', 'UNDO'}
    
    def execute(self, context):
        """执行操作的主方法
        
        Args:
            context: Blender 上下文
            
        Returns:
            {'FINISHED'} 或 {'CANCELLED'}
        """
        return {'FINISHED'}
    
    def invoke(self, context, event):
        """处理用户交互"""
        return context.window_manager.invoke_props_dialog(self)
```

## 高级主题

### 自定义属性系统

```python
class CustomProperties(bpy.types.PropertyGroup):
    # 各种属性类型
    int_prop: bpy.props.IntProperty(
        name="整数",
        default=0,
        min=0,
        max=100
    )
    
    float_prop: bpy.props.FloatProperty(
        name="浮点数",
        default=1.0,
        min=0.0,
        max=10.0,
        step=0.1,
        precision=2
    )
    
    string_prop: bpy.props.StringProperty(
        name="字符串",
        maxlength=128
    )
    
    enum_prop: bpy.props.EnumProperty(
        name="枚举",
        items=[
            ('A', "选项 A", "第一个选项"),
            ('B', "选项 B", "第二个选项"),
        ],
        default='A'
    )
    
    bool_prop: bpy.props.BoolProperty(
        name="布尔值",
        default=True
    )
    
    # 颜色属性
    color_prop: bpy.props.FloatVectorProperty(
        name="颜色",
        size=4,
        min=0.0,
        max=1.0,
        subtype='COLOR',
        default=(1, 1, 1, 1)
    )
    
    # 路径属性
    path_prop: bpy.props.StringProperty(
        name="文件路径",
        subtype='FILE_PATH'
    )
    
    # 旋转属性（欧拉角）
    euler_prop: bpy.props.FloatVectorProperty(
        name="欧拉角",
        size=3,
        subtype='EULER'
    )

# 注册属性组
bpy.utils.register_class(CustomProperties)
bpy.types.Object.my_props = bpy.props.PointerProperty(type=CustomProperties)
```

### 模态操作符

```python
class ModalOperator(bpy.types.Operator):
    bl_idname = "object.modal_example"
    bl_label = "模态操作示例"
    
    def __init__(self):
        self._timer = None
        self._count = 0
    
    def execute(self, context):
        wm = context.window_manager
        self._timer = wm.event_timer_add(0.1, window=context.window)
        wm.modal_handler_add(self)
        return {'RUNNING_MODAL'}
    
    def modal(self, context, event):
        if event.type == 'ESC':
            return self.cancel(context)
        
        if event.type == 'LEFTMOUSE' and event.value == 'PRESS':
            self._count += 1
            self.report({'INFO'}, f"点击次数: {self._count}")
        
        if event.type == 'TIMER':
            # 定时更新
            pass
        
        return {'RUNNING_MODAL'}
    
    def finish(self, context):
        wm = context.window_manager
        if self._timer:
            wm.event_timer_remove(self._timer)
        return {'FINISHED'}
    
    def cancel(self, context):
        return self.finish(context)
```

### 上下文覆盖

```python
# 临时覆盖上下文执行操作
def safe_operator_execution():
    obj = bpy.data.objects.get("Cube")
    if obj:
        # 临时将活动对象设置为 obj
        with bpy.context.temp_override(object=obj):
            bpy.ops.object.copy()
            bpy.ops.object.paste()
    
    # 多重覆盖
    with bpy.context.temp_override(
        object=obj,
        active_object=obj,
        selected_objects=[obj]
    ):
        bpy.ops.object.delete()
```

### UI 列表和集合

```python
class ListItem(bpy.types.PropertyGroup):
    name: bpy.props.StringProperty(name="名称")
    value: bpy.props.FloatProperty(name="值")
    enabled: bpy.props.BoolProperty(name="启用", default=True)

class MY_UL_custom_list(bpy.types.UIList):
    def draw_item(self, context, layout, data, item, icon, active_data, active_propname):
        if self.layout_type in {'DEFAULT', 'COMPACT'}:
            row = layout.row(align=True)
            row.prop(item, "enabled", text="", emboss=False, icon='CHECKBOX_HLT' if item.enabled else 'CHECKBOX_DEHLT')
            row.prop(item, "name", text="", emboss=False)
            row.prop(item, "value", text="")
        elif self.layout_type in {'GRID'}:
            layout.alignment = 'CENTER'
            layout.prop(item, "name", text="")

# 使用 UIList
class ListPanel(bpy.types.Panel):
    def draw(self, context):
        layout = self.layout
        scene = context.scene
        
        row = layout.row()
        row.template_list(
            "MY_UL_custom_list",
            "",
            scene,
            "my_list",
            scene,
            "my_list_index",
            rows=5
        )


---
## bpy_scripting_templates

# 脚本模板模块

## 脚本模板概述

Blender Python脚本开发遵循一套成熟的模板模式，这些模板提供了标准化的代码结构，便于维护和扩展。理解并熟练运用这些模板是创建专业插件的基础。本模块涵盖了从简单脚本到完整插件的各种模板，以及代码组织和最佳实践。

```python
# 脚本类型
# 简单脚本 - 直接运行的Python代码
# 运算符脚本 - 定义bpy.ops.operator_name类型
# 面板脚本 - 创建UI面板
# 属性脚本 - 定义自定义属性
# 完整插件 - 包含注册和注销的完整模块
# 混合脚本 - 结合多种功能的复杂脚本

# 模板结构
# 1. 文档字符串 - 模块说明
# 2. 导入语句 - 依赖模块
# 3. 类定义 - 运算符、面板、属性组
# 4. 注册函数 - 插件注册
# 5. 注销函数 - 插件清理
# 6. 主入口 - 直接运行时执行
```

## 简单脚本模板

### 基础操作脚本

```python
"""
简单对象操作脚本

这个脚本演示了Blender Python的基本操作，
包括创建、修改和删除对象。

使用方法：
- 在Blender中打开脚本编辑器
- 粘贴代码并运行
"""

import bpy
import math

def create_centered_cube(size=1.0, location=(0, 0, 0)):
    """创建居中的立方体"""
    bpy.ops.mesh.primitive_cube_add(size=size, location=location)
    cube = bpy.context.active_object
    cube.name = "MyCube"
    return cube

def setup_scene():
    """设置基本场景"""
    # 清除默认立方体
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # 创建新对象
    cube = create_centered_cube(size=2.0)
    
    # 添加材质
    mat = bpy.data.materials.new(name="CubeMaterial")
    mat.diffuse_color = (0.2, 0.5, 0.8, 1.0)
    cube.data.materials.append(mat)
    
    # 添加灯光
    bpy.ops.object.light_add(type='SUN', location=(5, 5, 10))
    
    # 设置相机
    bpy.ops.object.camera_add(location=(0, -10, 5))
    camera = bpy.context.active_object
    camera.rotation_euler = (math.radians(60), 0, 0)
    bpy.context.scene.camera = camera

if __name__ == "__main__":
    setup_scene()
```

### 批量处理脚本

```python
"""
批量处理脚本模板

用于批量处理场景中的多个对象，
提供遍历、筛选和处理功能。

功能：
- 遍历所有对象
- 根据条件筛选
- 批量应用变换
- 生成处理报告
"""

import bpy
from typing import List, Callable, Optional

class BatchProcessor:
    def __init__(self):
        self.report = []
    
    def filter_by_type(self, obj_type: str) -> List[bpy.types.Object]:
        return [obj for obj in bpy.context.scene.objects if obj.type == obj_type]
    
    def filter_by_name(self, name_pattern: str) -> List[bpy.types.Object]:
        import re
        pattern = re.compile(name_pattern)
        return [obj for obj in bpy.context.scene.objects if pattern.match(obj.name)]
    
    def filter_selected(self) -> List[bpy.types.Object]:
        return bpy.context.selected_objects
    
    def filter_visible(self) -> List[bpy.types.Object]:
        return [obj for obj in bpy.context.scene.objects if obj.visible_get()]
    
    def process(self, objects: List[bpy.types.Object], processor: Callable, report: bool = True) -> None:
        for i, obj in enumerate(objects):
            try:
                processor(obj, i)
                if report:
                    self.report.append(f"处理 {obj.name}: 成功")
            except Exception as e:
                if report:
                    self.report.append(f"处理 {obj.name}: 失败 - {e}")
    
    def get_report(self) -> str:
        return "\n".join(self.report)

def scale_objects(factor: float):
    """缩放所有选中对象"""
    processor = BatchProcessor()
    selected = processor.filter_selected()
    
    for obj in selected:
        obj.scale = obj.scale * factor
    
    print(f"已缩放 {len(selected)} 个对象")

def rename_by_type():
    """按类型重命名对象"""
    processor = BatchProcessor()
    
    for obj_type in ['MESH', 'LIGHT', 'CAMERA']:
        objects = processor.filter_by_type(obj_type)
        for i, obj in enumerate(objects):
            obj.name = f"{obj_type}_{i+1:03d}"
    
    print(processor.get_report())

if __name__ == "__main__":
    rename_by_type()
```

### 动画脚本

```python
"""
动画脚本模板

用于创建和修改关键帧动画，
支持多种动画类型和时间控制。

功能：
- 创建关键帧
- 修改动画曲线
- 批量动画操作
- 导出动画数据
"""

import bpy
import math

class AnimationController:
    def __init__(self, obj: bpy.types.Object):
        self.obj = obj
        self.action = None
        self.get_action()
    
    def get_action(self):
        if not self.obj.animation_data:
            self.obj.animation_data_create()
        self.action = self.obj.animation_data.action
        if not self.action:
            self.action = bpy.data.actions.new(name=f"{self.obj.name}Action")
            self.obj.animation_data.action = self.action
    
    def insert_keyframe(self, data_path: str, frame: int, value: float):
        self.obj.keyframe_insert(data_path=data_path, frame=frame)
    
    def set_location_keyframes(self, start_frame: int, end_frame: int, 
                                start_pos, end_pos, interpolation='BEZIER'):
        for frame in range(start_frame, end_frame + 1):
            t = (frame - start_frame) / (end_frame - start_frame)
            x = start_pos[0] + (end_pos[0] - start_pos[0]) * t
            y = start_pos[1] + (end_pos[1] - start_pos[1]) * t
            z = start_pos[2] + (end_pos[2] - start_pos[2]) * t
            
            self.obj.location = (x, y, z)
            self.insert_keyframe('location', frame, None)
        
        self.set_interpolation(interpolation)
    
    def set_interpolation(self, interpolation: str, data_path: str = None):
        fcurves = self.action.fcurves
        for fcurve in fcurves:
            if data_path and fcurve.data_path != data_path:
                continue
            for keyframe in fcurve.keyframe_points:
                keyframe.interpolation = interpolation
    
    def add_noise_modifier(self, data_path: str, noise_scale: float = 1.0):
        fcurve = self.action.fcurves.find(data_path)
        if fcurve:
            mod = fcurve.modifiers.new('NOISE')
            mod.scale = noise_scale
    
    def clear_action(self):
        if self.action:
            bpy.data.actions.remove(self.action)
            self.get_action()

def create_bounce_animation(obj, bounce_height: float = 2.0, duration: int = 60):
    """创建弹跳动画"""
    controller = AnimationController(obj)
    
    # 起始位置
    obj.location = (0, 0, 0)
    controller.insert_keyframe('location', 1)
    
    # 弹跳最高点
    obj.location = (0, 0, bounce_height)
    controller.insert_keyframe('location', duration // 2)
    
    # 回到地面
    obj.location = (0, 0, 0)
    controller.insert_keyframe('location', duration)
    
    # 设置弹性插值
    controller.set_interpolation('ELASTIC')
    
    return controller

if __name__ == "__main__":
    obj = bpy.context.active_object
    if obj:
        create_bounce_animation(obj)
```

## 运算符模板

### 基础运算符

```python
"""
基础运算符模板

用于创建自定义Blender运算符，
继承bpy.types.Operator类。

运算符类型：
- OBJECT_OT_xxx - 对象操作
- MESH_OT_xxx - 网格操作
- SCENE_OT_xxx - 场景操作
"""

import bpy

class OBJECT_OT_simple_operator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "简单运算符"
    bl_description = "这是一个简单的自定义运算符"
    bl_options = {'REGISTER', 'UNDO'}
    
    # 属性定义
    my_int: bpy.props.IntProperty(
        name="整数",
        description="整数参数",
        default=10,
        min=0,
        max=100
    )
    
    my_float: bpy.props.FloatProperty(
        name="浮点数",
        description="浮点数参数",
        default=1.0,
        min=0.0,
        max=10.0
    )
    
    my_bool: bpy.props.BoolProperty(
        name="布尔值",
        description="布尔参数",
        default=True
    )
    
    my_enum: bpy.props.EnumProperty(
        name="枚举",
        description="选择选项",
        items=[
            ('OPTION_A', '选项A', '第一个选项'),
            ('OPTION_B', '选项B', '第二个选项'),
            ('OPTION_C', '选项C', '第三个选项')
        ],
        default='OPTION_A'
    )
    
    my_string: bpy.props.StringProperty(
        name="字符串",
        description="字符串参数",
        default="默认文本",
        maxlen=100
    )
    
    def execute(self, context):
        # 获取活动对象
        obj = context.active_object
        if not obj:
            self.report({'ERROR'}, "没有活动对象")
            return {'CANCELLED'}
        
        # 执行操作
        self.report({'INFO'}, f"执行: {self.my_string}, 值: {self.my_int}")
        
        return {'FINISHED'}
    
    def invoke(self, context, event):
        # 可以在这里处理鼠标事件
        return self.execute(context)

class OBJECT_OT_delete_dangling(bpy.types.Operator):
    bl_idname = "object.delete_dangling"
    bl_label = "删除悬垂点"
    bl_description = "删除没有连接到任何面的顶点"
    bl_options = {'REGISTER', 'UNDO'}
    
    @classmethod
    def poll(cls, context):
        obj = context.active_object
        return obj and obj.type == 'MESH' and obj.mode == 'EDIT'
    
    def execute(self, context):
        obj = context.active_object
        bpy.ops.mesh.select_all(action='DESELECT')
        bpy.ops.mesh.select_dangling()
        bpy.ops.mesh.delete(type='VERT')
        
        self.report({'INFO'}, "已删除悬垂点")
        return {'FINISHED'}
```

### 高级运算符

```python
"""
高级运算符模板

包含多个属性、分组调用和高级功能。

功能：
- 多属性处理
- 分组操作
- 条件检查
- 错误处理
"""

import bpy
from mathutils import Vector

class OBJECT_OT_batch_transform(bpy.types.Operator):
    bl_idname = "object.batch_transform"
    bl_label = "批量变换"
    bl_description = "批量移动、旋转和缩放选中的对象"
    bl_options = {'REGISTER', 'UNDO'}
    
    # 移动属性
    move_x: bpy.props.FloatProperty(
        name="移动X",
        default=0.0,
        step=0.1
    )
    
    move_y: bpy.props.FloatProperty(
        name="移动Y",
        default=0.0,
        step=0.1
    )
    
    move_z: bpy.props.FloatProperty(
        name="移动Z",
        default=0.0,
        step=0.1
    )
    
    # 旋转属性
    rotate_x: bpy.props.FloatProperty(
        name="旋转X",
        unit='ROTATION',
        default=0.0
    )
    
    rotate_y: bpy.props.FloatProperty(
        name="旋转Y",
        unit='ROTATION',
        default=0.0
    )
    
    rotate_z: bpy.props.FloatProperty(
        name="旋转Z",
        unit='ROTATION',
        default=0.0
    )
    
    # 缩放属性
    scale_x: bpy.props.FloatProperty(
        name="缩放X",
        default=1.0,
        min=0.01,
        max=100.0
    )
    
    scale_y: bpy.props.FloatProperty(
        name="缩放Y",
        default=1.0,
        min=0.01,
        max=100.0
    )
    
    scale_z: bpy.props.FloatProperty(
        name="缩放Z",
        default=1.0,
        min=0.01,
        max=100.0
    )
    
    use_local_space: bpy.props.BoolProperty(
        name="本地空间",
        default=False
    )
    
    use_center_origin: bpy.props.BoolProperty(
        name="以原点为中心",
        default=False
    )
    
    def execute(self, context):
        objects = context.selected_objects
        if not objects:
            self.report({'WARNING'}, "没有选中的对象")
            return {'CANCELLED'}
        
        count = 0
        for obj in objects:
            if self.use_center_origin:
                old_loc = obj.location.copy()
                obj.location = (0, 0, 0)
                obj.location = old_loc
            
            if not self.use_local_space:
                obj.location.x += self.move_x
                obj.location.y += self.move_y
                obj.location.z += self.move_z
                
                obj.rotation_euler.x += self.rotate_x
                obj.rotation_euler.y += self.rotate_y
                obj.rotation_euler.z += self.rotate_z
                
                obj.scale.x *= self.scale_x
                obj.scale.y *= self.scale_y
                obj.scale.z *= self.scale_z
            else:
                # 本地空间变换
                move_vec = Vector((self.move_x, self.move_y, self.move_z))
                move_vec.rotate(obj.rotation_euler)
                obj.location += move_vec
                
                obj.rotation_euler.rotate_axis('X', self.rotate_x)
                obj.rotation_euler.rotate_axis('Y', self.rotate_y)
                obj.rotation_euler.rotate_axis('Z', self.rotate_z)
            
            count += 1
        
        self.report({'INFO'}, f"已变换 {count} 个对象")
        return {'FINISHED'}
    
    def invoke(self, context, event):
        wm = context.window_manager
        return wm.invoke_props_dialog(self)
```

### 交互运算符

```python
"""
交互运算符模板

支持鼠标交互的运算符，
包括点击、拖拽等操作。

类型：
- 确认操作
- 弹窗选择
- 坐标输入
- 鼠标回调
"""

import bpy
from bpy.props import FloatVectorProperty

class OBJECT_OT_place_cursor(bpy.types.Operator):
    bl_idname = "object.place_cursor"
    bl_label = "放置光标"
    bl_description = "点击场景放置3D光标"
    bl_options = {'REGISTER', 'UNDO'}
    
    location: FloatVectorProperty(
        name="位置",
        subtype='XYZ',
        unit='LENGTH'
    )
    
    @classmethod
    def poll(cls, context):
        return context.area and context.area.type == 'VIEW_3D'
    
    def execute(self, context):
        context.scene.cursor.location = self.location
        self.report({'INFO'}, f"光标位置: {self.location}")
        return {'FINISHED'}
    
    def invoke(self, context, event):
        if event.shift:
            return self.execute(context)
        
        bpy.ops.view3d.cursor3d('INVOKE_DEFAULT')
        return {'FINISHED'}

class OBJECT_OT_transform_gizmo(bpy.types.Operator):
    bl_idname = "object.transform_gizmo"
    bl_label = "变换操作器"
    bl_description = "使用操作器进行交互式变换"
    bl_options = {'REGISTER', 'UNDO'}
    
    transform_type: bpy.props.EnumProperty(
        name="变换类型",
        items=[
            ('MOVE', '移动', '移动对象'),
            ('ROTATE', '旋转', '旋转对象'),
            ('SCALE', '缩放', '缩放对象')
        ],
        default='MOVE'
    )
    
    def execute(self, context):
        if self.transform_type == 'MOVE':
            bpy.ops.transform.translate('INVOKE_DEFAULT')
        elif self.transform_type == 'ROTATE':
            bpy.ops.transform.rotate('INVOKE_DEFAULT')
        else:
            bpy.ops.transform.resize('INVOKE_DEFAULT')
        return {'FINISHED'}

class OBJECT_OT_quick_pie(bpy.types.Operator):
    bl_idname = "object.quick_pie"
    bl_label = "快捷菜单"
    bl_description = "打开快捷操作菜单"
    bl_options = {'REGISTER'}
    
    def execute(self, context):
        bpy.ops.wm.call_menu_pie(name="VIEW3D_MT_object_quick_menu")
        return {'FINISHED'}
```

## 面板模板

### 基础面板

```python
"""
基础面板模板

用于创建自定义UI面板，
继承bpy.types.Panel类。

面板类型：
- OBJECT_PT_xxx - 对象面板
- DATA_PT_xxx - 数据面板
- RENDER_PT_xxx - 渲染面板
"""

import bpy

class OBJECT_PT_my_panel(bpy.types.Panel):
    bl_label = "我的面板"
    bl_idname = "OBJECT_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '工具'
    bl_order = 0
    bl_options = {'DEFAULT_CLOSED'}
    
    @classmethod
    def poll(cls, context):
        return context.object is not None
    
    def draw_header(self, context):
        layout = self.layout
        layout.label(text="面板标题")
    
    def draw(self, context):
        layout = self.layout
        obj = context.object
        
        # 简单列
        col = layout.column()
        col.label(text="基本信息")
        col.prop(obj, "name")
        
        # 分隔线
        layout.separator()
        
        # 属性列
        split = layout.split(factor=0.3)
        split.label(text="位置:")
        split.prop(obj, "location", text="")
        
        split = layout.split(factor=0.3)
        split.label(text="旋转:")
        split.prop(obj, "rotation_euler", text="")
        
        split = layout.split(factor=0.3)
        split.label(text="缩放:")
        split.prop(obj, "scale", text="")
        
        # 操作符按钮
        layout.separator()
        row = layout.row()
        row.operator("object.duplicate", icon='DUPLICATE')
        row.operator("object.delete", icon='X')
```

### 高级面板

```python
"""
高级面板模板

包含折叠、选项卡、列表等复杂UI元素。

功能：
- 嵌套面板
- 属性列
- 操作符行
- 实时更新
"""

import bpy

class ObjectSettingsPanel:
    @staticmethod
    def draw_transform_panel(layout, obj):
        """绘制变换面板"""
        box = layout.box()
        box.label(text="变换控制")
        
        col = box.column(align=True)
        col.prop(obj, "location", text="位置")
        col.prop(obj, "rotation_euler", text="旋转")
        col.prop(obj, "scale", text="缩放")
        
        # 快捷操作
        row = box.row(align=True)
        row.operator("object.location_clear", text="清除位置")
        row.operator("object.rotation_clear", text="清除旋转")
        row.operator("object.scale_clear", text="清除缩放")
    
    @staticmethod
    def draw_visibility_panel(layout, obj):
        """绘制可见性面板"""
        box = layout.box()
        box.label(text="可见性")
        
        row = box.row(align=True)
        row.prop(obj, "hide_viewport", text="隐藏", toggle=1)
        row.prop(obj, "hide_render", text="渲染隐藏", toggle=1)
        row.prop(obj, "display_type", text="")
        
        row = box.row(align=True)
        row.prop(obj, "hide_select", text="不可选", toggle=1)
        row.prop(obj, "show_instancer", text="实例", toggle=1)

class OBJECT_PT_advanced_panel(bpy.types.Panel):
    bl_label = "高级面板"
    bl_idname = "OBJECT_PT_advanced_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '高级工具'
    
    def draw(self, context):
        layout = self.layout
        obj = context.object
        
        if not obj:
            layout.label(text="没有活动对象")
            return
        
        # 使用标签页
        tab = layout.column_heading()
        tab.prop(obj, "name", text="对象名称")
        
        # 折叠部分
        col = layout.column(align=True)
        col.use_property_split = True
        col.use_property_decorate = False
        
        # 变换
        ObjectSettingsPanel.draw_transform_panel(layout, obj)
        
        # 可见性
        ObjectSettingsPanel.draw_visibility_panel(layout, obj)
        
        # 自定义属性
        if obj.keys():
            box = layout.box()
            box.label(text="自定义属性")
            for key in obj.keys():
                box.prop(obj, f'["{key}"]', text=key)

class DATA_PT_custom_props(bpy.types.Panel):
    bl_label = "自定义属性"
    bl_idname = "DATA_PT_custom_props"
    bl_space_type = 'PROPERTIES'
    bl_region_type = 'WINDOW'
    bl_context = 'data'
    
    @classmethod
    def poll(cls, context):
        return context.material is not None
    
    def draw(self, context):
        layout = self.layout
        mat = context.material
        
        if not mat.keys():
            layout.label(text="没有自定义属性")
            return
        
        col = layout.column()
        for key in mat.keys():
            value = mat.get(key)
            if isinstance(value, (int, float)):
                col.prop(mat, f'["{key}"]', text=key)
            elif isinstance(value, str):
                col.prop(mat, f'["{key}"]', text=key)
```

## 属性模板

### 属性组

```python
"""
属性组模板

用于创建自定义属性集合，
继承bpy.types.PropertyGroup类。

用途：
- 存储复合数据
- UI面板属性
- 场景设置
"""

import bpy
from bpy.props import PointerProperty, CollectionProperty

class MyObjectSettings(bpy.types.PropertyGroup):
    enabled: bpy.props.BoolProperty(
        name="启用",
        default=True
    )
    
    name: bpy.props.StringProperty(
        name="名称",
        default="设置"
    )
    
    count: bpy.props.IntProperty(
        name="数量",
        default=10,
        min=0,
        max=100
    )
    
    factor: bpy.props.FloatProperty(
        name="因子",
        default=1.0,
        min=0.0,
        max=10.0
    )
    
    color: bpy.props.FloatVectorProperty(
        name="颜色",
        subtype='COLOR',
        size=4,
        min=0.0,
        max=1.0,
        default=(1.0, 1.0, 1.0, 1.0)
    )
    
    object_ref: bpy.props.PointerProperty(
        name="对象引用",
        type=bpy.types.Object
    )
    
    enum_choice: bpy.props.EnumProperty(
        name="选项",
        items=[
            ('A', '选项A', '第一个选项'),
            ('B', '选项B', '第二个选项'),
            ('C', '选项C', '第三个选项')
        ],
        default='A'
    )

class MyCollectionItem(bpy.types.PropertyGroup):
    name: bpy.props.StringProperty(
        name="名称",
        default="项目"
    )
    
    value: bpy.props.FloatProperty(
        name="值",
        default=0.0
    )
    
    enabled: bpy.props.BoolProperty(
        name="启用",
        default=True
    )

class MySceneSettings(bpy.types.PropertyGroup):
    object_settings: PointerProperty(
        type=MyObjectSettings,
        name="对象设置"
    )
    
    collection: CollectionProperty(
        type=MyCollectionItem,
        name="集合"
    )
    
    def add_item(self, name, value):
        item = self.collection.add()
        item.name = name
        item.value = value
        return item
    
    def clear_collection(self):
        self.collection.clear()
```

### 注册属性

```python
"""
属性注册模板

用于在Blender中注册自定义属性。

注册位置：
- Scene - 场景属性
- Object - 对象属性
- Material - 材质属性
- Mesh - 网格属性
"""

def register_object_properties():
    """注册对象属性"""
    bpy.types.Object.my_settings = PointerProperty(
        type=MyObjectSettings,
        name="我的设置"
    )
    
    bpy.types.Object.my_collection = CollectionProperty(
        type=MyCollectionItem,
        name="我的集合"
    )

def unregister_object_properties():
    """注销对象属性"""
    del bpy.types.Object.my_settings
    del bpy.types.Object.my_collection

def register_scene_properties():
    """注册场景属性"""
    bpy.types.Scene.my_settings = PointerProperty(
        type=MySceneSettings,
        name="场景设置"
    )

def unregister_scene_properties():
    """注销场景属性"""
    del bpy.types.Scene.my_settings

def register_material_properties():
    """注册材质属性"""
    bpy.types.Material.my_material_props = PointerProperty(
        type=MyObjectSettings,
        name="材质属性"
    )

def unregister_material_properties():
    """注销材质属性"""
    del bpy.types.Material.my_material_props

# 在主注册函数中调用
def register():
    register_object_properties()
    register_scene_properties()
    register_material_properties()

def unregister():
    unregister_material_properties()
    unregister_scene_properties()
    unregister_object_properties()
```

## 完整插件模板

### 插件结构

```python
"""
完整插件模板

包含所有必要组件的标准插件结构。

结构：
- 1. 文档字符串
- 2. 导入语句
- 3. 常量定义
- 4. 类定义
- 5. 注册函数
- 6. 注销函数
- 7. 主入口

bl_info格式：
{
    "name": "插件名称",
    "author": "作者",
    "version": (1, 0, 0),
    "blender": (3, 6, 0),
    "location": "位置说明",
    "description": "插件描述",
    "warning": "警告信息",
    "doc_url": "文档URL",
    "category": "分类"
}
"""

bl_info = {
    "name": "我的Blender插件",
    "author": "作者姓名",
    "version": (1, 0, 0),
    "blender": (4, 0, 0),
    "location": "3D视图 > 工具",
    "description": "这是一个示例插件，演示了完整的插件结构",
    "warning": "",
    "doc_url": "",
    "category": "开发工具"
}

import bpy
import os
from . import operators
from . import panels
from . import properties

class AddonPreferences(bpy.types.AddonPreferences):
    bl_idname = __name__
    
    preference_path: bpy.props.StringProperty(
        name="设置路径",
        subtype='FILE_PATH',
        default=""
    )
    
    show_advanced: bpy.props.BoolProperty(
        name="显示高级选项",
        default=False
    )
    
    def draw(self, context):
        layout = self.layout
        layout.prop(self, "preference_path")
        layout.prop(self, "show_advanced")

def register():
    """注册插件"""
    # 注册属性
    properties.register()
    
    # 注册运算符
    operators.register()
    
    # 注册面板
    panels.register()
    
    # 注册插件偏好设置
    bpy.utils.register_class(AddonPreferences)
    
    print(f"插件已注册: {bl_info['name']} v{'.'.join(map(str, bl_info['version']))}")

def unregister():
    """注销插件"""
    # 注销插件偏好设置
    bpy.utils.unregister_class(AddonPreferences)
    
    # 注销面板
    panels.unregister()
    
    # 注销运算符
    operators.unregister()
    
    # 注销属性
    properties.unregister()
    
    print(f"插件已注销: {bl_info['name']}")

if __name__ == "__main__":
    register()
```

### 多文件插件结构

```python
"""
多文件插件 __init__.py

演示如何组织多文件插件项目。

目录结构：
plugin_name/
    __init__.py
    operators/
        __init__.py
        object_ops.py
        mesh_ops.py
    panels/
        __init__.py
        main_panel.py
        settings_panel.py
    properties/
        __init__.py
        custom_props.py
    utils/
        __init__.py
        helpers.py
"""

# __init__.py
import bpy
from . import operators
from . import panels
from . import properties
from . import utils

classes = []

def register():
    properties.register()
    operators.register()
    panels.register()
    utils.register()

def unregister():
    utils.unregister()
    panels.unregister()
    operators.unregister()
    properties.unregister()

# operators/__init__.py
from .object_ops import OBJECT_OT_my_operator
from .mesh_ops import MESH_OT_my_mesh_op

classes = [
    OBJECT_OT_my_operator,
    MESH_OT_my_mesh_op,
]

def register():
    for cls in classes:
        bpy.utils.register_class(cls)

def unregister():
    for cls in classes:
        bpy.utils.unregister_class(cls)

# panels/__init__.py
from .main_panel import OBJECT_PT_main_panel
from .settings_panel import OBJECT_PT_settings_panel

classes = [
    OBJECT_PT_main_panel,
    OBJECT_PT_settings_panel,
]

def register():
    for cls in classes:
        bpy.utils.register_class(cls)

def unregister():
    for cls in classes:
        bpy.utils.unregister_class(cls)
```

## 最佳实践

### 代码组织

```python
"""
代码组织最佳实践

遵循PEP 8和Blender代码规范，
提高代码可读性和可维护性。

规范要点：
- 命名规范
- 导入顺序
- 代码注释
- 函数长度
- 类设计
"""

# 命名规范
# 类名: PascalCase
# 函数/变量: snake_case
# 常量: UPPER_SNAKE_CASE
# 私有成员: _前缀

# 导入顺序
# 1. Python标准库
import os
import sys
import math
from typing import List, Dict, Optional

# 2. Blender模块
import bpy
import bpy.utils
import bpy.props
from bpy.props import FloatProperty, IntProperty, StringProperty

# 3. 第三方库
# import numpy as np

# 4. 本地模块
from . import utils
from .operators import BaseOperator

# 类设计原则
# - 单一职责
# - 避免过深继承
# - 使用组合
# - 保持简短

class BaseObjectOperator(bpy.types.Operator):
    """基类运算符，提供通用功能"""
    
    bl_options = {'REGISTER', 'UNDO'}
    
    @classmethod
    def poll(cls, context):
        return context.active_object is not None
    
    def execute(self, context):
        return {'FINISHED'}
    
    def report_error(self, message):
        self.report({'ERROR'}, message)

class ObjectModifier(BaseObjectOperator):
    """对象修饰符基类"""
    
    modifier_name: StringProperty(name="名称")
    
    def apply_modifier(self, obj):
        raise NotImplementedError()
    
    def execute(self, context):
        obj = context.active_object
        if not obj:
            return {'CANCELLED'}
        
        try:
            self.apply_modifier(obj)
            return {'FINISHED'}
        except Exception as e:
            self.report_error(str(e))
            return {'CANCELLED'}
```

### 错误处理

```python
"""
错误处理最佳实践

提供健壮的错误处理机制，
避免插件崩溃。

策略：
- 验证输入
- 使用try-except
- 提供有用错误信息
- 记录错误日志
"""

import bpy
import traceback
import logging

class PluginLogger:
    def __init__(self, name):
        self.logger = logging.getLogger(name)
        self.logger.setLevel(logging.DEBUG)
        
        handler = logging.StreamHandler()
        handler.setLevel(logging.DEBUG)
        formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
        handler.setFormatter(formatter)
        self.logger.addHandler(handler)
    
    def debug(self, message):
        self.logger.debug(message)
    
    def info(self, message):
        self.logger.info(message)
    
    def warning(self, message):
        self.logger.warning(message)
    
    def error(self, message):
        self.logger.error(message)
    
    def exception(self, message):
        self.logger.exception(message)

logger = PluginLogger(__name__)

class SafeOperator(bpy.types.Operator):
    """安全运算符模板"""
    
    @classmethod
    def poll(cls, context):
        if not context.active_object:
            return False
        if context.active_object.type != 'MESH':
            return False
        return True
    
    def execute(self, context):
        try:
            result = self.safe_execute(context)
            return result
        except ValueError as e:
            logger.error(f"值错误: {e}")
            self.report({'ERROR'}, str(e))
            return {'CANCELLED'}
        except Exception as e:
            logger.exception("未预期错误")
            self.report({'ERROR'}, f"错误: {e}")
            return {'CANCELLED'}
    
    def safe_execute(self, context):
        raise NotImplementedError()
```

### 性能优化

```python
"""
性能优化最佳实践

提高插件执行效率，
减少资源占用。

优化策略：
- 减少Blender API调用
- 使用批量操作
- 避免重复计算
- 延迟处理
"""

import bpy

class BatchOperator:
    """批量操作优化"""
    
    def __init__(self):
        self.selection_cache = None
    
    def save_selection(self):
        self.selection_cache = bpy.context.selected_objects.copy()
        bpy.ops.object.select_all(action='DESELECT')
    
    def restore_selection(self):
        if self.selection_cache:
            for obj in self.selection_cache:
                obj.select_set(True)
    
    def batch_apply(self, objects, operation):
        self.save_selection()
        
        for obj in objects:
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            operation(obj)
        
        self.restore_selection()

class CachedProperty:
    """缓存属性装饰器"""
    
    def __init__(self, func):
        self.func = func
        self.cache = {}
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
        if instance not in self.cache:
            self.cache[instance] = self.func(instance)
        return self.cache[instance]

class MeshAnalyzer:
    """网格分析器优化"""
    
    @CachedProperty
    def vertex_count(self):
        obj = bpy.context.active_object
        if obj and obj.type == 'MESH':
            return len(obj.data.vertices)
        return 0
    
    def batch_analyze(self, objects):
        results = {}
        for obj in objects:
            if obj.type == 'MESH':
                results[obj.name] = {
                    'vertices': len(obj.data.vertices),
                    'edges': len(obj.data.edges),
                    'faces': len(obj.data.polygons)
                }
        return results
```


---
## bpy_blender_as_module

# Blender as Python Module - 作为 Python 模块运行

## 概述

Blender 支持作为 Python 模块运行，允许将 `import bpy` 添加到任何 Python 脚本中，从而访问 Blender 的全部功能。这使得开发者可以在不启动 Blender 图形界面的情况下使用 Blender 的渲染引擎、图像处理工具、3D 建模功能等。

## 安装方式

### 通过 PIP 安装（推荐）

```bash
# 安装预编译的 bpy 模块
pip install bpy
```

### 自行编译

如果需要特定版本或自定义构建，可以从源码编译：

```bash
# 克隆 Blender 源码
git clone https://developer.blender.org/source/blender.git
cd blender

# 按照官方构建指南编译 Python 模块
# 参见: https://developer.blender.org/docs/handbook/building_blender/python_module/
```

## 使用场景

### 数据可视化与渲染

```python
import bpy
import math

# 创建场景
bpy.ops.wm.read_factory_settings(use_empty=True)

# 创建相机
bpy.ops.object.camera_add(location=(0, -10, 5))
camera = bpy.context.active_object
camera.rotation_euler = (math.radians(80), 0, 0)
bpy.context.scene.camera = camera

# 创建光源
bpy.ops.object.light_add(type='SUN', location=(5, -5, 10))
light = bpy.context.active_object
light.data.energy = 3.0

# 创建 3D 数据可视化
import numpy as np

# 生成螺旋数据
t = np.linspace(0, 4 * np.pi, 100)
x = np.cos(t) * t
y = np.sin(t) * t
z = np.linspace(0, 5, 100)

# 创建点云
mesh = bpy.data.meshes.new("SpiralMesh")
obj = bpy.data.objects.new("Spiral", mesh)
bpy.context.collection.objects.link(obj)

# 添加顶点
verts = [(xi, yi, zi) for xi, yi, zi in zip(x, y, z)]
mesh.from_pydata(verts, [], [])
mesh.update()

# 渲染
bpy.context.scene.render.engine = 'CYCLES'
bpy.context.scene.cycles.samples = 128
bpy.context.scene.render.filepath = "/tmp/spiral_render.png"
bpy.ops.render.render(write_still=True)
```

### 图像处理

```python
import bpy

# 加载图像
bpy.ops.wm.read_factory_settings(use_empty=True)

# 导入图像
bpy.ops.image.open(filepath="/path/to/input.png")

# 获取图像
img = bpy.data.images[0]

# 创建合成节点
bpy.context.scene.use_nodes = True
tree = bpy.context.scene.node_tree

# 清除默认节点
for node in tree.nodes:
    tree.nodes.remove(node)

# 创建节点链
render_layers = tree.nodes.new(type='CompositorNodeRLayers')
blur = tree.nodes.new(type='CompositorNodeBlur')
output = tree.nodes.new(type='CompositorNodeComposite')

# 连接节点
tree.links.new(render_layers.outputs[0], blur.inputs[0])
tree.links.new(blur.outputs[0], output.inputs[0])

# 设置模糊参数
blur.size_x = 10
blur.size_y = 10

# 渲染
bpy.ops.render.render()
```

### 3D 文件转换

```python
import bpy
import os

def convert_obj_to_fbx(input_path, output_path):
    """将 OBJ 文件转换为 FBX 格式"""
    bpy.ops.wm.read_factory_settings(use_empty=True)
    
    # 导入 OBJ
    bpy.ops.import_scene.obj(filepath=input_path)
    
    # 获取导入的对象
    objects = bpy.context.selected_objects
    
    # 导出 FBX
    bpy.ops.export_scene.fbx(
        filepath=output_path,
        use_selection=True,
        axis_forward='-Z',
        axis_up='Y'
    )

def batch_convert(directory):
    """批量转换目录中的文件"""
    for filename in os.listdir(directory):
        if filename.endswith('.obj'):
            input_path = os.path.join(directory, filename)
            output_path = os.path.join(directory, filename.replace('.obj', '.fbx'))
            convert_obj_to_fbx(input_path, output_path)
```

### 自动化渲染管线

```python
import bpy
import os
from pathlib import Path

class RenderPipeline:
    def __init__(self, output_dir):
        self.output_dir = Path(output_dir)
        self.output_dir.mkdir(parents=True, exist_ok=True)
        self.setup_scene()
    
    def setup_scene(self):
        """设置基础场景"""
        bpy.ops.wm.read_factory_settings(use_empty=True)
        
        # 设置渲染引擎
        bpy.context.scene.render.engine = 'CYCLES'
        bpy.context.scene.cycles.samples = 64
        
        # 设置输出格式
        bpy.context.scene.render.image_settings.file_format = 'PNG'
        bpy.context.scene.render.resolution_x = 1920
        bpy.context.scene.render.resolution_y = 1080
    
    def render_object(self, obj_name, material_name=None):
        """渲染单个对象"""
        # 创建基本几何体
        bpy.ops.mesh.primitive_cube_add(size=2, location=(0, 0, 0))
        obj = bpy.context.active_object
        obj.name = obj_name
        
        if material_name:
            mat = bpy.data.materials.new(name=material_name)
            mat.use_nodes = True
            obj.data.materials.append(mat)
        
        # 设置相机
        bpy.ops.object.camera_add(location=(5, -5, 5))
        camera = bpy.context.active_object
        camera.rotation_euler = (math.radians(60), 0, math.radians(45))
        bpy.context.scene.camera = camera
        
        # 渲染
        output_path = self.output_dir / f"{obj_name}.png"
        bpy.context.scene.render.filepath = str(output_path)
        bpy.ops.render.render(write_still=True)
        
        # 清理
        bpy.data.objects.remove(obj, do_unlink=True)
    
    def render_animation(self, obj_name, start_frame=1, end_frame=100):
        """渲染动画"""
        # 创建对象
        bpy.ops.mesh.primitive_uv_sphere_add(radius=1, location=(0, 0, 0))
        obj = bpy.context.active_object
        obj.name = obj_name
        
        # 动画关键帧
        obj.location = (3, 0, 0)
        obj.keyframe_insert(data_path="location", frame=start_frame)
        obj.location = (-3, 0, 0)
        obj.keyframe_insert(data_path="location", frame=end_frame)
        
        # 设置动画渲染
        bpy.context.scene.frame_start = start_frame
        bpy.context.scene.frame_end = end_frame
        bpy.context.scene.render.filepath = str(self.output_dir / f"{obj_name}_frame_")
        bpy.context.scene.render.image_settings.file_format = 'PNG'
        bpy.context.scene.render.image_settings.use_sequence = True
        
        bpy.ops.render.render(animation=True)

# 使用示例
pipeline = RenderPipeline("/tmp/render_output")
pipeline.render_object("TestCube", "BlueMaterial")
```

### Python IDE 集成与调试

```python
import bpy

# 设置 Blender 可执行文件路径
import shutil
blender_bin = shutil.which("blender")
if blender_bin:
    bpy.app.binary_path = blender_bin
    print(f"Found Blender at: {blender_bin}")
else:
    print("Unable to find blender!")

# 在 IDE 中调试
def debug_scene():
    """调试当前场景状态"""
    print("=== Scene Debug Info ===")
    print(f"Objects: {len(bpy.data.objects)}")
    print(f"Meshes: {len(bpy.data.meshes)}")
    print(f"Materials: {len(bpy.data.materials)}")
    print(f"Cameras: {len(bpy.data.cameras)}")
    
    for obj in bpy.data.objects:
        print(f"  - {obj.name}: {obj.type}")
    
    print("=== End Debug Info ===")

debug_scene()
```

## 与普通 Blender 脚本的差异

### 命令行参数不可用

在模块模式下，通过命令行参数控制的设置不可用：

```python
# 这些命令行参数在模块模式下无效：
# blender --background
# blender --threads 4
# blender --log debug

# 需要通过 Python API 设置
bpy.context.scene.render.threads = 4
bpy.app.handlers.render_pre.append(log_handler)
```

### 信号处理器不初始化

Blender 的典型信号处理器不会初始化：

```python
# 没有特殊的 Control-C 处理
# 需要自行实现中断处理
import signal
import sys

def signal_handler(sig, frame):
    print("Received interrupt signal!")
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)
```

### 启动和偏好设置

```python
# 加载模块时包含默认启动场景
import bpy
# 现在有一个默认立方体、相机和灯光

# 如果要从空文件开始
bpy.ops.wm.read_factory_settings(use_empty=True)

# 用户的启动和偏好设置被忽略
# 表现得像传递了 --factory-startup 参数

# 可以手动加载用户偏好
bpy.ops.wm.read_userpref()
bpy.ops.wm.read_homefile()
```

### 模块导入顺序

```python
# 必须先导入 bpy，再导入其他 Blender 模块
import bpy
import mathutils   # 在 bpy 之后导入
import gpu         # 在 bpy 之后导入
import freestyle   # 在 bpy 之后导入
```

## 限制

### 不支持重载

```python
# 这样会报错
import importlib
importlib.reload(bpy)  # 异常！

# 应该使用运算符重置内部状态
bpy.ops.wm.read_factory_settings(use_empty=True)
```

### 单一 .blend 文件限制

只能同时编辑一个 .blend 文件：

```python
# 无法同时加载多个 .blend 文件
# 需要使用库 API 在文件之间传递数据
from bpy.types import BlendDataLibraries

# 加载库数据
with bpy.data.libraries.load("/path/to/library.blend") as (data_from, data_to):
    data_to.meshes = data_from.meshes
    data_to.materials = data_from.materials

# 使用临时数据上下文
bpy.data.temp_data(use=True)
# ... 执行操作
bpy.data.temp_data(use=False)
```

### 多进程方案

对于需要同时处理多个 .blend 文件的场景：

```python
import multiprocessing as mp

def process_blend_file(filepath, output_dir):
    import bpy
    bpy.ops.wm.read_factory_settings(use_empty=True)
    
    # 加载并处理文件
    bpy.ops.wm.open_mainfile(filepath=filepath)
    
    # 执行操作...
    
    # 保存结果
    output_path = os.path.join(output_dir, os.path.basename(filepath))
    bpy.ops.wm.save_mainfile(filepath=output_path)

if __name__ == "__main__":
    with mp.Pool(processes=4) as pool:
        pool.map(process_blend_file, blend_files)
```

## GPU 使用注意事项

```python
import bpy

# 其他 Python 模块可能以阻止 Blender/Cycles 访问 GPU 的方式使用 GPU
# 需要确保 GPU 资源被正确释放

# 在模块模式下使用 GPU
bpy.context.scene.cycles.device = 'GPU'
bpy.context.scene.cycles.gpu_use_features = True

# 检查 GPU 可用性
prefs = bpy.context.preferences.addons['cycles'].preferences
for device_type in prefs.get_device_types():
    for device in prefs.get_devices_for_type(device_type):
        print(f"{device.name}: {device.use}")
```


---
## bpy_drivers_expressions

# 驱动程序与表达式模块

## 驱动程序概述

驱动程序是Blender动画系统中强大的自动化工具，允许一个属性的值由其他属性通过数学表达式计算得出。通过Python表达式，可以创建复杂的动画关系，如跟随动画、程序化动画和参数化变形。驱动程序减少了手动关键帧的工作，使动画调整更加灵活和高效。

```python
# 驱动程序核心概念
# 驱动表达式 - 计算属性值的Python表达式
# 驱动变量 - 表达式中引用的其他属性
# 数据路径 - 属性在Python API中的位置
# 变换通道 - 位置、旋转、缩放等变换属性

# 驱动程序类型
# 简单驱动 - 单变量线性关系
# 复杂驱动 - 多变量数学表达式
# 条件驱动 - 包含条件判断的表达式
# 脚本驱动 - 完整的Python函数

# 常用场景
# 对象跟随 - 一个对象跟随另一个的位置
# 参数化变形 - 根据参数调整形状
# 动画关联 - 多个动画同步播放
# 动态调整 - 根据时间或其他因素调整属性
```

## 驱动程序基础

### 创建驱动程序

```python
import bpy
from mathutils import Vector

def create_simple_driver(target_obj, target_prop, driven_obj, driven_prop, expression="var"):
    """创建简单的单变量驱动程序"""
    # 获取驱动属性
    fcurve = driven_obj.driver_add(target_prop)
    
    # 清除默认变量
    fcurve.driver.variables.clear()
    
    # 添加驱动变量
    var = fcurve.driver.variables.new()
    var.name = "var"
    var.targets[0].id = target_obj
    
    # 根据属性类型设置数据路径
    if target_prop in ['location', 'rotation_euler', 'scale']:
        var.targets[0].data_path = f'{target_prop}[0]'
    else:
        var.targets[0].data_path = target_prop
    
    # 设置表达式
    fcurve.driver.expression = expression
    
    return fcurve

def create_distance_driver(driven_obj, driven_prop, target1_obj, target2_obj):
    """创建基于距离的驱动程序"""
    fcurve = driven_obj.driver_add(driven_prop)
    
    # 清除默认变量
    fcurve.driver.variables.clear()
    
    # 添加两个目标变量
    var1 = fcurve.driver.variables.new()
    var1.name = "loc1"
    var1.targets[0].id = target1_obj
    var1.targets[0].data_path = "location"
    
    var2 = fcurve.driver.variables.new()
    var2.name = "loc2"
    var2.targets[0].id = target2_obj
    var2.targets[0].data_path = "location"
    
    # 设置距离计算表达式
    fcurve.driver.expression = "(loc1[0]-loc2[0])**2 + (loc1[1]-loc2[1])**2 + (loc1[2]-loc2[2])**2"
    fcurve.driver.expression = "sqrt((loc1[0]-loc2[0])**2 + (loc1[1]-loc2[1])**2 + (loc1[2]-loc2[2])**2)"
    
    return fcurve
```

### 管理驱动程序

```python
def get_object_drivers(obj):
    """获取对象的所有驱动程序"""
    drivers = []
    if obj.animation_data and obj.animation_data.drivers:
        for fcurve in obj.animation_data.drivers:
            drivers.append({
                'data_path': fcurve.data_path,
                'expression': fcurve.driver.expression,
                'variables': [(v.name, v.targets[0].data_path) for v in fcurve.driver.variables]
            })
    return drivers

def remove_driver(obj, data_path):
    """移除指定属性的驱动程序"""
    if obj.animation_data:
        for fcurve in obj.animation_data.drivers:
            if fcurve.data_path == data_path:
                obj.driver_remove(data_path)
                return True
    return False

def remove_all_drivers(obj):
    """移除对象的所有驱动程序"""
    if obj.animation_data:
        for fcurve in list(obj.animation_data.drivers):
            obj.driver_remove(fcurve.data_path)

def copy_driver(source_obj, source_prop, target_obj, target_prop):
    """复制驱动程序到另一个属性"""
    # 获取源驱动程序
    source_fcurve = None
    if source_obj.animation_data:
        for fcurve in source_obj.animation_data.drivers:
            if fcurve.data_path == source_prop:
                source_fcurve = fcurve
                break
    
    if not source_fcurve:
        return None
    
    # 创建新的驱动程序
    target_fcurve = target_obj.driver_add(target_prop)
    
    # 复制表达式
    target_fcurve.driver.expression = source_fcurve.driver.expression
    
    # 复制变量
    for source_var in source_fcurve.driver.variables:
        target_var = target_fcurve.driver.variables.new()
        target_var.name = source_var.name
        target_var.targets[0].id = source_var.targets[0].id
        target_var.targets[0].data_path = source_var.targets[0].data_path
        target_var.targets[0].transform_type = source_var.targets[0].transform_type
        target_var.targets[0].transform_space = source_var.targets[0].transform_space
    
    return target_fcurve
```

### 驱动程序属性

```python
def get_driver_info(driver):
    """获取驱动程序详细信息"""
    info = {
        'expression': driver.expression,
        'is_valid': driver.is_valid,
        'variables': []
    }
    
    for var in driver.variables:
        var_info = {
            'name': var.name,
            'targets': []
        }
        
        for target in var.targets:
            target_info = {
                'id': target.id.name if target.id else None,
                'data_path': target.data_path,
                'transform_type': target.transform_type,
                'transform_space': target.transform_space
            }
            var_info['targets'].append(target_info)
        
        info['variables'].append(var_info)
    
    return info

def set_driver_mode(driver, mode='Averaged'):
    """设置驱动模式"""
    driver.mode = mode

def set_driver_extrapolation(driver, extrapolation='CONSTANT'):
    """设置外推模式"""
    for fcurve in driver.fcurves:
        fcurve.extrapolation = extrapolation

def set_driver_keyframe_points(driver, keyframes):
    """设置驱动程序关键帧"""
    for fcurve in driver.fcurves:
        for i, (frame, value) in enumerate(keyframes):
            if i < len(fcurve.keyframe_points):
                kp = fcurve.keyframe_points[i]
                kp.co = (frame, value)
            else:
                fcurve.keyframe_points.add((frame, value))
        fcurve.update()
```

## 驱动变量

### 变量类型

```python
def create_single_target_driver(obj, target_obj, target_prop, expression="var"):
    """创建单目标驱动程序"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "var"
    var.type = 'SINGLE_PROP'
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = expression
    
    return fcurve

def create_rotation_driver(obj, target_obj, expression="var"):
    """创建旋转驱动，使用变换通道"""
    fcurve = obj.driver_add('rotation_euler', 0)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "rot"
    var.type = 'TRANSFORMS'
    var.targets[0].id = target_obj
    var.targets[0].transform_type = 'ROT_X'
    var.targets[0].transform_space = 'LOCAL_SPACE'
    
    fcurve.driver.expression = expression
    
    return fcurve

def create_distance_between_objects(obj, obj1, obj2, expression="distance"):
    """创建对象间距离驱动"""
    fcurve = obj.driver_add('scale', 0)
    fcurve.driver.variables.clear()
    
    var1 = fcurve.driver.variables.new()
    var1.name = "p1"
    var1.type = 'TRANSFORMS'
    var1.targets[0].id = obj1
    var1.targets[0].transform_type = 'LOC_X'
    var1.targets[0].transform_space = 'WORLD_SPACE'
    
    var2 = fcurve.driver.variables.new()
    var2.name = "p2"
    var2.type = 'TRANSFORMS'
    var2.targets[0].id = obj2
    var2.targets[0].transform_type = 'LOC_X'
    var2.targets[0].transform_space = 'WORLD_SPACE'
    
    fcurve.driver.expression = expression
    
    return fcurve
```

### 变量引用

```python
def create_chained_drivers(main_driver_obj, driven_objs):
    """创建链式驱动，一个对象驱动多个对象"""
    for i, driven_obj in enumerate(driven_objs):
        fcurve = driven_obj.driver_add('location', 0)
        fcurve.driver.variables.clear()
        
        var = fcurve.driver.variables.new()
        var.name = f"var{i}"
        var.type = 'TRANSFORMS'
        var.targets[0].id = main_driver_obj
        var.targets[0].transform_type = 'LOC_X'
        var.targets[0].transform_space = 'WORLD_SPACE'
        
        fcurve.driver.expression = f"var{i}"

def create_rotational_velocity_driver(driven_obj, target_obj):
    """创建旋转速度驱动"""
    fcurve = driven_obj.driver_add('rotation_euler', 2)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "rot_z"
    var.type = 'TRANSFORMS'
    var.targets[0].id = target_obj
    var.targets[0].transform_type = 'ROT_Z'
    var.targets[0].transform_space = 'LOCAL_SPACE'
    
    # 速度乘数
    fcurve.driver.expression = "rot_z * 0.5"
    
    return fcurve

def create_sine_wave_driver(obj, prop, frequency=1.0, amplitude=1.0, offset=0.0):
    """创建正弦波驱动"""
    fcurve = obj.driver_add(prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "frame"
    var.type = 'SINGLE_PROP'
    var.targets[0].id = bpy.context.scene
    var.targets[0].data_path = "frame_current"
    
    # 正弦表达式
    expr = f"sin(frame * {frequency} + {offset}) * {amplitude}"
    fcurve.driver.expression = expr
    
    return fcurve
```

### 变量变换类型

```python
# 可用的变换类型
TRANSFORM_TYPES = [
    'LOC_X', 'LOC_Y', 'LOC_Z',
    'ROT_X', 'ROT_Y', 'ROT_Z',
    'ROT_W',  # 四元数旋转W分量
    'SCALE_X', 'SCALE_Y', 'SCALE_Z',
    'SCALE_AVG',  # 平均缩放
    'LOC_AVG',    # 平均位置
]

# 变换空间
TRANSFORM_SPACES = [
    'WORLD_SPACE',  # 世界空间
    'LOCAL_SPACE',  # 本地空间
    'TRANSFORM_SPACE',  # 变换空间
    'PREVIOUS_SPACE',  # 上一帧空间
]

def create_transform_space_driver(obj, target_obj, transform_type, space):
    """创建指定变换空间和类型的驱动"""
    fcurve = obj.driver_add('scale', 0)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "transform"
    var.type = 'TRANSFORMS'
    var.targets[0].id = target_obj
    var.targets[0].transform_type = transform_type
    var.targets[0].transform_space = space
    
    fcurve.driver.expression = "transform * 0.01"
    
    return fcurve
```

## 驱动表达式

### 数学表达式

```python
def create_math_expression_driver(obj, prop, expression, variables):
    """创建数学表达式驱动"""
    fcurve = obj.driver_add(prop)
    fcurve.driver.variables.clear()
    
    for i, var_info in enumerate(variables):
        var = fcurve.driver.variables.new()
        var.name = var_info['name']
        var.targets[0].id = var_info['target']
        var.targets[0].data_path = var_info['data_path']
    
    fcurve.driver.expression = expression
    
    return fcurve

def create_inverse_driver(obj, target_obj, target_prop):
    """创建反比例驱动"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = "1 / val if val != 0 else 0"
    
    return fcurve

def create_power_driver(obj, target_obj, target_prop, exponent=2):
    """创建幂函数驱动"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = f"val ** {exponent}"
    
    return fcurve

def create_clamped_driver(obj, target_obj, target_prop, min_val=0.0, max_val=1.0):
    """创建限制范围驱动"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = f"max({min_val}, min(val, {max_val}))"
    
    return fcurve
```

### 条件表达式

```python
def create_conditional_driver(obj, target_obj, prop, threshold, high_val, low_val):
    """创建条件判断驱动"""
    fcurve = obj.driver_add(prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = prop
    
    fcurve.driver.expression = f"{high_val} if val > {threshold} else {low_val}"
    
    return fcurve

def create_range_driver(obj, target_obj, prop, 
                        min_threshold, max_threshold,
                        min_val, mid_val, max_val):
    """创建范围分段驱动"""
    fcurve = obj.driver_add(prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = prop
    
    expr = f"""
{val} = var
if {val} < {min_threshold}:
    {min_val}
elif {val} > {max_threshold}:
    {max_val}
else:
    {mid_val}
"""
    fcurve.driver.expression = expr
    
    return fcurve

def create_step_function_driver(obj, target_obj, target_prop, step_size):
    """创建阶梯函数驱动"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = f"int(val / {step_size}) * {step_size}"
    
    return fcurve

def create_smooth_step_driver(obj, target_obj, target_prop, edge0=0.0, edge1=1.0):
    """创建平滑阶梯驱动"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    # 平滑阶梯函数
    expr = f"""
val = var
t = clamp((val - {edge0}) / ({edge1} - {edge0}), 0, 1)
t * t * (3 - 2 * t)
"""
    fcurve.driver.expression = expr
    
    return fcurve
```

### 高级表达式

```python
def create_interpolation_driver(obj, target_obj, target_prop, keys, values):
    """创建插值驱动"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    # 构建线性插值表达式
    expr_parts = []
    for i, (key, value) in enumerate(zip(keys, values)):
        if i == 0:
            expr_parts.append(f"if val <= {key}: {value}")
        elif i == len(keys) - 1:
            expr_parts.append(f"elif val >= {key}: {value}")
        else:
            expr_parts.append(f"elif val < {key}: {value}")
    
    fcurve.driver.expression = "\nelse: ".join(expr_parts)
    
    return fcurve

def create_vector_length_driver(obj, target_obj):
    """创建向量长度驱动"""
    fcurve = obj.driver_add('scale', 0)
    fcurve.driver.variables.clear()
    
    for i, axis in enumerate(['x', 'y', 'z']):
        var = fcurve.driver.variables.new()
        var.name = f"loc_{axis}"
        var.type = 'TRANSFORMS'
        var.targets[0].id = target_obj
        var.targets[0].transform_type = f'LOC_{axis.upper()}'
        var.targets[0].transform_space = 'WORLD_SPACE'
    
    fcurve.driver.expression = "sqrt(loc_x**2 + loc_y**2 + loc_z**2)"
    
    return fcurve

def create_euler_rotation_driver(obj, target_obj):
    """创建欧拉角驱动"""
    fcurve = obj.driver_add('rotation_euler', 2)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "rot"
    var.type = 'TRANSFORMS'
    var.targets[0].id = target_obj
    var.targets[0].transform_type = 'ROT_Z'
    var.targets[0].transform_space = 'LOCAL_SPACE'
    
    fcurve.driver.expression = "radians(degrees(rot)) * 2"
    
    return fcurve
```

## 形状键驱动

### 形状键基础

```python
def create_shape_key_driver(sk_driver_obj, sk_target_obj, key_name, driver_prop, expression="var"):
    """创建形状键驱动程序"""
    sk_driver = sk_driver_obj.data.shape_keys.key_blocks[key_name]
    sk_driver.driver_add('value')
    
    fcurve = sk_driver.driver
    
    # 清除默认变量
    fcurve.variables.clear()
    
    # 添加驱动变量
    var = fcurve.variables.new()
    var.name = "var"
    var.targets[0].id = sk_target_obj
    var.targets[0].data_path = driver_prop
    
    # 设置表达式
    fcurve.expression = expression
    
    return fcurve

def create_morph_target_driver(obj, target_obj, target_prop='location', multiplier=1.0):
    """创建变形目标驱动"""
    if not obj.data.shape_keys:
        return None
    
    for key_block in obj.data.shape_keys.key_blocks:
        fcurve = key_block.driver_add('value')
        fcurve.driver.variables.clear()
        
        var = fcurve.driver.variables.new()
        var.name = "morph_val"
        var.targets[0].id = target_obj
        var.targets[0].data_path = target_prop
        
        fcurve.driver.expression = f"morph_val * {multiplier}"
    
    return True

def set_shape_key_influence(obj, key_name, influence):
    """设置形状键影响程度"""
    if obj.data.shape_keys:
        key_block = obj.data.shape_keys.key_blocks.get(key_name)
        if key_block:
            key_block.value = influence
            key_block.keyframe_insert(data_path='value')
```

### 批量形状键驱动

```python
def create_blink_driver(eye_obj, target_obj, blink_prop='location.y', threshold=0.05):
    """创建眨眼驱动"""
    for key_name in ['Basis', 'EyeOpen', 'EyeClosed']:
        if key_name not in eye_obj.data.shape_keys.key_blocks:
            continue
        
        key_block = eye_obj.data.shape_keys.key_blocks[key_name]
        fcurve = key_block.driver_add('value')
        fcurve.driver.variables.clear()
        
        var = fcurve.driver.variables.new()
        var.name = "blink"
        var.targets[0].id = target_obj
        var.targets[0].data_path = blink_prop
        
        if key_name == 'EyeOpen':
            fcurve.driver.expression = f"1 - clamp(blink / {threshold}, 0, 1)"
        elif key_name == 'EyeClosed':
            fcurve.driver.expression = f"clamp(blink / {threshold}, 0, 1)"
        else:
            fcurve.driver.expression = "1"

def create_weight_driver(mesh_obj, vertex_group, target_prop='location', range_min=0, range_max=1):
    """创建顶点权重驱动"""
    for key_block in mesh_obj.data.shape_keys.key_blocks:
        fcurve = key_block.driver_add('value')
        fcurve.driver.variables.clear()
        
        var = fcurve.driver.variables.new()
        var.name = "val"
        var.type = 'SINGLE_PROP'
        var.targets[0].id = mesh_obj
        var.targets[0].data_path = f'["{vertex_group}"]'
        
        fcurve.driver.expression = f"clamp((val - {range_min}) / ({range_max} - {range_min}), 0, 1)"
    
    return True
```

## 材质驱动

### 材质属性驱动

```python
def create_material_driver(material, prop_path, target_obj, target_prop, expression="var"):
    """创建材质属性驱动"""
    fcurve = material.driver_add(prop_path)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "var"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = expression
    
    return fcurve

def create_emission_strength_driver(material, target_obj):
    """创建自发光强度驱动"""
    fcurve = material.node_tree.nodes["Principled BSDF"].driver_add('inputs[6].default_value')
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "energy"
    var.targets[0].id = target_obj
    var.targets[0].data_path = "location[2]"
    
    fcurve.driver.expression = "max(energy * 10, 0)"
    
    return fcurve

def create_color_shift_driver(material, target_obj):
    """创建颜色变换驱动"""
    base_node = material.node_tree.nodes["Principled BSDF"]
    
    for i, channel in enumerate(['R', 'G', 'B']):
        fcurve = base_node.driver_add(f'inputs[0].default_value', i)
        fcurve.driver.variables.clear()
        
        var = fcurve.driver.variables.new()
        var.name = f"pos_{channel}"
        var.type = 'TRANSFORMS'
        var.targets[0].id = target_obj
        var.targets[0].transform_type = f'LOC_{channel}'
        var.targets[0].transform_space = 'WORLD_SPACE'
        
        fcurve.driver.expression = f"pos_{channel} / 10"
    
    return True
```

### 节点驱动

```python
def create_node_value_driver(node, input_name, target_obj, target_prop, expression="var"):
    """创建节点输入驱动"""
    fcurve = node.driver_add(f'inputs["{input_name}"].default_value')
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = expression
    
    return fcurve

def create_mix_factor_driver(mix_node, target_obj, axis='X'):
    """创建混合因子驱动"""
    fcurve = mix_node.driver_add('fac')
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "mix"
    var.type = 'TRANSFORMS'
    var.targets[0].id = target_obj
    var.targets[0].transform_type = f'LOC_{axis.upper()}'
    var.targets[0].transform_space = 'WORLD_SPACE'
    
    fcurve.driver.expression = "clamp(mix, 0, 1)"
    
    return fcurve

def create_node_output_driver(output_node, target_obj, target_prop='location.y'):
    """创建节点输出驱动"""
    fcurve = output_node.driver_add('default_value')
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = target_obj
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = "val * 0.5"
    
    return fcurve
```

## 驱动程序工具

### 驱动验证

```python
def validate_driver(driver):
    """验证驱动程序有效性"""
    info = {
        'is_valid': driver.is_valid,
        'expression': driver.expression,
        'errors': []
    }
    
    # 检查表达式
    if not driver.expression:
        info['errors'].append("表达式为空")
    
    # 检查变量
    for var in driver.variables:
        for target in var.targets:
            if not target.id:
                info['errors'].append(f"变量 '{var.name}' 的目标无效")
    
    info['is_valid'] = len(info['errors']) == 0
    
    return info

def fix_broken_drivers(obj):
    """修复损坏的驱动程序"""
    fixed_count = 0
    
    if obj.animation_data and obj.animation_data.drivers:
        for fcurve in list(obj.animation_data.drivers):
            driver = fcurve.driver
            
            # 检查变量目标是否仍然存在
            for var in list(driver.variables):
                for target in var.targets:
                    if not target.id:
                        # 移除无效变量
                        driver.variables.remove(var)
                        fixed_count += 1
    
    return fixed_count

def find_driver_sources(obj):
    """查找驱动该对象的所有源"""
    sources = []
    
    for other_obj in bpy.data.objects:
        if other_obj == obj:
            continue
        
        if other_obj.animation_data and other_obj.animation_data.drivers:
            for fcurve in other_obj.animation_data.drivers:
                for var in fcurve.driver.variables:
                    for target in var.targets:
                        if target.id == obj:
                            sources.append({
                                'object': other_obj,
                                'data_path': fcurve.data_path,
                                'expression': fcurve.driver.expression
                            })
    
    return sources
```

### 驱动备份与恢复

```python
def backup_drivers(obj):
    """备份对象的所有驱动程序"""
    if not obj.animation_data:
        return []
    
    backup = []
    for fcurve in obj.animation_data.drivers:
        driver_info = {
            'data_path': fcurve.data_path,
            'expression': fcurve.driver.expression,
            'mode': fcurve.driver.mode,
            'variables': []
        }
        
        for var in fcurve.driver.variables:
            var_info = {
                'name': var.name,
                'type': var.type,
                'targets': []
            }
            
            for target in var.targets:
                target_info = {
                    'id_name': target.id.name if target.id else None,
                    'data_path': target.data_path,
                    'transform_type': target.transform_type,
                    'transform_space': target.transform_space
                }
                var_info['targets'].append(target_info)
            
            driver_info['variables'].append(var_info)
        
        backup.append(driver_info)
    
    return backup

def restore_drivers(obj, backup):
    """恢复驱动程序"""
    # 先移除所有现有驱动
    remove_all_drivers(obj)
    
    for driver_info in backup:
        fcurve = obj.driver_add(driver_info['data_path'])
        
        # 设置表达式
        fcurve.driver.expression = driver_info['expression']
        fcurve.driver.mode = driver_info['mode']
        
        # 恢复变量
        for var_info in driver_info['variables']:
            var = fcurve.driver.variables.new()
            var.name = var_info['name']
            var.type = var_info['type']
            
            for i, target_info in enumerate(var_info['targets']):
                if target_info['id_name']:
                    target_obj = bpy.data.objects.get(target_info['id_name'])
                    if target_obj:
                        var.targets[i].id = target_obj
                var.targets[i].data_path = target_info['data_path']
                var.targets[i].transform_type = target_info['transform_type']
                var.targets[i].transform_space = target_info['transform_space']
```

### 批量驱动操作

```python
def apply_driver_to_selection(source_obj, prop, expression="var"):
    """将驱动应用到所有选中对象"""
    for obj in bpy.context.selected_objects:
        if obj == source_obj:
            continue
        
        fcurve = obj.driver_add(prop)
        fcurve.driver.variables.clear()
        
        var = fcurve.driver.variables.new()
        var.name = "src"
        var.targets[0].id = source_obj
        var.targets[0].data_path = prop
        
        fcurve.driver.expression = expression

def create_synchronized_anim(objects, prop, expression="var"):
    """创建同步动画驱动"""
    if not objects:
        return
    
    # 使用第一个对象作为主驱动
    master = objects[0]
    master_curve = master.driver_add(prop)
    master_curve.driver.variables.clear()
    
    var = master_curve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = master
    var.targets[0].data_path = prop
    
    master_curve.driver.expression = expression
    
    # 为其他对象创建跟随驱动
    for obj in objects[1:]:
        fcurve = obj.driver_add(prop)
        fcurve.driver.variables.clear()
        
        var = fcurve.driver.variables.new()
        var.name = "master_val"
        var.targets[0].id = master
        var.targets[0].data_path = prop
        
        fcurve.driver.expression = expression

def remove_drivers_by_expression(expression):
    """移除使用指定表达式的所有驱动"""
    count = 0
    
    for obj in bpy.data.objects:
        if obj.animation_data and obj.animation_data.drivers:
            for fcurve in list(obj.animation_data.drivers):
                if expression in fcurve.driver.expression:
                    obj.driver_remove(fcurve.data_path)
                    count += 1
    
    return count
```

## 高级应用

### 机械动画

```python
def create_piston_driver(piston_obj, crank_obj, arm_length=2.0):
    """创建活塞驱动"""
    # 活塞位置驱动
    piston_curve = piston_obj.driver_add('location', 1)
    piston_curve.driver.variables.clear()
    
    var_rot = piston_curve.driver.variables.new()
    var_rot.name = "angle"
    var_rot.type = 'TRANSFORMS'
    var_rot.targets[0].id = crank_obj
    var_rot.targets[0].transform_type = 'ROT_Z'
    var_rot.targets[0].transform_space = 'WORLD_SPACE'
    
    var_pos = piston_curve.driver.variables.new()
    var_pos.name = "pos"
    var_pos.type = 'TRANSFORMS'
    var_pos.targets[0].id = crank_obj
    var_pos.targets[0].transform_type = 'LOC_Y'
    var_pos.targets[0].transform_space = 'WORLD_SPACE'
    
    piston_curve.driver.expression = "pos + arm_length * cos(angle)"

def create_wheel_rotation_driver(wheel_obj, vehicle_obj, wheel_radius=0.5):
    """创建车轮旋转驱动"""
    fcurve = wheel_obj.driver_add('rotation_euler', 2)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "loc_x"
    var.type = 'TRANSFORMS'
    var.targets[0].id = vehicle_obj
    var.targets[0].transform_type = 'LOC_X'
    var.targets[0].transform_space = 'WORLD_SPACE'
    
    fcurve.driver.expression = "-loc_x / wheel_radius"

def create_crane_arm_driver(arm_obj, pivot_obj, length=5.0):
    """创建起重机臂驱动"""
    fcurve = arm_obj.driver_add('location', 0)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "angle"
    var.type = 'TRANSFORMS'
    var.targets[0].id = pivot_obj
    var.targets[0].transform_type = 'ROT_Z'
    var.targets[0].transform_space = 'LOCAL_SPACE'
    
    fcurve.driver.expression = f"length * cos(angle)"
```

### 程序化动画

```python
def create_wave_deformation(mesh_obj, amplitude=0.5, frequency=2.0, direction=(1, 0, 0)):
    """创建波浪变形驱动"""
    if not mesh_obj.data.shape_keys:
        return
    
    for key_block in mesh_obj.data.shape_keys.key_blocks:
        for i, vert in enumerate(mesh_obj.data.vertices):
            fcurve = key_block.driver_add('co', i)
            fcurve.driver.variables.clear()
            
            var_x = fcurve.driver.variables.new()
            var_x.name = "x"
            var_x.type = 'SINGLE_PROP'
            var_x.targets[0].id = vert
            var_x.data_path = "co[0]"
            
            var_time = fcurve.driver.variables.new()
            var_time.name = "frame"
            var_time.type = 'SINGLE_PROP'
            var_time.targets[0].id = bpy.context.scene
            var_time.data_path = "frame_current"
            
            # 波浪表达式
            fcurve.driver.expression = f"sin(x * {frequency} + frame / 10) * {amplitude}"

def create_breathe_driver(obj, scale_factor=0.1, speed=1.0):
    """创建呼吸动画驱动"""
    if not obj.data.shape_keys:
        return
    
    basis = obj.data.shape_keys.key_blocks.get('Basis')
    if not basis:
        return
    
    fcurve = basis.driver_add('value')
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "frame"
    var.type = 'SINGLE_PROP'
    var.targets[0].id = bpy.context.scene
    var.targets[0].data_path = "frame_current"
    
    # 呼吸表达式
    fcurve.driver.expression = f"(sin(frame / 10 * {speed}) + 1) * 0.5 * {scale_factor}"

def create_ik_target_driver(end_effector, ik_target, pole_target):
    """创建IK目标驱动"""
    # 设置末端执行器位置
    fcurve_loc = end_effector.driver_add('location')
    fcurve_loc.driver.variables.clear()
    
    var = fcurve_loc.driver.variables.new()
    var.name = "target"
    var.targets[0].id = ik_target
    var.targets[0].data_path = "location"
    
    fcurve_loc.driver.expression = "target"
    
    # 设置pole目标方向
    fcurve_rot = end_effector.driver_add('rotation_euler', 2)
    fcurve_rot.driver.variables.clear()
    
    var = fcurve_rot.driver.variables.new()
    var.name = "pole"
    var.targets[0].id = pole_target
    var.targets[0].data_path = "location[0]"
    
    fcurve_rot.driver.expression = "atan2(pole, 1)"
```

### 交互式驱动

```python
def create_slider_controlled_property(obj, prop_name, min_val=0, max_val=1):
    """创建滑块控制的属性"""
    # 添加自定义属性作为控制器
    if prop_name not in obj.keys():
        obj[prop_name] = (min_val + max_val) / 2
    
    # 设置属性UI滑块
    obj.property_overridable_library_set(f'["{prop_name}"]', True)
    
    # 创建驱动
    fcurve = obj.driver_add(prop_name)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "controller"
    var.targets[0].id = obj
    var.targets[0].data_path = f'["{prop_name}"]'
    
    fcurve.driver.expression = "controller"
    
    return True

def create_layer_weight_driver(material, layer_weight_node, target_prop='location'):
    """创建层权重驱动"""
    fcurve = layer_weight_node.driver_add('inputs[0].default_value')
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "val"
    var.targets[0].id = bpy.context.active_object
    var.targets[0].data_path = target_prop
    
    fcurve.driver.expression = "clamp(val, 0, 1)"
    
    return fcurve

def create_time_offset_driver(obj, target_obj, target_prop, offset_frames=10):
    """创建时间偏移驱动"""
    fcurve = obj.driver_add(target_prop)
    fcurve.driver.variables.clear()
    
    var = fcurve.driver.variables.new()
    var.name = "frame"
    var.type = 'SINGLE_PROP'
    var.targets[0].id = bpy.context.scene
    var.targets[0].data_path = "frame_current"
    
    var_target = fcurve.driver.variables.new()
    var_target.name = "target_val"
    var_target.type = 'SINGLE_PROP'
    var_target.targets[0].id = target_obj
    var_target.targets[0].data_path = target_prop
    
    fcurve.driver.expression = f"target_val at (frame - {offset_frames})"
```


---
## bpy_mathutils

# mathutils 模块 - 数学类型与工具

## 模块概述

mathutils 模块提供对 Blender 数学运算的访问，是进行 3D 开发的基础工具模块。该模块包含多种数学类型，支持向量、矩阵、四元数、欧拉角和颜色等数学对象的创建和操作。

## 核心类

### Vector - 向量类

向量是 3D 开发中最常用的数学对象之一，支持 2D、3D 和 4D 向量运算。

```python
import mathutils
from math import radians

# 创建 3D 向量
vec = mathutils.Vector((1.0, 2.0, 3.0))

# 向量运算
vec.length()                    # 获取向量长度
vec.normalize()                 # 归一化向量
vec.dot(other_vec)              # 点积运算
vec.cross(other_vec)            # 叉积运算
vec.angle(other_vec)            # 计算两向量夹角
vec.to_3d()                     # 转换为 3D 向量
vec.to_4d()                     # 转换为 4D 向量
vec.to_euler()                  # 转换为欧拉角
vec.to_quaternion()             # 转换为四元数
vec.to_matrix()                 # 转换为旋转矩阵
```

### Matrix - 矩阵类

矩阵用于表示变换，包括旋转、缩放、平移和投影等操作。

```python
import mathutils
from math import radians

# 创建旋转矩阵
mat_rot = mathutils.Matrix.Rotation(radians(90.0), 4, 'X')

# 创建平移矩阵
mat_trans = mathutils.Matrix.Translation(vec)

# 组合变换
mat = mat_trans @ mat_rot      # 矩阵乘法
mat.invert()                   # 矩阵求逆

# 矩阵转换
mat.to_3x3()                   # 转换为 3x3 矩阵
mat.to_quaternion()            # 转换为四元数
mat.to_euler()                 # 转换为欧拉角
mat.decompose()                # 分解为位置、旋转、缩放
```

### Euler - 欧拉角类

欧拉角使用三个旋转角度表示方向，适合角度输入和用户界面。

```python
import mathutils
from math import radians

# 创建欧拉角（弧度制）
euler = mathutils.Euler((radians(90), 0, 0), 'XYZ')

# 欧拉角转换
euler.to_quaternion()          # 转换为四元数
euler.to_matrix()              # 转换为旋转矩阵
euler.rotate(quat)             # 使用四元数旋转
euler.rotate_axis('X', angle)  # 绕轴旋转
```

### Quaternion - 四元数类

四元数是表示 3D 旋转的最佳方式，避免了欧拉角的万向节死锁问题。

```python
import mathutils
from math import radians

# 创建四元数
quat = mathutils.Quaternion((1, 0, 0), radians(90))

# 四元数运算
quat.rotation_difference(other_quat)  # 计算旋转差
quat.slerp(other_quat, factor)        # 球面线性插值
quat.invert()                         # 四元数求逆
quat.normalize()                      # 归一化
quat.to_euler()                       # 转换为欧拉角
quat.to_matrix()                      # 转换为矩阵
```

### Color - 颜色类

颜色对象用于管理 RGB 和 HSV 颜色空间。

```python
import mathutils

# 创建颜色（RGB 值 0-1）
col = mathutils.Color((0.0, 0.0, 1.0))  # 蓝色

# 访问颜色分量
col.r, col.g, col.b                    # RGB 分量
col.h, col.s, col.v                    # HSV 分量
col[:]                                 # 获取所有分量

# 颜色操作
col.s *= 0.5                           # 调整饱和度
col += mathutils.Color((0.25, 0, 0))   # 颜色相加
col * 255                              # 转换为 0-255 范围

# 颜色空间转换
col.from_aces_to_scene_linear()        # ACES 到场景线性
col.from_acescg_to_scene_linear()      # ACEScg 到场景线性
```

## 子模块

### mathutils.geometry - 几何工具

提供几何计算功能，包括多边形重心、交点计算等。

```python
import mathutils.geometry as geometry

# 多边形重心计算
center = geometry.polygon_center(vertices, polygon)

# 直线与平面交点
intersection = geometry.intersect_line_plane(line_a, line_b, plane_co, plane_no)

# 直线与球体交点
intersections = geometry.intersect_line_sphere(line_a, line_b, sphere_co, sphere_radius)
```

### mathutils.bvhtree - BVH 树

用于空间查询和碰撞检测的边界体积层次结构。

```python
import mathutils.bvhtree as bvhtree

# 从网格创建 BVH 树
bvh = bvhtree.BVHTree.FromObject(obj, depsgraph)

# 最近点查询
nearest_point, normal, index, distance = bvh.find_nearest(point)

# 光线投射
hit_location, hit_normal, hit_index, hit_distance = bvh.ray_cast(origin, direction)
```

### mathutils.kdtree - KD 树

用于最近邻搜索的空间分割数据结构。

```python
import mathutils.kdtree as kdtree

# 创建 KD 树
size = 1000
kd = kdtree.KDTree(size)

# 插入点数据
for i in range(size):
    kd.insert((i, i, i), i)

# 重新构建
kd.balance()

# 查找最近点
coord, index, distance = kd.find((0.5, 0.5, 0.5))

# 查找范围内所有点
results = kd.find_range((0, 0, 0), 10.0)
```

### mathutils.interpolate - 插值工具

提供曲线和曲面的插值功能。

```python
import mathutils.interpolate as interpolate

# 贝塞尔插值
point = interpolate.bezier_interp(c0, c1, c2, c3, t)

# 三次样条插值
points = interpolate.cubic_interp(ctrl_points, t)
```

### mathutils.noise - 噪声工具

提供各种噪声函数用于程序化生成。

```python
import mathutils.noise as noise

# 柏林噪声
value = noise.noise(vector, scale, detail, distortion, noise_basis)

# 随机平滑噪声
value = noise.snoise(vector, scale, detail, distortion)

#  voronoi 噪声
value = noise.voronoi(x, y, z, scale, randomness, distance_metric)
```

## 实际应用示例

### 顶点变换

```python
import mathutils
import bpy

# 获取活动对象的网格
obj = bpy.context.active_object
mesh = obj.data

# 创建变换矩阵
mat_loc = mathutils.Matrix.Translation(obj.location)
mat_rot = obj.rotation_euler.to_matrix().to_4x4()
mat_scale = mathutils.Matrix.Scale(obj.scale.x, 4, (1, 0, 0))
mat_scale @= mathutils.Matrix.Scale(obj.scale.y, 4, (0, 1, 0))
mat_scale @= mathutils.Matrix.Scale(obj.scale.z, 4, (0, 0, 1))
transform_mat = mat_loc @ mat_rot @ mat_scale

# 变换所有顶点
for vert in mesh.vertices:
    vert.co = transform_mat @ vert.co
```

### 法线计算

```python
import mathutils
from math import radians

# 计算面法线
v1 = mathutils.Vector(face.vertices[1] - face.vertices[0])
v2 = mathutils.Vector(face.vertices[2] - face.vertices[0])
normal = v1.cross(v2).normalize()

# 计算顶点法线（平均相邻面法线）
vert_normal = mathutils.Vector((0, 0, 0))
for poly in mesh.polygons:
    if vert_idx in poly.vertices:
        vert_normal += poly.normal
vert_normal.normalize()
```

## 性能优化

```python
import mathutils

# 避免在循环中重复创建向量对象
vec_base = mathutils.Vector((1, 2, 3))

# 使用原地运算（更快）
vec_base.normalize()              # 修改原对象
vec_base.negate()                 # 取反
vec_base.scale(2.0)               # 缩放

# 使用 @ 运算符进行矩阵乘法（Python 3.5+）
result = matrix_a @ matrix_b      # 比 multiply() 更快
```


