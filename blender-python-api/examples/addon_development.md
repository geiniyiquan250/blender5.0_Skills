# 插件开发指南

本指南介绍如何开发 Blender 插件。

## 插件基础

### 插件结构

```python
bl_info = {
    "name": "My Addon",
    "author": "Your Name",
    "version": (1, 0, 0),
    "blender": (3, 0, 0),
    "location": "View3D > Sidebar",
    "description": "My addon description",
    "category": "3D View",
}

import bpy

class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "My Operator"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        # 执行操作
        return {'FINISHED'}

class MyPanel(bpy.types.Panel):
    bl_label = "My Panel"
    bl_idname = "VIEW3D_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'My Addon'

    def draw(self, context):
        layout = self.layout
        layout.operator("object.my_operator")

def register():
    bpy.utils.register_class(MyOperator)
    bpy.utils.register_class(MyPanel)

def unregister():
    bpy.utils.unregister_class(MyOperator)
    bpy.utils.unregister_class(MyPanel)

if __name__ == "__main__":
    register()
```

## 操作符 (Operator)

### 基础操作符

```python
class SimpleOperator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "Simple Operator"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        # 执行操作
        print("Operator executed")
        return {'FINISHED'}

def register():
    bpy.utils.register_class(SimpleOperator)

def unregister():
    bpy.utils.unregister_class(SimpleOperator)
```

### 带属性的操作符

```python
class OperatorWithProps(bpy.types.Operator):
    bl_idname = "object.operator_with_props"
    bl_label = "Operator with Properties"
    bl_options = {'REGISTER', 'UNDO'}

    # 定义属性
    count: bpy.props.IntProperty(
        name="Count",
        default=5,
        min=1,
        max=100
    )

    size: bpy.props.FloatProperty(
        name="Size",
        default=2.0,
        min=0.1,
        max=10.0
    )

    def execute(self, context):
        # 使用属性
        for i in range(self.count):
            bpy.ops.mesh.primitive_cube_add(size=self.size)
        return {'FINISHED'}

def register():
    bpy.utils.register_class(OperatorWithProps)

def unregister():
    bpy.utils.unregister_class(OperatorWithProps)
```

### 模态操作符

```python
class ModalOperator(bpy.types.Operator):
    bl_idname = "object.modal_operator"
    bl_label = "Modal Operator"

    def __init__(self):
        self.timer = None

    def __del__(self):
        if self.timer:
            self.timer.cancel()

    def modal(self, context, event):
        if event.type == 'TIMER':
            # 定时器事件
            print("Timer event")
        elif event.type == 'ESC':
            # 取消操作
            return {'CANCELLED'}
        elif event.type == 'RET':
            # 完成操作
            return {'FINISHED'}
        return {'RUNNING_MODAL'}

    def invoke(self, context, event):
        # 创建定时器
        self.timer = context.window_manager.event_timer_add(0.1, window=context.window)
        context.window_manager.modal_handler_add(self)
        return {'RUNNING_MODAL'}

def register():
    bpy.utils.register_class(ModalOperator)

def unregister():
    bpy.utils.unregister_class(ModalOperator)
```

## 面板 (Panel)

### 基础面板

```python
class SimplePanel(bpy.types.Panel):
    bl_label = "Simple Panel"
    bl_idname = "VIEW3D_PT_simple_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'Simple'

    def draw(self, context):
        layout = self.layout
        layout.label(text="Hello World")

def register():
    bpy.utils.register_class(SimplePanel)

def unregister():
    bpy.utils.unregister_class(SimplePanel)
```

### 面板布局

```python
class LayoutPanel(bpy.types.Panel):
    bl_label = "Layout Panel"
    bl_idname = "VIEW3D_PT_layout_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'Layout'

    def draw(self, context):
        layout = self.layout

        # 标签
        layout.label(text="Label")

        # 分隔符
        layout.separator()

        # 按钮
        layout.operator("object.simple_operator")

        # 属性
        obj = context.active_object
        if obj:
            layout.prop(obj, "name")
            layout.prop(obj, "location")

        # 行
        row = layout.row()
        row.prop(obj, "rotation_euler")

        # 列
        col = layout.column()
        col.prop(obj, "scale")
        col.prop(obj, "dimensions")

        # 分割
        split = layout.split()
        split.prop(obj, "location", index=0)
        split.prop(obj, "location", index=1)
        split.prop(obj, "location", index=2)

def register():
    bpy.utils.register_class(LayoutPanel)

def unregister():
    bpy.utils.unregister_class(LayoutPanel)
```

### 属性面板

