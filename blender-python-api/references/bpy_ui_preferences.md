# bpy_ui_preferences

---
## bpy_ui_design

# Blender UI/UX 设计指南

本模块整合前端 UI/UX 设计最佳实践到 Blender 插件界面开发中，帮助创建专业、美观且易用的 Blender 插件界面。

## 概述

Blender 插件的界面是用户与工具交互的第一接触点。优秀的 UI/UX 设计能让插件更专业、更易用，大大提升用户体验。遵循前端设计原则，可以避免"AI 生成"的廉价感，创造真正精致的产品级界面。

**核心设计原则：**
- **目的明确**：界面解决什么问题？目标用户是谁？
- **风格鲜明**：选择极端的设计方向——极简主义、极繁主义、复古未来主义、自然有机、奢华精致、活泼趣味等
- **差异化**：什么是让人过目不忘的亮点？
- **一致性**：视觉语言、交互模式、动画效果保持统一

## 核心功能

### bpy.types.UILayout

Blender UI 的核心组件，用于构建插件界面布局。

```python
import bpy

class MyPanel(bpy.types.Panel):
    bl_label = "我的插件面板"
    bl_idname = "OBJECT_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '我的插件'

    def draw(self, context):
        layout = self.layout
        col = layout.column()
        col.operator("object.my_operator", text="执行操作")
        col.prop(context.object, "name")
```

### 布局系统

**列布局 (Column)：**
```python
def draw(self, context):
    layout = self.layout
    col = layout.column()
    col.operator("object.add", text="添加对象")
    col.operator("object.delete", text="删除对象")
    col.separator()
    col.prop(context.object, "scale")
```

**行布局 (Row)：**
```python
def draw(self, context):
    layout = self.layout
    row = layout.row()
    row.operator("object.translate", text="移动")
    row.operator("object.rotate", text="旋转")
    row.operator("object.scale", text="缩放")
```

**列分割 (Split)：**
```python
def draw(self, context):
    layout = self.layout
    split = layout.split()
    col_left = split.column()
    col_right = split.column()
    col_left.label(text="左侧面板")
    col_right.label(text="右侧面板")
```

**折叠框 (Box/Collapsible)：**
```python
def draw(self, context):
    layout = self.layout
    box = layout.box()
    box.label(text="高级设置")
    box.prop(context.scene, "my_enum", text="选项")
    box.prop(context.scene, "my_float", text="数值")
```

**标签页 (Tab)：**
```python
def draw(self, context):
    layout = self.layout
    tab = layout.tab_set("my_tabs", default='tab1')
    
    tab.tab("基础设置", active=True)
    col = tab.column()
    col.prop(context.scene, "my_prop")
    
    tab.tab("高级设置")
    col = tab.column()
    col.prop(context.scene, "my_advanced_prop")
```

## 实用函数

### 自定义 UI 元素类

```python
import bpy
from bpy.props import EnumProperty, FloatProperty, BoolProperty

class MySettings(bpy.types.PropertyGroup):
    mode: EnumProperty(
        name="模式",
        description="选择操作模式",
        items=[
            ('CREATE', "创建", "创建新对象"),
            ('MODIFY', "修改", "修改现有对象"),
            ('DELETE', "删除", "删除对象"),
        ],
        default='CREATE'
    )
    
    intensity: FloatProperty(
        name="强度",
        description="效果强度",
        default=1.0,
        min=0.0,
        max=10.0
    )
    
    use_advanced: BoolProperty(
        name="使用高级选项",
        description="启用高级功能",
        default=False
    )

# 注册属性组
bpy.utils.register_class(MySettings)
bpy.types.Scene.my_settings = bpy.props.PointerProperty(type=MySettings)
```

### 动态 UI 构建

```python
def draw_dynamic_list(self, context):
    layout = self.layout
    scene = context.scene
    
    for i, item in enumerate(scene.my_list):
        row = layout.row(align=True)
        row.prop(item, "name", text=f"项 {i}")
        row.prop(item, "value", text="")
        
        # 添加操作按钮
        op = row.operator("my.remove_item", text="", icon='REMOVE')
        op.index = i
```

### 搜索和过滤界面

```python
def draw_search(self, context):
    layout = self.layout
    scene = context.scene
    
    row = layout.row()
    row.prop(scene, "search_filter", text="搜索", icon='VIEWZOOM')
    
    # 过滤显示列表
    for item in scene.my_list:
        if scene.search_filter == "" or scene.search_filter.lower() in item.name.lower():
            row = layout.row()
            row.label(text=item.name)
            row.prop(item, "enabled", text="")
```

### 进度条和状态

```python
def draw_progress(self, context):
    layout = self.layout
    scene = context.scene
    
    # 进度条
    row = layout.row()
    row.prop(scene, "progress", text="进度", slider=True)
    
    # 状态指示器
    if scene.processing:
        row = layout.row()
        row.label(text="处理中...", icon='TIME')
```

## 常见问题排查

### 问题：UI 元素不显示

**可能原因：**
1. 面板的 `bl_space_type` 或 `bl_region_type` 设置错误
2. 面板未注册或注册失败
3. 条件判断阻止了界面绘制

```python
# 错误示例：条件永远为假
def draw(self, context):
    if False:  # 永远不会执行
        self.layout.label(text="此文本不会显示")

# 正确示例：使用有效的条件判断
def draw(self, context):
    obj = context.object
    if obj is not None:
        self.layout.prop(obj, "name")
```

### 问题：布局混乱或对齐问题

**解决方案：** 使用 `align=True` 参数和对齐工具

```python
# 不对齐的按钮
row = layout.row()
row.operator("op1")
row.operator("op2")

# 对齐的按钮
row = layout.row(align=True)
row.operator("op1")
row.operator("op2")
```

### 问题：图标不显示

**解决方案：** 使用有效的图标名称

```python
# 无效图标（会显示默认图标或空白）
layout.label(text="设置", icon='INVALID_ICON')

# 有效图标
layout.label(text="设置", icon='PREFERENCES')
layout.operator("object.delete", icon='DELETE')
```

### 问题：属性控件样式不一致

**解决方案：** 使用 `expand` 和 `slider` 参数控制显示样式

```python
# 默认显示（紧凑）
layout.prop(scene, "my_float")

# 展开显示（带标签）
layout.prop(scene, "my_float", expand=False)

# 滑块显示
layout.prop(scene, "my_float", slider=True)
```

### 问题：大型列表性能差

**解决方案：** 使用 `template_list` 替代手动绘制

```python
class MyListPanel(bpy.types.Panel):
    def draw(self, context):
        layout = self.layout
        scene = context.scene
        
        # 使用 template_list 提高性能
        layout.template_list(
            "MY_UL_my_list",  # UIList 类名
            "",               # list_id
            scene,            # 数据来源
            "my_list",        # 集合属性名
            scene,            # 激活项数据
            "my_list_index",  # 激活项索引属性
            rows=5            # 显示行数
        )
```

## 设计最佳实践

### 视觉层次

**重要性分级：** 使用字体大小、间距和视觉权重创建清晰的信息层次

```python
def draw(self, context):
    layout = self.layout
    
    # 一级标题 - 主要操作
    col = layout.column(align=True)
    col.label(text="主要操作", icon='PLAY')
    col.operator("object.add", text="创建对象")
    
    # 二级标题 - 次要操作
    col.separator()
    col.label(text="辅助操作")
    row = col.row(align=True)
    row.operator("object.duplicate", text="复制")
    row.operator("object.delete", text="删除")
```

### 配色和主题

Blender 插件应遵循 Blender 主题，但可以通过以下方式增强视觉效果：

```python
def draw_styled_panel(self, context):
    layout = self.layout
    
    # 使用 Blender 主题色
    split = layout.split(factor=0.02)  # 窄边距
    
    # 装饰条
    col = split.column()
    col.separator(factor=5)
    col.label(text="", icon='DOT')  # 状态指示点
    
    # 主内容
    col = split.column()
    col.prop(context.scene, "my_setting")
```

