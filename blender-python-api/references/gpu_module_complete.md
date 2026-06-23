# gpu_module_complete

---
## gpu

# GPU Module Reference

GPU 模块提供 GPU 编程功能，用于创建自定义着色器和效果。

## GPU 概述

GPU 模块允许直接访问 Blender 的 GPU 功能，用于创建高性能的自定义渲染效果。

### 何时使用 GPU 模块

- 创建自定义着色器
- 实现高性能的视觉效果
- 绘制自定义几何体
- 创建 GPU 加速的工具

## gpu.types

GPU 类型定义。

### GPUOffScreen

离屏渲染缓冲区。

```python
import gpu

# 创建离屏缓冲区
offscreen = gpu.types.GPUOffScreen(512, 512)

# 获取纹理
texture = offscreen.color_texture

# 绑定离屏缓冲区
offscreen.bind()

# 解绑离屏缓冲区
offscreen.unbind()

# 释放离屏缓冲区
offscreen.free()
```

### GPUShader

GPU 着色器。

```python
import gpu

# 创建着色器
shader = gpu.types.GPUShader(
    vertex_shader="""
        uniform mat4 modelMatrix;
        uniform mat4 viewMatrix;
        uniform mat4 projectionMatrix;

        in vec3 position;
        in vec2 texCoord;

        out vec2 vTexCoord;

        void main()
        {
            vTexCoord = texCoord;
            gl_Position = projectionMatrix * viewMatrix * modelMatrix * vec4(position, 1.0);
        }
    """,
    fragment_shader="""
        uniform sampler2D image;
        in vec2 vTexCoord;
        out vec4 fragColor;

        void main()
        {
            fragColor = texture(image, vTexCoord);
        }
    """
)

# 绑定着色器
shader.bind()

# 设置 uniform
shader.uniform_float("modelMatrix", model_matrix)
shader.uniform_float("viewMatrix", view_matrix)
shader.uniform_float("projectionMatrix", projection_matrix)
shader.uniform_int("image", 0)

# 解绑着色器
shader.unbind()
```

### GPUTexture

GPU 纹理。

```python
import gpu

# 从图像创建纹理
texture = gpu.types.GPUTexture((512, 512))

# 绑定纹理
texture.bind(0)

# 解绑纹理
texture.unbind()

# 释放纹理
texture.free()
```

## gpu.shader

着色器工具函数。

### create_from_info

从 GPUShaderCreateInfo 创建着色器。

```python
import gpu

# 创建着色器信息
shader_info = gpu.types.GPUShaderCreateInfo()

# 添加着色器代码
shader_info.vertex_in(0, 'vec3', 'position')
shader_info.vertex_out(0, 'vec2', 'texCoord')

# 创建着色器
shader = gpu.shader.create_from_info(shader_info)
```

### from_builtin

获取内置着色器。

```python
import gpu

# 获取内置着色器
shader = gpu.shader.from_builtin('3D_UNIFORM_COLOR')

# 使用裁剪平面配置
shader_clipped = gpu.shader.from_builtin('3D_UNIFORM_COLOR', config='CLIPPED')
```

### unbind

解绑当前绑定的着色器。

```python
import gpu

# 解绑着色器
gpu.shader.unbind()
```

## gpu.state

GPU 状态管理。

### active_framebuffer_get

获取当前活动的帧缓冲区。

```python
import gpu

# 获取当前帧缓冲区
framebuffer = gpu.state.active_framebuffer_get()
```

### blend_set

设置混合模式。

```python
import gpu

# 启用混合
gpu.state.blend_set('ALPHA')

# 禁用混合
gpu.state.blend_set('NONE')
```

### depth_test_set

设置深度测试。

```python
import gpu

# 启用深度测试
gpu.state.depth_test_set('LESS')

# 禁用深度测试
gpu.state.depth_test_set('NONE')
```

### viewport_set

设置视口。

```python
import gpu

# 设置视口大小
gpu.state.viewport_set(0, 0, 800, 600)
```

## gpu.matrix

矩阵操作工具。

### load_identity

加载单位矩阵。

```python
import gpu

# 加载单位矩阵
gpu.matrix.load_identity()
```

### load_matrix

加载矩阵。

```python
import gpu
import mathutils

# 创建变换矩阵
matrix = mathutils.Matrix.Identity(4)

# 加载矩阵
gpu.matrix.load_matrix(matrix)
```

### push_matrix / pop_matrix

矩阵堆栈操作。

```python
import gpu

# 保存当前矩阵
gpu.matrix.push_matrix()

# 应用变换
gpu.matrix.translate((1, 0, 0))

# 恢复矩阵
gpu.matrix.pop_matrix()
```

### translate / rotate / scale

变换操作。

```python
import gpu

# 平移
gpu.matrix.translate((1, 2, 3))

# 旋转
gpu.matrix.rotate(mathutils.Quaternion((1, 0, 0), math.radians(45)))

# 缩放
gpu.matrix.scale((2, 2, 2))
```

## gpu.texture

纹理工具函数。

### from_image

从图像创建纹理。

```python
import gpu
import bpy

# 从图像创建纹理
image = bpy.data.images.get('texture.png')
if image:
    texture = gpu.texture.from_image(image)
```

## gpu.capabilities

GPU 能力检测。

### compute_shader_support_get

检查计算着色器支持。

```python
import gpu

# 检查计算着色器支持
has_compute = gpu.capabilities.compute_shader_support_get()
```

### extensions_get

获取支持的扩展列表。

```python
import gpu

# 获取扩展列表
extensions = gpu.capabilities.extensions_get()
```

## 使用示例

### 自定义着色器绘制

```python
import gpu
import bgl

# 创建自定义着色器
vertex_shader = '''
    uniform mat4 ModelViewProjectionMatrix;
    in vec3 pos;
    void main() {
        gl_Position = ModelViewProjectionMatrix * vec4(pos, 1.0);
    }
'''

fragment_shader = '''
    uniform vec4 color;
    out vec4 fragColor;
    void main() {
        fragColor = color;
    }
'''

shader = gpu.types.GPUShader(vertex_shader, fragment_shader)

# 绘制三角形
vertices = [(-1, -1, 0), (1, -1, 0), (0, 1, 0)]

shader.bind()
shader.uniform_float('color', (1, 0, 0, 1))

gpu.draw.batch(shader, 'TRIS', {'pos': vertices})
```

### 离屏渲染

```python
import gpu

# 创建离屏缓冲区
offscreen = gpu.types.GPUOffScreen(512, 512)

# 绑定离屏缓冲区
offscreen.bind()

# 设置视口
gpu.state.viewport_set(0, 0, 512, 512)

# 清除缓冲区
gpu.state.clear(color=(0, 0, 0, 1))

# 绘制内容
# ...

# 解绑离屏缓冲区
offscreen.unbind()

# 获取纹理
texture = offscreen.color_texture
```

## 性能优化建议

- 尽量减少着色器切换
- 批量绘制调用
- 重用纹理和缓冲区对象
- 使用实例化渲染
- 避免每帧创建和销毁GPU资源
image = bpy.data.images.get("MyImage")
texture = gpu.types.GPUTexture(image.size, data=image.pixels)

# 从数据创建纹理
texture = gpu.types.GPUTexture((512, 512), data=pixels)

# 绑定纹理
texture.bind(0)

# 解绑纹理
texture.unbind()

# 更新纹理
texture.update(data=new_pixels)

# 释放纹理
texture.free()
```

### GPUFrameBuffer

GPU 帧缓冲区。

```python
import gpu

# 创建帧缓冲区
framebuffer = gpu.types.GPUFrameBuffer(
    color_slots=(gpu.types.GPUTexture((512, 512)),),
    depth_slot=gpu.types.GPUTexture((512, 512))
)

# 绑定帧缓冲区
framebuffer.bind()

# 解绑帧缓冲区
framebuffer.unbind()

# 释放帧缓冲区
framebuffer.free()
```

## gpu.shader

着色器操作。

### 内置着色器

```python
import gpu

# 2D 平面着色器
shader = gpu.shader.from_builtin('2D_UNIFORM_COLOR')

# 2D 图像着色器
shader = gpu.shader.from_builtin('2D_IMAGE')

# 2D 平滑颜色着色器
shader = gpu.shader.from_builtin('2D_SMOOTH_COLOR')

# 2D 平滑线条着色器
shader = gpu.shader.from_builtin('2D_SMOOTH_LINE')

# 3D 平面着色器
shader = gpu.shader.from_builtin('3D_UNIFORM_COLOR')

# 3D 平滑颜色着色器
shader = gpu.shader.from_builtin('3D_SMOOTH_COLOR')

# 3D 平滑线条着色器
shader = gpu.shader.from_builtin('3D_SMOOTH_LINE')

# 3D 多边形线条着色器
shader = gpu.shader.from_builtin('3D_POLYLINE_FLAT_COLOR')

# 3D 多边形线条平滑颜色着色器
shader = gpu.shader.from_builtin('3D_POLYLINE_SMOOTH_COLOR')

# 点着色器
shader = gpu.shader.from_builtin('POINTS')

# 线着色器
shader = gpu.shader.from_builtin('LINE')
```

### 着色器 uniform

```python
import gpu

shader = gpu.shader.from_builtin('2D_UNIFORM_COLOR')
shader.bind()

# 设置颜色
shader.uniform_float("color", (1.0, 0.0, 0.0, 1.0))

# 设置矩阵
shader.uniform_float("modelViewMatrix", model_view_matrix)
shader.uniform_float("projectionMatrix", projection_matrix)

# 设置纹理
shader.uniform_int("image", 0)
```

## gpu.state

GPU 状态管理。

### 状态设置

```python
import gpu

# 设置混合模式
gpu.state.blend_set('ALPHA')
gpu.state.blend_set('ADDITIVE')
gpu.state.blend_set('SUBTRACT')
gpu.state.blend_set('REVERSE_SUBTRACT')
gpu.state.blend_set('MULTIPLY')
gpu.state.blend_set('PREMULTIPLIED')
gpu.state.blend_set('INVERT')
gpu.state.blend_set('NONE')

# 设置深度测试
gpu.state.depth_test_set('LESS')
gpu.state.depth_test_set('LESS_EQUAL')
gpu.state.depth_test_set('GREATER')
gpu.state.depth_test_set('GREATER_EQUAL')
gpu.state.depth_test_set('EQUAL')
gpu.state.depth_test_set('NOTEQUAL')
gpu.state.depth_test_set('ALWAYS')
gpu.state.depth_test_set('NONE')

# 设置写入掩码
gpu.state.depth_mask_set(True)
gpu.state.depth_mask_set(False)

# 设置线宽
gpu.state.line_width_set(2.0)

# 设置点大小
gpu.state.point_size_set(5.0)

# 设置面剔除
gpu.state.face_culling_set('BACK')
gpu.state.face_culling_set('FRONT')
gpu.state.face_culling_set('NONE')
```

## gpu.matrix

矩阵操作。

### 矩阵管理

```python
import gpu
import mathutils

# 获取模型视图矩阵
model_view_matrix = gpu.matrix.get_model_view_matrix()

# 获取投影矩阵
projection_matrix = gpu.matrix.get_projection_matrix()

# 设置模型视图矩阵
gpu.matrix.set_model_view_matrix(model_view_matrix)

# 设置投影矩阵
gpu.matrix.set_projection_matrix(projection_matrix)

# 推入矩阵栈
gpu.matrix.push()

# 弹出矩阵栈
gpu.matrix.pop()

# 重置矩阵
gpu.matrix.reset()

# 乘以矩阵
gpu.matrix.multiply_matrix(matrix)

# 平移
gpu.matrix.translate((1.0, 2.0, 3.0))

# 旋转
gpu.matrix.rotate(mathutils.Euler((0.0, 0.0, math.radians(45))))

# 缩放
gpu.matrix.scale((2.0, 2.0, 2.0))
```

## gpu.select

选择操作。

### 选择查询

```python
import gpu

# 开始选择
gpu.select.begin(mode='ALL')

# 加载选择 ID
gpu.select.load_id(id)

# 结束选择
hits = gpu.select.end()

# 获取命中结果
for hit in hits:
    print(f"Hit ID: {hit.id}, Depth: {hit.depth}")
```

## gpu.extras

额外的 GPU 工具。

### 批量绘制

```python
import gpu
import gpu_extras.batch

# 创建顶点数据
vertices = (
    (-1.0, -1.0),
    (1.0, -1.0),
    (0.0, 1.0),
)

# 创建批量绘制对象
batch = gpu_extras.batch.batch_for_shader(
    shader,
    'TRIS',
    {"pos": vertices}
)

