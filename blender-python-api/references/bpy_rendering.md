# bpy_rendering

---
## bpy_rendering_assets

# Blender 渲染、资产与系统配置指南

本模块涵盖 Blender 渲染系统、命令行操作、资产库管理、自定义属性和数据块系统的完整技能，帮助开发者创建专业的渲染工作流和资源管理系统。

## 概述

Blender 提供了强大的渲染引擎（EEVEE、Cycles、Workbench）、灵活的命令行接口、完善的数据块系统和资产管理系统。理解这些系统对于开发专业级插件和自动化工作流程至关重要。

**核心概念：**
- 多引擎渲染配置和优化
- 命令行批处理和自动化
- 资产库组织和目录管理
- 自定义属性的类型和使用
- 数据块的生命周期和链接

## 核心功能

### 渲染系统

Blender 支持多种渲染引擎，每种引擎都有不同的特点和适用场景。

**渲染设置：**

```python
import bpy

# 获取当前渲染设置
scene = bpy.context.scene
render = scene.render

# 基本设置
render.engine = 'CYCLES'  # 渲染引擎: CYCLES, EEVEE, BLENDER_WORKBENCH

# 分辨率设置
render.resolution_x = 1920
render.resolution_y = 1080
render.resolution_percentage = 100

# 帧范围设置
scene.frame_start = 1
scene.frame_end = 100
scene.frame_step = 1

# 输出设置
render.filepath = "//render/output_"
render.use_file_extension = True
render.image_settings.file_format = 'PNG'
render.image_settings.color_mode = 'RGBA'
render.image_settings.compression = 90

# 启用抗锯齿
render.antialiasing_samples = '16'
```

**Cycles 渲染设置：**

```python
cycles = scene.cycles

# 设备设置
cycles.device = 'GPU'  # CPU, GPU
cycles.samples = 128  # 采样数
cycles.adaptive_threshold = 0.01  # 自适应阈值

# GPU 渲染设备
import sys
if sys.platform.startswith('win'):
    cycles.device = 'CUDA'  # NVIDIA GPU
elif sys.platform == 'darwin':
    cycles.device = 'METAL'  # Apple Silicon
elif 'linux' in sys.platform:
    cycles.device = 'HIP'  # AMD GPU

# 分布式渲染

Blender 支持通过 Cycles Sample Subset 功能实现单帧分布式渲染，以及通过命令行实现远程渲染。

### Cycles Sample Subset 分布式渲染

使用 Sample Subset 功能可以将单帧渲染任务分配到多台机器，然后合并结果。

**基本原理：**
- 将总采样数分成多个子集
- 每台机器渲染一个子集
- 使用 merge_images 合并结果

**设置示例：**

```python
import bpy

scene = bpy.context.scene
cycles = scene.cycles

# 设置总采样数（所有机器必须一致）
cycles.samples = 2048

# 禁用降噪（合并前必须禁用）
cycles.use_denoising = False

# 启用 Sample Subset
cycles.use_sample_subset = True

# 第一台机器：渲染前 1024 个采样
cycles.sample_offset = 0
cycles.sample_subset_length = 1024

# 第二台机器：渲染后 1024 个采样
# cycles.sample_offset = 1024
# cycles.sample_subset_length = 1024
```

**合并渲染结果：**

```python
# 在 Python 控制台或脚本中运行
bpy.ops.cycles.merge_images(
    input_filepath1=r"machine1_output.exr",
    input_filepath2=r"machine2_output.exr",
    output_filepath=r"combined_output.exr"
)
```

**分布式渲染工作流程：**

```python
def setup_distributed_render(machine_id, total_machines, total_samples):
    """设置分布式渲染参数"""
    scene = bpy.context.scene
    cycles = scene.cycles
    
    # 设置总采样数
    cycles.samples = total_samples
    
    # 禁用降噪
    cycles.use_denoising = False
    
    # 启用 Sample Subset
    cycles.use_sample_subset = True
    
    # 计算每台机器的采样数
    samples_per_machine = total_samples // total_machines
    
    # 设置当前机器的偏移和长度
    cycles.sample_offset = machine_id * samples_per_machine
    cycles.sample_subset_length = samples_per_machine
    
    # 设置输出文件名（包含机器ID）
    scene.render.filepath = f"//render/frame_{machine_id:04d}"
    
    return cycles.sample_offset, cycles.sample_subset_length

# 使用示例
# 机器 0
setup_distributed_render(0, 2, 2048)

# 机器 1
# setup_distributed_render(1, 2, 2048)
```

**合并多个渲染结果：**

```python
def merge_multiple_renderings(input_files, output_file):
    """合并多个渲染文件"""
    if len(input_files) < 2:
        print("至少需要两个输入文件")
        return
    
    # 先合并前两个
    bpy.ops.cycles.merge_images(
        input_filepath1=input_files[0],
        input_filepath2=input_files[1],
        output_filepath=output_file
    )
    
    # 依次合并剩余文件
    for i in range(2, len(input_files)):
        temp_output = f"temp_merge_{i}.exr"
        bpy.ops.cycles.merge_images(
            input_filepath1=output_file,
            input_filepath2=input_files[i],
            output_filepath=temp_output
        )
        # 替换输出文件
        import os
        os.replace(temp_output, output_file)

# 使用示例
input_files = [
    "render_machine0.exr",
    "render_machine1.exr",
    "render_machine2.exr"
]
merge_multiple_renderings(input_files, "final_render.exr")
```

### 命令行远程渲染

Blender 支持通过命令行进行无界面渲染，适合通过 SSH 等远程 shell 进行渲染。

**基本远程渲染命令：**

```bash
# 后台渲染单帧
blender -b file.blend -f 10

# 后台渲染动画
blender -b file.blend -a

# 使用 Cycles 渲染，指定帧范围和线程数
blender -b file.blend -E CYCLES -s 10 -e 500 -t 2 -a

# 指定输出路径和格式
blender -b file.blend -o /project/renders/frame_##### -F OPEN_EXR -a
```

**远程渲染脚本：**

```python
# remote_render.py
import bpy
import sys

def render_remote(blend_file, output_path, frame=None, animation=False):
    """远程渲染函数"""
    # 打开文件
    bpy.ops.wm.open_mainfile(filepath=blend_file)
    
    # 设置输出路径
    bpy.context.scene.render.filepath = output_path
    
    # 渲染
    if animation:
        bpy.ops.render.render(animation=True, write_still=False)
    elif frame is not None:
        bpy.context.scene.frame_set(frame)
        bpy.ops.render.render(write_still=True)
    else:
        bpy.ops.render.render(write_still=True)

if __name__ == "__main__":
    # 从命令行参数获取
    blend_file = sys.argv[4] if len(sys.argv) > 4 else ""
    output_path = sys.argv[5] if len(sys.argv) > 5 else "//render/"
    
    render_remote(blend_file, output_path, animation=True)
```

**批处理渲染任务：**

```python
# batch_remote_render.py
import subprocess
import os

def render_on_remote_machine(host, blend_file, output_dir, frame_range):
    """在远程机器上渲染"""
    # 使用 SSH 执行远程渲染
    cmd = f"""
    ssh {host} 'blender -b {blend_file} -o {output_dir}/frame_##### -s {frame_range[0]} -e {frame_range[1]} -a'
    """
    
    result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
    
    if result.returncode == 0:
        print(f"远程渲染成功: {host}")
    else:
        print(f"远程渲染失败: {host}")
        print(result.stderr)

# 使用示例
render_on_remote_machine(
    host="render-farm-01",
    blend_file="/path/to/scene.blend",
    output_dir="/output/renders",
    frame_range=(1, 250)
)
```

### 多机器帧分配渲染

将动画的不同帧分配到不同机器渲染。

**帧分配策略：**

```python
def distribute_frames(total_frames, num_machines):
    """分配帧到不同机器"""
    frames_per_machine = total_frames // num_machines
    distribution = []
    
    for i in range(num_machines):
        start = i * frames_per_machine + 1
        end = (i + 1) * frames_per_machine
        distribution.append((start, end))
    
    # 处理余数帧
    remainder = total_frames % num_machines
    for i in range(remainder):
        distribution[i] = (distribution[i][0], distribution[i][1] + 1)
    
    return distribution

# 使用示例
frames = distribute_frames(250, 4)
# 结果: [(1, 63), (64, 126), (127, 190), (191, 250)]
```

**生成渲染命令：**

```python
def generate_render_commands(blend_file, output_dir, frame_distribution):
    """生成每台机器的渲染命令"""
    commands = []
    
    for i, (start, end) in enumerate(frame_distribution):
        cmd = (
            f"blender -b {blend_file} "
            f"-o {output_dir}/machine{i}/frame_##### "
            f"-s {start} -e {end} -a"
        )
        commands.append(cmd)
    
    return commands

# 使用示例
commands = generate_render_commands(
    blend_file="scene.blend",
    output_dir="//render",
    frame_distribution=frames
)

for i, cmd in enumerate(commands):
    print(f"机器 {i}: {cmd}")
```

### 渲染任务队列管理

创建简单的渲染任务队列系统。

**任务队列定义：**

```python
class RenderTask:
    def __init__(self, name, blend_file, output_dir, 
                 frame_start, frame_end, machine_id=None):
        self.name = name
        self.blend_file = blend_file
        self.output_dir = output_dir
        self.frame_start = frame_start
        self.frame_end = frame_end
        self.machine_id = machine_id
        self.status = "pending"  # pending, running, completed, failed

class RenderQueue:
    def __init__(self):
        self.tasks = []
        self.machines = []
    
    def add_task(self, task):
        self.tasks.append(task)
    
    def assign_to_machine(self, task, machine_id):
        task.machine_id = machine_id
        task.status = "running"
    
    def complete_task(self, task):
        task.status = "completed"
    
    def get_pending_tasks(self):
        return [t for t in self.tasks if t.status == "pending"]
```

**任务分发脚本：**

```python
def distribute_render_tasks(queue, num_machines):
    """分发渲染任务到多台机器"""
    pending = queue.get_pending_tasks()
    
    for i, task in enumerate(pending):
        machine_id = i % num_machines
        queue.assign_to_machine(task, machine_id)
        
        # 生成渲染命令
        cmd = (
            f"blender -b {task.blend_file} "
            f"-o {task.output_dir}/machine{machine_id}/{task.name}_frame_##### "
            f"-s {task.frame_start} -e {task.frame_end} -a"
        )
        
        print(f"任务 {task.name} -> 机器 {machine_id}")
        print(f"命令: {cmd}")

# 使用示例
queue = RenderQueue()
queue.add_task(RenderTask("scene1", "scene1.blend", "//render", 1, 100))
queue.add_task(RenderTask("scene2", "scene2.blend", "//render", 1, 100))
queue.add_task(RenderTask("scene3", "scene3.blend", "//render", 1, 100))

distribute_render_tasks(queue, 3)
```

# 降噪
cycles.use_denoising = True
cycles.denoiser = 'NLM'  # NLM, OPENIMAGEDENOISE
```

**EEVEE 渲染设置：**

```python
eevee = scene.eevee

# 采样设置
eevee.taa_render_samples = 64

# 阴影设置
eevee.use_shadows = True
eevee.shadow_cube_size = '2048'
eevee.shadow_cascade_size = '2048'

# 环境光遮蔽
eevee.use_gtao = True
eevee.gtao_distance = 2.0
eevee.gtao_factor = 1.0

# 屏幕空间反射
eevee.use SSR = True
eevee.ssr_halfres = False

# 体积渲染
eevee.use_volumetric_shadows = True
eevee.volumetric_light_sample = 0.1
```

### 命令行操作

Blender 可以通过命令行进行无界面渲染和批处理操作。

**基本命令行参数：**

```bash
# 后台渲染
blender -b file.blend -a

# 渲染特定帧
blender -b file.blend -f 100
blender -b file.blend -f 1..100
blender -b file.blend -f 1,3,5,7

# 指定场景和输出
blender -b file.blend -S SceneName -o //render/ -F PNG -x 1 -a

# 指定渲染引擎
blender -b file.blend -E CYCLES -f 1

# 渲染线程数
blender -b file.blend -t 8 -a

# 输出路径模板
blender -b file.blend -o //render/frame_##### -a
# # 被替换为帧号，支持零填充
```

**Python 脚本执行：**

```bash
# 运行 Python 脚本
blender -b file.blend -P script.py

# 执行表达式
blender -b --python-expr "import bpy; bpy.ops.render.render(write_still=True)"

# 运行文本块
blender -b --python-text "MyScript" -a

# 控制台模式
blender --python-console

# 退出代码
blender --python-exit-code 1 -b file.blend -f 1
```

