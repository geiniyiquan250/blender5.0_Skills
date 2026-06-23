# bpy_ops_render_material

---
## bpy_ops_render

# bpy.ops.render 模块

渲染操作模块，用于控制渲染设置和执行渲染操作。

## 概述

bpy.ops.render 模块包含用于设置渲染参数、执行渲染和查看渲染结果的运算符。这些运算符控制渲染引擎、输出设置和渲染过程。

## 常用操作

### 渲染执行

```python
import bpy

# 渲染当前帧
bpy.ops.render.render(write_still=False)

# 渲染动画
bpy.ops.render.render(animation=True, write_still=False)

# 渲染区域
bpy.ops.render.render(use_area=False)

# 背景渲染
bpy.ops.render.render(background=True)

# 取消渲染
bpy.ops.render.cancel()

# 查看渲染
bpy.ops.render.view_show()
```

### 渲染设置

```python
# 引擎选择
bpy.ops.render.engine_set(engine='CYCLES')
bpy.ops.render.engine_set(engine='BLENDER_EEVEE')

# 设置渲染分辨率
bpy.ops.render.render()

# 设置采样
bpy.ops.render.render()
```

### 视口渲染

```python
# 渲染视口
bpy.ops.render.opengl()

# 渲染视口动画
bpy.ops.render.opengl(animation=True)

# 渲染视口并保存
bpy.ops.render.opengl(write_still=True)

# 渲染视口区域
bpy.ops.render.opengl(view_context=False)
```

### 渲染层设置

```python
# 切换渲染层
bpy.ops.render.render_set()

# 设置渲染层
bpy.ops.render.render_set(layer='RenderLayer')

# 清除渲染层
bpy.ops.render.render_clear()
```

### 渲染 Pass 设置

```python
# 切换Pass
bpy.ops.render.render_set()

# 添加Pass
bpy.ops.render.render_set()

# 移除Pass
bpy.ops.render.render_set()
```

### 图像操作

```python
# 打开渲染结果
bpy.ops.render.read_render()

# 保存渲染
bpy.ops.render.render()

# 清除渲染结果
bpy.ops.render.render_clear()

# 重新加载渲染结果
bpy.ops.render.read_render()
```

### 动画播放

```python
# 播放渲染预览
bpy.ops.render.play_rendered_anim()

# 暂停播放
bpy.ops.render.play_rendered_anim()
```

## 实际应用示例

### 创建标准渲染设置

```python
def setup_standard_render():
    """设置标准渲染参数"""
    scene = bpy.context.scene
    
    # 设置渲染引擎
    scene.render.engine = 'CYCLES'
    
    # 设置分辨率
    scene.render.resolution_x = 1920
    scene.render.resolution_y = 1080
    scene.render.resolution_percentage = 100
    
    # 设置帧范围
    scene.frame_start = 1
    scene.frame_end = 250
    scene.render.fps = 24
    
    # 设置输出格式
    scene.render.image_settings.file_format = 'PNG'
    scene.render.filepath = '//renders/frame_'
    
    return scene
```

### 创建批量渲染

```python
def batch_render_scenes(scene_files, output_dir):
    """批量渲染多个场景"""
    import os
    
    for scene_file in scene_files:
        # 打开场景
        bpy.ops.wm.open_mainfile(filepath=scene_file)
        
        # 获取场景名称
        scene_name = os.path.splitext(os.path.basename(scene_file))[0]
        
        # 设置输出路径
        bpy.context.scene.render.filepath = os.path.join(
            output_dir,
            f'{scene_name}_'
        )
        
        # 执行渲染
        bpy.ops.render.render(
            animation=True,
            write_still=False
        )
```

### 创建分层渲染

```python
def setup分层_render(layers_config):
    """设置分层渲染"""
    scene = bpy.context.scene
    
    # 启用分层渲染
    scene.render.use_multilayer = True
    
    # 配置渲染层和Pass
    for layer_name, passes in layers_config.items():
        if layer_name not in scene.render.layers:
            continue
            
        render_layer = scene.render.layers[layer_name]
        
        # 设置Pass
        render_layer.use_pass_combined = 'COMBINED' in passes
        render_layer.use_pass_z = 'DEPTH' in passes
        render_layer.use_pass_normal = 'NORMAL' in passes
        render_layer.use_pass_diffuse = 'DIFFUSE' in passes
        render_layer.use_pass_glossy = 'GLOSSY' in passes
        render_layer.use_pass_emit = 'EMIT' in passes
        render_layer.use_pass_shadow = 'SHADOW' in passes
    
    return scene.render
```

### 创建渲染队列

```python
def create_render_queue(render_tasks):
    """创建渲染队列"""
    for task in render_tasks:
        scene_name = task['scene']
        output_path = task['output']
        frame_start = task.get('start', 1)
        frame_end = task.get('end', 250)
        
        # 设置帧范围
        bpy.context.scene.frame_start = frame_start
        bpy.context.scene.frame_end = frame_end
        
        # 设置输出
        bpy.context.scene.render.filepath = output_path
        
        # 渲染
        bpy.ops.render.render(
            animation=True,
            write_still=False
        )
```

### 创建高分辨率渲染

```python
def setup_high_resolution_render(width=8192, height=8192, samples=500):
    """设置高分辨率渲染"""
    scene = bpy.context.scene
    
    # 设置分辨率
    scene.render.resolution_x = width
    scene.render.resolution_y = height
    
    # 设置采样
    scene.cycles.samples = samples
    scene.cycles.adaptive_threshold = 0.01
    
    # 设置Tiles
    scene.cycles.tile_size = 2048
    
    # 启用GPU渲染
    scene.cycles.device = 'GPU'
    
    return scene
```

### 创建Web渲染

```python
def setup_web_render():
    """设置网页渲染参数"""
    scene = bpy.context.scene
    
    # 设置适合Web的分辨率
    scene.render.resolution_x = 1920
    scene.render.resolution_y = 1080
    
    # 设置Web格式
    scene.render.image_settings.file_format = 'WEBP'
    scene.render.image_settings.webp_codec = 'LOSSY'
    scene.render.image_settings.quality = 90
    
    # 优化透明度
    scene.render.image_settings.exr_codec = 'DWAA'
    
    return scene
```

### 创建动画渲染

```python
def setup_animation_render(output_path, start_frame=1, end_frame=250):
    """设置动画渲染"""
    scene = bpy.context.scene
    
    # 设置帧范围
    scene.frame_start = start_frame
    scene.frame_end = end_frame
    
    # 设置输出格式
    scene.render.image_settings.file_format = 'FFMPEG'
    scene.render.ffmpeg.format = 'MPEG4'
    scene.render.ffmpeg.codec = 'H264'
    scene.render.ffmpeg.constant_rate_factor = 'HIGH'
    
    # 设置输出路径
    scene.render.filepath = output_path
    
    # 设置音频（如果有）
    scene.render.ffmpeg.audio_codec = 'AAC'
    
    return scene
```

### 创建实时预览渲染

```python
def setup_realtime_preview():
    """设置实时预览渲染"""
    scene = bpy.context.scene
    
    # 使用Eevee引擎
    scene.render.engine = 'BLENDER_EEVEE'
    
    # 优化实时性能
    scene.eevee.taa_render_samples = 16
    scene.eevee.use_gtao = True
    scene.eevee.use_bloom = True
    scene.eevee.use_ssr = True
    
    # 降低采样
    scene.eevee.samples = 32
    
    # 启用快速GI
    scene.eevee.use_gtao = True
    
    return scene
```

## 使用场景

- **静态渲染**：渲染单帧图像
- **动画渲染**：渲染动画序列
- **分层渲染**：分离不同渲染元素
- **批量渲染**：处理多个渲染任务
- **实时预览**：快速预览效果
- **高分辨率**：输出大型图像

## 注意事项

- 高采样增加渲染时间
- 高分辨率需要更多内存
- 动画渲染需要足够磁盘空间
- GPU渲染需要兼容设备
- 分层渲染增加处理时间
- 取消渲染可能留下临时文件


---
## bpy_ops_render_animation

# bpy.ops.render - 动画渲染操作模块

Blender动画渲染操作符，用于渲染动画序列和视频输出。

## 概述

`bpy.ops.render.render` 模块提供用于在Blender中渲染动画的功能，支持序列帧渲染、视频编码输出和批量渲染操作。

## 核心功能

### 基础渲染

```python
import bpy

# 渲染动画
bpy.ops.render.render(
    animation=True,
    write_still=False,
    use_viewport=False,
    endframe=None,
    frame=None,
    filepath=None
)

# 渲染当前帧
bpy.ops.render.render(
    animation=False,
    write_still=True,
    use_viewport=False
)

# 渲染到视图
bpy.ops.render.render(
    animation=False,
    write_still=False,
    use_viewport=True
)
```

### 渲染设置

```python
# 渲染动画
bpy.ops.render.render(
    animation=True
)

# 渲染静帧
bpy.ops.render.render(
    animation=False,
    write_still=True
)

# 使用视口渲染
bpy.ops.render.render(
    animation=False,
    write_still=False,
    use_viewport=True
)
```

### 帧控制

```python
# 渲染指定帧
bpy.ops.render.render(
    animation=False,
    frame=50
)

# 渲染结束帧
bpy.ops.render.render(
    animation=True,
    endframe=100
)
```

### 输出路径

```python
# 设置输出路径渲染
bpy.ops.render.render(
    animation=True,
    filepath='//render/animation_'
)
```

### 视口渲染选项

```python
# 视口渲染设置
bpy.context.scene.render.use_lock_interface = False
bpy.context.scene.render.use_placeholder = False

# 渲染所有区域
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        space = area.spaces.active
        space.shading.type = 'RENDERED'
```

## 实用函数

```python
def render_animation(output_path='//render/'):
    """渲染动画"""
    bpy.context.scene.render.filepath = output_path
    bpy.ops.render.render(animation=True)

def render_current_frame():
    """渲染当前帧"""
    bpy.ops.render.render(animation=False, write_still=True)

def render_to_view():
    """渲染到视图"""
    bpy.ops.render.render(animation=False, write_still=False, use_viewport=True)

def render_frame_range(start, end, output_path='//render/'):
    """渲染帧范围"""
    bpy.context.scene.frame_start = start
    bpy.context.scene.frame_end = end
    bpy.context.scene.render.filepath = output_path
    bpy.ops.render.render(animation=True)

def render_with_settings(
    output_path='//render/',
    engine='CYCLES',
    samples=128,
    resolution=(1920, 1080),
    start_frame=1,
    end_frame=250
):
    """使用指定设置渲染"""
    scene = bpy.context.scene
    scene.render.engine = engine
    scene.render.resolution_x = resolution[0]
    scene.render.resolution_y = resolution[1]
    scene.render.filepath = output_path
    scene.frame_start = start_frame
    scene.frame_end = end_frame
    
    if engine == 'CYCLES':
        scene.cycles.samples = samples
    
    bpy.ops.render.render(animation=True)

def batch_render_scenes(scenes, output_dir='//render/'):
    """批量渲染场景"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    original_scene = bpy.context.scene.name
    
    for scene_name in scenes:
        bpy.context.window.scene = bpy.data.scenes[scene_name]
        scene = bpy.context.scene
        
        output_path = os.path.join(output_dir, f'{scene_name}_')
        scene.render.filepath = output_path
        
        bpy.ops.render.render(animation=True)
    
    # 恢复原始场景
    bpy.context.window.scene = bpy.data.scenes[original_scene]

def render_eevee_quick(output_path='//render/'):
    """快速EEVEE渲染"""
    scene = bpy.context.scene
    scene.render.engine = 'BLENDER_EEVEE'
    scene.render.filepath = output_path
    scene.eevee.taa_render_samples = 32
    bpy.ops.render.render(animation=True)

def render_cycles_high_quality(output_path='//render/', samples=256):
    """高质量Cycles渲染"""
    scene = bpy.context.scene
    scene.render.engine = 'CYCLES'
    scene.render.filepath = output_path
    scene.cycles.samples = samples
    scene.cycles.adaptive_threshold = 0.01
    scene.cycles.time_limit = 300  # 5分钟限制
    bpy.ops.render.render(animation=True)
```

## 常见问题排查

### 渲染不工作
```python
# 检查渲染设置
print(f"渲染引擎: {bpy.context.scene.render.engine}")
print(f"分辨率: {bpy.context.scene.render.resolution_x} x {bpy.context.scene.render.resolution_y}")

# 检查输出路径
print(f"输出路径: {bpy.context.scene.render.filepath}")

# 检查帧范围
print(f"帧范围: {bpy.context.scene.frame_start} - {bpy.context.scene.frame_end}")

# 确保输出目录存在
import os
output_dir = os.path.dirname(bpy.context.scene.render.filepath)
if not os.path.exists(output_dir):
    os.makedirs(output_dir)
```