# 绘制
batch.draw(shader)
```

### 绘制 2D 线条

```python
import gpu
import gpu_extras

# 创建线条数据
coords = (
    (0.0, 0.0),
    (100.0, 100.0),
)

# 创建批量绘制对象
batch = gpu_extras.batch.batch_for_shader(
    gpu.shader.from_builtin('2D_UNIFORM_COLOR'),
    'LINES',
    {"pos": coords}
)

# 绘制
batch.draw()
```

### 绘制 3D 线条

```python
import gpu
import gpu_extras
import mathutils

# 创建线条数据
coords = (
    (0.0, 0.0, 0.0),
    (1.0, 1.0, 1.0),
)

# 创建批量绘制对象
batch = gpu_extras.batch.batch_for_shader(
    gpu.shader.from_builtin('3D_UNIFORM_COLOR'),
    'LINES',
    {"pos": coords}
)

# 绘制
batch.draw()
```

### 绘制 2D 图像

```python
import gpu
import gpu_extras

# 创建图像数据
image = bpy.data.images.get("MyImage")
coords = (
    (0.0, 0.0),
    (image.size[0], 0.0),
    (image.size[0], image.size[1]),
    (0.0, image.size[1]),
)

# 创建批量绘制对象
batch = gpu_extras.batch.batch_for_shader(
    gpu.shader.from_builtin('2D_IMAGE'),
    'TRI_FAN',
    {
        "pos": coords,
        "texCoord": ((0.0, 0.0), (1.0, 0.0), (1.0, 1.0), (0.0, 1.0)),
    }
)

# 绘制
batch.draw()
```

## 完整示例

### 绘制 2D 矩形

```python
import gpu
import gpu_extras.batch

def draw_callback():
    # 创建着色器
    shader = gpu.shader.from_builtin('2D_UNIFORM_COLOR')
    shader.bind()

    # 设置颜色
    shader.uniform_float("color", (1.0, 0.0, 0.0, 1.0))

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

# 注册绘制回调
handle = bpy.types.SpaceView3D.draw_handler_add(draw_callback, (), 'WINDOW', 'POST_PIXEL')

# 注销绘制回调
bpy.types.SpaceView3D.draw_handler_remove(handle, 'WINDOW')
```

### 绘制 3D 立方体

```python
import gpu
import gpu_extras.batch
import mathutils

def draw_callback():
    # 创建着色器
    shader = gpu.shader.from_builtin('3D_UNIFORM_COLOR')
    shader.bind()

    # 设置颜色
    shader.uniform_float("color", (0.0, 1.0, 0.0, 1.0))

    # 创建立方体顶点
    coords = (
        (-1, -1, -1), (1, -1, -1), (1, 1, -1), (-1, 1, -1),
        (-1, -1, 1), (1, -1, 1), (1, 1, 1), (-1, 1, 1),
    )

    # 创建索引
    indices = (
        (0, 1, 2), (2, 3, 0),
        (4, 5, 6), (6, 7, 4),
        (0, 1, 5), (5, 4, 0),
        (2, 3, 7), (7, 6, 2),
        (0, 3, 7), (7, 4, 0),
        (1, 2, 6), (6, 5, 1),
    )

    # 创建批量绘制对象
    batch = gpu_extras.batch.batch_for_shader(
        shader,
        'TRIS',
        {"pos": coords},
        indices=indices
    )

    # 绘制
    batch.draw()

# 注册绘制回调
handle = bpy.types.SpaceView3D.draw_handler_add(draw_callback, (), 'WINDOW', 'POST_VIEW')

# 注销绘制回调
bpy.types.SpaceView3D.draw_handler_remove(handle, 'WINDOW')
```

### 自定义着色器

```python
import gpu
import gpu_extras.batch

# 创建自定义着色器
shader = gpu.types.GPUShader(
    vertex_shader="""
        uniform mat4 modelViewMatrix;
        uniform mat4 projectionMatrix;

        in vec3 position;
        in vec3 color;

        out vec3 vColor;

        void main()
        {
            vColor = color;
            gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
        }
    """,
    fragment_shader="""
        in vec3 vColor;
        out vec4 fragColor;

        void main()
        {
            fragColor = vec4(vColor, 1.0);
        }
    """
)

# 创建顶点数据
vertices = (
    (-1.0, -1.0, 0.0),
    (1.0, -1.0, 0.0),
    (0.0, 1.0, 0.0),
)

colors = (
    (1.0, 0.0, 0.0),
    (0.0, 1.0, 0.0),
    (0.0, 0.0, 1.0),
)

# 创建批量绘制对象
batch = gpu_extras.batch.batch_for_shader(
    shader,
    'TRIS',
    {
        "position": vertices,
        "color": colors,
    }
)

# 绘制
shader.bind()
batch.draw()
```

### 离屏渲染

```python
import gpu
import gpu_extras.batch

# 创建离屏缓冲区
offscreen = gpu.types.GPUOffScreen(512, 512)

# 绑定离屏缓冲区
offscreen.bind()

# 清除缓冲区
gpu.state.depth_mask_set(True)
gpu.state.depth_test_set('LESS')
gpu.state.depth_mask_set(False)

# 创建着色器
shader = gpu.shader.from_builtin('2D_UNIFORM_COLOR')
shader.bind()

# 设置颜色
shader.uniform_float("color", (1.0, 0.0, 0.0, 1.0))

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

# 解绑离屏缓冲区
offscreen.unbind()

# 获取纹理
texture = offscreen.color_texture

# 释放离屏缓冲区
offscreen.free()
```


---
## gpu_detailed

# gpu - GPU 编程模块完整参考

Blender GPU 模块提供底层 GPU 访问功能，用于自定义着色器和效果开发。

## Overview

`gpu` 模块及其子模块提供了在 Blender 中进行 GPU 编程的能力，包括自定义着色器、离屏渲染、纹理操作和选择功能。

## 子模块概览

```python
import gpu
from gpu import types
from gpu import state
from gpu import matrix
from gpu import shader
from gpu import texture
from gpu import select
from gpu import platform
```

## GPU 初始化与检测

```python
import gpu

# 检查 GPU 是否可用
print(f"GPU 可用: {gpu.is_compute_available()}")

# 获取 GPU 信息
print(f"GPU 厂商: {gpu.platform.info_get()['VENDOR']}")
print(f"GPU 渲染器: {gpu.platform.info_get()['RENDERER']}")
print(f"驱动版本: {gpu.platform.info_get()['VERSION']}")

# 检查 OpenGL 上下文
print(f"OpenGL 上下文: {gpu.context.active}")
```

## GPU 类型 (gpu.types)

### 离屏缓冲区 (GPUOffScreen)

```python
import gpu
from gpu import types

# 创建离屏渲染缓冲区
offscreen = gpu.types.GPUOffScreen(512, 512)

# 检查是否有效
print(f"离屏有效: {offscreen.is_valid}")

# 获取纹理
texture = offscreen.texture
print(f"纹理: {texture}")

# 绑定和渲染
with offscreen.bind():
    # 在这里进行渲染操作
    pass

# 清理
offscreen.free()

# 使用上下文管理器
with gpu.types.GPUOffScreen(512, 512) as offscreen:
    # 渲染操作
    pass
```

### 着色器程序 (GPUShader)

```python
import gpu
from gpu import types

# 创建着色器
shader = gpu.types.GPUShader(
    vertex_shader="""
    in vec3 position;
    in vec3 color;
    out vec3 v_color;
    uniform mat4 u_model_view_projection_matrix;
    
    void main() {
        gl_Position = u_model_view_projection_matrix * vec4(position, 1.0);
        v_color = color;
    }
    """,
    fragment_shader="""
    in vec3 v_color;
    out vec4 fragColor;
    
    void main() {
        fragColor = vec4(v_color, 1.0);
    }
    """
)

# 绑定着色器
shader.bind()

# 设置 uniform
shader.uniform_float("u_model_view_projection_matrix", matrix)

# 获取 uniform 位置
loc = shader.uniform_location("u_model_view_projection_matrix")

# 清理
shader.free()

# 检查编译状态
print(f"着色器编译成功: {shader.is_valid}")
```

### GPU 纹理 (GPUTexture)

```python
import gpu
from gpu import types
import numpy as np

# 从数据创建纹理
data = np.zeros((256, 256, 4), dtype=np.uint8)
texture = gpu.types.GPUTexture(256, 256, data=data)

# 从图像创建
import bpy
img = bpy.data.images[0]
texture = gpu.types.GPUTexture(img.size[0], img.size[1], data=np.array(img.pixels))

# 绑定纹理
texture.bind(0)  # 绑定到纹理单元 0

# 设置过滤器
texture.filter(gpu.types.GPU_NEAREST, gpu.types.GPU_NEAREST)

# 设置包裹模式
texture.filter(gpu.types.GPU_LINEAR, gpu.types.GPU_LINEAR, 
               gpu.types.GPU_REPEAT, gpu.types.GPU_REPEAT)

# 读取纹理数据
data = texture.read()
print(f"数据形状: {data.shape}")

# 清理
texture.free()
```

### 顶点缓冲区 (GPUVertBuf)

```python
import gpu
from gpu import types
import numpy as np

# 定义顶点格式
format = gpu.types.GPUVertFormat()
format.attr_add(id="position", comp_type=gpu.types.FLOAT3, 
                fetch=gpu.types.FLOAT, offset=0)
format.attr_add(id="color", comp_type=gpu.types.UBYTE4, 
                fetch=gpu.types.INT, offset=12)

# 创建顶点数据
vertices = np.array([
    [0.0, 0.0, 0.0, 255, 0, 0, 255],
    [1.0, 0.0, 0.0, 0, 255, 0, 255],
    [0.0, 1.0, 0.0, 0, 0, 255, 255],
], dtype=np.float32)

# 创建顶点缓冲区
vbo = gpu.types.GPUVertBuf(format=format, len=3, data=vertices)

# 绑定
vbo.bind()

# 清理
vbo.free()
```

### 索引缓冲区 (GPUIndexBuf)

```python
import gpu
from gpu import types
import numpy as np

# 创建索引数据
indices = np.array([0, 1, 2], dtype=np.uint32)

# 创建索引缓冲区
ibo = gpu.types.GPUIndexBuf(data=indices)

# 绑定
ibo.bind()

# 清理
ibo.free()
```

### 批处理 (GPUBatch)

```python
import gpu
from gpu import types

# 创建批处理
batch = gpu.types.GPUBatch(vert_buf=vbo, elem_buf=ibo, prim_type=gpu.types.TRIANGLES)

# 绘制
batch.draw()

# 不同的图元类型
batch = gpu.types.GPUBatch(vert_buf=vbo, prim_type=gpu.types.POINTS)
batch = gpu.types.GPUBatch(vert_buf=vbo, prim_type=gpu.types.LINES)
batch = gpu.types.GPUBatch(vert_buf=vbo, prim_type=gpu.types.TRIANGLE_STRIP)

# 清理
batch.free()
```

## GPU 状态管理 (gpu.state)

```python
from gpu import state

# 混合状态
state.blend_set('ADD')
state.blend_set('ALPHA')
state.blend_set('NONE')

# 深度测试
state.depth_mask_set(True)
state.depth_test_set('LEQUAL')
state.depth_test_set('ALWAYS')
state.depth_test_set('NEVER')
state.depth_test_set('LESS')
state.depth_test_set('EQUAL')

# 面剔除
state.face_culling_set('BACK')
state.face_culling_set('FRONT')
state.face_culling_set('BOTH')
state.face_culling_set('NONE')

# 保存和恢复状态
state.push()
# ... 修改状态
state.pop()

# 视口设置
state.viewport_set(0, 0, 512, 512)

# 裁剪平面
state.scissor_test_set(True)
state.scissor_set(0, 0, 512, 512)

# 颜色掩码
state.color_mask_set(True, True, True, True)

# 清除缓冲区
state.clear_color(0.0, 0.0, 0.0, 1.0)
state.clear(depth=1.0, stencil=0)
state.clear_color()
```

## GPU 矩阵操作 (gpu.matrix)

```python
from gpu import matrix

# 投影矩阵
matrix.load_projection_matrix(1.0, 1920/1080, 0.1, 100.0)

# 视图矩阵
matrix.load_view_matrix(
    ((1, 0, 0, 0),
     (0, 1, 0, 0),
     (0, 0, 1, 0),
     (0, 0, 5, 1))
)

# 模型矩阵
matrix.load_matrix()

# 组合矩阵
matrix.multiply()

# 矩阵操作
matrix.translate(1.0, 2.0, 3.0)
matrix.rotate(0.785, 'X')
matrix.scale(2.0, 2.0, 2.0)

