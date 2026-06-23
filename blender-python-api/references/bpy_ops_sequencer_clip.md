# bpy_ops_sequencer_clip

---
## bpy_ops_sequencer

# bpy.ops.sequencer 模块

序列编辑器操作模块，用于管理视频编辑的时间线和媒体片段。

## 概述

bpy.ops.sequencer 模块包含用于在序列编辑器中操作视频、音频和图像序列的运算符。序列编辑器允许你组合多个媒体片段、应用特效、调整时间并创建最终的视频输出。

## 常用操作

### 片段操作

```python
import bpy

# 剪切片段
bpy.ops.sequencer.cut(frame=100, type='SOFT', side='LEFT')

# 删除选中片段
bpy.ops.sequencer.delete()

# 复制片段
bpy.ops.sequencer.duplicate()

# 分离片段
bpy.ops.sequencer.split(type='EDITOR', frame=100)

# 拼接片段
bpy.ops.sequencer.snap(frame=100)

# 清除间隙
bpy.ops.sequencer.gap_remove(all=False)

# 填充间隙
bpy.ops.sequencer.gap_insert()
```

### 片段移动

```python
# 移动片段
bpy.ops.sequencer.move(type='START', frame=100)

# 左移一帧
bpy.ops.sequencer.offset_left()

# 右移一帧
bpy.ops.sequencer.offset_right()

# 移动到结束
bpy.ops.sequencer.move_to_end()

# 交换片段
bpy.ops.sequencer.swap()

# 滑动片段
bpy.ops.sequencer.slide()
```

### 选择操作

```python
# 全选
bpy.ops.sequencer.select_all(action='SELECT')

# 取消全选
bpy.ops.sequencer.select_all(action='DESELECT')

# 反选
bpy.ops.sequencer.select_all(action='INVERT')

# 选择左侧
bpy.ops.sequencer.select_lefthand()

# 选择右侧
bpy.ops.sequencer.select_righthand()

# 框选
bpy.ops.sequencer.select_border(gesture_mode=0, xmin=100, xmax=200, ymin=50, ymax=150)

# 选择链接片段
bpy.ops.sequencer.select_linked()

# 选择同组
bpy.ops.sequencer.select_grouped(type='TYPE')
```

### 播放控制

```python
# 播放
bpy.ops.sequencer.play( reverse=False, sync=False )

# 暂停
bpy.ops.sequencer.play()

# 跳转到开始
bpy.ops.sequencer.recenter()

# 跳转到上一个标记
bpy.ops.sequencer.prev_marker()

# 跳转到下一个标记
bpy.ops.sequencer.next_marker()
```

### 特效操作

```python
# 添加特效
bpy.ops.sequencer.effect_strip_add(type='CROSS', frame_start=100, frame_end=200)

# 应用颜色分级
bpy.ops.sequencer.color_tag_set(tag='RED')

# 应用速度控制
bpy.ops.sequencer.speed_factor_set(factor=1.0)

# 设置混合模式
bpy.ops.sequencer.blend_mode_set(mode='REPLACE')

# 设置不透明度
bpy.ops.sequencer.opacity_set(opacity=1.0)

# 设置音量
bpy.ops.sequencer.volume_set(volume=1.0)
```

### 时间线操作

```python

# 标记时间点
bpy.ops.sequencer.marker_add(name='Marker')

# 重命名标记
bpy.ops.sequencer.rename_marker(name='NewName')

# 删除标记
bpy.ops.sequencer.delete_marker()

# 添加工作区域
bpy.ops.sequencer.view_toggle()

# 设置预览范围
bpy.ops.sequencer.view_all()

# 设置显示比例
bpy.ops.sequencer.zoom(10.0)

# 吸附到帧
bpy.ops.sequencer.snap(frame=100)
```

## 实际应用示例

### 创建基本视频序列

```python
def create_video_sequence(filepath, start_frame=1, channel=1):
    """创建视频序列"""
    # 确保在序列编辑器中
    bpy.context.area.type = 'SEQUENCE_EDITOR'
    
    # 添加视频片段
    bpy.ops.sequencer.movie_strip_add(
        filepath=filepath,
        frame_start=start_frame,
        channel=channel
    )
    
    return bpy.context.active_strip
```

### 创建多轨混合

```python
def create_multi_track_sequence(video_files, audio_files):
    """创建多轨混合序列"""
    bpy.context.area.type = 'SEQUENCE_EDITOR'
    
    # 创建视频轨道
    for i, video_file in enumerate(video_files):
        bpy.ops.sequencer.movie_strip_add(
            filepath=video_file,
            frame_start=1,
            channel=i + 1
        )
    
    # 创建音频轨道
    for i, audio_file in enumerate(audio_files):
        bpy.ops.sequencer.sound_strip_add(
            filepath=audio_file,
            frame_start=1,
            channel=len(video_files) + i + 1
        )
```

### 创建转场效果

```python
def create_transition(strip1_name, strip2_name, transition_type='CROSS'):
    """创建转场效果"""
    # 获取片段
    seq_editor = bpy.context.scene.sequence_editor
    strips = seq_editor.sequences_all
    
    strip1 = strips.get(strip1_name)
    strip2 = strips.get(strip2_name)
    
    if strip1 and strip2:
        # 计算重叠区域
        overlap_start = max(strip1.frame_final_end, strip2.frame_final_start)
        overlap_end = min(strip1.frame_final_end, strip2.frame_final_end)
        
        if overlap_start < overlap_end:
            # 添加转场
            bpy.ops.sequencer.effect_strip_add(
                type=transition_type,
                frame_start=overlap_start,
                frame_end=overlap_end,
                channel=strip2.channel + 1
            )
```

### 创建速度变化

```python
def adjust_playback_speed(strip_name, speed_factor):
    """调整播放速度"""
    seq_editor = bpy.context.scene.sequence_editor
    strip = seq_editor.sequences_all.get(strip_name)
    
    if strip:
        # 计算新长度
        original_length = strip.frame_end - strip.frame_start + 1
        new_length = int(original_length / speed_factor)
        
        # 应用速度控制
        bpy.ops.sequencer.strip_modifier_add(type='SPEED')
        
        # 获取速度控制修改器
        for modifier in strip.modifiers:
            if modifier.type == 'SPEED':
                modifier.speed_factor = speed_factor
                modifier.use_frame_blending = True
```

### 创建颜色校正

```python
def apply_color_correction(strip_name, contrast=1.0, saturation=1.0):
    """应用颜色校正"""
    seq_editor = bpy.context.scene.sequence_editor
    strip = seq_editor.sequences_all.get(strip_name)
    
    if strip:
        # 添加颜色校正效果
        bpy.ops.sequencer.strip_modifier_add(type='COLOR_BALANCE')
        
        # 获取颜色平衡修改器
        for modifier in strip.modifiers:
            if modifier.type == 'COLOR_BALANCE':
                modifier.color_balance.contrast = contrast
                modifier.color_balance.saturation = saturation
```

### 创建文本叠加

```python
def add_text_overlay(text, start_frame, end_frame, channel=1):
    """添加文本叠加"""
    bpy.ops.sequencer.effect_strip_add(
        type='TEXT',
        frame_start=start_frame,
        frame_end=end_frame,
        channel=channel
    )
    
    # 设置文本内容
    text_strip = bpy.context.active_strip
    text_strip.text = text
    text_strip.font_size = 24
    text_strip.color = (1, 1, 1, 1)
```

### 创建画面合成

```python
def create_composite(video_strip, overlay_strip, blend_mode='ALPHA_OVER'):
    """创建画面合成"""
    # 确保覆盖层在视频上方
    if overlay_strip.channel <= video_strip.channel:
        overlay_strip.channel = video_strip.channel + 1
    
    # 设置混合模式
    overlay_strip.blend_method = blend_mode
    
    # 调整位置和大小
    bpy.ops.sequencer.select_all(action='DESELECT')
    overlay_strip.select = True
    bpy.context.scene.sequence_editor.active_strip = overlay_strip
    
    # 移动到正确位置
    bpy.ops.sequencer.move(
        type='START',
        frame=video_strip.frame_start
    )
```

### 创建音频淡入淡出