**渲染批处理脚本：**

```python
# batch_render.py
import bpy
import os
import sys

def batch_render(file_list, output_dir):
    for blend_file in file_list:
        # 打开文件
        bpy.ops.wm.open_mainfile(filepath=blend_file)
        
        # 设置输出路径
        scene = bpy.context.scene
        scene.render.filepath = os.path.join(output_dir, 
                                              os.path.basename(blend_file).replace('.blend', '_'))
        
        # 渲染
        bpy.ops.render.render(write_still=True)
        
        print(f"完成渲染: {blend_file}")

# 命令行调用
if __name__ == "__main__":
    files = sys.argv[4:]  # 跳过 blender, -b, script.py
    output = sys.argv[3] if len(sys.argv) > 3 else "//render/"
    batch_render(files, output)
```

### 资产库管理

Blender 的资产库系统提供了灵活的资产组织和目录管理功能。

**资产库基本操作：**

```python
# 访问资产浏览器空间
space = bpy.context.space_data
space.ui_mode = 'ASSETS'

# 资产库操作
bpy.ops.asset.library_open(filepath="C:/path/to/library")

# 标记资产
obj = bpy.context.object
bpy.ops.object.asset_mark()

# 取消标记
bpy.ops.object.asset_clear()

# 创建预览
bpy.ops.object.asset_generate_preview()
```

**资产目录操作：**

```python
# 资产目录管理
bpy.ops.asset.catalog_new()  # 创建新目录
bpy.ops.asset.catalogs_save()  # 保存目录

# 获取资产库根目录
def get_asset_library_path(library_name):
    for lib in bpy.data.libraries:
        if lib.name == library_name:
            return lib.filepath
    return None

# 资产过滤
def filter_assets_by_catalog(library_name, catalog_path):
    library = bpy.data.libraries[library_name]
    # 过滤逻辑
    for asset in bpy.data.objects:
        if asset.asset_data is None:
            continue
        # 检查资产是否在指定目录
        catalog_id = asset.asset_data.catalog_id
```

**目录定义文件：**

```python
# blender_assets.cats.txt 格式
# 目录路径由 / 分隔
# 例: Characters/Ellie/Poses

def create_catalog_file(library_path, catalogs):
    """创建资产目录定义文件"""
    file_path = os.path.join(library_path, "blender_assets.cats.txt")
    
    with open(file_path, 'w', encoding='utf-8') as f:
        f.write("# Asset Catalogs\n")
        f.write("# Generated by Blender\n\n")
        
        for path, name, uuid in catalogs:
            if name:
                f.write(f'{path}\n')
                f.write(f'    name = "{name}"\n')
            else:
                f.write(f'{path}\n')
            f.write(f'    id = {uuid}\n\n')
```

### 自定义属性

自定义属性允许在 Blender 数据块中存储用户自定义数据。

**添加自定义属性：**

```python
# 添加简单属性
obj = bpy.context.object
obj["my_int"] = 10
obj["my_float"] = 3.14
obj["my_string"] = "hello"

# 使用 IDProperties 添加
obj.id_properties_ui("my_prop").update(
    min=0.0,
    max=100.0,
    default=50.0
)
```

**属性类型和配置：**

```python
from bpy.props import (
    IntProperty, FloatProperty, StringProperty,
    BoolProperty, EnumProperty, FloatVectorProperty
)

# 整数属性
def update_int_prop(self, context):
    print(f"值已更新: {self.my_int}")

bpy.types.Object.my_int = IntProperty(
    name="整数属性",
    description="自定义整数属性",
    default=0,
    min=-1000,
    max=1000,
    step=1,
    update=update_int_prop
)

# 浮点数属性
bpy.types.Object.my_float = FloatProperty(
    name="浮点属性",
    description="自定义浮点属性",
    default=1.0,
    min=0.0,
    max=10.0,
    step=0.1,
    precision=2,
    subtype='NONE',  # NONE, PIXEL, PERCENTAGE, FACTOR, ANGLE, TIME, DISTANCE, POWER, TEMPERATURE, WAVELENGTH
    unit='LENGTH'  # NONE, LENGTH, AREA, VOLUME, ROTATION, TIME, FREQUENCY, VELOCITY, ACCELERATION
)

# 字符串属性
bpy.types.Object.my_string = StringProperty(
    name="字符串属性",
    description="自定义字符串属性",
    default="默认值",
    maxlen=128,
    subtype='NONE'  # NONE, FILE_PATH, DIR_PATH, FILE_NAME, BYTE_STRING, PASSWORD
)

# 布尔属性
bpy.types.Object.my_bool = BoolProperty(
    name="布尔属性",
    description="自定义布尔属性",
    default=True,
    options={'ANIMATABLE'}
)

# 枚举属性
bpy.types.Object.my_enum = EnumProperty(
    name="枚举属性",
    description="自定义枚举属性",
    items=[
        ('A', "选项 A", "第一个选项"),
        ('B', "选项 B", "第二个选项"),
        ('C', "选项 C", "第三个选项"),
    ],
    default='A'
)

# 向量属性
bpy.types.Object.my_color = FloatVectorProperty(
    name="颜色",
    description="颜色属性",
    size=4,
    min=0.0,
    max=1.0,
    default=(1.0, 1.0, 1.0, 1.0),
    subtype='COLOR',
    options={'ANIMATABLE'}
)

bpy.types.Object.my_location = FloatVectorProperty(
    name="位置",
    description="位置向量",
    size=3,
    subtype='TRANSLATION'
)

bpy.types.Object.my_euler = FloatVectorProperty(
    name="欧拉角",
    description="欧拉角",
    size=3,
    subtype='EULER'
)
```

### 数据块系统

Blender 的核心数据组织单位是数据块（Data-Blocks）。

**数据块类型：**

```python
# Blender 中的主要数据块类型
DATA_BLOCKS = {
    'Action': '动画动作',
    'Armature': '骨骼数据',
    'Brush': '画笔',
    'Camera': '相机',
    'Collection': '集合',
    'Curve': '曲线',
    'Curves': '新曲线类型',
    'Font': '字体',
    'GreasePencil': '涂鸦铅笔',
    'Image': '图像',
    'Key': '形状键',
    'Lattice': '晶格',
    'Library': '库引用',
    'Light': '灯光',
    'LightProbe': '光照探针',
    'LineStyle': '线条样式',
    'Mask': '遮罩',
    'Material': '材质',
    'Mesh': '网格',
    'Metaball': 'Metaball',
    'MovieClip': '视频剪辑',
    'NodeTree': '节点树',
    'Object': '对象',
    'PaintCurve': '绘制曲线',
    'Palette': '调色板',
    'ParticleSettings': '粒子设置',
    'PointCloud': '点云',
    'Scene': '场景',
    'Screen': '屏幕',
    'Sound': '声音',
    'Speaker': '扬声器',
    'Text': '文本',
    'Texture': '纹理',
    'VectorCurve': '矢量曲线',
    'Volume': '体积',
    'WindowManager': '窗口管理器',
    'WorkSpace': '工作区',
    'World': '世界',
}
```

**数据块基本操作：**

```python
# 获取数据块列表
for obj in bpy.data.objects:
    print(f"对象: {obj.name}, 类型: {obj.type}")

# 创建数据块
mesh = bpy.data.meshes.new("MyMesh")
obj = bpy.data.objects.new("MyObject", mesh)

# 删除数据块
bpy.data.meshes.remove(mesh)
bpy.data.objects.remove(obj)

# 复制数据块
obj_copy = obj.copy()
obj_copy.name = obj.name + "_copy"

# 检查数据块是否使用
def is_datablock_used(datablock):
    return datablock.users > 0

# 获取所有未使用的数据块
def get_unused_datablocks():
    unused = []
    for collection in [
        bpy.data.objects,
        bpy.data.meshes,
        bpy.data.materials,
        bpy.data.images,
    ]:
        for item in collection:
            if item.users == 0:
                unused.append(item)
    return unused
```

**库链接：**

```python
# 链接外部库
lib = bpy.data.libraries.load("//path/to/library.blend")

# 获取库中的对象
linked_objects = lib.objects

# 将对象链接到场景
for obj in linked_objects:
    bpy.context.scene.collection.objects.link(obj)

# 创建本地副本（使库引用独立）
obj.make_local()

# 库覆盖
def create_library_override(target_library, target_object):
    """创建库覆盖"""
    override = bpy.data.libraries[target_library].make_override([target_object])
    return override
```

### 导入导出

Blender 支持多种文件格式的导入和导出。

**常用导入操作：**

```python
# 导入 OBJ
bpy.ops.wm.obj_import(
    filepath="//models/model.obj",
    axis_forward='-Z',
    axis_up='Y',
    use_split_objects=True,
    use_split_materials=True
)

# 导入 FBX
bpy.ops.import_scene.fbx(
    filepath="//models/model.fbx",
    axis_forward='-Z',
    axis_up='Y',
    use_anim=True,
    use_custom_normals=True
)

# 导入 GLTF
bpy.ops.import_scene.gltf(
    filepath="//models/model.glb",
    export_format='GLB',
    import_pack_images=True,
    use_materials=True
)

# 导入 PLY
bpy.ops.wm.ply_import(
    filepath="//models/model.ply"
)

# 导入 STL
bpy.ops.wm.stl_import(
    filepath="//models/model.stl"
)

# 导入 USD
bpy.ops.usd.import_(
    filepath="//models/model.usd"
)

# 导入 Alembic
bpy.ops.wm.alembic_import(
    filepath="//models/model.abc",
    is_sequence=False
)
```

**常用导出操作：**

```python
# 导出 OBJ
bpy.ops.wm.obj_export(
    filepath="//export/model.obj",
    export_selected=True,
    axis_forward='-Z',
    axis_up='Y',
    use_materials=True
)

# 导出 FBX
bpy.ops.export_scene.fbx(
    filepath="//export/model.fbx",
    export_selected=True,
    use_selection=True,
    axis_forward='-Z',
    axis_up='Y',
    apply_scale_options='FBX_SCALE_ALL'
)

# 导出 GLTF
bpy.ops.export_scene.gltf(
    filepath="//export/model.glb",
    export_format='GLB',
    export_selected=True,
    use_selection=True,
    export_apply=True
)

# 导出 PLY
bpy.ops.wm.ply_export(
    filepath="//export/model.ply",
    export_selected=True
)

# 导出 STL
bpy.ops.wm.stl_export(
    filepath="//export/model.stl",
    export_selected=True
)

# 导出 USD
bpy.ops.usd.export_(
    filepath="//export/model.usd",
    export_selected=True
)

# 导出 Alembic
bpy.ops.wm.alembic_export(
    filepath="//export/model.abc",
    export_selected=True,
    export_uv=True,
    export_normals=True
)
```

## 实用函数

### 渲染工具函数

```python
def setup_render_preset(scene_name="Default", resolution=(1920, 1080)):
    """设置渲染预设"""
    scene = bpy.data.scenes.get(scene_name)
    if not scene:
        scene = bpy.data.scenes.new(scene_name)
    
    render = scene.render
    render.resolution_x = resolution[0]
    render.resolution_y = resolution[1]
    render.resolution_percentage = 100
    
    return scene

def render_to_sequence(output_dir, file_format='PNG'):
    """渲染序列帧"""
    scene = bpy.context.scene
    render = scene.render
    
    # 保存原始设置
    original_path = render.filepath
    original_format = render.image_settings.file_format
    
    # 设置输出
    render.filepath = output_dir
    render.image_settings.file_format = file_format
    
    # 渲染所有帧
    bpy.ops.render.render(animation=True)
    
    # 恢复设置
    render.filepath = original_path
    render.image_settings.file_format = original_format

def get_render_stats():
    """获取渲染统计信息"""
    scene = bpy.context.scene
    return {
        "engine": scene.render.engine,
        "resolution": (scene.render.resolution_x, scene.render.resolution_y),
        "frame_range": (scene.frame_start, scene.frame_end),
        "total_frames": scene.frame_end - scene.frame_start + 1,
        "samples": getattr(scene.cycles, 'samples', 'N/A'),
    }
```

### 资产管理工具

