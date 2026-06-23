# bpy_advanced

---
## bpy_advanced_features

# 进阶特性模块

## 进阶特性概述

本章涵盖高级使用内容（典型用法可能不需要的主题）。命令行、控制台窗口、扩展系统、应用程序模板、部署和目录结构都属于高级特性范畴，掌握这些内容可以提升Blender的使用效率和自动化能力。

```python
# 进阶特性分类
# 命令行使用 - 批处理和自动化
# 扩展系统 - 插件管理和分发
# 应用程序模板 - 自定义启动界面
# 目录结构 - 文件组织方式
# 部署配置 - 自定义安装和分发
# 运算符系统 - Blender核心操作机制
# 快捷键编辑 - 自定义输入绑定

# 进阶特性用途
# 自动化工作流程
# 团队协作和标准化
# 自定义开发环境
# 大规模部署
# 性能优化和问题诊断
```

## 命令行使用

### 命令行基础

控制台窗口（也称为终端）是显示Blender操作、状态和内部错误的操作系统文本窗口。当手动从终端启动Blender时，Blender输出会显示在启动它的控制台窗口中。

```python
# 命令行使用场景
# 渲染动画
# 需要使用不同参数启动Blender的自动化和批处理
# Python开发，查看print()函数输出
# Blender意外退出时查看错误信息
# 故障排除，查看--debug消息

# 启动Blender
blender  # 启动默认界面
blender -b file.blend  # 后台模式启动
blender -S "Scene" file.blend  # 指定场景

# 关闭控制台窗口
# 关闭控制台窗口也会关闭Blender
# 丢失任何未保存的工作
```

### 渲染参数

```python
# 基础渲染参数
blender -b file.blend -f 10
# -b 后台模式，不显示界面
# -f 帧号，渲染单帧

# 渲染帧范围
blender -b file.blend -s 1 -e 100 -a
# -s 起始帧
# -e 结束帧
# -a 所有活动相机

# 渲染输出设置
blender -b file.blend -F PNG -o //render/frame_####
# -F 输出格式
# -o 输出路径

# Cycles特定参数
blender -b file.blend --cycles-device CPU
# --cycles-device 指定渲染设备
# 可选：CPU、CUDA、OPTIX、HIP、METAL

# 采样设置
blender -b file.blend --cycles-samples 128
# --cycles-samples 采样数
```

### Python参数

```python
# Python脚本执行
blender --python script.py
# 执行指定Python脚本

# Python控制台
blender --python-console
# 启动Python控制台

# Python表达式
blender --python-expr "import bpy; print(bpy.data.objects[0].name)"
# 执行Python表达式

# 启动脚本
blender --python-startup startup.py
# 每次启动时执行脚本

# 禁用Python
blender --disable-python
# 禁用Python支持
```

### 调试参数

```python
# 调试模式
blender --debug
# 启用调试输出

# 调试所有类型
blender --debug-all
# 启用所有调试输出

# 内存调试
blender --debug-memory
# 输出内存分配信息

# 事件调试
blender --debug-events
# 输出事件处理信息

# 运算符调试
blender --debug-operators
# 输出运算符调用信息

# 释放构建
blender --release
# 以发布模式运行
```

### 窗口参数

```python
# 窗口设置
blender --window-fullscreen
# 全屏模式启动

blender --window-geometry 0 0 1920 1080
# 设置窗口位置和大小

blender --no-window-focus
# 启动时不聚焦窗口

blender --start-fullscreen
# 启动时全屏

# 屏数量
blender --screen 2
# 打开多个屏幕
```

### 日志参数

```python
# 日志输出
blender --log level
# 设置日志级别
# 级别：debug、info、warning、error

# 日志文件
blender --log-file output.log
# 指定日志输出文件

# 控制台日志
blender --verbose 1
# 设置详细程度
```

## 扩展系统

### 扩展基础

扩展系统是Blender 4.2引入的新插件管理方式，与传统的add-on相比具有更好的版本管理和分发能力。

```python
# 扩展系统特点
# 自包含 - 所有依赖项随扩展一起提供
# 版本管理 - 支持版本更新
# 命名空间 - 避免命名冲突
# 依赖管理 - 支持Python Wheels

# 扩展 vs 传统插件
# 扩展使用manifest.json文件
# 扩展使用bl_ext命名空间
# 扩展支持自动更新
# 传统插件使用bl_info字典
```

### 扩展开发

```python
# 扩展目录结构
# extension/
# ├── __init__.py
# ├── manifest.json
# ├── modules/
# └── wheels/

# manifest.json示例
{
    "schema_version": "1.0.0",
    "id": "my_extension",
    "version": "1.0.0",
    "name": "My Extension",
    "author": "Author Name",
    "description": "Description of the extension",
    "blender_version_min": "4.2.0",
    "blender_version_max": "5.0.0",
    "dependencies": [
        "requests>=2.25.0"
    ]
}

# 创建扩展包
import bpy
extension_dir = bpy.utils.extension_path_user(__package__, path="", create=True)
```

### 传统插件转换

```python
# 转换步骤
# 1. 创建manifest.json文件
# 2. 删除bl_info信息
# 3. 将模块名引用替换为__package__
# 4. 使用相对导入
# 5. 使用wheels打包外部依赖
# 6. 彻底测试

# 转换前后对比
# 之前：bl_idname = "kitsu"
# 之后：bl_idname = "bl_ext.repo_name.kitsu"

# 访问偏好设置
# 之前：bpy.context.preferences.addons["kitsu"]
# 之后：bpy.context.preferences.addons["bl_ext.repo_name.kitsu"]
```

### 依赖管理

```python
# 打包依赖
# 使用Python Wheels打包第三方模块
# 避免版本冲突
# 确保自包含

# 本地存储
# 扩展不能假设自己的目录可写
# 使用工具函数获取用户目录
extension_dir = bpy.utils.extension_path_user(__package__, path="", create=True)

# 供应商化依赖
# 使用vendorize工具
# 避免版本冲突
# 需要额外设置工作
```

### 在线访问

```python
# 检查在线访问权限
if bpy.app.online_access:
    # 可以访问网络
    pass

# 覆盖设置
# 可以通过命令行覆盖
# --offline-mode / --online-mode

# 检查覆盖
if bpy.app.online_access_override is not None:
    can_access = bpy.app.online_access_override
```

## 应用程序模板

### 模板基础

应用程序模板允许您自定义Blender的启动界面和工作环境，适用于团队协作和标准化工作流程。

```python
# 模板位置
# 用户模板：~/.config/blender/版本/templates/
# 系统模板：Blender安装目录/templates/

# 模板类型
# 项目模板 - 预设场景和设置
# 启动模板 - 自定义启动界面
# 工作室模板 - 团队标准化配置

# 创建模板
# 1. 在Blender中设置工作环境
# 2. 导出为模板
# 3. 放置在模板目录
```

### 模板结构

```python
# 模板目录结构
# template_name/
# ├── startup.blend  # 启动文件
# ├── preview.png    # 预览图像
# └── template.json  # 模板信息

# template.json示例
{
    "name": "My Template",
    "version": "1.0.0",
    "author": "Author",
    "description": "Description"
}

# 使用模板
blender --template "template_name"
```

### 自定义启动

```python
# 自定义启动文件
# startup.blend包含：
# 默认场景设置
# 常用工具架配置
# 快捷键设置
# 工作区布局

# 启动脚本
# 每次启动时执行
# 用于设置环境
# 加载常用资源
```

## Blender目录结构

### 用户目录

```python
# 用户配置目录
# Linux/macOS: ~/.config/blender/
# Windows: %APPDATA%\Blender\

# 版本子目录
# ~/.config/blender/3.6/
# ~/.config/blender/4.2/

# 子目录结构
# config/      - 用户配置
# scripts/     - Python脚本
# shaders/     - 着色器
# cache/       - 缓存文件
# temp/        - 临时文件

# 启动文件
# startup.blend  - 默认启动场景
# userpref.blend - 用户偏好设置
```

### 安装目录

```python
# 安装目录结构
# Linux: /usr/share/blender/
# macOS: /Applications/Blender.app/Contents/Resources/
# Windows: C:\Program Files\Blender Foundation\Blender\

# 子目录
# 3.6/       - 版本特定文件
# scripts/   - 内置脚本
# shaders/   - 着色器文件
# datafiles/ - 数据文件
# locale/    - 翻译文件
```

### 数据目录

```python
# 临时目录
# Linux: /tmp/blender/
# macOS: /tmp/blender/
# Windows: %TEMP%\blender\

# 缓存目录
# 用于缓存渲染结果
# 用于缓存几何体
# 用于缓存纹理

# 交换文件
# 内存不足时使用
# 自动管理
```

## 部署Blender

### 自定义安装

```python
# 安装选项
# Windows: 使用安装程序或ZIP压缩包
# macOS: DMG镜像或ZIP压缩包
# Linux: Snap、Flatpak或源码编译

# 自定义安装位置
# 指定安装目录
# 设置环境变量
# 配置启动脚本

# 环境变量
# BLENDER_USER_CONFIG - 用户配置目录
# BLENDER_USER_SCRIPTS - 用户脚本目录
# BLENDER_SYSTEM_SCRIPTS - 系统脚本目录
# BLENDER_SYSTEM_SHADERS - 系统着色器目录
```

### 便携版本

```python
# 便携版创建
# 1. 解压Blender压缩包
# 2. 配置用户目录
# 3. 复制常用资源

# 便携版配置
# 设置BLENDER_USER_CONFIG
# 设置BLENDER_USER_SCRIPTS

# 使用场景
# USB设备携带
# 多台机器使用
# 环境标准化
```

### 团队部署

```python
# 部署策略
# 统一安装版本
# 统一配置模板
# 统一插件集合
# 统一资源库

# 自动化部署
# 使用脚本安装
# 批量配置设置
# 分发模板文件

# 版本控制配置
# 将配置纳入版本控制
# 同步团队设置
# 管理变更历史
```

## 运算符系统

### 运算符基础

运算符是Blender中执行操作的核心机制，从菜单项到快捷键，所有操作都通过运算符实现。

```python
# 运算符类型
# bpy.ops.* - Python运算符
# WM_OT_* - 窗口管理运算符
# OBJECT_OT_* - 对象运算符
# MESH_OT_* - 网格运算符

# 调用运算符
import bpy
bpy.ops.object.select_all(action='SELECT')

# 运算符属性
# 确定操作
# 取消操作
# 属性更新
# 回调函数

# 运算符属性
# bl_idname - 唯一标识符
# bl_label - 显示名称
# bl_description - 描述
# bl_options - 选项
```

### 创建运算符

```python
# 创建自定义运算符
import bpy

class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "My Custom Operator"
    bl_description = "Perform a custom operation"
    bl_options = {'REGISTER', 'UNDO'}

    # 属性定义
    my_property: bpy.props.FloatProperty(
        name="My Property",
        default=1.0
    )

    def execute(self, context):
        # 执行操作
        print(f"Property value: {self.my_property}")
        return {'FINISHED'}

    def invoke(self, context, event):
        # 调用对话框
        return context.window_manager.invoke_props_dialog(self)

# 注册运算符
bpy.utils.register_class(MyOperator)

# 调用运算符
bpy.ops.object.my_operator(my_property=2.0)
```

### 运算符选项

```python
# 运算符选项
# REGISTER - 注册到撤销栈
# UNDO - 支持撤销
# GRAB_POINTER - 捕获指针
# BLOCKING - 阻塞操作
# MACRO - 宏运算符
# PRESET - 预设运算符
# INTERNAL - 内部运算符

# 示例
class PresetOperator(bpy.types.Operator):
    bl_idname = "object.my_preset"
    bl_options = {'REGISTER', 'UNDO', 'PRESET'}
```

### 运算符搜索

```python
# 搜索运算符
bpy.ops.wm.search_operator(text="Select All")

# 列出所有运算符
for op in bpy.ops.__dict__.values():
    if hasattr(op, 'bl_idname'):
        print(op.bl_idname)

# 获取运算符属性
op = bpy.ops.object.select_all
print(f"ID: {op.bl_idname}")
print(f"Label: {op.bl_label}")
```

## 快捷键编辑

### 快捷键基础

快捷键编辑允许您自定义Blender的输入绑定，以适应不同的工作流程和偏好。

```python
# 快捷键结构
# 键位 - 按下的键组合
# 运算符 - 触发的操作
# 上下文 - 生效的条件

# 快捷键类型
# 键盘快捷键
# 鼠标快捷键
# 触控板手势
# 平板电脑输入

# 快捷键修饰符
# CTRL - Ctrl键
# SHIFT - Shift键
# ALT - Alt键
# OS - Windows/Command键
```

### 编辑快捷键

```python
# 访问快捷键设置
import bpy
prefs = bpy.context.preferences

# 获取快捷键映射
keyconfig = prefs.keymaps.active

# 添加快捷键
keyconfig.keymap_items.new(
    idname="object.my_operator",
    type='F5',
    value='PRESS',
    ctrl=True,
    shift=False
)

# 修改快捷键
for item in keyconfig.keymap_items:
    if item.idname == "object.select_all":
        item.type = 'A'
        item.ctrl = True

# 删除快捷键
for item in keyconfig.keymap_items:
    if item.idname == "object.my_operator":
        keyconfig.keymap_items.remove(item)

# 重置快捷键
bpy.ops.wm.keymap_restore(item_id="3D View")
```

### 快捷键预设

```python
# 内置预设
# Blender默认
# Blender行业标准

# 导入预设
bpy.ops.wm.keyconfig_import(
    filepath="//custom_keymap.py"
)

# 导出预设
bpy.ops.wm.keyconfig_export(
    filepath="//custom_keymap.py"
)

# 切换预设
bpy.ops.wm.keyconfig_activate(name="Blender Industry")
```

## 旋转系统

### 旋转基础

Blender使用多种旋转表示方法，每种方法都有其特定的用途和优势。

```python
# 旋转类型
# 欧拉角 - 三个旋转角度
# 轴角 - 轴和角度
# 四元数 - 四元数
# 矩阵 - 变换矩阵

# 欧拉角
# 直观易懂
# 可能有万向节锁
# XYZ、ZXZ等顺序

# 轴角
# 直观表示
# 紧凑存储
# 用于骨骼旋转

# 四元数
# 无万向节锁
# 平滑插值
# 空间效率高

# 矩阵
# 最通用
# 支持任意变换
# 适合数学计算
```

### 旋转转换

```python
import bpy
import math

obj = bpy.context.active_object

# 获取旋转
euler = obj.rotation_euler
quaternion = obj.rotation_quaternion
matrix = obj.matrix_world.to_3x3()

# 设置旋转
obj.rotation_euler = (math.radians(45), 0, 0)
obj.rotation_quaternion = (1, 0, 0, 0)

# 转换
from mathutils import Euler, Quaternion, Matrix

# 欧拉到四元数
euler = Euler((math.radians(45), 0, 0), 'XYZ')
quat = euler.to_quaternion()

# 四元数到欧拉
euler = quat.to_euler('XYZ')

# 欧拉到矩阵
matrix = euler.to_matrix()

# 矩阵到四元数
quat = matrix.to_quaternion()
```

### 旋转插值

```python
from mathutils import Vector, Quaternion, slerp

# 四元数插值
q1 = Quaternion((1, 0, 0), 0)
q2 = Quaternion((0, 1, 0), math.radians(90))

# 使用slerp插值
slerped = q1.slerp(q2, 0.5)

# 使用内置slerp
slerped = slerp(q1, q2, 0.5)

# 欧拉角插值
# 需要先转换为四元数
euler1 = Euler((0, 0, 0), 'XYZ')
euler2 = Euler((math.radians(90), 0, 0), 'XYZ')

q1 = euler1.to_quaternion()
q2 = euler2.to_quaternion()
slerped_quat = q1.slerp(q2, 0.5)
slerped_euler = slerped_quat.to_euler('XYZ')
```

## 限制和约束

### 系统限制

```python
# 顶点限制
# 最大顶点数：约20亿
# 实际限制取决于内存

# 面限制
# 最大面数：约20亿
# 实际限制取决于内存

# 对象限制
# 理论上无限制
# 实际受限于性能

# 动画长度
# 最大帧数：无限
# 受限于文件格式

# 文件大小
# 理论上无限制
# 实际受限于文件系统
```

### 性能约束

```python
# 内存限制
# 32位系统：约4GB
# 64位系统：受限于物理内存

# GPU限制
# VRAM限制
# 纹理大小限制
# 着色器复杂度限制

# 视口限制
# 多边形数量
# 灯光数量
# 反射数量

# 渲染限制
# 采样数量
# 光线反弹次数
# 体积步数
```

## Python脚本

### 自动化脚本

```python
# 批处理渲染
import subprocess
import os

blender_path = "/path/to/blender"
input_dir = "//render_queue/"
output_dir = "//output/"

for filename in os.listdir(input_dir):
    if filename.endswith(".blend"):
        input_file = os.path.join(input_dir, filename)
        output_file = os.path.join(output_dir, filename.replace(".blend", ".png"))
        
        cmd = [
            blender_path,
            "-b", input_file,
            "-F", "PNG",
            "-o", output_file,
            "-f", "1"
        ]
        
        subprocess.run(cmd)

# 自动化配置
def configure_blender():
    prefs = bpy.context.preferences
    
    # 设置UI
    prefs.view.ui_scale = 1.0
    
    # 设置系统
    prefs.system.gpu_backend = 'VULKAN'
    
    # 设置路径
    prefs.filepaths.render_output_directory = "//render/"
    
    # 保存
    bpy.ops.wm.save_userpref()
```

### 环境检测

```python
# 检测系统信息
import bpy
import platform
import sys

print(f"操作系统: {platform.system()}")
print(f"版本: {platform.version()}")
print(f"架构: {platform.machine()}")
print(f"Python版本: {sys.version}")
print(f"Blender版本: bpy.app.version_string}")

# 检测GPU
prefs = bpy.context.preferences
print(f"GPU后端: {prefs.system.gpu_backend}")

# 检测内存
import psutil
process = psutil.Process()
print(f"内存使用: {process.memory_info().rss / 1024 / 1024:.2f} MB")
```

## 最佳实践

### 命令行使用

```python
# 使用后台渲染
# -b 参数启用后台模式
# 适合批处理和服务器渲染

# 日志记录
# 使用 --log-file 参数记录输出
# 便于问题排查

# 错误处理
# 检查退出代码
# 处理渲染失败
# 记录错误信息
```

### 扩展开发

```python
# 遵循编码规范
# 使用清晰的命名
# 添加详细文档
# 测试跨版本兼容性

# 依赖管理
# 声明所有依赖
# 使用版本约束
# 避免版本冲突

# 用户体验
# 提供配置选项
# 添加进度反馈
# 处理错误情况
```

### 团队协作

```python
# 统一配置
# 使用应用程序模板
# 版本控制配置
# 定期同步更新

# 文档记录
# 记录自定义设置
# 编写操作指南
# 分享最佳实践

# 培训支持
# 提供培训材料
# 建立支持渠道
# 定期更新文档
```

## 常见问题解决

### 命令行问题

```python
# Blender不启动
# 检查路径是否正确
# 检查权限设置
# 查看错误消息

# 渲染失败
# 检查文件路径
# 验证场景完整性
# 查看渲染日志

# 参数不识别
# 检查参数拼写
# 确认Blender版本
# 查看帮助信息
```

### 扩展问题

```python
# 扩展无法启用
# 查看控制台错误
# 检查依赖是否满足
# 验证文件完整性

# 依赖冲突
# 使用虚拟环境
# 锁定依赖版本
# 使用wheels打包

# 更新失败
# 检查网络连接
# 验证仓库地址
# 手动更新尝试
```

### 性能问题

```python
# 内存不足
# 减少场景复杂度
# 使用代理对象
# 增加虚拟内存

# 渲染缓慢
# 降低采样数
# 使用GPU渲染
# 优化场景设置

# 启动缓慢
# 减少启动脚本
# 清理未使用数据
# 使用SSD存储
```


---
## bpy_advanced_python

# Blender Python高级模式模块

## Python高级模式概述

Blender Python开发涉及多种高级编程模式和最佳实践。这些模式帮助开发者编写更清晰、更可维护、更高效的代码。掌握这些模式对于创建专业的Blender插件和工具至关重要。

```python
# 高级Python模式
# 装饰器模式 - 扩展函数功能
# 上下文管理器 - 资源管理
# 工厂模式 - 创建对象
# 单例模式 - 共享状态
# 观察者模式 - 事件处理
# 策略模式 - 算法切换
# 模板方法模式 - 算法骨架
# 状态模式 - 状态管理

# Python特性
# 生成器 - 惰性计算
# 迭代器 - 遍历数据
# 描述符 - 属性管理
# 元类 - 类创建
# 上下文管理器 - 资源管理

# Blender特定模式
# 注册系统
# 运算符链式调用
# 属性更新回调
# 事件处理系统
```

## 装饰器模式

### 基本装饰器

```python
# 简单函数装饰器
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"调用函数: {func.__name__}")
        result = func(*args, **kwargs)
        print(f"函数完成: {func.__name__}")
        return result
    return wrapper

@my_decorator
def my_function():
    print("执行我的函数")

# 类装饰器
def class_decorator(cls):
    class WrappedClass(cls):
        def new_method(self):
            print("添加的新方法")
    return WrappedClass

@class_decorator
class MyClass:
    pass

# 带参数的装饰器
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"你好, {name}!")
```

### Blender装饰器

```python
# 运算符装饰器
class RepeatOperator(bpy.types.Operator):
    bl_idname = "object.repeat_operator"
    bl_label = "重复运算符"
    bl_options = {'REGISTER', 'UNDO'}
    
    times: bpy.props.IntProperty(
        name="次数",
        default=3,
        min=1,
        max=100
    )
    
    def execute(self, context):
        for _ in range(self.times):
            bpy.ops.object.duplicate()
        return {'FINISHED'}

# 面板装饰器
class MyPanel(bpy.types.Panel):
    bl_label = "我的面板"
    bl_idname = "OBJECT_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '工具'
    
    def draw(self, context):
        layout = self.layout
        layout.label(text="面板内容")

# 注册装饰器
def register_panel(panel_class):
    bpy.utils.register_class(panel_class)
    return panel_class

@register_panel
class NewPanel(bpy.types.Panel):
    bl_label = "新面板"
    bl_idname = "OBJECT_PT_new_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '工具'
    
    def draw(self, context):
        pass
```

### 高级装饰器

```python
# 属性验证装饰器
def validate_range(min_val, max_val):
    def decorator(func):
        def wrapper(value):
            if not (min_val <= value <= max_val):
                print(f"警告: 值 {value} 超出范围 [{min_val}, {max_val}]")
            return func(value)
        return wrapper
    return decorator

@validate_range(0, 100)
def set_intensity(value):
    print(f"设置强度: {value}")

# 日志装饰器
def log_operation(operation_name):
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(f"[{operation_name}] 开始")
            result = func(*args, **kwargs)
            print(f"[{operation_name}] 完成")
            return result
        return wrapper
    return decorator

@log_operation("对象操作")
def modify_object(obj_name):
    obj = bpy.data.objects.get(obj_name)
    if obj:
        obj.location += (0.1, 0, 0)

# 错误处理装饰器
def handle_errors(func):
    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except Exception as e:
            print(f"错误: {e}")
            import traceback
            traceback.print_exc()
            return None
    return wrapper

@handle_errors
def risky_operation():
    raise ValueError("模拟错误")
```

## 上下文管理器

### 基本用法

```python
# 简单的上下文管理器
class FileManager:
    def __init__(self, filename):
        self.filename = filename
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, 'r')
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        return False

# 使用with语句
with FileManager("data.txt") as fm:
    content = fm.file.read()
    print(content)
```

### Blender上下文管理器