# 获取当前矩阵
mvp = matrix.get_model_view_projection_matrix()
model = matrix.get_model_matrix()
view = matrix.get_view_matrix()
proj = matrix.get_projection_matrix()

# 矩阵堆栈
matrix.push()
matrix.translate(1, 0, 0)
# ... 渲染操作
matrix.pop()

# 正交投影
matrix.ortho_projection(-1, 1, -1, 1, -1, 1)

# 透视投影
matrix.perspective_projection(0.1, 10.0, 45, 1920/1080)

# 逆矩阵
inv_matrix = matrix.inverse(model)
```

## GPU 着色器编程 (gpu.shader)

```python
from gpu import shader

# 创建着色器程序
def create_custom_shader():
    """创建自定义着色器"""
    return shader.from_builtin('UNIFORM_COLOR')

# 内置着色器
from gpu import types

# 统一颜色着色器
color_shader = shader.from_builtin('UNIFORM_COLOR')
color_shader.bind()
color_shader.uniform_float("color", (1.0, 0.5, 0.2, 1.0))

# 3D 着色器
shader_3d = shader.from_builtin('SMOOTH_3D')

# 3D 法线着色器
normal_shader = shader.from_builtin('FLAT_3D')

# 多边形描边着色器
poly_shader = shader.from_builtin('POLYLINE_UNIFORM_COLOR')

# 自定义着色器
custom_shader = shader.compile(
    vertex_code="""
    uniform mat4 u_model_view_projection_matrix;
    in vec3 position;
    in vec3 normal;
    out vec3 v_normal;
    
    void main() {
        gl_Position = u_model_view_projection_matrix * vec4(position, 1.0);
        v_normal = normal;
    }
    """,
    fragment_code="""
    in vec3 v_normal;
    out vec4 fragColor;
    
    void main() {
        vec3 light = normalize(vec3(1.0, 1.0, 1.0));
        float diff = max(dot(v_normal, light), 0.2);
        fragColor = vec4(vec3(diff), 1.0);
    }
    """
)

# 清理
color_shader.dispose()
custom_shader.dispose()
```

## GPU 选择功能 (gpu.select)

```python
from gpu import select

# 初始化选择
select.select_init()

# 绘制对象用于选择
with select.draw_region_overlay(bpy.context.region):
    for obj in bpy.context.scene.objects:
        select.draw_id(obj)

# 获取选择
def get_selected_object(x, y):
    """获取指定位置的选中对象"""
    ids = []
    for obj in bpy.context.scene.objects:
        ids.append(obj)
    
    # 框选
    select.select_box(0, 0, 100, 100)
    
    # 圆形选择
    select.select_circle(50, 50, 20)
    
    # 套索选择
    select.select_lasso([(10, 10), (20, 20), (10, 30)])
    
    return select.select_pick(x, y)
```

## GPU 平台信息 (gpu.platform)

```python
from gpu import platform

# 获取 GPU 信息
info = platform.info_get()
print(f"厂商: {info['VENDOR']}")
print(f"渲染器: {info['RENDERER']}")
print(f"版本: {info['VERSION']}")
print(f"着色语言版本: {info['GLSL_VERSION']}")

# 检查特性支持
print(f"Compute 支持: {platform.info_get()['COMPUTE']}")
print(f"Geometry Shader 支持: {platform.info_get()['GEOMETRY_SHADER']}")
print(f"Tessellation 支持: {platform.info_get()['TESSELLATION_SHADER']}")

# 获取详细 GPU 信息
gpu_info = platform.get_gpu_info()
print(gpu_info)
```

## 完整示例

### 自定义网格绘制

```python
import bpy
import gpu
from gpu import types, matrix, state, shader
import numpy as np

class CustomDrawOperator(bpy.types.Operator):
    bl_idname = "object.custom_draw"
    bl_label = "自定义绘制"
    
    def execute(self, context):
        return {'FINISHED'}
    
    def draw(self, context):
        pass
    
    def invoke(self, context, event):
        # 注册绘制回调
        self.draw_callback = context.space_data.draw_handler_add(
            self.draw_callback_function, (), 'WINDOW', 'POST_VIEW'
        )
        return {'RUNNING_MODAL'}
    
    def draw_callback_function(self):
        """绘制回调函数"""
        # 准备数据
        vertices = np.array([
            [0, 0, 0],
            [1, 0, 0],
            [0, 1, 0],
        ], dtype=np.float32)
        
        colors = np.array([
            [255, 0, 0, 255],
            [0, 255, 0, 255],
            [0, 0, 255, 255],
        ], dtype=np.uint8)
        
        # 创建顶点格式
        fmt = types.GPUVertFormat()
        fmt.attr_add(id="position", comp_type=types.FLOAT3, 
                    fetch=types.FLOAT, offset=0)
        fmt.attr_add(id="color", comp_type=types.UBYTE4, 
                    fetch=types.INT, offset=12)
        
        # 创建缓冲区
        vbo = types.GPUVertBuf(format=fmt, len=3, data=np.concatenate([vertices, colors], axis=1))
        
        # 创建着色器
        sh = shader.from_builtin('SMOOTH_3D')
        sh.bind()
        
        # 设置矩阵
        matrix.load_view_matrix(
            ((1, 0, 0, 0),
             (0, 1, 0, 0),
             (0, 0, 1, 0),
             (0, 0, -5, 1))
        )
        matrix.load_projection_matrix(1.0, 16/9, 0.1, 100.0)
        
        # 创建批处理
        batch = types.GPUBatch(vert_buf=vbo, prim_type=types.TRIANGLES)
        
        # 绘制
        state.blend_set('ALPHA')
        batch.draw(sh)
        
        # 清理
        batch.free()
        sh.dispose()
        vbo.free()
```

### 离屏渲染

```python
import bpy
import gpu
from gpu import types, matrix, state, shader
import numpy as np

def render_to_texture(obj, width=512, height=512):
    """离屏渲染到纹理"""
    # 创建离屏缓冲区
    offscreen = types.GPUOffScreen(width, height)
    
    with offscreen.bind():
        # 清除
        state.clear_color(0.2, 0.2, 0.2, 1.0)
        state.clear(depth=1.0)
        
        # 设置相机
        matrix.load_view_matrix(
            ((1, 0, 0, 0),
             (0, 1, 0, 0),
             (0, 0, 1, 0),
             (0, 0, -5, 1))
        )
        matrix.load_projection_matrix(1.0, width/height, 0.1, 100.0)
        
        # 渲染对象
        # ... 渲染代码
        
        # 读取像素
        buffer = offscreen.texture.read()
    
    # 清理
    offscreen.free()
    
    return buffer
```

### 动态纹理生成

```python
import bpy
import gpu
from gpu import types
import numpy as np

def generate_gradient_texture(width=256, height=256):
    """生成渐变纹理"""
    # 创建图像数据
    data = np.zeros((width, height, 4), dtype=np.uint8)
    
    for y in range(height):
        for x in range(width):
            data[y, x, 0] = int(255 * x / width)  # R
            data[y, x, 1] = int(255 * y / height)  # G
            data[y, x, 2] = 128  # B
            data[y, x, 3] = 255  # A
    
    # 创建纹理
    texture = types.GPUTexture(width, height, data=data)
    
    return texture

def apply_texture_to_material(texture, material_name="Gradient"):
    """应用纹理到材质"""
    # 创建或获取材质
    if material_name not in bpy.data.materials:
        mat = bpy.data.materials.new(name=material_name)
        mat.use_nodes = True
    else:
        mat = bpy.data.materials[material_name]
    
    # 获取着色器节点
    nodes = mat.node_tree.nodes
    principled = nodes.get("Principled BSDF")
    
    if principled:
        # 创建图像节点
        img_node = nodes.new(type='ShaderNodeTexImage')
        img_node.width_hidden = 200
        
        # 设置图像
        img = bpy.data.images.new("DynamicGradient", 
                                  width=256, height=256)
        
        # 从 GPU 纹理复制数据
        gpu_data = texture.read()
        img.pixels = gpu_data.flatten().tolist()
        
        img_node.image = img
        img_node.interpolation = 'Linear'
        
        # 连接到 Principled BSDF
        principled.inputs["Base Color"].default_value = (1, 1, 1, 1)
    
    return mat
```

## 性能优化

### 1. 资源共享

```python
# 共享顶点缓冲区
vbo1 = types.GPUVertBuf(format, len, data)
vbo2 = types.GPUVertBuf(existing=vbo1)
```

### 2. 批处理

```python
# 合并绘制调用
batch1.draw()
batch2.draw()
# 替换为
batches = [batch1, batch2]
for batch in batches:
    batch.draw()
```

### 3. 及时清理

```python
def cleanup_gpu_resources():
    """清理 GPU 资源"""
    for tex in textures:
        tex.free()
    for buf in vbos:
        buf.free()
    for sh in shaders:
        sh.dispose()
```

### 4. 使用上下文管理器

```python
# 推荐
with gpu.types.GPUOffScreen(w, h) as offscreen:
    # 渲染操作
    pass
# 自动清理

# 不推荐
offscreen = gpu.types.GPUOffScreen(w, h)
# 使用后需要手动 offscreen.free()
```

## 常见问题

### Q: 着色器编译失败
A: 检查 GLSL 语法，确保使用正确的版本和语法。

### Q: 纹理不显示
A: 检查纹理绑定单元和采样器设置。

### Q: 性能差
A: 使用批处理减少绘制调用，避免频繁状态切换。

### Q: 内存泄漏
A: 确保所有 GPU 资源正确释放，使用上下文管理器。

## 相关模块

- [gpu.types](gpu_types_module.md) - GPU 类型定义
- [gpu.shader](gpu_shader_module.md) - GPU 着色器
- [gpu.state](gpu_state_module.md) - GPU 状态管理
- [gpu.texture](gpu_texture_module.md) - GPU 纹理
- [gpu.matrix](gpu_matrix_module.md) - GPU 矩阵操作
- [gpu.select](gpu_select_module.md) - GPU 选择
- [gpu.platform](gpu_platform_module.md) - GPU 平台信息


---
## gpu_capabilities

# gpu.capabilities - GPU能力查询模块

gpu.capabilities模块提供了查询GPU硬件能力和特性支持的函数，用于检查GPU功能和确定可用的渲染特性。

## 概述

GPU能力查询模块允许开发者检查当前GPU的硬件能力和API特性支持。这对于编写适应性代码、根据硬件能力调整渲染策略非常重要。

## 核心功能

### 版本查询

```python
import gpu
from gpu.capabilities import (
    gpu_info_get,
    gpu_shader_info_get,
    gpu_capabilities_get
)

def get_gpu_vendor():
    """获取GPU厂商"""
    return gpu_info_get('VENDOR')

def get_gpu_renderer():
    """获取GPU渲染器"""
    return gpu_info_get('RENDERER')

def get_gpu_version():
    """获取GPU驱动版本"""
    return gpu_info_get('VERSION')

def get_gpu_api_version():
    """获取GPU API版本"""
    return gpu_info_get('API_VERSION')

def get_gpu_shader_version():
    """获取着色器版本"""
    return gpu_shader_info_get('VERSION')
```

### 特性检测

```python
from gpu.capabilities import (
    supports_glsl_compat,
    supports_glsl_es,
    supports_compute_shader,
    supports_geometry_shader,
    supports_tessellation_shader
)

def check_opengl_support():
    """检查OpenGL支持"""
    return {
        'glsl_compat': supports_glsl_compat(),
        'glsl_es': supports_glsl_es(),
        'geometry_shader': supports_geometry_shader(),
        'tessellation': supports_tessellation_shader(),
        'compute': supports_compute_shader()
    }

def check_advanced_features():
    """检查高级特性支持"""
    features = {}
    
    if supports_compute_shader():
        features['compute'] = True
    
    if supports_geometry_shader():
        features['geometry'] = True
    
    if supports_tessellation_shader():
        features['tessellation'] = True
    
    return features

def get_max_texture_size():
    """获取最大纹理尺寸"""
    return gpu_capabilities_get('MAX_TEXTURE_SIZE')

def get_max_uniform_blocks():
    """获取最大Uniform块数量"""
    return gpu_capabilities_get('MAX_UNIFORM_BLOCKS')
```

### 性能指标

```python
def get_gpu_memory_size():
    """获取GPU内存大小"""
    return gpu_capabilities_get('GPU_MEMORY_SIZE')

def get_max_texture_units():
    """获取最大纹理单元数"""
    return gpu_capabilities_get('MAX_TEXTURE_UNITS')

def get_max_vertex_attribs():
    """获取最大顶点属性数"""
    return gpu_capabilities_get('MAX_VERTEX_ATTRIBS')