### 渲染空白
```python
# 检查渲染层
print(f"活跃场景: {bpy.context.scene.name}")

# 检查对象可见性
for obj in bpy.context.scene.objects:
    if obj.type == 'MESH':
        print(f"{obj.name}: 隐藏={obj.hide_render}, 可见={obj.visible_get()}")

# 确保对象可见
for obj in bpy.context.scene.objects:
    obj.hide_render = False

# 检查相机
print(f"活动相机: {bpy.context.scene.camera}")

# 设置相机
if bpy.context.scene.camera is None:
    for obj in bpy.context.scene.objects:
        if obj.type == 'CAMERA':
            bpy.context.scene.camera = obj
            break
```

### 动画问题
```python
# 检查动画数据
print(f"帧数: {bpy.context.scene.frame_end - bpy.context.scene.frame_start + 1}")

# 检查帧速率
print(f"帧速率: {bpy.context.scene.render.fps}")

# 检查帧步长
print(f"帧步长: {bpy.context.scene.render.frame_step}")

# 设置正确的帧范围
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 250
```

### 性能问题
```python
# 检查采样数
if bpy.context.scene.render.engine == 'CYCLES':
    print(f"采样数: {bpy.context.scene.cycles.samples}")

# 减少采样数
bpy.context.scene.cycles.samples = 64

# 启用降噪
bpy.context.scene.cycles.use_denoising = True

# 使用GPU渲染
bpy.context.scene.cycles.device = 'GPU'

# 检查显存
print(f"显存: {bpy.context.preferences.addons['cycles'].preferences.get_compute_device_type()}")
```

### 输出格式问题
```python
# 检查输出格式
print(f"输出格式: {bpy.context.scene.render.image_settings.file_format}")

# 设置输出格式
bpy.context.scene.render.image_settings.file_format = 'FFMPEG'
bpy.context.scene.render.ffmpeg.format = 'MPEG4'
bpy.context.scene.render.ffmpeg.codec = 'H264'

# 检查视频编码
print(f"视频编码: {bpy.context.scene.render.ffmpeg.codec}")

# 设置质量
bpy.context.scene.render.ffmpeg.video_bitrate = 8000
```

### 渲染时间过长
```python
# 检查渲染时间
import time
start_time = time.time()
bpy.ops.render.render(animation=True)
end_time = time.time()
print(f"渲染时间: {end_time - start_time} 秒")

# 减少分辨率
bpy.context.scene.render.resolution_percentage = 50

# 减少采样数
bpy.context.scene.cycles.samples = 32

# 跳过帧
bpy.context.scene.render.frame_step = 2
```


---
## bpy_ops_render_opengl

# bpy.ops.render - OpenGL渲染操作模块

Blender OpenGL渲染操作符，用于使用OpenGL引擎进行快速渲染。

## 概述

`bpy.ops.render.opengl` 模块提供用于使用OpenGL引擎进行快速渲染的功能，支持图像和动画的快速输出。

## 核心功能

### 基础OpenGL渲染

```python
import bpy

# 渲染当前帧
bpy.ops.render.opengl(
    filepath='//render.png',
    filter_blender=False,
    filter_image=False,
    filter_movie=False,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    view_context=True,
    write_still=False,
    bake=False,
    use_antialiasing=True,
    use_compositing=False,
    use_sequencer=False,
    use_viewer=False,
    show_full_view=False
)
```

### 渲染设置

```python
# 渲染并保存
bpy.ops.render.opengl(
    filepath='//render.png',
    write_still=True
)

# 不保存
bpy.ops.render.opengl(
    filepath='//render.png',
    write_still=False
)

# 使用抗锯齿
bpy.ops.render.opengl(
    filepath='//render.png',
    use_antialiasing=True
)

# 不使用抗锯齿
bpy.ops.render.opengl(
    filepath='//render.png',
    use_antialiasing=False
)
```

### 合成设置

```python
# 使用合成
bpy.ops.render.opengl(
    filepath='//composited.png',
    use_compositing=True
)

# 不使用合成
bpy.ops.render.opengl(
    filepath='//render.png',
    use_compositing=False
)
```

### 序列器设置

```python
# 使用序列器
bpy.ops.render.opengl(
    filepath='//with_sequencer.png',
    use_sequencer=True
)

# 不使用序列器
bpy.ops.render.opengl(
    filepath='//render.png',
    use_sequencer=False
)
```

### 视图器设置

```python
# 使用视图器
bpy.ops.render.opengl(
    filepath='//viewer.png',
    use_viewer=True
)

# 不使用视图器
bpy.ops.render.opengl(
    filepath='//render.png',
    use_viewer=False
)
```

### 全视图

```python
# 使用全视图
bpy.ops.render.opengl(
    filepath='//full_view.png',
    show_full_view=True
)

# 不使用全视图
bpy.ops.render.opengl(
    filepath='//render.png',
    show_full_view=False
)
```

### 烘焙设置

```python
# 启用烘焙
bpy.ops.render.opengl(
    filepath='//baked.png',
    bake=True
)

# 禁用烘焙
bpy.ops.render.opengl(
    filepath='//render.png',
    bake=False
)
```

### 视图上下文

```python
# 使用当前视图
bpy.ops.render.opengl(
    filepath='//render.png',
    view_context=True
)

# 不使用当前视图
bpy.ops.render.opengl(
    filepath='//render.png',
    view_context=False
)
```

## 实用函数

```python
def opengl_render_frame(filepath, resolution=1.0):
    """OpenGL渲染单帧"""
    bpy.context.scene.render.resolution_percentage = int(resolution * 100)
    bpy.ops.render.opengl(
        filepath=filepath,
        write_still=True,
        use_antialiasing=True,
        use_compositing=True,
        use_sequencer=False
    )

def opengl_render_quick(filepath):
    """快速OpenGL渲染"""
    bpy.ops.render.opengl(
        filepath=filepath,
        write_still=True,
        use_antialiasing=False,
        use_compositing=False,
        use_sequencer=False
    )

def opengl_render_composited(filepath):
    """OpenGL渲染带合成"""
    bpy.ops.render.opengl(
        filepath=filepath,
        write_still=True,
        use_antialiasing=True,
        use_compositing=True,
        use_sequencer=False
    )

def opengl_render_sequencer(filepath):
    """OpenGL渲染带序列器"""
    bpy.ops.render.opengl(
        filepath=filepath,
        write_still=True,
        use_antialiasing=True,
        use_compositing=False,
        use_sequencer=True
    )

def opengl_render_high_quality(filepath):
    """高质量OpenGL渲染"""
    bpy.ops.render.opengl(
        filepath=filepath,
        write_still=True,
        use_antialiasing=True,
        use_compositing=True,
        use_sequencer=False,
        view_context=True
    )

def opengl_render_animation(start, end, output_dir):
    """OpenGL渲染动画"""
    import os
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    scene = bpy.context.scene
    original_frame = scene.frame_current
    
    for frame in range(start, end + 1):
        scene.frame_set(frame)
        filepath = os.path.join(output_dir, f'frame_{frame:04d}.png')
        bpy.ops.render.opengl(
            filepath=filepath,
            write_still=True,
            use_antialiasing=True
        )
    
    scene.frame_set(original_frame)
```

## 常见问题排查

### 渲染空白
```python
# 检查渲染设置
print(f"渲染引擎: {bpy.context.scene.render.engine}")
print(f"分辨率: {bpy.context.scene.render.resolution_x} x {bpy.context.scene.render.resolution_y}")

# 设置正确的引擎
bpy.context.scene.render.engine = 'BLENDER_EEVEE'

# 检查渲染层
print(f"活跃场景: {bpy.context.scene.name}")

# 确保视图可见
for area in bpy.context.screen.areas:
    if area.type == 'VIEW_3D':
        space = area.spaces.active
        space.shading.type = 'SOLID'
```

### 抗锯齿问题
```python
# 检查抗锯齿设置
print(f"抗锯齿: {bpy.context.scene.render.use_antialiasing}")

# 启用抗锯齿
bpy.context.scene.render.use_antialiasing = True

# 设置采样数
bpy.context.scene.eevee.taa_render_samples = 16

# 重新渲染
bpy.ops.render.opengl(
    filepath='//aa_render.png',
    use_antialiasing=True
)
```

### 合成不工作
```python
# 检查合成节点
print(f"合成节点: {bpy.context.scene.node_tree}")

if bpy.context.scene.node_tree:
    for node in bpy.context.scene.node_tree.nodes:
        print(f"节点: {node.type}, 名称: {node.name}")

# 检查合成使用
print(f"使用合成: {bpy.context.scene.render.use_compositing}")

# 启用合成
bpy.context.scene.render.use_compositing = True

# 重新渲染
bpy.ops.render.opengl(
    filepath='//composite.png',
    use_compositing=True
)
```

### 序列器问题
```python
# 检查序列器
print(f"序列器: {bpy.context.scene.sequence_editor}")

if bpy.context.scene.sequence_editor:
    for strip in bpy.context.scene.sequence_editor.sequences:
        print(f"条带: {strip.name}, 类型: {strip.type}")

# 检查序列器使用
print(f"使用序列器: {bpy.context.scene.render.use_sequencer}")

# 启用序列器
bpy.context.scene.render.use_sequencer = True

# 重新渲染
bpy.ops.render.opengl(
    filepath='//sequencer.png',
    use_sequencer=True
)
```

### 性能问题
```python
# 检查性能设置
print(f"分辨率百分比: {bpy.context.scene.render.resolution_percentage}")

# 降低分辨率
bpy.context.scene.render.resolution_percentage = 50

# 禁用抗锯齿
bpy.ops.render.opengl(
    filepath='//fast.png',
    use_antialiasing=False,
    use_compositing=False,
    use_sequencer=False
)

# 重新渲染
print("降低质量设置后重新渲染")
```


---
## bpy_ops_cycles

# bpy.ops.cycles - Cycles Render Operations Module

Blender Cycles 渲染器操作符，用于 Cycles 渲染相关的操作。

## Overview

`bpy.ops.cycles` 模块包含 Cycles 渲染器的操作命令，支持渲染、光线追踪和着色器编辑。这个模块主要用于 Cycles 渲染引擎的控制和优化。

## 核心功能

```python
import bpy

# Cycles 渲染
bpy.ops.render.render()

# 采样控制
bpy.ops.render.sample()

# 光线追踪
bpy.ops.render.cycles_render()
```

## 实际应用示例

### Cycles 渲染管理器

```python
import bpy

class CyclesRenderManager:
    """Cycles 渲染管理器"""
    
    def __init__(self):
        self.render_settings = {}
    
    def set_samples(self, samples):
        """设置采样数"""
        bpy.context.scene.cycles.samples = samples
    
    def set_max_samples(self, samples):
        """设置最大采样"""
        bpy.context.scene.cycles.max_samples = samples
    
    def set_min_samples(self, samples):
        """设置最小采样"""
        bpy.context.scene.cycles.min_samples = samples
    
    def enable_adaptive_sampling(self, adaptive=True):
        """启用自适应采样"""
        bpy.context.scene.cycles.use_adaptive_sampling = adaptive
    
    def set_adaptive_threshold(self, threshold=0.05):
        """设置自适应阈值"""
        bpy.context.scene.cycles.adaptive_threshold = threshold
    
    def set_adaptive_steps(self, steps=2):
        """设置自适应步数"""
        bpy.context.scene.cycles.adaptive_steps = steps
    
    def set_tile_size(self, size=256):
        """设置分块大小"""
        bpy.context.scene.cycles.tile_size = size
    
    def set_tile_order(self, order='CENTER'):
        """设置分块顺序"""
        bpy.context.scene.cycles.tile_order = order
    
    def use_gpu(self, use=True):
        """使用 GPU"""
        bpy.context.scene.cycles.device = 'GPU' if use else 'CPU'
    
    def get_devices(self):
        """获取可用设备"""
        return bpy.context.scene.cycles.devices
    
    def set_device(self, device_type='CUDA'):
        """设置设备"""
        bpy.context.scene.cycles.device = device_type
    
    def enable_progressive_refine(self, enable=True):
        """启用渐进细化"""
        bpy.context.scene.cycles.use_progressive_refine = enable
    
    def set_blur_glossy(self, blur=0.0):
        """设置模糊光泽"""
        bpy.context.scene.cycles.blur_glossy = blur
    
    def set_light_sampling_threshold(self, threshold=0.0):
        """设置光线采样阈值"""
        bpy.context.scene.cycles.light_sampling_threshold = threshold
    
    def set_caustics_refractive(self, enable=True):
        """启用折射焦散"""
        bpy.context.scene.cycles.caustics_refractive = enable
    
    def set_caustics_reflective(self, enable=True):
        """启用反射焦散"""
        bpy.context.scene.cycles.caustics_reflective = enable
    
    def export_render_settings(self, filepath):
        """导出渲染设置"""
        settings = bpy.context.scene.cycles
        
        data = {
            'samples': settings.samples,
            'max_samples': settings.max_samples,
            'min_samples': settings.min_samples,
            'device': settings.device,
            'tile_size': settings.tile_size,
            'blur_glossy': settings.blur_glossy
        }
        
        import json
        
        with open(filepath, 'w') as f:
            json.dump(data, f, indent=2)
        
        return filepath
    
    def import_render_settings(self, filepath):
        """导入渲染设置"""
        import json
        
        with open(filepath, 'r') as f:
            data = json.load(f)
        
        settings = bpy.context.scene.cycles
        
        for key, value in data.items():
            if hasattr(settings, key):
                setattr(settings, key, value)
        
        return True


def setup_cycles_manager():
    """设置 Cycles 渲染管理器"""
    manager = CyclesRenderManager()
    
    print("Cycles 渲染管理器已设置")
    print("可用功能:")
    print("- set_samples: 设置采样")
    print("- use_gpu: 使用 GPU")
    print("- enable_adaptive_sampling: 自适应采样")
    
    return manager
```