```python
# 模式切换上下文管理器
class ModeSwitcher:
    def __init__(self, obj, mode):
        self.obj = obj
        self.mode = mode
        self.original_mode = None
    
    def __enter__(self):
        self.original_mode = self.obj.mode
        bpy.ops.object.mode_set(mode=self.mode)
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        bpy.ops.object.mode_set(mode=self.original_mode)
        return False

# 使用
obj = bpy.context.active_object
with ModeSwitcher(obj, 'EDIT'):
    # 在编辑模式下操作
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.mesh.subdivide(number_cuts=2)

# 选择上下文管理器
class SelectionContext:
    def __init__(self, objects):
        self.objects = objects
        self.previous_selection = None
    
    def __enter__(self):
        self.previous_selection = bpy.context.selected_objects.copy()
        bpy.ops.object.select_all(action='DESELECT')
        for obj in self.objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = self.objects[0]
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        bpy.ops.object.select_all(action='DESELECT')
        for obj in self.previous_selection:
            obj.select_set(True)
        return False

# 使用
cubes = [obj for obj in bpy.data.objects if obj.type == 'MESH']
with SelectionContext(cubes):
    bpy.ops.object.delete()

# 属性动画上下文管理器
class AnimationContext:
    def __init__(self, obj):
        self.obj = obj
        self.keyframes = {}
    
    def __enter__(self):
        # 记录当前位置
        self.keyframes['location'] = self.obj.location.copy()
        self.keyframes['rotation'] = self.obj.rotation_euler.copy()
        self.keyframes['scale'] = self.obj.scale.copy()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        # 恢复位置
        self.obj.location = self.keyframes['location']
        self.obj.rotation_euler = self.keyframes['rotation']
        self.obj.scale = self.keyframes['scale']
        return False

# 使用
with AnimationContext(bpy.context.active_object):
    bpy.context.active_object.location = (5, 0, 0)
```

### 高级上下文管理器

```python
# 临时设置上下文管理器
class TemporarySettings:
    def __init__(self, **kwargs):
        self.settings = kwargs
        self.original = {}
    
    def __enter__(self):
        context = bpy.context
        for attr, value in self.settings.items():
            if hasattr(context, attr):
                self.original[attr] = getattr(context, attr)
                setattr(context, attr, value)
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        context = bpy.context
        for attr, value in self.original.items():
            setattr(context, attr, value)
        return False

# 使用
with TemporarySettings(user_edit = False, scene_unit_settings.system='METRIC'):
    bpy.ops.object.transform_apply(location=True)

# 视口设置上下文管理器
class ViewportSettings:
    def __init__(self, area):
        self.area = area
        self.original = {}
    
    def __enter__(self):
        space = self.area.spaces[0]
        self.original = {
            'shading_type': space.shading.type,
            'show_overlays': space.overlay.show_overlays
        }
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        space = self.area.spaces[0]
        space.shading.type = self.original['shading_type']
        space.overlay.show_overlays = self.original['show_overlays']
        return False

# 使用
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        with ViewportSettings(area):
            space = area.spaces[0]
            space.shading.type = 'RENDERED'
            # 渲染预览
```

## 工厂模式

### 对象工厂

```python
# 简单工厂
class ObjectFactory:
    @staticmethod
    def create_mesh(name, vertices, faces):
        mesh = bpy.data.meshes.new(name)
        mesh.from_pydata(vertices, [], faces)
        mesh.update()
        return mesh
    
    @staticmethod
    def create_cube(name, size=1.0, location=(0, 0, 0)):
        bpy.ops.mesh.primitive_cube_add(size=size, location=location)
        obj = bpy.context.active_object
        obj.name = name
        return obj
    
    @staticmethod
    def create_sphere(name, radius=1.0, segments=32, ring_count=16):
        bpy.ops.mesh.primitive_uv_sphere_add(
            radius=radius,
            segments=segments,
            ring_count=ring_count
        )
        obj = bpy.context.active_object
        obj.name = name
        return obj
    
    @staticmethod
    def create_cylinder(name, radius=1.0, depth=2.0, vertices=32):
        bpy.ops.mesh.primitive_cylinder_add(
            radius=radius,
            depth=depth,
            vertices=vertices
        )
        obj = bpy.context.active_object
        obj.name = name
        return obj

# 使用工厂
factory = ObjectFactory()
cube = factory.create_cube("MyCube", size=2.0, location=(0, 0, 0))
sphere = factory.create_sphere("MySphere", radius=1.5)
```

### 材质工厂

```python
# 材质工厂
class MaterialFactory:
    @staticmethod
    def create_basic_material(name, color=(0.8, 0.8, 0.8, 1.0)):
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = False
        mat.diffuse_color = color
        return mat
    
    @staticmethod
    def create_pbr_material(name, base_color, metallic, roughness):
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        
        # 设置基础属性
        mat.node_tree.nodes["Principled BSDF"].inputs["Base Color"].default_value = base_color
        mat.node_tree.nodes["Principled BSDF"].inputs["Metallic"].default_value = metallic
        mat.node_tree.nodes["Principled BSDF"].inputs["Roughness"].default_value = roughness
        
        return mat
    
    @staticmethod
    def create_emission_material(name, color, strength=1.0):
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        mat.node_tree.nodes.clear()
        
        # 创建发光节点
        output = mat.node_tree.nodes.new('ShaderNodeOutputMaterial')
        emission = mat.node_tree.nodes.new('ShaderNodeEmission')
        
        emission.inputs["Color"].default_value = color
        emission.inputs["Strength"].default_value = strength
        
        mat.node_tree.links.new(emission.outputs["Emission"], output.inputs["Surface"])
        
        return mat

# 使用材质工厂
mat_factory = MaterialFactory()
red_pbr = mat_factory.create_pbr_material(
    "RedMetal",
    base_color=(1.0, 0.0, 0.0, 1.0),
    metallic=0.9,
    roughness=0.2
)
```

### 运算符工厂

```python
# 运算符工厂
class OperatorFactory:
    @staticmethod
    def create_transform_operator(name, target_property, value):
        class DynamicOperator(bpy.types.Operator):
            bl_idname = f"object.transform_{name}"
            bl_label = f"设置{value}"
            bl_options = {'REGISTER', 'UNDO'}
            
            def execute(self, context):
                setattr(context.active_object, target_property, value)
                return {'FINISHED'}
        
        return DynamicOperator
    
    @staticmethod
    def create_select_operator(name, object_names):
        class SelectOperator(bpy.types.Operator):
            bl_idname = f"object.select_{name}"
            bl_label = f"选择{name}"
            bl_options = {'REGISTER'}
            
            def execute(self, context):
                bpy.ops.object.select_all(action='DESELECT')
                for obj_name in object_names:
                    obj = bpy.data.objects.get(obj_name)
                    if obj:
                        obj.select_set(True)
                return {'FINISHED'}
        
        return SelectOperator

# 使用运算符工厂
MoveUp = OperatorFactory.create_transform_operator(
    "move_up", "location", (0, 0, 1)
)
bpy.utils.register_class(MoveUp)
```

## 单例模式

### Blender单例

```python
# Blender数据访问单例
class SceneManager:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance._initialized = False
        return cls._instance
    
    def __init__(self):
        if self._initialized:
            return
        self._initialized = True
        self.cache = {}
    
    def get_scene(self):
        return bpy.context.scene
    
    def get_selected_objects(self):
        return bpy.context.selected_objects
    
    def get_active_object(self):
        return bpy.context.active_object
    
    def clear_selection(self):
        bpy.ops.object.select_all(action='DESELECT')
    
    def select_objects(self, objects):
        self.clear_selection()
        for obj in objects:
            obj.select_set(True)

# 使用单例
manager = SceneManager()
scene = manager.get_scene()
```

### 注册表单例

```python
# 注册表单例
class Registry:
    _instance = None
    _classes = {}
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
    
    def register(self, cls):
        self._classes[cls.bl_idname] = cls
        return cls
    
    def unregister(self, bl_idname):
        if bl_idname in self._classes:
            del self._classes[bl_idname]
    
    def get(self, bl_idname):
        return self._classes.get(bl_idname)
    
    def get_all(self):
        return list(self._classes.values())

# 使用注册表
registry = Registry()

@registry.register
class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "我的运算符"

# 批量注册
def register_all():
    for cls in registry.get_all():
        try:
            bpy.utils.register_class(cls)
        except:
            print(f"无法注册: {cls.bl_idname}")

def unregister_all():
    for cls in registry.get_all():
        try:
            bpy.utils.unregister_class(cls)
        except:
            print(f"无法注销: {cls.bl_idname}")
```

## 观察者模式

### 事件观察者

```python
# 观察者基类
class Observer:
    def update(self, subject, *args, **kwargs):
        pass

class Subject:
    def __init__(self):
        self._observers = []
    
    def attach(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)
    
    def detach(self, observer):
        self._observers.remove(observer)
    
    def notify(self, *args, **kwargs):
        for observer in self._observers:
            observer.update(self, *args, **kwargs)

# Blender事件观察者
class BlenderEventSubject(Subject):
    def __init__(self):
        super().__init__()
        self._frame_cache = {}
    
    def check_frame_change(self, scene):
        current_frame = scene.frame_current
        if id(scene) not in self._frame_cache:
            self._frame_cache[id(scene)] = current_frame
            return
        
        if self._frame_cache[id(scene)] != current_frame:
            self._frame_cache[id(scene)] = current_frame
            self.notify('frame_change', frame=current_frame)

class FrameChangeObserver(Observer):
    def update(self, subject, event, **kwargs):
        if event == 'frame_change':
            print(f"帧已更改: {kwargs['frame']}")

# 设置观察者
subject = BlenderEventSubject()
observer = FrameChangeObserver()
subject.attach(observer)

# 在帧更改处理程序中使用
bpy.app.handlers.frame_change_post.append(
    lambda scene: subject.check_frame_change(scene)
)
```

### 属性观察者

```python
# 属性更新观察者
class PropertyObserver:
    def __init__(self):
        self._callbacks = {}
    
    def add_callback(self, obj, prop_name, callback):
        key = (id(obj), prop_name)
        if key not in self._callbacks:
            self._callbacks[key] = []
        self._callbacks[key].append(callback)
    
    def remove_callback(self, obj, prop_name, callback):
        key = (id(obj), prop_name)
        if key in self._callbacks:
            self._callbacks[key].remove(callback)
    
    def on_property_change(self, self_ptr, context):
        # 这个方法会在属性更新时调用
        key = (id(self_ptr), 'my_property')
        if key in self._callbacks:
            for callback in self._callbacks[key]:
                callback(getattr(self_ptr, 'my_property'))

# 使用属性观察者
class ObservableScene(bpy.types.PropertyGroup):
    my_property: bpy.props.FloatProperty(
        name="属性",
        update=lambda self, context: self.on_change(self, context)
    )
    
    def on_change(self, self_ptr, context):
        print(f"属性已更新: {self_ptr.my_property}")
```

### 场景观察者

```python
# 场景变化观察者
class SceneObserver:
    def __init__(self):
        self._handlers = []
    
    def add_handler(self, event_type, handler):
        self._handlers.append((event_type, handler))
    
    def setup(self):
        bpy.app.handlers.depsgraph_update_post.append(self.on_update)
        bpy.app.handlers.frame_change_post.append(self.on_frame_change)
        bpy.app.handlers.load_post.append(self.on_load)
    
    def on_update(self, depsgraph):
        for event_type, handler in self._handlers:
            if event_type == 'update':
                handler(depsgraph)
    
    def on_frame_change(self, scene):
        for event_type, handler in self._handlers:
            if event_type == 'frame_change':
                handler(scene)
    
    def on_load(self, context):
        for event_type, handler in self._handlers:
            if event_type == 'load':
                handler(context)

# 使用场景观察者
observer = SceneObserver()
observer.add_handler('update', lambda d: print("场景已更新"))
observer.add_handler('frame_change', lambda s: print(f"帧: {s.frame_current}"))
observer.setup()
```

## 迭代器和生成器

### 自定义迭代器

```python
# 迭代器类
class ObjectIterator:
    def __init__(self, scene):
        self.scene = scene
        self._index = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self._index < len(self.scene.objects):
            obj = self.scene.objects[self._index]
            self._index += 1
            return obj
        raise StopIteration

# 使用迭代器
for obj in ObjectIterator(bpy.context.scene):
    print(f"对象: {obj.name}")

# 生成器函数
def mesh_objects_generator(scene):
    for obj in scene.objects:
        if obj.type == 'MESH':
            yield obj

# 使用生成器
for obj in mesh_objects_generator(bpy.context.scene):
    print(f"网格对象: {obj.name}")

# 复杂生成器
def keyframe_generator(obj, frame_range=None):
    if frame_range is None:
        frame_range = range(
            bpy.context.scene.frame_start,
            bpy.context.scene.frame_end + 1
        )
    
    for frame in frame_range:
        bpy.context.scene.frame_set(frame)
        yield (frame, obj.location.copy())

# 使用键帧生成器
for frame, location in keyframe_generator(bpy.context.active_object):
    print(f"帧 {frame}: 位置 {location}")
```

### 惰性计算

```python
# 惰性属性
class LazyProperty:
    def __init__(self, func):
        self.func = func
        self.attr_name = f"_lazy_{func.__name__}"
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
        if not hasattr(instance, self.attr_name):
            setattr(instance, self.attr_name, self.func(instance))
        return getattr(instance, self.attr_name)

class ExpensiveData:
    @LazyProperty
    def complex_calculation(self):
        result = 0
        for i in range(1000000):
            result += i
        return result

# 使用惰性属性
data = ExpensiveData()
print(data.complex_calculation)  # 只计算一次

# 惰性导入
class LazyModule:
    def __init__(self, module_name):
        self._module_name = module_name
        self._module = None
    
    def __getattr__(self, name):
        if self._module is None:
            self._module = __import__(self._module_name)
        return getattr(self._module, name)

# 使用惰性导入
numpy = LazyModule("numpy")
arr = numpy.array([1, 2, 3])  # 实际导入numpy
```

## 描述符

### 属性描述符

```python
# 验证描述符
class RangeValidator:
    def __init__(self, min_val, max_val):
        self.min_val = min_val
        self.max_val = max_val
        self.attr_name = None
    
    def __set_name__(self, owner, name):
        self.attr_name = f"_{name}"
    
    def __get__(self, instance, owner):
        return getattr(instance, self.attr_name)
    
    def __set__(self, instance, value):
        if not (self.min_val <= value <= self.max_val):
            raise ValueError(f"值必须在 {self.min_val} 和 {self.max_val} 之间")
        setattr(instance, self.attr_name, value)

# 使用验证描述符
class TransformSettings:
    scale = RangeValidator(0.01, 100.0)
    rotation = RangeValidator(-360, 360)
    
    def __init__(self):
        self._scale = 1.0
        self._rotation = 0.0

# 缓存描述符
class CachedProperty:
    def __init__(self, func):
        self.func = func
        self.attr_name = f"_cache_{func.__name__}"
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
        if not hasattr(instance, self.attr_name):
            setattr(instance, self.attr_name, self.func(instance))
        return getattr(instance, self.attr_name)

# 使用缓存描述符
class MeshAnalyzer:
    @CachedProperty
    def vertex_count(self):
        obj = bpy.context.active_object
        if obj and obj.type == 'MESH':
            return len(obj.data.vertices)
        return 0
    
    @CachedProperty
    def face_count(self):
        obj = bpy.context.active_object
        if obj and obj.type == 'MESH':
            return len(obj.data.polygons)
        return 0
```

### Blender描述符

```python
# Blender属性描述符
class BlenderProperty:
    def __init__(self, prop_type, **kwargs):
        self.prop_type = prop_type
        self.kwargs = kwargs
        self.attr_name = None
    
    def __set_name__(self, owner, name):
        self.attr_name = f"_{name}"
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
        return getattr(instance, self.attr_name)
    
    def __set__(self, instance, value):
        setattr(instance, self.attr_name, value)

# 事件描述符
class EventProperty:
    def __init__(self, event_name):
        self.event_name = event_name
        self.attr_name = None
    
    def __set_name__(self, owner, name):
        self.attr_name = f"_{name}"
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
        return getattr(instance, self.attr_name)
    
    def __set__(self, instance, value):
        setattr(instance, self.attr_name, value)
        # 触发事件
        if hasattr(instance, '_event_system'):
            instance._event_system.notify(self.event_name, value)
```

## 最佳实践

### 代码组织

```python
# 模块化代码结构
"""
my_addon/
    __init__.py
    operators/
        __init__.py
        transform_ops.py
        mesh_ops.py
    panels/
        __init__.py
        main_panel.py
    utils/
        __init__.py
        math_utils.py
        validation.py
"""

# __init__.py示例
from .operators import register as register_ops
from .panels import register as register_panels
from .utils import register as register_utils

def register():
    register_ops()
    register_panels()
    register_utils()

def unregister():
    pass
```

### 命名约定

```python
# Blender命名约定
# 类名: PascalCase (例如 MyOperator, CustomPanel)
# 函数/方法: snake_case (例如 my_function, calculate_transform)
# 常量: UPPER_SNAKE_CASE (例如 MAX_VERTS, DEFAULT_SCALE)
# 私有成员: _prefix (例如 _private_method, _cache_data)
# 模块级变量: snake_case (例如 my_variable, scene_data)

# ID命名约定
# 运算符: object.my_operator
# 面板: OBJECT_PT_my_panel
# 菜单: OBJECT_MT_my_menu
# 属性: scene.my_property

# 避免的名称
# 不要使用Python保留字
# 不要使用Blender内部名称
# 不要使用过长的ID名
```

### 性能优化

```python
# 避免不必要的重复计算
class OptimizedCalculator:
    def __init__(self):
        self._cache = {}
    
    def expensive_operation(self, key):
        if key in self._cache:
            return self._cache[key]
        
        # 昂贵计算
        result = sum(range(1000))
        self._cache[key] = result
        return result

# 批量操作
def batch_operation(objects, operation):
    """批量执行操作的优化版本"""
    # 保存当前选择
    previous_selection = bpy.context.selected_objects.copy()
    previous_active = bpy.context.active_object
    
    # 清除选择
    bpy.ops.object.select_all(action='DESELECT')
    
    # 选择所有对象
    for obj in objects:
        obj.select_set(True)
    bpy.context.view_layer.objects.active = objects[0] if objects else None
    
    # 执行操作
    operation()
    
    # 恢复选择
    bpy.ops.object.select_all(action='DESELECT')
    for obj in previous_selection:
        obj.select_set(True)
    if previous_active:
        bpy.context.view_layer.objects.active = previous_active
```

### 错误处理

```python
# 健壮的错误处理
class SafeOperator(bpy.types.Operator):
    bl_idname = "object.safe_operator"
    bl_label = "安全运算符"
    bl_options = {'REGISTER', 'UNDO'}
    
    def execute(self, context):
        try:
            # 危险操作
            result = self.safe_operation()
            return {'FINISHED'}
        except ValueError as e:
            self.report({'ERROR'}, str(e))
            return {'CANCELLED'}
        except Exception as e:
            self.report({'ERROR'}, f"未知错误: {e}")
            return {'CANCELLED'}
    
    def safe_operation(self):
        # 实现安全操作
        obj = context.active_object
        if not obj:
            raise ValueError("没有活动对象")
        
        if obj.type != 'MESH':
            raise ValueError("对象必须是网格")
        
        return True

# 验证器
def validate_selection(required_type=None):
    def decorator(func):
        def wrapper(self, context):
            obj = context.active_object
            if not obj:
                self.report({'ERROR'}, "没有活动对象")
                return {'CANCELLED'}
            
            if required_type and obj.type != required_type:
                self.report({'ERROR'}, f"对象必须是{required_type}类型")
                return {'CANCELLED'}
            
            return func(self, context)
        return wrapper
    return decorator

# 使用验证器
@validate_selection(required_type='MESH')
class ValidatedOperator(bpy.types.Operator):
    bl_idname = "object.validated_operator"
    bl_label = "已验证运算符"
    
    def execute(self, context):
        print("操作成功")
        return {'FINISHED'}
```


---
## bpy_api_advanced

# Blender Python API高级模块

## API高级概述

Blender Python API（bpy）提供了丰富的功能用于控制Blender的各个方面。掌握高级API用法对于创建复杂的插件和自动化脚本至关重要。本模块涵盖了数据访问、操作符调用、上下文管理、性能优化等高级主题，以及与其他Python模块的集成技巧。

```python
# API核心模块
# bpy.data - 访问所有数据块
# bpy.context - 当前操作上下文
# bpy.ops - 调用操作符
# bpy.props - 定义属性
# bpy.types - Blender类型定义
# bpy.utils - 工具函数
# bpy.app - 应用程序级功能

# 高级主题
# 数据访问优化
# 操作符链式调用
# 上下文管理
# 事件处理
# 内存管理
# 多线程注意事项
```

## 数据块操作

### 集合管理

```python
import bpy
from bpy.props import PointerProperty, CollectionProperty

def get_or_create_collection(name, parent=None):
    """获取或创建集合"""
    collection = bpy.data.collections.get(name)
    if not collection:
        collection = bpy.data.collections.new(name)
        if parent:
            parent.children.link(collection)
        else:
            bpy.context.scene.collection.children.link(collection)
    return collection

def link_object_to_collection(obj, collection):
    """将对象链接到集合"""
    if obj.name not in collection.objects:
        collection.objects.link(obj)
    
    # 从其他集合中移除
    for coll in obj.users_collection:
        if coll != collection:
            coll.objects.unlink(obj)

def move_object_to_collection(obj, target_collection):
    """移动对象到目标集合"""
    link_object_to_collection(obj, target_collection)

def duplicate_object_with_collection(obj, collection=None):
    """复制对象并链接到指定集合"""
    new_obj = obj.copy()
    new_obj.data = obj.data.copy() if obj.data else None
    
    target = collection or obj.users_collection[0] if obj.users_collection else bpy.context.scene.collection
    target.objects.link(new_obj)
    
    return new_obj

def get_all_objects_in_collections(collections=None):
    """获取集合中的所有对象"""
    if collections is None:
        collections = bpy.data.collections
    
    objects = []
    for coll in collections:
        objects.extend(coll.objects)
    return objects
```

### 材质访问

```python
def get_material_usage(material):
    """获取材质使用统计"""
    users = material.users
    objects_using = []
    
    for obj in bpy.data.objects:
        if obj.data and hasattr(obj.data, 'materials'):
            for mat in obj.data.materials:
                if mat == material:
                    objects_using.append(obj)
                    break
    
    return {
        'user_count': users,
        'objects': objects_using
    }

def reassign_material(old_material, new_material):
    """重新分配材质"""
    count = 0
    for obj in bpy.data.objects:
        if obj.data and hasattr(obj.data, 'materials'):
            for i, mat in enumerate(obj.data.materials):
                if mat == old_material:
                    obj.data.materials[i] = new_material
                    count += 1
    return count

def remove_unused_materials():
    """移除未使用的材质"""
    removed = []
    for mat in list(bpy.data.materials):
        if mat.users == 0:
            bpy.data.materials.remove(mat)
            removed.append(mat.name)
    return removed

def create_material_override(obj, base_material):
    """为对象创建材质覆盖"""
    if not obj.data.materials:
        obj.data.materials.append(base_material)
    
    override_mat = base_material.copy()
    override_mat.name = f"{obj.name}_{base_material.name}"
    obj.data.materials[0] = override_mat
    
    return override_mat
```

### 场景管理