def get_max_varying_floats():
    """获取最大Varying浮点数"""
    return gpu_capabilities_get('MAX_VARYING_FLOATS')

def get_max_compute_work_group_size():
    """获取最大计算工作组大小"""
    return gpu_capabilities_get('MAX_COMPUTE_WORK_GROUP_SIZE')
```

## 实用函数

### 兼容性检查

```python
def is_opengl_es_compatible():
    """检查OpenGL ES兼容性"""
    return supports_glsl_es()

def requires_fallback():
    """检查是否需要回退"""
    return not supports_glsl_compat()

def check_minimum_requirements():
    """检查最低要求"""
    requirements = {
        'glsl_compat': True,
        'texture_size': 4096,
        'texture_units': 8
    }
    
    if not supports_glsl_compat():
        return False, "GLSL compatibility not supported"
    
    if get_max_texture_size() < requirements['texture_size']:
        return False, "Texture size too small"
    
    if get_max_texture_units() < requirements['texture_units']:
        return False, "Not enough texture units"
    
    return True, "All requirements met"

def get_recommended_settings():
    """获取推荐设置"""
    settings = {}
    
    if supports_compute_shader():
        settings['use_compute'] = True
    else:
        settings['use_compute'] = False
    
    if supports_geometry_shader():
        settings['use_geometry'] = True
    else:
        settings['use_geometry'] = False
    
    texture_size = get_max_texture_size()
    if texture_size >= 16384:
        settings['texture_quality'] = 'HIGH'
    elif texture_size >= 4096:
        settings['texture_quality'] = 'MEDIUM'
    else:
        settings['texture_quality'] = 'LOW'
    
    return settings
```

### 能力报告

```python
def generate_capability_report():
    """生成能力报告"""
    report = {
        'vendor': get_gpu_vendor(),
        'renderer': get_gpu_renderer(),
        'version': get_gpu_version(),
        'api_version': get_gpu_api_version(),
        'shader_version': get_gpu_shader_version(),
        'features': check_opengl_support(),
        'limits': {
            'max_texture_size': get_max_texture_size(),
            'max_texture_units': get_max_texture_units(),
            'max_vertex_attribs': get_max_vertex_attribs(),
            'memory_size': get_gpu_memory_size()
        }
    }
    return report

def print_capability_report():
    """打印能力报告"""
    report = generate_capability_report()
    
    print("GPU Capabilities Report")
    print("=" * 40)
    print(f"Vendor: {report['vendor']}")
    print(f"Renderer: {report['renderer']}")
    print(f"Version: {report['version']}")
    print(f"API Version: {report['api_version']}")
    print(f"Shader Version: {report['shader_version']}")
    print("\nFeatures:")
    for feature, supported in report['features'].items():
        print(f"  {feature}: {'Supported' if supported else 'Not Supported'}")
    print("\nLimits:")
    for limit, value in report['limits'].items():
        print(f"  {limit}: {value}")
```

### 特性比较

```python
def compare_capabilities(other_gpu_info):
    """比较GPU能力"""
    current = generate_capability_report()
    comparison = {}
    
    for key in current:
        if key in other_gpu_info:
            comparison[key] = {
                'current': current[key],
                'other': other_gpu_info[key],
                'match': current[key] == other_gpu_info[key]
            }
    
    return comparison

def has_feature(feature_name):
    """检查特定特性"""
    features = check_opengl_support()
    return features.get(feature_name, False)

def is_feature_better(current_value, feature_name):
    """检查特性是否更好"""
    return current_value > get_capability(feature_name)

def get_capability(capability_name):
    """获取特定能力值"""
    capability_map = {
        'texture_size': get_max_texture_size,
        'texture_units': get_max_texture_units,
        'vertex_attribs': get_max_vertex_attribs,
        'memory': get_gpu_memory_size
    }
    
    if capability_name in capability_map:
        return capability_map[capability_name]()
    return None
```

## 常见问题排查

### 特性不可用

GPU驱动问题：
- 更新GPU驱动程序
- 检查GPU是否支持所需特性
- 使用兼容模式

### 性能不足

硬件限制：
- 降低渲染设置
- 禁用高级特性
- 优化着色器代码

### 报告不准确

API版本问题：
- 确认Blender版本支持
- 检查GPU API初始化
- 重新启动Blender


---
## gpu_shader

# gpu.shader - GPU着色器模块

gpu.shader模块提供了GPU着色器程序的创建、编译和管理功能，用于编写自定义GPU渲染效果。

## 概述

GPU着色器模块是Blender GPU库的核心组件，提供了创建和操作GLSL着色器程序的能力。支持顶点着色器、片元着色器和计算着色器。

## 核心功能

### 着色器创建

```python
import gpu
from gpu.shader import (
    ShaderCreateInfo,
    shader_create_from_info,
    Shader,
    Uniform
)

def create_simple_vertex_shader():
    """创建简单顶点着色器"""
    info = ShaderCreateInfo()
    info.define('VERSION', '330')
    info.vertex_in(0, 'POSITION', 'vec3')
    info.vertex_out(0, 'texCoord', 'vec2')
    info.uniform_world_or_model_matrix('ModelMatrix')
    info.vertex_source('''
        in vec3 position;
        out vec2 texCoord;
        uniform mat4 ModelMatrix;
        void main() {
            gl_Position = ModelMatrix * vec4(position, 1.0);
            texCoord = position.xy * 0.5 + 0.5;
        }
    ''')
    info.fragment_source('''
        in vec2 texCoord;
        out vec4 fragColor;
        void main() {
            fragColor = vec4(texCoord, 0.0, 1.0);
        }
    ''')
    return shader_create_from_info(info)

def create_custom_shader(vert_src, frag_src):
    """创建自定义着色器"""
    info = ShaderCreateInfo()
    info.vertex_source(vert_src)
    info.fragment_source(frag_src)
    return shader_create_from_info(info)

def create_basic_color_shader():
    """创建基础颜色着色器"""
    info = ShaderCreateInfo()
    info.define('PRECISION', 'mediump float')
    info.vertex_in(0, 'pos', 'vec3')
    info.vertex_in(1, 'color', 'vec4')
    info.vertex_out(0, 'fragColor', 'vec4')
    info.uniform_world_or_model_matrix('ModelMatrix')
    info.uniform_view_projection_matrix('ViewProjectionMatrix')
    info.vertex_source('''
        in vec3 pos;
        in vec4 color;
        uniform mat4 ModelMatrix;
        uniform mat4 ViewProjectionMatrix;
        out vec4 fragColor;
        void main() {
            gl_Position = ViewProjectionMatrix * ModelMatrix * vec4(pos, 1.0);
            fragColor = color;
        }
    ''')
    info.fragment_source('''
        in vec4 fragColor;
        out vec4 color;
        void main() {
            color = fragColor;
        }
    ''')
    return shader_create_from_info(info)
```

### Uniform管理

```python
from gpu.shader import Uniform, UniformBuf

def set_uniform_float(shader, name, value):
    """设置浮点Uniform"""
    uniform = shader.uniforms.get(name)
    if uniform:
        uniform_float = Uniform(name, 'FLOAT')
        uniform_float.set(value)

def set_uniform_vec2(shader, name, value):
    """设置vec2 Uniform"""
    uniform = Uniform(name, 'VEC2')
    uniform.set(value)

def set_uniform_vec3(shader, name, value):
    """设置vec3 Uniform"""
    uniform = Uniform(name, 'VEC3')
    uniform.set(value)

def set_uniform_vec4(shader, name, value):
    """设置vec4 Uniform"""
    uniform = Uniform(name, 'VEC4')
    uniform.set(value)

def set_uniform_mat4(shader, name, value):
    """设置mat4 Uniform"""
    uniform = Uniform(name, 'MAT4')
    uniform.set(value)

def set_uniform_int(shader, name, value):
    """设置整数Uniform"""
    uniform = Uniform(name, 'INT')
    uniform.set(value)

def set_uniform_sampler(shader, name, unit):
    """设置采样器Uniform"""
    uniform = Uniform(name, 'SAMPLER_2D')
    uniform.set(unit)

def create_uniform_buffer(data, binding=0):
    """创建Uniform缓冲区"""
    ubo = UniformBuf(len(data), binding)
    ubo.update(data)
    return ubo
```

### 批处理渲染

```python
def begin_batch(batch_type='POINTS'):
    """开始批处理"""
    gpu.begin_batch(batch_type)

def end_batch():
    """结束批处理"""
    gpu.end_batch()

def batch_vertex_format(format):
    """设置批处理顶点格式"""
    gpu.batch_vertex_format(format)

def batch_push_vertex(x, y, z, u=0, v=0):
    """推送顶点数据"""
    gpu.batch_vertex_3f(x, y, z)
    gpu.batch_vertex_2f(u, v)

def draw_batch(shader, batch_type='POINTS'):
    """绘制批处理"""
    shader.bind()
    gpu.draw_batch(batch_type)

def create_point_batch(points, colors=None):
    """创建点批处理"""
    batch = gpu.create_batch(
        'POINTS',
        [
            ('POSITION', '3f'),
            ('COLOR', '4f')
        ],
 if colors else None        points,
        colors
    )
    return batch

def create_line_batch(lines, color=(1, 1, 1, 1)):
    """创建线批处理"""
    batch = gpu.create_batch(
        'LINES',
        [
            ('POSITION', '3f'),
            ('COLOR', '4f')
        ],
        lines,
        [color] * len(lines)
    )
    return batch

def create_triangle_batch(triangles, colors=None):
    """创建三角形批处理"""
    batch = gpu.create_batch(
        'TRIANGLES',
        [
            ('POSITION', '3f'),
            ('COLOR', '4f' if colors else None)
        ],
        triangles,
        colors
    )
    return batch
```

## 实用函数

### 着色器编译

```python
def compile_shader(source, shader_type='VERTEX'):
    """编译着色器"""
    shader = gpu.Shader()
    shader.set_source(source)
    return shader.compile(shader_type)

def compile_shader_from_file(filepath, shader_type='VERTEX'):
    """从文件编译着色器"""
    with open(filepath, 'r') as f:
        source = f.read()
    return compile_shader(source, shader_type)

def check_shader_errors(shader):
    """检查着色器错误"""
    log = shader.get_log()
    return log if log else None

def validate_shader(shader):
    """验证着色器"""
    return shader.validate()
```

### 着色器程序

```python
def create_program(vertex_shader, fragment_shader):
    """创建着色器程序"""
    program = gpu.ShaderProgram()
    program.attach_shader(vertex_shader)
    program.attach_shader(fragment_shader)
    return program

def link_program(program):
    """链接程序"""
    return program.link()

def use_program(program):
    """使用程序"""
    program.bind()

def release_program():
    """释放程序"""
    gpu.ShaderProgram.unbind_all()

def get_active_program():
    """获取当前使用的程序"""
    return gpu.ShaderProgram.active_get()
```

### 高级渲染

```python
def draw_instanced(mesh, count, shader):
    """实例化绘制"""
    shader.bind()
    mesh.draw(shader, count=count)

def draw_elements(mesh, shader, mode='TRIANGLES'):
    """索引绘制"""
    shader.bind()
    mesh.draw_elements(shader, mode)

def draw_arrays(mesh, shader, mode='TRIANGLES'):
    """数组绘制"""
    shader.bind()
    mesh.draw(shader, mode)

def draw_range_arrays(mesh, start, end, shader, mode='TRIANGLES'):
    """范围数组绘制"""
    shader.bind()
    mesh.draw_range(shader, start, end, mode)
```

## 常见问题排查

### 着色器编译失败

GLSL语法错误：
- 检查着色器源代码语法
- 确认GLSL版本兼容
- 查看编译日志获取详细信息

### Uniform设置无效

名称或类型错误：
- 确认Uniform名称与着色器中一致
- 检查Uniform类型匹配
- 验证Uniform是否被优化掉

### 批处理性能问题

顶点数量过大：
- 减少单次批处理的顶点数量
- 使用合适的绘制模式
- 考虑分批处理


---
## gpu_matrix

# gpu.matrix - GPU矩阵模块

gpu.matrix模块提供了GPU渲染所需的矩阵操作函数，包括模型视图矩阵、投影矩阵和各种矩阵变换。

## 概述

GPU矩阵模块封装了图形渲染中常用的矩阵操作。用于设置渲染管线中的变换矩阵，处理3D到2D的投影转换，以及各种几何变换。

## 核心功能

### 矩阵初始化

```python
import gpu
from gpu.matrix import (
    identity,
    orthographic,
    perspective,
    lookat
)