```python
def mark_selection_as_assets():
    """将选中对象标记为资产"""
    selected = bpy.context.selected_objects
    for obj in selected:
        if obj.asset_data is None:
            bpy.ops.object.asset_mark()
            # 设置默认预览
            bpy.ops.object.asset_generate_preview()

def create_asset_catalog(library_path, catalog_path, name=None):
    """创建资产目录"""
    import uuid
    
    # 生成 UUID
    catalog_uuid = str(uuid.uuid4()).upper()
    
    # 目录定义
    catalog_def = {
        'path': catalog_path,
        'name': name,
        'id': catalog_uuid
    }
    
    # 更新目录文件
    catalog_file = os.path.join(library_path, "blender_assets.cats.txt")
    
    # 读取现有内容
    existing_content = ""
    if os.path.exists(catalog_file):
        with open(catalog_file, 'r') as f:
            existing_content = f.read()
    
    # 添加新目录
    with open(catalog_file, 'a') as f:
        f.write(f"\n{catalog_path}\n")
        if name:
            f.write(f'    name = "{name}"\n')
        f.write(f"    id = {catalog_uuid}\n")
    
    return catalog_uuid

def batch_assign_catalog(assets, catalog_path):
    """批量分配资产到目录"""
    for asset in assets:
        if asset.asset_data:
            # 假设 catalog_path 映射到 UUID
            asset.asset_data.catalog_id = get_catalog_uuid(catalog_path)
```

### 数据块工具

```python
def find_datablock_by_name(datatype, name):
    """按名称查找数据块"""
    collection = getattr(bpy.data, datatype)
    return collection.get(name)

def get_datablock_users(datablock):
    """获取数据块的使用者数量"""
    return datablock.users

def cleanup_unused_datablocks():
    """清理未使用的数据块"""
    collections_to_check = [
        bpy.data.objects,
        bpy.data.meshes,
        bpy.data.materials,
        bpy.data.textures,
        bpy.data.images,
        bpy.data.curves,
        bpy.data.lights,
        bpy.data.cameras,
    ]
    
    removed = []
    for collection in collections_to_check:
        for item in list(collection):
            if item.users == 0:
                collection.remove(item)
                removed.append((type(item).__name__, item.name))
    
    return removed

def deep_copy_scene(source_scene, new_name):
    """深度复制场景"""
    # 复制场景
    new_scene = source_scene.copy()
    new_scene.name = new_name
    
    # 复制所有对象
    object_mapping = {}
    for obj in source_scene.objects:
        new_obj = obj.copy()
        new_obj.data = obj.data.copy() if obj.data else None
        object_mapping[obj] = new_obj
        new_scene.collection.objects.link(new_obj)
    
    # 更新引用
    for new_obj in object_mapping.values():
        for attr in ['parent', '约束', ' modifiers']:
            # 更新父对象和引用
            pass
    
    return new_scene
```

### 属性工具

```python
def copy_custom_properties(source, target, exclude=None):
    """复制自定义属性"""
    exclude = exclude or []
    
    for key in source.keys():
        if key.startswith('_') and key[1:] in ['RNA_UI', 'nap']:
            continue
        if key in exclude:
            continue
        target[key] = source[key]

def has_custom_property(obj, prop_name):
    """检查对象是否有指定的自定义属性"""
    return prop_name in obj

def get_property_type(obj, prop_name):
    """获取属性类型"""
    if prop_name not in obj:
        return None
    
    value = obj[prop_name]
    return type(value).__name__

def export_properties_to_dict(obj):
    """导出所有自定义属性到字典"""
    props = {}
    for key in obj.keys():
        if not key.startswith('_'):
            props[key] = obj[key]
    return props

def import_properties_from_dict(obj, props_dict):
    """从字典导入自定义属性"""
    for key, value in props_dict.items():
        obj[key] = value
```

## 常见问题排查

### 问题：渲染输出颜色不正确

**可能原因：**
1. 色彩管理设置
2. 颜色空间转换
3. 文件格式不支持

**解决方案：**

```python
# 检查色彩管理
scene = bpy.context.scene
view_settings = scene.view_settings

# 设置正确的显示色彩空间
view_settings.display_device = 'sRGB'
view_settings.view_transform = 'Standard'

# 设置文件输出色彩空间
render = scene.render
render.image_settings.color_mode = 'RGBA'
render.image_settings.color_management = 'OVERRIDE'
render.image_settings.look = 'None'

# 对于 EXR 格式
if render.image_settings.file_format == 'OPEN_EXR':
    render.image_settings.color_mode = 'RGBA'
    render.image_settings.exr_codec = 'DWAA'
```

### 问题：资产库目录不更新

**解决方案：**

```python
# 确保保存目录
bpy.ops.asset.catalogs_save()

# 重新加载库
bpy.ops.wm.revert_mainfile()

# 手动编辑目录文件
def manual_catalog_edit(library_path, catalog_path, new_name=None, new_path=None):
    """手动编辑目录"""
    catalog_file = os.path.join(library_path, "blender_assets.cats.txt")
    
    with open(catalog_file, 'r') as f:
        lines = f.readlines()
    
    # 找到并修改目录
    for i, line in enumerate(lines):
        if line.strip() == catalog_path:
            if new_name:
                lines[i + 1] = f'    name = "{new_name}"\n'
            if new_path:
                lines[i] = f"{new_path}\n"
            break
    
    with open(catalog_file, 'w') as f:
        f.writelines(lines)
```

### 问题：库链接后对象不可见

**解决方案：**

```python
# 检查对象是否在集合中
obj = bpy.data.objects.get("LinkedObject")
if obj:
    # 检查是否在活动场景中
    if obj.name not in bpy.context.scene.collection.objects:
        # 添加到场景
        bpy.context.scene.collection.objects.link(obj)
    
    # 检查是否被隐藏
    obj.hide_viewport = False
    obj.hide_render = False
    
    # 检查集合可见性
    for collection in obj.users_collection:
        collection.hide_viewport = False
        collection.hide_render = False
```

### 问题：自定义属性不显示

**解决方案：**

```python
# 确保属性已正确注册
bpy.utils.register_class(MyProperties)

# 确保指针属性已关联
bpy.types.Object.my_props = bpy.props.PointerProperty(type=MyProperties)

# 检查属性是否存在
obj = bpy.context.object
if hasattr(obj, 'my_props'):
    print(f"属性值: {obj.my_props.my_int}")
else:
    # 属性可能未注册
    print("属性未找到，请重新注册插件")

# 检查 IDProperties
for key in obj.keys():
    print(f"属性 {key}: {obj[key]}")
```

### 问题：导入导出失败

**解决方案：**

```python
# 检查文件路径
import os
filepath = "//models/model.obj"
absolute_path = bpy.path.abspath(filepath)
if not os.path.exists(absolute_path):
    print(f"文件不存在: {absolute_path}")
    # 尝试查找
    for ext in ['.obj', '.fbx', '.glb']:
        alt_path = bpy.path.abspath(f"//models/model{ext}")
        if os.path.exists(alt_path):
            filepath = alt_path
            break

# 检查导入插件是否启用
addons = bpy.context.preferences.addons
if 'io_scene_obj' not in addons:
    bpy.ops.preferences.addon_enable(module='io_scene_obj')

# 捕获导入错误
try:
    bpy.ops.wm.obj_import(filepath=filepath)
except Exception as e:
    print(f"导入错误: {e}")
```

## 设计最佳实践

### 1. 渲染优化

```python
# Cycles GPU 渲染优化
def optimize_cycles_render():
    scene = bpy.context.scene
    cycles = scene.cycles
    
    # 降低采样用于预览
    cycles.samples = 64
    
    # 启用自适应采样
    cycles.use_adaptive_sampling = True
    cycles.adaptive_threshold = 0.05
    
    # 限制最大光线深度
    cycles.max_bounces = 6
    cycles.diffuse_bounces = 3
    cycles.glossy_bounces = 3
    cycles.transmission_bounces = 3
    cycles.volume_bounces = 0
    
    # 降噪
    cycles.use_denoising = True

# EEVEE 优化
def optimize_eevee_render():
    scene = bpy.context.scene
    eevee = scene.eevee
    
    # 降低采样
    eevee.taa_render_samples = 32
    
    # 优化阴影
    eevee.use_shadows = True
    eevee.shadow_cube_size = '1024'
    eevee.use_clustered_shading = True
    
    # 简化效果
    eevee.use_gtao = False
    eevee.use_ssr = False
```

### 2. 资产管理最佳实践

```python
# 资产命名规范
def create_asset_name(category, name, variant=''):
    """创建规范的资产名称"""
    parts = [category, name]
    if variant:
        parts.append(variant)
    return '_'.join(parts)

# 批量处理资产
def batch_process_assets(assets, process_func):
    """批量处理资产"""
    results = []
    for asset in assets:
        try:
            result = process_func(asset)
            results.append((asset, 'success', result))
        except Exception as e:
            results.append((asset, 'error', str(e)))
    return results

# 资产预览生成
def generate_asset_previews(assets):
    """为资产生成预览"""
    for asset in assets:
        if asset.asset_data:
            # 设置活动对象
            bpy.context.view_layer.objects.active = asset
            asset.select_set(True)
            
            # 生成预览
            bpy.ops.object.asset_generate_preview()
            
            # 清除选择
            asset.select_set(False)
```

### 3. 数据块管理最佳实践

```python
# 使用命名约定
PREFIXES = {
    'MESH': 'Mesh_',
    'OBJ': 'Obj_',
    'MAT': 'Mat_',
    'TEX': 'Tex_',
    'SND': 'Snd_',
    'CAM': 'Cam_',
    'LGT': 'Lgt_',
    'COL': 'Col_',
    'SCN': 'Scn_',
    'ACT': 'Act_',
}

def create_named_datablock(datatype, prefix, base_name):
    """创建命名规范的数据块"""
    name = f"{PREFIXES.get(datatype.upper(), '')}{base_name}"
    collection = getattr(bpy.data, datatype.lower() + 's')
    
    if datatype.upper() == 'MESH':
        return collection.new(name)
    elif datatype.upper() == 'OBJECT':
        return collection.new(name, None)
    else:
        return collection.new(name)

# 数据块引用检查
def check_references(datablock):
    """检查数据块的引用关系"""
    references = {
        'referenced_by': [],  # 被哪些数据块引用
        'references': [],     # 引用了哪些数据块
    }
    
    # 遍历所有数据块查找引用
    for other_datablock in bpy.data.objects:
        if other_datablock == datablock:
            continue
        
        # 检查对象引用
        if datablock in other_datablock.children:
            references['referenced_by'].append(other_datablock)
        
        if other_datablock.parent == datablock:
            references['referenced_by'].append(other_datablock)
    
    return references
```

### 4. 属性定义最佳实践

```python
# 使用属性组组织相关属性
class MyObjectProperties(bpy.types.PropertyGroup):
    """对象自定义属性组"""
    
    # 基础属性
    enabled: bpy.props.BoolProperty(
        name="启用",
        description="是否启用此功能",
        default=True
    )
    
    # 数值属性
    intensity: bpy.props.FloatProperty(
        name="强度",
        description="效果强度",
        default=1.0,
        min=0.0,
        max=10.0,
        step=0.1,
        precision=2
    )
    
    # 颜色属性
    color: bpy.props.FloatVectorProperty(
        name="颜色",
        description="效果颜色",
        size=4,
        min=0.0,
        max=1.0,
        default=(1.0, 1.0, 1.0, 1.0),
        subtype='COLOR'
    )
    
    # 枚举属性
    mode: bpy.props.EnumProperty(
        name="模式",
        description="操作模式",
        items=[
            ('MODE_A', "模式 A", "第一个模式"),
            ('MODE_B', "模式 B", "第二个模式"),
            ('MODE_C', "模式 C", "第三个模式"),
        ],
        default='MODE_A'
    )
    
    # 引用属性
    target: bpy.props.PointerProperty(
        name="目标对象",
        description="操作的目标对象",
        type=bpy.types.Object
    )

# 注册属性组
bpy.utils.register_class(MyObjectProperties)
bpy.types.Object.my_properties = bpy.props.PointerProperty(type=MyObjectProperties)
```

## 高级主题

### 系统偏好设置

```python
import bpy

def get_system_preferences():
    """获取系统偏好设置"""
    prefs = bpy.context.preferences
    return {
        'system': prefs.system,
        'view': prefs.view,
        'edit': prefs.edit,
        'input': prefs.input,
    }

def configure_gpu_backend(backend='VULKAN'):
    """配置 GPU 后端"""
    prefs = bpy.context.preferences
    prefs.system.gpu_backend = backend  # OPENGL, VULKAN
    
    # Vulkan 设备选择
    if backend == 'VULKAN':
        prefs.system.gpu_preferred_device = 'Auto'

def configure_cycles_devices():
    """配置 Cycles 渲染设备"""
    prefs = bpy.context.preferences
    system = prefs.system
    
    # CUDA 设备
    system.cycles_debug = False
    system.cycles_compute_device_type = 'CUDA'
    
    # GPU 设置
    system.gpu_extensions = True
    system.use_eevee_viewport_samples = False

def configure_network_settings():
    """配置网络设置"""
    prefs = bpy.context.preferences
    system = prefs.system
    
    # 在线访问
    system.use_online_access = True
    
    # 超时设置
    system.network_timeout = 30  # 秒
    
    # 连接限制
    system.network_connection_limit = 10
```

