# bpy_ops_import_export

---
## bpy_ops_import_export

# bpy.ops.import_export - Import and Export Operations Module

Blender 导入导出操作符，用于各种格式的文件导入和导出。

## Overview

`bpy.ops.import_export` 模块包含所有文件导入和导出的操作命令，支持多种 3D 格式、图像格式和动画格式。这个模块涵盖了从 OBJ、FBX、glTF 到 STL、PLY 等各种常用格式的导入导出功能。

## 核心导入操作

### 3D 模型导入

```python
import bpy

# 导入 OBJ
bpy.ops.import_scene.obj(filepath="//model.obj")

# 导入 FBX
bpy.ops.import_scene.fbx(filepath="//model.fbx")

# 导入 glTF
bpy.ops.import_scene.gltf(filepath="//model.gltf")
bpy.ops.import_scene.gltf(filepath="//model.glb")

# 导入 DAE
bpy.ops.import_scene.dae(filepath="//model.dae")

# 导入 3DS
bpy.ops.import_scene.three_ds(filepath="//model.3ds")

# 导入 X3D
bpy.ops.import_scene.x3d(filepath="//model.x3d")

# 导入 LWO
bpy.ops.import_scene.lwo(filepath="//model.lwo")

# 导入 LWS
bpy.ops.import_scene.lws(filepath="//model.lws")

# 导入 MS3D
bpy.ops.import_scene.ms3d(filepath="//model.ms3d")

# 导入 MDD
bpy.ops.import_scene.mdd(filepath="//model.mdd")

def import_obj_file(filepath, use_smooth_groups=True, use_edges=True):
    """导入 OBJ 文件"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        use_smooth_groups=use_smooth_groups,
        use_edges=use_edges
    )
    return bpy.context.active_object

def import_fbx_file(filepath, use_automatic_bone_orientation=True):
    """导入 FBX 文件"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        use_automatic_bone_orientation=use_automatic_bone_orientation
    )
    return bpy.context.active_object

def import_gltf_file(filepath):
    """导入 glTF 文件"""
    bpy.ops.import_scene.gltf(filepath=filepath)
    return bpy.context.active_object
```

### 网格数据导入

```python
# 导入 PLY
bpy.ops.import_mesh.ply(filepath="//model.ply")

# 导入 STL
bpy.ops.import_mesh.stl(filepath="//model.stl")

# 导入 ABC（Alembic）
bpy.ops.wm.alembic_import(filepath="//model.abc")

# 导入 USD
bpy.ops.wm.usd_import(filepath="//model.usd")
bpy.ops.wm.usd_import(filepath="//model.usdz")

# 导入 VRML
bpy.ops.import_scene.vrml(filepath="//model.wrl")

# 导入 IFC
bpy.ops.import_scene.ifc(filepath="//model.ifc")

def import_ply_file(filepath):
    """导入 PLY 文件"""
    bpy.ops.import_mesh.ply(filepath=filepath)
    return bpy.context.active_object

def import_stl_file(filepath, scale=1.0):
    """导入 STL 文件"""
    bpy.ops.import_mesh.stl(filepath=filepath, scale=scale)
    return bpy.context.active_object

def import_alembic(filepath, frame_start=1, frame_end=30):
    """导入 Alembic 文件"""
    bpy.ops.wm.alembic_import(
        filepath=filepath,
        start=frame_start,
        end=frame_end
    )
    return bpy.context.active_object

def import_usd_file(filepath):
    """导入 USD 文件"""
    bpy.ops.wm.usd_import(filepath=filepath)
    return bpy.context.active_object
```

### 图像和纹理导入

```python
# 打开图像
bpy.ops.image.open(filepath="//texture.png")

# 打开序列图像
bpy.ops.image.open(filepath="//image_001.png")

# 打开 HDR
bpy.ops.image.open(filepath="//hdri.exr")

# 导入视频
bpy.ops.import_anim.gltf(filepath="//animation.glb")

def import_image(filepath):
    """导入图像"""
    bpy.ops.image.open(filepath=filepath)
    return bpy.context.active_object

def import_image_sequence(directory, start_frame=1):
    """导入图像序列"""
    import os
    files = sorted([f for f in os.listdir(directory) if f.endswith(('.png', '.jpg', '.jpeg'))])
    
    images = []
    for i, filename in enumerate(files):
        filepath = os.path.join(directory, filename)
        bpy.ops.image.open(filepath=filepath)
        img = bpy.context.active_image
        img.source = 'SEQUENCE'
        img_user = img.users[0] if img.users else None
        images.append(img)
    
    return images

def import_hdri(filepath):
    """导入 HDR 环境图"""
    bpy.ops.image.open(filepath=filepath)
    img = bpy.context.active_image
    img.source = 'FILE'
    return img
```

### 动画数据导入

```python
# 导入 MDD（动画变形）
bpy.ops.import_scene.mdd(filepath="//animation.mdd")

# 导入 BVH
bpy.ops.import_anim.bvh(filepath="//animation.bvh")

# 导入 glTF 动画
bpy.ops.import_scene.gltf(filepath="//animation.glb", export_format='GLTF')

# 导入 FBX 动画
bpy.ops.import_scene.fbx(filepath="//animation.fbx")

# 导入 Alembic 动画
bpy.ops.wm.alembic_import(filepath="//animation.abc")

# 导入 USD 动画
bpy.ops.wm.usd_import(filepath="//animation.usd")

def import_bvh_file(filepath, target_armature=None):
    """导入 BVH 动作文件"""
    bpy.ops.import_anim.bvh(
        filepath=filepath,
        target=target_armature
    )
    return bpy.context.active_object

def import_mdd_animation(filepath, target_object=None):
    """导入 MDD 动画"""
    bpy.ops.import_scene.mdd(filepath=filepath)
    return bpy.context.active_object
```

## 核心导出操作

### 3D 模型导出

```python
# 导出 OBJ
bpy.ops.export_scene.obj(filepath="//model.obj")

# 导出 FBX
bpy.ops.export_scene.fbx(filepath="//model.fbx")

# 导出 glTF
bpy.ops.export_scene.gltf(filepath="//model.gltf")
bpy.ops.export_scene.gltf(filepath="//model.glb")

# 导出 DAE
bpy.ops.export_scene.dae(filepath="//model.dae")

# 导出 3DS
bpy.ops.export_scene.three_ds(filepath="//model.3ds")

# 导出 X3D
bpy.ops.export_scene.x3d(filepath="//model.x3d")

# 导出 VRML
bpy.ops.export_scene.vrml(filepath="//model.wrl")

def export_obj_file(filepath, selection_only=False, use_smooth_groups=True):
    """导出 OBJ 文件"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        use_selection=selection_only,
        use_smooth_groups=use_smooth_groups
    )

def export_fbx_file(filepath, selection_only=False, apply_scale_options='FBX_SCALE_ALL'):
    """导出 FBX 文件"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=selection_only,
        apply_scale_options=apply_scale_options
    )

def export_gltf_file(filepath, export_format='GLTF_EMBEDDED', selection_only=False):
    """导出 glTF 文件"""
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format=export_format,
        use_selection=selection_only
    )
```

### 网格数据导出

```python
# 导出 PLY
bpy.ops.export_mesh.ply(filepath="//model.ply")

# 导出 STL
bpy.ops.export_mesh.stl(filepath="//model.stl")

# 导出 ABC（Alembic）
bpy.ops.wm.alembic_export(filepath="//model.abc")

# 导出 USD
bpy.ops.wm.usd_export(filepath="//model.usd")

# 导出 VRM
bpy.ops.export_scene.vrml(filepath="//model.vrm")

def export_ply_file(filepath, selection_only=False):
    """导出 PLY 文件"""
    bpy.ops.export_mesh.ply(
        filepath=filepath,
        use_selection=selection_only
    )

def export_stl_file(filepath, selection_only=False, scale=1.0):
    """导出 STL 文件"""
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        use_selection=selection_only,
        global_scale=scale
    )

def export_alembic(filepath, frame_start=1, frame_end=30):
    """导出 Alembic 文件"""
    bpy.ops.wm.alembic_export(
        filepath=filepath,
        start=frame_start,
        end=frame_end
    )

def export_usd_file(filepath, frame_start=1, frame_end=30):
    """导出 USD 文件"""
    bpy.ops.wm.usd_export(
        filepath=filepath,
        start=frame_start,
        end=frame_end
    )
```

### 图像导出

```python
# 保存图像
bpy.ops.image.save(filepath="//texture.png")

# 另存为
bpy.ops.image.save_as(filepath="//texture_new.png")

# 渲染并保存
bpy.ops.render.render(write_still=True)

# 导出渲染结果
bpy.ops.render.render(write_still=True)

def save_current_image(filepath):
    """保存当前图像"""
    bpy.ops.image.save(filepath=filepath)

def save_render_result(filepath):
    """保存渲染结果"""
    bpy.context.scene.render.filepath = filepath
    bpy.ops.render.render(write_still=True)

def export_render_sequence(output_dir, file_format='PNG'):
    """导出渲染序列"""
    scene = bpy.context.scene
    
    for frame in range(scene.frame_start, scene.frame_end + 1):
        scene.frame_set(frame)
        
        filename = f"frame_{frame:04d}.{file_format.lower()}"
        filepath = bpy.path.join(output_dir, filename)
        
        bpy.context.scene.render.filepath = filepath
        bpy.ops.render.render(write_still=True)
```

### 动画导出

```python
# 导出 BVH
bpy.ops.export_anim.bvh(filepath="//animation.bvh")

# 导出 glTF 动画
bpy.ops.export_scene.gltf(filepath="//animation.glb")

# 导出 ABC 动画
bpy.ops.wm.alembic_export(filepath="//animation.abc")

# 导出 USD 动画
bpy.ops.wm.usd_export(filepath="//animation.usd")

# 导出 FBX 动画
bpy.ops.export_scene.fbx(filepath="//animation.fbx", export_anim=True)

def export_bvh_file(filepath, frame_start=1, frame_end=30):
    """导出 BVH 动画"""
    bpy.ops.export_anim.bvh(
        filepath=filepath,
        frame_start=frame_start,
        frame_end=frame_end
    )

def export_animation(filepath, format='GLB'):
    """导出动画"""
    if format in ['GLB', 'GLTF']:
        bpy.ops.export_scene.gltf(
            filepath=filepath,
            export_format='GLTF_EMBEDDED' if format == 'GLTF' else 'GLB',
            export_anim=True
        )
    elif format == 'ABC':
        bpy.ops.wm.alembic_export(filepath=filepath)
    elif format == 'FBX':
        bpy.ops.export_scene.fbx(filepath=filepath, export_anim=True)
```

## 格式特定选项

### FBX 导出选项

```python
def export_fbx_animated(filepath, armature=None):
    """导出带动画的 FBX"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=True if armature is None else False,
        apply_scale_options='FBX_SCALE_ALL',
        axis_forward='-Z',
        axis_up='Y',
        use_space_deform=True,
        bake_anim=True,
        bake_anim_use_all_bones=True,
        use_armature_deform=True
    )

def export_fbx_static(filepath):
    """导出静态 FBX"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=True,
        apply_scale_options='FBX_SCALE_ALL',
        axis_forward='-Z',
        axis_up='Y',
        bake_anim=False
    )
```

### glTF 导出选项

```python
def export_gltf_with_materials(filepath, selection_only=False):
    """导出带材质的 glTF"""
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format='GLTF_EMBEDDED',
        use_selection=selection_only,
        export_materials='EXPORT',
        export_colors=True,
        export_cameras=False,
        export_lights=False
    )

def export_gltf_animated(filepath, selection_only=False):
    """导出带动画的 glTF"""
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format='GLB',
        use_selection=selection_only,
        export_anim=True,
        export_anim_use_nla_strips=True,
        export_anim_all_contacts=False,
        export_anim_sampling=True,
        export_anim_keyframe_tiers=False,
        export_lights=True,
        export_cameras=True
    )

def export_gltf_mesh_only(filepath, selection_only=False):
    """仅导出网格的 glTF"""
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format='GLTF_SEPARATE',
        use_selection=selection_only,
        export_materials='EXPORT',
        export_anim=False,
        export_cameras=False,
        export_lights=False,
        export_extras=False
    )
```

### OBJ 导出选项

```python
def export_obj_with_materials(filepath):
    """导出带材质的 OBJ"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        use_selection=True,
        use_materials=True,
        use_triangles=False,
        use_normals=True,
        use_smooth_groups=True,
        use_uvs=True,
        axis_forward='-Z',
        axis_up='Y'
    )

def export_obj_for_3d_printing(filepath):
    """导出 3D 打印用的 OBJ"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        use_selection=True,
        use_materials=False,
        use_triangles=True,
        use_normals=True,
        use_smooth_groups=False,
        use_uvs=True,
        axis_forward='-Z',
        axis_up='Y'
    )
```

### STL 导出选项

```python
def export_stl_for_3d_printing(filepath, selection_only=False):
    """导出 3D 打印用的 STL"""
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        use_selection=selection_only,
        global_scale=1.0,
        apply_modifiers=True,
        use_mesh_modifiers=True,
        batch_mode='OFF'
    )

def export_stl_with_scale(filepath, scale=0.001, selection_only=False):
    """导出缩放的 STL"""
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        use_selection=selection_only,
        global_scale=scale,
        apply_modifiers=True,
        use_mesh_modifiers=True
    )
```

## 批量处理

```python
import bpy
import os

class BatchImporter:
    """批量导入器"""
    
    def __init__(self, directory):
        self.directory = directory
    
    def import_all_obj(self):
        """导入所有 OBJ 文件"""
        files = [f for f in os.listdir(self.directory) if f.endswith('.obj')]
        
        for filename in files:
            filepath = os.path.join(self.directory, filename)
            bpy.ops.import_scene.obj(filepath=filepath)
            print(f"导入: {filename}")
        
        return len(files)
    
    def import_all_fbx(self):
        """导入所有 FBX 文件"""
        files = [f for f in os.listdir(self.directory) if f.endswith('.fbx')]
        
        for filename in files:
            filepath = os.path.join(self.directory, filename)
            bpy.ops.import_scene.fbx(filepath=filepath)
            print(f"导入: {filename}")
        
        return len(files)
    
    def import_all_gltf(self):
        """导入所有 glTF 文件"""
        for ext in ['.gltf', '.glb']:
            files = [f for f in os.listdir(self.directory) if f.endswith(ext)]
            
            for filename in files:
                filepath = os.path.join(self.directory, filename)
                bpy.ops.import_scene.gltf(filepath=filepath)
                print(f"导入: {filename}")
        
        return len(files)

class BatchExporter:
    """批量导出器"""
    
    def __init__(self, directory):
        self.directory = directory
    
    def export_selected_as_obj(self):
        """导出选中的对象为 OBJ"""
        objects = bpy.context.selected_objects
        
        for obj in objects:
            bpy.ops.object.select_all(action='DESELECT')
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            
            filename = f"{obj.name}.obj"
            filepath = os.path.join(self.directory, filename)
            
            bpy.ops.export_scene.obj(filepath=filepath, use_selection=True)
            print(f"导出: {filename}")
        
        return len(objects)
    
    def export_selected_as_fbx(self):
        """导出选中的对象为 FBX"""
        objects = bpy.context.selected_objects
        
        for obj in objects:
            bpy.ops.object.select_all(action='DESELECT')
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            
            filename = f"{obj.name}.fbx"
            filepath = os.path.join(self.directory, filename)
            
            bpy.ops.export_scene.fbx(filepath=filepath, use_selection=True)
            print(f"导出: {filename}")
        
        return len(objects)
    
    def export_frame_range(self, output_dir, file_format='PNG'):
        """导出帧范围"""
        scene = bpy.context.scene
        
        for frame in range(scene.frame_start, scene.frame_end + 1):
            scene.frame_set(frame)
            
            filename = f"frame_{frame:04d}.{file_format.lower()}"
            filepath = os.path.join(output_dir, filename)
            
            bpy.context.scene.render.filepath = filepath
            bpy.ops.render.render(write_still=True)
            print(f"导出: {filename}")
        
        return scene.frame_end - scene.frame_start + 1

def batch_import_directory(directory, format='obj'):
    """批量导入目录"""
    importer = BatchImporter(directory)
    
    if format == 'obj':
        return importer.import_all_obj()
    elif format == 'fbx':
        return importer.import_all_fbx()
    elif format == 'gltf':
        return importer.import_all_gltf()
    
    return 0

def batch_export_directory(directory, format='obj'):
    """批量导出到目录"""
    exporter = BatchExporter(directory)
    
    if format == 'obj':
        return exporter.export_selected_as_obj()
    elif format == 'fbx':
        return exporter.export_selected_as_fbx()
    
    return 0
```

## 导入导出工程面板

```python
class ImportExportPanel(bpy.types.Panel):
    """导入导出面板"""
    bl_label = "导入导出"
    bl_idname = "IMPORT_EXPORT_PT_main"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '导入导出'
    
    def draw(self, context):
        layout = self.layout
        
        layout.label(text="3D 模型导入:")
        
        row = layout.row()
        row.operator("import_scene.obj", text="OBJ")
        row.operator("import_scene.fbx", text="FBX")
        row.operator("import_scene.gltf", text="glTF")
        
        row = layout.row()
        row.operator("import_mesh.ply", text="PLY")
        row.operator("import_mesh.stl", text="STL")
        row.operator("import_scene.dae", text="DAE")
        
        layout.separator()
        layout.label(text="3D 模型导出:")
        
        row = layout.row()
        row.operator("export_scene.obj", text="OBJ")
        row.operator("export_scene.fbx", text="FBX")
        row.operator("export_scene.gltf", text="glTF")
        
        row = layout.row()
        row.operator("export_mesh.ply", text="PLY")
        row.operator("export_mesh.stl", text="STL")
        row.operator("export_scene.dae", text="DAE")
        
        layout.separator()
        layout.label(text="动画导入:")
        
        row = layout.row()
        row.operator("import_anim.bvh", text="BVH")
        row.operator("import_scene.mdd", text="MDD")
        row.operator("wm.alembic_import", text="Alembic")
        
        layout.separator()
        layout.label(text="动画导出:")
        
        row = layout.row()
        row.operator("export_anim.bvh", text="BVH")
        row.operator("wm.alembic_export", text="Alembic")
        row.operator("wm.usd_export", text="USD")

class BatchProcessPanel(bpy.types.Panel):
    """批量处理面板"""
    bl_label = "批量处理"
    bl_idname = "IMPORT_EXPORT_PT_batch"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = '导入导出'
    
    def draw(self, context):
        layout = self.layout
        
        layout.label(text="批量导入:")
        
        row = layout.row()
        row.operator("batch_import.obj", text="导入 OBJ 目录")
        row.operator("batch_import.fbx", text="导入 FBX 目录")
        
        row = layout.row()
        row.operator("batch_import.gltf", text="导入 glTF 目录")
        row.operator("batch_import.images", text="导入图像目录")
        
        layout.separator()
        layout.label(text="批量导出:")
        
        row = layout.row()
        row.operator("batch_export.obj", text="导出 OBJ")
        row.operator("batch_export.fbx", text="导出 FBX")
        
        row = layout.row()
        row.operator("batch_export.images", text="导出图像序列")
        row.operator("batch_export.video", text="导出视频")
```

## 最佳实践

### 1. 导入最佳实践

```python
def import_with_cleanup(filepath, format='obj'):
    """导入并清理"""
    # 导入前选择所有对象
    bpy.ops.object.select_all(action='SELECT')
    
    # 导入
    if format == 'obj':
        bpy.ops.import_scene.obj(filepath=filepath)
    elif format == 'fbx':
        bpy.ops.import_scene.fbx(filepath=filepath)
    elif format == 'gltf':
        bpy.ops.import_scene.gltf(filepath=filepath)
    
    # 选择导入的对象
    imported_objects = [obj for obj in bpy.context.selected_objects if obj != bpy.context.active_object]
    
    return imported_objects

def import_and_setup(filepath, format='obj'):
    """导入并设置"""
    objects = import_with_cleanup(filepath, format)
    
    # 设置为平滑着色
    for obj in objects:
        if obj.type == 'MESH':
            bpy.ops.object.shade_smooth()
    
    # 重置变换
    bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
    
    return objects
```

### 2. 导出最佳实践

```python
def export_selected_animated(filepath, format='glb'):
    """导出选中的动画对象"""
    # 确保选中的对象有动画
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        use_selection=True,
        export_anim=True,
        export_anim_use_nla_strips=True
    )

def export_for_web(filepath, format='glb'):
    """导出网页用模型"""
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format='GLB',
        use_selection=False,
        export_materials='EXPORT',
        export_colors=True,
        export_anim=True,
        export_cameras=False,
        export_lights=True,
        export_extras=False,
        export_yup=True
    )

def export_for_game_engine(filepath, format='fbx'):
    """导出游戏引擎模型"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=True,
        apply_scale_options='FBX_SCALE_ALL',
        axis_forward='-Z',
        axis_up='Y',
        bake_anim=True,
        use_armature_deform=True,
        export_anim=True,
        export_lights=True,
        export_cameras=False
    )
```

### 3. 格式转换

```python
def convert_format(input_path, output_path, input_format, output_format):
    """转换格式"""
    # 导入
    if input_format == 'obj':
        bpy.ops.import_scene.obj(filepath=input_path)
    elif input_format == 'fbx':
        bpy.ops.import_scene.fbx(filepath=input_path)
    elif input_format == 'gltf':
        bpy.ops.import_scene.gltf(filepath=input_path)
    
    # 导出
    if output_format == 'obj':
        bpy.ops.export_scene.obj(filepath=output_path)
    elif output_format == 'fbx':
        bpy.ops.export_scene.fbx(filepath=output_path)
    elif output_format == 'glb':
        bpy.ops.export_scene.gltf(filepath=output_path, export_format='GLB')

def batch_convert_format(input_dir, output_dir, input_format, output_format):
    """批量转换格式"""
    import os
    
    input_files = [f for f in os.listdir(input_dir) if f.endswith(f'.{input_format}')]
    
    for filename in input_files:
        input_path = os.path.join(input_dir, filename)
        
        base_name = os.path.splitext(filename)[0]
        output_filename = f"{base_name}.{output_format}"
        output_path = os.path.join(output_dir, output_filename)
        
        convert_format(input_path, output_path, input_format, output_format)
        print(f"转换: {filename} -> {output_filename}")
    
    return len(input_files)
```

## 常见问题

### Q: 导入的模型没有材质
A: 某些格式不支持材质，确保导出时包含材质信息。

### Q: 动画丢失
A: 检查时间轴设置，确保导出时包含动画数据。

### Q: 坐标系不同
A: 导出时调整轴向设置，Blender 使用 Y-up，DirectX 使用 Z-up。

### Q: 文件过大
A: 使用 glTF 的二进制格式，减小纹理分辨率或使用压缩。

## 相关模块

- [bpy.ops.file](bpy_ops_file_module.md) - 文件操作
- [bpy.ops.object](bpy_ops_object_module.md) - 对象操作
- [bpy.ops.scene](bpy_ops_detailed_module.md) - 场景操作
- [bpy.types](bpy_types_detailed_module.md) - 数据类型


---
## bpy_ops_import_fbx

# bpy.ops.import_scene - FBX导入操作模块

Blender FBX文件导入操作符，用于将FBX格式文件导入Blender。

## 概述

`bpy.ops.import_scene.fbx` 模块提供用于将Autodesk FBX格式文件导入Blender的功能，支持几何体、材质、纹理、骨骼和动画的导入。

## 核心功能

### 基础导入

```python
import bpy

# 导入FBX文件
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.fbx'}],
    ui_tab='MAIN',
    use_manual_orientation=False,
    global_scale=1.0,
    bake_space_transform=False,
    use_custom_normals=True,
    use_image_search=True,
    use_alpha_decals=False,
    decal_offset=0.0,
    use_anim=True,
    use_anim_action_all=True,
    use_anim_adaptive_constraints=False,
    use_anim_cache=False,
    use_conical_deform=False,
    use_apply_scale=True,
    use_decal_offset=False,
    axis_forward='-Z',
    axis_up='Y'
)
```

### 坐标轴设置

```python
# Blender默认轴向
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    axis_forward='-Z',
    axis_up='Y'
)

# 传统FBX轴向
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    axis_forward='-Z',
    axis_up='Y'
)

# 其他轴向组合
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    axis_forward='Y',
    axis_up='Z'
)
```

### 缩放设置

```python
# 设置缩放
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    global_scale=1.0
)

# 缩放10倍
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    global_scale=10.0
)

# 应用缩放
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_apply_scale=True
)
```

### 法线设置

```python
# 使用自定义法线
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_custom_normals=True
)

# 不使用自定义法线
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_custom_normals=False
)
```

### 动画设置

```python
# 导入动画
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_anim=True
)

# 不导入动画
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_anim=False
)

# 导入所有动作
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_anim=True,
    use_anim_action_all=True
)

# 自适应约束
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_anim_adaptive_constraints=True
)
```

### 骨骼导入

```python
# 导入骨骼
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_armature_deform=True
)

# 不导入骨骼
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_armature_deform=False
)
```

### 纹理搜索

```python
# 启用纹理搜索
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_image_search=True
)

# 禁用纹理搜索
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_image_search=False
)
```

### 材质设置

```python
# 使用Alpha贴图
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_alpha_decals=True
)

# 设置偏移
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    decal_offset=0.0
)

# 使用蒙皮
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_custom_skinning=True
)
```

## 实用函数

```python
def import_fbx_model(filepath, scale=1.0):
    """导入FBX模型"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        global_scale=scale,
        use_custom_normals=True,
        use_anim=False,
        use_armature_deform=True,
        use_image_search=True
    )
    return list(bpy.context.selected_objects)

def import_fbx_with_animations(filepath, scale=1.0):
    """导入FBX带动画"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        global_scale=scale,
        use_custom_normals=True,
        use_anim=True,
        use_anim_action_all=True,
        use_armature_deform=True,
        use_image_search=True
    )
    return list(bpy.context.selected_objects)

def import_fbx_for_editing(filepath):
    """导入FBX用于编辑"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        global_scale=1.0,
        use_custom_normals=True,
        use_anim=False,
        use_armature_deform=False,
        use_image_search=False
    )
    return list(bpy.context.selected_objects)

def import_fbx_character(filepath):
    """导入FBX角色"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        global_scale=1.0,
        use_custom_normals=True,
        use_anim=True,
        use_anim_action_all=True,
        use_armature_deform=True,
        use_image_search=True,
        use_apply_scale=True
    )
    return list(bpy.context.selected_objects)

def batch_import_fbx_files(fbx_files, scale=1.0):
    """批量导入FBX文件"""
    all_objects = []
    for filepath in fbx_files:
        bpy.ops.import_scene.fbx(
            filepath=filepath,
            global_scale=scale
        )
        imported = list(bpy.context.selected_objects)
        all_objects.extend(imported)
    return all_objects

def import_fbx_and_merge(filepath):
    """导入FBX并合并"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        global_scale=1.0,
        use_custom_normals=True
    )
    
    objects = list(bpy.context.selected_objects)
    if len(objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return objects
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查选中对象
selected = bpy.context.selected_objects
print(f"导入对象数: {len(selected)}")

# 检查对象类型
for obj in selected:
    print(f"{obj.name}: 类型={obj.type}")

# 检查隐藏状态
for obj in selected:
    print(f"{obj.name}: 隐藏={obj.hide_viewport}")
    obj.hide_viewport = False

# 检查层级关系
for obj in selected:
    print(f"{obj.name}: 父级={obj.parent}")
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 检查骨骼
if obj.type == 'ARMATURE':
    print(f"骨骼数: {len(obj.data.bones)}")

# 检查NLA轨道
if obj.animation_data:
    for track in obj.animation_data.nla_tracks:
        print(f"NLA轨道: {track.name}")

# 重新导入带动画
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_anim=True,
    use_anim_action_all=True
)
```

### 材质丢失
```python
# 检查材质
print(f"材质数: {len(bpy.data.materials)}")

# 检查纹理
print(f"纹理数: {len(bpy.data.textures)}")

# 检查图像
print(f"图像数: {len(bpy.data.images)}")

# 检查纹理搜索
bpy.ops.import_scene.fbx(
    filepath='//model.fbx',
    use_image_search=True
)

# 手动分配材质
obj = bpy.context.active_object
if obj.data.materials:
    for slot in obj.material_slots:
        print(f"材质槽: {slot.material}")
```

### 骨骼问题
```python
# 检查骨骼
armature = None
for obj in bpy.context.selected_objects:
    if obj.type == 'ARMATURE':
        armature = obj
        break

if armature:
    print(f"骨骼数: {len(armature.data.bones)}")
    
    # 检查权重
    for vert in armature.children[0].data.vertices:
        if vert.groups:
            print(f"顶点 {vert.index}: 权重组={len(vert.groups)}")

# 检查父级关系
for obj in bpy.context.selected_objects:
    if obj.parent and obj.parent.type == 'ARMATURE':
        print(f"{obj.name} 父级: {obj.parent.name}")
```


---
## bpy_ops_export_fbx

# bpy.ops.export_scene - FBX导出操作模块

Blender FBX文件导出操作符，用于将场景导出为FBX格式。

## 概述

`bpy.ops.export_scene.fbx` 模块提供用于将Blender场景导出为Autodesk FBX格式的功能，支持几何体、材质、纹理、骨骼和动画的导出。

## 核心功能

### 基础导出

```python
import bpy

# 导出选中对象
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=True,
    use_selection=False,
    mesh_smooth_type='OFF',
    use_mesh_smooth=False,
    use_mesh_modifiers=True,
    use_triangles=False,
    use_nurbs=False,
    use_armature_deform=False,
    bake_space_transform=False,
    use_anim=True,
    use_anim_action_all=True,
    use_anim_only=False,
    use_shape_keys=False,
    use_custom_props=False,
    apply_scale_options='FBX_SCALE_NONE',
    axis_forward='-Z',
    axis_up='Y',
    bake_anim_scenes=True,
    bake_anim_use_all_bones=True,
    bake_anim_use_nla_strips=True,
    bake_anim_use_all_actions=True,
    bake_anim_force_startend_keying=True,
    bake_anim_step=1.0,
    bake_anim_simplify_factor=1.0,
    path_mode='ABSOLUTE',
    embed_textures=False,
    batch_mode='OFF',
    use_batch_own_dir=False,
    use_metadata=True
)
```

### 导出范围

```python
# 只导出选中对象
bpy.ops.export_scene.fbx(
    filepath='//selected.fbx',
    export_selected=True
)

# 导出所有对象
bpy.ops.export_scene.fbx(
    filepath='//all.fbx',
    export_selected=False
)
```

### 网格设置

```python
# 平滑类型
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    mesh_smooth_type='OFF'
)

# 边平滑
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    mesh_smooth_type='EDGE'
)

# 面平滑
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    mesh_smooth_type='FACE'
)

# 使用网格平滑
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    use_mesh_smooth=True
)

# 应用网格修改器
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    use_mesh_modifiers=True
)

# 三角化
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    use_triangles=True
)
```

### 骨骼设置

```python
# 导出骨骼变形
bpy.ops.export_scene.fbx(
    filepath='//armature.fbx',
    use_armature_deform=True
)

# 不导出骨骼
bpy.ops.export_scene.fbx(
    filepath='//mesh.fbx',
    use_armature_deform=False
)
```

### 动画设置

```python
# 导出动画
bpy.ops.export_scene.fbx(
    filepath='//animation.fbx',
    use_anim=True
)

# 不导出动画
bpy.ops.export_scene.fbx(
    filepath='//static.fbx',
    use_anim=False
)

# 导出所有动作
bpy.ops.export_scene.fbx(
    filepath='//animations.fbx',
    use_anim=True,
    use_anim_action_all=True
)

# 只导出动画
bpy.ops.export_scene.fbx(
    filepath='//anim_only.fbx',
    use_anim=True,
    use_anim_only=True
)
```

### 变形目标

```python
# 导出变形目标
bpy.ops.export_scene.fbx(
    filepath='//shapekeys.fbx',
    use_shape_keys=True
)

# 不导出变形目标
bpy.ops.export_scene.fbx(
    filepath='//no_shapekeys.fbx',
    use_shape_keys=False
)
```