```python
def add_audio_fades(strip_name, fade_in=1.0, fade_out=1.0):
    """添加音频淡入淡出"""
    seq_editor = bpy.context.scene.sequence_editor
    strip = seq_editor.sequences_all.get(strip_name)
    
    if strip and strip.type == 'SOUND':
        # 添加淡入
        if fade_in > 0:
            bpy.ops.sequencer.strip_modifier_add(type='SOUND_FADE_IN')
            for modifier in strip.modifiers:
                if modifier.type == 'SOUND_FADE_IN':
                    modifier.frame_start = strip.frame_start
                    modifier.frame_end = strip.frame_start + fade_in
        
        # 添加淡出
        if fade_out > 0:
            bpy.ops.sequencer.strip_modifier_add(type='SOUND_FADE_OUT')
            for modifier in strip.modifiers:
                if modifier.type == 'SOUND_FADE_OUT':
                    modifier.frame_start = strip.frame_end - fade_out
                    modifier.frame_end = strip.frame_end
```

### 创建画面稳定

```python
def add_stabilization(strip_name, clip_source):
    """添加画面稳定"""
    seq_editor = bpy.context.scene.sequence_editor
    strip = seq_editor.sequences_all.get(strip_name)
    
    if strip:
        # 添加稳定器效果
        bpy.ops.sequencer.strip_modifier_add(type='STABILIZE_2D')
        
        # 获取稳定器修改器
        for modifier in strip.modifiers:
            if modifier.type == 'STABILIZE_2D':
                modifier.clip = clip_source
                modifier.sensor_width = 32
                modifier.sensor_height = 18
                modifier.focal_length = 35
```

## 使用场景

- **视频编辑**：组合和编辑多个视频片段
- **音频混合**：混合多个音轨和音效
- **转场效果**：创建平滑的场景转换
- **颜色校正**：调整视频色彩和对比度
- **特效叠加**：添加文字、图形和视觉特效
- **速度调整**：创建慢动作或快进效果
- **画面合成**：合成多个视频层
- **音频处理**：调整音量、淡入淡出

## 注意事项

- 序列编辑器需要在正确的区域类型下操作
- 片段操作前需要确保选中目标片段
- 效果叠加需要考虑渲染顺序
- 混合模式影响最终输出效果
- 速度变化会改变片段长度
- 大文件序列需要足够的内存
- 预览时注意帧率设置
- 导出前应检查时间线完整性
- 特效链不要太长以免影响性能
- 定期保存工作进度


---
## bpy_ops_sequencer_strip_modifier

# bpy.ops.sequencer - 条带修改器操作模块

Blender序列器条带修改器操作符，用于管理视频序列条带的修改器。

## 概述

`bpy.ops.sequencer.strip_modifier` 模块提供用于在Blender序列器中对条带应用和修改修改器的功能，包括颜色校正、变换、模糊等效果。

## 核心功能

### 修改器管理

```python
import bpy

# 添加修改器
bpy.ops.sequencer.strip_modifier_add(
    type='COLOR_BALANCE'
)

# 添加颜色平衡修改器
bpy.ops.sequencer.strip_modifier_add(
    type='COLOR_BALANCE'
)

# 添加曲线修改器
bpy.ops.sequencer.strip_modifier_add(
    type='CURVES'
)

# 添加亮度对比度修改器
bpy.ops.sequencer.strip_modifier_add(
    type='BRIGHT_CONTRAST'
)

# 添加色调映射修改器
bpy.ops.sequencer.strip_modifier_add(
    type='TONEMAP'
)

# 添加模糊修改器
bpy.ops.sequencer.strip_modifier_add(
    type='BLUR'
)

# 添加变换修改器
bpy.ops.sequencer.strip_modifier_add(
    type='TRANSFORM'
```

### 修改器选择

```python
# 选择修改器
bpy.ops.sequencer.strip_modifier_select(
    strip='STRIP',
    strip_index=0,
    modifier_index=0,
    extend=False
)

# 扩展选择
bpy.ops.sequencer.strip_modifier_select(
    strip='STRIP',
    strip_index=0,
    modifier_index=1,
    extend=True
)
```

### 修改器移除

```python
# 移除修改器
bpy.ops.sequencer.strip_modifier_remove(
    strip='STRIP',
    strip_index=0,
    modifier_index=0
)

# 移除所有修改器
def remove_all_modifiers(strip):
    while strip.modifiers:
        bpy.context.scene.sequence_editor.active_strip = strip
        bpy.ops.sequencer.strip_modifier_remove(
            strip=strip.name,
            strip_index=0,
            modifier_index=0
        )
```

### 修改器移动

```python
# 上移修改器
bpy.ops.sequencer.strip_modifier_move(
    strip='STRIP',
    strip_index=0,
    modifier_index=1,
    direction='UP'
)

# 下移修改器
bpy.ops.sequencer.strip_modifier_move(
    strip='STRIP',
    strip_index=0,
    modifier_index=0,
    direction='DOWN'
)
```

### 修改器复制

```python
# 复制修改器
bpy.ops.sequencer.strip_modifier_copy(
    strip='STRIP',
    strip_index=0,
    modifier_index=0
)
```

## 实用函数

```python
def add_color_balance(strip):
    """添加颜色平衡修改器"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='COLOR_BALANCE')
    return strip.modifiers[-1]

def add_brightness_contrast(strip):
    """添加亮度对比度修改器"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='BRIGHT_CONTRAST')
    return strip.modifiers[-1]

def add_curves(strip):
    """添加曲线修改器"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='CURVES')
    return strip.modifiers[-1]

def add_blur(strip, size=10, angle=0):
    """添加模糊修改器"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='BLUR')
    modifier = strip.modifiers[-1]
    modifier.size_x = size
    modifier.size_y = size
    modifier.angle = angle
    return modifier

def add_transform(strip, scale_x=1.0, scale_y=1.0):
    """添加变换修改器"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='TRANSFORM')
    modifier = strip.modifiers[-1]
    modifier.scale_x = scale_x
    modifier.scale_y = scale_y
    return modifier

def add_crop(strip, left=0, right=0, top=0, bottom=0):
    """添加裁剪修改器"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='CROP')
    modifier = strip.modifiers[-1]
    modifier.left = left
    modifier.right = right
    modifier.top = top
    modifier.bottom = bottom
    return modifier

def apply_color_grading(strip, lift=(1, 1, 1), gamma=(1, 1, 1), gain=(1, 1, 1)):
    """应用色彩分级"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='COLOR_BALANCE')
    mod = strip.modifiers[-1]
    mod.color_balance.lift_factor = lift
    mod.color_balance.gamma_factor = gamma
    mod.color_balance.gain_factor = gain
    return mod

def create_vignette(strip, radius=0.5, softness=0.5):
    """创建暗角效果"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='WIPE')
    mod = strip.modifiers[-1]
    mod.wipe.angle = 0
    mod.wipe.blur = softness
    mod.wipe.edge_radius = radius
    return mod

def add_glow(strip, threshold=0.8, radius=15):
    """添加发光效果"""
    bpy.context.scene.sequence_editor.active_strip = strip
    bpy.ops.sequencer.strip_modifier_add(type='GAUSSIAN_BLUR')
    mod = strip.modifiers[-1]
    mod.blur.radius = radius
    return mod

def duplicate_modifier(strip, modifier_name):
    """复制修改器"""
    for i, mod in enumerate(strip.modifiers):
        if mod.name == modifier_name:
            bpy.context.scene.sequence_editor.active_strip = strip
            bpy.ops.sequencer.strip_modifier_select(
                strip=strip.name,
                strip_index=0,
                modifier_index=i
            )
            bpy.ops.sequencer.strip_modifier_copy(
                strip=strip.name,
                strip_index=0,
                modifier_index=i
            )
            return
```

## 常见问题排查

### 修改器不工作
```python
# 检查活动条带
strip = bpy.context.scene.sequence_editor.active_strip
print(f"活动条带: {strip}")

# 检查修改器
print(f"修改器数: {len(strip.modifiers) if strip else 0}")

if strip and strip.modifiers:
    for mod in strip.modifiers:
        print(f"修改器: {mod.name}, 类型: {mod.type}")
        print(f"  启用: {mod.show_render}")
        print(f"  显示: {mod.show_viewport}")
```