### 动画和微交互

虽然 Blender UI 不直接支持 CSS 动画，但可以通过状态反馈创造微交互感：

```python
class MyOperator(bpy.types.Operator):
    def execute(self, context):
        # 成功状态反馈
        self.report({'INFO'}, "操作成功完成")
        return {'FINISHED'}
    
    def invoke(self, context, event):
        # 确认对话框
        return context.window_manager.invoke_confirm(self, event)
```

### 响应式布局

根据上下文调整界面：

```python
def draw_responsive(self, context):
    layout = self.layout
    scene = context.scene
    
    # 根据屏幕宽度调整布局
    width = context.region.width
    
    if width > 300:
        # 宽屏：使用两列布局
        split = layout.split(factor=0.5)
        col = split.column()
        col.prop(scene, "setting_a")
        col = split.column()
        col.prop(scene, "setting_b")
    else:
        # 窄屏：单列布局
        col = layout.column()
        col.prop(scene, "setting_a")
        col.prop(scene, "setting_b")
```

### 无障碍设计

```python
def draw_accessible(self, context):
    layout = self.layout
    
    # 使用清晰的标签
    row = layout.row()
    row.label(text="输出路径：")  # 明确的标签
    row.prop(context.scene, "output_path", text="")
    
    # 使用图标增强语义
    row = layout.row()
    row.operator("object.delete", text="删除对象", icon='DELETE')
    
    # 提供工具提示
    row = layout.row()
    row.prop(context.scene, "my_setting", text="自定义设置")
    row.set_shader_safe()  # 确保文字清晰可读
```

## 高级 UI 模式

### 向导式界面

```python
class WizardPanel(bpy.types.Panel):
    def draw(self, context):
        layout = self.layout
        scene = context.scene
        
        # 步骤指示器
        row = layout.row()
        for i in range(3):
            icon = 'RADIOBUT_ON' if scene.wizard_step == i else 'RADIOBUT_OFF'
            row.label(text=f"步骤 {i+1}", icon=icon)
        
        # 根据步骤显示不同内容
        if scene.wizard_step == 0:
            self.draw_step_one(context)
        elif scene.wizard_step == 1:
            self.draw_step_two(context)
        else:
            self.draw_step_three(context)
```

### 实时预览面板

```python
class PreviewPanel(bpy.types.Panel):
    def draw(self, context):
        layout = self.layout
        scene = context.scene
        
        # 参数控制
        box = layout.box()
        box.label(text="参数设置")
        box.prop(scene, "preview_param_a")
        box.prop(scene, "preview_param_b")
        
        # 预览区域
        row = layout.row()
        row.template_preview(context.scene.my_preview_object)
```

### 预设和模板选择

```python
class PresetPanel(bpy.types.Panel):
    def draw(self, context):
        layout = self.layout
        scene = context.scene
        
        # 预设选择
        row = layout.row()
        row.prop(scene, "preset", text="预设")
        
        # 应用预设按钮
        row = layout.row()
        row.operator("my.apply_preset", text="应用预设")
        
        # 自定义按钮
        row = layout.row()
        row.operator("my.save_preset", text="保存为预设")
```

## 样式参考

### 常用图标

| 图标 | 用途 |
|------|------|
| `ADD` | 添加操作 |
| `REMOVE` | 删除操作 |
| `PLAY` | 播放/开始 |
| `PAUSE` | 暂停 |
| `RECOVER_LAST` | 重做/恢复 |
| `TIME` | 时间/进度 |
| `INFO` | 信息提示 |
| `ERROR` | 错误 |
| `WARNING` | 警告 |
| `QUESTION` | 疑问帮助 |
| `SETTINGS` | 设置 |
| `PREFERENCES` | 首选项 |
| `SAVE` | 保存 |
| `FILE` | 文件操作 |
| `FOLDER` | 文件夹 |
| `IMPORT` | 导入 |
| `EXPORT` | 导出 |
| `TRIA_RIGHT` | 展开 |
| `TRIA_DOWN` | 折叠 |
| `DOWNARROW_HLT` | 高亮箭头 |
| `CHECKBOX_DEHLT` | 未选中 |
| `CHECKBOX_HLT` | 选中 |

### 布局参数参考

```python
# 分割比例
split = layout.split(factor=0.5)  # 50% 分割

# 列间距
col = layout.column(align=True)  # 紧密对齐
col.separator(factor=2.0)  # 额外间距

# 行内间距
row = layout.row(align=True)
row.alignment = 'EXPAND'  # 扩展对齐
row.scale_x = 1.5  # 水平缩放


---
## bpy_preferences

# 偏好设置模块

## 偏好设置概述

偏好设置编辑器包含控制Blender行为的设置。编辑器的左侧将可用选项分组为多个部分。Blender偏好设置包含控制Blender行为的设置，可以通过编辑菜单或按Ctrl加逗号键访问。

```python
# 访问偏好设置
# 菜单：编辑 -> 偏好设置
# 快捷键：Ctrl+逗号

# 偏好设置管理
# 自动保存偏好设置 - 默认情况下退出时保存更改
# 保存偏好设置 - 手动保存设置
# 恢复到保存的设置 - 撤销未保存的修改
# 加载默认设置 - 重置所有自定义设置

# 重要说明
# 加载默认设置后，当前会话将禁用自动保存
# 这允许切换回默认设置进行测试
# 而不会意外自动保存覆盖手动配置

# 备份建议
# 建议备份偏好设置
# 以防丢失配置
```

## 界面设置

### 显示选项

界面配置允许您更改UI元素的显示方式及其反应方式。

```python
# 分辨率缩放
# 调整字体和按钮的大小
# 相对于自动检测的DPI
# 可用缩放替代此设置

# 线宽
# 缩放界面中的线条和点
# 例如按钮轮廓、边、3D视口中的顶点点
# 选项：细、默认、粗

# 启动画面
# 启动时显示启动画面

# 开发者额外选项
# 显示旨在帮助开发者的设置和菜单项
# 包括：运算符搜索、序列器缓存设置

# 按钮上下文菜单
# 在线Python参考
# 复制Python命令
# 编辑源码
# 编辑翻译

# 实验性选项卡
# 正在进行的工作功能
# 可以在此启用
```

### 工具提示

```python
# 用户工具提示
# 启用时鼠标指针悬停在控件上时显示工具提示
# 提示说明指针下方的功能
# 显示关联的热键（如果有）
# 禁用时仍可按住Alt强制显示工具提示

# Python工具提示
# 在工具提示下方显示属性的Python信息

# 搜索排序
# 按最近选择排序
# 显示最近选择的搜索结果
# 否则按字母顺序排序
```

### 编辑器选项

```python
# 区域重叠
# 使区域重叠视口
# 工具栏和侧边栏区域
# 将显示重叠在主区域上

# 角落手柄
# 显示每个区域角落的小手柄
# 可用于拆分或合并区域
# 对触屏设备特别有用

# 数字输入箭头
# 启用时始终在数字输入字段中显示箭头
# 禁用时悬停时显示箭头

# 导航控件
# 在区域右上角显示导航控件
# 影响3D视口和图像空间

# 边界宽度
# 设置每个编辑器区域的填充
# 较大的值增加区域控制的命中区域
# 可改善触屏设备或残障人士的可用性

# 颜色选择器类型
# 选择颜色选择器类型
# 显示点击任何颜色字段时
# 选项：圆形HSV、圆形HSL、方形等
```

### 标题栏对齐

```python
# 标题栏对齐
# 标题栏停靠位置
# 选项：顶部、底部、左侧、右侧

# 面板类型
# 统一面板类型
# 设置面板的默认显示样式

# 区域大小
# 设置默认区域大小
# 滑块区域、按钮区域等
```

## 系统设置

### Cycles渲染设备

系统部分允许您设置显卡选项、内存限制和声音设置。

```python
# Cycles渲染设备
# 选择Cycles渲染引擎使用的计算设备
# 可使用CPU或某些GPU渲染图像

