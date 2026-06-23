# bpy_ops_ui_workspace

---
## bpy_ops_ui

# bpy.ops.ui - 用户界面操作模块

Blender用户界面操作符，用于控制UI元素的显示和交互。

## 概述

`bpy.ops.ui` 模块提供用于操作Blender用户界面的功能，包括面板控制、菜单操作和UI元素管理。

## 核心功能

### 面板操作

```python
import bpy

# 展开所有面板
bpy.ops.ui.expanded_toggle(
    all=True
)

# 收起所有面板
bpy.ops.ui.expanded_toggle(
    all=True
)

# 切换面板展开
bpy.ops.ui.expanded_toggle(
    all=False
)

# 重置面板布局
bpy.ops.ui.reset_default(
    release=True
)
```

### 数值输入

```python
# 数值输入
bpy.ops.ui.copy_as_driver()

# 复制为驱动
bpy.ops.ui.copy_as_driver()

# 粘贴驱动
bpy.ops.ui.paste_driver()

# 数值弹窗
bpy.ops.ui.数值_input(
    data_path='scale',
    data_path_index=0
)
```

### 搜索功能

```python
# 搜索操作符
bpy.ops.ui.search_menu()

# 搜索菜单
bpy.ops.ui.search_menu(
    menu='SEARCH_MT '
)

# 打开搜索
bpy.ops.ui.search_menu()
```

### 属性操作

```python
# 复制属性
bpy.ops.ui.copy_property()

# 粘贴属性
bpy.ops.ui.paste_property()

# 重置属性
bpy.ops.ui.reset_property()

# 应用到所有
bpy.ops.ui.apply_property()
```

### 颜色选择

```python
# 颜色选择
bpy.ops.ui.color_picker(
    data_path='color'
)

# 颜色选择器操作
bpy.ops.ui.color_picker(
    data_path='active_material.diffuse_color',
    data_path_index=0,
    color=(1.0, 0.5, 0.25, 1.0)
)
```

### 快捷键操作

```python
# 添加快捷键
bpy.ops.ui.add_shortcut()

# 编辑快捷键
bpy.ops.ui.edit_shortcut()

# 删除快捷键
bpy.ops.ui.remove_shortcut()

# 重置快捷键
bpy.ops.ui.reset_shortcut()
```

## 实用函数

```python
def collapse_all_panels():
    """收起所有面板"""
    bpy.ops.ui.expanded_toggle(all=True)

def expand_all_panels():
    """展开所有面板"""
    bpy.ops.ui.expanded_toggle(all=False)

def reset_ui_layout():
    """重置UI布局"""
    bpy.ops.ui.reset_default(release=True)

def copy_property_to_driver(data_path, index=0):
    """复制属性为驱动"""
    bpy.ops.ui.copy_as_driver()
    # 在目标属性上右键粘贴

def search_menu_items(search_text):
    """搜索菜单项"""
    bpy.ops.ui.search_menu()
    # 在搜索框中输入文本

def reset_active_property():
    """重置活动属性"""
    bpy.ops.ui.reset_property()

def apply_property_to_all():
    """应用属性到所有"""
    bpy.ops.ui.apply_property()
```

## 常见问题排查

### 面板不响应
```python
# 检查面板状态
print(f"面板展开: {bpy.context.preferences.system.view.use_expanded_panels}")

# 重置UI
bpy.ops.ui.reset_default(release=True)

# 刷新UI
bpy.ops.wm.redraw_timer(type='DRAW_WIN_SWAP', iterations=1)
```

### 属性无法编辑
```python
# 检查属性锁定
data_path = 'active_object.location'
# 检查该属性是否被锁定
print(f"属性可编辑: {not getattr(bpy.context.active_object, 'lock_location', [False, False, False])[0]}")

# 解除锁定
bpy.context.active_object.lock_location = (False, False, False)
bpy.context.active_object.lock_rotation = (False, False, False)
bpy.context.active_object.lock_scale = (False, False, False)
```

### 搜索菜单不工作
```python
# 检查区域类型
print(f"区域类型: {bpy.context.area.type}")

# 确保在支持搜索的区域
if bpy.context.area.type in ['PROPERTIES', 'DOPESHEET_EDITOR']:
    print("支持搜索菜单")
else:
    print("当前区域不支持搜索菜单")

# 尝试快捷键搜索
# 按F3打开搜索菜单
```

### UI显示问题
```python
# 检查UI缩放
print(f"UI缩放: {bpy.context.preferences.system.ui_scale}")

# 重启UI
bpy.ops.wm.read_homefile()

# 检查显示设置
print(f"主题: {bpy.context.preferences.ui.theme}")
```


---
## bpy_ops_wm

# bpy.ops.wm - 窗口管理操作模块

Blender窗口管理操作符，用于管理窗口、上下文和系统级操作。

## 概述

`bpy.ops.wm` 模块提供用于管理Blender窗口、上下文、系统和用户界面操作的功能。这是Blender中最通用的操作符模块之一。

## 核心功能

### 文件操作

```python
import bpy

# 新建文件
bpy.ops.wm.read_homefile(
    use_empty=True
)

# 打开文件
bpy.ops.wm.open_mainfile(
    filepath='//project.blend',
    load_ui=True
)

# 保存文件
bpy.ops.wm.save_mainfile(
    filepath='//project.blend',
    write_python=True
)

# 另存为
bpy.ops.wm.save_as_mainfile(
    filepath='//new_project.blend',
    copy=True
)

# 保存副本
bpy.ops.wm.save_homefile()

# 恢复上次保存
bpy.ops.wm.revert_mainfile()

# 追加文件
bpy.ops.wm.append(
    filepath='//library.blend',
    directory='//library.blend/Object/',
    files=[{'name': 'Cube'}],
    link=False,
    relative_path=True
)

# 链接文件
bpy.ops.wm.link(
    filepath='//library.blend',
    directory='//library.blend/Object/',
    files=[{'name': 'Cube'}],
    relative_path=True
)
```

### 导入操作

```python
# 导入主菜单
bpy.ops.wm.read_homefile(
    use_factory_settings=True
)

# 导入用户设置
bpy.ops.wm.read_user_interface()

# 导入键位设置
bpy.ops.wm.keyconfig_import(
    filepath='//keymap.py'
)

# 导入主题
bpy.ops.wm.theme_import(
    filepath='//theme.xml'
)

# 导入配
```python
# 导出配
bpy.ops.wm.properties_add(data_path='scene.render.resolution_percentage')
```

### 上下文操作

```python
# 复制数据路径
bpy.ops.wm.context_path_copy(
    data_path='active_object.location'
)

# 粘贴数据路径
bpy.ops.wm.context_path_paste()

# 枚举数值
bpy.ops.wm.context_enum_parse(
    data_path='object.type',
    items='["MESH", "CURVE", "SURFACE", "META", "FONT", "ARMATURE", "LATTICE", "EMPTY", "CAMERA", "LIGHT"]'
)

# 设置布尔值
bpy.ops.wm.context_boolean_set(
    data_path='object.select_get()',
    value=True
)

# 设置浮点值
bpy.ops.wm.context_set_float(
    data_path='object.location.x',
    value=1.0
)

# 设置整数值
bpy.ops.wm.context_set_int(
    data_path='object.pass_index',
    value=1
)

# 设置字符串
bpy.ops.wm.context_set_string(
    data_path='object.name',
    value='NewName'
)
```

### 工具操作

```python
# 运行Python脚本
bpy.ops.wm.python_file_run(
    filepath='//script.py'
)

# 运行Python控制台命令
bpy.ops.wm.console_toggle()

# 切换脚本区域
bpy.ops.wm.script_reload(
    doctors=True
)

# 重新加载脚本
bpy.ops.wm.script_reload(
    module='my_module'
)
```

### 批处理操作

```python
# 批量打开文件
bpy.ops.wm.open_mainfile(
    filepath='file1.blend'
)

# 批量保存
bpy.ops.wm.save_mainfile(
    filepath='project.blend'
)

# 导出所有打开的文件
bpy.ops.wm.save_all_mainfile(
    path='//backup'
)
```

### 撤销重做

```python
# 撤销
bpy.ops.wm.undo()

# 重做
bpy.ops.wm.redo()

# 撤销历史步数
bpy.ops.wm.undo_push(
    message='自定义操作'
)

# 清除撤销历史
bpy.ops.wm.undo_history_clear()
```

### 窗口管理

```python
# 新建窗口
bpy.ops.wm.window_new()

# 最大化窗口
bpy.ops.wm.window_fullscreen_toggle()

# 关闭窗口
bpy.ops.wm.window_close()

# 切换控制台
bpy.ops.wm.console_toggle()

# 设置窗口标题
bpy.ops.wm.window_title_set(
    title='Blender - Project'
)
```

### 进度条

```python
# 显示进度条
bpy.ops.wm.progress_start(
    title='Processing',
    message='正在处理...'
)

# 更新进度
bpy.ops.wm.progress_update(
    value=0.5
)

# 结束进度条
bpy.ops.wm.progress_end()

# 取消进度
bpy.ops.wm.progress_cancel()
```

### 系统操作

```python
# 调用菜单
bpy.ops.wm.call_menu(
    name='VIEW3D_MT_object_context_menu'
)

# 调用面板
bpy.ops.wm.call_panel(
    name='DATA_PT_context_material',
    keep_open=True
)

# 调用操作符
bpy.ops.wm.call_operator(
    name='object.simple_operator',
    properties={'prop': value}
)

# 设置键位
bpy.ops.wm.keyconfig_assign(
    filepath='//keymap.py'
)

# 重置键位
bpy.ops.wm.keyconfig_reset()
```

## 实用函数

```python
def save_backup():
    """保存备份"""
    import time
    timestamp = time.strftime('%Y%m%d_%H%M%S')
    bpy.ops.wm.save_as_mainfile(
        filepath=f'//backup_{timestamp}.blend',
        copy=True
    )

def export_all_objects(filepath):
    """导出所有对象"""
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.export_scene.fbx(filepath=filepath)
    bpy.ops.object.select_all(action='DESELECT')