### 自定义属性

```python
# 导出自定义属性
bpy.ops.export_scene.fbx(
    filepath='//custom.fbx',
    use_custom_props=True
)

# 不导出自定义属性
bpy.ops.export_scene.fbx(
    filepath='//no_custom.fbx',
    use_custom_props=False
)
```

### 缩放应用

```python
# 不应用缩放
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    apply_scale_options='FBX_SCALE_NONE'
)

# 应用FBX缩放
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    apply_scale_options='FBX_SCALE_UNITS'
)

# 应用所有缩放
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    apply_scale_options='FBX_SCALE_ALL'
)
```

### 坐标轴设置

```python
# Blender默认轴向
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    axis_forward='-Z',
    axis_up='Y'
)

# 其他轴向组合
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    axis_forward='Y',
    axis_up='Z'
)
```

### 烘焙动画

```python
# 烘焙动画场景
bpy.ops.export_scene.fbx(
    filepath='//baked.fbx',
    bake_anim_scenes=True
)

# 烘焙所有骨骼
bpy.ops.export_scene.fbx(
    filepath='//baked.fbx',
    bake_anim_use_all_bones=True
)

# 烘焙NLA轨道
bpy.ops.export_scene.fbx(
    filepath='//baked.fbx',
    bake_anim_use_nla_strips=True
)

# 烘焙所有动作
bpy.ops.export_scene.fbx(
    filepath='//baked.fbx',
    bake_anim_use_all_actions=True
)

# 强制开始结束关键帧
bpy.ops.export_scene.fbx(
    filepath='//baked.fbx',
    bake_anim_force_startend_keying=True
)

# 设置烘焙步长
bpy.ops.export_scene.fbx(
    filepath='//baked.fbx',
    bake_anim_step=0.1
)

# 简化因子
bpy.ops.export_scene.fbx(
    filepath='//baked.fbx',
    bake_anim_simplify_factor=1.0
)
```

### 批处理

```python
# 禁用批处理
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    batch_mode='OFF'
)

# 启用批处理
bpy.ops.export_scene.fbx(
    filepath='//export/',
    batch_mode='COLLECTION'
)

# 按对象批处理
bpy.ops.export_scene.fbx(
    filepath='//export/',
    batch_mode='OBJECT'
)
```

## 实用函数

```python
def export_fbx_model(filepath, apply_modifiers=True):
    """导出FBX模型"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=apply_modifiers,
        mesh_smooth_type='OFF',
        use_armature_deform=False
    )

def export_fbx_with_animations(filepath):
    """导出FBX带动画"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=True,
        use_armature_deform=True,
        use_anim=True,
        use_anim_action_all=True,
        bake_anim_use_nla_strips=True,
        bake_anim_use_all_actions=True
    )

def export_fbx_character(filepath):
    """导出FBX角色"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=True,
        use_armature_deform=True,
        use_anim=True,
        use_anim_action_all=True,
        use_shape_keys=True,
        bake_anim_scenes=True,
        bake_anim_use_all_bones=True,
        bake_anim_use_nla_strips=True,
        bake_anim_use_all_actions=True
    )

def export_fbx_for_unreal(filepath):
    """导出FBX用于Unreal"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=True,
        use_triangles=True,
        use_armature_deform=True,
        use_anim=True,
        use_anim_action_all=True,
        bake_anim_scenes=True,
        bake_anim_use_all_bones=True,
        bake_anim_use_nla_strips=True,
        bake_anim_use_all_actions=True,
        bake_anim_force_startend_keying=True,
        apply_scale_options='FBX_SCALE_UNITS'
    )

def export_fbx_for_maya(filepath):
    """导出FBX用于Maya"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=True,
        use_armature_deform=True,
        use_anim=True,
        use_anim_action_all=True,
        bake_anim_scenes=True,
        apply_scale_options='FBX_SCALE_NONE'
    )

def batch_export_fbx_by_collection(output_dir):
    """按集合批量导出FBX"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for collection in bpy.data.collections:
        bpy.ops.object.select_all(action='DESELECT')
        
        for obj in collection.objects:
            obj.select_set(True)
        
        if bpy.context.selected_objects:
            bpy.context.view_layer.active_layer_collection = bpy.context.layer_collection.children[collection.name]
            filepath = os.path.join(output_dir, f'{collection.name}.fbx')
            bpy.ops.export_scene.fbx(
                filepath=filepath,
                export_selected=True,
                use_mesh_modifiers=True
            )
```

## 常见问题排查

### 导出后模型丢失
```python
# 检查选中对象
selected = [obj for obj in bpy.context.selected_objects if obj.type == 'MESH']
print(f"选中网格: {len(selected)}")

# 确保对象可见
for obj in selected:
    obj.hide_viewport = False
    obj.hide_render = False

# 检查导出路径
print(f"导出路径: {bpy.path.abspath('//model.fbx')}")

# 确保文件扩展名正确
filepath = '//model.fbx'
if not filepath.endswith('.fbx'):
    filepath += '.fbx'
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")
    if obj.animation_data.action:
        print(f"帧范围: {obj.animation_data.action.frame_range}")

# 检查骨骼动画
if obj.type == 'ARMATURE':
    print(f"骨骼数: {len(obj.data.bones)}")

# 确保导出动画选项
bpy.ops.export_scene.fbx(
    filepath='//anim.fbx',
    export_selected=True,
    use_anim=True,
    use_anim_action_all=True,
    bake_anim_use_all_actions=True
)
```

### 材质丢失
```python
# 检查材质
obj = bpy.context.active_object
print(f"材质数: {len(obj.data.materials) if obj.data else 0}")

# 检查材质类型
for mat in obj.data.materials:
    print(f"材质: {mat.name}")
    print(f"使用节点: {mat.use_nodes}")

# 确保导出材质
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    export_selected=True,
    mesh_smooth_type='OFF'
)

# 检查导出设置
print("FBX导出设置:")
print("  - 导出选中: True")
print("  - 使用网格修改器: True")
print("  - 导出动画: True")
```

### 骨骼问题
```python
# 检查骨骼
armature = None
for obj in bpy.context.selected_objects:
    if obj.type == 'ARMATURE':
        armature = obj
        break

if armature:
    print(f"骨骼数: {len(armature.data.bones)}")
    
    # 检查绑定
    for child in armature.children:
        if child.type == 'MESH':
            print(f"子对象: {child.name}")
            for mod in child.modifiers:
                if mod.type == 'ARMATURE':
                    print(f"绑定到: {mod.object}")

# 确保导出骨骼
bpy.ops.export_scene.fbx(
    filepath='//armature.fbx',
    export_selected=True,
    use_armature_deform=True
)
```

### 缩放问题
```python
# 检查缩放
obj = bpy.context.active_object
print(f"缩放: {obj.scale}")

# 应用缩放
bpy.ops.object.transform_apply(scale=True)

# 设置正确的缩放选项
bpy.ops.export_scene.fbx(
    filepath='//model.fbx',
    export_selected=True,
    apply_scale_options='FBX_SCALE_UNITS'
)

# 检查单位设置
print(f"场景单位: {bpy.context.scene.unit_settings.system}")
print(f"比例: {bpy.context.scene.unit_settings.scale_length}")
```


---
## bpy_ops_import_obj

# bpy.ops.import_mesh - OBJ导入操作模块

Blender OBJ文件导入操作符，用于将OBJ网格文件导入Blender。

## 概述

`bpy.ops.import_mesh.obj` 模块提供用于将Wavefront OBJ格式网格文件导入Blender的功能，支持几何体、材质和纹理的导入。

## 核心功能

### 基础导入

```python
import bpy

# 导入OBJ文件
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.obj'}],
    ui_tab='MAIN',
    use_smooth_groups=True,
    use_split_objects=True,
    use_split_groups=True,
    use_groups_as_vgroups=False,
    use_image_search=True,
    split_mode='ON',
    global_clamp_size=0,
    axis_forward='-Z',
    axis_up='Y'
)
```

### 网格分组选项

```python
# 分割对象
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_split_objects=True
)

# 分割组
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_split_groups=True
)

# 不分割
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_split_objects=False,
    use_split_groups=False
)

# 使用组作为顶点组
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_groups_as_vgroups=True
)
```

### 光滑组设置

```python
# 使用光滑组
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_smooth_groups=True
)

# 不使用光滑组
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_smooth_groups=False
)
```

### 图像搜索

```python
# 启用图像搜索
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_image_search=True
)

# 禁用图像搜索
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_image_search=False
)
```

### 轴向设置

```python
# Blender默认轴向
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    axis_forward='-Z',
    axis_up='Y'
)

# 传统OBJ轴向
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    axis_forward='-Z',
    axis_up='Y'
)

# 其他轴向组合
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    axis_forward='Y',
    axis_up='Z'
)
```

### 尺寸限制

```python
# 设置尺寸限制
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    global_clamp_size=10.0
)

# 无限制
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    global_clamp_size=0
)
```

## 实用函数

```python
def import_obj_model(filepath, split_objects=True):
    """导入OBJ模型"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        use_split_objects=split_objects,
        use_smooth_groups=True,
        use_image_search=True
    )
    return list(bpy.context.selected_objects)

def import_obj_with_materials(filepath):
    """导入OBJ带材质"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        use_split_objects=True,
        use_smooth_groups=True,
        use_image_search=True,
        ui_tab='MATERIALS'
    )
    return list(bpy.context.selected_objects)

def import_obj_and_merge(filepath):
    """导入OBJ并合并"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        use_split_objects=False,
        use_split_groups=False,
        use_smooth_groups=True
    )
    
    # 合并所有对象
    objects = list(bpy.context.selected_objects)
    if len(objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return objects

def import_obj_scaled(filepath, target_scale=1.0):
    """导入OBJ并缩放"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        use_split_objects=True,
        global_clamp_size=target_scale
    )
    return list(bpy.context.selected_objects)

def import_obj_for_editing(filepath):
    """导入OBJ用于编辑"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        use_split_objects=True,
        use_split_groups=False,
        use_groups_as_vgroups=True,
        use_smooth_groups=True,
        use_image_search=False
    )
    return list(bpy.context.selected_objects)

def batch_import_obj_files(obj_files):
    """批量导入OBJ文件"""
    all_objects = []
    
    for filepath in obj_files:
        bpy.ops.import_scene.obj(filepath=filepath)
        imported = list(bpy.context.selected_objects)
        all_objects.extend(imported)
    
    return all_objects

def import_obj_with_transform(filepath, rotation=(0, 0, 0), scale=(1, 1, 1)):
    """导入OBJ并应用变换"""
    bpy.ops.import_scene.obj(filepath=filepath)
    obj = bpy.context.active_object
    
    obj.rotation_euler = [r * (3.14159 / 180) for r in rotation]
    obj.scale = scale
    
    bpy.ops.object.transform_apply(location=False, rotation=True, scale=True)
    
    return obj
```

## 常见问题排查

### 导入后材质丢失
```python
# 检查材质
print(f"材质数: {len(bpy.data.materials)}")

# 检查纹理
print(f"纹理数: {len(bpy.data.textures)}")

# 检查图像
print(f"图像数: {len(bpy.data.images)}")

# 搜索材质
mat = bpy.data.materials.get('Material')
if mat:
    print(f"材质: {mat.name}")
    if mat.use_nodes:
        for node in mat.node_tree.nodes:
            print(f"节点: {node.type}")

# 重新导入带材质搜索
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_image_search=True,
    ui_tab='MATERIALS'
)
```

### 对象方向错误
```python
# 检查轴向设置
print(f"前向: {-Z}")
print(f"上向: {Y}")

# 检查对象方向
obj = bpy.context.active_object
print(f"旋转: {obj.rotation_euler}")

# 尝试不同轴向
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    axis_forward='-Z',
    axis_up='Y'
)

# 手动旋转
obj.rotation_euler = (0, 0, math.radians(90))
bpy.ops.object.transform_apply(rotation=True)
```

### 网格分割问题
```python
# 检查导入的对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 检查对象名称
for obj in bpy.context.selected_objects:
    print(f"对象: {obj.name}")

# 尝试合并
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.join()

# 重新导入不分割
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_split_objects=False,
    use_split_groups=False
)
```

### 图像找不到
```python
# 检查文件路径
print(f"当前目录: {bpy.path.abspath('//')}")
print(f"纹理目录: {bpy.path.abspath('//textures/')}")

# 列出可用图像
print("可用图像:")
for img in bpy.data.images:
    print(f"  {img.name}: {img.filepath}")

# 手动加载纹理
img = bpy.data.images.load('//textures/texture.png')

# 重新导入带图像搜索
bpy.ops.import_scene.obj(
    filepath='//model.obj',
    use_image_search=True
)
```


---
## bpy_ops_export_obj

# bpy.ops.export_scene - OBJ导出操作模块

Blender OBJ文件导出操作符，用于将场景导出为OBJ格式。

## 概述

`bpy.ops.export_scene.obj` 模块提供用于将Blender场景导出为Wavefront OBJ格式的功能，支持几何体、材质和纹理的导出。

## 核心功能

### 基础导出

```python
import bpy

# 导出选中对象
bpy.ops.export_scene.obj(
    filepath='//model.obj',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=True,
    use_selection=False,
    use_triangles=False,
    use_normals=True,
    use_uv=True,
    use_materials=True,
    use_nurbs=False,
    use_vertex_groups=False,
    use_blen_objects=True,
    group_by_object=False,
    group_by_material=False,
    keep_vertex_order=True,
    use_verbose=False,
    use_mirror_x=False,
    global_scale=1.0,
    path_mode='RELATIVE'
)
```

### 导出范围

```python
# 只导出选中对象
bpy.ops.export_scene.obj(
    filepath='//selected.obj',
    export_selected=True
)

# 导出所有对象
bpy.ops.export_scene.obj(
    filepath='//all.obj',
    export_selected=False
)
```

### 几何体设置

```python
# 三角化
bpy.ops.export_scene.obj(
    filepath='//triangulated.obj',
    use_triangles=True
)

# 不三角化
bpy.ops.export_scene.obj(
    filepath='//model.obj',
    use_triangles=False
)

# 导出法线
bpy.ops.export_scene.obj(
    filepath='//normals.obj',
    use_normals=True
)

# 不导出法线
bpy.ops.export_scene.obj(
    filepath='//no_normals.obj',
    use_normals=False
)
```

### UV设置

```python
# 导出UV
bpy.ops.export_scene.obj(
    filepath='//uv.obj',
    use_uv=True
)

# 不导出UV
bpy.ops.export_scene.obj(
    filepath='//no_uv.obj',
    use_uv=False
)
```

### 材质设置

```python
# 导出材质
bpy.ops.export_scene.obj(
    filepath='//materials.obj',
    use_materials=True
)

# 不导出材质
bpy.ops.export_scene.obj(
    filepath='//no_materials.obj',
    use_materials=False
)
```

### 顶点组设置

```python
# 导出顶点组
bpy.ops.export_scene.obj(
    filepath='//vertex_groups.obj',
    use_vertex_groups=True
)

# 不导出顶点组
bpy.ops.export_scene.obj(
    filepath='//no_groups.obj',
    use_vertex_groups=False
)
```

### 分组设置

```python
# 按对象分组
bpy.ops.export_scene.obj(
    filepath='//by_object.obj',
    group_by_object=True
)

# 按材质分组
bpy.ops.export_scene.obj(
    filepath='//by_material.obj',
    group_by_material=True
)

# 不分组
bpy.ops.export_scene.obj(
    filepath='//no_group.obj',
    group_by_object=False,
    group_by_material=False
)
```

### 顶点顺序

```python
# 保持顶点顺序
bpy.ops.export_scene.obj(
    filepath='//ordered.obj',
    keep_vertex_order=True
)

# 不保持顶点顺序
bpy.ops.export_scene.obj(
    filepath='//unordered.obj',
    keep_vertex_order=False
)
```

### 缩放和镜像

```python
# 设置全局缩放
bpy.ops.export_scene.obj(
    filepath='//scaled.obj',
    global_scale=1.0
)

# 镜像X轴
bpy.ops.export_scene.obj(
    filepath='//mirrored.obj',
    use_mirror_x=True
)
```

### 路径模式

```python
# 相对路径
bpy.ops.export_scene.obj(
    filepath='//model.obj',
    path_mode='RELATIVE'
)

# 绝对路径
bpy.ops.export_scene.obj(
    filepath='//model.obj',
    path_mode='ABSOLUTE'
)

# 条带
bpy.ops.export_scene.obj(
    filepath='//model.obj',
    path_mode='STRIP'
)
```

### 详细输出

```python
# 详细输出
bpy.ops.export_scene.obj(
    filepath='//verbose.obj',
    use_verbose=True
)

# 不详细输出
bpy.ops.export_scene.obj(
    filepath='//quiet.obj',
    use_verbose=False
)
```

## 实用函数

```python
def export_obj_model(filepath, apply_modifiers=True):
    """导出OBJ模型"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        export_selected=True,
        use_triangles=False,
        use_normals=True,
        use_uv=True,
        use_materials=True,
        use_vertex_groups=False
    )

def export_obj_triangulated(filepath):
    """导出OBJ三角化"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=True,
        use_materials=True
    )

def export_obj_for_cad(filepath):
    """导出OBJ用于CAD"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=False,
        use_materials=False,
        global_scale=1.0
    )

def export_obj_for_print(filepath):
    """导出OBJ用于3D打印"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=False,
        use_materials=False,
        use_vertex_groups=False
    )

def export_obj_with_materials(filepath):
    """导出OBJ带材质"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        export_selected=True,
        use_triangles=False,
        use_normals=True,
        use_uv=True,
        use_materials=True,
        path_mode='RELATIVE'
    )

def export_obj_batched(output_dir):
    """批量导出OBJ"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for obj in bpy.data.objects:
        if obj.type == 'MESH':
            bpy.ops.object.select_all(action='DESELECT')
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            filepath = os.path.join(output_dir, f'{obj.name}.obj')
            bpy.ops.export_scene.obj(
                filepath=filepath,
                export_selected=True,
                use_triangles=False,
                use_normals=True,
                use_uv=True
            )

def export_obj_collection(collection, filepath):
    """导出集合为OBJ"""
    bpy.ops.object.select_all(action='DESELECT')
    
    for obj in collection.objects:
        if obj.type == 'MESH':
            obj.select_set(True)
    
    if bpy.context.selected_objects:
        bpy.ops.export_scene.obj(
            filepath=filepath,
            export_selected=True,
            use_triangles=False,
            use_normals=True,
            use_uv=True,
            use_materials=True
        )
```

## 常见问题排查

### 导出后模型缺失
```python
# 检查选中对象
selected = [obj for obj in bpy.context.selected_objects if obj.type == 'MESH']
print(f"选中网格: {len(selected)}")

# 确保对象可见
for obj in selected:
    obj.hide_viewport = False
    obj.hide_render = False

# 检查网格数据
for obj in selected:
    print(f"{obj.name}: 顶点数={len(obj.data.vertices)}, 面数={len(obj.data.polygons)}")
```

### UV丢失
```python
# 检查UV层
mesh = bpy.context.active_object.data
print(f"UV层: {[l.name for l in mesh.uv_layers]}")

# 确保导出UV选项
bpy.ops.export_scene.obj(
    filepath='//uv.obj',
    export_selected=True,
    use_uv=True
)
```

### 材质丢失
```python
# 检查材质
obj = bpy.context.active_object
print(f"材质数: {len(obj.data.materials) if obj.data else 0}")

# 确保导出材质选项
bpy.ops.export_scene.obj(
    filepath='//mat.obj',
    export_selected=True,
    use_materials=True
)

# 检查mtl文件
mtl_path = bpy.path.abspath('//model.mtl')
print(f"mtl文件存在: {bpy.path.exists(mtl_path)}")
```

### 法线问题
```python
# 检查法线
mesh = bpy.context.active_object.data
print(f"自定义法线: {mesh.has_custom_normals}")

# 重新计算法线
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.select_all(action='SELECT')
bpy.ops.mesh.normals_make_consistent(inside=False)
bpy.ops.object.mode_set(mode='OBJECT')

# 确保导出法线选项
bpy.ops.export_scene.obj(
    filepath='//normals.obj',
    export_selected=True,
    use_normals=True
)
```

### 文件路径问题
```python
# 检查路径模式
print(f"路径模式: RELATIVE")

# 使用相对路径
bpy.ops.export_scene.obj(
    filepath='//model.obj',
    path_mode='RELATIVE'
)

# 使用绝对路径
bpy.ops.export_scene.obj(
    filepath='C:/project/model.obj',
    path_mode='ABSOLUTE'
)

# 检查导出目录
export_dir = bpy.path.dirname(bpy.path.abspath('//model.obj'))
print(f"导出目录: {export_dir}")
print(f"目录存在: {bpy.path.exists(export_dir)}")
```

### 顶点顺序问题
```python
# 检查顶点顺序
obj = bpy.context.active_object
print(f"顶点数: {len(obj.data.vertices)}")

# 重新排序顶点
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.select_all(action='SELECT')
bpy.ops.mesh.remove_doubles()
bpy.ops.object.mode_set(mode='OBJECT')

# 保持顶点顺序导出
bpy.ops.export_scene.obj(
    filepath='//ordered.obj',
    export_selected=True,
    keep_vertex_order=True
)
```


---
## bpy_ops_import_gltf

# bpy.ops.import_gltf - GLTF导入操作模块

Blender GLTF格式导入操作符，用于从glTF/glb文件导入场景和模型。

## 概述

`bpy.ops.import_gltf` 模块提供用于将glTF 2.0标准格式文件导入Blender的功能，支持glb、gltf和嵌入式格式。

## 核心功能

### 基础导入

```python
import bpy

# 导入GLB文件
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_pack_images=True,
    merge_vertices=False,
    import_shading='ORIGINAL',
    import_colors=True,
    import_cameras=True,
    import_lights=True,
    import_materials=True,
    import_modifiers=True,
    import_normals=True,
    import_animations=True,
    import_current_frame=True
)

# 导入glTF文件
bpy.ops.import_gltf(
    filepath='//model.gltf',
    import_pack_images=True
)

# 导入并合并顶点
bpy.ops.import_gltf(
    filepath='//model.glb',
    merge_vertices=True
)
```

### 图像处理选项

```python
# 打包图像到blend文件
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_pack_images=True
)

# 不打包图像
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_pack_images=False
)

# 保留外部纹理
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_pack_images=False
)
```

### 几何体处理

```python
# 合并顶点
bpy.ops.import_gltf(
    filepath='//model.glb',
    merge_vertices=True,
    tolerance=0.001
)

# 导入法线
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_normals=True
)

# 不导入法线
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_normals=False
)

# 导入变形目标
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_morph=True
)

# 导入顶点颜色
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_colors=True
)
```

### 材质和着色

```python
# 使用原始着色
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_shading='ORIGINAL'
)

# 转换为标准着色
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_shading='VERTEX'
)

# 转换为面着色
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_shading='FACE'
)

# 导入材质
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_materials=True
)

# 不导入材质
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_materials=False
)
```

### 导入修改器

```python
# 导入修改器
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_modifiers=True
)

# 不导入修改器
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_modifiers=False
)
```

### 动画导入

```python
# 导入动画
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_animations=True
)

# 不导入动画
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_animations=False
)

# 导入到当前帧
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_current_frame=True
)

# 设置导入帧范围
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_animations=True,
    import_frame_range=(1, 100)
)
```

### 相机和灯光

```python
# 导入相机
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_cameras=True
)

# 导入灯光
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_lights=True
)

# 不导入相机
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_cameras=False
)

# 不导入灯光
bpy.ops.import_gltf(
    filepath='//model.glb',
    import_lights=False
)
```

## 实用函数

```python
def import_gltf_model(filepath):
    """导入GLTF模型"""
    bpy.ops.import_gltf(
        filepath=filepath,
        import_pack_images=True,
        import_shading='ORIGINAL',
        import_materials=True,
        import_normals=True,
        import_animations=True,
        import_cameras=True,
        import_lights=True
    )
    return list(bpy.context.selected_objects)

def import_gltf_for_editing(filepath):
    """导入GLTF用于编辑"""
    bpy.ops.import_gltf(
        filepath=filepath,
        import_pack_images=False,
        merge_vertices=False,
        import_shading='ORIGINAL',
        import_materials=True,
        import_normals=True,
        import_modifiers=False,
        import_animations=False
    )
    return list(bpy.context.selected_objects)

def import_gltf_with_animations(filepath):
    """导入GLTF带动画"""
    bpy.ops.import_gltf(
        filepath=filepath,
        import_pack_images=True,
        import_shading='ORIGINAL',
        import_materials=True,
        import_normals=True,
        import_morph=True,
        import_animations=True,
        import_colors=True
    )
    return list(bpy.context.selected_objects)

def import_gltf_and_merge(filepath):
    """导入GLTF并合并顶点"""
    bpy.ops.import_gltf(
        filepath=filepath,
        merge_vertices=True,
        tolerance=0.0001,
        import_pack_images=False,
        import_materials=True
    )
    return list(bpy.context.selected_objects)

def batch_import_gltf_files(filepaths):
    """批量导入GLTF文件"""
    all_objects = []
    for filepath in filepaths:
        bpy.ops.import_gltf(filepath=filepath)
        imported = list(bpy.context.selected_objects)
        all_objects.extend(imported)
    return all_objects

def import_gltf_optimized(filepath):
    """导入优化版本"""
    bpy.ops.import_gltf(
        filepath=filepath,
        import_pack_images=True,
        merge_vertices=True,
        import_shading='ORIGINAL',
        import_normals=True,
        import_morph=True,
        import_colors=True,
        import_modifiers=True
    )
    return list(bpy.context.selected_objects)
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查选中对象
selected = bpy.context.selected_objects
print(f"导入对象数: {len(selected)}")

# 检查对象类型
for obj in selected:
    print(f"{obj.name}: 类型={obj.type}")

# 检查隐藏状态
for obj in selected:
    print(f"{obj.name}: 隐藏={obj.hide_viewport}")

# 显示所有对象
bpy.ops.object.select_all(action='SELECT')
for obj in bpy.context.selected_objects:
    obj.hide_viewport = False
```

### 纹理丢失
```python
# 检查材质
obj = bpy.context.active_object
if obj.data.materials:
    for mat in obj.data.materials:
        print(f"材质: {mat.name}")
        if mat.use_nodes:
            for node in mat.node_tree.nodes:
                if node.type == 'TEX_IMAGE':
                    print(f"纹理: {node.image}")

# 重新导入并打包图像
bpy.ops.import_gltf(filepath='//model.glb', import_pack_images=True)

# 检查纹理路径
print(f"纹理路径: {bpy.path.abspath('//')}")
```

### 动画不播放
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 检查帧范围
if obj.animation_data.action:
    print(f"帧范围: {obj.animation_data.action.frame_range}")

# 启用动画播放
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 250

# 检查NLA
for track in obj.animation_data.nla_tracks:
    print(f"NLA轨道: {track.name}")
```

### 材质显示问题
```python
# 检查着色设置
space = bpy.context.area.spaces.active
print(f"着色类型: {space.shading.type}")

# 切换到材质预览
space.shading.type = 'MATERIAL'

# 检查材质节点
mat = bpy.context.active_object.active_material
if mat and mat.use_nodes:
    bsdf = mat.node_tree.nodes.get('Principled BSDF')
    if bsdf:
        print(f"基础颜色: {bsdf.inputs['Base Color'].default_value}")

# 重新导入原始着色
bpy.ops.import_gltf(filepath='//model.glb', import_shading='ORIGINAL')
```


---
## bpy_ops_export_gltf

# bpy.ops.export_gltf - GLTF导出操作模块

Blender GLTF格式导出操作符，用于将场景导出为glTF/glb格式。

## 概述

`bpy.ops.export_gltf` 模块提供用于将Blender场景导出为glTF 2.0标准格式的功能，支持glb、gltf和glb嵌入式格式。

## 核心功能

### 基础导出

```python
import bpy

# 导出为GLB
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_format='GLB',
    export_selected=True,
    use_selection=True
)

# 导出为glTF
bpy.ops.export_gltf(
    filepath='//model.gltf',
    export_format='GLTF_SEPARATE',
    export_selected=False
)

# 导出嵌入式glTF
bpy.ops.export_gltf(
    filepath='//model.gltf',
    export_format='GLTF_EMBEDDED',
    export_selected=True
)
```

### 导出选项

```python
# 导出所有内容
bpy.ops.export_gltf(
    filepath='//scene.glb',
    export_format='GLB',
    export_selected=False
)

# 只导出选中对象
bpy.ops.export_gltf(
    filepath='//selected.glb',
    export_format='GLB',
    export_selected=True,
    use_selection=True
)

# 导出活动集合
bpy.ops.export_gltf(
    filepath='//collection.glb',
    export_format='GLB',
    export_active_collection=True
)

# 应用修改器
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_apply=True
)

# 应用变换
bpy.ops.export_gltf(
    filepath='//model.glb',
    apply_transforms=True
)
```

### 几何体导出选项

```python
# 导出UV
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_uv=True
)

# 导出法线
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_normals=True
)

# 导出变形目标
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_morph=True
)

# 导出网格数据
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_mesh=True
)

# 导出点云
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_pointcloud=True
)

# 导出顶点颜色
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_colors=True
)

# 三角化
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_triangulated_mesh=True
)
```

### 动画导出选项

```python
# 导出动画
bpy.ops.export_gltf(
    filepath='//animation.glb',
    export_animations=True
)

# 导出NLA轨道
bpy.ops.export_gltf(
    filepath='//animation.glb',
    export_nla_strips=True
)

# 导出所有动作
bpy.ops.export_gltf(
    filepath='//animations.glb',
    export_anim_action_all=True
)

# 导出活动动作
bpy.ops.export_gltf(
    filepath='//animation.glb',
    export_anim_action='ACTIVE'
)

# 导出骨骼
bpy.ops.export_gltf(
    filepath='//armature.glb',
    export_skins=True
)

# 导出所有影响
bpy.ops.export_gltf(
    filepath='//armature.glb',
    export_all_influences=True
)

# 骨骼仅变形
bpy.ops.export_gltf(
    filepath='//armature.glb',
    deform_bones_only=True
)

# 导出相机
bpy.ops.export_gltf(
    filepath='//scene.glb',
    export_cameras=True
)

# 导出灯光
bpy.ops.export_gltf(
    filepath='//scene.glb',
    export_lights=True
)
```

### PBR材质导出

```python
# 导出PBR材质
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_pbr_material=True
)

# 导出材质名称
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_material_names=True
)

# 导出纹理
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_textures=True
)
```

### 自定义属性导出

```python
# 导出自定义属性
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_custom_properties=True
)

# 导出Y轴向上
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_yup=True
)
```

## 实用函数

```python
def export_selected_for_web(filepath='//model.glb'):
    """导出选中对象用于Web"""
    bpy.ops.export_gltf(
        filepath=filepath,
        export_format='GLB',
        export_selected=True,
        use_selection=True,
        export_apply=True,
        export_uv=True,
        export_normals=True,
        export_morph=True,
        export_animations=True,
        export_skins=True,
        export_cameras=True,
        export_lights=True,
        export_pbr_material=True,
        export_textures=True
    )

def export_for_AR(filepath='//ar_model.glb'):
    """导出AR模型"""
    bpy.ops.export_gltf(
        filepath=filepath,
        export_format='GLB',
        export_selected=True,
        use_selection=True,
        export_apply=True,
        export_triangulated_mesh=True,
        export_animations=True,
        export_skins=True,
        export_morph=True
    )

def export_baked_textures(filepath='//baked.glb'):
    """导出烘焙纹理"""
    bpy.ops.export_gltf(
        filepath=filepath,
        export_format='GLB',
        export_selected=True,
        use_selection=True,
        export_apply=True,
        export_pbr_material=True,
        export_textures=True,
        export_uv=True
    )

def export_gltf_with_custom_props(filepath='//custom.glb'):
    """导出带自定义属性"""
    bpy.ops.export_gltf(
        filepath=filepath,
        export_format='GLB',
        export_selected=True,
        use_selection=True,
        export_custom_properties=True
    )