```python
def get_scene_hierarchy(scene):
    """获取场景层级结构"""
    hierarchy = {
        'name': scene.name,
        'objects': [],
        'collections': []
    }
    
    for obj in scene.objects:
        hierarchy['objects'].append({
            'name': obj.name,
            'type': obj.type,
            'parent': obj.parent.name if obj.parent else None
        })
    
    for coll in scene.collection.children:
        hierarchy['collections'].append({
            'name': coll.name,
            'object_count': len(coll.objects)
        })
    
    return hierarchy

def duplicate_scene(source_scene, new_name):
    """复制场景"""
    new_scene = source_scene.copy()
    new_scene.name = new_name
    
    # 复制所有对象
    object_mapping = {}
    for obj in source_scene.objects:
        new_obj = obj.copy()
        new_obj.data = obj.data.copy() if obj.data else None
        new_scene.collection.objects.link(new_obj)
        object_mapping[obj.name] = new_obj
    
    # 更新父引用
    for obj in new_scene.objects:
        if obj.parent and obj.parent.name in object_mapping:
            obj.parent = object_mapping[obj.parent.name]
    
    bpy.context.scene.collection.children.link(new_scene.collection)
    
    return new_scene

def link_collection_to_scene(collection, scene):
    """将集合链接到场景"""
    if collection.name not in scene.collection.children:
        scene.collection.children.link(collection)

def unlink_collection_from_scene(collection, scene):
    """从场景取消链接集合"""
    if collection.name in scene.collection.children:
        scene.collection.children.unlink(collection)
```

## 操作符调用

### 高级操作符调用

```python
def call_operator_with_overrides(op_idname, override_dict, **kwargs):
    """使用属性覆盖调用操作符"""
    with bpy.context.temp_override(**override_dict):
        bpy.ops.object.select_all(action='SELECT')
    
    return bpy.ops.object.select_all(action='DESELECT')

def safe_operator_call(op_idname, **kwargs):
    """安全调用操作符，返回结果和状态"""
    try:
        result = getattr(bpy.ops, op_idname)(**kwargs)
        return result, True
    except RuntimeError as e:
        print(f"操作符调用失败: {op_idname}, 错误: {e}")
        return str(e), False

def batch_operator_call(op_idname, objects, **kwargs):
    """批量调用操作符处理多个对象"""
    results = []
    
    # 保存当前选择
    original_selection = bpy.context.selected_objects.copy()
    original_active = bpy.context.active_object
    
    try:
        bpy.ops.object.select_all(action='DESELECT')
        
        for obj in objects:
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            
            result = safe_operator_call(op_idname, **kwargs)
            results.append((obj.name, result))
            
            obj.select_set(False)
    
    finally:
        # 恢复选择
        bpy.ops.object.select_all(action='DESELECT')
        for obj in original_selection:
            obj.select_set(True)
        if original_active:
            bpy.context.view_layer.objects.active = original_active
    
    return results

def operator_with_undo_group(op_idname, group_name, **kwargs):
    """在撤销组中调用操作符"""
    bpy.ops.ed.undo_push(message=group_name)
    
    try:
        result = getattr(bpy.ops, op_idname)(**kwargs)
        return result
    finally:
        pass
```

### 操作符属性设置

```python
def set_operator_properties(op_idname, properties):
    """设置操作符默认属性"""
    op = getattr(bpy.ops, op_idname)
    
    for key, value in properties.items():
        if hasattr(op.properties, key):
            setattr(op.properties, key, value)

def get_operator_properties(op_idname):
    """获取操作符当前属性"""
    op = getattr(bpy.ops, op_idname)
    props = {}
    
    for key in dir(op.properties):
        if not key.startswith('_'):
            try:
                props[key] = getattr(op.properties, key)
            except:
                pass
    
    return props

def create_operator_preset(op_idname, preset_name, properties):
    """创建操作符预设"""
    # 保存到用户预设
    prefs = bpy.context.preferences
    addon_prefs = prefs.addons.get(__name__.split('.')[0])
    
    if not hasattr(addon_prefs, 'operator_presets'):
        addon_prefs.operator_presets = {}
    
    addon_prefs.operator_presets[preset_name] = {
        'op_idname': op_idname,
        'properties': properties
    }

def apply_operator_preset(preset_name):
    """应用操作符预设"""
    prefs = bpy.context.preferences
    addon_prefs = prefs.addons.get(__name__.split('.')[0])
    
    if not hasattr(addon_prefs, 'operator_presets'):
        return False
    
    preset = addon_prefs.operator_presets.get(preset_name)
    if not preset:
        return False
    
    set_operator_properties(preset['op_idname'], preset['properties'])
    return True
```

## 上下文管理

### 高级上下文操作

```python
import contextlib

@contextlib.contextmanager
def temp_override_context(context_dict):
    """临时覆盖上下文"""
    original_context = {}
    for key, value in context_dict.items():
        original_context[key] = getattr(bpy.context, key, None)
        setattr(bpy.context, key, value)
    
    try:
        yield
    finally:
        for key, value in original_context.items():
            if value is not None:
                setattr(bpy.context, key, value)

@contextlib.contextmanager
def temp_scene(scene):
    """临时切换场景"""
    original_scene = bpy.context.scene
    bpy.context.window.scene = scene
    
    try:
        yield
    finally:
        bpy.context.window.scene = original_scene

@contextlib.contextmanager
def temp_selection(objects):
    """临时选择集"""
    original_selection = bpy.context.selected_objects.copy()
    original_active = bpy.context.active_object
    
    bpy.ops.object.select_all(action='DESELECT')
    for obj in objects:
        obj.select_set(True)
    if objects:
        bpy.context.view_layer.objects.active = objects[0]
    
    try:
        yield
    finally:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in original_selection:
            obj.select_set(True)
        if original_active:
            bpy.context.view_layer.objects.active = original_active

@contextlib.contextmanager
def temp_mode(mode):
    """临时切换模式"""
    obj = bpy.context.active_object
    original_mode = obj.mode if obj else 'OBJECT'
    
    if obj:
        bpy.ops.object.mode_set(mode=mode)
    
    try:
        yield
    finally:
        if obj:
            bpy.ops.object.mode_set(mode=original_mode)

@contextlib.contextmanager
def temp_view3d_shading(shading_type):
    """临时切换3D视图着色"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces[0]
            original_shading = space.shading.type
            space.shading.type = shading_type
    
    try:
        yield
    finally:
        for area in bpy.context.screen.areas:
            if area.type == 'VIEW_3D':
                space = area.spaces[0]
                space.shading.type = original_shading
```

### 窗口管理

```python
def get_all_windows():
    """获取所有窗口"""
    return list(bpy.context.window_manager.windows)

def get_all_screens():
    """获取所有屏幕"""
    return list(bpy.data.screens)

def create_window(screen_name=None):
    """创建新窗口"""
    screen = bpy.data.screens.get(screen_name)
    if not screen:
        screen = bpy.data.screens.new(screen_name or "New Screen")
    
    window = bpy.context.window_manager.windows.new(screen=screen)
    return window

def close_window(window):
    """关闭窗口"""
    bpy.context.window_manager.windows.remove(window)

def get_area_by_type(area_type):
    """按类型获取区域"""
    for area in bpy.context.screen.areas:
        if area.type == area_type:
            return area
    return None

def get_space_by_type(space_type):
    """按类型获取空间"""
    for area in bpy.context.screen.areas:
        for space in area.spaces:
            if space.type == space_type:
                return space
    return None

def split_area(area, direction='VERTICAL', factor=0.5):
    """分割区域"""
    bpy.ops.screen.area_split(direction=direction, factor=factor)
    
    # 返回新区域
    for new_area in bpy.context.screen.areas:
        if new_area != area:
            return new_area
    return None

def join_areas(area1, area2):
    """合并区域"""
    bpy.context.window.screen.areas.active = area1
    bpy.ops.screen.area_join(min_x=area1.x, min_y=area1.y, max_x=area2.x, max_y=area2.y)
```

## 数据遍历

### 高级遍历技巧

```python
def traverse_hierarchy(root, include_self=False):
    """遍历对象层级结构"""
    if include_self:
        yield root
    
    for child in root.children:
        yield from traverse_hierarchy(child, include_self=True)

def traverse_data_blocks(data_type):
    """遍历指定类型的所有数据块"""
    data_collection = getattr(bpy.data, data_type)
    for item in data_collection:
        yield item

def find_data_block_by_name(data_type, name):
    """按名称查找数据块"""
    data_collection = getattr(bpy.data, data_type)
    return data_collection.get(name)

def find_objects_by_type(object_type):
    """按类型查找对象"""
    return [obj for obj in bpy.data.objects if obj.type == object_type]

def find_objects_by_name_pattern(pattern):
    """按名称模式查找对象"""
    import re
    regex = re.compile(pattern)
    return [obj for obj in bpy.data.objects if regex.match(obj.name)]

def find_objects_with_data(data_type):
    """查找包含指定类型数据的对象"""
    objects = []
    for obj in bpy.data.objects:
        if obj.data and isinstance(obj.data, data_type):
            objects.append(obj)
    return objects

def get_orphan_data_blocks():
    """获取孤儿数据块（未被使用的数据）"""
    orphans = []
    
    for collection in bpy.data.collections:
        if not collection.users:
            orphans.append(('collection', collection))
    
    for mesh in bpy.data.meshes:
        if not mesh.users:
            orphans.append(('mesh', mesh))
    
    for material in bpy.data.materials:
        if not material.users:
            orphans.append(('material', material))
    
    for obj in bpy.data.objects:
        if not obj.users:
            orphans.append(('object', obj))
    
    return orphans
```

### 批量数据操作

```python
def batch_rename(data_type, old_name, new_name):
    """批量重命名"""
    count = 0
    data_collection = getattr(bpy.data, data_type)
    
    for item in data_collection:
        if old_name in item.name:
            item.name = item.name.replace(old_name, new_name)
            count += 1
    
    return count

def batch_delete_unused(data_type):
    """批量删除未使用的数据"""
    data_collection = getattr(bpy.data, data_type)
    deleted = []
    
    for item in list(data_collection):
        if item.users == 0:
            data_collection.remove(item)
            deleted.append(item.name)
    
    return deleted

def batch_select_by_property(property_name, property_value):
    """按属性值批量选择"""
    selected = []
    
    for obj in bpy.data.objects:
        if obj.get(property_name) == property_value:
            obj.select_set(True)
            selected.append(obj.name)
    
    return selected

def batch_apply_transform():
    """批量应用变换到所有选中对象"""
    for obj in bpy.context.selected_objects:
        bpy.context.view_layer.objects.active = obj
        bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
```

## 性能优化

### 优化数据访问

```python
def cache_data_access(data_getter, cache_key):
    """缓存数据访问结果"""
    if not hasattr(cache_data_access, 'cache'):
        cache_data_access.cache = {}
    
    if cache_key not in cache_data_access.cache:
        cache_data_access.cache[cache_key] = data_getter()
    
    return cache_data_access.cache[cache_key]

def clear_cache():
    """清除缓存"""
    if hasattr(cache_data_access, 'cache'):
        cache_data_access.cache.clear()

def optimize_batch_creation(object_count, creator_func):
    """优化批量创建"""
    # 禁用实时更新
    bpy.context.scene.tool_settings.use_sculpt_session = False
    
    objects = []
    for i in range(object_count):
        obj = creator_func(i)
        objects.append(obj)
    
    return objects

def disable_redraw():
    """禁用重绘"""
    for area in bpy.context.screen.areas:
        area.tag_redraw = False

def enable_redraw():
    """启用重绘"""
    for area in bpy.context.screen.areas:
        area.tag_redraw = True

@contextlib.contextmanager
def no_redraw():
    """临时禁用重绘"""
    disable_redraw()
    try:
        yield
    finally:
        enable_redraw()
```

### 内存管理

```python
def get_memory_usage():
    """获取内存使用情况"""
    import psutil
    process = psutil.Process()
    return {
        'memory_mb': process.memory_info().rss / 1024 / 1024,
        'memory_percent': process.memory_percent()
    }

def cleanup_unused_data():
    """清理未使用的数据"""
    removed = {
        'images': 0,
        'materials': 0,
        'meshes': 0,
        'textures': 0
    }
    
    for img in list(bpy.data.images):
        if img.users == 0:
            bpy.data.images.remove(img)
            removed['images'] += 1
    
    for mat in list(bpy.data.materials):
        if mat.users == 0:
            bpy.data.materials.remove(mat)
            removed['materials'] += 1
    
    for mesh in list(bpy.data.meshes):
        if mesh.users == 0:
            bpy.data.meshes.remove(mesh)
            removed['meshes'] += 1
    
    return removed

def force_garbage_collection():
    """强制垃圾回收"""
    import gc
    gc.collect()

def efficient_object_iteration():
    """高效的对象迭代"""
    scene = bpy.context.scene
    
    # 使用直接遍历而不是过滤
    for obj in scene.objects:
        yield obj
    
    # 或者使用列表推导
    meshes = [obj for obj in scene.objects if obj.type == 'MESH']
```

### 延迟计算

```python
class LazyProperty:
    """延迟计算属性"""
    
    def __init__(self, func):
        self.func = func
        self.attr_name = f'_lazy_{func.__name__}'
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
        if not hasattr(instance, self.attr_name):
            setattr(instance, self.attr_name, self.func(instance))
        return getattr(instance, self.attr_name)

class DeferredCalculator:
    """延迟计算器"""
    
    def __init__(self):
        self._pending = {}
        self._timer = None
    
    def schedule(self, key, calculation_func, delay=0.1):
        self._pending[key] = calculation_func
        
        if self._timer is None:
            self._timer = bpy.app.timers.register(self._execute_pending, first_interval=delay)
    
    def _execute_pending(self):
        for key, func in self._pending.items():
            func()
        self._pending.clear()
        self._timer = None
        return None
    
    def cancel(self, key):
        if key in self._pending:
            del self._pending[key]
```

## 类型检查与验证

### 运行时类型检查

```python
def is_object_type(obj, object_type):
    """检查对象类型"""
    return obj and obj.type == object_type

def is_mesh_object(obj):
    """检查是否为网格对象"""
    return is_object_type(obj, 'MESH')

def is_armature_object(obj):
    """检查是否为骨架对象"""
    return is_object_type(obj, 'ARMATURE')

def is_light_object(obj):
    """检查是否为灯光对象"""
    return is_object_type(obj, 'LIGHT')

def is_camera_object(obj):
    """检查是否为相机对象"""
    return is_object_type(obj, 'CAMERA')

def has_animation_data(obj):
    """检查是否有动画数据"""
    return obj and obj.animation_data is not None

def has_shape_keys(obj):
    """检查是否有形状键"""
    return obj and obj.data and hasattr(obj.data, 'shape_keys') and obj.data.shape_keys

def has_modifier(obj, modifier_type):
    """检查是否有指定类型的修饰符"""
    return any(isinstance(mod, modifier_type) for mod in obj.modifiers)
```

### 属性验证

```python
def validate_property_value(value, prop_type, constraints=None):
    """验证属性值"""
    if constraints is None:
        constraints = {}
    
    # 类型检查
    if 'min' in constraints and value < constraints['min']:
        return False, f"值小于最小值 {constraints['min']}"
    
    if 'max' in constraints and value > constraints['max']:
        return False, f"值大于最大值 {constraints['max']}"
    
    if 'step' in constraints and (value % constraints['step']) != 0:
        return False, f"值不匹配步长 {constraints['step']}"
    
    if 'enum_items' in constraints:
        valid_values = [item[0] for item in constraints['enum_items']]
        if value not in valid_values:
            return False, f"无效的枚举值"
    
    return True, "有效"

def get_property_info(obj, prop_path):
    """获取属性信息"""
    parts = prop_path.split('.')
    current = obj
    
    for part in parts:
        if current is None:
            return None
        
        if '[' in part:
            # 数组访问
            prop_name = part.split('[')[0]
            index = int(part.split('[')[1].rstrip(']'))
            current = getattr(current, prop_name)[index]
        else:
            current = getattr(current, part)
    
    return current
```

## 数学运算

### 向量运算

```python
from mathutils import Vector, Matrix, Euler, Quaternion

def get_object_position(obj, space='WORLD'):
    """获取对象位置"""
    if space == 'WORLD':
        return obj.location.copy()
    elif space == 'LOCAL':
        return obj.matrix_local.to_translation()
    elif space == 'PARENT':
        return obj.matrix_parent_inverse.to_translation()

def set_object_position(obj, position, space='WORLD'):
    """设置对象位置"""
    if space == 'WORLD':
        obj.location = position
    elif space == 'LOCAL':
        obj.matrix_local.translation = position

def transform_point(point, matrix, direction='TO_LOCAL'):
    """变换点"""
    if direction == 'TO_LOCAL':
        return matrix.inverted() @ point
    elif direction == 'TO_WORLD':
        return matrix @ point

def get_distance(obj1, obj2):
    """计算两点间距离"""
    return (obj1.location - obj2.location).length

def get_angle_between_vectors(v1, v2):
    """计算向量间角度"""
    dot = v1.dot(v2)
    mag1 = v1.length
    mag2 = v2.length
    return math.acos(dot / (mag1 * mag2))

def create_rotation_matrix(axis, angle):
    """创建旋转矩阵"""
    rot_mat = Matrix.Rotation(angle, 4, axis)
    return rot_mat
```

### 矩阵运算

```python
def get_object_matrix(obj, space='WORLD'):
    """获取对象矩阵"""
    if space == 'WORLD':
        return obj.matrix_world.copy()
    elif space == 'LOCAL':
        return obj.matrix_local.copy()
    elif space == 'PARENT':
        return obj.matrix_parent_inverse.copy()

def set_object_matrix(obj, matrix, space='WORLD'):
    """设置对象矩阵"""
    if space == 'WORLD':
        obj.matrix_world = matrix
    elif space == 'LOCAL':
        obj.matrix_local = matrix

def decompose_matrix(matrix):
    """分解矩阵为位置、旋转、缩放"""
    return matrix.decompose()

def create_transform_matrix(location, rotation, scale):
    """创建变换矩阵"""
    mat_loc = Matrix.Translation(location)
    mat_rot = rotation.to_matrix().to_4x4()
    mat_scale = Matrix.Diagonal(scale).to_4x4()
    
    return mat_loc @ mat_rot @ mat_scale

def apply_transform_to_vertices(obj, transform_matrix):
    """将变换应用到顶点"""
    mesh = obj.data
    
    for vertex in mesh.vertices:
        # 世界坐标
        world_co = obj.matrix_world @ vertex.co
        # 应用变换
        new_co = transform_matrix @ world_co
        # 回到本地坐标
        vertex.co = obj.matrix_world.inverted() @ new_co
```

## 事件与回调

### 高级事件处理

```python
class EventHandler:
    """事件处理器"""
    
    def __init__(self):
        self._handlers = {}
    
    def register(self, event_type, handler, priority=0):
        if event_type not in self._handlers:
            self._handlers[event_type] = []
        self._handlers[event_type].append((priority, handler))
        self._handlers[event_type].sort(key=lambda x: x[0], reverse=True)
    
    def unregister(self, event_type, handler):
        if event_type in self._handlers:
            self._handlers[event_type] = [
                (p, h) for p, h in self._handlers[event_type]
                if h != handler
            ]
    
    def trigger(self, event_type, *args, **kwargs):
        if event_type in self._handlers:
            for priority, handler in self._handlers[event_type]:
                handler(*args, **kwargs)

# 使用示例
event_handler = EventHandler()

def my_frame_handler(scene):
    print(f"帧: {scene.frame_current}")

event_handler.register('frame_change', my_frame_handler)
```

### 自定义属性更新

```python
class ObservableProperty:
    """可观察属性"""
    
    def __init__(self, prop_name, owner_class):
        self.prop_name = prop_name
        self.owner_class = owner_class
        self._callbacks = []
    
    def register_callback(self, callback):
        self._callbacks.append(callback)
    
    def notify_change(self, obj, old_value, new_value):
        for callback in self._callbacks:
            callback(obj, self.prop_name, old_value, new_value)

def create_observable_property(prop_name):
    """创建可观察属性"""
    def getter(self):
        return getattr(self, f'_{prop_name}')
    
    def setter(self, value):
        old_value = getattr(self, f'_{prop_name}', None)
        setattr(self, f'_{prop_name}', value)
        
        if hasattr(self, f'_observers_{prop_name}'):
            for callback in getattr(self, f'_observers_{prop_name}'):
                callback(self, old_value, value)
    
    return property(getter, setter)

class ObservableMixin:
    """可观察混入类"""
    
    def add_observer(self, prop_name, callback):
        observers_attr = f'_observers_{prop_name}'
        if not hasattr(self, observers_attr):
            setattr(self, observers_attr, [])
        getattr(self, observers_attr).append(callback)
    
    def remove_observer(self, prop_name, callback):
        observers_attr = f'_observers_{prop_name}'
        if hasattr(self, observers_attr):
            observers = getattr(self, observers_attr)
            if callback in observers:
                observers.remove(callback)
```

## 插件开发工具

### 插件工具函数

```python
def get_addon_preferences(addon_name=None):
    """获取插件偏好设置"""
    name = addon_name or __name__.split('.')[0]
    return bpy.context.preferences.addons.get(name)

def get_addon_version(addon_name=None):
    """获取插件版本"""
    prefs = get_addon_preferences(addon_name)
    if prefs and hasattr(prefs, 'bl_info'):
        return prefs.bl_info.get('version', (0, 0, 0))
    return (0, 0, 0)

def is_addon_enabled(addon_name):
    """检查插件是否启用"""
    prefs = bpy.context.preferences.addons.get(addon_name)
    return prefs is not None

def reload_addon(addon_name):
    """重新加载插件"""
    try:
        bpy.ops.preferences.addon_disable(module=addon_name)
        bpy.ops.preferences.addon_enable(module=addon_name)
        return True
    except:
        return False

def get_user_preferences(category='interface'):
    """获取用户偏好设置"""
    prefs = bpy.context.preferences
    return getattr(prefs, category)
```

### 国际化支持

```python
def register_localization():
    """注册本地化"""
    import os
    import bpy
    
    addon_name = __name__.split('.')[0]
    addon_path = os.path.dirname(__file__)
    locale_path = os.path.join(addon_path, 'locales')
    
    # 注册翻译
    bpy.app.translations.register(
        addon_name,
        {
            "My Panel": {"zh_CN": "我的面板"},
            "My Operator": {"zh_CN": "我的操作符"},
            "Settings": {"zh_CN": "设置"},
        }
    )

def unregister_localization():
    """注销本地化"""
    addon_name = __name__.split('.')[0]
    bpy.app.translations.unregister(addon_name)

def translate(text):
    """翻译文本"""
    return bpy.app.translations.pgettext(text)
```

### 调试工具

```python
def print_object_info(obj):
    """打印对象信息"""
    info = {
        'name': obj.name,
        'type': obj.type,
        'location': tuple(obj.location),
        'rotation': tuple(obj.rotation_euler),
        'scale': tuple(obj.scale),
        'matrix_world': [list(row) for row in obj.matrix_world],
    }
    
    print(f"对象信息: {info}")

def log_operation(operation_name, *args, **kwargs):
    """记录操作"""
    print(f"[{operation_name}] {' '.join(map(str, args))} {kwargs}")

def profile_function(func):
    """函数性能分析"""
    import time
    
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - start
        print(f"{func.__name__} 耗时: {elapsed:.4f}秒")
        return result
    
    return wrapper

def dump_context(context):
    """转储上下文"""
    info = {
        'scene': context.scene.name if context.scene else None,
        'active_object': context.active_object.name if context.active_object else None,
        'selected_objects': [obj.name for obj in context.selected_objects],
        'area': context.area.type if context.area else None,
        'space_data': context.space_data.type if context.space_data else None,
    }
    
    print(f"上下文: {info}")
```


---
## bpy_app_handlers

# bpy.app.handlers Module Reference

bpy.app.handlers 模块提供应用程序事件处理程序，用于在特定事件发生时执行自定义代码。