def reset_matrices():
    """重置所有矩阵"""
    gpu.matrix.load_identity()

def create_identity_matrix():
    """创建单位矩阵"""
    return identity()

def create_orthographic_matrix(
    left, right, bottom, top, near, far
):
    """创建正交投影矩阵"""
    return orthographic(left, right, bottom, top, near, far)

def create_perspective_matrix(
    fovy, aspect, near, far
):
    """创建透视投影矩阵"""
    return perspective(fovy, aspect, near, far)

def create_lookat_matrix(eye, center, up):
    """创建视图矩阵"""
    return lookat(eye, center, up)
```

### 矩阵操作

```python
from gpu.matrix import (
    translate,
    rotate,
    scale,
    multiply,
    inverse,
    transpose
)

def apply_translation(x, y, z):
    """应用平移变换"""
    gpu.matrix.translate((x, y, z))

def apply_rotation(angle, axis):
    """应用旋转变换"""
    gpu.matrix.rotate(angle, axis)

def apply_scale(x, y, z):
    """应用缩放变换"""
    gpu.matrix.scale((x, y, z))

def multiply_matrices(mat1, mat2):
    """矩阵相乘"""
    return multiply(mat1, mat2)

def get_inverse_matrix(mat):
    """获取矩阵的逆"""
    return inverse(mat)

def get_transpose_matrix(mat):
    """获取矩阵的转置"""
    return transpose(mat)
```

### 投影矩阵

```python
def setup_perspective_projection(fov=45, near=0.1, far=1000):
    """设置透视投影"""
    width = bpy.context.region.width
    height = bpy.context.region.height
    aspect = width / height if height > 0 else 1.0
    
    gpu.matrix.perspective(math.radians(fov), aspect, near, far)

def setup_orthographic_projection(
    left=None, right=None, 
    bottom=None, top=None,
    near=-100, far=100
):
    """设置正交投影"""
    if left is None:
        left = -1
    if right is None:
        right = 1
    if bottom is None:
        bottom = -1
    if top is None:
        top = 1
    
    gpu.matrix.orthographic(left, right, bottom, top, near, far)

def setup_view_matrix(eye, center, up):
    """设置视图矩阵"""
    gpu.matrix.lookat(eye, center, up)

def get_current_projection_matrix():
    """获取当前投影矩阵"""
    return gpu.matrix.get_projection_matrix()

def get_current_model_view_matrix():
    """获取当前模型视图矩阵"""
    return gpu.matrix.get_model_view_matrix()
```

## 实用函数

### 模型变换

```python
def push_matrix():
    """保存矩阵状态"""
    gpu.matrix.push()

def pop_matrix():
    """恢复矩阵状态"""
    gpu.matrix.pop()

def load_matrix(mat):
    """加载矩阵"""
    gpu.matrix.load_matrix(mat)

def store_matrix():
    """存储当前矩阵"""
    return gpu.matrix.get_state()

def restore_matrix(state):
    """恢复矩阵状态"""
    gpu.matrix.set_state(state)

def transform_point(point, matrix=None):
    """变换点"""
    if matrix is None:
        matrix = gpu.matrix.get_model_view_matrix()
    homogeneous = Vector((point.x, point.y, point.z, 1.0))
    transformed = matrix @ homogeneous
    return Vector((transformed.x, transformed.y, transformed.z))
```

### 视图控制

```python
def orbit_view(azimuth, elevation, distance):
    """轨道视图"""
    gpu.matrix.load_identity()
    gpu.matrix.translate((0, 0, -distance))
    gpu.matrix.rotate(math.radians(elevation), (1, 0, 0))
    gpu.matrix.rotate(math.radians(azimuth), (0, 0, 1))

def pan_view(dx, dy):
    """平移视图"""
    gpu.matrix.translate((dx, dy, 0))

def zoom_view(factor):
    """缩放视图"""
    gpu.matrix.scale((factor, factor, factor))

def reset_view():
    """重置视图"""
    gpu.matrix.load_identity()
    gpu.matrix.translate((0, 0, -5))
```

### 矩阵计算

```python
def calculate_normal_matrix(model_view):
    """计算法线矩阵"""
    normal_matrix = model_view.to_3x3().inverted().transposed()
    return normal_matrix

def calculate_shadow_matrix(light_dir, plane):
    """计算阴影矩阵"""
    dot = plane.normal.dot(light_dir)
    shadow_matrix = Matrix([
        [dot - light_dir.x * plane.normal.x, -light_dir.x * plane.normal.y, -light_dir.x * plane.normal.z, 0],
        [-light_dir.y * plane.normal.x, dot - light_dir.y * plane.normal.y, -light_dir.y * plane.normal.z, 0],
        [-light_dir.z * plane.normal.x, -light_dir.z * plane.normal.y, dot - light_dir.z * plane.normal.z, 0],
        [-light_dir.x * plane.constant, -light_dir.y * plane.constant, -light_dir.z * plane.constant, dot]
    ])
    return shadow_matrix

def decompose_matrix(matrix):
    """分解矩阵"""
    location = matrix.translation
    rotation = matrix.to_euler()
    scale = matrix.to_scale()
    return location, rotation, scale
```

## 常见问题排查

### 矩阵变换无效

矩阵未更新：
- 确保调用load_matrix加载新矩阵
- 检查矩阵乘顺序
- 验证矩阵有效性

### 投影错误

参数范围问题：
- 检查near和far值是否合理
- 确认aspect比率为正
- 验证fov值在有效范围内

### 坐标系问题

变换顺序错误：
- 理解矩阵乘法的非交换性
- 检查变换顺序是否符合预期
- 使用push/pop管理矩阵状态


---
## gpu_select

# gpu.select - GPU选择模块

gpu.select模块提供了GPU对象选择功能，用于在3D视图中进行射线投射和对象选择。

## 概述

GPU选择模块封装了GPU渲染中的对象选择功能，支持通过射线投射获取视图中指定位置的对象。这对于开发交互式工具和视口选择功能非常有用。

## 核心功能

### 射线投射

```python
import gpu
from gpu.select import (
    ray_cast,
    ray_cast_all,
    ray_cast_scene,
    bvh_ray_cast
)

def cast_ray_from_view(view_matrix, projection_matrix, mouse_x, mouse_y):
    """从视图位置发射射线"""
    origin, direction = gpu.select.mouse_ray_cast(
        mouse_x, mouse_y, 
        view_matrix, 
        projection_matrix
    )
    return origin, direction

def cast_ray_from_camera(camera, mouse_x, mouse_y):
    """从相机位置发射射线"""
    view_matrix = camera.matrix_world.inverted()
    projection_matrix = camera.calc_matrix_camera(
        bpy.context.scene,
        bpy.context.view_layer
    )
    return cast_ray_from_view(view_matrix, projection_matrix, mouse_x, mouse_y)

def cast_ray_to_object(origin, direction, obj, distance=1000):
    """向对象投射射线"""
    return gpu.select.ray_cast_object(
        origin, 
        direction, 
        obj, 
        distance
    )

def cast_ray_scene(origin, direction, scene=None, distance=1000):
    """在场景中投射射线"""
    if scene is None:
        scene = bpy.context.scene
    return gpu.select.ray_cast_scene(
        origin, 
        direction, 
        scene, 
        distance
    )
```

### 对象选择

```python
from gpu.select import (
    select_object,
    select_objects_in_box,
    select_objects_in_circle,
    deselect_all
)

def pick_object(mouse_x, mouse_y):
    """拾取对象"""
    hit = gpu.select.pick(
        mouse_x, 
        mouse_y,
        bpy.context.view_layer,
        predicate=lambda obj: obj.visible_get()
    )
    return hit

def pick_objects_in_region(x, y, width, height):
    """在区域中拾取对象"""
    hits = gpu.select.pick_region(
        x, y, width, height,
        bpy.context.view_layer
    )
    return hits

def pick_objects_near_point(x, y, radius):
    """在圆形区域内拾取对象"""
    hits = gpu.select.pick_circle(
        x, y, radius,
        bpy.context.view_layer
    )
    return hits

def select_under_mouse(mouse_x, mouse_y, extend=False):
    """选择鼠标下的对象"""
    if not extend:
        deselect_all()
    
    hit = pick_object(mouse_x, mouse_y)
    if hit:
        obj = hit.object
        obj.select_set(True)
        bpy.context.view_layer.objects.active = obj
    return hit
```

### BVH射线投射

```python
from gpu.select import BVHTree

def create_bvh_from_mesh(mesh):
    """从网格创建BVH树"""
    verts = [v.co for v in mesh.vertices]
    faces = [p.vertices for p in mesh.polygons]
    return BVHTree.FromPolygons(verts, faces)

def ray_cast_bvh(bvh_tree, origin, direction, distance=1000):
    """在BVH树上投射射线"""
    return bvh_tree.ray_cast(origin, direction, distance)

def find_closest_point_bvh(bvh_tree, point):
    """查找BVH树中最近的点"""
    return bvh_tree.find_nearest(point)

def find_points_in_sphere_bvh(bvh_tree, center, radius):
    """查找球体内的点"""
    return bvh_tree.find_near(center, radius)

def find_all_intersections_bvh(bvh_tree, origin, direction, distance=1000):
    """查找所有交点"""
    return bvh_tree.ray_cast_all(origin, direction, distance)
```

## 实用函数

### 距离计算

```python
def distance_to_object(origin, direction, obj):
    """计算到对象的距离"""
    hit = cast_ray_to_object(origin, direction, obj)
    if hit:
        return hit.distance
    return None

def distance_to_plane(origin, direction, plane_co, plane_no):
    """计算到平面的距离"""
    denom = direction.dot(plane_no)
    if abs(denom) > 1e-6:
        t = (plane_co - origin).dot(plane_no) / denom
        if t >= 0:
            return t
    return None

def get_closest_intersection(origin, direction, objects):
    """获取最近的交点"""
    closest = None
    min_dist = float('inf')
    
    for obj in objects:
        hit = cast_ray_to_object(origin, direction, obj)
        if hit and hit.distance < min_dist:
            min_dist = hit.distance
            closest = hit
    
    return closest
```

### 区域查询

```python
def get_objects_in_frustum(camera, matrix=None):
    """获取视锥体内的对象"""
    if matrix is None:
        matrix = camera.matrix_world
    
    frustum = gpu.select.calculate_frustum(
        camera.calc_matrix_camera(bpy.context.scene, bpy.context.view_layer),
        bpy.context.region.width,
        bpy.context.region.height
    )
    
    objects_in_frustum = []
    for obj in bpy.context.scene.objects:
        if gpu.select.object_in_frustum(obj, frustum, matrix):
            objects_in_frustum.append(obj)
    
    return objects_in_frustum

def get_objects_near_ray(origin, direction, distance, objects):
    """获取射线附近的对象"""
    nearby = []
    for obj in objects:
        dist = distance_to_object(origin, direction, obj)
        if dist is not None and dist <= distance:
            nearby.append(obj)
    return nearby
```

### 碰撞检测

```python
def check_object_collision(obj1, obj2):
    """检查两个对象是否碰撞"""
    bvh1 = create_bvh_from_mesh(obj1.to_mesh())
    bvh2 = create_bvh_from_mesh(obj2.to_mesh())
    return bvh1.intersects(bvh2)

def get_collision_contacts(obj1, obj2):
    """获取碰撞接触点"""
    bvh1 = create_bvh_from_mesh(obj1.to_mesh())
    bvh2 = create_bvh_from_mesh(obj2.to_mesh())
    return bvh1.intersections(bvh2)

def find_overlapping_bvh(bvh1, bvh2, threshold=0.001):
    """查找重叠区域"""
    overlaps = []
    for tri1 in bvh1.triangles():
        for tri2 in bvh2.triangles():
            if tri1.intersects(tri2, threshold):
                overlaps.append((tri1, tri2))
    return overlaps
```

## 常见问题排查

### 射线未命中

参数问题：
- 检查射线原点和方向
- 确认距离足够大
- 验证对象可见性

### 选择不准确

坐标问题：
- 确认鼠标坐标正确
- 检查视口转换
- 验证投影矩阵

### 性能问题

BVH过大：
- 简化网格BVH
- 使用包围盒预检测
- 限制搜索距离


---
## gpu_platform

# gpu.platform - GPU平台模块

gpu.platform模块提供了查询GPU平台信息和跨平台抽象的功能，用于处理不同GPU后端的兼容性问题。

## 概述

GPU平台模块封装了GPU后端的底层细节，提供统一的接口来查询当前使用的图形API和平台相关信息。这对于编写可移植的GPU代码非常重要。

## 核心功能

### 平台信息

```python
import gpu
from gpu.platform import (
    get_os_type,
    get_gpu_type,
    get_backend_type,
    BACKEND_OPENGL,
    BACKEND_VULKAN,
    BACKEND_CYCLES,
    OS_WINDOWS,
    OS_MACOS,
    OS_LINUX
)