# 设备选项
# None - 使用CPU作为计算设备
# CUDA - 兼容的NVIDIA CUDA设备
# OptiX - 兼容的NVIDIA OptiX设备
# HIP - 兼容的AMD HIP设备
# oneAPI - 兼容的Intel oneAPI设备
# Metal - 兼容的Apple Metal设备

# 跨设备分配内存
# 跨多个GPU分配资源
# 而非复制数据
# 有效释放更大场景的空间
# 需要GPU通过高带宽通信协议连接
# 目前仅支持NVIDIA GPU上的NVLink

# GPU上的Embree
# 在Intel GPU上使用硬件光线追踪
# 提供更好的整体性能
# 仅支持oneAPI渲染设备

# HIP RT
# 通过在RDNA2及以上启用AMD硬件光线追踪来加速渲染
# 仅在使用HIP渲染设备时可用

# MetalRT
# 用于光线追踪的MetalRT
# 对大量使用曲线的场景使用更少内存
# 选项：关闭、打开、自动
```

### 显示图形

```python
# 显示图形设置
# 控制Blender如何绘制用户界面和其他显示图形
# 这些选项可以影响性能和兼容性

# 后端
# 选择用于绘制界面和渲染显示内容的图形API
# 更改后端需要重启Blender才能生效

# OpenGL
# 使用OpenGL后端
# 传统后端，兼容广泛系统

# Vulkan
# 使用Vulkan后端
# Vulkan可能提供更好的性能
# 更好地支持现代GPU功能
# 但兼容性可能因系统和驱动程序而异

# 设备（Vulkan）
# 指定用于显示绘制操作的GPU设备
# 对于多GPU系统有用
# 可以强制Blender使用特定GPU进行UI渲染
# 选项：自动
```

### 操作系统设置

```python
# 设置默认Blender
# 使当前安装成为默认Blender
# 仅适用于Windows和Linux

# 注册
# 使当前使用的Blender安装成为生成缩略图的默认值
# 成为打开blend文件的默认程序

# 取消注册
# 移除文件关联和缩略图生成

# 所有用户
# 为所有用户注册Blender
# 需要提升权限

# Linux注册
# 文件安装在/usr/local（所有用户）
# 或~/.local（当前用户）
# 安装桌面文件和图标
# 设置*.blend的文件关联
# 安装缩略图生成器
```

### 网络设置

```python
# 允许在线访问
# 允许Blender访问互联网
# 遵循此设置的附加组件仅在启用时才连接到互联网
# 但Blender无法阻止第三方附加组件违反此规则

# 网络超时
# 设置网络请求超时时间
```

## 快捷键设置

### 快捷键基础

```python
# 访问快捷键设置
# 偏好设置 -> 快捷键

# 快捷键结构
# 键位 - 按下的键
# 值 - 触发的事件
# 上下文 - 生效的条件

# 快捷键类型
# 键盘快捷键 - 键盘按键组合
# 鼠标快捷键 - 鼠标按钮和滚轮
# 触控板手势 - 多点触控手势
# 平板电脑输入 - 压感和倾斜

# 快捷键修饰符
# Ctrl - 控制键
# Shift - 转换键
# Alt - 替换键
# OS - Windows/Command键
```

### 快捷键管理

```python
# 添加快捷键
import bpy
prefs = bpy.context.preferences

# 获取活动密钥映射
keyconfig = prefs.keymaps.active

# 添加新快捷键
keyconfig.keymap_items.new(
    idname="wm.call_menu",
    type='A',
    value='PRESS',
    ctrl=True,
    shift=True
)

# 搜索快捷键
keymap = prefs.keymaps["3D View"]
for item in keymap.keymap_items:
    if "transform" in item.idname:
        print(f"{item.name}: {item.type}")

# 重置快捷键
bpy.ops.wm.keymap_restore(item_id="3D View")
```

### 快捷键预设

```python
# Blender默认
# 标准Blender快捷键配置

# Blender行业标准
# 行业兼容的快捷键配置

# 自定义
# 用户定义的快捷键配置

# 导入快捷键
bpy.ops.wm.keyconfig_import(
    filepath="//keymap.py"
)

# 导出快捷键
bpy.ops.wm.keyconfig_export(
    filepath="//keymap.py"
)
```

## 插件设置

### 插件管理

```python
# 访问插件设置
# 偏好设置 -> 插件

# 启用插件
bpy.ops.preferences.addon_enable(module="add-on-name")

# 禁用插件
bpy.ops.preferences.addon_disable(module="add-on-name")

# 安装插件
bpy.ops.preferences.addon_install(
    filepath="//path/to/addon.py"
)

# 卸载插件
bpy.ops.preferences.addon_remove(module="add-on-name")

# 更新插件
bpy.ops.preferences.addon_refresh()

# 查看插件信息
for addon in bpy.context.preferences.addons:
    print(f"名称: {addon.module}")
    print(f"描述: {addon.bl_info.get('description', '')}")
```

### 插件配置

```python
# 插件设置
addon = bpy.context.preferences.addons["add-on-name"]

# 访问插件首选项
prefs = addon.preferences

# 更改设置
prefs.property_name = value

# 保存设置
# 插件设置自动保存到用户配置

# 重置插件设置
# 查找重置选项或删除配置
```

### 插件安全性

```python
# 插件安全级别
# 允许所有插件
# 不信任的插件需要确认
# 仅允许受信任的插件

# 信任检查
# 检查插件来源
# 验证插件签名
# 审查插件权限

# 安全建议
# 只安装可信来源的插件
# 定期更新插件
# 审查插件权限要求
```

## 文件路径设置

### 文件路径配置

```python
# 访问文件路径设置
# 偏好设置 -> 文件路径

# 数据类型
# 图像 - 默认图像保存路径
# 临时文件 - 临时文件目录
# 渲染输出 - 渲染结果保存路径
# 播放输出 - 动画播放输出路径

# 路径变量
# // - 当前blend文件目录
# / - 根目录
# ~ - 用户主目录

# 配置路径
import bpy
prefs = bpy.context.preferences.filepaths

# 设置渲染输出路径
prefs.render_output_directory = "//render/"

# 设置图像保存路径
prefs.image_save_directory = "//textures/"

# 设置临时文件路径
prefs.temporary_directory = "/tmp/blender/"
```

### 资源库路径

```python
# 资产库路径
# 附加资产库位置
# 用于组织和访问资产

# 添加资产库
prefs.asset_libraries.new(name="MyAssets", path="//assets/")

# 移除资产库
prefs.asset_libraries.remove(asset_lib)

# 配置资产库
asset_lib = prefs.asset_libraries["MyAssets"]
asset_lib.path = "//assets/"
asset_lib.import_method = 'APPEND'
```

### 外部应用程序

```python
# 外部应用程序
# 配置外部编辑器
# 配置渲染器
# 配置版本控制

# 设置外部编辑器
prefs.file_extensions.other_file = "/path/to/editor"

# 配置外部渲染器
prefs.filepath = "/path/to/renderer"
```

## 主题设置

### 主题基础

```python
# 访问主题设置
# 偏好设置 -> 主题

# 主题预设
# Blender默认
# 深色主题
# 浅色主题
# 高对比度

# 创建自定义主题
import bpy
prefs = bpy.context.preferences.themes["Default"]

# 修改主题颜色
prefs.space.gradients.highlight = (0.5, 0.5, 0.5)

# 保存主题
bpy.ops.wm.save_userpref()
```

### 主题颜色

```python
# 界面颜色
# 背景色
# 面板颜色
# 按钮颜色
# 文字颜色

# 3D视口颜色
# 网格线颜色
# 坐标轴颜色
# 选择颜色
# 活动颜色

# 序列器颜色
# 片段颜色
# 过渡颜色
# 文本颜色

