# 实用脚本和示例

本目录包含实用的 Blender Python 脚本示例。

## 基础脚本

### 创建立方体

```python
import bpy

# 创建立方体
bpy.ops.mesh.primitive_cube_add(size=2.0, location=(0.0, 0.0, 0.0))

# 获取活动对象
obj = bpy.context.active_object

# 设置对象名称
obj.name = "MyCube"

# 设置对象位置
obj.location = (0.0, 0.0, 0.0)

# 设置对象旋转
obj.rotation_euler = (0.0, 0.0, 0.0)

# 设置对象缩放
obj.scale = (1.0, 1.0, 1.0)
```

### 创建球体

```python
import bpy

# 创建球体
bpy.ops.mesh.primitive_uv_sphere_add(radius=1.0, location=(0.0, 0.0, 0.0))

# 获取活动对象
obj = bpy.context.active_object

# 设置对象名称
obj.name = "MySphere"

# 设置细分级别
obj.data.resolution_u = 32
obj.data.resolution_v = 16
```

### 创建平面

```python
import bpy

# 创建平面
bpy.ops.mesh.primitive_plane_add(size=10.0, location=(0.0, 0.0, 0.0))

# 获取活动对象
obj = bpy.context.active_object

# 设置对象名称
obj.name = "MyPlane"
```

## 材质脚本

### 创建基础材质

```python
import bpy

# 创建材质
mat = bpy.data.materials.new(name="MyMaterial")

# 启用节点
mat.use_nodes = True

# 获取节点树
nodes = mat.node_tree.nodes
links = mat.node_tree.links

# 获取 BSDF 节点
bsdf = nodes.get("Principled BSDF")

# 设置颜色
bsdf.inputs['Base Color'].default_value = (1.0, 0.0, 0.0, 1.0)

# 设置金属度
bsdf.inputs['Metallic'].default_value = 0.0

# 设置粗糙度
bsdf.inputs['Roughness'].default_value = 0.5

# 将材质分配给活动对象
obj = bpy.context.active_object
if obj.material_slots:
    obj.material_slots[0].material = mat
else:
    obj.data.materials.append(mat)
```

### 创建纹理材质

```python
import bpy

# 创建材质
mat = bpy.data.materials.new(name="TexturedMaterial")

# 启用节点
mat.use_nodes = True

# 获取节点树
nodes = mat.node_tree.nodes
links = mat.node_tree.links

# 清除默认节点
nodes.clear()

# 创建输出节点
output = nodes.new(type='ShaderNodeOutputMaterial')
output.location = (400, 0)

# 创建 BSDF 节点
bsdf = nodes.new(type='ShaderNodeBsdfPrincipled')
bsdf.location = (0, 0)

# 创建纹理节点
tex_node = nodes.new(type='ShaderNodeTexImage')
tex_node.location = (-400, 0)

# 加载图像
image = bpy.data.images.load(filepath="path/to/image.png")
tex_node.image = image

# 连接节点
links.new(bsdf.outputs['BSDF'], output.inputs['Surface'])
links.new(tex_node.outputs['Color'], bsdf.inputs['Base Color'])

# 将材质分配给活动对象
obj = bpy.context.active_object
if obj.material_slots:
    obj.material_slots[0].material = mat
else:
    obj.data.materials.append(mat)
```

## 动画脚本

### 创建关键帧

```python
import bpy

# 获取活动对象
obj = bpy.context.active_object

# 设置起始位置
obj.location = (0.0, 0.0, 0.0)

# 插入关键帧
obj.keyframe_insert(data_path="location", frame=1)

# 设置结束位置
obj.location = (5.0, 0.0, 0.0)

# 插入关键帧
obj.keyframe_insert(data_path="location", frame=25)

# 设置起始旋转
obj.rotation_euler = (0.0, 0.0, 0.0)

# 插入关键帧
obj.keyframe_insert(data_path="rotation_euler", frame=1)

# 设置结束旋转
obj.rotation_euler = (0.0, 0.0, 3.14159)

# 插入关键帧
obj.keyframe_insert(data_path="rotation_euler", frame=25)
```

### 创建旋转动画

```python
import bpy
import math

# 获取活动对象
obj = bpy.context.active_object

# 创建旋转动画
for frame in range(1, 101):
    angle = math.radians(frame * 3.6)
    obj.rotation_euler = (0.0, 0.0, angle)
    obj.keyframe_insert(data_path="rotation_euler", frame=frame)
```

## 网格编辑脚本

### 修改顶点位置