## handlers 概述

handlers 允许脚本响应 Blender 的各种事件：
- 帧更改
- 文件保存/加载
- 渲染开始/结束
- 场景更新
- 撤销/重做

### 何时使用 handlers

- 自动保存和备份
- 数据验证和同步
- 动画更新
- 渲染前/后处理
- 场景变更响应

## 可用处理程序列表

### frame_change_post

在帧更改后执行。

```python
import bpy

def frame_change_handler(scene):
    print(f"Frame changed to: {scene.frame_current}")

# 注册处理程序
bpy.app.handlers.frame_change_post.append(frame_change_handler)

# 注销处理程序
bpy.app.handlers.frame_change_post.remove(frame_change_handler)
```

### frame_change_pre

在帧更改前执行。

```python
import bpy

def frame_change_pre_handler(scene):
    print(f"About to change frame from: {scene.frame_current}")

bpy.app.handlers.frame_change_pre.append(frame_change_pre_handler)
```

### render_pre

在渲染开始前执行。

```python
import bpy

def render_pre_handler(scene):
    print(f"Starting render: {scene.name}")
    print(f"Resolution: {scene.render.resolution_x}x{scene.render.resolution_y}")

bpy.app.handlers.render_pre.append(render_pre_handler)
```

### render_post

在渲染结束后执行。

```python
import bpy

def render_post_handler(scene):
    print(f"Render completed: {scene.name}")

bpy.app.handlers.render_post.append(render_post_handler)
```

### render_cancel

渲染取消时执行。

```python
import bpy

def render_cancel_handler(scene):
    print(f"Render cancelled: {scene.name}")

bpy.app.handlers.render_cancel.append(render_cancel_handler)
```

### save_pre

在保存文件前执行。

```python
import bpy

def save_pre_handler(filepath):
    print(f"Saving file: {filepath}")

bpy.app.handlers.save_pre.append(save_pre_handler)
```

### save_post

在保存文件后执行。

```python
import bpy

def save_post_handler(filepath):
    print(f"File saved: {filepath}")

bpy.app.handlers.save_post.append(save_post_handler)
```

### load_pre

在加载文件前执行。

```python
import bpy

def load_pre_handler(filepath):
    print(f"Loading file: {filepath}")

bpy.app.handlers.load_pre.append(load_pre_handler)
```

### load_post

在加载文件后执行。

```python
import bpy

def load_post_handler(filepath):
    print(f"File loaded: {filepath}")
    # 执行加载后的初始化操作

bpy.app.handlers.load_post.append(load_post_handler)
```

### undo_pre

在撤销前执行。

```python
import bpy

def undo_pre_handler():
    print("About to undo")

bpy.app.handlers.undo_pre.append(undo_pre_handler)
```

### undo_post

在撤销后执行。

```python
import bpy

def undo_post_handler():
    print("Undo completed")

bpy.app.handlers.undo_post.append(undo_post_handler)
```

### redo_pre

在重做前执行。

```python
import bpy

def redo_pre_handler():
    print("About to redo")

bpy.app.handlers.redo_pre.append(redo_pre_handler)
```

### redo_post

在重做后执行。

```python
import bpy

def redo_post_handler():
    print("Redo completed")

bpy.app.handlers.redo_post.append(redo_post_handler)
```

### version_update

当 Blender 版本更新时执行。

```python
import bpy

def version_update_handler():
    current_version = bpy.app.version
    print(f"Blender version: {current_version}")

bpy.app.handlers.version_update.append(version_update_handler)
```

### dependency_update

当依赖项更新时执行。

```python
import bpy

def dependency_update_handler(scene):
    print(f"Dependencies updated in scene: {scene.name}")

bpy.app.handlers.dependency_update.append(dependency_update_handler)
```

### object_base_update

当对象基础数据更新时执行。

```python
import bpy

def object_base_update_handler(scene):
    print(f"Object base updated in scene: {scene.name}")

bpy.app.handlers.object_base_update.append(object_base_update_handler)
```

## 处理程序属性

### frame_change_pre

帧更改前处理程序列表。

```python
import bpy

print(f"Number of frame_change_pre handlers: {len(bpy.app.handlers.frame_change_pre)}")

for handler in bpy.app.handlers.frame_change_pre:
    print(f"Handler: {handler}")
```

### frame_change_post

帧更改后处理程序列表。

```python
import bpy

print(f"Number of frame_change_post handlers: {len(bpy.app.handlers.frame_change_post)}")
```

### render_pre

渲染前处理程序列表。

```python
import bpy

print(f"Number of render_pre handlers: {len(bpy.app.handlers.render_pre)}")
```

### render_post

渲染后处理程序列表。

```python
import bpy

print(f"Number of render_post handlers: {len(bpy.app.handlers.render_post)}")
```

### save_pre

保存前处理程序列表。

```python
import bpy

print(f"Number of save_pre handlers: {len(bpy.app.handlers.save_pre)}")
```

### save_post

保存后处理程序列表。

```python
import bpy

print(f"Number of save_post handlers: {len(bpy.app.handlers.save_post)}")
```

### load_pre

加载前处理程序列表。

```python
import bpy

print(f"Number of load_pre handlers: {len(bpy.app.handlers.load_pre)}")
```

### load_post

加载后处理程序列表。

```python
import bpy

print(f"Number of load_post handlers: {len(bpy.app.handlers.load_post)}")
```

## 完整示例

### 自动保存备份

```python
import bpy
import os
import shutil
from datetime import datetime

backup_dir = "//backups"

def auto_backup(filepath):
    """在保存时自动创建备份"""
    if not filepath:
        return
    
    # 创建备份目录
    backup_path = bpy.path.abspath(backup_dir)
    if not os.path.exists(backup_path):
        os.makedirs(backup_path)
    
    # 生成备份文件名
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    filename = os.path.basename(filepath)
    backup_file = os.path.join(backup_path, f"{timestamp}_{filename}")
    
    # 复制文件
    shutil.copy2(filepath, backup_file)
    print(f"Backup created: {backup_file}")

# 注册处理程序
bpy.app.handlers.save_post.append(auto_backup)
```

### 帧范围同步

```python
import bpy

def sync_frame_range(scene):
    """同步所有场景的帧范围"""
    start_frame = scene.frame_start
    end_frame = scene.frame_end
    
    for other_scene in bpy.data.scenes:
        if other_scene != scene:
            other_scene.frame_start = start_frame
            other_scene.frame_end = end_frame

# 在帧更改时同步
bpy.app.handlers.frame_change_post.append(sync_frame_range)
```

### 渲染统计

```python
import bpy
import time

render_stats = {
    'total_renders': 0,
    'total_time': 0.0,
    'last_render_time': 0.0
}

def start_render_stats(scene):
    render_stats['start_time'] = time.time()

def end_render_stats(scene):
    end_time = time.time()
    duration = end_time - render_stats.get('start_time', end_time)
    render_stats['total_renders'] += 1
    render_stats['total_time'] += duration
    render_stats['last_render_time'] = duration
    
    print(f"Render #{render_stats['total_renders']}: {duration:.2f}s")
    print(f"Total renders: {render_stats['total_renders']}")
    print(f"Average time: {render_stats['total_time'] / render_stats['total_renders']:.2f}s")

bpy.app.handlers.render_pre.append(start_render_stats)
bpy.app.handlers.render_post.append(end_render_stats)
```

### 加载后初始化

```python
import bpy

def setup_scene_on_load(filepath):
    """加载文件后设置场景"""
    scene = bpy.context.scene
    
    # 设置默认渲染设置
    scene.render.engine = 'CYCLES'
    scene.cycles.samples = 128
    
    # 设置时间线范围
    scene.frame_start = 1
    scene.frame_end = 250
    
    print(f"Scene initialized: {scene.name}")

bpy.app.handlers.load_post.append(setup_scene_on_load)
```

### 验证数据完整性

```python
import bpy

def validate_scene(scene):
    """验证场景数据的完整性"""
    issues = []
    
    # 检查对象
    for obj in scene.objects:
        if not obj.data and obj.type not in ('EMPTY', 'CAMERA', 'LIGHT'):
            issues.append(f"Object {obj.name} has no data")
        
        # 检查变换
        if obj.scale.x < 0 or obj.scale.y < 0 or obj.scale.z < 0:
            issues.append(f"Object {obj.name} has negative scale")
    
    # 检查材质
    for mat in bpy.data.materials:
        if not mat.node_tree and not mat.use_nodes:
            issues.append(f"Material {mat.name} has no nodes")
    
    # 报告问题
    if issues:
        print(f"Scene validation issues ({len(issues)}):")
        for issue in issues:
            print(f"  - {issue}")
    else:
        print("Scene validation passed")

# 在保存前验证
def validate_on_save(filepath):
    validate_scene(bpy.context.scene)

bpy.app.handlers.save_pre.append(validate_on_save)
```

### 动画录制

```python
import bpy

class AnimationRecorder:
    def __init__(self):
        self.animations = {}
    
    def record_frame(self, scene):
        frame = scene.frame_current
        
        for obj in scene.objects:
            if obj.animation_data and obj.animation_data.action:
                action = obj.animation_data.action.name
                
                if action not in self.animations:
                    self.animations[action] = {}
                
                self.animations[action][frame] = {
                    'location': obj.location.copy(),
                    'rotation': obj.rotation_euler.copy(),
                    'scale': obj.scale.copy()
                }
    
    def get_animation(self, action_name):
        return self.animations.get(action_name, {})

recorder = AnimationRecorder()

def start_recording(scene):
    global recorder
    recorder = AnimationRecorder()

def record_frame_handler(scene):
    recorder.record_frame(scene)

# 开始录制时初始化
# bpy.app.handlers.frame_change_post.append(record_frame_handler)
```

## 注意事项

### 处理程序执行顺序

- 处理程序按注册顺序执行
- 相同类型的多个处理程序都会执行
- 处理程序中发生的错误会中断后续处理程序

### 避免无限循环

```python
# 错误示例：在处理程序中修改帧会触发新的处理程序调用
def bad_handler(scene):
    scene.frame_current += 1  # 这会导致无限循环

bpy.app.handlers.frame_change_post.append(bad_handler)  # 不要这样做
```

### 性能影响

- 处理程序应尽量轻量
- 避免在处理程序中进行复杂计算
- 使用条件检查减少不必要的操作

### 注册和注销

```python
import bpy

def my_handler(scene):
    pass

# 注册
bpy.app.handlers.frame_change_post.append(my_handler)

# 注销
if my_handler in bpy.app.handlers.frame_change_post:
    bpy.app.handlers.frame_change_post.remove(my_handler)

# 安全注销
try:
    bpy.app.handlers.frame_change_post.remove(my_handler)
except ValueError:
    pass
```

## 最佳实践

1. **模块化**：将处理程序定义在单独的函数或类中
2. **清理**：在插件卸载时移除所有处理程序
3. **错误处理**：在处理程序中添加异常处理
4. **条件执行**：仅在需要时执行处理程序
5. **性能优化**：避免在处理程序中进行耗时操作


---
## bpy_callbacks_handlers

# Python回调与处理程序模块

## 回调与处理程序概述

Blender的回调系统允许开发者在特定事件发生时执行自定义代码。处理程序（Handlers）是Blender内置的回调机制，用于响应帧更改、场景更新、加载完成等事件。理解和正确使用这些机制对于创建响应式的插件至关重要。

```python
# Blender回调类型
# 帧更改处理程序 - frame_change_pre/post
# 场景更新处理程序 - depsgraph_update_pre/post
# 加载处理程序 - load_pre/post
# 保存处理程序 - save_pre/post
# 渲染处理程序 - render_pre/post
# 撤销处理程序 - undo_post/pre
# 粘贴处理程序 - paste_post

# 回调用途
# 自动保存和备份
# 实时更新和同步
# 性能监控
# 数据验证
# 自定义行为触发

# 回调特点
# 在主线程执行
# 避免长时间运行的操作
# 不要修改Blender状态
# 注意循环依赖
```

## 帧处理程序

### 帧更改处理

```python
# 帧前处理程序
def frame_change_pre_handler(scene):
    print(f"即将进入帧: {scene.frame_current}")
    
    # 检查帧范围
    if scene.frame_current > scene.frame_end:
        print("超出帧范围")
        return
    
    # 同步其他元素
    obj = scene.active_object
    if obj:
        print(f"活动对象: {obj.name}")

# 注册帧前处理程序
bpy.app.handlers.frame_change_pre.append(frame_change_pre_handler)

# 帧后处理程序
def frame_change_post_handler(scene):
    print(f"已切换到帧: {scene.frame_current}")
    
    # 更新依赖图
    depsgraph = bpy.context.evaluated_depsgraph_get()
    
    # 检查场景变化
    for update in depsgraph.updates:
        print(f"更新: {update.id.name}")

# 注册帧后处理程序
bpy.app.handlers.frame_change_post.append(frame_change_post_handler)

# 移除处理程序
bpy.app.handlers.frame_change_pre.remove(frame_change_pre_handler)
bpy.app.handlers.frame_change_post.remove(frame_change_post_handler)
```

### 帧动画处理

```python
# 批量帧处理
def process_frame_range(scene, start_frame, end_frame):
    original_frame = scene.frame_current
    
    for frame in range(start_frame, end_frame + 1):
        scene.frame_set(frame)
        process_frame_data(scene)
    
    # 恢复原始帧
    scene.frame_set(original_frame)

def process_frame_data(scene):
    # 处理帧数据
    for obj in scene.objects:
        if obj.animation_data:
            for track in obj.animation_data.nla_tracks:
                for strip in track.strips:
                    print(f"动作: {strip.name}, 帧: {strip.frame_start}-{strip.frame_end}")

# 帧范围处理程序
def frame_range_handler(scene):
    if scene.frame_current == scene.frame_end:
        print("动画播放完成")
        # 可以在这里触发后续操作

bpy.app.handlers.frame_change_post.append(frame_range_handler)
```

## 场景更新处理程序

### 依赖图更新

```python
# 依赖图更新前
def depsgraph_update_pre_handler(scene, depsgraph):
    print("依赖图即将更新")
    
    # 收集将要更新的对象
    updates = list(depsgraph.updates)
    print(f"将要更新的对象数: {len(updates)}")

bpy.app.handlers.depsgraph_update_pre.append(depsgraph_update_pre_handler)

# 依赖图更新后
def depsgraph_update_post_handler(scene, depsgraph):
    print("依赖图已更新")
    
    # 处理更新
    for update in depsgraph.updates:
        id_block = update.id
        
        if id_block is scene:
            print("场景已更新")
        elif hasattr(id_block, 'name'):
            print(f"对象已更新: {id_block.name}")

bpy.app.handlers.depsgraph_update_post.append(depsgraph_update_post_handler)
```

### 对象变化跟踪

```python
# 跟踪对象属性变化
class ObjectChangeTracker:
    def __init__(self):
        self._last_state = {}
    
    def track_object(self, obj):
        self._last_state[obj.name] = {
            'location': obj.location.copy(),
            'rotation': obj.rotation_euler.copy(),
            'scale': obj.scale.copy()
        }
    
    def check_changes(self, scene, depsgraph):
        for update in depsgraph.updates:
            obj = update.id
            if not hasattr(obj, 'name'):
                continue
            
            if obj.name not in self._last_state:
                self.track_object(obj)
                print(f"新对象: {obj.name}")
                continue
            
            # 比较状态
            last = self._last_state[obj.name]
            current = {
                'location': obj.location.copy(),
                'rotation': obj.rotation_euler.copy(),
                'scale': obj.scale.copy()
            }
            
            if last != current:
                print(f"对象变化: {obj.name}")
                self.track_object(obj)

# 使用跟踪器
tracker = ObjectChangeTracker()

def track_updates(scene, depsgraph):
    tracker.check_changes(scene, depsgraph)

bpy.app.handlers.depsgraph_update_post.append(track_updates)
```

### 几何体更新

```python
# 跟踪几何体变化
class GeometryChangeTracker:
    def __init__(self):
        self._last_vert_count = {}
    
    def check_mesh_changes(self, depsgraph):
        for update in depsgraph.updates:
            obj = update.id
            if obj.type != 'MESH':
                continue
            
            # 获取评估后的网格
            eval_obj = obj.evaluated_get(depsgraph)
            mesh = eval_obj.to_mesh()
            
            if obj.name not in self._last_vert_count:
                self._last_vert_count[obj.name] = len(mesh.vertices)
                print(f"新网格对象: {obj.name}, 顶点数: {len(mesh.vertices)}")
                continue
            
            if self._last_vert_count[obj.name] != len(mesh.vertices):
                print(f"网格变化: {obj.name}")
                print(f"  旧顶点数: {self._last_vert_count[obj.name]}")
                print(f"  新顶点数: {len(mesh.vertices)}")
                self._last_vert_count[obj.name] = len(mesh.vertices)
            
            eval_obj.to_mesh_clear()

tracker = GeometryChangeTracker()

def track_geometry_changes(scene, depsgraph):
    tracker.check_mesh_changes(depsgraph)

bpy.app.handlers.depsgraph_update_post.append(track_geometry_changes)
```

## 加载和保存处理程序

### 文件加载

```python
# 加载前处理程序
def load_pre_handler(filepath):
    print(f"即将加载文件: {filepath}")
    
    # 清理旧数据
    cleanup_before_load()

def cleanup_before_load():
    # 清理临时数据
    if hasattr(bpy.data, 'my_temp_data'):
        delattr(bpy.data, 'my_temp_data')

bpy.app.handlers.load_pre.append(load_pre_handler)

# 加载后处理程序
def load_post_handler(context):
    print("文件加载完成")
    
    # 初始化新数据
    initialize_data()
    
    # 设置默认视图
    setup_default_view()

def initialize_data():
    # 初始化插件数据
    print("初始化插件数据")

def setup_default_view():
    # 设置默认3D视图
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces[0]
            space.shading.type = 'SOLID'

bpy.app.handlers.load_post.append(load_post_handler)
```

### 文件保存

```python
# 保存前处理程序
def save_pre_handler(filepath):
    print(f"即将保存文件: {filepath}")
    
    # 清理临时数据
    cleanup_before_save()

def cleanup_before_save():
    # 移除未使用的数据块
    for mesh in list(bpy.data.meshes):
        if mesh.users == 0:
            bpy.data.meshes.remove(mesh)

bpy.app.handlers.save_pre.append(save_pre_handler)

# 保存后处理程序
def save_post_handler(filepath):
    print(f"文件已保存: {filepath}")
    
    # 记录保存时间
    import time
    bpy.app.handlers.save_time = time.time()

bpy.app.handlers.save_post.append(save_post_handler)
```

### 自动保存

```python
# 自动保存处理
class AutoSaver:
    def __init__(self, interval=300):  # 5分钟
        self.interval = interval
        self.last_save = 0
    
    def check_autosave(self, scene):
        import time
        current_time = time.time()
        
        if current_time - self.last_save > self.interval:
            self.perform_autosave()
            self.last_save = current_time
    
    def perform_autosave(self):
        print("执行自动保存")
        bpy.ops.wm.save_mainfile(filepath=bpy.data.filepath)

saver = AutoSaver()

def autosave_handler(scene):
    saver.check_autosave(scene)

# 注册处理程序
bpy.app.handlers.save_post.append(autosave_handler)
```

## 渲染处理程序

### 渲染前后

```python
# 渲染前处理程序
def render_pre_handler(scene, depsgraph):
    print("开始渲染")
    
    # 设置渲染环境
    scene.render.engine = 'CYCLES'
    
    # 清理场景
    cleanup_for_render()

def cleanup_for_render():
    # 隐藏不需要渲染的对象
    for obj in bpy.data.objects:
        if obj.get('temp_object'):
            obj.hide_render = True

bpy.app.handlers.render_pre.append(render_pre_handler)

# 渲染初始化处理程序
def render_init_handler(scene):
    print("渲染初始化")
    
    # 获取渲染设置
    engine = scene.render.engine
    resolution = (scene.render.resolution_x, scene.render.resolution_y)
    print(f"渲染引擎: {engine}")
    print(f"分辨率: {resolution}")

bpy.app.handlers.render_init.append(render_init_handler)

# 渲染完成处理程序
def render_complete_handler(scene):
    print("渲染完成")
    
    # 保存渲染结果
    save_render_result()

def save_render_result():
    # 保存渲染图像
    print("保存渲染结果")

bpy.app.handlers.render_complete.append(render_complete_handler)

# 渲染取消处理程序
def render_cancel_handler(scene):
    print("渲染已取消")
    
    # 清理临时数据
    cleanup_after_cancel()

def cleanup_after_cancel():
    print("清理渲染临时数据")

bpy.app.handlers.render_cancel.append(render_cancel_handler)
```

### 渲染层

```python
# 渲染层处理
class RenderLayerHandler:
    def __init__(self):
        self.render_times = {}
    
    def render_layer_init(self, scene, depsgraph):
        layer = scene.render.layers.active.name
        print(f"开始渲染层: {layer}")
        self.render_times[layer] = time.time()
    
    def render_layer_complete(self, scene):
        layer = scene.render.layers.active.name
        if layer in self.render_times:
            elapsed = time.time() - self.render_times[layer]
            print(f"渲染层 {layer} 完成，耗时: {elapsed:.2f}秒")

handler = RenderLayerHandler()
bpy.app.handlers.render_init.append(handler.render_layer_init)
bpy.app.handlers.render_complete.append(handler.render_layer_complete)
```

## 撤销处理程序

### 撤销前后

```python
# 撤销前处理程序
def undo_pre_handler(context):
    print("即将撤销操作")
    
    # 记录撤销前状态
    record_undo_state()

def record_undo_state():
    scene = bpy.context.scene
    print(f"当前帧: {scene.frame_current}")

bpy.app.handlers.undo_pre.append(undo_pre_handler)

# 撤销后处理程序
def undo_post_handler(context):
    print("撤销完成")
    
    # 同步UI
    sync_ui_after_undo()

def sync_ui_after_undo():
    # 更新界面显示
    for area in bpy.context.screen.areas:
        area.tag_redraw()

bpy.app.handlers.undo_post.append(undo_post_handler)
```

### 重做处理

```python
# 重做前处理程序
def redo_pre_handler(context):
    print("即将重做操作")

bpy.app.handlers.redo_pre.append(redo_pre_handler)

# 重做后处理程序
def redo_post_handler(context):
    print("重做完成")
    
    # 同步数据
    sync_data_after_redo()

def sync_data_after_redo():
    print("同步数据")

bpy.app.handlers.redo_post.append(redo_post_handler)
```

## 粘贴处理程序

```python
# 粘贴后处理程序
def paste_post_handler(context):
    print("粘贴完成")
    
    # 处理粘贴的对象
    handle_pasted_objects()

def handle_pasted_objects():
    # 找到新粘贴的对象
    for obj in bpy.context.selected_objects:
        if obj.get('just_pasted'):
            print(f"处理粘贴的对象: {obj.name}")
            obj.location += (0.5, 0.5, 0)

bpy.app.handlers.paste_post.append(paste_post_handler)
```

## 自定义回调系统

### 事件系统

```python
# 自定义事件系统
class EventSystem:
    def __init__(self):
        self._handlers = {}
    
    def register(self, event_name, handler):
        if event_name not in self._handlers:
            self._handlers[event_name] = []
        self._handlers[event_name].append(handler)
    
    def unregister(self, event_name, handler):
        if event_name in self._handlers:
            self._handlers[event_name].remove(handler)
    
    def trigger(self, event_name, *args, **kwargs):
        if event_name in self._handlers:
            for handler in self._handlers[event_name]:
                handler(*args, **kwargs)

# 创建全局事件系统
events = EventSystem()

# 注册事件
def my_event_handler(data):
    print(f"收到事件数据: {data}")

events.register("custom_event", my_event_handler)

# 触发事件
events.trigger("custom_event", {"key": "value"})
```