# 文本编辑器颜色
# 关键字颜色
# 字符串颜色
# 注释颜色
# 函数颜色

# 主题区域
# 空间 - 通用空间设置
# 区域 - 区域特定设置
# 小工具 - 小工具颜色
```

### 字体设置

```python
# 界面字体
# 设置界面使用的字体
# 影响所有UI文本

# 3D视口字体
# 设置3D视口文本字体
# 影响坐标轴和尺寸标注

# 序列器字体
# 设置序列器文本字体
# 影响字幕和标题

# 继承设置
# 使用系统字体
# 自定义字体路径
```

## 性能设置

### 内存设置

```python
# 内存设置
# 撤销步骤 - 最大撤销数量
# 撤销内存限制 - 内存使用上限
# 视频内存限制 - GPU内存限制

# 配置内存设置
prefs = bpy.context.preferences
prefs.edit.undo_steps = 50
prefs.system.memory_cache_limit = 4096

# 序列器缓存
# 序列器缓存大小
# 内存使用优化
```

### 视口性能

```python
# 视口设置
# 视口更新方式
# 背景更新频率
# 自动刷新间隔

# 性能选项
# 使用GPU进行显示
# 启用视口抗锯齿
# 减少屏幕外绘制

# 纹理压缩
# 启用纹理压缩
# 压缩方法
```

### 多线程设置

```python
# 多线程设置
# 线程数量
# 自动检测
# 手动设置

# 配置线程
import bpy
scene = bpy.context.scene
scene.render.threads = 0  # 自动检测

# 后台渲染线程
scene.render.use_multithreaded = True
```

## 辅助功能

### 视觉辅助

```python
# 视觉辅助选项
# 高对比度界面
# 放大UI元素
# 调整字体大小
# 颜色盲模式

# 颜色盲模式
# 红色盲（Protanopia）
# 绿色盲（Deuteranopia）
# 蓝色盲（Tritanopia）

# 屏幕阅读器
# 启用屏幕阅读器支持
# 调整语音设置
```

### 输入辅助

```python
# 输入辅助
# 替代键盘快捷键
# 鼠标按键交换
# 触屏优化

# 平板设置
# 压感灵敏度
# 倾斜感应
# 笔按钮映射

# 轨迹球设置
# 轨迹球灵敏度
# 反转轴向
```

### 导航辅助

```python
# 导航辅助
# 平滑导航
# 鼠标反转
# 自动深度

# 缩放设置
# 缩放灵敏度
# 缩放步长
# 缩放到鼠标
```

## Python脚本

### 偏好设置访问

```python
# 访问偏好设置
import bpy
prefs = bpy.context.preferences

# 获取系统偏好设置
system_prefs = prefs.system

# 获取视图偏好设置
view_prefs = prefs.view

# 获取编辑偏好设置
edit_prefs = prefs.edit

# 获取文件路径偏好设置
paths_prefs = prefs.filepaths

# 获取主题偏好设置
theme_prefs = prefs.themes
```

### 修改偏好设置

```python
# 修改界面设置
prefs.view.ui_scale = 1.0
prefs.view.ui_line_width = 'DEFAULT'

# 修改系统设置
system_prefs.gpu_backend = 'VULKAN'
system_prefs.memory_cache_limit = 8192

# 修改编辑设置
edit_prefs.undo_steps = 100
edit_prefs.use_global_undo = True

# 保存偏好设置
bpy.ops.wm.save_userpref()

# 加载默认设置
bpy.ops.wm.read_factory_userpref()

# 恢复保存的设置
bpy.ops.wm.read_userpref()
```

### 批量操作

```python
# 批量配置
def setup_preferences():
    prefs = bpy.context.preferences

    # 设置UI
    prefs.view.ui_scale = 1.0
    prefs.view.show_splash = False

    # 设置系统
    prefs.system.gpu_backend = 'VULKAN'
    prefs.system.memory_cache_limit = 4096

    # 设置文件路径
    prefs.filepaths.render_output_directory = "//render/"
    prefs.filepaths.temporary_directory = "/tmp/blender/"

    # 保存
    bpy.ops.wm.save_userpref()

# 重置偏好设置
def reset_preferences():
    bpy.ops.wm.read_factory_userpref()
```

## 常见问题解决

### 偏好设置问题

```python
# 偏好设置不保存
# 检查自动保存是否启用
# 手动保存偏好设置
# 检查文件权限

# 界面显示异常
# 加载默认设置
# 重启Blender
# 检查主题设置

# 性能问题
# 减少撤销步骤
# 增加内存限制
# 调整视口设置

# 快捷键冲突
# 检查快捷键冲突
# 重置快捷键
# 使用唯一快捷键
```

### 系统兼容问题

```python
# GPU不支持
# 检查驱动程序版本
# 尝试不同后端
# 使用CPU渲染

# 内存不足
# 增加内存限制
# 减少场景复杂度
# 使用代理对象

# 插件冲突
# 禁用所有插件
# 逐个启用
# 查找冲突插件
```

### 恢复设置

```python
# 备份偏好设置
import shutil
import bpy

config_dir = bpy.utils.resource_path('USER') + "/config/"
backup_dir = "//backup/preferences/"

shutil.copytree(config_dir, backup_dir)

# 从备份恢复
shutil.copytree(backup_dir, config_dir)

# 导出偏好设置
bpy.ops.wm.userpref_export(filepath="//preferences.py")

# 导入偏好设置
bpy.ops.wm.userpref_import(filepath="//preferences.py")
```


---
## bpy_workspace_ui

# 工作区与界面定制模块

## 工作区概述

Blender的工作区系统允许用户自定义界面布局以适应不同的工作流程。每个工作区包含一组预定义的编辑器区域，可以根据需要进行调整和保存。工作区对于提高工作效率非常重要，特别是在处理复杂的项目时。

```python
# 工作区概念
# 工作区是界面的顶层组织单位
# 每个工作区包含多个区域
# 区域可以包含不同的编辑器

# 工作区类型
# 建模 - 3D建模工作区
# 雕刻 - 雕刻和纹理绘制工作区
# 动画 - 动画制作工作区
# 渲染 - 渲染和合成工作区
# Scripting - Python脚本工作区
# 默认 - 综合工作区

# 工作区切换
# 使用顶部工作区选择器
# 使用快捷键（数字键1-9）
# 使用Python代码切换

# 自定义工作区
# 调整区域大小
# 拆分和合并区域
# 更改编辑器类型
# 保存自定义布局
```

## 工作区管理

### 访问工作区

```python
# 获取所有工作区
for workspace in bpy.data.workspaces:
    print(f"工作区: {workspace.name}")

# 获取当前工作区
current_workspace = bpy.context.workspace
print(f"当前工作区: {current_workspace.name}")

# 切换工作区
bpy.context.window.workspace = bpy.data.workspaces["Scripting"]

# 创建新工作区
new_workspace = bpy.data.workspaces.new(name="MyWorkspace")

# 删除工作区
bpy.data.workspaces.remove(new_workspace)

# 重命名工作区
new_workspace.name = "RenamedWorkspace"
```

### 工作区配置

```python
# 工作区基本配置
workspace = bpy.context.workspace

# 设置界面缩放
workspace.ui_scale = 1.0

# 获取界面缩放
current_scale = workspace.ui_scale

# 访问界面参数
params = workspace.get("ui_parameters", {})
print(f"参数: {params}")

# 工作区图标
workspace.icon = 'COLOR'  # 图标标识
```

### 工作区快捷键

```python
# 工作区快捷键
# 数字键1-9 - 切换到对应工作区
# Ctrl+Tab - 工作区饼图菜单
# Shift+Space - 切换全屏

# 设置快捷键
import bpy
keyconfig = bpy.context.preferences.keymaps.active

# 添加快捷键
keyconfig.keymap_items.new(
    idname="wm.workspace_cycle",
    type='ONE',
    value='PRESS',
    ctrl=False,
    shift=False
)