### 快捷键配置

```python
def export_keymap(filepath):
    """导出当前快捷键配置"""
    bpy.ops.preferences.keyconfig_export(filepath=filepath, all_keymaps=True)

def import_keymap(filepath):
    """导入快捷键配置"""
    bpy.ops.preferences.keyconfig_import(filepath=filepath)

def get_keymap_items(operator_name):
    """获取操作符的快捷键配置"""
    keymaps = bpy.context.window_manager.keyconfigs.default.keymaps
    items = []
    
    for keymap in keymaps:
        for item in keymap.keymap_items:
            if operator_name in item.idname:
                items.append({
                    'keymap': keymap.name,
                    'name': item.name,
                    'type': item.type,
                    'value': item.value,
                    'shift': item.shift,
                    'ctrl': item.ctrl,
                    'alt': item.alt,
                })
    
    return items

def add_shortcut(keymap_name, operator_idname, hotkey, shift=False, ctrl=False, alt=False):
    """添加快捷键"""
    keymaps = bpy.context.window_manager.keyconfigs.default.keymaps
    
    for keymap in keymaps:
        if keymap.name == keymap_name:
            item = keymap.keymap_items.new(
                idname=operator_idname,
                type=hotkey,
                value='PRESS'
            )
            item.shift = shift
            item.ctrl = ctrl
            item.alt = alt
            return item
    
    return None
```

### 场景转换工具

```python
def duplicate_scene_with_objects(source_scene_name, new_scene_name):
    """复制场景及其中的所有对象"""
    source_scene = bpy.data.scenes.get(source_scene_name)
    if not source_scene:
        return None
    
    # 复制场景
    new_scene = source_scene.copy()
    new_scene.name = new_scene_name
    
    # 复制对象并更新引用
    object_mapping = {}
    
    for obj in source_scene.objects:
        # 复制对象
        new_obj = obj.copy()
        new_obj.data = obj.data.copy() if obj.data else None
        
        # 复制动画数据
        if obj.animation_data:
            new_obj.animation_data = obj.animation_data.copy()
        
        # 更新集合引用
        for collection in obj.users_collection:
            if collection != source_scene.collection:
                collection.objects.link(new_obj)
        
        object_mapping[obj] = new_obj
    
    # 更新约束和修改器中的引用
    for new_obj in object_mapping.values():
        for constraint in new_obj.constraints:
            if constraint.target in object_mapping:
                constraint.target = object_mapping[constraint.target]
        
        for modifier in new_obj.modifiers:
            if hasattr(modifier, 'object') and modifier.object in object_mapping:
                modifier.object = object_mapping[modifier.object]
    
    return new_scene
```

### 批处理导入工具

```python
import os
import glob

def batch_import_files(directory, file_patterns, import_func):
    """批量导入文件"""
    imported_files = []
    
    for pattern in file_patterns:
        full_pattern = os.path.join(directory, pattern)
        files = glob.glob(full_pattern)
        
        for filepath in files:
            try:
                import_func(filepath)
                imported_files.append(filepath)
            except Exception as e:
                print(f"导入失败: {filepath}, 错误: {e}")
    
    return imported_files

def import_all_obj_files(directory):
    """导入目录中所有 OBJ 文件"""
    return batch_import_files(
        directory,
        ['*.obj'],
        lambda f: bpy.ops.wm.obj_import(filepath=f)
    )

def import_all_fbx_files(directory):
    """导入目录中所有 FBX 文件"""
    return batch_import_files(
        directory,
        ['*.fbx'],
        lambda f: bpy.ops.import_scene.fbx(filepath=f)
    )

def import_all_gltf_files(directory):
    """导入目录中所有 GLTF/GLB 文件"""
    patterns = ['*.gltf', '*.glb']
    return batch_import_files(
        directory,
        patterns,
        lambda f: bpy.ops.import_scene.gltf(filepath=f)
    )
```


---
## bpy_rendering_export

# 渲染与导出模块

## 渲染概述

渲染是将 3D 场景转换为 2D 图像的过程。Blender 包含三种具有不同优势的渲染引擎。

```python
# 渲染定义
# 将 3D 场景转换为 2D 图像
# 涉及光线追踪、阴影计算

# Blender 渲染引擎
# EEVEE - 基于物理的实时渲染器
# Cycles - 基于物理的路径追踪器
# Workbench - 为布局、建模和预览而设计

# 第三方渲染器
# 可作为插件使用
# 每个渲染器有各自的设置
# 控制渲染质量和性能

# 渲染要素
# 相机 - 定义视角
# 灯光 - 提供照明
# 材质 - 定义表面外观
```

## 渲染引擎

### EEVEE渲染器

EEVEE 是基于物理的实时渲染器，专为现代 OpenGL 和 Vulkan 渲染设计。

```python
# EEVEE 特点
# 实时预览
# 基于物理的着色
# 适合游戏和实时应用
# 支持大多数 Blender 特性

# 采样设置
# 采样数 - 控制渲染质量
# 时间限制 - 最大渲染时间
# 抖动 - 减少条纹

# 阴影设置
# 阴影类型 - 接触阴影等
# 阴影分辨率 - 阴影质量
# 软阴影 - 边缘柔和度

# 环境光遮蔽
# 启用 AO
# 半径 - 遮蔽范围
# 距离 - 影响距离
# 衰减 - 边缘衰减

# 屏幕空间反射
# 启用 SSR
# 粗糙度阈值
# 反射次数
```

### Cycles渲染器

Cycles 是基于物理的路径追踪器，提供逼真的渲染效果。

```python
# Cycles 特点
# 路径追踪算法
# 逼真的光照和反射
# 支持 GPU 和 CPU 渲染
# 适合最终渲染输出

# 采样设置
# 采样数 - 光线追踪样本
# 时间限制 - 最大渲染时间
# 噪声阈值 - 收敛精度

# 光照设置
# 光线反弹次数
# 漫反射反弹
# 镜面反射反弹
# 透明反弹

# 毛发设置
# 毛发采样
# 粗糙度设置
# 截断阈值

# 卷设置
# 卷采样
# 步长大小
# 密度比例

# 性能优化
# 块大小
# 线程模式
# 设备选择
```

### Workbench渲染器

Workbench 渲染器专为布局、建模和预览而设计。

```python
# Workbench 特点
# 快速渲染
# 低内存占用
# 适合建模和布局
# 可视化工具

# 显示方式
# 颜色 - 实体着色
# 材质预览 - 带材质显示
# 渲染 - 简单渲染

# 环境设置
# 灯光类型
# 灯光强度
# 环境颜色
```

## 渲染设置

### 分辨率和帧率

```python
# 分辨率设置
# X 和 Y 分辨率
# 图像宽度和高度
# 以像素为单位

# 百分比
# 相对于分辨率值的缩放
# 用于小尺寸测试渲染
# 保持相同比例

# 像素宽高比
# 非方形像素控制
# 用于电视等特殊显示
# 预扭曲图像

# 帧率
# 每秒显示的帧数
# 用于动画渲染
# 预设和自定义帧率

# FPS
# 帧率，以每秒帧数表示

# 基础值
# 更精确的帧率表示
# 用于 NTSC 等标准
# 表示为分数
```

### 渲染区域

```python
# 渲染区域
# 渲染视图的一部分
# 而非整个帧
# 加快测试渲染

# 裁剪到渲染区域
# 将渲染图像裁剪到区域大小
# 而非在其周围渲染透明背景
```

### 输出格式

```python
# 图像格式
# PNG - 无损压缩，支持 Alpha
# JPEG - 有损压缩，不支持 Alpha
# EXR - OpenEXR，高动态范围
# TIFF - 无损压缩，支持多层
# BMP - 未压缩位图

# 视频格式
# AVI - 未压缩视频
# MPEG - 有损视频压缩
# QuickTime - Apple 格式
# H.264 - 高效视频编码

# 编码设置
# 比特率 - 视频质量
# GOP 长度 - 关键帧间隔
# 编码器 - 视频编码器选择
```

## 色彩管理

### 色彩空间

```python
# 色彩空间定义
# 图像数据的颜色范围
# 影响颜色显示方式

# 常见色彩空间
# sRGB - 标准红绿蓝
# Linear - 线性色彩空间
# Rec.709 - 高清视频
# Raw - 原始数据
# ACES - 电影标准

# 色彩空间转换
# 输入色彩空间
# 显示色彩空间
# 查看变换
```

### 色彩管理流程

```python
# 渲染色彩管理
# 启用色彩管理
# 选择工作流程
# 配置色彩空间

# 查看变换
# 色调映射
# 曝光调整
# 伽马校正

# 宽容度
# 高光恢复
# 暗部细节
```

## 灯光设置

### 灯光类型

```python
# 点光源
# 向所有方向发光
# 类似灯泡

# 太阳光
# 平行光线
# 类似太阳光
# 角度影响阴影

# 聚光灯
# 锥形光束
# 可控角度和衰减
# 用于舞台照明

# 区域光
# 矩形或圆形区域光
# 更柔和的阴影
# 模拟窗户光

# 体积光
# 光束效果
# 需要体积着色器
```

### 灯光属性

```python
# 强度
# 灯光亮度
# 单位流明或瓦特

# 颜色
# 灯光颜色
# 使用颜色或温度

# 衰减
# 距离衰减
# 线性或平方衰减

# 阴影
# 启用/禁用阴影
# 阴影质量
# 软阴影设置
```

## 相机设置

### 相机属性

```python
# 焦距
# 镜头焦距
# 影响视角

# 传感器尺寸
# 相机传感器大小
# 影响视野和景深

# 景深
# 启用景深
# 对焦距离
# 光圈设置

# 运动模糊
# 启用运动模糊
# 模糊量
# 时间段设置

# 裁切
# 近裁剪面
# 远裁剪面
```

## 渲染层和通道

### 渲染层

```python
# 渲染层定义
# 将渲染分成多个层
# 分别渲染不同部分

# 集合排除
# 从渲染中排除集合
# 减少渲染时间

# 集合包含
# 只渲染特定集合

# 蒙版集合
# 使用集合作为蒙版
```

### 渲染通道

```python
# 组合通道
# 基础颜色
# 漫反射
# 镜面反射

# 光照通道
# 直接光照
# 间接光照
# 环境光遮蔽

# 遮罩通道
# 对象 ID
# 材质 ID
# 阴影遮罩

# 特效通道
# 深度 Z
# 速度向量
# 正常向量
```

## 导入导出

### 导入概述

有时您可能想要利用来自其他 2D 或 3D 软件的文件，或者您可能想要将在 Blender 中制作的内容在其他软件中编辑。Blender 提供了广泛的文件格式可用于导入和导出。

```python
# 导入文件
# 使用导入菜单
# 选择文件格式
# 配置导入选项

# 常用格式
# OBJ - 几何数据
# FBX - 动画角色
# Alembic - 高精度缓存
# USD - 通用场景描述
# PLY - 多边形数据
# STL - 3D 打印模型
```

### FBX格式

FBX（Filmbox）格式广泛用于在应用程序之间交换 3D 数据，尤其适用于动画角色和复杂场景数据。

```python
# FBX 导入选项
# 比例 - 缩放导入的对象
# 自定义属性 - 导入用户属性
# 几何数据 - 法线、顶点颜色
# 材质 - 材质处理
# 动画 - 动画导入
# 骨架 - 骨骼选项

# FBX 导出选项
# 包含 - 选择导出对象
# 变换 - 应用变换
# 几何数据 - 导出属性
# 动画 - 导出动画数据
# 骨架 - 骨骼导出
# 材质 - 材质处理
```

### OBJ格式

OBJ 格式是一种流行的纯文本格式，但只支持基本几何体和材质。

```python
# OBJ 导入选项
# 比例 - 缩放导入对象
# 轴向转换 - 坐标轴转换
# 分割选项 - 按对象或组分割
# 验证网格 - 检查数据错误
# 材质 - 材质处理

# OBJ 限制
# 不支持骨架
# 不支持灯光
# 不支持相机
# 不支持空对象
# 不支持父子关系

# OBJ 导出选项
# 包含 - 选择导出对象
# 几何属性 - UV、法线、颜色
# 曲线处理 - NURBS 导出
# 网格三角化
# 应用修饰符
# 应用变换

# 分组选项
# 对象组
# 材质组
# 顶点组
# 平滑组
```

### 其他格式