### 渲染优化工具

```python
import bpy

class RenderOptimizationTool:
    """渲染优化工具"""
    
    def __init__(self):
        self.optimization_profiles = {}
    
    def create_preview_profile(self):
        """创建预览配置"""
        return {
            'samples': 32,
            'tile_size': 64,
            'max_bounces': 2,
            'use_gpu': True,
            'resolution_percent': 50
        }
    
    def create_final_profile(self):
        """创建最终渲染配置"""
        return {
            'samples': 256,
            'tile_size': 256,
            'max_bounces': 12,
            'use_gpu': True,
            'resolution_percent': 100
        }
    
    def create_quick_profile(self):
        """创建快速渲染配置"""
        return {
            'samples': 64,
            'tile_size': 128,
            'max_bounces': 4,
            'use_gpu': True,
            'resolution_percent': 75
        }
    
    def apply_profile(self, profile):
        """应用配置"""
        scene = bpy.context.scene
        cycles = scene.cycles
        
        cycles.samples = profile.get('samples', 128)
        cycles.tile_size = profile.get('tile_size', 256)
        scene.render.resolution_x = int(scene.render.resolution_x * profile.get('resolution_percent', 100) / 100)
        scene.render.resolution_y = int(scene.render.resolution_y * profile.get('resolution_percent', 100) / 100)
    
    def optimize_for_interactive(self):
        """优化交互渲染"""
        profile = {
            'samples': 16,
            'tile_size': 32,
            'use_gpu': True,
            'max_bounces': 2,
            'use_adaptive_sampling': True
        }
        
        self.apply_profile(profile)
    
    def optimize_for_final(self):
        """优化最终渲染"""
        profile = self.create_final_profile()
        self.apply_profile(profile)
    
    def set_resolution_scale(self, percent):
        """设置分辨率缩放"""
        bpy.context.scene.render.resolution_percentage = percent
    
    def set_quality_setting(self, quality='MEDIUM'):
        """设置质量"""
        settings = {
            'LOW': {'samples': 32, 'max_bounces': 2, 'tile_size': 64},
            'MEDIUM': {'samples': 128, 'max_bounces': 6, 'tile_size': 128},
            'HIGH': {'samples': 256, 'max_bounces': 12, 'tile_size': 256}
        }
        
        if quality in settings:
            for key, value in settings[quality].items():
                if key == 'samples':
                    bpy.context.scene.cycles.samples = value
                elif key == 'max_bounces':
                    bpy.context.scene.cycles.max_bounces = value
                elif key == 'tile_size':
                    bpy.context.scene.cycles.tile_size = value
    
    def enable_denoising(self, enable=True):
        """启用降噪"""
        bpy.context.scene.cycles.use_denoising = enable
    
    def set_denoiser(self, denoiser='OPENIMAGEDENOISE'):
        """设置降噪器"""
        bpy.context.scene.cycles.denoiser = denoiser
    
    def set_bvh_settings(self, bvh_type='DYNAMIC', spatial=None):
        """设置 BVH"""
        bpy.context.scene.cycles.bvh_type = bvh_type
        
        if spatial:
            bpy.context.scene.cycles.bvh_spatial_override = spatial
    
    def set_volume_step_size(self, size=0.1):
        """设置体积步长"""
        bpy.context.scene.cycles.volume_step_size = size
    
    def set_texture_limit(self, limit='OFF', max_size=4096):
        """设置纹理限制"""
        bpy.context.scene.cycles.texture_limit = limit
        
        if limit != 'OFF':
            bpy.context.scene.cycles.texture_limit_max = max_size
    
    def export_render_stats(self, filepath):
        """导出渲染统计"""
        scene = bpy.context.scene
        
        stats = {
            'resolution': f"{scene.render.resolution_x}x{scene.render.resolution_y}",
            'samples': scene.cycles.samples,
            'tile_size': scene.cycles.tile_size,
            'device': scene.cycles.device,
            'render_time': 'N/A'
        }
        
        import json
        
        with open(filepath, 'w') as f:
            json.dump(stats, f, indent=2)
        
        return filepath
    
    def estimate_render_time(self, samples=None):
        """估算渲染时间"""
        if samples is None:
            samples = bpy.context.scene.cycles.samples
        
        scene = bpy.context.scene
        
        # 简化的估算
        pixels = scene.render.resolution_x * scene.render.resolution_y
        pixels = pixels * (scene.render.resolution_percentage / 100) ** 2
        
        seconds_per_sample = 0.001  # 假设
        total_seconds = pixels * samples * seconds_per_sample
        
        return total_seconds


def setup_render_optimization():
    """设置渲染优化"""
    tool = RenderOptimizationTool()
    
    print("渲染优化工具已设置")
    print("可用配置:")
    print("- create_preview_profile: 预览")
    print("- create_final_profile: 最终")
    print("- create_quick_profile: 快速")
    
    return tool
```

### 着色器工具

```python
import bpy

class CyclesShaderTool:
    """Cycles 着色器工具"""
    
    def __init__(self):
        self.shaders = {}
    
    def get_active_material(self):
        """获取当前材质"""
        obj = bpy.context.active_object
        if obj and obj.active_material:
            return obj.active_material
        return None
    
    def get_principled_bsdf(self, material):
        """获取 Principled BSDF"""
        if not material or not material.use_nodes:
            return None
        
        nodes = material.node_tree.nodes
        return nodes.get('Principled BSDF')
    
    def set_base_color(self, material, color):
        """设置基础颜色"""
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            bsdf.inputs['Base Color'].default_value = color
    
    def set_metallic(self, material, metallic):
        """设置金属度"""
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            bsdf.inputs['Metallic'].default_value = metallic
    
    def set_roughness(self, material, roughness):
        """设置粗糙度"""
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            bsdf.inputs['Roughness'].default_value = roughness
    
    def set_specular(self, material, specular):
        """设置高光"""
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            bsdf.inputs['Specular'].default_value = specular
    
    def set_ior(self, material, ior):
        """设置折射率"""
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            bsdf.inputs['IOR'].default_value = ior
    
    def set_transmission(self, material, transmission):
        """设置透射"""
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            bsdf.inputs['Transmission'].default_value = transmission
    
    def set_emission(self, material, color, strength=1.0):
        """设置自发光"""
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            bsdf.inputs['Emission Color'].default_value = color
            bsdf.inputs['Emission Strength'].default_value = strength
    
    def create_glass_material(self, name, color=(1, 1, 1, 1), ior=1.45):
        """创建玻璃材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        
        bsdf = self.get_principled_bsdf(mat)
        if bsdf:
            bsdf.inputs['Base Color'].default_value = color
            bsdf.inputs['Metallic'].default_value = 0.0
            bsdf.inputs['Roughness'].default_value = 0.0
            bsdf.inputs['IOR'].default_value = ior
            bsdf.inputs['Transmission'].default_value = 1.0
        
        return mat
    
    def create_metal_material(self, name, color=(0.8, 0.8, 0.8, 1), 
                             roughness=0.3):
        """创建金属材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        
        bsdf = self.get_principled_bsdf(mat)
        if bsdf:
            bsdf.inputs['Base Color'].default_value = color
            bsdf.inputs['Metallic'].default_value = 1.0
            bsdf.inputs['Roughness'].default_value = roughness
        
        return mat
    
    def create_plastic_material(self, name, color=(0.2, 0.5, 0.8, 1),
                               roughness=0.5):
        """创建塑料材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        
        bsdf = self.get_principled_bsdf(mat)
        if bsdf:
            bsdf.inputs['Base Color'].default_value = color
            bsdf.inputs['Metallic'].default_value = 0.0
            bsdf.inputs['Roughness'].default_value = roughness
        
        return mat
    
    def create_emission_material(self, name, color=(1, 1, 1, 1),
                                 strength=5.0):
        """创建发光材质"""
        mat = bpy.data.materials.new(name=name)
        mat.use_nodes = True
        
        bsdf = self.get_principled_bsdf(mat)
        if bsdf:
            bsdf.inputs['Base Color'].default_value = (0, 0, 0, 1)
            bsdf.inputs['Emission Color'].default_value = color
            bsdf.inputs['Emission Strength'].default_value = strength
        
        return mat
    
    def add_noise_texture(self, material, scale=1.0, detail=15):
        """添加噪声纹理"""
        nodes = material.node_tree.nodes
        links = material.node_tree.links
        
        noise = nodes.new(type='ShaderNodeTexNoise')
        noise.inputs['Scale'].default_value = scale
        noise.inputs['Detail'].default_value = detail
        
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            links.new(noise.outputs['Fac'], bsdf.inputs['Roughness'])
    
    def add_wave_texture(self, material, scale=1.0):
        """添加波浪纹理"""
        nodes = material.node_tree.nodes
        links = material.node_tree.links
        
        wave = nodes.new(type='ShaderNodeTexWave')
        wave.inputs['Scale'].default_value = scale
        
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            links.new(wave.outputs['Color'], bsdf.inputs['Base Color'])
    
    def add_voronoi_texture(self, material, scale=1.0):
        """添加 Voronoi 纹理"""
        nodes = material.node_tree.nodes
        links = material.node_tree.links
        
        voronoi = nodes.new(type='ShaderNodeTexVoronoi')
        voronoi.inputs['Scale'].default_value = scale
        
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            links.new(voronoi.outputs['Distance'], bsdf.inputs['Roughness'])
    
    def add_bump_map(self, material, texture_node, strength=1.0):
        """添加凹凸贴图"""
        nodes = material.node_tree.nodes
        links = material.node_tree.links
        
        bump = nodes.new(type='ShaderNodeBump')
        bump.inputs['Strength'].default_value = strength
        
        links.new(texture_node.outputs['Height'], bump.inputs['Height'])
        
        bsdf = self.get_principled_bsdf(material)
        if bsdf:
            links.new(bump.outputs['Normal'], bsdf.inputs['Normal'])


def setup_cycles_shaders():
    """设置 Cycles 着色器"""
    tool = CyclesShaderTool()
    
    print("Cycles 着色器工具已设置")
    print("可用功能:")
    print("- create_glass_material: 玻璃")
    print("- create_metal_material: 金属")
    print("- create_emission_material: 发光")
    
    return tool
```


---
## bpy_ops_eevee

# bpy.ops.eevee - EEVEE渲染操作模块

bpy.ops.eevee模块提供了EEVEE实时渲染引擎的操作功能，用于渲染设置、采样控制和后期处理。

## 概述

EEVEE是Blender的实时渲染引擎，基于OpenGL。这个模块提供了EEVEE特有的渲染操作和设置调整功能。

## 核心功能

### 渲染操作

```python
import bpy

def render_screenshot(filepath=None):
    """渲染截图"""
    bpy.ops.eevee.render_screenshot(filepath=filepath)

def bake_direct_diffuse():
    """烘焙直接漫反射"""
    bpy.ops.eevee.bake(
        type='DIFFUSE',
        direct=True,
        indirect=False
    )

def bake_indirect_diffuse():
    """烘焙间接漫反射"""
    bpy.ops.eevee.bake(
        type='DIFFUSE',
        direct=False,
        indirect=True
    )

def bake_shadows():
    """烘焙阴影"""
    bpy.ops.eevee.bake(type='SHADOW')
```

### 采样控制

```python
def set_aa_samples(samples):
    """设置抗锯齿采样数"""
    bpy.context.scene.eevee.taa_render_samples = samples

def set_shadow_samples(samples):
    """设置阴影采样数"""
    bpy.context.scene.eevee.shadow_cube_size = samples
    bpy.context.scene.eevee.shadow_cascade_size = samples

def set_gi_samples(samples):
    """设置GI采样数"""
    bpy.context.scene.eevee.gi_render_samples = samples
```

## 实用函数

### 光照设置

```python
def setup_sun_light(
    energy=3.0,
    color=(1, 0.9, 0.8),
    angle=0.1
):
    """设置太阳光"""
    bpy.context.scene.eevee.sun_light_direction = (0, 0, 1)
    bpy.context.scene.eevee.sun_light_color = color
    bpy.context.scene.eevee.sun_soft_shadows = True
    bpy.context.scene.eevee.sun_angle = angle

def enable_shadows():
    """启用阴影"""
    bpy.context.scene.eevee.use_shadows = True

def disable_shadows():
    """禁用阴影"""
    bpy.context.scene.eevee.use_shadows = False
```

