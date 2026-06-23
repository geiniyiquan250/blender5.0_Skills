# bmesh_module

---
## bmesh

# BMesh Module Reference

BMesh 模块提供低级别的网格编辑功能，用于高级网格操作。

## BMesh 概述

BMesh 是 Blender 的内部网格表示结构，提供比 bpy.ops.mesh 更高效的网格操作方式。

### 何时使用 BMesh

- 需要高性能的网格编辑操作
- 批量修改网格顶点、边、面
- 创建自定义网格算法
- 复杂的网格拓扑操作

## BMesh 基础

### 创建 BMesh

```python
import bmesh

# 创建新的 BMesh
bm = bmesh.new()

# 从现有网格创建 BMesh
mesh = bpy.context.object.data
bm = bmesh.new()
bm.from_mesh(mesh)

# 从编辑模式获取 BMesh
bm = bmesh.from_edit_mesh(mesh)
```

### 释放 BMesh

```python
# 释放 BMesh 内存
bm.free()

# 如果是从编辑模式获取的，需要释放
bmesh.update_edit_mesh(mesh)
```

### 写回网格

```python
# 将 BMesh 写回网格
bm.to_mesh(mesh)

# 更新网格
mesh.update()
```

## BMesh 类型

### BMVert (顶点)

顶点是网格的基本构建块。

```python
# 创建顶点
vert = bm.verts.new((0.0, 0.0, 0.0))

# 访问顶点坐标
co = vert.co

# 访问顶点索引
index = vert.index

# 访问连接的边
for edge in vert.link_edges:
    print(edge)

# 访问连接的面
for face in vert.link_faces:
    print(face)
```

### BMEdge (边)

边连接两个顶点。

```python
# 创建边
edge = bm.edges.new((vert1, vert2))

# 访问边的顶点
v1, v2 = edge.verts

# 访问边的索引
index = edge.index

# 访问连接的面
for face in edge.link_faces:
    print(face)

# 检查边是否是流形
is_manifold = edge.is_manifold
```

### BMFace (面)

面由三个或更多顶点组成。

```python
# 创建面
face = bm.faces.new((vert1, vert2, vert3))

# 访问面的顶点
for vert in face.verts:
    print(vert.co)

# 访问面的边
for edge in face.edges:
    print(edge)

# 访问面的法线
normal = face.normal

# 访问面的中心
center = face.calc_center_median()

# 访问面的面积
area = face.calc_area()
```

### BMLoop (循环)

循环是面的一部分，表示顶点-边-面关系。

```python
# 访问面的循环
for loop in face.loops:
    print(loop.vert)
    print(loop.edge)
    print(loop.face)
```

## BMesh 操作

### bmesh.ops.transform

变换 BMesh 元素。

```python
import mathutils

# 缩放
bmesh.ops.transform(
    bm,
    matrix=mathutils.Matrix.Scale(2.0, 4),
    space=matrix_world,
    verts=bm.verts
)

# 旋转
bmesh.ops.transform(
    bm,
    matrix=mathutils.Matrix.Rotation(math.radians(45), 4, 'Z'),
    space=matrix_world,
    verts=bm.verts
)

# 平移
bmesh.ops.transform(
    bm,
    matrix=mathutils.Matrix.Translation((1.0, 0.0, 0.0)),
    space=matrix_world,
    verts=bm.verts
)
```

### bmesh.ops.create_grid

创建网格。

```python
# 创建 10x10 网格
bmesh.ops.create_grid(
    bm,
    x_segments=10,
    y_segments=10,
    size=2.0
)
```

### bmesh.ops.create_cube

创建立方体。

```python
# 创建立方体
bmesh.ops.create_cube(bm, size=2.0)
```

### bmesh.ops.create_circle

创建圆形。

```python
# 创建圆形
bmesh.ops.create_circle(
    bm,
    cap_ends=True,
    cap_tris=True,
    segments=32,
    diameter=2.0
)
```

### bmesh.ops.extrude_face_region

挤出面区域。

```python
# 挤出选中的面
result = bmesh.ops.extrude_face_region(
    bm,
    geom=bm.faces[:],
    use_keep_orig=True,
    use_normal_flip=False,
    use_normal_from_adjacent=False
)

# 获取挤出的几何体
extruded_verts = result['geom']
```

### bmesh.ops.subdivide_edges

细分边。

```python
# 细分边
result = bmesh.ops.subdivide_edges(
    bm,
    edges=bm.edges[:],
    cuts=2,
    use_grid_fill=True,
    use_single_edge=False
)
```

### bmesh.ops.bisect_plane

用平面切割网格。

```python
# 用平面切割
result = bmesh.ops.bisect_plane(
    bm,
    geom=bm.verts[:] + bm.edges[:] + bm.faces[:],
    dist=0.01,
    plane_co=(0.0, 0.0, 0.0),
    plane_no=(0.0, 0.0, 1.0),
    use_snap_center=False,
    clear_outer=True,
    clear_inner=False
)
```

## BMesh 几何工具

### bmesh.geometry.intersect_face_point

检查点是否在面内。

```python
# 检查点是否在面内
point = mathutils.Vector((0.5, 0.5, 0.0))
result = bmesh.geometry.intersect_face_point(face, point)
```

### bmesh.geometry.distance_point_to_face

计算点到面的距离。

```python
# 计算点到面的距离
point = mathutils.Vector((1.0, 1.0, 1.0))
distance = bmesh.geometry.distance_point_to_face(face, point)
```

### bmesh.geometry.face_split

分割面。

```python
# 分割面
bmesh.geometry.face_split(face, vert1, vert2)
```

## BMesh 实用工具

### bmesh.utils.face_split_edgenet

用边网分割面。

```python
# 用边网分割面
bmesh.utils.face_split_edgenet(face, verts)
```

### bmesh.utils.vert_collapse_edge

通过边折叠顶点。

```python
# 通过边折叠顶点
bmesh.utils.vert_collapse_edge(edge, vert)
```

### bmesh.utils.edge_split

分割边。

```python
# 分割边
bmesh.utils.edge_split(edge, vert, factor=0.5)
```

## BMesh 高级用法

### 网格清理

```python
# 移除重复顶点
bmesh.ops.remove_doubles(bm, verts=bm.verts, dist=0.0001)

# 移除孤立顶点
bmesh.ops.delete(bm, geom=bm.verts, context='VERTS')

# 三角化面
bmesh.ops.triangulate(bm, faces=bm.faces)

# 重新计算法线
bmesh.ops.recalc_face_normals(bm, faces=bm.faces)
```

### 网格分析

```python
# 检查网格是否有效
is_valid = bm.is_valid

# 获取边界边
boundary_edges = [edge for edge in bm.edges if edge.is_boundary]

# 获取非流形边
non_manifold_edges = [edge for edge in bm.edges if not edge.is_manifold]

# 计算网格体积
volume = bm.calc_volume()

# 计算网格表面积
surface_area = sum(face.calc_area() for face in bm.faces)
```

### 网格遍历

```python
# 遍历所有顶点
for vert in bm.verts:
    print(f"Vertex {vert.index}: {vert.co}")

# 遍历所有边
for edge in bm.edges:
    print(f"Edge {edge.index}: {[v.index for v in edge.verts]}")

# 遍历所有面
for face in bm.faces:
    print(f"Face {face.index}: {[v.index for v in face.verts]}")

# 遍历顶点的连接边
for vert in bm.verts:
    for edge in vert.link_edges:
        print(f"Vertex {vert.index} connected to edge {edge.index}")

# 遍历边的连接面
for edge in bm.edges:
    for face in edge.link_faces:
        print(f"Edge {edge.index} connected to face {face.index}")
```

### 完整示例：创建复杂网格

```python
import bpy
import bmesh
import mathutils

# 创建新的网格对象
mesh = bpy.data.meshes.new("ComplexMesh")
obj = bpy.data.objects.new("ComplexObject", mesh)
bpy.context.scene.collection.objects.link(obj)

# 创建 BMesh
bm = bmesh.new()

# 创建基础网格
bmesh.ops.create_grid(bm, x_segments=5, y_segments=5, size=4.0)

# 挤出中心面
center_faces = [face for face in bm.faces if face.calc_center_median().length < 1.0]
if center_faces:
    result = bmesh.ops.extrude_face_region(bm, geom=center_faces)
    extruded_verts = [elem for elem in result['geom'] if isinstance(elem, bmesh.types.BMVert)]
    
    # 移动挤出的顶点
    for vert in extruded_verts:
        vert.co.z += 1.0

# 细分网格
bmesh.ops.subdivide_edges(bm, edges=bm.edges[:], cuts=1)

# 添加随机变形
for vert in bm.verts:
    vert.co += mathutils.Vector((
        (vert.co.x * 0.1) if vert.co.x > 0 else 0,
        (vert.co.y * 0.1) if vert.co.y > 0 else 0,
        math.sin(vert.co.x) * 0.2
    ))

# 清理网格
bmesh.ops.remove_doubles(bm, verts=bm.verts, dist=0.001)
bmesh.ops.recalc_face_normals(bm, faces=bm.faces)

# 写回网格
bm.to_mesh(mesh)
bm.free()

# 更新网格
mesh.update()

print("复杂网格创建完成")
```

## 性能优化建议

1. **批量操作**：尽量使用 bmesh.ops 进行批量操作，而不是逐个修改元素
2. **避免频繁转换**：减少 bmesh 和 mesh 之间的转换次数
3. **使用适当的数据结构**：使用 BMesh 的链接关系进行高效遍历
4. **及时释放资源**：使用完成后及时调用 bm.free()
5. **合理使用编辑模式**：对于复杂操作，使用编辑模式可以避免频繁的数据转换

```python
# 创建圆形
bmesh.ops.create_circle(
    bm,
    radius=1.0,
    segments=32,
    cap_tris=False,
    cap_fill=False
)
```

### bmesh.ops.create_cone

创建圆锥。

```python
# 创建圆锥
bmesh.ops.create_cone(
    bm,
    radius1=1.0,
    radius2=0.0,
    depth=2.0,
    segments=32
)
```

### bmesh.ops.create_uvsphere

创建 UV 球体。

```python
# 创建 UV 球体
bmesh.ops.create_uvsphere(
    bm,
    radius=1.0,
    segments=32,
    ring_count=16
)
```

### bmesh.ops.create_icosphere

创建二十面体球体。

```python
# 创建二十面体球体
bmesh.ops.create_icosphere(
    bm,
    radius=1.0,
    subdivisions=2
)
```

### bmesh.ops.extrude_edge_face_indiv

挤出边和面。

```python
# 挤出选中的边
geom = bm.edges[:]
result = bmesh.ops.extrude_edge_face_indiv(bm, geom=geom)

# 移动挤出的几何体
bmesh.ops.transform(
    bm,
    matrix=mathutils.Matrix.Translation((0.0, 0.0, 1.0)),
    verts=result['geom']
)
```

### bmesh.ops.extrude_vert_indiv

挤出顶点。

```python
# 挤出选中的顶点
geom = bm.verts[:]
result = bmesh.ops.extrude_vert_indiv(bm, geom=geom)

# 移动挤出的顶点
bmesh.ops.transform(
    bm,
    matrix=mathutils.Matrix.Translation((0.0, 0.0, 1.0)),
    verts=result['geom']
)
```

### bmesh.ops.bevel

倒角边。

```python
# 倒角选中的边
geom = bm.edges[:]
bmesh.ops.bevel(
    bm,
    geom=geom,
    offset=0.1,
    segments=2,
    profile=0.5,
    clamp_overlap=True
)
```

### bmesh.ops.bisect_plane

用平面切割网格。

```python
# 用平面切割网格
geom = bm.faces[:]
result = bmesh.ops.bisect_plane(
    bm,
    geom=geom,
    plane_co=(0.0, 0.0, 0.0),
    plane_no=(0.0, 0.0, 1.0),
    clear_inner=False,
    clear_outer=False
)
```

### bmesh.ops.bridge_loops

连接两个循环。

```python
# 连接两个循环
loops = [loop1, loop2]
bmesh.ops.bridge_loops(
    bm,
    loops=loops,
    use_cyclic=False,
    use_merge=False,
    use_bridge_loops=True
)
```

### bmesh.ops.duplicate

复制几何体。

```python
# 复制选中的几何体
geom = bm.faces[:]
result = bmesh.ops.duplicate(bm, geom=geom)

# 移动复制的几何体
bmesh.ops.transform(
    bm,
    matrix=mathutils.Matrix.Translation((1.0, 0.0, 0.0)),
    verts=result['geom']
)
```

### bmesh.ops.delete

删除几何体。

```python
# 删除选中的面
geom = bm.faces[:]
bmesh.ops.delete(bm, geom=geom, context='FACES')

# 删除选中的边
geom = bm.edges[:]
bmesh.ops.delete(bm, geom=geom, context='EDGES')

# 删除选中的顶点
geom = bm.verts[:]
bmesh.ops.delete(bm, geom=geom, context='VERTS')
```

### bmesh.ops.merge

合并元素。

```python
# 合并选中的顶点
geom = bm.verts[:]
bmesh.ops.merge(
    bm,
    geom=geom,
    dist=0.01,
    use_verts=False
)
```

### bmesh.ops.split

分割几何体。

```python
# 分割选中的边
geom = bm.edges[:]
bmesh.ops.split(bm, geom=geom)
```

### bmesh.ops.smooth

平滑网格。

```python
# 平滑选中的顶点
geom = bm.verts[:]
bmesh.ops.smooth(
    bm,
    verts=geom,
    factor=0.5,
    mirror_clip_x=False,
    mirror_clip_y=False,
    mirror_clip_z=False,
    clip_dist=0.0,
    use_axis_x=True,
    use_axis_y=True,
    use_axis_z=True
)
```

### bmesh.ops.inset

内插面。

```python
# 内插选中的面
geom = bm.faces[:]
bmesh.ops.inset(
    bm,
    geom=geom,
    thickness=0.1,
    depth=0.0,
    use_even_offset=False,
    use_boundary=True,
    use_interpolate=True
)
```

### bmesh.ops.subdivide_edges

细分边。

```python
# 细分选中的边
geom = bm.edges[:]
bmesh.ops.subdivide_edges(
    bm,
    edges=geom,
    cuts=1,
    use_grid_fill=False,
    use_smooth_even=False
)
```

### bmesh.ops.weld_vert

焊接顶点。

```python
# 焊接选中的顶点
verts = bm.verts[:]
bmesh.ops.weld_vert(bm, verts=verts)
```

### bmesh.ops.remove_doubles

移除重复顶点。

```python
# 移除重复顶点
bmesh.ops.remove_doubles(
    bm,
    verts=bm.verts,
    dist=0.0001
)
```

### bmesh.ops.connect_verts

连接顶点。

```python
# 连接选中的顶点
verts = bm.verts[:]
bmesh.ops.connect_verts(bm, verts=verts)
```

### bmesh.ops.edge_split

分割边。

```python
# 分割选中的边
edges = bm.edges[:]
bmesh.ops.edge_split(bm, edges=edges)
```

### bmesh.ops.face_split

分割面。

```python
# 分割选中的面
faces = bm.faces[:]
bmesh.ops.face_split(bm, faces=faces)
```

### bmesh.ops.reverse_faces

反转面法线。

```python
# 反转选中的面
faces = bm.faces[:]
bmesh.ops.reverse_faces(bm, faces=faces)
```

### bmesh.ops.recalc_face_normals

重新计算面法线。

```python
# 重新计算所有面的法线
bmesh.ops.recalc_face_normals(bm, faces=bm.faces)
```

### bmesh.ops.rotate

旋转几何体。

```python
# 旋转选中的顶点
import mathutils

geom = bm.verts[:]
bmesh.ops.rotate(
    bm,
    cent=(0.0, 0.0, 0.0),
    matrix=mathutils.Matrix.Rotation(math.radians(45), 4, 'Z'),
    verts=geom
)
```

### bmesh.ops.scale

缩放几何体。

```python
# 缩放选中的顶点
geom = bm.verts[:]
bmesh.ops.scale(
    bm,
    vec=(2.0, 2.0, 2.0),
    space=mathutils.Matrix(),
    verts=geom
)
```