def batch_export_objects(objects, output_dir):
    """批量导出对象"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for obj in objects:
        if obj.type == 'MESH':
            bpy.ops.object.select_all(action='DESELECT')
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            filepath = os.path.join(output_dir, f'{obj.name}.glb')
            bpy.ops.export_gltf(
                filepath=filepath,
                export_format='GLB',
                export_selected=True,
                use_selection=True
            )
```

## 常见问题排查

### 导出后模型丢失
```python
# 检查选中对象
selected = [obj for obj in bpy.context.selected_objects]
print(f"选中: {len(selected)}")

# 检查对象类型
print(f"对象类型: {[obj.type for obj in selected]}")

# 确保对象可见
for obj in selected:
    print(f"{obj.name}: 隐藏={obj.hide_viewport}")

# 检查原点位置
obj = bpy.context.active_object
print(f"原点: {obj.location}")
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 检查NLA轨道
for track in obj.animation_data.nla_tracks:
    print(f"轨道: {track.name}")

# 确保导出动画选项
bpy.ops.export_gltf(
    filepath='//anim.glb',
    export_animations=True,
    export_anim_action_all=True
)
```

### 材质问题
```python
# 检查材质
obj = bpy.context.active_object
print(f"材质: {obj.data.materials if obj.data else 'N/A'}")

# 检查节点材质
if obj.data.materials:
    mat = obj.data.materials[0]
    print(f"使用节点: {mat.use_nodes}")

# 烘焙材质
bpy.ops.export_gltf(
    filepath='//model.glb',
    export_pbr_material=True,
    export_textures=True
)
```


---
## bpy_ops_import_abc

# bpy.ops.import_scene - Alembic导入操作模块

Blender Alembic文件导入操作符，用于将Alembic（.abc）格式文件导入Blender。

## 概述

`bpy.ops.import_scene.abc` 模块提供用于将Alembic格式文件导入Blender的功能，支持几何体、动画和材质的高保真导入。

## 核心功能

### 基础导入

```python
import bpy

# 导入Alembic文件
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.abc'}],
    ui_tab='MAIN',
    global_scale=1.0,
    unit_scale=1.0,
    use_custom_normals=True,
    use_uv=True,
    use_selection=False,
    visible_objects_only=False,
    import_guide=False,
    import_cameras=True,
    import_lights=True,
    import_materials=True,
    import_subdiv=False,
    import_anim=True,
    import_anim_named_stashes=False,
    import_anim_use_default=True,
    import_anim_range=True,
    fabrik_solve=False,
    reload=True
)
```

### 几何体导入

```python
# 导入UV
bpy.ops.import_scene.abc(
    filepath='//uv.abc',
    use_uv=True
)

# 不导入UV
bpy.ops.import_scene.abc(
    filepath='//no_uv.abc',
    use_uv=False
)

# 使用自定义法线
bpy.ops.import_scene.abc(
    filepath='//normals.abc',
    use_custom_normals=True
)

# 不使用自定义法线
bpy.ops.import_scene.abc(
    filepath='//no_normals.abc',
    use_custom_normals=False
)
```

### 缩放设置

```python
# 设置全局缩放
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    global_scale=1.0
)

# 设置单位缩放
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    unit_scale=1.0
)
```

### 选择导入

```python
# 导入选中对象
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    use_selection=True
)

# 导入所有对象
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    use_selection=False
)
```

### 可见性设置

```python
# 只导入可见对象
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    visible_objects_only=True
)

# 导入所有对象
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    visible_objects_only=False
)
```

### 引导设置

```python
# 导入引导
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    import_guide=True
)

# 不导入引导
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    import_guide=False
)
```

### 相机和灯光

```python
# 导入相机
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    import_cameras=True
)

# 不导入相机
bpy.ops.import_scene.abc(
    filepath='//no_cameras.abc',
    import_cameras=False
)

# 导入灯光
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    import_lights=True
)

# 不导入灯光
bpy.ops.import_scene.abc(
    filepath='//no_lights.abc',
    import_lights=False
)
```

### 材质导入

```python
# 导入材质
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    import_materials=True
)

# 不导入材质
bpy.ops.import_scene.abc(
    filepath='//no_materials.abc',
    import_materials=False
)
```

### 细分设置

```python
# 导入细分
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    import_subdiv=True
)

# 不导入细分
bpy.ops.import_scene.abc(
    filepath='//no_subdiv.abc',
    import_subdiv=False
)
```

### 动画导入

```python
# 导入动画
bpy.ops.import_scene.abc(
    filepath='//anim.abc',
    import_anim=True
)

# 不导入动画
bpy.ops.import_scene.abc(
    filepath='//static.abc',
    import_anim=False
)

# 导入命名缓存
bpy.ops.import_scene.abc(
    filepath='//stashes.abc',
    import_anim=True,
    import_anim_named_stashes=True
)

# 使用默认动画
bpy.ops.import_scene.abc(
    filepath='//anim.abc',
    import_anim=True,
    import_anim_use_default=True
)

# 使用帧范围
bpy.ops.import_scene.abc(
    filepath='//anim.abc',
    import_anim=True,
    import_anim_range=True
)
```

### FABRIK设置

```python
# 使用FABRIK求解
bpy.ops.import_scene.abc(
    filepath='//fabrik.abc',
    fabrik_solve=True
)

# 不使用FABRIK
bpy.ops.import_scene.abc(
    filepath='//no_fabrik.abc',
    fabrik_solve=False
)
```

### 重载设置

```python
# 启用重载
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    reload=True
)

# 禁用重载
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    reload=False
)
```

## 实用函数

```python
def import_abc_model(filepath, scale=1.0):
    """导入Alembic模型"""
    bpy.ops.import_scene.abc(
        filepath=filepath,
        global_scale=scale,
        use_custom_normals=True,
        use_uv=True,
        import_cameras=True,
        import_lights=True,
        import_materials=True,
        import_anim=False
    )
    return list(bpy.context.selected_objects)

def import_abc_with_animations(filepath, scale=1.0):
    """导入Alembic带动画"""
    bpy.ops.import_scene.abc(
        filepath=filepath,
        global_scale=scale,
        use_custom_normals=True,
        use_uv=True,
        import_cameras=True,
        import_lights=True,
        import_materials=True,
        import_anim=True,
        import_anim_use_default=True
    )
    return list(bpy.context.selected_objects)

def import_abc_for_editing(filepath):
    """导入Alembic用于编辑"""
    bpy.ops.import_scene.abc(
        filepath=filepath,
        global_scale=1.0,
        use_custom_normals=True,
        use_uv=True,
        import_cameras=False,
        import_lights=False,
        import_materials=False,
        import_anim=False
    )
    return list(bpy.context.selected_objects)

def import_abc_character(filepath):
    """导入Alembic角色"""
    bpy.ops.import_scene.abc(
        filepath=filepath,
        global_scale=1.0,
        use_custom_normals=True,
        use_uv=True,
        import_cameras=True,
        import_lights=True,
        import_materials=True,
        import_anim=True,
        import_anim_use_default=True,
        import_anim_range=True
    )
    return list(bpy.context.selected_objects)

def batch_import_abc_files(abc_files, scale=1.0):
    """批量导入Alembic文件"""
    all_objects = []
    for filepath in abc_files:
        bpy.ops.import_scene.abc(
            filepath=filepath,
            global_scale=scale
        )
        imported = list(bpy.context.selected_objects)
        all_objects.extend(imported)
    return all_objects

def import_abc_and_merge(filepath):
    """导入Alembic并合并"""
    bpy.ops.import_scene.abc(
        filepath=filepath,
        global_scale=1.0,
        use_custom_normals=True
    )
    
    objects = list(bpy.context.selected_objects)
    if len(objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return objects
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查选中对象
selected = bpy.context.selected_objects
print(f"导入对象数: {len(selected)}")

# 检查对象类型
for obj in selected:
    print(f"{obj.name}: 类型={obj.type}")

# 检查隐藏状态
for obj in selected:
    print(f"{obj.name}: 隐藏={obj.hide_viewport}")
    obj.hide_viewport = False

# 检查显示类型
for obj in selected:
    obj.display_type = 'SOLID'
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 检查骨骼动画
if obj.type == 'ARMATURE':
    print(f"骨骼数: {len(obj.data.bones)}")

# 重新导入带动画
bpy.ops.import_scene.abc(
    filepath='//anim.abc',
    import_anim=True,
    import_anim_use_default=True
)
```

### 材质丢失
```python
# 检查材质
print(f"材质数: {len(bpy.data.materials)}")

# 检查纹理
print(f"纹理数: {len(bpy.data.textures)}")

# 重新导入带材质
bpy.ops.import_scene.abc(
    filepath='//mat.abc',
    import_materials=True,
    import_anim=False
)
```

### 比例问题
```python
# 检查导入比例
obj = bpy.context.active_object
print(f"尺寸: {obj.dimensions}")

# 检查单位设置
print(f"场景单位: {bpy.context.scene.unit_settings.system}")

# 重新导入带正确比例
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    global_scale=0.01,
    unit_scale=1.0
)
```

### 性能问题
```python
# 检查对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 减少导入内容
bpy.ops.import_scene.abc(
    filepath='//model.abc',
    use_selection=True,
    visible_objects_only=True,
    import_cameras=False,
    import_lights=False,
    import_anim=False
)

# 导入后优化
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.join()
```


---
## bpy_ops_export_abc

# bpy.ops.export_scene - Alembic导出操作模块

Blender Alembic文件导出操作符，用于将场景导出为Alembic（.abc）格式。

## 概述

`bpy.ops.export_scene.abc` 模块提供用于将Blender场景导出为Alembic格式的功能，支持几何体、动画和材质的高保真导出。

## 核心功能

### 基础导出

```python
import bpy

# 导出Alembic文件
bpy.ops.export_scene.abc(
    filepath='//model.abc',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=False,
    export_uv=True,
    export_normals=True,
    export_children=True,
    export_parent_curves=True,
    use_instancing=True,
    export_global_scale=1.0,
    axis_forward='Z',
    axis_up='Y',
    apply_subdiv=False,
    export_anim=True,
    export_anim_named_stashes=False,
    export_anim_only=False,
    export_anim_use_frame_range=True,
    export_anim_use_nla_strips=True,
    export_anim_free_image_path=False,
    export_visibility=True,
    export_cameras=True,
    export_lights=True,
    export_materials=True,
    export_packed_images=True,
    export_texture_compaction=True,
    custom_props=True,
    export_gps_shader=True
)
```

### 导出范围

```python
# 只导出选中对象
bpy.ops.export_scene.abc(
    filepath='//selected.abc',
    export_selected=True
)

# 导出所有对象
bpy.ops.export_scene.abc(
    filepath='//all.abc',
    export_selected=False
)
```

### 几何体导出

```python
# 导出UV
bpy.ops.export_scene.abc(
    filepath='//uv.abc',
    export_uv=True
)

# 不导出UV
bpy.ops.export_scene.abc(
    filepath='//no_uv.abc',
    export_uv=False
)

# 导出法线
bpy.ops.export_scene.abc(
    filepath='//normals.abc',
    export_normals=True
)

# 不导出法线
bpy.ops.export_scene.abc(
    filepath='//no_normals.abc',
    export_normals=False
)

# 导出子对象
bpy.ops.export_scene.abc(
    filepath='//children.abc',
    export_children=True
)

# 不导出子对象
bpy.ops.export_scene.abc(
    filepath='//no_children.abc',
    export_children=False
)
```

### 曲线导出

```python
# 导出父级曲线
bpy.ops.export_scene.abc(
    filepath='//curves.abc',
    export_parent_curves=True
)

# 不导出父级曲线
bpy.ops.export_scene.abc(
    filepath='//no_curves.abc',
    export_parent_curves=False
)
```

### 实例导出

```python
# 使用实例
bpy.ops.export_scene.abc(
    filepath='//instanced.abc',
    use_instancing=True
)

# 不使用实例
bpy.ops.export_scene.abc(
    filepath='//no_instanced.abc',
    use_instancing=False
)
```

### 缩放设置

```python
# 设置全局缩放
bpy.ops.export_scene.abc(
    filepath='//scaled.abc',
    export_global_scale=1.0
)

# 缩放10倍
bpy.ops.export_scene.abc(
    filepath='//scaled.abc',
    export_global_scale=10.0
)
```

### 坐标轴设置

```python
# Blender默认轴向
bpy.ops.export_scene.abc(
    filepath='//model.abc',
    axis_forward='Z',
    axis_up='Y'
)

# 其他轴向组合
bpy.ops.export_scene.abc(
    filepath='//model.abc',
    axis_forward='-Z',
    axis_up='Y'
)
```

### 细分设置

```python
# 应用细分
bpy.ops.export_scene.abc(
    filepath='//subdiv.abc',
    apply_subdiv=True
)

# 不应用细分
bpy.ops.export_scene.abc(
    filepath='//no_subdiv.abc',
    apply_subdiv=False
)
```

### 动画设置

```python
# 导出动画
bpy.ops.export_scene.abc(
    filepath='//anim.abc',
    export_anim=True
)

# 不导出动画
bpy.ops.export_scene.abc(
    filepath='//static.abc',
    export_anim=False
)

# 只导出动画
bpy.ops.export_scene.abc(
    filepath='//anim_only.abc',
    export_anim=True,
    export_anim_only=True
)

# 导出NLA轨道
bpy.ops.export_scene.abc(
    filepath='//nla.abc',
    export_anim=True,
    export_anim_use_nla_strips=True
)

# 导出命名缓存
bpy.ops.export_scene.abc(
    filepath='//stashes.abc',
    export_anim=True,
    export_anim_named_stashes=True
)
```

### 帧范围

```python
# 使用帧范围
bpy.ops.export_scene.abc(
    filepath='//range.abc',
    export_anim=True,
    export_anim_use_frame_range=True
)

# 设置自定义帧范围
# 通过scene.frame_start和scene.frame_end设置
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 100
```

### 能见度导出

```python
# 导出能见度
bpy.ops.export_scene.abc(
    filepath='//visibility.abc',
    export_visibility=True
)

# 不导出能见度
bpy.ops.export_scene.abc(
    filepath='//no_visibility.abc',
    export_visibility=False
)
```

### 相机和灯光

```python
# 导出相机
bpy.ops.export_scene.abc(
    filepath='//cameras.abc',
    export_cameras=True
)

# 不导出相机
bpy.ops.export_scene.abc(
    filepath='//no_cameras.abc',
    export_cameras=False
)

# 导出灯光
bpy.ops.export_scene.abc(
    filepath='//lights.abc',
    export_lights=True
)

# 不导出灯光
bpy.ops.export_scene.abc(
    filepath='//no_lights.abc',
    export_lights=False
)
```

### 材质和纹理

```python
# 导出材质
bpy.ops.export_scene.abc(
    filepath='//materials.abc',
    export_materials=True
)

# 不导出材质
bpy.ops.export_scene.abc(
    filepath='//no_materials.abc',
    export_materials=False
)

# 打包图像
bpy.ops.export_scene.abc(
    filepath='//packed.abc',
    export_packed_images=True
)

# 不打包图像
bpy.ops.export_scene.abc(
    filepath='//no_packed.abc',
    export_packed_images=False
)

# 纹理压缩
bpy.ops.export_scene.abc(
    filepath='//compact.abc',
    export_texture_compaction=True
)
```

### 自定义属性

```python
# 导出自定义属性
bpy.ops.export_scene.abc(
    filepath='//props.abc',
    custom_props=True
)

# 不导出自定义属性
bpy.ops.export_scene.abc(
    filepath='//no_props.abc',
    custom_props=False
)
```

### GPU着色器

```python
# 导出GPU着色器
bpy.ops.export_scene.abc(
    filepath='//shader.abc',
    export_gps_shader=True
)

# 不导出GPU着色器
bpy.ops.export_scene.abc(
    filepath='//no_shader.abc',
    export_gps_shader=False
)
```

## 实用函数

```python
def export_abc_model(filepath, apply_modifiers=True):
    """导出Alembic模型"""
    bpy.ops.export_scene.abc(
        filepath=filepath,
        export_selected=True,
        export_uv=True,
        export_normals=True,
        export_anim=False,
        export_cameras=True,
        export_lights=True
    )

def export_abc_animation(filepath, start_frame=None, end_frame=None):
    """导出Alembic动画"""
    if start_frame is not None:
        bpy.context.scene.frame_start = start_frame
    if end_frame is not None:
        bpy.context.scene.frame_end = end_frame
    
    bpy.ops.export_scene.abc(
        filepath=filepath,
        export_selected=True,
        export_uv=True,
        export_normals=True,
        export_anim=True,
        export_anim_use_frame_range=True,
        export_anim_use_nla_strips=True,
        export_cameras=True,
        export_lights=True
    )

def export_abc_for_vfx(filepath):
    """导出Alembic用于VFX"""
    bpy.ops.export_scene.abc(
        filepath=filepath,
        export_selected=True,
        export_uv=True,
        export_normals=True,
        export_children=True,
        use_instancing=True,
        export_anim=True,
        export_anim_use_frame_range=True,
        export_anim_use_nla_strips=True,
        export_visibility=True,
        export_cameras=True,
        export_lights=True,
        export_materials=True,
        custom_props=True
    )

def export_abc_full_scene(filepath):
    """导出完整场景"""
    bpy.ops.export_scene.abc(
        filepath=filepath,
        export_selected=False,
        export_uv=True,
        export_normals=True,
        export_children=True,
        export_anim=True,
        export_anim_use_frame_range=True,
        export_visibility=True,
        export_cameras=True,
        export_lights=True,
        export_materials=True,
        custom_props=True
    )

def batch_export_abc(objects, output_dir):
    """批量导出Alembic"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for obj in objects:
        bpy.ops.object.select_all(action='DESELECT')
        obj.select_set(True)
        bpy.context.view_layer.objects.active = obj
        
        filepath = os.path.join(output_dir, f'{obj.name}.abc')
        bpy.ops.export_scene.abc(
            filepath=filepath,
            export_selected=True,
            export_uv=True,
            export_normals=True,
            export_anim=True
        )
```

## 常见问题排查

### 导出文件过大
```python
# 检查对象数
meshes = [obj for obj in bpy.data.objects if obj.type == 'MESH']
print(f"网格数: {len(meshes)}")

# 检查顶点总数
total_verts = sum(len(obj.data.vertices) for obj in meshes)
print(f"总顶点数: {total_verts}")

# 建议优化
# 1. 减少导出精度
# 2. 使用实例
# 3. 禁用UV和法线导出
bpy.ops.export_scene.abc(
    filepath='//optimized.abc',
    export_uv=False,
    export_normals=False
)
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")
    if obj.animation_data.action:
        print(f"帧范围: {obj.animation_data.action.frame_range}")

# 检查NLA轨道
if obj.animation_data:
    for track in obj.animation_data.nla_tracks:
        print(f"NLA轨道: {track.name}")

# 确保导出动画选项
bpy.ops.export_scene.abc(
    filepath='//anim.abc',
    export_anim=True,
    export_anim_use_frame_range=True,
    export_anim_use_nla_strips=True
)
```

### 材质丢失
```python
# 检查材质
obj = bpy.context.active_object
print(f"材质数: {len(obj.data.materials) if obj.data else 0}")

# 确保导出材质
bpy.ops.export_scene.abc(
    filepath='//mat.abc',
    export_selected=True,
    export_materials=True,
    export_packed_images=True
)
```

### 坐标轴问题
```python
# 检查导出后的方向
# 在第三方软件中打开ABC文件检查

# 尝试不同轴向
bpy.ops.export_scene.abc(
    filepath='//zy.abc',
    axis_forward='Z',
    axis_up='Y'
)

bpy.ops.export_scene.abc(
    filepath='//-zy.abc',
    axis_forward='-Z',
    axis_up='Y'
)
```

### 性能问题
```python
# 检查帧范围
print(f"帧范围: {bpy.context.scene.frame_start} - {bpy.context.scene.frame_end}")

# 减少帧数
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 100

# 禁用高精度选项
bpy.ops.export_scene.abc(
    filepath='//fast.abc',
    export_uv=False,
    export_normals=False,
    export_children=False,
    custom_props=False
)
```


---
## bpy_ops_import_dae

# bpy.ops.import_scene - COLLADA导入操作模块

Blender COLLADA（DAE）文件导入操作符，用于将COLLADA格式文件导入Blender。

## 概述

`bpy.ops.import_scene.dae` 模块提供用于将COLLADA（DAE）格式文件导入Blender的功能，支持几何体、材质、纹理、骨骼和动画的导入。

## 核心功能

### 基础导入

```python
import bpy

# 导入DAE文件
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.dae'}],
    ui_tab='MAIN',
    import_units=False,
    import_animation=True,
    import_cameras=True,
    import_lights=True,
    import_meshes=True,
    import_curves=True,
    import_surfaces=True,
    import_nurbs=True,
    fix_orientation=True,
    axis_forward='Z',
    axis_up='Y',
    import_uv=True,
    import_normals=True,
    import_materials=True,
    import_textures=True,
    import_blendShapes=True,
    import_animations=True,
    animation_mode='REPLACE',
    import_force_end_frame=False,
    custom_normals=True
)
```

### 单位设置

```python
# 导入单位
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    import_units=True
)

# 不导入单位
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    import_units=False
)
```

### 动画导入

```python
# 导入动画
bpy.ops.import_scene.dae(
    filepath='//anim.dae',
    import_animation=True
)

# 不导入动画
bpy.ops.import_scene.dae(
    filepath='//static.dae',
    import_animation=False
)

# 导入动画类型
bpy.ops.import_scene.dae(
    filepath='//anim.dae',
    import_animations=True,
    animation_mode='REPLACE'
)

# 附加动画
bpy.ops.import_scene.dae(
    filepath='//anim.dae',
    import_animations=True,
    animation_mode='APPEND'
)

# 强制结束帧
bpy.ops.import_scene.dae(
    filepath='//anim.dae',
    import_animation=True,
    import_force_end_frame=True
)
```

### 几何体导入

```python
# 导入网格
bpy.ops.import_scene.dae(
    filepath='//meshes.dae',
    import_meshes=True
)

# 不导入网格
bpy.ops.import_scene.dae(
    filepath='//no_meshes.dae',
    import_meshes=False
)

# 导入曲线
bpy.ops.import_scene.dae(
    filepath='//curves.dae',
    import_curves=True
)

# 不导入曲线
bpy.ops.import_scene.dae(
    filepath='//no_curves.dae',
    import_curves=False
)

# 导入曲面
bpy.ops.import_scene.dae(
    filepath='//surfaces.dae',
    import_surfaces=True
)

# 不导入曲面
bpy.ops.import_scene.dae(
    filepath='//no_surfaces.dae',
    import_surfaces=False
)

# 导入NURBS
bpy.ops.import_scene.dae(
    filepath='//nurbs.dae',
    import_nurbs=True
)

# 不导入NURBS
bpy.ops.import_scene.dae(
    filepath='//no_nurbs.dae',
    import_nurbs=False
)
```

### 相机导入

```python
# 导入相机
bpy.ops.import_scene.dae(
    filepath='//cameras.dae',
    import_cameras=True
)

# 不导入相机
bpy.ops.import_scene.dae(
    filepath='//no_cameras.dae',
    import_cameras=False
)
```

### 灯光导入

```python
# 导入灯光
bpy.ops.import_scene.dae(
    filepath='//lights.dae',
    import_lights=True
)

# 不导入灯光
bpy.ops.import_scene.dae(
    filepath='//no_lights.dae',
    import_lights=False
)
```

### 方向修正

```python
# 修正方向
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    fix_orientation=True
)

# 不修正方向
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    fix_orientation=False
)
```

### 坐标轴设置

```python
# 设置前向轴
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    axis_forward='Z'
)

# 设置上向轴
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    axis_up='Y'
)

# 其他轴向组合
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    axis_forward='-Z',
    axis_up='Y'
)
```

### UV导入

```python
# 导入UV
bpy.ops.import_scene.dae(
    filepath='//uv.dae',
    import_uv=True
)

# 不导入UV
bpy.ops.import_scene.dae(
    filepath='//no_uv.dae',
    import_uv=False
)
```

### 法线导入

```python
# 导入法线
bpy.ops.import_scene.dae(
    filepath='//normals.dae',
    import_normals=True
)

# 不导入法线
bpy.ops.import_scene.dae(
    filepath='//no_normals.dae',
    import_normals=False
)

# 自定义法线
bpy.ops.import_scene.dae(
    filepath='//custom_normals.dae',
    custom_normals=True
)
```

### 材质导入

```python
# 导入材质
bpy.ops.import_scene.dae(
    filepath='//materials.dae',
    import_materials=True
)

# 不导入材质
bpy.ops.import_scene.dae(
    filepath='//no_materials.dae',
    import_materials=False
)

# 导入纹理
bpy.ops.import_scene.dae(
    filepath='//textures.dae',
    import_textures=True
)

# 不导入纹理
bpy.ops.import_scene.dae(
    filepath='//no_textures.dae',
    import_textures=False
)
```

### 混合形状导入

```python
# 导入混合形状
bpy.ops.import_scene.dae(
    filepath='//blendshapes.dae',
    import_blendShapes=True
)

# 不导入混合形状
bpy.ops.import_scene.dae(
    filepath='//no_blendshapes.dae',
    import_blendShapes=False
)
```

## 实用函数

```python
def import_dae_model(filepath):
    """导入DAE模型"""
    bpy.ops.import_scene.dae(
        filepath=filepath,
        import_animation=False,
        import_cameras=True,
        import_lights=True,
        import_meshes=True,
        import_uv=True,
        import_normals=True,
        import_materials=True,
        import_textures=True
    )
    return list(bpy.context.selected_objects)

def import_dae_with_animations(filepath):
    """导入DAE带动画"""
    bpy.ops.import_scene.dae(
        filepath=filepath,
        import_animation=True,
        import_animations=True,
        animation_mode='REPLACE',
        import_cameras=True,
        import_lights=True,
        import_meshes=True,
        import_uv=True,
        import_normals=True,
        import_materials=True,
        import_textures=True,
        import_blendShapes=True
    )
    return list(bpy.context.selected_objects)

def import_dae_for_editing(filepath):
    """导入DAE用于编辑"""
    bpy.ops.import_scene.dae(
        filepath=filepath,
        import_animation=False,
        import_cameras=False,
        import_lights=False,
        import_meshes=True,
        import_uv=True,
        import_normals=True,
        import_materials=False,
        import_textures=False
    )
    return list(bpy.context.selected_objects)

def import_dae_character(filepath):
    """导入DAE角色"""
    bpy.ops.import_scene.dae(
        filepath=filepath,
        import_animation=True,
        import_animations=True,
        animation_mode='REPLACE',
        import_cameras=True,
        import_lights=True,
        import_meshes=True,
        import_uv=True,
        import_normals=True,
        import_materials=True,
        import_textures=True,
        import_blendShapes=True
    )
    return list(bpy.context.selected_objects)

def import_dae_and_merge(filepath):
    """导入DAE并合并"""
    bpy.ops.import_scene.dae(
        filepath=filepath,
        import_animation=False,
        import_cameras=False,
        import_lights=False,
        import_meshes=True
    )
    
    objects = list(bpy.context.selected_objects)
    if len(objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return objects

def batch_import_dae_files(dae_files):
    """批量导入DAE文件"""
    all_objects = []
    for filepath in dae_files:
        bpy.ops.import_scene.dae(filepath=filepath)
        imported = list(bpy.context.selected_objects)
        all_objects.extend(imported)
    return all_objects
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查选中对象
selected = bpy.context.selected_objects
print(f"导入对象数: {len(selected)}")

# 检查对象类型
for obj in selected:
    print(f"{obj.name}: 类型={obj.type}")

# 检查隐藏状态
for obj in selected:
    print(f"{obj.name}: 隐藏={obj.hide_viewport}")
    obj.hide_viewport = False

# 检查显示类型
for obj in selected:
    obj.display_type = 'SOLID'
```

### 材质丢失
```python
# 检查材质
print(f"材质数: {len(bpy.data.materials)}")

# 检查纹理
print(f"纹理数: {len(bpy.data.textures)}")

# 检查图像
print(f"图像数: {len(bpy.data.images)}")

# 重新导入带材质
bpy.ops.import_scene.dae(
    filepath='//mat.dae',
    import_materials=True,
    import_textures=True
)
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 检查骨骼动画
if obj.type == 'ARMATURE':
    print(f"骨骼数: {len(obj.data.bones)}")

# 重新导入带动画
bpy.ops.import_scene.dae(
    filepath='//anim.dae',
    import_animation=True,
    import_animations=True,
    animation_mode='REPLACE'
)
```

### 骨骼问题
```python
# 检查骨骼
armature = None
for obj in bpy.context.selected_objects:
    if obj.type == 'ARMATURE':
        armature = obj
        break

if armature:
    print(f"骨骼数: {len(armature.data.bones)}")
    
    # 检查权重
    for child in armature.children:
        if child.type == 'MESH':
            print(f"子对象: {child.name}")
            for mod in child.modifiers:
                if mod.type == 'ARMATURE':
                    print(f"绑定到: {mod.object}")

# 重新导入带骨骼
bpy.ops.import_scene.dae(
    filepath='//armature.dae',
    import_animation=True,
    import_meshes=True
)
```

### 混合形状丢失
```python
# 检查混合形状
obj = bpy.context.active_object
if hasattr(obj.data, 'shape_keys'):
    print(f"混合形状: {obj.data.shape_keys}")

# 确保导入混合形状
bpy.ops.import_scene.dae(
    filepath='//blendshapes.dae',
    import_meshes=True,
    import_blendShapes=True
)
```

### 方向问题
```python
# 检查导入的变换
obj = bpy.context.active_object
print(f"位置: {obj.location}")
print(f"旋转: {obj.rotation_euler}")
print(f"缩放: {obj.scale}")

# 尝试不同轴向
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    axis_forward='Z',
    axis_up='Y',
    fix_orientation=True
)

bpy.ops.import_scene.dae(
    filepath='//model.dae',
    axis_forward='-Z',
    axis_up='Y',
    fix_orientation=False
)
```

### 性能问题
```python
# 检查对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 减少导入内容
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    import_cameras=False,
    import_lights=False,
    import_curves=False,
    import_surfaces=False,
    import_nurbs=False,
    import_animation=False
)

# 导入后优化
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.join()
```

### 单位问题
```python
# 检查单位
print(f"场景单位: {bpy.context.scene.unit_settings.system}")
print(f"比例: {bpy.context.scene.unit_settings.scale_length}")

# 导入单位
bpy.ops.import_scene.dae(
    filepath='//model.dae',
    import_units=True
)

# 手动调整
obj = bpy.context.active_object
obj.scale = (0.01, 0.01, 0.01)
bpy.ops.object.transform_apply(scale=True)
```


---
## bpy_ops_export_dae

# bpy.ops.export_scene - COLLADA导出操作模块

Blender COLLADA（DAE）文件导出操作符，用于将场景导出为COLLADA格式。

## 概述

`bpy.ops.export_scene.dae` 模块提供用于将Blender场景导出为COLLADA（DAE）格式的功能，支持几何体、材质、纹理、骨骼和动画的导出。

## 核心功能

### 基础导出

```python
import bpy

# 导出DAE文件
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=False,
    use_triangles=False,
    use_cameras=True,
    use_lights=True,
    use_selection=False,
    use_visible=True,
    use_active_objects=False,
    use_export_nurbs=True,
    use_mesh_modifiers=True,
    use_yup=True,
    apply_modifiers=True,
    export_uv=True,
    export_normals=True,
    export_cameras=True,
    export_lights=True,
    export_materials=True,
    export_textures=True,
    export_cubes_as_cubes=False,
    bake_space_transform=False,
    max_file_size=0.0,
    export_frame_range=True,
    export_frame_step=1,
    export_anim=True,
    export_anim_action_only=False,
    export_anim_action_name='',
    export_animation_type='ANIMATION',
    export_baked_animations=True,
    export_baked_matrix=True,
    export_embedded=False,
    export_apply_scale='FBX_SCALE_ALL',
    export_metallic_roughness=True,
    export_original_specular=True,
    export_node_name_from_object=True,
    export_extra_images=False
)
```

### 导出范围

```python
# 只导出选中对象
bpy.ops.export_scene.dae(
    filepath='//selected.dae',
    export_selected=True
)