### 间接光

```python
def enable_indirect_lighting():
    """启用间接光"""
    bpy.context.scene.eevee.use_gtao = True
    bpy.context.scene.eevee.use_ambient_occlusion = True

def set_gtao_radius(radius):
    """设置GTAO半径"""
    bpy.context.scene.eevee.gtao_radius = radius

def set_gtao_distance(distance):
    """设置GTAO距离"""
    bpy.context.scene.eevee.gtao_distance = distance

def set_ao_distance(distance):
    """设置AO距离"""
    bpy.context.scene.eevee.ao_distance = distance
```

### 屏幕空间反射

```python
def enable_reflections():
    """启用反射"""
    bpy.context.scene.eevee.use_refraction = True
    bpy.context.scene.eevee.use_screen_space_reflections = True

def set_reflection_quality(quality):
    """设置反射质量"""
    bpy.context.scene.eevee.ssr_quality = quality

def set_refraction_quality(quality):
    """设置折射质量"""
    bpy.context.scene.eevee.refraction_quality = quality
```

## 常见问题排查

### 渲染问题

画面异常：
- 检查采样数是否足够
- 确认光照设置正确
- 验证纹理压缩设置

### 性能问题

渲染慢：
- 降低采样数
- 禁用阴影或反射
- 减少后期处理效果


---
## bpy_ops_workbench

# bpy.ops.workbench - Workbench渲染操作模块

bpy.ops.workbench模块提供了Workbench渲染引擎的操作功能，用于创建和配置高质量的渲染图像。

## 概述

Workbench是Blender的默认实时渲染引擎，专为建模和预览设计。这个模块提供了Workbench特有的渲染设置和输出操作。

## 核心功能

### 渲染操作

```python
import bpy

def render_screenshot(filepath=None):
    """渲染截图"""
    bpy.ops.workbench.render_screenshot(filepath=filepath)

def render_border():
    """渲染边框"""
    bpy.ops.render.render(
        use_viewport=True,
        write_still=True
    )
```

### 引擎设置

```python
def set_engine(engine_type='BLENDER_WORKBENCH'):
    """设置渲染引擎"""
    bpy.context.scene.render.engine = engine_type

def set_samples(samples):
    """设置采样数"""
    bpy.context.scene.eevee.taa_render_samples = samples

def set_resolution(resolution):
    """设置分辨率"""
    bpy.context.scene.render.resolution_x = resolution[0]
    bpy.context.scene.render.resolution_y = resolution[1]
```

## 实用函数

### 显示设置

```content="use_scene_lights",
        use_scene_world=True
    )

def set_material_mode(mode='MATERIAL'):
    """设置材质模式"""
    bpy.context.space_data.shading.type = 'MATERIAL'

def set_color_mode(mode='sRGB'):
    """设置颜色模式"""
    bpy.context.scene.display_settings.display_device = mode

def toggle_overlays():
    """切换叠加层"""
    bpy.context.space_data.overlay.show_overlays = not \
        bpy.context.space_data.overlay.show_overlays
```

### 性能设置

```python
def set_viewport_quality(quality):
    """设置视口质量"""
    bpy.context.scene.render.viewport_quality = quality

def enable_floor():
    """启用地面"""
    bpy.context.space_data.shading.show_floor = True

def set_light_intensity(intensity):
    """设置光照强度"""
    bpy.context.space_data.shading.light = intensity
```

## 常见问题排查

### 渲染问题

画面异常：
- 检查分辨率设置
- 确认光照正确
- 验证材质可用

### 性能问题

渲染慢：
- 降低采样数
- 减少分辨率
- 禁用阴影效果


---
## bpy_ops_material

# bpy.ops.material 模块

材质操作模块，用于在材质属性面板中创建和管理材质。

## 概述

bpy.ops.material 模块包含用于创建、编辑和应用材质的运算符。这些运算符控制材质的属性、输出和与其他对象的关联。

## 常用操作

### 材质创建

```python
import bpy

# 新建材质
bpy.ops.material.new(name='NewMaterial')

# 新建材质并设置
bpy.ops.material.new(name='MyMaterial')

# 复制材质
bpy.ops.material.copy()

# 粘贴材质
bpy.ops.material.paste()

# 删除材质
bpy.ops.material.delete()

# 重命名材质
bpy.ops.material.rename(name='RenamedMaterial')
```

### 材质分配

```python
# 分配材质到物体
bpy.ops.object.material_slot_add()

# 分配新材质
bpy.ops.object.material_slot_add()

# 分配材质
bpy.ops.object.material_slot_assign()

# 取消分配
bpy.ops.object.material_slot_deselect()

# 选择已分配
bpy.ops.object.material_slot_select()

# 复制材质分配
bpy.ops.object.material_slot_copy()

# 粘贴材质分配
bpy.ops.object.material_slot_paste()

# 删除材质槽
bpy.ops.object.material_slot_remove()

# 删除未使用材质
bpy.ops.object.material_slot_remove_unused()
```

### 材质设置

```python
# 设置表面
bpy.ops.material.surface_set()

# 清除表面
bpy.ops.material.surface_clear()

# 设置体积
bpy.ops.material.volume_set()

# 清除体积
bpy.ops.material.volume_clear()

# 设置置换
bpy.ops.material.displacement_set()

# 清除置换
bpy.ops.material.displacement_clear()
```

### 材质预览

```python
# 设置预览类型
bpy.ops.material.preview_sample()

# 刷新预览
bpy.ops.material.preview_refresh()
```

## 实际应用示例

### 创建标准PBR材质

```python
def create_pbr_material(name, base_color, metallic, roughness):
    """创建标准PBR材质"""
    # 创建新材质
    mat = bpy.data.materials.new(name=name)
    mat.use_nodes = True
    
    # 获取节点树
    tree = mat.node_tree
    
    # 清理默认节点
    for node in tree.nodes:
        tree.nodes.remove(node)
    
    # 创建Principled BSDF
    principled = tree.nodes.new('ShaderNodeBsdfPrincipled')
    principled.location = (0, 0)
    principled.inputs['Base Color'].default_value = (*base_color, 1.0)
    principled.inputs['Metallic'].default_value = metallic
    principled.inputs['Roughness'].default_value = roughness
    
    # 创建输出
    output = tree.nodes.new('ShaderNodeOutputMaterial')
    output.location = (300, 0)
    
    # 连接节点
    tree.links.new(principled.outputs['BSDF'], output.inputs['Surface'])
    
    return mat
```

### 创建程序化纹理材质

```python
def create_procedural_material(name):
    """创建程序化纹理材质"""
    mat = bpy.data.materials.new(name=name)
    mat.use_nodes = True
    tree = mat.node_tree
    
    # 创建纹理坐标
    tex_coord = tree.nodes.new('ShaderNodeTexCoord')
    tex_coord.location = (-600, 0)
    
    # 创建映射
    mapping = tree.nodes.new('ShaderNodeMapping')
    mapping.location = (-400, 0)
    
    # 创建噪声纹理
    noise = tree.nodes.new('ShaderNodeTexNoise')
    noise.location = (-200, 0)
    noise.noise_scale = 5.0
    
    # 创建颜色渐变
    gradient = tree.nodes.new('ShaderNodeValToRGB')
    gradient.location = (0, 0)
    
    # 创建Principled BSDF
    principled = tree.nodes.new('ShaderNodeBsdfPrincipled')
    principled.location = (200, 0)
    
    # 创建输出
    output = tree.nodes.new('ShaderNodeOutputMaterial')
    output.location = (400, 0)
    
    # 连接节点
    tree.links.new(tex_coord.outputs['Generated'], mapping.inputs['Vector'])
    tree.links.new(mapping.outputs['Vector'], noise.inputs['Vector'])
    tree.links.new(noise.outputs['Fac'], gradient.inputs['Fac'])
    tree.links.new(gradient.outputs['Color'], principled.inputs['Base Color'])
    tree.links.new(principled.outputs['BSDF'], output.inputs['Surface'])
    
    return mat
```

### 创建玻璃材质

```python
def create_glass_material(name, transmission=1.0, roughness=0.0, ior=1.45):
    """创建玻璃材质"""
    mat = bpy.data.materials.new(name=name)
    mat.use_nodes = True
    tree = mat.node_tree
    
    # 清理
    for node in tree.nodes:
        tree.nodes.remove(node)
    
    # 创建Principled BSDF
    principled = tree.nodes.new('ShaderNodeBsdfPrincipled')
    principled.location = (0, 0)
    principled.inputs['Base Color'].default_value = (1.0, 1.0, 1.0, 1.0)
    principled.inputs['Metallic'].default_value = 0.0
    principled.inputs['Roughness'].default_value = roughness
    principled.inputs['IOR'].default_value = ior
    principled.inputs['Transmission'].default_value = transmission
    
    # 创建输出
    output = tree.nodes.new('ShaderNodeOutputMaterial')
    output.location = (300, 0)
    
    # 连接
    tree.links.new(principled.outputs['BSDF'], output.inputs['Surface'])
    
    return mat
```

### 创建发光材质

```python
def create_emission_material(name, color, strength=5.0):
    """创建发光材质"""
    mat = bpy.data.materials.new(name=name)
    mat.use_nodes = True
    tree = mat.node_tree
    
    # 清理
    for node in tree.nodes:
        tree.nodes.remove(node)
    
    # 创建发射节点
    emission = tree.nodes.new('ShaderNodeEmission')
    emission.location = (0, 0)
    emission.inputs['Color'].default_value = (*color, 1.0)
    emission.inputs['Strength'].default_value = strength
    
    # 创建输出
    output = tree.nodes.new('ShaderNodeOutputMaterial')
    output.location = (300, 0)
    
    # 连接
    tree.links.new(emission.outputs['Emission'], output.inputs['Surface'])
    
    return mat
```

### 批量创建材质

```python
def batch_create_materials(material_data):
    """批量创建材质"""
    created_materials = []
    
    for name, params in material_data.items():
        if params['type'] == 'pbr':
            mat = create_pbr_material(
                name,
                params.get('base_color', (1, 1, 1)),
                params.get('metallic', 0.0),
                params.get('roughness', 0.5)
            )
        elif params['type'] == 'glass':
            mat = create_glass_material(
                name,
                params.get('transmission', 1.0),
                params.get('roughness', 0.0)
            )
        elif params['type'] == 'emission':
            mat = create_emission_material(
                name,
                params.get('color', (1, 1, 1)),
                params.get('strength', 5.0)
            )
        else:
            mat = create_pbr_material(name)
        
        created_materials.append(mat)
    
    return created_materials
```

### 应用材质到物体

```python
def apply_material_to_objects(material, objects):
    """应用材质到多个物体"""
    for obj in objects:
        # 添加材质槽
        obj.data.materials.append(material)
    
    return objects
```

### 复制物体材质

```python
def copy_material_from_source(source_obj, target_objects):
    """从源物体复制材质到目标"""
    if not source_obj.data.materials:
        return
    
    # 获取源材质
    source_mat = source_obj.data.materials[0]
    
    # 复制材质
    new_mat = source_mat.copy()
    new_mat.name = f'{source_mat.name}_copy'
    
    # 应用到目标
    apply_material_to_objects(new_mat, target_objects)
    
    return new_mat
```

### 创建材质库

```python
def create_material_library(materials_dict, library_name='MaterialLibrary'):
    """创建材质库"""
    library = bpy.data.textures.new(name=library_name)
    
    materials = []
    for name, params in materials_dict.items():
        if params['type'] == 'pbr':
            mat = create_pbr_material(name, params['color'], params['metallic'], params['roughness'])
        else:
            mat = create_pbr_material(name)
        
        materials.append(mat)
    
    return materials
```

## 使用场景

- **材质创建**：创建新的材质
- **材质管理**：组织和复制材质
- **PBR工作流**：创建物理材质
- **程序化纹理**：使用节点创建材质
- **批量应用**：快速应用材质到多个物体
- **材质库**：创建可复用材质集合

## 注意事项

- 材质使用节点系统更灵活
- PBR材质需要正确设置
- 玻璃材质需要传输设置
- 发光材质影响渲染
- 材质复制会共享节点树
- 大量材质影响性能


---
## bpy_ops_texture

# bpy.ops.texture 模块

纹理操作模块，用于创建和管理程序化纹理。

## 概述

bpy.ops.texture 模块包含用于创建和配置程序化纹理的运算符。这些运算符控制纹理类型、参数和预览显示。

## 常用操作

### 纹理创建