### 修改器顺序问题
```python
# 检查修改器顺序
strip = bpy.context.scene.sequence_editor.active_strip
for i, mod in enumerate(strip.modifiers):
    print(f"{i}: {mod.name} ({mod.type})")

# 上移修改器
bpy.ops.sequencer.strip_modifier_move(
    strip=strip.name,
    strip_index=0,
    modifier_index=1,
    direction='UP'
)

# 下移修改器
bpy.ops.sequencer.strip_modifier_move(
    strip=strip.name,
    strip_index=0,
    modifier_index=0,
    direction='DOWN'
)
```

### 修改器效果不明显
```python
# 检查修改器参数
strip = bpy.context.scene.sequence_editor.active_strip
if strip.modifiers:
    mod = strip.modifiers[0]
    print(f"修改器类型: {mod.type}")
    
    # 打印参数
    for prop in dir(mod):
        if not prop.startswith('_'):
            val = getattr(mod, prop)
            if not callable(val):
                print(f"  {prop}: {val}")

# 增强效果
if mod.type == 'BLUR':
    mod.size_x = 20
    mod.size_y = 20
elif mod.type == 'BRIGHT_CONTRAST':
    mod.contrast = 20
    mod.brightness = 0.1
```

### 修改器冲突
```python
# 检查冲突的修改器
strip = bpy.context.scene.sequence_editor.active_strip
for i, mod in enumerate(strip.modifiers):
    print(f"{i}: {mod.name} ({mod.type})")
    for j, other in enumerate(strip.modifiers):
        if i != j and mod.type == other.type:
            print(f"  冲突: {other.name}")

# 禁用冲突修改器
for mod in strip.modifiers:
    mod.show_render = False

# 重新启用
for mod in strip.modifiers:
    mod.show_render = True
```


---
## bpy_ops_video_seq

# bpy.ops.sequencer - 序列编辑器操作模块

bpy.ops.sequencer模块提供了视频序列编辑器（Video Sequence Editor）中的操作功能，用于管理视频、音频和图像序列。

## 概述

视频序列编辑器是Blender中用于非线性视频编辑的工具。这个模块提供了条带管理、效果应用和编辑操作的功能。

## 核心功能

### 条带管理

```python
import bpy

def add_movie(filepath, location=0):
    """添加视频条带"""
    bpy.ops.sequencer.movie_strip_add(
        filepath=filepath,
        frame_start=location,
        channel=1
    )

def add_image(filepath, frame_start=0):
    """添加图像条带"""
    bpy.ops.sequencer.image_strip_add(
        filepath=filepath,
        frame_start=frame_start,
        channel=1
    )

def add_audio(filepath, frame_start=0):
    """添加音频条带"""
    bpy.ops.sequencer.sound_strip_add(
        filepath=filepath,
        frame_start=frame_start,
        channel=2
    )

def add_text(name, frame_start, duration=30):
    """添加文字条带"""
    bpy.ops.sequencer.text_strip_add(
        text=name,
        frame_start=frame_start,
        frame_end=frame_start + duration
    )
```

### 条带操作

```python
def delete_strip():
    """删除条带"""
    bpy.ops.sequencer.delete()

def duplicate_strip():
    """复制条带"""
    bpy.ops.sequencer.duplicate()

def cut_strip(frame, type='SOFT'):
    """切割条带"""
    bpy.ops.sequencer.cut(
        frame=frame,
        type=type,
        side='BOTH'
    )

def slip_strip(offset):
    """滑动条带"""
    bpy.ops.sequencer.slip(offset=offset)

def snap_strip():
    """吸附条带"""
    bpy.ops.sequencer.snap(frame=bpy.context.scene.frame_current)
```

### 选择操作

```python
def select_all():
    """全选"""
    bpy.ops.sequencer.select_all(action='SELECT')

def deselect_all():
    """取消全选"""
    bpy.ops.sequencer.select_all(action='DESELECT')

def select_border(extend=False):
    """框选"""
    bpy.ops.sequencer.select_border(
        extend=extend,
        channel_select=False
    )

def select_active_side(side='LEFT'):
    """选择激活侧"""
    bpy.ops.sequencer.select_active_side(side=side)
```

## 实用函数

### 效果添加

```python
def add_effect(effect_type, strip1=None, strip2=None):
    """添加效果"""
    bpy.ops.sequencer.effect_strip_add(
        type=effect_type,
        frame_start=strip1.frame_start if strip1 else 0,
        channel=1
    )

def add_crossfade():
    """添加交叉淡化"""
    add_effect('CROSS')

def add_wipe():
    """添加擦除"""
    add_effect('WIPE')

def add_glow():
    """添加发光"""
    add_effect('GLOW')

def add_color_mix():
    """添加颜色混合"""
    add_effect('COLOR')
```

### 时间操作

```python
def set_frame_start(frame):
    """设置起始帧"""
    bpy.context.scene.frame_start = frame

def set_frame_end(frame):
    """设置结束帧"""
    bpy.context.scene.frame_end = frame

def set_render_fps(fps):
    """设置渲染帧率"""
    bpy.context.scene.render.fps = fps

def preview_range_set(start, end):
    """设置预览范围"""
    bpy.context.scene.frame_preview_start = start
    bpy.context.scene.frame_preview_end = end
```

## 常见问题排查

### 条带问题

加载失败：
- 确认文件路径正确
- 检查编解码器支持
- 验证文件完整性

### 效果问题

效果不显示：
- 确认输入条带存在
- 检查效果设置正确
- 验证通道层叠顺序


---
## bpy_ops_clip

# bpy.ops.clip - Clip Operations Module

剪辑操作模块，用于视频追踪、相机解算和运动导入。

## 概述

`bpy.ops.clip` 模块是Blender运动追踪工作区的核心操作模块，提供了视频素材加载、特征点检测、追踪、相机解算、三维重建等功能。该模块涵盖了从素材导入到最终数据应用的全流程。

## 核心功能

### 素材管理

```python
import bpy

# 打开视频素材
bpy.ops.clip.open(
    filepath='',
    directory='',
    files='',
    filter_blender=False,
    filter_image=True,
    filter_movie=True,
    filter_python=False,
    filter_font=False,
    filter_sound=False,
    filter_text=False,
    filter_archive=False,
    filter_btx=False,
    filter_alembic=False,
    filter_usd=False,
    filter_obj=False,
    filter_volume=False,
    filter_folder=True,
    filter_blenlib=False,
    filemode=9,
    relative_path=True,
    display_type='DEFAULT',
    sort_method=''
)

# 加载输入素材
bpy.ops.clip.load_input()

# 加载视频
bpy.ops.clip.load_movie()
```

### 标记点操作

```python
# 添加标记
bpy.ops.clip.add_marker(location=(0.0, 0.0))

# 点击位置添加标记
bpy.ops.clip.add_marker_at_click()

# 移动添加标记
bpy.ops.clip.add_marker_move()

# 滑动添加标记
bpy.ops.clip.add_marker_slide()

# 删除标记
bpy.ops.clip.delete_marker(confirm=True)

# 删除代理
bpy.ops.clip.delete_proxy()

# 删除轨迹
bpy.ops.clip.delete_track(confirm=True)

# 锁定轨迹
bpy.ops.clip.lock_tracks()

# 解锁轨迹
bpy.ops.clip.unlock_tracks()

# 禁用标记
bpy.ops.clip.disable_markers(action='DISABLE')
```

### 特征点检测

```python
# 检测特征点
bpy.ops.clip.detect_features(
    placement='FRAME',
    margin=16,
    threshold=0.5,
    min_distance=120
)
```

### 追踪操作