### 属性更新回调

```python
# 属性更新系统
class PropertyUpdateSystem:
    def __init__(self):
        self._watched = {}
    
    def watch(self, obj, prop_name, callback):
        key = (id(obj), prop_name)
        if key not in self._watched:
            self._watched[key] = []
        self._watched[key].append(callback)
    
    def unwatch(self, obj, prop_name, callback):
        key = (id(obj), prop_name)
        if key in self._watched:
            self._watched[key].remove(callback)
    
    def notify(self, obj, prop_name, old_value, new_value):
        key = (id(obj), prop_name)
        if key in self._watched:
            for callback in self._watched[key]:
                callback(obj, prop_name, old_value, new_value)

# 使用更新系统
updates = PropertyUpdateSystem()

def on_property_change(obj, prop, old, new):
    print(f"属性变化: {obj.name}.{prop}: {old} -> {new}")

# 监听属性变化
obj = bpy.context.active_object
if obj:
    updates.watch(obj, 'location', on_property_change)
```

### 场景变化通知

```python
# 场景变化通知系统
class SceneNotifier:
    def __init__(self):
        self._subscribers = []
    
    def subscribe(self, callback):
        self._subscribers.append(callback)
    
    def unsubscribe(self, callback):
        self._subscribers.remove(callback)
    
    def notify(self, event_type, data):
        for callback in self._subscribers:
            callback(event_type, data)

# 集成到Blender处理程序
scene_notifier = SceneNotifier()

def depsgraph_handler(scene, depsgraph):
    scene_notifier.notify("update", {"depsgraph": depsgraph})

bpy.app.handlers.depsgraph_update_post.append(depsgraph_handler)

# 订阅场景变化
def scene_change_handler(event_type, data):
    print(f"场景事件: {event_type}")

scene_notifier.subscribe(scene_change_handler)
```

## 性能注意事项

### 优化回调

```python
# 高效的帧处理程序
class OptimizedFrameHandler:
    def __init__(self):
        self._last_frame = -1
        self._cache = {}
    
    def handle_frame_change(self, scene):
        # 避免重复处理
        if scene.frame_current == self._last_frame:
            return
        
        self._last_frame = scene.frame_current
        
        # 只处理必要的对象
        active_obj = scene.active_object
        if active_obj:
            self.process_active_object(active_obj)
    
    def process_active_object(self, obj):
        # 缓存处理结果
        cache_key = obj.name
        if cache_key not in self._cache:
            self._cache[cache_key] = self.calculate_object_data(obj)
        
        # 使用缓存的数据
        data = self._cache[cache_key]
        return data
    
    def calculate_object_data(self, obj):
        # 计算对象数据
        return {"name": obj.name, "type": obj.type}

handler = OptimizedFrameHandler()
bpy.app.handlers.frame_change_post.append(handler.handle_frame_change)
```

### 批量更新

```python
# 批量更新处理
class BatchUpdater:
    def __init__(self):
        self._pending_updates = []
        self._update_timer = None
    
    def schedule_update(self, obj, prop, value):
        self._pending_updates.append((obj, prop, value))
        
        if self._update_timer is None:
            self._start_timer()
    
    def _start_timer(self):
        import bpy
        self._update_timer = bpy.app.timers.register(
            self._process_updates,
            first_interval=0.1
        )
    
    def _process_updates(self):
        # 批量应用更新
        for obj, prop, value in self._pending_updates:
            setattr(obj, prop, value)
        
        self._pending_updates.clear()
        self._update_timer = None
        return None  # 停止定时器

updater = BatchUpdater()
```

### 避免内存泄漏

```python
# 清理处理程序
class HandlerManager:
    def __init__(self):
        self._handlers = []
    
    def add_handler(self, handler_list, handler):
        handler_list.append(handler)
        self._handlers.append((handler_list, handler))
    
    def remove_all(self):
        for handler_list, handler in self._handlers:
            if handler in handler_list:
                handler_list.remove(handler)
        self._handlers.clear()

manager = HandlerManager()

def my_handler(scene):
    pass

manager.add_handler(bpy.app.handlers.frame_change_post, my_handler)

# 在插件卸载时清理
def unregister():
    manager.remove_all()
```

## 最佳实践

### 处理程序设计

```python
# 轻量级处理程序
def light_handler(scene):
    # 尽可能轻量
    if not bpy.context.active_object:
        return
    
    # 只做必要的事情
    pass

# 避免阻塞操作
def non_blocking_handler(scene):
    # 不要在这里做耗时操作
    # 不要调用可能阻塞的操作
    pass

# 幂等处理程序
def idempotent_handler(scene):
    # 多次执行结果相同
    current_frame = scene.frame_current
    # 基于当前帧状态执行操作
    pass

# 线程安全
def thread_safe_handler(scene):
    # Blender处理程序在主线程运行
    # 可以安全访问Blender数据
    pass
```

### 错误处理

```python
# 健壮的处理程序
def robust_handler(scene):
    try:
        # 危险操作
        obj = scene.active_object
        if obj:
            process_object(obj)
    except Exception as e:
        # 记录错误但不中断Blender
        print(f"处理程序错误: {e}")
        import traceback
        traceback.print_exc()

def process_object(obj):
    # 处理对象
    pass

# 重试机制
def retry_handler(scene, max_retries=3):
    for attempt in range(max_retries):
        try:
            risky_operation()
            break
        except Exception as e:
            if attempt == max_retries - 1:
                print(f"重试失败: {e}")
```

### 性能监控

```python
# 处理程序性能监控
import time

class PerformanceMonitor:
    def __init__(self):
        self._timings = {}
    
    def monitor(self, handler_name):
        def decorator(func):
            def wrapper(scene):
                start = time.time()
                result = func(scene)
                elapsed = time.time() - start
                
                if handler_name not in self._timings:
                    self._timings[handler_name] = []
                self._timings[handler_name].append(elapsed)
                
                if elapsed > 0.1:  # 超过100ms
                    print(f"警告: {handler_name} 耗时 {elapsed:.3f}秒")
                
                return result
            return wrapper
        return decorator
    
    def get_stats(self):
        stats = {}
        for name, timings in self._timings.items():
            stats[name] = {
                'count': len(timings),
                'avg': sum(timings) / len(timings),
                'max': max(timings)
            }
        return stats

monitor = PerformanceMonitor()

@monitor.monitor("frame_handler")
def frame_handler(scene):
    pass

@monitor.monitor("depsgraph_handler")
def depsgraph_handler(scene, depsgraph):
    pass
```

## 高级用法

### 条件处理程序

```python
# 条件处理程序
class ConditionalHandler:
    def __init__(self):
        self._conditions = []
    
    def add_condition(self, condition_func):
        self._conditions.append(condition_func)
    
    def check_conditions(self, *args, **kwargs):
        for condition in self._conditions:
            if not condition(*args, **kwargs):
                return False
        return True

# 条件示例
def is_playing(scene):
    return bpy.context.screen.is_animation_playing

def is_editing_mode(scene):
    obj = bpy.context.active_object
    return obj and obj.mode == 'EDIT'

handler = ConditionalHandler()
handler.add_condition(is_playing)
handler.add_condition(is_editing_mode)

def conditional_frame_handler(scene):
    if not handler.check_conditions(scene):
        return
    
    # 只在条件满足时执行
    print("处理帧")
```

### 优先级处理程序

```python
# 优先级处理程序
class PriorityHandler:
    def __init__(self):
        self._handlers = {}  # priority -> [handlers]
    
    def add_handler(self, handler, priority=0):
        if priority not in self._handlers:
            self._handlers[priority] = []
        self._handlers[priority].append(handler)
    
    def execute(self, scene):
        # 按优先级从高到低执行
        for priority in sorted(self._handlers.keys(), reverse=True):
            for handler in self._handlers[priority]:
                handler(scene)

# 使用优先级
handler = PriorityHandler()

def high_priority_handler(scene):
    print("高优先级")

def low_priority_handler(scene):
    print("低优先级")

handler.add_handler(high_priority_handler, priority=10)
handler.add_handler(low_priority_handler, priority=0)

# 执行
def frame_handler(scene):
    handler.execute(scene)

bpy.app.handlers.frame_change_post.append(frame_handler)
```

### 延迟处理程序

```python
# 延迟处理程序
class DelayedHandler:
    def __init__(self, delay=0.1):
        self.delay = delay
        self._pending = False
        self._timer = None
    
    def request_update(self, scene):
        if self._pending:
            return
        
        self._pending = True
        self._schedule_update(scene)
    
    def _schedule_update(self, scene):
        import bpy
        self._timer = bpy.app.timers.register(
            lambda: self._execute(scene),
            first_interval=self.delay
        )
    
    def _execute(self, scene):
        self._pending = False
        self._timer = None
        self._do_update(scene)
    
    def _do_update(self, scene):
        print(f"延迟更新: 帧 {scene.frame_current}")

handler = DelayedHandler()

def frame_handler(scene):
    handler.request_update(scene)

bpy.app.handlers.frame_change_post.append(frame_handler)
```


---
## bpy_addon_security

# Blender 插件安全与扩展系统指南

本模块涵盖 Blender 插件安全、扩展系统最佳实践以及脚本执行机制，帮助开发者创建安全、可靠的 Blender 插件。

## 概述

Blender 的 Python 脚本系统提供了强大的扩展能力，但也带来了安全风险。理解脚本执行机制和安全最佳实践对于开发专业级插件至关重要。Blender 4.2 引入了新的扩展系统（Extensions），替代了传统的插件（Add-ons）模式，提供了更好的依赖管理和沙箱支持。

**核心概念：**
- 脚本自动执行与安全风险
- 插件与扩展系统的区别
- 用户偏好设置与权限控制
- 依赖管理和本地存储

## 核心功能

### 脚本自动执行

Blender 支持多种自动执行脚本的方式，每种方式都有不同的安全含义。

**自动执行场景：**

```python
import bpy

# 场景1：注册文本块
# 在文本编辑器中创建一个文本数据块，启用"Register"选项
# 该脚本将在 Blender 启动时自动执行

# 场景2：动画驱动
# Python 表达式用于驱动属性值，常见于高级绑定和动画
# 示例：在驱动编辑器中使用表达式
driver_expr = "sin(frame * 0.1)"  # 简单正弦波驱动

# 场景3：Freestyle 渲染
# Freestyle 使用脚本控制线条样式
# 即使关闭自动执行，这些脚本也会运行
```

**手动执行场景（需要用户交互）：**

```python
# 这些操作会触发脚本执行，即使自动执行已关闭
# 但用户应该意识到这些行为

# 1. 在文本编辑器中运行脚本
# bpy.ops.text.run_script()

# 2. Freestyle 渲染时的线条样式脚本
# 3. 从文件浏览器打开文件时的脚本执行
```

### 安全控制机制

Blender 提供了多层安全控制来管理脚本执行。

**文件浏览器信任源：**

```python
# 当通过文件浏览器打开 blend 文件时
# 可以设置"Trusted Source"选项控制自动执行
# 此选项可以逐个文件设置
```

**用户偏好设置：**

```python
# 在偏好设置中控制自动执行
# 位置：编辑 > 偏好设置 > 文件路径 > 自动运行 Python 脚本

# 启用后，"Trusted Source"选项将默认勾选
# 可以排除特定目录（通常排除下载目录）

# 命令行覆盖：
# blender --background --enable-autoexec  # 启用自动执行
# blender --background --disable-autoexec  # 禁用自动执行
```

**在线访问控制：**

```python
# 检查在线访问权限
import bpy

if bpy.app.online_access:
    # 用户允许在线访问，可以进行网络请求
    pass
else:
    # 用户禁止在线访问，插件不应尝试网络连接

# 检查是否允许用户更改此设置
if bpy.app.online_access_override is None:
    # 用户可以在偏好设置中更改
    pass
else:
    # 此设置被命令行锁定
    pass
```

### 传统插件与扩展系统

Blender 4.2 引入了新的扩展系统，逐步替代传统插件模式。

**传统插件结构：**

```python
bl_info = {
    "name": "我的插件",
    "blender": (2, 80, 0),  # 最低 Blender 版本
    "category": "Object",
    "version": (1, 0, 0),
    "author": "作者名",
    "description": "插件描述",
}

import bpy

class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "我的操作"
    
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

**扩展系统结构（Blender 4.2+）：**

```python
# manifest.toml - 扩展清单文件
"""
name = "我的扩展"
version = "1.0.0"
blender_version_min = "4.2.0"
blender_version_max = "5.0.0"
maintainer = "作者名 <email@example.com>"
license = "GPL-3.0-or-later"
"""

# __init__.py - 扩展主模块
from . import utils  # 使用相对导入

class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "我的操作"
    
    def execute(self, context):
        return {'FINISHED'}

def register():
    bpy.utils.register_class(MyOperator)

def unregister():
    bpy.utils.unregister_class(MyOperator)
```

**迁移指南：**

```python
# 1. 创建 manifest.toml 清单文件
# 2. 移除 bl_info 字典（现在在 manifest.toml 中定义）
# 3. 将模块名引用替换为 __package__

# 之前：
class MyPrefs(bpy.types.AddonPreferences):
    bl_idname = "my_addon"

# 之后：
class MyPrefs(bpy.types.AddonPreferences):
    bl_idname = __package__  # 使用 __package__ 替代硬编码名称

# 4. 使用相对导入
# 之前：from my_module import utils
# 现在：from . import utils

# 5. 对于子包，使用多层相对导入
from .. import __package__ as base_package
```

### 依赖管理

扩展系统支持多种依赖管理方式。

**Python Wheels（推荐）：**

```python
# 在 manifest.toml 中声明依赖
"""
[dependencies]
python = ">=3.10"
numpy = ">=1.24.0"
"""

# Blender 会自动安装声明的依赖
```

**Vendoring 第三方库：**

```python
# 使用 vendorize 工具将依赖打包为子模块
# 优点：避免版本冲突
# 缺点：需要额外配置
```

**本地存储：**

```python
# 获取用户可写的扩展目录
extension_dir = bpy.utils.extension_path_user(__package__, path="", create=True)

# 创建子目录
sub_dir = bpy.utils.extension_path_user(__package__, path="cache", create=True)

# 注意：此目录在扩展升级时会保留
# 但卸载扩展时会删除
```

## 实用函数

### 类型注册检查

```python
def check_registered(cls):
    """检查类是否已注册，避免重复注册"""
    try:
        bpy.utils.unregister_class(cls)
    except RuntimeError:
        pass
    bpy.utils.register_class(cls)
```

### 版本兼容性检查

```python
def check_blender_version(major, minor=0):
    """检查 Blender 版本是否满足要求"""
    import sys
    if sys.version_info < (major, minor):
        return False
    return True

# 使用示例
if check_blender_version(4, 2):
    # 使用扩展系统
    pass
else:
    # 使用传统插件
    pass
```

### 安全的网络请求

```python
import urllib.request
import json

def fetch_online_data(url):
    """安全的在线数据获取"""
    # 检查在线访问权限
    if not bpy.app.online_access:
        return None
    
    try:
        with urllib.request.urlopen(url) as response:
            data = json.loads(response.read().decode())
        return data
    except Exception as e:
        print(f"网络请求失败: {e}")
        return None
```

### 插件偏好设置

```python
class MyAddonPreferences(bpy.types.AddonPreferences):
    bl_idname = __package__
    
    api_key: bpy.props.StringProperty(
        name="API Key",
        description="请输入 API 密钥",
        default="",
        subtype='PASSWORD'
    )
    
    auto_update: bpy.props.BoolProperty(
        name="自动更新",
        description="自动检查更新",
        default=True
    )
    
    def draw(self, context):
        layout = self.layout
        col = layout.column()
        col.prop(self, "api_key")
        col.prop(self, "auto_update")
```

### 调试和错误处理

```python
import traceback

class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "我的操作"
    
    def execute(self, context):
        try:
            # 执行主要逻辑
            result = self.run_operation(context)
            self.report({'INFO'}, "操作成功")
            return {'FINISHED'}
        except Exception as e:
            # 记录错误
            error_msg = f"操作失败: {str(e)}"
            self.report({'ERROR'}, error_msg)
            
            # 在控制台打印详细错误
            print("详细错误信息:")
            traceback.print_exc()
            
            return {'CANCELLED'}
    
    def run_operation(self, context):
        # 实际的操作逻辑
        pass
```

## 常见问题排查

### 问题：插件启用后无响应

**可能原因：**
1. 注册过程中发生异常
2. 依赖未正确安装
3. 版本不兼容

**排查步骤：**

```python
# 1. 打开控制台查看错误信息
# 2. 检查 Blender 系统控制台输出
# 3. 使用调试模式启动 Blender
#    blender --debug
```

**解决方案：**

```python
# 添加详细的注册日志
def register():
    print("开始注册插件...")
    try:
        bpy.utils.register_class(MyOperator)
        print("MyOperator 注册成功")
    except Exception as e:
        print(f"注册失败: {e}")
        raise
    
    print("插件注册完成")
```

### 问题：脚本不被信任

**解决方案：**

```python
# 1. 通过文件浏览器打开文件，勾选"Trusted Source"
# 2. 或在偏好设置中启用"Auto Run Python Scripts"
# 3. 或使用命令行参数
#    blender --enable-autoexec file.blend
```

### 问题：扩展依赖冲突

**解决方案：**

```python
# 1. 使用独立的虚拟环境
# 2. 使用 vendorize 打包依赖
# 3. 指定精确的版本号
"""
[dependencies]
numpy = "==1.24.0"
"""
```

### 问题：__package__ 使用错误

**错误示例：**

```python
# 错误：硬编码模块名
class MyPrefs(bpy.types.AddonPreferences):
    bl_idname = "my_addon"  # 错误

# 错误：使用字符串字面量
class MyPrefs(bpy.types.AddonPreferences):
    bl_idname = "bl_ext.my_repo.my_addon"  # 错误
```

**正确示例：**

```python
# 正确：使用 __package__
class MyPrefs(bpy.types.AddonPreferences):
    bl_idname = __package__
```

### 问题：相对导入错误

**错误示例：**

```python
# 错误：从父包导入
from my_addon import utils

# 错误：绝对导入
from my_addon.submodule import utils
```

**正确示例：**

```python
# 正确：当前包内的相对导入
from . import utils

# 正确：子包的相对导入
from .submodule import utils

# 正确：父包的相对导入
from .. import utils
```

## 设计最佳实践

### 1. 安全性优先

```python
# 始终检查在线访问权限
if not bpy.app.online_access:
    return

# 不要在代码中硬编码敏感信息
# 使用 bpy.props.StringProperty(subtype='PASSWORD')

# 最小化权限请求
# 只请求必要的权限
```

### 2. 版本兼容性

```python
def get_blender_version():
    """获取 Blender 版本元组"""
    return bpy.app.version

# 检查版本后使用适当的 API
if get_blender_version() >= (4, 2, 0):
    # 使用新 API
    pass
else:
    # 使用兼容的旧 API
    pass
```

### 3. 错误恢复

```python
def safe_register(classes_to_register):
    """安全注册多个类"""
    registered = []
    try:
        for cls in classes_to_register:
            bpy.utils.register_class(cls)
            registered.append(cls)
    except Exception as e:
        # 回滚已注册的类
        for cls in reversed(registered):
            try:
                bpy.utils.unregister_class(cls)
            except Exception:
                pass
        raise
```

### 4. 性能优化

```python
# 使用批量操作
bpy.ops.object.select_all(action='DESELECT')

# 避免频繁的上下文切换
with bpy.context.temp_override(window=context.window):
    bpy.ops.object.delete()

# 使用合适的集合操作
for obj in bpy.data.objects:
    if obj.type == 'MESH':
        # 处理网格对象
        pass
```

### 5. 用户体验

```python
# 提供清晰的反馈
self.report({'INFO'}, f"已处理 {count} 个对象")

# 支持撤销
class MyOperator(bpy.types.Operator):
    bl_options = {'REGISTER', 'UNDO'}
    
    def execute(self, context):
        # 修改数据
        return {'FINISHED'}

# 提供进度反馈
if 'progress' not in bpy.app.driver_namespace:
    bpy.app.driver_namespace['progress'] = 0

bpy.app.driver_namespace['progress'] += 1
```

## 高级主题

### 自定义属性系统

```python
class MyObjectProperties(bpy.types.PropertyGroup):
    my_int: bpy.props.IntProperty(
        name="整数",
        default=0,
        min=0,
        max=100
    )
    
    my_float: bpy.props.FloatProperty(
        name="浮点数",
        default=1.0,
        min=0.0,
        max=10.0,
        step=0.1,
        precision=2
    )
    
    my_string: bpy.props.StringProperty(
        name="字符串",
        maxlength=128
    )
    
    my_enum: bpy.props.EnumProperty(
        name="枚举",
        items=[
            ('A', "选项 A", "第一个选项"),
            ('B', "选项 B", "第二个选项"),
            ('C', "选项 C", "第三个选项"),
        ],
        default='A'
    )
    
    my_bool: bpy.props.BoolProperty(
        name="布尔值",
        default=True
    )

# 注册属性组
bpy.utils.register_class(MyObjectProperties)
bpy.types.Object.my_props = bpy.props.PointerProperty(type=MyObjectProperties)

# 使用属性
obj = bpy.context.active_object
print(obj.my_props.my_int)
```

### 事件处理

```python
import bpy

class ModalOperator(bpy.types.Operator):
    bl_idname = "object.modal_operator"
    bl_label = "模态操作"
    
    def __init__(self):
        self._timer = None
    
    def execute(self, context):
        # 设置模态处理
        wm = context.window_manager
        self._timer = wm.event_timer_add(0.1, window=context.window)
        wm.modal_handler_add(self)
        return {'RUNNING_MODAL'}
    
    def modal(self, context, event):
        if event.type == 'ESC':
            # 取消操作
            return self.cancel(context)
        
        if event.type == 'LEFTMOUSE' and event.value == 'PRESS':
            # 执行操作
            return self.finish(context)
        
        # 继续等待事件
        return {'RUNNING_MODAL'}
    
    def finish(self, context):
        wm = context.window_manager
        wm.event_timer_remove(self._timer)
        self.report({'INFO'}, "操作完成")
        return {'FINISHED'}
    
    def cancel(self, context):
        wm = context.window_manager
        wm.event_timer_remove(self._timer)
        self.report({'INFO'}, "操作已取消")
        return {'CANCELLED'}
```

### 上下文覆盖

```python
# 临时覆盖上下文执行操作
obj = bpy.data.objects.get("Cube")
if obj:
    with bpy.context.temp_override(object=obj):
        bpy.ops.object.copy()
        bpy.ops.object.paste()
```

### UI 列表和集合

```python
class MyItem(bpy.types.PropertyGroup):
    name: bpy.props.StringProperty(name="名称")
    value: bpy.props.FloatProperty(name="值")

class MY_UL_my_list(bpy.types.UIList):
    def draw_item(self, context, layout, data, item, icon, active_data, active_propname):
        if self.layout_type in {'DEFAULT', 'COMPACT'}:
            layout.prop(item, "name", text="", emboss=False)
        elif self.layout_type in {'GRID'}:
            layout.alignment = 'CENTER'
            layout.prop(item, "name", text="")

# 注册 UIList
bpy.utils.register_class(MY_UL_my_list)

# 在面板中使用
class MyPanel(bpy.types.Panel):
    def draw(self, context):
        layout = self.layout
        scene = context.scene
        
        layout.template_list(
            "MY_UL_my_list",
            "",
            scene,
            "my_items",
            scene,
            "my_items_index",
            rows=5
        )


---
## bpy_gotchas

# Blender Python API 陷阱与常见问题