```python
import bpy

# 新建纹理
bpy.ops.texture.new(name='NewTexture')

# 新建程序纹理
bpy.ops.texture.new(name='ProceduralTexture')

# 复制纹理
bpy.ops.texture.copy()

# 粘贴纹理
bpy.ops.texture.paste()

# 删除纹理
bpy.ops.texture.delete()

# 重命名纹理
bpy.ops.texture.rename(name='RenamedTexture')
```

### 纹理类型

```python
# 新建云纹
bpy.ops.texture.new(name='Clouds')

# 新建木纹
bpy.ops.texture.new(name='Wood')

# 新建大理石纹
bpy.ops.texture.new(name='Marble')

# 新建电磁场
bpy.ops.texture.new(name='Electric')

# 新建漩涡
bpy.ops.texture.new(name='Swirl')

# 新建噪声
bpy.ops.texture.new(name='Noise')

# 新建混合
bpy.ops.texture.new(name='Blend')

# 新建魔法
bpy.ops.texture.new(name='Magic')

# 新建补丁
bpy.ops.texture.new(name='Patchy')

# 新建Voronoi
bpy.ops.texture.new(name='Voronoi')

# 新建Musgrave
bpy.ops.texture.new(name='Musgrave')

# 新建Ocean
bpy.ops.texture.new(name='Ocean')
```

### 纹理预览

```python
# 刷新预览
bpy.ops.texture.preview_refresh()

# 采样预览
bpy.ops.texture.preview_sample()

# 缩放预览
bpy.ops.texture.preview_use_size()
```

### 纹理插槽

```python
# 添加插槽
bpy.ops.texture.slot_add()

# 删除插槽
bpy.ops.texture.slot_remove()

# 移动插槽
bpy.ops.texture.slot_move(type='UP')
bpy.ops.texture.slot_move(type='DOWN')

# 复制插槽
bpy.ops.texture.slot_copy()

# 粘贴插槽
bpy.ops.texture.slot_paste()
```

## 实际应用示例

### 创建云纹

```python
def create_clouds_texture(name, size=1.0, nabla=0.1, depth=2):
    """创建云纹"""
    tex = bpy.data.textures.new(name=name, type='CLOUDS')
    tex.noise_scale = size
    tex.noise_depth = depth
    
    return tex
```

### 创建木纹

```python
def create_wood_texture(name, noise_scale=1.0, turbulence=5.0, depth=2):
    """创建木纹"""
    tex = bpy.data.textures.new(name=name, type='WOOD')
    tex.noise_scale = noise_scale
    tex.turbulence = turbulence
    tex.noise_depth = depth
    
    return tex
```

### 创建Voronoi纹理

```python
def create_voronoi_texture(name, distance_metric='SQUARED', color_mode='INTENSITY'):
    """创建Voronoi纹理"""
    tex = bpy.data.textures.new(name=name, type='VORONOI')
    tex.distance_metric = distance_metric
    tex.color_mode = color_mode
    
    return tex
```

### 创建Musgrave纹理

```python
def create_musgrave_texture(name, musgrave_type='FBM', dimension=2.0):
    """创建Musgrave纹理"""
    tex = bpy.data.textures.new(name=name, type='MUSGRAVE')
    tex.musgrave_type = musgrave_type
    tex.dimension = dimension
    
    return tex
```

### 创建Ocean纹理

```python
def create_ocean_texture(name, resolution=64, spatial_scale=1.0, time_scale=1.0):
    """创建Ocean纹理"""
    tex = bpy.data.textures.new(name=name, type='OCEAN')
    tex.resolution = resolution
    tex.spatial_scale = spatial_scale
    tex.time_scale = time_scale
    
    return tex
```

### 创建程序化材质纹理

```python
def create_procedural_material_texture():
    """创建程序化材质纹理集"""
    textures = {
        'procedural_brick': create_brick_texture('Brick'),
        'procedural_wood': create_wood_texture('Wood'),
        'procedural_metal': create_voronoi_texture('Metal'),
        'procedural_rock': create_musgrave_texture('Rock'),
    }
    
    return textures
```

### 创建无缝纹理

```python
def create_seamless_texture(source_texture):
    """创建无缝纹理"""
    # 复制纹理
    new_tex = source_texture.copy()
    new_tex.name = f'{source_texture.name}_seamless'
    
    # 设置无缝参数
    # 注意：Blender程序纹理本身是无缝的
    
    return new_tex
```

### 批量创建自然纹理

```python
def create_natural_textures():
    """创建自然纹理集"""
    textures = {
        'natural_grass': create_clouds_texture('Grass', size=2.0),
        'natural_rock': create_voronoi_texture('Rock', distance_metric='EUCLIDEAN'),
        'natural_sand': create_musgrave_texture('Sand', musgrave_type='RIDGED_MULTIFRACTAL'),
        'natural_water': create_ocean_texture('Water', resolution=128),
        'natural_wood': create_wood_texture('WoodGrain', noise_scale=1.5),
    }
    
    return textures
```

## 使用场景

- **程序化纹理**：创建可调节的纹理
- **材质制作**：为材质提供纹理输入
- **无缝纹理**：创建可平铺纹理
- **自然纹理**：创建自然材质效果
- **纹理库**：创建可复用纹理集合
- **批处理**：批量生成纹理

## 注意事项

- 程序纹理可以无限缩放
- 不同纹理类型有不同参数
- 纹理与材质配合使用
- 高分辨率影响性能
- 纹理可以混合使用
- 某些纹理类型已被节点系统替代


---
## bpy_ops_shader_node

# bpy.ops.node - 着色器节点操作模块

bpy.ops.node模块提供了着色器节点编辑器中的操作功能，用于创建和连接材质节点。

## 概述

着色器节点编辑器是Blender中创建材质的重要工具。这个模块提供了添加、连接和编辑着色器节点的操作。

## 核心功能

### 添加节点

```python
import bpy

def add_shader_node(node_type):
    """添加着色器节点"""
    bpy.ops.node.add_node(
        type=node_type,
        use_transform=True
    )

def add_output_node():
    """添加输出节点"""
    add_shader_node('ShaderNodeOutputMaterial')

def add_material_output():
    """添加材质输出"""
    add_shader_node('ShaderNodeOutputMaterial')

def add_principled_bsdf():
    """添加Principled BSDF节点"""
    add_shader_node('ShaderNodeBsdfPrincipled')

def add_glossy_bsdf():
    """添加光滑BSDF节点"""
    add_shader_node('ShaderNodeBsdfGlossy')

def add_diffuse_bsdf():
    """添加漫反射BSDF节点"""
    add_shader_node('ShaderNodeBsdfDiffuse')

def add_emission():
    """添加自发光节点"""
    add_shader_node('ShaderNodeEmission')

def add_transparent_bsdf():
    """添加透明BSDF节点"""
    add_shader_node('ShaderNodeBsdfTransparent')

def add_glass_bsdf():
    """添加玻璃BSDF节点"""
    add_shader_node('ShaderNodeBsdfGlass')
```

### 纹理节点

```python
def add_image_texture():
    """添加图像纹理节点"""
    add_shader_node('ShaderNodeTexImage')

def add_noise_texture():
    """添加噪声纹理节点"""
    add_shader_node('ShaderNodeTexNoise')

def add_wave_texture():
    """添加波浪纹理节点"""
    add_shader_node('ShaderNodeTexWave')

def add_voronoi_texture():
    """添加Vorono纹理节点"""
    add_shader_node('ShaderNodeTexVoronoi')

def add_musgrave_texture():
    """添加Musgrave纹理节点"""
    add_shader_node('ShaderNodeTexMusgrave')

def add_gradient_texture():
    """添加渐变纹理节点"""
    add_shader_node('ShaderNodeTexGradient')
```

### 颜色节点

```python
def add_color_ramp():
    """添加颜色渐变"""
    add_shader_node('ShaderNodeValToRGB')

def add_hue_saturation():
    """添加色相饱和度"""
    add_shader_node('ShaderNodeHueSaturation')

def add_rgb_curves():
    """添加RGB曲线"""
    add_shader_node('ShaderNodeCurvesRGB')

def add_invert():
    """添加反相"""
    add_shader_node('ShaderNodeInvert')

def add_brightness_contrast():
    """添加亮度对比度"""
    add_shader_node('ShaderNodeBrightContrast')
```

## 实用函数

### 输入输出节点

```python
def add_texture_coordinate():
    """添加纹理坐标"""
    add_shader_node('ShaderNodeTexCoord')

def add_mapping():
    """添加映射"""
    add_shader_node('ShaderNodeMapping')

def add_normal():
    """添加法线"""
    add_shader_node('ShaderNodeNormal')

def add_bump():
    """添加凹凸"""
    add_shader_node('ShaderNodeBump')

def add_vector_rotate():
    """添加向量旋转"""
    add_shader_node('ShaderNodeVectorRotate')
```

### 混合节点

```python
def add_mix_shader():
    """添加混合着色器"""
    add_shader_node('ShaderNodeMixShader')

def add_add_shader():
    """添加添加着色器"""
    add_shader_node('ShaderNodeAddShader')

def add_mix_color():
    """添加混合颜色"""
    add_shader_node('ShaderNodeMixRGB')

def add_mix():
    """添加混合"""
    add_shader_node('ShaderNodeMath', 'Mix')
```

## 常见问题排查

### 节点添加失败

类型错误：
- 确认节点类型正确
- 检查编辑器类型
- 验证节点可用性

### 链接问题

连接失败：
- 确认Socket兼容
- 检查输入输出存在
- 验证连接方向


---
## bpy_ops_world

# bpy.ops.world 模块

世界环境操作模块，用于管理场景的世界环境和光照设置。

## 概述

bpy.ops.world 模块包含用于配置场景世界环境的运算符。世界环境控制场景的背景颜色、环境光照、雾效和整体氛围。

## 常用操作

### 世界创建

```python
import bpy

# 创建新世界
bpy.ops.world.new(name='MyWorld')

# 设置活动世界
bpy.ops.world.set_active()

# 复制世界
bpy.ops.world.duplicate(world='MyWorld')
```

### 颜色设置

```python
# 设置背景颜色
bpy.ops.world.color_set(color=(0.1, 0.1, 0.1, 1.0))

# 设置环境颜色
bpy.ops.world.ambient_color_set(color=(0.5, 0.5, 0.5))

# 设置地平线颜色
bpy.ops.world.horizon_color_set(color=(0.0, 0.0, 0.0))

# 设置天顶颜色
bpy.ops.world.zenith_color_set(color=(0.0, 0.1, 0.3))
```

### 环境纹理

```python
# 加载环境纹理
bpy.ops.world.texface_image_add(filepath='//hdr.exr')

# 加载平铺环境纹理
bpy.ops.world.texface_image_add(filepath='//hdr.exr', tiling='4')

# 设置环境纹理强度
bpy.ops.world.light_texmat_image_set(intensity=1.0)
```

### 雾效设置

```python
# 设置雾类型
bpy.ops.world.mist_set(type='LINEAR')

# 设置线性雾
bpy.ops.world.mist_set(type='LINEAR', start=5.0, depth=20.0)

# 设置指数雾
bpy.ops.world.mist_set(type='EXP2', start=5.0, depth=20.0)

# 设置平方反比雾
bpy.ops.world.mist_set(type='INVERSE_SQUARE', start=5.0, depth=20.0)

# 清除雾效
bpy.ops.world.mist_clear()
```

### 环镜光遮蔽

```python
# 设置环境光遮蔽
bpy.ops.world.ao_environment_map_create()

# 创建环境光遮蔽纹理
bpy.ops.world.ao_environment_map_create(samples=16)

# 设置环境光遮蔽因子
bpy.ops.world.ao_set(factor=1.0)
```

### 节点操作

```python
# 新建节点树
bpy.ops.world.new()

# 复制节点树
bpy.ops.world.duplicate()

# 粘贴节点
bpy.ops.world.node_tree_socket_copy()

# 粘贴节点
bpy.ops.world.node_tree_socket_paste()
```

### 灯光设置

```python
# 添加环境光
bpy.ops.world.light_add()

# 添加区域光
bpy.ops.world.light_add(type='AREA')

# 添加太阳光
bpy.ops.world.light_add(type='SUN')
```

## 实际应用示例

### 创建程序化天空

```python
def create_procedural_sky():
    """创建程序化天空环境"""
    # 创建新世界
    bpy.ops.world.new(name='ProceduralSky')
    world = bpy.data.worlds['ProceduralSky']
    
    # 设置节点材质
    world.use_nodes = True
    nodes = world.node_tree.nodes
    links = world.node_tree.links
    
    # 创建天空节点
    sky_node = nodes.new(type='ShaderNodeTexSky')
    sky_node.sun_direction = (0.0, 0.0, 1.0)
    sky_node.turbidity = 2.0
    sky_node.ground_albedo = 0.3
    
    # 创建坐标节点
    coord_node = nodes.new(type='ShaderNodeTexCoord')
    
    # 创建映射节点
    mapping_node = nodes.new(type='ShaderNodeMapping')
    
    # 创建背景节点
    background_node = nodes.new(type='ShaderNodeBackground')
    
    # 创建输出节点
    output_node = nodes.get('World Output')
    
    # 连接节点
    links.new(coord_node.outputs['Generated'], mapping_node.inputs['Vector'])
    links.new(mapping_node.outputs['Vector'], sky_node.inputs['Vector'])
    links.new(sky_node.outputs['Color'], background_node.inputs['Color'])
    links.new(background_node.outputs['Background'], output_node.inputs['Surface'])
    
    return world
```