# 导出所有对象
bpy.ops.export_scene.dae(
    filepath='//all.dae',
    export_selected=False
)
```

### 几何体导出

```python
# 三角化
bpy.ops.export_scene.dae(
    filepath='//triangulated.dae',
    use_triangles=True
)

# 不三角化
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    use_triangles=False
)

# 导出UV
bpy.ops.export_scene.dae(
    filepath='//uv.dae',
    export_uv=True
)

# 不导出UV
bpy.ops.export_scene.dae(
    filepath='//no_uv.dae',
    export_uv=False
)

# 导出法线
bpy.ops.export_scene.dae(
    filepath='//normals.dae',
    export_normals=True
)

# 不导出法线
bpy.ops.export_scene.dae(
    filepath='//no_normals.dae',
    export_normals=False
)
```

### 修改器设置

```python
# 应用修改器
bpy.ops.export_scene.dae(
    filepath='//applied.dae',
    use_mesh_modifiers=True
)

# 不应用修改器
bpy.ops.export_scene.dae(
    filepath='//not_applied.dae',
    use_mesh_modifiers=False
)

# 应用修改器变换
bpy.ops.export_scene.dae(
    filepath='//modifiers.dae',
    apply_modifiers=True
)
```

### 坐标轴设置

```python
# Y轴向上
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    use_yup=True
)

# Z轴向上
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    use_yup=False
)
```

### 相机和灯光

```python
# 导出相机
bpy.ops.export_scene.dae(
    filepath='//cameras.dae',
    export_cameras=True
)

# 不导出相机
bpy.ops.export_scene.dae(
    filepath='//no_cameras.dae',
    export_cameras=False
)

# 导出灯光
bpy.ops.export_scene.dae(
    filepath='//lights.dae',
    export_lights=True
)

# 不导出灯光
bpy.ops.export_scene.dae(
    filepath='//no_lights.dae',
    export_lights=False
)
```

### 材质和纹理

```python
# 导出材质
bpy.ops.export_scene.dae(
    filepath='//materials.dae',
    export_materials=True
)

# 不导出材质
bpy.ops.export_scene.dae(
    filepath='//no_materials.dae',
    export_materials=False
)

# 导出纹理
bpy.ops.export_scene.dae(
    filepath='//textures.dae',
    export_textures=True
)

# 不导出纹理
bpy.ops.export_scene.dae(
    filepath='//no_textures.dae',
    export_textures=False
)
```

### 可见性设置

```python
# 导出可见对象
bpy.ops.export_scene.dae(
    filepath='//visible.dae',
    use_visible=True
)

# 导出活动对象
bpy.ops.export_scene.dae(
    filepath='//active.dae',
    use_active_objects=True
)
```

### 曲线导出

```python
# 导出NURBS
bpy.ops.export_scene.dae(
    filepath='//nurbs.dae',
    use_export_nurbs=True
)

# 不导出NURBS
bpy.ops.export_scene.dae(
    filepath='//no_nurbs.dae',
    use_export_nurbs=False
)
```

### 立方体导出

```python
# 立方体导出为立方体
bpy.ops.export_scene.dae(
    filepath='//cubes.dae',
    export_cubes_as_cubes=True
)

# 立方体导出为其他
bpy.ops.export_scene.dae(
    filepath='//no_cubes.dae',
    export_cubes_as_cubes=False
)
```

### 空间变换

```python
# 烘焙空间变换
bpy.ops.export_scene.dae(
    filepath='//baked.dae',
    bake_space_transform=True
)

# 不烘焙空间变换
bpy.ops.export_scene.dae(
    filepath='//no_baked.dae',
    bake_space_transform=False
)
```

### 文件大小限制

```python
# 无限制
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    max_file_size=0.0
)

# 设置最大文件大小
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    max_file_size=100.0
)
```

### 动画设置

```python
# 导出动画
bpy.ops.export_scene.dae(
    filepath='//anim.dae',
    export_anim=True
)

# 不导出动画
bpy.ops.export_scene.dae(
    filepath='//static.dae',
    export_anim=False
)

# 只导出动作
bpy.ops.export_scene.dae(
    filepath='//action.dae',
    export_anim=True,
    export_anim_action_only=True
)

# 指定动作名称
bpy.ops.export_scene.dae(
    filepath='//named.dae',
    export_anim=True,
    export_anim_action_name='WalkCycle'
)

# 动画类型
bpy.ops.export_scene.dae(
    filepath='//animation.dae',
    export_animation_type='ANIMATION'
)

# 烘焙动画
bpy.ops.export_scene.dae(
    filepath='//baked.dae',
    export_anim=True,
    export_baked_animations=True
)

# 烘焙矩阵
bpy.ops.export_scene.dae(
    filepath='//matrix.dae',
    export_anim=True,
    export_baked_matrix=True
)
```

### 帧范围

```python
# 使用帧范围
bpy.ops.export_scene.dae(
    filepath='//range.dae',
    export_frame_range=True
)

# 设置帧步长
bpy.ops.export_scene.dae(
    filepath='//step.dae',
    export_frame_step=1
)
```

### 嵌入选项

```python
# 不嵌入
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    export_embedded=False
)
```

### 缩放应用

```python
# 应用所有缩放
bpy.ops.export_scene.dae(
    filepath='//scaled.dae',
    export_apply_scale='FBX_SCALE_ALL'
)

# 不应用缩放
bpy.ops.export_scene.dae(
    filepath='//no_scale.dae',
    export_apply_scale='FBX_SCALE_NONE'
)
```

### 材质属性

```python
# 导出金属度粗糙度
bpy.ops.export_scene.dae(
    filepath='//mr.dae',
    export_metallic_roughness=True
)

# 导出原始高光
bpy.ops.export_scene.dae(
    filepath='//specular.dae',
    export_original_specular=True
)
```

### 节点名称

```python
# 从对象名导出节点名
bpy.ops.export_scene.dae(
    filepath='//named.dae',
    export_node_name_from_object=True
)

# 不从对象名导出
bpy.ops.export_scene.dae(
    filepath='//no_name.dae',
    export_node_name_from_object=False
)
```

### 额外图像

```python
# 导出额外图像
bpy.ops.export_scene.dae(
    filepath='//extra.dae',
    export_extra_images=True
)

# 不导出额外图像
bpy.ops.export_scene.dae(
    filepath='//no_extra.dae',
    export_extra_images=False
)
```

## 实用函数

```python
def export_dae_model(filepath, apply_modifiers=True):
    """导出DAE模型"""
    bpy.ops.export_scene.dae(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=apply_modifiers,
        use_yup=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_textures=True,
        export_anim=False
    )

def export_dae_with_animations(filepath):
    """导出DAE带动画"""
    bpy.ops.export_scene.dae(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=True,
        use_yup=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_textures=True,
        export_anim=True,
        export_baked_animations=True
    )

def export_dae_for_sketchfab(filepath):
    """导出DAE用于Sketchfab"""
    bpy.ops.export_scene.dae(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=True,
        use_yup=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_textures=True,
        export_anim=True,
        export_baked_animations=True,
        export_cameras=True,
        export_lights=True
    )

def export_dae_for_web(filepath):
    """导出DAE用于Web"""
    bpy.ops.export_scene.dae(
        filepath=filepath,
        export_selected=True,
        use_mesh_modifiers=True,
        use_yup=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_textures=True,
        export_anim=False,
        use_triangles=True
    )

def batch_export_dae_by_collection(output_dir):
    """按集合批量导出DAE"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for collection in bpy.data.collections:
        bpy.ops.object.select_all(action='DESELECT')
        
        for obj in collection.objects:
            obj.select_set(True)
        
        if bpy.context.selected_objects:
            bpy.context.view_layer.active_layer_collection = bpy.context.layer_collection.children[collection.name]
            filepath = os.path.join(output_dir, f'{collection.name}.dae')
            bpy.ops.export_scene.dae(
                filepath=filepath,
                export_selected=True,
                use_mesh_modifiers=True,
                use_yup=True,
                export_uv=True,
                export_normals=True,
                export_materials=True
            )
```

## 常见问题排查

### 导出文件无法打开
```python
# 检查文件路径
filepath = '//model.dae'
print(f"文件存在: {bpy.path.exists(filepath)}")

# 检查文件大小
import os
file_size = os.path.getsize(bpy.path.abspath(filepath))
print(f"文件大小: {file_size} 字节")

# 尝试不同的导出选项
bpy.ops.export_scene.dae(
    filepath='//model.dae',
    use_yup=True,
    export_embedded=False
)
```

### 材质丢失
```python
# 检查材质
obj = bpy.context.active_object
print(f"材质数: {len(obj.data.materials) if obj.data else 0}")

# 确保导出材质和纹理
bpy.ops.export_scene.dae(
    filepath='//mat.dae',
    export_selected=True,
    export_materials=True,
    export_textures=True
)

# 检查纹理路径
print(f"纹理目录: {bpy.path.abspath('//textures/')}")
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 确保导出动画
bpy.ops.export_scene.dae(
    filepath='//anim.dae',
    export_selected=True,
    export_anim=True,
    export_baked_animations=True
)

# 检查骨骼动画
if obj.type == 'ARMATURE':
    print(f"骨骼数: {len(obj.data.bones)}")
```

### 骨骼问题
```python
# 检查骨骼
armature = None
for obj in bpy.context.selected_objects:
    if obj.type == 'ARMATURE':
        armature = obj
        break

if armature:
    print(f"骨骼数: {len(armature.data.bones)}")
    
    # 检查绑定
    for child in armature.children:
        if child.type == 'MESH':
            print(f"子对象: {child.name}")
            for mod in child.modifiers:
                if mod.type == 'ARMATURE':
                    print(f"绑定到: {mod.object}")

# 确保导出骨骼
bpy.ops.export_scene.dae(
    filepath='//armature.dae',
    export_selected=True,
    use_mesh_modifiers=True,
    export_anim=True,
    export_baked_animations=True
)
```

### 坐标轴问题
```python
# 检查轴向
print(f"Y轴向上: {bpy.context.scene.unit_settings.system}")

# 尝试不同轴向
bpy.ops.export_scene.dae(
    filepath='//yup.dae',
    use_yup=True
)

bpy.ops.export_scene.dae(
    filepath='//zup.dae',
    use_yup=False
)

# 在导入软件中检查方向
```

### 性能问题
```python
# 检查对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 减少导出内容
bpy.ops.export_scene.dae(
    filepath='//optimized.dae',
    export_selected=True,
    use_mesh_modifiers=True,
    use_triangles=True,
    export_uv=True,
    export_normals=True,
    export_materials=True,
    export_anim=False,
    export_cameras=False,
    export_lights=False
)

# 设置文件大小限制
bpy.ops.export_scene.dae(
    filepath='//limited.dae',
    export_selected=True,
    max_file_size=50.0
)
```


---
## bpy_ops_import_usd

# bpy.ops.import_scene - USD导入操作模块

Blender USD文件导入操作符，用于将USD（Universal Scene Description）格式文件导入Blender。

## 概述

`bpy.ops.import_scene.usd` 模块提供用于将USD格式文件导入Blender的功能，支持几何体、材质、动画和高级渲染设置的导入。

## 核心功能

### 基础导入

```python
import bpy

# 导入USD文件
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.usd'}],
    ui_tab='MAIN',
    usd_format='USD',
    import_cameras=True,
    import_lights=True,
    import_materials=True,
    import_curves=True,
    import_particles=True,
    import_procedural='NONE',
    import_subdiv=False,
    import_anim=True,
    import_anim_use_default=True,
    import_anim_range=True,
    import_assets=True,
    scale=1.0,
    import_scale=1.0,
    import_transformation_type='壹致',
    apply_unit_scale=True,
    usd_conversion='NONE',
    export_transformation_type='壹致',
    use_euler_filter=True,
    import_guide=False,
    uv_format='NONE',
    normal_format='NONE',
    light_intensity_match=False
)
```

### USD格式选择

```python
# 导入USD格式
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    usd_format='USD'
)

# 导入USDZ格式
bpy.ops.import_scene.usd(
    filepath='//model.usdz',
    usd_format='USDZ'
)

# 导入USDA格式
bpy.ops.import_scene.usd(
    filepath='//model.usda',
    usd_format='USDA'
)
```

### 几何体导入

```python
# 导入网格
bpy.ops.import_scene.usd(
    filepath='//meshes.usd',
    import_meshes=True
)

# 不导入网格
bpy.ops.import_scene.usd(
    filepath='//no_meshes.usd',
    import_meshes=False
)
```

### 相机导入

```python
# 导入相机
bpy.ops.import_scene.usd(
    filepath='//cameras.usd',
    import_cameras=True
)

# 不导入相机
bpy.ops.import_scene.usd(
    filepath='//no_cameras.usd',
    import_cameras=False
)
```

### 灯光导入

```python
# 导入灯光
bpy.ops.import_scene.usd(
    filepath='//lights.usd',
    import_lights=True
)

# 不导入灯光
bpy.ops.import_scene.usd(
    filepath='//no_lights.usd',
    import_lights=False
)

# 灯光强度匹配
bpy.ops.import_scene.usd(
    filepath='//lights.usd',
    import_lights=True,
    light_intensity_match=True
)
```

### 材质导入

```python
# 导入材质
bpy.ops.import_scene.usd(
    filepath='//materials.usd',
    import_materials=True
)

# 不导入材质
bpy.ops.import_scene.usd(
    filepath='//no_materials.usd',
    import_materials=False
)
```

### 曲线导入

```python
# 导入曲线
bpy.ops.import_scene.usd(
    filepath='//curves.usd',
    import_curves=True
)

# 不导入曲线
bpy.ops.import_scene.usd(
    filepath='//no_curves.usd',
    import_curves=False
)
```

### 粒子导入

```python
# 导入粒子
bpy.ops.import_scene.usd(
    filepath='//particles.usd',
    import_particles=True
)

# 不导入粒子
bpy.ops.import_scene.usd(
    filepath='//no_particles.usd',
    import_particles=False
)
```

### 程序化导入

```python
# 不导入程序化
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_procedural='NONE'
)

# 导入为程序化
bpy.ops.import_scene.usd(
    filepath='//procedural.usd',
    import_procedural='CREATE'
)
```

### 细分导入

```python
# 导入细分
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_subdiv=True
)

# 不导入细分
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_subdiv=False
)
```

### 动画导入

```python
# 导入动画
bpy.ops.import_scene.usd(
    filepath='//anim.usd',
    import_anim=True
)

# 不导入动画
bpy.ops.import_scene.usd(
    filepath='//static.usd',
    import_anim=False
)

# 使用默认动画
bpy.ops.import_scene.usd(
    filepath='//anim.usd',
    import_anim=True,
    import_anim_use_default=True
)

# 使用帧范围
bpy.ops.import_scene.usd(
    filepath='//anim.usd',
    import_anim=True,
    import_anim_range=True
)
```

### 资源导入

```python
# 导入资源
bpy.ops.import_scene.usd(
    filepath='//assets.usd',
    import_assets=True
)

# 不导入资源
bpy.ops.import_scene.usd(
    filepath='//no_assets.usd',
    import_assets=False
)
```

### 缩放设置

```python
# 设置缩放
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    scale=1.0
)

# 设置导入缩放
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_scale=1.0
)

# 应用单位缩放
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    apply_unit_scale=True
)
```

### 变换类型

```python
# 一致变换
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_transformation_type='壹致'
)

# 矩阵变换
bpy.ops.import_scene.usd(
    filepath='//matrix.usd',
    import_transformation_type='矩阵'
)
```

### USD转换

```python
# 无转换
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    usd_conversion='NONE'
)

# 使用导出变换类型
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    usd_conversion='NONE',
    export_transformation_type='壹致'
)
```

### 欧拉滤波

```python
# 使用欧拉滤波
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    use_euler_filter=True
)

# 不使用欧拉滤波
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    use_euler_filter=False
)
```

### 引导导入

```python
# 导入引导
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_guide=True
)

# 不导入引导
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_guide=False
)
```

### UV格式

```python
# 无UV
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    uv_format='NONE'
)

# 导入UV
bpy.ops.import_scene.usd(
    filepath='//uv.usd',
    uv_format='IMPORT'
)
```

### 法线格式

```python
# 无法线
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    normal_format='NONE'
)

# 导入法线
bpy.ops.import_scene.usd(
    filepath='//normals.usd',
    normal_format='IMPORT'
)
```

## 实用函数

```python
def import_usd_model(filepath, scale=1.0):
    """导入USD模型"""
    bpy.ops.import_scene.usd(
        filepath=filepath,
        scale=scale,
        import_cameras=True,
        import_lights=True,
        import_materials=True,
        import_anim=False
    )
    return list(bpy.context.selected_objects)

def import_usd_with_animations(filepath, scale=1.0):
    """导入USD带动画"""
    bpy.ops.import_scene.usd(
        filepath=filepath,
        scale=scale,
        import_cameras=True,
        import_lights=True,
        import_materials=True,
        import_anim=True,
        import_anim_use_default=True
    )
    return list(bpy.context.selected_objects)

def import_usd_for_editing(filepath):
    """导入USD用于编辑"""
    bpy.ops.import_scene.usd(
        filepath=filepath,
        scale=1.0,
        import_cameras=False,
        import_lights=False,
        import_materials=False,
        import_anim=False
    )
    return list(bpy.context.selected_objects)

def import_usd_character(filepath):
    """导入USD角色"""
    bpy.ops.import_scene.usd(
        filepath=filepath,
        scale=1.0,
        import_cameras=True,
        import_lights=True,
        import_materials=True,
        import_curves=True,
        import_anim=True,
        import_anim_use_default=True,
        import_anim_range=True
    )
    return list(bpy.context.selected_objects)

def import_usdz_for_ar(filepath, scale=1.0):
    """导入USDZ用于AR"""
    bpy.ops.import_scene.usd(
        filepath=filepath,
        scale=scale,
        usd_format='USDZ',
        import_cameras=True,
        import_lights=True,
        import_materials=True,
        import_anim=False
    )
    return list(bpy.context.selected_objects)

def batch_import_usd_files(usd_files, scale=1.0):
    """批量导入USD文件"""
    all_objects = []
    for filepath in usd_files:
        bpy.ops.import_scene.usd(
            filepath=filepath,
            scale=scale
        )
        imported = list(bpy.context.selected_objects)
        all_objects.extend(imported)
    return all_objects

def import_usd_and_merge(filepath, scale=1.0):
    """导入USD并合并"""
    bpy.ops.import_scene.usd(
        filepath=filepath,
        scale=scale,
        import_cameras=False,
        import_lights=False,
        import_materials=False
    )
    
    objects = list(bpy.context.selected_objects)
    if len(objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return objects
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查选中对象
selected = bpy.context.selected_objects
print(f"导入对象数: {len(selected)}")

# 检查对象类型
for obj in selected:
    print(f"{obj.name}: 类型={obj.type}")

# 检查隐藏状态
for obj in selected:
    print(f"{obj.name}: 隐藏={obj.hide_viewport}")
    obj.hide_viewport = False

# 检查显示类型
for obj in selected:
    obj.display_type = 'SOLID'
```

### 材质丢失
```python
# 检查材质
print(f"材质数: {len(bpy.data.materials)}")

# 检查纹理
print(f"纹理数: {len(bpy.data.textures)}")

# 重新导入带材质
bpy.ops.import_scene.usd(
    filepath='//mat.usd',
    import_materials=True
)
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 重新导入带动画
bpy.ops.import_scene.usd(
    filepath='//anim.usd',
    import_anim=True,
    import_anim_use_default=True
)
```

### 比例问题
```python
# 检查导入比例
obj = bpy.context.active_object
print(f"尺寸: {obj.dimensions}")

# 检查单位设置
print(f"场景单位: {bpy.context.scene.unit_settings.system}")

# 重新导入带正确比例
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    scale=0.01,
    apply_unit_scale=True
)
```

### 性能问题
```python
# 检查对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 减少导入内容
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_cameras=False,
    import_lights=False,
    import_materials=False,
    import_curves=False,
    import_particles=False,
    import_anim=False
)

# 导入后优化
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.join()
```

### 灯光强度问题
```python
# 检查灯光强度
for obj in bpy.context.selected_objects:
    if obj.type == 'LIGHT':
        print(f"{obj.name}: 强度={obj.data.energy}")

# 使用强度匹配
bpy.ops.import_scene.usd(
    filepath='//lights.usd',
    import_lights=True,
    light_intensity_match=True
)

# 手动调整强度
for obj in bpy.context.selected_objects:
    if obj.type == 'LIGHT':
        obj.data.energy *= 100  # 根据需要调整
```

### 坐标轴问题
```python
# 检查导入的变换
obj = bpy.context.active_object
print(f"位置: {obj.location}")
print(f"旋转: {obj.rotation_euler}")
print(f"缩放: {obj.scale}")

# 尝试不同的变换类型
bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_transformation_type='壹致'
)

bpy.ops.import_scene.usd(
    filepath='//model.usd',
    import_transformation_type='矩阵'
)
```


---
## bpy_ops_export_usd

# bpy.ops.export_scene - USD导出操作模块

Blender USD文件导出操作符，用于将场景导出为USD（Universal Scene Description）格式。

## 概述

`bpy.ops.export_scene.usd` 模块提供用于将Blender场景导出为USD格式的功能，支持几何体、材质、动画和高级渲染设置的导出。

## 核心功能

### 基础导出

```python
import bpy

# 导出USD文件
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=False,
    usd_format='USD',
    export_materials=True,
    export_cameras=True,
    export_lights=True,
    export_curves=True,
    export_particles=True,
    use_instance_attributes=True,
    use_visible=True,
    use_active_objects=False,
    export_uv=True,
    export_normals=True,
    export_colors=True,
    export_meshes=True,
    export_lods='NONE',
    export_image_format='NONE',
    export_thumbnail=True,
    export_procedural='NONE',
    export_custom_properties=True,
    export_preview=True,
    apply_subdiv=False,
    export_anim=True,
    export_anim_use_frame_range=True,
    export_anim_use_nla_strips=True,
    export_anim_armature=True,
    export_anim_shape_keys=True,
    export_anim_smooth_curves=True,
    export_anim_always_sample=True,
    export_anim_including_final=False,
    export_anim_step=1.0,
    export_anim_simplify_factor=0.0,
    export_parents=True,
    export_material_variant='CURRENT',
    default_uv='UVMap',
    default_material='Principled BSDF',
    path_mode='RELATIVE',
    export_format='EXPORT',
    rescale_double_sided=True,
    export_transformation_type='壹致',
    export_level_of_detail=True,
    export_compact_usd=False
)
```

### USD格式选择

```python
# 导出为USD格式
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    usd_format='USD'
)

# 导出为USDZ格式
bpy.ops.export_scene.usd(
    filepath='//model.usdz',
    usd_format='USDZ'
)

# 导出为USDA格式
bpy.ops.export_scene.usd(
    filepath='//model.usda',
    usd_format='USDA'
)
```

### 导出范围

```python
# 只导出选中对象
bpy.ops.export_scene.usd(
    filepath='//selected.usd',
    export_selected=True
)

# 导出所有对象
bpy.ops.export_scene.usd(
    filepath='//all.usd',
    export_selected=False
)
```

### 几何体导出

```python
# 导出网格
bpy.ops.export_scene.usd(
    filepath='//meshes.usd',
    export_meshes=True
)

# 不导出网格
bpy.ops.export_scene.usd(
    filepath='//no_meshes.usd',
    export_meshes=False
)

# 导出UV
bpy.ops.export_scene.usd(
    filepath='//uv.usd',
    export_uv=True
)

# 不导出UV
bpy.ops.export_scene.usd(
    filepath='//no_uv.usd',
    export_uv=False
)

# 导出法线
bpy.ops.export_scene.usd(
    filepath='//normals.usd',
    export_normals=True
)

# 不导出法线
bpy.ops.export_scene.usd(
    filepath='//no_normals.usd',
    export_normals=False
)

# 导出颜色
bpy.ops.export_scene.usd(
    filepath='//colors.usd',
    export_colors=True
)

# 不导出颜色
bpy.ops.export_scene.usd(
    filepath='//no_colors.usd',
    export_colors=False
)
```

### 曲线和粒子

```python
# 导出曲线
bpy.ops.export_scene.usd(
    filepath='//curves.usd',
    export_curves=True
)

# 不导出曲线
bpy.ops.export_scene.usd(
    filepath='//no_curves.usd',
    export_curves=False
)

# 导出粒子
bpy.ops.export_scene.usd(
    filepath='//particles.usd',
    export_particles=True
)

# 不导出粒子
bpy.ops.export_scene.usd(
    filepath='//no_particles.usd',
    export_particles=False
)
```

### 相机和灯光

```python
# 导出相机
bpy.ops.export_scene.usd(
    filepath='//cameras.usd',
    export_cameras=True
)

# 不导出相机
bpy.ops.export_scene.usd(
    filepath='//no_cameras.usd',
    export_cameras=False
)

# 导出灯光
bpy.ops.export_scene.usd(
    filepath='//lights.usd',
    export_lights=True
)

# 不导出灯光
bpy.ops.export_scene.usd(
    filepath='//no_lights.usd',
    export_lights=False
)
```

### 材质导出

```python
# 导出材质
bpy.ops.export_scene.usd(
    filepath='//materials.usd',
    export_materials=True
)

# 不导出材质
bpy.ops.export_scene.usd(
    filepath='//no_materials.usd',
    export_materials=False
)

# 设置默认材质
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    default_material='Principled BSDF'
)
```

### 可见性设置

```python
# 导出可见对象
bpy.ops.export_scene.usd(
    filepath='//visible.usd',
    use_visible=True
)

# 导出活动对象
bpy.ops.export_scene.usd(
    filepath='//active.usd',
    use_active_objects=True
)

# 使用实例属性
bpy.ops.export_scene.usd(
    filepath='//instances.usd',
    use_instance_attributes=True
)
```

### LOD设置

```python
# 不导出LOD
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    export_lods='NONE'
)

# 导出所有LOD
bpy.ops.export_scene.usd(
    filepath='//lods.usd',
    export_lods='ALL'
)
```

### 图像导出

```python
# 图像格式
bpy.ops.export_scene.usd(
    filepath='//images.usd',
    export_image_format='PNG'
)

# 不导出图像
bpy.ops.export_scene.usd(
    filepath='//no_images.usd',
    export_image_format='NONE'
)

# 导出缩略图
bpy.ops.export_scene.usd(
    filepath='//thumb.usd',
    export_thumbnail=True
)

# 不导出缩略图
bpy.ops.export_scene.usd(
    filepath='//no_thumb.usd',
    export_thumbnail=False
)
```

### 程序化导出

```python
# 不使用程序化
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    export_procedural='NONE'
)

# 导出为程序化
bpy.ops.export_scene.usd(
    filepath='//procedural.usd',
    export_procedural='EXPORT'
)
```

### 自定义属性

```python
# 导出自定义属性
bpy.ops.export_scene.usd(
    filepath='//props.usd',
    export_custom_properties=True
)

# 不导出自定义属性
bpy.ops.export_scene.usd(
    filepath='//no_props.usd',
    export_custom_properties=False
)
```

### 预览设置

```python
# 导出预览
bpy.ops.export_scene.usd(
    filepath='//preview.usd',
    export_preview=True
)

# 不导出预览
bpy.ops.export_scene.usd(
    filepath='//no_preview.usd',
    export_preview=False
)
```

### 细分设置

```python
# 应用细分
bpy.ops.export_scene.usd(
    filepath='//subdiv.usd',
    apply_subdiv=True
)

# 不应用细分
bpy.ops.export_scene.usd(
    filepath='//no_subdiv.usd',
    apply_subdiv=False
)
```

### 动画设置

```python
# 导出动画
bpy.ops.export_scene.usd(
    filepathd',
    export='//anim.us_anim=True
)

# 不导出动画
bpy.ops.export_scene.usd(
    filepath='//static.usd',
    export_anim=False
)

# 使用帧范围
bpy.ops.export_scene.usd(
    filepath='//range.usd',
    export_anim=True,
    export_anim_use_frame_range=True
)

# 使用NLA轨道
bpy.ops.export_scene.usd(
    filepath='//nla.usd',
    export_anim=True,
    export_anim_use_nla_strips=True
)

# 导出骨骼动画
bpy.ops.export_scene.usd(
    filepath='//armature.usd',
    export_anim=True,
    export_anim_armature=True
)

# 导出形态键动画
bpy.ops.export_scene.usd(
    filepath='//shapekey.usd',
    export_anim=True,
    export_anim_shape_keys=True
)
```

### 父对象设置

```python
# 导出父对象
bpy.ops.export_scene.usd(
    filepath='//parents.usd',
    export_parents=True
)

# 不导出父对象
bpy.ops.export_scene.usd(
    filepath='//no_parents.usd',
    export_parents=False
)
```

### 路径模式

```python
# 相对路径
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    path_mode='RELATIVE'
)

# 绝对路径
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    path_mode='ABSOLUTE'
)
```

### 变换类型

```python
# 一致变换
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    export_transformation_type='壹致'
)

# 矩阵变换
bpy.ops.export_scene.usd(
    filepath='//matrix.usd',
    export_transformation_type='矩阵'
)
```

### LOD设置

```python
# 导出LOD
bpy.ops.export_scene.usd(
    filepath='//lod.usd',
    export_level_of_detail=True
)

# 不导出LOD
bpy.ops.export_scene.usd(
    filepath='//no_lod.usd',
    export_level_of_detail=False
)
```

### 紧凑USD

```python
# 紧凑USD
bpy.ops.export_scene.usd(
    filepath='//compact.usd',
    export_compact_usd=True
)

# 不紧凑USD
bpy.ops.export_scene.usd(
    filepath='//no_compact.usd',
    export_compact_usd=False
)
```

## 实用函数

```python
def export_usd_model(filepath, apply_modifiers=True):
    """导出USD模型"""
    bpy.ops.export_scene.usd(
        filepath=filepath,
        export_selected=True,
        export_meshes=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_cameras=True,
        export_lights=True,
        export_anim=False
    )

def export_usd_animation(filepath, start_frame=None, end_frame=None):
    """导出USD动画"""
    if start_frame is not None:
        bpy.context.scene.frame_start = start_frame
    if end_frame is not None:
        bpy.context.scene.frame_end = end_frame
    
    bpy.ops.export_scene.usd(
        filepath=filepath,
        export_selected=True,
        export_meshes=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_anim=True,
        export_anim_use_frame_range=True,
        export_anim_use_nla_strips=True
    )

def export_usd_for_unreal(filepath):
    """导出USD用于Unreal"""
    bpy.ops.export_scene.usd(
        filepath=filepath,
        export_selected=True,
        export_meshes=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_cameras=True,
        export_lights=True,
        export_anim=True,
        export_anim_use_frame_range=True,
        export_anim_use_nla_strips=True,
        export_anim_armature=True,
        export_anim_shape_keys=True,
        export_parents=True,
        export_format='EXPORT'
    )

def export_usd_for_maya(filepath):
    """导出USD用于Maya"""
    bpy.ops.export_scene.usd(
        filepath=filepath,
        export_selected=True,
        export_meshes=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_cameras=True,
        export_lights=True,
        export_curves=True,
        export_particles=True,
        export_anim=True,
        export_anim_use_frame_range=True,
        export_anim_armature=True,
        export_anim_shape_keys=True
    )

def export_usdz_for_ar(filepath):
    """导出USDZ用于AR"""
    bpy.ops.export_scene.usd(
        filepath=filepath,
        export_selected=True,
        export_meshes=True,
        export_uv=True,
        export_normals=True,
        export_materials=True,
        export_cameras=True,
        export_lights=True,
        export_anim=False,
        usd_format='USDZ'
    )

def batch_export_usd_by_collection(output_dir):
    """按集合批量导出USD"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for collection in bpy.data.collections:
        bpy.ops.object.select_all(action='DESELECT')
        
        for obj in collection.objects:
            obj.select_set(True)
        
        if bpy.context.selected_objects:
            bpy.context.view_layer.active_layer_collection = bpy.context.layer_collection.children[collection.name]
            filepath = os.path.join(output_dir, f'{collection.name}.usd')
            bpy.ops.export_scene.usd(
                filepath=filepath,
                export_selected=True,
                export_meshes=True,
                export_uv=True,
                export_normals=True,
                export_materials=True
            )