本模块记录了使用 Blender Python API 时容易遇到的陷阱、常见错误和最佳实践，帮助开发者避免崩溃和意外行为。

## Python 线程限制

Blender 本身不线程安全，这意味着从 Python 子线程调用任何 Blender API 都可能导致崩溃或未定义行为。

```python
import threading
import time

def my_thread():
    time.sleep(1)
    # 错误：在子线程中访问 Blender 数据会导致崩溃
    bpy.ops.object.select_all(action='SELECT')

thread = threading.Thread(target=my_thread)
thread.start()
```

### 正确的多线程处理方式

```python
import threading
import bpy

def my_thread():
    # 错误示范
    pass

def my_thread_safe():
    # 正确：在主线程中操作 Blender，在子线程中只做纯计算
    pass

thread = threading.Thread(target=my_thread_safe)
thread.start()
```

所有 Blender API 调用必须在主线程中执行，子线程只能用于不涉及 Blender 的纯计算任务。如果需要在线程间传递数据，使用线程安全的队列或全局变量。

## 内部数据与 Python 对象的生命周期

Blender 的内部数据（如 Mesh、Object、Node 等）由 C/C++ 管理，Python 对象只是对这些内部数据的包装。当内部数据被删除时，Python 对象可能会变成无效引用。

```python
import bpy

# 错误：创建并删除对象后，Python 引用仍然存在
cube = bpy.data.objects.new("Cube", bpy.data.meshes.new("CubeMesh"))
bpy.context.collection.objects.link(cube)

# 删除对象
bpy.data.objects.remove(cube)

# 此时 cube 变量仍然存在，但指向的数据已无效
# 访问 cube.location 会导致崩溃或错误
```

### 对象生命周期管理

```python
import bpy

# 正确做法1：使用上下文管理器
with bpy.data.linestyle.new("Test", bezier=True) as linestyle:
    linestyle.color = (1, 0, 0)

# 正确做法2：显式管理引用计数
cube = bpy.data.objects.new("Cube", bpy.data.meshes.new("CubeMesh"))
bpy.context.collection.objects.link(cube)

# 使用完毕后清理
bpy.data.objects.remove(cube)
cube = None  # 断开引用
```

ID 类型对象（Mesh、Object、Material 等）的 Python 实例会被缓存，而非 ID 类型（如 EditBone、PoseBone 等）则是按需创建，用完即销毁。

## 运算符使用陷阱

### 上下文依赖

许多运算符依赖于当前上下文，在错误的环境中调用会导致失败或崩溃。

```python
import bpy

# 错误：在物体模式下尝试进行编辑模式操作
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.select_all(action='SELECT')  # 错误：当前不在编辑模式

# 正确：先切换模式
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.object.mode_set(mode='OBJECT')  # 切换回物体模式
```

### 运算符返回值处理

```python
import bpy

# 运算符返回 {'FINISHED'} 表示成功
result = bpy.ops.object.select_all(action='SELECT')
print(result)  # {'FINISHED'}

# 错误：假设所有操作都会成功
bpy.ops.object.delete()  # 如果没有选中的物体，会返回 {'CANCELLED'}
```

### 运算符状态变化

```python
import bpy

# 许多运算符会改变 Blender 的内部状态
bpy.ops.wm.read_homefile(use_empty=True)

# 之后访问之前获取的对象引用可能会失效
# obj = bpy.data.objects[0]  # 这个对象可能已经不存在了
```

## 网格访问与模式陷阱

### 编辑模式与物体模式的数据差异

```python
import bpy

# 错误：在物体模式下直接访问编辑模式的网格数据
obj = bpy.context.active_object
mesh = obj.data  # 这返回的是物体模式的 Mesh

# 在 Edit Mode 下，这个 mesh 不会反映编辑的更改
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.delete(type='VERT')

# 切换回物体模式
bpy.ops.object.mode_set(mode='OBJECT')

# 正确做法：使用 bmesh 模块处理编辑模式
import bmesh

obj = bpy.context.active_object
bm = bmesh.from_edit_mesh(obj.data)

# 对 bm 进行操作
bm.verts.ensure_lookup_table()
bm.verts[0].co.x += 1.0

# 提交更改
bmesh.update_edit_mesh(obj.data)
```

### Mesh 数据更新

```python
import bpy

# 错误：修改 Mesh 顶点后不更新
mesh = bpy.context.active_object.data
mesh.vertices[0].co.x = 5.0

# 渲染或某些操作不会看到更改
# 正确：标记需要更新
mesh.update()

# 对于动态更新的场景，需要在每次更新后调用
for v in mesh.vertices:
    v.co *= 1.01
mesh.update()
```

## 骨骼与骨架陷阱

### 骨骼引用的有效性

```python
import bpy

# 错误：混淆不同模式下的骨骼对象
armature = bpy.data.armatures[0]

# Edit Mode 下的骨骼
bpy.ops.object.mode_set(mode='EDIT')
edit_bones = armature.edit_bones
# edit_bones[0] 是 EditBone 对象

# Object Mode/Pose Mode 下的骨骼
bpy.ops.object.mode_set(mode='OBJECT')
pose = armature.pose
pose_bones = pose.bones
# pose_bones[0] 是 PoseBone 对象

# 错误：EditBone 和 PoseBone 是不同的对象
# 不能在 Edit Mode 下使用 PoseBone，反之亦然
```

### 骨骼命名与引用

```python
import bpy

# 骨骼名称可能不唯一（在不同的集合中）
armature = bpy.context.active_object.data

# 正确：通过完整路径或索引获取
bone = armature.bones["BoneName"]  # 按名称获取
bone = armature.bones[0]  # 按索引获取

# 访问 PoseBone 需要通过 pose 属性
pose_bone = armature.pose.bones["BoneName"]
```

## 文件路径与编码陷阱

### 路径处理

```python
import bpy
import os

# 错误：使用相对路径或特殊字符
path = "./textures/image.png"

# 正确：使用 Blender 的路径处理
path = bpy.path.abspath("./textures/image.png")

# 错误：硬编码路径分隔符
path = "C:\\Users\\Name\\Documents\\file.blend"

# 正确：使用 os.path 或 Blender 路径工具
path = os.path.join("C:\\Users\\Name\\Documents", "file.blend")

# 正确：使用斜杠（Blender 自动处理）
path = "C:/Users/Name/Documents/file.blend"
```

### 字符串编码

```python
import bpy

# 错误：假设所有字符串都是 ASCII
name = "我的对象"

# Blender 内部使用 UTF-8 编码
# 大多数情况下没问题，但在某些系统上可能有兼容性问题

# 正确：使用标准字符串，避免特殊字符
bpy.ops.mesh.primitive_cube_add(size=2, location=(0, 0, 0))
obj = bpy.context.active_object
obj.name = "Cube_Normal_Name"  # 使用 ASCII 或简单字符
```

## 上下文陷阱

### 上下文依赖性

```python
import bpy

# 错误：假设 context 总是包含预期的对象
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        for region in area.regions:
            if region.type == 'WINDOW':
                override = {
                    'area': area,
                    'region': region,
                    'space_data': area.spaces.active,
                    'scene': bpy.context.scene,
                }
                # 使用 override 调用运算符
                bpy.ops.view3d.view_all(override, center=bpy.context.space_data.region_3d.perspective == 'ORTHO')
```

### Context 变化的影响

```python
import bpy

# 某些操作会改变 context
print(bpy.context.active_object)  # 有对象

# 执行删除操作
bpy.ops.object.delete()

# 之后 context.active_object 可能变为 None
print(bpy.context.active_object)  # None
```

## 数据命名限制与库冲突

### 名称重复与限制

```python
import bpy

# 错误：假设名称是唯一的或可以被任意设置
obj1 = bpy.data.objects.new("Cube", bpy.data.meshes.new("Mesh"))
obj2 = bpy.data.objects.new("Cube", bpy.data.meshes.new("Mesh"))  # Blender 会自动添加后缀

print(obj1.name)  # "Cube.001"
print(obj2.name)  # "Cube.002"

# 错误：名称长度超过限制
long_name = "A" * 1000
obj = bpy.data.objects.new(long_name, bpy.data.meshes.new("Mesh"))
# 名称会被截断，可能导致意外结果
```

### 库数据冲突

```python
import bpy

# 从库中链接数据时可能发生命名冲突
# 错误：直接使用名称引用
lib_data = bpy.data.libraries.load("library.blend")

# 正确：使用自己的映射
mesh_name_mapping = {}
for mesh in lib_data.meshes:
    new_mesh = mesh.copy()
    mesh_name_mapping[mesh.name] = new_mesh

# 使用映射而不是名称
new_mesh = mesh_name_mapping["OriginalName"]
```

## 依赖图与更新延迟

```python
import bpy

# 错误：修改属性后立即访问计算后的值
bpy.context.active_object.location = (10, 0, 0)
# 这个位置可能还没更新
print(bpy.context.active_object.location)  # 可能不是 (10, 0, 0)

# 正确：强制更新
bpy.context.view_layer.update()

# 对于更复杂的依赖关系
bpy.context.active_object.rotation_euler = (1.57, 0, 0)
bpy.context.view_layer.update()
# 现在所有依赖于此的约束和动画都已计算
```

## 常见崩溃原因与避免方法

### 无效的内存访问

```python
import bpy

# 错误：访问已删除的对象
obj = bpy.data.objects[0]
bpy.data.objects.remove(obj)

# 仍然尝试访问
print(obj.name)  # 可能崩溃

# 正确：移除后断开引用
bpy.data.objects.remove(obj)
obj = None
```

### 无限循环与性能问题

```python
import bpy

# 错误：在每一帧更新时进行大量计算
def update_handler(scene, depsgraph):
    # 每次场景更新时都执行，可能导致卡顿
    for obj in bpy.data.objects:
        obj.location.x += 0.001

# 正确：使用定时器或节流
bpy.app.handlers.depsgraph_update_post.clear()
```

### 运算符调用顺序

```python
import bpy

# 某些运算符有特定的调用顺序要求
# 错误：颠倒操作顺序
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
# 可能失败或产生意外结果

# 正确：按照逻辑顺序应用变换
bpy.ops.object.transform_apply(scale=True)
bpy.ops.object.transform_apply(rotation=True)
bpy.ops.object.transform_apply(location=True)
```

## 最佳实践总结

避免这些陷阱的关键在于：理解 Blender 的数据模型和生命周期管理；始终在正确的上下文中使用 API；处理运算符的返回值；避免在子线程中访问 Blender；及时更新依赖图以获取最新状态；不要依赖名称进行长期引用，而是使用对象引用或自定义映射。



---
## bpy_ops.md

# bpy.ops Operators Reference

Complete reference for all Blender operators organized by category.

## Mesh Operators (bpy.ops.mesh)

### Creation

- `primitive_cube_add()` - Add cube
- `primitive_uv_sphere_add()` - Add UV sphere
- `primitive_ico_sphere_add()` - Add icosphere
- `primitive_cylinder_add()` - Add cylinder
- `primitive_cone_add()` - Add cone
- `primitive_torus_add()` - Add torus
- `primitive_grid_add()` - Add grid
- `primitive_monkey_add()` - Add monkey
- `primitive_plane_add()` - Add plane
- `primitive_circle_add()` - Add circle

### Editing

- `delete()` - Delete selected elements
- `duplicate()` - Duplicate selected elements
- `extrude_region_move()` - Extrude region
- `extrude_manifold()` - Extrude manifold
- `inset()` - Inset faces
- `bevel()` - Bevel edges
- `bisect()` - Bisect mesh
- `knife_project()` - Knife project
- `knife_cut()` - Knife cut
- `rip()` - Rip mesh
- `split()` - Split mesh
- `merge()` - Merge elements
- `separate()` - Separate mesh
- `join()` - Join meshes

### Modification

- `smooth()` - Smooth mesh
- `shrink_fatten()` - Shrink/fatten mesh
- `push_pull()` - Push/pull vertices
- `normals_make_consistent()` - Make normals consistent
- `flip_normals()` - Flip normals
- `recalculate_inside()` - Recalculate inside
- `recalculate_outside()` - Recalculate outside

### UV Mapping

- `uv_reset()` - Reset UV
- `uv_sphere_project()` - Sphere project UV
- `uv_cylinder_project()` - Cylinder project UV
- `uv_cube_project()` - Cube project UV

## Object Operators (bpy.ops.object)

### Creation

- `add()` - Add new object
- `duplicate()` - Duplicate object
- `duplicate_move()` - Duplicate and move
- `duplicate_move_linked()` - Duplicate linked
- `join()` - Join objects
- `separate()` - Separate objects

### Transformation

- `location_clear()` - Clear location
- `rotation_clear()` - Clear rotation
- `scale_clear()` - Clear scale
- `origin_clear()` - Clear origin
- `origin_set()` - Set origin
- `transform_apply()` - Apply transform

### Display

- `hide_view_clear()` - Clear hide view
- `hide_view_set()` - Set hide view
- `hide_render_clear()` - Clear hide render
- `hide_render_set()` - Set hide render
- `show_all()` - Show all objects
- `hide_all()` - Hide all objects

### Selection

- `select_all()` - Select all objects
- `deselect_all()` - Deselect all objects
- `select_by_type()` - Select by type
- `select_by_layer()` - Select by layer
- `select_same()` - Select same

### Parenting

- `parent_set()` - Set parent
- `parent_clear()` - Clear parent

### Collection

- `collection_link()` - Link to collection
- `collection_unlink()` - Unlink from collection
- `collection_objects_link()` - Link objects to collection
- `collection_instance_add()` - Add collection instance

## Material Operators (bpy.ops.material)

### Creation

- `new()` - Create new material
- `copy()` - Copy material
- `paste()` - Paste material

### Assignment

- `assign()` - Assign material to selection
- `select_all()` - Select all materials

### Slot Operations

- `slot_add()` - Add material slot
- `slot_remove()` - Remove material slot
- `slot_copy()` - Copy material slot
- `slot_paste()` - Paste material slot

## Texture Operators (bpy.ops.texture)

### Creation

- `new()` - Create new texture
- `copy()` - Copy texture

### Slot Operations

- `slot_add()` - Add texture slot
- `slot_remove()` - Remove texture slot

## Render Operators (bpy.ops.render)

### Rendering

- `render()` - Render scene
- `render_animation()` - Render animation
- `view_cancel()` - Cancel render
- `play_rendered_anim()` - Play rendered animation

### Settings

- `preset_add()` - Add render preset
- `preset_add_experimental()` - Add experimental preset

### View

- `view_show()` - Show render view
- `view_cancel()` - Cancel render view

## Node Operators (bpy.ops.node)

### Creation

- `add()` - Add node
- `add_search()` - Add node from search
- `link()` - Link nodes
- `link_make()` - Make link
- `link_cut()` - Cut link
- `link_detach()` - Detach link

### Editing

- `delete()` - Delete node
- `duplicate()` - Duplicate node
- `group_make()` - Make group
- `group_ungroup()` - Ungroup
- `group_insert()` - Insert group
- `group_edit()` - Edit group
- `group_exit()` - Exit group edit
- `read_sample()` - Read sample
- `read_viewlayers()` - Read view layers
- `read_fullsampled_layers()` - Read full sampled layers

### Organization

- `attach()` - Attach nodes
- `detach()` - Detach nodes
- `join()` - Join nodes
- `hide()` - Hide node
- `mute_toggle()` - Toggle mute
- `preview_toggle()` - Toggle preview
- `options_toggle()` - Toggle options
- `inputs_show()` - Show inputs
- `outputs_show()` - Show outputs
- `copy_color()` - Copy color
- `paste_color()` - Paste color

## Animation Operators (bpy.ops.anim)

### Keyframing

- `keyframe_insert()` - Insert keyframe
- `keyframe_delete()` - Delete keyframe
- `keyframe_insert_menu()` - Insert keyframe menu
- `keyframe_delete_v3d()` - Delete keyframe in 3D view

### Playback

- `play()` - Play animation
- `stop()` - Stop animation
- `pause()` - Pause animation

### Navigation

- `frame_jump()` - Jump to frame
- `frame_next()` - Next frame
- `frame_prev()` - Previous frame
- `end_frame_set()` - Set end frame
- `start_frame_set()` - Set start frame

## Scene Operators (bpy.ops.scene)

### Management

- `new()` - Create new scene
- `delete()` - Delete scene
- `duplicate()` - Duplicate scene

### Operations

- `add()` - Add scene
- `remove()` - Remove scene
- `link()` - Link scene

## File Operators (bpy.ops.file)

### Import

- `import_scene()` - Import scene
- `import_obj()` - Import OBJ
- `import_fbx()` - Import FBX
- `import_collada()` - Import Collada
- `import_blend()` - Import blend
- `import_svg()` - Import SVG
- `import_stl()` - Import STL
- `import_ply()` - Import PLY

### Export

- `export_scene()` - Export scene
- `export_obj()` - Export OBJ
- `export_fbx()` - Export FBX
- `export_collada()` - Export Collada
- `export_blend()` - Export blend
- `export_svg()` - Export SVG
- `export_stl()` - Export STL
- `export_ply()` - Export PLY

### File Operations

- `open()` - Open file
- `save()` - Save file
- `save_as()` - Save as
- `save_mainfile()` - Save main file
- `save_mainfile_as()` - Save main file as
- `save_incremental()` - Save incremental
- `recover_auto_save()` - Recover auto save
- `recover_last_session()` - Recover last session
- `reload()` - Reload file
- `close()` - Close file
- `quit()` - Quit Blender

## Screen Operators (bpy.ops.screen)

### Management

- `new()` - Create new screen
- `delete()` - Delete screen
- `duplicate()` - Duplicate screen
- `area_join()` - Join areas
- `area_split()` - Split areas
- `area_options()` - Area options
- `region_quadview()` - Quad view
- `region_scale()` - Scale region
- `region_flip()` - Flip region
- `screen_full_area()` - Full area
- `screen_set()` - Set screen
- `screen_set_quadview()` - Set quadview
- `screenshot()` - Screenshot
- `userpref_show()` - Show user preferences
- `spacedata_cleanup()` - Space data cleanup
- `workspace_to_scene()` - Workspace to scene

## UI Operators (bpy.ops.ui)

### Interface

- `reorder_to_front()` - Reorder to front
- `reorder_to_back()` - Reorder to back
- `reorder_to_top()` - Reorder to top
- `reorder_to_bottom()` - Reorder to bottom
- `copy_to_region()` - Copy to region
- `copy_data_path_button()` - Copy data path
- `drop_material()` - Drop material
- `drop_image()` - Drop image
- `drop_world()` - Drop world

### Menus

- `reports_to_textblock()` - Reports to text block
- `view2d_zoom()` - 2D view zoom
- `view2d_pan()` - 2D view pan
- `view2d_scroller_activate()` - Activate 2D scroller

## View2D Operators (bpy.ops.view2d)

### Navigation

- `pan()` - Pan view
- `zoom()` - Zoom view
- `zoom_in()` - Zoom in
- `zoom_out()` - Zoom out
- `zoom_border()` - Zoom border
- `scroll_down()` - Scroll down
- `scroll_up()` - Scroll up
- `scroll_left()` - Scroll left
- `scroll_right()` - Scroll right
- `zoom_ratio()` - Zoom ratio
- `reset()` - Reset view

## View3D Operators (bpy.ops.view3d)

### Navigation

- `rotate()` - Rotate view
- `pan()` - Pan view
- `zoom()` - Zoom view
- `zoom_in()` - Zoom in
- `zoom_out()` - Zoom out
- `dolly()` - Dolly view
- `fly()` - Fly view
- `walk()` - Walk view
- `roll()` - Roll view
- `orbit()` - Orbit view
- `move()` - Move view
- `ndof()` - NDOF navigation
- `view_all()` - View all
- `view_center()` - Center view
- `view_lock()` - Lock view
- `view_lock_clear()` - Clear view lock
- `camera_to_view()` - Camera to view
- `view_to_camera()` - View to camera
- `localview_set()` - Set local view
- `localview_clear()` - Clear local view

### Display

- `view_persportho()` - Perspective/orthographic
- `background_image_add()` - Add background image
- `background_image_remove()` - Remove background image
- `clip_border()` - Clip border
- `render_border()` - Render border
- `clear_render_border()` - Clear render border
- `home()` - Home view
- `smoothview()` - Smooth view
- `properties()` - Properties
- `toolshelf()` - Tool shelf

## Script Operators (bpy.ops.script)

### Execution

- `execute()` - Execute script
- `python_file_run()` - Run Python file
- `text_run()` - Run text block

### Management

- `reload()` - Reload scripts
- `auto_execute_py()` - Auto execute Python

## Console Operators (bpy.ops.console)

### Operations

- `clear()` - Clear console
- `copy()` - Copy console
- `paste()` - Paste to console
- `scrollback_append()` - Append scrollback
- `history_cycle()` - Cycle history
- `indent()` - Indent
- `unindent()` - Unindent

## Text Operators (bpy.ops.text)

### File Operations

- `new()` - New text block
- `open()` - Open text file
- `save()` - Save text
- `save_as()` - Save text as
- `reload()` - Reload text

### Editing

- `cut()` - Cut text
- `copy()` - Copy text
- `paste()` - Paste text
- `duplicate_line()` - Duplicate line
- `delete()` - Delete text
- `join_lines()` - Join lines
- `split_lines()` - Split lines
- `move()` - Move text
- `line_break()` - Line break
- `line_number()` - Line number
- `jump()` - Jump to line
- `find()` - Find text
- `replace()` - Replace text
- `to_3d_object()` - Text to 3D object

## Image Operators (bpy.ops.image)

### File Operations

- `new()` - New image
- `open()` - Open image
- `save()` - Save image
- `save_as()` - Save image as
- `save_sequence()` - Save sequence
- `save_all_modified()` - Save all modified
- `reload()` - Reload image

### Editing

- `invert()` - Invert image
- `flip()` - Flip image
- `resize()` - Resize image
- `scale()` - Scale image
- `crop()` - Crop image
- `clear()` - Clear image
- `fill()` - Fill image

## Sound Operators (bpy.ops.sound)

### File Operations

- `open()` - Open sound
- `open_mono()` - Open mono sound
- `pack()` - Pack sound
- `unpack()` - Unpack sound

## Camera Operators (bpy.ops.camera)

### Creation

- `add()` - Add camera

### Operations

- `presets_add()` - Add camera preset
- `presets_add_experimental()` - Add experimental preset

## Light Operators (bpy.ops.light)

### Creation

- `add()` - Add light
- `add_point()` - Add point light
- `add_sun()` - Add sun light
- `add_spot()` - Add spot light
- `add_area()` - Add area light
- `add_hemi()` - Add hemi light

### Operations

- `presets_add()` - Add light preset

## World Operators (bpy.ops.world)

### Creation

- `new()` - Create new world

### Operations

- `new()` - Create new world

## Collection Operators (bpy.ops.collection)

### Management

- `create()` - Create collection
- `remove()` - Remove collection
- `objects_add()` - Add objects to collection
- `objects_remove()` - Remove objects from collection
- `objects_remove_all()` - Remove all objects from collection
- `objects_move_to_active()` - Move objects to active collection
- `objects_move_to_index()` - Move objects to index
- `objects_move_to_other_collection()` - Move to other collection
- `objects_select_all()` - Select all objects in collection
- `objects_deselect_all()` - Deselect all objects in collection
- `link_to_scene()` - Link to scene
- `unlink_from_scene()` - Unlink from scene

## Outliner Operators (bpy.ops.outliner)

### Operations

- `collection_drop()` - Drop collection
- `object_drop()` - Drop object
- `id_copy()` - Copy ID
- `id_paste()` - Paste ID
- `id_delete()` - Delete ID
- `orphans_purge()` - Purge orphans
- `show_active()` - Show active
- `show_one_level()` - Show one level
- `show_hierarchy()` - Show hierarchy