```python
# 选择操作
bpy.ops.clip.select()
bpy.ops.clip.select_all(action='SELECT')
bpy.ops.clip.select_border(gesture_mode=0, xmin=0, xmax=0, ymin=0, ymax=0)
bpy.ops.clip.select_circle(x=0, y=0, radius=0)
bpy.ops.clip.select_lasso(path=[], mode='SET')
bpy.ops.clip.select_grouped(type='', extend=False)
bpy.ops.clip.select_less()
bpy.ops.clip.select_more()

# 追踪
# 注意：具体的追踪操作需要结合追踪设置

# 清除轨迹路径
bpy.ops.clip.clear_track_path(action='REMAINED', clear_active=False)

# 过滤轨迹
bpy.ops.clip.filter_tracks(track_threshold=5.0)

# 清理轨迹
bpy.ops.clip.clean_tracks(frames=0, error=0.0, action='SELECT')

# 合并轨迹
bpy.ops.clip.merge_tracks()

# 平均轨迹
bpy.ops.clip.average_tracks(keep_original=True)

# 复制轨迹
bpy.ops.clip.copy_tracks()

# 粘贴轨迹
bpy.ops.clip.paste_tracks()

# 反转轨迹
bpy.ops.clip.track_mirror()

# 连接轨迹
bpy.ops.clip.join_tracks()

# 轨迹转空物体
bpy.ops.clip.track_to_empty()
```

### 平面追踪

```python
# 创建平面轨迹
bpy.ops.clip.create_plane_track()
```

### 相机解算

```python
# 应用解算缩放
bpy.ops.clip.apply_solution_scale(distance=0.0)

# 清除解算
bpy.ops.clip.clear_solution()

# 约束转f曲线
bpy.ops.clip.constraint_to_fcurve()

# 解算转约束
bpy.ops.clip.solver_as_constraint()

# 解算约束转f曲线
bpy.ops.clip.solver_constraint_to_fcurve()

# 束转网格
bpy.ops.clip.bundles_to_mesh()

# 曲线插值
bpy.ops.clip.graph_interpolate()
```

### 稳定化

```python
# 添加稳定器
bpy.ops.clip.stabilizer_add()

# 添加稳定器目标
bpy.ops.clip.stabilizer_add_target()

# 移除稳定器
bpy.ops.clip.stabilizer_remove()

# 选择稳定器
bpy.ops.clip.stabilizer_select()

# 设置稳定器为激活
bpy.ops.clip.stabilizer_set_active()
```

### 代理管理

```python
# 创建代理
bpy.ops.clip.make_proxy()

# 重建代理
bpy.ops.clip.rebuild_proxy()
```

### 关键帧操作

```python
# 插入关键帧
bpy.ops.clip.keyframe_insert()

# 删除关键帧
bpy.ops.clip.keyframe_delete()

# 设置解算关键帧
bpy.ops.clip.set_solver_keyframe()
```

### 轨迹颜色

```python
# 添加轨迹颜色预设
bpy.ops.clip.track_color_preset_add(name='', remove_name=False, remove_active=False, color=(0, 0, 0))
```

### 追踪对象管理

```python
# 创建追踪对象
bpy.ops.clip.tracking_object_create()

# 删除追踪对象
bpy.ops.clip.tracking_object_delete()
```

### 追踪设置

```python
# 添加追踪设置预设
bpy.ops.clip.tracking_settings_preset_add(name='', remove_name=False, remove_active=False)
```

### 相机预设

```python
# 添加相机预设
bpy.ops.clip.camera_preset_add(name='', remove_name=False, remove_active=False, use_focal_length=True)
```

### 用户预设

```python
# 添加用户预设
bpy.ops.clip.user_preset_add(name='', remove_name=False, remove_active=False)
```

### 帧操作

```python
# 跳转帧
bpy.ops.clip.frame_jump(position='PATHSTART')

# 改变帧
bpy.ops.clip.change_frame(frame=0)

# 设置场景帧
bpy.ops.clip.set_scene_frames()

# 预读取
bpy.ops.clip.prefetch()
```

### 视图操作

```python
# 视图操作
bpy.ops.clip.view_all()
bpy.ops.clip.view_center_cursor()
bpy.ops.clip.view_center_pick()
bpy.ops.clip.view_selected()
bpy.ops.clip.view_ndof()
bpy.ops.clip.view_zoom_in()
bpy.ops.clip.view_zoom_out()
bpy.ops.clip.view_zoom_ratio(ratio=1.0)

# 曲线视图
bpy.ops.clip.graph_view_all()
bpy.ops.clip.graph_center_current_frame()

# 时间轴视图
bpy.ops.clip.dopesheet_view_all()

# 设置活动剪辑
bpy.ops.clip.set_active_clip()
```

### 光标操作

```python
# 设置光标
bpy.ops.clip.cursor_set(location=(0.0, 0.0))

# 移动2D光标
bpy.ops.clip.move_2d_cursor(location=(0.0, 0.0))
```

### 曲线编辑

```python
# 曲线操作
bpy.ops.clip.graph_select(location=(0.0, 0.0), extend=False)
bpy.ops.clip.graph_select_all(action='SELECT')
bpy.ops.clip.graph_select_border(gesture_mode=0, xmin=0, xmax=0, ymin=0, ymax=0)
bpy.ops.clip.graph_delete_curve()
bpy.ops.clip.graph_delete_knot()
bpy.ops.clip.graph_follow_batch()
```

### 隐藏操作

```python
# 隐藏轨迹
bpy.ops.clip.hide_tracks(action='HIDE', unselected=False)
```

### 模式设置

```python
# 设置模式
bpy.ops.clip.mode_set(mode='TRACKING')
```

## 实用函数

```python
# 自动追踪选定标记
def auto_track_selected(backwards=False, sequence=False):
    # 设置追踪参数
    # 执行追踪
    pass

# 批量处理视频素材
def load_video_sequence(directory):
    # 加载目录下所有视频文件
    pass

# 创建完整的相机解算流程
def full_camera_solve():
    # 1. 加载素材
    # 2. 检测特征
    # 3. 追踪
    # 4. 解算相机
    pass

# 应用追踪数据到场景
def apply_tracking_to_scene():
    # 创建相机
    # 应用约束
    pass
```

## 常见问题排查

**问题：特征点检测失败**

- 检查视频素材是否清晰
- 调整检测参数（margin、threshold、min_distance）
- 尝试在对比度高的区域手动添加标记

**问题：追踪不稳定**

- 减小追踪搜索半径
- 启用子像素精度
- 清理误差大的追踪点
- 使用更好的特征点

**问题：相机解算失败**

- 确保有足够的特征点覆盖画面
- 检查是否有移动物体干扰
- 尝试增加关键帧数量
- 调整最小匹配数量

**问题：代理生成失败**

- 检查磁盘空间
- 确认输出路径可写
- 尝试重新生成代理

**问题：追踪数据无法应用**

- 确保解算已成功完成
- 检查场景中是否有冲突的相机
- 尝试重建场景结构

**问题：视频不同步**

- 检查素材帧率设置
- 确认场景帧率与素材匹配
- 调整时间偏移

**问题：稳定化效果不佳**

- 增加稳定器标记点数量
- 调整稳定化权重
- 尝试不同的稳定化模式


---
## bpy_ops_movieclip

# bpy.ops.clip - 剪辑操作模块

bpy.ops.clip模块提供了电影剪辑编辑器（movie clip editor）中的操作功能，用于跟踪和遮罩处理。

## 概述

电影剪辑编辑器用于视频素材的运动跟踪、稳定化和遮罩编辑。这个模块提供了跟踪操作和素材管理功能。

## 核心功能

### 素材管理

```python
import bpy

def open_movieclip(filepath):
    """打开电影剪辑"""
    bpy.ops.clip.open(filepath=filepath)

def reload_movieclip():
    """重新加载剪辑"""
    bpy.ops.clip.reload()

def add_marker(tracking_object=None):
    """添加标记"""
    bpy.ops.clip.add_marker_at_cursor(
        tracking_object=tracking_object
    )

def delete_marker():
    """删除标记"""
    bpy.ops.clip.delete_marker()
```

### 跟踪操作

```python
def track_markers(backwards=False):
    """跟踪标记"""
    bpy.ops.clip.track_markers(
        backwards=backwards,
        sequence=False
    )

def refine_markers():
    """精炼标记"""
    bpy.ops.clip.refine_markers()

def solve_camera():
    """解算相机"""
    bpy.ops.clip.solve_camera()

def solve_object():
    """解算对象"""
    bpy.ops.clip.solve_object()
```

## 实用函数

### 跟踪设置