```

## 常见问题排查

### 导出文件无法打开
```python
# 检查文件路径
filepath = '//model.usd'
print(f"文件存在: {bpy.path.exists(filepath)}")

# 检查USD格式
print(f"USD格式: {bpy.context.scene.usd_export_}

# 确保使用正确的扩展名
if not filepath.endswith(('.usd', '.usdz', '.usda')):
    filepath += '.usd'

# 尝试不同的格式
bpy.ops.export_scene.usd(
    filepath='//model.usda',
    usd_format='USDA'
)
```

### 材质丢失
```python
# 检查材质
obj = bpy.context.active_object
print(f"材质数: {len(obj.data.materials) if obj.data else 0}")

# 确保导出材质
bpy.ops.export_scene.usd(
    filepath='//mat.usd',
    export_selected=True,
    export_materials=True
)

# 检查默认材质
print(f"默认材质: {bpy.context.scene.usd_export_principled BSDF}")
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 确保导出动画
bpy.ops.export_scene.usd(
    filepath='//anim.usd',
    export_selected=True,
    export_anim=True,
    export_anim_use_frame_range=True,
    export_anim_use_nla_strips=True
)

# 检查帧范围
print(f"帧范围: {bpy.context.scene.frame_start} - {bpy.context.scene.frame_end}")
```

### 性能问题
```python
# 检查对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 减少导出内容
bpy.ops.export_scene.usd(
    filepath='//optimized.usd',
    export_selected=True,
    export_meshes=True,
    export_uv=True,
    export_normals=True,
    export_materials=True,
    export_cameras=False,
    export_lights=False,
    export_curves=False,
    export_particles=False,
    export_anim=False,
    export_custom_properties=False
)

# 使用紧凑USD
bpy.ops.export_scene.usd(
    filepath='//compact.usd',
    export_selected=True,
    export_compact_usd=True
)
```

### 坐标轴问题
```python
# 检查USD坐标轴设置
print(f"USD前向: {bpy.context.scene.usd_export_}")
print(f"USD上向: {bpy.context.scene.usd_export_}")

# USD默认使用Y轴向上
# 在导入软件中检查方向
# 如果需要，使用变换修改器调整
```

### 纹理路径问题
```python
# 检查路径模式
print(f"路径模式: {bpy.context.scene.usd_export_path_mode}")

# 使用绝对路径
bpy.ops.export_scene.usd(
    filepath='//model.usd',
    path_mode='ABSOLUTE'
)

# 检查纹理引用
# 在USD文件中检查纹理路径是否正确
```


---
## bpy_ops_import_x3d

# bpy.ops.import_scene - X3D导入操作模块

Blender X3D文件导入操作符，用于将X3D格式文件导入Blender。

## 概述

`bpy.ops.import_scene.x3d` 模块提供用于将X3D格式文件导入Blender的功能，支持Web3D内容和交互式3D场景的导入。

## 核心功能

### 基础导入

```python
import bpy

# 导入X3D文件
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.x3d'}],
    ui_tab='MAIN',
    use_unit_scale=True,
    use_normals=True,
    use_compression=True,
    import_scale=1.0,
    animation_mode='REPLACE',
   ani_filters=None,
    const_speed=True,
    use_cameras=True,
    use_lamps=True,
    use_glob=True,
    axis_forward='Z',
    axis_up='Y'
)
```

### 单位设置

```python
# 使用单位缩放
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    use_unit_scale=True
)

# 不使用单位缩放
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    use_unit_scale=False
)
```

### 几何体设置

```python
# 导入法线
bpy.ops.import_scene.x3d(
    filepath='//normals.x3d',
    use_normals=True
)

# 不导入法线
bpy.ops.import_scene.x3d(
    filepath='//no_normals.x3d',
    use_normals=False
)
```

### 压缩设置

```python
# 使用压缩
bpy.ops.import_scene.x3d(
    filepath='//compressed.x3d',
    use_compression=True
)

# 不使用压缩
bpy.ops.import_scene.x3d(
    filepath='//uncompressed.x3d',
    use_compression=False
)
```

### 缩放设置

```python
# 设置导入缩放
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    import_scale=1.0
)

# 缩放10倍
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    import_scale=10.0
)

# 缩小10倍
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    import_scale=0.1
)
```

### 动画设置

```python
# 导入动画
bpy.ops.import_scene.x3d(
    filepath='//anim.x3d',
    animation_mode='REPLACE'
)

# 替换动画
bpy.ops.import_scene.x3d(
    filepath='//anim.x3d',
    animation_mode='REPLACE'
)

# 附加动画
bpy.ops.import_scene.x3d(
    filepath='//anim.x3d',
    animation_mode='APPEND'
)

# 常速
bpy.ops.import_scene.x3d(
    filepath='//anim.x3d',
    const_speed=True
)

# 动画过滤器
bpy.ops.import_scene.x3d(
    filepath='//anim.x3d',
    ani_filters=['position', 'rotation']
)
```

### 相机导入

```python
# 导入相机
bpy.ops.import_scene.x3d(
    filepath='//cameras.x3d',
    use_cameras=True
)

# 不导入相机
bpy.ops.import_scene.x3d(
    filepath='//no_cameras.x3d',
    use_cameras=False
)
```

### 灯光导入

```python
# 导入灯光
bpy.ops.import_scene.x3d(
    filepath='//lamps.x3d',
    use_lamps=True
)

# 不导入灯光
bpy.ops.import_scene.x3d(
    filepath='//no_lamps.x3d',
    use_lamps=False
)
```

### 坐标轴设置

```python
# 设置前向轴
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    axis_forward='Z'
)

# 设置上向轴
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    axis_up='Y'
)

# 其他轴向组合
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    axis_forward='-Z',
    axis_up='Y'
)
```

### 球体设置

```python
# 使用全局
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    use_glob=True
)

# 不使用全局
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    use_glob=False
)
```

## 实用函数

```python
def import_x3d_model(filepath, scale=1.0):
    """导入X3D模型"""
    bpy.ops.import_scene.x3d(
        filepath=filepath,
        import_scale=scale,
        use_normals=True,
        use_cameras=True,
        use_lamps=True,
        animation_mode='REPLACE'
    )
    return list(bpy.context.selected_objects)

def import_x3d_with_animations(filepath, scale=1.0):
    """导入X3D带动画"""
    bpy.ops.import_scene.x3d(
        filepath=filepath,
        import_scale=scale,
        use_normals=True,
        use_cameras=True,
        use_lamps=True,
        animation_mode='REPLACE',
        const_speed=True
    )
    return list(bpy.context.selected_objects)

def import_x3d_for_editing(filepath, scale=1.0):
    """导入X3D用于编辑"""
    bpy.ops.import_scene.x3d(
        filepath=filepath,
        import_scale=scale,
        use_normals=True,
        use_cameras=False,
        use_lamps=False,
        animation_mode='REPLACE'
    )
    return list(bpy.context.selected_objects)

def import_x3d_compressed(filepath, scale=1.0):
    """导入压缩X3D"""
    bpy.ops.import_scene.x3d(
        filepath=filepath,
        import_scale=scale,
        use_normals=True,
        use_compression=True,
        use_cameras=True,
        use_lamps=True
    )
    return list(bpy.context.selected_objects)

def import_x3d_character(filepath, scale=1.0):
    """导入X3D角色"""
    bpy.ops.import_scene.x3d(
        filepath=filepath,
        import_scale=scale,
        use_normals=True,
        use_cameras=True,
        use_lamps=True,
        animation_mode='REPLACE',
        const_speed=True
    )
    return list(bpy.context.selected_objects)

def batch_import_x3d_files(x3d_files, scale=1.0):
    """批量导入X3D文件"""
    all_objects = []
    for filepath in x3d_files:
        bpy.ops.import_scene.x3d(
            filepath=filepath,
            import_scale=scale
        )
        imported = list(bpy.context.selected_objects)
        all_objects.extend(imported)
    return all_objects

def import_x3d_and_merge(filepath, scale=1.0):
    """导入X3D并合并"""
    bpy.ops.import_scene.x3d(
        filepath=filepath,
        import_scale=scale,
        use_normals=True,
        use_cameras=False,
        use_lamps=False
    )
    
    objects = list(bpy.context.selected_objects)
    if len(objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return objects
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查选中对象
selected = bpy.context.selected_objects
print(f"导入对象数: {len(selected)}")

# 检查对象类型
for obj in selected:
    print(f"{obj.name}: 类型={obj.type}")

# 检查隐藏状态
for obj in selected:
    print(f"{obj.name}: 隐藏={obj.hide_viewport}")
    obj.hide_viewport = False

# 检查显示类型
for obj in selected:
    obj.display_type = 'SOLID'
```

### 材质丢失
```python
# 检查材质
print(f"材质数: {len(bpy.data.materials)}")

# 检查纹理
print(f"纹理数: {len(bpy.data.textures)}")

# 检查图像
print(f"图像数: {len(bpy.data.images)}")

# 重新导入
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    use_normals=True,
    use_cameras=True,
    use_lamps=True
)
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 检查骨骼动画
if obj.type == 'ARMATURE':
    print(f"骨骼数: {len(obj.data.bones)}")

# 重新导入带动画
bpy.ops.import_scene.x3d(
    filepath='//anim.x3d',
    animation_mode='REPLACE',
    const_speed=True
)
```

### 比例问题
```python
# 检查导入比例
obj = bpy.context.active_object
print(f"尺寸: {obj.dimensions}")

# 检查单位设置
print(f"场景单位: {bpy.context.scene.unit_settings.system}")

# 重新导入带正确比例
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    import_scale=0.01,
    use_unit_scale=True
)
```

### 性能问题
```python
# 检查对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 减少导入内容
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    use_cameras=False,
    use_lamps=False,
    animation_mode='REPLACE'
)

# 导入后优化
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.join()
```

### 坐标轴问题
```python
# 检查导入的变换
obj = bpy.context.active_object
print(f"位置: {obj.location}")
print(f"旋转: {obj.rotation_euler}")
print(f"缩放: {obj.scale}")

# 尝试不同轴向
bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    axis_forward='Z',
    axis_up='Y'
)

bpy.ops.import_scene.x3d(
    filepath='//model.x3d',
    axis_forward='-Z',
    axis_up='Y'
)
```

### 压缩问题
```python
# 检查压缩状态
print(f"压缩文件: {filepath.endswith('.x3dz')}")

# 尝试解压
import gzip
with gzip.open(filepath, 'rb') as f:
    content = f.read()
    with open(filepath[:-1], 'wb') as out:
        out.write(content)

# 导入解压后的文件
bpy.ops.import_scene.x3d(
    filepath=filepath[:-1],
    use_compression=False
)
```

### 动画速度问题
```python
# 检查动画速度
obj = bpy.context.active_object
if obj.animation_data:
    print(f"动画长度: {obj.animation_data.action.frame_range}")

# 使用常速
bpy.ops.import_scene.x3d(
    filepath='//anim.x3d',
    animation_mode='REPLACE',
    const_speed=True
)

# 调整速度
if obj.animation_data:
    for fcurve in obj.animation_data.action.fcurves:
        for keyframe in fcurve.keyframe_points:
            keyframe.interpolation = 'LINEAR'
```


---
## bpy_ops_export_x3d

# bpy.ops.export_scene - X3D导出操作模块

Blender X3D文件导出操作符，用于将场景导出为X3D格式。

## 概述

`bpy.ops.export_scene.x3d` 模块提供用于将Blender场景导出为X3D格式的功能，支持Web3D应用和交互式3D内容的创建。

## 核心功能

### 基础导出

```python
import bpy

# 导出X3D文件
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=False,
    use_triangles=True,
    use_compression=True,
    use_normals=True,
    use_uv=True,
    use_mesh_modifiers=True,
    use_materials=True,
    use_textures=True,
    use_lights=True,
    use_cameras=True,
    use_yup=True,
    use_selection=False,
    export_defines=True,
    export_cameras=True,
    export_animations=True,
    export_lamps=True,
    export_face_materials=False,
    export_inlines=True,
    export_original_icosphere=True,
    export_lod=True,
    export_normals=True,
    export_ptc=True,
    export_normals_mode='FAST',
    export_color=True,
    export_embedded=True,
    export_blender_player=True,
    x3d_naming='BLENDER',
    export_appname=True,
    export_format='X3D',
    global_matrix=None,
    copy_textures=False,
    export_image_format='NAME',
    tesselated_normals=True
)
```

### 导出范围

```python
# 只导出选中对象
bpy.ops.export_scene.x3d(
    filepath='//selected.x3d',
    export_selected=True
)

# 导出所有对象
bpy.ops.export_scene.x3d(
    filepath='//all.x3d',
    export_selected=False
)
```

### 几何体导出

```python
# 三角化
bpy.ops.export_scene.x3d(
    filepath='//triangulated.x3d',
    use_triangles=True
)

# 不三角化
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    use_triangles=False
)

# 导出法线
bpy.ops.export_scene.x3d(
    filepath='//normals.x3d',
    use_normals=True
)

# 不导出法线
bpy.ops.export_scene.x3d(
    filepath='//no_normals.x3d',
    use_normals=False
)

# 导出UV
bpy.ops.export_scene.x3d(
    filepath='//uv.x3d',
    use_uv=True
)

# 不导出UV
bpy.ops.export_scene.x3d(
    filepath='//no_uv.x3d',
    use_uv=False
)
```

### 修改器设置

```python
# 应用网格修改器
bpy.ops.export_scene.x3d(
    filepath='//modifiers.x3d',
    use_mesh_modifiers=True
)

# 不应用修改器
bpy.ops.export_scene.x3d(
    filepath='//no_modifiers.x3d',
    use_mesh_modifiers=False
)
```

### 材质导出

```python
# 导出材质
bpy.ops.export_scene.x3d(
    filepath='//materials.x3d',
    use_materials=True
)

# 不导出材质
bpy.ops.export_scene.x3d(
    filepath='//no_materials.x3d',
    use_materials=False
)

# 导出纹理
bpy.ops.export_scene.x3d(
    filepath='//textures.x3d',
    use_textures=True
)

# 不导出纹理
bpy.ops.export_scene.x3d(
    filepath='//no_textures.x3d',
    use_textures=False
)

# 导出面材质
bpy.ops.export_scene.x3d(
    filepath='//face_mats.x3d',
    export_face_materials=True
)

# 导出颜色
bpy.ops.export_scene.x3d(
    filepath='//color.x3d',
    export_color=True
)
```

### 相机和灯光

```python
# 导出相机
bpy.ops.export_scene.x3d(
    filepath='//cameras.x3d',
    use_cameras=True
)

# 不导出相机
bpy.ops.export_scene.x3d(
    filepath='//no_cameras.x3d',
    use_cameras=False
)

# 导出灯光
bpy.ops.export_scene.x3d(
    filepath='//lamps.x3d',
    export_lamps=True
)

# 不导出灯光
bpy.ops.export_scene.x3d(
    filepath='//no_lamps.x3d',
    export_lamps=False
)

# 使用灯光
bpy.ops.export_scene.x3d(
    filepath='//lights.x3d',
    use_lights=True
)
```

### 动画导出

```python
# 导出动画
bpy.ops.export_scene.x3d(
    filepath='//anim.x3d',
    export_animations=True
)

# 不导出动画
bpy.ops.export_scene.x3d(
    filepath='//static.x3d',
    export_animations=False
)
```

### 坐标轴设置

```python
# Y轴向上
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    use_yup=True
)

# Z轴向上
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    use_yup=False
)
```

### 压缩设置

```python
# 使用压缩
bpy.ops.export_scene.x3d(
    filepath='//compressed.x3d',
    use_compression=True
)

# 不使用压缩
bpy.ops.export_scene.x3d(
    filepath='//uncompressed.x3d',
    use_compression=False
)
```

### 内联设置

```python
# 导出内联
bpy.ops.export_scene.x3d(
    filepath='//inlines.x3d',
    export_inlines=True
)

# 不导出内联
bpy.ops.export_scene.x3d(
    filepath='//no_inlines.x3d',
    export_inlines=False
)
```

### LOD设置

```python
# 导出LOD
bpy.ops.export_scene.x3d(
    filepath='//lod.x3d',
    export_lod=True
)

# 不导出LOD
bpy.ops.export_scene.x3d(
    filepath='//no_lod.x3d',
    export_lod=False
)
```

### 命名设置

```python
# Blender命名
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    x3d_naming='BLENDER'
)

# X3D原生命名
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    x3d_naming='X3D'
)
```

### 导出格式

```python
# X3D格式
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    export_format='X3D'
)

# VRML格式
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    export_format='VRML97'
)
```

### 应用程序名称

```python
# 导出应用程序名
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    export_appname=True
)

# 不导出应用程序名
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    export_appname=False
)
```

### 嵌入设置

```python
# 嵌入纹理
bpy.ops.export_scene.x3d(
    filepath='//embedded.x3d',
    export_embedded=True
)

# 不嵌入纹理
bpy.ops.export_scene.x3d(
    filepath='//no_embedded.x3d',
    export_embedded=False
)

# 复制纹理
bpy.ops.export_scene.x3d(
    filepath='//copied.x3d',
    copy_textures=True
)
```

### 法线模式

```python
# 快速法线
bpy.ops.export_scene.x3d(
    filepath='//fast.x3d',
    export_normals_mode='FAST'
)

# 精确法线
bpy.ops.export_scene.x3d(
    filepath='//exact.x3d',
    export_normals_mode='EXACT'
)

# 细分法线
bpy.ops.export_scene.x3d(
    filepath='//tessellated.x3d',
    tesselated_normals=True
)
```

### 点云设置

```python
# 导出点云
bpy.ops.export_scene.x3d(
    filepath='//pointcloud.x3d',
    export_ptc=True
)

# 不导出点云
bpy.ops.export_scene.x3d(
    filepath='//no_ptc.x3d',
    export_ptc=False
)
```

### 球体设置

```python
# 使用原始二十面体
bpy.ops.export_scene.x3d(
    filepath='//icosphere.x3d',
    export_original_icosphere=True
)

# 不使用原始二十面体
bpy.ops.export_scene.x3d(
    filepath='//no_icosphere.x3d',
    export_original_icosphere=False
)
```

### Blender播放器

```python
# 导出Blender播放器
bpy.ops.export_scene.x3d(
    filepath='//blender_player.x3d',
    export_blender_player=True
)

# 不导出Blender播放器
bpy.ops.export_scene.x3d(
    filepath='//no_player.x3d',
    export_blender_player=False
)
```

## 实用函数

```python
def export_x3d_model(filepath, apply_modifiers=True):
    """导出X3D模型"""
    bpy.ops.export_scene.x3d(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=True,
        use_mesh_modifiers=apply_modifiers,
        use_materials=True,
        use_textures=True,
        use_lights=True,
        use_cameras=True,
        export_animations=False
    )

def export_x3d_web_model(filepath):
    """导出X3D Web模型"""
    bpy.ops.export_scene.x3d(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=True,
        use_mesh_modifiers=True,
        use_materials=True,
        use_textures=True,
        use_lights=True,
        use_cameras=True,
        export_animations=True,
        use_compression=True,
        export_embedded=True
    )

def export_x3d_for_vrml_viewer(filepath):
    """导出X3D用于VRML查看器"""
    bpy.ops.export_scene.x3d(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=True,
        use_mesh_modifiers=True,
        use_materials=True,
        use_textures=True,
        use_lights=True,
        use_cameras=True,
        export_animations=True,
        export_format='VRML97'
    )

def export_x3d_compressed(filepath):
    """导出压缩X3D"""
    bpy.ops.export_scene.x3d(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=True,
        use_mesh_modifiers=True,
        use_materials=True,
        use_textures=True,
        use_compression=True,
        export_embedded=True
    )

def export_x3d_with_animation(filepath):
    """导出X3D带动画"""
    bpy.ops.export_scene.x3d(
        filepath=filepath,
        export_selected=True,
        use_triangles=True,
        use_normals=True,
        use_uv=True,
        use_mesh_modifiers=True,
        use_materials=True,
        use_textures=True,
        use_lights=True,
        use_cameras=True,
        export_animations=True,
        use_yup=True
    )

def batch_export_x3d_by_collection(output_dir):
    """按集合批量导出X3D"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for collection in bpy.data.collections:
        bpy.ops.object.select_all(action='DESELECT')
        
        for obj in collection.objects:
            obj.select_set(True)
        
        if bpy.context.selected_objects:
            bpy.context.view_layer.active_layer_collection = bpy.context.layer_collection.children[collection.name]
            filepath = os.path.join(output_dir, f'{collection.name}.x3d')
            bpy.ops.export_scene.x3d(
                filepath=filepath,
                export_selected=True,
                use_triangles=True,
                use_normals=True,
                use_uv=True,
                use_mesh_modifiers=True,
                use_materials=True
            )
```

## 常见问题排查

### 导出文件无法打开
```python
# 检查文件路径
filepath = '//model.x3d'
print(f"文件存在: {bpy.path.exists(filepath)}")

# 检查文件大小
import os
file_size = os.path.getsize(bpy.path.abspath(filepath))
print(f"文件大小: {file_size} 字节")

# 尝试不同的导出格式
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    export_format='X3D'
)

bpy.ops.export_scene.x3d(
    filepath='//model.wrl',
    export_format='VRML97'
)
```

### 材质丢失
```python
# 检查材质
obj = bpy.context.active_object
print(f"材质数: {len(obj.data.materials) if obj.data else 0}")

# 确保导出材质和纹理
bpy.ops.export_scene.x3d(
    filepath='//mat.x3d',
    export_selected=True,
    use_materials=True,
    use_textures=True
)

# 检查纹理路径
print(f"纹理目录: {bpy.path.abspath('//textures/')}")

# 嵌入纹理
bpy.ops.export_scene.x3d(
    filepath='//embedded.x3d',
    export_selected=True,
    use_materials=True,
    use_textures=True,
    export_embedded=True
)
```

### 动画丢失
```python
# 检查动画数据
obj = bpy.context.active_object
print(f"动画数据: {obj.animation_data}")

if obj.animation_data:
    print(f"动作: {obj.animation_data.action}")

# 确保导出动画
bpy.ops.export_scene.x3d(
    filepath='//anim.x3d',
    export_selected=True,
    export_animations=True
)

# 检查骨骼动画
if obj.type == 'ARMATURE':
    print(f"骨骼数: {len(obj.data.bones)}")
```

### 坐标轴问题
```python
# 检查轴向
print(f"Y轴向上: {bpy.context.scene.unit_settings.system}")

# 尝试不同轴向
bpy.ops.export_scene.x3d(
    filepath='//yup.x3d',
    use_yup=True
)

bpy.ops.export_scene.x3d(
    filepath='//zup.x3d',
    use_yup=False
)

# 在X3D查看器中检查方向
```

### 性能问题
```python
# 检查对象数
print(f"对象数: {len(bpy.context.selected_objects)}")

# 减少导出内容
bpy.ops.export_scene.x3d(
    filepath='//optimized.x3d',
    export_selected=True,
    use_triangles=True,
    use_normals=True,
    use_uv=True,
    use_mesh_modifiers=True,
    use_materials=True,
    use_textures=False,
    use_lights=False,
    use_cameras=False,
    export_animations=False
)

# 使用压缩
bpy.ops.export_scene.x3d(
    filepath='//compressed.x3d',
    export_selected=True,
    use_compression=True,
    export_embedded=True
)
```

### 纹理路径问题
```python
# 检查路径模式
print(f"路径模式: {bpy.context.scene.x3d_export_path_mode}")

# 使用相对路径
bpy.ops.export_scene.x3d(
    filepath='//model.x3d',
    copy_textures=True
)

# 嵌入纹理
bpy.ops.export_scene.x3d(
    filepath='//embedded.x3d',
    export_embedded=True
)

# 检查纹理引用
# 在X3D文件中检查纹理路径是否正确
```

### 法线问题
```python
# 检查法线导出模式
print(f"法线模式: {bpy.context.scene.x3d_export_normals_mode}")

# 使用快速法线
bpy.ops.export_scene.x3d(
    filepath='//fast.x3d',
    export_normals_mode='FAST'
)

# 使用精确法线
bpy.ops.export_scene.x3d(
    filepath='//exact.x3d',
    export_normals_mode='EXACT'
)

# 细分法线
bpy.ops.export_scene.x3d(
    filepath='//tessellated.x3d',
    tesselated_normals=True
)
```


---
## bpy_ops_import_stl

# bpy.ops.import_mesh - STL导入操作模块

Blender STL文件导入操作符，用于将STL网格文件导入Blender。

## 概述

`bpy.ops.import_mesh.stl` 模块提供用于将STL格式网格文件导入Blender的功能，支持ASCII和二进制STL格式。

## 核心功能

### 基础导入

```python
import bpy

# 导入STL文件
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.stl'}],
    global_matrix=None,
    use_mesh_validate=True,
    use_facet_normal=False
)

# 导入多个STL文件
bpy.ops.import_mesh.stl(
    filepath='//parts/',
    directory='//parts/',
    files=[{'name': 'part1.stl'}, {'name': 'part2.stl'}, {'name': 'part3.stl'}]
)
```

### 变换设置

```python
# 应用全局矩阵
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    global_matrix=None
)

# 自定义变换矩阵
import mathutils
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    global_matrix=matrix
)

# 缩放导入
scale_matrix = mathutils.Matrix.Scale(0.01, 4)
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    global_matrix=scale_matrix
)
```

### 网格验证

```python
# 启用网格验证
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    use_mesh_validate=True
)

# 禁用网格验证
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    use_mesh_validate=False
)
```

### 法线设置

```python
# 使用面法线
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    use_facet_normal=True
)

# 不使用面法线
bpy.ops.import_mesh.stl(
    filepath='//model.stl',
    use_facet_normal=False
)
```

### 批量导入

```python
# 导入目录下所有STL
def import_all_stl_files(directory):
    import os
    stl_files = [f for f in os.listdir(directory) if f.lower().endswith('.stl')]
    stl_files.sort()
    
    if stl_files:
        bpy.ops.import_mesh.stl(
            filepath=os.path.join(directory, stl_files[0]),
            directory=directory,
            files=[{'name': f} for f in stl_files]
        )
    
    return list(bpy.context.selected_objects)

# 导入并合并
def import_and_merge_stl(stl_files, merge=True):
    objects = []
    for stl_file in stl_files:
        bpy.ops.import_mesh.stl(filepath=stl_file)
        imported = bpy.context.active_object
        objects.append(importited)
    
    if merge and len(objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return objects
```

## 实用函数

```python
def import_stl_model(filepath, scale=1.0):
    """导入STL模型"""
    bpy.ops.import_mesh.stl(filepath=filepath)
    obj = bpy.context.active_object
    
    if scale != 1.0:
        obj.scale = (scale, scale, scale)
        bpy.ops.object.transform_apply(scale=True)
    
    return obj

def import_stl_with_transform(filepath, rotation=(0, 0, 0), translation=(0, 0, 0)):
    """导入STL并应用变换"""
    import math
    import mathutils
    
    euler = mathutils.Euler([math.radians(r) for r in rotation], 'XYZ')
    matrix = mathutils.Matrix.Translation(translation) @ euler.to_matrix().to_4x4()
    
    bpy.ops.import_mesh.stl(filepath=filepath, global_matrix=matrix)
    return bpy.context.active_object

def import_stl_parts(directory, merge=True):
    """导入目录下所有STL部件"""
    import os
    
    stl_files = [os.path.join(directory, f) for f in os.listdir(directory) if f.lower().endswith('.stl')]
    stl_files.sort()
    
    all_objects = []
    for stl_file in stl_files:
        bpy.ops.import_mesh.stl(filepath=stl_file)
        obj = bpy.context.active_object
        all_objects.append(obj)
    
    if merge and len(all_objects) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in all_objects:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = all_objects[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    
    return all_objects

def import_stl_and_setup_material(filepath, material_name='STLMaterial'):
    """导入STL并设置材质"""
    bpy.ops.import_mesh.stl(filepath=filepath)
    obj = bpy.context.active_object
    
    # 创建材质
    mat = bpy.data.materials.get(material_name)
    if not mat:
        mat = bpy.data.materials.new(name=material_name)
    
    obj.data.materials.append(mat)
    
    return obj

def batch_import_stl_for_print(stl_files, apply_transforms=True):
    """批量导入STL用于3D打印"""
    objects = []
    
    for stl_file in stl_files:
        bpy.ops.import_mesh.stl(filepath=stl_file)
        obj = bpy.context.active_object
        obj.name = bpy.path.basename(stl_file).replace('.stl', '')
        objects.append(obj)
    
    if apply_transforms:
        bpy.ops.object.select_all(action='SELECT')
        bpy.ops.object.transform_apply(
            location=True,
            rotation=True,
            scale=True
        )
    
    return objects

def import_scaled_stl(filepath, target_size=10.0):
    """导入并缩放到目标大小"""
    bpy.ops.import_mesh.stl(filepath=filepath)
    obj = bpy.context.active_object
    
    # 计算缩放
    max_dim = max(obj.dimensions)
    if max_dim > 0:
        scale = target_size / max_dim
        obj.scale = (scale, scale, scale)
    
    bpy.ops.object.transform_apply(scale=True)
    
    return obj
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查对象
obj = bpy.context.active_object
print(f"对象: {obj.name}")
print(f"类型: {obj.type}")
print(f"顶点数: {len(obj.data.vertices)}")

# 检查隐藏状态
print(f"隐藏: {obj.hide_viewport}")
obj.hide_viewport = False

# 检查显示类型
obj.display_type = 'SOLID'

# 检查渲染属性
print(f"渲染隐藏: {obj.hide_render}")
obj.hide_render = False
```

### 模型方向错误
```python
# 检查方向
obj = bpy.context.active_object
print(f"旋转: {obj.rotation_euler}")
print(f"缩放: {obj.scale}")

# 重置方向
obj.rotation_euler = (0, 0, 0)

# 应用变换
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)

# 重新导入带变换
import mathutils
matrix = mathutils.Matrix.Rotation(math.radians(90), 4, 'X')
bpy.ops.import_mesh.stl(filepath='//model.stl', global_matrix=matrix)
```

### 模型比例问题
```python
# 检查尺寸
obj = bpy.context.active_object
print(f"尺寸: {obj.dimensions}")

# 检查单位
print(f"场景单位: {bpy.context.scene.unit_settings.system}")
print(f"比例: {bpy.context.scene.unit_settings.scale_length}")

# 应用正确的缩放
bpy.ops.object.transform_apply(scale=True)

# 重新导入并缩放
bpy.ops.import_mesh.stl(filepath='//model.stl')
obj = bpy.context.active_object
obj.scale = (0.01, 0.01, 0.01)
bpy.ops.object.transform_apply(scale=True)
```

### 网格质量问题
```python
# 检查网格
mesh = bpy.context.active_object.data
print(f"顶点数: {len(mesh.vertices)}")
print(f"面数: {len(mesh.polygons)}")
print(f"边数: {len(mesh.edges)}")

# 检查非流形几何体
non_manifold = 0
for edge in mesh.edges:
    if len(edge.vertices) != 2:
        non_manifold += 1
print(f"非流形边: {non_manifold}")

# 检查孤立顶点
print(f"孤立顶点: {len(mesh.vertices)}")

# 重新导入并验证
bpy.ops.import_mesh.stl(filepath='//model.stl', use_mesh_validate=True)
```


---
## bpy_ops_export_stl

# bpy.ops.export_mesh - STL导出操作模块

Blender STL文件导出操作符，用于将网格导出为STL格式。

## 概述

`bpy.ops.export_mesh.stl` 模块提供用于将Blender网格导出为STL格式的功能，支持ASCII和二进制STL格式。

## 核心功能

### 基础导出

```python
import bpy

# 导出选中对象
bpy.ops.export_mesh.stl(
    filepath='//model.stl',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=True,
    apply_modifiers=True,
    export_type='MESH'
)

# 导出所有对象
bpy.ops.export_mesh.stl(
    filepath='//all_models.stl',
    export_selected=False
)
```

### 导出选项

```python
# 只导出选中对象
bpy.ops.export_mesh.stl(
    filepath='//selected.stl',
    export_selected=True
)

# 导出所有对象
bpy.ops.export_mesh.stl(
    filepath='//all.stl',
    export_selected=False
)

# 应用修改器
bpy.ops.export_mesh.stl(
    filepath='//applied.stl',
    apply_modifiers=True
)

# 不应用修改器
bpy.ops.export_mesh.stl(
    filepath='//not_applied.stl',
    apply_modifiers=False
)
```

### 导出类型

```python
# 导出为网格
bpy.ops.export_mesh.stl(
    filepath='//model.stl',
    export_type='MESH'
)

# 导出低多边形
bpy.ops.export_mesh.stl(
    filepath='//lowpoly.stl',
    export_type='LOW_POLY'
)

# 导出为CAD
bpy.ops.export_mesh.stl(
    filepath='//cad.stl',
    export_type='CAD'
)
```

### STL格式选项

```python
# 导出为ASCII STL
bpy.ops.export_mesh.stl(
    filepath='//ascii.stl',
    ascii_format=True
)

# 导出为二进制STL
bpy.ops.export_mesh.stl(
    filepath='//binary.stl',
    ascii_format=False
)
```

### 法线和面选项

```python
# 导出法线
bpy.ops.export_mesh.stl(
    filepath='//with_normals.stl',
    export_normals=True
)

# 不导出法线
bpy.ops.export_mesh.stl(
    filepath='//no_normals.stl',
    export_normals=False
)

# 导出面颜色
bpy.ops.export_mesh.stl(
    filepath='//with_colors.stl',
    export_colors=True
)

# 不导出面颜色
bpy.ops.export_mesh.stl(
    filepath='//no_colors.stl',
    export_colors=False
)
```

### 三角化选项

```python
# 三角化网格
bpy.ops.export_mesh.stl(
    filepath='//triangulated.stl',
    export_triangulated_mesh=True
)

# 不三角化
bpy.ops.export_mesh.stl(
    filepath='//not_triangulated.stl',
    export_triangulated_mesh=False
)
```

## 实用函数

```python
def export_selected_for_3d_print(filepath='//print_ready.stl'):
    """导出选中对象用于3D打印"""
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True,
        ascii_format=False,
        export_triangulated_mesh=True
    )

def export_model_for_cad(filepath='//cad_model.stl'):
    """导出模型用于CAD"""
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True,
        export_type='CAD',
        ascii_format=False
    )

def export_all_meshes(filepath='//all_meshes.stl'):
    """导出所有网格"""
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        export_selected=False,
        apply_modifiers=True
    )

def batch_export_meshes(output_dir):
    """批量导出网格"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for obj in bpy.data.objects:
        if obj.type == 'MESH':
            bpy.ops.object.select_all(action='DESELECT')
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            filepath = os.path.join(output_dir, f'{obj.name}.stl')
            bpy.ops.export_mesh.stl(
                filepath=filepath,
                export_selected=True,
                apply_modifiers=True
            )