def batch_process_files(file_list, output_dir):
    """批量处理文件"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for filepath in file_list:
        bpy.ops.wm.open_mainfile(filepath=filepath)
        # 处理当前文件
        process_current_file()
        # 保存到输出目录
        output_path = os.path.join(output_dir, os.path.basename(filepath))
        bpy.ops.wm.save_mainfile(filepath=output_path)

def create_project_backup(project_path, backup_dir):
    """创建项目备份"""
    import os
    import shutil
    import datetime
    
    if not os.path.exists(backup_dir):
        os.makedirs(backup_dir)
    
    timestamp = datetime.datetime.now().strftime('%Y%m%d_%H%M%S')
    backup_path = os.path.join(backup_dir, f'backup_{timestamp}')
    
    shutil.copy2(project_path, backup_path)
    print(f'备份创建成功: {backup_path}')

def import_objects_from_library(library_path, object_names):
    """从库中导入对象"""
    bpy.ops.wm.append(
        filepath=library_path,
        directory=f'{library_path}/Object/',
        files=[{'name': name} for name in object_names],
        link=False,
        relative_path=True
    )

def link_objects_from_library(library_path, object_names):
    """从库中链接对象"""
    bpy.ops.wm.link(
        filepath=library_path,
        directory=f'{library_path}/Object/',
        files=[{'name': name} for name in object_names],
        relative_path=True
    )
```

## 常见问题排查

### 文件保存失败
```python
# 检查文件路径
filepath = '//project.blend'
print(f"绝对路径: {bpy.path.abspath(filepath)}")

# 检查目录是否存在
import os
dir_path = os.path.dirname(bpy.path.abspath(filepath))
print(f"目录存在: {os.path.exists(dir_path)}")

# 检查文件权限
try:
    with open(bpy.path.abspath(filepath), 'w') as f:
        f.write('test')
    print("可写")
except PermissionError:
    print("权限不足")

# 检查场景是否有效
print(f"场景有效: {bpy.context.scene is not None}")

# 尝试保存副本
bpy.ops.wm.save_as_mainfile(filepath='//copy.blend', copy=True)
```

### 导入失败
```python
# 检查文件是否存在
filepath = '//library.blend'
print(f"文件存在: {os.path.exists(bpy.path.abspath(filepath))}")

# 检查Blend文件版本
try:
    bpy.ops.wm.open_mainfile(filepath=filepath, load_ui=False)
    print("文件可打开")
except:
    print("文件损坏或版本不兼容")

# 检查数据路径
directory = '//library.blend/Object/'
print(f"目录存在: {os.path.exists(bpy.path.abspath(directory))}")

# 列出可用数据块
print("可用对象:")
for obj in bpy.data.objects:
    print(f"  {obj.name}")
```

### 撤销历史耗尽
```python
# 检查撤销步数
print(f"撤销次数: {bpy.context.scene.undo_steps}")

# 增加撤销步数
bpy.context.scene.undo_steps = 256

# 清除历史
bpy.ops.wm.undo_history_clear()

# 手动推送操作
bpy.ops.wm.undo_push(message='我的操作')

# 检查内存使用
print(f"内存: {bpy.context.preferences.filepaths.file_preview_type}")
```

### 窗口问题
```python
# 检查窗口数
print(f"窗口数: {len(bpy.data.window_managers[0].windows)}")

# 检查活动窗口
print(f"活动窗口: {bpy.context.window}")

# 切换到下一个窗口
for win in bpy.data.window_managers[0].windows:
    bpy.context.window_manager.windows.active = win

# 重新打开窗口
bpy.ops.wm.window_new()

# 重置界面
bpy.ops.wm.read_homefile(use_factory_settings=True)
```


---
## bpy_ops_wm_path_open

# bpy.ops.wm - 路径打开操作模块

Blender窗口管理器路径操作符，用于打开文件路径和URL。

## 概述

`bpy.ops.wm.path_open` 模块提供用于在文件管理器中打开指定路径的功能，支持文件、目录和URL的打开。

## 核心功能

### 打开文件路径

```python
import bpy

# 打开文件
bpy.ops.wm.path_open(
    filepath='//file.txt'
)

# 打开目录
bpy.ops.wm.path_open(
    filepath='//'
)

# 打开绝对路径
bpy.ops.wm.path_open(
    filepath='C:/project/blend/file.blend'
)

# 打开相对路径
bpy.ops.wm.path_open(
    filepath='./textures/'
)
```

### 打开纹理目录

```python
# 打开纹理目录
bpy.ops.wm.path_open(
    filepath='//textures/'
)

# 打开图像目录
bpy.ops.wm.path_open(
    filepath='//images/'
)
```

### 打开导出目录

```python
# 打开导出目录
bpy.ops.wm.path_open(
    filepath='//exports/'
)

# 打开渲染目录
bpy.ops.wm.path_open(
    filepath='//renders/'
)
```

### 打开项目目录

```python
# 打开blend文件目录
bpy.ops.wm.path_open(
    filepath='//'
)

# 打开上一级目录
bpy.ops.wm.path_open(
    filepath='../'
)
```

### 打开绝对路径

```python
# 打开Windows路径
bpy.ops.wm.path_open(
    filepath='C:/Users/Name/Documents/blender/'
)

# 打开网络路径
bpy.ops.wm.path_open(
    filepath='//server/share/project/'
)
```

## 实用函数

```python
def open_blend_directory():
    """打开blend文件所在目录"""
    bpy.ops.wm.path_open(filepath='//')

def open_texture_folder():
    """打开纹理文件夹"""
    texture_dir = bpy.path.abspath('//textures/')
    if bpy.path.exists(texture_dir):
        bpy.ops.wm.path_open(filepath=texture_dir)
    else:
        print(f"纹理目录不存在: {texture_dir}")

def open_render_folder():
    """打开渲染输出文件夹"""
    render_path = bpy.context.scene.render.filepath
    render_dir = bpy.path.dirname(render_path)
    if bpy.path.exists(render_dir):
        bpy.ops.wm.path_open(filepath=render_dir)
    else:
        print(f"渲染目录不存在: {render_dir}")

def open_export_folder():
    """打开导出文件夹"""
    export_dir = bpy.path.abspath('//exports/')
    if bpy.path.exists(export_dir):
        bpy.ops.wm.path_open(filepath=export_dir)
    else:
        print(f"导出目录不存在: {export_dir}")

def open_addon_folder():
    """打开插件文件夹"""
    addon_dir = bpy.utils.user_resource('SCRIPTS', path='addons')
    if bpy.path.exists(addon_dir):
        bpy.ops.wm.path_open(filepath=addon_dir)
    else:
        print(f"插件目录不存在: {addon_dir}")

def open_config_folder():
    """打开配置文件夹"""
    config_dir = bpy.utils.user_resource('CONFIG')
    if bpy.path.exists(config_dir):
        bpy.ops.wm.path_open(filepath=config_dir)
    else:
        print(f"配置目录不存在: {config_dir}")

def open_temp_folder():
    """打开临时文件夹"""
    import tempfile
    temp_dir = tempfile.gettempdir()
    if bpy.path.exists(temp_dir):
        bpy.ops.wm.path_open(filepath=temp_dir)
    else:
        print(f"临时目录不存在: {temp_dir}")

def open_user_script_folder():
    """打开用户脚本文件夹"""
    script_dir = bpy.utils.user_resource('SCRIPTS', path='startup')
    if bpy.path.exists(script_dir):
        bpy.ops.wm.path_open(filepath=script_dir)
    else:
        print(f"脚本目录不存在: {script_dir}")
```

## 常见问题排查

### 路径不存在
```python
# 检查路径
filepath = '//textures/'
print(f"路径存在: {bpy.path.exists(filepath)}")

# 获取绝对路径
abs_path = bpy.path.abspath(filepath)
print(f"绝对路径: {abs_path}")
print(f"绝对路径存在: {bpy.path.exists(abs_path)}")

# 列出目录内容
if bpy.path.exists(abs_path):
    import os
    print(f"目录内容: {os.listdir(abs_path)}")
```

### 权限问题
```python
# 检查路径权限
import os
filepath = '//exports/'
abs_path = bpy.path.abspath(filepath)

if os.access(abs_path, os.W_OK):
    print("可写")
else:
    print("不可写")
    print("请检查目录权限")

# 尝试创建目录
if not os.path.exists(abs_path):
    os.makedirs(abs_path, exist_ok=True)
    print("已创建目录")
```

### 路径格式问题
```python
# 检查路径格式
filepath = '//textures/image.png'
print(f"文件存在: {bpy.path.exists(filepath)}")

# 使用正确的路径分隔符
filepath = '//textures/image.png'
print(f"路径: {filepath}")

# 规范化路径
import os
norm_path = os.path.normpath(filepath)
print(f"规范化路径: {norm_path}")
```

### 网络路径问题
```python
# 检查网络路径
filepath = '//server/project/'
abs_path = bpy.path.abspath(filepath)
print(f"网络路径: {abs_path}")

# 检查是否可访问
import os
if os.path.exists(abs_path):
    print("网络路径可访问")
    print(f"目录内容: {os.listdir(abs_path)}")
else:
    print("网络路径不可访问")
```


---
## bpy_ops_screen_region_toggle

# bpy.ops.screen - 区域切换操作模块

Blender屏幕区域切换操作符，用于控制编辑器区域的显示和隐藏。

## 概述

`bpy.ops.screen.region_toggle` 模块提供用于切换Blender编辑器区域显示状态的功能，包括工具栏、侧边栏、面板等区域的显示控制。

## 核心功能

### 区域切换

```python
import bpy

# 切换工具栏
bpy.ops.screen.region_toggle(
    region_type='TOOL_PROPS'
)

# 切换侧边栏
bpy.ops.screen.region_toggle(
    region_type='UI'
)

# 切换导航面板
bpy.ops.screen.region_toggle(
    region_type='NAVIGATION_REGION'
)

# 切换头部区域
bpy.ops.screen.region_toggle(
    region_type='HEADER'
)

# 切换脚部区域
bpy.ops.screen.region_toggle(
    region_type='FOOTER'
)
```

### 工具栏切换

```python
# 打开工具栏
bpy.ops.screen.region_toggle(
    region_type='TOOL_PROPS'
)

# 关闭工具栏
bpy.ops.screen.region_toggle(
    region_type='TOOL_PROPS'
)

# 切换工具栏
def toggle_toolbar():
    bpy.ops.screen.region_toggle(region_type='TOOL_PROPS')
```

### UI区域切换

```python
# 打开侧边栏
bpy.ops.screen.region_toggle(
    region_type='UI'
)

# 关闭侧边栏
bpy.ops.screen.region_toggle(
    region_type='UI'
)

# 切换侧边栏
def toggle_sidebar():
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_ui = not space.show_region_ui
```

### 导航区域切换

```python
# 打开导航面板
bpy.ops.screen.region_toggle(
    region_type='NAVIGATION_REGION'
)

# 关闭导航面板
bpy.ops.screen.region_toggle(
    region_type='NAVIGATION_REGION'
)
```

### 头部区域切换

```python
# 切换头部
bpy.ops.screen.region_toggle(
    region_type='HEADER'
)

# 显示头部
def show_header():
    for area in bpy.context.screen.areas:
        area.show_menus = True

# 隐藏头部
def hide_header():
    for area in bpy.context.screen.areas:
        area.show_menus = False
```

### 脚部区域切换

```python
# 切换脚部
bpy.ops.screen.region_toggle(
    region_type='FOOTER'
)
```

## 实用函数

```python
def toggle_toolshelf():
    """切换工具架"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = not space.show_region_toolbar

def toggle_ui_panel():
    """切换UI面板"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_ui = not space.show_region_ui

def toggle_navigation():
    """切换导航面板"""
    bpy.ops.screen.region_toggle(region_type='NAVIGATION_REGION')

def show_all_regions():
    """显示所有区域"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = True
            space.show_region_ui = True
            space.show_region_header = True

def hide_all_regions():
    """隐藏所有区域"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = False
            space.show_region_ui = False
            space.show_region_header = False

def maximize_3d_view():
    """最大化3D视图"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = False
            space.show_region_ui = False
            space.show_region_header = False

def restore_regions():
    """恢复区域显示"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = True
            space.show_region_ui = True
            space.show_region_header = True

def toggle_fullscreen():
    """切换全屏"""
    bpy.ops.screen.screen_full_area()
```

## 常见问题排查

### 区域不显示
```python
# 检查区域状态
space = bpy.context.area.spaces.active
print(f"工具架: {space.show_region_toolbar}")
print(f"UI面板: {space.show_region_ui}")
print(f"头部: {space.show_region_header}")

# 启用所有区域
space.show_region_toolbar = True
space.show_region_ui = True
space.show_region_header = True

# 检查区域类型
print(f"区域类型: {bpy.context.area.type}")

# 确保在3D视图
if bpy.context.area.type != 'VIEW_3D':
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            bpy.context.window.screen.areas[0] = area
            break
```

### 区域切换无响应
```python
# 检查区域类型
print(f"当前区域类型: {bpy.context.area.type}")

# 检查区域是否存在
region_types = ['TOOL_PROPS', 'UI', 'NAVIGATION_REGION', 'HEADER', 'FOOTER']
for rtype in region_types:
    print(f"{rtype}: 支持")

# 重置视图
bpy.ops.screen.screen_full_area(use_hide_panels=False)

# 刷新UI
bpy.ops.wm.redraw_timer(type='DRAW_WIN_SWAP', iterations=1)
```

### 全屏问题
```python
# 检查全屏状态
space = bpy.context.area.spaces.active
print(f"全屏: {space.use_fullscreen}")

# 退出全屏
if space.use_fullscreen:
    bpy.ops.screen.screen_full_area(use_hide_panels=False)

# 切换全屏
bpy.ops.screen.screen_full_area(use_hide_panels=True)
```

### 区域布局混乱
```python
# 重置工作区
bpy.ops.wm.read_homefile(use_empty=True)

# 重置当前工作区
bpy.ops.wm.read_homefile()

# 保存当前布局为默认
# 在Blender中: 工作区 -> 保存为默认
```


---
## bpy_ops_toolshelf

# bpy.ops.toolshelf - 工具架操作模块

Blender工具架操作符，用于管理工具架面板和工具选择。

## 概述

`bpy.ops.toolshelf` 模块提供用于操作Blender工具架（Tool Shelf）面板的功能，包括面板切换、工具选择和参数调整。

## 核心功能

### 面板操作

```python
import bpy

# 切换到雕刻面板
bpy.ops.toolshelf.tab_change(
    tab='sculpt'
)

# 切换到雕刻面板
bpy.ops.toolshelf.tab_change(
    tab='vertex_paint'
)

# 切换到顶点绘制面板
bpy.ops.toolshelf.tab_change(
    tab='vertex_paint'
)

# 切换到权重绘制面板
bpy.ops.toolshelf.tab_change(
    tab='weight_paint'
)

# 切换到纹理绘制面板
bpy.ops.toolshelf.tab_change(
    tab='texture_paint'
)

# 切换到编辑面板
bpy.ops.toolshelf.tab_change(
    tab='mesh_edit'
)

# 切换到UV编辑面板
bpy.ops.toolshelf.tab_change(
    tab='uv_edit'
)

# 切换到粒子面板
bpy.ops.toolshelf.tab_change(
    tab='particle_edit'
)
```

### 工具架切换

```python
# 打开工具架
bpy.ops.screen.toolbar_prompt(
    tool_name='builtin.move'
)

# 关闭工具架
bpy.ops.screen.toolbar_prompt(
    tool_name='builtin.move'
)

# 展开工具架
bpy.ops.screen.toolbar_prompt(
    tool_name='builtin.move',
    prompt='OPEN'
)

# 收起工具架
bpy.ops.screen.toolbar_prompt(
    tool_name='builtin.move',
    prompt='CLOSE'
)
```

### 工具选择

```python
# 选择移动工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.move'
)

# 选择旋转工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.rotate'
)

# 选择缩放工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.scale'
)

# 选择智能变换工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.transform'
)

# 选择雕刻工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.sculpt'
)

# 选择重新拓扑工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.measure'
)

# 选择注释工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.annotate'
)

# 选择选择工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.select'
)
```

### 笔刷操作

```python
# 选择笔刷
bpy.ops.paint.brush_select(
    sculpt_tool='DRAW'
)

# 选择雕刻笔刷
bpy.ops.paint.brush_select(
    sculpt_tool='DRAW',
    texture_paint_tool='DRAW'
)

# 选择顶点绘制笔刷
bpy.ops.paint.brush_select(
    vertex_paint_tool='MIX'
)

# 选择权重绘制笔刷
bpy.ops.paint.brush_select(
    weight_paint_tool='DRAW'
)

# 选择纹理绘制笔刷
bpy.ops.paint.brush_select(
    texture_paint_tool='DRAW'
)
```

### 模式切换

```python
# 切换到对象模式
bpy.ops.object.mode_set(
    mode='OBJECT'
)

# 切换到编辑模式
bpy.ops.object.mode_set(
    mode='EDIT'
)

# 切换到雕刻模式
bpy.ops.sculpt.sculptmode_toggle()

# 切换到顶点绘制模式
bpy.ops.paint.vertex_paint_toggle()

# 切换到权重绘制模式
bpy.ops.paint.weight_paint_toggle()

# 切换到纹理绘制模式
bpy.ops.paint.texture_paint_toggle()

# 切换到姿势模式
bpy.ops.pose.select_hierarchy(
    direction='PARENT'
)
```

## 实用函数

```python
def show_toolshelf():
    """显示工具架"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = True