### bmesh.ops.translate

平移几何体。

```python
# 平移选中的顶点
geom = bm.verts[:]
bmesh.ops.translate(
    bm,
    vec=(1.0, 0.0, 0.0),
    space=mathutils.Matrix(),
    verts=geom
)
```

## BMesh 选择

### 选择顶点

```python
# 选择所有顶点
for vert in bm.verts:
    vert.select = True

# 选择特定顶点
bm.verts[0].select = True

# 选择范围内的顶点
for vert in bm.verts:
    if vert.co.length < 1.0:
        vert.select = True
```

### 选择边

```python
# 选择所有边
for edge in bm.edges:
    edge.select = True

# 选择特定边
bm.edges[0].select = True

# 选择连接到选中顶点的边
for vert in bm.verts:
    if vert.select:
        for edge in vert.link_edges:
            edge.select = True
```

### 选择面

```python
# 选择所有面
for face in bm.faces:
    face.select = True

# 选择特定面
bm.faces[0].select = True

# 选择法线向上的面
for face in bm.faces:
    if face.normal.z > 0.5:
        face.select = True
```

### 隐藏元素

```python
# 隐藏选中的顶点
for vert in bm.verts:
    if vert.select:
        vert.hide = True

# 隐藏选中的边
for edge in bm.edges:
    if edge.select:
        edge.hide = True

# 隐藏选中的面
for face in bm.faces:
    if face.select:
        face.hide = True
```

## BMesh 查询

### 查找顶点

```python
# 查找特定坐标的顶点
target_co = (0.0, 0.0, 0.0)
for vert in bm.verts:
    if (vert.co - target_co).length < 0.001:
        print(f"Found vertex: {vert.index}")
        break
```

### 查找边

```python
# 查找连接两个顶点的边
v1 = bm.verts[0]
v2 = bm.verts[1]
for edge in v1.link_edges:
    if v2 in edge.verts:
        print(f"Found edge: {edge.index}")
        break
```

### 查找面

```python
# 查找包含特定顶点的面
vert = bm.verts[0]
for face in vert.link_faces:
    print(f"Found face: {face.index}")
```

### 获取边界边

```python
# 获取所有边界边
boundary_edges = [edge for edge in bm.edges if len(edge.link_faces) == 1]
```

### 获取孤立顶点

```python
# 获取所有孤立顶点
isolated_verts = [vert for vert in bm.verts if len(vert.link_edges) == 0]
```

## BMesh 遍历

### 遍历顶点

```python
# 遍历所有顶点
for vert in bm.verts:
    print(f"Vertex {vert.index}: {vert.co}")

# 遍历选中的顶点
for vert in bm.verts:
    if vert.select:
        print(f"Selected vertex {vert.index}: {vert.co}")
```

### 遍历边

```python
# 遍历所有边
for edge in bm.edges:
    print(f"Edge {edge.index}: {edge.verts[0].co} -> {edge.verts[1].co}")

# 遍历选中的边
for edge in bm.edges:
    if edge.select:
        print(f"Selected edge {edge.index}")
```

### 遍历面

```python
# 遍历所有面
for face in bm.faces:
    print(f"Face {face.index}: {len(face.verts)} vertices")

# 遍历选中的面
for face in bm.faces:
    if face.select:
        print(f"Selected face {face.index}")
```

## BMesh 实用函数

### bmesh.geometry

```python
import bmesh.geometry

# 获取面的中心
center = bmesh.geometry.face_center(face)

# 获取面的法线
normal = bmesh.geometry.face_normal(face)

# 获取面的面积
area = bmesh.geometry.face_area(face)

# 获取边的长度
length = bmesh.geometry.edge_length(edge)

# 获取顶点之间的距离
dist = bmesh.geometry.vert_distance(vert1, vert2)

# 获取顶点的法线
normal = bmesh.geometry.vert_normal(vert)

# 获取顶点的切线
tangent = bmesh.geometry.vert_tangent(vert)
```

### bmesh.utils

```python
import bmesh.utils

# 获取面的循环
loops = bmesh.utils.face_loops(face)

# 获取边的循环
loops = bmesh.utils.edge_loops(edge)

# 获取顶点的循环
loops = bmesh.utils.vert_loops(vert)

# 检查边是否是边界
is_boundary = bmesh.utils.edge_is_boundary(edge)

# 检查顶点是否是边界
is_boundary = bmesh.utils.vert_is_boundary(vert)

# 获取面的边界边
boundary_edges = bmesh.utils.face_boundary_edges(face)

# 获取面的边界顶点
boundary_verts = bmesh.utils.face_boundary_verts(face)
```

## BMesh 完整示例

```python
import bmesh
import bpy
import mathutils

# 创建立方体并修改
obj = bpy.context.object
if obj and obj.type == 'MESH':
    mesh = obj.data
    bm = bmesh.new()
    bm.from_mesh(mesh)
    
    # 选择所有面
    for face in bm.faces:
        face.select = True
    
    # 内插面
    bmesh.ops.inset(bm, geom=bm.faces[:], thickness=0.1)
    
    # 挤出选中的面
    result = bmesh.ops.extrude_edge_face_indiv(bm, geom=bm.faces[:])
    
    # 移动挤出的几何体
    bmesh.ops.transform(
        bm,
        matrix=mathutils.Matrix.Translation((0.0, 0.0, 0.5)),
        verts=result['geom']
    )
    
    # 写回网格
    bm.to_mesh(mesh)
    mesh.update()
    
    # 释放 BMesh
    bm.free()
```


---
## bmesh_geometry

# bmesh.geometry 模块

bmesh.geometry 模块提供了 BMesh 数据结构的几何运算函数，包括距离计算、射线投射、网格分析等操作。

## 几何运算概述

几何运算模块包含了一系列用于处理三维几何数据的实用函数，这些函数可以直接应用于 BMesh 对象。

### 何时使用几何运算

- 计算点与几何元素之间的距离
- 执行射线投射检测
- 分析网格属性
- 进行几何变换和计算

## 距离计算

### bmesh.geometry.distance_point_to_plane

计算点到平面的距离。

```python
import bmesh
import bpy
from mathutils import Vector

# 获取当前 BMesh
bm = bmesh.from_edit_mesh(bpy.context.object.data)

# 定义平面（通过三个顶点定义）
v1 = bm.verts[0]
v2 = bm.verts[1]
v3 = bm.verts[2]

# 定义测试点
point = Vector((1.0, 2.0, 3.0))

# 计算点到平面的距离
plane_normal = (v2.co - v1.co).cross(v3.co - v1.co).normalized()
distance = bmesh.geometry.distance_point_to_plane(point, v1.co, plane_normal)

print(f"点到平面的距离: {distance}")
```

### bmesh.geometry.distance_point_to_face

计算点到面的距离。

```python
import bmesh
import bpy
from mathutils import Vector

# 获取当前 BMesh
bm = bmesh.from_edit_mesh(bpy.context.object.data)

# 获取一个面
face = bm.faces[0]

# 定义测试点
point = Vector((5.0, 5.0, 5.0))

# 计算点到面的最短距离
distance = bmesh.geometry.distance_point_to_face(point, face)

print(f"点到面的距离: {distance:.4f}")
```

## 射线投射

### bmesh.geometry.ray_cast

执行射线投射检测。

```python
import bmesh
import bpy
from mathutils import Vector, Matrix

# 获取当前 BMesh
bm = bmesh.from_edit_mesh(bpy.context.object.data)

# 定义射线原点（相机位置）
ray_origin = Vector((10.0, 10.0, 10.0))

# 定义射线方向（朝向场景中心）
ray_direction = (Vector((0.0, 0.0, 0.0)) - ray_origin).normalized()

# 执行射线投射
location, normal, face_index = bmesh.geometry.ray_cast(
    bm, ray_origin, ray_direction
)

if location:
    print(f"击中位置: {location}")
    print(f"击中法线: {normal}")
    print(f"击中面索引: {face_index}")
else:
    print("射线未击中任何几何体")
```

### 多射线投射

```python
import bmesh
import bpy
from mathutils import Vector

def ray_cast_multiple(bm, origins, directions):
    """执行多条射线的投射检测"""
    results = []
    
    for origin, direction in zip(origins, directions):
        result = bmesh.geometry.ray_cast(bm, origin, direction)
        results.append(result)
    
    return results

# 示例：从多个光源位置投射射线
bm = bmesh.from_edit_mesh(bpy.context.object.data)

light_positions = [
    Vector((10.0, 10.0, 10.0)),
    Vector((-10.0, 10.0, 10.0)),
    Vector((0.0, 10.0, -10.0))
]

ray_directions = [
    (Vector((0.0, 0.0, 0.0)) - pos).normalized()
    for pos in light_positions
]

results = ray_cast_multiple(bm, light_positions, ray_directions)

for i, (loc, norm, face_idx) in enumerate(results):
    print(f"射线 {i}: {'击中' if loc else '未击中'}")
```

## 网格分析

### bmesh.geometry.mesh_area

计算网格总表面积。

```python
import bmesh
import bpy

# 获取当前 BMesh
bm = bmesh.from_edit_mesh(bpy.context.object.data)

# 计算总表面积
total_area = bmesh.geometry.mesh_area(bm)

print(f"网格总表面积: {total_area:.4f} 平方单位")

# 计算单个面的面积
for i, face in enumerate(bm.faces):
    area = bmesh.geometry.face_area(face)
    print(f"面 {i} 面积: {area:.4f}")
```

### bmesh.geometry.face_area

计算单个面的面积。

```python
import bmesh
import bpy

# 获取当前 BMesh
bm = bmesh.from_edit_mesh(bpy.context.object.data)

# 计算每个面的面积
for face in bm.faces:
    area = bmesh.geometry.face_area(face)
    if area > 0.1:  # 找出较大的面
        print(f"面 {face.index}: 面积 = {area:.4f}")
```

### bmesh.geometry.bound_box_2d

计算二维边界框。

```python
import bmesh
import bpy
import numpy as np

def get_mesh_bbox_2d(bm, axis='XY'):
    """获取网格在指定平面上的二维边界框"""
    vertices = [v.co for v in bm.verts]
    
    if axis == 'XY':
        coords = [(v.x, v.y) for v in vertices]
    elif axis == 'XZ':
        coords = [(v.x, v.z) for v in vertices]
    elif axis == 'YZ':
        coords = [(v.y, v.z) for v in vertices]
    else:
        coords = [(v.x, v.y) for v in vertices]
    
    min_x = min(c[0] for c in coords)
    max_x = max(c[0] for c in coords)
    min_y = min(c[1] for c in coords)
    max_y = max(c[1] for c in coords)
    
    return (min_x, min_y, max_x, max_y)

# 获取 XY 平面边界框
bm = bmesh.from_edit_mesh(bpy.context.object.data)
bbox = get_mesh_bbox_2d(bm, 'XY')

print(f"X 范围: {bbox[0]:.2f} 到 {bbox[2]:.2f}")
print(f"Y 范围: {bbox[1]:.2f} 到 {bbox[3]:.2f}")
```

## 实际应用示例

### 碰撞检测系统

```python
import bmesh
import bpy
from mathutils import Vector, Matrix

class BMeshCollisionDetector:
    def __init__(self, obj):
        self.obj = obj
        self.bm = bmesh.new()
        self.bm.from_mesh(obj.data)
        self.bm.verts.ensure_lookup_table()
        self.bm.faces.ensure_lookup_table()
    
    def point_inside_mesh(self, point):
        """检查点是否在网格内部"""
        # 使用射线投射方法：从点到无穷远的射线
        direction = Vector((0.0, 0.0, 1.0))
        
        # 多次投射以提高准确性
        intersections = 0
        
        for test_dir in [
            Vector((1.0, 0.0, 0.0)),
            Vector((0.0, 1.0, 0.0)),
            Vector((0.0, 0.0, 1.0)),
            Vector((1.0, 1.0, 1.0)).normalized()
        ]:
            loc, norm, face_idx = bmesh.geometry.ray_cast(
                self.bm, point - test_dir * 1000, test_dir
            )
            if loc:
                intersections += 1
        
        return intersections % 2 == 1
    
    def distance_to_surface(self, point):
        """计算点到网格表面的最短距离"""
        min_distance = float('inf')
        
        for face in self.bm.faces:
            dist = bmesh.geometry.distance_point_to_face(point, face)
            if dist < min_distance:
                min_distance = dist
        
        return min_distance
    
    def closest_point_on_surface(self, point):
        """找到网格表面上最近的点"""
        closest_point = None
        min_distance = float('inf')
        
        for face in self.bm.faces:
            # 计算投影点
            plane_normal = face.normal
            projected = bmesh.geometry.distance_point_to_plane(
                point, face.calc_center_median(), plane_normal
            )
            
            # 检查点是否在面内
            face_point = point - plane_normal * projected
            if self._point_in_face(face_point, face):
                dist = abs(projected)
                if dist < min_distance:
                    min_distance = dist
                    closest_point = face_point
        
        # 如果没有找到面内投影，检查边和顶点
        if not closest_point:
            closest_point, _ = self.closest_point_on_edges(point)
        
        return closest_point
    
    def _point_in_face(self, point, face):
        """检查点是否在面内"""
        # 使用重心坐标方法
        return face.point_inside(point)
    
    def closest_point_on_edges(self, point):
        """找到网格边上最近的点"""
        closest_point = None
        min_distance = float('inf')
        
        for edge in self.bm.edges:
            v1, v2 = edge.verts
            edge_vec = v2.co - v1.co
            edge_length = edge_vec.length
            
            if edge_length < 0.0001:
                continue
            
            # 计算点到线段的投影
            t = ((point - v1.co).dot(edge_vec)) / (edge_length * edge_length)
            t = max(0.0, min(1.0, t))
            
            closest = v1.co + edge_vec * t
            dist = (point - closest).length
            
            if dist < min_distance:
                min_distance = dist
                closest_point = closest
        
        return closest_point, min_distance
    
    def intersect_ray(self, ray_origin, ray_direction):
        """检测射线与网格的交点"""
        location, normal, face_index = bmesh.geometry.ray_cast(
            self.bm, ray_origin, ray_direction
        )
        
        return location, normal, face_index
    
    def intersect_segment(self, point1, point2):
        """检测线段与网格的交点"""
        direction = (point2 - point1).normalized()
        distance = (point2 - point1).length
        
        location, normal, face_index = bmesh.geometry.ray_cast(
            self.bm, point1, direction, distance
        )
        
        return location, normal, face_index
    
    def cleanup(self):
        """清理资源"""
        self.bm.free()


def detect_collisions(object_a, object_b):
    """检测两个对象之间的碰撞"""
    detector_a = BMeshCollisionDetector(object_a)
    detector_b = BMeshCollisionDetector(object_b)
    
    collision_points = []
    
    # 检测 B 的顶点是否在 A 内部
    for vert in detector_b.bm.verts:
        world_co = object_b.matrix_world @ vert.co
        if detector_a.point_inside_mesh(world_co):
            collision_points.append(('vertex_A', world_co))
    
    # 检测 A 的顶点是否在 B 内部
    for vert in detector_a.bm.verts:
        world_co = object_a.matrix_world @ vert.co
        if detector_b.point_inside_mesh(world_co):
            collision_points.append(('vertex_B', world_co))
    
    detector_a.cleanup()
    detector_b.cleanup()
    
    return collision_points
```

### 网格优化工具