def export_with_modifiers(filepath, obj):
    """导出对象及其修改器"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True
    )

def export_low_poly(filepath, obj):
    """导出低多边形版本"""
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 添加简模修改器
    mod = obj.modifiers.new(name='Decimate', type='DECIMATE')
    mod.ratio = 0.1
    
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True,
        export_type='LOW_POLY'
    )
    
    # 删除修改器
    obj.modifiers.remove(mod)
```

## 常见问题排查

### 导出文件为空
```python
# 检查选中对象
selected = [obj for obj in bpy.context.selected_objects if obj.type == 'MESH']
print(f"选中网格: {len(selected)}")

# 检查所有网格
meshes = [obj for obj in bpy.data.objects if obj.type == 'MESH']
print(f"所有网格: {len(meshes)}")

# 检查网格数据
for mesh in meshes:
    print(f"{mesh.name}: 顶点数={len(mesh.data.vertices)}, 面数={len(mesh.data.polygons)}")

# 确保网格可见
for obj in selected:
    obj.hide_viewport = False
```

### 修改器未应用
```python
# 检查修改器
obj = bpy.context.active_object
print(f"修改器数: {len(obj.modifiers)}")

for mod in obj.modifiers:
    print(f"修改器: {mod.type}, 显示={mod.show_viewport}, 渲染={mod.show_render}")

# 显示所有修改器
for mod in obj.modifiers:
    mod.show_viewport = True
    mod.show_render = True

# 手动应用修改器
bpy.ops.object.convert(target='MESH')

# 重新导出
bpy.ops.export_mesh.stl(
    filepath='//applied.stl',
    export_selected=True,
    apply_modifiers=True
)
```

### STL格式问题
```python
# 检查文件格式
print("STL格式选项:")
print("  - ASCII: 文本格式, 可读")
print("  - 二进制: 紧凑, 快速")

# 尝试ASCII格式
bpy.ops.export_mesh.stl(
    filepath='//ascii.stl',
    ascii_format=True
)

# 尝试二进制格式
bpy.ops.export_mesh.stl(
    filepath='//binary.stl',
    ascii_format=False
)

# 检查文件大小
import os
ascii_size = os.path.getsize(bpy.path.abspath('//ascii.stl'))
binary_size = os.path.getsize(bpy.path.abspath('//binary.stl'))
print(f"ASCII大小: {ascii_size} 字节")
print(f"二进制大小: {binary_size} 字节")
```

### 三角化问题
```python
# 检查面类型
mesh = bpy.context.active_object.data
quads = sum(1 for p in mesh.polygons if len(p.vertices) == 4)
tris = sum(1 for p in mesh.polygons if len(p.vertices) == 3)
ngons = sum(1 for p in mesh.polygons if len(p.vertices) > 4)

print(f"四边形: {quads}")
print(f"三角形: {tris}")
print(f"多边形: {ngons}")

# 添加三角化修改器
mod = bpy.context.active_object.modifiers.new(name='Triangulate', type='TRIANGULATE')
mod.show_viewport = True
mod.show_render = True

# 重新导出
bpy.ops.export_mesh.stl(
    filepath='//triangulated.stl',
    export_selected=True,
    apply_modifiers=True,
    export_triangulated_mesh=True
)
```


---
## bpy_ops_import_ply

# bpy.ops.import_mesh - PLY导入操作模块

Blender PLY文件导入操作符，用于将PLY网格文件导入Blender。

## 概述

`bpy.ops.import_mesh.ply` 模块提供用于将Polygon File Format（PLY）格式网格文件导入Blender的功能，支持顶点和面数据。

## 核心功能

### 基础导入

```python
import bpy

# 导入PLY文件
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    directory='//',
    files=[{'name': 'model.ply'}]
)

# 导入多个PLY文件
bpy.ops.import_mesh.ply(
    filepath='//parts/',
    directory='//parts/',
    files=[{'name': 'part1.ply'}, {'name': 'part2.ply'}]
)
```

### 颜色导入

```python
# 导入顶点颜色
bpy.ops.import_mesh.ply(
    filepath='//colored_model.ply',
    import_vertex_color=True
)

# 不导入顶点颜色
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    import_vertex_color=False
)
```

### 法线导入

```python
# 导入法线
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    import_normals=True
)

# 不导入法线
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    import_normals=False
)
```

### UV导入

```python
# 导入UV
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    import_uv=True
)

# 不导入UV
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    import_uv=False
)
```

### 变换设置

```python
# 应用全局矩阵
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    global_matrix=None
)

# 自定义变换矩阵
import mathutils
matrix = mathutils.Matrix.Rotation(math.radians(45), 4, 'Z')
bpy.ops.import_mesh.ply(
    filepath='//model.ply',
    global_matrix=matrix
)
```

## 实用函数

```python
def import_ply_model(filepath, apply_transform=True):
    """导入PLY模型"""
    bpy.ops.import_mesh.ply(filepath=filepath)
    obj = bpy.context.active_object
    
    if apply_transform:
        bpy.ops.object.transform_apply(
            location=True,
            rotation=True,
            scale=True
        )
    
    return obj

def import_ply_with_colors(filepath):
    """导入PLY带颜色"""
    bpy.ops.import_mesh.ply(
        filepath=filepath,
        import_vertex_color=True,
        import_normals=True,
        import_uv=True
    )
    return bpy.context.active_object

def import_ply_for_editing(filepath):
    """导入PLY用于编辑"""
    bpy.ops.import_mesh.ply(
        filepath=filepath,
        import_vertex_color=False,
        import_normals=False,
        import_uv=False
    )
    obj = bpy.context.active_object
    
    # 进入编辑模式检查网格
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.object.mode_set(mode='OBJECT')
    
    return obj

def import_ply_scaled(filepath, target_size=1.0):
    """导入PLY并缩放"""
    bpy.ops.import_mesh.ply(filepath=filepath)
    obj = bpy.context.active_object
    
    max_dim = max(obj.dimensions)
    if max_dim > 0:
        scale = target_size / max_dim
        obj.scale = (scale, scale, scale)
    
    bpy.ops.object.transform_apply(scale=True)
    
    return obj

def batch_import_ply_files(ply_files):
    """批量导入PLY文件"""
    objects = []
    
    for ply_file in ply_files:
        bpy.ops.import_mesh.ply(filepath=ply_file)
        obj = bpy.context.active_object
        obj.name = bpy.path.basename(ply_file).replace('.ply', '')
        objects.append(obj)
    
    return objects

def import_ply_and_setup(filepath, name, material=None):
    """导入PLY并设置"""
    bpy.ops.import_mesh.ply(filepath=filepath)
    obj = bpy.context.active_object
    obj.name = name
    
    if material:
        obj.data.materials.append(material)
    
    return obj
```

## 常见问题排查

### 导入后模型不可见
```python
# 检查对象
obj = bpy.context.active_object
print(f"对象: {obj.name}")
print(f"类型: {obj.type}")
print(f"顶点数: {len(obj.data.vertices)}")

# 检查隐藏状态
print(f"隐藏: {obj.hide_viewport}")
obj.hide_viewport = False

# 检查显示类型
obj.display_type = 'SOLID'
```

### 颜色不显示
```python
# 检查顶点颜色层
mesh = bpy.context.active_object.data
print(f"顶点颜色层: {[l.name for l in mesh.vertex_colors]}")

# 启用顶点颜色
if mesh.vertex_colors:
    mat = bpy.context.active_object.active_material
    if mat:
        print(f"使用顶点颜色: {mat.use_vertex_color_paint}")
        mat.use_vertex_color_paint = True

# 检查材质
obj = bpy.context.active_object
if obj.data.materials:
    for mat in obj.data.materials:
        print(f"材质: {mat.name}")
```

### 法线问题
```python
# 检查法线
mesh = bpy.context.active_object.data
print(f"自定义法线: {mesh.has_custom_normals}")

# 重新计算法线
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.select_all(action='SELECT')
bpy.ops.mesh.normals_make_consistent(inside=False)
bpy.ops.object.mode_set(mode='OBJECT')

# 检查面法线
for poly in mesh.polygons:
    if not poly.use_smooth:
        print(f"平面面: {poly.index}")
```

### 网格质量问题
```python
# 检查网格
mesh = bpy.context.active_object.data
print(f"顶点数: {len(mesh.vertices)}")
print(f"面数: {len(mesh.polygons)}")
print(f"边数: {len(mesh.edges)}")

# 检查零面积面
zero_area = 0
for poly in mesh.polygons:
    if poly.area < 0.0001:
        zero_area += 1
print(f"零面积面: {zero_area}")

# 检查重叠顶点
print(f"顶点数: {len(mesh.vertices)}")
```


---
## bpy_ops_export_ply

# bpy.ops.export_mesh - PLY导出操作模块

Blender PLY文件导出操作符，用于将网格导出为PLY格式。

## 概述

`bpy.ops.export_mesh.ply` 模块提供用于将Blender网格导出为PLY（Polygon File Format）格式的功能，支持顶点和面数据的保存。

## 核心功能

### 基础导出

```python
import bpy

# 导出选中对象
bpy.ops.export_mesh.ply(
    filepath='//model.ply',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    export_selected=True,
    apply_modifiers=True,
    export_normals=True,
    export_uv=True,
    export_colors=True
)

# 导出所有对象
bpy.ops.export_mesh.ply(
    filepath='//all_models.ply',
    export_selected=False
)
```

### 法线导出

```python
# 导出法线
bpy.ops.export_mesh.ply(
    filepath='//with_normals.ply',
    export_normals=True
)

# 不导出法线
bpy.ops.export_mesh.ply(
    filepath='//no_normals.ply',
    export_normals=False
)
```

### UV导出

```python
# 导出UV
bpy.ops.export_mesh.ply(
    filepath='//with_uv.ply',
    export_uv=True
)

# 不导出UV
bpy.ops.export_mesh.ply(
    filepath='//no_uv.ply',
    export_uv=False
)
```

### 颜色导出

```python
# 导出顶点颜色
bpy.ops.export_mesh.ply(
    filepath='//with_colors.ply',
    export_colors=True
)

# 不导出顶点颜色
bpy.ops.export_mesh.ply(
    filepath='//no_colors.ply',
    export_colors=False
)

# 导出面颜色
bpy.ops.export_mesh.ply(
    filepath='//face_colors.ply',
    export_face_colors=True
)
```

### 导出选项

```python
# 应用修改器
bpy.ops.export_mesh.ply(
    filepath='//applied.ply',
    apply_modifiers=True
)

# 只导出选中对象
bpy.ops.export_mesh.ply(
    filepath='//selected.ply',
    export_selected=True
)

# 导出所有网格
bpy.ops.export_mesh.ply(
    filepath='//all.ply',
    export_selected=False
)

# 三角化
bpy.ops.export_mesh.ply(
    filepath='//triangulated.ply',
    export_triangulated_mesh=True
)
```

### 坐标轴设置

```python
# Blender默认轴向
bpy.ops.export_mesh.ply(
    filepath='//model.ply',
    axis_forward='Z',
    axis_up='Y'
)

# 其他轴向
bpy.ops.export_mesh.ply(
    filepath='//model.ply',
    axis_forward='-Z',
    axis_up='Y'
)
```

## 实用函数

```python
def export_ply_model(filepath, obj=None, apply_modifiers=True):
    """导出PLY模型"""
    if obj:
        bpy.ops.object.select_all(action='DESELECT')
        obj.select_set(True)
        bpy.context.view_layer.objects.active = obj
    
    bpy.ops.export_mesh.ply(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=apply_modifiers,
        export_normals=True,
        export_uv=True,
        export_colors=True
    )

def export_ply_with_colors(filepath):
    """导出PLY带颜色"""
    bpy.ops.export_mesh.ply(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True,
        export_normals=True,
        export_uv=True,
        export_colors=True
    )

def export_ply_simple(filepath):
    """导出简单PLY（仅几何体）"""
    bpy.ops.export_mesh.ply(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True,
        export_normals=False,
        export_uv=False,
        export_colors=False
    )

def export_ply_for_scanning(filepath):
    """导出PLY用于扫描"""
    bpy.ops.export_mesh.ply(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True,
        export_normals=True,
        export_uv=True,
        export_colors=True,
        export_triangulated_mesh=True
    )

def batch_export_ply(output_dir):
    """批量导出PLY文件"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for obj in bpy.data.objects:
        if obj.type == 'MESH':
            bpy.ops.object.select_all(action='DESELECT')
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            filepath = os.path.join(output_dir, f'{obj.name}.ply')
            bpy.ops.export_mesh.ply(
                filepath=filepath,
                export_selected=True,
                apply_modifiers=True
            )

def export_ply_selection(objects, filepath):
    """导出选中的对象列表"""
    bpy.ops.object.select_all(action='DESELECT')
    for obj in objects:
        if obj.type == 'MESH':
            obj.select_set(True)
    
    if bpy.context.selected_objects:
        bpy.context.view_layer.objects.active = bpy.context.selected_objects[0]
        bpy.ops.export_mesh.ply(
            filepath=filepath,
            export_selected=True,
            apply_modifiers=True
        )
```

## 常见问题排查

### 导出文件不完整
```python
# 检查选中对象
selected = [obj for obj in bpy.context.selected_objects if obj.type == 'MESH']
print(f"选中网格: {len(selected)}")

# 检查网格数据
for obj in selected:
    print(f"{obj.name}: 顶点数={len(obj.data.vertices)}, 面数={len(obj.data.polygons)}")

# 确保网格可见
for obj in selected:
    obj.hide_viewport = False
    obj.hide_render = False
```

### 颜色丢失
```python
# 检查顶点颜色层
mesh = bpy.context.active_object.data
print(f"顶点颜色层: {[l.name for l in mesh.vertex_colors]}")

# 检查材质
obj = bpy.context.active_object
print(f"材质: {obj.data.materials}")

# 检查是否启用了顶点颜色
mat = obj.active_material
if mat:
    print(f"使用顶点颜色: {mat.use_vertex_color_paint}")

# 确保导出颜色选项
bpy.ops.export_mesh.ply(
    filepath='//colors.ply',
    export_selected=True,
    export_colors=True
)
```

### UV丢失
```python
# 检查UV层
mesh = bpy.context.active_object.data
print(f"UV层: {[l.name for l in mesh.uv_layers]}")

# 检查UV数据
for uv in mesh.uv_layers:
    print(f"UV层: {uv.name}, 数据长度: {len(uv.data)}")

# 确保导出UV选项
bpy.ops.export_mesh.ply(
    filepath='//uv.ply',
    export_selected=True,
    export_uv=True
)
```

### 法线问题
```python
# 检查法线
mesh = bpy.context.active_object.data
print(f"自定义法线: {mesh.has_custom_normals}")

# 重新计算法线
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.select_all(action='SELECT')
bpy.ops.mesh.normals_make_consistent(inside=False)
bpy.ops.object.mode_set(mode='OBJECT')

# 确保导出法线选项
bpy.ops.export_mesh.ply(
    filepath='//normals.ply',
    export_selected=True,
    export_normals=True
)
```

### 轴向问题
```python
# 检查导出后的方向
# 在第三方软件中打开PLY文件检查

# 尝试不同轴向
bpy.ops.export_mesh.ply(
    filepath='//zy.ply',
    axis_forward='Z',
    axis_up='Y'
)

bpy.ops.export_mesh.ply(
    filepath='//-zy.ply',
    axis_forward='-Z',
    axis_up='Y'
)
```


---
## bpy_ops_import_curve

# bpy.ops.import_curve - 曲线导入操作模块

Blender曲线导入操作符，用于从各种文件格式导入曲线数据。

## 概述

`bpy.ops.import_curve` 模块提供用于从外部文件格式导入曲线数据的功能。支持SVG、DXF、AI等常见的曲线文件格式。

## 核心功能

### SVG导入

```python
import bpy

# 导入SVG文件
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    filter_btx=False,
    filter_collada=False,
    filter_alembic=False,
    directory='',
    files=[{'name': 'design.svg', 'name': 'design.svg'}],
    ui_tab='MAIN',
    use_sequence=False,
    use_svg_version='',
    scale=1.0,
    stroke_to_curve=True,
    fill=True,
    pre_simplify=True,
    pre_simplify_distance=0.1,
    apply_transforms=True,
    center=True,
    use_colors=True
)

# 导入SVG并创建曲线
bpy.ops.import_curve.svg(
    filepath='//vector_art.svg',
    stroke_to_curve=True,
    fill=True
)

# 导入SVG作为路径
bpy.ops.import_curve.svg(
    filepath='//path.svg',
    stroke_to_curve=True,
    fill=False
)
```

### DXF导入

```python
# 导入DXF文件
bpy.ops.import_curve.dxf(
    filepath='//drawing.dxf',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    filter_btx=False,
    filter_collada=False,
    filter_alembic=False,
    directory='',
    files=[{'name': 'drawing.dxf', 'name': 'drawing.dxf'}],
    ui_tab='MAIN',
    scale=1.0,
    apply_transforms=True,
    center=True,
    layers='ALL',
    colors=True,
    line_thickness=True,
    use_matcaps=False,
    use_plain_lines=True,
    use_default_illumination=True,
    backslash_as_forward=False
)

# 导入DXF中的所有层
bpy.ops.import_curve.dxf(
    filepath='//cad_drawing.dxf',
    layers='ALL'
)

# 只导入指定层
bpy.ops.import_curve.dxf(
    filepath='//cad_drawing.dxf',
    layers=['Layer1', 'Layer2']
)
```

### Adobe Illustrator导入

```python
# 导入AI文件
bpy.ops.import_curve.ai(
    filepath='//illustration.ai',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    filter_btx=False,
    filter_collada=False,
    filter_alembic=False,
    directory='',
    files=[{'name': 'illustration.ai', 'name': 'illustration.ai'}],
    ui_tab='MAIN',
    scale=1.0,
    apply_transforms=True,
    center=True,
    resolution=12,
    use_stroke=True,
    use_fill=True
)

# 高精度导入AI
bpy.ops.import_curve.ai(
    filepath='//logo.ai',
    resolution=24
)

# 只导入路径
bpy.ops.import_curve.ai(
    filepath='//logo.ai',
    use_stroke=True,
    use_fill=False
)
```

### 通用曲线导入选项

```python
# 设置导入比例
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    scale=0.01
)

# 应用变换
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    apply_transforms=True
)

# 居中曲线
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    center=True
)

# 导入后简化
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    pre_simplify=True,
    pre_simplify_distance=0.01
)
```

### 导入后处理

```python
# 导入后选择导入的曲线
bpy.ops.import_curve.svg(filepath='//design.svg')
bpy.ops.object.select_all(action='DESELECT')
for obj in bpy.context.selected_objects:
    if obj.type == 'CURVE':
        obj.select_set(True)
bpy.context.view_layer.objects.active = bpy.context.selected_objects[0]

# 合并导入的曲线
bpy.ops.import_curve.svg(filepath='//design.svg')
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.join()

# 分离导入的曲线
bpy.ops.import_curve.svg(filepath='//design.svg')
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.join()
bpy.ops.curve.separate()
```

## 实用函数

```python
def import_svg_file(filepath, scale=1.0, merge=True):
    """导入SVG文件并返回曲线对象"""
    bpy.ops.import_curve.svg(
        filepath=filepath,
        scale=scale,
        stroke_to_curve=True,
        fill=True
    )
    curves = [obj for obj in bpy.context.selected_objects if obj.type == 'CURVE']
    if merge and len(curves) > 1:
        bpy.ops.object.select_all(action='DESELECT')
        for obj in curves:
            obj.select_set(True)
        bpy.context.view_layer.objects.active = curves[0]
        bpy.ops.object.join()
        return [bpy.context.active_object]
    return curves

def import_dxf_file(filepath, layers=None, scale=1.0):
    """导入DXF文件"""
    kwargs = {
        'filepath': filepath,
        'scale': scale,
        'layers': 'ALL' if layers is None else layers
    }
    bpy.ops.import_curve.dxf(**kwargs)
    return list(bpy.context.selected_objects)

def batch_import_svg_files(filepaths, scale=1.0):
    """批量导入SVG文件"""
    curves = []
    for filepath in filepaths:
        result = import_svg_file(filepath, scale)
        curves.extend(result)
    return curves

def import_and_center_svg(filepath, scale=1.0):
    """导入SVG并居中"""
    bpy.ops.import_curve.svg(
        filepath=filepath,
        scale=scale,
        center=True
    )
    # 设置原点到几何中心
    bpy.ops.object.origin_set(type='ORIGIN_GEOMETRY', center='BOUNDS')
    return bpy.context.selected_objects

def import_svg_with_options(filepath, stroke=True, fill=True, simplify=True):
    """使用自定义选项导入SVG"""
    bpy.ops.import_curve.svg(
        filepath=filepath,
        stroke_to_curve=stroke,
        fill=fill,
        pre_simplify=simplify,
        pre_simplify_distance=0.1
    )
    return bpy.context.selected_objects

def convert_imported_to_bezier():
    """将导入的曲线转换为贝塞尔曲线"""
    for obj in bpy.context.selected_objects:
        if obj.type == 'CURVE':
            for spline in obj.data.splines:
                spline.type = 'BEZIER'
```

## 常见问题排查

### 导入后曲线不可见
```python
# 检查曲线是否选中
print(f"选中对象数: {len(bpy.context.selected_objects)}")

# 检查曲线类型
for obj in bpy.context.selected_objects:
    if obj.type == 'CURVE':
        print(f"曲线类型: {obj.data.splines[0].type if obj.data.splines else 'N/A'}")

# 检查渲染属性
obj = bpy.context.active_object
print(f"显示类型: {obj.display_type}")
print(f"渲染: {obj.hide_render}")

# 切换显示
for obj in bpy.context.selected_objects:
    obj.hide_viewport = False
    obj.hide_render = False
```

### 导入的曲线精度问题
```python
# 增加导入分辨率
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    scale=1.0
)

# 减少简化距离
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    pre_simplify=True,
    pre_simplify_distance=0.01
)

# 不简化
bpy.ops.import_curve.svg(
    filepath='//design.svg',
    pre_simplify=False
)

# 检查曲线点数
obj = bpy.context.active_object
if obj.type == 'CURVE':
    for spline in obj.data.splines:
        print(f"点数: {len(spline.bezier_points) if spline.type == 'BEZIER' else len(spline.points)}")
```

### 颜色丢失
```python
# 确保导入颜色
bpy.ops.import_curve.svg(
    filepath='//colored_design.svg',
    use_colors=True
)

# 检查材质
obj = bpy.context.active_object
if obj.data.materials:
    print(f"材质数: {len(obj.data.materials)}")
else:
    print("没有材质")

# 手动添加材质
mat = bpy.data.materials.new(name='ImportedColor')
mat.use_nodes = True
bsdf = mat.node_tree.nodes['Principled BSDF']
bsdf.inputs['Base Color'].default_value = (1, 0, 0, 1)
bpy.context.active_object.data.materials.append(mat)
```


---
## bpy_ops_export_curve

# bpy.ops.export_curve - 曲线导出操作模块

Blender曲线导出操作符，用于将曲线数据导出为各种文件格式。

## 概述

`bpy.ops.export_curve` 模块提供用于将Blender中的曲线、路径和矢量图形导出为外部文件格式的功能。

## 核心功能

### SVG导出

```python
import bpy

# 导出为SVG
bpy.ops.export_curve.svg(
    filepath='//output.svg',
    export_selected=True,
    apply_modifiers=True,
    split_curves=False,
    export_hair_curves=False,
    export_particle_systems=False,
    scale=1.0
)

# 导出选中曲线为SVG
bpy.ops.export_curve.svg(
    filepath='//curves.svg',
    export_selected=True
)

# 导出所有曲线为SVG
bpy.ops.export_curve.svg(
    filepath='//all_curves.svg',
    export_selected=False
)

# 分离曲线导出
bpy.ops.export_curve.svg(
    filepath='//separated.svg',
    split_curves=True
)
```

### DXF导出

```python
# 导出为DXF
bpy.ops.export_curve.dxf(
    filepath='//output.dxf',
    export_selected=True,
    apply_modifiers=True,
    scale=1.0,
    version='R12',
    use_mesh_data=False,
    use_line_endings=False
)

# 导出选中对象为DXF
bpy.ops.export_curve.dxf(
    filepath='//selected.dxf',
    export_selected=True
)

# 导出特定版本
bpy.ops.export_curve.dxf(
    filepath='//r14.dxf',
    version='R14'
)

bpy.ops.export_curve.dxf(
    filepath='//r2000.dxf',
    version='R2000'
)
```

### Adobe Illustrator导出

```python
# 导出为AI
bpy.ops.export_curve.ai(
    filepath='//output.ai',
    export_selected=True,
    scale=1.0,
    resolution=12,
    export_hair_curves=False
)

# 高精度导出
bpy.ops.export_curve.ai(
    filepath='//high_res.ai',
    resolution=24
)

# 只导出选中对象
bpy.ops.export_curve.ai(
    filepath='//selected.ai',
    export_selected=True
)
```

### 通用导出选项

```python
# 设置导出比例
bpy.ops.export_curve.svg(
    filepath='//scaled.svg',
    scale=2.0
)

# 应用修改器
bpy.ops.export_curve.svg(
    filepath='//with_modifiers.svg',
    apply_modifiers=True
)

# 导出毛发曲线
bpy.ops.export_curve.svg(
    filepath='//with_hair.svg',
    export_hair_curves=True
)

# 导出粒子系统
bpy.ops.export_curve.svg(
    filepath='//with_particles.svg',
    export_particle_systems=True
)
```

### 批量导出

```python
# 批量导出多个文件
def batch_export_svg(curves, base_path):
    for i, curve in enumerate(curves):
        bpy.ops.object.select_all(action='DESELECT')
        curve.select_set(True)
        bpy.context.view_layer.objects.active = curve
        bpy.ops.export_curve.svg(
            filepath=f'{base_path}_{i}.svg',
            export_selected=True
        )

# 导出所有曲线到目录
def export_all_curves_to_directory(output_dir):
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    curves = [obj for obj in bpy.data.objects if obj.type == 'CURVE']
    for curve in curves:
        bpy.ops.object.select_all(action='DESELECT')
        curve.select_set(True)
        bpy.context.view_layer.objects.active = curve
        filepath = os.path.join(output_dir, f'{curve.name}.svg')
        bpy.ops.export_curve.svg(filepath=filepath, export_selected=True)
```

## 实用函数

```python
def export_selected_as_svg(filepath, scale=1.0):
    """导出选中的曲线为SVG"""
    bpy.ops.export_curve.svg(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True,
        scale=scale
    )

def export_all_curves_as_svg(filepath):
    """导出所有曲线为SVG"""
    bpy.ops.export_curve.svg(
        filepath=filepath,
        export_selected=False,
        apply_modifiers=True
    )

def export_curves_to_dxf(filepath, selected_only=True, version='R12'):
    """导出曲线为DXF格式"""
    bpy.ops.export_curve.dxf(
        filepath=filepath,
        export_selected=selected_only,
        version=version
    )

def export_to_ai(filepath, resolution=12, selected_only=True):
    """导出为Adobe Illustrator格式"""
    bpy.ops.export_curve.ai(
        filepath=filepath,
        export_selected=selected_only,
        resolution=resolution
    )

def export_curve_with_options(filepath, format='svg', **options):
    """使用自定义选项导出曲线"""
    if format == 'svg':
        bpy.ops.export_curve.svg(filepath=filepath, **options)
    elif format == 'dxf':
        bpy.ops.export_curve.dxf(filepath=filepath, **options)
    elif format == 'ai':
        bpy.ops.export_curve.ai(filepath=filepath, **options)
    else:
        raise ValueError(f'不支持的格式: {format}')

def export_nurbs_to_spline(filepath):
    """将NURBS曲面导出为样条曲线SVG"""
    # 转换为贝塞尔曲线
    for obj in bpy.context.selected_objects:
        if obj.type == 'SURFACE':
            bpy.ops.object.convert(target='CURVE')
    
    bpy.ops.export_curve.svg(
        filepath=filepath,
        export_selected=True,
        apply_modifiers=True
    )
```

## 常见问题排查

### 导出为空
```python
# 检查选中的曲线
curves = [obj for obj in bpy.context.selected_objects if obj.type == 'CURVE']
print(f"选中曲线数: {len(curves)}")

# 检查所有曲线
all_curves = [obj for obj in bpy.data.objects if obj.type == 'CURVE']
print(f"总曲线数: {len(all_curves)}")

# 确保曲线有数据
for curve in all_curves:
    print(f"曲线点数: {sum(len(spline.bezier_points) if spline.type == 'BEZIER' else len(spline.points) for spline in curve.data.splines)}")

# 取消隐藏
for curve in all_curves:
    curve.hide_viewport = False
```

### 导出比例错误
```python
# 检查导出比例
bpy.ops.export_curve.svg(filepath='//test.svg', scale=1.0)