```python
# Alembic (ABC)
# 高精度动画缓存
# 支持复杂动画数据
# 用于视觉效果

# USD
# 通用场景描述
# 行业标准
# 支持层和变体

# PLY
# 多边形文件格式
# 支持颜色和法线
# 用于扫描数据

# STL
# 3D 打印格式
# 只支持几何体
# 无材质或颜色
```

### 导出最佳实践

```python
# 选择正确格式
# 根据目标软件选择
# 考虑特性支持
# 平衡文件大小

# 准备场景
# 应用变换
# 清理不需要的对象
# 检查命名

# 测试导出
# 导入测试文件
# 验证数据完整性
# 检查动画和材质

# 优化文件
# 减少不必要的精度
# 使用适当的压缩
# 清理未使用的数据
```

## 渲染输出

### 渲染动画

```python
# 动画渲染设置
# 帧范围
# 起始帧和结束帧
# 帧步长

# 输出设置
# 输出路径
# 文件格式
# 编码设置

# 渲染方法
# 序列渲染
# 批处理渲染
# 后台渲染
```

### 渲染图像

```python
# 图像渲染
# 单帧渲染
# 区域渲染
# 立体渲染

# 图像保存
# 保存图像
# 使用渲染设置中的格式
# 管理文件命名
```

## 性能优化

### 渲染优化

```python
# 减少采样
# 使用测试渲染
# 降低分辨率
# 减少反弹次数

# 优化场景
# 减少多边形数量
# 使用代理对象
# 优化灯光

# 使用缓存
# 缓存模拟
# 缓存几何体
# 预计算动画

# GPU 渲染
# 选择正确的 GPU
# 优化显存使用
# 使用降噪
```

### 视口优化

```python
# 视口设置
# 使用简化设置
# 降低显示分辨率
# 禁用不必要的效果

# 快速渲染
# 使用 EEVEE
# 降低质量设置
# 使用遮挡剔除
```

## Python脚本

### 渲染操作

```python
# 设置渲染引擎
import bpy
bpy.context.scene.render.engine = 'CYCLES'

# 配置 Cycles
scene = bpy.context.scene
scene.cycles.samples = 128
scene.cycles.use_adaptive_sampling = True

# 配置 EEVEE
scene.eevee.taa_render_samples = 32
scene.eevee.use_shadows = True

# 设置分辨率
scene.render.resolution_x = 1920
scene.render.resolution_y = 1080
scene.render.resolution_percentage = 100

# 设置帧率
scene.render.fps = 24
scene.render.fps_base = 1
```

### 渲染输出

```python
# 设置输出路径
import bpy
scene = bpy.context.scene
scene.render.filepath = "//render/output/"

# 设置文件格式
scene.render.image_settings.file_format = 'PNG'

# 设置颜色管理
scene.render.image_settings.color_mode = 'RGBA'
scene.render.image_settings.color_management = 'OVERRIDE'

# 渲染帧
bpy.ops.render.render(write_still=True)

# 渲染动画
bpy.ops.render.render(animation=True)
```

### 导出操作

```python
# 导出 OBJ
import bpy
bpy.ops.wm.obj_export(
    filepath="//export/model.obj",
    export_selected=True,
    apply_transform=True,
    export_normals=True,
    export_uv=True
)

# 导出 FBX
import bpy
bpy.ops.wm.fbx_export(
    filepath="//export/animation.fbx",
    export_selected=True,
    export_anim=True,
    export_skins=True
)

# 导出 Alembic
import bpy
bpy.ops.wm.alembic_export(
    filepath="//export/cache.abc",
    export_selected=True,
    export_uv=True,
    export_normals=True
)
```

### 批处理渲染

```python
# 批处理多个场景
import bpy

scenes = ['Scene1', 'Scene2', 'Scene3']

for scene_name in scenes:
    # 切换场景
    bpy.context.window.scene = bpy.data.scenes[scene_name]
    
    # 设置输出
    scene = bpy.context.scene
    scene.render.filepath = f"//render/{scene_name}_"
    
    # 渲染
    bpy.ops.render.render(animation=True)
```

## 常见问题解决

### 渲染问题

```python
# 渲染太慢
# 降低采样数
# 减少分辨率
# 使用 GPU 渲染
# 优化场景复杂度

# 渲染有噪点
# 增加采样数
# 使用降噪
# 优化灯光设置
# 增加光线反弹

# 阴影问题
# 检查阴影设置
# 增加阴影分辨率
# 调整阴影距离
# 禁用自阴影

# 颜色不正确
# 检查色彩管理设置
# 确认色彩空间
# 调整曝光和伽马
```

### 导出问题

```python
# 导入/导出错误
# 检查文件格式
# 验证文件完整性
# 更新插件

# 数据丢失
# 检查导出选项
# 验证支持的特性
# 测试导入导出

# 比例错误
# 检查轴向设置
# 应用变换
# 验证单位设置

# 动画丢失
# 检查动画导出选项
# 确认动作已应用
# 验证骨骼导出
```


---
## bpy_compositing

# 合成与后期处理模块

## 合成概述

合成节点允许您组装和增强图像（或电影）。使用合成节点，您可以将两段素材粘合在一起，一次性对整个序列进行着色。您可以静态或动态地改变单张图像或整个电影片段的颜色，动态方式会随着片段的进展而改变。

```python
# 合成定义
# 使用节点将图像导入 Blender
# 更改图像
# 可选地与其他图像合并
# 最后保存

# 术语说明
# 术语"图像"可以指单张图片
# 编号序列中的图片
# 或电影片段的一帧
# 合成器一次处理一张图像
# 无论您提供什么类型的输入

# 合成应用
# 前景演员在蓝幕前的原始素材
# 或渲染对象
# 可以叠加在背景上
# 将两者合成在一起
# 就得到了合成素材
```

## 合成工作流程

### 开始合成

```python
# 打开合成器
# 切换到 Compositor 工作区
# 或在图像编辑器中选择合成类型

# 创建节点树
# 单击标题中的新建按钮
# 添加合成节点树数据块到场景
# 可以跨不同场景重用
# 或从其他 blend-file 链接/追加

# 启用合成
# 在输出的后期处理属性中
# 必须启用合成管线选项
# 才能让渲染引擎使用合成节点树
```

### 基本设置

```python
# 节点连接流程
# 输入节点 - 导入图像或视频
# 处理节点 - 应用效果
# 输出节点 - 渲染和保存

# 节点图布局
# 从基本设置开始
# 添加和连接各种类型的合成节点
# 构建从简单颜色校正到复杂合成的效果

# 通用节点控制
# 节点移动 - 拖拽标题栏
# 节点连接 - 从端口拖拽连线
# 节点选择 - 点击选择
# 节点删除 - 按 Delete 键
```

## 合成数据类型

### 数据维度

```python
# 合成节点操作的数据有两种维度
# 图像 - 2D 像素数据
# 单值 - 无维度的单一值

# 数据处理
# 例如 Levels 节点输出单一值
# Render Layers 节点输出图像
# 期望单值的节点输入
# 在给定图像时假设默认值

# 默认值规则
# 默认值被视为恒等值
# 因此对输出没有影响
# Transform 节点的 X、Y、Angle 输入默认值为零
# Scale 输入默认值为 1
```

### 数据类型

三种类型的数据存在，所有数据都以半精度格式存储：

```python
# Float 类型
# 有符号浮点数
# 整数数据也存储为浮点数
# 因为不存在整数类型

# Vector 类型
# 4D 向量
# 可以有不同的解释
# 取决于使用它的节点

# 2D 向量解释
# 最后两个分量被忽略
# 例如 Displace 节点的 Vector 输入

# 3D 向量解释
# 最后一个分量被忽略
# 例如 Separate XYZ 节点的 Vector 输入

# 两个连续的 2D 向量
# 例如 Vector Blur 节点期望的 Velocity Pass
# 在 X 和 Y 分量中有 2D Previous Velocity
# 在 Z 和 W 分量中有 2D Next Velocity

# Color 类型
# 4D 向量存储红、绿、蓝和 Alpha
# 颜色是自由形式的
# 不符合特定的色彩空间
# 或 alpha 存储模型
```

### 隐式转换

```python
# 当节点输入被给予与其类型不同的数据时
# 执行以下隐式转换

# Float 转 Vector
# f => Vector(f, f, f, 0)

# Float 转 Color
# f => Color(f, f, f, 1)

# Vector 转 Float
# (x, y, z, w) => Average(x, y, z)

# Vector 转 Color
# (x, y, z, w) => Color(x, y, z, 1)

# Color 转 Float
# (r, g, b, a) => Luminance(r, g, b)

# Color 转 Vector
# (r, g, b, a) => Vector(r, g, b, 0)

# 示例
# Math 节点期望浮点输入
# 颜色类型自动转换为浮点
```

## 合成空间

### 图像域

合成器设计为允许在无限合成空间中进行合成。因此，图像不仅由其大小表示，还由它们在该空间中的变换表示，就像 3D 对象有变换一样。恒等变换表示在空间中居中的图像。

```python
# 图像变换
# 使用节点如 Transform、Translate、Rotate
# 变换图像

# 域定义
# 图像在空间中由其变换和大小占据的矩形区域
# 称为图像的"域"

# 恒等变换
# 图像在空间中居中
# 无变换应用

# 变换影响
# 图像可以缩放和平移
# 改变其在合成空间中的位置和大小
```

### 操作域

合成节点在合成空间的特定矩形区域上操作，称为"操作域"。节点只考虑与操作域重叠的输入图像区域，忽略其余部分。

```python
# 操作域概念
# 节点只操作特定矩形区域
# 只考虑与操作域重叠的输入图像
# 忽略其余部分

# 不完全重叠处理
# 如果输入图像没有完全覆盖操作域
# 该输入的操作域的其余部分
# 将被假设为零值
# 零向量
# 或透明零颜色
# 取决于类型

# 零填充
# 超出图像边界的区域
# 填充为零值
# 确保节点正确处理
```

## 节点类型

### 输入节点

```python
# 图像输入
# 导入外部图像文件
# 支持多种格式

# 视频输入
# 导入视频片段
# 支持序列帧

# Render Layers 节点
# 导入渲染层
# 包含多个渲染通道

# 纹理输入
# 导入纹理图像
# 用于遮罩和渐变

# 颜色输入
# 提供纯色
# 用于合成背景

# 渐变输入
# 创建渐变颜色
# 用于遮罩和效果
```

### 输出节点

```python
# 复合输出
# 合成最终结果输出

# 查看器节点
# 在节点编辑器中预览图像

# 保存输出
# 配置输出格式和路径

# 分割输出
# 将合成结果输出到不同通道
```

### 颜色校正节点

```python
# 色相/饱和度
# 调整图像的色相、饱和度、亮度
# 范围：色相-1到1，饱和度-1到1，亮度-1到1

# 色彩平衡
# 调整阴影、中间调、高光的颜色
# 控制 CMY 和 RGB 颜色

# 曲线
# 使用曲线调整亮度和颜色
# 通道：RGB、红、绿、蓝、亮度

# 曝光
# 调整图像的整体曝光
# 影响所有颜色通道

# 亮度对比度
# 简单调整亮度和对比度

# 色调
# 调整图像的色调偏移

# 色彩校正
# 高级色彩校正工具
# 控制范围、饱和度、对比度
```

### 滤镜节点

```python
# 模糊
# 高斯模糊 - 径向对称模糊
# 运动模糊 - 模拟运动效果
# 快速模糊 - 低质量快速模糊
# 变量模糊 - 基于深度模糊
# 方向模糊 - 指定方向模糊
# 缩放模糊 - 径向缩放模糊

# 锐化
# 增强图像边缘清晰度

# 柔和化
# 减少图像噪点和细节

# 素描
# 创建素描效果
# 检测边缘并反转

# 块状模糊
# 像素化效果
# 创建马赛克效果

# 去噪
# 减少图像噪点
# 保留边缘细节
```

### 遮罩节点

```python
# 椭圆遮罩
# 创建椭圆遮罩
# 可选择羽化边缘

# 矩形遮罩
# 创建矩形遮罩
# 可选择圆角

# 圆形遮罩
# 创建圆形遮罩

# 键控遮罩
# 基于颜色创建遮罩
# 用于抠像和合成

# 亮度遮罩
# 基于亮度创建遮罩
# 用于选择高光或阴影

# 颜色遮罩
# 基于颜色范围创建遮罩
```

### 变换节点

```python
# 变换
# 组合位置、旋转、缩放
# 一次应用多个变换

# 平移
# 移动图像位置
# 支持 X 和 Y 偏移

# 旋转
# 旋转图像
# 指定角度

# 缩放
# 调整图像大小
# 统一或非统一缩放

# 裁剪
# 裁剪图像区域
# 定义裁剪框

# 调整大小
# 将图像缩放到特定尺寸
```

