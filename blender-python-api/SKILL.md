---
name: blender-python-api
description: 完整的Blender 5.0 Python API参考和开发指南。适用于Blender Python脚本开发、插件创建、任务自动化或工具开发。涵盖bpy模块、操作符、类型、bmesh、GPU编程及核心Blender Python API。
metadata:
  short-description: Blender Python API 参考
---

# Blender 5.0 Python API开发指南

本技能提供对Blender 5.0 Python API的全面访问，支持开发插件、自动化脚本和自定义工具。

## 快速入门

```python
import bpy

# 访问活动对象
obj = bpy.context.active_object
print(f"活动对象: {obj.name}")

# 创建新立方体
bpy.ops.mesh.primitive_cube_add()

# 保存blend文件
bpy.ops.wm.save_as_mainfile(filepath="test.blend")
```

## 核心模块概述

Blender的Python API由几个关键模块组成：

### bpy - Blender主API模块
访问Blender数据和功能的主要模块。

**关键组件：**
- `bpy.context` - 上下文相关的数据访问
- `bpy.data` - 访问所有blend文件数据
- `bpy.ops` - Blender命令操作符
- `bpy.types` - 所有Blender数据的类型定义
- `bpy.props` - UI属性定义
- `bpy.path` - 文件路径工具
- `bpy.app` - 应用程序信息
- `bpy.msgbus` - 模块间通信的消息总线

**使用场景：** 通用Blender脚本编写、数据访问和自动化

### bpy.ops - 操作符
以编程方式执行Blender命令的主要方式。

**主要操作符类别：**
- `bpy.ops.mesh` - 网格操作（创建、修改、变换）
- `bpy.ops.object` - 对象操作（添加、删除、复制）
- `bpy.ops.curve` - 曲线操作
- `bpy.ops.armature` - 骨骼/骨架操作
- `bpy.ops.material` - 材质操作
- `bpy.ops.texture` - 纹理操作
- `bpy.ops.render` - 渲染操作
- `bpy.ops.node` - 节点编辑器操作
- `bpy.ops.anim` - 动画操作
- `bpy.ops.scene` - 场景操作
- `bpy.ops.file` - 文件导入导出
- `bpy.ops.screen` - 屏幕/工作区操作
- `bpy.ops.ui` - UI操作
- `bpy.ops.view2d` - 2D视图操作
- `bpy.ops.view3d` - 3D视图操作
- `bpy.ops.script` - 脚本执行
- `bpy.ops.console` - 控制台操作
- `bpy.ops.text` - 文本编辑器操作
- `bpy.ops.image` - 图像操作
- `bpy.ops.sound` - 声音操作
- `bpy.ops.camera` - 相机操作
- `bpy.ops.light` - 光照操作
- `bpy.ops.world` - 世界设置
- `bpy.ops.collection` - 集合操作
- `bpy.ops.outliner` - 大纲操作
- `bpy.ops.constraint` - 约束操作
- `bpy.ops.modifier` - 修改器操作
- `bpy.ops.pose` - 姿势操作
- `bpy.ops.nla` - 非线性动画操作
- `bpy.ops.marker` - 标记操作
- `bpy.ops.mask` - 遮罩操作
- `bpy.ops.paint` - 绘画操作
- `bpy.ops.sculpt` - 雕刻操作
- `bpy.ops.gpencil` - 涂鸦铅笔操作
- `bpy.ops.dpaint` - 动态绘画操作
- `bpy.ops.uv` - UV操作
- `bpy.ops.cloth` - 布料模拟
- `bpy.ops.fluid` - 流体模拟
- `bpy.ops.rigidbody` - 刚体模拟
- `bpy.ops.particle` - 粒子系统
- `bpy.ops.cycles` - Cycles渲染器操作
- `bpy.ops.ed` - 编辑模式操作
- `bpy.ops.transform` - 变换操作
- `bpy.ops.boid` - Boid粒子系统
- `bpy.ops.mball` - Metaball操作
- `bpy.ops.lattice` - 晶格修改器操作
- `bpy.ops.sequencer` - 视频序列编辑
- `bpy.ops.grease_pencil` - 涂鸦铅笔动画
- `bpy.ops.pointcloud` - 点云数据操作
- `bpy.ops.poselib` - 姿势库管理
- `bpy.ops.workspace` - 工作区管理
- `bpy.ops.ptcache` - 物理缓存操作
- `bpy.ops.clip` - 运动跟踪操作
- `bpy.ops.geometry` - 几何计算
- `bpy.ops.surface` - NURBS曲面操作
- `bpy.ops.spreadsheet` - 电子表格编辑器操作
- `bpy.ops.font` - 字体操作
- `bpy.ops.palette` - 调色板操作
- `bpy.ops.brush` - 笔刷操作
- `bpy.ops.buttons` - UI按钮操作
- `bpy.ops.action` - 动作操作
- `bpy.ops.graph` - 图表编辑器操作
- `bpy.ops.info` - 信息操作
- `bpy.ops.preferences` - 首选项操作
- `bpy.ops.asset` - 资源浏览器操作
- `bpy.ops.gizmogroup` - 小工具组操作
- `bpy.ops.export_anim` - 动画导出
- `bpy.ops.export_scene` - 场景导出
- `bpy.ops.import_anim` - 动画导入
- `bpy.ops.import_curve` - 曲线导入
- `bpy.ops.import_scene` - 场景导入
- `bpy.ops.cachefile` - 缓存文件操作
- `bpy.ops.extensions` - 扩展操作
- `bpy.ops.sculpt_curves` - 雕刻曲线操作

**使用场景：** 以编程方式执行Blender命令

### bpy.types - 类型定义
定义所有Blender数据类型及其属性。

**主要类型类别：**
- `bpy.types.Scene` - 场景数据
- `bpy.types.Object` - 对象数据
- `bpy.types.Mesh` - 网格数据
- `bpy.types.Material` - 材质数据
- `bpy.types.Texture` - 纹理数据
- `bpy.types.Image` - 图像数据
- `bpy.types.Armature` - 骨骼数据
- `bpy.types.Bone` - 骨骼数据
- `bpy.types.Curve` - 曲线数据
- `bpy.types.Lattice` - 晶格数据
- `bpy.types.MetaBall` - Metaball数据
- `bpy.types.Camera` - 相机数据
- `bpy.types.Light` - 光照数据
- `bpy.types.World` - 世界数据
- `bpy.types.Speaker` - 扬声器数据
- `bpy.types.Sound` - 声音数据
- `bpy.types.Action` - 动画动作数据
- `bpy.types.Pose` - 姿势数据
- `bpy.types.Constraint` - 约束数据
- `bpy.types.Modifier` - 修改器数据
- `bpy.types.NodeTree` - 节点树数据
- `bpy.types.Node` - 节点数据
- `bpy.types.Collection` - 集合数据
- `bpy.types.BlendData` - Blend文件数据容器
- `bpy.types.Area` - 屏幕区域数据
- `bpy.types.Space` - 编辑器空间数据
- `bpy.types.Region` - UI区域数据
- `bpy.types.Panel` - UI面板数据
- `bpy.types.Menu` - UI菜单数据
- `bpy.types.Operator` - 操作符数据
- `bpy.types.PropertyGroup` - 属性组数据
- `bpy.types.Addon` - 插件数据
- `bpy.types.Preferences` - 首选项数据
- `bpy.types.KeyMap` - 快捷键映射数据
- `bpy.types.KeyMapItem` - 快捷键映射项数据
- `bpy.types.WindowManager` - 窗口管理器数据
- `bpy.types.WorkSpace` - 工作区数据
- `bpy.types.Screen` - 屏幕数据
- `bpy.types.Theme` - 主题数据
- `bpy.types.UILayout` - UI布局数据
- `bpy.types.RenderSettings` - 渲染设置
- `bpy.types.ImagePaint` - 图像绘画数据
- `bpy.types.VertexPaint` - 顶点绘画数据
- `bpy.types.Sculpt` - 雕刻数据
- `bpy.types.GreasePencil` - 涂鸦铅笔数据
- `bpy.types.ParticleSettings` - 粒子设置
- `bpy.types.ClothSettings` - 布料设置
- `bpy.types.SoftBodySettings` - 软体设置
- `bpy.types.RigidBodyWorld` - 刚体世界数据
- `bpy.types.RigidBodyConstraint` - 刚体约束数据
- `bpy.types.MovieClip` - 电影剪辑数据
- `bpy.types.MovieTracking` - 电影跟踪数据
- `bpy.types.Mask` - 遮罩数据
- `bpy.types.MaskLayer` - 遮罩层数据
- `bpy.types.MaskSpline` - 遮罩样条数据
- `bpy.types.AOV` - 任意输出变量数据
- `bpy.types.AOVs` - AOV集合数据
- `bpy.types.AnimData` - 动画数据
- `bpy.types.FCurve` - F曲线数据
- `bpy.types.Keyframe` - 关键帧数据
- `bpy.types.NlaStrip` - NLA条带数据
- `bpy.types.NlaTrack` - NLA轨道数据
- `bpy.types.PoseBone` - 姿势骨骼数据
- `bpy.types.ActionGroup` - 动作组数据
- `bpy.types.ActionSlot` - 动作插槽数据
- `bpy.types.ActionChannelbag` - 动作通道袋数据
- `bpy.types.ActionChannelbagFCurves` - 动作通道袋F曲线数据
- `bpy.types.ActionConstraint` - 动作约束数据
- `bpy.types.ActionLayer` - 动作层数据
- `bpy.types.ActionStrip` - 动作条带数据
- `bpy.types.ActionStrips` - 动作条带集合
- `bpy.types.ActionSlots` - 动作插槽集合
- `bpy.types.ActionKeyframeStrip` - 动作关键帧条带数据
- `bpy.types.ActionPoseMarkers` - 动作姿势标记数据
- `bpy.types.ArmatureConstraint` - 骨骼约束数据
- `bpy.types.ArmatureConstraintTargets` - 骨骼约束目标数据
- `bpy.types.ArmatureEditBones` - 骨骼编辑模式骨骼数据
- `bpy.types.ArmatureBones` - 骨骼集合数据
- `bpy.types.AssetLibraryCollection` - 资源库集合数据
- `bpy.types.AssetLibraryReference` - 资源库引用数据
- `bpy.types.AssetMetaData` - 资源元数据
- `bpy.types.AssetRepresentation` - 资源表示数据
- `bpy.types.AssetShelf` - 资源架数据
- `bpy.types.AssetTag` - 资源标签数据
- `bpy.types.AssetTags` - 资源标签集合
- `bpy.types.AssetWeakReference` - 资源弱引用数据
- `bpy.types.Annotation` - 注释数据
- `bpy.types.AnnotationFrame` - 注释帧数据
- `bpy.types.AnnotationFrames` - 注释帧集合
- `bpy.types.AnnotationLayer` - 注释层数据
- `bpy.types.AnnotationLayers` - 注释层集合
- `bpy.types.AnnotationStroke` - 注释笔划数据
- `bpy.types.AnnotationStrokePoint` - 注释笔划点数据
- `bpy.types.Addon` - 插件数据
- `bpy.types.Addons` - 插件集合
- `bpy.types.AddonPreferences` - 插件首选项数据
- `bpy.types.AreaLight` - 区域光数据
- `bpy.types.AreaSpaces` - 区域空间集合
- `bpy.types.AlphaOverStrip` - Alpha叠加条带数据
- `bpy.types.AlphaUnderStrip` - Alpha下层条带数据
- `bpy.types.AdjustmentStrip` - 调整条带数据
- `bpy.types.AnimationData` - 动画数据
- `bpy.types.AnimationVisualizer` - 动画可视化数据
- `bpy.types.AnimViz` - 动画可视化数据
- `bpy.types.AnimVizMotionPaths` - 动画可视化运动路径数据
- `bpy.types.AnyType` - 任意类型数据
- `bpy.types.ArrayModifier` - 数组修改器数据
- `bpy.types.ArmatureModifier` - 骨骼修改器数据
- `bpy.types.Attribute` - 属性数据
- `bpy.types.AttributeGroup` - 属性组数据
- `bpy.types.AttributeGroupCurves` - 属性组曲线数据
- `bpy.types.AttributeGroupGreasePencil` - 属性组涂鸦铅笔数据
- `bpy.types.AttributeGroupGreasePencilDrawing` - 属性组涂鸦铅笔绘图数据
- `bpy.types.AttributeGroupMesh` - 属性组网格数据
- `bpy.types.AttributeGroupPointCloud` - 属性组点云数据
- `bpy.types.ASSETBROWSER_UL_metadata_tags` - 资源浏览器元数据标签UI列表
- `bpy.types.BakeSettings` - 烘焙设置数据
- `bpy.types.BevelModifier` - 倒角修改器数据
- `bpy.types.BezierSplinePoint` - 贝塞尔样条点数据
- `bpy.types.BlendData` - Blend数据容器
- `bpy.types.BlendDataActions` - Blend数据动作
- `bpy.types.BlendDataAnnotations` - Blend数据注释
- `bpy.types.BlendDataArmatures` - Blend数据骨骼
- `bpy.types.BlendDataBrushes` - Blend数据笔刷
- `bpy.types.BlendDataCacheFiles` - Blend数据缓存文件
- `bpy.types.BlendDataCameras` - Blend数据相机
- `bpy.types.BlendDataCollections` - Blend数据集合
- `bpy.types.BlendDataCurves` - Blend数据曲线
- `bpy.types.BlendDataFonts` - Blend数据字体
- `bpy.types.BlendDataGreasePencilsV3` - Blend数据涂鸦铅笔V3
- `bpy.types.BlendDataHairCurves` - Blend数据毛发曲线
- `bpy.types.BlendDataImages` - Blend数据图像
- `bpy.types.BlendDataLattices` - Blend数据晶格
- `bpy.types.BlendDataLibraries` - Blend数据库
- `bpy.types.BlendDataLights` - Blend数据光照
- `bpy.types.BlendDataLineStyles` - Blend数据线型
- `bpy.types.BlendDataMasks` - Blend数据遮罩
- `bpy.types.BlendDataMaterials` - Blend数据材质
- `bpy.types.BlendDataMeshes` - Blend数据网格
- `bpy.types.BlendDataMovieClips` - Blend数据电影剪辑
- `bpy.types.BlendDataNodeTrees` - Blend数据节点树
- `bpy.types.BlendDataObjects` - Blend数据对象
- `bpy.types.BlendDataPaintCurves` - Blend数据绘画曲线
- `bpy.types.BlendDataParticles` - Blend数据粒子
- `bpy.types.BlendDataPalettes` - Blend数据调色板
- `bpy.types.BlendDataPointClouds` - Blend数据点云
- `bpy.types.BlendDataProbes` - Blend数据探针
- `bpy.types.BlendDataScenes` - Blend数据场景
- `bpy.types.BlendDataScreens` - Blend数据屏幕
- `bpy.types.BlendDataSounds` - Blend数据声音
- `bpy.types.BlendDataSpeakers` - Blend数据扬声器
- `bpy.types.BlendDataTexts` - Blend数据文本
- `bpy.types.BlendDataTextures` - Blend数据纹理
- `bpy.types.BlendDataVolumes` - Blend数据体积
- `bpy.types.BlendDataWindowManagers` - Blend数据窗口管理器
- `bpy.types.BlendDataWorkSpaces` - Blend数据工作区
- `bpy.types.BlendFileColorspace` - Blend文件色彩空间数据
- `bpy.types.BlendImportContext` - Blend导入上下文数据
- `bpy.types.Event` - 事件数据
- `bpy.types.Gizmo` - 小工具数据
- `bpy.types.GizmoGroup` - 小工具组数据
- `bpy.types.ID` - ID数据
- `bpy.types.KeyConfig` - 快捷键配置数据
- `bpy.types.KeyMap` - 快捷键映射数据
- `bpy.types.KeyMapItem` - 快捷键映射项数据
- `bpy.types.KeyingSet` - 关键帧设置数据
- `bpy.types.KeyingSetAll` - 全部关键帧设置数据
- `bpy.types.Library` - 库数据
- `bpy.types.Macro` - 宏数据
- `bpy.types.Mesh` - 网格数据
- `bpy.types.MeshVertex` - 网格顶点数据
- `bpy.types.MeshEdge` - 网格边数据
- `bpy.types.MeshLoop` - 网格循环数据
- `bpy.types.MeshPolygon` - 网格多边形数据
- `bpy.types.Node` - 节点数据
- `bpy.types.NodeInternal` - 节点内部数据
- `bpy.types.NodeLink` - 节点链接数据
- `bpy.types.NodeSocket` - 节点插座数据
- `bpy.types.NodeTree` - 节点树数据
- `bpy.types.Operator` - 操作符数据
- `bpy.types.OperatorProperties` - 操作符属性数据
- `bpy.types.Panel` - 面板数据
- `bpy.types.Pose` - 姿势数据
- `bpy.types.PoseBone` - 姿势骨骼数据
- `bpy.types.PropertyGroup` - 属性组数据
- `bpy.types.RenderEngine` - 渲染引擎数据
- `bpy.types.RenderLayer` - 渲染层数据
- `bpy.types.RenderPass` - 渲染通道数据
- `bpy.types.RenderSettings` - 渲染设置数据
- `bpy.types.Scene` - 场景数据
- `bpy.types.Sequence` - 序列数据
- `bpy.types.SequenceEditor` - 序列编辑器数据
- `bpy.types.SequenceStrip` - 序列条带数据
- `bpy.types.Space` - 空间数据
- `bpy.types.SpaceClipEditor` - 空间剪辑编辑器数据
- `bpy.types.SpaceConsole` - 空间控制台数据
- `bpy.types.SpaceDopeSheetEditor` - 空间动画摄影表编辑器数据
- `bpy.types.SpaceFileBrowser` - 空间文件浏览器数据
- `bpy.types.SpaceGraphEditor` - 空间图表编辑器数据
- `bpy.types.SpaceImageEditor` - 空间图像编辑器数据
- `bpy.types.SpaceInfo` - 空间信息数据
- `bpy.types.SpaceNLA` - 空间NLA数据
- `bpy.types.SpaceNodeEditor` - 空间节点编辑器数据
- `bpy.types.SpaceOutliner` - 空间大纲数据
- `bpy.types.SpacePreferences` - 空间首选项数据
- `bpy.types.SpaceProperties` - 空间属性数据
- `bpy.types.SpaceSequenceEditor` - 空间序列编辑器数据
- `bpy.types.SpaceSpreadsheet` - 空间电子表格数据
- `bpy.types.SpaceStatusBar` - 空间状态栏数据
- `bpy.types.SpaceTextEditor` - 空间文本编辑器数据
- `bpy.types.SpaceTimeline` - 空间时间线数据
- `bpy.types.SpaceTopBar` - 空间顶部栏数据
- `bpy.types.SpaceUserPreferences` - 空间用户首选项数据
- `bpy.types.SpaceView3D` - 空间3D视图数据
- `bpy.types.Theme` - 主题数据
- `bpy.types.Timer` - 定时器数据
- `bpy.types.ToolSettings` - 工具设置数据
- `bpy.types.UIList` - UI列表数据
- `bpy.types.UILayout` - UI布局数据
- `bpy.types.WindowManager` - 窗口管理器数据
- `bpy.types.WorkSpace` - 工作区数据
- `bpy.types.WorkSpaceTool` - 工作区工具数据