def hide_toolshelf():
    """隐藏工具架"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = False

def toggle_toolshelf():
    """切换工具架显示"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = not space.show_region_toolbar

def switch_to_sculpt_mode():
    """切换到雕刻模式"""
    if bpy.context.active_object:
        bpy.ops.sculpt.sculptmode_toggle()

def switch_to_edit_mode():
    """切换到编辑模式"""
    if bpy.context.active_object:
        bpy.ops.object.mode_set(mode='EDIT')

def switch_to_object_mode():
    """切换到对象模式"""
    if bpy.context.active_object:
        bpy.ops.object.mode_set(mode='OBJECT')

def select_brush(brush_name):
    """选择笔刷"""
    brushes = bpy.data.brushes
    if brush_name in brushes:
        bpy.context.tool_settings.sculpt.brush = brushes[brush_name]

def set_brush_size(size):
    """设置笔刷大小"""
    bpy.context.tool_settings.sculpt.brush.size = size

def set_brush_strength(strength):
    """设置笔刷强度"""
    bpy.context.tool_settings.sculpt.brush.strength = strength

def show_ui_panel():
    """显示UI面板"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_ui = True

def hide_ui_panel():
    """隐藏UI面板"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_ui = False
```

## 常见问题排查

### 工具架不显示
```python
# 检查工具架状态
space = bpy.context.area.spaces.active
print(f"工具架: {space.show_region_toolbar}")

# 启用工具架
space.show_region_toolbar = True

# 检查区域
print(f"区域类型: {bpy.context.area.type}")

# 确保在3D视图中
if bpy.context.area.type != 'VIEW_3D':
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            bpy.context.window.screen.areas[0] = area
            break
```

### 工具无法选择
```python
# 检查当前模式
print(f"当前模式: {bpy.context.mode}")

# 检查活动对象
obj = bpy.context.active_object
print(f"活动对象: {obj}")

if obj:
    print(f"对象类型: {obj.type}")

# 确保在正确的模式
if bpy.context.mode == 'OBJECT':
    bpy.ops.object.mode_set(mode='EDIT')

# 检查工具
print(f"当前工具: {bpy.context.workspace.tools.active}")
```

### 笔刷不工作
```python
# 检查笔刷
brush = bpy.context.tool_settings.sculpt.brush
print(f"当前笔刷: {brush}")

# 检查笔刷设置
print(f"笔刷大小: {brush.size}")
print(f"笔刷强度: {brush.strength}")

# 选择正确笔刷
bpy.ops.paint.brush_select(sculpt_tool='DRAW')

# 重置笔刷设置
brush.size = 50
brush.strength = 1.0
```

### 面板切换问题
```python
# 检查当前面板
print(f"当前选项卡: {bpy.context.workspace.tools.active.paint_mode}")

# 切换到雕刻
bpy.ops.toolshelf.tab_change(tab='sculpt')

# 检查笔刷
bpy.ops.paint.brush_select(sculpt_tool='DRAW')

# 重置工具架
bpy.ops.screen.toolbar_prompt(tool_name='builtin.move', prompt='OPEN')
```


---
## bpy_ops_header

# bpy.ops.header - 标题栏操作模块

Blender标题栏操作符，用于管理编辑器标题栏和菜单。

## 概述

`bpy.ops.header` 模块提供用于操作Blender各种编辑器标题栏的功能，包括菜单控制、工具栏切换和工作区管理。

## 核心功能

### 标题栏菜单

```python
import bpy

# 打开标题栏菜单
bpy.ops.header.menu(
    menu='HEADER_MT_context_menu'
)

# 打开文件菜单
bpy.ops.header.menu(
    menu='TOPBAR_MT_file'
)

# 打开编辑菜单
bpy.ops.header.menu(
    menu='TOPBAR_MT_edit'
)

# 打开渲染菜单
bpy.ops.header.menu(
    menu='TOPBAR_MT_render'
)

# 打开窗口菜单
bpy.ops.header.menu(
    menu='TOPBAR_MT_window'
)

# 打开帮助菜单
bpy.ops.header.menu(
    menu='TOPBAR_MT_help'
)
```

### 区域菜单

```python
# 打开区域菜单
bpy.ops.header.menu(
    menu='HEADER_MT_area'
)

# 打开视图菜单
bpy.ops.header.menu(
    menu='HEADER_MT_view'
)

# 打开选择菜单
bpy.ops.header.menu(
    menu='HEADER_MT_select'
)

# 打开编辑菜单
bpy.ops.header.menu(
    menu='HEADER_MT_edit'
)

# 打开对象菜单
bpy.ops.header.menu(
    menu='HEADER_MT_object'
)

# 打开标记菜单
bpy.ops.header.menu(
    menu='HEADER_MT_marker'
)
```

### 标题栏选择

```python
# 选择标题栏
bpy.ops.header.select(
    extend=False,
    deselect=False,
    toggle=False,
    deselect_all=True
)

# 全选标题栏
bpy.ops.header.select_all(
    action='SELECT'
)

# 取消全选标题栏
bpy.ops.header.select_all(
    action='DESELECT'
)

# 反选标题栏
bpy.ops.header.select_all(
    action='INVERT'
)
```

### 标题栏绘制

```python
# 刷新标题栏
bpy.ops.header.reports_banner_update()

# 更新标题栏
bpy.ops.header.header_update()

# 重绘标题栏
bpy.ops.header.draw(
    draw=COLLECTION_EDITOR_HT_header
)
```

### 工具栏操作

```python
# 切换工具栏
bpy.ops.screen.toolbar_prompt(
    tool_name='builtin.move'
)

# 展开工具栏
bpy.ops.screen.toolbar_prompt(
    tool_name='builtin.move',
    prompt='OPEN'
)

# 收起工具栏
bpy.ops.screen.toolbar_prompt(
    tool_name='builtin.move',
    prompt='CLOSE'
)
```

### 工作区操作

```python
# 添加工作区
bpy.ops.wm.workspace_add()

# 删除工作区
bpy.ops.wm.workspace_remove(
    workspace='默认'
)

# 切换到下一个工作区
bpy.ops.wm.workspace_cycle(
    direction='NEXT'
)

# 切换到上一个工作区
bpy.ops.wm.workspace_cycle(
    direction='PREV'
)

# 重命名工作区
bpy.ops.wm.workspace_duplicate()
```

### 区域操作

```python
# 最大化区域
bpy.ops.screen.screen_full_area(
    use_hide_panels=False
)

# 退出全屏
bpy.ops.screen.screen_full_area(
    use_hide_panels=True
)

# 翻转区域
bpy.ops.screen.region_flip()

# 区域查找
bpy.ops.screen.region_flip()
```

## 实用函数

```python
def show_workspace_menu():
    """显示工作区菜单"""
    bpy.ops.header.menu(menu='WORKSPACE_MT_selector')

def show_toolbar():
    """显示工具栏"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = True

def hide_toolbar():
    """隐藏工具栏"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_region_toolbar = False

def show_header():
    """显示标题栏"""
    for area in bpy.context.screen.areas:
        area.show_menus = True

def hide_header():
    """隐藏标题栏"""
    for area in bpy.context.screen.areas:
        area.show_menus = False

def toggle_fullscreen():
    """切换全屏"""
    bpy.ops.screen.screen_full_area()

def cycle_workspace_next():
    """切换到下一个工作区"""
    bpy.ops.wm.workspace_cycle(direction='NEXT')

def cycle_workspace_prev():
    """切换到上一个工作区"""
    bpy.ops.wm.workspace_cycle(direction='PREV')

def add_workspace(name='New Workspace'):
    """添加工作区"""
    bpy.ops.wm.workspace_add()
    ws = bpy.data.workspaces[-1]
    ws.name = name

def duplicate_workspace():
    """复制当前工作区"""
    bpy.ops.wm.workspace_duplicate()
```

## 常见问题排查

### 标题栏不显示
```python
# 检查区域类型
area = bpy.context.area
print(f"区域类型: {area.type}")
print(f"显示菜单: {area.show_menus}")

# 启用菜单
area.show_menus = True

# 刷新UI
bpy.ops.wm.redraw_timer(type='DRAW_WIN_SWAP', iterations=1)
```

### 工作区丢失
```python
# 检查工作区
print(f"工作区数: {len(bpy.data.workspaces)}")
for ws in bpy.data.workspaces:
    print(f"工作区: {ws.name}")

# 切换工作区
for ws in bpy.data.workspaces:
    if ws.name == 'Layout':
        bpy.context.window.workspace = ws
        break

# 添加新工作区
bpy.ops.wm.workspace_add()
```

### 工具栏问题
```python
# 检查工具栏状态
space = bpy.context.area.spaces.active
print(f"工具栏: {space.show_region_toolbar}")
print(f"UI面板: {space.show_region_ui}")
print(f"工具设置: {space.show_region_toolbar}")

# 启用工具栏
space.show_region_toolbar = True

# 重置工具
bpy.ops.wm.tool_set_by_id(name='builtin.move')
```

### 全屏问题
```python
# 检查当前状态
print(f"全屏: {bpy.context.area.spaces.active.use_fullscreen}")

# 退出全屏
if bpy.context.area.spaces.active.use_fullscreen:
    bpy.ops.screen.screen_full_area()

# 检查多边形编辑器状态
print(f"多边形编辑器: {bpy.context.area.spaces.active.show_region_toolbar}")
```


---
## bpy_ops_buttons

# bpy.ops.buttons - 按钮操作模块

Blender按钮操作符，用于在属性面板中操作各种按钮和控件。

## 概述

`bpy.ops.buttons` 模块提供用于在Blender属性面板、编辑器面板和其他UI区域中操作按钮和控件的功能。这对于UI自动化测试和批处理操作非常有用。

## 核心功能

### 属性面板按钮操作

```python
import bpy

# 刷新属性面板
bpy.ops.buttons.refresh()

# 更新所有属性
bpy.ops.buttons.update()

# 应用所有修改
bpy.ops.buttons.apply()

# 重置所有属性
bpy.ops.buttons.reset()

# 撤销属性修改
bpy.ops.buttons.undo()

# 重做属性修改
bpy.ops.buttons.redo()
```

### 数值按钮操作

```python
# 增加数值
bpy.ops.buttons.increment(
    delta=1
)

# 减少数值
bpy.ops.buttons.decrement(
    delta=-1
)

# 设置精确值
bpy.ops.buttons.set_value(
    value=1.5
)

# 拖动滑块
bpy.ops.buttons.drag(
    value=0.0,
    delta=1.0
)

# 循环数值
bpy.ops.buttons.cycle(
    reverse=False
)

# 反向循环
bpy.ops.buttons.cycle(
    reverse=True
)
```

### 复选框操作

```python
# 切换复选框
bpy.ops.buttons.toggle()

# 启用
bpy.ops.buttons.set_boolean(
    value=True
)

# 禁用
bpy.ops.buttons.set_boolean(
    value=False
)
```

### 颜色按钮操作

```python
# 设置颜色
bpy.ops.buttons.color_picker(
    color=(1.0, 0.0, 0.0, 1.0)
)

# 设置RGB颜色
bpy.ops.buttons.color_picker(
    color=(1.0, 0.0, 0.0)
)

# 设置HSV颜色
bpy.ops.buttons.color_picker(
    color=(0.0, 1.0, 1.0),
    color_mode='HSV'
)

# 切换颜色通道
bpy.ops.buttons.cycle_color()

# 切换颜色模式
bpy.ops.buttons.cycle_color_mode()

# 恢复默认颜色
bpy.ops.buttons.color_reset()
```

### 枚举按钮操作

```python
# 设置枚举值
bpy.ops.buttons.enum_set(
    value='OPTION_A'
)

# 循环枚举值
bpy.ops.buttons.enum_next()

# 反向循环枚举值
bpy.ops.buttons.enum_prev()

# 搜索枚举值
bpy.ops.buttons.enum_search(
    query='search_term'
)
```

### 字符串按钮操作

```python
# 设置字符串
bpy.ops.buttons.string_set(
    value='new_string'
)

# 清空字符串
bpy.ops.buttons.string_clear()

# 撤销字符串修改
bpy.ops.buttons.string_undo()

# 重做字符串修改
bpy.ops.buttons.string_redo()
```

### 向量按钮操作

```python
# 设置向量值
bpy.ops.buttons.vector_set(
    value=(1.0, 2.0, 3.0)
)

# 设置向量X分量
bpy.ops.buttons.vector_set_x(
    value=1.0
)

# 设置向量Y分量
bpy.ops.buttons.vector_set_y(
    value=2.0
)

# 设置向量Z分量
bpy.ops.buttons.vector_set_z(
    value=3.0
)

# 循环向量分量
bpy.ops.buttons.vector_cycle()
```

### 文件路径按钮操作

```python
# 选择文件
bpy.ops.buttons.file_browse(
    filepath='//textures/material.png',
    filter_blender=True,
    filter_image=True,
    filter_movie=False,
    filter_python=False,
    filter_font=True,
    filter_sound=False,
    filter_text=False,
    filter_archive=False
)

# 选择目录
bpy.ops.buttons.directory_browse(
    directory='//textures/'
)

# 选择图像
bpy.ops.buttons.file_browse(
    filepath='//image.png',
    filter_image=True
)

# 清除文件路径
bpy.ops.buttons.file_clear()

# 相对路径切换
bpy.ops.buttons.path_relative_toggle()
```

### ID选择按钮操作

```python
# 选择ID数据块
bpy.ops.buttons.id_select(
    id_name='Material',
    id_type='MATERIAL'
)