### 跟踪节点

```python
# 跟踪数据输入
# 导入跟踪数据
# 用于稳定和运动模糊

# 稳定化
# 使用跟踪数据稳定图像

# 相机运动模糊
# 基于相机运动应用模糊

# 对象运动模糊
# 基于对象运动应用模糊
```

### 实用工具节点

```python
# 数学运算
# 加、减、乘、除
# 最小、最大、平均
# 正弦、余弦、切线
# 对数、指数、根

# 比较
# 小于、大于、等于
# 检查值之间的关系

# 通道分离
# 将图像分离为 RGB 通道
# 或分离 RGBA 分量

# 通道组合
# 组合通道为图像

# 值到颜色
# 将数值映射到颜色
# 用于可视化数据

# 颜色到值
# 将颜色转换为数值
# 提取亮度或特定通道
```

### 颜色节点

```python
# 混合颜色
# 混合两个图像
# 支持多种混合模式

# 亮度/对比度调整
# 简单调整图像亮度

# 伽马校正
# 应用伽马校正
# 非线性亮度调整

# 色调映射
# 将 HDR 映射到 LDR
# 保留细节和对比度

# 色彩空间转换
# 在色彩空间之间转换
# 例如线性到 sRGB
```

### 效果节点

```python
# 斑驳
# 添加斑驳效果
# 模拟镜头不完美

# 景深
# 基于深度应用模糊
# 模拟相机景深

# 镜头光晕
# 添加镜头光晕效果
# 模拟镜头反射

# 胶片颗粒
# 添加电影颗粒效果
# 增加真实感

# 色差
# 添加色差效果
# 模拟镜头色散

# 污渍和划痕
# 添加污渍和划痕
# 模拟旧胶片效果

# 倾斜偏移
# 创建移轴摄影效果
# 模糊特定区域
```

### 抠像节点

```python
# 键控
# 基于颜色键出背景
# 用于绿幕/蓝幕合成

# 差异键
# 使用差异图像键出背景

# 亮度键
# 基于亮度创建遮罩

# 颜色键
# 基于特定颜色创建遮罩

# 通道键
# 使用特定通道创建遮罩
```

## 节点组

### 创建节点组

```python
# 选择节点
# 在节点编辑器中选择节点

# 创建组
# 按 M 或使用菜单创建节点组
# 或按 Ctrl+G

# 编辑组
# 双击组进入编辑模式
# 可以添加和连接节点

# 重命名组
# 在属性中重命名
# 使用描述性名称

# 使用组
# 从添加菜单中拖放组
# 或搜索组名称
```

### 组输入输出

```python
# 组输入节点
# 定义组的公开接口
# 添加和配置输入插座

# 组输出节点
# 定义组的输出
# 连接要公开的图像

# 嵌套组
# 可以在组中嵌套其他组
# 复用复杂的节点设置
```

## 渲染输出

### 渲染设置

```python
# 渲染帧
# 渲染单个帧或图像
# 使用渲染按钮

# 渲染动画
# 渲染一系列帧
# 配置帧范围和输出

# 输出格式
# PNG - 支持 Alpha 通道
# JPEG - 有损压缩
# EXR - 支持浮点和多层
# TIFF - 高质量输出

# 色彩管理
# 启用色彩管理
# 选择色彩空间
# 配置查看变换
```

### 保存选项

```python
# 保存图像
# 使用保存图像命令
# 使用渲染面板中的图像格式设置

# 保存序列
# 如果输入电影片段
# 或使用时间节点
# 使用动画按钮和设置

# Alpha 通道
# 如果以后要叠加
# 使用支持 Alpha 通道的图像格式
# 例如 PNG

# Z 深度
# 如果以后要前后排列
# 或创建景深效果
# 使用支持 Z 深度的格式
# 例如 EXR

# 保存为视频
# 使用 AVI 或 QuickTime 格式
# 所有帧在一个文件中
# 使用动画按钮和设置
```

## 合成示例

### 颜色分级

```python
# 改变图像情绪
# 冷色 - 添加蓝色调
# 暖色 - 添加橙色调
# 怀旧 - 柔化图像
# 愤怒 - 添加红色调或增强红色
# 惊喜 - 锐化和增强对比度
# 快乐 - 添加黄色
# 真实感 - 添加灰尘和漂浮物纹理
```

### 合成叠加

```python
# 前景叠加
# 导入前景图像或渲染对象
# 导入背景图像
# 调整大小和位置
# 使用遮罩处理边缘
# 添加阴影效果

# 绿幕抠像
# 导入绿幕素材
# 使用键控节点抠像
# 调整容差和屏蔽
# 添加边缘清理
# 合成到新背景
```

### 特效应用

```python
# 添加镜头光晕
# 添加光晕节点
# 调整光晕类型和强度
# 定位光晕位置

# 添加颗粒
# 添加胶片颗粒节点
# 调整颗粒强度和大小
# 控制噪点类型

# 调整景深
# 添加景深节点
# 配置焦点距离
# 调整模糊量

# 添加色彩效果
# 添加晕影效果
# 添加色差效果
# 添加渐晕效果
```

## 实时合成器

### 实时预览

```python
# 实时性能
# 实时合成器提供实时预览
# 使用 GPU 加速
# 支持交互式编辑

# 视口显示
# 在 3D 视口中显示合成结果
# 使用合成预览节点
# 快速查看最终效果

# 性能优化
# 减少节点数量
# 降低预览分辨率
# 使用代理图像
```

### 批处理

```python
# 批处理渲染
# 配置多个渲染作业
# 使用命令行渲染
# 自动处理多个文件

# 脚本化合成
# 使用 Python 自动化合成
# 创建批处理脚本
# 处理多个镜头
```

## 最佳实践

### 组织节点树

```python
# 命名约定
# 为节点和组使用描述性名称
# 保持一致的命名风格
# 添加注释说明复杂节点

# 布局整洁
# 从左到右或从上到下排列
# 保持连线整洁
# 避免交叉连线

# 分组相关节点
# 使用节点组封装功能
# 复用常用设置
# 简化主节点树
```

### 性能优化

```python
# 减少节点数量
# 合并可以合并的节点
# 使用更高效的节点

# 使用代理
# 使用低分辨率预览
# 加快编辑速度
# 渲染时使用全分辨率

# 优化节点顺序
# 调整节点顺序以提高性能
# 尽早排除不需要的区域
# 减少处理的数据量
```

### 工作流程

```python
# 基础到复杂
# 从简单调整开始
# 逐步添加效果
# 测试每个步骤

# 保存版本
# 保存不同阶段的版本
# 记录更改
# 便于回退和修改

# 测试渲染
# 定期渲染测试
# 检查合成效果
# 调整问题区域
```

## 节点脚本

### Python访问

```python
# 访问合成节点树
scene = bpy.context.scene
node_tree = scene.node_tree

# 遍历节点
for node in node_tree.nodes:
    print(f"{node.name}: {node.type}")

# 创建节点
node = node_tree.nodes.new('CompositorNodeRLayers')
node.location = (300, 300)

# 连接节点
input_node = node_tree.nodes['Render Layers']
output_node = node_tree.nodes['Composite']
node_tree.links.new(output_node.inputs[0], input_node.outputs[0])

# 修改属性
node.blur = 10
node.size_x = 5
node.size_y = 5
```

### 自动化

```python
# 创建标准合成设置
def create_basic_composite():
    scene = bpy.context.scene
    tree = scene.node_tree

    # 清除现有节点
    tree.nodes.clear()

    # 创建渲染层节点
    rlayers = tree.nodes.new('CompositorNodeRLayers')
    rlayers.location = (0, 0)

    # 创建复合节点
    composite = tree.nodes.new('CompositorNodeComposite')
    composite.location = (400, 0)

    # 连接节点
    tree.links.new(
        composite.inputs[0],
        rlayers.outputs[0]
    )

    # 启用合成
    scene.render.use_compositing = True

# 创建颜色分级节点
def add_color_grading():
    scene = bpy.context.scene
    tree = scene.node_tree

    # 添加曲线节点
    curves = tree.nodes.new('CompositorNodeCurves')
    curves.location = (300, 200)

    # 配置曲线
    # ...
```

## 常见问题解决

### 渲染问题

```python
# 黑色输出
# 检查节点连接
# 确保所有输入已连接
# 检查节点启用状态

# 丢失的图像
# 检查文件路径
# 确保图像已加载
# 检查图像序列帧范围

# 错误的颜色
# 检查色彩空间设置
# 确保正确的输入输出色彩空间
# 检查色彩管理设置

# 内存不足
# 减少图像分辨率
# 使用分块渲染
# 关闭不需要的渲染层
```

### 节点问题

```python
# 节点不工作
# 检查节点类型是否正确
# 验证输入类型
# 检查节点是否启用

# 性能差
# 减少节点数量
# 使用更简单的节点
# 使用预览模式

# 预览不同
# 检查输出分辨率
# 验证色彩空间
# 考虑后期缩放
```


---
## bpy_materials_shading

# 材质与着色模块

## 材质系统概述

材质控制网格、曲线、体积和其他对象的外观。它们定义对象由什么物质构成、颜色和纹理，以及光线如何与之交互。基于物理的材质可以使用 Principled BSDF、Principled Hair 和 Principled Volume 着色器创建。使用这些超级着色器，可以创建多种多样的材质，包括塑料、玻璃、金属、布料、皮肤、头发、烟雾和火。

```python
# 材质创建位置
# 材质属性面板
# 着色器编辑器
# 两者提供相同着色节点和材质设置的

# 不同视图
# 默认着色工作区有着色器编辑器和3D视口
# 可以设置为材质预览或渲染着色
# 以交互式预览材质如何与场景中的对象和灯光交互

# 材质分配
# 材质是可以分配给一个或多个对象的数据块
# 可以为网格的不同部分分配不同的材质

# 纹理来源
# 图像纹理可以从头创建
# 或通过图像纹理节点加载现有图像
# 还有各种程序化纹理节点可用
```

## 材质组件

材质由三个着色器组成，定义表面的外观、对象内部的体积以及表面的置换。

### 表面着色器

表面着色器控制网格表面的纹理和光线交互。一个或多个 BSDF 指定入射光是被反射回表面、折射进入网格还是被吸收。发射定义表面如何发光，允许任何表面成为光源。

```python
# BSDF 术语
# BSDF 是双向散射分布函数的缩写
# 它定义光线在表面如何反射和折射

# 反射
# BSDF 将入射光线反射到表面的同一侧

# 透射
# BSDF 透射入射光线穿过表面
# 从另一侧离开

# 折射
# BSDF 是一种透射
# 透射入射光线并改变其方向
# 当它从表面的另一侧离开时

# BSDF 参数
# 与非基于物理的渲染器的主要区别
# 来自灯光的直接反射和来自其他表面的间接反射
# 不是分离的
# 而是使用单个 BSDF 处理
# 这限制了一些可能性
# 但有助于用更少的参数创建一致的渲染

# 粗糙度
# 对于光泽 BSDF
# 粗糙度参数控制反射的锐度
# 从 0.0（完全锐利）到 1.0（非常柔和）
```

### 体积着色器

体积着色器定义网格的内部。材质可以只有体积着色器用于烟雾和火等情况，也可以与表面着色器结合用于浑浊玻璃等材质。

```python
# 体积材质类型
# 烟雾和火 - 仅体积着色器
# 浑浊玻璃 - 体积和表面着色器组合

# 体积特性
# 定义光线穿过介质时的行为
# 吸收和散射
# 密度分布
```

### 置换

表面的形状及其内部的体积可以通过置换改变。这样，纹理可以用来使网格表面更加详细。根据设置，置换可能是虚拟的，仅修改表面法线以给出置换的印象（称为凹凸映射），或真实和虚拟置换的组合。

```python
# 置换类型
# 真实置换 - 实际改变几何体
# 凹凸映射 - 仅修改法线
# 混合置换 - 结合两者

# 置换应用
# 增加表面细节
# 创建凹凸效果
# 无需增加几何体复杂度
```

## 着色节点系统

灵活的着色节点系统用于设置纹理和创建完全不同类型的材质，如卡通着色。

### 着色器节点

```python
# 着色器节点类型
Diffuse BSDF - 漫反射着色
Glossy BSDF - 光泽着色
Glass BSDF - 玻璃着色
Transmission BSDF - 透射着色
Emission - 发射
Background - 背景
Ambient Occlusion - 环境光遮蔽
Hair BSDF - 头发着色
Principled BSDF - 基于物理的BSDF
Principled Hair - 基于物理的头发
Principled Volume - 基于物理的体积
Mix Shader - 混合着色器
Add Shader - 添加着色器
```