```python
import bpy

# 获取活动对象
obj = bpy.context.active_object

# 获取网格
mesh = obj.data

# 遍历顶点
for vert in mesh.vertices:
    # 修改顶点位置
    vert.co.x *= 1.5
    vert.co.y *= 1.5
    vert.co.z *= 1.5

# 更新网格
mesh.update()
```

### 创建自定义网格

```python
import bpy
import bmesh

# 创建新网格
mesh = bpy.data.meshes.new(name="CustomMesh")

# 创建 BMesh
bm = bmesh.new()

# 添加顶点
v1 = bm.verts.new((0.0, 0.0, 0.0))
v2 = bm.verts.new((1.0, 0.0, 0.0))
v3 = bm.verts.new((1.0, 1.0, 0.0))
v4 = bm.verts.new((0.0, 1.0, 0.0))

# 添加面
bm.faces.new((v1, v2, v3, v4))

# 写入网格
bm.to_mesh(mesh)
mesh.update()

# 释放 BMesh
bm.free()

# 创建对象
obj = bpy.data.objects.new(name="CustomObject", object_data=mesh)
bpy.context.collection.objects.link(obj)
bpy.context.view_layer.objects.active = obj
obj.select_set(True)
```

### 应用修改器

```python
import bpy

# 获取活动对象
obj = bpy.context.active_object

# 添加细分表面修改器
modifier = obj.modifiers.new(name="Subsurf", type='SUBSURF')
modifier.levels = 2
modifier.render_levels = 3

# 应用修改器
bpy.ops.object.modifier_apply(modifier=modifier.name)
```

## 对象操作脚本

### 复制对象

```python
import bpy

# 获取活动对象
obj = bpy.context.active_object

# 复制对象
new_obj = obj.copy()

# 链接到场景
bpy.context.collection.objects.link(new_obj)

# 设置新对象位置
new_obj.location = (2.0, 0.0, 0.0)

# 激活新对象
bpy.context.view_layer.objects.active = new_obj
new_obj.select_set(True)
```

### 删除对象

```python
import bpy

# 获取活动对象
obj = bpy.context.active_object

# 删除对象
bpy.data.objects.remove(obj, do_unlink=True)
```

### 隐藏/显示对象

```python
import bpy

# 获取活动对象
obj = bpy.context.active_object

# 隐藏对象
obj.hide_viewport = True

# 显示对象
obj.hide_viewport = False
```

## 渲染脚本

### 渲染场景

```python
import bpy

# 设置渲染设置
render = bpy.context.scene.render
render.engine = 'CYCLES'
render.resolution_x = 1920
render.resolution_y = 1080
render.filepath = "output.png"
render.image_settings.file_format = 'PNG'

# 渲染场景
bpy.ops.render.render(write_still=True)
```

### 批量渲染

```python
import bpy

# 设置渲染设置
render = bpy.context.scene.render
render.engine = 'CYCLES'
render.resolution_x = 1920
render.resolution_y = 1080

# 渲染动画
bpy.ops.render.render(animation=True)
```

## 文件操作脚本

### 导出对象

```python
import bpy

# 选择要导出的对象
bpy.ops.object.select_all(action='DESELECT')
obj = bpy.data.objects.get("MyObject")
obj.select_set(True)

# 导出为 OBJ
bpy.ops.export_scene.obj(
    filepath="export.obj",
    use_selection=True,
    use_mesh_modifiers=True
)
```

### 导入对象

```python
import bpy

# 导入 OBJ 文件
bpy.ops.import_scene.obj(filepath="import.obj")
```

## 工具脚本

### 批量重命名对象

```python
import bpy

# 重命名所有对象
for i, obj in enumerate(bpy.context.selected_objects):
    obj.name = f"Object_{i:03d}"
```

### 批量应用材质

```python
import bpy

# 创建材质
mat = bpy.data.materials.new(name="BatchMaterial")

# 将材质分配给所有选中的对象
for obj in bpy.context.selected_objects:
    if obj.type == 'MESH':
        if obj.material_slots:
            obj.material_slots[0].material = mat
        else:
            obj.data.materials.append(mat)
```

### 清理场景

```python
import bpy

# 删除所有未使用的材质
for mat in bpy.data.materials:
    if mat.users == 0:
        bpy.data.materials.remove(mat)

# 删除所有未使用的网格
for mesh in bpy.data.meshes:
    if mesh.users == 0:
        bpy.data.meshes.remove(mesh)

# 删除所有未使用的对象
for obj in bpy.data.objects:
    if obj.users == 0:
        bpy.data.objects.remove(obj)
```

## 插件开发脚本

### 简单插件模板

```python
import bpy

class SimpleOperator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "Simple Operator"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        # 创建立方体
        bpy.ops.mesh.primitive_cube_add()
        return {'FINISHED'}

def register():
    bpy.utils.register_class(SimpleOperator)

def unregister():
    bpy.utils.unregister_class(SimpleOperator)

if __name__ == "__main__":
    register()
```