```python
import bmesh
import bpy
from mathutils import Vector

class MeshOptimizer:
    def __init__(self, obj):
        self.obj = obj
        self.bm = bmesh.new()
        self.bm.from_mesh(obj.data)
    
    def remove_small_faces(self, min_area=0.001):
        """移除面积过小的面"""
        faces_to_remove = []
        
        for face in self.bm.faces:
            area = bmesh.geometry.face_area(face)
            if area < min_area:
                faces_to_remove.append(face)
        
        for face in faces_to_remove:
            bmesh.ops.delete(self.bm, geom=[face], context='FACES')
        
        print(f"移除了 {len(faces_to_remove)} 个小面")
        return len(faces_to_remove)
    
    def merge_close_vertices(self, threshold=0.001):
        """合并距离过近的顶点"""
        result = bmesh.ops.remove_doubles(
            self.bm,
            verts=self.bm.verts,
            dist=threshold
        )
        
        print(f"合并了 {len(result['vert_len'])} 个顶点")
        return len(result['vert_len'])
    
    def fill_holes(self, hole_size_limit=10):
        """填充孔洞"""
        holes_filled = 0
        
        for face in self.bm.faces:
            face.select_set(False)
        
        # 找到边界边
        boundary_edges = [e for e in self.bm.edges if len(e.link_faces) == 1]
        
        while boundary_edges:
            # 获取边界循环
            boundary_verts = self._get_boundary_loop(boundary_edges[0])
            
            if len(boundary_verts) <= hole_size_limit:
                # 填充孔洞
                bmesh.ops.holes_fill(self.bm, edges=[boundary_edges[0]])
                holes_filled += 1
            
            boundary_edges = [e for e in self.bm.edges if len(e.link_faces) == 1]
        
        print(f"填充了 {holes_filled} 个孔洞")
        return holes_filled
    
    def _get_boundary_loop(self, start_edge):
        """获取边界循环的顶点"""
        loop_verts = []
        current_edge = start_edge
        visited_edges = set()
        
        while current_edge not in visited_edges:
            visited_edges.add(current_edge)
            
            # 获取边界边上的两个顶点
            v1, v2 = current_edge.verts
            
            # 找到下一个边界边
            next_edge = None
            for edge in v2.link_edges:
                if edge != current_edge and len(edge.link_faces) == 1:
                    next_edge = edge
                    break
            
            if next_edge:
                current_edge = next_edge
                loop_verts.append(v1.co.copy())
            else:
                break
        
        return loop_verts
    
    def recalculate_normals(self):
        """重新计算法线"""
        bmesh.ops.recalc_face_normals(self.bm, faces=self.bm.faces)
    
    def triangulate(self):
        """三角化网格"""
        result = bmesh.ops.triangulate(
            self.bm,
            faces=self.bm.faces,
            quad_method='BEAUTY',
            ngon_method='BEAUTY'
        )
        
        print(f"三角化了 {len(result['faces'])} 个面")
        return len(result['faces'])
    
    def decimate(self, ratio=0.5):
        """简化网格"""
        # 计算目标顶点数
        original_vert_count = len(self.bm.verts)
        target_vert_count = int(original_vert_count * ratio)
        
        if target_vert_count < 4:
            print("简化比例太小")
            return 0
        
        result = bmesh.ops.decimate(
            self.bm,
            verts=self.bm.verts,
            edges=self.bm.edges,
            factor=1.0 - ratio,
            use_edge_collapse=True
        )
        
        removed = original_vert_count - len(self.bm.verts)
        print(f"简化网格：移除了 {removed} 个顶点")
        return removed
    
    def apply_changes(self):
        """应用更改到原始网格"""
        self.bm.to_mesh(self.obj.data)
        self.bm.free()
        self.obj.data.update()


def optimize_mesh(obj, remove_small=True, merge_close=True, 
                  triangulate=True, decimate_ratio=None):
    """完整的网格优化流程"""
    optimizer = MeshOptimizer(obj)
    
    if remove_small:
        optimizer.remove_small_faces()
    
    if merge_close:
        optimizer.merge_close_vertices()
    
    if triangulate:
        optimizer.triangulate()
    
    if decimate_ratio is not None:
        optimizer.decimate(decimate_ratio)
    
    optimizer.recalculate_normals()
    optimizer.apply_changes()
    
    print(f"网格 {obj.name} 优化完成")
```

### 几何测量工具

```python
import bmesh
import bpy
from mathutils import Vector, Matrix
import numpy as np

class GeometryMeasurer:
    def __init__(self, obj):
        self.obj = obj
        self.bm = bmesh.new()
        self.bm.from_mesh(obj.data)
        self.bm.verts.ensure_lookup_table()
        self.bm.faces.ensure_lookup_table()
    
    def measure_all(self):
        """执行所有测量"""
        measurements = {
            'bounding_box': self.measure_bounding_box(),
            'surface_area': self.measure_surface_area(),
            'volume': self.measure_volume(),
            'dimensions': self.measure_dimensions(),
            'vertex_count': len(self.bm.verts),
            'edge_count': len(self.bm.edges),
            'face_count': len(self.bm.faces),
            'triangle_count': self._count_triangles(),
            'quad_count': self._count_quads(),
            'ngon_count': self._count_ngons()
        }
        
        return measurements
    
    def measure_bounding_box(self):
        """测量边界框"""
        coords = [v.co for v in self.bm.verts]
        
        min_x = min(v.x for v in coords)
        max_x = max(v.x for v in coords)
        min_y = min(v.y for v in coords)
        max_y = max(v.y for v in coords)
        min_z = min(v.z for v in coords)
        max_z = max(v.z for v in coords)
        
        return {
            'min': Vector((min_x, min_y, min_z)),
            'max': Vector((max_x, max_y, max_z)),
            'center': Vector(( (min_x + max_x) / 2, (min_y + max_y) / 2, (min_z + max_z) / 2)),
            'size': Vector((max_x - min_x, max_y - min_y, max_z - min_z))
        }
    
    def measure_surface_area(self):
        """测量表面积"""
        total_area = 0.0
        
        for face in self.bm.faces:
            if len(face.verts) == 3:
                # 三角形
                v1, v2, v3 = face.verts
                area = 0.5 * (v2.co - v1.co).cross(v3.co - v1.co).length
            elif len(face.verts) == 4:
                # 四边形 - 分割为两个三角形
                v1, v2, v3, v4 = face.verts
                area1 = 0.5 * (v2.co - v1.co).cross(v3.co - v1.co).length
                area2 = 0.5 * (v4.co - v1.co).cross(v3.co - v1.co).length
                area = area1 + area2
            else:
                # 多边形
                area = bmesh.geometry.face_area(face)
            
            total_area += area
        
        return total_area
    
    def measure_volume(self):
        """测量体积（仅对封闭网格有效）"""
        total_volume = 0.0
        
        for face in self.bm.faces:
            if len(face.verts) < 3:
                continue
            
            # 使用有符号体积方法
            v1 = face.verts[0].co
            for i in range(1, len(face.verts) - 1):
                v2 = face.verts[i].co
                v3 = face.verts[i + 1].co
                
                # 计算四面体体积
                cross = (v2 - v1).cross(v3 - v1)
                volume = v1.dot(cross) / 6.0
                total_volume += volume
        
        return abs(total_volume)
    
    def measure_dimensions(self):
        """测量尺寸"""
        bbox = self.measure_bounding_box()
        return bbox['size']
    
    def _count_triangles(self):
        """统计三角形数量"""
        return sum(1 for f in self.bm.faces if len(f.verts) == 3)
    
    def _count_quads(self):
        """统计四边形数量"""
        return sum(1 for f in self.bm.faces if len(f.verts) == 4)
    
    def _count_ngons(self):
        """统计多边形数量"""
        return sum(1 for f in self.bm.faces if len(f.verts) > 4)
    
    def cleanup(self):
        """清理资源"""
        self.bm.free()


def analyze_selected_object():
    """分析选中对象"""
    obj = bpy.context.active_object
    
    if not obj or obj.type != 'MESH':
        print("请选中一个网格对象")
        return
    
    measurer = GeometryMeasurer(obj)
    measurements = measurer.measure_all()
    measurer.cleanup()
    
    print(f"\n=== 对象分析报告: {obj.name} ===")
    print(f"尺寸: X={measurements['dimensions'].x:.3f}, "
          f"Y={measurements['dimensions'].y:.3f}, "
          f"Z={measurements['dimensions'].z:.3f}")
    print(f"表面积: {measurements['surface_area']:.3f}")
    print(f"体积: {measurements['volume']:.3f}")
    print(f"顶点数: {measurements['vertex_count']}")
    print(f"边数: {measurements['edge_count']}")
    print(f"面数: {measurements['face_count']}")
    print(f"  - 三角形: {measurements['triangle_count']}")
    print(f"  - 四边形: {measurements['quad_count']}")
    print(f"  - 多边形: {measurements['ngon_count']}")
```

## 最佳实践

1. **性能考虑**：对于大型网格，尽量减少重复的几何计算
2. **数据更新**：修改 BMesh 后记得调用 bm.to_mesh() 应用更改
3. **法线方向**：确保网格法线方向一致，闭合网格使用统一法线
4. **体积计算**：体积计算要求网格是闭合的，否则结果不准确
5. **射线投射**：使用适当的距离限制可以提高投射性能


---
## bmesh_ops

# bmesh.ops 模块

## 模块概述

bmesh.ops 模块提供了在 BMesh 数据结构上进行高级网格操作的函数集合。这些操作涵盖了建模过程中最常用的功能，包括挤出、倒角、循环切割、溶解、桥接等。与直接操作顶点、边、面相比，使用 bmesh.ops 中的函数可以更高效地完成复杂的网格修改任务，同时自动处理拓扑关系的变化。

每个操作函数都接受 BMesh 对象作为第一个参数，并返回操作结果的字典。大多数操作还会返回修改或创建的元素列表，方便后续处理。这些函数设计为可组合使用，可以通过组合多个基础操作来实现复杂的建模效果。bmesh.ops 中的所有函数都是无状态的纯函数，不会修改原始输入参数的值，而是返回包含结果的新数据。

理解 bmesh.ops 的工作原理对于编写高效的网格处理脚本至关重要。每个操作都有其特定的参数和返回值格式，了解这些细节可以帮助开发者更好地控制操作的行为。此外，bmesh.ops 中的一些操作可能比手动遍历和修改元素更高效，因为它们使用了 Blender 内部优化的算法。

## 挤出操作

### 挤出顶点

extrude_vert 函数将选中的顶点沿法线方向挤出，创建新的顶点和边。这个操作常用于从点开始构建网格或在现有网格上添加新结构。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))
bme.faces.new((v1, v2, v3))

# 选择一个顶点
v1.select = True

print("挤出顶点测试:")
print(f"操作前顶点数: {len(bme.verts)}")

# 挤出顶点
result = bmesh.ops.extrude_vert_only(bme, verts=[v1])
print(f"操作返回: {list(result.keys())}")

# 获取新创建的顶点
new_verts = result['verts']
print(f"新创建的顶点数: {len(new_verts)}")

for v in new_verts:
    print(f"  新顶点: {v.co}")
    v.select = False

print(f"操作后顶点数: {len(bme.verts)}")
```

### 挤出边

extrude_edge_only 函数将选中的边沿法线方向挤出，创建新的边和面。这个操作常用于在网格边缘添加厚度或创建管道结构。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))
v4 = bme.verts.new((1.0, 1.0, 0.0))
bme.faces.new((v1, v2, v4, v3))

# 选择一条边
edge = bme.edges[0]
edge.select = True

print("挤出边测试:")
print(f"操作前: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 挤出边
result = bmesh.ops.extrude_edge_only(bme, edges=[edge])
print(f"操作返回键: {list(result.keys())}")

# 获取新创建的元素
new_edges = result['edges']
new_verts = result['verts']
print(f"新创建的边数: {len(new_edges)}")
print(f"新创建的顶点数: {len(new_verts)}")

print(f"操作后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")
```

### 挤出面

extrude_face_region 函数是功能最强大的挤出操作，可以同时挤出多个面。这个操作会创建新的面和连接侧面，支持自定义挤出方向和距离。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建立方体作为测试
bme = bmesh.new()
verts = []
for x in [-1, 1]:
    for y in [-1, 1]:
        for z in [-1, 1]:
            verts.append(bme.verts.new((x, y, z)))
bme.verts.ensure_lookup_table()

# 定义六个面
face_verts = [
    (0, 1, 3, 2),  # 前面
    (5, 4, 7, 6),  # 后面
    (4, 0, 3, 7),  # 左面
    (1, 5, 6, 2),  # 右面
    (3, 2, 6, 7),  # 顶面
    (4, 5, 1, 0),  # 底面
]
faces = []
for fv in face_verts:
    faces.append(bme.faces.new([verts[i] for i in fv]))

# 选择顶面
top_face = faces[4]
top_face.select = True

print("挤出面测试:")
print(f"操作前: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")

# 挤出面
result = bmesh.ops.extrude_face_region(bme, geom=[top_face])
print(f"操作返回键: {list(result.keys())}")

# 获取新创建的元素
new_faces = [elem for elem in result['geom'] if isinstance(elem, bmesh.types.BMFace)]
new_edges = [elem for elem in result['geom'] if isinstance(elem, bmesh.types.BMEdge)]
new_verts = [elem for elem in result['geom'] if isinstance(elem, bmesh.types.BMVert)]

print(f"新创建的面数: {len(new_faces)}")
print(f"新创建的边数: {len(new_edges)}")
print(f"新创建的顶点数: {len(new_verts)}")

# 移动挤出的面
extrusion_height = 1.0
for v in new_verts:
    v.co.z += extrusion_height

print(f"操作后: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")
```

## 变换操作

### 旋转

rotate 函数对指定的元素进行旋转变换。这个操作支持围绕任意轴旋转任意角度，常用于对齐物体或创建对称结构。

```python
import bmesh
import bpy
from mathutils import Vector, Matrix, Euler

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((1.0, 0.0, 0.0))
v2 = bme.verts.new((2.0, 0.0, 0.0))
v3 = bme.verts.new((1.5, 1.0, 0.0))
bme.faces.new((v1, v2, v3))

print("旋转变换测试:")
print(f"原始坐标: {v1.co}, {v2.co}, {v3.co}")

# 创建旋转矩阵（围绕 Z 轴旋转 90 度）
angle = 1.5708  # 90度弧度
rotation_matrix = Matrix.Rotation(angle, 4, 'Z')

# 旋转所有顶点
verts = list(bme.verts)
result = bmesh.ops.rotate(bme, cent=(0, 0, 0), matrix=rotation_matrix, verts=verts)

print(f"操作返回键: {list(result.keys())}")

print(f"旋转后坐标: {v1.co}, {v2.co}, {v3.co}")

# 围绕特定点旋转
bme2 = bmesh.new()
v1 = bme2.verts.new((1.0, 0.0, 0.0))
v2 = bme2.verts.new((2.0, 0.0, 0.0))
v3 = bme2.verts.new((1.5, 1.0, 0.0))
bme2.faces.new((v1, v2, v3))

print("\n围绕特定点旋转测试:")
print(f"原始坐标: {v1.co}, {v2.co}, {v3.co}")

# 围绕中心点 (1.5, 0.5, 0) 旋转
center = Vector((1.5, 0.5, 0))
result = bmesh.ops.rotate(bme2, cent=center, matrix=rotation_matrix, verts=list(bme2.verts))

print(f"围绕 {center} 旋转后:")
print(f"  {v1.co}, {v2.co}, {v3.co}")
```

### 缩放

scale 函数对指定的元素进行缩放变换。这个操作支持各向异性缩放，可以分别控制 X、Y、Z 轴的缩放因子。

```python
import bmesh
import bpy
from mathutils import Vector, Matrix

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((1.0, 1.0, 1.0))
v2 = bme.verts.new((2.0, 1.0, 1.0))
v3 = bme.verts.new((1.5, 2.0, 1.0))
bme.faces.new((v1, v2, v3))

print("缩放变换测试:")
print(f"原始坐标: {v1.co}, {v2.co}, {v3.co}")

# 创建缩放矩阵（X轴压缩到0.5，Y轴拉伸到2倍）
scale_matrix = Matrix.Diagonal((0.5, 2.0, 1.0, 1.0))

# 缩放所有顶点
verts = list(bme.verts)
result = bmesh.ops.scale(bme, cent=(0, 0, 0), matrix=scale_matrix, verts=verts)

print(f"操作返回键: {list(result.keys())}")

print(f"缩放后坐标: {v1.co}, {v2.co}, {v3.co}")

# 均匀缩放
bme2 = bmesh.new()
for i in range(8):
    x = 1 if i & 1 else -1
    y = 1 if i & 2 else -1
    z = 1 if i & 4 else -1
    bme2.verts.new((x, y, z))
bme2.verts.ensure_lookup_table()

print("\n均匀缩放测试:")
print(f"原始: 顶点数={len(bme2.verts)}")