### 颜色节点

```python
# 颜色操作节点
Mix - 混合颜色
Color Ramp - 颜色渐变
RGB Curves - RGB曲线
Hue Saturation Value - 色相饱和度值
Invert - 反转
Blackbody - 黑体
White Balance - 白平衡
Light Falloff - 光线衰减
Wavelength - 波长
Separate Color - 分离颜色
Combine Color - 合并颜色
```

### 纹理节点

```python
# 程序化纹理
Noise Texture - 噪声纹理
Voronoi Texture - Voronoi纹理
Wave Texture - 波纹纹理
Brick Texture - 砖块纹理
Checker Texture - 棋盘纹理
Gradient Texture - 渐变纹理
Magic Texture - 魔法纹理
Marble Texture - 大理石纹理
Wood Texture - 木纹纹理
Clouds Texture - 云纹理
Distorted Noise - 扭曲噪声
Stucci Texture - Stucci纹理
Image Texture - 图像纹理
```

### 输入节点

```python
# 几何数据输入
Texture Coordinate - 纹理坐标
Geometry - 几何信息
Point Info - 点信息
Barycentric Texture - 重心纹理
Object Info - 对象信息
Light Path - 光线路径
Fresnel - 菲涅尔
Layer Weight - 层权重
Value - 值
Camera Data - 相机数据
```

### 输出节点

```python
# 着色输出
Material Output - 材质输出
Light Output - 灯光输出
World Output - 世界输出
```

### 实用工具节点

```python
# 数学运算
Math - 数学运算
Vector Math - 向量运算
Clamp - 钳制
Map Range - 映射范围
```

## 基于物理的着色

材质系统是为基于物理的渲染而构建的，分离材质的外观和用于渲染它的算法。这使得更容易实现逼真的结果和平衡的照明，但有一些事项需要记住。

```python
# 能量守恒
# 为了使材质与全局照明良好配合
# 它们应该是能量守恒的
# 这意味着它们不能反射比入射更多的光

# 能量守恒规则
# 颜色值在 0.0 到 1.0 范围内
# BSDF 仅使用混合着色器节点混合在一起
# 这将自动为真

# 破坏能量守恒
# 使用高于 1.0 的颜色值
# 或使用添加着色器节点
# 必须小心以保持材质在各种照明条件下可预测地行为

# 最佳实践
# 避免使用 Add Shader 混合
# 保持颜色值在合理范围
# 使用 Mix Shader 进行组合
```

## 材质分配

材质可以分配给一个或多个对象，不同的材质可以分配给网格的不同部分。

```python
# 材质槽位
# 每个对象可以有多个材质槽位
# 材质按索引顺序应用
# 索引决定材质应用顺序

# 多材质分配
# 在编辑模式下选择面
# 在属性面板中分配材质
# 或使用顶点组控制分布

# 材质替代
# 使用材质替换节点
# 在着色器中切换材质
```

## Principled BSDF

Principled BSDF 是基于物理的渲染的标准着色器，用于创建大多数材质。

```python
# Principled BSDF 参数
Base Color - 基础颜色
Metallic - 金属度
Roughness - 粗糙度
IOR - 折射率
Alpha - 透明度
Normal - 法线
Clearcoat - 清漆层
Clearcoat Roughness - 清漆层粗糙度
Sheen - 光泽
Sheen Tint - 光泽色调
Specular - 高光
Specular Tint - 高光色调
Anisotropic - 各向异性
Anisotropic Rotation - 各向异性旋转
Transmission - 透射
Transmission Roughness - 透射粗糙度

# 典型材质设置
# 塑料：Metallic 0，Roughness 0.1-0.5
# 金属：Metallic 1，Roughness 0.1-0.3
# 玻璃：Metallic 0，Roughness 0，Transmission 1，IOR 1.45
# 皮肤：Metallic 0，Roughness 0.3-0.5，Subsurface 散射
```

## 光照设置

### 灯光类型

```python
# 灯光类型
Point Light - 点光源
Sun Light - 太阳光
Spot Light - 聚光灯
Area Light - 区域光

# 灯光参数
Power - 功率
Radius - 半径
Color - 颜色
Strength - 强度
```

### 灯光链接

```python
# 灯光链接
# 控制哪些灯光影响哪些对象
# 用于分离不同区域的照明
# 创建局部照明效果
```

## 渲染设置

### Cycles 渲染器

Cycles 是基于物理的光线追踪渲染器。

```python
# Cycles 核心设置
Samples - 采样数
Tile Size - 块大小
Device - 设备（CPU/GPU）

# 光线追踪设置
Max Bounces - 最大反弹
Diffuse Bounces - 漫反射反弹
Glossy Bounces - 光泽反射反弹
Transmission Bounces - 透射反弹
Volume Bounces - 体积反弹

# 降噪
Use Denoising - 使用降噪
Denoiser - 降噪器
```

### EEVEE 渲染器

EEVEE 是实时光栅化渲染器。

```python
# EEVEE 核心设置
Samples - 采样数
Shadow Type - 阴影类型

# 屏幕空间反射
SSR - 屏幕空间反射
SSR Half Res - 半分辨率

# 环境光遮蔽
GTAO - 地面真实环境光遮蔽
Distance - 距离
Factor - 因子

# 体积
Volumetric - 体积渲染
Grid Size - 网格大小
```

## 颜色管理

颜色管理控制如何处理渲染图像的颜色值。

```python
# 颜色空间
Linear - 线性颜色空间
sRGB - sRGB 颜色空间
Raw - 原始颜色空间

# 视图变换
Standard - 标准
Filmic - 电影级
Raw - 原始

# 色调映射
Exposure - 曝光
Gamma - 伽马
```

## 材质最佳实践

### 性能优化

```python
# 纹理优化
使用适当分辨率的纹理
使用压缩格式
考虑使用程序化纹理减少内存

# 着色器优化
避免过于复杂的节点树
使用节点组重用逻辑
限制透明层叠数量

# 渲染优化
使用适当的采样数
启用降噪
使用自适应采样
```

### 物理准确性

```python
# 能量守恒
避免使用 Add Shader
保持颜色值合理
使用物理准确的 IOR 值

# 材质一致性
使用一致的粗糙度和金属度
避免不自然的颜色组合
考虑光照条件下的表现
```

## 材质创建流程

```python
# 基础材质创建
# 1. 创建新材质
# 2. 选择基础着色器
# 3. 添加颜色和纹理
# 4. 设置物理参数
# 5. 测试渲染效果

# 纹理工作流程
# 1. 创建或加载纹理
# 2. 添加纹理坐标和映射
# 3. 使用纹理调制参数
# 4. 调整颜色和强度
# 5. 测试材质效果

# PBR 工作流程
# 1. 设置基础颜色
# 2. 添加法线/凹凸
# 3. 设置粗糙度贴图
# 4. 添加金属度贴图
# 5. 调整高光强度
# 6. 测试光照效果
```

## 体积渲染

体积渲染用于创建烟雾、火、雾等效果。

```python
# 体积着色器
Principled Volume - 基于物理的体积
Volume Absorption - 体积吸收
Volume Scatter - 体积散射

# 体积参数
Density - 密度
Color - 颜色
Anisotropy - 各向异性
Temperature - 温度

# 体积应用
烟雾效果
火焰效果
云层效果
大气效果
```

## 头发材质

头发材质用于渲染头发和毛发。

```python
# 头发着色器
Principled Hair - 基于物理的头发
Hair BSDF - 头发 BSDF

# 头发参数
Color - 颜色
Roughness - 粗糙度
Melanin - 黑色素
Radial Roughness - 径向粗糙度
IOR - 折射率
```

## 置换和法线

```python
# 置换方法
Displacement Node - 置换节点
Bump Node - 凹凸节点
Normal Map Node - 法线贴图节点

# 法线贴图
切线空间法线
对象空间法线
世界空间法线

# 凹凸贴图
高度图转换为法线
模拟表面细节
无需增加几何复杂度
```

## 材质库和工作流程

```python
# 材质库
Blender 附带材质库
在线材质资源
自定义材质库

# 材质重用
创建可重用材质
使用节点组
保持材质模块化

# 资产浏览器
将材质标记为资产
组织材质目录
快速访问常用材质
```

## 高级着色技术

### 次表面散射

```python
# 次表面散射
模拟光线穿透表面
散射到内部后退出
用于皮肤、蜡、大理石等

# SSS 参数
Radius - 散射半径
Scale - 缩放
Color - 颜色
```

### 清漆和涂层

```python
# 清漆效果
Clearcoat - 清漆层
Clearcoat Roughness - 清漆层粗糙度
用于车漆、漆木等
```

### 各向异性

```python
# 各向异性反射
Anisotropic - 各向异性
Anisotropic Rotation - 各向异性旋转
用于拉丝金属、丝绸等
```

### 卡通着色

```python
# 卡通效果
Shader to RGB - 着色器转RGB
使用颜色渐变进行色调分离
创建动漫风格效果
```


---
## bpy_uv_texture

# UV编辑与纹理映射模块

## UV映射概述

UV 编辑器用于编辑 UV 映射，描述 2D 图像如何映射到 3D 对象。

```python
# UV 映射定义
# 描述 2D 图像如何映射到 3D 对象
# 决定纹理如何包裹在模型上

# 为什么需要 UV 映射
# 当所需外观难以用程序化纹理实现时
# 或当纹理不均匀时
# 例如汽车只有在有划痕的地方有划痕
# 而不是随机分布在整个车身

# UV 名称来源
# U 代表水平轴
# V 代表垂直轴
# 选择这些字母是为了避免与 X 和 Y 混淆
# X 和 Y 指 3D 空间中的轴
```

## UV原理

### UV映射类比

理解 UV 映射的最佳类比是剪开一个纸板盒。如果您拿起剪刀沿边缘剪开，您就可以将其平铺在桌面上。当您向下看桌子时，我们可以说 U 是左右方向，V 是上下方向。

```python
# 剪开纸盒类比
# 沿边缘剪开
# 平铺在桌面上
# U 是左右方向
# V 是上下方向

# 下一步
# 将展开的盒子放在海报上
# 剪下与盒子形状匹配的海报
# 将海报粘到盒子上
# 最后重新组装盒子
# 得到一个用 2D 图像纹理化的 3D 盒子

# UV 映射描述
# 描述网格的面如何在纹理上布局
# 可以完全控制映射过程
# 可以单独切割、移动、旋转、缩放每个面
# 甚至可以独立扭曲它们
# 面可以在 UV 映射中重叠
```

### UV空间

```python
# 3D 空间与 UV 空间
# 3D 空间使用 XYZ 轴
# UV 空间使用 UV 轴

# 示例
# 3D 空间中的圆顶
# 在 UV 空间中展平为圆盘
# 每个 3D 面用其在 UV 映射中覆盖的图像部分进行纹理化

# 常见问题
# UV 映射中的失真
# 2D 纹理中的方格大小都相同
# 但在应用到 3D 圆顶时大小不同
# 底部比顶部小
# 这是因为 UV 映射中的面
# 与 3D 空间中的相对大小不同

# 解决方案
# 通过手动引导和调整展平过程
# 使用接缝来最小化失真
# 但并非总是能完全消除
```

## UV编辑器界面

### 头部工具栏

```python
# 同步选择
# 在 UV 编辑器和 3D 视口之间同步选择
# 允许在一个编辑器中选择
# 自动反映在另一个编辑器中

# 选择模式
# 要选择的 UV 元素类型
# 顶点选择
# 边选择
# 面选择

# 粘性选择模式
# 自动选择其他哪些顶点
# 禁用 - 不自动选择
# 共享顶点 - 选择共享顶点的所有面
# 共享位置 - 选择位置相同的所有顶点
# 连锁 - 选择相连的图岛

# 查看菜单
# 控制内容在编辑器中的显示方式

# 选择菜单
# 选择 UV 的工具

# 图像菜单
# 打开和操作图像的工具

# UV 菜单
# 包含展开网格和编辑 UV 的工具

# 中心点
# 控制变换的中心点
```

### 图像显示

```python
# 图像选择
# 数据块菜单用于选择图像
# 当图像在 UV 编辑器中加载或创建时
# 侧边栏中出现图像面板

# 图像固定
# 启用时当前图像保持可见
# 无论对象选择如何
# 仅在 3D 视口处于编辑模式或纹理绘制模式时发生切换

# 显示通道
# 选择显示哪些颜色通道
# 颜色和 Alpha - 启用透明度
# 在图像后显示棋盘格
# 颜色 - 显示彩色图像
# 不带 alpha 通道
# Alpha - 将 alpha 通道显示为灰度图像
# 白色区域不透明
# 黑色区域透明
# Z 缓冲区 - 显示 Z 缓冲区
```