def get_current_os():
    """获取当前操作系统"""
    return get_os_type()

def get_gpu_backend():
    """获取GPU后端"""
    return get_backend_type()

def is_opengl_backend():
    """检查是否为OpenGL后端"""
    return get_gpu_backend() == BACKEND_OPENGL

def is_vulkan_backend():
    """检查是否为Vulkan后端"""
    return get_gpu_backend() == BACKEND_VULKAN

def is_cycles_backend():
    """检查是否为Cycles后端"""
    return get_gpu_backend() == BACKEND_CYCLES
```

### 驱动信息

```python
from gpu.platform import (
    get_driver_version,
    get_driver_info,
    is_driver_supported
)

def get_current_driver_version():
    """获取当前驱动版本"""
    return get_driver_version()

def get_driver_details():
    """获取驱动详情"""
    return get_driver_info()

def check_driver_support():
    """检查驱动支持"""
    info = get_driver_info()
    return {
        'version': info.get('version'),
        'renderer': info.get('renderer'),
        'vendor': info.get('vendor'),
        'supported': is_driver_supported()
    }
```

### 设备信息

```python
from gpu.platform import (
    get_device_info,
    get_device_count,
    get_active_device
)

def get_all_gpu_devices():
    """获取所有GPU设备"""
    return get_device_info()

def get_gpu_device_count():
    """获取GPU设备数量"""
    return get_device_count()

def get_current_device():
    """获取当前设备"""
    return get_active_device()

def select_device(device_id):
    """选择设备"""
    gpu.platform.set_active_device(device_id)
```

## 实用函数

### 兼容性检查

```python
def requires_compatibility_mode():
    """检查是否需要兼容模式"""
    return gpu.platform.requires_compatibility()

def is_feature_supported(feature):
    """检查特性支持"""
    supported_features = [
        'compute_shader',
        'geometry_shader',
        'tessellation_shader',
        'texture_float',
        'texture_half_float',
        'draw_instanced',
        'viewport_array',
        'debug_output'
    ]
    return feature in supported_features

def get_minimum_supported_version():
    """获取最低支持版本"""
    backend = get_gpu_backend()
    if backend == BACKEND_OPENGL:
        return '3.3'
    elif backend == BACKEND_VULKAN:
        return '1.2'
    return None
```

### 平台适配

```python
def get_platform_specific_code(platform):
    """获取平台特定代码"""
    platform_codes = {
        'windows': '''
            #ifdef _WIN32
            // Windows specific code
            #endif
        ''',
        'macos': '''
            #ifdef __APPLE__
            // macOS specific code
            #endif
        ''',
        'linux': '''
            #ifdef __linux__
            // Linux specific code
            #endif
        '''
    }
    return platform_codes.get(platform, '')

def adapt_shader_for_platform(source):
    """适配着色器到平台"""
    backend = get_gpu_backend()
    if backend == BACKEND_OPENGL:
        return adapt_opengl_shader(source)
    elif backend == BACKEND_VULKAN:
        return adapt_vulkan_shader(source)
    return source

def get_platform_limits():
    """获取平台限制"""
    return {
        'max_texture_size': gpu.capabilities_get('MAX_TEXTURE_SIZE'),
        'max_uniform_vectors': gpu.capabilities_get('MAX_UNIFORM_VECTORS'),
        'max_vertex_attribs': gpu.capabilities_get('MAX_VERTEX_ATTRIBS'),
        'max_texture_units': gpu.capabilities_get('MAX_TEXTURE_UNITS')
    }
```

### 错误处理

```python
from gpu.platform import (
    get_error_code,
    get_error_string,
    GPUError
)

def check_gpu_errors():
    """检查GPU错误"""
    error_code = get_error_code()
    if error_code != 0:
        error_msg = get_error_string(error_code)
        raise GPUError(f"GPU Error {error_code}: {error_msg}")

def handle_gpu_exception(exception):
    """处理GPU异常"""
    error_info = {
        'type': type(exception).__name__,
        'message': str(exception),
        'backend': get_gpu_backend(),
        'driver': get_driver_version()
    }
    return error_info

def is_gpu_responsive():
    """检查GPU响应"""
    try:
        check_gpu_errors()
        return True
    except GPUError:
        return False
```

## 常见问题排查

### 平台不支持

后端不可用：
- 检查GPU驱动是否正确安装
- 确认Blender支持该后端
- 尝试切换到其他后端

### 特性不可用

API版本问题：
- 检查GPU能力支持
- 使用兼容模式
- 更新GPU驱动

### 性能问题

后端选择：
- 根据GPU类型选择合适后端
- 考虑使用OpenGL进行兼容性
- 使用Vulkan获得更好性能


---
## gpu_state

# gpu.state - GPU状态管理模块

gpu.state模块提供了GPU渲染状态的设置和管理功能，包括混合模式、深度测试、面剔除等渲染状态控制。

## 概述

GPU状态管理模块允许开发者控制GPU渲染管线中的各种状态设置。正确设置渲染状态对于实现正确的视觉效果和优化渲染性能至关重要。

## 核心功能

### 混合状态

```python
import gpu
from gpu.state import (
    blend_set,
    blend_get,
    BLEND_ADD,
    BLEND_ALPHA,
    BLEND_MULTIPLY,
    BLEND_SUBTRACT,
    BLEND_DISABLED
)

def enable_alpha_blending():
    """启用Alpha混合"""
    blend_set(BLEND_ALPHA)

def enable_additive_blending():
    """启用加法混合"""
    blend_set(BLEND_ADD)

def enable_multiply_blending():
    """启用乘法混合"""
    blend_set(BLEND_MULTIPLY)

def disable_blending():
    """禁用混合"""
    blend_set(BLEND_DISABLED)

def set_custom_blend(src_rgb, dst_rgb, src_alpha, dst_alpha):
    """设置自定义混合"""
    gpu.state.blend_set_func(
        src_rgb, dst_rgb, src_alpha, dst_alpha
    )

def get_current_blend_mode():
    """获取当前混合模式"""
    return blend_get()
```

### 深度状态

```python
from gpu.state import (
    depth_test_set,
    depth_mask_set,
    depth_range_set,
    depth_test_less,
    depth_test_lequal,
    depth_test_equal,
    depth_test_always
)

def enable_depth_test():
    """启用深度测试"""
    depth_test_set(True)

def disable_depth_test():
    """禁用深度测试"""
    depth_test_set(False)

def set_depth_test_func(func='LESS'):
    """设置深度测试函数"""
    if func == 'LESS':
        depth_test_less()
    elif func == 'LEQUAL':
        depth_test_lequal()
    elif func == 'EQUAL':
        depth_test_equal()
    elif func == 'ALWAYS':
        depth_test_always()

def enable_depth_write():
    """启用深度写入"""
    depth_mask_set(True)

def disable_depth_write():
    """禁用深度写入"""
    depth_mask_set(False)

def set_depth_range(near, far):
    """设置深度范围"""
    depth_range_set(near, far)
```

### 模板状态

```python
from gpu.state import (
    stencil_test_set,
    stencil_mask_set,
    stencil_func_set,
    stencil_op_set
)

def enable_stencil_test():
    """启用模板测试"""
    stencil_test_set(True)

def disable_stencil_test():
    """禁用模板测试"""
    stencil_test_set(False)

def set_stencil_mask(mask):
    """设置模板掩码"""
    stencil_mask_set(mask)

def set_stencil_func(func, ref, mask):
    """设置模板函数"""
    stencil_func_set(func, ref, mask)

def set_stencil_op(sfail, dpfail, dppass):
    """设置模板操作"""
    stencil_op_set(sfail, dpfail, dppass)
```

## 实用函数

### 面剔除

```python
from gpu.state import (
    cull_face_set,
    cull_face_front,
    cull_face_back,
    cull_face_front_and_back
)

def enable_culling():
    """启用面剔除"""
    cull_face_set(True)

def disable_culling():
    """禁用面剔除"""
    cull_face_set(False)

def set_cull_face(mode='BACK'):
    """设置剔除面"""
    if mode == 'FRONT':
        cull_face_front()
    elif mode == 'BACK':
        cull_face_back()
    elif mode == 'BOTH':
        cull_face_front_and_back()
```

### 视口设置

```python
from gpu.state import viewport_set, viewport_get

def set_viewport(x, y, width, height):
    """设置视口"""
    viewport_set(x, y, width, height)

def get_viewport():
    """获取当前视口"""
    return viewport_get()

def set_scissor(x, y, width, height):
    """设置裁剪区域"""
    gpu.state.scissor_set(x, y, width, height)

def get_scissor():
    """获取当前裁剪区域"""
    return gpu.state.scissor_get()

def enable_scissor_test():
    """启用裁剪测试"""
    gpu.state.scissor_test_set(True)
```

### 着色器状态

```python
from gpu.state import (
    polygon_mode_set,
    POLYGON_MODE_FILL,
    POLYGON_MODE_LINE,
    POLYGON_MODE_POINT
)

def set_polygon_mode(mode='FILL'):
    """设置多边形模式"""
    if mode == 'FILL':
        polygon_mode_set(POLYGON_MODE_FILL)
    elif mode == 'LINE':
        polygon_mode_set(POLYGON_MODE_LINE)
    elif mode == 'POINT':
        polygon_mode_set(POLYGON_MODE_POINT)

def enable_program_point_size():
    """启用程序点大小"""
    gpu.state.program_point_size_set(True)
```

### 缓冲状态

```python
from gpu.state import (
    color_mask_set,
    clear_color_set,
    clear_depth_stencil_set
)

def set_color_mask(r, g, b, a):
    """设置颜色掩码"""
    color_mask_set(r, g, b, a)

def set_clear_color(r, g, b, a):
    """设置清除颜色"""
    clear_color_set(r, g, b, a)

def set_clear_depth(depth):
    """设置清除深度"""
    clear_depth_stencil_set(depth=depth)

def set_clear_stencil(stencil):
    """设置清除模板"""
    clear_depth_stencil_set(stencil=stencil)
```

## 状态保存恢复

```python
def save_state():
    """保存当前状态"""
    state = {
        'blend': blend_get(),
        'viewport': viewport_get(),
        'depth_test': gpu.state.depth_test_get(),
        'cull_face': gpu.state.cull_face_get()
    }
    return state

def restore_state(state):
    """恢复保存的状态"""
    blend_set(state['blend'])
    viewport_set(*state['viewport'])
    if state['depth_test']:
        enable_depth_test()
    else:
        disable_depth_test()

class StateGuard:
    """状态守卫"""
    def __init__(self):
        self.saved_state = None
    
    def __enter__(self):
        self.saved_state = save_state()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        restore_state(self.saved_state)
```

## 常见问题排查

### 渲染结果异常

状态冲突：
- 检查混合模式是否正确
- 确认深度测试启用状态
- 验证面剔除设置

### 性能问题

状态切换过多：
- 减少状态切换频率
- 批处理相似状态的操作
- 使用状态排序

### 状态未生效

状态应用时机：
- 确保在绘制前设置状态
- 检查是否被其他代码覆盖
- 确认上下文正确


---
## gpu_texture

# gpu.texture - GPU纹理模块

gpu.texture模块提供了GPU纹理对象的创建、加载和管理功能，用于在着色器中使用图像数据。

## 概述

GPU纹理模块封装了纹理操作的各种功能，包括纹理创建、绑定、更新和采样设置。纹理是GPU渲染中用于存储图像数据的重要资源。

## 核心功能

### 纹理创建

```python
import gpu
from gpu.texture import (
    Texture,
    Texture1D,
    Texture2D,
    Texture3D,
    TextureCube,
    is_compatible,
    image_from_blender
)

def create_2d_texture(width, height, data=None, format='RGBA8', samples=0):
    """创建2D纹理"""
    if samples > 0:
        tex = gpu.texture.Texture2DMultisample(width, height, samples, format)
    else:
        tex = gpu.texture.Texture2D(width, height, format)
    if data:
        tex.update(data)
    return tex

def create_3d_texture(width, height, depth, data=None, format='RGBA8'):
    """创建3D纹理"""
    tex = gpu.texture.Texture3D(width, height, depth, format)
    if data:
        tex.update(data)
    return tex

def create_cubemap_texture(size, data=None, format='RGBA8'):
    """创建立方体贴图"""
    tex = gpu.texture.TextureCube(size, format)
    if data:
        tex.update(data)
    return tex