# 创建均匀缩放矩阵
uniform_scale = 0.5
scale_matrix = Matrix.Diagonal((uniform_scale,) * 4)

result = bmesh.ops.scale(bme2, cent=(0, 0, 0), matrix=scale_matrix, verts=list(bme2.verts))

print(f"均匀缩放 {uniform_scale} 倍后:")
for v in bme2.verts:
    print(f"  {v.co}")
```

### 平移

translate 函数对指定的元素进行平移变换。这个操作将元素移动到新的位置，支持批量移动多个元素。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))
bme.faces.new((v1, v2, v3))

print("平移变换测试:")
print(f"原始坐标: {v1.co}, {v2.co}, {v3.co}")

# 平移向量
translation = Vector((2.0, 3.0, 4.0))

# 平移所有顶点
verts = list(bme.verts)
result = bmesh.ops.translate(bme, vec=translation, verts=verts)

print(f"操作返回键: {list(result.keys())}")

print(f"平移后坐标: {v1.co}, {v2.co}, {v3.co}")

# 批量平移测试
bme2 = bmesh.new()
for x in range(3):
    for y in range(3):
        bme2.verts.new((x, y, 0))
bme2.verts.ensure_lookup_table()

print("\n批量平移测试:")
for v in bme2.verts:
    print(f"  平移前: {v.co}")

result = bmesh.ops.translate(bme2, vec=translation, verts=list(bme2.verts))

for v in bme2.verts:
    print(f"  平移后: {v.co}")
```

## 网格修改操作

### 倒角

bevel 函数对边或顶点进行倒角处理，创建平滑的过渡。这个操作在工业设计中非常重要，可以使锐利的边缘变得圆滑。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建立方体
bme = bmesh.new()
verts = []
for x in [-1, 1]:
    for y in [-1, 1]:
        for z in [-1, 1]:
            verts.append(bme.verts.new((x, y, z)))
bme.verts.ensure_lookup_table()

# 定义六个面
face_verts = [
    (0, 1, 3, 2),  # 前面
    (5, 4, 7, 6),  # 后面
    (4, 0, 3, 7),  # 左面
    (1, 5, 6, 2),  # 右面
    (3, 2, 6, 7),  # 顶面
    (4, 5, 1, 0),  # 底面
]
for fv in face_verts:
    bme.faces.new([verts[i] for i in fv])

# 选择所有边
for e in bme.edges:
    e.select = True