```python
def set_track_pattern(frame):
    """设置跟踪图案"""
    bpy.ops.clip.set_track_pattern(frame=frame)

def set_search_margin(margin):
    """设置搜索边距"""
    bpy.context.space_data.clip.settings.search_size = margin

def set_keyframe_first():
    """设置首关键帧"""
    bpy.ops.clip.keyframe_insert(
        action='KEYFRAME',
        piano=False,
        focus=False
    )
```

### 遮罩操作

```python
def add_track_masks():
    """添加跟踪遮罩"""
    bpy.ops.clip.add_track_masks()

def edit_mask():
    """编辑遮罩"""
    bpy.ops.clip.set_active_mask()

def delete_mask():
    """删除遮罩"""
    bpy.ops.clip.remove_track_masks()
```

## 常见问题排查

### 跟踪问题

跟踪失败：
- 检查素材质量
- 调整搜索区域
- 确认特征点明显

### 解算问题

解算不良：
- 增加跟踪点数量
- 调整关键帧选择
- 检查相机参数


---
## bpy_ops_cachefile

# bpy.ops.cachefile - Cachefile Operations Module

缓存文件操作模块，用于加载和管理几何缓存文件。

## 概述

`bpy.ops.cachefile` 模块提供了Blender几何缓存系统的操作功能，支持打开Alembic（.abc）、USD（.usd）等格式的缓存文件。该模块主要用于动画数据的导入和播放，支持多缓存文件的层叠管理。

## 核心功能

### 文件打开操作

```python
import bpy

# 打开缓存文件
bpy.ops.cachefile.open(
    filepath='',
    hide_props_region=True,
    check_existing=False,
    filter_alembic=True,
    filter_usd=True,
    relative_path=True
)

# 重新加载缓存文件
bpy.ops.cachefile.reload()
```

### 缓存层管理

```python
# 添加缓存层
bpy.ops.cachefile.layer_add(
    filepath='',
    hide_props_region=True,
    check_existing=False,
    filter_alembic=True,
    filter_usd=True,
    relative_path=True
)

# 移动缓存层
bpy.ops.cachefile.layer_move(direction='UP')

# 移除缓存层
bpy.ops.cachefile.layer_remove()
```

## 实用函数

```python
# 加载Alembic缓存文件
def load_alembic_cache(filepath):
    bpy.ops.cachefile.open(
        filepath=filepath,
        filter_alembic=True,
        filter_usd=False,
        relative_path=True
    )

# 加载USD缓存文件
def load_usd_cache(filepath):
    bpy.ops.cachefile.open(
        filepath=filepath,
        filter_alembic=False,
        filter_usd=True,
        relative_path=True
    )

# 切换缓存层顺序
def move_cache_layer(layer_index, direction):
    # 先选中对应的缓存层
    # 然后移动
    bpy.ops.cachefile.layer_move(direction=direction)

# 移除所有缓存层
def remove_all_layers():
    # 获取缓存文件对象
    cachefile = bpy.context.active_object
    if cachefile and cachefile.type == 'CACHE_FILE':
        # 逐个移除层
        while cachefile.cache_layers:
            bpy.ops.cachefile.layer_remove()
```

## 常见问题排查

**问题：缓存文件无法打开**

- 确认文件格式是否受支持（Alembic或USD）
- 检查文件路径是否正确且可访问
- 验证是否安装了相应的插件

**问题：缓存动画不播放**

- 确保对象已正确绑定到缓存
- 检查时间轴范围设置
- 尝试重新加载缓存：`bpy.ops.cachefile.reload()`

**问题：多层缓存冲突**

- 调整缓存层的渲染顺序
- 检查每层的相对路径设置
- 确保不同层的对象名称不冲突

**问题：缓存文件过大导致卡顿**

- 考虑使用分层加载策略
- 优化Alembic/USD导出设置
- 使用`bpy.ops.cachefile.layer_remove()`移除不需要的层

**问题：相对路径失效**

- 检查项目路径设置
- 确保.blend文件已保存
- 验证相对路径的基准位置


---
## bpy_ops_asset

# bpy.ops.asset - Asset Operations Module

资产操作模块，用于创建、管理和发布Blender资产。

## 概述

`bpy.ops.asset` 模块提供了Blender资产系统的操作功能，包括资产的标记、清除、目录管理、资源包安装、标签管理等。资产系统是Blender用于管理和复用资源的核心功能，支持材质、模型、场景等各类资源的Asset Browser管理。

## 核心功能

### 资产标记操作

```python
import bpy

# 标记为资产
bpy.ops.asset.mark()

# 标记单个对象为资产
bpy.ops.asset.mark_single()

# 清除资产标记
bpy.ops.asset.clear(set_fake_user=False)

# 清除单个资产标记
bpy.ops.asset.clear_single(set_fake_user=False)

# 分配动作到资产
bpy.ops.asset.assign_action()
```

### 目录管理操作

```python
# 新建目录
bpy.ops.asset.catalog_new(parent_path='')

# 删除目录
bpy.ops.asset.catalog_delete(catalog_id='')

# 撤销目录操作
bpy.ops.asset.catalog_undo()

# 重做目录操作
bpy.ops.asset.catalog_redo()

# 推入目录操作到撤销栈
bpy.ops.asset.catalog_undo_push()

# 保存所有目录
bpy.ops.asset.catalogs_save()
```

### 资源包操作

```python
# 安装资源包
bpy.ops.asset.bundle_install(
    asset_library_reference='',
    filepath='',
    hide_props_region=True,
    check_existing=True
)

# 刷新资产库
bpy.ops.asset.library_refresh()

# 打开包含的blend文件
bpy.ops.asset.open_containing_blend_file()
```

### 标签管理

```python
# 添加标签
bpy.ops.asset.tag_add()

# 移除标签
bpy.ops.asset.tag_remove()
```

### 预览操作

```python
# 截图预览
bpy.ops.asset.screenshot_preview(
    p1=(0, 0),
    p2=(0, 0),
    force_square=True
)
```

## 实用函数

```python
# 批量标记选中对象为资产
def mark_selected_assets():
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.asset.mark()

# 清除场景中所有资产标记
def clear_all_assets(set_fake_user=True):
    for obj in bpy.data.objects:
        if obj.asset_data:
            obj.asset_clear(set_fake_user=set_fake_user)

# 为资产添加多个标签
def add_asset_tags(tag_names):
    for tag_name in tag_names:
        bpy.ops.asset.tag_add()
        # 设置标签名称
        # 注意：需要在Asset Browser中手动设置标签名称
```

## 常见问题排查

**问题：资产标记后不在Asset Browser中显示**

- 确保对象有有效的数据（网格、材质等）
- 尝试刷新资产库：`bpy.ops.asset.library_refresh()`

**问题：资源包安装失败**

- 检查文件路径是否正确
- 确认资源包格式是否有效（.blend或.zip）
- 验证资产库路径是否可写

**问题：目录操作无响应**

- 检查是否在编辑模式下操作
- 确认有足够的权限修改目录结构

**问题：标签无法添加**

- 确保当前有选中的资产
- 某些资产类型不支持标签

**问题：预览截图失败**

- 确保对象在视图中可见
- 检查渲染设置是否正确


---
## bpy_ops_gizmo

# bpy.ops.gizmo - 小工具操作模块

Blender小工具操作符，用于管理变换小工具和自定义小工具。

## 概述

`bpy.ops.gizmo` 模块提供用于操作Blender中各种小工具（Gizmo）的功能，包括选择、移动、配置和自定义小工具。

## 核心功能

### 小工具选择

```python
import bpy

# 选择小工具
bpy.ops.gizmo.select(
    extend=False,
    deselect=False,
    toggle=False,
    deselect_all=True
)

# 全选小工具
bpy.ops.gizmo.select_all(
    action='SELECT'
)

# 取消全选小工具
bpy.ops.gizmo.select_all(
    action='DESELECT'
)

# 反选小工具
bpy.ops.gizmo.select_all(
    action='INVERT'
)
```

### 小工具类型