```python
class PropertiesPanel(bpy.types.Panel):
    bl_label = "Properties Panel"
    bl_idname = "VIEW3D_PT_properties_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'Properties'

    def draw(self, context):
        layout = self.layout
        props = context.scene.my_props

        # 布尔属性
        layout.prop(props, "bool_prop")

        # 整数属性
        layout.prop(props, "int_prop")

        # 浮点属性
        layout.prop(props, "float_prop")

        # 字符串属性
        layout.prop(props, "string_prop")

        # 枚举属性
        layout.prop(props, "enum_prop")

        # 向量属性
        layout.prop(props, "vector_prop")

        # 颜色属性
        layout.prop(props, "color_prop")

class MyProperties(bpy.types.PropertyGroup):
    bool_prop: bpy.props.BoolProperty(name="Boolean", default=False)
    int_prop: bpy.props.IntProperty(name="Integer", default=0, min=0, max=100)
    float_prop: bpy.props.FloatProperty(name="Float", default=0.0, min=0.0, max=1.0)
    string_prop: bpy.props.StringProperty(name="String", default="")
    enum_prop: bpy.props.EnumProperty(
        name="Enum",
        items=[
            ('A', "Option A", "Description A"),
            ('B', "Option B", "Description B"),
            ('C', "Option C", "Description C"),
        ]
    )
    vector_prop: bpy.props.FloatVectorProperty(name="Vector", default=(0.0, 0.0, 0.0))
    color_prop: bpy.props.FloatColorProperty(name="Color", default=(0.8, 0.8, 0.8, 1.0))

def register():
    bpy.utils.register_class(MyProperties)
    bpy.utils.register_class(PropertiesPanel)
    bpy.types.Scene.my_props = bpy.props.PointerProperty(type=MyProperties)

def unregister():
    bpy.utils.unregister_class(MyProperties)
    bpy.utils.unregister_class(PropertiesPanel)
    del bpy.types.Scene.my_props
```

## 菜单 (Menu)

### 基础菜单

```python
class SimpleMenu(bpy.types.Menu):
    bl_label = "Simple Menu"
    bl_idname = "VIEW3D_MT_simple_menu"

    def draw(self, context):
        layout = self.layout
        layout.operator("object.simple_operator")
        layout.operator("mesh.primitive_cube_add")
        layout.operator("mesh.primitive_uv_sphere_add")

def register():
    bpy.utils.register_class(SimpleMenu)

def unregister():
    bpy.utils.unregister_class(SimpleMenu)
```

### 添加到现有菜单

```python
def menu_func(self, context):
    self.layout.operator("object.simple_operator")

def register():
    bpy.utils.register_class(SimpleOperator)
    bpy.types.VIEW3D_MT_object.append(menu_func)

def unregister():
    bpy.utils.unregister_class(SimpleOperator)
    bpy.types.VIEW3D_MT_object.remove(menu_func)
```

## 属性 (Properties)

### 基础属性

```python
class MyProperties(bpy.types.PropertyGroup):
    # 布尔属性
    bool_prop: bpy.props.BoolProperty(
        name="Boolean",
        description="Boolean property",
        default=False
    )

    # 整数属性
    int_prop: bpy.props.IntProperty(
        name="Integer",
        description="Integer property",
        default=0,
        min=0,
        max=100
    )

    # 浮点属性
    float_prop: bpy.props.FloatProperty(
        name="Float",
        description="Float property",
        default=0.0,
        min=0.0,
        max=1.0
    )

    # 字符串属性
    string_prop: bpy.props.StringProperty(
        name="String",
        description="String property",
        default=""
    )

    # 枚举属性
    enum_prop: bpy.props.EnumProperty(
        name="Enum",
        description="Enum property",
        items=[
            ('A', "Option A", "Description A"),
            ('B', "Option B", "Description B"),
            ('C', "Option C", "Description C"),
        ]
    )

    # 向量属性
    vector_prop: bpy.props.FloatVectorProperty(
        name="Vector",
        description="Vector property",
        default=(0.0, 0.0, 0.0),
        size=3
    )

    # 颜色属性
    color_prop: bpy.props.FloatColorProperty(
        name="Color",
        description="Color property",
        default=(0.8, 0.8, 0.8, 1.0)
    )

def register():
    bpy.utils.register_class(MyProperties)
    bpy.types.Scene.my_props = bpy.props.PointerProperty(type=MyProperties)

def unregister():
    bpy.utils.unregister_class(MyProperties)
    del bpy.types.Scene.my_props
```

### 属性更新