print("倒角测试:")
print(f"操作前: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 倒角操作
edges = list(bme.edges)
result = bmesh.ops.bevel(bme, geom=edges, offset=0.2, segments=3, vertex_only=False)

print(f"操作返回键: {list(result.keys())}")

# 获取新创建的元素
new_faces = [e for e in result['faces'] if isinstance(e, bmesh.types.BMFace)]
print(f"新创建的面数: {len(new_faces)}")

print(f"操作后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 顶点倒角测试
bme2 = bmesh.new()
v1 = bme2.verts.new((0.0, 0.0, 0.0))
v2 = bme2.verts.new((2.0, 0.0, 0.0))
v3 = bme2.verts.new((1.0, 1.0, 0.0))
bme2.faces.new((v1, v2, v3))

print("\n顶点倒角测试:")
v1.select = True
result = bmesh.ops.bevel(bme2, geom=[v1], offset=0.1, segments=2, vertex_only=True)
print(f"顶点倒角后: 顶点数={len(bme2.verts)}, 边数={len(bme2.edges)}, 面数={len(bme2.faces)}")
```

### 内插

inset 函数对面进行内插操作，在保持外轮廓的同时缩小面。这个操作常用于创建边框效果或添加细节。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((2.0, 0.0, 0.0))
v3 = bme.verts.new((2.0, 2.0, 0.0))
v4 = bme.verts.new((0.0, 2.0, 0.0))
f = bme.faces.new((v1, v2, v3, v4))

print("内插测试:")
print(f"操作前: 面数={len(bme.faces)}, 边数={len(bme.edges)}")

# 内插操作
result = bmesh.ops.inset_individual(bme, faces=[f], thickness=0.2, depth=0.0)

print(f"操作返回键: {list(result.keys())}")

# 获取新创建的面
inner_faces = [e for e in result['faces'] if isinstance(e, bmesh.types.BMFace)]
print(f"新创建的内插面数: {len(inner_faces)}")

print(f"操作后: 面数={len(bme.faces)}, 边数={len(bme.edges)}")

# 多面内插测试
bme2 = bmesh.new()
# 创建 2x2 的面网格
faces = []
for y in range(2):
    for x in range(2):
        v1 = bme2.verts.new((x, y, 0))
        v2 = bme2.verts.new((x + 1, y, 0))
        v3 = bme2.verts.new((x + 1, y + 1, 0))
        v4 = bme2.verts.new((x, y + 1, 0))
        faces.append(bme2.faces.new((v1, v2, v3, v4)))

print("\n多面内插测试:")
for f in faces:
    f.select = True

result = bmesh.ops.inset_individual(bme2, faces=faces, thickness=0.1, depth=0.0)
print(f"操作后: 面数={len(bme2.faces)}, 边数={len(bme2.edges)}")
```

### 循环切割

subdivide 函数对边进行细分切割，增加网格密度。这个操作支持多种切割模式，可以精确控制新顶点的位置。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((4.0, 0.0, 0.0))
v3 = bme.verts.new((4.0, 4.0, 0.0))
v4 = bme.verts.new((0.0, 4.0, 0.0))
bme.faces.new((v1, v2, v3, v4))

print("循环切割测试:")
print(f"操作前: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 选择所有边
edges = list(bme.edges)
for e in edges:
    e.select = True

# 细分操作
result = bmesh.ops.subdivide(bme, edges=edges, cuts=2, use_grid_fill=True, smoothness=0.0)

print(f"操作返回键: {list(result.keys())}")

# 获取新创建的顶点
new_verts = result['verts']
print(f"新创建的顶点数: {len(new_verts)}")

print(f"操作后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 边细分测试
bme2 = bmesh.new()
v1 = bme2.verts.new((0.0, 0.0, 0.0))
v2 = bme2.verts.new((1.0, 0.0, 0.0))
e = bme2.edges.new((v1, v2))

print("\n单边细分测试:")
print(f"操作前: 边数={len(bme2.edges)}")

result = bmesh.ops.subdivide(bme2, edges=[e], cuts=3)
print(f"操作后: 边数={len(bme2.edges)}, 顶点数={len(bme2.verts)}")

for v in bme2.verts:
    print(f"  顶点: {v.co}")
```

## 删除和溶解操作

### 删除元素

delete 函数删除指定的元素及其关联的拓扑结构。根据删除模式的不同，可以只删除元素本身或同时删除与之相连的孤立元素。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((1.0, 1.0, 0.0))
v4 = bme.verts.new((0.0, 1.0, 0.0))
v5 = bme.verts.new((0.5, 0.5, 0.0))  # 中心点
f1 = bme.faces.new((v1, v2, v5))
f2 = bme.faces.new((v2, v3, v5))
f3 = bme.faces.new((v3, v4, v5))
f4 = bme.faces.new((v4, v1, v5))

print("删除测试:")
print(f"操作前: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 删除一个面
result = bmesh.ops.delete(bme, geom=[f1], context='FACES')
print(f"删除面后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 删除一条边
edges = list(bme.edges)
result = bmesh.ops.delete(bme, geom=[edges[0]], context='EDGES')
print(f"删除边后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 删除一个顶点
verts = list(bme.verts)
result = bmesh.ops.delete(bme, geom=[verts[-1]], context='VERTS')
print(f"删除顶点后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 删除模式测试
bme2 = bmesh.new()
v1 = bme2.verts.new((0.0, 0.0, 0.0))
v2 = bme2.verts.new((1.0, 0.0, 0.0))
v3 = bme2.verts.new((0.5, 1.0, 0.0))
f = bme2.faces.new((v1, v2, v3))

print("\n不同删除模式测试:")
print(f"初始: 顶点数={len(bme2.verts)}, 边数={len(bme2.edges)}, 面数={len(bme2.faces)}")

# 仅删除面（保留边和顶点）
result = bmesh.ops.delete(bme2, geom=[f], context='FACES_KEEP_BORDERS')
print(f"仅删除面: 顶点数={len(bme2.verts)}, 边数={len(bme2.edges)}, 面数={len(bme2.faces)}")
```

### 溶解操作

dissolve 函数溶解元素，移除不必要的几何体同时保持网格形状。溶解比普通删除更智能，可以处理边界和相邻元素的关系。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
# 创建细分平面
for y in range(3):
    for x in range(3):
        bme.verts.new((x, y, 0))
bme.verts.ensure_lookup_table()

# 创建面
for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme.faces.new([
            bme.verts[i],
            bme.verts[i + 1],
            bme.verts[i + 4],
            bme.verts[i + 3]
        ])

print("溶解测试:")
print(f"操作前: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 溶解内部顶点
verts = list(bme.verts)
internal_verts = [v for v in verts if v.index not in [0, 2, 6, 8]]  # 保留四个角
result = bmesh.ops.dissolve(bme, verts=internal_verts, edges=[], faces=[])

print(f"溶解顶点后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 溶解边测试
bme2 = bmesh.new()
for i in range(4):
    bme2.verts.new((i, 0, 0))
bme2.verts.ensure_lookup_table()
for i in range(3):
    bme2.edges.new((bme2.verts[i], bme2.verts[i + 1]))

print("\n溶解边测试:")
print(f"操作前: 边数={len(bme2.edges)}")

edges = list(bme2.edges)
result = bmesh.ops.dissolve(bme2, verts=[], edges=[edges[1]], faces=[])
print(f"溶解中间边后: 边数={len(bme2.edges)}, 顶点数={len(bme2.verts)}")

# 溶解面测试
bme3 = bmesh.new()
for y in range(3):
    for x in range(3):
        bme3.verts.new((x, y, 0))
bme3.verts.ensure_lookup_table()

for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme3.faces.new([
            bme3.verts[i],
            bme3.verts[i + 1],
            bme3.verts[i + 4],
            bme3.verts[i + 3]
        ])

print("\n溶解面测试:")
print(f"操作前: 面数={len(bme3.faces)}")

faces = list(bme3.faces)
result = bmesh.ops.dissolve(bme3, verts=[], edges=[], faces=[faces[1]])  # 溶解中心面
print(f"溶解面后: 面数={len(bme3.faces)}")
```

## 桥接和连接操作

### 桥接边

bridge_edge_loops 函数连接两组边界边，创建一个新的面带。这个操作常用于连接两个分开的网格部分或创建管道。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建两个分离的方环
def create_square_ring(bme, center, size):
    """创建一个方形环"""
    half = size / 2
    verts = []
    for x in [-half, half]:
        for y in [-half, half]:
            v = bme.verts.new((center[0] + x, center[1] + y, center[2]))
            verts.append(v)

    # 创建四条边
    bme.edges.new((verts[0], verts[1]))
    bme.edges.new((verts[1], verts[3]))
    bme.edges.new((verts[3], verts[2]))
    bme.edges.new((verts[2], verts[0]))

    return verts

bme = bmesh.new()
ring1 = create_square_ring(bme, (0, 0, 0), 2)
ring2 = create_square_ring(bme, (0, 0, 2), 2)

print("桥接边测试:")
print(f"操作前: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 选择要桥接的边
ring1_edges = [e for e in bme.edges if all(v in ring1 for v in e.verts)]
ring2_edges = [e for e in bme.edges if all(v in ring2 for v in e.verts)]

for e in ring1_edges:
    e.select = True
for e in ring2_edges:
    e.select = True

# 桥接操作
all_edges = ring1_edges + ring2_edges
result = bmesh.ops.bridge_edge_loops(bme, edges=all_edges)

print(f"操作返回键: {list(result.keys())}")

# 获取新创建的面
new_faces = [e for e in result['faces'] if isinstance(e, bmesh.types.BMFace)]
print(f"新创建的面数: {len(new_faces)}")

print(f"操作后: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 验证结构
print("\n桥接后的边:")
for e in bme.edges:
    v1, v2 = e.verts
    z1, z2 = v1.co.z, v2.co.z
    if z1 != z2:
        print(f"  桥接边: {v1.co} - {v2.co}")
```

### 连接两个面

join_face 函数将多个面合并成一个面。这个操作会删除面之间的内部边，创建一个更大的面。

```python
import bmesh
import bpy
from mathutils import Vector

# 创建 2x2 的面网格
bme = bmesh.new()
for y in range(2):
    for x in range(2):
        v1 = bme.verts.new((x, y, 0))
        v2 = bme.verts.new((x + 1, y, 0))
        v3 = bme.verts.new((x + 1, y + 1, 0))
        v4 = bme.verts.new((x, y + 1, 0))
        bme.faces.new((v1, v2, v3, v4))

print("连接面测试:")
print(f"操作前: 面数={len(bme.faces)}, 边数={len(bme.edges)}")

# 选择要合并的面
faces = list(bme.faces)
result = bmesh.ops.join_face(bme, faces=faces)

print(f"操作返回键: {list(result.keys())}")

# 获取合并后的面
merged_faces = [e for e in result['faces'] if isinstance(e, bmesh.types.BMFace)]
print(f"合并后的面数: {len(merged_faces)}")

print(f"操作后: 面数={len(bme.faces)}, 边数={len(bme.edges)}")

if merged_faces:
    merged = merged_faces[0]
    print(f"合并面的顶点数: {len(merged.verts)}")
```

## 实践示例

### 程序化管道生成器

这个示例展示了如何使用 bmesh.ops 创建程序化管道。

```python
import bmesh
from mathutils import Vector

class PipeGenerator:
    def __init__(self, bme):
        self.bme = bme
        self.radius = 0.5
        self.segments = 8
        self.length = 5.0

    def set_parameters(self, radius=0.5, segments=8, length=5.0):
        """设置管道参数"""
        self.radius = radius
        self.segments = segments
        self.length = length

    def create_pipe_segment(self, start_point, end_point):
        """创建管道段"""
        # 计算方向和长度
        direction = end_point - start_point
        pipe_length = direction.length

        # 创建截面顶点
        section_verts = []
        for i in range(self.segments):
            angle = 2 * 3.14159 * i / self.segments
            x = self.radius * 1.0
            y = self.radius * 0.0
            z = self.radius * 0.0
            section_verts.append(Vector((x, y, z)))

        # 创建管的逻辑（这里简化处理）
        # 实际需要更复杂的算法来创建正确的网格

        return []

    def create_cylinder(self, center, height, radius=None):
        """创建圆柱体"""
        if radius is None:
            radius = self.radius

        verts = []
        n = self.segments

        # 创建顶部和底部顶点
        for i in range(n):
            angle = 2 * 3.14159 * i / n
            x = center.x + radius * 1.0
            y = center.y + radius * 0.0
            z = center.z - height / 2
            verts.append(self.bme.verts.new((x, y, z)))

        for i in range(n):
            angle = 2 * 3.14159 * i / n
            x = center.x + radius * 1.0
            y = center.y + radius * 0.0
            z = center.z + height / 2
            verts.append(self.bme.verts.new((x, y, z)))

        self.bme.verts.ensure_lookup_table()

        # 创建侧面
        for i in range(n):
            i2 = (i + 1) % n
            self.bme.faces.new([
                verts[i],
                verts[i2],
                verts[i2 + n],
                verts[i + n]
            ])

        return verts

    def extrude_pipe(self, radius=None, length=None):
        """挤出管道"""
        if radius is None:
            radius = self.radius
        if length is None:
            length = self.length

        # 选择所有侧面边
        side_edges = []
        for e in self.bme.edges:
            v1, v2 = e.verts
            if abs(v1.co.z - v2.co.z) < 0.001:
                side_edges.append(e)

        for e in side_edges:
            e.select = True

        # 挤出操作
        result = bmesh.ops.extrude_edge_only(self.bme, edges=side_edges)

        new_edges = result['edges']
        new_verts = result['verts']

        # 移动新顶点
        translation = Vector((0, 0, length))
        bmesh.ops.translate(self.bme, vec=translation, verts=new_verts)

        return new_verts

# 使用示例
bme = bmesh.new()
generator = PipeGenerator(bme)

print("管道生成器测试:")
print(f"初始: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")

# 创建圆柱体
generator.set_parameters(radius=0.5, segments=16, length=2.0)
generator.create_cylinder(Vector((0, 0, 0)), 2.0)

print(f"创建圆柱体后: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")

# 挤出管道
generator.extrude_pipe(length=3.0)

print(f"挤出管道后: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")
```

### 网格细分工具

这个示例展示了如何使用 bmesh.ops 创建网格细分工具。

```python
import bmesh
from mathutils import Vector

class MeshSubdivider:
    def __init__(self, bme):
        self.bme = bme

    def subdivide_edges(self, edges, cuts=1, smoothness=0.0):
        """细分选中的边"""
        if not edges:
            return []

        result = bmesh.ops.subdivide(
            self.bme,
            edges=edges,
            cuts=cuts,
            use_grid_fill=False,
            smoothness=smoothness
        )

        return result.get('verts', [])

    def subdivide_faces(self, faces, cuts=1):
        """细分面"""
        if not faces:
            return []

        # 获取面的所有边
        edges = []
        for f in faces:
            for e in f.edges:
                if e not in edges:
                    edges.append(e)

        result = bmesh.ops.subdivide(
            self.bme,
            edges=edges,
            cuts=cuts,
            use_grid_fill=True,
            smoothness=0.0
        )

        return result.get('verts', [])

    def linear_subdivide(self, cuts=1):
        """线性细分所有面"""
        faces = list(self.bme.faces)
        for f in faces:
            f.select = True

        return self.subdivide_faces(faces, cuts)

    def catmull_clark_subdivide(self, iterations=1):
        """Catmull-Clark 细分（简化版）"""
        for _ in range(iterations):
            # 收集面的中心点
            face_centers = []
            for f in self.bme.faces:
                center = f.center_median
                face_centers.append(center)

            # 为每个面创建中心顶点
            center_verts = []
            for center in face_centers:
                v = self.bme.verts.new(center)
                center_verts.append(v)

            self.bme.verts.ensure_lookup_table()

            # 连接面中心到面的顶点
            faces = list(self.bme.faces)
            for i, f in enumerate(faces):
                for loop in f.loops:
                    v = loop.vert
                    # 这里需要更复杂的逻辑来创建正确的连接

        return list(self.bme.verts)

# 使用示例
bme = bmesh.new()
# 创建细分测试网格
for y in range(3):
    for x in range(3):
        bme.verts.new((x, y, 0))
bme.verts.ensure_lookup_table()

for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme.faces.new([
            bme.verts[i],
            bme.verts[i + 1],
            bme.verts[i + 4],
            bme.verts[i + 3]
        ])

subdivider = MeshSubdivider(bme)

print("网格细分工具测试:")
print(f"初始: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")

# 细分边
edges = list(bme.edges)
for e in edges:
    e.select = True
subdivider.subdivide_edges(edges, cuts=2)

print(f"细分边后: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")

# 细分面
faces = list(bme.faces)
for f in faces:
    f.select = True
subdivider.subdivide_faces(faces, cuts=1)

print(f"细分面后: 顶点数={len(bme.verts)}, 面数={len(bme.faces)}")
```

## 注意事项

### 操作返回值的处理

bmesh.ops 中的大多数函数返回一个字典，包含修改的元素列表。返回的字典键因操作而异，常见的键包括 'verts'、'edges'、'faces' 等。在使用返回值时，应该先检查键是否存在，并正确处理返回的元素类型。

### 性能考虑

bmesh.ops 中的操作虽然经过优化，但在处理大规模网格时仍可能消耗较多资源。对于需要频繁执行的操作，可以考虑批量处理或使用更底层的顶点操作来提高性能。

### 拓扑完整性

在使用 bmesh.ops 进行操作后，应该验证网格的拓扑完整性。某些操作可能导致网格退化或产生非流形结构，这些问题可能在后续操作中导致错误。

---
## bmesh_types

# bmesh.types 模块

## 模块概述

bmesh.types 模块定义了 BMesh 数据结构中使用的所有类型。BMesh 是 Blender 的自定义网格数据结构，提供了比传统 Mesh 更强大和灵活的网格操作能力。该模块包含网格操作所需的核心类型定义，如顶点、边、面、循环等，这些类型构成了 BMesh 的基本构建块。

与标准的 Mesh 数据结构不同，BMesh 使用显式的边和循环数据结构，这使得网格操作更加精确和可控。BMesh 维护了顶点、边、面之间的完整拓扑关系，可以方便地访问相邻元素和进行复杂的网格修改。这种设计使得 BMesh 成为实现雕刻工具、建模操作、程序化几何生成等功能的理想选择。

BMesh 类型系统的一个关键特性是其面向对象的结构。每个类型都继承自基类并包含特定于该元素的属性和方法。例如，BMVert 包含坐标、法线、选择状态等属性，而 BMEdge 包含两个端点的引用和边的硬度标记。理解这些类型的特性和相互关系是掌握 BMesh 编程的基础。

## BMesh 类

### 创建和初始化

BMesh 类是整个 BMesh 数据结构的容器。可以通过多种方式创建 BMesh 对象：从现有的 Mesh 数据创建、创建新的空 BMesh、或从其他 BMesh 复制。BMesh 对象维护了顶点、边、面的集合以及它们之间的拓扑关系。

```python
import bpy
import bmesh
from mathutils import Vector

# 创建新的空 BMesh
bme = bmesh.new()
print(f"新 BMesh: 顶点数={len(bme.verts)}, 边数={len(bme.edges)}, 面数={len(bme.faces)}")

# 从现有 Mesh 创建 BMesh
obj = bpy.context.active_object
if obj and obj.type == 'MESH':
    mesh = obj.data
    bme = bmesh.new(mesh=mesh)
    print(f"\n从 Mesh 创建的 BMesh:")
    print(f"  顶点数: {len(bme.verts)}")
    print(f"  边数: {len(bme.edges)}")
    print(f"  面数: {len(bme.faces)}")

# 复制现有 BMesh
def duplicate_bmesh(source_bme):
    """复制 BMesh"""
    dest_bme = bmesh.new()
    dest_bme.verts.ensure_lookup_table()
    dest_bme.edges.ensure_lookup_table()
    dest_bme.faces.ensure_lookup_table()
    
    # 复制顶点
    vert_map = {}
    for v in source_bme.verts:
        vert_map[v] = dest_bme.verts.new(v.co)
    
    # 复制边
    edge_map = {}
    for e in source_bme.edges:
        v1, v2 = e.verts
        edge_map[e] = dest_bme.edges.new((vert_map[v1], vert_map[v2]))
    
    # 复制面
    for f in source_bme.faces:
        verts = [vert_map[v] for v in f.verts]
        dest_bme.faces.new(verts)
    
    return dest_bme

# 使用示例
if 'bme' in dir():
    bme_copy = duplicate_bmesh(bme)
    print(f"\n复制的 BMesh:")
    print(f"  顶点数: {len(bme_copy.verts)}")
```

### 顶点管理

BMesh 提供了丰富的顶点管理功能，包括创建、删除、查找和遍历顶点。每个顶点可以存储坐标、法线、选择状态、自定义层数据等信息。

```python
import bmesh

# 创建顶点
bme = bmesh.new()

# 单个创建
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))

print("创建的顶点:")
for v in bme.verts:
    print(f"  索引={v.index}, 坐标={v.co}, 选择={v.select}")

# 批量创建
coords = [
    (1.0, 1.0, 0.0),
    (1.0, 0.0, 1.0),
    (0.0, 1.0, 1.0)
]
verts = bme.verts.new(coords for coords in coords)
print(f"\n批量创建了 {len(verts)} 个顶点")

# 确保顶点索引表更新
bme.verts.ensure_lookup_table()
print("\n更新后的顶点索引:")
for v in bme.verts:
    print(f"  索引={v.index}")

# 按索引查找顶点
if len(bme.verts) > 0:
    v = bme.verts[0]
    print(f"\n索引 0 的顶点: {v.co}")

# 查找特定坐标的顶点
def find_vert_by_co(bme, target_co, tolerance=0.001):
    """按坐标查找顶点"""
    for v in bme.verts:
        if (v.co - target_co).length < tolerance:
            return v
    return None

target = Vector((0.0, 0.0, 0.0))
found = find_vert_by_co(bme, target)
print(f"\n查找坐标 {target}: {'找到' if found else '未找到'}")
```

### 边和面管理

边和面是构成网格拓扑结构的关键元素。BMesh 提供了创建、删除和管理边和面的完整功能。

```python
import bmesh
from mathutils import Vector

bme = bmesh.new()

# 创建顶点
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))
v4 = bme.verts.new((1.0, 1.0, 0.0))

# 创建边
e1 = bme.edges.new((v1, v2))
e2 = bme.edges.new((v2, v4))
e3 = bme.edges.new((v4, v3))
e4 = bme.edges.new((v3, v1))
e5 = bme.edges.new((v1, v4))  # 对角线

print("创建的边:")
for e in bme.edges:
    v1, v2 = e.verts
    print(f"  边 {e.index}: {v1.co} - {v2.co}, 选中={e.select}")

# 创建面
f1 = bme.faces.new((v1, v2, v4, v3))
print(f"\n创建的面:")
print(f"  面 {f1.index}: 顶点数={len(f1.verts)}, 边数={len(f1.edges)}")

# 遍历面的顶点和边
print("\n面的拓扑结构:")
for i, v in enumerate(f1.verts):
    print(f"  顶点 {i}: {v.co}")

for i, e in enumerate(f1.edges):
    print(f"  边 {i}: {e.index}")

# 检查边是否属于面
print(f"\n对角线边 {e5.index} 是否在面中: {e5 in f1.edges}")
```

### 自定义数据层

BMesh 支持自定义数据层，允许为顶点、边、面附加额外的数据。这些数据层可以用于存储自定义属性、UV 坐标、顶点颜色等。

```python
import bmesh

bme = bmesh.new()

# 创建一些顶点
for i in range(4):
    bme.verts.new((i, 0, 0))
bme.verts.ensure_lookup_table()

# 创建浮点类型的数据层
float_layer = bme.verts.layers.float.new("my_float_data")
print("创建浮点数据层:", float_layer.name)

# 写入数据
for i, v in enumerate(bme.verts):
    v[float_layer] = i * 0.5

# 读取数据
print("\n读取浮点数据:")
for v in bme.verts:
    print(f"  顶点 {v.index}: {v[float_layer]}")

# 创建整数类型的数据层
int_layer = bme.verts.layers.int.new("my_int_data")
print("\n创建整数数据层:", int_layer.name)

# 创建字符串类型的数据层（用于顶点组名称等）
string_layer = bme.verts.layers.string.new("my_string_data")
print("创建字符串数据层:", string_layer.name)

# 顶点颜色层
if len(bme.faces) > 0:
    col_layer = bme.faces.layers.color.new("my_vertex_color")
    print("\n创建顶点颜色层:", col_layer.name)
```

## BMVert 类

### 顶点属性

BMVert 类代表网格中的顶点，包含坐标、法线、选择状态等核心属性，以及访问相邻元素的方法。

```python
import bmesh
from mathutils import Vector

bme = bmesh.new()

# 创建四面体
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))
v4 = bme.verts.new((0.0, 0.0, 1.0))

# 创建面（底面和三个侧面）
bme.faces.new((v1, v2, v3))
bme.faces.new((v1, v3, v4))
bme.faces.new((v1, v4, v2))
bme.faces.new((v2, v4, v3))

print("顶点属性测试:")
for v in bme.verts:
    print(f"\n顶点 {v.index}:")
    print(f"  坐标: {v.co}")
    print(f"  索引: {v.index}")
    print(f"  选择状态: {v.select}")
    print(f"  隐藏状态: {v.hide}")

    # 计算法线
    if v.normal.length > 0:
        print(f"  法线: {v.normal}")
    else:
        print(f"  法线: (未计算)")

    # 访问相邻顶点
    neighbor_coords = [n.co for n in v.link_verts]
    print(f"  相邻顶点: {neighbor_coords}")

    # 访问相邻边
    edge_indices = [e.index for e in v.link_edges]
    print(f"  相邻边: {edge_indices}")

    # 访问相邻面
    face_indices = [f.index for f in v.link_faces]
    print(f"  相邻面: {face_indices}")
```

### 顶点操作方法

BMVert 提供了多种操作方法，包括坐标变换、选择操作、隐藏操作等。

```python
import bmesh
from mathutils import Vector, Euler

bme = bmesh.new()

# 创建多个顶点
for i in range(5):
    bme.verts.new((i, 0, 0))
bme.verts.ensure_lookup_table()

print("顶点操作测试:")

# 选择操作
for v in bme.verts:
    v.select = True
print("所有顶点已选中")

# 隐藏操作
bme.verts[0].hide = True
print("顶点 0 已隐藏")

# 坐标操作
v = bme.verts[1]
print(f"\n顶点 {v.index} 原始坐标: {v.co}")

# 移动顶点
v.co = Vector((5.0, 5.0, 5.0))
print(f"移动后坐标: {v.co}")

# 计算到原点的距离
distance = v.co.length
print(f"到原点的距离: {distance}")

# 计算到另一个顶点的距离
v1 = bme.verts[0]
v2 = bme.verts[2]
distance_to_v2 = (v1.co - v2.co).length
print(f"顶点 0 到顶点 2 的距离: {distance_to_v2}")

# 顶点链操作
print("\n顶点链:")
for i in range(min(3, len(bme.verts))):
    v = bme.verts[i]
    print(f"  顶点 {i}: 链接边数={len(v.link_edges)}, 链接面数={len(v.link_faces)}")
```

## BMEdge 类

### 边属性

BMEdge 类代表网格中的边，包含两个端点的引用、选择状态、硬度标记等属性。

```python
import bmesh

bme = bmesh.new()

# 创建网格
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((1.0, 1.0, 0.0))
v4 = bme.verts.new((0.0, 1.0, 0.0))

# 创建边
e1 = bme.edges.new((v1, v2))
e2 = bme.edges.new((v2, v3))
e3 = bme.edges.new((v3, v4))
e4 = bme.edges.new((v4, v1))
e5 = bme.edges.new((v1, v3))

# 创建面
bme.faces.new((v1, v2, v3, v4))

print("边属性测试:")
for e in bme.edges:
    v1, v2 = e.verts
    print(f"\n边 {e.index}:")
    print(f"  端点: {v1.co} - {v2.co}")
    print(f"  长度: {e.calc_length():.4f}")
    print(f"  选择状态: {e.select}")
    print(f"  硬度(用于平滑着色): {e.use_edge_sharp}")
    print(f"  循环 seam: {e.use_seam}")
    print(f"  可见性: {not e.hide}")

    # 访问相邻面
    face_indices = [f.index for f in e.link_faces]
    print(f"  相邻面: {face_indices}")
```

### 边操作方法

BMEdge 提供了计算长度、判断相邻、获取共享面等方法。

```python
import bmesh

bme = bmesh.new()

# 创建更复杂的网格
verts = []
for x in range(3):
    for y in range(3):
        verts.append(bme.verts.new((x, y, 0)))
bme.verts.ensure_lookup_table()

# 创建边
edges = []
for y in range(3):
    for x in range(2):
        i = y * 3 + x
        edges.append(bme.edges.new((verts[i], verts[i + 1])))
for x in range(3):
    for y in range(2):
        i = y * 3 + x
        edges.append(bme.edges.new((verts[i], verts[i + 3])))

# 创建面
for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme.faces.new((verts[i], verts[i + 1], verts[i + 4], verts[i + 3]))

print("边操作测试:")
for e in bme.edges:
    print(f"\n边 {e.index}:")
    print(f"  长度: {e.calc_length():.4f}")

    # 判断是否是边界边
    is_boundary = e.is_boundary
    print(f"  边界边: {is_boundary}")

    # 判断是否是manifold边
    is_manifold = e.is_manifold
    print(f"  流形边: {is_manifold}")

    # 判断是否是wire边
    is_wire = e.is_wire
    print(f"  Wire边: {is_wire}")

    # 获取两个共享顶点
    v1, v2 = e.verts
    print(f"  顶点: {v1.index}, {v2.index}")

    # 获取相邻面
    adjacent_faces = list(e.link_faces)
    print(f"  相邻面数: {len(adjacent_faces)}")
```

## BMFace 类

### 面属性

BMFace 类代表网格中的面，包含顶点引用、边引用、法线、材质索引等属性。

```python
import bmesh
from mathutils import Vector

bme = bmesh.new()

# 创建立方体的顶点
verts = [
    bme.verts.new((-1, -1, -1)),
    bme.verts.new((1, -1, -1)),
    bme.verts.new((1, 1, -1)),
    bme.verts.new((-1, 1, -1)),
    bme.verts.new((-1, -1, 1)),
    bme.verts.new((1, -1, 1)),
    bme.verts.new((1, 1, 1)),
    bme.verts.new((-1, 1, 1))
]
bme.verts.ensure_lookup_table()

# 定义六个面
face_indices = [
    (0, 1, 2, 3),  # 前面
    (5, 4, 7, 6),  # 后面
    (4, 0, 3, 7),  # 左面
    (1, 5, 6, 2),  # 右面
    (3, 2, 6, 7),  # 顶面
    (4, 5, 1, 0),  # 底面
]

faces = []
for indices in face_indices:
    face_verts = [verts[i] for i in indices]
    faces.append(bme.faces.new(face_verts))

print("面属性测试:")
for f in faces:
    print(f"\n面 {f.index}:")
    print(f"  顶点数: {len(f.verts)}")
    print(f"  边数: {len(f.edges)}")
    print(f"  选择状态: {f.select}")
    print(f"  隐藏状态: {f.hide}")

    # 计算面积
    area = f.calc_area()
    print(f"  面积: {area:.4f}")

    # 计算法线
    normal = f.normal
    print(f"  法线: {normal}")

    # 获取材质索引
    mat_index = f.material_index
    print(f"  材质索引: {mat_index}")

    # 访问顶点
    print(f"  顶点索引: {[v.index for v in f.verts]}")
```

### 面操作方法

BMFace 提供了计算面积、法线、循环等方法。

```python
import bmesh
from mathutils import Vector

bme = bmesh.new()

# 创建三角形网格
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((2.0, 0.0, 0.0))
v3 = bme.verts.new((1.0, 2.0, 0.0))

f = bme.faces.new((v1, v2, v3))

print("面操作测试:")
print(f"\n面 {f.index}:")

# 计算面积
area = f.calc_area()
print(f"  面积: {area:.4f}")

# 验证面积（底*高/2）
base = (v2.co - v1.co).length
height = 2.0
expected_area = base * height / 2
print(f"  期望面积: {expected_area:.4f}")

# 计算法线
normal = f.normal
print(f"  法线: {normal}")

# 获取循环
loops = list(f.loops)
print(f"  循环数: {len(loops)}")
for i, loop in enumerate(loops):
    print(f"    循环 {i}: 顶点={loop.vert.index}, 边={loop.edge.index}")

# 判断面是否平整
is_planar = f.is_planar()
print(f"  是否平整: {is_planar}")

# 获取面的中心
center = f.center_median
print(f"  中心点(中位数): {center}")

center_geometric = f.center()
print(f"  中心点(几何): {center_geometric}")
```

## BMLoop 类

### 循环结构

BMLoop 是 BMesh 中的循环结构，连接顶点、边和面。每个面由多个循环组成，每个循环对应面的一条边。

```python
import bmesh

bme = bmesh.new()

# 创建正方形
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((1.0, 1.0, 0.0))
v4 = bme.verts.new((0.0, 1.0, 0.0))

f = bme.faces.new((v1, v2, v3, v4))

print("循环结构测试:")
print(f"面 {f.index} 有 {len(f.loops)} 个循环")

for i, loop in enumerate(f.loops):
    print(f"\n循环 {i}:")
    print(f"  顶点: {loop.vert.co}")
    print(f"  边: {loop.edge.index}")
    print(f"  面: {loop.face.index}")

    # 获取相邻循环（同一面上的下一个和上一个）
    next_loop = loop.link_loop_next
    prev_loop = loop.link_loop_prev
    print(f"  下一个循环: {next_loop.vert.index}")
    print(f"  上一个循环: {prev_loop.vert.index}")

    # 获取相邻循环（通过边连接到相邻面）
    radial_next = loop.link_loop_radial_next
    print(f"  径向下一个: {radial_next.vert.index if radial_next else '无'}")
```

### 循环遍历

循环提供了在网格拓扑中导航的能力，可以方便地访问相邻元素。

```python
import bmesh

bme = bmesh.new()

# 创建更复杂的网格以便演示循环遍历
verts = []
for x in range(3):
    for y in range(3):
        verts.append(bme.verts.new((x, y, 0)))
bme.verts.ensure_lookup_table()

# 创建面
for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme.faces.new((verts[i], verts[i + 1], verts[i + 4], verts[i + 3]))

print("循环遍历测试:")

# 遍历所有面的循环
for f in bme.faces:
    print(f"\n面 {f.index} 的循环:")
    for loop in f.loops:
        print(f"  顶点 {loop.vert.index} -> ", end="")

    # 使用循环遍历整个面
    start_loop = f.loops[0]
    current_loop = start_loop
    print("\n  遍历整个面:")
    i = 0
    while True:
        print(f"    顶点 {current_loop.vert.index}")
        current_loop = current_loop.link_loop_next
        i += 1
        if current_loop == start_loop or i >= len(f.loops):
            break

# 使用循环访问相邻面
print("\n通过循环访问相邻面:")
for f in bme.faces:
    adjacent_faces = set()
    for loop in f.loops:
        # 通过边的另一个面找到相邻面
        edge = loop.edge
        for adjacent_loop in edge.link_loops:
            if adjacent_loop.face != f:
                adjacent_faces.add(adjacent_loop.face.index)

    print(f"  面 {f.index} 的相邻面: {sorted(adjacent_faces)}")
```

## 实践示例

### 网格分析工具

这个示例展示了如何使用 bmesh.types 进行网格分析。

```python
import bmesh

class MeshAnalyzer:
    def __init__(self, bme):
        self.bme = bme

    def analyze_mesh(self):
        """全面分析网格"""
        report = {
            'vertex_count': len(self.bme.verts),
            'edge_count': len(self.bme.edges),
            'face_count': len(self.bme.faces),
            'total_area': 0,
            'total_edges': 0,
            'boundary_edges': 0,
            'manifold_edges': 0,
            'wire_edges': 0,
            'avg_face_area': 0,
            'hidden_verts': 0,
            'hidden_edges': 0,
            'hidden_faces': 0
        }

        # 统计面积
        for f in self.bme.faces:
            report['total_area'] += f.calc_area()
            if f.hide:
                report['hidden_faces'] += 1

        # 计算平均面面积
        if report['face_count'] > 0:
            report['avg_face_area'] = report['total_area'] / report['face_count']

        # 统计边
        for e in self.bme.edges:
            report['total_edges'] += 1
            if e.hide:
                report['hidden_edges'] += 1
            if e.is_boundary():
                report['boundary_edges'] += 1
            if e.is_manifold():
                report['manifold_edges'] += 1
            if e.is_wire():
                report['wire_edges'] += 1

        # 统计顶点
        for v in self.bme.verts:
            if v.hide:
                report['hidden_verts'] += 1

        return report

    def get_topology_info(self):
        """获取拓扑信息"""
        info = {
            'vertex_connectivity': {},
            'edge_connectivity': {},
            'face_sizes': {}
        }

        # 顶点连接度
        for v in self.bme.verts:
            edge_count = len(v.link_edges)
            face_count = len(v.link_faces)
            info['vertex_connectivity'][edge_count] = info['vertex_connectivity'].get(edge_count, 0) + 1

        # 边连接度
        for e in self.bme.edges:
            face_count = len(list(e.link_faces))
            info['edge_connectivity'][face_count] = info['edge_connectivity'].get(face_count, 0) + 1

        # 面大小
        for f in self.bme.faces:
            size = len(f.verts)
            info['face_sizes'][size] = info['face_sizes'].get(size, 0) + 1

        return info

    def print_report(self):
        """打印分析报告"""
        report = self.analyze_mesh()
        topology = self.get_topology_info()

        print("=" * 50)
        print("网格分析报告")
        print("=" * 50)

        print(f"\n基本统计:")
        print(f"  顶点数: {report['vertex_count']}")
        print(f"  边数: {report['edge_count']}")
        print(f"  面数: {report['face_count']}")

        print(f"\n几何属性:")
        print(f"  总面积: {report['total_area']:.4f}")
        print(f"  平均面面积: {report['avg_face_area']:.4f}")

        print(f"\n边类型统计:")
        print(f"  边界边: {report['boundary_edges']}")
        print(f"  流形边: {report['manifold_edges']}")
        print(f"  Wire边: {report['wire_edges']}")

        print(f"\n隐藏元素:")
        print(f"  隐藏顶点: {report['hidden_verts']}")
        print(f"  隐藏边: {report['hidden_edges']}")
        print(f"  隐藏面: {report['hidden_faces']}")

        print(f"\n拓扑信息:")
        print(f"  顶点连接度: {topology['vertex_connectivity']}")
        print(f"  边连接度: {topology['edge_connectivity']}")
        print(f"  面大小分布: {topology['face_sizes']}")

# 使用示例
bme = bmesh.new()
for x in range(3):
    for y in range(3):
        bme.verts.new((x, y, 0))
bme.verts.ensure_lookup_table()

for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme.faces.new((bme.verts[i], bme.verts[i + 1], bme.verts[i + 4], bme.verts[i + 3]))

analyzer = MeshAnalyzer(bme)
analyzer.print_report()
```

### 网格验证工具

这个示例展示了如何使用 bmesh.types 验证网格的有效性。

```python
import bmesh
from mathutils import Vector

class MeshValidator:
    def __init__(self, bme):
        self.bme = bme
        self.errors = []
        self.warnings = []
        self.info = []

    def validate(self):
        """验证网格的有效性"""
        self.errors = []
        self.warnings = []
        self.info = []

        self._check_vertices()
        self._check_edges()
        self._check_faces()
        self._check_manifold()
        self._check_geometry()

        return len(self.errors) == 0

    def _check_vertices(self):
        """检查顶点"""
        for v in self.bme.verts:
            # 检查孤立顶点
            if len(v.link_edges) == 0 and len(v.link_faces) == 0:
                self.warnings.append(f"顶点 {v.index} 是孤立的")

            # 检查重复坐标
            for other in self.bme.verts:
                if other != v and other != v:
                    if (v.co - other.co).length < 0.0001:
                        self.errors.append(f"顶点 {v.index} 和 {other.index} 坐标相同")
                        break

    def _check_edges(self):
        """检查边"""
        for e in self.bme.edges:
            # 检查零长度边
            if e.calc_length() < 0.0001:
                self.errors.append(f"边 {e.index} 长度为零")

            # 检查退化面
            for f in e.link_faces:
                if len(f.verts) < 3:
                    self.errors.append(f"面 {f.index} 顶点数不足")

    def _check_faces(self):
        """检查面"""
        for f in self.bme.faces:
            # 检查退化面
            if f.calc_area() < 0.0001:
                self.warnings.append(f"面 {f.index} 面积为零")

            # 检查非平面面
            if not f.is_planar():
                self.warnings.append(f"面 {f.index} 不是平整的")

    def _check_manifold(self):
        """检查流形"""
        boundary_count = 0
        non_manifold_count = 0

        for e in self.bme.edges:
            if e.is_boundary():
                boundary_count += 1
            elif not e.is_manifold():
                non_manifold_count += 1

        if boundary_count > 0:
            self.info.append(f"网格有 {boundary_count} 条边界边（边缘未封闭）")

        if non_manifold_count > 0:
            self.errors.append(f"网格有 {non_manifold_count} 条非流形边")

    def _check_geometry(self):
        """检查几何形状"""
        total_area = sum(f.calc_area() for f in self.bme.faces)
        if total_area == 0:
            self.warnings.append("网格总面积为零")

        # 检查法线方向
        normals = [f.normal for f in self.bme.faces]
        if normals:
            avg_normal = sum(normals, Vector((0, 0, 0))) / len(normals)
            if avg_normal.length < 0.001:
                self.warnings.append("大部分面法线方向不一致")

    def print_results(self):
        """打印验证结果"""
        is_valid = self.validate()

        print("=" * 50)
        print("网格验证结果")
        print("=" * 50)

        print(f"\n有效性: {'通过' if is_valid else '失败'}")

        if self.errors:
            print(f"\n错误 ({len(self.errors)}):")
            for error in self.errors:
                print(f"  - {error}")

        if self.warnings:
            print(f"\n警告 ({len(self.warnings)}):")
            for warning in self.warnings:
                print(f"  - {warning}")

        if self.info:
            print(f"\n信息 ({len(self.info)}):")
            for info in self.info:
                print(f"  - {info}")

        return is_valid

# 使用示例
bme = bmesh.new()
for x in range(3):
    for y in range(3):
        bme.verts.new((x, y, 0))
bme.verts.ensure_lookup_table()
for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme.faces.new((bme.verts[i], bme.verts[i + 1], bme.verts[i + 4], bme.verts[i + 3]))

validator = MeshValidator(bme)
validator.print_results()
```

## 注意事项

### 内存管理

BMesh 对象在使用完毕后应该调用 free 方法释放内存。这对于在循环中创建大量临时 BMesh 的场景尤其重要，可以避免内存泄漏。

### 索引维护

在对 BMesh 进行结构性修改（如添加、删除元素）后，应该调用 ensure_lookup_table 方法来更新索引查找表。在使用索引直接访问元素之前，务必确保索引表已更新。

### 拓扑关系访问

通过 link_verts、link_edges、link_faces 等属性访问相邻元素时，返回的是迭代器。如果需要多次访问同一组元素，应该先转换为列表以避免重复遍历。

---
## bmesh_types_detailed

# bmesh.types - BMesh 数据类型参考

BMesh 是 Blender 的底层网格建模系统，提供比传统网格数据访问更强大和灵活的网格几何操作。

## Overview

`bmesh.types` 模块定义了 BMesh 中所有的数据类型，包括 BMesh 本身、顶点、边、面等核心元素。这些类型提供了对网格几何的低级别访问。

## 核心数据类型

### BMesh - 网格对象

```python
import bpy
import bmesh

# 创建 BMesh 对象
bm = bmesh.new()

# 从现有网格创建 BMesh
mesh = bpy.context.active_object.data
bm = bmesh.new()
bm.from_mesh(mesh)

# 从对象创建（包含变换）
bm = bmesh.new()
bm.from_object(bpy.context.active_object, bpy.context.evaluated_depsgraph_get())

# 操作完成后写回网格
bm.to_mesh(mesh)
bm.free()

# 使用 with 语句
with bmesh.new() as bm:
    # 在这里操作 bm
    bmesh.ops.create_cube(bm, size=1.0)
    mesh = bpy.context.active_object.data
    bm.to_mesh(mesh)
```

### BMesh 基础属性

```python
# 顶点
print(f"顶点数: {len(bm.vertices)}")

# 边
print(f"边数: {len(bm.edges)}")

# 面
print(f"面数: {len(bm.faces)}")

# 循环（边角点）
print(f"循环数: {len(bm.loops)}")

# 层数据
print(f"顶点层: {list(bm.verts.layers.deform.keys())}")
print(f"颜色层: {list(bm.verts.layers.color.keys())}")
```

## 顶点 (BMVert)

```python
import bmesh

# 创建顶点
v1 = bm.verts.new((0.0, 0.0, 0.0))
v2 = bm.verts.new((1.0, 0.0, 0.0))
v3 = bm.verts.new((1.0, 1.0, 0.0))
v4 = bm.verts.new((0.0, 1.0, 0.0))

# 创建后需要更新
bm.verts.ensure_lookup_table()

# 顶点属性
print(f"顶点索引: {v1.index}")
print(f"顶点坐标: {v1.co}")           # Vector
print(f"顶点法线: {v1.normal}")       # Vector
print(f"所属边: {v1.link_edges}")     # 边列表
print(f"所属面: {v1.link_faces}")     # 面列表

# 修改坐标
v1.co.x = 2.0
v1.co.y = 3.0
v1.co.z = 4.0

# 顶点选择状态
v1.select = True
print(f"是否选中: {v1.select}")

# 隐藏顶点
v1.hide = True
print(f"是否隐藏: {v1.hide}")

# 顶点排序
bm.verts.sort()

# 批量创建
verts = [bm.verts.new((x, 0, 0)) for x in range(5)]
bm.verts.ensure_lookup_table()

# 查找顶点
vertex = bm.verts.get((1.0, 0.0, 0.0))
if vertex:
    print(f"找到顶点: {vertex.index}")

# 遍历所有顶点
for v in bm.verts:
    print(f"顶点 {v.index}: {v.co}")
```

## 边 (BMEdge)

```python
import bmesh

# 创建边（需要两个顶点）
e1 = bm.edges.new((v1, v2))
e2 = bm.edges.new((v2, v3))
e3 = bm.edges.new((v3, v4))
e4 = bm.edges.new((v4, v1))
e5 = bm.edges.new((v1, v3))  # 对角线

# 使用现有顶点创建边
e6 = bm.edges.get((v1, v2))

# 边属性
print(f"边索引: {e1.index}")
print(f"起点: {e1.verts[0]}")  # BMVert
print(f"终点: {e1.verts[1]}")  # BMVert
print(f"长度: {e1.calc_length()}")  # float
print(f"所属面: {e1.link_faces}")  # 面列表

# 边选择状态
e1.select = True
print(f"是否选中: {e1.select}")

# 边折缝（用于权重/几何折缝）
e1.seam = True
print(f"是否接缝: {e1.seam}")

# 边粗糙（用于法线计算）
e1.crease = 0.5
print(f"粗糙度: {e1.crease}")

# 边类型检查
print(f"是否为边界边: {e1.is_boundary}")
print(f"是否为 Wire 边: {e1.is_wire}")
print(f"是否为 manifold 边: {e1.is_manifold}")

# 遍历边
for e in bm.edges:
    print(f"边 {e.index}: 长度 = {e.calc_length():.3f}")
```

## 面 (BMFace)

```python
import bmesh

# 创建面（需要3-4个顶点）
f1 = bm.faces.new((v1, v2, v3, v4))

# 面属性
print(f"面索引: {f1.index}")
print(f"顶点数: {len(f1.verts)}")
print(f"边数: {len(f1.edges)}")
print(f"法线: {f1.normal}")           # Vector
print(f"面积: {f1.calc_area()}")       # float
print(f"周长: {f1.calc_perimeter()}")  # float
print(f"中心: {f1.calc_center_median()}")  # Vector
print(f"几何中心: {f1.calc_center_bounds()}")  # Vector

# 获取顶点和边
for v in f1.verts:
    print(f"  顶点: {v.co}")

for e in f1.edges:
    print(f"  边: {e.index}")

# 循环（顶点-边对）
for loop in f1.loops:
    print(f"循环: vert={loop.vert.index}, edge={loop.edge.index}")
    print(f"  UV: {loop[bm.verts.layers.uv.active]}")
    print(f"  颜色: {loop[bm.verts.layers.color.active]}")

# 面选择状态
f1.select = True
print(f"是否选中: {f1.select}")

# 面隐藏
f1.hide = True
print(f"是否隐藏: {f1.hide}")

# 面填充
f1.material_index = 0

# 面法线方向
f1.flip()
print(f"翻转后面法线: {f1.normal}")

# 检查面类型
print(f"是否为三角面: {len(f1.verts) == 3}")
print(f"是否为四边面: {len(f1.verts) == 4}")
print(f"是否为多边面: {len(f1.verts) > 4}")

# 遍历所有面
for f in bm.faces:
    print(f"面 {f.index}: 面积 = {f.calc_area():.3f}")
```

## 循环 (BMLoop)

```python
import bmesh

# 循环是连接顶点、边和面的特殊对象
# 每个面有 N 个循环，每个循环包含一个顶点和一条边

for f in bm.faces:
    for loop in f.loops:
        # 循环属性
        print(f"顶点: {loop.vert}")
        print(f"边: {loop.edge}")
        print(f"邻接面: {loop.link_loop_next}")
        print(f"邻接面: {loop.link_loop_prev}")
        
        # 访问层数据
        if bm.verts.layers.uv.active:
            uv_layer = bm.verts.layers.uv.active
            print(f"UV: {loop[uv_layer].uv}")
        
        if bm.verts.layers.color.active:
            color_layer = bm.verts.layers.color.active
            print(f"颜色: {loop[color_layer].color}")
```

## 层数据 (BMLayer)

### 顶点组层（变形层）

```python
import bmesh

# 创建变形层
deform_layer = bm.verts.layers.deform.new("Weights")

# 访问变形数据
v = bm.verts[0]
v_deform = v[deform_layer]

# 设置权重
v_deform[0] = 1.0  # 顶点组索引 0 的权重
v_deform[1] = 0.5  # 顶点组索引 1 的权重

# 读取权重
weight_0 = v_deform.get(0, 0.0)

# 遍历所有变形组
for group_index, weight in v_deform.items():
    print(f"组 {group_index}: 权重 {weight}")
```

### UV 层

```python
import bmesh

# 创建 UV 层
uv_layer = bm.verts.layers.uv.new("UVMap")

# 访问 UV 数据
for f in bm.faces:
    for loop in f.loops:
        uv = loop[uv_layer]
        print(f"UV: {uv.uv}")
        
        # 设置 UV
        uv.uv = (loop.vert.co.x, loop.vert.co.y)
```

### 顶点颜色层

```python
import bmesh

# 创建顶点颜色层
color_layer = bm.verts.layers.color.new("Col")

# 访问颜色数据
for f in bm.faces:
    for loop in f.loops:
        color = loop[color_layer]
        print(f"颜色: {color}")
        
        # 设置颜色
        color.color = (1.0, 0.5, 0.2, 1.0)
```

## 顶点索引层

```python
import bmesh

# 创建自定义整数层
custom_int_layer = bm.verts.layers.int.new("CustomInt")

# 设置和获取
v = bm.verts[0]
v[custom_int_layer] = 42

value = v[custom_int_layer]
print(f"自定义整数: {value}")

# 浮点层
float_layer = bm.verts.layers.float.new("CustomFloat")
v[float_layer] = 3.14

# 字符串层
string_layer = bm.verts.layers.string.new("CustomString")
v[string_layer] = "vertex_data"
```

## 常用操作模式

### 遍历和过滤

```python
# 遍历所有顶点
for v in bm.verts:
    print(f"顶点 {v.index}: {v.co}")

# 遍历选中的顶点
selected_verts = [v for v in bm.verts if v.select]
print(f"选中顶点数: {len(selected_verts)}")

# 遍历边界顶点
boundary_verts = [v for v in bm.verts if any(e.is_boundary for e in v.link_edges)]
print(f"边界顶点数: {len(boundary_verts)}")

# 遍历孤立顶点
isolated_verts = [v for v in bm.verts if len(v.link_edges) == 0]
print(f"孤立顶点数: {len(isolated_verts)}")
```

### 创建标准几何体

```python
import bmesh
import math

def create_icosphere(bm, subdivisions=2, radius=1.0):
    """创建二十面体球体"""
    t = (1.0 + math.sqrt(5.0)) / 2.0
    
    verts = [
        bm.verts.new((-1, t, 0)),
        bm.verts.new((1, t, 0)),
        bm.verts.new((-1, -t, 0)),
        bm.verts.new((1, -t, 0)),
        bm.verts.new((0, -1, t)),
        bm.verts.new((0, 1, t)),
        bm.verts.new((0, -1, -t)),
        bm.verts.new((0, 1, -t)),
        bm.verts.new((t, 0, -1)),
        bm.verts.new((t, 0, 1)),
        bm.verts.new((-t, 0, -1)),
        bm.verts.new((-t, 0, 1)),
    ]
    
    bm.verts.ensure_lookup_table()
    
    faces = [
        (0, 11, 5), (0, 5, 1), (0, 1, 7), (0, 7, 10), (0, 10, 11),
        (1, 5, 9), (5, 11, 4), (11, 10, 2), (10, 7, 6), (7, 1, 8),
        (3, 9, 4), (3, 4, 2), (3, 2, 6), (3, 6, 8), (3, 8, 9),
        (4, 9, 5), (2, 4, 11), (6, 2, 10), (8, 6, 7), (9, 8, 1),
    ]
    
    for f in faces:
        bm.faces.new(tuple(verts[i] for i in f))
    
    # 缩放到半径
    for v in bm.verts:
        v.co = v.co.normalized() * radius
    
    return bm

# 使用
with bmesh.new() as bm:
    create_icosphere(bm, subdivisions=2, radius=1.0)
```

### 数据转换

```python
import bmesh
import numpy as np

def get_vertex_data(bm):
    """提取顶点数据为 numpy 数组"""
    coords = np.array([v.co.to_tuple() for v in bm.verts])
    normals = np.array([v.normal.to_tuple() for v in bm.verts])
    return coords, normals

def set_vertex_colors_from_height(bm, min_height=0, max_height=2):
    """根据高度设置顶点颜色"""
    color_layer = bm.verts.layers.color.new("HeightColor")
    
    heights = [v.co.z for v in bm.verts]
    min_h = min(heights)
    max_h = max(heights)
    
    for f in bm.faces:
        for loop in f.loops:
            h = loop.vert.co.z
            t = (h - min_h) / (max_h - min_h) if max_h > min_h else 0.5
            
            # 渐变颜色：蓝 -> 绿 -> 红
            if t < 0.5:
                r = t * 2
                g = 1 - r
                b = 0
            else:
                r = 1
                g = 1 - (t - 0.5) * 2
                b = 0
            
            loop[color_layer].color = (r, g, b, 1.0)
```

## 性能优化

### 1. 批量操作

```python
# 不推荐：逐个操作
for v in bm.verts:
    v.co.x *= 2

# 推荐：使用 bmesh.ops
bmesh.ops.scale(bm, vec=(2, 1, 1), verts=bm.verts)
```

### 2. 避免频繁的索引查找

```python
# 不推荐
for i in range(len(bm.verts)):
    v = bm.verts[i]
    
# 推荐
bm.verts.ensure_lookup_table()
for v in bm.verts:
    pass
```

### 3. 及时清理

```python
# 删除孤立顶点
bmesh.ops.remove_doubles(bm, verts=bm.verts, dist=0.0)

# 删除非流形几何
bmesh.ops.dissolve_degenerate(bm, edges=bm.edges, dist=0.001)
```

## 与传统网格的互操作

```python
import bmesh

# 从网格创建 BMesh
mesh = bpy.context.active_object.data
bm = bmesh.new()
bm.from_mesh(mesh)

# 操作 BMesh
bmesh.ops.subdivide_edges(bm, edges=bm.edges[:10], cuts=2)

# 写回网格
bm.to_mesh(mesh)
bm.free()

# 更新对象
mesh.update()

# 从 BMesh 创建新网格
new_mesh = bpy.data.meshes.new("NewMesh")
bm = bmesh.new()
bmesh.ops.create_cube(bm, size=1.0)
bm.to_mesh(new_mesh)
bm.free()

# 创建新对象
new_obj = bpy.data.objects.new("NewObject", new_mesh)
bpy.context.collection.objects.link(new_obj)
```

## 常见问题

### Q: 顶点坐标不更新
A: 确保操作后调用 `bm.verts.ensure_lookup_table()` 更新索引表。

### Q: 修改不生效
A: 检查是否调用 `bm.to_mesh(mesh)` 将更改写回网格。

### Q: 内存泄漏
A: 总是使用 `bm.free()` 或 `with bmesh.new() as bm:` 确保释放 BMesh 对象。

## 相关模块

- [bmesh.ops](bmesh_ops_module.md) - BMesh 操作
- [bmesh.utils](bmesh_utils_module.md) - BMesh 工具函数
- [bmesh.geometry](bmesh_geometry_module.md) - BMesh 几何计算
- [mathutils](mathutils_module.md) - 数学工具


---
## bmesh_utils

# bmesh.utils 模块

## 模块概述

bmesh.utils 模块提供了一系列用于 BMesh 数据结构的实用工具函数。这些函数补充了 bmesh.ops 和 bmesh.types 的功能，提供了网格验证、法线计算、边方向判断等辅助功能。虽然这些函数不像 bmesh.ops 中的操作那样直接修改网格，但它们在网格处理流程中扮演着重要的角色。

该模块中的函数可以分为几个主要类别：法线计算函数用于获取和设置元素的方向信息，网格验证函数用于检查网格的有效性，几何计算函数用于测量距离和角度，拓扑查询函数用于获取网格的结构信息。这些函数的共同特点是它们都是纯函数，不会修改传入的网格数据。

理解 bmesh.utils 中的工具函数对于编写健壮的网格处理脚本至关重要。这些函数可以帮助检测潜在的网格问题、计算必要的数据用于后续操作、以及验证操作结果的正确性。在复杂的建模流程中，合理使用这些工具函数可以显著提高代码的可靠性和可维护性。

## 法线计算函数

### 计算顶点法线

calc_tangent_edge 函数计算与顶点相连的边的切线方向。这个函数在需要计算顶点切线用于纹理映射或光照计算时非常有用。

```python
import bmesh
from bmesh.utils import calc_tangent_edge
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))
f = bme.faces.new((v1, v2, v3))

print("计算顶点切线测试:")
print(f"面 {f.index} 的法线: {f.normal}")

for v in bme.verts:
    print(f"\n顶点 {v.index}:")
    print(f"  坐标: {v.co}")
    print(f"  法线: {v.normal}")

    # 计算关联边的切线
    for e in v.link_edges:
        tangent = calc_tangent_edge(e, v)
        if tangent:
            v1, v2 = e.verts
            print(f"  边 {e.index} ({v1.co} - {v2.co}) 的切线: {tangent}")
```

### 计算循环切线

calc_tangent_loop 函数计算循环的切线方向。循环是 BMesh 中的基本拓扑结构之一，每个循环对应面的一条边。

```python
import bmesh
from bmesh.utils import calc_tangent_loop, calc_tangent_edge
from mathutils import Vector

# 创建更复杂的测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((1.0, 1.0, 0.0))
v4 = bme.verts.new((0.0, 1.0, 0.0))
f = bme.faces.new((v1, v2, v3, v4))

print("计算循环切线测试:")
print(f"面 {f.index} 的法线: {f.normal}")

# 遍历所有循环
for loop in f.loops:
    print(f"\n循环:")
    print(f"  顶点: {loop.vert.co}")
    print(f"  边: {loop.edge.index}")

    # 计算循环切线
    tangent = calc_tangent_loop(loop)
    print(f"  切线: {tangent}")

    # 计算边的切线
    edge_tangent = calc_tangent_edge(loop.edge, loop.vert)
    print(f"  边切线: {edge_tangent}")
```

### 边切线计算

calc_tangent_edge 函数计算特定边在某个顶点处的切线向量。这个函数考虑了边在面中的位置和方向，返回一个单位向量表示切线方向。

```python
import bmesh
from bmesh.utils import calc_tangent_edge
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
# 创建立方体
verts = []
for x in [-1, 1]:
    for y in [-1, 1]:
        for z in [-1, 1]:
            verts.append(bme.verts.new((x, y, z)))
bme.verts.ensure_lookup_table()

face_verts = [
    (0, 1, 3, 2),  # 前面
    (5, 4, 7, 6),  # 后面
    (4, 0, 3, 7),  # 左面
    (1, 5, 6, 2),  # 右面
    (3, 2, 6, 7),  # 顶面
    (4, 5, 1, 0),  # 底面
]
for fv in face_verts:
    bme.faces.new([verts[i] for i in fv])

print("边切线计算测试:")
print(f"顶面法线: {bme.faces[4].normal}")

# 分析顶面的四条边
top_face = bme.faces[4]
for loop in top_face.loops:
    edge = loop.edge
    tangent = calc_tangent_edge(edge, loop.vert)
    print(f"\n边 {edge.index} 在顶点 {loop.vert.index}:")
    print(f"  边的另一个顶点: {edge.other_vert(loop.vert).co}")
    print(f"  切线: {tangent}")
```

## 边方向函数

### 边方向判断

edge_direction 函数返回边的方向向量，从第一个顶点指向第二个顶点。这个函数在需要确定边的方向用于后续操作时非常有用。

```python
import bmesh
from bmesh.utils import edge_direction
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((2.0, 0.0, 0.0))
e = bme.edges.new((v1, v2))

print("边方向测试:")
print(f"顶点1: {v1.co}")
print(f"顶点2: {v2.co}")

# 获取边的方向
direction = edge_direction(e)
print(f"边方向: {direction}")

# 验证方向
expected = v2.co - v1.co
print(f"期望方向: {expected}")
print(f"方向正确: {direction == expected}")

# 测试多条边
print("\n多条边方向测试:")
for i in range(4):
    v1 = bme.verts.new((i, 0, 0))
    v2 = bme.verts.new((i + 1, 0, 0))
    e = bme.edges.new((v1, v2))
    direction = edge_direction(e)
    print(f"  边 {len(bme.edges) - 1}: 方向={direction}")
```

### 共享顶点查找

edge_share_vert 函数检查两条边是否共享顶点，并返回共享的顶点（如果有）。

```python
import bmesh
from bmesh.utils import edge_share_vert

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((1.0, 1.0, 0.0))
v4 = bme.verts.new((0.0, 1.0, 0.0))

e1 = bme.edges.new((v1, v2))
e2 = bme.edges.new((v2, v3))
e3 = bme.edges.new((v1, v3))  # 对角线，不共享顶点

print("边共享顶点测试:")
print(f"边1: {v1.co} - {v2.co}")
print(f"边2: {v2.co} - {v3.co}")
print(f"边3: {v1.co} - {v3.co}")

# 检查边1和边2
shared = edge_share_vert(e1, e2)
print(f"\n边1和边2共享顶点: {shared}")
if shared:
    print(f"  共享顶点: {shared.co}")

# 检查边1和边3
shared = edge_share_vert(e1, e3)
print(f"\n边1和边3共享顶点: {shared}")

# 检查边2和边3
shared = edge_share_vert(e2, e3)
print(f"\n边2和边3共享顶点: {shared}")
if shared:
    print(f"  共享顶点: {shared.co}")
```

## 网格验证函数

### 网格有效性检查

mesh_linked_submesh_check 函数检查网格的连通性，判断网格是否由多个分离的部分组成。

```python
import bmesh
from bmesh.utils import mesh_linked_submesh_check

# 创建分离的网格
bme = bmesh.new()

# 第一个三角形
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.5, 1.0, 0.0))
bme.faces.new((v1, v2, v3))

# 第二个三角形（分离）
v4 = bme.verts.new((5.0, 0.0, 0.0))
v5 = bme.verts.new((6.0, 0.0, 0.0))
v6 = bme.verts.new((5.5, 1.0, 0.0))
bme.faces.new((v4, v5, v6))

print("网格连通性测试:")
print(f"面数: {len(bme.faces)}")
print(f"顶点数: {len(bme.verts)}")

# 检查连通性
is_connected = mesh_linked_submesh_check(bme)
print(f"网格是否连通: {is_connected}")

# 创建连通的网格
bme2 = bmesh.new()
for x in range(3):
    for y in range(3):
        bme2.verts.new((x, y, 0))
bme2.verts.ensure_lookup_table()

for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme2.faces.new([
            bme2.verts[i],
            bme2.verts[i + 1],
            bme2.verts[i + 4],
            bme2.verts[i + 3]
        ])

print("\n连通网格测试:")
print(f"面数: {len(bme2.faces)}")
is_connected = mesh_linked_submesh_check(bme2)
print(f"网格是否连通: {is_connected}")
```

### 顶点有效性检查

vert_separate_by_angle 函数根据角度阈值分离顶点。这个函数常用于复制硬边处的顶点，使它们不再共享。

```python
import bmesh
from bmesh.utils import vert_separate_by_angle

# 创建测试网格（带硬边）
bme = bmesh.new()
# 创建立方体
verts = []
for x in [-1, 1]:
    for y in [-1, 1]:
        for z in [-1, 1]:
            verts.append(bme.verts.new((x, y, z)))
bme.verts.ensure_lookup_table()

face_verts = [
    (0, 1, 3, 2),
    (5, 4, 7, 6),
    (4, 0, 3, 7),
    (1, 5, 6, 2),
    (3, 2, 6, 7),
    (4, 5, 1, 0),
]
for fv in face_verts:
    bme.faces.new([verts[i] for i in fv])

print("顶点分离测试:")
print(f"操作前顶点数: {len(bme.verts)}")

# 选择一个顶点
v = bme.verts[0]
print(f"顶点 {v.index} 的相邻面数: {len(v.link_faces)}")

# 计算相邻面之间的角度
adjacent_normals = []
for f in v.link_faces:
    adjacent_normals.append(f.normal)

print(f"相邻面法线: {[n for n in adjacent_normals]}")

# 分离顶点（基于角度）
result = vert_separate_by_angle(bme, verts=[v], angle=1.0)
print(f"分离后顶点数: {len(bme.verts)}")

# 检查新创建的顶点
new_verts = result['verts']
print(f"新创建的顶点数: {len(new_verts)}")
```

## 几何计算函数

### 边长度计算

edge_calc_length 函数计算边的长度。这个函数提供了直接计算边长度的能力，与通过计算两个端点距离的方式等效。

```python
import bmesh
from bmesh.utils import edge_calc_length

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((3.0, 4.0, 0.0))
e = bme.edges.new((v1, v2))

print("边长度计算测试:")
print(f"顶点1: {v1.co}")
print(f"顶点2: {v2.co}")

# 计算边长度
length = edge_calc_length(e)
print(f"边长度: {length}")

# 验证（3-4-5 三角形）
expected_length = 5.0
print(f"期望长度: {expected_length}")
print(f"计算正确: {abs(length - expected_length) < 0.0001}")

# 测试多条边
print("\n多条边长度测试:")
edges = []
for i in range(5):
    x = i * 2
    v1 = bme.verts.new((x, 0, 0))
    v2 = bme.verts.new((x + 1, 0, 0))
    edges.append(bme.edges.new((v1, v2)))

for i, e in enumerate(edges):
    length = edge_calc_length(e)
    print(f"  边 {i}: 长度={length}")
```

### 距离计算

vertex_distance_to_plane 函数计算顶点到平面的距离。这个函数在需要判断顶点相对于某个平面的位置时非常有用。

```python
import bmesh
from bmesh.utils import vertex_distance_to_plane
from mathutils import Vector

# 创建测试网格
bme = bmesh.new()
v1 = bme.verts.new((0.0, 0.0, 0.0))
v2 = bme.verts.new((1.0, 0.0, 0.0))
v3 = bme.verts.new((0.0, 1.0, 0.0))
bme.faces.new((v1, v2, v3))

# 定义平面（使用面的位置和法线）
f = bme.faces[0]
plane_co = f.center_median
plane_no = f.normal

print("顶点到平面距离测试:")
print(f"平面中心: {plane_co}")
print(f"平面法线: {plane_no}")

for v in bme.verts:
    distance = vertex_distance_to_plane(v, plane_co, plane_no)
    print(f"\n顶点 {v.index}:")
    print(f"  坐标: {v.co}")
    print(f"  到平面距离: {distance:.4f}")

    # 验证：面上的顶点距离应该接近0
    if v in f.verts:
        print(f"  验证: 顶点在面上，距离应接近0")
```

## 实践示例

### 网格分析器

这个示例展示了如何使用 bmesh.utils 中的函数创建网格分析工具。

```python
import bmesh
from bmesh.utils import (
    edge_calc_length, edge_direction, edge_share_vert,
    calc_tangent_edge, calc_tangent_loop
)
from mathutils import Vector

class MeshAnalyzer:
    def __init__(self, bme):
        self.bme = bme

    def analyze_edges(self):
        """分析所有边"""
        analysis = {
            'total_edges': len(self.bme.edges),
            'avg_length': 0,
            'min_length': float('inf'),
            'max_length': 0,
            'direction_stats': {'X': 0, 'Y': 0, 'Z': 0, 'DIAGONAL': 0},
            'shared_vert_stats': {0: 0, 1: 0, 2: 0}
        }

        total_length = 0
        for e in self.bme.edges:
            # 长度分析
            length = edge_calc_length(e)
            total_length += length
            analysis['min_length'] = min(analysis['min_length'], length)
            analysis['max_length'] = max(analysis['max_length'], length)

            # 方向分析
            direction = edge_direction(e)
            abs_x, abs_y, abs_z = abs(direction.x), abs(direction.y), abs(direction.z)
            if abs_x > abs_y and abs_x > abs_z:
                analysis['direction_stats']['X'] += 1
            elif abs_y > abs_x and abs_y > abs_z:
                analysis['direction_stats']['Y'] += 1
            elif abs_z > abs_x and abs_z > abs_y:
                analysis['direction_stats']['Z'] += 1
            else:
                analysis['direction_stats']['DIAGONAL'] += 1

        analysis['avg_length'] = total_length / max(1, len(self.bme.edges))

        return analysis

    def analyze_faces(self):
        """分析所有面"""
        analysis = {
            'total_faces': len(self.bme.faces),
            'face_sizes': {},
            'normal_directions': {'X': 0, 'Y': 0, 'Z': 0},
            'total_area': 0
        }

        for f in self.bme.faces:
            # 面大小分析
            size = len(f.verts)
            analysis['face_sizes'][size] = analysis['face_sizes'].get(size, 0) + 1

            # 法线方向分析
            normal = f.normal
            abs_x, abs_y, abs_z = abs(normal.x), abs(normal.y), abs(normal.z)
            if abs_x > abs_y and abs_x > abs_z:
                analysis['normal_directions']['X'] += 1
            elif abs_y > abs_x and abs_y > abs_z:
                analysis['normal_directions']['Y'] += 1
            else:
                analysis['normal_directions']['Z'] += 1

            # 面积分析
            analysis['total_area'] += f.calc_area()

        return analysis

    def analyze_topology(self):
        """分析拓扑结构"""
        analysis = {
            'vertex_connections': {},
            'edge_connections': {},
            'is_manifold': True,
            'boundary_edges': 0
        }

        for v in self.bme.verts:
            conn = len(v.link_edges)
            analysis['vertex_connections'][conn] = analysis['vertex_connections'].get(conn, 0) + 1

        for e in self.bme.edges:
            conn = len(list(e.link_faces))
            analysis['edge_connections'][conn] = analysis['edge_connections'].get(conn, 0) + 1
            if e.is_boundary():
                analysis['boundary_edges'] += 1
                analysis['is_manifold'] = False

        return analysis

    def full_report(self):
        """生成完整报告"""
        edge_analysis = self.analyze_edges()
        face_analysis = self.analyze_faces()
        topology_analysis = self.analyze_topology()

        print("=" * 60)
        print("网格分析报告")
        print("=" * 60)

        print("\n边分析:")
        print(f"  总边数: {edge_analysis['total_edges']}")
        print(f"  平均长度: {edge_analysis['avg_length']:.4f}")
        print(f"  长度范围: {edge_analysis['min_length']:.4f} - {edge_analysis['max_length']:.4f}")
        print(f"  方向分布: {edge_analysis['direction_stats']}")

        print("\n面分析:")
        print(f"  总面数: {face_analysis['total_faces']}")
        print(f"  面大小分布: {face_analysis['face_sizes']}")
        print(f"  法线方向分布: {face_analysis['normal_directions']}")
        print(f"  总面积: {face_analysis['total_area']:.4f}")

        print("\n拓扑分析:")
        print(f"  顶点连接度分布: {topology_analysis['vertex_connections']}")
        print(f"  边连接度分布: {topology_analysis['edge_connections']}")
        print(f"  边界边数: {topology_analysis['boundary_edges']}")
        print(f"  是否为流形: {topology_analysis['is_manifold']}")

# 使用示例
bme = bmesh.new()
for x in range(3):
    for y in range(3):
        bme.verts.new((x, y, 0))
bme.verts.ensure_lookup_table()

for y in range(2):
    for x in range(2):
        i = y * 3 + x
        bme.faces.new([
            bme.verts[i],
            bme.verts[i + 1],
            bme.verts[i + 4],
            bme.verts[i + 3]
        ])

analyzer = MeshAnalyzer(bme)
analyzer.full_report()
```

### 法线验证工具

这个示例展示了如何使用 bmesh.utils 中的函数验证和修正网格法线。

```python
import bmesh
from bmesh.utils import calc_tangent_edge, calc_tangent_loop
from mathutils import Vector

class NormalValidator:
    def __init__(self, bme):
        self.bme = bme

    def validate_face_normals(self):
        """验证面的法线方向"""
        issues = []

        for f in self.bme.faces:
            # 计算法线
            normal = f.normal

            # 检查法线是否有效
            if normal.length < 0.001:
                issues.append({
                    'type': 'zero_normal',
                    'face': f.index,
                    'message': '面法线长度为0'
                })

            # 检查法线是否朝上（假设地面朝上）
            if normal.z < 0:
                issues.append({
                    'type': 'inverted_normal',
                    'face': f.index,
                    'normal': normal,
                    'message': '面法线朝下，可能需要翻转'
                })

        return issues

    def validate_vertex_normals(self):
        """验证顶点法线"""
        issues = []

        for v in self.bme.verts:
            normal = v.normal

            if normal.length < 0.001:
                issues.append({
                    'type': 'zero_normal',
                    'vertex': v.index,
                    'message': '顶点法线长度为0'
                })

        return issues

    def analyze_tangents(self):
        """分析切线方向"""
        tangent_info = []

        for f in self.bme.faces:
            for loop in f.loops:
                tangent = calc_tangent_loop(loop)
                if tangent and tangent.length > 0.001:
                    tangent_info.append({
                        'face': f.index,
                        'vertex': loop.vert.index,
                        'tangent': tangent,
                        'tangent_length': tangent.length
                    })

        return tangent_info

    def flip_face_normals(self, faces=None):
        """翻转面法线"""
        if faces is None:
            faces = list(self.bme.faces)

        for f in faces:
            # 翻转顶点顺序
            verts = list(f.verts)
            verts.reverse()
            f.verts = verts

        return faces

    def print_validation_report(self):
        """打印验证报告"""
        face_issues = self.validate_face_normals()
        vertex_issues = self.validate_vertex_normals()
        tangents = self.analyze_tangents()

        print("=" * 60)
        print("法线验证报告")
        print("=" * 60)

        print(f"\n面法线问题: {len(face_issues)} 个")
        for issue in face_issues[:5]:
            print(f"  - {issue['message']} (面 {issue['face']})")

        print(f"\n顶点法线问题: {len(vertex_issues)} 个")
        for issue in vertex_issues[:5]:
            print(f"  - {issue['message']} (顶点 {issue['vertex']})")

        print(f"\n切线分析: {len(tangents)} 个")
        if tangents:
            avg_length = sum(t['tangent_length'] for t in tangents) / len(tangents)
            print(f"  平均切线长度: {avg_length:.4f}")

# 使用示例
bme = bmesh.new()
# 创建有问题的网格（有些面法线朝下）
verts = []
for x in [-1, 1]:
    for y in [-1, 1]:
        for z in [-1, 1]:
            verts.append(bme.verts.new((x, y, z)))
bme.verts.ensure_lookup_table()

# 创建一个法线朝下的面
bme.faces.new([verts[1], verts[0], verts[2]])  # 顶点顺序导致法线朝下

validator = NormalValidator(bme)
validator.print_validation_report()
```

## 注意事项

### 函数兼容性

bmesh.utils 中的某些函数可能有版本限制或在不同 Blender 版本中行为不同。在使用这些函数时，应该参考当前 Blender 版本的官方文档确认函数的可用性和行为。

### 性能考虑

虽然 bmesh.utils 中的函数本身开销不大，但在处理大规模网格时应该注意调用的频率。如果需要多次计算相同的数据（如法线），应该考虑缓存结果而不是重复计算。

### 返回值处理

某些函数返回字典或特殊类型的值，在使用前应该仔细检查返回值的结构和类型。对于可能返回 None 的函数，应该添加适当的空值检查。