**使用场景：** 访问和修改Blender数据结构

### bpy.types - Blender类型详细参考
Blender数据类型的全面文档，包括对象、网格、材质、相机、光照和动画系统。

**内容：**
- 场景和对象管理
- 网格几何类型
- 材质和着色
- 曲线和曲面
- 骨骼和动画
- 相机和光照
- 节点树
- UI组件
- 动画数据类型

**文档：** [bpy_types_detailed_module.md](references/bpy_context_data_types.md)

**使用场景：** 深入理解Blender的数据类型系统

### bpy.ops - Blender操作详细参考
完整的Blender操作参考，涵盖所有命令类别，用于对象操作、编辑和自动化。

**内容：**
- 网格操作（创建、编辑、变换）
- 对象操作（选择、删除、复制）
- 曲线和曲面操作
- 骨骼操作
- 材质和纹理操作
- 渲染操作
- 文件导入导出
- 动画控制
- UI操作

**文档：** [bpy_ops_detailed_module.md](references/bpy_advanced.md)

**使用场景：** 编写以编程方式执行Blender命令的脚本

### bpy.ops.image - 图像操作模块
图像编辑器操作的全面指南，包括图像加载、保存、编辑和UV映射。

**内容：**
- 图像管理（新建、打开、保存、重新加载）
- 图像编辑（调整大小、裁剪、旋转、反转）
- 图像绘制和绘画工具
- 图像滤镜和效果
- UV编辑操作
- 图像采样和投影
- 批量图像处理