### 创建HDRI环境

```python
def create_hdri_environment(hdr_path, strength=1.0):
    """创建HDRI环境"""
    # 创建新世界
    bpy.ops.world.new(name='HDRI_Environment')
    world = bpy.data.worlds['HDRI_Environment']
    
    # 设置节点材质
    world.use_nodes = True
    nodes = world.node_tree.nodes
    links = world.node_tree.links
    
    # 创建环境纹理节点
    env_tex = nodes.new(type='ShaderNodeTexEnvironment')
    env_tex.image = bpy.data.images.load(hdr_path)
    env_tex.interpolation = 'Linear'
    
    # 创建背景节点
    background = nodes.new(type='ShaderNodeBackground')
    background.inputs['Strength'].default_value = strength
    
    # 获取输出节点
    output = nodes.get('World Output')
    
    # 连接节点
    links.new(env_tex.outputs['Color'], background.inputs['Color'])
    links.new(background.outputs['Background'], output.inputs['Surface'])
    
    # 设置世界到场景
    bpy.context.scene.world = world
    
    return world
```

### 创建雾效场景

```python
def create_foggy_scene(fog_start=5.0, fog_depth=20.0, fog_color=(0.8, 0.8, 0.9)):
    """创建雾效场景"""
    # 获取当前世界
    world = bpy.context.scene.world
    
    if not world:
        bpy.ops.world.new(name='FoggyWorld')
        world = bpy.data.worlds['FoggyWorld']
    
    # 设置雾效
    world.mist_settings.use_mist = True
    world.mist_settings.intensity = 0.5
    world.mist_settings.start = fog_start
    world.mist_settings.depth = fog_depth
    world.mist_settings.falloff = 'LINEAR'
    
    # 设置雾颜色
    world.mist_settings.color = fog_color
    
    return world
```

### 创建渐变背景

```python
def create_gradient_background(top_color=(0.0, 0.1, 0.4), bottom_color=(0.5, 0.7, 0.9)):
    """创建渐变背景"""
    # 创建新世界
    bpy.ops.world.new(name='GradientBackground')
    world = bpy.data.worlds['GradientBackground']
    
    # 设置节点材质
    world.use_nodes = True
    nodes = world.node_tree.nodes
    links = world.node_tree.links
    
    # 创建渐变纹理节点
    gradient = nodes.new(type='ShaderNodeTexGradient')
    gradient.gradient_type = 'LINEAR'
    
    # 创建颜色节点
    top_col = nodes.new(type='ShaderNodeRGB')
    top_col.outputs[0].default_value = (*top_color, 1.0)
    
    bottom_col = nodes.new(type='ShaderNodeRGB')
    bottom_col.outputs[0].default_value = (*bottom_color, 1.0)
    
    # 创建混合节点
    mix = nodes.new(type='ShaderNodeMixRGB')
    mix.inputs['Fac'].default_value = 0.5
    
    # 创建背景节点
    background = nodes.new(type='ShaderNodeBackground')
    
    # 创建输出节点
    output = nodes.get('World Output')
    
    # 连接节点
    links.new(top_col.outputs['Color'], mix.inputs['Color1'])
    links.new(bottom_col.outputs['Color'], mix.inputs['Color2'])
    links.new(gradient.outputs['Color'], mix.inputs['Fac'])
    links.new(mix.outputs['Color'], background.inputs['Color'])
    links.new(background.outputs['Background'], output.inputs['Surface'])
    
    return world
```

### 创建程序化星星背景

```python
def create_star_background(star_count=1000, star_size=1.0, background_color=(0.0, 0.0, 0.05)):
    """创建星星背景"""
    # 创建新世界
    bpy.ops.world.new(name='StarBackground')
    world = bpy.data.worlds['StarBackground']
    
    # 设置节点材质
    world.use_nodes = True
    nodes = world.node_tree.nodes
    links = world.node_tree.links
    
    # 创建背景色
    background = nodes.new(type='ShaderNodeBackground')
    background.inputs['Color'].default_value = (*background_color, 1.0)
    
    # 创建输出节点
    output = nodes.get('World Output')
    
    # 连接节点
    links.new(background.outputs['Background'], output.inputs['Surface'])
    
    return world
```

### 创建多云天空

```python
def create_cloudy_sky(cloud_coverage=0.5, sun_direction=(1.0, 0.5, 0.5)):
    """创建多云天空"""
    # 创建新世界
    bpy.ops.world.new(name='CloudySky')
    world = bpy.data.worlds['CloudySky']
    
    # 设置节点材质
    world.use_nodes = True
    nodes = world.node_tree.nodes
    links = world.node_tree.links
    
    # 创建天空节点
    sky = nodes.new(type='ShaderNodeTexSky')
    sky.sun_direction = sun_direction
    sky.turbidity = 3.0
    sky.exposure = 1.0
    sky.ground_albedo = 0.5
    
    # 创建噪波纹理
    noise = nodes.new(type='ShaderNodeTexNoise')
    noise.noise_scale = 5.0
    noise.noise_depth = 2
    
    # 创建颜色渐变
    gradient = nodes.new(type='ShaderNodeTexGradient')
    gradient.gradient_type = 'LINEAR'
    
    # 创建混合节点
    mix = nodes.new(type='ShaderNodeMixRGB')
    mix.inputs['Fac'].default_value = cloud_coverage
    
    # 创建背景节点
    background = nodes.new(type='ShaderNodeBackground')
    
    # 创建输出节点
    output = nodes.get('World Output')
    
    # 连接节点
    links.new(sky.outputs['Color'], mix.inputs['Color1'])
    links.new(noise.outputs['Color'], mix.inputs['Color2'])
    links.new(mix.outputs['Color'], background.inputs['Color'])
    links.new(background.outputs['Background'], output.inputs['Surface'])
    
    return world
```

## 使用场景

- **环境设置**：设置场景的整体光照和氛围
- **HDRI照明**：使用HDRI图像进行环境照明
- **雾效制作**：创建雾、烟等大气效果
- **天空系统**：创建程序化天空和云层
- **背景设置**：设置渲染背景
- **光照烘焙**：烘焙环境光照

## 注意事项

- 世界设置影响整个场景的照明
- HDRI贴图需要高动态范围格式
- 雾效影响场景的深度感
- 节点系统提供更灵活的控制
- 世界设置可以链接到其他场景
- 环境光遮蔽需要额外的计算时间


---
## bpy_ops_light_probe

# bpy.ops.light_probe - 光照探针操作模块

bpy.ops.light_probe模块提供了光照探针（ light probe）对象的创建和管理操作，用于改进光照渲染效果。

## 概述

光照探针是用于捕捉环境光照和反射的工具，在EEVEE和Cycles渲染中非常重要。这个模块提供了创建和编辑光照探针的操作。

## 核心功能

### 探针创建

```python
import bpy

def add_reflection_cube(location=None):
    """添加反射立方体贴图探针"""
    bpy.ops.object.lightprobe_add(
        type='CUBEMAP',
        location=location or bpy.context.scene.cursor.location
    )

def add_reflection_plane(location=None):
    """添加反射平面探针"""
    bpy.ops.object.lightprobe_add(
        type='PLANAR',
        location=location or bpy.context.scene.cursor.location
    )

def add_irradiance_volume(location=None):
    """添加辐照度体积探针"""
    bpy.ops.object.lightprobe_add(
        type='VOLUME_SAMPLE',
        location=location or bpy.context.scene.cursor.location
    )
```

### 探针设置

```python
def set_intensity(intensity):
    """设置强度"""
    probe = bpy.context.active_object
    if probe and probe.type == 'LIGHT_PROBE':
        probe.data.intensity = intensity

def set_radius(radius):
    """设置半径"""
    probe = bpy.context.active_object
    if probe and probe.type == 'LIGHT_PROBE':
        probe.data.radius = radius

def set_resolution(resolution):
    """设置分辨率"""
    probe = bpy.context.active_object
    if probe and probe.type == 'LIGHT_PROBE':
        probe.data.resolution = resolution

def set_clip_start(distance):
    """设置近裁剪"""
    probe = bpy.context.active_object
    if probe and probe.type == 'LIGHT_PROBE':
        probe.data.clip_start = distance

def set_clip_end(distance):
    """设置远裁剪"""
    probe = bpy.context.active_object
    if probe and probe.type == 'LIGHT_PROBE':
        probe.data.clip_end = distance
```

### 探针更新

```python
def hide_probe():
    """隐藏探针"""
    bpy.ops.object.hide_probe()

def show_probe():
    """显示探针"""
    bpy.ops.object.show_probe()

def duplicate_probe():
    """复制探针"""
    bpy.ops.object.duplicate()
```

## 实用函数

### 批处理创建

```python
def create_probe_grid(
    size=(3, 3, 3),
    spacing=2.0,
    probe_type='VOLUME_SAMPLE'
):
    """创建探针网格"""
    center = bpy.context.scene.cursor.location
    probes = []
    
    for x in range(size[0]):
        for y in range(size[1]):
            for z in range(size[2]):
                location = (
                    center.x + (x - size[0]//2) * spacing,
                    center.y + (y - size[1]//2) * spacing,
                    center.z + (z - size[2]//2) * spacing
                )
                bpy.ops.object.lightprobe_add(type=probe_type, location=location)
                probes.append(bpy.context.active_object)
    
    return probes
```

### 属性调整

```python
def adjust_probe_range(probe, influence_distance):
    """调整探针影响范围"""
    probe.data.influence_distance = influence_distance
    probe.data.falloff = 1.0

def optimize_for_realtime(probe):
    """优化实时渲染"""
    probe.data.resolution = 32
    probe.data.clip_end = 10.0

def optimize_for_quality(probe):
    """优化质量"""
    probe.data.resolution = 256
    probe.data.clip_end = 50.0
```

## 常见问题排查

### 探针不可见

显示问题：
- 检查探针是否隐藏
- 确认渲染引擎支持
- 验证图层可见性

### 光照异常

设置问题：
- 检查探针范围设置
- 确认分辨率足够
- 验证位置是否合理


---
## bpy_ops_compositor

# bpy.ops.compositor - 合成器操作模块

bpy.ops.compositor模块提供了合成器（ compositor）节点编辑器中的操作功能，用于管理和编辑合成节点。

## 概述

合成器是Blender中用于图像后期处理的重要工具。这个模块提供了在合成器环境中进行节点操作和渲染设置的操作。

## 核心功能

### 节点操作

```python
import bpy

def add_render_layer_node():
    """添加渲染层节点"""
    bpy.ops.compositor.add_node(
        type='CompositorNodeRLayers',
        use_transform=True
    )

def add_composite_node():
    """添加合成节点"""
    bpy.ops.compositor.add_node(
        type='CompositorNodeComposite',
        use_transform=True
    )

def add_viewer_node():
    """添加查看器节点"""
    bpy.ops.compositor.add_node(
        type='CompositorNodeViewer',
        use_transform=True
    )

def delete_selected_nodes():
    """删除选中节点"""
    bpy.ops.compositor.delete()

def duplicate_nodes():
    """复制节点"""
    bpy.ops.compositor.duplicate()
```

### 节点连接

```python
def link_nodes():
    """链接节点"""
    bpy.ops.compositor.link()

def unlink_nodes():
    """取消链接"""
    bpy.ops.compositor.unlink()

def reroute():
    """添加路由点"""
    bpy.ops.compositor.add_reroute()
```

### 选择操作

```python
def select_all():
    """全选"""
    bpy.ops.compositor.select_all(action='SELECT')

def deselect_all():
    """取消全选"""
    bpy.ops.compositor.select_all(action='DESELECT')

def select_hierarchy():
    """选择层级"""
    bpy.ops.compositor.select_hierarchy(direction='CHILD')

def select_border():
    """框选"""
    bpy.ops.compositor.select_border(extend=False)
```

## 实用函数

### 视口操作

```python
def view_all():
    """查看全部"""
    bpy.ops.compositor.view_all()

def view_selected():
    """查看选中"""
    bpy.ops.compositor.view_selected()

def zoom_in():
    """放大"""
    bpy.ops.compositor.zoom_in()

def zoom_out():
    """缩小"""
    bpy.ops.compositor.zoom_out()
```

### 混合操作

```python
def start_open_render():
    """开始开放渲染"""
    bpy.ops.render.opengl.render(
        animation=False,
        write_still=False
    )

def clear_render_result():
    """清除渲染结果"""
    bpy.ops.render.result_clear()
```

## 常见问题排查

### 节点连接问题