```python
# 切换到移动小工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.move'
)

# 切换到旋转小工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.rotate'
)

# 切换到缩放小工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.scale'
)

# 切换到智能移动
bpy.ops.wm.tool_set_by_id(
    name='builtin.transform'
)

# 切换到标注小工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.annotate'
)

# 切换到测量小工具
bpy.ops.wm.tool_set_by_id(
    name='builtin.measure'
)
```

### 小工具操作

```python
# 激活小工具
bpy.ops.gizmo.activate(
    type='START'
)

# 拖动小工具
bpy.ops.gizmo.drag(
    type='TRANSLATE'
)

# 点击小工具
bpy.ops.gizmo.click()

# 释放小工具
bpy.ops.gizmo.release()
```

### 小工具配置

```python
# 设置小工具属性
bpy.ops.gizmo.properties(
    op='GIZMO'
)

# 切换小工具显示
bpy.ops.gizmo.toggle()

# 显示小工具
bpy.ops.gizmo.show(
    type='ALL'
)

# 隐藏小工具
bpy.ops.gizmo.hide(
    type='ALL'
)

# 重置小工具
bpy.ops.gizmo.reset()
```

### 自定义小工具

```python
# 添加自定义小工具
bpy.ops.gizmo.add(
    type='CUSTOM'
)

# 删除自定义小工具
bpy.ops.gizmo.remove()

# 编辑自定义小工具
bpy.ops.gizmo.edit(
    type='CUSTOM'
)

# 复制小工具
bpy.ops.gizmo.duplicate()

# 粘贴小工具
bpy.ops.gizmo.paste()
```

### 小工具约束

```python
# 设置约束轴
bpy.ops.transform.constraint(
    constraint_axis=(True, False, False)
)

# 设置约束模式
bpy.ops.transform.constraint(
    mode='OFFSET'
)

# 清除约束
bpy.ops.transform.constraint_clear()
```

## 实用函数

```python
def enable_gizmos():
    """启用小工具"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_gizmo = True

def disable_gizmos():
    """禁用小工具"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_gizmo = False

def toggle_move_gizmo():
    """切换移动小工具"""
    bpy.ops.wm.tool_set_by_id(name='builtin.move')

def toggle_rotate_gizmo():
    """切换旋转小工具"""
    bpy.ops.wm.tool_set_by_id(name='builtin.rotate')

def toggle_scale_gizmo():
    """切换缩放小工具"""
    bpy.ops.wm.tool_set_by_id(name='builtin.scale')

def set_gizmo_opacity(opacity):
    """设置小工具不透明度"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.gizmo_opacity = opacity

def set_gizmo_size(size):
    """设置小工具大小"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.gizmo_size = size

def enable_axis_gizmos():
    """启用轴小工具"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_gizmo_navigate = True
            space.show_gizmo_object_rotate = True
            space.show_gizmo_object_translate = True
            space.show_gizmo_object_scale = True

def disable_axis_gizmos():
    """禁用轴小工具"""
    for area in bpy.context.screen.areas:
        if area.type == 'VIEW_3D':
            space = area.spaces.active
            space.show_gizmo_navigate = False
            space.show_gizmo_object_rotate = False
            space.show_gizmo_object_translate = False
            space.show_gizmo_object_scale = False
```

## 常见问题排查

### 小工具不显示
```python
# 检查小工具显示设置
space = bpy.context.area.spaces.active
print(f"小工具显示: {space.show_gizmo}")

# 启用小工具
space.show_gizmo = True

# 检查小工具类型
print(f"移动小工具: {space.show_gizmo_object_translate}")
print(f"旋转小工具: {space.show_gizmo_object_rotate}")
print(f"缩放小工具: {space.show_gizmo_object_scale}")

# 启用所有小工具
space.show_gizmo_object_translate = True
space.show_gizmo_object_rotate = True
space.show_gizmo_object_scale = True
```

### 小工具太小
```python
# 检查小工具大小
space = bpy.context.area.spaces.active
print(f"小工具大小: {space.gizmo_size}")

# 设置更大的大小
space.gizmo_size = 60

# 检查不透明度
print(f"不透明度: {space.gizmo_opacity}")
space.gizmo_opacity = 1.0
```

### 小工具无响应
```python
# 检查活动对象
obj = bpy.context.active_object
print(f"活动对象: {obj}")

# 检查对象是否被锁定
print(f"锁定: {obj.lock_location}, {obj.lock_rotation}, {obj.lock_scale}")

# 解锁对象
obj.lock_location = (False, False, False)
obj.lock_rotation = (False, False, False)
obj.lock_scale = (False, False, False)

# 检查模式
print(f"模式: {bpy.context.mode}")
```

### 自定义小工具问题
```python
# 检查自定义小工具
print(f"小工具数: {len(bpy.data.gizmos)}")

# 列出小工具
for gizmo in bpy.data.gizmos:
    print(f"小工具: {gizmo.name}")

# 重置小工具
bpy.ops.gizmo.reset()

# 重新创建小工具
bpy.ops.gizmo.add(type='CUSTOM')
```


---
## bpy_ops_gizmogroup

# bpy.ops.gizmogroup - Gizmo组操作模块

Blender小工具组操作符，用于管理3D视口中的Gizmo交互控件。

## 概述

`bpy.ops.gizmogroup` 模块提供用于创建、编辑和管理Gizmo组的操作功能。Gizmo是Blender中用于用户交互的小型图形控件，常见于变换手柄、旋转环等。

## 核心功能

### Gizmo组管理

```python
import bpy

# 激活Gizmo组
bpy.ops.gizmogroup.group_pick()

# 创建新的Gizmo组
bpy.ops.gizmogroup.group_add()

# 删除当前Gizmo组
bpy.ops.gizmogroup.group_remove()

# 复制Gizmo组
bpy.ops.gizmogroup.group_copy()

# 粘贴复制的Gizmo组
bpy.ops.gizmogroup.group_paste()

# 选择Gizmo组中的所有Gizmo
bpy.ops.gizmogroup.select_all(action='SELECT')

# 取消选择所有Gizmo
bpy.ops.gizmogroup.select_all(action='DESELECT')

# 反选Gizmo
bpy.ops.gizmogroup.select_all(action='INVERT')

# 链接Gizmo组到活动对象
bpy.ops.gizmogroup.attach_to_active()

# 从活动对象分离Gizmo组
bpy.ops.gizmogroup.detach()

# 显示Gizmo组
bpy.ops.gizmogroup.show()

# 隐藏Gizmo组
bpy.ops.gizmogroup.hide()

# 切换Gizmo组显示状态
bpy.ops.gizmogroup.toggle()
```

### Gizmo操作

```python
# 选择特定的Gizmo
bpy.ops.gizmogroup.select(extend=False, deselect=False, toggle=False)

# 根据类型选择Gizmo
bpy.ops.gizmogroup.select_by_type(type='TRANSLATE')

# 搜索并选择Gizmo
bpy.ops.gizmogroup.search_select(name='gizmo_name')

# 删除选中的Gizmo
bpy.ops.gizmogroup.delete()

# 复制Gizmo
bpy.ops.gizmogroup.duplicate()

# 移动Gizmo到其他组
bpy.ops.gizmogroup.move_to_group(group_name='new_group')

# 重命名Gizmo
bpy.ops.gizmogroup.rename(name='new_gizmo_name')

# 设置Gizmo属性
bpy.ops.gizmogroup.set_property(prop_name='value')

# 获取Gizmo属性
bpy.ops.gizmogroup.get_property(prop_name='prop_name')
```

### Gizmo样式设置

```python
# 设置Gizmo颜色
bpy.ops.gizmogroup.color_set(color=(1.0, 0.0, 0.0, 1.0))

# 设置Gizmo线宽
bpy.ops.gizmogroup.line_width_set(width=2.0)

# 设置Gizmo尺寸
bpy.ops.gizmogroup.scale_set(scale=1.0)

# 重置样式为默认值
bpy.ops.gizmogroup.style_reset()

# 应用预设样式
bpy.ops.gizmogroup.preset_apply(preset='default')

# 保存当前样式为预设
bpy.ops.gizmogroup.preset_save(name='my_preset')

# 删除预设
bpy.ops.gizmogroup.preset_delete(name='my_preset')
```

### Gizmo变换操作