### 带面板的插件

```python
import bpy

class SimplePanel(bpy.types.Panel):
    bl_label = "Simple Panel"
    bl_idname = "VIEW3D_PT_simple_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'Simple'

    def draw(self, context):
        layout = self.layout
        layout.operator("object.simple_operator")

class SimpleOperator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "Create Cube"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        bpy.ops.mesh.primitive_cube_add()
        return {'FINISHED'}

def register():
    bpy.utils.register_class(SimplePanel)
    bpy.utils.register_class(SimpleOperator)

def unregister():
    bpy.utils.unregister_class(SimplePanel)
    bpy.utils.unregister_class(SimpleOperator)

if __name__ == "__main__":
    register()
```

### 带属性的插件

```python
import bpy

class SimpleProperties(bpy.types.PropertyGroup):
    size: bpy.props.FloatProperty(
        name="Size",
        default=2.0,
        min=0.1,
        max=10.0
    )

class SimplePanel(bpy.types.Panel):
    bl_label = "Simple Panel"
    bl_idname = "VIEW3D_PT_simple_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'Simple'

    def draw(self, context):
        layout = self.layout
        props = context.scene.simple_props
        layout.prop(props, "size")
        layout.operator("object.simple_operator")

class SimpleOperator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "Create Cube"
    bl_options = {'REGISTER', 'UNDO'}

    size: bpy.props.FloatProperty(name="Size", default=2.0)

    def execute(self, context):
        props = context.scene.simple_props
        bpy.ops.mesh.primitive_cube_add(size=props.size)
        return {'FINISHED'}

def register():
    bpy.utils.register_class(SimpleProperties)
    bpy.utils.register_class(SimplePanel)
    bpy.utils.register_class(SimpleOperator)
    bpy.types.Scene.simple_props = bpy.props.PointerProperty(type=SimpleProperties)

def unregister():
    bpy.utils.unregister_class(SimpleProperties)
    bpy.utils.unregister_class(SimplePanel)
    bpy.utils.unregister_class(SimpleOperator)
    del bpy.types.Scene.simple_props

if __name__ == "__main__":
    register()
```

## 高级脚本

### 批量创建对象

```python
import bpy
import math

# 创建环形排列的立方体
num_objects = 10
radius = 5.0

for i in range(num_objects):
    angle = 2 * math.pi * i / num_objects
    x = radius * math.cos(angle)
    y = radius * math.sin(angle)

    # 创建立方体
    bpy.ops.mesh.primitive_cube_add(size=1.0, location=(x, y, 0.0))

    # 获取活动对象
    obj = bpy.context.active_object

    # 设置对象名称
    obj.name = f"Cube_{i:03d}"

    # 设置对象旋转
    obj.rotation_euler = (0.0, 0.0, angle)
```

### 创建程序化纹理

```python
import bpy
import math

# 创建图像
width = 512
height = 512
image = bpy.data.images.new(name="ProceduralTexture", width=width, height=height)

# 创建像素数据
pixels = [0.0] * (width * height * 4)

# 生成程序化纹理
for y in range(height):
    for x in range(width):
        index = (y * width + x) * 4

        # 计算颜色
        r = (x / width)
        g = (y / height)
        b = math.sin(x * 0.1) * 0.5 + 0.5
        a = 1.0

        # 设置像素
        pixels[index] = r
        pixels[index + 1] = g
        pixels[index + 2] = b
        pixels[index + 3] = a

# 设置像素数据
image.pixels = pixels
```

### 创建自定义 UI

```python
import bpy

class CustomUIPanel(bpy.types.Panel):
    bl_label = "Custom UI"
    bl_idname = "VIEW3D_PT_custom_ui"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'Custom'

    def draw(self, context):
        layout = self.layout

        # 添加标签
        layout.label(text="Custom UI Panel")

        # 添加分隔符
        layout.separator()

        # 添加按钮
        layout.operator("object.simple_operator")

        # 添加属性
        obj = context.active_object
        if obj:
            layout.prop(obj, "location")
            layout.prop(obj, "rotation_euler")
            layout.prop(obj, "scale")

class SimpleOperator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "Create Cube"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        bpy.ops.mesh.primitive_cube_add()
        return {'FINISHED'}

def register():
    bpy.utils.register_class(CustomUIPanel)
    bpy.utils.register_class(SimpleOperator)

def unregister():
    bpy.utils.unregister_class(CustomUIPanel)
    bpy.utils.unregister_class(SimpleOperator)

if __name__ == "__main__":
    register()
```