# 选择材质
bpy.ops.buttons.id_select(
    id_name='MyMaterial',
    id_type='MATERIAL'
)

# 选择纹理
bpy.ops.buttons.id_select(
    id_name='MyTexture',
    id_type='TEXTURE'
)

# 选择对象
bpy.ops.buttons.id_select(
    id_name='MyObject',
    id_type='OBJECT'
)

# 新建ID数据块
bpy.ops.buttons.id_new(
    id_type='MATERIAL',
    name='NewMaterial'
)

# 删除ID数据块
bpy.ops.buttons.id_delete()

# 复制ID数据块
bpy.ops.buttons.id_copy()

# 粘贴ID数据块
bpy.ops.buttons.id_paste()
```

### 数组按钮操作

```python
# 增加数组元素
bpy.ops.buttons.array_add()

# 移除数组元素
bpy.ops.buttons.array_remove()

# 移动数组元素
bpy.ops.buttons.array_move(
    from_index=0,
    to_index=1
)

# 重新排序数组
bpy.ops.buttons.array_reorder()
```

### 颜色渐变按钮操作

```python
# 添加渐变色标
bpy.ops.buttons.gradient_add(
    position=0.5,
    color=(1.0, 0.0, 0.0, 1.0)
)

# 移除渐变色标
bpy.ops.buttons.gradient_remove(
    index=0
)

# 选择渐变色标
bpy.ops.buttons.gradient_select(
    index=0
)

# 移动渐变色标
bpy.ops.buttons.gradient_move(
    from_index=0,
    to_index=1
)

# 设置渐变类型
bpy.ops.buttons.gradient_type(
    type='LINEAR'
)

# 循环渐变
bpy.ops.buttons.gradient_cycle()
```

### 关键帧按钮操作

```python
# 插入关键帧
bpy.ops.buttons.keyframe_insert()

# 删除关键帧
bpy.ops.buttons.keyframe_delete()

# 清除关键帧
bpy.ops.buttons.keyframe_clear()

# 复制关键帧
bpy.ops.buttons.keyframe_copy()

# 粘贴关键帧
bpy.ops.buttons.keyframe_paste()

# 跳转关键帧
bpy.ops.buttons.keyframe_jump(
    direction='NEXT'
)

# 关键帧类型切换
bpy.ops.buttons.keyframe_type()
```

## 实用函数

```python
def get_button_value(prop_path):
    """获取属性值"""
    props = bpy.context
    for attr in prop_path.split('.'):
        props = getattr(props, attr)
    return props

def set_material_color(mat_name, color):
    """设置材质颜色"""
    mat = bpy.data.materials.get(mat_name)
    if mat:
        bpy.context.active_object.active_material = mat
        bpy.ops.buttons.color_picker(color=color)

def toggle_shading_mode(mode):
    """切换着色模式"""
    bpy.context.space_data.shading.type = mode

def set_render_engine(engine_name):
    """设置渲染引擎"""
    bpy.context.scene.render.engine = engine_name

def toggle_viewport_overlays():
    """切换视口叠加层"""
    space = bpy.context.space_data
    space.overlay.show_overlays = not space.overlay.show_overlays

def cycle_display_mode():
    """循环显示模式"""
    modes = ['WIREFRAME', 'SOLID', 'MATERIAL', 'RENDERED']
    space = bpy.context.space_data
    current = modes.index(space.shading.type)
    space.shading.type = modes[(current + 1) % len(modes)]

def select_all_in_panel():
    """全选面板中的所有属性"""
    for area in bpy.context.screen.areas:
        if area.type == 'PROPERTIES':
            for region in area.regions:
                if region.type == 'WINDOW':
                    for ui_item in region.ui_items:
                        ui_item.select = True

def get_panel_properties(panel_id):
    """获取面板中的所有属性"""
    properties = []
    panel = eval(panel_id)
    if panel:
        for prop in panel.bl_rna.properties:
            if not prop.is_hidden:
                properties.append(prop.identifier)
    return properties
```

## 常见问题排查

### 按钮操作无响应
```python
# 检查是否在正确的上下文
print(f"当前区域类型: {bpy.context.area.type if bpy.context.area else 'None'}")

# 检查属性面板区域
for area in bpy.context.screen.areas:
    if area.type == 'PROPERTIES':
        print(f"属性面板区域: {area}")

# 确保面板已展开
bpy.ops.buttons.refresh()

# 尝试重新初始化上下文
import bpy
bpy.ops.buttons.update()
```

### 属性值不更新
```python
# 检查属性是否存在
prop_path = 'scene.render.engine'
try:
    value = eval(f'bpy.{prop_path}')
    print(f"属性值: {value}")
except:
    print(f"属性不存在: {prop_path}")

# 手动触发更新
bpy.ops.buttons.update()

# 检查属性是否可编辑
obj = bpy.context.active_object
print(f"可编辑: {not obj.library}")

# 检查ID块是否链接
print(f"库: {obj.library}")
```

### 颜色按钮问题
```python
# 检查颜色空间
space = bpy.context.space_data
if space:
    print(f"颜色管理: {space.used_view_map.color_management}")

# 检查材质
mat = bpy.context.active_object.active_material
if mat:
    print(f"材质使用节点: {mat.use_nodes}")

# 检查节点着色器
if mat.use_nodes:
    bsdf = mat.node_tree.nodes.get('Principled BSDF')
    if bsdf:
        print(f"基础颜色: {bsdf.inputs['Base Color'].default_value}")

# 重置颜色
bpy.ops.buttons.color_reset()
```


---
## bpy_ops_file

# bpy.ops.file 模块

文件操作模块，用于在文件浏览器中管理文件和目录。

## 概述

bpy.ops.file 模块包含用于在文件浏览器中执行文件操作、管理目录和执行外部程序的运算符。这些运算符控制文件的读取、写入、执行和路径管理。

## 常用操作

### 文件操作

```python
import bpy

# 刷新
bpy.ops.file.refresh()

# 选择所有
bpy.ops.file.select_all(action='SELECT')
bpy.ops.file.select_all(action='DESELECT')
bpy.ops.file.select_all(action='INVERT')

# 边界框选择
bpy.ops.file.select_border(gesture_mode=0, extend=False)

# 套索选择
bpy.ops.file.select_lasso(path=None, mode='SET')

# 选择书签
bpy.ops.file.select_bookmark(dir='NEXT')

# 取消选择书签
bpy.ops.file.select_bookmark(dir='PREV')
```

### 目录操作

```python
# 创建目录
bpy.ops.file.make_directory()

# 删除文件
bpy.ops.file.delete()

# 重命名文件
bpy.ops.file.rename()

# 复制文件
bpy.ops.file.copy(filepath='//destination/file.ext')

# 移动文件
bpy.ops.file.move(filepath='//new_location/file.ext')

# 执行文件
bpy.ops.file.execute()

# 取消文件执行
bpy.ops.file.cancel()

# 重试文件执行
bpy.ops.file.retry()
```

### 目录导航

```python
# 上一级目录
bpy.ops.file.parent()

# 刷新当前目录
bpy.ops.file.refresh()

# 跳转到路径
bpy.ops.file.directory_new()

# 创建新目录
bpy.ops.file.directory_new()

# 历史记录
bpy.ops.file.history()
```

### 书签操作

```python
# 添加书签
bpy.ops.file.bookmark_add(name='MyBookmark')

# 切换书签
bpy.ops.file.bookmark_toggle()

# 删除书签
bpy.ops.file.bookmark_delete()
```

### 过滤操作

```python
# 设置文件过滤
bpy.ops.file.filter(extension='.blend')

# 显示所有文件
bpy.ops.file.filter(extension='*')

# 只显示Blend文件
bpy.ops.file.filter(extension='.blend')

# 显示隐藏文件
bpy.ops.file.show_hidden()
```

### 路径操作

```python
# 复制路径
bpy.ops.file.copy_filepath()

# 复制绝对路径
bpy.ops.file.copy_filepath()

# 复制相对路径
bpy.ops.file.copy_filepath(filepath='//relative/path')

# 粘贴路径
bpy.ops.file.paste_filepath()

# 重命名
bpy.ops.file.rename()
```

### 外部程序

```python
# 打开文件
bpy.ops.file.open_recent()

# 打开外部编辑器
bpy.ops.file.external_edit()

# 执行脚本
bpy.ops.file.run_python_script()

# 执行系统命令
bpy.ops.file.run_system_command()
```

## 实际应用示例

### 批量重命名文件

```python
def batch_rename_files(directory, pattern, replacement):
    """批量重命名文件"""
    import os
    import re
    
    files = os.listdir(directory)
    
    for filename in files:
        if re.search(pattern, filename):
            new_name = re.sub(pattern, replacement, filename)
            old_path = os.path.join(directory, filename)
            new_path = os.path.join(directory, new_name)
            
            os.rename(old_path, new_path)
    
    # 刷新文件浏览器
    bpy.ops.file.refresh()
```

### 清理旧备份

```python
def cleanup_old_backups(directory, keep_count=5):
    """清理旧备份文件"""
    import os
    import glob
    
    # 获取所有备份文件
    backup_files = glob.glob(os.path.join(directory, 'backup_*.blend'))
    backup_files.sort(key=os.path.getmtime)
    
    # 删除旧备份
    if len(backup_files) > keep_count:
        for old_file in backup_files[:-keep_count]:
            os.remove(old_file)
            print(f"已删除: {old_file}")
    
    # 刷新
    bpy.ops.file.refresh()
```

### 复制项目文件

```python
def copy_project_files(source_dir, dest_dir, file_patterns=['*.blend', '*.py', '*.json']):
    """复制项目文件"""
    import os
    import shutil
    
    # 创建目标目录
    os.makedirs(dest_dir, exist_ok=True)
    
    for pattern in file_patterns:
        source_files = glob.glob(os.path.join(source_dir, '**', pattern), recursive=True)
        
        for file_path in source_files:
            rel_path = os.path.relpath(file_path, source_dir)
            dest_path = os.path.join(dest_dir, rel_path)
            
            os.makedirs(os.path.dirname(dest_path), exist_ok=True)
            shutil.copy2(file_path, dest_path)
            print(f"已复制: {rel_path}")
    
    bpy.ops.file.refresh()
```

### 同步资产目录

```python
def sync_assets_directory(source_dir, dest_dir):
    """同步资产目录"""
    import os
    import shutil
    
    # 复制所有文件
    for root, dirs, files in os.walk(source_dir):
        rel_path = os.path.relpath(root, source_dir)
        dest_root = os.path.join(dest_dir, rel_path)
        
        os.makedirs(dest_root, exist_ok=True)
        
        for file in files:
            source_file = os.path.join(root, file)
            dest_file = os.path.join(dest_root, file)
            
            # 如果源文件更新则复制
            if not os.path.exists(dest_file) or \
               os.path.getmtime(source_file) > os.path.getmtime(dest_file):
                shutil.copy2(source_file, dest_file)
    
    bpy.ops.file.refresh()
```

### 创建项目结构

```python
def create_project_structure(base_path, structure):
    """创建标准项目结构"""
    import os
    
    for name, substructure in structure.items():
        current_path = os.path.join(base_path, name)
        os.makedirs(current_path, exist_ok=True)
        
        if isinstance(substructure, dict):
            create_project_structure(current_path, substructure)
        elif isinstance(substructure, list):
            for subdir in substructure:
                subdir_path = os.path.join(current_path, subdir)
                os.makedirs(subdir_path, exist_ok=True)
    
    # 刷新
    bpy.ops.file.refresh()
    
    return base_path

# 示例用法
project_structure = {
    'assets': {
        'models': [],
        'textures': [],
        'materials': [],
        'animations': []
    },
    'scenes': [],
    'scripts': [],
    'reference': [],
    'exports': []
}
```

### 清理临时文件

```python
def cleanup_temp_files(directory, extensions=['.blend1', '.blend2', '~']):
    """清理临时文件"""
    import os
    import glob
    
    for ext in extensions:
        temp_files = glob.glob(os.path.join(directory, f'*{ext}'))
        
        for temp_file in temp_files:
            try:
                os.remove(temp_file)
                print(f"已删除临时文件: {temp_file}")
            except Exception as e:
                print(f"无法删除 {temp_file}: {e}")
    
    bpy.ops.file.refresh()
```

### 创建资源归档

```python
def create_assets_archive(source_dir, archive_path, formats=['.blend', '.fbx', '.obj']):
    """创建资源归档"""
    import os
    import zipfile
    
    with zipfile.ZipFile(archive_path, 'w', zipfile.ZIP_DEFLATED) as zipf:
        for root, dirs, files in os.walk(source_dir):
            for file in files:
                if any(file.endswith(fmt) for fmt in formats):
                    file_path = os.path.join(root, file)
                    arcname = os.path.relpath(file_path, source_dir)
                    zipf.write(file_path, arcname)
                    print(f"已添加: {arcname}")
    
    return archive_path
```

## 使用场景

- **文件管理**：在文件浏览器中管理文件
- **目录操作**：创建、重命名、移动文件
- **批量操作**：批量重命名、清理文件
- **项目组织**：创建项目目录结构
- **资产管理**：同步和备份资产
- **外部集成**：执行外部程序

## 注意事项

- 文件操作是不可逆的
- 确认后再删除文件
- 路径使用相对路径更灵活
- 批量操作前先备份
- 注意文件权限问题
- 同步操作可能覆盖文件


---
## bpy_ops_info

# bpy.ops.info - Info Area Operations Module

Blender 信息区域操作符，用于获取系统信息和日志记录。

## Overview

`bpy.ops.info` 模块包含信息区域的操作命令，支持报告选择、复制和系统信息获取。这个模块主要用于调试和信息查询。

## 核心功能

```python
import bpy