```python
def update_int_prop(self, context):
    print(f"Int property updated: {self.int_prop}")

class MyProperties(bpy.types.PropertyGroup):
    int_prop: bpy.props.IntProperty(
        name="Integer",
        description="Integer property",
        default=0,
        min=0,
        max=100,
        update=update_int_prop
    )

def register():
    bpy.utils.register_class(MyProperties)
    bpy.types.Scene.my_props = bpy.props.PointerProperty(type=MyProperties)

def unregister():
    bpy.utils.unregister_class(MyProperties)
    del bpy.types.Scene.my_props
```

## 处理程序 (Handlers)

### 帧更改处理程序

```python
def frame_change_handler(scene):
    print(f"Frame changed: {scene.frame_current}")

def register():
    bpy.app.handlers.frame_change_post.append(frame_change_handler)

def unregister():
    bpy.app.handlers.frame_change_post.remove(frame_change_handler)
```

### 保存前处理程序

```python
def save_pre_handler(dummy):
    print("Saving file...")

def register():
    bpy.app.handlers.save_pre.append(save_pre_handler)

def unregister():
    bpy.app.handlers.save_pre.remove(save_pre_handler)
```

### 加载后处理程序

```python
def load_post_handler(dummy):
    print("File loaded")

def register():
    bpy.app.handlers.load_post.append(load_post_handler)

def unregister():
    bpy.app.handlers.load_post.remove(load_post_handler)
```

## 绘制处理程序 (Draw Handlers)

### 3D 视图绘制

```python
def draw_callback_3d():
    import gpu
    import gpu_extras.batch

    # 创建着色器
    shader = gpu.shader.from_builtin('3D_UNIFORM_COLOR')
    shader.bind()

    # 设置颜色
    shader.uniform_float("color", (1.0, 0.0, 0.0, 1.0))

    # 创建线条数据
    coords = (
        (0.0, 0.0, 0.0),
        (1.0, 1.0, 1.0),
    )

    # 创建批量绘制对象
    batch = gpu_extras.batch.batch_for_shader(
        shader,
        'LINES',
        {"pos": coords}
    )

    # 绘制
    batch.draw()

def register():
    handle = bpy.types.SpaceView3D.draw_handler_add(
        draw_callback_3d, (), 'WINDOW', 'POST_VIEW'
    )

def unregister():
    bpy.types.SpaceView3D.draw_handler_remove(handle, 'WINDOW')
```

### 2D 绘制

```python
def draw_callback_2d():
    import gpu
    import gpu_extras.batch

    # 创建着色器
    shader = gpu.shader.from_builtin('2D_UNIFORM_COLOR')
    shader.bind()

    # 设置颜色
    shader.uniform_float("color", (0.0, 1.0, 0.0, 1.0))

    # 创建矩形顶点
    coords = (
        (100, 100),
        (200, 100),
        (200, 200),
        (100, 200),
    )

    # 创建批量绘制对象
    batch = gpu_extras.batch.batch_for_shader(
        shader,
        'TRI_FAN',
        {"pos": coords}
    )

    # 绘制
    batch.draw()

def register():
    handle = bpy.types.SpaceView3D.draw_handler_add(
        draw_callback_2d, (), 'WINDOW', 'POST_PIXEL'
    )

def unregister():
    bpy.types.SpaceView3D.draw_handler_remove(handle, 'WINDOW')
```

## 完整插件示例

```python
bl_info = {
    "name": "My Addon",
    "author": "Your Name",
    "version": (1, 0, 0),
    "blender": (3, 0, 0),
    "location": "View3D > Sidebar",
    "description": "My addon description",
    "category": "3D View",
}

import bpy

class MyProperties(bpy.types.PropertyGroup):
    count: bpy.props.IntProperty(
        name="Count",
        default=5,
        min=1,
        max=100
    )

    size: bpy.props.FloatProperty(
        name="Size",
        default=2.0,
        min=0.1,
        max=10.0
    )

class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "Create Cubes"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        props = context.scene.my_props
        for i in range(props.count):
            bpy.ops.mesh.primitive_cube_add(size=props.size)
        return {'FINISHED'}

class MyPanel(bpy.types.Panel):
    bl_label = "My Panel"
    bl_idname = "VIEW3D_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'My Addon'

    def draw(self, context):
        layout = self.layout
        props = context.scene.my_props

        layout.prop(props, "count")
        layout.prop(props, "size")
        layout.separator()
        layout.operator("object.my_operator")

def register():
    bpy.utils.register_class(MyProperties)
    bpy.utils.register_class(MyOperator)
    bpy.utils.register_class(MyPanel)
    bpy.types.Scene.my_props = bpy.props.PointerProperty(type=MyProperties)

def unregister():
    bpy.utils.unregister_class(MyProperties)
    bpy.utils.unregister_class(MyOperator)
    bpy.utils.unregister_class(MyPanel)
    del bpy.types.Scene.my_props

if __name__ == "__main__":
    register()
```