```python
# 变换选中的Gizmo
bpy.ops.gizmogroup.transform(mode='TRANSLATION')

# 平移Gizmo
bpy.ops.gizmogroup.translate(value=(1.0, 0.0, 0.0))

# 旋转Gizmo
bpy.ops.gizmogroup.rotate(angle=45.0, axis='Z')

# 缩放Gizmo
bpy.ops.gizmogroup.scale(value=(1.5, 1.5, 1.5))

# 对齐Gizmo到网格
bpy.ops.gizmogroup.snap_to_grid()

# 对齐Gizmo到原点
bpy.ops.gizmogroup.snap_to_origin()

# 对齐到选中对象
bpy.ops.gizmogroup.snap_to_selected()
```

### Gizmo动画

```python
# 为Gizmo位置插入关键帧
bpy.ops.gizmogroup.insert_keyframe()

# 删除Gizmo关键帧
bpy.ops.gizmogroup.delete_keyframe()

# 清除Gizmo动画
bpy.ops.gizmogroup.clear_keyframe()

# 设置Gizmo驱动
bpy.ops.gizmogroup.driver_add()

# 移除Gizmo驱动
bpy.ops.gizmogroup.driver_remove()
```

### 高级功能

```python
# 绑定Gizmo到对象
bpy.ops.gizmogroup.bind(target_object='Cube')

# 解绑Gizmo
bpy.ops.gizmogroup.unbind()

# 设置Gizmo可见性
bpy.ops.gizmogroup.visibility_set(visible=True)

# 设置Gizmo选择模式
bpy.ops.gizmogroup.select_mode(mode='ALL')

# 分组Gizmo
bpy.ops.gizmogroup.group_objects(objects=['Cube', 'Sphere', 'Cylinder'])

# 取消分组
bpy.ops.gizmogroup.ungroup_objects(objects=['Cube'])

# 应用变换到Gizmo
bpy.ops.gizmogroup.apply_transform()

# 重置Gizmo变换
bpy.ops.gizmogroup.reset_transform()

# 复制Gizmo属性
bpy.ops.gizmogroup.copy_properties(source='gizmo_name')

# 粘贴Gizmo属性
bpy.ops.gizmogroup.paste_properties()
```

## 实用函数

```python
def get_active_gizmo_group():
    """获取当前激活的Gizmo组"""
    return bpy.context.gizmo_group

def get_gizmos_in_group(group):
    """获取组中的所有Gizmo"""
    return list(group.gizmos)

def create_custom_gizmo(name, group_name=None):
    """创建自定义Gizmo"""
    if group_name:
        bpy.ops.gizmogroup.group_pick()
    bpy.ops.gizmogroup.group_add()
    bpy.ops.gizmogroup.rename(name=name)
    return bpy.context.gizmo_group

def hide_all_gizmos():
    """隐藏所有Gizmo"""
    for gizmo_group in bpy.context.scene.gizmo_groups:
        gizmo_group.hide()

def show_all_gizmos():
    """显示所有Gizmo"""
    for gizmo_group in bpy.context.scene.gizmo_groups:
        gizmo_group.show()

def select_gizmos_by_name_prefix(prefix):
    """按名称前缀选择Gizmo"""
    for gizmo in bpy.context.gizmo_group.gizmos:
        if gizmo.name.startswith(prefix):
            gizmo.select_set(True)
```

## 常见问题排查

### Gizmo不显示
```python
# 确保Gizmo组可见
bpy.ops.gizmogroup.show()

# 确保在正确的空间类型
print(bpy.context.space_data.type)

# 检查Gizmo组是否绑定
if bpy.context.gizmo_group:
    print("Gizmo组已绑定")
else:
    print("Gizmo组未绑定")
```

### Gizmo选择问题
```python
# 重置选择状态
bpy.ops.gizmogroup.select_all(action='DESELECT')

# 切换选择模式
bpy.ops.gizmogroup.select_all(action='SELECT')

# 手动选择Gizmo
for gizmo in bpy.context.gizmo_group.gizmos:
    if gizmo.name == "target_gizmo":
        gizmo.select_set(True)
```


---
## bpy_ops_modifier

# bpy.ops.modifier 模块

修饰器操作模块，用于创建和管理物体修饰器。

## 概述

bpy.ops.modifier 模块包含用于在物体上添加、编辑和应用修饰器的运算符。修饰器是非破坏性修改物体几何体的工具，包括细分、表面细分、布尔运算、阵列等。

## 常用操作

### 修饰器添加

```python
import bpy

# 添加修饰器到选中物体
bpy.ops.object.modifier_add(type='SUBSURF')

# 添加指定类型的修饰器
bpy.ops.object.modifier_add(type='ARRAY')
bpy.ops.object.modifier_add(type='BOOLEAN')
bpy.ops.object.modifier_add(type='BEVEL')
bpy.ops.object.modifier_add(type='MIRROR')
bpy.ops.object.modifier_add(type='DECIMATE')
bpy.ops.object.modifier_add(type='REMESH')
bpy.ops.object.modifier_add(type='WIREFRAME')
bpy.ops.object.modifier_add(type='DISPLACE')
bpy.ops.object.modifier_add(type='SMOOTH')
bpy.ops.object.modifier_add(type='SIMPLE_DEFORM')
bpy.ops.object.modifier_add(type='CAST')
bpy.ops.object.modifier_add(type='LATTICE')
```

### 修饰器类型

```python
# 表面细分
bpy.ops.object.modifier_add(type='SUBSURF')
bpy.ops.object.modifier_add(type='SUBSURF', levels=2, render_levels=3)

# 阵列修饰器
bpy.ops.object.modifier_add(type='ARRAY')
bpy.ops.object.modifier_add(type='ARRAY', use_relative_offset=False, use_constant_offset=True)

# 布尔修饰器
bpy.ops.object.modifier_add(type='BOOLEAN')
bpy.ops.object.modifier_add(type='BOOLEAN', operation='DIFFERENCE', object='Cube_001')

# 倒角修饰器
bpy.ops.object.modifier_add(type='BEVEL')
bpy.ops.object.modifier_add(type='BEVEL', width=0.1, segments=3, limit_method='ANGLE')

# 镜像修饰器
bpy.ops.object.modifier_add(type='MIRROR')
bpy.ops.object.modifier_add(type='MIRROR', axis={'X'}, use_mirror_merge=True)

# 精简修饰器
bpy.ops.object.modifier_add(type='DECIMATE')
bpy.ops.object.modifier_add(type='DECIMATE', ratio=0.5, decimate_type='COLLAPSE')

# 重构修饰器
bpy.ops.object.modifier_add(type='REMESH')
bpy.ops.object.modifier_add(type='REMESH', mode='VOXEL', voxel_size=0.1)

# 线框修饰器
bpy.ops.object.modifier_add(type='WIREFRAME')
bpy.ops.object.modifier_add(type='WIREFRAME', thickness=0.05, use_replace=True)

# 置换修饰器
bpy.ops.object.modifier_add(type='DISPLACE')
bpy.ops.object.modifier_add(type='DISPLACE', texture='CloudTexture', strength=0.5)

# 平滑修饰器
bpy.ops.object.modifier_add(type='SMOOTH')
bpy.ops.object.modifier_add(type='SMOOTH', factor=1.0, iterations=5)

# 简单变形修饰器
bpy.ops.object.modifier_add(type='SIMPLE_DEFORM')
bpy.ops.object.modifier_add(type='SIMPLE_DEFORM', deform_method='TWIST', angle=3.14159)

# 铸造修饰器
bpy.ops.object.modifier_add(type='CAST')
bpy.ops.object.modifier_add(type='CAST', cast_type='SPHERE', factor=1.0)

# 晶格修饰器
bpy.ops.object.modifier_add(type='LATTICE')
bpy.ops.object.modifier_add(type='LATTICE', object='LatticeObject')

# 体积转网格修饰器
bpy.ops.object.modifier_add(type='VOLUME_TO_MESH')
bpy.ops.object.modifier_add(type='VOLUME_TO_MESH', resolution=0.1)

# 法线编辑修饰器
bpy.ops.object.modifier_add(type='NORMAL_EDIT')
bpy.ops.object.modifier_add(type='NORMAL_EDIT', mode='RADIAL', factor=1.0)

# 加权法线修饰器
bpy.ops.object.modifier_add(type='WEIGHTED_NORMAL')
bpy.ops.object.modifier_add(type='WEIGHTED_NORMAL', weight=1.0)

# 数据传输修饰器
bpy.ops.object.modifier_add(type='DATA_TRANSFER')
bpy.ops.object.modifier_add(type='DATA_TRANSFER', object='SourceObject', use_reverse_transfer=True)
```

