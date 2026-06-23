# Blender 5.0 Python API 快速参考

## 基础导入

```python
import bpy
import bmesh
import mathutils
import math
```

## 常用操作

### 创建对象

```python
# 立方体
bpy.ops.mesh.primitive_cube_add(size=2.0)

# 球体
bpy.ops.mesh.primitive_uv_sphere_add(radius=1.0)

# 圆柱
bpy.ops.mesh.primitive_cylinder_add(radius=1.0, depth=2.0)

# 圆锥
bpy.ops.mesh.primitive_cone_add(radius1=1.0, radius2=0.0, depth=2.0)

# 平面
bpy.ops.mesh.primitive_plane_add(size=10.0)
```

### 选择对象

```python
# 获取活动对象
obj = bpy.context.active_object

# 获取所有选中对象
selected_objects = bpy.context.selected_objects

# 选择对象
obj.select_set(True)

# 取消选择
obj.select_set(False)

# 清除所有选择
bpy.ops.object.select_all(action='DESELECT')
```

### 对象变换

```python
# 位置
obj.location = (1.0, 2.0, 3.0)

# 旋转 (欧拉角)
obj.rotation_euler = (0.0, 0.0, math.radians(45))

# 缩放
obj.scale = (2.0, 2.0, 2.0)

# 应用变换
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
```

### 材质操作

```python
# 创建材质
mat = bpy.data.materials.new(name="MyMaterial")
mat.use_nodes = True

# 分配材质
obj.data.materials.append(mat)

# 设置颜色
bsdf = mat.node_tree.nodes.get("Principled BSDF")
bsdf.inputs['Base Color'].default_value = (1.0, 0.0, 0.0, 1.0)
```

### 网格操作

```python
# 获取网格
mesh = obj.data

# 遍历顶点
for vert in mesh.vertices:
    print(vert.co)

# 遍历面
for face in mesh.polygons:
    print(face.normal)

# 更新网格
mesh.update()
```

### BMesh 操作

```python
import bmesh

# 创建 BMesh
bm = bmesh.new()
bm.from_mesh(mesh)

# 添加顶点
vert = bm.verts.new((0.0, 0.0, 0.0))

# 添加面
bm.faces.new((v1, v2, v3, v4))

# 写回网格
bm.to_mesh(mesh)
bm.free()
```

### 动画操作

```python
# 插入关键帧
obj.keyframe_insert(data_path="location", frame=1)

# 设置当前帧
scene.frame_current = 25

# 获取帧范围
start = scene.frame_start
end = scene.frame_end
```

### 渲染操作

```python
# 设置渲染引擎
scene.render.engine = 'CYCLES'

# 设置分辨率
scene.render.resolution_x = 1920
scene.render.resolution_y = 1080

# 渲染
bpy.ops.render.render(write_still=True)
```

## 常用属性

### 对象属性

```python
obj.name  # 名称
obj.type  # 类型
obj.location  # 位置
obj.rotation_euler  # 旋转
obj.scale  # 缩放
obj.matrix_world  # 世界矩阵
obj.parent  # 父对象
obj.data  # 数据块
```

### 场景属性

```python
scene.name  # 名称
scene.frame_current  # 当前帧
scene.frame_start  # 起始帧
scene.frame_end  # 结束帧
scene.camera  # 活动相机
scene.render  # 渲染设置
```

### 网格属性

```python
mesh.name  # 名称
mesh.vertices  # 顶点列表
mesh.edges  # 边列表
mesh.polygons  # 面列表
mesh.materials  # 材质列表
```

### 材质属性

```python
mat.name  # 名称
mat.use_nodes  # 使用节点
mat.node_tree  # 节点树
mat.diffuse_color  # 漫反射颜色
```

## 常用函数

### 数学函数

```python
import math

# 角度转换
rad = math.radians(45)
deg = math.degrees(rad)

# 三角函数
sin_val = math.sin(rad)
cos_val = math.cos(rad)
tan_val = math.tan(rad)

# 其他函数
abs_val = abs(-5)
round_val = round(3.14159, 2)
```

### 向量运算

```python
import mathutils

# 创建向量
vec = mathutils.Vector((1.0, 2.0, 3.0))

# 向量长度
length = vec.length

# 归一化
normalized = vec.normalized()

# 点积
dot = vec1.dot(vec2)

# 叉积
cross = vec1.cross(vec2)

# 线性插值
result = vec1.lerp(vec2, 0.5)
```

### 矩阵运算

```python
import mathutils

# 创建矩阵
matrix = mathutils.Matrix()

# 平移矩阵
matrix = mathutils.Matrix.Translation((1.0, 2.0, 3.0))

# 旋转矩阵
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')

# 缩放矩阵
matrix = mathutils.Matrix.Scale(2.0, 4)

# 矩阵乘法
result = mat1 @ mat2
```

## 常用操作符

### 网格操作符

```python
bpy.ops.mesh.primitive_cube_add()
bpy.ops.mesh.primitive_uv_sphere_add()
bpy.ops.mesh.delete()
bpy.ops.mesh.extrude_region_move()
bpy.ops.mesh.bevel()
bpy.ops.mesh.subdivide()
```