def create_1d_texture(width, data=None, format='RGBA8'):
    """创建1D纹理"""
    tex = gpu.texture.Texture1D(width, format)
    if data:
        tex.update(data)
    return tex
```

### 纹理加载

```python
def load_texture_from_image(image):
    """从Blender图像加载纹理"""
    tex = image_to_texture(image)
    return tex

def load_texture_from_file(filepath):
    """从文件加载纹理"""
    img = bpy.data.images.load(filepath)
    return load_texture_from_image(img)

def load_texture_array(filepaths, width, height, format='RGBA8'):
    """加载纹理数组"""
    tex = gpu.texture.Texture2DArray(width, height, len(filepaths), format)
    for i, filepath in enumerate(filepaths):
        img = bpy.data.images.load(filepath)
        tex.update(img.pixels, layer=i)
    return tex

def load_cubemap_from_files(filepaths, size, format='RGBA8'):
    """从文件加载立方体贴图"""
    tex = gpu.texture.TextureCube(size, format)
    for i, filepath in enumerate(filepaths):
        img = bpy.data.images.load(filepath)
        tex.update(img.pixels, face=i)
    return tex
```

### 纹理更新

```python
def update_texture(tex, data, level=0):
    """更新纹理数据"""
    tex.update(data, level)

def update_texture_region(tex, data, x, y, width, height, level=0):
    """更新纹理区域"""
    tex.update_region(data, x, y, width, height, level)

def replace_texture_data(tex, data):
    """替换纹理数据"""
    tex.replace(data)

def clear_texture(tex, color=(0, 0, 0, 0)):
    """清除纹理"""
    tex.clear(color)
```

## 实用函数

### 纹理采样

```python
from gpu.texture import (
    sampler_wrap_repeat,
    sampler_wrap_clamp,
    sampler_filter_linear,
    sampler_filter_nearest
)

def create_default_sampler():
    """创建默认采样器"""
    return gpu.texture.Sampler()

def create_linear_sampler():
    """创建线性采样器"""
    sampler = gpu.texture.Sampler()
    sampler.filter = sampler_filter_linear()
    return sampler

def create_nearest_sampler():
    """创建最近邻采样器"""
    sampler = gpu.texture.Sampler()
    sampler.filter = sampler_filter_nearest()
    return sampler

def create_repeat_sampler():
    """创建重复寻址采样器"""
    sampler = gpu.texture.Sampler()
    sampler.wrap = sampler_wrap_repeat()
    return sampler

def create_clamped_sampler():
    """创建钳制寻址采样器"""
    sampler = gpu.texture.Sampler()
    sampler.wrap = sampler_wrap_clamp()
    return sampler
```

### 纹理绑定

```python
def bind_texture(tex, unit=0):
    """绑定纹理到单元"""
    tex.bind(unit)

def unbind_texture(unit=0):
    """解绑纹理"""
    gpu.texture.unbind(unit)

def active_texture(unit):
    """激活纹理单元"""
    gpu.texture.active(unit)

def get_bound_texture(unit):
    """获取绑定的纹理"""
    return gpu.texture.unit_get_binding(unit)
```

### 纹理查询

```python
def get_texture_size(tex):
    """获取纹理尺寸"""
    return tex.size

def get_texture_format(tex):
    """获取纹理格式"""
    return tex.format

def get_texture_levels(tex):
    """获取纹理级别数"""
    return tex.levels

def is_texture_ready(tex):
    """检查纹理是否就绪"""
    return tex.is_created()

def get_texture_memory_size(tex):
    """获取纹理内存大小"""
    return tex.memory_size()
```

## 高级功能

### 纹理压缩

```python
def create_compressed_texture(filepath, format='DXT1'):
    """创建压缩纹理"""
    return gpu.texture.create_compressed(filepath, format)

def compress_texture(tex, format='DXT1'):
    """压缩纹理"""
    return tex.compress(format)
```

### 纹理生成

```python
def create_noise_texture(width, height, noise_type='PERLIN'):
    """创建噪声纹理"""
    tex = gpu.texture.Texture2D(width, height, 'RGBA8')
    noise_data = generate_noise_data(width, height, noise_type)
    tex.update(noise_data)
    return tex

def create_gradient_texture(width, height, gradient_type='LINEAR'):
    """创建渐变纹理"""
    tex = gpu.texture.Texture2D(width, height, 'RGBA8')
    gradient_data = generate_gradient_data(width, height, gradient_type)
    tex.update(gradient_data)
    return tex

def create_checker_texture(size, num_checks=8, color1=(1,1,1,1), color2=(0,0,0,1)):
    """创建棋盘纹理"""
    tex = gpu.texture.Texture2D(size, size, 'RGBA8')
    checker_data = generate_checker_data(size, num_checks, color1, color2)
    tex.update(checker_data)
    return tex
```

### 纹理管理

```python
class TextureManager:
    """纹理管理器"""
    def __init__(self):
        self.textures = {}
        self.max_size = 1024
    
    def load(self, name, filepath):
        """加载纹理"""
        if name in self.textures:
            return self.textures[name]
        tex = load_texture_from_file(filepath)
        self.textures[name] = tex
        return tex
    
    def get(self, name):
        """获取纹理"""
        return self.textures.get(name)
    
    def release(self, name):
        """释放纹理"""
        tex = self.textures.pop(name)
        tex.release()
    
    def clear(self):
        """清除所有纹理"""
        for tex in self.textures.values():
            tex.release()
        self.textures.clear()
```

## 常见问题排查

### 纹理加载失败

格式不支持：
- 确认图像格式被支持
- 检查文件路径正确性
- 验证文件完整性

### 纹理采样问题

采样参数错误：
- 检查采样器配置
- 确认纹理坐标范围
- 验证mipmap生成

### 内存不足

纹理过大：
- 使用更小的纹理尺寸
- 考虑纹理压缩
- 及时释放未使用的纹理


---
## gpu_types

# gpu.types - GPU类型定义模块

gpu.types模块定义了Blender GPU库中使用的核心数据类型，包括缓冲区类型、顶点格式、着色器类型等。

## 概述

GPU类型模块定义了GPU渲染所需的底层数据类型。这些类型是构建自定义GPU渲染效果的基础，理解这些类型对于开发GPU扩展至关重要。

## 核心类型

### 缓冲区类型

```python
from gpu.types import (
    GPUBuffer,
    GPUVertBuf,
    GPUIndexBuf,
    GPUUniformBuf
)

class GPUBuffer:
    """GPU缓冲区基类型"""
    def __init__(self, size=None, data=None):
        self._obj = None
        if size or data:
            self._obj = gpu.buffer.create(size or len(data))
            if data:
                self._obj.data = data
    
    def bind(self, binding_point):
        """绑定缓冲区"""
        self._obj.bind(binding_point)
    
    def unbind(self):
        """解绑缓冲区"""
        gpu.buffer.unbind(self._obj)
    
    def clear(self, value):
        """清除缓冲区"""
        self._obj.clear(value)
    
    def read(self, size=0, offset=0):
        """读取缓冲区数据"""
        return self._obj.read(size, offset)

class GPUVertBuf(GPUBuffer):
    """GPU顶点缓冲区"""
    def __init__(self, format, len):
        self._obj = gpu.vertex_buffer.create(format, len)
        self._format = format
        self._len = len
    
    def fill(self, data, start=0):
        """填充顶点数据"""
        self._obj.data = data
        self._obj.update(start)

class GPUIndexBuf(GPUBuffer):
    """GPU索引缓冲区"""
    def __init__(self, type, len):
        self._obj = gpu.index_buffer.create(type, len)
        self._type = type
        self._len = len
    
    def fill(self, data, start=0):
        """填充索引数据"""
        self._obj.data = data
        self._obj.update(start)

class GPUUniformBuf(GPUBuffer):
    """GPU Uniform缓冲区"""
    def __init__(self, size, binding=0):
        self._obj = gpu.uniform_buffer.create(size, binding)
        self._size = size
    
    def update(self, data, offset=0):
        """更新Uniform数据"""
        self._obj.update(data, offset)
```

### 顶点格式

```python
from gpu.types import GPUVertFormat

class GPUVertFormat:
    """GPU顶点格式"""
    def __init__(self):
        self._obj = gpu.vertex_format.create()
        self.attributes = []
    
    def attribute_add(self, name, comp_type, length, fetch_mode):
        """添加顶点属性"""
        self._obj.attribute_add(name, comp_type, length, fetch_mode)
        self.attributes.append({
            'name': name,
            'type': comp_type,
            'length': length,
            'mode': fetch_mode
        })
    
    def clear(self):
        """清除格式"""
        self._obj.clear()
    
    @staticmethod
    def create():
        """创建顶点格式"""
        return GPUVertFormat()
```

### 着色器类型

```python
from gpu.types import (
    GPUShader,
    GPUShaderCreateInfo,
    GPUShaderVertGen,
    GPUShaderVertIn
)

class GPUShader:
    """GPU着色器"""
    def __init__(self, vert, frag=None, geom=None, defines=None):
        self._obj = None
        self._uniforms = {}
        if vert and frag:
            self._obj = gpu.shader.create(vert, frag, geom, defines)
    
    def bind(self):
        """绑定着色器"""
        self._obj.bind()
    
    def unbind(self):
        """解绑着色器"""
        gpu.shader.unbind()
    
    def uniform_int(self, name, value):
        """设置整数Uniform"""
        self._obj.uniform_int(name, value)
    
    def uniform_float(self, name, value):
        """设置浮点Uniform"""
        self._obj.uniform_float(name, value)
    
    def uniform_float_array(self, name, value):
        """设置浮点数组Uniform"""
        self._obj.uniform_float_array(name, value)
    
    def uniform_2fv(self, name, value):
        """设置2维向量Uniform"""
        self._obj.uniform_2fv(name, value)
    
    def uniform_3fv(self, name, value):
        """设置3维向量Uniform"""
        self._obj.uniform_3fv(name, value)
    
    def uniform_4fv(self, name, value):
        """设置4维向量Uniform"""
        self._obj.uniform_4fv(name, value)
    
    def uniform_mat4(self, name, value):
        """设置矩阵Uniform"""
        self._obj.uniform_mat4(name, value)
```

## 扩展类型

### 批量绘制类型

```python
from gpu.types import (
    GPUBatch,
    GPUBatchPresets
)

class GPUBatch:
    """GPU批处理"""
    def __init__(self, type, vertbuf, indexbuf=None):
        self._obj = gpu.batch.create(type, vertbuf._obj, indexbuf._obj if indexbuf else None)
        self._type = type
        self._vertbuf = vertbuf
    
    def draw(self):
        """绘制批处理"""
        self._obj.draw()
    
    def draw_range(self, start, end):
        """绘制范围"""
        self._obj.draw_range(start, end)
    
    def instance(self, count):
        """实例化绘制"""
        self._obj.instance(count)
    
    @staticmethod
    def points(vertbuf):
        """创建点批处理"""
        return GPUBatch('POINTS', vertbuf)
    
    @staticmethod
    def line_strip(vertbuf):
        """创建线带批处理"""
        return GPUBatch('LINE_STRIP', vertbuf)
    
    @staticmethod
    def lines(vertbuf):
        """创建线批处理"""
        return GPUBatch('LINES', vertbuf)
    
    @staticmethod
    def triangles(vertbuf, indexbuf=None):
        """创建三角批处理"""
        return GPUBatch('TRIANGLES', vertbuf, indexbuf)
```

### 纹理类型

```python
from gpu.types import (
    GPUTexture,
    GPUTexture1D,
    GPUTexture2D,
    GPUTexture3D,
    GPUTextureCube
)

class GPUTexture:
    """GPU纹理基类型"""
    def __init__(self):
        self._obj = None
        self._format = 'RGBA8'
        self._size = (0, 0, 0)
    
    def bind(self, unit):
        """绑定纹理"""
        self._obj.bind(unit)
    
    def unbind(self, unit):
        """解绑纹理"""
        gpu.texture.unbind(unit)
    
    def clear(self, color):
        """清除纹理"""
        self._obj.clear(color)
    
    def read(self):
        """读取纹理数据"""
        return self._obj.read()

class GPUTexture2D(GPUTexture):
    """2D纹理"""
    def __init__(self, width, height, format='RGBA8', samples=0):
        super().__init__()
        self._format = format
        self._size = (width, height, 0)
        if samples > 0:
            self._obj = gpu.texture.create_2d_multisample(width, height, samples, format)
        else:
            self._obj = gpu.texture.create_2d(width, height, format)
    
    def update(self, data, level=0):
        """更新纹理数据"""
        self._obj.update(data, level)
    
    def read(self):
        """读取纹理"""
        return self._obj.read()