链接失败：
- 确认节点类型兼容
- 检查Socket类型匹配
- 验证输出输入存在

### 渲染问题

渲染失败：
- 检查节点设置正确
- 确认输入数据有效
- 验证输出节点存在


---
## bpy_ops_camera

# bpy.ops.camera 模块

相机操作模块，用于创建和管理3D场景中的相机。

## 概述

bpy.ops.camera 模块包含用于创建、编辑和配置相机的运算符。这些运算符用于在3D场景中添加相机、设置相机属性和执行相机操作。

## 常用操作

### 相机创建

```python
import bpy

# 创建新相机
bpy.ops.object.camera_add(location=(0, 0, 5))

# 创建相机并设置为活动相机
bpy.ops.object.camera_add(location=(0, -5, 2))
bpy.context.scene.camera = bpy.context.active_object

# 从选中对象创建相机
bpy.ops.object.camera_add_from_view()

# 同步相机到视图
bpy.ops.view3d.object_as_camera()
```

### 相机设置

```python
# 设置相机为正交投影
camera = bpy.context.active_object.data
camera.type = 'ORTHO'

# 设置相机为透视投影
camera.type = 'PERSP'

# 设置相机焦距（毫米）
camera.lens = 50.0

# 设置传感器尺寸
camera.sensor_width = 36.0
camera.sensor_height = 24.0

# 设置相机裁剪范围
camera.clip_start = 0.1
camera.clip_end = 1000.0

# 设置景深
camera.dof.use_dof = True
camera.dof.focus_object = bpy.data.objects['FocusObj']
camera.dof.aperture_fstop = 2.8
```

### 相机视图

```python
# 切换到相机视图
bpy.ops.view3d.view_camera()

# 设置相机为当前视图
bpy.ops.view3d.camera_to_view()

# 将视图对齐到相机
bpy.ops.view3d.camera_to_view_selected()

# 锁定相机到视图
bpy.ops.view3d.lock_camera_to_view()

# 解锁相机
bpy.ops.view3d.unlock_camera_to_view()
```

### 相机动画

```python
# 相机环绕动画
bpy.ops.view3d.camera_add_view_rotation(path='//animation.blend')

# 相机轨道
bpy.ops.view3d.camera_view_frame()

# 平滑相机
bpy.ops.view3d.camera_smooth()
```

## 实际应用示例

### 创建轨道相机动画

```python
import bpy
import math

def create_orbit_camera_animation(target_obj, radius, frames=100):
    """创建相机环绕目标动画"""
    # 创建相机
    bpy.ops.object.camera_add(location=(radius, 0, 2))
    camera = bpy.context.active_object
    camera.name = "OrbitCamera"
    
    # 设置相机目标
    camera.rotation_euler = (math.radians(90), 0, 0)
    
    # 创建动画
    camera.animation_data_create()
    action = camera.animation_data.action = bpy.data.actions.new(name="OrbitAnimation")
    
    for i in range(frames):
        angle = (2 * math.pi * i) / frames
        
        # 设置帧
        bpy.context.scene.frame_set(i)
        
        # 计算新位置
        x = radius * math.cos(angle)
        y = radius * math.sin(angle)
        
        # 设置位置
        camera.location = (x, y, 2)
        camera.keyframe_insert(data_path="location", frame=i)
    
    # 设置相机
    bpy.context.scene.camera = camera
    
    return camera
```

### 创建相机切换动画

```python
def create_camera_switch_animation(camera1, camera2, switch_frame):
    """创建两个相机之间的切换动画"""
    scene = bpy.context.scene
    
    # 设置初始相机
    scene.camera = camera1
    
    # 在切换帧设置相机
    scene.frame_set(switch_frame)
    scene.camera = camera2
    
    # 插入关键帧
    camera2.keyframe_insert(data_path="location", frame=switch_frame)
    camera2.keyframe_insert(data_path="rotation_euler", frame=switch_frame)
    
    # 添加淡入淡出效果
    scene.frame_set(switch_frame - 1)
    camera1.hide_render = False
    camera1.keyframe_insert(data_path="hide_render", frame=switch_frame - 1)
    camera1.hide_render = True
    camera1.keyframe_insert(data_path="hide_render", frame=switch_frame)
```

### 设置相机跟随

```python
def setup_camera_follow(camera_obj, target_obj, offset=(0, -5, 2)):
    """设置相机跟随目标物体"""
    # 创建跟随约束
    constraint = camera_obj.constraints.new(type='TRACK_TO')
    constraint.target = target_obj
    constraint.track_axis = 'TRACK_NEGATIVE_Z'
    constraint.up_axis = 'UP_Y'
    
    # 创建复制位置约束
    loc_constraint = camera_obj.constraints.new(type='COPY_LOCATION')
    loc_constraint.target = target_obj
    loc_constraint.use_offset = True
    loc_constraint.head_tail = 0
    
    # 设置偏移
    camera_obj.location = (
        target_obj.location.x + offset[0],
        target_obj.location.y + offset[1],
        target_obj.location.z + offset[2]
    )
```

## 使用场景

- **场景相机配置**：设置主相机和渲染相机
- **相机动画**：创建相机移动和切换动画
- **相机特效**：设置景深和运动模糊
- **多相机系统**：管理多个相机和视角切换
- **自动化拍摄**：在脚本中控制相机进行自动化渲染

## 注意事项

- 相机操作需要先选中相机对象
- 相机属性更改后需要更新视图
- 多相机切换时注意渲染输出的一致性


---
## bpy_ops_brush

# bpy.ops.brush 模块

画笔操作模块，用于管理和操作绘画画笔。

## 概述

bpy.ops.brush 模块包含用于管理绘画画笔、画笔预设和画笔设置的运算符。这些运算符用于创建、编辑、删除画笔，以及管理画笔库。

## 常用操作

### 画笔选择和管理

```python
import bpy

# 激活画笔
bpy.ops.brush.active_index_set(brush_type='SCULPT', index=0)

# 获取当前活动画笔
active_brush = bpy.context.tool_settings.sculpt.brush

# 复制画笔
bpy.ops.brush.add()
bpy.ops.brush.duplicate(name="New Brush")

# 删除画笔
bpy.ops.brush.remove()

# 重命名画笔
bpy.ops.brush.rename(name="Renamed Brush")
```

### 画笔预设

```python
# 保存画笔预设
bpy.ops.brush.preset_add(name="My Preset")

# 删除画笔预设
bpy.ops.brush.preset_remove(name="My Preset")

# 调用画笔预设菜单
bpy.ops.brush.preset_menu()
```

### 画笔图标

```python
# 添加自定义画笔图标
bpy.ops.brush.icon_add(filepath="//brush_icon.png")

# 清除画笔图标
bpy.ops.brush.icon_clear()
```

### 纹理管理

```python
# 添加纹理到画笔
bpy.ops.brush.texture_add()

# 移除画笔纹理
bpy.ops.brush.texture_remove()

# 创建纹理
bpy.ops.brush.texture_slot_new()
```

### 曲线管理

```python
# 添加曲线到画笔
bpy.ops.brush.curve_brush_add()

# 清除画笔曲线
bpy.ops.brush.curve_brush_clear()

# 插槽操作
bpy.ops.brush.curve_brush_slot_stroke()
bpy.ops.brush.curve_brush_slot_strength()
bpy.ops.brush.curve_brush_slot_color()
```

## 实际应用示例

### 创建自定义画笔

```python
def create_custom_brush(name, size=50, strength=1.0, color=(1, 0.5, 0)):
    """创建自定义画笔"""
    brush_type = bpy.context.tool_settings.sculpt.brush
    
    # 复制当前画笔
    bpy.ops.sculpt.brush_add(name=name)
    
    # 设置画笔属性
    brush = bpy.context.tool_settings.sculpt.brush
    brush.size = size
    brush.strength = strength
    
    # 设置颜色
    brush.cursor_color_add(color)
    
    return brush
```

### 画笔库管理

```python
def export_brush_library(filepath):
    """导出画笔库"""
    brushes = []
    for brush in bpy.data.brushes:
        if brush.sculpt:
            brush_data = {
                'name': brush.name,
                'size': brush.size,
                'strength': brush.strength,
                'hardness': brush.hardness,
            }
            brushes.append(brush_data)
    
    # 保存到文件
    import json
    with open(filepath, 'w', encoding='utf-8') as f:
        json.dump(brushes, f, ensure_ascii=False, indent=2)

def import_brush_library(filepath):
    """导入画笔库"""
    import json
    
    with open(filepath, 'r', encoding='utf-8') as f:
        brushes = json.load(f)
    
    for brush_data in brushes:
        # 激活雕塑工具
        bpy.ops.sculpt.brush_add(name=brush_data['name'])
        
        # 应用设置
        brush = bpy.context.tool_settings.sculpt.brush
        brush.size = brush_data.get('size', 50)
        brush.strength = brush_data.get('strength', 1.0)
        brush.hardness = brush_data.get('hardness', 0.5)
```

### 批量画笔设置

```python
def batch_update_brush_settings(size_factor=1.0, strength_factor=1.0):
    """批量更新所有画笔设置"""
    for brush in bpy.data.brushes:
        if brush.sculpt:
            brush.size = max(1, int(brush.size * size_factor))
            brush.strength = min(1.0, brush.strength * strength_factor)

def reset_all_brushes():
    """重置所有画笔为默认值"""
    for brush in bpy.data.brushes:
        if brush.sculpt:
            brush.size = 50
            brush.strength = 1.0
            brush.hardness = 0.5
```

## 使用场景

- **画笔创建和管理**：创建自定义画笔和预设
- **画笔库操作**：导入导出画笔库
- **批量设置**：批量调整画笔参数
- **纹理和曲线管理**：管理画笔的纹理和力量曲线
- **自动化脚本**：在脚本中动态创建和配置画笔

## 注意事项

- 画笔操作需要先激活相应的绘制模式
- 不同绘制模式（雕塑、绘制、纹理绘制）的画笔是分开的
- 画笔设置更改后可能需要重绘笔触预览


---
## bpy_ops_node

# bpy.ops.node 模块

节点操作模块，用于在节点编辑器中创建和管理节点。

## 概述

bpy.ops.node 模块包含用于在各种节点编辑器（材质、合成、几何节点）中操作节点的运算符。这些运算符控制节点的创建、连接、编辑和布局。

## 常用操作

### 节点创建

```python
import bpy

# 添加节点
bpy.ops.node.add_node(type='ShaderNodeBsdfDiffuse', use_transform=True)

# 添加图像节点
bpy.ops.node.add_node(type='ShaderNodeTexImage', use_transform=True)

# 添加混合节点
bpy.ops.node.add_node(type='ShaderNodeMixShader', use_transform=True)

# 添加输出节点
bpy.ops.node.add_node(type='ShaderNodeOutputMaterial', use_transform=True)

# 添加组
bpy.ops.node.group_make()

# 添加组输入
bpy.ops.node.group_input_add()

# 添加组输出
bpy.ops.node.group_output_add()
```

### 节点选择

```python
# 全选
bpy.ops.node.select_all(action='SELECT')
bpy.ops.node.select_all(action='DESELECT')
bpy.ops.node.select_all(action='INVERT')

# 选择节点
bpy.ops.node.select(extend=False)

# 取消选择
bpy.ops.node.select(extend=False, deselect_all=True)

# 选择同类型
bpy.ops.node.select_same_type()

# 选择连接
bpy.ops.node.select_linked()

# 选择输入输出
bpy.ops.node.select_inactive()
bpy.ops.node.select_active()

# 边界框选择
bpy.ops.node.select_border(gesture_mode=0, extend=False)

# 套索选择
bpy.ops.node.select_lasso(path=None, mode='SET')
```

### 节点操作

```python
# 删除节点
bpy.ops.node.delete()

# 剪切节点
bpy.ops.node.cut(linked=False)

# 复制节点
bpy.ops.node.duplicate(linked=False)

# 移动节点
bpy.ops.node.move(type='CHECK_LINKS')

# 粘贴节点
bpy.ops.node.paste()
```

### 链接操作

```python
# 链接节点
bpy.ops.node.link(
    from_node='Diffuse',
    from_socket='BSDF',
    to_node='Mix',
    to_socket='Fac'
)

# 断开链接
bpy.ops.node.link(
    from_node='Diffuse',
    from_socket='BSDF',
    to_node='Mix',
    to_socket='Fac'
)

# 断开所有链接
bpy.ops.node.links_cut(path=None)

# 重新链接
bpy.ops.node.reconnect_all()
```

### 节点编辑

```python
# 重命名节点
bpy.ops.node.rename(name='NewName')

# 隐藏节点
bpy.ops.node.hide_toggle()

# 展开节点
bpy.ops.node.node_preview()

# 缩放节点
bpy.ops.node.resize()
```

### 视图操作