# 全选报告
bpy.ops.info.select_all(action='SELECT')

# 复制报告
bpy.ops.info.copy()

# 报告类型过滤
bpy.ops.info.report_type_update()
bpy.ops.info.report_copy()

# 重启 Blender
bpy.ops.info.restart_blender()
```

## 实际应用示例

### 信息报告管理器

```python
import bpy

class InfoReportManager:
    """信息报告管理器"""
    
    def __init__(self):
        self.reports = []
    
    def select_all_reports(self):
        """全选报告"""
        bpy.ops.info.select_all(action='SELECT')
    
    def deselect_all_reports(self):
        """取消选择"""
        bpy.ops.info.select_all(action='DESELECT')
    
    def select_by_type(self, report_type):
        """按类型选择"""
        self.select_all_reports()
        # 过滤类型
        for area in bpy.context.screen.areas:
            if area.type == 'INFO':
                for region in area.regions:
                    if region.type == 'WINDOW':
                        for ui_item in region.ui_items:
                            if hasattr(ui_item, 'report_type'):
                                if ui_item.report_type != report_type:
                                    ui_item.select = False
    
    def copy_all_reports(self):
        """复制所有报告"""
        self.select_all_reports()
        bpy.ops.info.copy()
        return bpy.context.window_manager.clipboard
    
    def copy_error_reports(self):
        """复制错误报告"""
        self.select_by_type('ERROR')
        return self.copy_all_reports()
    
    def copy_warning_reports(self):
        """复制警告报告"""
        self.select_by_type('WARNING')
        return self.copy_all_reports()
    
    def get_report_summary(self):
        """获取报告摘要"""
        summary = {
            'errors': 0,
            'warnings': 0,
            'info': 0,
            'debug': 0
        }
        
        for area in bpy.context.screen.areas:
            if area.type == 'INFO':
                for region in area.regions:
                    if region.type == 'WINDOW':
                        for ui_item in region.ui_items:
                            if hasattr(ui_item, 'report_type'):
                                summary[ui_item.report_type] = summary.get(ui_item.report_type, 0) + 1
        
        return summary
    
    def clear_all_reports(self):
        """清除所有报告"""
        bpy.ops.info.select_all(action='SELECT')
        bpy.ops.info.delete(type='SELECTED')
    
    def export_reports(self, filepath):
        """导出报告"""
        content = self.copy_all_reports()
        
        with open(filepath, 'w', encoding='utf-8') as f:
            f.write(content)
        
        return filepath
    
    def restart_blender_safe(self):
        """安全重启 Blender"""
        # 保存当前状态
        bpy.ops.wm.save_homefile()
        
        # 重启
        bpy.ops.info.restart_blender()


def setup_info_manager():
    """设置信息管理器"""
    manager = InfoReportManager()
    
    # 获取当前报告摘要
    summary = manager.get_report_summary()
    print(f"报告摘要: {summary}")
    
    return manager
```

### 系统诊断工具

```python
import bpy
import sys
import platform

class SystemDiagnostics:
    """系统诊断工具"""
    
    def __init__(self):
        self.diagnostics = {}
    
    def get_blender_info(self):
        """获取 Blender 信息"""
        return {
            'version': bpy.app.version_string,
            'version_major': bpy.app.version_major,
            'version_minor': bpy.app.version_minor,
            'build_hash': bpy.app.build_hash,
            'build_branch': bpy.app.build_branch,
            'platform': platform.system(),
            'architecture': platform.architecture(),
            'python_version': sys.version
        }
    
    def get_memory_usage(self):
        """获取内存使用"""
        return {
            'total_memory_mb': bpy.app.driver_mem_cache / (1024 * 1024),
            'texture_memory_mb': bpy.app.tex_mem_cache / (1024 * 1024)
        }
    
    def get_render_info(self):
        """获取渲染信息"""
        scene = bpy.context.scene
        return {
            'engine': scene.render.engine,
            'resolution': f"{scene.render.resolution_x}x{scene.render.resolution_y}",
            'fps': scene.render.fps,
            'frame_start': scene.frame_start,
            'frame_end': scene.frame_end
        }
    
    def get_object_stats(self):
        """获取对象统计"""
        stats = {
            'total_objects': len(bpy.data.objects),
            'meshes': 0,
            'cameras': 0,
            'lights': 0,
            'armatures': 0,
            'curves': 0
        }
        
        for obj in bpy.data.objects:
            stats[f'{obj.type}s'] = stats.get(f'{obj.type}s', 0) + 1
        
        return stats
    
    def run_full_diagnostics(self):
        """运行完整诊断"""
        self.diagnostics = {
            'blender': self.get_blender_info(),
            'memory': self.get_memory_usage(),
            'render': self.get_render_info(),
            'objects': self.get_object_stats()
        }
        
        return self.diagnostics
    
    def export_diagnostics(self, filepath):
        """导出诊断结果"""
        import json
        
        if not self.diagnostics:
            self.run_full_diagnostics()
        
        with open(filepath, 'w', encoding='utf-8') as f:
            json.dump(self.diagnostics, f, indent=2, ensure_ascii=False)
        
        return filepath
    
    def print_diagnostics(self):
        """打印诊断结果"""
        if not self.diagnostics:
            self.run_full_diagnostics()
        
        print("=== Blender 诊断信息 ===")
        print(f"版本: {self.diagnostics['blender']['version']}")
        print(f"平台: {self.diagnostics['blender']['platform']}")
        print(f"内存: {self.diagnostics['memory']['total_memory_mb']:.2f} MB")
        print(f"对象数: {self.diagnostics['objects']['total_objects']}")


def run_system_check():
    """运行系统检查"""
    diagnostics = SystemDiagnostics()
    diagnostics.run_full_diagnostics()
    diagnostics.print_diagnostics()
    return diagnostics
```


---
## bpy_ops_preferences

# bpy.ops.preferences 模块

用户偏好操作模块，用于管理Blender的全局用户设置。

## 概述

bpy.ops.preferences 模块包含用于访问和修改Blender用户偏好设置的运算符。这些设置控制界面、输入、文件路径和系统级选项。

## 常用操作

### 界面设置

```python
import bpy

# 打开用户偏好
bpy.ops.screen.userpref_show()

# 切换主题
bpy.ops.preferences.theme_install(filepath='//theme.xml')

# 选择主题
bpy.ops.preferences.active_theme_update()

# 设置界面缩放
bpy.ops.preferences.addons_enable(module='add-on_name')
bpy.ops.preferences.addons_disable(module='add-on_name')
```

### 插件管理

```python
# 启用插件
bpy.ops.preferences.addon_enable(module='module_name')

# 禁用插件
bpy.ops.preferences.addon_disable(module='module_name')

# 安装插件
bpy.ops.preferences.addon_install(filepath='//addon.py')

# 卸载插件
bpy.ops.preferences.addon_remove(module='module_name')

# 更新插件
bpy.ops.preferences.addon_refresh()

# 显示插件信息
bpy.ops.preferences.addon_expand(module='module_name')

# 折叠插件信息
bpy.ops.preferences.addon_collapse(module='module_name')
```

### 快捷键设置

```python
# 分配快捷键
bpy.ops.preferences.keyconfig_assign(filepath='//keymap.xml')

# 重置快捷键
bpy.ops.preferences.keyconfig_presets_edit(idname='Blender')

# 导出快捷键
bpy.ops.preferences.keyconfig_export(filepath='//keymap.xml')

# 添加快捷键
bpy.ops.preferences.keyitem_add()

# 删除快捷键
bpy.ops.preferences.keyitem_remove()

# 重命名快捷键
bpy.ops.preferences.keyitem_rename()
```

### 文件路径

```python
# 设置文件路径
bpy.ops.preferences.app_install_build(filepath='//blender.exe')

# 配置搜索路径
bpy.ops.preferences.file_paths(
    font_directory='//fonts/',
    texture_directory='//textures/',
    render_output_directory='//renders/',
    sound_directory='//sounds/',
    temporary_directory='//temp/'
)

# 设置渲染输出
bpy.ops.preferences.render_output(
    filepath='//renders/frame_'
)

# 设置资源路径
bpy.ops.preferences.asset_libraries(
    library='//assets/'
)
```

### 系统设置

```python
# 设置光标
bpy.ops.preferences.cursor_draw_add()
bpy.ops.preferences.cursor_draw_remove()

# 设置音频设备
bpy.ops.preferences.addon_disable(module='module_name')

# OpenCL设置
bpy.ops.preferences.open_last_problem()

# 撤销设置
bpy.ops.preferences.undo_presets_edit()
```

### 主题设置

```python
# 安装主题
bpy.ops.preferences.theme_install(filepath='//theme.xml')

# 重置主题
bpy.ops.preferences.theme_reset()

# 导出主题
bpy.ops.preferences.theme_export(filepath='//theme.xml')

# 选择颜色集
bpy.ops.preferences.active_theme_update()
```

### 编辑器设置

```python

```

### 快捷键导入导出

```python
# 导入快捷键配置
bpy.ops.preferences.keyconfig_assign(filepath='//keymap.xml')

# 导出快捷键配置
bpy.ops.preferences.keyconfig_export(filepath='//keymap.xml')

# 重置默认快捷键
bpy.ops.preferences.keyconfig_presets_edit(idname='Blender')
```

## 实际应用示例

### 安装和启用插件

```python
def install_and_enable_addon(filepath, module_name):
    """安装并启用插件"""
    # 安装插件
    bpy.ops.preferences.addon_install(filepath=filepath)
    
    # 启用插件
    bpy.ops.preferences.addon_enable(module=module_name)
    
    # 刷新
    bpy.ops.preferences.addon_refresh()
    
    return True
```

### 批量启用插件

```python
def batch_enable_addons(addons_list):
    """批量启用插件"""
    for module in addons_list:
        try:
            bpy.ops.preferences.addon_enable(module=module)
            print(f"已启用: {module}")
        except Exception as e:
            print(f"无法启用 {module}: {e}")
```

### 导出自定义快捷键

```python
def export_custom_keymap(output_path):
    """导出当前快捷键配置"""
    bpy.ops.preferences.keyconfig_export(filepath=output_path)
    print(f"快捷键已导出到: {output_path}")
```

### 配置项目文件路径

```python
def setup_project_paths(project_path):
    """配置项目文件路径"""
    bpy.ops.preferences.file_paths(
        font_directory=f'{project_path}/assets/fonts/',
        texture_directory=f'{project_path}/assets/textures/',
        render_output_directory=f'{project_path}/renders/',
        sound_directory=f'{project_path}/assets/audio/',
        temporary_directory=f'{project_path}/temp/'
    )
```

### 备份用户设置

```python
def backup_user_preferences(output_path):
    """备份用户偏好设置"""
    # 导出快捷键
    bpy.ops.preferences.keyconfig_export(
        filepath=f'{output_path}/keymap.xml'
    )
    
    # 复制用户配置目录
    import shutil
    import os
    
    prefs_dir = os.path.expanduser('~/.config/blender')
    backup_dir = f'{output_path}/blender_prefs'
    
    shutil.copytree(prefs_dir, backup_dir)
    
    return backup_dir
```

### 恢复用户设置

```python
def restore_user_preferences(backup_path):
    """恢复用户偏好设置"""
    # 导入快捷键
    bpy.ops.preferences.keyconfig_assign(
        filepath=f'{backup_path}/keymap.xml'
    )
    
    # 复制用户配置
    import shutil
    import os
    
    prefs_dir = os.path.expanduser('~/.config/blender')
    backup_dir = f'{backup_path}/blender_prefs'
    
    if os.path.exists(backup_dir):
        shutil.copytree(backup_dir, prefs_dir, dirs_exist_ok=True)
```

### 创建团队插件配置

```python
def create_team_addon_config(addons_list, config_file):
    """创建团队插件配置"""
    import json
    
    config = {
        'required_addons': addons_list,
        'version': '1.0',
        'date': '2024-01-01'
    }
    
    with open(config_file, 'w') as f:
        json.dump(config, f, indent=2)
    
    return config_file
```

### 应用团队插件配置

```python
def apply_team_addon_config(config_file):
    """应用团队插件配置"""
    import json
    
    with open(config_file, 'r') as f:
        config = json.load(f)
    
    # 启用所有必需插件
    for module in config['required_addons']:
        try:
            bpy.ops.preferences.addon_enable(module=module)
        except Exception as e:
            print(f"插件 {module} 未找到或无法启用")
```

## 使用场景

- **插件管理**：安装和管理插件
- **快捷键定制**：个性化快捷键配置
- **团队协作**：统一团队设置
- **环境配置**：设置项目文件路径
- **主题定制**：应用自定义主题
- **设置备份**：保存和恢复设置

## 注意事项

- 修改设置后可能需要重启
- 插件可能与其他功能冲突
- 快捷键可能覆盖默认设置
- 主题可能不兼容所有版本
- 路径使用绝对路径更可靠
- 团队设置需要版本控制


---
## bpy_ops_console

# bpy.ops.console - Console Operations Module

控制台操作模块，用于控制Blender Python控制台。

## 概述

`bpy.ops.console` 模块提供了Blender Python控制台（Text Editor中的Python Console）的操作功能，包括代码执行、自动补全、历史记录管理、文本编辑等。该模块主要用于交互式编程和调试。

## 核心功能

### 代码执行

```python
import bpy

# 执行代码
bpy.ops.console.execute(interactive=False)
```

### 自动补全

```python
# 自动补全
bpy.ops.console.autocomplete()

# 缩进或自动补全
bpy.ops.console.indent_or_autocomplete()
```

### 文本编辑

```python
# 插入文本
bpy.ops.console.insert(text='')

# 删除
bpy.ops.console.delete(type='NEXT_CHARACTER')

# 移动光标
bpy.ops.console.move(type='LINE_BEGIN', select=False)