## UV展开方法

### 展开

```python
# 快捷键 U
# 在 3D 视口或 UV 编辑器中

# 功能
# 沿接缝切割选中的面
# 将其展平
# 并在 UV 映射上布局
# 之前存在的 UV 坐标被覆盖
# 适用于有机形状

# 方法选项
# Angle Based - 使用基于角度展平 (ABF)
# 此方法给出网格的良好 2D 表示
# Conformal - 使用最小二保角映射 (LSCM)
# 通常不如基于角度的 UV 映射准确
# 但在简单对象上表现更好
# Minimum Stretch - 使用可扩展局部单射映射 (SLIM)
# 尝试最小化面积和角度的失真

# 填充孔洞
# 在展开之前虚拟填充网格中的孔洞
# 以更好地避免重叠并保持对称

# 使用细分曲面
# 使用由细分曲面修饰符计算的新顶点位置
# 而非运行任何修饰符之前的原始位置

# 修正宽高比
# 调整 UV 映射以考虑与材质关联的图像的宽高比
# 确保 UV 在非方形纹理上展开时正确缩放
```

### 智能UV投影

```python
# 快捷键 U
# 检查所选面之间的角度
# 沿任何锐利边缘切割
# 然后沿其平均法线投影每组分离的面
# 并在 UV 映射上布局
# 适用于机械对象或建筑

# 角度限制
# 邻面法线之间的最大允许角度
# 低于该角度限制时
# 面会彼此分开
# 低限制将创建许多小的 UV 图岛
# 高限制将创建一些大的图岛

# 旋转方法
# 轴对齐 - 自动旋转以避免浪费空间
# 轴对齐（水平）- 旋转图岛使其水平对齐
# 轴对齐（垂直）- 旋转图岛使其垂直对齐

# 区域权重
# 值为 0 时
# 每组面的投影向量就是其面法线的平均值
# 值为 1 时
# 使用面区域加权的平均值
# 其他值混合两者
```

### 立方体投影

```python
# 投影方法
# 从多个方向投影
# 将网格投影到立方体上
# 然后展开立方体面

# 适用场景
# 简单几何体
# 机械零件
# 建筑元素
```

### 圆柱体投影

```python
# 投影方法
# 将网格投影到圆柱体上
# 沿 Y 轴展开

# 适用场景
# 圆柱形物体
# 管道
# 瓶子和罐子
```

### 球体投影

```python
# 投影方法
# 将网格投影到球体上
# 类似于地球仪

# 适用场景
# 球形物体
# 行星
# 头部（粗略）
```

### 展开接缝

```python
# 接缝定义
# 接缝是您切割网格以创建 UV 展开的地方
# 沿接缝切割网格
# 然后展平

# 创建接缝
# 在编辑模式下选择边
# 标记为接缝

# 切割边
# 边被标记为接缝
# 在 UV 展开期间会沿这些边切割

# 优化接缝
# 将接缝放置在不明显的区域
# 沿着自然边界
# 沿着锐利边缘
```

## UV编辑工具

### 选择工具

```python
# 选择所有
# 选择所有 UV

# 选择无
# 取消选择所有

# 反选
# 反转选择

# 选择相似
# 选择具有相似属性的 UV
# 例如相似大小或方向

# 选择连通
# 选择相连的 UV 元素

# 框选
# 拖拽框选区域

# 圈选
# 绘制选择区域
```

### 变换工具

```python
# 移动
# 移动选中的 UV
# 按 G 键或使用手柄

# 旋转
# 旋转选中的 UV
# 按 R 键或使用手柄

# 缩放
# 缩放选中的 UV
# 按 S 键或使用手柄

# 旋转 90 度
# 顺时针或逆时针旋转 90 度

# 水平/垂直镜像
# 镜像选中的 UV

# 对齐
# 对齐 UV 到网格
# 对齐到光标
# 对齐到选区
```

### 对齐和分布

```array
# 排列
# 水平分布 UV
# 垂直分布 UV
# 排序 UV

# 对齐
# 左对齐
# 右对齐
# 顶部对齐
# 底部对齐
# 水平和垂直居中

# 调整大小
# 匹配 UV 大小
# 缩放到边界框
# 平均缩放到边界框
```

### 特殊操作

```python
# 焊接
# 将选中的 UV 顶点合并

# 缝合
# 将 UV 边缝合在一起

# 移动
# 移动 UV 到活动面

# 交换
# 交换 UV

# 最小化拉伸
# 减少 UV 拉伸

# 修正拉伸
# 显示拉伸热图
# 减少拉伸
```

## UV映射属性

### UV地图属性

```python
# UV 地图名称
# 为 UV 映射命名
# 支持多个 UV 映射

# 活动渲染
# 将此 UV 映射设为用于渲染

# 活动显示
# 将此 UV 映射显示在视口中

# 自动平滑
# 自动平滑 UV 边缘
# 减少接缝可见性

# 切线层
# 为法线贴图选择切线层
```

### 纹理空间

```python
# 纹理空间
# UV 映射占据的空间
# 定义 UV 映射的边界

# 位置
# 纹理空间的位置偏移

# 大小
# 纹理空间的大小

# 自动纹理空间
# 自动计算纹理空间

# 编辑纹理空间
# 手动调整纹理空间
```

## 纹理映射

### 纹理坐标

```python
# 纹理坐标类型
# UV - 使用 UV 映射坐标
# 对象 - 使用对象空间坐标
# 世界 - 使用世界空间坐标
# 切线 - 使用切线空间坐标
# 生成 - 使用生成坐标

# 坐标节点
# 纹理坐标节点提供各种坐标
# 可以连接到映射节点
# 然后连接到纹理节点
```

### 映射节点

```python
# 映射变换
# 移动、旋转、缩放纹理

# 向量旋转
# 旋转纹理向量

# 曲率
# 计算曲率数据

# 最小最大
# 计算最小和最大坐标值
```

### 图像纹理

```python
# 图像纹理节点
# 加载图像文件作为纹理
# 支持多种图像格式

# 图像属性
# 源 - 图像来源
# 颜色空间 - 颜色空间设置
# Alpha - Alpha 处理方式

# 插值
# 线性 - 平滑插值
# 最近邻 - 像素化
# 立方体 - 平滑但较慢
# 四元 - 平滑但更慢
```

## 纹理绘制

### 纹理绘制模式

```python
# 进入纹理绘制
# 在对象模式中选择对象
# 切换到纹理绘制模式

# 要求
# 对象必须有材质
# 材质必须有图像纹理节点
# UV 映射必须正确

# 笔刷设置
# 笔刷大小
# 笔刷硬度
# 笔刷不透明度
# 笔刷颜色
```

### 笔刷和纹理

```python
# 笔刷类型
# 绘制 - 添加颜色
# 模糊 - 模糊区域
# 涂抹 - 涂抹颜色
# 克隆 - 克隆区域
# 橡皮擦 - 移除颜色

# 纹理槽
# 在纹理空间中放置纹理
# 控制绘制纹理的位置

# 纹理混合
# 混合模式
# 混合强度
```

### 遮罩和投影

```python
# 投影绘制
# 从图像投影绘制
# 使用纹理图像作为参考

# 屏幕投影
# 从屏幕空间投影
# 适合详细工作

# 视图投影
# 从当前视图投影
```

## 最佳实践

### UV规划

```python
# 规划 UV
# 在开始之前考虑最终纹理
# 确定接缝放置
# 规划图岛布局

# 纹理分辨率
# 考虑最终纹理分辨率
# 高分辨率模型需要更多 UV 空间

# 图岛数量
# 平衡图岛数量和失真
# 更多图岛通常意味着更少失真
# 但更复杂的 UV 布局

# 保持比例
# 尽量保持 UV 比例与 3D 表面一致
# 减少纹理拉伸
# 提高纹理质量
```

### 优化纹理

```python
# 纹理空间利用
# 最大化纹理空间利用
# 避免浪费空间
# 填充整个 UV 空间

# 像素密度
# 保持一致的像素密度
# 避免某些区域像素过度集中
# 其他区域像素稀疏

# 避免重叠
# 避免 UV 重叠
# 除非有意共享纹理

# 接缝处理
# 将接缝放在不明显的区域
# 沿自然边界放置
# 最小化接缝可见性
```

### 工作流程

```python
# 迭代工作流
# 从粗略展开开始
# 调整接缝和位置
# 逐步细化

# 保存版本
# 保存不同阶段的版本
# 记录更改
# 便于回退

# 测试纹理
# 应用测试纹理
# 检查拉伸和不一致
# 调整 UV 直到满意
```

## Python脚本

### 创建UV映射

```python
# 创建 UV 映射
import bpy
obj = bpy.context.active_object

# 创建新的 UV 映射
uv_map = obj.data.uv_layers.new(name="UVMap")

# 访问 UV 数据
uv_data = obj.data.uv_layers[0].data

# 遍历 UV 坐标
for i, uv in enumerate(uv_data):
    print(f"顶点 {i}: u={uv.vector.x}, v={uv.vector.y}")

# 设置 UV 坐标
for i, uv in enumerate(uv_data):
    uv.vector.x = i * 0.1
    uv.vector.y = i * 0.1
```

### UV操作

```python
# 展开网格
import bpy
obj = bpy.context.active_object

# 确保在编辑模式
bpy.ops.object.mode_set(mode='EDIT')

# 选择所有面
bpy.ops.mesh.select_all(action='SELECT')

# 智能投影
bpy.ops.uv.smart_project(
    angle_limit=1.0,
    island_margin=0.03,
    area_weight=0.0,
    correct_aspect=True
)

# 展开
bpy.ops.uv.unwrap(
    method='ANGLE_BASED',
    fill_holes=True,
    correct_aspect=True
)
```

### 批量处理

```python
# 批量设置 UV
import bpy

for obj in bpy.context.selected_objects:
    if obj.type == 'MESH':
        bpy.context.view_layer.objects.active = obj
        
        # 切换到编辑模式
        bpy.ops.object.mode_set(mode='EDIT')
        
        # 选择所有
        bpy.ops.mesh.select_all(action='SELECT')
        
        # 展开
        bpy.ops.uv.unwrap(method='ANGLE_BASED')
        
        # 切换回对象模式
        bpy.ops.object.mode_set(mode='OBJECT')
```

### UV分析

```python
# 分析 UV
import bpy
import math

obj = bpy.context.active_object
uv_layer = obj.data.uv_layers.active

# 计算 UV 面积
total_area = 0.0
for poly in obj.data.polygons:
    # 获取 UV 坐标
    uv_coords = []
    for loop_index in poly.loop_indices:
        uv = uv_layer.data[loop_index].uv
        uv_coords.append(uv)
    
    # 计算多边形面积（简化）
    area = poly.area
    total_area += area

print(f"UV 映射面积: {total_area:.4f}")

# 检查拉伸
max_stretch = 0.0
min_stretch = 1.0

# 遍历 UV 顶点
for uv in uv_layer.data:
    stretch = abs(uv.vector.x - uv.vector.y)
    max_stretch = max(max_stretch, stretch)
    min_stretch = min(min_stretch, stretch)

print(f"最大拉伸: {max_stretch:.4f}")
print(f"最小拉伸: {min_stretch:.4f}")
```

## 常见问题解决

### 拉伸问题

```python
# UV 拉伸
# 检查拉伸热图
# 红色表示高拉伸
# 蓝色表示低拉伸

# 减少拉伸
# 添加更多接缝
# 调整 UV 布局
# 使用智能投影
# 手动调整 UV 顶点

# 使用修正拉伸
# 使用最小化拉伸展开方法
# 增加迭代次数
# 使用权重控制区域大小
```

### 接缝问题

```python
# 接缝可见
# 检查 UV 接缝位置
# 将接缝移到不明显的区域
# 增加纹理分辨率
# 使用更好的纹理过滤

# 纹理不连续
# 确保 UV 边缘对齐
# 匹配相邻面的 UV 坐标
# 使用接缝工具焊接 UV
```

### 映射问题

```python
# 纹理重复
# 检查 UV 缩放
# 确保 UV 填满整个纹理空间
# 使用纹理比例调整

# 纹理倒置
# 检查 UV 方向
# 翻转 UV 坐标
# 检查镜像设置

# 纹理丢失
# 确保 UV 映射存在
# 选择正确的 UV 映射
# 检查纹理图像是否加载
```