## Constraint Operators (bpy.ops.constraint)

### Creation

- `add()` - Add constraint
- `add_target()` - Add target

### Operations

- `delete()` - Delete constraint
- `limit_reset()` - Reset limit
- `limit_distance_reset()` - Reset distance limit
- `limit_rotation_reset()` - Reset rotation limit
- `limit_scale_reset()` - Reset scale limit

## Modifier Operators (bpy.ops.modifier)

### Creation

- `add()` - Add modifier
- `copy()` - Copy modifier
- `paste()` - Paste modifier

### Operations

- `move_up()` - Move up
- `move_down()` - Move down
- `apply_as_shapekey()` - Apply as shape key
- `apply()` - Apply modifier
- `copy_to_selected()` - Copy to selected
- `remove()` - Remove modifier

## Pose Operators (bpy.ops.pose)

### Operations

- `hide()` - Hide pose
- `reveal()` - Reveal pose
- `select_all()` - Select all
- `deselect_all()` - Deselect all
- `select_linked()` - Select linked
- `select_hierarchy()` - Select hierarchy
- `select_parent()` - Select parent
- `select_grouped()` - Select grouped

### Transform

- `push()` - Push pose
- `relax()` - Relax pose
- `break_down()` - Break down

### Libraries

- `paste()` - Paste pose
- `blend_to_scene()` - Blend to scene
- `blend_to_scene_selected()` - Blend to scene selected
- `select_prev()` - Select previous
- `select_next()` - Select next

## NLA Operators (bpy.ops.nla)

### Operations

- `action_pushdown()` - Push down action
- `action_unlink()` - Unlink action
- `action_clip_add()` - Add action clip
- `transition_add()` - Add transition
- `soundclip_add()` - Add sound clip
- `meta_add()` - Add meta
- `meta_remove()` - Remove meta
- `tracks_delete()` - Delete tracks
- `selected_action_add()` - Add selected action
- `selected_action_clip_add()` - Add selected action clip
- `selected_action_unlink()` - Unlink selected action
- `tweakmode_enter()` - Enter tweak mode
- `tweakmode_exit()` - Exit tweak mode

## Marker Operators (bpy.ops.marker)

### Operations

- `add()` - Add marker
- `delete()` - Delete marker
- `duplicate()` - Duplicate marker
- `move()` - Move marker
- `rename()` - Rename marker
- `select_all()` - Select all
- `deselect_all()` - Deselect all
- `make_links_scene()` - Make links to scene
- `jump()` - Jump to marker
- `camera_bind()` - Bind to camera

## Mask Operators (bpy.ops.mask)

### Creation

- `new()` - New mask
- `delete()` - Delete mask

### Editing

- `add_feather_vertex()` - Add feather vertex
- `add_feather_vertex_slide()` - Slide feather vertex
- `add_vertex()` - Add vertex
- `add_vertex_slide()` - Slide vertex
- `cyclic_toggle()` - Toggle cyclic
- `duplicate()` - Duplicate mask
- `slide_point()` - Slide point
- `slide_spline_curvature()` - Slide spline curvature
- `switch_direction()` - Switch direction

### Selection

- `select_all()` - Select all
- `deselect_all()` - Deselect all
- `select_less()` - Select less
- `select_more()` - Select more
- `select_next()` - Select next
- `select_prev()` - Select previous

## Paint Operators (bpy.ops.paint)

### Operations

- `weight_from_bones()` - Weight from bones
- `weight_sample()` - Sample weight
- `weight_sample_group()` - Sample weight group
- `weight_set()` - Set weight
- `weight_gradient()` - Weight gradient
- `weight_bone_limit()` - Bone weight limit
- `weight_bone_normalize()` - Normalize bone weight
- `weight_bone_auto_normalize()` - Auto normalize bone weight
- `weight_bone_remove()` - Remove bone weight
- `weight_vgroup_normalize()` - Normalize vertex group weight
- `weight_vgroup_remove()` - Remove vertex group weight
- `weight_vgroup_select()` - Select vertex group
- `weight_vgroup_deselect()` - Deselect vertex group
- `weight_vgroup_lock()` - Lock vertex group
- `weight_vgroup_unlock()` - Unlock vertex group
- `weight_vgroup_assign()` - Assign vertex group
- `weight_vgroup_unassign()` - Unassign vertex group
- `weight_vgroup_select_all()` - Select all vertex groups
- `weight_vgroup_deselect_all()` - Deselect all vertex groups
- `weight_vgroup_lock_all()` - Lock all vertex groups
- `weight_vgroup_unlock_all()` - Unlock all vertex groups
- `weight_clean()` - Clean weight
- `weight_normalize_all()` - Normalize all weights
- `weight_set()` - Set weight
- `weight_bake()` - Bake weight
- `weight_sample()` - Sample weight
- `weight_sample_group()` - Sample weight group

## Sculpt Operators (bpy.ops.sculpt)

### Operations

- `symmetrize()` - Symmetrize
- `optimize()` - Optimize
- `dirty_mask()` - Dirty mask

### Tools

- `brush_stroke()` - Brush stroke
- `set_persistent_base()` - Set persistent base

## Grease Pencil Operators (bpy.ops.gpencil)

### Drawing

- `draw()` - Draw
- `draw_on_back()` - Draw on back

### Editing

- `blank_frame_add()` - Add blank frame
- `active_frames_delete_all()` - Delete all active frames
- `frame_duplicate()` - Duplicate frame
- `frame_delete()` - Delete frame
- `layer_add()` - Add layer
- `layer_remove()` - Remove layer
- `layer_move()` - Move layer
- `layer_merge()` - Merge layer
- `layer_duplicate()` - Duplicate layer
- `layer_isolate()` - Isolate layer
- `layer_reveal()` - Reveal layer
- `layer_hide()` - Hide layer
- `layer_lock_all()` - Lock all layers
- `layer_unlock_all()` - Unlock all layers

### Stroke Editing

- `stroke_apply_width()` - Apply width
- `stroke_apply_strength()` - Apply strength
- `stroke_apply_hardness()` - Apply hardness
- `stroke_apply_randomness()` - Apply randomness
- `stroke_apply_spacing()` - Apply spacing
- `stroke_apply_thickness()` - Apply thickness
- `stroke_apply_uv_rotation()` - Apply UV rotation

### Selection

- `select_all()` - Select all
- `deselect_all()` - Deselect all
- `select_box()` - Box select
- `select_circle()` - Circle select
- `select_lasso()` - Lasso select
- `select_stroke()` - Stroke select

## Dynamic Paint Operators (bpy.ops.dpaint)

### Operations

- `brush_toggle()` - Toggle brush
- `sample_color()` - Sample color
- `new()` - New dynamic paint

## Vertex Paint Operators (bpy.ops.vertex_paint)

### Operations

- `draw()` - Draw
- `sample()` - Sample

## Texture Paint Operators (bpy.ops.texture_paint)

### Operations

- `bucket_fill()` - Bucket fill
- `sample_color()` - Sample color
- `get_color()` - Get color
- `tool_set()` - Set tool

## UV Operators (bpy.ops.uv)

### Mapping

- `reset()` - Reset UV
- `sphere_project()` - Sphere project
- `cylinder_project()` - Cylinder project
- `cube_project()` - Cube project
- `smart_project()` - Smart project
- `lightmap_pack()` - Lightmap pack
- `layout()` - Layout
- `unwrap()` - Unwrap
- `seams_from_islands()` - Seams from islands
- `seams_from_islands_mark()` - Mark seams from islands

### Editing

- `add()` - Add UV
- `remove()` - Remove UV
- `select_all()` - Select all
- `deselect_all()` - Deselect all
- `select_loop()` - Select loop
- `select_edge_ring()` - Select edge ring
- `select_linked()` - Select linked
- `select_pinned()` - Select pinned
- `weld()` - Weld UV
- `snap_cursor()` - Snap cursor
- `cursor_set()` - Set cursor
- `pack_islands()` - Pack islands

## Cloth Operators (bpy.ops.cloth)

### Simulation

- `preset_add()` - Add preset
- `preset_add_experimental()` - Add experimental preset

## Fluid Operators (bpy.ops.fluid)

### Simulation

- `preset_add()` - Add preset
- `preset_add_experimental()` - Add experimental preset

## Smoke Operators (bpy.ops.smoke)

### Simulation

- `preset_add()` - Add preset
- `preset_add_experimental()` - Add experimental preset

## Soft Body Operators (bpy.ops.soft_body)

### Simulation

- `preset_add()` - Add preset
- `preset_add_experimental()` - Add experimental preset

## Rigid Body Operators (bpy.ops.rigidbody)

### Simulation

- `preset_add()` - Add preset
- `preset_add_experimental()` - Add experimental preset

## Particle Operators (bpy.ops.particle)

### Creation

- `new()` - New particle system
- `delete()` - Delete particle system

### Editing

- `preset_add()` - Add preset
- `preset_add_experimental()` - Add experimental preset

## Cycles Operators (bpy.ops.cycles)

### Rendering

- `denoise_toggle()` - Toggle denoise
- `use_shading_nodes()` - Use shading nodes

## Edit Operators (bpy.ops.ed)

### Editing

- `undo()` - Undo
- `redo()` - Redo
- `undo_push()` - Push undo
- `redo_repeat()` - Repeat redo

## Transform Operators (bpy.ops.transform)

### Transformations

- `translate()` - Translate
- `rotate()` - Rotate
- `resize()` - Resize
- `scale()` - Scale
- `mirror()` - Mirror
- `bend()` - Bend
- `shear()` - Shear
- `push_pull()` - Push/pull
- `tosphere()` - To sphere
- `tosphere_gizmo()` - To sphere gizmo
- `skin_resize()` - Skin resize
- `edge_bevel()` - Edge bevel
- `edge_slide()` - Edge slide
- `vertex_slide()` - Vertex slide
- `vertex_random()` - Randomize vertices
- `vertex_smooth()` - Smooth vertices
- `transform()` - Transform
- `create_orientation()` - Create orientation
- `delete_orientation()` - Delete orientation

## Boid Operators (bpy.ops.boid)

### Creation

- `add()` - Add boid system

### Editing

- `preset_add()` - Add preset

## Metaball Operators (bpy.ops.mball)

### Creation

- `add_meta_ball()` - Add metaball

### Editing

- `delete_metaelems()` - Delete meta elements

## Lattice Operators (bpy.ops.lattice)

### Creation

- `add()` - Add lattice

### Editing

- `select_all()` - Select all
- `make_regular()` - Make regular

## Curve Operators (bpy.ops.curve)

### Creation

- `primitive_bezier_curve_add()` - Add bezier curve
- `primitive_bezier_circle_add()` - Add bezier circle
- `primitive_nurbs_curve_add()` - Add NURBS curve
- `primitive_nurbs_circle_add()` - Add NURBS circle
- `primitive_nurbs_path_add()` - Add NURBS path

### Editing

- `delete()` - Delete curve
- `duplicate()` - Duplicate curve
- `separate()` - Separate curve
- `cyclic_toggle()` - Toggle cyclic
- `decimate()` - Decimate curve
- `subdivide()` - Subdivide curve
- `smooth()` - Smooth curve
- `smooth_radius()` - Smooth radius
- `smooth_tilt()` - Smooth tilt
- `bevel()` - Bevel curve
- `extrude()` - Extrude curve
- `draw()` - Draw curve
- `pen()` - Pen curve
- `radius_set()` - Set radius

## Surface Operators (bpy.ops.surface)

### Creation

- `primitive_nurbs_surface_curve_add()` - Add NURBS surface curve
- `primitive_nurbs_surface_torus_add()` - Add NURBS surface torus
- `primitive_nurbs_surface_sphere_add()` - Add NURBS surface sphere
- `primitive_nurbs_surface_cylinder_add()` - Add NURBS surface cylinder
- `primitive_nurbs_surface_cube_add()` - Add NURBS surface cube
- `primitive_nurbs_surface_monkey_add()` - Add NURBS surface monkey

### Editing

- `resolution_set()` - Set resolution

## Font Operators (bpy.ops.font)

### Operations

- `open()` - Open font
- `unload()` - Unload font

## Palette Operators (bpy.ops.palette)

### Operations

- `add()` - Add palette
- `delete()` - Delete palette

## Brush Operators (bpy.ops.brush)

### Operations

- `add()` - Add brush
- `curve_preset_add()` - Add curve preset
- `reset()` - Reset brush

## Buttons Operators (bpy.ops.buttons)

### Operations

- `directed_box()` - Directed box
- `directed_circle()` - Directed circle
- `directed_cone()` - Directed cone
- `directed_cylinder()` - Directed cylinder

## Clip Operators (bpy.ops.clip)

### Editing

- `delete()` - Delete clip
- `select_all()` - Select all
- `select_box()` - Box select
- `select_circle()` - Circle select
- `select_lasso()` - Lasso select

## Action Operators (bpy.ops.action)

### Operations

- `new()` - New action
- `pushdown()` - Push down
- `unlink()` - Unlink
- `clean()` - Clean
- `clean_unused()` - Clean unused
- `layer_add()` - Add layer
- `layer_remove()` - Remove layer
- `layer_next()` - Next layer
- `layer_prev()` - Previous layer
- `layer_activate()` - Activate layer
- `layer_move()` - Move layer

## Pose Library Operators (bpy.ops.poselib)

### Operations

- `browse_interactive()` - Browse interactive
- `apply_pose()` - Apply pose
- `add()` - Add pose
- `unlink()` - Unlink pose

## Graph Operators (bpy.ops.graph)

### Editing

- `delete()` - Delete keyframes
- `duplicate()` - Duplicate keyframes
- `copy()` - Copy keyframes
- `paste()` - Paste keyframes
- `select_all()` - Select all
- `deselect_all()` - Deselect all
- `select_box()` - Box select
- `select_circle()` - Circle select
- `select_lasso()` - Lasso select
- `select_column_markers()` - Select column markers
- `select_leftright()` - Select left/right
- `select_more()` - Select more
- `select_less()` - Select less
- `view_all()` - View all
- `zoom()` - Zoom
- `zoom_in()` - Zoom in
- `zoom_out()` - Zoom out
- `pan()` - Pan

## Info Operators (bpy.ops.info)

### Operations

- `add_directed()` - Add directed
- `report_display()` - Display report

## Preferences Operators (bpy.ops.preferences)

### Operations

- `add()` - Add preferences
- `remove()` - Remove preferences
- `context_toggle()` - Toggle context
- `activate_section()` - Activate section
- `use_experiment_add()` - Add use experiment

## Workspace Operators (bpy.ops.workspace)

### Operations

- `add()` - Add workspace
- `delete()` - Delete workspace
- `duplicate()` - Duplicate workspace
- `append_activate()` - Append activate

## Asset Operators (bpy.ops.asset)

### Operations

- `open()` - Open asset
- `clear()` - Clear asset
- `library_refresh()` - Refresh library

## Gizmo Group Operators (bpy.ops.gizmogroup)

### Operations

- `add()` - Add gizmo group

## Armature Operators (bpy.ops.armature)

### Creation

- `add()` - Add armature

### Editing

- `select_all()` - Select all
- `deselect_all()` - Deselect all
- `select_hierarchy()` - Select hierarchy
- `select_linked()` - Select linked
- `select_less()` - Select less
- `select_more()` - Select more
- `select_parent()` - Select parent
- `select_mirror()` - Select mirror
- `align()` - Align

## Export Animation Operators (bpy.ops.export_anim)

### Export

- `fbx()` - Export FBX animation

## Export Scene Operators (bpy.ops.export_scene)

### Export

- `fbx()` - Export FBX scene
- `obj()` - Export OBJ scene
- `gltf()` - Export glTF scene

## Import Animation Operators (bpy.ops.import_anim)

### Import

- `fbx()` - Import FBX animation

## Import Curve Operators (bpy.ops.import_curve)

### Import

- `svg()` - Import SVG curve

## Import Scene Operators (bpy.ops.import_scene)

### Import

- `fbx()` - Import FBX scene
- `obj()` - Import OBJ scene
- `gltf()` - Import glTF scene
- `collada()` - Import Collada scene

## Point Cache Operators (bpy.ops.ptcache)

### Operations

- `bake()` - Bake point cache
- `bake_all()` - Bake all point caches

## Cache File Operators (bpy.ops.cachefile)

### Operations

- `open()` - Open cache file

## Extensions Operators (bpy.ops.extensions)

### Operations

- `update()` - Update extensions
- `update_all()` - Update all extensions
- `update_ui()` - Update UI extensions
- `repo_sync()` - Sync repository
- `repo_sync_all()` - Sync all repositories
- `install()` - Install extension
- `uninstall()` - Uninstall extension
- `disable()` - Disable extension
- `enable()` - Enable extension
- `show()` - Show extension

## Point Cloud Operators (bpy.ops.pointcloud)

### Operations

- `add()` - Add point cloud

## Spreadsheet Operators (bpy.ops.spreadsheet)

### Operations

- `add()` - Add spreadsheet
- `delete()` - Delete spreadsheet

## Text Editor Operators (bpy.ops.text_editor)

### Operations

- `find()` - Find
- `replace()` - Replace
- `jump()` - Jump
- `line_number()` - Line number

## Sequencer Operators (bpy.ops.sequencer)

### Editing

- `cut()` - Cut
- `copy()` - Copy
- `paste()` - Paste
- `delete()` - Delete
- `duplicate()` - Duplicate
- `split()` - Split
- `gap_insert()` - Insert gap
- `gap_remove()` - Remove gap
- `meta_toggle()` - Toggle meta
- `meta_make()` - Make meta
- `meta_separate()` - Separate meta
- `mute_toggle()` - Toggle mute
- `unmute()` - Unmute
- `lock_toggle()` - Toggle lock
- `unlock()` - Unlock
- `select_all()` - Select all
- `select_handles_all()` - Select all handles
- `select_side_of_frame()` - Select side of frame
- `select_box()` - Box select
- `select_linked_pick()` - Pick linked
- `select_linked_extend()` - Extend linked selection
- `select_more()` - Select more
- `select_less()` - Select less
- `select_active_side()` - Select active side
- `select_grouped()` - Select grouped
- `view_frame()` - View frame
- `view_zoom()` - Zoom view
- `view_all()` - View all
- `refresh_all()` - Refresh all
- `rebuild_proxy()` - Rebuild proxy
- `export_subtitles()` - Export subtitles
- `import_subtitles()` - Import subtitles

## Sculpt Curves Operators (bpy.ops.sculpt_curves)

### Operations

- `brush_stroke()` - Brush stroke

## Paint Curve Operators (bpy.ops.paintcurve)

### Operations

- `new()` - New paint curve
- `add()` - Add paint curve
- `delete()` - Delete paint curve
- `select_all()` - Select all
- `deselect_all()` - Deselect all

## Surface Operators (bpy.ops.surface)

### Operations

- `add()` - Add surface
- `delete()` - Delete surface

## Transform Operators (bpy.ops.transform)

### Operations

- `create_orientation()` - Create orientation
- `delete_orientation()` - Delete orientation


---
## bpy_ops_summary.md

# bpy.ops - Blender Python Operations Module Reference

This document provides an overview of all bpy.ops submodules that have been documented in the skills package.

## Overview

The `bpy.ops` module contains all operator functions in Blender's Python API. Operators are actions that can be performed in Blender, such as creating objects, manipulating data, and executing commands.

## Documented Submodules

### High Priority Modules

1. **[bpy_ops_text_module.md](bpy_ops_text_module.md)** - Text Operations
   - Text object creation and editing
   - Selection and manipulation
   - Style and formatting

2. **[bpy_ops_import_scene_module.md](bpy_ops_import_scene_module.md)** - Scene Import Operations
   - Import FBX, OBJ, and other 3D formats
   - Import settings and options
   - Batch import functionality

3. **[bpy_ops_export_scene_module.md](bpy_ops_export_scene_module.md)** - Scene Export Operations
   - Export to GLTF, FBX, and other formats
   - Export settings and options
   - Batch export functionality

4. **[bpy_ops_font_module.md](bpy_ops_font_module.md)** - Font Operations
   - Text object creation and editing
   - Font selection and properties
   - Text styling and effects

5. **[bpy_ops_uv_module.md](bpy_ops_uv_module.md)** - UV Operations
   - UV selection and editing
   - UV projection and unwrapping
   - UV layout management

### Medium Priority Modules

6. **[bpy_ops_sound_module.md](bpy_ops_sound_module.md)** - Sound Operations
   - Audio playback and control
   - Volume and pitch management
   - Audio sequence editing

7. **[bpy_ops_cachefile_module.md](bpy_ops_cachefile_module.md)** - Cache File Operations
   - Alembic cache import/export
   - Cache management and playback
   - Cache optimization

8. **[bpy_ops_info_module.md](bpy_ops_info_module.md)** - Info Area Operations
   - Report management
   - System diagnostics
   - Information display

9. **[bpy_ops_lattice_module.md](bpy_ops_lattice_module.md)** - Lattice Operations
   - Lattice creation and editing
   - Deformation tools
   - Lattice animation

10. **[bpy_ops_marker_module.md](bpy_ops_marker_module.md)** - Timeline Marker Operations
    - Marker creation and management
    - Scene organization
    - Audio sync tools

11. **[bpy_ops_view2d_module.md](bpy_ops_view2d_module.md)** - 2D View Operations
    - View navigation
    - Text editor tools
    - Image viewer controls

12. **[bpy_ops_palette_module.md](bpy_ops_palette_module.md)** - Palette Operations
    - Color palette management
    - Color manipulation
    - Color scheme creation

13. **[bpy_ops_surface_module.md](bpy_ops_surface_module.md)** - Surface Operations
    - NURBS surface creation
    - Surface modeling tools
    - Curve operations

14. **[bpy_ops_uilist_module.md](bpy_ops_uilist_module.md)** - UI List Operations
    - UI list management
    - Collection management
    - Data block operations

15. **[bpy_ops_script_module.md](bpy_ops_script_module.md)** - Script Operations
    - Python script execution
    - Automation library
    - Batch processing

## Module Categories

### Import/Export Modules
- `bpy.ops.import_scene` - Import 3D formats
- `bpy.ops.export_scene` - Export 3D formats
- `bpy.ops.cachefile` - Cache file operations

### Editor Modules
- `bpy.ops.text` - Text editor
- `bpy.ops.uv` - UV editor
- `bpy.ops.view2d` - 2D views

### Object Modules
- `bpy.ops.font` - Text objects
- `bpy.ops.lattice` - Lattice objects
- `bpy.ops.surface` - Surface objects

### Media Modules
- `bpy.ops.sound` - Audio operations
- `bpy.ops.marker` - Timeline markers

### Utility Modules
- `bpy.ops.palette` - Color palettes
- `bpy.ops.uilist` - UI lists
- `bpy.ops.script` - Python scripts
- `bpy.ops.info` - Information

## Common Patterns

### Operator Execution
```python
import bpy

# Execute operator
bpy.ops.object.add()

# Execute with parameters
bpy.ops.object.delete(confirm=False)

# Check context before execution
if bpy.context.active_object:
    bpy.ops.object.modifier_apply()
```

### Context Handling
```python
# Set context for operation
with bpy.context.temp_override(area=area, region=region):
    bpy.ops.view2d.zoom()
```

### Error Handling
```python
try:
    bpy.ops.object.delete()
except RuntimeError as e:
    print(f"Delete failed: {e}")
```

## Best Practices

1. **Check Context**: Always check if the required context is available
2. **Use temp_override**: Use context temp_override for safe execution
3. **Handle Errors**: Wrap operators in try-except blocks
4. **Use Report**: Use bpy.ops.info.report_* for user feedback
5. **Batch Operations**: Group multiple operations together

## Additional Resources

- Blender Python API Documentation
- Official Blender Operator Reference
- Community Examples and Tutorials

