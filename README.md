# Lesson 1 UE入门 
#### 本节内容概述
- UE**学习途径**和**方法**的介绍

- UE**编辑器使用**和**编程技巧**

- UE引擎工具了解
#### 本节课程作业
- **源码编译**UE4/5，**新建**一个C++工程，进行**简单场景编辑**和**工程设置**

- **编译并构建**安装包，确保能够用来将游戏工程**安装到手机**正常运行(若无Android手机，可构建桌面版本)
#### 本节课程目标
- 熟悉UE引擎的**编辑器操作**，了解UE引擎的**游戏模式框架**

- 能够**独立获取和编译**UE源码

- 能够在UE引擎中实现**蓝图编程**和**C++编程**

- 能够使用UE引擎构建Android平台游戏安装包

## PART 1
#### 游戏引擎特性：
- data-driven
- reusable
- no game content
- 通用性与偏向性
- 可扩展性
- 完善工具链

#### 渲染：
##### 方法上区分：
- Deferred Renderer
    - 编辑器、PC、Console默认渲染管线
    - Feature Levels "SM4","SM5"

- Forward+ Renderer
    - 桌面、VR游戏，支持MSAA
    - Feature Levels "SM5"
##### 渲染架构上区分：

- Immediate Mode Rendering

- Tile Based Rendering

- Tile Based Deferred Rendering
##### 移动端渲染管线：
- Mobile Renderer
    - Forward Render, Defferred Render
    - Feature Levels "ES2", "ES3_1", "Vulkan"
    - mobile pipline:
        - view setup
        - gpu particle simulation
        - shadow map rendering
        - base pass
        - decals
        - modulated shadows
        - translucency
        - post-processing & tonemapping
        - HUD & UI

#### 物理：
##### 主流物理引擎：
- Havok
- PhysX
- Bullet
##### 物理引擎包含内容：
- 碰撞检测
- 动态约束
- 刚体物理
- 车辆物理
- 布娃娃系统

    ....



## PART 2
### Editor
#### 特性：
- 不仅是一个世界编辑器
- 管理整个游戏资产数据
- 提供统一、实时、所见即所得的资产数据库视图
#### 源码版位置：
- PathTo\UnrealEngine\Engine\Binaries\Win64\UE4Editor.exe(UE4)
- PathTo\UnrealEngine\Engine\Binaries\Win64\UnrealEditor.exe(UE5)