# 缩进
bpy.ops.console.indent()

# 取消缩进
bpy.ops.console.unindent()

# 清除行
bpy.ops.console.clear_line()

# 清除
bpy.ops.console.clear(scrollback=True, history=False)
```

### 选择操作

```python
# 全选
bpy.ops.console.select_all()

# 选择设置
bpy.ops.console.select_set()

# 选择单词
bpy.ops.console.select_word()
```

### 剪贴板操作

```python
# 复制
bpy.ops.console.copy(delete=False)

# 复制为脚本
bpy.ops.console.copy_as_script()

# 粘贴
bpy.ops.console.paste(selection=False)
```

### 历史记录

```python
# 添加历史
bpy.ops.console.history_append(text='', current_character=0, remove_duplicates=False)

# 历史循环
bpy.ops.console.history_cycle(reverse=False)
```

### 输出控制

```python
# 添加回滚内容
bpy.ops.console.scrollback_append(text='', type='OUTPUT')
```

### 语言设置

```python
# 设置语言
bpy.ops.console.language(language='')
```

### 其他操作

```python
# 显示横幅
bpy.ops.console.banner()
```

## 实用函数

```python
# 在控制台执行代码并获取输出
def execute_console_code(code):
    # 插入代码
    bpy.ops.console.insert(text=code)
    # 执行
    bpy.ops.console.execute()
    # 获取输出
    # 注意：需要从控制台区域获取输出内容

# 清空控制台
def clear_console(keep_history=True):
    bpy.ops.console.clear(scrollback=True, history=not keep_history)

# 添加自定义输出
def console_output(text, output_type='OUTPUT'):
    bpy.ops.console.scrollback_append(text=text, type=output_type)

# 快速测试代码片段
def test_code_snippet(code):
    # 清理当前行
    bpy.ops.console.clear_line()
    # 插入代码
    bpy.ops.console.insert(text=code)
    # 执行
    bpy.ops.console.execute()
```

## 常见问题排查

**问题：自动补全不工作**

- 检查控制台是否处于编辑模式
- 确认Python环境正常
- 尝试手动调用`autocomplete()`

**问题：代码执行无响应**

- 检查语法错误
- 确认代码完整
- 尝试分步执行

**问题：历史记录丢失**

- 检查是否调用了`clear(history=True)`
- 确认Blender未异常退出
- 尝试使用`history_append()`保存重要命令

**问题：输出内容混乱**

- 使用`clear()`清理控制台
- 检查是否有多个输出语句
- 尝试重定向输出到文件

**问题：控制台卡顿**

- 减少输出内容
- 清理历史记录
- 避免大量循环输出


---
## bpy_ops_script

# bpy.ops.script - Python Script Operations Module

Blender Python 脚本操作符，用于执行和管理 Python 脚本。

## Overview

`bpy.ops.script` 模块包含脚本执行和管理的操作命令，支持 Python 脚本的运行、文本操作和脚本加载。这个模块主要用于脚本自动化和扩展 Blender 功能。

## 核心功能

```python
import bpy

# 执行 Python 脚本
bpy.ops.script.python_file_run(filepath="C:/scripts/myscript.py")

# 新建脚本
bpy.ops.script.new()

# 重新加载模块
bpy.ops.script.reload()
```

## 实际应用示例

### 脚本管理器

```python
import bpy
import sys
import os

class ScriptManager:
    """脚本管理器"""
    
    def __init__(self):
        self.script_dir = os.path.join(bpy.app.tempdir, "scripts")
        os.makedirs(self.script_dir, exist_ok=True)
    
    def execute_file(self, filepath):
        """执行脚本文件"""
        if os.path.exists(filepath):
            bpy.ops.script.python_file_run(filepath=filepath)
            return True
        return False
    
    def execute_code(self, code):
        """执行代码"""
        try:
            exec(code, globals())
            return True
        except Exception as e:
            print(f"执行错误: {e}")
            return False
    
    def create_script(self, name, code):
        """创建脚本"""
        filepath = os.path.join(self.script_dir, f"{name}.py")
        
        with open(filepath, 'w', encoding='utf-8') as f:
            f.write(code)
        
        return filepath
    
    def load_script(self, filepath):
        """加载脚本"""
        with open(filepath, 'r', encoding='utf-8') as f:
            return f.read()
    
    def save_script(self, name, code):
        """保存脚本"""
        filepath = os.path.join(self.script_dir, f"{name}.py")
        return self.create_script(name, code)
    
    def list_scripts(self):
        """列出脚本"""
        if os.path.exists(self.script_dir):
            return [f[:-3] for f in os.listdir(self.script_dir) 
                   if f.endswith('.py')]
        return []
    
    def delete_script(self, name):
        """删除脚本"""
        filepath = os.path.join(self.script_dir, f"{name}.py")
        if os.path.exists(filepath):
            os.remove(filepath)
            return True
        return False
    
    def duplicate_script(self, name, new_name):
        """复制脚本"""
        code = self.load_script(os.path.join(self.script_dir, f"{name}.py"))
        return self.create_script(new_name, code)
    
    def rename_script(self, name, new_name):
        """重命名脚本"""
        code = self.load_script(os.path.join(self.script_dir, f"{name}.py"))
        self.delete_script(name)
        return self.create_script(new_name, code)
    
    def export_script(self, name, output_path):
        """导出脚本"""
        code = self.load_script(os.path.join(self.script_dir, f"{name}.py"))
        
        with open(output_path, 'w', encoding='utf-8') as f:
            f.write(code)
        
        return output_path
    
    def import_script(self, filepath):
        """导入脚本"""
        code = self.load_script(filepath)
        name = os.path.splitext(os.path.basename(filepath))[0]
        return self.create_script(name, code)
    
    def reload_all_scripts(self):
        """重新加载所有脚本"""
        bpy.ops.script.reload()
        
        # 重新导入用户模块
        for module_name in list(sys.modules.keys()):
            if module_name.startswith('user_'):
                del sys.modules[module_name]


def setup_script_manager():
    """设置脚本管理器"""
    manager = ScriptManager()
    
    print("脚本管理器已设置")
    print(f"脚本目录: {manager.script_dir}")
    print(f"脚本列表: {manager.list_scripts()}")
    
    return manager
```

### 自动化脚本库

```python
import bpy
import os

class AutomationScriptLibrary:
    """自动化脚本库"""
    
    def __init__(self):
        self.scripts = {}
        self.categories = {}
    
    def register_script(self, name, category, code):
        """注册脚本"""
        self.scripts[name] = code
        if category not in self.categories:
            self.categories[category] = []
        self.categories[category].append(name)
    
    def get_script(self, name):
        """获取脚本"""
        return self.scripts.get(name)
    
    def get_scripts_by_category(self, category):
        """按类别获取脚本"""
        return self.categories.get(category, [])
    
    def get_all_categories(self):
        """获取所有类别"""
        return list(self.categories.keys())
    
    def execute_script(self, name):
        """执行脚本"""
        code = self.get_script(name)
        if code:
            return bpy.ops.script.execute_python(code=code)
    
    def execute_script_with_args(self, name, **kwargs):
        """带参数执行脚本"""
        code = self.get_script(name)
        if code:
            # 格式化代码
            formatted_code = code.format(**kwargs)
            return bpy.ops.script.execute_python(code=formatted_code)
    
    def create_category_script(self, category):
        """创建类别脚本"""
        scripts = self.get_scripts_by_category(category)
        combined_code = "\n\n".join(
            f"# {name}\n{self.scripts[name]}" 
            for name in scripts
        )
        
        self.scripts[f"{category}_all"] = combined_code
        return combined_code
    
    def export_category(self, category, output_dir):
        """导出类别"""
        scripts = self.get_scripts_by_category(category)
        os.makedirs(output_dir, exist_ok=True)
        
        for name in scripts:
            code = self.get_script(name)
            filepath = os.path.join(output_dir, f"{name}.py")
            
            with open(filepath, 'w', encoding='utf-8') as f:
                f.write(code)
        
        return len(scripts)
    
    def import_directory(self, directory):
        """导入目录"""
        count = 0
        
        for filename in os.listdir(directory):
            if filename.endswith('.py'):
                filepath = os.path.join(directory, filename)
                name = os.path.splitext(filename)[0]
                
                with open(filepath, 'r', encoding='utf-8') as f:
                    code = f.read()
                
                self.register_script(name, "Imported", code)
                count += 1
        
        return count
    
    def create_script_documentation(self, name):
        """创建脚本文档"""
        code = self.get_script(name)
        if not code:
            return None
        
        # 解析文档字符串
        lines = code.split('\n')
        doc = []
        in_doc = False
        
        for line in lines:
            if line.startswith('"""') or line.startswith("'''"):
                in_doc = not in_doc
            elif in_doc:
                doc.append(line)
        
        return '\n'.join(doc)
    
    def list_scripts_with_docs(self):
        """列出带文档的脚本"""
        result = []
        for name in self.scripts:
            doc = self.create_script_documentation(name)
            result.append({
                'name': name,
                'documentation': doc or "无文档"
            })
        return result


def setup_automation_library():
    """设置自动化脚本库"""
    library = AutomationScriptLibrary()
    
    # 注册常用脚本
    library.register_script(
        "setup_scene",
        "Scene",
        '''
import bpy

def setup_basic_scene():
    bpy.ops.object.light_add(type='SUN', location=(5, 5, 10))
    bpy.ops.object.camera_add(location=(0, -10, 5))
    return True
'''
    )
    
    library.register_script(
        "cleanup_scene",
        "Scene",
        '''
import bpy

def remove_unused_materials():
    for mat in list(bpy.data.materials):
        if mat.users == 0:
            bpy.data.materials.remove(mat)
    return True
'''
    )
    
    library.register_script(
        "export_glb",
        "Export",
        '''
import bpy

def export_selected(filepath):
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        use_selection=True
    )
    return filepath
'''
    )
    
    print("自动化脚本库已设置")
    print(f"类别: {library.get_all_categories()}")
    
    return library
```

### 批量处理工具

```python
import bpy
import os

class BatchProcessingTool:
    """批量处理工具"""
    
    def __init__(self):
        self.queue = []
        self.results = []
    
    def add_task(self, task_type, **kwargs):
        """添加任务"""
        self.queue.append({
            'type': task_type,
            'params': kwargs
        })
    
    def clear_queue(self):
        """清空队列"""
        self.queue = []
    
    def process_queue(self, progress_callback=None):
        """处理队列"""
        self.results = []
        
        for i, task in enumerate(self.queue):
            result = self._execute_task(task)
            self.results.append(result)
            
            if progress_callback:
                progress_callback(i + 1, len(self.queue))
        
        return self.results
    
    def _execute_task(self, task):
        """执行任务"""
        task_type = task['type']
        params = task['params']
        
        if task_type == 'export':
            return self._export_task(params)
        elif task_type == 'import':
            return self._import_task(params)
        elif task_type == 'render':
            return self._render_task(params)
        elif task_type == 'apply_modifiers':
            return self._apply_modifiers_task(params)
        
        return {'success': False, 'error': '未知任务类型'}
    
    def _export_task(self, params):
        """导出任务"""
        obj_name = params.get('object')
        filepath = params.get('filepath')
        format = params.get('format', 'GLTF')
        
        if format == 'GLTF':
            bpy.ops.wm.gltf_export(
                filepath=filepath,
                selected_objects=[bpy.data.objects.get(obj_name)] if obj_name else None
            )
        
        return {'success': True, 'file': filepath}
    
    def _import_task(self, params):
        """导入任务"""
        filepath = params.get('filepath')
        
        if filepath.endswith('.glb') or filepath.endswith('.gltf'):
            bpy.ops.wm.gltf_import(filepath=filepath)
        elif filepath.endswith('.fbx'):
            bpy.ops.import_scene.fbx(filepath=filepath)
        elif filepath.endswith('.obj'):
            bpy.ops.import_scene.obj(filepath=filepath)
        
        return {'success': True, 'imported': filepath}
    
    def _render_task(self, params):
        """渲染任务"""
        filepath = params.get('filepath')
        engine = params.get('engine', 'CYCLES')
        
        scene = bpy.context.scene
        old_engine = scene.render.engine
        scene.render.engine = engine
        scene.render.filepath = filepath
        
        bpy.ops.render.render(write_still=True)
        
        scene.render.engine = old_engine
        
        return {'success': True, 'rendered': filepath}
    
    def _apply_modifiers_task(self, params):
        """应用修改器任务"""
        obj_name = params.get('object')
        obj = bpy.data.objects.get(obj_name)
        
        if obj:
            for modifier in obj.modifiers[:]:
                bpy.ops.object.modifier_apply(modifier=modifier.name)
            return {'success': True, 'object': obj_name}
        
        return {'success': False, 'error': '对象不存在'}
    
    def add_export_tasks(self, objects, output_dir, format='GLB'):
        """添加导出任务"""
        for obj in objects:
            if obj.type in ['MESH', 'CURVE', 'ARMATURE']:
                filepath = os.path.join(output_dir, f"{obj.name}.{format.lower()}")
                self.add_task('export', object=obj.name, filepath=filepath, format=format)
    
    def add_import_task(self, filepath):
        """添加导入任务"""
        self.add_task('import', filepath=filepath)
    
    def add_render_tasks(self, output_dir, format='PNG'):
        """添加渲染任务"""
        for frame in range(
            bpy.context.scene.frame_start,
            bpy.context.scene.frame_end + 1
        ):
            filepath = os.path.join(
                output_dir, 
                f"frame_{frame:04d}.{format.lower()}"
            )
            self.add_task('render', filepath=filepath)
    
    def get_task_summary(self):
        """获取任务摘要"""
        summary = {}
        for task in self.queue:
            task_type = task['type']
            summary[task_type] = summary.get(task_type, 0) + 1
        return summary
    
    def export_queue(self, filepath):
        """导出队列"""
        import json
        
        with open(filepath, 'w') as f:
            json.dump(self.queue, f, indent=2)
        
        return filepath
    
    def import_queue(self, filepath):
        """导入队列"""
        import json
        
        with open(filepath, 'r') as f:
            self.queue = json.load(f)
        
        return len(self.queue)