### 修饰器操作

```python
# 选中修饰器
bpy.ops.object.modifier_select(modifier='Subsurf')

# 移动修饰器
bpy.ops.object.modifier_move_up(modifier='Subsurf')
bpy.ops.object.modifier_move_down(modifier='Subsurf')

# 删除修饰器
bpy.ops.object.modifier_remove(modifier='Subsurf')

# 应用修饰器
bpy.ops.object.modifier_apply(modifier='Subsurf')
bpy.ops.object.modifier_apply_as='DATA_TRANSFER', modifier='Boolean')

# 应用为形状键
bpy.ops.object.modifier_apply_as_shapekey(modifier='Subsurf', suffix='Subsurf')

# 复制修饰器
bpy.ops.object.modifier_copy(modifier='Subsurf')

# 转换所有修饰器为网格
bpy.ops.object.convert_to_thresholded()

# 转换所有修饰器为实时细分
bpy.ops.object.convert_to_realistic()
```

## 实际应用示例

### 创建程序化建筑

```python
def create_modern_building(base_size=(10, 10, 20), segments=4):
    """创建现代建筑模型"""
    # 创建基础立方体
    bpy.ops.mesh.primitive_cube_add(size=1)
    building = bpy.context.active_object
    building.name = 'ModernBuilding'
    
    # 设置基础大小
    building.scale = (base_size[0], base_size[1], base_size[2])
    
    # 添加镜像修饰器
    mirror = building.modifiers.new(name='Mirror', type='MIRROR')
    mirror.use_x = True
    mirror.use_y = True
    mirror.use_mirror_merge = True
    
    # 添加细分修饰器
    subsurf = building.modifiers.new(name='Subsurf', type='SUBSURF')
    subsurf.levels = 2
    subsurf.render_levels = 3
    
    # 添加倒角修饰器
    bevel = building.modifiers.new(name='Bevel', type='BEVEL')
    bevel.width = 0.2
    bevel.segments = 3
    bevel.limit_method = 'ANGLE'
    bevel.angle_limit = 0.523599
    
    return building
```

### 创建程序化地形

```python
def create_terrain(size=100, subdivisions=5):
    """创建程序化地形"""
    # 创建平面
    bpy.ops.mesh.primitive_plane_add(size=size)
    terrain = bpy.context.active_object
    terrain.name = 'ProceduralTerrain'
    
    # 细分平面
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.subdivide(number_cuts=subdivisions)
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # 添加置换修饰器
    displace = terrain.modifiers.new(name='Displace', type='DISPLACE')
    
    # 创建程序化纹理
    tex = bpy.data.textures.new('TerrainNoise', type='CLOUDS')
    tex.noise_scale = 2.0
    tex.noise_depth = 2
    displace.texture = tex
    displace.strength = 5.0
    
    # 添加平滑修饰器
    smooth = terrain.modifiers.new(name='Smooth', type='SMOOTH')
    smooth.factor = 0.5
    smooth.iterations = 3
    
    # 添加权重法线修饰器
    weighted_normal = terrain.modifiers.new(name='WeightedNormal', type='WEIGHTED_NORMAL')
    weighted_normal.weight = 1.0
    
    return terrain
```

### 创建程序化晶格结构

```python
def create_lattice_structure(obj, repetitions=(3, 3, 3)):
    """创建晶格结构"""
    # 创建晶格
    bpy.ops.object.lattice_add()
    lattice = bpy.context.active_object
    lattice.name = 'LatticeStructure'
    
    # 设置晶格分辨率
    lattice.data.points_u = repetitions[0] * 2
    lattice.data.points_v = repetitions[1] * 2
    lattice.data.points_w = repetitions[2] * 2
    
    # 调整晶格大小
    lattice.scale = obj.scale
    
    # 清除目标物体
    bpy.ops.object.select_all(action='DESELECT')
    obj.select_set(True)
    bpy.context.view_layer.objects.active = obj
    
    # 添加晶格修饰器
    lattice_mod = obj.modifiers.new(name='Lattice', type='LATTICE')
    lattice_mod.object = lattice
    
    return lattice
```

### 创建程序化瓦片阵列

```python
def create_tile_array(tile_obj, count=(10, 10), spacing=1.5):
    """创建瓦片阵列"""
    tiles = []
    
    for i in range(count[0]):
        for j in range(count[1]):
            # 复制瓦片
            bpy.ops.object.select_all(action='DESELECT')
            tile_obj.select_set(True)
            bpy.ops.object.duplicate()
            
            new_tile = bpy.context.active_object
            new_tile.name = f'Tile_{i}_{j}'
            
            # 定位
            x = (i - count[0] / 2) * spacing
            y = (j - count[1] / 2) * spacing
            new_tile.location = (x, y, 0)
            
            tiles.append(new_tile)
    
    # 选中所有瓦片
    bpy.ops.object.select_all(action='SELECT')
    
    # 添加阵列修饰器
    for tile in tiles:
        bpy.context.view_layer.objects.active = tile
        
        # X方向阵列
        array_x = tile.modifiers.new(name='ArrayX', type='ARRAY')
        array_x.count = 1
        array_x.use_relative_offset = False
        array_x.use_constant_offset = True
        array_x.constant_offset_displace = (spacing, 0, 0)
        
        # Y方向阵列
        array_y = tile.modifiers.new(name='ArrayY', type='ARRAY')
        array_y.count = 1
        array_y.use_relative_offset = False
        array_y.use_constant_offset = True
        array_y.constant_offset_displace = (0, spacing, 0)
    
    return tiles
```

### 创建程序化布尔运算

```python
def create_boolean_difference(base_obj, cutter_obj):
    """创建布尔减法运算"""
    # 选中基础物体
    bpy.ops.object.select_all(action='DESELECT')
    base_obj.select_set(True)
    bpy.context.view_layer.objects.active = base_obj
    
    # 添加布尔修饰器
    bool_mod = base_obj.modifiers.new(name='Boolean', type='BOOLEAN')
    bool_mod.operation = 'DIFFERENCE'
    bool_mod.object = cutter_obj
    
    return bool_mod

def create_boolean_union(objects):
    """创建布尔并集运算"""
    # 确保所有物体被选中
    bpy.ops.object.select_all(action='SELECT')
    
    # 以第一个物体为目标
    target = objects[0]
    bpy.context.view_layer.objects.active = target
    
    # 合并其他物体
    for obj in objects[1:]:
        # 添加布尔并集
        bool_mod = target.modifiers.new(name='BooleanUnion', type='BOOLEAN')
        bool_mod.operation = 'UNION'
        bool_mod.object = obj
    
    return target

def create_boolean_intersection(base_obj, intersect_obj):
    """创建布尔交集运算"""
    # 选中基础物体
    bpy.ops.object.select_all(action='DESELECT')
    base_obj.select_set(True)
    bpy.context.view_layer.objects.active = base_obj
    
    # 添加布尔修饰器
    bool_mod = base_obj.modifiers.new(name='Boolean', type='BOOLEAN')
    bool_mod.operation = 'INTERSECT'
    bool_mod.object = intersect_obj
    
    return bool_mod
```

## 使用场景

- **建模**：非破坏性修改网格几何体
- **程序化生成**：创建复杂的程序化模型
- **地形生成**：使用噪声和置换创建地形
- **建筑建模**：使用阵列和镜像创建建筑元素
- **特效制作**：使用布尔运算创建复杂形状
- **角色建模**：使用细分和倒角创建有机形状

## 注意事项

- 修饰器按顺序执行，后添加的修饰器影响前面的效果
- 高细分等级会影响性能
- 布尔运算需要物体有正确的法线方向
- 应用修饰器会永久修改网格
- 某些修饰器需要特定的网格结构才能正常工作