## Notes

This documentation is part of the Blender Python Skills Package and is designed to provide practical examples and workflows for each submodule.

Last Updated: 2024


---
## bpy_ops_detailed

# bpy.ops - Blender Operations Reference

Blender 操作符完整参考，涵盖所有可用的操作命令和调用方式。

## Overview

`bpy.ops` 模块包含 Blender 的所有操作符（Operators），用于执行各种操作如创建对象、编辑网格、渲染场景等。操作符是 Blender 命令系统的核心。

## 操作符分类

### 网格操作 (mesh)

```python
import bpy

# 创建几何体
bpy.ops.mesh.primitive_cube_add(size=1, location=(0, 0, 0))
bpy.ops.mesh.primitive_cube_add(size=1)  # 在原点创建立方体

bpy.ops.mesh.primitive_uv_sphere_add(radius=1, segments=32, ring_count=16)
bpy.ops.mesh.primitive_cylinder_add(radius=1, depth=2, vertices=32)
bpy.ops.mesh.primitive_cone_add(radius1=1, depth=2, vertices=32)
bpy.ops.mesh.primitive_torus_add(major_radius=1, minor_radius=0.25, major_segments=48, minor_segments=12)
bpy.ops.mesh.primitive_circle_add(radius=1, fill_type='NGON', vertices=32)
bpy.ops.mesh.primitive_grid_add(x_subdivisions=10, y_subdivisions=10, size=2)
bpy.ops.mesh.primitive_monkey_add()

# 网格编辑操作
bpy.ops.mesh.select_all(action='SELECT')      # 全选
bpy.ops.mesh.select_all(action='DESELECT')    # 全不选
bpy.ops.mesh.select_all(action='INVERT')      # 反选
bpy.ops.mesh.select_all(action='TOGGLE')      # 切换选择

bpy.ops.mesh.delete(type='VERT')              # 删除顶点
bpy.ops.mesh.delete(type='EDGE')              # 删除边
bpy.ops.mesh.delete(type='FACE')              # 删除面
bpy.ops.mesh.delete(type='EDGE_FACE')         # 删除边和面
bpy.ops.mesh.delete(type='ONLY_FACE')         # 仅删除面

bpy.ops.mesh.subdivide(number_cuts=1)         # 细分
bpy.ops.mesh.subdivide(number_cuts=2, smoothness=1.0)

bpy.ops.mesh.merge(type='CENTER')             # 合并
bpy.ops.mesh.merge(type='CURSOR')             # 合并到光标
bpy.ops.mesh.merge(type='COLLAPSE')           # 折叠

bpy.ops.mesh.edge_face_add()                  # 添加边/面
bpy.ops.mesh.fill()                           # 填充
bpy.ops.mesh.bevel(offset=0.1, segments=3)    # 倒角

# 变换操作
bpy.ops.transform.translate(value=(1, 0, 0))  # 移动
bpy.ops.transform.rotate(value=0.785)         # 旋转
bpy.ops.transform.resize(value=(1.5, 1.5, 1.5))  # 缩放

# 切变和扭曲
bpy.ops.transform.shear(value=(0, 1, 0))
bpy.ops.transform.distort(type='SPHERE')

# 网格清理
bpy.ops.mesh.decimate(ratio=0.5)              # 三角化简化
bpy.ops.mesh.dissolve_verts()                 # 溶解顶点
bpy.ops.mesh.dissolve_edges()                 # 溶解边
bpy.ops.mesh.dissolve_faces()                 # 溶解面
bpy.ops.mesh.dissolve_limited(angle_limit=0.1)  # 有限角度溶解

# 法线和材质
bpy.ops.mesh.normals_make_consistent(inside=False)
bpy.ops.mesh.flip_normals()
bpy.ops.mesh.set_normals_from_faces()

# 分离和连接
bpy.ops.mesh.separate(type='SELECTED')        # 分离选定
bpy.ops.mesh.separate(type='MATERIAL')        # 按材质分离
bpy.ops.mesh.separate(type='LOOSE')           # 分离 loose 部分
bpy.ops.mesh.join()                           # 连接

# 挤出和内缩
bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value":(0, 0, 1)})
bpy.ops.mesh.extrude_edges_move(TRANSFORM_OT_translate={"value":(0, 0, 1)})
bpy.ops.mesh.inset_individual(use_individual=True)
bpy.ops.mesh.inset_use_outset(False)
```

### 对象操作 (object)

```python
# 对象创建
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.select_all(action='DESELECT')
bpy.ops.object.select_all(action='INVERT')

bpy.ops.object.delete(use_global=False)       # 删除对象

# 复制
bpy.ops.object.duplicate(linked=False, mode='TRANSLATION')
bpy.ops.object.duplicate(linked=True)         # 关联复制

# 变换
bpy.ops.object.transform_apply(location=False, rotation=True, scale=False)

# 变换模式
bpy.ops.object.mode_set(mode='OBJECT')
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.object.mode_set(mode='SCULPT')
bpy.ops.object.mode_set(mode='VERTEX_PAINT')
bpy.ops.object.mode_set(mode='WEIGHT_PAINT')
bpy.ops.object.mode_set(mode='TEXTURE_PAINT')

# 变换操作
bpy.ops.object.origin_set(type='ORIGIN_GEOMETRY', center='MEDIAN')
bpy.ops.object.origin_set(type='ORIGIN_CURSOR', center='MEDIAN')
bpy.ops.object.origin_set(type='ORIGIN_CENTER_OF_MASS')

# 对齐
bpy.ops.object.align(objects=[], align_mode='OPT_1', align_axis='X')

# 父子关系
bpy.ops.object.parent_set()
bpy.ops.object.parent_clear(type='CLEAR_KEEP_TRANSFORM')
bpy.ops.object.parent_clear(type='CLEAR')

# 约束
bpy.ops.object.constraint_add(type='COPY_LOCATION')
bpy.ops.object.constraint_add(type='COPY_ROTATION')
bpy.ops.object.constraint_add(type='COPY_SCALE')
bpy.ops.object.constraint_add(type='LOOK_AT')
bpy.ops.object.constraint_add(type='TRACK_TO')
bpy.ops.object.constraint_add(type='IK')
bpy.ops.object.constraint_add(type='COPY_TRANSFORMS')

# 修改器
bpy.ops.object.modifier_add(type='SUBSURF')
bpy.ops.object.modifier_add(type='ARMATURE')
bpy.ops.object.modifier_add(type='MIRROR')
bpy.ops.object.modifier_add(type='ARRAY')
bpy.ops.object.modifier_add(type='BEVEL')
bpy.ops.object.modifier_add(type='BOOLEAN')
bpy.ops.object.modifier_add(type='DECIMATE')
bpy.ops.object.modifier_add(type='DISPLACE')
bpy.ops.object.modifier_add(type='SOLIDIFY')
bpy.ops.object.modifier_remove(modifier="Subsurf")

# 应用修改器
bpy.ops.object.modifier_apply(modifier="Subsurf")
bpy.ops.object.modifier_apply_as_shapekey(modifier="Subsurf")

# 集合操作
bpy.ops.object.collection_link(collection=bpy.data.collections[0])
bpy.ops.object.collection_unlink(collection=bpy.data.collections[0])

# 可见性
bpy.ops.object.hide_view_set(unselected=False)
bpy.ops.object.hide_view_clear()
bpy.ops.object.hide_select_set(unselected=False)
```

### 曲线操作 (curve)

```python
# 创建曲线
bpy.ops.curve.primitive_bezier_curve_add(radius=1)
bpy.ops.curve.primitive_bezier_circle_add(radius=1)
bpy.ops.curve.primitive_nurbs_path_add(points=4)
bpy.ops.curve.primitive_nurbs_circle_add()
bpy.ops.curve.primitive_nurbs_curve_add()

# 曲线编辑
bpy.ops.curve.subdivide(number_cuts=1)
bpy.ops.curve.delete(type='VERT')
bpy.ops.curve.duplicate()

# 曲线类型
bpy.ops.curve.cycloid_offset()
bpy.ops.curve.cycloid_offset_add(lower=True)

# 倒角
bpy.ops.curve.bevel_depth_set(0.1)
bpy.ops.curve.bevel_profile(path='Vertex')

# 曲线到网格
bpy.ops.curve.reveal()
bpy.ops.curve.hide(unselected=False)
```

###  armature 操作

```python
# 创建骨骼
bpy.ops.armature.simple_add(length=1)

# 骨骼选择
bpy.ops.armature.select_all(action='SELECT')
bpy.ops.armature.select_hierarchy(direction='PARENT', extend=False)
bpy.ops.armature.select_hierarchy(direction='CHILD', extend=True)

# 骨骼操作
bpy.ops.armature.delete()
bpy.ops.armature.duplicate()
bpy.ops.armature.subdivide(number_cuts=1)

# 骨骼变换
bpy.ops.transform.translate()
bpy.ops.transform.rotate()
bpy.ops.transform.resize()

# 骨骼设置
bpy.ops.armature.autoside_names(axis='XAXIS')
bpy.ops.armature.flip_names()

# 命名规范
bpy.armature.data.bones[0].name = "Bone"
```

### 材质操作 (material)

```python
# 创建材质
bpy.ops.material.new()
bpy.ops.object.material_slot_add()
bpy.ops.object.material_slot_remove()

# 分配材质
bpy.ops.object.material_slot_assign()
bpy.ops.object.material_slot_select()
bpy.ops.object.material_slot_deselect()

# 材质预览
bpy.ops.material.preview_refresh()
```

### 纹理操作 (texture)

```python
# 创建纹理
bpy.ops.texture.new()

# 纹理预览
bpy.ops.texture.preview_refresh()
```

### 渲染操作 (render)

```python
# 渲染
bpy.ops.render.render(write_still=False, use_viewport=False)
bpy.ops.render.render(write_still=True)  # 保存图像

# 动画渲染
bpy.ops.render.render(animation=True)

# 区域渲染
bpy.ops.render.render_border(xmin=0, xmax=100, ymin=0, ymax=100)

# 渲染设置
bpy.ops.render.open_render(filepath="//render.png")

# 视口渲染
bpy.ops.render.view_show()
bpy.ops.render.render(write_still=False, use_viewport=True)
```

### 场景操作 (scene)

```python
# 场景管理
bpy.ops.scene.new(type='EMPTY')
bpy.ops.scene.new(type='LINK_COPY')
bpy.ops.scene.new(type='FULL_COPY')

# 删除场景
bpy.ops.scene.delete()

# 场景设置
bpy.ops.scene.unit_system_assume()

# 清理
bpy.ops.scene.remove_empty()
```

### 文件操作 (file)

```python
# 保存
bpy.ops.wm.save_mainfile(filepath="//test.blend")
bpy.ops.wm.save_as_mainfile(filepath="//test.blend")
bpy.ops.wm.revert_mainfile()

# 导入
bpy.ops.wm.import_mesh(filepath="//model.obj")
bpy.ops.wm.import_curve(filepath="//model.svg")
bpy.ops.wm.import_scene(filepath="//model.fbx")

# 导出
bpy.ops.wm.export_mesh(filepath="//model.obj")
bpy.ops.wm.export_curve(filepath="//model.svg")
bpy.ops.wm.export_scene(filepath="//model.fbx")
```

### 节点操作 (node)

```python
# 节点编辑
bpy.ops.node.select_all(action='SELECT')
bpy.ops.node.select_all(action='DESELECT')
bpy.ops.node.select_all(action='INVERT')

# 节点操作
bpy.ops.node.delete()
bpy.ops.node.duplicate()
bpy.ops.node.copy()

# 链接操作
bpy.ops.node.links_detach()
bpy.ops.node.links_mute(data_path='//')

# 视图操作
bpy.ops.node.view_all()
bpy.ops.node.view_selected()
```

### 动画操作 (anim)

```python
# 关键帧
bpy.ops.keyframe_insert(type='Location')
bpy.ops.keyframe_insert(type='Rotation')
bpy.ops.keyframe_insert(type='Scaling')
bpy.ops.keyframe_insert(type='ALL')

# 删除关键帧
bpy.ops.keyframe_delete(type='Location')
bpy.ops.keyframe_delete(type='ALL')

# 播放控制
bpy.ops.screen.frame_jump(end=False)
bpy.ops.screen.frame_offset(delta=1)
bpy.ops.screen.animation_play()

# NLA 操作
bpy.ops.nla.action_push_down()
bpy.ops.nla.tracks_add()
bpy.ops.nla.tracks_delete()
```

### 3D 视图操作 (view3d)

```python
# 视图导航
bpy.ops.view3d.view_axis(axis='TOP', align_quaternion=(1, 0, 0, 0))
bpy.ops.view3d.view_axis(axis='FRONT')
bpy.ops.view3d.view_axis(axis='RIGHT')

# 相机视图
bpy.ops.view3d.camera_to_view()
bpy.ops.view3d.view_camera()

# 视口设置
bpy.ops.view3d.view_persportho()
bpy.ops.view3d.localview(frame_selected=False)

# 区域选择
bpy.ops.view3d.select_border(xmin=0, xmax=100, ymin=0, ymax=100, extend=False)
bpy.ops.view3d.select_circle(x=100, y=100, radius=10, mode='SET')

# 栅格和吸附
bpy.ops.view3d.snap_cursor_to_selected()
bpy.ops.view3d.snap_cursor_to_center()
bpy.ops.view3d.snap_selected_to_grid()
bpy.ops.view3d.snap_selected_to_cursor()
```

### UI 操作 (ui)

```python
# 撤销/重做
bpy.ops.ed.undo()
bpy.ops.ed.redo()
bpy.ops.ed.undo_history(item=0)

# 复制粘贴
bpy.ops.ui.copy_buffer()
bpy.ops.ui.paste_buffer()

# 自动保存
bpy.ops.ui.auto_repeat_undo()
```

### 文本操作 (text)

```python
# 文本编辑器
bpy.ops.text.new()
bpy.ops.text.open(filepath="//script.py")
bpy.ops.text.save()
bpy.ops.text.save_as(filepath="//script_copy.py")

# 文本编辑
bpy.ops.text.move(type='LINE_BEGIN')
bpy.ops.text.move(type='LINE_END')
bpy.ops.text.delete(type='NEXT_CHARACTER')

# 运行脚本
bpy.ops.text.run_script()
```

### 2D 视图操作 (view2d)

```python
# 2D 视图导航
bpy.ops.view2d.scroller_activate()
bpy.ops.view2d.scroll_left(delta=1)
bpy.ops.view2d.scroll_down(delta=1)

# 视图缩放
bpy.ops.view2d.zoom(delta=1)
bpy.ops.view2d.zoom_in()
bpy.ops.view2d.zoom_out()
```

## 操作符调用模式

### 基本调用

```python
import bpy

# 简单调用
bpy.ops.mesh.primitive_cube_add()

# 带参数调用
bpy.ops.mesh.primitive_uv_sphere_add(radius=2, segments=64, ring_count=32)

# 使用字典传递参数
bpy.ops.transform.translate({"value": (1.0, 0.0, 0.0)})
```

### 返回值处理

```python
result = bpy.ops.mesh.primitive_cube_add()
print(f"结果: {result}")

# 检查是否成功
if result == {'FINISHED'}:
    print("操作成功")
elif result == {'CANCELLED'}:
    print("操作取消")
elif result == {'RUNNING_MODAL'}:
    print("操作正在运行")

# 处理可能的错误
try:
    bpy.ops.object.delete()
except Exception as e:
    print(f"删除失败: {e}")
```

### 操作上下文

```python
# 使用操作符上下文
with bpy.context.temp_override(
    active_object=obj,
    selected_objects=[obj1, obj2]
):
    bpy.ops.object.join()
```

## 常用操作符组合

### 创建并设置对象

```python
def create_configured_cube(name, location, size, material=None):
    """创建配置好的立方体"""
    bpy.ops.mesh.primitive_cube_add(size=size, location=location)
    obj = bpy.context.active_object
    obj.name = name
    
    if material:
        obj.data.materials.append(material)
    
    return obj
```

### 批量处理对象

```python
def batch_transform_objects(translation=(0, 0, 1)):
    """批量变换对象"""
    bpy.ops.object.select_all(action='DESELECT')
    
    for obj in bpy.context.scene.objects:
        if obj.select_get():
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            bpy.ops.transform.translate(value=translation)
    
    bpy.ops.object.select_all(action='DESELECT')
```

## 最佳实践

### 1. 正确设置活动对象
```python
bpy.context.view_layer.objects.active = obj
obj.select_set(True)
```

### 2. 使用适当的选择模式
```python
bpy.ops.object.select_all(action='DESELECT')
for obj in target_objects:
    obj.select_set(True)
```

### 3. 处理操作符返回值
```python
result = bpy.ops.mesh.subdivide()
if 'FINISHED' not in result:
    print(f"操作失败: {result}")
```

### 4. 使用 bpy.ops 的注意事项
- bpy.ops 是 Blender 命令的包装器
- 操作符调用会改变上下文状态
- 频繁调用可能影响性能
- 复杂操作考虑使用 bmesh API

## 相关模块

- [bpy.context](bpy_context_module.md) - 上下文访问
- [bpy.types](bpy_types_detailed_module.md) - 数据类型
- [bpy.data](bpy_data_module.md) - 数据访问


---
## bpy_5_1_api_changes.md

# Blender 5.1 Python API 变更参考

本文档介绍 Blender 5.1 版本中 Python API 的变更内容。

## Python 版本升级

Blender 5.1 将 Python 升级到版本 3.13，与 VFX 平台 2026 保持一致。

## 新增功能

### bpy.app.cachedir

返回 Blender 使用的缓存目录。

**类型**: `str`

```python
import bpy
print(f"缓存目录: {bpy.app.cachedir}")
```

### bpy.app.handlers.exit_pre

新增退出前处理器,允许在 Blender 退出时运行 Python 代码。

```python
import bpy

def exit_callback():
    print("Blender 正在退出...")
    # 执行清理操作

bpy.app.handlers.exit_pre.append(exit_callback)
```

### window.find_playing_scene()

查找当前正在播放的场景。

**返回**: `bpy.types.Window` 或 None

```python
import bpy

scene = bpy.context.window.find_playing_scene()
if scene:
    print(f"正在播放的场景: {scene.name}")
```

### windows.find_playing()

查找正在播放场景的窗口。

**返回**: `bpy.types.WindowManager.windows` 集合

```python
import bpy

windows = bpy.context.windows.find_playing()
for window in windows:
    print(f"窗口: {window.screen.name}")
```

### UILayout.template_ID_session_uid

新增模板 ID 搜索菜单,允许使用 ID 的 session uid 作为 int 输入值。

```python
import bpy

layout = bpy.context.layout
layout.template_ID_session_uid(bpy.context.scene, "active_object")
```

### gpu.types.GPUFrameBuffer.read_color 和 read_depth

读取颜色和深度数据时,越界会抛出 ValueError。

```python
import bpy
import gpu

fb = gpu.types.GPUFrameBuffer()
try:
    color = fb.read_color(0, 0, 1, 1, 4, 0, 'FLOAT')
except ValueError as e:
    print(f"读取越界: {e}")
```

### strip.content_end

视频序列编辑器中新增的只读属性,用于访问序列内容的结束帧(可能超出右柄)。

**类型**: `int` (只读)

```python
import bpy

strip = bpy.context.active_sequence_strip
print(f"内容结束帧: {strip.content_end}")
```

## API 变更

### mesh.validate()

现在在调用时会修复无效(非有限)的 Multiresolution 切线向量数据。

```python
import bpy

obj = bpy.context.active_object
mesh = obj.data
mesh.validate()  # 现在会修复无效的切线向量数据
```

### UILayout.template_list.columns 参数弃用

columns 参数已被弃用,自 5.0 版本起已失效。

```python
import bpy

layout = bpy.context.layout
layout.template_list("MY_UL_List", "list_id", context, "items", context, "active_index")
# columns 参数已无效,请使用其他布局方式
```

### 笔刷 stroke_method 属性重组

多个笔刷属性已合并为 `brush.stroke_method` 枚举字段:

| 旧属性 | 新枚举值 |
|--------|---------|
| use_airbrush | AIRBRUSH |
| use_anchor | ANCHORED |
| use_space | SPACE |
| use_line | LINE |
| use_curve | CURVE |
| use_restore_mesh | DRAG_DOT |

```python
import bpy

brush = bpy.context.tool_settings.image_paint.brush

# 旧方式(已弃用)
# brush.use_airbrush = True
# brush.use_space = True

# 新方式
brush.stroke_method = 'AIRBRUSH'
```

### sculpt.sample_color 操作符移除

`sculpt.sample_color` 操作符已被移除,其功能已合并到 `paint.sample_color`。

```python
import bpy

# 旧方式(已移除)
# bpy.ops.sculpt.sample_color()

# 新方式
bpy.ops.paint.sample_color()
```

### sequencer.retiming_segment_speed_set.keep_retiming 参数移除

`keep_retiming` 选项已被移除,因为只有设置为 true 时片段才会正确调整时间。设置为 false 仅会添加不需要的钳制,导致错误行为。

## VSE 序列时间属性重命名

视频序列编辑器中的序列时间属性已重命名以提高清晰度:

| 旧属性 | 新属性 | 说明 |
|--------|--------|------|
| frame_final_duration | duration | 最终持续时间 |
| frame_final_start | left_handle | 左手柄位置 |
| frame_final_end | right_handle | 右手柄位置 |
| frame_offset_start | left_handle_offset | 左手柄偏移 |
| frame_offset_end | right_handle_offset | 右手柄偏移 |
| frame_duration | content_duration | 内容持续时间 |
| frame_start | content_start | 内容起始帧 |
| animation_offset_start | content_trim_start | 内容修剪起始 |
| animation_offset_end | content_trim_end | 内容修剪结束 |

```python
import bpy

strip = bpy.context.active_sequence_strip

# 旧方式(仍可用但已弃用)
# duration = strip.frame_final_duration
# left_handle = strip.frame_final_start

# 新方式
duration = strip.duration
left_handle = strip.left_handle
right_handle = strip.right_handle
content_start = strip.content_start
content_end = strip.content_end  # 新增,只读访问内容结束帧
```

## macOS 权限变更

在 macOS 上,脚本和扩展现在可以访问设备摄像头、麦克风和系统音频。用户将看到系统权限对话框。

## Py-defined RNA structs 名称属性覆盖

Py 定义的 RNA struct 现在可以完全覆盖其 'name property'(例如用作集合中的字符串键、UI 小部件如搜索字段等)。

## 迁移指南

### 旧代码到新代码的迁移

#### 笔刷操作迁移

```python
# 旧代码
brush.use_airbrush = True
brush.use_space = True

# 新代码
brush.stroke_method = 'AIRBRUSH'
```

#### VSE 序列属性迁移

```python
# 旧代码
duration = strip.frame_final_duration
start = strip.frame_final_start

# 新代码
duration = strip.duration
start = strip.left_handle
```

#### 序列重时操作迁移

```python
# 旧代码(已移除 keep_retiming 参数)
bpy.ops.sequencer.retiming_segment_speed_set(keep_retiming=False)

# 新代码
bpy.ops.sequencer.retiming_segment_speed_set()
```

#### 采样颜色操作迁移

```python
# 旧代码(已移除)
bpy.ops.sculpt.sample_color()

# 新代码
bpy.ops.paint.sample_color()
```

## 注意事项

1. **向后兼容性**: 旧的 API 在 5.1 中仍可用,但会显示弃用警告。建议尽快迁移到新 API。
2. **Python 3.13**: 确保您的脚本与 Python 3.13 兼容。
3. **macOS 权限**: 在 macOS 上运行脚本时,注意新的权限要求。
4. **GPU FrameBuffer**: 读取数据时添加异常处理以捕获越界错误。