def setup_batch_processor():
    """设置批量处理器"""
    processor = BatchProcessingTool()
    
    print("批量处理工具已设置")
    print(f"当前队列: {processor.get_task_summary()}")
    
    return processor
```

### Python 环境工具

```python
import bpy
import sys
import subprocess

class PythonEnvironmentTool:
    """Python 环境工具"""
    
    def __init__(self):
        self.packages = {}
    
    def get_python_version(self):
        """获取 Python 版本"""
        return sys.version
    
    def get_python_path(self):
        """获取 Python 路径"""
        return sys.executable
    
    def get_installed_packages(self):
        """获取已安装包"""
        return list(sys.modules.keys())
    
    def check_package(self, package_name):
        """检查包"""
        return package_name in sys.modules
    
    def import_package(self, package_name):
        """导入包"""
        try:
            module = __import__(package_name)
            return module
        except ImportError:
            return None
    
    def run_pip_command(self, command):
        """运行 pip 命令"""
        python_path = sys.executable
        
        try:
            result = subprocess.run(
                [python_path, '-m', 'pip'] + command.split(),
                capture_output=True,
                text=True
            )
            return result.stdout + result.stderr
        except Exception as e:
            return str(e)
    
    def install_package(self, package_name):
        """安装包"""
        return self.run_pip_command(f"install {package_name}")
    
    def uninstall_package(self, package_name):
        """卸载包"""
        return self.run_pip_command(f"uninstall -y {package_name}")
    
    def list_updatable_packages(self):
        """列出可更新包"""
        return self.run_pip_command("list --outdated")
    
    def upgrade_package(self, package_name):
        """更新包"""
        return self.run_pip_command(f"install --upgrade {package_name}")
    
    def create_virtual_environment(self, path):
        """创建虚拟环境"""
        return self.run_pip_command(f"-m venv {path}")
    
    def execute_in_venv(self, venv_path, command):
        """在虚拟环境中执行"""
        if sys.platform == 'win32':
            python_path = os.path.join(venv_path, 'Scripts', 'python.exe')
        else:
            python_path = os.path.join(venv_path, 'bin', 'python')
        
        try:
            result = subprocess.run(
                [python_path] + command.split(),
                capture_output=True,
                text=True
            )
            return result.stdout + result.stderr
        except Exception as e:
            return str(e)
    
    def get_blender_modules(self):
        """获取 Blender 模块"""
        modules = []
        for name in dir(bpy):
            if not name.startswith('_'):
                modules.append(name)
        return modules
    
    def get_bpy_modules(self):
        """获取 bpy 子模块"""
        modules = []
        for attr in dir(bpy):
            if isinstance(getattr(bpy, attr), type(bpy)):
                modules.append(attr)
        return modules
    
    def export_environment_info(self, filepath):
        """导出环境信息"""
        info = {
            'python_version': self.get_python_version(),
            'python_path': self.get_python_path(),
            'blender_version': bpy.app.version_string,
            'installed_packages': self.get_installed_packages()[:50]
        }
        
        import json
        
        with open(filepath, 'w') as f:
            json.dump(info, f, indent=2)
        
        return filepath


def setup_python_environment():
    """设置 Python 环境"""
    tool = PythonEnvironmentTool()
    
    print("Python 环境工具已设置")
    print(f"Python 版本: {tool.get_python_version()[:50]}")
    print(f"Blender 版本: {bpy.app.version_string}")
    
    return tool
```


---
## bpy_ops_spreadsheet

# bpy.ops.spreadsheet - 电子表格操作模块

Blender电子表格操作符，用于在Blender内置电子表格编辑器中管理数据。

## 概述

`bpy.ops.spreadsheet` 模块提供用于操作Blender电子表格编辑器的功能，用于查看和编辑属性集合、几何节点数据等表格化数据。

## 核心功能

### 表格选择操作

```python
import bpy

# 全选
bpy.ops.spreadsheet.select_all(action='SELECT')

# 全不选
bpy.ops.spreadsheet.select_all(action='DESELECT')

# 反选
bpy.ops.spreadsheet.select_all(action='INVERT')

# 选择行
bpy.ops.spreadsheet.select_row(row=0)

# 选择列
bpy.ops.spreadsheet.select_column(column=0)

# 选择单元格
bpy.ops.spreadsheet.select_cell(row=0, column=0)

# 选择范围
bpy.ops.spreadsheet.select_range(row_start=0, row_end=10, column_start=0, column_end=5)

# 选择行范围
bpy.ops.spreadsheet.select_row_range(row_start=0, row_end=10)

# 选择列范围
bpy.ops.spreadsheet.select_column_range(column_start=0, column_end=5)

# 选择整列
bpy.ops.spreadsheet.select_column_all()

# 选择整行
bpy.ops.spreadsheet.select_row_all()
```

### 表格编辑操作

```python
# 复制选中
bpy.ops.spreadsheet.copy()

# 粘贴
bpy.ops.spreadsheet.paste()

# 粘贴值
bpy.ops.spreadsheet.paste_values()

# 清除内容
bpy.ops.spreadsheet.clear_contents()

# 清除格式
bpy.ops.spreadsheet.clear_formatting()

# 删除行
bpy.ops.spreadsheet.delete_rows()

# 删除列
bpy.ops.spreadsheet.delete_columns()

# 插入行
bpy.ops.spreadsheet.insert_rows(count=1)

# 插入列
bpy.ops.spreadsheet.insert_columns(count=1)

# 重命名列
bpy.ops.spreadsheet.rename_column(name='New Name')

# 重命名表
bpy.ops.spreadsheet.rename_sheet(name='New Sheet')
```

### 单元格操作

```python
# 设置单元格值
bpy.ops.spreadsheet.set_value(value='text', row=0, column=0)

# 获取单元格值
bpy.ops.spreadsheet.get_value(row=0, column=0)

# 设置公式
bpy.ops.spreadsheet.set_formula(formula='=SUM(A1:A10)', row=0, column=0)

# 计算公式
bpy.ops.spreadsheet.calculate_formulas()

# 设置单元格格式
bpy.ops.spreadsheet.set_cell_format(format='NUMBER', row=0, column=0)

# 合并单元格
bpy.ops.spreadsheet.merge_cells(row_start=0, row_end=2, column_start=0, column_end=2)

# 取消合并
bpy.ops.spreadsheet.unmerge_cells(row=0, column=0)
```

### 排序和筛选

```python
# 排序
bpy.ops.spreadsheet.sort(column=0, order='ASCENDING')

# 降序排序
bpy.ops.spreadsheet.sort(column=0, order='DESCENDING')

# 筛选
bpy.ops.spreadsheet.filter(column=0, criteria='EQUALS', value='text')

# 清除筛选
bpy.ops.spreadsheet.clear_filter()

# 高级筛选
bpy.ops.spreadsheet.advanced_filter(criteria={'column': 0, 'operator': 'GREATER', 'value': 10})

# 按颜色筛选
bpy.ops.spreadsheet.filter_by_color(color='RED')

# 排序筛选
bpy.ops.spreadsheet.sort_filter(column=0, filter_criteria={'column': 1, 'value': 'text'})
```

### 查找和替换

```python
# 查找
bpy.ops.spreadsheet.find(text='search_text')

# 查找下一个
bpy.ops.spreadsheet.find_next()

# 查找上一个
bpy.ops.spreadsheet.find_previous()

# 替换
bpy.ops.spreadsheet.replace(text='old', new='new')

# 替换所有
bpy.ops.spreadsheet.replace_all(text='old', new='new')

# 使用正则表达式查找
bpy.ops.spreadsheet.find_regex(pattern='regex')

# 高亮所有匹配
bpy.ops.spreadsheet.highlight_all(text='search_text')

# 清除高亮
bpy.ops.spreadsheet.clear_highlight()
```

### 格式设置

```python
# 设置数字格式
bpy.ops.spreadsheet.set_number_format(format='0.00', row=0, column=0)

# 设置日期格式
bpy.ops.spreadsheet.set_date_format(format='YYYY-MM-DD', row=0, column=0)

# 设置百分比
bpy.ops.spreadsheet.set_percentage(row=0, column=0)

# 设置货币
bpy.ops.spreadsheet.set_currency(currency='USD', row=0, column=0)

# 设置字体
bpy.ops.spreadsheet.set_font(font='Arial', row=0, column=0)

# 设置字号
bpy.ops.spreadsheet.set_font_size(size=12, row=0, column=0)

# 设置粗体
bpy.ops.spreadsheet.set_bold(row=0, column=0, bold=True)

# 设置斜体
bpy.ops.spreadsheet.set_italic(row=0, column=0, italic=True)

# 设置对齐
bpy.ops.spreadsheet.set_alignment(alignment='CENTER', row=0, column=0)

# 设置背景色
bpy.ops.spreadsheet.set_background_color(color=(1.0, 1.0, 1.0, 1.0), row=0, column=0)

# 设置文字颜色
bpy.ops.spreadsheet.set_text_color(color=(0.0, 0.0, 0.0, 1.0), row=0, column=0)

# 设置边框
bpy.ops.spreadsheet.set_border(style='THIN', row=0, column=0)

# 应用格式到整列
bpy.ops.spreadsheet.format_column(column=0, format='NUMBER')

# 应用格式到整行
bpy.ops.spreadsheet.format_row(row=0, format='TEXT')
```

### 视图操作

```python
# 调整列宽
bpy.ops.spreadsheet.set_column_width(width=100, column=0)

# 调整行高
bpy.ops.spreadsheet.set_row_height(height=20, row=0)

# 自动调整列宽
bpy.ops.spreadsheet.auto_fit_column(column=0)

# 自动调整所有列
bpy.ops.spreadsheet.auto_fit_all_columns()

# 冻结窗格
bpy.ops.spreadsheet.freeze_panes(row=5, column=2)

# 取消冻结
bpy.ops.spreadsheet.unfreeze_panes()

# 隐藏列
bpy.ops.spreadsheet.hide_column(column=0)

# 显示列
bpy.ops.spreadsheet.show_column(column=0)

# 隐藏行
bpy.ops.spreadsheet.hide_row(row=0)

# 显示行
bpy.ops.spreadsheet.show_row(row=0)

# 滚动到单元格
bpy.ops.spreadsheet.scroll_to_cell(row=0, column=0)

# 切换行号显示
bpy.ops.spreadsheet.toggle_row_numbers()

# 切换列标显示
bpy.ops.spreadsheet.toggle_column_letters()
```

### 条件格式

```python
# 添加条件格式
bpy.ops.spreadsheet.add_conditional_format(
    range='A1:C10',
    criteria={'type': 'GREATER', 'value': 100},
    format={'background': (1.0, 0.0, 0.0, 1.0)}
)

# 清除条件格式
bpy.ops.spreadsheet.clear_conditional_format(range='A1:C10')

# 管理条件格式规则
bpy.ops.spreadsheet.manage_conditional_formats()

# 数据条
bpy.ops.spreadsheet.add_data_bars(range='A1:A10')

# 色阶
bpy.ops.spreadsheet.add_color_scale(range='A1:A10')

# 图标集
bpy.ops.spreadsheet.add_icon_set(range='A1:A10', icons='TRAFFIC_LIGHTS')
```

### 数据操作

```python
# 去除重复项
bpy.ops.spreadsheet.remove_duplicates(range='A1:C10')

# 删除空行
bpy.ops.spreadsheet.remove_empty_rows()

# 删除空列
bpy.ops.spreadsheet.remove_empty_columns()

# 填充序列
bpy.ops.spreadsheet.fill_series(start_value=1, step=1)

# 填充公式
bpy.ops.spreadsheet.fill_formula()

# 填充向下
bpy.ops.spreadsheet.fill_down()

# 填充向右
bpy.ops.spreadsheet.fill_right()

# 转置
bpy.ops.spreadsheet.transpose(range='A1:C10')

# 删除重复值
bpy.ops.spreadsheet.remove_duplicate_values()
```

### 导入导出

```python
# 导出到CSV
bpy.ops.spreadsheet.export_csv(filepath='//data.csv')

# 导出到TSV
bpy.ops.spreadsheet.export_tsv(filepath='//data.tsv')

# 导出到JSON
bpy.ops.spreadsheet.export_json(filepath='//data.json')

# 导出到Excel
bpy.ops.spreadsheet.export_excel(filepath='//data.xlsx')

# 导入CSV
bpy.ops.spreadsheet.import_csv(filepath='//data.csv')

# 导入TSV
bpy.ops.spreadsheet.import_tsv(filepath='//data.tsv')

# 导入JSON
bpy.ops.spreadsheet.import_json(filepath='//data.json')
```

### 公式和函数

```python
# 插入求和公式
bpy.ops.spreadsheet.insert_sum(row=0, column=0, range='A1:A10')

# 插入平均值公式
bpy.ops.spreadsheet.insert_average(row=0, column=0, range='A1:A10')

# 插入计数公式
bpy.ops.spreadsheet.insert_count(row=0, column=0, range='A1:A10')

# 插入最大值公式
bpy.ops.spreadsheet.insert_max(row=0, column=0, range='A1:A10')

# 插入最小值公式
bpy.ops.spreadsheet.insert_min(row=0, column=0, range='A1:A10')

# 插入IF公式
bpy.ops.spreadsheet.insert_if(row=0, column=0, condition='A1>10', true='Yes', false='No')