# 移除快捷键
for item in keyconfig.keymap_items:
    if item.idname == "wm.workspace_cycle":
        keyconfig.keymap_items.remove(item)
```

## 区域管理

### 区域结构

```python
# Blender界面由区域组成
# 区域是界面的基本单元
# 每个区域可以包含不同的编辑器

# 区域类型
# TOP - 顶部区域（菜单栏）
# HEADER - 标题区域
# UI - 侧边栏
# WINDOW - 主视口区域
# CHANNELS - 通道区域
# TEMPORARY - 临时区域

# 访问当前区域
area = bpy.context.area
print(f"区域类型: {area.type}")

# 遍历所有区域
for area in bpy.context.screen.areas:
    print(f"区域类型: {area.type}")

# 获取区域空间
space = area.spaces[0]
print(f"空间类型: {space.type}")
```

### 区域操作

```python
# 拆分区域
bpy.ops.screen.area_split(direction='VERTICAL', factor=0.5)

# 合并区域
# 将鼠标移动到区域边界
# 当光标变成双箭头时拖动

# 区域位置
x = area.x
y = area.y
width = area.width
height = area.height

# 区域尺寸
print(f"位置: ({x}, {y})")
print(f"尺寸: {width} x {height}")

# 设置区域大小
area.width = 400
area.height = 600

# 最大化区域
bpy.ops.screen.area最大化()

# 恢复区域
bpy.ops.screen.back_to_previous()
```

### 空间管理

```python
# 访问空间
space = area.spaces[0]

# 获取空间类型
print(f"空间类型: {space.type}")

# 常用空间类型
# VIEW_3D - 3D视口
# IMAGE_EDITOR - 图像编辑器
# NODE_EDITOR - 节点编辑器
# TEXT_EDITOR - 文本编辑器
# SPREADSHEET - 电子表格编辑器
# SEQUENCE_EDITOR - 序列编辑器
# DOPESHEET_EDITOR - 关键帧编辑器
# GRAPH_EDITOR - 曲线编辑器
# NLA_EDITOR - NLA编辑器
# OUTLINER - 大纲编辑器
# PROPERTIES - 属性编辑器
# FILE_BROWSER - 文件浏览器
# CONSOLE - 控制台

# 创建新空间
new_space = area.spaces.new(type='VIEW_3D')

# 移除空间
area.spaces.remove(new_space)
```

## 编辑器配置

### 3D视口配置

```python
# 访问3D视口空间
space = None
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        space = area.spaces[0]
        break

# 设置显示类型
space.shading.type = 'SOLID'  # 实体
space.shading.type = 'MATERIAL_PREVIEW'  # 材质预览
space.shading.type = 'RENDERED'  # 渲染
space.shading.type = 'WIREFRAME'  # 线框

# 设置实体着色选项
space.shading.show_xray = False  # X射线
space.shading.xray_alpha = 0.5  # X射线透明度
space.shading.show_lights = True  # 显示灯光
space.shading.show_cameras = True  # 显示相机
space.shading.show_meshes = True  # 显示网格
space.shading.show_curves = True  # 显示曲线
space.shading.show_empty = True  # 显示空对象

# 设置背景类型
space.shading.background_type = 'VIEWPORT'  # 视口
space.shading.background_type = 'THEME'  # 主题
space.shading.background_type = 'COLOR'  # 颜色
space.shading.background_type = 'IMAGE'  # 图像

# 设置主题背景色
space.shading.theme_state = 'THEME'  # 使用主题
space.shading.background_color = (0.05, 0.05, 0.05)  # 自定义颜色
```

### 视口显示选项

```python
# 网格显示
space.show_gizmo = True  # 显示小工具
space.show_gizmo_navigate = True  # 导航小工具
space.show_gizmo_object = True  # 对象小工具
space.show_gizmo_light = True  # 灯光小工具
space.show_gizmo_camera = True  # 相机小工具

# 叠加显示
space.overlay.show_overlays = True  # 显示叠加
space.overlay.show_wireframes = False  # 线框
space.overlay.show_face_orientation = False  # 面方向
space.overlay.show_face_normals = False  # 面法线
space.overlay.show_vertex_normals = False  # 顶点法线

# 游标显示
space.show_cursor = True  # 显示3D游标
space.cursor_location = (0, 0, 0)  # 游标位置

# 统计信息
space.show_stats = True  # 显示统计
```

### 图像编辑器配置

```python
# 访问图像编辑器空间
space = None
for area in bpy.context.screen.areas:
    if area.type == 'IMAGE_EDITOR':
        space = area.spaces[0]
        break

# 设置图像
space.image = bpy.data.images.load("//texture.png")

# 显示模式
space.ui_mode = 'VIEW'  # 查看
space.ui_mode = 'PAINT'  # 绘制
space.ui_mode = 'MASK'  # 遮罩

# 图像显示
space.display_channels = 'COLOR'  # 颜色
space.display_channels = 'COLOR_ALPHA'  # 颜色和Alpha
space.display_channels = 'ALPHA'  # 仅Alpha
space.display_channels = 'Z_BUFFER'  # Z缓冲区

# 图像缩放
space.zoom = 1.0  # 缩放比例

# 图像平移
space.cursor_location = (100, 100)  # 游标位置
```

## 界面主题

### 主题设置

```python
# 访问主题
theme = bpy.context.preferences.themes["Default"]

# 设置界面颜色
theme.interface.main_color = (0.05, 0.05, 0.05)
theme.interface.text_color = (0.9, 0.9, 0.9)
theme.interface.header_color = (0.1, 0.1, 0.1)

# 设置3D视口颜色
theme.view_3d.header = (0.1, 0.1, 0.1)
theme.view_3d.space = (0.05, 0.05, 0.05)
theme.view_3d.text = (0.9, 0.9, 0.9)
theme.view_3d.header_text = (0.9, 0.9, 0.9)

# 设置曲线编辑器颜色
theme.graph_editor.header = (0.1, 0.1, 0.1)
theme.graph_editor.space = (0.05, 0.05, 0.05)

# 设置节点编辑器颜色
theme.node_editor.node_frame = (0.1, 0.1, 0.1)
theme.node_editor.node_socket = (0.5, 0.5, 0.5)

# 设置文本编辑器颜色
theme.text_editor.space = (0.05, 0.05, 0.05)
theme.text_editor.text = (0.9, 0.9, 0.9)

# 应用主题更改
bpy.ops.wm.save_userpref()
```

### 主题预设

```python
# 内置主题
# Blender默认
# 暗色主题
# 浅色主题
# 高对比度

# 切换主题
bpy.context.preferences.themes.active = bpy.context.preferences.themes["Dark"]

# 导出主题
bpy.ops.wm.theme_add(filepath="//my_theme.xml")

# 导入主题
bpy.ops.wm.theme_remove(theme="Dark")

# 重置主题
bpy.ops.wm.theme_set(defaults=True)
```

## 自定义UI元素

### 创建面板

```python
import bpy

class MyPanel(bpy.types.Panel):
    bl_label = "我的面板"
    bl_idname = "OBJECT_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '工具'
    
    def draw(self, context):
        layout = self.layout
        layout.label(text="欢迎使用我的面板!")
        
        # 添加按钮
        layout.operator("object.simple_operator")
        
        # 添加属性
        layout.prop(context.scene, "my_property")
        
        # 添加分割线
        layout.separator()
        
        # 添加列
        col = layout.column()
        col.operator("object.copy")
        col.operator("object.delete")

# 注册面板
bpy.utils.register_class(MyPanel)