### 对象操作符

```python
bpy.ops.object.duplicate()
bpy.ops.object.join()
bpy.ops.object.parent_set()
bpy.ops.object.parent_clear()
bpy.ops.object.transform_apply()
```

### 材质操作符

```python
bpy.ops.material.new()
bpy.ops.material.assign()
bpy.ops.material.slot_add()
```

### 渲染操作符

```python
bpy.ops.render.render()
bpy.ops.render.render_animation()
```

## 插件开发

### 注册/注销

```python
def register():
    bpy.utils.register_class(MyClass)

def unregister():
    bpy.utils.unregister_class(MyClass)
```

### 操作符

```python
class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "My Operator"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        return {'FINISHED'}
```

### 面板

```python
class MyPanel(bpy.types.Panel):
    bl_label = "My Panel"
    bl_idname = "VIEW3D_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = 'My Addon'

    def draw(self, context):
        layout = self.layout
```

### 属性

```python
class MyProperties(bpy.types.PropertyGroup):
    my_prop: bpy.props.IntProperty(default=0)
```

## 常用事件

### 处理程序

```python
# 帧更改
bpy.app.handlers.frame_change_post.append(handler)

# 保存前
bpy.app.handlers.save_pre.append(handler)

# 加载后
bpy.app.handlers.load_post.append(handler)
```

### 绘制处理程序

```python
# 3D 视图绘制
handle = bpy.types.SpaceView3D.draw_handler_add(
    callback, (), 'WINDOW', 'POST_VIEW'
)

# 2D 绘制
handle = bpy.types.SpaceView3D.draw_handler_add(
    callback, (), 'WINDOW', 'POST_PIXEL'
)
```

## 常用路径

### 文件路径

```python
import bpy

# 获取 blend 文件路径
filepath = bpy.data.filepath

# 获取脚本路径
script_path = bpy.path.abspath("//")

# 获取用户路径
user_path = bpy.utils.user_resource('SCRIPTS')
```

## 常用工具

### 消息

```python
# 报告消息
self.report({'INFO'}, "Info message")
self.report({'WARNING'}, "Warning message")
self.report({'ERROR'}, "Error message")
```

### 进度条

```python
# 显示进度条
wm = bpy.context.window_manager
wm.progress_begin(0, 100)
wm.progress_update(50)
wm.progress_end()
```

### 计时器

```python
# 注册计时器
def timer_function():
    print("Timer")
    return 0.1

bpy.app.timers.register(timer_function)

# 注销计时器
bpy.app.timers.unregister(timer_function)
```

## 常用技巧

### 检查上下文

```python
# 检查是否有活动对象
if bpy.context.active_object:
    obj = bpy.context.active_object

# 检查对象类型
if obj.type == 'MESH':
    pass
```

### 遍历对象

```python
# 遍历所有对象
for obj in bpy.data.objects:
    print(obj.name)

# 遍历选中对象
for obj in bpy.context.selected_objects:
    print(obj.name)

# 遍历场景对象
for obj in bpy.context.scene.objects:
    print(obj.name)
```

### 撤销/重做

```python
# 启用撤销
bpy.ops.ed.undo_push(message="My operation")

# 撤销
bpy.ops.ed.undo()

# 重做
bpy.ops.ed.redo()
```

### 刷新界面

```python
# 刷新场景
bpy.context.scene.update()

# 刷新区域
for window in bpy.context.window_manager.windows:
    for area in window.screen.areas:
        area.tag_redraw()
```

## 常见问题

### 获取对象数据

```python
# 获取对象数据
if obj.type == 'MESH':
    mesh = obj.data
elif obj.type == 'ARMATURE':
    arm = obj.data
```

### 访问顶点组

```python
# 获取顶点组
for group in obj.vertex_groups:
    print(group.name)

# 获取顶点权重
for vert in obj.data.vertices:
    for group in vert.groups:
        print(group.group, group.weight)
```

### 访问修改器

```python
# 遍历修改器
for modifier in obj.modifiers:
    print(modifier.name)

# 应用修改器
bpy.ops.object.modifier_apply(modifier=modifier.name)
```

### 访问约束

```python
# 遍历约束
for constraint in obj.constraints:
    print(constraint.name)

# 添加约束
obj.constraints.new(type='COPY_LOCATION')
```

## 性能优化

### 批量操作

```python
# 禁用更新
bpy.context.view_layer.update()

# 执行操作
for obj in objects:
    obj.location = (0.0, 0.0, 0.0)

# 启用更新
bpy.context.view_layer.update()
```

### 使用 BMesh

```python
# 对于大量网格操作，使用 BMesh
bm = bmesh.new()
bm.from_mesh(mesh)

# 执行操作
bm.ops.transform(...)

# 写回网格
bm.to_mesh(mesh)
bm.free()
```

### 避免频繁的 API 调用

```python
# 缓存常用值
obj = bpy.context.active_object
mesh = obj.data

# 使用缓存值
for vert in mesh.vertices:
    pass
```