```python
# 框选缩放
bpy.ops.node.view_selected()

# 查看全部
bpy.ops.node.view_all()

# 放大
bpy.ops.node.zoom_in()

# 缩小
bpy.ops.node.zoom_out()

# 背景选择
bpy.ops.node.backimage_add(filepath='//image.png')

# 清除背景
bpy.ops.node.backimage_remove()
```

### 侧边栏

```python
# 切换侧边栏
bpy.ops.node.sidebar_toggle()

# 显示属性
bpy.ops.node.properties()

# 显示节点颜色
bpy.ops.node.options_toggle()
```

### 搜索

```python
# 搜索节点
bpy.ops.node.search(

# 添加到收藏
bpy.ops.node.add_favorites()
```

## 实际应用示例

### 创建材质节点设置

```python
def create_material_nodes(material_name='NewMaterial'):
    """创建新材质的节点"""
    # 创建材质
    mat = bpy.data.materials.new(name=material_name)
    mat.use_nodes = True
    tree = mat.node_tree
    
    # 清理默认节点
    for node in tree.nodes:
        tree.nodes.remove(node)
    
    # 创建Principled BSDF
    principled = tree.nodes.new('ShaderNodeBsdfPrincipled')
    principled.location = (0, 0)
    
    # 创建输出
    output = tree.nodes.new('ShaderNodeOutputMaterial')
    output.location = (300, 0)
    
    # 连接节点
    tree.links.new(principled.outputs['BSDF'], output.inputs['Surface'])
    
    return mat
```

### 创建程序化纹理

```python
def create_procedural_texture():
    """创建程序化纹理节点"""
    # 获取活动材质
    mat = bpy.context.active_object.active_material
    tree = mat.node_tree
    
    # 创建坐标
    tex_coord = tree.nodes.new('ShaderNodeTexCoord')
    tex_coord.location = (-600, 0)
    
    # 创建映射
    mapping = tree.nodes.new('ShaderNodeMapping')
    mapping.location = (-400, 0)
    
    # 创建噪声
    noise = tree.nodes.new('ShaderNodeTexNoise')
    noise.location = (-200, 0)
    noise.noise_scale = 5.0
    
    # 创建颜色渐变
    gradient = tree.nodes.new('ShaderNodeValToRGB')
    gradient.location = (0, 0)
    
    # 创建Principled BSDF
    principled = tree.nodes.new('ShaderNodeBsdfPrincipled')
    principled.location = (200, 0)
    
    # 创建输出
    output = tree.nodes.new('ShaderNodeOutputMaterial')
    output.location = (400, 0)
    
    # 连接节点
    tree.links.new(tex_coord.outputs['Generated'], mapping.inputs['Vector'])
    tree.links.new(mapping.outputs['Vector'], noise.inputs['Vector'])
    tree.links.new(noise.outputs['Fac'], gradient.inputs['Fac'])
    tree.links.new(gradient.outputs['Color'], principled.inputs['Base Color'])
    tree.links.new(principled.outputs['BSDF'], output.inputs['Surface'])
    
    return tree
```

### 创建混合材质

```python
def create_blended_material(mat_a, mat_b, mix_factor=0.5):
    """创建两种材质的混合"""
    # 获取第一个材质的节点树
    tree = mat_a.node_tree
    
    # 创建混合节点
    mix = tree.nodes.new('ShaderNodeMixShader')
    mix.location = (300, 0)
    mix.inputs['Fac'].default_value = mix_factor
    
    # 清理节点并重新连接
    output = tree.nodes.get('Material Output')
    
    # 假设两个材质已存在
    bsdf_a = tree.nodes.get('Principled BSDF')
    bsdf_b = mat_b.node_tree.nodes.get('Principled BSDF')
    
    if bsdf_b:
        bsdf_b_copy = tree.nodes.new('ShaderNodeBsdfPrincipled')
        bsdf_b_copy.location = (-200, 200)
        bsdf_b_copy.inputs['Base Color'].default_value = bsdf_b.inputs['Base Color'].default_value
        
        tree.links.new(bsdf_b_copy.outputs['BSDF'], mix.inputs[2])
    
    tree.links.new(bsdf_a.outputs['BSDF'], mix.inputs[1])
    tree.links.new(mix.outputs['Shader'], output.inputs['Surface'])
    
    return mix
```

### 创建几何节点设置

```python
def setup_geometry_nodes():
    """设置几何节点"""
    # 添加几何节点修改器
    obj = bpy.context.active_object
    geo_mod = obj.modifiers.new(name='GeometryNodes', type='NODES')
    
    # 获取节点组
    node_group = geo_mod.node_group
    
    # 清理默认节点
    for node in node_group.nodes:
        node_group.nodes.remove(node)
    
    # 创建组输入
    group_input = node_group.nodes.new('NodeGroupInput')
    group_input.location = (-200, 0)
    
    # 创建组输出
    group_output = node_group.nodes.new('NodeGroupOutput')
    group_output.location = (400, 0)
    
    # 创建几何节点
    set_position = node_group.nodes.new('GeometryNodeSetPosition')
    set_position.location = (100, 0)
    
    # 创建输入属性
    position_in = node_group.interface.new_socket(name='Position', in_out='INPUT')
    position_in.socket_type = 'NodeSocketVector')
    
    # 连接节点
    node_group.links.new(group_input.outputs['Position'], set_position.inputs['Position'])
    node_group.links.new(set_position.outputs['Geometry'], group_output.inputs['Geometry'])
    
    return node_group
```

### 批量更新节点颜色

```python
def batch_update_node_colors(material, color_map):
    """批量更新节点颜色"""
    tree = material.node_tree
    
    for node in tree.nodes:
        if node.name in color_map:
            node.color = color_map[node.name]
```

### 创建节点组

```python
def create_node_group():
    """创建可复用节点组"""
    # 创建新节点组
    group = bpy.data.node_groups.new(name='CustomGroup', type='SHADER')
    tree = group.nodes
    
    # 清理默认节点
    for node in tree.nodes:
        tree.nodes.remove(node)
    
    # 创建输入
    group_input = tree.nodes.new('NodeGroupInput')
    group_input.location = (-200, 0)
    
    # 创建输出
    group_output = tree.nodes.new('NodeGroupOutput')
    group_output.location = (300, 0)
    
    # 添加自定义输入
    group.interface.new_socket(name='Color', in_out='INPUT', socket_type='NodeSocketColor')
    group.interface.new_socket(name='Fac', in_out='INPUT', socket_type='NodeSocketFloat')
    
    # 添加自定义输出
    group.interface.new_socket(name='Shader', in_out='OUTPUT', socket_type='NodeSocketShader')
    
    # 创建节点
    principled = tree.nodes.new('ShaderNodeBsdfPrincipled')
    principled.location = (0, 0)
    
    # 连接节点
    tree.links.new(group_input.outputs['Color'], principled.inputs['Base Color'])
    tree.links.new(principled.outputs['BSDF'], group_output.inputs['Shader'])
    
    return group
```

## 使用场景

- **材质创建**：使用节点创建复杂材质
- **程序化纹理**：创建可调节的纹理效果
- **几何节点**：程序化修改几何体
- **合成设置**：创建后期处理效果
- **节点复用**：创建自定义节点组
- **批处理**：自动化节点操作

## 注意事项

- 节点操作需要正确的节点类型名称
- 链接操作需要指定正确的Socket
- 节点编辑器有多种类型（材质、合成、几何）
- 节点组可以跨材质复用
- 大量节点影响性能
- 搜索功能可以帮助快速找到节点


---
## bpy_ops_node_add

# bpy.ops.node - 节点添加操作模块

bpy.ops.node模块提供了节点编辑器中添加和操作节点的功能，用于创建各种类型的节点。

## 概述

节点编辑器是Blender中用于创建材质、合成和几何节点树的重要工具。这个模块提供了添加、连接和编辑节点的操作。

## 核心功能

### 添加节点

```python
import bpy

def add_input_node(node_type):
    """添加输入节点"""
    bpy.ops.node.add_node(
        type=node_type,
        use_transform=True
    )

def add_output_node():
    """添加输出节点"""
    bpy.ops.node.add_node(
        type='ShaderNodeOutputMaterial',
        use_transform=True
    )

def add_texture_node():
    """添加纹理节点"""
    bpy.ops.node.add_node(
        type='ShaderNodeTexImage',
        use_transform=True
    )

def add_color_node():
    """添加颜色节点"""
    bpy.ops.node.add_node(
        type='ShaderNodeRGB',
        use_transform=True
    )

def add_value_node():
    """添加值节点"""
    bpy.ops.node.add_node(
        type='ShaderNodeValue',
        use_transform=True
    )

def add_vector_node():
    """添加向量节点"""
    bpy.ops.node.add_node(
        type='ShaderNodeVector',
        use_transform=True
    )
```

### 节点操作

```python
def delete_node():
    """删除节点"""
    bpy.ops.node.delete()

def duplicate_node():
    """复制节点"""
    bpy.ops.node.duplicate()

def detach_node():
    """分离节点"""
    bpy.ops.node.detach()

def hide_node():
    """隐藏节点"""
    bpy.ops.node.hide_toggle()

def mute_node():
    """静音节点"""
    bpy.ops.node.mute_toggle()
```

### 节点连接

```python
def link_nodes():
    """链接节点"""
    bpy.ops.node.link(

        type='NodeSocketVector',
        from_socket='Emission',
        to_socket='Surface'
    )

def unlink_nodes():
    """取消链接"""
    bpy.ops.node.links_detach()

def add_reroute():
    """添加路由点"""
    bpy.ops.node.add_reroute()
```

## 实用函数

### 选择操作

```python
def select_all():
    """全选"""
    bpy.ops.node.select_all(action='SELECT')

def deselect_all():
    """取消全选"""
    bpy.ops.node.select_all(action='DESELECT')

def select_hierarchy():
    """选择层级"""
    bpy.ops.node.select_hierarchy(
        direction='CHILD',
        extend=False
    )

def select_border():
    """框选"""
    bpy.ops.node.select_border(
        extend=False,
        tweak=False
    )
```

### 视口操作

```python
def view_center_cursor():
    """将视口居中到光标"""
    bpy.ops.node.view_center_cursor()

def view_selected():
    """查看选中"""
    bpy.ops.node.view_selected()

def zoom_in():
    """放大"""
    bpy.ops.node.zoom_in()

def zoom_out():
    """缩小"""
    bpy.ops.node.zoom_out()
```

### 节点组

```python
def join_nodes():
    """加入节点组"""
    bpy.ops.node.join_group()

def group_insert():
    """插入组"""
    bpy.ops.node.group_insert()

def group_ungroup():
    """取消组"""
    bpy.ops.node.group_ungroup()
```

## 常见问题排查

### 节点添加失败

类型错误：
- 确认节点类型名称正确
- 检查节点是否可用
- 验证编辑器类型匹配

### 链接问题

连接失败：
- 确认Socket类型兼容
- 检查输入输出是否存在
- 验证连接方向正确


---
## bpy_ops_properties

# bpy.ops.buttons - 属性面板操作模块

bpy.ops.buttons模块提供了属性编辑器（ properties editor）中的操作功能，用于编辑和修改对象属性。

## 概述

属性编辑器是Blender中用于查看和编辑对象属性的核心面板。这个模块提供了在属性面板中进行编辑的操作。

## 核心功能

### 属性编辑

```python
import bpy

def edit_property(data_path, property_name, value):
    """编辑属性"""
    bpy.ops.buttons.edit(
        data_path=data_path,
        property=property_name,
        value=value
    )

def reset_property(data_path, property_name):
    """重置属性"""
    bpy.ops.buttons.reset(
        data_path=data_path,
        property=property_name
    )

def copy_property(data_path, property_name):
    """复制属性"""
    bpy.ops.buttons.copy(
        data_path=data_path,
        property=property_name
    )
```

### 上下文操作

```python
def context_tabs_iterate():
    """切换上下文标签"""
    bpy.ops.buttons.context_tabs_iterate()

def context_cycle():
    """循环上下文"""
    bpy.ops.buttons.context_cycle()
```

## 实用函数

### 数值调整

```python
def number_edit(data_path, property_name, value, soft_min, soft_max):
    """编辑数值属性"""
    bpy.ops.buttons.number_edit(
        data_path=data_path,
        property=property_name,
        value=value,
        soft_min=soft_min,
        soft_max=soft_max
    )

def number_factor(data_path, property_name, factor):
    """数值乘数"""
    bpy.ops.buttons.number_factor(
        data_path=data_path,
        property=property_name,
        factor=factor
    )
```

### 文件选择

```python
def file_browse(filepath):
    """文件浏览"""
    bpy.ops.buttons.file_browse(filepath=filepath)

def directory_browse(directory):
    """目录浏览"""
    bpy.ops.buttons.directory_browse(directory=directory)
```

## 常见问题排查

### 属性编辑失败

路径错误：
- 确认data_path正确
- 检查属性是否存在
- 验证值类型匹配

### 上下文问题

面板不响应：
- 确认选中对象
- 检查属性面板类型
- 验证编辑模式正确