# 检查对象缩放
obj = bpy.context.active_object
print(f"对象缩放: {obj.scale}")

# 应用变换
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)

# 重新导出
bpy.ops.export_curve.svg(filepath='//corrected.svg', scale=1.0)
```

### 导出格式不支持
```python
# 检查导出格式
print("支持的格式: SVG, DXF, AI")

# 检查版本兼容性
bpy.ops.export_curve.dxf(filepath='//test.dxf', version='R12')

# 尝试不同版本
for version in ['R12', 'R14', 'R2000', 'R2004', 'R2007', 'R2010', 'R2013', 'R2018']:
    try:
        bpy.ops.export_curve.dxf(filepath=f'//test_{version}.dxf', version=version)
        print(f"版本 {version} 可用")
    except:
        print(f"版本 {version} 不可用")
```


---
## bpy_ops_import_scene

# bpy.ops.import_scene - Scene Import Operations Module

Blender 场景导入操作符，用于将各种格式的 3D 场景文件导入到 Blender 中。

## Overview

`bpy.ops.import_scene` 模块包含场景导入操作命令，支持多种 3D 格式的导入，包括 FBX、OBJ、3DS、GLTF 等常用格式。这个模块提供了灵活的导入参数配置。

## 核心功能

### 导入格式支持

```python
import bpy

# 导入 FBX 格式
bpy.ops.import_scene.fbx(
    filepath="C:/models/character.fbx",
    axis_forward='-Z',
    axis_up='Y',
    apply_unit_scale=True,
    apply_transform=True,
    use_custom_normals=True,
    use_animations=True,
    use_anim_action_all=True,
    batch_mode='OFF',
    use_subset_collection=False
)

# 导入 OBJ 格式
bpy.ops.import_scene.obj(
    filepath="C:/models/model.obj",
    axis_forward='-Z',
    axis_up='Y',
    use_edges=True,
    use_smooth_groups=True,
    use_split_objects=True,
    use_split_groups=True,
    use_groups_as_vgroups=False,
    use_image_search=True,
    split_mode='OFF'
)

# 导入 GLTF/GLB 格式
bpy.ops.import_scene.gltf(
    filepath="C:/models/scene.glb",
    export_format='GLB',
    import_pack_images=True,
    use_eps=False,
    legacy_import_vectors=False,
    export_copyright="",
    export_image_format='AUTO'
)
```

### FBX 导入选项

```python
def import_fbx(filepath, import_animations=True):
    """导入 FBX 文件"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        axis_forward='-Z',
        axis_up='Y',
        apply_unit_scale=True,
        apply_transform=True,
        use_custom_normals=True,
        use_animations=import_animations,
        use_anim_action_all=True,
        batch_mode='OFF'
    )
    print(f"已导入: {filepath}")
    return bpy.context.selected_objects

def import_fbx_with_options(
    filepath,
    scale=1.0,
    import_anim=True,
    import_materials=True
):
    """带选项的 FBX 导入"""
    bpy.ops.import_scene.fbx(
        filepath=filepath,
        axis_forward='-Z',
        axis_up='Y',
        use_unit_scale=scale,
        use_custom_normals=True,
        use_animations=import_anim,
        use_anim_action_all=True,
        force_connect_children=False,
        automatic_bone_orientation=True
    )
    return bpy.context.selected_objects
```

### OBJ 导入选项

```python
def import_obj(filepath, split_by_material=True):
    """导入 OBJ 文件"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        axis_forward='-Z',
        axis_up='Y',
        use_edges=True,
        use_smooth_groups=True,
        use_split_objects=not split_by_material,
        use_split_groups=split_by_material,
        use_groups_as_vgroups=False,
        use_image_search=True,
        split_mode='OFF'
    )
    print(f"已导入 OBJ: {filepath}")
    return bpy.context.selected_objects

def import_obj_with_materials(filepath):
    """导入 OBJ 并处理材质"""
    bpy.ops.import_scene.obj(
        filepath=filepath,
        axis_forward='-Z',
        axis_up='Y',
        use_edges=True,
        use_smooth_groups=True,
        use_split_objects=True,
        use_groups_as_vgroups=False,
        use_image_search=True,
        createMaterials=True,
        mtl_path="",  # 自动查找同名 .mtl 文件
        split_mode='OFF'
    )
    return bpy.context.selected_objects
```

### GLTF 导入选项

```python
def import_gltf(filepath, pack_images=True):
    """导入 GLTF/GLB 文件"""
    bpy.ops.import_scene.gltf(
        filepath=filepath,
        import_pack_images=pack_images,
        export_format='GLB',
        # 其他选项使用默认值
    )
    print(f"已导入 GLTF: {filepath}")
    return bpy.context.selected_objects

def import_gltf_animations(filepath):
    """导入 GLTF 动画"""
    bpy.ops.import_scene.gltf(
        filepath=filepath,
        import_pack_images=True,
        export_format='GLB',
        export_animations=True,
        export_frame_range=True,
        export_nla_strips=True
    )
    return bpy.context.selected_objects
```

### 3DS 导入选项

```python
# 导入 3DS 格式
bpy.ops.import_scene.autodesk_3ds(
    filepath="C:/models/model.3ds",
    axis_forward='-Z',
    axis_up='Y',
    use_unit_scale=True,
    use_image_search=True,
    resolve_lib_collections=True
)

def import_3ds(filepath):
    """导入 3DS 文件"""
    bpy.ops.import_scene.autodesk_3ds(
        filepath=filepath,
        axis_forward='-Z',
        axis_up='Y',
        use_unit_scale=True,
        use_image_search=True
    )
    print(f"已导入 3DS: {filepath}")
    return bpy.context.selected_objects
```

### LWO 导入选项

```python
# 导入 LightWave 格式
bpy.ops.import_scene.lwo(
    filepath="C:/models/model.lwo",
    axis_forward='-Z',
    axis_up='Y',
    use_unit_scale=True,
    use_image_search=True
)

def import_lwo(filepath):
    """导入 LWO 文件"""
    bpy.ops.import_scene.lwo(
        filepath=filepath,
        axis_forward='-Z',
        axis_up='Y',
        use_unit_scale=True
    )
    return bpy.context.selected_objects
```

### X3D 导入选项

```python
# 导入 X3D 格式
bpy.ops.import_scene.x3d(
    filepath="C:/models/scene.x3d",
    axis_forward='-Z',
    axis_up='Y',
    use_unit_scale=True,
    use_triangulated_mesh=False,
    use_normals=True,
    use_compression=True
)

def import_x3d(filepath):
    """导入 X3D 文件"""
    bpy.ops.import_scene.x3d(
        filepath=filepath,
        axis_forward='-Z',
        axis_up='Y',
        use_unit_scale=True
    )
    return bpy.context.selected_objects
```

## 实际应用示例

### 批量导入管理器

```python
import bpy
import os
from pathlib import Path

class BatchImportManager:
    """批量导入管理器"""
    
    def __init__(self):
        self.imported_files = []
        self.failed_files = []
    
    def detect_format(self, filepath):
        """检测文件格式"""
        ext = Path(filepath).suffix.lower()
        format_map = {
            '.fbx': 'fbx',
            '.obj': 'obj',
            '.gltf': 'gltf',
            '.glb': 'gltf',
            '.3ds': '3ds',
            '.lwo': 'lwo',
            '.x3d': 'x3d',
            '.dae': 'collada'
        }
        return format_map.get(ext, 'unknown')
    
    def import_file(self, filepath, options=None):
        """导入单个文件"""
        if options is None:
            options = {}
        
        format_type = self.detect_format(filepath)
        
        try:
            if format_type == 'fbx':
                bpy.ops.import_scene.fbx(filepath=filepath, **options)
            elif format_type == 'obj':
                bpy.ops.import_scene.obj(filepath=filepath, **options)
            elif format_type == 'gltf':
                bpy.ops.import_scene.gltf(filepath=filepath, **options)
            elif format_type == '3ds':
                bpy.ops.import_scene.autodesk_3ds(filepath=filepath, **options)
            elif format_type == 'lwo':
                bpy.ops.import_scene.lwo(filepath=filepath, **options)
            elif format_type == 'x3d':
                bpy.ops.import_scene.x3d(filepath=filepath, **options)
            else:
                print(f"未知格式: {filepath}")
                self.failed_files.append(filepath)
                return None
            
            self.imported_files.append(filepath)
            return bpy.context.selected_objects
        except Exception as e:
            print(f"导入失败: {filepath} - {e}")
            self.failed_files.append(filepath)
            return None
    
    def import_directory(self, directory, extensions=None, recursive=True):
        """导入目录中的所有文件"""
        if extensions is None:
            extensions = ['.fbx', '.obj', '.gltf', '.glb']
        
        path = Path(directory)
        imported = []
        
        if recursive:
            files = path.rglob("*")
        else:
            files = path.glob("*")
        
        for file_path in files:
            if file_path.is_file() and file_path.suffix.lower() in extensions:
                result = self.import_file(str(file_path))
                if result:
                    imported.append(str(file_path))
        
        return imported
    
    def get_import_report(self):
        """获取导入报告"""
        return {
            'successful': len(self.imported_files),
            'failed': len(self.failed_files),
            'imported_files': self.imported_files.copy(),
            'failed_files': self.failed_files.copy()
        }


def batch_import_models(directory):
    """批量导入模型"""
    manager = BatchImportManager()
    
    print(f"开始导入目录: {directory}")
    imported = manager.import_directory(
        directory,
        extensions=['.fbx', '.obj', '.glb']
    )
    
    report = manager.get_import_report()
    print(f"\n导入完成:")
    print(f"  成功: {report['successful']} 个文件")
    print(f"  失败: {report['failed']} 个文件")
    
    return manager
```

### 导入场景设置器

```python
import bpy

class ImportSceneSetup:
    """导入场景设置器"""
    
    def setup_after_import(self, collection_name=None):
        """导入后的场景设置"""
        # 创建集合
        if collection_name:
            if collection_name not in bpy.data.collections:
                bpy.ops.object.collection_add(name=collection_name)
            collection = bpy.data.collections[collection_name]
        
        # 整理对象
        for obj in bpy.context.selected_objects:
            obj.select_set(True)
            
            # 重置变换
            obj.location = (0, 0, 0)
            obj.rotation_euler = (0, 0, 0)
            obj.scale = (1, 1, 1)
            
            # 添加到集合
            if collection_name:
                bpy.context.view_layer.active_layer_collection.collection.objects.link(obj)
                bpy.data.collections[collection_name].objects.unlink(obj)
        
        # 应用所有变换
        bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
        
        # 设置渲染引擎
        bpy.context.scene.render.engine = 'CYCLES'
        
        # 设置渲染分辨率
        bpy.context.scene.render.resolution_x = 1920
        bpy.context.scene.render.resolution_y = 1080
        
        return bpy.context.selected_objects
    
    def import_with_setup(self, filepath, collection_name="Imported"):
        """导入并设置场景"""
        # 清除现有对象
        bpy.ops.object.select_all(action='SELECT')
        bpy.ops.object.delete()
        
        # 导入文件
        result = bpy.ops.import_scene.gltf(filepath=filepath)
        
        if 'FINISHED' in result:
            return self.setup_after_import(collection_name)
        
        return None
    
    def create_import_preset(self, name, settings):
        """创建导入预设"""
        preset = {
            'name': name,
            'settings': settings
        }
        return preset


def import_and_prepare_scene(filepath, collection_name="Imported"):
    """导入并准备场景"""
    setup = ImportSceneSetup()
    return setup.import_with_setup(filepath, collection_name)
```

### 材质和纹理处理

```python
import bpy
import os

class MaterialProcessor:
    """材质处理器"""
    
    def __init__(self):
        self.materials_created = 0
        self.textures_found = 0
    
    def find_textures_in_directory(self, directory, extensions=None):
        """查找目录中的纹理文件"""
        if extensions is None:
            extensions = ['.jpg', '.jpeg', '.png', '.tga', '.tif', '.tiff']
        
        textures = {}
        for root, dirs, files in os.walk(directory):
            for file in files:
                if any(file.lower().endswith(ext) for ext in extensions):
                    name = os.path.splitext(file)[0]
                    if name not in textures:
                        textures[name] = []
                    textures[name].append(os.path.join(root, file))
        
        self.textures_found = sum(len(paths) for paths in textures.values())
        return textures
    
    def apply_texture_to_material(self, material_name, texture_path):
        """将纹理应用到材质"""
        if material_name not in bpy.data.materials:
            return False
        
        mat = bpy.data.materials[material_name]
        mat.use_nodes = True
        
        nodes = mat.node_tree.nodes
        links = mat.node_tree.links
        
        # 创建纹理节点
        tex_node = nodes.new(type='ShaderNodeTexImage')
        tex_node.location = (-300, 0)
        
        # 加载图像
        if os.path.exists(texture_path):
            image = bpy.data.images.load(texture_path)
            tex_node.image = image
        
        # 连接到原理化 BSDF
        principled = nodes.get('Principled BSDF')
        if principled:
            links.new(tex_node.outputs['Color'], principled.inputs['Base Color'])
        
        return True
    
    def create_material_from_texture(self, texture_path, material_name=None):
        """从纹理创建材质"""
        if material_name is None:
            material_name = os.path.splitext(os.path.basename(texture_path))[0]
        
        # 创建材质
        mat = bpy.data.materials.new(name=material_name)
        mat.use_nodes = True
        
        nodes = mat.node_tree.nodes
        links = mat.node_tree.links
        
        # 获取原理化 BSDF
        principled = nodes.get('Principled BSDF')
        if not principled:
            principled = nodes.new(type='ShaderNodeBsdfPrincipled')
        
        # 创建纹理节点
        tex_node = nodes.new(type='ShaderNodeTexImage')
        tex_node.location = (-400, 0)
        
        if os.path.exists(texture_path):
            image = bpy.data.images.load(texture_path)
            tex_node.image = image
        
        # 连接节点
        links.new(tex_node.outputs['Color'], principled.inputs['Base Color'])
        
        self.materials_created += 1
        return mat
    
    def process_imported_materials(self, base_path):
        """处理导入的材质"""
        textures = self.find_textures_in_directory(base_path)
        materials = {}
        
        for name, paths in textures.items():
            if paths:
                mat = self.create_material_from_texture(paths[0], name)
                materials[name] = mat
        
        return materials


def setup_materials_for_imported_objects(base_path):
    """为导入的对象设置材质"""
    processor = MaterialProcessor()
    materials = processor.process_imported_materials(base_path)
    
    print(f"创建了 {processor.materials_created} 个材质")
    print(f"找到 {processor.textures_found} 个纹理")
    
    return materials
```


---
## bpy_ops_export_scene

# bpy.ops.export_scene - Scene Export Operations Module

Blender 场景导出操作符，用于将 Blender 场景导出为各种 3D 格式文件。

## Overview

`bpy.ops.export_scene` 模块包含场景导出操作命令，支持将场景导出为 FBX、OBJ、GLTF、3DS 等常用 3D 格式。这个模块提供了丰富的导出选项，满足不同平台和工作流程的需求。

## 核心功能

### 导出格式支持

```python
import bpy

# 导出 FBX 格式
bpy.ops.export_scene.fbx(
    filepath="C:/exports/character.fbx",
    use_selection=False,
    axis_forward='-Z',
    axis_up='Y',
    apply_unit_scale=True,
    apply_scale_options='FBX_SCALE_ALL',
    bake_space_transform=False,
    use_custom_normals=True,
    use_animations=True,
    use_anim_action_all=True,
    export_anim=True,
    export_anim_shape_keys=True,
    export_anim_use_nla_strips=True,
    export_anim_free_NLA=False,
    export_anim_use_all_bones=True,
    export_anim_force_euler_rotation=True,
    export_current_frame=True,
    export_skins=True,
    export_all_influences=False,
    export_deform_bones_only=False,
    export_lights=True,
    export_cameras=True,
    export_materials=True,
    export_textures=True,
    export_yup=True
)

# 导出 OBJ 格式
bpy.ops.export_scene.obj(
    filepath="C:/exports/model.obj",
    use_selection=False,
    use_triangles=False,
    use_normals=True,
    use_uvs=True,
    use_materials=True,
    use_nurbs=False,
    use_vertex_groups=False,
    use_mesh_modifiers=True,
    use_smooth_groups=False,
    use_smooth_groups_bitflags=False,
    use_polygon_groups=False,
    keep_vertex_order=False,
    export_uvs=True,
    export_normals=True,
    export_materials=True,
    export_cameras=False,
    export_lights=False,
    apply_modifiers=True,
    group_by_object=False,
    group_by_material=False,
    keep_vertex_order=True
)

# 导出 GLTF 格式
bpy.ops.export_scene.gltf(
    filepath="C:/exports/scene.glb",
    export_format='GLB',
    use_selection=False,
    export_apply=True,
    export_texcoords=True,
    export_normals=True,
    export_draco_mesh_compression_enable=False,
    export_draco_mesh_compression_level=6,
    export_draco_position_quantization=14,
    export_draco_normal_quantization=10,
    export_draco_texcoord_quantization=12,
    export_materials='EXPORT',
    export_colors=True,
    export_cameras=False,
    export_lights=False,
    export_yup=True,
    export_animations=True,
    export_frame_range=True,
    export_frame_step=1,
    export_nla_strips=True,
    export_nla_strip_merged_animation_name='NLA Merged',
    export_anim_single_armature=True,
    export_anim_use_nla_strips=True,
    export_anim_use_all_actions=True,
    export_anim_action_from_nla=False,
    export_anim_force_euler_rotation=True,
    export_anim_bake_all_skipped=False,
    export_anim_no_root_bones=False,
    export_anim_use_current_frame=False,
    export_anim_loop_restart=True
)

# 导出 ABC 格式（Alembic）
bpy.ops.export_scene.alembic(
    filepath="C:/exports/scene.abc",
    apply_unit_scale=True,
    export_hair=True,
    export_particles=True,
    export_anim=True,
    export_anim_include='SELECTED_OBJECTS',
    export_anim_hierarchy=True,
    export_anim_cacheable=True,
    export_anim_range=True,
    export_anim_frame_start=1,
    export_anim_frame_end=250,
    export_anim_step=1,
    export_anim_proxy=True,
    export_surface_type='MESH',
    export_group_by_object=False,
    export_group_by_collection=False,
    use_instance_collection=True,
    export_material_assignments=True
)
```

### FBX 导出详细选项

```python
def export_fbx(filepath, objects=None, animations=True, materials=True):
    """导出 FBX 文件"""
    if objects is None:
        objects = bpy.context.scene.objects
    
    # 选择对象
    bpy.ops.object.select_all(action='DESELECT')
    for obj in objects:
        obj.select_set(True)
    
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=True,
        axis_forward='-Z',
        axis_up='Y',
        apply_unit_scale=True,
        use_custom_normals=True,
        use_animations=animations,
        use_anim_action_all=True,
        export_anim=animations,
        export_skins=True,
        export_lights=True,
        export_cameras=True,
        export_materials=materials,
        export_textures=False,
        export_yup=True
    )
    print(f"已导出 FBX: {filepath}")
    return True

def export_fbx_animations_only(filepath, action_name=None):
    """仅导出动画"""
    if action_name:
        action = bpy.data.actions.get(action_name)
        if action:
            action.use_fake_user = True
    
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=False,
        export_anim=True,
        export_anim_shape_keys=True,
        export_anim_use_nla_strips=True,
        export_skins=False,
        export_materials=False,
        export_cameras=False,
        export_lights=False
    )
    return True