**文档：** [bpy_ops_image_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 图像处理、纹理创建、UV编辑工作流

### bpy.ops.armature - 骨骼操作模块
骨骼系统操作的完整参考，包括骨骼创建、编辑和姿势操作。

**内容：**
- 骨骼创建和选择
- 骨骼变换和细分
- 骨骼连接和断开
- 姿势模式操作
- 约束管理
- IK/FK切换
- 权重绘画集成
- 完整绑定示例

**文档：** [bpy_ops_armature_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 角色绑定、骨骼动画、骨骼操作

### bpy.ops.node - 节点编辑器操作模块
着色器、合成和纹理节点节点编辑器操作的全面指南。

**内容：**
- 节点选择和链接
- 节点创建和组织
- 着色器节点操作
- 合成节点操作
- 纹理节点操作
- 节点组管理
- 节点动画
- 批量节点处理

**文档：** [bpy_ops_node_module.md](references/bpy_ops_render_material.md)

**使用场景：** 材质创建、合成工作流、程序化纹理

### bpy.ops.curve - 曲线和曲面操作模块
贝塞尔曲线、NURBS曲线和曲面操作的完整参考。

**内容：**
- 从点创建曲线
- 贝塞尔和NURBS操作
- 曲线点操作
- 曲线细分和平滑
- 曲面创建和编辑
- 曲线修改器和变形
- 路径动画设置
- 曲线工程工具

**文档：** [bpy_ops_curve_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 曲线建模、路径动画、曲面生成

### bpy.ops.material - 材质操作模块
材质创建、分配和预览操作的全面指南。

**内容：**
- 材质创建和管理
- 材质分配给对象
- 预览生成和HDR支持
- 材质槽操作
- 批量材质处理
- 材质比较和转换
- 材质优化
- 导出和导入工作流

**文档：** [bpy_ops_material_module.md](references/bpy_ops_render_material.md)

**使用场景：** 材质管理、资产生组织、工作流优化

### bpy.ops.render - 渲染操作模块
渲染操作的完整参考，包括图像、动画和视口渲染。

**内容：**
- 帧和动画渲染
- 视口和区域渲染
- 渲染层管理
- 渲染通道配置
- 输出设置和格式
- 批量渲染工作流
- 渐进式和分布式渲染
- 渲染优化

**文档：** [bpy_ops_render_module.md](references/bpy_ops_render_material.md)

**使用场景：** 生产渲染、渲染设置、输出配置

### bpy.ops.animation - 动画操作模块
动画关键帧、曲线编辑和播放控制的全面指南。

**内容：**
- 关键帧插入和删除
- 动画曲线操作
- 插值和缓动
- 时间线和播放控制
- 动作和NLA管理
- 动画数据操作
- 批量动画处理
- 动画验证和优化

**文档：** [bpy_ops_animation_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 关键帧动画、动作管理、时间线编辑

### bpy.ops.object - 对象操作模块
Blender中对象创建、选择、变换和管理的完整参考。

**内容：**
- 对象选择（全部、边框、圆形、套索）
- 对象变换（位置、旋转、缩放）
- 对象创建（网格、曲线、空对象、相机、光照）
- 对象复制和删除
- 父子关系
- 集合管理
- 显示和可见性设置
- 约束和修改器操作

**文档：** [bpy_ops_object_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 对象操作、场景组织、资产管理

### bpy.ops.constraint - 约束操作模块
对象动画中约束创建和管理的完整参考。

**内容：**
- 约束创建和编辑
- 约束类型（COPY_LOCATION、COPY_ROTATION、COPY_SCALE、TRACK_TO等）
- 约束目标管理
- 约束影响控制
- 相机跟随系统
- 父子关系
- 眼睛跟踪系统
- 绑定链创建

**文档：** [bpy_ops_constraint_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 角色绑定、相机控制、动画系统、机械动画

### bpy.ops.modifier - 修改器操作模块
网格操作修改器创建和管理的全面指南。

**内容：**
- 修改器创建和配置
- 修改器类型（SUBSURF、ARRAY、BOOLEAN、BEVEL、MIRROR等）
- 修改器应用和移除
- 修改器顺序和堆叠
- 建筑和地形生成
- 布尔运算
- 数组和晶格结构
- 程序化建模

**文档：** [bpy_ops_modifier_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 非破坏性建模、程序化生成、网格修改

### bpy.ops.collection - 集合操作模块
集合管理和组织的完整参考。

**内容：**
- 集合创建和删除
- 对象链接和取消链接
- 集合层次和嵌套
- 集合可见性设置
- 集合选择
- 资产生管理集合
- 项目组织
- 批量操作集合

**文档：** [bpy_ops_collection_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 场景组织、资产管理、可见性控制、批量操作

### bpy.ops.outliner - 大纲操作模块
大纲视图操作和场景层次管理的全面指南。

**内容：**
- 大纲选择和导航
- 树展开和折叠
- 数据块可见性
- 从大纲管理集合
- 搜索和过滤
- 场景层次组织
- 孤立数据清理
- 库覆盖管理

**文档：** [bpy_ops_outliner_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 场景管理、层次导航、数据组织

### bpy.ops.graph - 图表编辑器操作模块
图表编辑器操作和动画曲线操作的完整参考。

**内容：**
- 关键帧插入和删除
- 曲线选择和编辑
- 曲线插值类型
- 曲线平滑和简化
- F曲线修改器
- 动画清理
- 循环设置
- 曲线噪声和效果

**文档：** [bpy_ops_graph_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 动画曲线编辑、关键帧操作、运动优化

### bpy.ops.nla - 非线性动画操作模块
NLA编辑器操作（动画层和混合）的全面指南。

**内容：**
- NLA轨道创建和管理
- 动画条带操作
- 动作推拉
- 动画混合模式
- 分层动画系统
- 动画排序
- 过渡创建
- 随机动画选择

**文档：** [bpy_ops_nla_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 复杂动画系统、角色动画、运动混合

### bpy.ops.mesh - 网格操作模块
编辑模式下网格编辑操作的全面指南，包括顶点、边和面操作。

**内容：**
- 网格选择（顶点、边、面）
- 网格创建和编辑
- 细分和倒角操作
- 挤出、内插和填充操作
- 网格清理和优化
- 法线和拓扑编辑
- 高级网格工具

**文档：** [bpy_ops_mesh_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 网格建模、几何编辑、拓扑优化

### bpy.ops.pose - 姿势操作模块
姿势模式下骨骼姿势操作的完整参考，用于角色动画。

**内容：**
- 骨骼选择和层次
- 姿势变换（位置、旋转、缩放）
- 姿势关键帧操作
- 姿势复制和镜像
- 约束管理
- IK/FK控制
- 姿势动画工作流

**文档：** [bpy_ops_pose_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 角色绑定、姿势动画、骨骼动画

### bpy.ops.ui - UI操作模块
UI操作的全面指南，包括面板、菜单、对话框和用户交互。

**内容：**
- 面板创建和管理
- 菜单和弹出操作
- UI布局和控件
- 快捷键管理
- 自定义操作符创建
- 用户交互对话框
- UI优化

**文档：** [bpy_ops_ui_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** UI自定义、工具创建、插件开发

### bpy.ops.file - 文件操作模块
文件浏览器操作、路径管理和文件操作的完整参考。

**内容：**
- 文件选择和浏览
- 目录操作（创建、删除、重命名）
- 路径操作和工具
- 文件备份和版本控制
- 书签管理
- 文件系统操作

**文档：** [bpy_ops_file_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 文件管理、资产组织、工作流自动化

### bpy.ops.import_export - 导入导出操作模块
导入导出3D模型、动画和媒体文件的全面指南。

**内容：**
- 3D模型导入（OBJ、FBX、glTF、DAE）
- 网格数据导入（PLY、STL、Alembic、USD）
- 动画导入（BVH、MDD、glTF）
- 带格式特定选项的3D模型导出
- 动画导出工作流
- 批处理和格式转换

**文档：** [bpy_ops_import_export_module.md](references/bpy_ops_import_export.md)

**使用场景：** 资产管道、格式转换、工作流集成

### bpy.ops.screen - 屏幕操作模块
屏幕布局、区域管理和视口控制的完整参考。

**内容：**
- 屏幕创建和管理
- 区域分割和配置
- 动画播放控制
- 视口导航和相机控制
- 工作区管理
- 多显示器支持
- UI布局自定义

**文档：** [bpy_ops_screen_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 屏幕自定义、视口管理、工作区设置

### bpy.ops.action - 动作操作模块
动作编辑器和时间线编辑操作的完整参考。

**内容：**
- 关键帧选择和编辑
- 通道操作和管理
- 帧范围和时间线控制
- 复制、粘贴和复制关键帧
- 关键帧类型转换
- 动作数据块管理

**文档：** [bpy_ops_action_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 动画时间线编辑、关键帧管理、运动数据操作

### bpy.ops.anim - 动画操作模块
动画播放和时间线控制操作。

**内容：**
- 动画播放控制
- 时间线导航和帧跳转
- 动画系统管理
- 自动关键帧设置
- 动画通道操作
- 关键帧设置管理

**文档：** [bpy_ops_anim_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 动画预览、时间线导航、播放控制自动化

### bpy.ops.brush - 笔刷操作模块
笔刷管理和预设操作。

**内容：**
- 笔刷创建和配置
- 笔刷预设管理
- 笔刷纹理和曲线设置
- 笔刷图标管理
- 库导入导出
- 批量笔刷操作

**文档：** [bpy_ops_brush_module.md](references/bpy_ops_render_material.md)

**使用场景：** 笔刷自定义、预设管理、工具设置自动化

### bpy.ops.camera - 相机操作模块
3D场景中相机创建和配置。

**内容：**
- 相机创建和设置
- 相机投影设置（透视/正交）
- 镜头和传感器配置
- 景深设置
- 相机视图对齐
- 相机动画和切换

**文档：** [bpy_ops_camera_module.md](references/bpy_ops_render_material.md)

**使用场景：** 场景相机设置、渲染配置、相机动画

### bpy.ops.scene - 场景操作模块
场景管理和配置操作。

**内容：**
- 场景创建和删除
- 帧范围和单位设置
- 场景渲染配置
- 色彩管理设置
- 显示首选项
- 场景数据导出导入

**文档：** [bpy_ops_scene_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 多场景管理、渲染配置、单位系统设置

### bpy.ops.transform - 变换操作模块
对象变换操作（移动、旋转、缩放）。

**内容：**
- 平移操作
- 旋转操作（轨道、旋转、角度）
- 缩放操作
- 剪切和变形操作
- 变换约束
- 比例编辑
- 对齐和吸附操作

**文档：** [bpy_ops_transform_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 对象定位、变换自动化、精确编辑

### bpy.ops.wm - 窗口管理器操作模块
窗口管理、文件操作和系统级功能。

**内容：**
- 文件保存、打开和追加
- 导入导出操作（OBJ、FBX、glTF）
- 窗口布局管理
- 系统操作和首选项
- 最近文件和书签
- 路径打开和脚本执行

**文档：** [bpy_ops_wm_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 文件管理、数据交换、项目自动化、批量导出

### bpy.ops.view3d - 3D视图操作模块
3D视口控制和交互操作。

**内容：**
- 视图切换（顶、底、前、后、左、右）
- 视图导航（缩放、平移、旋转）
- 视口显示选项
- 网格和参考平面控制
- 3D游标操作
- 着色模式切换

**文档：** [bpy_ops_view3d_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 视口控制、透视管理、光标定位

### bpy.ops.sculpt - 雕刻操作模块
雕刻模式操作和工具。

**内容：**
- 笔刷笔触执行
- 工具切换（绘制、平滑、抓取、充气、折痕）
- Dyntopo和网格细节控制
- 遮罩操作（绘制、扩展、填充）
- 对称操作
- 可见性管理
- 细节精炼

**文档：** [bpy_ops_sculpt_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 数字雕刻、有机建模、细节雕刻、遮罩绘画

### bpy.ops.paint - 绘画操作模块
顶点绘画、权重绘画和纹理绘画操作。

**内容：**
- 笔刷笔触执行
- 顶点颜色操作
- 权重绘画和管理
- 图像绘画和投影
- 选择操作
- 可见性控制

**文档：** [bpy_ops_paint_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 顶点着色、权重绘画、纹理绘画自动化

### bpy.ops.particle - 粒子操作模块
粒子系统创建和编辑操作。

**内容：**
- 粒子系统创建
- 粒子编辑（梳理、平滑、滑动）
- 毛发编辑和造型
- 粒子选择和操作
- 动力学模拟
- 粒子数据导出

**文档：** [bpy_ops_particle_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 粒子效果、毛发造型、物理模拟、植被生成

### bpy.ops.grease_pencil - 涂鸦铅笔操作模块
涂鸦铅笔创建和编辑操作。

**内容：**
- 帧管理（active_frame_delete, delete_frame, duplicate_frame等）
- 图层操作（layer_add, layer_remove, layer_move等）
- 描边操作（brush_stroke, stroke_smooth, stroke_simplify等）
- 选择操作（select_all, select_linked, select_similar等）
- 材质操作（material_copy_to_object, material_hide等）
- 绘制图元（primitive_arc, primitive_box, primitive_circle等）
- 曲线转换和编辑（convert_curve_type, set_curve_type等）

**文档：** [bpy_ops_grease_pencil_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 2D绘图、动画草图、故事板绘制、注释标注

### bpy.ops.curves - 曲线操作模块
毛发曲线和曲线对象的创建与编辑。

**内容：**
- 曲线梳理工
- 曲线平滑和滑动
- 曲线选择和操作
- 曲线生成和转换
- 曲线动力学模拟
- 曲线数据导出

**文档：** [bpy_ops_curves_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 毛发造型、植被生成、曲线动画

### bpy.ops.mask - 遮罩操作模块
遮罩创建和编辑操作。

**内容：**
- 遮罩创建和删除
- 遮罩样条线编辑
- 遮罩帧管理
- 遮罩选择操作
- 遮罩动画关键帧
- 遮罩到多边形转换

**文档：** [bpy_ops_mask_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 动态遮罩、合成抠图、动画遮罩

### bpy.ops.text_editor - 文本编辑器操作模块
文本编辑器和脚本编辑操作。

**内容：**
- 文本选择和导航
- 文本编辑（剪切、复制、粘贴、删除）
- 文本查找替换
- 缩进和格式化
- 文本折叠展开
- 脚本运行和调试

**文档：** [bpy_ops_text_editor_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** Python脚本编写、文本编辑自动化

### bpy.ops.info - 信息操作模块
信息区域报告和系统诊断操作。

**内容：**
- 报告管理（清除、选择、复制）
- 系统信息报告
- 操作日志输出
- 统计信息显示

**文档：** [bpy_ops_info_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 调试输出、日志记录、错误报告

### bpy.ops.preferences - 首选项操作模块
Blender首选项设置操作。

**内容：**
- 偏好设置编辑
- 插件管理（启用/禁用）
- 快捷键设置
- 主题配置
- 系统设置保存

**文档：** [bpy_ops_preferences_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 自动化首选项配置、插件管理

### bpy.ops.extensions - 扩展操作模块
Blender扩展和插件管理操作。

**内容：**
- 扩展搜索和安装
- 扩展更新和卸载
- 扩展仓库管理
- 插件启用禁用
- 扩展偏好设置

**文档：** [bpy_ops_extensions_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 插件管理、扩展仓库操作

### bpy.ops.cachefile - 缓存文件操作模块
Alembic和其他缓存文件操作。

**内容：**
- 缓存文件导入导出
- 缓存播放控制
- 缓存重载
- 缓存索引操作

**文档：** [bpy_ops_cachefile_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 复杂场景流、角色动画缓存

### bpy.ops.geometry - 几何操作模块
几何数据直接操作。

**内容：**
- 几何集合运算
- 几何数据转换
- 几何清理优化
- 几何实例化

**文档：** [bpy_ops_geometry_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 几何数据处理、程序化建模

### bpy.ops.gizmogroup - 小工具组操作模块
3D视口小工具组操作。

**内容：**
- 小工具组选择
- 小工具组绑定
- 小工具组显示控制
- 自定义小工具操作

**文档：** [bpy_ops_gizmogroup_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 交互式工具开发、自定义操作界面

### bpy.ops.asset - 资产管理操作模块
Blender资产浏览器和资产管理操作。

**内容：**
- 资产标记和创建
- 资产库管理
- 资产浏览和过滤
- 资产属性编辑
- 资产导入导出

**文档：** [bpy_ops_asset_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 资产管理、资产库构建、资源共享

### bpy.ops.boid - Boid粒子操作模块
群居粒子行为模拟操作。

**内容：**
- Boid粒子生成
- 群居行为设置（分离、对齐、凝聚）
- Boid力场控制
- 碰撞行为配置
- 粒子发射控制

**文档：** [bpy_ops_boid_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 群体动画、鸟类/鱼类模拟、群集行为

### bpy.ops.buttons - 按钮区域操作模块
属性编辑器和按钮区域操作。

**内容：**
- 材质/物体/修改器面板导航
- 数值输入和滑块控制
- 颜色选择器操作
- 菜单弹出控制
- 上下文感知操作

**文档：** [bpy_ops_buttons_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 属性编辑自动化、界面交互脚本

### bpy.ops.clip - 视频剪辑编辑器操作模块
影片剪辑和跟踪操作。

**内容：**
- 跟踪点管理（创建、删除、跟踪）
- 摄像机解算
- 遮罩跟踪
- 剪辑预览控制
- 畸变/去畸变操作
- 稳定化设置

**文档：** [bpy_ops_clip_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 摄像机跟踪、影片合成、稳定化处理

### bpy.ops.cloth - 布料模拟操作模块
布料物理模拟和编辑操作。

**内容：**
- 布料预设应用
- 布料参数编辑
- 布料缓存控制
- 布料褶皱编辑
- 碰撞设置
- 针点操作

**文档：** [bpy_ops_cloth_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 布料动画、衣物模拟、旗帜飘动

### bpy.ops.console - Python控制台操作模块
Python控制台交互操作。

**内容：**
- 控制台历史导航
- 自动补全触发
- 控制台输出清除
- Python代码执行
- 变量检查

**文档：** [bpy_ops_console_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 调试、交互式开发、快速测试

### bpy.ops.cycles - Cycles渲染操作模块
Cycles渲染器专用操作。

**内容：**
- 渲染设备设置
- 采样参数控制
- 光线追踪参数
- 降噪控制
- GPU/CPU渲染切换
- 渲染层管理

**文档：** [bpy_ops_cycles_module.md](references/bpy_ops_render_material.md)

**使用场景：** Cycles渲染配置、性能优化

### bpy.ops.dpaint - 绘图操作模块
顶点绘制和权重绘制操作。

**内容：**
- 顶点颜色绘制
- 权重绘制
- 画笔选择和设置
- 绘制蒙版控制
- 渐变工具
- 平滑工具

**文档：** [bpy_ops_dpaint_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 顶点着色、权重分配、纹理绘制

### bpy.ops.ed - 编辑器操作模块
通用编辑器操作和撤销系统。

**内容：**
- 撤销/重做操作
- 编辑器上下文切换
- 复制/粘贴操作
- 选择操作
- 删除操作

**文档：** [bpy_ops_ed_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 通用编辑功能、撤销系统集成

### bpy.ops.export_anim - 动画导出操作模块
动画数据导出操作。

**内容：**
- 关键帧导出
- 动作数据导出
- 动画曲线导出
- 变形目标导出
- 骨骼动画导出

**文档：** [bpy_ops_export_anim_module.md](references/bpy_ops_import_export.md)

**使用场景：** 动画导出、跨软件传输

### bpy.ops.export_scene - 场景导出操作模块
场景数据导出到各种格式。

**内容：**
- FBX导出
- OBJ导出
- GLTF/GLB导出
- ABC导出
- USD导出
- 导出选项配置

**文档：** [bpy_ops_export_scene_module.md](references/bpy_ops_import_export.md)

**使用场景：** 跨平台协作、游戏资产导出

### bpy.ops.fluid - 流体模拟操作模块
流体物理模拟操作。

**内容：**
- 域设置
- 发射器/障碍物设置
- 初始状态设置
- 缓存管理
- 求解器参数
- 边界条件

**文档：** [bpy_ops_fluid_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 流体模拟、水/烟雾效果

### bpy.ops.gpencil - 涂鸦铅笔操作模块（旧版）
旧版涂鸦铅笔（现为grease_pencil）操作。

**内容：**
- 涂鸦绘制
- 图层管理
- 帧操作
- 材质设置
- 笔刷控制

**文档：** [bpy_ops_gpencil_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 2D绘图、注释标注

### bpy.ops.import_anim - 动画导入操作模块
动画数据导入操作。

**内容：**
- 关键帧导入
- 动作数据导入
- 动画曲线导入
- 变形目标导入
- 骨骼动画导入

**文档：** [bpy_ops_import_anim_module.md](references/bpy_ops_import_export.md)

**使用场景：** 动画导入、跨软件协作

### bpy.ops.import_curve - 曲线导入操作模块
曲线数据导入操作。

**内容：**
- SVG导入
- 矢量图形导入
- 曲线数据解析
- 曲线转换
- 图层映射

**文档：** [bpy_ops_import_curve_module.md](references/bpy_ops_import_export.md)

**使用场景：** 矢量图形导入、图纸矢量化

### bpy.ops.import_scene - 场景导入操作模块
场景数据从各种格式导入。

**内容：**
- FBX导入
- OBJ导入
- GLTF/GLB导入
- ABC导入
- USD导入
- 导入选项配置

**文档：** [bpy_ops_import_scene_module.md](references/bpy_ops_import_export.md)

**使用场景：** 模型导入、资源整合

### bpy.ops.mball - 元球操作模块
元球（Metaball）创建和编辑操作。

**内容：**
- 元球创建
- 元球类型转换
- 元球分辨率设置
- 元球半径调整
- 元球预览控制

**文档：** [bpy_ops_mball_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 有机形状建模、液体模拟

### bpy.ops.poselib - 姿势库操作模块
动作姿势库管理操作。

**内容：**
- 姿势创建和保存
- 姿势应用
- 姿势预览
- 姿势重命名
- 姿势删除
- 姿势插值

**文档：** [bpy_ops_poselib_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 角色动画、动作库管理

### bpy.ops.ptcache - 物理缓存操作模块
物理模拟缓存管理操作。

**内容：**
- 缓存创建和更新
- 缓存重算
- 缓存显示设置
- 缓存烘焙
- 缓存清除
- 缓存时间控制

**文档：** [bpy_ops_ptcache_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 物理模拟缓存管理、性能优化

### bpy.ops.script - Python脚本操作模块
Python脚本执行和管理操作。

**内容：**
- 脚本运行
- 脚本加载
- 脚本注册
- 模块导入
- 脚本重载

**文档：** [bpy_ops_script_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 脚本自动化、批处理操作

### bpy.ops.sound - 音频操作模块
音频播放和编辑操作。

**内容：**
- 音频播放控制
- 音频剪辑管理
- 音量/音调调整
- 音频特效
- 波形显示

**文档：** [bpy_ops_sound_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 音频编辑、视频配音

### bpy.ops.surface - 曲面操作模块
NURBS曲面和曲面建模操作。

**内容：**
- 曲面创建
- 曲面控制点编辑
- 曲面类型转换
- 曲面细分
- 曲面闭合

**文档：** [bpy_ops_surface_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 曲面建模、工业设计

### bpy.ops.text - 文本操作模块
文本对象创建和编辑操作。

**内容：**
- 文本创建
- 文本内容编辑
- 字体选择
- 文本样式设置
- 文本转换为曲线

**文档：** [bpy_ops_text_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 3D文字、标题设计

### bpy.ops.texture - 纹理操作模块
纹理创建和编辑操作。

**内容：**
- 纹理类型选择
- 纹理参数设置
- 纹理预览
- 纹理插槽管理
- 程序纹理控制

**文档：** [bpy_ops_texture_module.md](references/bpy_ops_render_material.md)

**使用场景：** 材质编辑、程序化纹理

### bpy.ops.uilist - UI列表操作模块
自定义UI列表管理操作。

**内容：**
- 列表项添加
- 列表项删除
- 列表项移动
- 列表项重命名
- 列表排序
- 列表过滤

**文档：** [bpy_ops_uilist_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 自定义面板、数据列表管理

### bpy.ops.view2d - 2D视图操作模块
2D视图导航和操作。

**内容：**
- 视图平移
- 视图缩放
- 视图居中
- 区域分割
- 文本编辑器导航
- 图像查看器控制

**文档：** [bpy_ops_view2d_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 2D编辑器导航、视口控制

### bpy.ops.world - 世界环境操作模块
世界环境和环境设置操作。

**内容：**
- 环境颜色设置
- 环境纹理管理
- 背景设置
- 雾效控制
- 环境光照
- 视图层设置

**文档：** [bpy_ops_world_module.md](references/bpy_ops_render_material.md)

**使用场景：** 环境配置、背景设置

### bpy.ops.rigidbody - 刚体物理操作模块
刚体物理模拟操作。

**内容：**
- 刚体添加（主动/被动）
- 刚体删除
- 刚体类型转换
- 刚体约束设置
- 刚体缓存烘焙
- 刚体物理属性编辑

**文档：** [bpy_ops_rigidbody_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 物理模拟、碰撞效果、破坏动画

### bpy.ops.pointcloud - 点云操作模块
点云数据创建和编辑操作。

**内容：**
- 点云创建
- 点云转换
- 点云渲染设置
- 点云选择操作
- 点云属性编辑
- 点云与网格转换

**文档：** [bpy_ops_pointcloud_module.md](references/bpy_ops_mesh_curve.md)

**使用场景：** 点云处理、LIDAR数据、可视化

### bpy.ops.paintcurve - 绘制曲线操作模块
绘制曲线工具操作。

**内容：**
- 绘制曲线选择
- 绘制曲线应用
- 绘制曲线删除
- 绘制曲线重命名
- 绘制曲线复制

**文档：** [bpy_ops_paintcurve_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 笔刷轨迹记录、绘制动画

### bpy.ops.workspace - 工作区操作模块
工作区和界面布局操作。

**内容：**
- 工作区创建和删除
- 工作区切换
- 工作区重命名
- 工作区保存
- 界面布局编辑
- 屏幕区域管理

**文档：** [bpy_ops_workspace_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 多工作区管理、界面定制

### bpy.ops.sequencer - 序列编辑器操作模块
视频序列和音频编辑操作。

**内容：**
- 片段添加（视频/图像/音频/文字）
- 片段删除和移动
- 片段分割和连接
- 片段速度控制
- 转场和特效
- 音频混合控制
- 时间线导航

**文档：** [bpy_ops_sequencer_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 视频编辑、特效合成、时间线编辑

### bpy.ops.sculpt_curves - 曲线雕刻操作模块
曲线雕刻模式操作。

**内容：**
- 曲线梳理工
- 曲线平滑工
- 曲线增长/缩短
- 曲线选择操作
- 曲线密度控制
- 曲线半径调整

**文档：** [bpy_ops_sculpt_curves_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 毛发雕刻、曲线造型

### bpy.ops.spreadsheet - 表格编辑器操作模块
表格数据查看和编辑操作。

**内容：**
- 单元格导航
- 数据排序和过滤
- 列宽调整
- 视图模式切换
- 导出操作
- 数据复制粘贴

**文档：** [bpy_ops_spreadsheet_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 数据查看、属性检查、批量编辑

### bpy - Blender Python模块核心参考
Blender Python API的核心模块，提供对Blender所有功能的程序化访问。

**关键组件：**
- `bpy.ops` - 所有操作符函数
- `bpy.data` - Blender数据块访问
- `bpy.context` - 当前上下文访问
- `bpy.props` - 属性定义系统
- `bpy.path` - 文件路径工具
- `bpy.utils` - 实用工具函数
- `bpy.app` - 应用程序信息
- `bpy.msgbus` - 消息订阅系统

**使用场景：** Blender脚本编写、插件开发、批量处理

### bpy.app - Blender应用程序信息模块
Blender应用程序信息和全局设置访问。

**内容：**
- 版本信息（version, version_string）
- 构建信息（build_cinfo, build_date）
- 子模块访问（handlers, timers, icons, translations）
- 操作系统检测（os.name）
- Python路径和版本
- 内存使用统计

**文档：** [bpy_app_module.md](references/other_modules.md)

**使用场景：** 版本检查、系统适配、构建验证

### bpy.app.handlers - 应用程序事件处理器模块
Blender生命周期事件钩子函数。

**内容：**
- 加载回调（load_post, load_pre）
- 保存回调（save_post, save_pre）
- 渲染回调（render_init, render_complete）
- 帧更改回调（frame_change_pre/post）
- 场景初始化回调
- 撤消/重做回调

**文档：** [bpy_app_handlers_module.md](references/other_modules.md)

**使用场景：** 自动任务、状态同步、数据验证

### bpy.app.timers - 应用程序定时器模块
Blender内置定时器系统。

**内容：**
- 定时器注册和注销
- 定时器间隔设置
- 定时器函数定义
- 多次执行控制
- 定时器优先级

**文档：** [bpy_app_timers_module.md](references/other_modules.md)

**使用场景：** 定时任务、后台处理、周期性更新

### bpy.app.icons - 应用程序图标模块
Blender图标系统访问。

**内容：**
- 图标预加载
- 图标ID获取
- 图标渲染
- 自定义图标管理

**文档：** [bpy_app_icons_module.md](references/other_modules.md)

**使用场景：** UI定制、图标显示、界面开发

### bpy.app.translations - 应用程序翻译模块
Blender国际化翻译系统。

**内容：**
- 文本翻译函数
- 语言代码
- 翻译上下文
- 本地化字符串

**文档：** [bpy_app_translations_module.md](references/other_modules.md)

**使用场景：** 多语言插件开发、界面本地化

### bpy.context - Blender上下文模块
Blender当前状态和活动对象访问。

**内容：**
- 活动对象访问（active_object, selected_objects）
- 编辑器区域访问（area, regions, spaces）
- 场景和世界访问
- 工具设置和偏好
- 模式检测（object_mode, edit_mode）
- 视图3D和选择状态

**文档：** [bpy_context_module.md](references/bpy_context_data_types.md)

**使用场景：** 上下文感知脚本、条件操作、UI交互

### bpy.data - Blender数据模块
Blender所有数据块的集合访问。

**内容：**
- 对象（objects）
- 材质（materials）
- 纹理（textures）
- 图像（images）
- 场景（scenes）
- 集合（collections）
- 动作（actions）
- 相机（cameras）
- 灯光（lights）
- 曲线（curves）
- 网格（meshes）

**文档：** [bpy_data_module.md](references/bpy_context_data_types.md)

**使用场景：** 数据遍历、资源管理、场景构建

### bpy.msgbus - 消息总线订阅模块
Blender内部消息系统。

**内容：**
- 消息发布（publish）
- 消息订阅（subscribe）
- 消息取消订阅
- 订阅者标识
- 消息过滤

**文档：** [bpy_msgbus_module.md](references/other_modules.md)

**使用场景：** 状态同步、事件驱动、实时更新

### bpy.path - Blender路径模块
Blender相关路径操作工具。

**内容：**
- 绝对/相对路径转换
- 目录路径获取
- 文件名提取
- 扩展名处理
- 路径归一化
- 资源路径解析

**文档：** [bpy_path_module.md](references/bpy_props_path.md)

**使用场景：** 文件操作、资源加载、路径处理

### bpy.props - Blender属性定义模块
Blender属性系统定义。

**内容：**
- 属性类型（Int, Float, String, Bool, Enum, Pointer, Collection）
- 属性组定义
- 动画属性设置
- 属性更新回调
- 属性绘制函数

**文档：** [bpy_props_module.md](references/bpy_props_path.md)

**使用场景：** 插件UI、运算符属性、自定义数据类型

### bpy.utils - Blender工具函数模块
Blender内置工具函数集合。

**内容：**
- 注册/注销函数（register_class, unregister_class）
- 模块工具
- 偏好添加函数
- 批处理工具
- 路径工具
- 文本编码工具

**文档：** [bpy_utils_module.md](references/other_modules.md)

**使用场景：** 插件开发、类注册、资源管理

### bmesh - BMesh模块
用于高级网格操作的底层网格编辑功能。

**关键组件：**
- `bmesh.types` - BMesh类型定义
- `bmesh.ops` - BMesh操作
- `bmesh.geometry` - 几何操作
- `bmesh.utils` - 工具函数

**使用场景：** 高级网格编辑、自定义网格算法

### bmesh.types - BMesh类型详细参考
BMesh数据类型的深入文档，包括顶点、边、面、循环和图层数据，用于高级网格操作。

**内容：**
- BMesh核心对象和生命周期管理
- BMVert - 顶点类型，可访问坐标和法线
- BMEdge - 边类型，具有几何和选择状态
- BMFace - 面类型，可计算面积、法线和中心
- BMLoop - 循环类型，用于顶点-边-面关系
- 图层数据（变形、UV、颜色、自定义图层）
- 图层数据类型（int、float、string、vector）
- 性能模式和最佳实践
- 与传统网格数据的集成

**文档：** [bmesh_types_detailed_module.md](references/bmesh_module.md)

**使用场景：** 高级网格编辑、自定义几何算法、批量网格操作

### bpy.props - 属性模块
用于在Blender中创建自定义属性的属性定义。

**属性类型：**
- `bpy.props.BoolProperty` - 布尔属性
- `bpy.props.IntProperty` - 整型属性
- `bpy.props.FloatProperty` - 浮点属性
- `bpy.props.StringProperty` - 字符串属性
- `bpy.props.EnumProperty` - 枚举属性
- `bpy.props.CollectionProperty` - 集合属性
- `bpy.props.PointerProperty` - 指针属性
- `bpy.props.FloatVectorProperty` - 浮点向量属性
- `bpy.props.FloatColorProperty` - 浮点颜色属性
- `bpy.props.StringProperty` - 字符串属性
- `bpy.props.IntVectorProperty` - 整型向量属性
- `bpy.props.BoolVectorProperty` - 布尔向量属性

**使用场景：** 为插件和工具创建自定义属性

### bpy.path - 路径模块
用于Blender的文件路径工具。

**函数：**
- `bpy.path.abspath` - 获取绝对路径
- `bpy.path.basename` - 获取基名称
- `bpy.path.dirname` - 获取目录名称
- `bpy.path.display_name` - 获取显示名称
- `bpy.path.display_name_from_filepath` - 从文件路径获取显示名称
- `bpy.path.display_name_to_filepath` - 从显示名称获取文件路径
- `bpy.path.ensure_ext` - 确保文件扩展名
- `bpy.path.is_subdir` - 检查路径是否为子目录
- `bpy.path.native_pathsep` - 获取本机路径分隔符
- `bpy.path.resolve_ncase` - 不区分大小写解析路径

**使用场景：** 文件路径操作

### bpy.utils.units - 单位模块
用于测量格式化和转换的单位系统工具。

**函数：**
- `units.length` - 格式化长度值（带单位）
- `units.angle` - 格式化角度值（带单位）
- `units.time` - 格式化时间值（带单位）

**功能：**
- 支持公制（METRIC）和英制（IMPERIAL）单位系统
- 自动单位转换和显示
- 自定义精度格式化
- 批量测量处理

**文档：** [bpy_utils_units_module.md](references/other_modules.md)

**使用场景：** 单位系统管理和测量格式化

### bpy.app.handlers - 应用程序处理器模块
用于Blender应用程序事件的处理器。

**处理器类型：**
- `bpy.app.handlers.frame_change_pre` - 帧变化前
- `bpy.app.handlers.frame_change_post` - 帧变化后
- `bpy.app.handlers.render_pre` - 渲染前
- `bpy.app.handlers.render_post` - 渲染后
- `bpy.app.handlers.render_cancel` - 渲染取消
- `bpy.app.handlers.save_pre` - 保存前
- `bpy.app.handlers.save_post` - 保存后
- `bpy.app.handlers.load_pre` - 加载前
- `bpy.app.handlers.load_post` - 加载后
- `bpy.app.handlers.depsgraph_update_pre` - 依赖图更新前
- `bpy.app.handlers.depsgraph_update_post` - 依赖图更新后
- `bpy.app.handlers.object_insert_pre` - 对象插入前
- `bpy.app.handlers.object_insert_post` - 对象插入后
- `bpy.app.handlers.scene_update_pre` - 场景更新前
- `bpy.app.handlers.scene_update_post` - 场景更新后

**文档：** [bpy_app_handlers_module.md](references/other_modules.md)

**使用场景：** 响应Blender应用程序事件

### bpy.msgbus - 消息总线模块
模块间通信系统。

**函数：**
- `bpy.msgbus.clear_by_owner` - 按所有者清除消息
- `bpy.msgbus.publish_rna` - 发布RNA消息
- `bpy.msgbus.subscribe_rna` - 订阅RNA消息

**使用场景：** 模块间通信

### bpy.app.icons - 应用程序图标模块
用于自定义UI图标的图标加载和管理。

**函数：**
- `bpy.app.icons.load` - 从文件加载图标
- `bpy.app.icons.get` - 获取已加载的图标
- `bpy.app.icons.clear` - 清除图标缓存

**功能：**
- 支持多种图像格式（PNG、JPG、BMP、TGA）
- 图标缓存和管理
- UI图标集成
- 动态图标生成支持

**文档：** [bpy_app_icons_module.md](references/other_modules.md)

**使用场景：** 插件中的自定义图标管理

### bpy.app.translations - 应用程序翻译模块
国际化、本地化支持。

**函数：**
- `bpy.app.translations.register` - 注册翻译表
- `bpy.app.translations.unregister` - 注销翻译表
- `bpy.app.translations.pgettext` - 获取带上下文的翻译文本

**功能：**
- 多语言支持
- 上下文感知翻译
- 翻译域管理
- 动态语言切换

**文档：** [bpy_app_translations_module.md](references/other_modules.md)

**使用场景：** 创建多语言插件

### idprop - ID属性模块
Blender ID属性的类型访问，用于处理自定义属性数据的序列化和反序列化。

**类型：**
- `idprop.types.IDPropertyArray` - ID属性数组类型，支持数值数组的存储和操作
  - `to_list()` - 将数组转换为Python列表
  - `typecode` - 数组数据类型代码（'f'表示float，'d'表示double，'i'表示int，'b'表示bool）
- `idprop.types.IDPropertyGroup` - ID属性组类型，支持字典-like的键值对存储
  - `clear()` - 清除组中所有成员
  - `get(key, default=None)` - 获取指定键的值
  - `keys()` - 返回所有键的列表
  - `items()` - 返回键值对迭代器
  - `values()` - 返回所有值的迭代器
  - `to_dict()` - 转换为纯Python字典
  - `update(other)` - 使用另一个组或字典更新当前组

**使用场景：** 自定义属性存储、对象数据序列化、插件配置管理

### idprop.types - ID属性类型详细参考
ID属性类型系统的深入文档，包括数组和组类型的详细用法。

**内容：**
- IDPropertyArray - 数值数组类型
  - 创建和初始化
  - 元素访问和修改
  - 类型转换（to_list）
  - 类型代码和类型检查
- IDPropertyGroup - 键值组类型
  - 嵌套属性组
  - 递归遍历
  - 与Python字典互操作
  - 序列化和反序列化
- 属性验证和错误处理
- 性能优化技巧

**文档：** [idprop_types_module.md](references/bpy_props_path.md)

**使用场景：** 高级自定义属性、复杂数据结构、插件数据存储

### bpy.utils - 工具模块
Blender Python API中各种工具函数，用于对象操作、注册、序列化等任务。

**类注册函数：**
- `bpy.utils.register_class(cls)` - 注册Blender类（运算符、面板、属性组等）
- `bpy.utils.unregister_class(cls)` - 注销类
- `bpy.utils.register_submodule_factory(package, submodule_names)` - 批量注册模块中的类
- `bpy.utils.unregister_manual_map(manual_map)` - 注销手动映射

**对象工具函数：**
- `bpy.utils.object_pack_all()` - 打包所有未打包的图像
- `bpy.utils.object_unique_id(obj)` - 获取对象的唯一标识符

**文档：** [bpy_utils_module.md](references/other_modules.md)

**使用场景：** 注册和注销自定义类、序列化Blender数据

### bpy.utils.previews - 预览图标模块
用于创建和管理自定义UI预览图标。

**函数：**
- `previews.new()` - 创建新的预览集合
- `previews.clear()` - 清除预览集合
- `previews.load(name, path, filetype)` - 从文件加载预览图标

**使用场景：** 插件UI图标管理、自定义预览图像

**文档：** [bpy_utils_previews_module.md](references/other_modules.md)

### bpy.app - 应用程序模块
Blender应用程序级别信息和工具的访问，包括版本信息、图标管理、定时器、翻译等。

**应用程序信息：**
- `bpy.app.version` - Blender版本号（tuple格式）
- `bpy.app.version_string` - Blender版本字符串
- `bpy.app.binary_path` - Blender可执行文件路径
- `bpy.app.tempdir` - Blender临时目录
- `bpy.app.driver_namespace` - 驱动命名空间（用于存储自定义驱动函数）

**子模块：**
- `bpy.app.handlers` - 应用程序事件处理器
- `bpy.app.icons` - 应用程序图标管理
- `bpy.app.translations` - 应用程序翻译支持
- `bpy.app.timers` - 应用程序定时器

**使用场景：** 获取Blender版本信息、管理应用程序级资源

**文档：** [bpy_app_module.md](references/other_modules.md)

### bpy.data - 数据模块
访问所有blend文件数据，包括对象、网格、材质、纹理、场景等。

**主要数据集合：**
- `bpy.data.objects` - 所有场景中的对象
- `bpy.data.meshes` - 所有网格数据
- `bpy.data.materials` - 所有材质
- `bpy.data.textures` - 所有纹理
- `bpy.data.images` - 所有图像
- `bpy.data.cameras` - 所有相机
- `bpy.data.lights` - 所有灯光
- `bpy.data.scenes` - 所有场景
- `bpy.data.actions` - 所有动画动作
- `bpy.data.armatures` - 所有骨架数据
- `bpy.data.curves` - 所有曲线数据
- `bpy.data.libraries` - 所有库文件引用
- `bpy.data.texts` - 所有文本数据块
- `bpy.data.sounds` - 所有声音
- `bpy.data.fonts` - 所有字体
- `bpy.data.node_groups` - 所有节点组
- `bpy.data.brushes` - 所有笔刷
- `bpy.data.particles` - 所有粒子设置
- `bpy.data.worlds` - 所有世界设置

**通用属性：**
- `collections` - 数据集合列表
- `slurped` - 数据是否来自库文件

**使用场景：** 访问和管理Blender数据、场景数据操作、资源管理

**文档：** [bpy_data_module.md](references/bpy_context_data_types.md)

### bpy.context - 上下文模块
访问Blender当前上下文相关的数据，包括活动对象、选定对象、编辑模式状态等。

**主要上下文属性：**
- `bpy.context.active_object` - 当前活动对象
- `bpy.context.selected_objects` - 所有选定的对象
- `bpy.context.visible_objects` - 所有可见对象
- `bpy.context.editable_objects` - 所有可编辑对象
- `bpy.context.scene` - 当前场景
- `bpy.context.window_manager` - 窗口管理器
- `bpy.context.workspace` - 当前工作区
- `bpy.context.preferences` - 用户首选项
- `bpy.context.region` - 当前区域
- `bpy.context.area` - 当前区域类型
- `bpy.context.space_data` - 当前空间数据
- `bpy.context.screen` - 当前屏幕
- `bpy.context.view_layer` - 当前视图层

**方法：**
- `bpy.context.temp_override(**kwargs)` - 临时覆盖上下文属性

**使用场景：** 获取当前编辑状态、操作活动对象和选定对象、访问UI上下文

**文档：** [bpy_context_module.md](references/bpy_context_data_types.md)

## 专业模块

### blf - 字体模块
字体渲染和文本测量。

**函数：**
- `blf.position` - 设置字体位置
- `blf.size` - 设置字体大小
- `blf.draw` - 绘制文本
- `blf.dimensions` - 获取文本尺寸
- `blf.clip` - 设置文本裁剪

**文档：** [blf_module.md](references/other_modules.md)

**使用场景：** UI中的自定义文本渲染

### bl_math - 数学模块
用于Blender的数学工具。

**函数：**
- `bl_math.lerp` - 线性插值
- `bl_math.slerp` - 球面线性插值
- `bl_math.clamp` - 限制值范围
- `bl_math.floor` - 向下取整
- `bl_math.ceil` - 向上取整
- `bl_math.round` - 四舍五入
- `bl_math.modulo` - 取模运算
- `bl_math.snap` - 吸附值

**使用场景：** 数学运算

### gpu - GPU模块
用于自定义着色器和效果的GPU编程。

**组件：**
- `gpu.types` - GPU类型定义
- `gpu.shader` - 着色器操作
- `gpu.state` - GPU状态管理
- `gpu.matrix` - 矩阵操作
- `gpu.select` - 选择操作
- `gpu.texture` - 纹理操作
- `gpu.platform` - 平台信息
- `gpu.extras` - 额外GPU工具

**使用场景：** 自定义GPU着色器和效果

### gpu - GPU编程详细参考
Blender中GPU编程的完整指南，包括类型、状态管理、矩阵操作和着色器开发。

**内容：**
- GPU初始化和检测
- GPU类型（GPUOffScreen、GPUShader、GPUTexture、GPUVertBuf、GPUIndexBuf、GPUBatch）
- GPU状态管理（混合、深度测试、面剔除、视口）
- GPU矩阵操作（投影、视图、模型矩阵）
- GPU着色器编程（自定义着色器、内置着色器）
- GPU选择功能
- GPU平台信息
- 完整示例（自定义绘制、离屏渲染、动态纹理）
- 性能优化和最佳实践

**文档：** [gpu_detailed_module.md](references/gpu_module_complete.md)

**使用场景：** 高级GPU编程、自定义渲染效果、性能关键操作

### gpu.shader - GPU着色器模块
自定义GPU着色器的创建和管理。

**函数：**
- `gpu.shader.from_code` - 从代码创建着色器
- `gpu.shader.from_builtin` - 从内置创建着色器
- `shader.uniform_float` - 设置浮点统一变量
- `shader.uniform_int` - 设置整型统一变量
- `shader.uniform_vector_float` - 设置浮点向量统一变量

**文档：** [gpu_shader_module.md](references/gpu_module_complete.md)

**使用场景：** 编写自定义GPU着色器

### gpu.state - GPU状态模块
GPU渲染状态管理，包括混合、深度测试和面剔除。

**函数：**
- `gpu.state.blend_set` - 设置混合模式
- `gpu.state.depth_test_set` - 设置深度测试
- `gpu.state.depth_mask_set` - 设置深度掩码
- `gpu.state.cull_mode_set` - 设置面剔除模式
- `gpu.state.viewport_set` - 设置视口
- `gpu.state.scissor_set` - 设置裁剪区域

**文档：** [gpu_state_module.md](references/gpu_module_complete.md)

**使用场景：** 管理GPU渲染状态

### gpu.texture - GPU纹理模块
GPU纹理的创建、操作和采样。

**函数：**
- `gpu.texture.empty_create` - 创建空纹理
- `gpu.texture.from_image` - 从图像创建纹理
- `texture.read` - 读取纹理数据
- `texture.write` - 写入纹理数据
- `texture.format` - 获取纹理格式

**文档：** [gpu_texture_module.md](references/gpu_module_complete.md)

**使用场景：** GPU纹理操作

### gpu.types - GPU类型模块
GPU类型定义，包括缓冲区、着色器和纹理对象。

**类型：**
- `gpu.types.GPUOffScreen` - 离屏渲染缓冲区
- `gpu.types.GPUShader` - GPU着色器程序
- `gpu.types.GPUTexture` - GPU纹理对象
- `gpu.types.GPUBatch` - 用于几何渲染的GPU批次
- `gpu.types.GPUIndexBuf` - GPU索引缓冲区
- `gpu.types.GPUVertBuf` - GPU顶点缓冲区
- `gpu.types.GPUVertFormat` - 顶点格式定义

**文档：** [gpu_types_module.md](references/gpu_module_complete.md)

**使用场景：** 创建和管理GPU资源

### gpu.select - GPU选择模块
基于GPU的对象选择和拾取功能。

**函数：**
- `gpu.select.select_init` - 初始化选择操作
- `gpu.select.select_pick` - 拾取光标下的单个对象
- `gpu.select.select_box` - 矩形选择
- `gpu.select.select_circle` - 圆形选择
- `gpu.select.select_lasso` - 套索选择

**文档：** [gpu_select_module.md](references/gpu_module_complete.md)

**使用场景：** 交互式对象选择工具

### gpu.platform - GPU平台模块
GPU平台和设备信息查询。

**函数：**
- `gpu.platform.info_get` - 获取GPU平台信息
- `gpu.platform.get_gpu_info` - 获取详细的GPU信息

**属性：**
- `gpu.platform.info_get()['VENDOR']` - GPU供应商
- `gpu.platform.info_get()['RENDERER']` - GPU渲染器名称
- `gpu.platform.info_get()['VERSION']` - 驱动程序版本

**文档：** [gpu_platform_module.md](references/gpu_module_complete.md)

**使用场景：** 查询GPU硬件信息

### gpu.matrix - GPU矩阵模块
GPU渲染相关的矩阵操作和变换。

**函数：**
- `gpu.matrix.translate` - 矩阵平移变换
- `gpu.matrix.scale` - 矩阵缩放变换
- `gpu.matrix.rotate` - 矩阵旋转变换
- `gpu.matrix.multiply` - 矩阵乘法
- `gpu.matrix.push` - 保存当前矩阵
- `gpu.matrix.pop` - 恢复保存的矩阵
- `gpu.matrix.reset` - 重置为单位矩阵
- `gpu.matrix.get_projection_matrix` - 获取投影矩阵
- `gpu.matrix.get_view_matrix` - 获取视图矩阵
- `gpu.matrix.get_model_view_matrix` - 获取模型视图矩阵

**文档：** [gpu_matrix_module.md](references/gpu_module_complete.md)

**使用场景：** GPU自定义绘制、矩阵变换控制

### gpu_extras - GPU扩展模块
GPU模块的扩展工具和辅助函数。

**组件：**
- `gpu_extras.batch` - 批处理工具
- `gpu_extras.drawing` - 绘制辅助函数
- `gpu_extras.presets` - 着色器预设
- `gpu_extras.utils` - GPU通用工具

**文档：** [gpu_extras_module.md](references/gpu_module_complete.md)

**使用场景：** 简化GPU绘制代码、自定义渲染工具

### gpu_extras.presets - GPU绘制预设模块
提供常用的2D绘制预设功能，包括圆形和纹理绘制。

**函数：**
- `draw_circle_2d` - 绘制2D圆形
- `draw_texture_2d` - 绘制2D纹理

**文档：** [gpu_extras_presets_module.md](references/gpu_module_complete.md)

**使用场景：** UI绘制处理器、图像叠加效果

### imbuf - 图像缓冲区模块
用于图像处理的图像缓冲区操作。

**函数：**
- `imbuf.load` - 从文件加载图像
- `imbuf.load_from_buffer` - 从缓冲区加载图像
- `imbuf.new` - 创建新图像
- `imbuf.write` - 将图像写入文件

**类型：**
- `imbuf.types.ImBuf` - 图像缓冲区类，具有复制、裁剪、释放、调整大小方法
- `imbuf.types.ImBuf.channels` - 位平面数量
- `imbuf.types.ImBuf.filepath` - 与图像关联的文件路径
- `imbuf.types.ImBuf.planes` - 与图像关联的位数
- `imbuf.types.ImBuf.ppm` - 每米像素数
- `imbuf.types.ImBuf.size` - 图像尺寸（像素）

**文档：** [imbuf_module.md](references/other_modules.md)

**使用场景：** 图像处理和操作

### mathutils - 数学工具模块
高级数学工具。

**组件：**
- `mathutils.Vector` - 向量操作
- `mathutils.Matrix` - 矩阵操作
- `mathutils.Quaternion` - 四元数操作
- `mathutils.Euler` - 欧拉旋转操作
- `mathutils.Color` - 颜色操作
- `mathutils.geometry` - 几何工具
- `mathutils.noise` - 噪声函数

**使用场景：** 高级数学运算

### mathutils.geometry - 几何工具模块
几何计算工具函数集合。

**函数：**
- `intersect_line_line` - 直线相交测试
- `intersect_line_sphere` - 直线与球体相交
- `intersect_line_tri_ ATP` - 直线与三角形相交
- `intersect_ray_tri_ ATP` - 射线与三角形相交
- `intersect_tri_tri` - 三角形与三角形相交
- `intersect_sphere_sphere` - 球体与球体相交
- `distribute_points_to_n_directions` - 多方向点分布
- `get_blended_positions` - 混合位置计算
- `find_nearest_face` - 最近面查找
- `find_nearest_vertex` - 最近顶点查找

**文档：** [mathutils_geometry_module.md](references/mathutils_module.md)

**使用场景：** 碰撞检测、几何相交计算、位置插值

### mathutils.noise - 噪声模块
程序化噪声生成函数。

**函数：**
- `noise.noise` - Perlin噪声
- `noise.voronoi` - Voronoi噪声
- `noise.cellnoise` - 细胞噪声
- `noise.blender_noise` - Blender噪声
- `noise.willow` - 柳树噪声
- `noise.hetero` - 异构噪声
- `noise.multi_fractal` - 多重分形噪声
- `noise.fbm` - 分形布朗运动
- `noise.ridged_multi_fractal` - 岭多重分形噪声
- `noise.hybrid_multi_fractal` - 混合多重分形噪声

**文档：** [mathutils_noise_module.md](references/mathutils_module.md)

**使用场景：** 程序化纹理、地形生成、随机模式

### mathutils.bvhtree - BVH树模块
包围体层次结构树，用于加速空间查询。

**类：**
- `BVHTree` - BVH树数据结构
  - `from_object` - 从对象创建BVH树
  - `from_polygons` - 从多边形创建BVH树
  - `ray_cast` - 射线投射查询
  - `closest_point` - 最近点查询
  - `intersect` - 相交测试
  - `overlap` - 重叠测试

**文档：** [mathutils_bvhtree_module.md](references/mathutils_module.md)

**使用场景：** 碰撞检测、射线查询、空间优化

### mathutils.interpolate - 插值模块
各种插值算法实现。

**函数：**
- `interpolate.bezier` - 贝塞尔曲线插值
- `interpolate.bspline` - B样条插值
- `interpolate.catmull_rom` - Catmull-Rom样条
- `interpolate.cyclic_cubic` - 周期三次插值
- `interpolate.cyclic_catmull_rom` - 周期Catmull-Rom样条
- `interpolate.kovacs` - Kovacs插值
- `interpolate.smooth_ends` - 平滑端点
- `interpolate.least_squares` - 最小二乘法

**文档：** [mathutils_interpolate_module.md](references/mathutils_module.md)

**使用场景：** 曲线插值、动画路径、数据拟合

### mathutils.kdtree - KD树模块
K维树数据结构，用于快速近邻搜索。

**类：**
- `KDTree` - KD树数据结构
  - `from_object` - 从对象创建KD树
  - `from_points` - 从点集创建KD树
  - `find_nearest` - 最近邻查找
  - `find_nearest_range` - 范围最近邻查找
  - `to_list` - 转换为列表

**文档：** [mathutils_kdtree_module.md](references/mathutils_module.md)

**使用场景：** 近邻搜索、空间索引、点云处理

### aud - 音频模块
使用Audaspace进行音频处理和播放。

**类：**
- `aud.Device` - 音频播放设备，支持3D音频
- `aud.Sound` - 用于加载和管理音频文件的音频数据
- `aud.Handle` - 单个声音实例的播放控制句柄
- `aud.DynamicMusic` - 用于场景转换的动态音乐管理
- `aud.HRTF` - 用于双耳音频的头相关传输函数

**功能：**
- 带距离衰减的3D空间音频
- 带交叉淡入淡出的动态音乐
- HRTF双耳音频支持
- 支持多种音频格式（OGG、WAV、MP3、FLAC等）

**文档：** [aud_module.md](references/other_modules.md)

**使用场景：** 音频处理和播放

### freestyle - Freestyle模块
用于非真实感渲染（NPR）的Freestyle渲染。

**子模块：**
- `freestyle.types` - 视图映射组件数据类型（0D和1D元素）
- `freestyle.predicates` - 线条选择谓词
- `freestyle.functions` - 线条处理函数
- `freestyle.chainingiterators` - 线条链接迭代器
- `freestyle.shaders` - 线条样式着色器
- `freestyle.utils` - 样式模块编写工具

**关键概念：**
- 谓词 - 基于几何和材质属性选择线条
- 函数 - 计算用于样式的线条属性
- 链接迭代器 - 将相邻边连接成连续的线条
- 着色器 - 定义线条外观（颜色、厚度、样式）

**文档：** [freestyle_module.md](references/freestyle_module.md)

**使用场景：** 创建风格化线条渲染效果

### freestyle.types - Freestyle类型模块
Freestyle视图映射组件的数据类型定义。

**内容：**
- 0D元素（ViewVertex, ViewEdge, NonTesselatedStroke）
- 1D元素（SVertex, Stroke, StrokeShaderInput）
- 几何属性（x, y, z坐标）
- 材质属性（color, thickness）
- 曲线属性（curvature, angular_deflection）

**文档：** [freestyle_types_module.md](references/freestyle_module.md)

**使用场景：** Freestyle样式开发、线条属性访问

### freestyle.predicates - Freestyle谓词模块
用于选择线条的几何和材质谓词。

**内容：**
- 距离谓词（WithinDistance, SameShapeNamePredicate）
- 角度谓词（HigherLength, LengthGreaterThan）
- 曲率谓词（Curvature2DAngleGreaterThan）
- 材质谓词（SameMaterial, DifferentMaterial）
- 轮廓谓词（ExternalContour, InsideContour）
- Z深度谓词（ZValueSmallerThan, ZValueGreaterThan）

**文档：** [freestyle_predicates_module.md](references/freestyle_module.md)

**使用场景：** 线条筛选条件定义

### freestyle.functions - Freestyle函数模块
用于计算线条属性的函数。

**内容：**
- 曲率函数（Curvature2DAngle, CurveMaterial）
- 几何函数（GetNormal, GetAttributedCurveT）
- 长度函数（CurveLength, TotalLength）
- 方向函数（CurveViewMapDirection, Normal2DAngle）
- 距离函数（DistanceToCamera, DistanceToScreenEdge）

**文档：** [freestyle_functions_module.md](references/freestyle_module.md)

**使用场景：** 线条属性计算、动态样式

### freestyle.chainingiterators - Freestyle链接迭代器模块
用于将边链接成连续线条的迭代器。

**内容：**
- 链接策略（ChainPredicateIterator, ChainUVGradientIterator）
- 步进控制（Step, NextSteppableFunction）
- 链接条件（AngleThreshold, LengthThreshold）
- 闭合检测（ClosedCurve, OpenCurve）

**文档：** [freestyle_chainingiterators_module.md](references/freestyle_module.md)

**使用场景：** 线条生成和链接控制

### freestyle.shaders - Freestyle着色器模块
用于定义线条外观的着色器。

**内容：**
- 颜色着色器（ConstantColorShader, StrokeColorUserDefinedShader）
- 厚度着色器（ConstantThicknessShader, VaryingThicknessShader）
- 混合着色器（AlphaShader, ColorShader）
- 噪声着色器（NoiseDisplacementShader, ColorNoiseShader）
- 渐变着色器（LinearColorShader, CircularGradientShader）

**文档：** [freestyle_shaders_module.md](references/freestyle_module.md)

**使用场景：** 线条样式设计、渲染效果定制

### freestyle.utils - Freestyle工具模块
用于编写和调试样式模块的工具。

**内容：**
- 样式模块加载（loadStyleModule, writeStyleModule）
- 调试工具（printStream, printViewMap）
- 几何工具（make Steiner chain）
- 谓词工厂（Predicates）
- 迭代器工厂（Iterators）

**文档：** [freestyle_utils_module.md](references/freestyle_module.md)

**使用场景：** 样式模块开发、调试和测试

### bpy_extras - 额外模块
额外的Blender工具。

**组件：**
- `bpy_extras.node_utils` - 节点工具
- `bpy_extras.object_utils` - 对象工具
- `bpy_extras.mesh_utils` - 网格工具
- `bpy_extras.image_utils` - 图像工具
- `bpy_extras.asset_utils` - 资源工具
- `bpy_extras.anim_utils` - 动画工具
- `bpy_extras.view3d_utils` - 3D视图工具

**文档：** [bpy_extras_module.md](references/bpy_extras_module.md)

**使用场景：** 额外的工具函数

### bpy_extras.id_map_utils - ID映射工具模块
ID标识符映射和转换工具。

**功能：**
- ID标识符生成
- ID映射管理
- ID转换函数
- 批量ID处理
- 冲突检测和解决

**文档：** [bpy_extras_id_map_utils_module.md](references/bpy_extras_module.md)

**使用场景：** ID管理、数据迁移、标识符转换

### bpy_extras.io_utils - IO工具模块
导入导出通用工具函数。

**功能：**
- 导入导出路径处理
- 序列帧支持
- 路径格式转换
- 文件浏览器集成
- 导入导出事件回调

**文档：** [bpy_extras_io_utils_module.md](references/bpy_extras_module.md)

**使用场景：** 自定义导入导出、文件处理、批处理操作

### bpy_extras.keyconfig_utils - 快捷键配置工具模块
快捷键配置管理工具。

**功能：**
- 快捷键导出
- 快捷键导入
- 快捷键冲突检测
- 快捷键重置
- 快捷键预设管理

**文档：** [bpy_extras_keyconfig_utils_module.md](references/bpy_extras_module.md)

**使用场景：** 快捷键管理、配置备份、快捷键开发

### bpy.ops.gizmo - 小工具操作模块
3D视口小工具绘制和操作。

**内容：**
- 小工具创建
- 小工具绘制
- 小工具事件处理
- 小工具属性设置
- 小工具组关联

**文档：** [bpy_ops_gizmo_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 自定义Gizmo开发、交互式工具

### bpy.ops.addon - 插件操作模块
Blender插件管理操作。

**内容：**
- 插件启用/禁用
- 插件安装/卸载
- 插件偏好设置
- 插件信息查看
- 插件路径管理

**文档：** [bpy_ops_addon_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 插件管理、自动化安装

### bpy.ops.header - 标题栏操作模块
编辑器标题栏操作。

**内容：**
- 标题栏菜单
- 标题栏按钮
- 标题栏选择
- 标题栏搜索
- 标题栏布局

**文档：** [bpy_ops_header_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 标题栏定制、界面开发

### bpy.ops.area - 区域操作模块
编辑器区域管理操作。

**内容：**
- 区域分割/合并
- 区域大小调整
- 区域类型切换
- 区域边界控制
- 区域属性设置

**文档：** [bpy_ops_area_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 屏幕布局管理、界面定制

### bpy.ops.empty - 空对象操作模块
空对象创建和编辑操作。

**内容：**
- 空对象创建
- 空对象类型设置
- 空对象父级设置
- 空对象显示控制
- 空对象变换

**文档：** [bpy_ops_empty_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 锚点创建、控制器、空对象代理

### bpy.ops.clipboard - 剪贴板操作模块
编辑器剪贴板操作。

**内容：**
- 复制到剪贴板
- 从剪贴板粘贴
- 剪贴板格式选择
- 剪贴板历史
- 剪贴板清除

**文档：** [bpy_ops_clipboard_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 数据复制粘贴、跨编辑器操作

### bpy.ops.dopesheet - 动作编辑器操作模块
NLA/动作编辑器操作。

**内容：**
- 关键帧编辑
- 通道管理
- 动作裁剪
- 动作合成
- 曲线编辑
- 关键帧标记

**文档：** [bpy_ops_dopesheet_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 动画编辑、关键帧管理

### bpy.ops.compositor - 合成器操作模块
节点合成器编辑操作。

**内容：**
- 节点添加/删除
- 节点连接
- 节点属性编辑
- 合成预览控制
- 渲染层管理
- 跟踪/稳定化

**文档：** [bpy_ops_compositor_module.md](references/bpy_ops_render_material.md)

**使用场景：** 合成编辑、特效制作

### bpy.ops.eevee - Eevee渲染操作模块
Eevee实时渲染器操作。

**内容：**
- 渲染设置
- 光照设置
- 阴影控制
- 后处理效果
- 降噪设置
- 缓存控制

**文档：** [bpy_ops_eevee_module.md](references/bpy_ops_render_material.md)

**使用场景：** Eevee渲染配置、实时预览

### bpy.ops.workbench - Workbench渲染操作模块
Workbench渲染器操作。

**内容：**
- 渲染设置
- 光照设置
- 环境控制
- 材质覆盖
- 绘制类型
- 视口效果

**文档：** [bpy_ops_workbench_module.md](references/bpy_ops_render_material.md)

**使用场景：** Workbench渲染、Viewport预览

### bpy.ops.light_probe - 光照探针操作模块
光照探针创建和编辑。

**内容：**
- 探针创建（球体/立方体/平面）
- 探针位置调整
- 探针范围设置
- 探针更新控制
- 探针数据管理

**文档：** [bpy_ops_light_probe_module.md](references/bpy_ops_render_material.md)

**使用场景：** 光照烘焙、反射设置

### bpy.ops.toolshelf - 工具面板操作模块
工具面板（Tool Shelf）操作。

**内容：**
- 工具面板切换
- 工具选择
- 面板展开/折叠
- 面板重置
- 工具搜索

**文档：** [bpy_ops_toolshelf_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 工具面板管理、界面定制

### bpy.ops.speaker - 扬声器操作模块
扬声器对象创建和编辑。

**内容：**
- 扬声器创建
- 音频设置
- 距离衰减
- 声锥设置
- 播放控制
- 同步设置

**文档：** [bpy_ops_speaker_module.md](references/bpy_ops_physics_sim.md)

**使用场景：** 3D音频、场景音效

### bpy.ops.video_seq - 视频序列操作模块
视频序列编辑器操作（旧版）。

**内容：**
- 片段管理
- 转场效果
- 特效应用
- 音频同步
- 时间线编辑
- 预览控制

**文档：** [bpy_ops_video_seq_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 视频编辑、序列合成

### bpy.ops.viewport - 视口操作模块
3D视口显示控制。

**内容：**
- 视口切换
- 显示模式
- 渲染边界
- 视图分割
- 最大化切换
- 局部视图

**文档：** [bpy_ops_viewport_module.md](references/bpy_ops_render_material.md)

**使用场景：** 视口管理、视图控制

### bpy.ops.ruler - 标尺工具操作模块
标尺和角度测量工具。

**内容：**
- 标尺创建
- 标尺测量
- 角度测量
- 标尺清除
- 测量单位
- 标签显示

**文档：** [bpy_ops_view3d_ruler_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 距离测量、尺寸标注

### bpy.ops.shader_node - 着色器节点操作模块
材质节点编辑器操作。

**内容：**
- 节点添加
- 节点连接
- 节点属性
- 节点组
- 节点粘贴
- 节点搜索

**文档：** [bpy_ops_shader_node_module.md](references/bpy_ops_render_material.md)

**使用场景：** 材质编辑、节点组合

### bpy.ops.properties - 属性操作模块
属性编辑器操作。

**内容：**
- 属性面板
- 属性编辑
- 搜索属性
- 属性复制
- 上下文切换
- 属性重置

**文档：** [bpy_ops_properties_module.md](references/bpy_ops_render_material.md)

**使用场景：** 属性编辑、面板开发

### bpy.ops.strip_modifier - 序列条修改器操作模块
视频序列条修改器操作。

**内容：**
- 修改器添加
- 修改器参数
- 修改器顺序
- 修改器复制
- 修改器清除
- 修改器类型

**文档：** [bpy_ops_sequencer_strip_modifier_module.md](references/bpy_ops_sequencer_clip.md)

**使用场景：** 序列特效、颜色校正

### bpy.ops.keying_set - 关键帧设置操作模块
关键帧设置管理。

**内容：**
- 关键帧集创建
- 关键帧集应用
- 关键帧集编辑
- 关键帧集删除
- 关键帧集导出
- 路径设置

**文档：** [bpy_ops_keying_set_module.md](references/bpy_ops_animation_rigging.md)

**使用场景：** 批量关键帧、动画预设

### bpy.ops.group - 组操作模块
对象组管理操作。

**内容：**
- 组创建
- 组删除
- 组添加/移除
- 组选择
- 组操作
- 组实例化

**文档：** [bpy_ops_group_module.md](references/bpy_ops_object_transform.md)

**使用场景：** 对象分组、批量操作

### bpy.ops.paint_curve - 绘制曲线操作模块
绘制曲线工具操作。

**内容：**
- 曲线选择
- 曲线应用
- 曲线删除
- 曲线重命名
- 曲线复制
- 曲线转换

**文档：** [bpy_ops_paint_curve_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 笔刷轨迹、绘制动画

### bpy.ops.paint_texture - 纹理绘制操作模块
纹理绘制操作。

**内容：**
- 笔刷选择
- 纹理选择
- 绘制操作
- 蒙版控制
- 渐变工具
- 平滑工具

**文档：** [bpy_ops_paint_texture_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 纹理绘制、材质绘制

### bpy.ops.paint_vertex - 顶点绘制操作模块
顶点颜色绘制操作。

**内容：**
- 顶点选择
- 颜色选择
- 绘制操作
- 混合模式
- 笔刷设置
- 平滑操作

**文档：** [bpy_ops_paint_vertex_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 顶点着色、颜色编辑

### bpy.ops.paint_weight - 权重绘制操作模块
顶点权重绘制操作。

**内容：**
- 权重选择
- 权重绘制
- 权重平滑
- 权重转移
- 笔刷设置
- 蒙版控制

**文档：** [bpy_ops_paint_weight_module.md](references/bpy_ops_paint_sculpt.md)

**使用场景：** 权重编辑、骨骼绑定

### bpy.ops.screen_region_toggle - 区域切换操作模块
屏幕区域显示切换。

**内容：**
- 区域显示/隐藏
- 区域类型切换
- 全屏切换
- 最大化切换
- 分割控制
- 区域重置

**文档：** [bpy_ops_screen_region_toggle_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 界面布局、视口控制

### bpy.ops.wm_path_open - 路径打开操作模块
系统路径打开操作。

**内容：**
- 文件路径打开
- 目录打开
- URL打开
- 关联程序
- 路径验证
- 错误处理

**文档：** [bpy_ops_wm_path_open_module.md](references/bpy_ops_ui_workspace.md)

**使用场景：** 资源管理、文件操作

### bpy.ops.import_export - 导入导出操作模块
导入导出通用操作。

**内容：**
- 格式选择
- 选项设置
- 文件浏览
- 批量导入
- 预设管理
- 导入导出

**文档：** [bpy_ops_import_export_module.md](references/bpy_ops_import_export.md)

**使用场景：** 资源导入导出

### bpy.ops.import_hair - 毛发导入操作模块
毛发系统导入操作。

**内容：**
- 毛发导入
- 选项设置
- 图层映射
- 格式转换
- 数据验证
- 预览控制

**文档：** [bpy_ops_import_hair_module.md](references/bpy_ops_import_export.md)

**使用场景：** 毛发数据导入

### bpy.ops.import_images - 图像序列导入操作模块
图像序列批量导入。

**内容：**
- 图像选择
- 序列设置
- 时间范围
- 帧间隔
- 命名规则
- 堆叠方式

**文档：** [bpy_ops_import_images_module.md](references/bpy_ops_import_export.md)

**使用场景：** 图像序列、纹理序列

### bpy.ops.export_abc - Alembic导出操作模块
Alembic格式导出。

**内容：**
- 对象选择
- 导出选项
- 动画设置
- 压缩选项
- 分层导出
- 包含控制

**文档：** [bpy_ops_export_abc_module.md](references/bpy_ops_import_export.md)

**使用场景：** 复杂场景导出、动画缓存

### bpy.ops.export_dae - Collada导出操作模块
Collada格式导出。

**内容：**
- 对象导出
- 选项设置
- 材质导出
- 动画导出
- 骨骼导出
- 坐标转换

**文档：** [bpy_ops_export_dae_module.md](references/bpy_ops_import_export.md)

**使用场景：** 跨平台导出、CAD交换

### bpy.ops.export_fbx - FBX导出操作模块
FBX格式导出。

**内容：**
- 对象选择
- 导出选项
- 动画设置
- 材质设置
- 骨骼导出
- 批处理导出

**文档：** [bpy_ops_export_fbx_module.md](references/bpy_ops_import_export.md)

**使用场景：** 游戏引擎导出、跨软件协作

### bpy.ops.export_gltf - glTF导出操作模块
glTF/glb格式导出。

**内容：**
- 对象选择
- 导出选项
- 动画设置
- 材质导出
- 纹理处理
- 压缩选项

**文档：** [bpy_ops_export_gltf_module.md](references/bpy_ops_import_export.md)

**使用场景：** Web/AR/VR导出、实时渲染

### bpy.ops.export_mesh - 网格导出操作模块
通用网格格式导出。

**内容：**
- 网格选择
- 导出选项
- 法线设置
- UV设置
- 顶点颜色
- 格式选择

**文档：** [bpy_ops_export_mesh_module.md](references/bpy_ops_import_export.md)

**使用场景：** 网格导出、数据交换

### bpy.ops.export_obj - OBJ导出操作模块
OBJ格式导出。

**内容：**
- 对象选择
- 导出选项
- 材质导出
- 纹理处理
- 坐标设置
- 批处理导出

**文档：** [bpy_ops_export_obj_module.md](references/bpy_ops_import_export.md)

**使用场景：** 通用网格导出、3D打印

### bpy.ops.export_ply - PLY导出操作模块
PLY格式导出。

**内容：**
- 网格选择
- 导出选项
- 法线设置
- 颜色导出
- 格式选择
- 精度设置

**文档：** [bpy_ops_export_ply_module.md](references/bpy_ops_import_export.md)

**使用场景：** 3D扫描、数据交换

### bpy.ops.export_stl - STL导出操作模块
STL格式导出。

**内容：**
- 网格选择
- 导出选项
- 二进制/ASCII
- 精度设置
- 批处理导出
- 错误检查

**文档：** [bpy_ops_export_stl_module.md](references/bpy_ops_import_export.md)

**使用场景：** 3D打印、CAM加工

### bpy.ops.export_usd - USD导出操作模块
USD格式导出。

**内容：**
- 对象选择
- 导出选项
- 动画设置
- 材质导出
- 变体设置
- 分层导出

**文档：** [bpy_ops_export_usd_module.md](references/bpy_ops_import_export.md)

**使用场景：** 影视流程、跨软件协作

### bpy.ops.export_x3d - X3D导出操作模块
X3D格式导出。

**内容：**
- 对象选择
- 导出选项
- 动画导出
- 材质设置
- 交互设置
- 压缩选项

**文档：** [bpy_ops_export_x3d_module.md](references/bpy_ops_import_export.md)

**使用场景：** Web3D、交互式3D

### bpy.ops.import_abc - Alembic导入操作模块
Alembic格式导入。

**内容：**
- 文件选择
- 导入选项
- 动画设置
- 图层映射
- 缓存读取
- 时间控制

**文档：** [bpy_ops_import_abc_module.md](references/bpy_ops_import_export.md)

**使用场景：** 复杂场景导入、动画缓存

### bpy.ops.import_dae - Collada导入操作模块
Collada格式导入。

**内容：**
- 文件选择
- 导入选项
- 材质处理
- 骨骼导入
- 动画设置
- 坐标转换

**文档：** [bpy_ops_import_dae_module.md](references/bpy_ops_import_export.md)

**使用场景：** CAD导入、游戏资产

### bpy.ops.import_fbx - FBX导入操作模块
FBX格式导入。

**内容：**
- 文件选择
- 导入选项
- 动画设置
- 骨骼处理
- 材质导入
- 批处理导入

**文档：** [bpy_ops_import_fbx_module.md](references/bpy_ops_import_export.md)

**使用场景：** 游戏资产、动画导入

### bpy.ops.import_gltf - glTF导入操作模块
glTF/glb格式导入。

**内容：**
- 文件选择
- 导入选项
- 动画设置
- 材质处理
- 纹理导入
- 变体支持

**文档：** [bpy_ops_import_gltf_module.md](references/bpy_ops_import_export.md)

**使用场景：** Web3D、实时渲染资产

### bpy.ops.import_ply - PLY导入操作模块
PLY格式导入。

**内容：**
- 文件选择
- 导入选项
- 法线处理
- 颜色导入
- 顶点顺序
- 验证选项

**文档：** [bpy_ops_import_ply_module.md](references/bpy_ops_import_export.md)

**使用场景：** 3D扫描数据、点云导入

### bpy.ops.import_stl - STL导入操作模块
STL格式导入。

**内容：**
- 文件选择
- 导入选项
- 法线计算
- 单位设置
- 批处理导入
- 错误处理

**文档：** [bpy_ops_import_stl_module.md](references/bpy_ops_import_export.md)

**使用场景：** 3D打印模型、CAD数据

### bpy.ops.import_usd - USD导入操作模块
USD格式导入。

**内容：**
- 文件选择
- 导入选项
- 图层读取
- 变体选择
- 时间设置
- 代理设置

**文档：** [bpy_ops_import_usd_module.md](references/bpy_ops_import_export.md)

**使用场景：** 影视资产、USD流程

### bpy.ops.import_x3d - X3D导入操作模块
X3D格式导入。

**内容：**
- 文件选择
- 导入选项
- 动画导入
- 交互设置
- 材质处理
- 脚本支持

**文档：** [bpy_ops_import_x3d_module.md](references/bpy_ops_import_export.md)

**使用场景：** Web3D、交互式内容

## 开发工作流

### 创建插件

**插件基本结构：**
```python
bl_info = {
    "name": "My Add-on",
    "author": "Your Name",
    "version": (1, 0, 0),
    "blender": (5, 0, 0),
    "location": "View3D > Add > Mesh",
    "description": "My custom add-on",
    "category": "Mesh",
}

import bpy

def register():
    bpy.utils.register_class(MyOperator)
    bpy.types.VIEW3D_MT_mesh_add.append(menu_func)

def unregister():
    bpy.types.VIEW3D_MT_mesh_add.remove(menu_func)
    bpy.utils.unregister_class(MyOperator)

if __name__ == "__main__":
    register()
```

### 创建操作符

**操作符模板：**
```python
import bpy

class OBJECT_OT_my_operator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "My Operator"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        # 在这里编写代码
        return {'FINISHED'}

    def invoke(self, context, event):
        return self.execute(context)

def register():
    bpy.utils.register_class(OBJECT_OT_my_operator)

def unregister():
    bpy.utils.unregister_class(OBJECT_OT_my_operator)
```

### 创建面板

**面板模板：**
```python
import bpy

class VIEW3D_PT_my_panel(bpy.types.Panel):
    bl_label = "My Panel"
    bl_idname = "VIEW3D_PT_my_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = "My Category"

    def draw(self, context):
        layout = self.layout
        layout.label(text="Hello World")

def register():
    bpy.utils.register_class(VIEW3D_PT_my_panel)

def unregister():
    bpy.utils.unregister_class(VIEW3D_PT_my_panel)
```

### 创建属性

**属性模板：**
```python
import bpy

class MyProperties(bpy.types.PropertyGroup):
    my_bool: bpy.props.BoolProperty(
        name="My Bool",
        description="My boolean property",
        default=False
    )
    my_int: bpy.props.IntProperty(
        name="My Int",
        description="My integer property",
        default=0,
        min=0,
        max=100
    )
    my_float: bpy.props.FloatProperty(
        name="My Float",
        description="My float property",
        default=0.0,
        min=0.0,
        max=1.0
    )
    my_string: bpy.props.StringProperty(
        name="My String",
        description="My string property",
        default=""
    )
    my_enum: bpy.props.EnumProperty(
        name="My Enum",
        description="My enum property",
        items=[
            ('OPTION_A', "Option A", ""),
            ('OPTION_B', "Option B", ""),
        ],
        default='OPTION_A'
    )

def register():
    bpy.utils.register_class(MyProperties)
    bpy.types.Scene.my_props = bpy.props.PointerProperty(type=MyProperties)

def unregister():
    del bpy.types.Scene.my_props
    bpy.utils.unregister_class(MyProperties)
```

### 网格操作

**网格操作：**
```python
import bpy
import bmesh

# Create new mesh
mesh = bpy.data.meshes.new("MyMesh")
obj = bpy.data.objects.new("MyObject", mesh)
bpy.context.collection.objects.link(obj)

# Access mesh data
bm = bmesh.new()
bm.from_mesh(mesh)
bm.verts.ensure_lookup_table()

# Modify mesh
for vert in bm.verts:
    vert.co.z += 1.0

# Update mesh
bm.to_mesh(mesh)
mesh.update()
bm.free()
```

### 材质操作

**材质操作：**
```python
import bpy

# Create new material
mat = bpy.data.materials.new("MyMaterial")
mat.use_nodes = True
nodes = mat.node_tree.nodes
links = mat.node_tree.links

# Get nodes
output = nodes.get("Material Output")
bsdf = nodes.get("BSDF")

# Create nodes
tex = nodes.new(type='ShaderNodeTexImage', label="Texture")
tex.location = (-300, 200)

# Link nodes
links.new(tex.outputs['Color'], bsdf.inputs['Base Color'])
links.new(bsdf.outputs['BSDF'], output.inputs['Surface'])

# Load image
tex.image = bpy.data.images.load("path/to/image.png")
```

### 动画操作

**动画操作：**
```python
import bpy

# Create action
action = bpy.data.actions.new("MyAction")
obj = bpy.context.active_object
obj.animation_data_create()
obj.animation_data.action = action

# Add keyframe
obj.location = (0, 0, 0)
obj.keyframe_insert(data_path="location", frame=1)

obj.location = (1, 1, 1)
obj.keyframe_insert(data_path="location", frame=10)

# Set animation range
bpy.context.scene.frame_start = 1
bpy.context.scene.frame_end = 10
```

### 节点操作

**节点操作：**
```python
import bpy

# Get material
mat = bpy.context.active_object.active_material
nodes = mat.node_tree.nodes
links = mat.node_tree.links

# Create nodes
output = nodes.new(type='ShaderNodeOutputMaterial')
bsdf = nodes.new(type='ShaderNodeBsdfPrincipled')
mix = nodes.new(type='ShaderNodeMixRgb')

# Set positions
output.location = (400, 0)
bsdf.location = (200, 0)
mix.location = (0, 0)

# Link nodes
links.new(bsdf.outputs['BSDF'], output.inputs['Surface'])
links.new(mix.outputs['Result'], bsdf.inputs['Base Color'])
```

### GPU操作

**GPU操作：**
```python
import gpu
from gpu_extras.batch import batch_for_shader

# Create shader
shader = gpu.shader.from_builtin('2D_UNIFORM_COLOR')
batch = batch_for_shader(shader, 'TRIS', {
    'pos': ('v2f', 'IN_POSITION'),
    'color': ('v4f', 'IN_COLOR'),
})

# Draw
shader.bind()
batch.draw(shader)
```

## 参考文档

有关详细的API文档，请参阅参考文件：

### bpy.ops 操作符
- **[bpy_ops_object_transform.md](references/bpy_ops_object_transform.md)** - 对象/变换/场景/视图
- **[bpy_ops_mesh_curve.md](references/bpy_ops_mesh_curve.md)** - 网格/曲线/文本/UV/涂鸦
- **[bpy_ops_animation_rigging.md](references/bpy_ops_animation_rigging.md)** - 动画/骨骼/姿态/约束
- **[bpy_ops_import_export.md](references/bpy_ops_import_export.md)** - 所有导入导出格式
- **[bpy_ops_render_material.md](references/bpy_ops_render_material.md)** - 渲染/材质/节点/着色器
- **[bpy_ops_paint_sculpt.md](references/bpy_ops_paint_sculpt.md)** - 绘画/雕刻/权重
- **[bpy_ops_ui_workspace.md](references/bpy_ops_ui_workspace.md)** - UI/窗口/工作区/首选项
- **[bpy_ops_physics_sim.md](references/bpy_ops_physics_sim.md)** - 物理/粒子/流体/布料
- **[bpy_ops_sequencer_clip.md](references/bpy_ops_sequencer_clip.md)** - 序列/剪辑/修改器/资源

### bpy 核心模块
- **[bpy_context_data_types.md](references/bpy_context_data_types.md)** - context/data/types 基础
- **[bpy_props_path.md](references/bpy_props_path.md)** - 属性系统/路径/文件数据
- **[bpy_geometry_nodes.md](references/bpy_geometry_nodes.md)** - 几何节点完整参考
- **[bpy_rendering.md](references/bpy_rendering.md)** - 渲染/合成/材质/UV
- **[bpy_scripting.md](references/bpy_scripting.md)** - 脚本/动画/模板/驱动
- **[bpy_physics_materials.md](references/bpy_physics_materials.md)** - 物理/修改器/雕刻/涂鸦
- **[bpy_ui_preferences.md](references/bpy_ui_preferences.md)** - UI设计/偏好设置/Freestyle
- **[bpy_advanced.md](references/bpy_advanced.md)** - 高级功能/回调/安全/API变更

### 其他模块
- **[gpu_module_complete.md](references/gpu_module_complete.md)** - GPU编程完整参考
- **[bpy_extras_module.md](references/bpy_extras_module.md)** - 扩展工具模块
- **[freestyle_module.md](references/freestyle_module.md)** - Freestyle非真实感渲染
- **[mathutils_module.md](references/mathutils_module.md)** - 数学工具完整参考
- **[bmesh_module.md](references/bmesh_module.md)** - BMesh模块完整参考
- **[other_modules.md](references/other_modules.md)** - 其他辅助模块

## 最佳实践

1. **始终检查上下文** - 适当使用 `bpy.context`
2. **处理错误** - 对操作使用try-except块
3. **清理资源** - 释放BMesh和其他资源
4. **使用操作符** - 优先使用 `bpy.ops` 进行标准操作
5. **注册/注销** - 始终实现这两个函数
6. **版本兼容性** - 检查Blender版本兼容性
7. **性能** - 对大型网格操作使用BMesh
8. **撤销/重做** - 适当设置 `bl_options`
9. **文档** - 为类和函数添加文档字符串
10. **测试** - 在不同Blender版本中测试插件

## 常见模式

### 模态操作符
```python
class OBJECT_OT_modal_operator(bpy.types.Operator):
    bl_idname = "object.modal_operator"
    bl_label = "Modal Operator"

    def __init__(self):
        self._timer = None

    def modal(self, context, event):
        if event.type == 'TIMER':
            # Your code here
            pass
        elif event.type in {'RIGHTMOUSE', 'ESC'}:
            return {'CANCELLED'}
        return {'RUNNING_MODAL'}

    def execute(self, context):
        wm = context.window_manager
        self._timer = wm.event_timer_add(0.1, window=context.window)
        wm.modal_handler_add(self)
        return {'RUNNING_MODAL'}

    def cancel(self, context):
        wm = context.window_manager
        wm.event_timer_remove(self._timer)
        wm.modal_handler_remove(self)
```

### 属性组
```python
class MyPropertiesGroup(bpy.types.PropertyGroup):
    my_prop: bpy.props.StringProperty()

def register():
    bpy.utils.register_class(MyPropertiesGroup)
    bpy.types.Scene.my_group = bpy.props.PointerProperty(type=MyPropertiesGroup)

def unregister():
    del bpy.types.Scene.my_group
    bpy.utils.unregister_class(MyPropertiesGroup)
```

### 绘制回调
```python
import gpu
from gpu_extras.batch import batch_for_shader

def draw_callback():
    # Your drawing code
    pass

def register():
    bpy.types.SpaceView3D.draw_handler_add(draw_callback, 'WINDOW', 'POST_VIEW')

def unregister():
    bpy.types.SpaceView3D.draw_handler_remove(draw_callback)
```

## 故障排除

### 常见问题

1. **上下文错误** - 确保操作有正确的上下文
2. **内存泄漏** - 释放BMesh和其他资源
3. **注册错误** - 检查类名和ID
4. **导入错误** - 验证模块导入
5. **性能问题** - 对大型操作使用BMesh

### 调试
```python
import bpy

# Enable debug output
bpy.app.debug = True

# Print debug information
print(f"Blender version: {bpy.app.version}")
print(f"Active object: {bpy.context.active_object}")

# Check if operator is available
if hasattr(bpy.ops.mesh, 'primitive_cube_add'):
    bpy.ops.mesh.primitive_cube_add()
```

## 附加模块参考

### bpy_mathutils - 数学工具模块
mathutils模块提供向量、矩阵、四元数、欧拉角和颜色等数学类型。

**内容：**
- Vector向量类（2D/3D/4D）
- Matrix矩阵类（变换矩阵、旋转矩阵）
- Quaternion四元数类
- Euler欧拉角类
- Color颜色类
- geometry几何工具函数
- noise噪声生成函数
- bvhtree和kdtree空间数据结构

**文档：** [bpy_scripting.md](references/bpy_scripting.md)

**使用场景：** 3D数学计算、变换运算、空间查询

### bpy_freestyle - 非真实感渲染模块
Freestyle是Blender的非真实感渲染引擎，用于创建风格化线条效果。

**内容：**
- 视图映射组件（0D和1D元素）
- 线条选择谓词
- 线条处理函数
- 链式迭代器
- 笔触着色器
- 样式模块编写工具

**文档：** [other_modules.md](references/other_modules.md)

**使用场景：** 卡通轮廓、钢笔画、技术插图

### bpy_gotchas - 常见陷阱模块
Blender Python开发中常见问题和解决方案的汇总。

**内容：**
- 上下文相关操作
- 资源释放和内存管理
- 操作符执行失败
- 循环引用问题
- 线程安全注意事项
- 版本兼容性处理
- 调试技巧和工具

**文档：** [bpy_advanced.md](references/bpy_advanced.md)

**使用场景：** 问题排查、最佳实践、代码调试

### bpy_blender_as_module - Blender作为Python模块
将Blender作为Python模块运行的完整指南。

**内容：**
- 模块安装和配置
- 无头模式运行
- 数据导入导出
- 批处理操作
- 与其他Python库集成
- 渲染农场集成

**文档：** [bpy_scripting.md](references/bpy_scripting.md)

**使用场景：** 后端处理、自动化、批量渲染

### bpy_raycasting_spatial - 射线投射和空间查询模块
Blender射线投射和空间数据结构的详细参考。

**内容：**
- 射线投射操作
- 碰撞检测
- BVH树查询
- KD树查询
- 距离计算
- 最近元素搜索

**文档：** [bpy_physics_materials.md](references/bpy_physics_materials.md)

**使用场景：** 交互检测、拾取、空间分析

### bpy_drivers_expressions - 驱动和表达式模块
Blender驱动系统和Python表达式的使用指南。

**内容：**
- 驱动创建和管理
- 驱动变量和管道
- Python表达式
- 变换通道驱动
- 自定义驱动函数
- 驱动错误处理

**文档：** [bpy_scripting.md](references/bpy_scripting.md)

**使用场景：** 程序化动画、参数化建模

### bpy_scripting_templates - 脚本模板模块
Blender Python脚本的模板和代码示例。

**内容：**
- 操作符模板
- 面板模板
- 插件模板
- 完整插件示例
- UI列表模板
- 属性组模板

**文档：** [bpy_scripting.md](references/bpy_scripting.md)

**使用场景：** 快速开发、代码模板、示例学习

### bpy_callbacks_handlers - 回调处理器模块
Blender事件回调和处理器系统的详细参考。

**内容：**
- 加载保存回调
- 场景更新回调
- 帧变更回调
- 渲染回调
- 定时器回调
- 消息总线

**文档：** [bpy_advanced.md](references/bpy_advanced.md)

**使用场景：** 自动化任务、事件响应

### bpy_advanced_python - 高级Python模块
Blender Python开发的高级主题。

**内容：**
- Python 3标准库使用
- 上下文管理器和生成器
- 多线程和异步
- C扩展和性能优化
- 类型提示和静态分析

**文档：** [bpy_advanced.md](references/bpy_advanced.md)

**使用场景：** 高级开发、性能优化

### bpy_api_advanced - 高级API使用模块
Blender Python API的高级使用模式和技巧。

**内容：**
- 上下文操作和验证
- ID属性和数据访问
- 集合和遍历模式
- 属性动画和驱动
- 性能优化技术

**文档：** [bpy_advanced.md](references/bpy_advanced.md)

**使用场景：** 专业开发、性能优化

### bpy_data_inspection - 数据检查模块
Blender数据的检查和调试工具。

**内容：**
- 对象属性检查
- 数据块遍历
- 依赖关系分析
- 内存使用分析

**文档：** [bpy_context_data_types.md](references/bpy_context_data_types.md)

**使用场景：** 数据调试、问题排查

### bpy_grease_pencil - 涂鸦铅笔模块
Blender Grease Pencil 2D动画系统的完整参考。

**内容：**
- 绘制和编辑
- 图层管理
- 关键帧动画
- 修改器和效果
- 与3D场景集成

**文档：** [bpy_physics_materials.md](references/bpy_physics_materials.md)

**使用场景：** 2D动画、注释、概念设计

### bpy_advanced_features - 高级功能模块
Blender高级功能和特性的完整参考。

**内容：**
- 命令行操作
- 扩展系统
- 自定义操作符
- 多文档操作

**文档：** [bpy_advanced.md](references/bpy_advanced.md)

**使用场景：** 高级开发、系统集成

### bpy_preferences - 首选项模块
Blender首选项设置的完整参考。

**内容：**
- 编辑器首选项
- 系统设置
- 主题定制
- 快捷键设置

**文档：** [bpy_ui_preferences.md](references/bpy_ui_preferences.md)

**使用场景：** 设置管理、环境定制

### bpy_file_data - 文件和数据模块
Blender文件操作和数据管理的完整参考。

**内容：**
- 文件保存和加载
- 数据块操作
- 链接和追加
- 库文件管理

**文档：** [bpy_props_path.md](references/bpy_props_path.md)

**使用场景：** 文件操作、资产管理

### bpy_sculpt_paint - 雕刻和绘画模块
Blender雕刻和绘画系统的完整参考。

**内容：**
- 雕刻模式和笔刷
- 顶点绘画
- 权重绘画
- 纹理绘画

**文档：** [bpy_physics_materials.md](references/bpy_physics_materials.md)

**使用场景：** 雕刻、纹理绘制

### bpy_compositing - 合成模块
Blender合成节点系统的完整参考。

**内容：**
- 合成节点类型
- 图像处理
- 颜色校正
- 渲染层合成

**文档：** [bpy_rendering.md](references/bpy_rendering.md)

**使用场景：** 后期处理、特效制作

### bpy_python_scripting - Python脚本开发模块
Blender Python脚本开发的全面指南。

**内容：**
- 脚本结构和组织
- 错误处理和调试
- 测试和验证
- 性能优化

**文档：** [bpy_scripting.md](references/bpy_scripting.md)

**使用场景：** 脚本开发、自动化

### bpy_physics_simulation - 物理模拟模块
Blender物理模拟系统的完整参考。

**内容：**
- 粒子系统
- 布料模拟
- 流体模拟
- 刚体模拟

**文档：** [bpy_physics_materials.md](references/bpy_physics_materials.md)

**使用场景：** 物理效果、动画特效

### bpy_materials_shading - 材质和着色模块
Blender材质系统和着色工作流的完整参考。

**内容：**
- Principled BSDF
- 节点材质
- 纹理和程序化
- 材质分配

**文档：** [bpy_rendering.md](references/bpy_rendering.md)

**使用场景：** 材质创建、程序化着色

### bpy_animation_system - 动画系统模块
Blender动画系统的全面指南。

**内容：**
- 关键帧和动画曲线
- 动作和NLA
- 驱动系统
- 约束动画

**文档：** [bpy_scripting.md](references/bpy_scripting.md)

**使用场景：** 动画制作、动作管理

### bpy_geometry_nodes - 几何节点模块
Blender几何节点系统的完整参考。

**内容：**
- 节点组和节点树
- 几何数据流
- 属性系统
- 实例化

**文档：** [bpy_geometry_nodes.md](references/bpy_geometry_nodes.md)

**使用场景：** 程序化建模、实例化

### bpy_geometry_nodes_advanced - 几何节点高级模块
几何节点系统的高级功能和技巧。

**内容：**
- 随机化技术
- 反馈和缓存
- 命名属性
- 性能优化

**文档：** [bpy_geometry_nodes.md](references/bpy_geometry_nodes.md)

**使用场景：** 高级建模、性能优化

### bpy_geometry_nodes_zones_fields - 几何节点区域和字段模块
几何节点中区域和字段系统的深入讲解。

**内容：**
- 区域概念和类型
- 字段求值
- 字段上下文
- 区域嵌套

**文档：** [bpy_geometry_nodes.md](references/bpy_geometry_nodes.md)

**使用场景：** 复杂几何操作、迭代算法

### bpy_geometry_nodes_scene - 几何节点场景模块
几何节点与场景系统的交互。

**内容：**
- 场景对象访问
- 材质分配
- 集合实例化
- 变换传递

**文档：** [bpy_geometry_nodes.md](references/bpy_geometry_nodes.md)

**使用场景：** 场景程序化、资产管理

### bpy_modifiers_tools - 修改器和工具模块
Blender修改器和工具系统的完整参考。

**内容：**
- 修改器类型
- 修改器堆栈
- 修改器参数
- 修改器动画

**文档：** [bpy_physics_materials.md](references/bpy_physics_materials.md)

**使用场景：** 建模变形、程序化修改

### bpy_rendering_assets - 渲染资源模块
Blender渲染资源管理的完整参考。

**内容：**
- 渲染层和通道
- AOV配置
- 色彩管理
- 资源库集成
- Cycles Sample Subset 分布式渲染
- 命令行远程渲染
- 多机器帧分配渲染
- 渲染任务队列管理
- 图像合并操作

**文档：** [bpy_rendering.md](references/bpy_rendering.md)

**使用场景：** 渲染管理、资源组织、分布式渲染、远程渲染

### bpy_addon_security - 插件安全模块
Blender插件安全性和沙箱机制的完整参考。

**内容：**
- 权限系统
- 沙箱限制
- 安全编码实践

**文档：** [bpy_advanced.md](references/bpy_advanced.md)

**使用场景：** 插件安全、安全开发

### bpy_ui_design - UI设计模块
Blender插件UI设计的最佳实践和指南。

**内容：**
- 面板布局设计
- 控件和布局
- 主题集成
- 用户体验

**文档：** [bpy_ui_preferences.md](references/bpy_ui_preferences.md)

**使用场景：** 插件开发、UI设计

## 版本兼容性

本技能专为Blender 5.0设计。以下是与其他版本的兼容性说明：

- **Blender 4.0+**：大多数功能经过小幅调整后应可正常工作
- **Blender 3.0+**：部分较新的API可能不可用
- **Blender 2.8+**：基本功能应可工作，但新功能可能缺失

## 资源

### 官方文档
- [Blender Python API参考](https://docs.blender.org/api/current/)
- [Blender Python API示例](https://docs.blender.org/api/current/examples.html)

### 社区资源
- [Blender Stack Exchange](https://blender.stackexchange.com/)
- [Blender Artists论坛](https://www.blenderartists.org/)
- [Blender Python API Cookbook](https://wiki.blender.org/wiki/Dev:Py/Scripts/Cookbook)

### 开发工具
- [Blender插件模板](https://github.com/blender/blender-addon-template)
- [Blender Python API调试器](https://github.com/jpbouza/blender-python-debugger)

## 许可证

本技能采用Apache 2.0许可证。详情请参阅LICENSE文件。

## 贡献

欢迎贡献！请遵循以下指南：

1. Fork本仓库
2. 创建功能分支
3. 进行修改
4. 在Blender 5.0中全面测试
5. 提交Pull Request

## 更新日志

### v1.0.0（当前版本）
- Blender 5.0初始版本
- 完整的API参考文档
- 代码示例和最佳实践
- 开发工作流和模板

---

**注意**：本技能会持续维护和更新，以反映Blender Python API的变化。如需最新更新，请查看仓库的发行说明。
bpy.app.debug = True

# Print context information
print(f"Context: {bpy.context}")
print(f"Active object: {bpy.context.active_object}")

# 检查数据
for obj in bpy.data.objects:
    print(f"Object: {obj.name}, Type: {obj.type}")
```

## 资源

- **Blender API文档**：https://docs.blender.org/api/current/
- **Python API参考**：https://docs.blender.org/api/current/bpy.html
- **插件开发**：https://docs.blender.org/manual/en/latest/advanced/addons/
- **Blender Stack Exchange**：https://blender.stackexchange.com/
- **Blender Artists**：https://blenderartists.org/

## 版本信息

本技能专为Blender 5.0 API设计。对于其他版本，请检查兼容性并相应调整。

## Blender 5.1 版本更新 API

本章节介绍 Blender 5.1 版本中 Python API 的变更内容。

### Python 版本升级

Blender 5.1 将 Python 升级到版本 3.13，与 VFX 平台 2026 保持一致。

### 新增功能

#### bpy.app.cachedir
返回 Blender 使用的缓存目录。

```python
import bpy
print(f"缓存目录: {bpy.app.cachedir}")
```

#### bpy.app.handler.exit_pre
新增退出前处理器,允许在 Blender 退出时运行 Python 代码。

```python
import bpy

def exit_callback():
    print("Blender 正在退出...")
    # 执行清理操作

bpy.app.handlers.exit_pre.append(exit_callback)
```

#### window.find_playing_scene()
查找当前正在播放的场景。

```python
import bpy

scene = bpy.context.window.find_playing_scene()
if scene:
    print(f"正在播放的场景: {scene.name}")
```

#### windows.find_playing()
查找正在播放场景的窗口。

```python
import bpy

windows = bpy.context.windows.find_playing()
for window in windows:
    print(f"窗口: {window.screen.name}")
```

#### UILayout.template_ID_session_uid
新增模板 ID 搜索菜单,允许使用 ID 的 session uid 作为 int 输入值。

```python
import bpy

layout = bpy.context.layout
layout.template_ID_session_uid(bpy.context.scene, "active_object")
```

#### gpu.types.GPUFrameBuffer.read_color 和 read_depth
读取颜色和深度数据时,越界会抛出 ValueError。

```python
import bpy
import gpu

fb = gpu.types.GPUFrameBuffer()
try:
    color = fb.read_color(0, 0, 1, 1, 4, 0, 'FLOAT')
except ValueError as e:
    print(f"读取越界: {e}")
```

### API 变更

#### mesh.validate()
现在在调用时会修复无效(非有限)的 Multiresolution 切线向量数据。

```python
import bpy

obj = bpy.context.active_object
mesh = obj.data
mesh.validate()  # 现在会修复无效的切线向量数据
```

#### UILayout.template_list.columns 参数弃用
columns 参数已被弃用,自 5.0 版本起已失效。

```python
import bpy

layout = bpy.context.layout
layout.template_list("MY_UL_List", "list_id", context, "items", context, "active_index")
# columns 参数已无效,请使用其他布局方式
```

#### 笔刷stroke_method 属性重组
多个笔刷属性已合并为 `brush.stroke_method` 枚举字段:

| 旧属性 | 新枚举值 |
|--------|---------|
| use_airbrush | AIRBRUSH |
| use_anchor | ANCHORED |
| use_space | SPACE |
| use_line | LINE |
| use_curve | CURVE |
| use_restore_mesh | DRAG_DOT |

```python
import bpy

brush = bpy.context.tool_settings.image_paint.brush

# 旧方式(已弃用)
# brush.use_airbrush = True
# brush.use_space = True

# 新方式
brush.stroke_method = 'AIRBRUSH'
```

#### sculpt.sample_color 操作符移除
`sculpt.sample_color` 操作符已被移除,其功能已合并到 `paint.sample_color`。

```python
import bpy

# 旧方式(已移除)
# bpy.ops.sculpt.sample_color()

# 新方式
bpy.ops.paint.sample_color()
```

### VSE 序列时间属性重命名

视频序列编辑器中的序列时间属性已重命名以提高清晰度:

| 旧属性 | 新属性 | 说明 |
|--------|--------|------|
| frame_final_duration | duration | 最终持续时间 |
| frame_final_start | left_handle | 左手柄位置 |
| frame_final_end | right_handle | 右手柄位置 |
| frame_offset_start | left_handle_offset | 左手柄偏移 |
| frame_offset_end | right_handle_offset | 右手柄偏移 |
| frame_duration | content_duration | 内容持续时间 |
| frame_start | content_start | 内容起始帧 |
| animation_offset_start | content_trim_start | 内容修剪起始 |
| animation_offset_end | content_trim_end | 内容修剪结束 |

```python
import bpy

strip = bpy.context.active_sequence_strip

# 旧方式(仍可用但已弃用)
# duration = strip.frame_final_duration
# left_handle = strip.frame_final_start

# 新方式
duration = strip.duration
left_handle = strip.left_handle
right_handle = strip.right_handle
content_start = strip.content_start
content_end = strip.content_end  # 新增,只读访问内容结束帧
```

### macOS 权限变更

在 macOS 上,脚本和扩展现在可以访问设备摄像头、麦克风和系统音频。用户将看到系统权限对话框。

### Py-defined RNA structs 名称属性覆盖

Py 定义的 RNA struct 现在可以完全覆盖其 'name property'(例如用作集合中的字符串键、UI 小部件如搜索字段等)。

### sequencer.retiming_segment_speed_set 参数移除

`keep_retiming` 选项已被移除,因为只有设置为 true 时片段才会正确调整时间。设置为 false 仅会添加不需要的钳制,导致错误行为。