# 显示所有公式
bpy.ops.spreadsheet.show_formulas()

# 隐藏公式
bpy.ops.spreadsheet.hide_formulas()

# 计算所有
bpy.ops.spreadsheet.calculate_all()

# 重新计算
bpy.ops.spreadsheet.recalculate()
```

## 实用函数

```python
def get_sheet_data(sheet_name):
    """获取工作表数据"""
    sheet = bpy.data.spreadsheets.get(sheet_name)
    if sheet:
        return sheet.to_pydata()
    return None

def get_selected_cells():
    """获取选中的单元格"""
    context = bpy.context
    return context.space_data.selected_cells

def get_cell_value(row, column):
    """获取单元格值"""
    sheet = bpy.context.space_data.spreadsheet
    return sheet.get_value(row, column)

def set_cell_value(row, column, value):
    """设置单元格值"""
    sheet = bpy.context.space_data.spreadsheet
    sheet.set_value(row, column, value)

def get_row_count():
    """获取行数"""
    return len(bpy.context.space_data.spreadsheet.rows)

def get_column_count():
    """获取列数"""
    return len(bpy.context.space_data.spreadsheet.columns)

def export_to_dataframe(sheet_name):
    """导出为pandas DataFrame"""
    try:
        import pandas as pd
        sheet = bpy.data.spreadsheets.get(sheet_name)
        if sheet:
            return pd.DataFrame(sheet.to_pydata())
    except ImportError:
        print("pandas未安装")
    return None

def import_from_dataframe(df, sheet_name='Imported'):
    """从pandas DataFrame导入"""
    sheet = bpy.data.spreadsheets.new(name=sheet_name)
    sheet.from_pydata(df.values.tolist())
    return sheet

def filter_by_value(column, value):
    """按值筛选"""
    bpy.ops.spreadsheet.clear_filter()
    bpy.ops.spreadsheet.filter(column=column, criteria='EQUALS', value=value)

def get_column_stats(column):
    """获取列统计信息"""
    data = [get_cell_value(row, column) for row in range(get_row_count())]
    numeric_data = [d for d in data if isinstance(d, (int, float))]
    if numeric_data:
        return {
            'sum': sum(numeric_data),
            'avg': sum(numeric_data) / len(numeric_data),
            'min': min(numeric_data),
            'max': max(numeric_data),
            'count': len(numeric_data)
        }
    return None
```

## 常见问题排查

### 数据不显示
```python
# 检查当前工作表
print(f"当前工作表: {bpy.context.space_data.spreadsheet}")

# 检查行数列数
print(f"行数: {get_row_count()}")
print(f"列数: {get_column_count()}")

# 检查数据类型
sheet = bpy.context.space_data.spreadsheet
print(f"数据类型: {type(sheet)}")

# 刷新数据
bpy.ops.spreadsheet.refresh()
```

### 选择操作问题
```python
# 清除选择
bpy.ops.spreadsheet.select_all(action='DESELECT')

# 检查选择范围
print(f"选中行: {bpy.context.space_data.selected_rows}")
print(f"选中列: {bpy.context.space_data.selected_columns}")

# 重置选择
bpy.ops.spreadsheet.select_cell(row=0, column=0)
```

### 导入导出问题
```python
# 检查文件路径
import os
print(f"文件存在: {os.path.exists('//data.csv')}")

# 检查权限
print("检查文件读写权限")

# 尝试不同格式
bpy.ops.spreadsheet.export_csv(filepath='//test.csv')
```


---
## bpy_ops_extensions

# bpy.ops.extensions - 扩展操作模块

Blender扩展操作符，用于管理Blender扩展和插件系统。

## 概述

`bpy.ops.extensions` 模块提供用于安装、更新、管理和删除Blender扩展的操作功能。扩展系统是Blender 4.0+版本中新的插件管理机制。

## 核心功能

### 扩展仓库操作

```python
import bpy

# 打开扩展仓库
bpy.ops.extensions.open_ext_store()

# 刷新扩展仓库列表
bpy.ops.extensions.refresh()

# 更新扩展仓库索引
bpy.ops.extensions.update_repo_index()

# 搜索扩展
bpy.ops.extensions.search(query='material')

# 按分类浏览扩展
bpy.ops.extensions.browse(category='materials')

# 显示扩展详情
bpy.ops.extensions.view_details(extension_id='pkg_name')

# 返回扩展列表
bpy.ops.extensions.back_to_list()
```

### 扩展安装

```python
# 从仓库安装扩展
bpy.ops.extensions.install(extension_id='pkg_name')

# 从文件安装扩展
bpy.ops.extensions.install_from_file(filepath='//extension.zip')

# 从URL安装扩展
bpy.ops.extensions.install_from_url(url='https://example.com/extension.zip')

# 批量安装扩展
bpy.ops.extensions.batch_install(extension_ids=['pkg1', 'pkg2', 'pkg3'])

# 安装扩展到指定路径
bpy.ops.extensions.install(filepath='//extension.zip', target='//addons/')

# 检查依赖并安装
bpy.ops.extensions.install_with_deps(extension_id='pkg_name')
```

### 扩展更新

```python
# 检查更新
bpy.ops.extensions.check_for_updates()

# 更新单个扩展
bpy.ops.extensions.update(extension_id='pkg_name')

# 更新所有扩展
bpy.ops.extensions.update_all()

# 更新所有扩展到最新版本
bpy.ops.extensions.update_all_to_latest()

# 忽略特定扩展的更新
bpy.ops.extensions.ignore_update(extension_id='pkg_name')

# 取消忽略更新
bpy.ops.extensions.unignore_update(extension_id='pkg_name')

# 设置扩展更新策略
bpy.ops.extensions.set_update_policy(policy='auto')
```

### 扩展管理

```python
# 启用扩展
bpy.ops.extensions.enable(extension_id='pkg_name')

# 禁用扩展
bpy.ops.extensions.disable(extension_id='pkg_name')

# 切换扩展状态
bpy.ops.extensions.toggle(extension_id='pkg_name')

# 重新加载扩展
bpy.ops.extensions.reload(extension_id='pkg_name')

# 重启扩展
bpy.ops.extensions.restart(extension_id='pkg_name')

# 配置扩展
bpy.ops.extensions.configure(extension_id='pkg_name')

# 打开扩展设置面板
bpy.ops.extensions.open_preferences(extension_id='pkg_name')

# 查看扩展信息
bpy.ops.extensions.info(extension_id='pkg_name')

# 查看扩展使用统计
bpy.ops.extensions.usage_stats(extension_id='pkg_name')
```

### 扩展卸载

```python
# 卸载扩展
bpy.ops.extensions.remove(extension_id='pkg_name')

# 卸载并删除配置
bpy.ops.extensions.remove_with_config(extension_id='pkg_name')

# 批量卸载扩展
bpy.ops.extensions.batch_remove(extension_ids=['pkg1', 'pkg2'])

# 清理未使用的依赖
bpy.ops.extensions.cleanup_orphans()

# 清理扩展缓存
bpy.ops.extensions.clear_cache()
```

### 扩展备份

```python
# 备份扩展
bpy.ops.extensions.backup(extension_id='pkg_name', filepath='//backup.zip')

# 批量备份所有扩展
bpy.ops.extensions.backup_all(filepath='//all_extensions.zip')

# 从备份恢复扩展
bpy.ops.extensions.restore_from_backup(filepath='//backup.zip')

# 导入扩展设置
bpy.ops.extensions.import_settings(filepath='//settings.json')

# 导出扩展设置
bpy.ops.extensions.export_settings(extension_id='pkg_name', filepath='//settings.json')
```

### 扩展仓库管理

```python
# 添加自定义仓库
bpy.ops.extensions.repo_add(url='https://example.com/repo.json', name='My Repo')

# 移除自定义仓库
bpy.ops.extensions.repo_remove(repo_id='my_repo')

# 编辑仓库设置
bpy.ops.extensions.repo_edit(repo_id='my_repo', url='https://new_url.com/repo.json')

# 启用仓库
bpy.ops.extensions.repo_enable(repo_id='my_repo')

# 禁用仓库
bpy.ops.extensions.repo_disable(repo_id='my_repo')

# 刷新仓库
bpy.ops.extensions.repo_refresh(repo_id='my_repo')

# 设置仓库优先级
bpy.ops.extensions.repo_set_priority(repo_id='my_repo', priority=10)
```

### 扩展权限管理

```python
# 授予扩展权限
bpy.ops.extensions.grant_permission(extension_id='pkg_name', permission='internet')

# 撤销扩展权限
bpy.ops.extensions.revoke_permission(extension_id='pkg_name', permission='internet')

# 查看扩展权限
bpy.ops.extensions.view_permissions(extension_id='pkg_name')

# 重置所有权限
bpy.ops.extensions.reset_all_permissions(extension_id='pkg_name')
```

## 实用函数

```python
def get_installed_extensions():
    """获取所有已安装的扩展"""
    return list(bpy.context.preferences.extensions)

def get_enabled_extensions():
    """获取所有启用的扩展"""
    return [ext for ext in bpy.context.preferences.extensions if ext.enabled]

def get_extension_by_id(extension_id):
    """根据ID获取扩展"""
    for ext in bpy.context.preferences.extensions:
        if ext.module == extension_id:
            return ext
    return None

def is_extension_enabled(extension_id):
    """检查扩展是否已启用"""
    ext = get_extension_by_id(extension_id)
    return ext.enabled if ext else False

def get_extension_version(extension_id):
    """获取扩展版本"""
    ext = get_extension_by_id(extension_id)
    return ext.version if ext else None

def check_extension_updates():
    """检查扩展更新，返回有更新的扩展列表"""
    updates_available = []
    for ext in bpy.context.preferences.extensions:
        if ext.update_available:
            updates_available.append(ext)
    return updates_available

def install_required_extensions():
    """安装所有标记为需要安装的扩展"""
    for ext in bpy.context.preferences.extensions:
        if ext.pending_install:
            bpy.ops.extensions.install(extension_id=ext.module)

def get_extension_dependencies(extension_id):
    """获取扩展的依赖列表"""
    ext = get_extension_by_id(extension_id)
    return ext.dependencies if ext else []
```

## 常见问题排查

### 安装失败
```python
# 检查权限
bpy.ops.extensions.view_permissions(extension_id='pkg_name')

# 尝试手动安装
bpy.ops.extensions.install_from_file(filepath='//extension.zip')

# 检查日志
import logging
logging.basicConfig(level=logging.DEBUG)

# 验证文件完整性
import os
if os.path.exists('//extension.zip'):
    print("文件存在")
else:
    print("文件不存在")
```

### 扩展冲突
```python
# 列出所有扩展
for ext in bpy.context.preferences.extensions:
    print(f"{ext.module}: {ext.enabled}")

# 检查冲突
def check_conflicts():
    conflicts = []
    enabled = get_enabled_extensions()
    for ext in enabled:
        for dep in ext.dependencies:
            if not is_extension_enabled(dep):
                conflicts.append(f"{ext.module} 需要 {dep}")
    return conflicts

# 禁用冲突扩展
for ext_name in conflict_extensions:
    bpy.ops.extensions.disable(extension_id=ext_name)
```

### 更新问题
```python
# 强制刷新仓库
bpy.ops.extensions.update_repo_index()

# 手动检查更新
bpy.ops.extensions.check_for_updates()

# 回滚更新
for ext in updated_extensions:
    bpy.ops.extensions.install_from_file(filepath=ext.backup_path)
```


---
## bpy_ops_addon

# bpy.ops.addon - 插件操作模块

bpy.ops.addon模块提供了插件（ addon）管理的操作功能，用于安装、启用和禁用Blender插件。

## 概述

Blender的插件系统允许扩展功能。这个模块提供了插件的安装、启用、禁用和管理操作。

## 核心功能

### 插件安装

```python
import bpy

def install_addon(filepath):
    """安装插件"""
    bpy.ops.wm.addon_install(
        filepath=filepath,
        overwrite=True,
        target='DEFAULT'
    )

def enable_addon(module_name):
    """启用插件"""
    bpy.ops.wm.addon_enable(module=module_name)

def disable_addon(module_name):
    """禁用插件"""
    bpy.ops.wm.addon_disable(module=module_name)

def uninstall_addon(module_name):
    """卸载插件"""
    bpy.ops.wm.addon_remove(module=module_name)
```

### 插件管理

```python
def refresh_addons():
    """刷新插件列表"""
    bpy.ops.wm.addon_refresh()

def get_enabled_addons():
    """获取已启用的插件"""
    prefs = bpy.context.preferences.addons
    return [addon.module for addon in prefs]

def get_addon_info(module_name):
    """获取插件信息"""
    import addon_utils
    return addon_utils.module_get_info(module_name)
```

## 实用函数

### 插件搜索

```python
def find_addon_by_name(name):
    """按名称查找插件"""
    import addon_utils
    for addon in addon_utils.addons_fake_modules.values():
        if name.lower() in addon.module.lower():
            return addon
    return None

def list_all_addons():
    """列出所有插件"""
    import addon_utils
    return [(mod, info) for mod, info in 
            addon_utils.module_get_info().items()]
```

### 插件状态

```python
def is_addon_enabled(module_name):
    """检查插件是否启用"""
    return module_name in bpy.context.preferences.addons

def is_addon_installed(module_name):
    """检查插件是否安装"""
    import addon_utils
    return module_name in [m.module for m in 
                          addon_utils.addons_fake_modules.values()]
```

## 常见问题排查

### 安装失败

文件问题：
- 确认文件路径正确
- 检查文件权限
- 验证插件格式有效

### 启用失败

依赖问题：
- 检查Python依赖
- 确认Blender版本兼容
- 查看错误日志