```

### OBJ 导出详细选项

```python
def export_obj(filepath, selected_only=True, apply_modifiers=True):
    """导出 OBJ 文件"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        use_selection=selected_only,
        use_triangles=False,
        use_normals=True,
        use_uvs=True,
        use_materials=True,
        use_mesh_modifiers=apply_modifiers,
        export_uvs=True,
        export_normals=True,
        export_materials=True,
        export_cameras=False,
        export_lights=False,
        apply_modifiers=apply_modifiers,
        group_by_object=False,
        group_by_material=False,
        keep_vertex_order=True
    )
    print(f"已导出 OBJ: {filepath}")
    return True

def export_obj_with_mtl(filepath, selected_only=True):
    """导出 OBJ 并包含 MTL 文件"""
    bpy.ops.export_scene.obj(
        filepath=filepath,
        use_selection=selected_only,
        use_triangles=False,
        use_normals=True,
        use_uvs=True,
        use_materials=True,
        export_uvs=True,
        export_normals=True,
        export_materials=True,
        export_cameras=False,
        export_lights=False
    )
    
    # OBJ 导出自动生成同名 .mtl 文件
    mtl_path = filepath.replace('.obj', '.mtl')
    if os.path.exists(mtl_path):
        print(f"MTL 文件已生成: {mtl_path}")
    
    return True
```

### GLTF 导出详细选项

```python
def export_gltf(filepath, format='GLB', selected_only=False, export_anim=True):
    """导出 GLTF/GLB 文件"""
    fmt_map = {'GLB': 'GLB', 'GLTF_EMBEDDED': 'GLTF_EMBEDDED', 'GLTF_SEPARATE': 'GLTF_SEPARATE'}
    
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format=fmt_map.get(format, 'GLB'),
        use_selection=selected_only,
        export_apply=True,
        export_texcoords=True,
        export_normals=True,
        export_materials='EXPORT',
        export_colors=True,
        export_cameras=False,
        export_lights=False,
        export_animations=export_anim,
        export_frame_range=True,
        export_frame_step=1,
        export_nla_strips=True,
        export_anim_use_all_actions=True,
        export_anim_force_euler_rotation=True
    )
    print(f"已导出 GLTF: {filepath}")
    return True

def export_gltf_web(filepath):
    """导出适合 Web 使用的 GLTF"""
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format='GLB',
        use_selection=False,
        export_texcoords=True,
        export_normals=True,
        export_materials='EXPORT',
        export_colors=True,
        export_cameras=False,
        export_lights=False,
        export_animations=True,
        export_frame_range=True,
        export_nla_strips=True,
        export_anim_use_all_actions=True
    )
    return True

def export_gltf_animations(filepath, frame_start=1, frame_end=250):
    """导出 GLTF 动画"""
    bpy.ops.export_scene.gltf(
        filepath=filepath,
        export_format='GLB',
        use_selection=False,
        export_animations=True,
        export_anim_use_all_actions=True,
        export_anim_action_from_nla=False,
        export_anim_frame_start=frame_start,
        export_anim_frame_end=frame_end,
        export_nla_strips=True,
        export_nla_strip_merged_animation_name='ExportedAnimation'
    )
    return True
```

## 实际应用示例

### 批量导出管理器

```python
import bpy
import os
from pathlib import Path

class BatchExportManager:
    """批量导出管理器"""
    
    def __init__(self, export_dir):
        self.export_dir = export_dir
        self.exported_files = []
        self.failed_files = []
        os.makedirs(export_dir, exist_ok=True)
    
    def export_selected_objects(self, basename, format='fbx'):
        """导出选中的对象"""
        objects = bpy.context.selected_objects
        if not objects:
            print("没有选中的对象")
            return None
        
        filepath = os.path.join(self.export_dir, f"{basename}.{format}")
        
        if format == 'fbx':
            bpy.ops.export_scene.fbx(filepath=filepath, use_selection=True)
        elif format == 'obj':
            bpy.ops.export_scene.obj(filepath=filepath, use_selection=True)
        elif format == 'gltf':
            bpy.ops.export_scene.gltf(filepath=filepath, use_selection=True)
        
        self.exported_files.append(filepath)
        return filepath
    
    def export_by_collection(self, format='fbx'):
        """按集合导出"""
        for collection in bpy.data.collections:
            if not collection.objects:
                continue
            
            # 选择集合中的所有对象
            bpy.ops.object.select_all(action='DESELECT')
            for obj in collection.objects:
                obj.select_set(True)
            
            # 导出
            basename = collection.name.replace(' ', '_')
            filepath = os.path.join(self.export_dir, f"{basename}.{format}")
            
            try:
                if format == 'fbx':
                    bpy.ops.export_scene.fbx(filepath=filepath, use_selection=True)
                elif format == 'gltf':
                    bpy.ops.export_scene.gltf(filepath=filepath, use_selection=True)
                
                self.exported_files.append(filepath)
                print(f"已导出: {filepath}")
            except Exception as e:
                self.failed_files.append((collection.name, str(e)))
                print(f"导出失败: {collection.name} - {e}")
    
    def export_animation_ranges(self, format='gltf'):
        """按动画范围导出"""
        scene = bpy.context.scene
        
        # 导出当前动画范围
        filepath = os.path.join(self.export_dir, f"animation_{scene.frame_start}_{scene.frame_end}.{format}")
        
        if format == 'gltf':
            bpy.ops.export_scene.gltf(
                filepath=filepath,
                export_format='GLB',
                use_selection=False,
                export_animations=True,
                export_anim_frame_start=scene.frame_start,
                export_anim_frame_end=scene.frame_end
            )
        
        self.exported_files.append(filepath)
        return filepath
    
    def export_all_actions(self, format='gltf'):
        """导出所有动作"""
        for action in bpy.data.actions:
            if action.users == 0 and not action.use_fake_user:
                continue
            
            # 设置时间轴到动作范围
            if action.frame_range[1] > action.frame_range[0]:
                bpy.context.scene.frame_start = int(action.frame_range[0])
                bpy.context.scene.frame_end = int(action.frame_range[1])
            
            # 导出
            safe_name = "".join(c if c.isalnum() or c in ('_', '-') else '_' for c in action.name)
            filepath = os.path.join(self.export_dir, f"{safe_name}.{format}")
            
            if format == 'gltf':
                bpy.ops.export_scene.gltf(
                    filepath=filepath,
                    export_format='GLB',
                    export_animations=True,
                    export_anim_use_all_actions=False
                )
            
            self.exported_files.append(filepath)
        
        return self.exported_files
    
    def get_export_report(self):
        """获取导出报告"""
        return {
            'successful': len(self.exported_files),
            'failed': len(self.failed_files),
            'exported_files': self.exported_files.copy(),
            'failed_files': self.failed_files.copy()
        }


def batch_export_scene(export_dir, format='gltf'):
    """批量导出场景"""
    manager = BatchExportManager(export_dir)
    
    # 导出所有集合
    manager.export_by_collection(format=format)
    
    report = manager.get_export_report()
    print(f"\n导出完成:")
    print(f"  成功: {report['successful']} 个文件")
    print(f"  失败: {report['failed']} 个文件")
    
    return manager
```

### 导出预设系统

```python
import bpy
import json

class ExportPresetManager:
    """导出预设管理器"""
    
    presets = {}
    
    @classmethod
    def save_preset(cls, name, settings):
        """保存预设"""
        cls.presets[name] = settings
        return cls.presets
    
    @classmethod
    def get_preset(cls, name):
        """获取预设"""
        return cls.presets.get(name)
    
    @classmethod
    def list_presets(cls):
        """列出所有预设"""
        return list(cls.presets.keys())
    
    @classmethod
    def delete_preset(cls, name):
        """删除预设"""
        if name in cls.presets:
            del cls.presets[name]
            return True
        return False
    
    @classmethod
    def create_fbx_preset(cls, name, preset_type='character'):
        """创建 FBX 预设"""
        presets = {
            'character': {
                'use_selection': False,
                'export_anim': True,
                'export_skins': True,
                'export_materials': True,
                'export_lights': True,
                'export_cameras': True,
                'axis_forward': '-Z',
                'axis_up': 'Y'
            },
            'game_asset': {
                'use_selection': True,
                'export_anim': False,
                'export_skins': True,
                'export_materials': True,
                'export_lights': False,
                'export_cameras': False,
                'use_triangles': True,
                'apply_scale_options': 'FBX_SCALE_ALL'
            },
            'animation_only': {
                'use_selection': False,
                'export_anim': True,
                'export_skins': False,
                'export_materials': False,
                'export_lights': False,
                'export_cameras': False,
                'export_anim_use_all_actions': True
            }
        }
        
        if preset_type in presets:
            cls.presets[name] = presets[preset_type]
        
        return cls.presets[name]
    
    @classmethod
    def create_gltf_preset(cls, name, preset_type='web'):
        """创建 GLTF 预设"""
        presets = {
            'web': {
                'export_format': 'GLB',
                'use_selection': False,
                'export_materials': 'EXPORT',
                'export_animations': True,
                'export_cameras': False,
                'export_lights': False,
                'export_texcoords': True,
                'export_normals': True
            },
            'ar': {
                'export_format': 'GLB',
                'use_selection': True,
                'export_materials': 'EXPORT',
                'export_animations': False,
                'export_cameras': False,
                'export_lights': False,
                'export_draco_mesh_compression_enable': True
            },
            'interchange': {
                'export_format': 'GLTF_SEPARATE',
                'use_selection': False,
                'export_materials': 'EXPORT',
                'export_animations': True,
                'export_cameras': True,
                'export_lights': True,
                'export_texcoords': True,
                'export_normals': True
            }
        }
        
        if preset_type in presets:
            cls.presets[name] = presets[preset_type]
        
        return cls.presets[name]
    
    @classmethod
    def apply_preset(cls, name, filepath, format='fbx'):
        """应用预设导出"""
        preset = cls.presets.get(name)
        if not preset:
            print(f"未找到预设: {name}")
            return None
        
        kwargs = preset.copy()
        kwargs['filepath'] = filepath
        
        if format == 'fbx':
            bpy.ops.export_scene.fbx(**kwargs)
        elif format == 'gltf':
            bpy.ops.export_scene.gltf(**kwargs)
        
        return filepath
    
    @classmethod
    def export_to_preset(cls, preset_name, basename, export_dir, format='fbx'):
        """使用预设导出"""
        filepath = os.path.join(export_dir, f"{basename}.{format}")
        return cls.apply_preset(preset_name, filepath, format)


def setup_export_presets():
    """设置导出预设"""
    ExportPresetManager.create_fbx_preset('fbx_character', 'character')
    ExportPresetManager.create_fbx_preset('fbx_game_asset', 'game_asset')
    ExportPresetManager.create_fbx_preset('fbx_animation', 'animation_only')
    ExportPresetManager.create_gltf_preset('gltf_web', 'web')
    ExportPresetManager.create_gltf_preset('gltf_ar', 'ar')
    ExportPresetManager.create_gltf_preset('gltf_interchange', 'interchange')
    
    print("导出预设已创建:")
    print(ExportPresetManager.list_presets())
    
    return ExportPresetManager
```

### 游戏资产导出器

```python
import bpy
import os

class GameAssetExporter:
    """游戏资产导出器"""
    
    def __init__(self, export_dir):
        self.export_dir = export_dir
        os.makedirs(export_dir, exist_ok=True)
    
    def prepare_asset(self, obj, name):
        """准备资产"""
        # 复制对象
        bpy.ops.object.select_all(action='DESELECT')
        obj.select_set(True)
        bpy.context.view_layer.objects.active = obj
        bpy.ops.object.duplicate(linked=False)
        
        duplicate = bpy.context.active_object
        duplicate.name = name
        
        # 应用所有修改器
        for modifier in duplicate.modifiers:
            bpy.ops.object.modifier_apply(modifier=modifier.name)
        
        # 重置变换
        duplicate.location = (0, 0, 0)
        duplicate.rotation_euler = (0, 0, 0)
        duplicate.scale = (1, 1, 1)
        
        # 移除不需要的集合
        for collection in duplicate.users_collection:
            collection.objects.unlink(duplicate)
        
        return duplicate
    
    def export_static_mesh(self, obj, name, formats=None):
        """导出静态网格"""
        if formats is None:
            formats = ['fbx', 'obj', 'glb']
        
        prepared = self.prepare_asset(obj, name)
        results = []
        
        for fmt in formats:
            filepath = os.path.join(self.export_dir, f"{name}.{fmt}")
            
            if fmt == 'fbx':
                bpy.ops.export_scene.fbx(
                    filepath=filepath,
                    use_selection=True,
                    apply_scale_options='FBX_SCALE_ALL',
                    use_triangles=True,
                    export_anim=False,
                    export_materials=True
                )
            elif fmt == 'obj':
                bpy.ops.export_scene.obj(
                    filepath=filepath,
                    use_selection=True,
                    use_triangles=False,
                    export_anim=False,
                    export_materials=True
                )
            elif fmt == 'glb':
                bpy.ops.export_scene.gltf(
                    filepath=filepath,
                    export_format='GLB',
                    use_selection=True,
                    export_animations=False,
                    export_materials='EXPORT'
                )
            
            results.append(filepath)
        
        # 删除复制的对象
        bpy.data.objects.remove(prepared)
        
        return results
    
    def export_animated_mesh(self, obj, name, formats=None):
        """导出带动画的网格"""
        if formats is None:
            formats = ['fbx', 'glb']
        
        prepared = self.prepare_asset(obj, name)
        results = []
        
        for fmt in formats:
            filepath = os.path.join(self.export_dir, f"{name}_animated.{fmt}")
            
            if fmt == 'fbx':
                bpy.ops.export_scene.fbx(
                    filepath=filepath,
                    use_selection=True,
                    apply_scale_options='FBX_SCALE_ALL',
                    use_triangles=True,
                    export_anim=True,
                    export_skins=True,
                    export_anim_use_all_actions=True
                )
            elif fmt == 'glb':
                bpy.ops.export_scene.gltf(
                    filepath=filepath,
                    export_format='GLB',
                    use_selection=True,
                    export_animations=True,
                    export_anim_use_all_actions=True
                )
            
            results.append(filepath)
        
        return results
    
    def export_materials_only(self, mat, name):
        """仅导出材质信息"""
        # 材质无法直接导出，这里生成材质配置文件
        config = {
            'name': mat.name,
            'use_nodes': mat.use_nodes,
            'blend_method': mat.blend_method,
            'shadow_method': mat.shadow_method
        }
        
        if mat.use_nodes:
            config['node_tree'] = {
                'nodes': [n.type for n in mat.node_tree.nodes],
                'links': len(mat.node_tree.links)
            }
        
        filepath = os.path.join(self.export_dir, f"{name}_material.json")
        
        with open(filepath, 'w', encoding='utf-8') as f:
            json.dump(config, f, indent=2)
        
        return filepath
    
    def export_texture_reference(self, image, name):
        """导出纹理引用"""
        filepath = os.path.join(self.export_dir, f"{name}_texture.txt")
        
        with open(filepath, 'w', encoding='utf-8') as f:
            f.write(f"Name: {image.name}\n")
            f.write(f"Size: {image.size[0]} x {image.size[1]}\n")
            f.write(f"Has Alpha: {image.alpha_mode != 'NONE'}\n")
            f.write(f"Source Path: {image.filepath}\n")
        
        return filepath


def export_game_assets(export_dir, selected_only=True):
    """导出游戏资产"""
    exporter = GameAssetExporter(export_dir)
    
    if selected_only:
        objects = bpy.context.selected_objects
    else:
        objects = list(bpy.data.objects)
    
    results = {
        'static_meshes': [],
        'animated_meshes': [],
        'materials': [],
        'textures': []
    }
    
    for obj in objects:
        if obj.type == 'MESH':
            # 检查是否有动画
            has_animation = any(
                fcurves for fcurves in obj.animation_data.action.fcurves
            ) if obj.animation_data and obj.animation_data.action else False
            
            name = obj.name.replace(' ', '_')
            
            if has_animation:
                exported = exporter.export_animated_mesh(obj, name)
                results['animated_meshes'].extend(exported)
            else:
                exported = exporter.export_static_mesh(obj, name)
                results['static_meshes'].extend(exported)
        
        elif obj.type == 'MATERIAL':
            results['materials'].append(
                exporter.export_materials_only(obj, obj.name)
            )
        
        elif obj.type == 'EMPTY' and obj.instance_type == 'COLLECTION':
            # 处理实例化的集合
            pass
    
    print(f"资产导出完成:")
    print(f"  静态网格: {len(results['static_meshes'])} 个")
    print(f"  动画网格: {len(results['animated_meshes'])} 个")
    print(f"  材质信息: {len(results['materials'])} 个")
    
    return results
```


---
## bpy_ops_export_mesh

# bpy.ops.export_mesh - 网格导出操作模块

Blender网格导出操作符，用于将网格数据导出为各种3D文件格式。

## 概述

`bpy.ops.export_mesh` 模块提供用于将Blender中的网格数据导出为外部文件格式的功能，包括OBJ、STL、PLY等。

## 核心功能

### OBJ导出

```python
import bpy

# 导出为OBJ
bpy.ops.export_mesh.obj(
    filepath='//model.obj',
    export_selected=True,
    export_uv=True,
    export_normals=True,
    export_materials=True,
    export_triangulated=False,
    export_apply_modifiers=True,
    export_group_by_material=False,
    export_group_by_object=False,
    export_nurbs=False,
    export_curves_as_nurbs=False,
    export_alpha_mode='STRAIGHT',
    export_image_format='NONE',
    export_axis_forward='-Z',
    export_axis_up='Y',
    export_metalness=False,
    export_roughness=False
)

# 导出选中网格
bpy.ops.export_mesh.obj(
    filepath='//selected.obj',
    export_selected=True
)

# 导出所有网格
bpy.ops.export_mesh.obj(
    filepath='//all.obj',
    export_selected=False
)

# 导出带UV的网格
bpy.ops.export_mesh.obj(
    filepath='//with_uv.obj',
    export_uv=True
)

# 导出带法线的网格
bpy.ops.export_mesh.obj(
    filepath='//with_normals.obj',
    export_normals=True
)

# 导出带材质的网格
bpy.ops.export_mesh.obj(
    filepath='//with_materials.obj',
    export_materials=True
)

# 应用修改器后导出
bpy.ops.export_mesh.obj(
    filepath='//applied.obj',
    export_apply_modifiers=True
)

# 三角化导出
bpy.ops.export_mesh.obj(
    filepath='//triangulated.obj',
    export_triangulated=True
)
```

### STL导出

```python
# 导出为STL
bpy.ops.export_mesh.stl(
    filepath='//model.stl',
    export_selected=True,
    export_apply_modifiers=True,
    export_backface_culling=False,
    export_uv=False,
    export_normals=True,
    export_colors=False,
    export_triangulated_mesh=False
)

# 导出选中对象为STL
bpy.ops.export_mesh.stl(
    filepath='//selected.stl',
    export_selected=True
)

# 应用所有修改器
bpy.ops.export_mesh.stl(
    filepath='//applied.stl',
    export_apply_modifiers=True
)

# 三角化网格
bpy.ops.export_mesh.stl(
    filepath='//triangulated.stl',
    export_triangulated_mesh=True
)

# 导出为二进制STL
bpy.ops.export_mesh.stl(
    filepath='//binary.stl',
    export_ascii=False
)

# 导出为ASCII STL
bpy.ops.export_mesh.stl(
    filepath='//ascii.stl',
    export_ascii=True
)
```

### PLY导出

```python
# 导出为PLY
bpy.ops.export_mesh.ply(
    filepath='//model.ply',
    export_selected=True,
    export_uv=True,
    export_normals=True,
    export_colors=True,
    export_triangulated=False,
    export_apply_modifiers=True,
    export_colors_type='SRGB'
)

# 导出带颜色的PLY
bpy.ops.export_mesh.ply(
    filepath='//colored.ply',
    export_colors=True
)

# 导出带UV的PLY
bpy.ops.export_mesh.ply(
    filepath='//with_uv.ply',
    export_uv=True
)

# 导出顶点颜色
bpy.ops.export_mesh.ply(
    filepath='//vertex_colors.ply',
    export_colors=True,
    export_colors_type='LINEAR'
)
```

### 通用网格导出选项

```python
# 设置导出比例
bpy.ops.export_mesh.obj(
    filepath='//scaled.obj',
    export_scale=1.0
)

# 设置轴向
bpy.ops.export_mesh.obj(
    filepath='//y_up.obj',
    export_axis_forward='-Z',
    export_axis_up='Y'
)

# 选择导出格式
bpy.ops.export_mesh.stl(
    filepath='//model.stl',
    export_ascii=False
)

bpy.ops.export_mesh.ply(
    filepath='//model.ply',
    export_format='ASCII'
)

bpy.ops.export_mesh.ply(
    filepath='//model_binary.ply',
    export_format='BINARY'
)
```

### 批量导出

```python
# 批量导出为STL
def batch_export_stl(objects, output_dir):
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    for obj in objects:
        if obj.type == 'MESH':
            bpy.ops.object.select_all(action='DESELECT')
            obj.select_set(True)
            bpy.context.view_layer.objects.active = obj
            filepath = os.path.join(output_dir, f'{obj.name}.stl')
            bpy.ops.export_mesh.stl(filepath=filepath, export_selected=True)

# 导出所有变体
def export_all_variants(base_name):
    obj = bpy.context.active_object
    
    # 原始
    bpy.ops.export_mesh.obj(filepath=f'{base_name}_original.obj')
    
    # 三角化
    for modifier in obj.modifiers:
        if modifier.type == 'TRIANGULATE':
            modifier.show_viewport = True
    bpy.ops.export_mesh.obj(filepath=f'{base_name}_triangulated.obj', export_triangulated=True)

# 按材质导出
def export_by_material(output_dir):
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    objects_by_material = {}
    for obj in bpy.context.selected_objects:
        if obj.type == 'MESH' and obj.data.materials:
            mat = obj.data.materials[0]
            if mat.name not in objects_by_material:
                objects_by_material[mat.name] = []
            objects_by_material[mat.name].append(obj)
    
    for mat_name, objects in objects_by_material.items():
        for obj in objects:
            obj.select_set(True)
        bpy.ops.export_mesh.obj(
            filepath=os.path.join(output_dir, f'{mat_name}.obj'),
            export_group_by_material=True
        )
        for obj in objects:
            obj.select_set(False)
```

## 实用函数

```python
def export_selected_as_obj(filepath, apply_modifiers=True):
    """导出选中的网格为OBJ"""
    bpy.ops.export_mesh.obj(
        filepath=filepath,
        export_selected=True,
        export_apply_modifiers=apply_modifiers,
        export_uv=True,
        export_normals=True,
        export_materials=True
    )

def export_selected_as_stl(filepath, apply_modifiers=True):
    """导出选中的网格为STL"""
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        export_selected=True,
        export_apply_modifiers=apply_modifiers
    )

def export_selected_as_ply(filepath, apply_modifiers=True):
    """导出选中的网格为PLY"""
    bpy.ops.export_mesh.ply(
        filepath=filepath,
        export_selected=True,
        export_apply_modifiers=apply_modifiers,
        export_uv=True,
        export_normals=True,
        export_colors=True
    )

def export_mesh_with_options(filepath, format='obj', **options):
    """使用自定义选项导出网格"""
    if format == 'obj':
        bpy.ops.export_mesh.obj(filepath=filepath, **options)
    elif format == 'stl':
        bpy.ops.export_mesh.stl(filepath=filepath, **options)
    elif format == 'ply':
        bpy.ops.export_mesh.ply(filepath=filepath, **options)
    else:
        raise ValueError(f'不支持的格式: {format}')

def export_all_meshes(filepath, format='obj'):
    """导出所有网格"""
    bpy.ops.object.select_all(action='DESELECT')
    for obj in bpy.data.objects:
        if obj.type == 'MESH':
            obj.select_set(True)
    bpy.ops.export_mesh.obj(filepath=filepath, export_selected=False)

def export_mesh_stripped(filepath):
    """导出剥离所有数据的网格"""
    bpy.ops.export_mesh.obj(
        filepath=filepath,
        export_selected=True,
        export_apply_modifiers=True,
        export_uv=False,
        export_normals=False,
        export_materials=False
    )

def export_mesh_for_print(filepath):
    """导出为3D打印格式"""
    obj = bpy.context.active_object
    
    # 确保应用所有修改器
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 三角化
    if 'Triangulate' not in obj.modifiers:
        bpy.ops.object.modifier_add(type='TRIANGULATE')
    
    for modifier in obj.modifiers:
        modifier.show_viewport = True
        modifier.show_render = True
    
    # 导出
    bpy.ops.export_mesh.stl(
        filepath=filepath,
        export_selected=True,
        export_apply_modifiers=True,
        export_triangulated_mesh=True
    )
```

## 常见问题排查

### 导出为空文件
```python
# 检查选中的网格
meshes = [obj for obj in bpy.context.selected_objects if obj.type == 'MESH']
print(f"选中网格数: {len(meshes)}")

# 检查所有网格
all_meshes = [obj for obj in bpy.data.objects if obj.type == 'MESH']
print(f"总网格数: {len(all_meshes)}")

# 检查网格数据
for mesh in all_meshes:
    print(f"顶点数: {len(mesh.data.vertices)}")
    print(f"面数: {len(mesh.data.polygons)}")

# 确保网格未被隐藏
for mesh in all_meshes:
    mesh.hide_viewport = False
    mesh.hide_render = False
```

### 法线问题
```python
# 检查法线
mesh = bpy.context.active_object.data
print(f"面法线: {mesh.has_custom_normals}")

# 重新计算法线
bpy.ops.object.mode_set(mode='EDIT')
bpy.ops.mesh.select_all(action='SELECT')
bpy.ops.mesh.normals_make_consistent(inside=False)
bpy.ops.object.mode_set(mode='OBJECT')

# 导出时包含法线
bpy.ops.export_mesh.obj(
    filepath='//with_normals.obj',
    export_normals=True
)

# 切线空间
print(f"切线: {mesh.uv_layers.active.tangent_dimension if mesh.uv_layers.active else 'N/A'}")
```

### 材质丢失
```python
# 检查材质
obj = bpy.context.active_object
if obj.data.materials:
    print(f"材质数: {len(obj.data.materials)}")
    for mat in obj.data.materials:
        print(f"材质: {mat.name}")
else:
    print("没有材质")

# 导出时包含材质
bpy.ops.export_mesh.obj(
    filepath='//with_materials.obj',
    export_materials=True
)

# 检查材质路径
for mat in obj.data.materials:
    if mat.use_nodes:
        print("使用节点")
    else:
        print(f"基础颜色: {mat.diffuse_color}")
```

### 修改器未应用
```python
# 检查修改器
obj = bpy.context.active_object
print(f"修改器数: {len(obj.modifiers)}")

for modifier in obj.modifiers:
    print(f"修改器: {modifier.type}, 显示: {modifier.show_viewport}, 渲染: {modifier.show_render}")

# 应用所有修改器
bpy.ops.object.select_all(action='DESELECT')
obj.select_set(True)
bpy.context.view_layer.objects.active = obj
bpy.ops.object.convert(target='MESH')
bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)

# 导出时应用修改器
bpy.ops.export_mesh.obj(
    filepath='//applied.obj',
    export_apply_modifiers=True
)
```


---
## bpy_ops_import_images

# bpy.ops.import_images - 图像导入操作模块

Blender图像导入操作符，用于将图像和图像序列导入到Blender中。

## 概述

`bpy.ops.import_images` 模块提供用于将图像文件导入Blender的功能，支持单个图像、图像序列和图像平面创建。

## 核心功能

### 基础图像导入

```python
import bpy

# 导入单个图像
bpy.ops.import_images.as_planes(
    filepath='//texture.png',
    files=[{'name': 'texture.png'}],
    directory='//',
    filter_blender=False,
    filter_image=True,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    align='CURSOR'
)

# 导入图像序列
bpy.ops.import_images.as_planes(
    filepath='//frame_',
    files=[{'name': 'frame_001.png'}, {'name': 'frame_002.png'}],
    directory='//',
    filter_blender=False,
    filter_image=True,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    align='CURSOR',
    sequence=True
)
```

### 图像平面设置

```python
# 创建图像平面
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    files=[{'name': 'image.png'}],
    directory='//',
    filter_blender=False,
    filter_image=True,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    align='WORLD',
    scale=1.0,
    height=1.0
)

# 设置平面大小
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    height=2.0
)

# 设置平面缩放
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    scale=0.5
)
```

### 对齐方式

```python
# 光标对齐
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    align='CURSOR'
)

# 世界中心对齐
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    align='WORLD'
)

# 不对齐
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    align='NONE'
)
```

### 序列帧导入

```python
# 启用序列
bpy.ops.import_images.as_planes(
    filepath='//frame_',
    files=[{'name': 'frame_001.png'}],
    directory='//',
    sequence=True
)

# 禁用序列
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    sequence=False
)

# 批量导入序列
bpy.ops.import_images.as_planes(
    filepath='//render_',
    files=[{'name': f'render_{i:04d}.png' for i in range(1, 101)}],
    directory='//render/',
    sequence=True
)
```

### 图像序列播放器

```python
# 打开图像序列
bpy.ops.sequencer.images(
    filepath='//frame_',
    files=[{'name': 'frame_001.png'}],
    directory='//',
    filter_blender=False,
    filter_image=True,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False
)

# 导入到VSE
bpy.ops.sequencer.images(
    filepath='//animation_',
    files=[{'name': f'frame_{i:04d}.png' for i in range(1, 251)}],
    directory='//',
    filter_blender=False,
    filter_image=True,
    use_placeholders=True
)
```

## 实用函数

```python
def import_image_plane(image_path, height=1.0, scale=1.0):
    """导入图像为平面"""
    bpy.ops.import_images.as_planes(
        filepath=image_path,
        files=[{'name': bpy.path.basename(image_path)}],
        directory=bpy.path.dirname(image_path),
        align='WORLD',
        height=height,
        scale=scale
    )
    return bpy.context.active_object

def import_image_sequence(base_path, start_frame, end_frame, height=1.0):
    """导入图像序列"""
    files = [{'name': f'{base_path}{i:04d}.png'} for i in range(start_frame, end_frame + 1)]
    directory = bpy.path.dirname(base_path)
    
    bpy.ops.import_images.as_planes(
        filepath=base_path,
        files=files,
        directory=directory,
        align='WORLD',
        height=height,
        sequence=True
    )
    return list(bpy.context.selected_objects)

def import_all_images_from_directory(directory, height=1.0):
    """导入目录下所有图像"""
    import os
    
    image_files = [f for f in os.listdir(directory) if f.lower().endswith(('.png', '.jpg', '.jpeg', '.tif', '.tiff', '.exr'))]
    image_files.sort()
    
    if image_files:
        bpy.ops.import_images.as_planes(
            filepath=os.path.join(directory, image_files[0]),
            files=[{'name': f} for f in image_files],
            directory=directory,
            align='WORLD',
            height=height
        )
    
    return list(bpy.context.selected_objects)

def create_background_plate(image_path, location=(0, 0, 0)):
    """创建背景板"""
    plane = import_image_plane(image_path, height=10.0)
    plane.location = location
    plane.rotation_euler = (0, 0, 0)
    return plane

def create_reference_image(image_path, scale_factor=1.0):
    """创建参考图像"""
    bpy.ops.import_images.as_planes(
        filepath=image_path,
        files=[{'name': bpy.path.basename(image_path)}],
        directory=bpy.path.dirname(image_path),
        align='WORLD',
        scale=scale_factor
    )
    ref = bpy.context.active_object
    ref.name = 'Reference'
    ref.display_type = 'WIRE'
    return ref

def batch_import_planes(image_paths, spacing=1.5):
    """批量导入为平面"""
    planes = []
    for i, path in enumerate(image_paths):
        plane = import_image_plane(path, height=1.0)
        plane.location.x = i * spacing
        planes.append(plane)
    return planes

def import_skybox_images(base_path, ext='.png'):
    """导入天空盒图像"""
    faces = ['px', 'nx', 'py', 'ny', 'pz', 'nz']
    planes = []
    
    for face in faces:
        image_path = f'{base_path}_{face}{ext}'
        if bpy.path.exists(image_path):
            plane = import_image_plane(image_path, height=10.0)
            planes.append(plane)
    
    return planes
```

## 常见问题排查

### 图像不显示
```python
# 检查文件路径
filepath = '//texture.png'
print(f"文件存在: {bpy.path.exists(filepath)}")

# 检查目录
directory = '//'
print(f"目录存在: {bpy.path.exists(directory)}")

# 检查图像数据
print(f"图像数: {len(bpy.data.images)}")

# 重新加载图像
img = bpy.data.images.get('texture.png')
if img:
    img.reload()
```

### 平面大小不对
```python
# 检查导入的平面
plane = bpy.context.active_object
print(f"平面大小: {plane.dimensions}")

# 设置正确大小
bpy.ops.transform.resize(value=(1, 1, 1))
plane.scale = (1, 1, 1)

# 重新导入
bpy.ops.import_images.as_planes(
    filepath='//image.png',
    height=1.0,
    scale=1.0
)
```

### 图像序列不对齐
```python
# 检查序列设置
print(f"帧范围: {bpy.context.scene.frame_start} - {bpy.context.scene.frame_end}")

# 设置正确的帧范围
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 100

# 检查帧速率
print(f"帧速率: {bpy.context.scene.render.fps}")

# 重新排列序列
for strip in bpy.context.scene.sequence_editor.sequences:
    if strip.type == 'IMAGE':
        print(f"条带: {strip.name}, 帧偏移: {strip.frame_offset}")
```

### 颜色空间问题
```python
# 检查颜色空间
img = bpy.data.images.get('image.png')
if img:
    print(f"颜色空间: {img.colorspace_settings.name}")

# 设置颜色空间
img.colorspace_settings.name = 'sRGB'
img.colorspace_settings.name = 'Linear'

# 对于线性工作流
img.colorspace_settings.name = 'Linear'
```


---
## bpy_ops_import_hair

# bpy.ops.object - 粒子毛发导入操作模块

Blender粒子毛发导入操作符，用于导入和转换毛发系统。

## 概述

`bpy.ops.object` 模块中与毛发相关的操作符提供用于处理和导入毛发系统的功能，支持从各种格式导入粒子毛发并进行转换。

## 核心功能

### 转换为毛发

```python
import bpy

# 将曲线转换为毛发
bpy.ops.object.convert(
    target='HAIR'
)

# 将曲线转换为粒子
bpy.ops.object.convert(
    target='PARTICLE'
)

# 将网格转换为毛发
bpy.ops.object.convert(
    target='HAIR',
    source='MESH'
)
```

### 粒子系统设置

```python
# 添加粒子系统
bpy.ops.object.particle_system_add()

# 移除粒子系统
bpy.ops.object.particle_system_remove()

# 复制粒子系统
bpy.ops.object.particle_system_copy()
```

### 毛发编辑

```python
# 切换到毛发编辑模式
bpy.ops.object.mode_set(
    mode='PARTICLE_EDIT'
)

# 切换到对象模式
bpy.ops.object.mode_set(
    mode='OBJECT'
)
```

### 毛发系统属性

```python
# 获取活动粒子系统
particle_system = bpy.context.active_object.particle_systems.active

# 设置毛发长度
particle_system.settings.length = 1.0

# 设置毛发数量
particle_system.settings.count = 1000

# 设置毛发粗细
particle_system.settings.hair_thickness = 0.01

# 设置毛发分段
particle_system.settings.hair_step = 3
```

### 粒子发射设置

```python
# 设置发射类型
particle_system.settings.emitter_type = 'HAIR'

# 设置发射源
particle_system.settings.emitter = bpy.context.active_object

# 设置发射模式
particle_system.settings.use_emit_random = False
particle_system.settings.use_even_distribution = True

# 设置发射密度
particle_system.settings.distribution = 'RANDOM'
```

### 物理设置

```python
# 重力影响
particle_system.settings.gravity = 1.0

# 速度设置
particle_system.settings.normal_factor = 1.0

# 随机设置
particle_system.settings.random_factor = 0.2

# 生命周期
particle_system.settings.lifetime = 50
particle_system.settings.lifetime_random = 10
```

### 碰撞设置

```python
# 启用碰撞
particle_system.settings.collision_type = 'NONE'

# 设置碰撞对象
particle_system.settings.collider = None
```

### 渲染设置

```python
# 渲染类型
particle_system.settings.render_type = 'PATH'

# 路径渲染设置
particle_system.settings.render_step = 3

# 材质设置
particle_system.settings.material = 0

# 颜色设置
particle_system.settings.color = (1.0, 1.0, 1.0, 1.0)
```

### 引导设置

```python
# 引导设置
particle_system.settings.guides = 0

# 引导权重
particle_system.settings.guide_weight = 0.5

# 引导收集
particle_system.settings.guide_collection = None
```

### 子粒子设置

```python
# 子粒子设置
particle_system.settings.child_type = 'NONE'

# 子粒子数量
particle_system.settings.child_nbr = 0

# 子粒子半径
particle_system.settings.child_radius = 0.2

# 子粒子长度
particle_system.settings.child_length = 0.5
```

## 实用函数

```python
def create_hair_from_curves(curves_object, target_object, count=1000, length=1.0):
    """从曲线创建毛发"""
    # 选择曲线
    bpy.ops.object.select_all(action='DESELECT')
    curves_object.select_set(True)
    bpy.context.view_layer.objects.active = curves_object
    
    # 转换为毛发
    bpy.ops.object.convert(target='HAIR')
    
    # 设置发射对象
    hair = bpy.context.active_object
    hair.particle_systems.active.settings.emitter = target_object
    
    # 设置毛发参数
    settings = hair.particle_systems.active.settings
    settings.count = count
    settings.length = length
    
    return hair

def create_particle_hair(obj, count=1000, length=1.0, material_index=0):
    """创建粒子毛发"""
    # 选择对象
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 添加粒子系统
    bpy.ops.object.particle_system_add()
    
    # 获取粒子系统
    ps = obj.particle_systems.active
    
    # 设置参数
    ps.settings.count = count
    ps.settings.length = length
    ps.settings.render_type = 'PATH'
    ps.settings.hair_step = 3
    ps.settings.material = material_index
    
    # 设置为毛发类型
    ps.settings.type = 'HAIR'
    
    return ps

def convert_mesh_to_hair(obj, count=500, length=0.5):
    """将网格转换为毛发"""
    # 选择对象
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 添加粒子系统
    bpy.ops.object.particle_system_add()
    
    # 设置参数
    ps = obj.particle_systems.active
    ps.settings.count = count
    ps.settings.length = length
    ps.settings.type = 'HAIR'
    ps.settings.use_even_distribution = True
    ps.settings.distribution = 'RANDOM'
    
    return ps

def copy_hair_system(source_obj, target_obj):
    """复制毛发系统"""
    # 选择源对象
    bpy.ops.object.select_all(action='DESELECT')
    source_obj.select_set(True)
    bpy.context.view_layer.objects.active = source_obj
    
    # 复制粒子系统
    bpy.ops.object.particle_system_copy()
    
    # 获取新的粒子系统
    new_ps = source_obj.particle_systems[-1]
    
    # 设置发射对象
    new_ps.settings.emitter = target_obj
    
    return new_ps

def add_child_hair(obj, child_count=100, child_radius=0.2):
    """添加子毛发"""
    ps = obj.particle_systems.active
    if ps:
        ps.settings.child_type = 'SIMPLE'
        ps.settings.child_nbr = child_count
        ps.settings.child_radius = child_radius
        ps.settings.child_length = 0.5
        
        return ps
    
    return None

def set_hair_material(obj, material):
    """设置毛发材质"""
    ps = obj.particle_systems.active
    if ps:
        if isinstance(material, bpy.types.Material):
            ps.settings.material = obj.data.materials.find(material.name)
        else:
            ps.settings.material = material
        
        return True
    
    return False

def adjust_hair_density(obj, density_factor):
    """调整毛发密度"""
    ps = obj.particle_systems.active
    if ps:
        original_count = ps.settings.count
        ps.settings.count = int(original_count * density_factor)
        print(f"毛发数量: {original_count} -> {ps.settings.count}")
        return ps.settings.count
    
    return None

def adjust_hair_length(obj, length_factor):
    """调整毛发长度"""
    ps = obj.particle_systems.active
    if ps:
        original_length = ps.settings.length
        ps.settings.length = original_length * length_factor
        print(f"毛发长度: {original_length:.4f} -> {ps.settings.length:.4f}")
        return ps.settings.length
    
    return None

def set_hair_color(obj, color):
    """设置毛发颜色"""
    ps = obj.particle_systems.active
    if ps:
        ps.settings.color = color
        return True
    
    return False

def add_hair_collision(collision_obj, hair_obj):
    """添加毛发碰撞"""
    ps = hair_obj.particle_systems.active
    if ps:
        ps.settings.collision_type = 'HAIR'
        ps.settings.collider = collision_obj
        
        # 确保碰撞对象是有效的
        if collision_obj:
            print(f"碰撞对象: {collision_obj.name}")
        
        return ps
    
    return None

def create_wet_hair(obj, wetness=0.5):
    """创建湿润毛发效果"""
    ps = obj.particle_systems.active
    if ps:
        # 减少重力影响
        ps.settings.gravity = 1.0 - wetness
        
        # 增加粗细
        ps.settings.hair_thickness *= (1 + wetness)
        
        # 减少子粒子数量
        if ps.settings.child_type != 'NONE':
            ps.settings.child_nbr = int(ps.settings.child_nbr * (1 - wetness * 0.5))
        
        return ps
    
    return None
```

## 常见问题排查

### 毛发不显示
```python
# 检查粒子系统
obj = bpy.context.active_object
print(f"粒子系统数: {len(obj.particle_systems)}")

if obj.particle_systems:
    ps = obj.particle_systems.active
    print(f"活跃粒子系统: {ps.name}")
    print(f"毛发数量: {ps.settings.count}")
    print(f"毛发长度: {ps.settings.length}")
    
    # 检查粒子类型
    print(f"粒子类型: {ps.settings.type}")

# 检查渲染类型
if ps.settings.type == 'HAIR':
    print(f"渲染类型: {ps.settings.render_type}")

# 确保粒子系统可见
ps.settings.render_step = 3
ps.settings.use_hair_dynamics = False
```

### 毛发穿透问题
```python
# 检查碰撞设置
ps = bpy.context.active_object.particle_systems.active
print(f"碰撞类型: {ps.settings.collision_type}")
print(f"碰撞对象: {ps.settings.collider}")

# 添加碰撞
for obj in bpy.context.scene.objects:
    if obj.type == 'MESH':
        ps.settings.collider = obj
        ps.settings.collision_type = 'HAIR'
        break

# 增加碰撞质量
ps.settings.mass = 1.0

# 减少重力
ps.settings.gravity = 0.5
```

### 性能问题
```python
# 检查毛发数量
ps = bpy.context.active_object.particle_systems.active
print(f"毛发数量: {ps.settings.count}")

# 减少数量
ps.settings.count = int(ps.settings.count * 0.5)

# 减少分段数
ps.settings.hair_step = 2

# 禁用子粒子
ps.settings.child_type = 'NONE'

# 减少子粒子数量
ps.settings.child_nbr = int(ps.settings.child_nbr * 0.5)

# 检查渲染设置
ps.settings.render_step = 5
```

### 毛发造型问题
```python
# 检查毛发编辑模式
print(f"当前模式: {bpy.context.mode}")

# 切换到毛发编辑模式
bpy.ops.object.mode_set(mode='PARTICLE_EDIT')

# 检查选中的毛发
if bpy.context.mode == 'PARTICLE_EDIT':
    ps = bpy.context.active_object.particle_systems.active
    print(f"选中的毛发: {len(bpy.context.selected_editable_ particles)}")

# 重置毛发
bpy.ops.particle.reset()
```

### 材质问题
```python
# 检查材质
obj = bpy.context.active_object
ps = obj.particle_systems.active

print(f"材质槽数: {len(obj.data.materials)}")
print(f"分配的材质: {ps.settings.material}")

# 确保材质存在
if ps.settings.material >= len(obj.data.materials):
    ps.settings.material = 0

# 检查材质类型
if obj.data.materials:
    mat = obj.data.materials[ps.settings.material]
    print(f"材质: {mat.name}")
    print(f"使用节点: {mat.use_nodes}")
```

### 导出问题
```python
# 检查导出设置
print(f"导出粒子: {bpy.context.scene.particle_export_}

# 启用粒子导出
bpy.context.scene.particle_export = True

# 检查导出格式
print(f"导出格式: {bpy.context.scene.particle_export_format}")

# 转换为网格后导出
bpy.ops.object.convert(target='MESH')
bpy.ops.export_scene.obj(filepath='//hair_model.obj')
```