```

## 实用函数

### 类型创建

```python
def create_vertex_buffer(data, format):
    """创建顶点缓冲区"""
    vbo = GPUVertBuf(format, len(data))
    vbo.fill(data)
    return vbo

def create_index_buffer(data, type='UNSIGNED_INT'):
    """创建索引缓冲区"""
    ibo = GPUIndexBuf(type, len(data))
    ibo.fill(data)
    return ibo

def create_uniform_buffer(data, binding=0):
    """创建Uniform缓冲区"""
    ubo = GPUUniformBuf(len(data) * 4, binding)
    ubo.update(data)
    return ubo

def create_batch(type, vertbuf, indexbuf=None):
    """创建批处理"""
    return GPUBatch(type, vertbuf, indexbuf)
```

### 格式构建

```python
def build_vertex_format(*attributes):
    """构建顶点格式"""
    fmt = GPUVertFormat()
    for name, dtype, length in attributes:
        fmt.attribute_add(name, dtype, length, 'FLOAT')
    return fmt

def pos3_color4_format():
    """位置3分量颜色4分量格式"""
    return build_vertex_format(
        ('pos', 'FLOAT', 3),
        ('color', 'FLOAT', 4)
    )

def pos3_uv2_format():
    """位置3分量UV2分量格式"""
    return build_vertex_format(
        ('pos', 'FLOAT', 3),
        ('uv', 'FLOAT', 2)
    )

def pos3_normal3_format():
    """位置3分量法线3分量格式"""
    return build_vertex_format(
        ('pos', 'FLOAT', 3),
        ('normal', 'FLOAT', 3)
    )
```

## 常见问题排查

### 缓冲区创建失败

资源不足：
- 检查GPU内存
- 减少缓冲区大小
- 分批创建缓冲区

### 顶点格式错误

属性定义问题：
- 确认属性名称唯一
- 检查组件类型正确
- 验证属性数量限制

### 类型不匹配

类型使用错误：
- 使用正确的类型构造函数
- 确认参数类型匹配
- 检查GPU能力支持


---
## gpu_extras

# gpu_extras GPU扩展模块

提供 GPU 编程的实用工具函数，包括批次创建和常用绘图预设。

## 子模块

### gpu_extras.batch 批次模块

用于创建与着色器兼容的 GPU 批次对象。

#### batch_for_shader

```python
import gpu
from gpu_extras import batch

shader = gpu.shader.from_builtin('UNIFORM_COLOR')
batch = batch.batch_for_shader(
    shader,
    'TRIS',
    {"pos": [(0, 0), (1, 0), (1, 1), (0, 1)], "uv": [(0, 0), (1, 0), (1, 1), (0, 1)]},
    indices=[(0, 1, 2), (0, 2, 3)]
)
batch.draw(shader)
```

创建与指定着色器兼容的批次对象。

**参数：**

- `shader` (GPUShader): 要创建兼容批次的着色器。
- `type` (str): 图元类型，'POINTS'、'LINES'、'TRIS' 或 'LINES_ADJ'。
- `content` (dict): 将着色器属性名映射到顶点缓冲区数据的字典。
- `indices` (Buffer | Sequence[int]): 可选的索引缓冲区数据。

**返回：**

- GPUBatch: 配置好的兼容批次对象。

---

### gpu_extras.presets 预设模块

提供常用的 2D 绘图预设函数。

#### draw_circle_2d

```python
from gpu_extras import presets

presets.draw_circle_2d(
    position=(100, 100),
    color=(1.0, 0.0, 0.0, 1.0),
    radius=50.0,
    segments=32
)
```

绘制二维圆形。

**参数：**

- `position` (Sequence[float]): 圆形的 2D 绘制位置。
- `color` (Sequence[float]): 圆形的颜色 (RGBA)。
  要使用透明度，混合模式需设置为 ALPHA。
- `radius` (float): 圆形的半径。
- `segments` (int | None): 绘制圆形使用的线段数。
  更高的值效果更好但绘制时间更长。
  如果为 None 或未指定，将自动计算值。

#### draw_texture_2d

```python
import gpu
from gpu_extras import presets

texture = gpu.texture.from_image(bpy.data.images[0])
presets.draw_texture_2d(
    texture=texture,
    position=(0, 0),
    width=256,
    height=256,
    is_scene_linear_with_rec709_srgb_target=True
)
```

绘制二维纹理。

**参数：**

- `texture` (GPUTexture): 要绘制的 GPUTexture。
  例如使用 gpu.texture.from_image(image) 从 bpy.types.Image 获取纹理。
- `position` (2D Vector): 左下角的位置。
- `width` (float): 绘制时的宽度（不一定是纹理的原始宽度）。
- `height` (float): 绘制时的高度。
- `is_scene_linear_with_rec709_srgb_target` (bool):
  如果 texture 存储在线性颜色空间且目标帧缓冲区使用 Rec.709 sRGB 颜色空间则为 True。
  这在 'PRE_VIEW'、'POST_VIEW' 或 'POST_PIXEL' 绘制处理程序中绘制从 bpy.types.Image 获取的纹理时成立。
  否则假设颜色空间与帧缓冲区的颜色空间匹配。

---

## 使用示例

### 基本批次绘制

```python
import gpu
from gpu_extras import batch

# 创建顶点数据
vertices = [
    (0.0, 0.0, 0.0),  # 顶点 0
    (1.0, 0.0, 0.0),  # 顶点 1
    (1.0, 1.0, 0.0),  # 顶点 2
    (0.0, 1.0, 0.0),  # 顶点 3
]

colors = [
    (1.0, 0.0, 0.0, 1.0),  # 红色
    (0.0, 1.0, 0.0, 1.0),  # 绿色
    (0.0, 0.0, 1.0, 1.0),  # 蓝色
    (1.0, 1.0, 0.0, 1.0),  # 黄色
]

indices = [
    (0, 1, 2),  # 第一个三角形
    (0, 2, 3),  # 第二个三角形
]

# 创建着色器
shader = gpu.shader.from_builtin('UNIFORM_COLOR')

# 创建批次
batch_obj = batch.batch_for_shader(
    shader,
    'TRIS',
    {"pos": vertices, "color": colors},
    indices=indices
)

# 绘制批次
batch_obj.draw(shader)
```

### 绘制圆形

```python
from gpu_extras import presets
import gpu

# 设置混合模式以支持透明度
gpu.state.blend_set('ALPHA')

# 绘制带透明度的圆形
presets.draw_circle_2d(
    position=(200, 200),
    color=(0.0, 0.5, 1.0, 0.8),  # 半透明蓝色
    radius=75.0,
    segments=64
)
```

### 绘制纹理

```python
import bpy
import gpu
from gpu_extras import presets

# 获取图像
image = bpy.data.images.get("Texture")
if image:
    # 创建纹理
    texture = gpu.texture.from_image(image)
    
    # 设置纹理.draw_texture_2绘制
    presetsd(
        texture=texture,
        position=(50, 50),
        width=200,
        height=200,
        is_scene_linear_with_rec709_srgb_target=True
    )
```

### 完整自定义绘制处理程序

```python
import bpy
import gpu
from gpu_extras import batch, presets

draw_handler = None

def draw_callback():
    global draw_handler
    
    # 绘制背景矩形
    presets.draw_texture_2d(
        texture=gpu.texture.from_blank(100, 100),
        position=(10, 10),
        width=100,
        height=100,
        is_scene_linear_with_rec709_srgb_target=True
    )
    
    # 绘制装饰圆
    gpu.state.blend_set('ALPHA')
    presets.draw_circle_2d(
        position=(60, 60),
        color=(1.0, 1.0, 1.0, 0.5),
        radius=30.0
    )

# 注册绘制处理程序
draw_handler = bpy.types.SpaceView3D.draw_handler_add(
    draw_callback,
    (),
    'WINDOW',
    'POST_PIXEL'
)
```

---

## 注意事项

1. **着色器兼容性**: batch_for_shader 函数自动计算与着色器兼容的格式，确保顶点缓冲区属性与着色器输入匹配。

2. **颜色空间**: draw_texture_2d 的 is_scene_linear_with_rec709_srgb_target 参数对于正确显示从 Blender 图像读取的纹理非常重要。

3. **混合模式**: 使用 draw_circle_2d 绘制半透明圆形时，需要先设置 gpu.state.blend_set('ALPHA')。

4. **性能考虑**: draw_circle_2d 的 segments 参数影响性能，较高的值会产生更平滑的圆形但需要更多计算。

5. **绘制顺序**: 在 3D 视图中使用这些函数时，注意绘制顺序和深度测试设置。

---
## gpu_extras_presets

# gpu_extras.presets - GPU绘制预设模块

提供常用的2D绘制预设功能，包括圆形和纹理绘制。

## 概述

`gpu_extras.presets`模块包含两个实用的2D绘制函数，用于在GPU绘图中快速创建常见几何形状。这些预设简化了2D图形绘制过程，无需编写底层着色器代码。

## 函数

### draw_circle_2d

在2D空间中绘制圆形。

```python
gpu_extras.presets.draw_circle_2d(position, color, radius, *, segments=None)
```

**参数：**
- `position` (Sequence[float]) - 圆形的2D绘制位置
- `color` (Sequence[float]) - 圆形颜色 (RGBA格式)
- `radius` (float) - 圆形半径
- `segments` (int | None) - 圆形分段数，值越高越平滑，None则自动计算

**说明：**
- 使用 `ALPHA` 混合模式以支持透明效果
- 自动计算分段数可平衡性能和质量

**示例：**
```python
import gpu
from gpu_extras import presets

# 在屏幕中心绘制红色圆形
center = (bpy.context.region.width / 2, bpy.context.region.height / 2)
presets.draw_circle_2d(center, (1.0, 0.0, 0.0, 1.0), 50.0, segments=64)
```

### draw_texture_2d

绘制2D纹理到帧缓冲区。

```python
gpu_extras.presets.draw_texture_2d(texture, position, width, height, is_scene_linear_with_rec709_srgb_target=False)
```

**参数：**
- `texture` (gpu.types.GPUTexture) - 要绘制的GPU纹理
- `position` (2D Vector) - 纹理左下角位置
- `width` (float) - 绘制宽度
- `height` (float) - 绘制高度
- `is_scene_linear_with_rec709_srgb_target` (bool) - 是否将场景线性颜色空间转换为Rec.709 sRGB

**说明：**
- 用于绘制从 `bpy.types.Image` 获取的纹理
- 在 'PRE_VIEW'、'POST_VIEW' 或 'POST_PIXEL' 绘制处理器中使用时需设置颜色空间转换

**示例：**
```python
import gpu
from gpu import types
from gpu_extras import presets

# 从图像创建GPU纹理并绘制
image = bpy.data.images.get("MyImage")
if image:
    texture = gpu.texture.from_image(image)
    position = (100, 100)
    presets.draw_texture_2d(texture, position, 256, 256)
```

## 使用场景

### UI绘制处理器

在自定义UI面板中绘制装饰性圆形和图像：

```python
import bpy
import gpu
from gpu_extras import presets

class CustomPanel(bpy.types.Panel):
    bl_label = "自定义面板"
    bl_idname = "OBJECT_PT_custom"
    bl_region_type = 'WINDOW'
    bl_space_type = 'VIEW_3D'

    def draw(self, context):
        layout = self.layout
        layout.label(text="绘制示例")

def draw_circle_callback():
    center = (300, 300)
    presets.draw_circle_2d(center, (0.0, 0.8, 1.0, 1.0), 50.0, segments=32)

bpy.types.SpaceView3D.draw_handler_add(draw_circle_callback, (), 'WINDOW', 'POST_PIXEL')
```

### 图像叠加效果

在3D视口上叠加品牌水印或标识：

```python
import bpy
import gpu
from gpu_extras import presets

def draw_watermark():
    if "Logo" in bpy.data.images:
        logo_texture = gpu.texture.from_image(bpy.data.images["Logo"])
        pos = (20, bpy.context.region.height - 120)
        presets.draw_texture_2d(logo_texture, pos, 100, 50, is_scene_linear_with_rec709_srgb_target=True)

bpy.types.SpaceView3D.draw_handler_add(draw_watermark, (), 'WINDOW', 'POST_VIEW')
```

## 注意事项

1. **混合模式设置**：绘制半透明图形前需调用 `gpu.state.blend_set('ALPHA')`
2. **坐标系**：2D绘制使用屏幕坐标系，原点在左下角
3. **纹理兼容性**：确保纹理格式与绘制目标兼容
4. **性能优化**：减少分段数可提高绘制性能