# 移除面板
bpy.utils.unregister_class(MyPanel)
```

### 创建运算符

```python
class SimpleOperator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "简单运算符"
    bl_description = "执行简单操作的运算符"
    bl_options = {'REGISTER', 'UNDO'}
    
    # 属性定义
    my_float: bpy.props.FloatProperty(
        name="浮点数",
        description="一个浮点数属性",
        default=1.0,
        min=0.0,
        max=10.0
    )
    
    my_bool: bpy.props.BoolProperty(
        name="布尔值",
        description="一个布尔值属性",
        default=True
    )
    
    my_enum: bpy.props.EnumProperty(
        name="枚举",
        description="一个枚举属性",
        items=[
            ('OPTION1', "选项1", "第一个选项"),
            ('OPTION2', "选项2", "第二个选项"),
            ('OPTION3', "选项3', "第三个选项')
        ]
    )
    
    def execute(self, context):
        # 执行操作
        print(f"浮点数: {self.my_float}")
        print(f"布尔值: {self.my_bool}")
        print(f"枚举值: {self.my_enum}")
        return {'FINISHED'}
    
    def invoke(self, context, event):
        # 显示属性对话框
        return context.window_manager.invoke_props_dialog(self)
    
    def draw(self, context):
        # 自定义对话框布局
        layout = self.layout
        layout.prop(self, "my_float")
        layout.prop(self, "my_bool")
        layout.prop(self, "my_enum")

# 注册运算符
bpy.utils.register_class(SimpleOperator)

# 调用运算符
bpy.ops.object.simple_operator(my_float=2.0)
```

### 创建菜单

```python
class MyMenu(bpy.types.Menu):
    bl_idname = "OBJECT_MT_my_menu"
    bl_label = "我的菜单"
    
    def draw(self, context):
        layout = self.layout
        
        # 添加菜单项
        layout.operator("object.simple_operator", text="运算符1")
        layout.operator("object.simple_operator", text="运算符2")
        
        # 添加子菜单
        layout.menu("OBJECT_MT_sub_menu", text="子菜单")
        
        # 添加分割线
        layout.separator()
        
        # 添加搜索
        layout.operator("wm.search_menu", text="搜索...")

class SubMenu(bpy.types.Menu):
    bl_idname = "OBJECT_MT_sub_menu"
    bl_label = "子菜单"
    
    def draw(self, context):
        layout = self.layout
        layout.operator("object.copy", text="复制")
        layout.operator("object.delete", text="删除")

# 注册菜单
bpy.utils.register_class(MyMenu)
bpy.utils.register_class(SubMenu)

# 显示菜单
bpy.ops.wm.call_menu(name="OBJECT_MT_my_menu")

# 在按钮中使用菜单
layout.menu("OBJECT_MT_my_menu")
```

## 界面布局

### 动态布局

```python
# 创建动态面板
class DynamicPanel(bpy.types.Panel):
    bl_label = "动态面板"
    bl_idname = "OBJECT_PT_dynamic"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    
    @classmethod
    def poll(cls, context):
        # 检查条件
        return context.active_object is not None
    
    def draw(self, context):
        layout = self.layout
        
        # 根据条件显示不同内容
        obj = context.active_object
        if obj.type == 'MESH':
            layout.label(text="网格对象")
            layout.prop(obj.data, "vertices", text="顶点数")
        elif obj.type == 'CURVE':
            layout.label(text="曲线对象")
            layout.prop(obj.data, "splines", text="样条数")
        
        # 使用列布局
        col = layout.column()
        col.operator("object.transform_apply")
        
        # 使用行布局
        row = layout.row()
        row.operator("object.location_clear")
        row.operator("object.rotation_clear")
        row.operator("object.scale_clear")
        
        # 使用箱式布局
        box = layout.box()
        box.label(text="分组内容")
        box.operator("object.duplicate")
```

### 高级布局

```python
# 使用网格布局
def draw_grid_example(self, context):
    layout = self.layout
    
    # 创建网格
    grid = layout.grid_flow(
        columns=3,
        even_columns=True,
        even_rows=True,
        align=True
    )
    
    grid.operator("object.copy")
    grid.operator("object.delete")
    grid.operator("object.duplicate")
    grid.operator("object.join")
    grid.operator("object.separate")

# 使用分割布局
def draw_split_example(self, context):
    layout = self.layout
    
    # 创建分割
    split = layout.split(factor=0.5)
    
    # 左列
    left = split.column()
    left.label(text="左侧内容")
    left.operator("object.copy")
    
    # 右列
    right = split.column()
    right.label(text="右侧内容")
    right.operator("object.delete")

# 使用帧布局
def draw_frame_example(self, context):
    layout = self.layout
    
    # 创建帧
    frame = layout.frame()
    frame.label(text="帧内容")
    
    col = frame.column()
    col.operator("object.transform_apply")
    col.operator("object.duplicate")
```

## 交互式UI

### 回调和更新

```python
# 属性更新回调
class UpdatePanel(bpy.types.Panel):
    bl_label = "更新面板"
    bl_idname = "OBJECT_PT_update"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    
    def draw(self, context):
        layout = self.layout
        layout.prop(context.scene, "my_property")
        layout.prop(context.scene, "another_property")

# 注册属性更新函数
def update_callback(self, context):
    print(f"属性更新: {self.my_property}")
    print(f"另一个属性: {self.another_property}")

# 添加属性
bpy.types.Scene.my_property = bpy.props.FloatProperty(
    name="我的属性",
    default=1.0,
    update=update_callback
)

bpy.types.Scene.another_property = bpy.props.StringProperty(
    name="另一个属性",
    default="默认文本",
    update=update_callback
)
```

### 动态运算符

```python
# 动态菜单创建
class DynamicMenu(bpy.types.Menu):
    bl_idname = "OBJECT_MT_dynamic_menu"
    bl_label = "动态菜单"
    
    def draw(self, context):
        layout = self.layout
        
        # 动态添加菜单项
        for i in range(5):
            layout.operator(
                "object.simple_operator",
                text=f"动态项 {i+1}"
            )
        
        # 动态添加分隔符
        layout.separator()
        
        # 动态添加对象选择
        for obj in context.scene.objects:
            layout.operator(
                "object.select",
                text=f"选择 {obj.name}"
            ).select_object = obj.name

# 动态面板内容
class ObjectPanel(bpy.types.Panel):
    bl_label = "对象操作"
    bl_idname = "OBJECT_PT_object_ops"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    
    def draw(self, context):
        layout = self.layout
        
        # 显示活动对象信息
        if context.active_object:
            layout.label(
                text=f"活动对象: {context.active_object.name}",
                icon='OBJECT_DATAMODE'
            )
            
            # 动态创建对象操作
            row = layout.row()
            row.operator("object.select_all", text="全选")
            row.operator("object.select_inverse", text="反选")
            
            # 根据对象类型显示不同选项
            obj = context.active_object
            if obj.type == 'MESH':
                layout.prop(obj.data, "use_auto_smooth", text="自动平滑")
        else:
            layout.label(text="无活动对象")
```

### 实时更新

```python
# 实时属性面板
class RealTimePanel(bpy.types.Panel):
    bl_label = "实时数据"
    bl_idname = "OBJECT_PT_realtime"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    
    def draw(self, context):
        layout = self.layout
        
        # 显示帧信息
        layout.label(
            text=f"当前帧: {context.scene.frame_current}",
            icon='TIME'
        )
        
        # 显示对象数量
        layout.label(
            text=f"对象数量: {len(context.scene.objects)}",
            icon='OBJECT_DATA'
        )
        
        # 显示顶点统计
        total_verts = sum(
            len(obj.data.vertices) 
            for obj in context.scene.objects 
            if obj.type == 'MESH' and obj.data
        )
        layout.label(
            text=f"总顶点数: {total_verts}",
            icon='MESH_DATA'
        )
        
        # 显示内存使用
        import psutil
        process = psutil.Process()
        memory = process.memory_info().rss / 1024 / 1024
        layout.label(
            text=f"内存使用: {memory:.1f} MB",
            icon='MEMORY'
        )
```

## 小工具

### 创建小工具

```python
# 访问小工具组
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        for gizmo_group in area.spaces[0].gizmo_groups:
            print(f"小工具组: {gizmo_group.name}")
            for gizmo in gizmo_group.gizmos:
                print(f"  小工具: {gizmo.name}")

# 显示/隐藏小工具
space = bpy.context.area.spaces[0]
space.show_gizmo = True
space.show_gizmo_navigate = True
space.show_gizmo_object = True

# 自定义小工具
class MyGizmo(bpy.types.Gizmo):
    bl_idname = "OBJECT_GT_my_gizmo"
    bl_target_prop = "location"
    
    def draw(self, context):
        # 绘制小工具
        self.draw_line(
            start=(0, 0, 0),
            end=(1, 0, 0),
            color=(1, 0, 0, 1),
            width=3
        )

# 注册小工具
bpy.utils.register_class(MyGizmo)

# 添加到小工具组
# 在适当的空间中添加小工具
```

### 小工具组

```python
# 创建小工具组
class MyGizmoGroup(bpy.types.GizmoGroup):
    bl_label = "我的小工具组"
    bl_idname = "OBJECT_GGT_my_group"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'WINDOW'
    
    @classmethod
    def poll(cls, context):
        return context.area.type == 'VIEW_3D'
    
    def setup(self, context):
        # 创建小工具
        gizmo = self.gizmos.new("OBJECT_GT_my_gizmo")
        gizmo.target_set_func("location", get_location, set_location)

# 注册小工具组
bpy.utils.register_class(MyGizmoGroup)

# 启用小工具组
space = bpy.context.area.spaces[0]
space.gizmo_groups[0].show = True
```

## 脚本化界面

### 批量界面操作

```python
# 批量创建区域
def create_layout():
    screen = bpy.context.screen
    
    # 获取当前工作区
    workspace = bpy.context.workspace
    
    # 创建新屏幕布局
    layout = workspace.layouts.new(name="CustomLayout")
    
    return layout

# 配置所有视口
def configure_all_viewports():
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces[0]
            space.shading.type = 'SOLID'
            space.show_gizmo = True
            space.overlay.show_overlays = True

# 应用默认布局
def apply_default_layout():
    workspace = bpy.context.workspace
    
    # 恢复默认布局
    workspace.use_previews = True
    
    # 设置默认主题
    bpy.context.preferences.themes.active = \
        bpy.context.preferences.themes["Dark"]
```

### 界面保存和加载

```python
# 保存界面设置
bpy.ops.wm.save_userpref()

# 保存启动文件
bpy.ops.wm.save_homefile()

# 加载默认设置
bpy.ops.wm.read_factory_userpref()

# 导出界面配置
bpy.ops.wm.userpref_export(filepath="//preferences.py")

# 导入界面配置
bpy.ops.wm.userpref_import(filepath="//preferences.py")

# 保存屏幕布局
# 1. 调整区域到所需布局
# 2. 工作区 -> 保存更改
```

## 性能优化

### 界面性能

```python
# 优化面板绘制
class OptimizedPanel(bpy.types.Panel):
    bl_label = "优化面板"
    bl_idname = "OBJECT_PT_optimized"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    
    def draw(self, context):
        layout = self.layout
        
        # 避免不必要的更新
        scene = context.scene
        
        # 使用简单的布局
        col = layout.column()
        col.prop(scene, "frame_current")
        col.prop(scene, "frame_start")
        col.prop(scene, "frame_end")
        
        # 避免复杂计算
        # 在poll中过滤
        # 只在需要时更新

# 使用延迟更新
class DelayedPanel(bpy.types.Panel):
    bl_label = "延迟面板"
    bl_idname = "OBJECT_PT_delayed"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    
    def draw(self, context):
        layout = self.layout
        
        # 避免每帧更新
        if context.area.tag_redraw:
            layout.label(text="需要更新")
        
        # 使用静态值
        layout.label(text="静态内容")
```

### 内存管理

```python
# 清理未使用的UI资源
def cleanup_ui():
    # 清理未使用的主题
    for theme in bpy.context.preferences.themes:
        if not theme.users:
            bpy.context.preferences.themes.remove(theme)
    
    # 清理未使用的图标
    # Blender自动管理图标
    
    # 清理UI缓存
    bpy.ops.ui.refresh()

# 优化大列表显示
class LargeListPanel(bpy.types.Panel):
    bl_label = "大列表"
    bl_idname = "OBJECT_PT_large_list"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    
    def draw(self, context):
        layout = self.layout
        
        # 分页显示
        page_size = 10
        objects = context.scene.objects
        
        for i in range(min(page_size, len(objects))):
            obj = objects[i]
            layout.label(text=obj.name)
```

## 最佳实践

### 设计原则

```python
# 清晰的界面层次
# 使用分组和分割线
# 遵循Blender的界面约定
# 保持简洁

# 响应式设计
# 考虑不同屏幕尺寸
# 使用相对布局
# 测试不同分辨率

# 性能友好
# 避免频繁重绘
# 延迟复杂计算
# 使用缓存

# 一致性
# 使用标准控件
# 遵循命名约定
# 保持风格统一
```

### 工作流程优化

```python
# 创建自定义工作区
def create_anim_workspace():
    workspace = bpy.data.workspaces.new(name="Animation")
    
    # 添加必要的区域
    # 3D视口
    # 时间线
    # 大纲编辑器
    # 属性编辑器
    
    return workspace

# 保存工作区配置
def save_workspace_config(workspace):
    # 导出工作区设置
    config = {
        "name": workspace.name,
        "layouts": [layout.name for layout in workspace.layouts]
    }
    return config

# 恢复工作区配置
def restore_workspace_config(config):
    workspace = bpy.data.workspaces.get(config["name"])
    if workspace:
        # 应用配置
        pass
```

## 常见问题解决

### 界面问题

```python# 界面无响应
# 检查是否有错误
# 查看控制台输出
# 重置界面布局
bpy.ops.screen.reset_layout()

# 界面显示异常
# 检查主题设置
# 重启Blender
# 清除用户配置

# 面板不显示
# 检查poll方法返回值
# 检查bl_idname是否唯一
# 检查是否已注册

# 快捷键冲突
# 检查快捷键设置
# 使用不同的快捷键
# 重置快捷键
```

### 布局问题

```python
# 区域无法调整
# 检查是否锁定
# 尝试最大化恢复
# 重置界面布局

# 编辑器类型更改
# 右键点击区域标题
# 选择编辑器类型
# 使用快捷键切换

# 面板位置错误
# 检查bl_space_type
# 检查bl_region_type
# 检查bl_category
```


---
## bpy_freestyle

# freestyle 模块 - 非真实感渲染

## 模块概述

freestyle 模块是 Blender 的非真实感渲染（NPR）引擎，提供数据类型的视图映射组件（0D 和 1D 元素）、定义线条风格化规则的基础类（谓词、函数、链式迭代器和笔触着色器），以及样式模块编写辅助函数。Freestyle 专注于线条艺术渲染，可用于创建卡通轮廓、钢笔画等艺术效果。

## 子模块结构

### freestyle.types - 类型定义

包含 Freestyle 系统的核心数据类型定义。

```python
import freestyle.types

# 0D 要素类型
Vertex = freestyle.types.Vertex              # 顶点
ViewVertex = freestyle.types.ViewVertex      # 视图顶点

# 1D 要素类型
Edge = freestyle.types.Edge                  # 边
ViewEdge = freestyle.types.ViewEdge          # 视图边
Curve = freestyle.types.Curve                # 曲线

# 样式相关类型
Stroke = freestyle.types.Stroke              # 笔触
StrokeAttribute = freestyle.types.StrokeAttribute  # 笔触属性
```

### freestyle.predicates - 谓词模块

谓词用于判断 0D 和 1D 要素是否满足特定条件，是线条选择的基础。

```python
import freestyle.predicates as predicates

# 距离谓词
predicates.GT(distance, epsilon=0.0001)      # 大于指定距离
predicates.LT(distance, epsilon=0.0001)      # 小于指定距离

# 曲率谓词
predicates.GT、曲率(threshold)               # 曲率大于阈值
predicates.LT、曲率(threshold)               # 曲率小于阈值

# 长度谓词
predicates.GT_长度(length)                   # 长度大于指定值
predicates.LT_长度(length)                   # 长度小于指定值

# 方向谓词
predicates.Angle_2D(direction, angle)        # 2D 角度匹配
predicates.Angle_3D(direction, angle)        # 3D 角度匹配

# 组合谓词
predicates.NOT(predicate)                    # 逻辑非
predicates.AND(predicate1, predicate2)       # 逻辑与
predicates.OR(predicate1, predicate2)        # 逻辑或
```

### freestyle.functions - 函数模块

函数用于计算 0D 和 1D 要素的属性值，是样式规则的核心逻辑。

```python
import freestyle.functions as functions

# 曲率计算
curvature = functions.Curvature2DAngle()     # 2D 曲率角度
curvature = functions.Curvature3DAngle()     # 3D 曲率角度

# 距离计算
dist = functions.DistanceToCamera()          # 到相机的距离
dist = functions.DistanceToImageBorder()     # 到图像边缘的距离
dist = functions.DistanceToViewCenter()      # 到视图中心的距离

# 方向计算
direction = functions.Nanocad_方向()         # 获取 2D 轮廓方向
direction = functions.View3D方向()           # 获取 3D 视图方向

# 属性获取
thickness = functions.Get_thickness()        # 获取线条粗细
color = functions.Get_颜色()                 # 获取线条颜色
```

### freestyle.chainingiterators - 链式迭代器

链式迭代器定义如何从选定的边构建连续曲线，用于轮廓跟踪。

```python
import freestyle.chainingiterators as chaining_iterators

# 角度链式迭代器（根据角度连接边）
chainer = chaining_iterators.角度链式迭代器(
    angle_threshold=radians(30),
    protrusion_receding_angle=protrusion_angle,
    receding_angle=receding_angle
)

# 距离链式迭代器（根据距离连接边）
chainer = chaining_iterators.距离链式迭代器(
    distance_threshold=10.0
)

# 曲率链式迭代器（根据曲率连接边）
chainer = chaining_iterators.曲率链式迭代器(
    curvature_threshold=0.1
)

# 轮廓链式迭代器（专门用于轮廓）
chainer = chaining_iterators.Contour_链式迭代器(
    id="contour_chainer",
    orientation_flag=True
)
```

### freestyle.shaders - 笔触着色器

笔触着色器定义笔触的视觉外观，包括颜色、粗细、纹理等。

```python
import freestyle.shaders as shaders

# 笔触颜色着色器
color_shader = shaders.ConstantColorShader(
    color=(0.0, 0.0, 0.0, 1.0)
)
color_shader = shaders.GradientColorShader(
    color1=(0.0, 0.0, 0.0, 1.0),
    color2=(1.0, 1.0, 1.0, 1.0),
    axis='x'  # 或 'y', 'length'
)

# 笔触粗细着色器
thickness_shader = shaders.ConstantThicknessShader(
    thickness=2.0
)
thickness_shader = shaders.LinearThicknessShader(
    thickness_start=2.0,
    thickness_end=1.0,
    direction='forward'  # 或 'backward', 'both'
)
thickness_shader = shaders.Curvature2DThicknessShader(
    thickness_min=1.0,
    thickness_max=5.0
)

# 笔触纹理着色器
texture_shader = shaders.TextureShader(
    texture_path="/path/to/texture.png"
)

# 组合着色器
shader = shaders.BluePrintShader(
    blueprint_type=shaders.BLUEPRINT_CIRCLES,
    thickness=2.0,
    color=(0.0, 0.0, 0.8)
)
```

### freestyle.utils - 实用工具

提供样式模块编写的辅助函数。

```python
import freestyle.utils as utils

# 读取样式模块
style_module = utils.ImportStyleModule("/path/to/style.py")

# 创建操作符
utils.CreateNoInputOperator(
    name="MyOperator",
    description="My custom Freestyle operator"
)

# 注册样式模块
utils.RegisterStyleModule("/path/to/style.py")
```

## 完整样式模块示例

```python
import freestyle
import freestyle.types as types
import freestyle.predicates as predicates
import freestyle.functions as functions
import freestyle.chainingiterators as chaining_iterators
import freestyle.shaders as shaders

class MyStyleModule:
    def __init__(self):
        # 定义选择谓词
        self.selection_predicates = (
            predicates.AND(
                predicates.GT_长度(20.0),
                predicates.NOT(predicates.IsInvisible)
            )
        )
        
        # 定义链式迭代器
        self.chain_predicate = chaining_iterators.角度链式迭代器(
            angle_threshold=radians(25.0)
        )
        
        # 定义笔触着色器
        self.stroke_shaders = [
            shaders.ConstantColorShader(
                color=(0.0, 0.0, 0.0, 1.0)
            ),
            shaders.ConstantThicknessShader(
                thickness=2.0
            )
        ]
    
    def get_predicates(self):
        return self.selection_predicates
    
    def get_chaining_iterator(self):
        return self.chain_predicate
    
    def get_shaders(self):
        return self.stroke_shaders

# 注册样式模块
freestyle.utils.RegisterStyleModule(MyStyleModule)
```

## 常用样式效果

### 卡通轮廓

```python
import freestyle
import freestyle.predicates as predicates
import freestyle.chainingiterators as chaining_iterators
import freestyle.shaders as shaders

class CartoonStyle:
    def __init__(self):
        # 选择所有外部轮廓
        self.predicates = predicates.AND(
            predicates.IsExternal(),
            predicates.GT_长度(10.0)
        )
        
        # 使用角度链式迭代器
        self.chainer = chaining_iterators.角度链式迭代器(
            angle_threshold=radians(45.0)
        )
        
        # 黑色粗线条
        self.shaders = [
            shaders.ConstantColorShader(
                color=(0.0, 0.0, 0.0, 1.0)
            ),
            shaders.ConstantThicknessShader(
                thickness=3.0
            )
        ]
```

### 钢笔画效果

```python
import freestyle
import freestyle.shaders as shaders

class SketchStyle:
    def __init__(self):
        self.shaders = [
            shaders.BluePrintShader(
                blueprint_type=shaders.BLUEPRINT_CIRCLES,
                thickness=1.5,
                color=(0.2, 0.2, 0.3, 1.0)
            )
        ]
```

### 渐变线条效果

```python
import freestyle.shaders as shaders

class GradientStyle:
    def __init__(self):
        self.shaders = [
            shaders.GradientColorShader(
                color1=(0.0, 0.0, 0.0, 1.0),
                color2=(0.0, 0.5, 1.0, 1.0),
                axis='x'
            ),
            shaders.Curvature2DThicknessShader(
                thickness_min=1.0,
                thickness_max=4.0
            )
        ]
```

## 与渲染管线集成

```python
import bpy

# 创建 Freestyle 渲染层
scene = bpy.context.scene
scene.render.use_freestyle = True

# 设置线条样式
scene.render.line_thickness = 2.0

# 添加线条样式模块
style_module = scene.render.freestyle_settings.linesets.new("MyStyle")
style_module.select_silhouette = True
style_module.select_contour = True
style_module.select_edge_mark = False

# 加载自定义样式模块
style_module.module_name = "my_style_module"
```

## 性能优化

```python
import freestyle

# 使用适当的谓词组合减少计算量
predicates.AND(
    predicates.GT_长度(50.0),      # 首先过滤短边
    predicates.LT_距离到相机(100.0) # 然后过滤远距离
)

# 使用适当的链式迭代器
chainer = chaining_iterators.角度链式迭代器(
    angle_threshold=radians(30)   # 较小的阈值产生更短、更精确的线条
)

# 限制笔触着色器数量
self.shaders = [
    shaders.ConstantColorShader(color=(0, 0, 0, 1)),
    shaders.ConstantThicknessShader(thickness=2.0)
]
# 避免使用复杂的纹理着色器
```


