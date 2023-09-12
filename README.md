# Lesson 1 UE入门 —— 课堂笔记
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

## 游戏引擎概况
### 游戏引擎特性：
- data-driven
- reusable
- no game content
- 通用性与偏向性
- 可扩展性
- 完善工具链

### 渲染：
#### 方法上区分：
- Deferred Renderer
    - 编辑器、PC、Console默认渲染管线
    - Feature Levels "SM4","SM5"

- Forward+ Renderer
    - 桌面、VR游戏，支持MSAA
    - Feature Levels "SM5"
#### 渲染架构上区分：

- Immediate Mode Rendering
    - 带宽要求高
    - 能耗高

- Tile Based Rendering

- Tile Based Deferred Rendering
#### 移动端渲染管线：
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

### 物理：
#### 主流物理引擎：
- Havok
- PhysX
- Bullet
#### 物理引擎包含内容：
- 碰撞检测
- 动态约束
- 刚体物理
- 车辆物理
- 布娃娃系统

    ....



## Unreal Engine简介
### Editor
#### 特性：
- 不仅是一个世界编辑器
- 管理整个游戏资产数据
- 提供统一、实时、所见即所得的资产数据库视图
#### 源码版位置：
- PathTo\UnrealEngine\Engine\Binaries\Win64\UE4Editor.exe(UE4)
- PathTo\UnrealEngine\Engine\Binaries\Win64\UnrealEditor.exe(UE5)
#### 具体功能&操作：
- 请参考官方文档：https://docs.unrealengine.com/5.2/zh-CN/unreal-editor-interface/
- 典型功能：
    - 地图关卡的创建和分层
    - 可视化游戏世界
    - 导航
    - 选取
    - 属性设置
    - 安放与对齐辅助工具
    - 快速迭代
    - Volume
    - 光源
#### Content Browser
##### 目录结构：
- Content目录下文件结构与Windows资源管理器磁盘结构一一对应
- UE资源为*.uasset文件
- 非UE资源，**建议单独目录存放**
##### 资源迁移：
- 使用资源迁移工具来保证内部资源之间的**相互引用关系**不丢失
- 资源必须在Content目录下
##### 命名规范：
- https://github.com/Allar/ue4-style-guide
- https://github.com/skylens-inc/ue4-style-guide
#### Derived Data Cache
一个在工作室中共享的资源仓库，其中保存了各种版本的资源文件在不同平台上对应的Derived Data
- Separate source and derived data
    - Compressed textures
    - Shader Code
    - Optimized meshes
- Different representation per platform
    - When engine needs derived data for platform
    - Key = asset + target platform, Request key from DDC Server
- Many advantages
    - Doesn't bloat .uasset size
    - Change processing without touching .uasset

### Project Struct
#### Project 项目
项目(Project)是一个自成体系的单元，保存所有组成单独游戏的所有内容和代码，并与硬盘上的一组目录相一致

项目文件：.uproject
#### World 世界
世界场景(World)中包含载入的关卡列表，它可处理关卡流送和动态Actor的生成（创建）
#### Level 关卡
level（关卡）是用户定义的游戏区域
我们主要通过放置、变换及编辑Actor的属性来创建、查看及修改关卡
在虚幻编辑器中，每个关卡都被保存为单独的.**umap**文件，也被称为“**地图**”
#### Sub-Levels 子关卡
- A small area of whole map
- Decouple the work between artist and designer
#### Actors
- 可放入关卡中的对象都是Actors
- Actors是支持三维转换（如平移、旋转和缩放）的泛类型，通常包括一个或者多个Actor Components
- 可通过游戏进程代码（C++或蓝图）创建及销毁Actor
- 在C++中，AActor是所有Actor的基本类
- 没有Location, Rotation (两者都在 root component)
- 用SpawnActor()创建
- 必须显式用Destroy()销毁
#### Component 组件
组件(Component)是可添加到Actor的一项功能

组件不可独立存在，但在将其添加到Actor后，该Actor便可以访问并可以使用该组件所提供的功能

## 编译UE源码
参考官方文档：https://docs.unrealengine.com/5.2/zh-CN/setting-up-your-development-environment-for-cplusplus-in-unreal-engine/

### 编译过程
- 运行Setup.bat，下载依赖文件
- 运行GenerateProjectFiles.bat， 生成工程文件.sln
- 生成项目文件
    - 运行Engine\Binaries\Win64\UnrealVersionSelector-Win64-Shipping.exe 注册引擎
    - 工程文件右键菜单中生成项目*.sln文件
- 编译通过Unreal Build Tool (UBT) 完成
    - 调用Unreal Header Tools (UHT) 预处理头文件，解析头文件中引擎相关类元数据，产生**胶水代码**(放置于Intermediate目录)
    - 调用特定平台普通C++编译器，正式开始编译

### 编译规则
- 目标文件 (Target)
    - 支持客户端、服务器、编辑器等
    - 通过C#源文件声明，扩展名为.target.cs，存储在项目Source目录下
- 模块 (Module)
    - 通过C#源文件声明，扩展名为.build.cs，存储在项目Source目录下
    - 属于一个模块的C++源代码与.build.cs文件并列存储

### Module 
官方文档：https://docs.unrealengine.com/5.2/en-US/unreal-engine-modules/

#### Module 类型
- Developer - Code for additional tools (output log, source control, profiling, cooking, derived data cahe, ...)
    - Asset cooking : __Target__, __Format__
    - Mesh tools : __Mesh__
    - Profiling : __Profile__
    - Shader compiler support : __Shader__
    - Logging : __Log__
- Editor - Code for implementing the Unreal Editor. many systems have separate Runtime and Editor modules
    - Blueprint : __Kismet__, __Blueprint__
    - Animation : __Anim__
    - Various independent editor panels : __Editor__
- Runtime - Code needed while the game is running
    - Basic functionality for engine and major subsystems : __Core__, __Engine__
    - Rendering : __Render__, __RHI__
    - Slate UI : __Slate__
    - Audio, Video : __Audio__, __Media__
    - AI : __AI__, __Nav__
- ThirdParty - External code from other companies
- Plugins - Extensions for Editor, Games, or both
- Programs - Build and header tools, lightmass light baking, shader compiler,...

#### Module 依赖规则
- Runtime modules must not have dependencies to Editor or Developer modules
- Plug-in modules must not have dependencies to other plug-ins

#### Important Modules for Beginners
- Engine(Runtime) - Game classes & engine framework
- UMG(Runtime) - Unreal Motion Graphics implementation
- Slate(Runtime) - Widget library & high-level UI features
- SlateCore(Runtime) - Fundamental UI functionality
- Core(Runtime) - Fundamental core types & functions

### Entry Point
LaunchEngineLoop.cpp

## 游戏框架
### GameWorld
- 静态元素
- 动态元素

### GameMode 游戏模式
- GameMode类负责设置正在执行的游戏的规则
- 规则可包括玩家如何加入游戏、是否可暂停游戏、关卡过渡、以及任何特定的游戏行为（例如获胜条件）
- Only exists on the server and single player instances
- GameState is used to replicate game state to clients
- Default game mode can be set in Project Settings
- Per-map overrides in World Setting

### GameState 游戏状态
- GameState包含要复制到游戏中的每个客户端的信息，它表示整个游戏的“游戏状态”
- 它通常包含有关游戏分数、比赛是否已经开始和基于世界场景玩家人数要生成的AI数量的信息等等

### PlayState 玩家状态
- PlayState是游戏玩家的状态，非玩家AI将不会拥有玩家状态
- 在玩家状态中的数据包括玩家姓名或得分、当前等级或生命值等

### Pawn 兵卒
- Pawn是Actor的一个子类，充当游戏中的生命体
- An agent in the world
- Optionally possessed by Controller
- Good place to implement health
- No movement or input code by default

### Character 角色
- 角色(Character)是Pawn Actor的子类，用作玩家角色
- 角色子类包括碰撞设置、双足运动的输入绑定，以及由玩家控制的运动附加代码
- Special Pawn that can walk
- Comes with useful Components
- Handles collision
- Client-side movement prediction(UcharacterMovementComponent)

### PlayerController 玩家控制器
- PlayerController类用于在游戏中获取玩家输入并将其转换为交互
- 玩家控制器通常拥有一个Pawn或角色作为游戏中玩家的代表

## Blueprint
### 机制
- 是一个特殊的asset，在编译后会生成对应的字节码
- 引擎运行时会读取字节码，并交由**蓝图虚拟机**动态解释执行

### 分类
- __Level Blueprint__
    - 自动创建，每个Level一个
    - 生命周期是整个关卡存在的时间
    - 监听Level级别的事件
- __Blueprint Class__
    - 自己可以随意添加，需要继承已有的类
    - 可以添加不同的组件来丰富功能
    - 通常被放置在关卡中，执行自身的逻辑功能
- Data-Only Blueprint
- Blueprint Interface
- Blueprint Macros

### 使用
- 从监听的事件开始
- 其他Actor接口调用
    - 从关卡中拖进来Actor，调用其接口
    - 右键选择系统提供的默认Actor
- 调用蓝图库中的函数
#### 使用原则
- 用于数值配置
- 用于简单的效果展示
#### 使用方法
- 使用编辑器来创建蓝图类，将不同功能的组件组合起来
- 蓝图类继承C++类，由程序暴露可视化变量，函数和事件

#### Blueprint与C++之间成员变量
- 通过UPROPERTY()
#### Blueprint与C++之间的相互函数调用
- UFUNCTION(BlueprintCallable) : C++实现函数体，蓝图调用
- UFUNCTION(BlueprintImplementableEvent) : 蓝图中实现函数体，C++中声明和调用
- UFUNCTION(BlueprintNativeEvent) :C++中实现默认函数体，蓝图中可以选择重载
    - C++函数体定义有语法规定： [FunctionName]_Implementation instead of [FunctionName]


## Lua
### Lua Plugin for UE
- slua-unreal : https://github.com/Tencent/sluaunreal
- UnLua : https://github.com/Tencent/UnLua
- unreal的插件
- 通过unreal的反射能力，导出蓝图接口和静态C++接口给Lua语言
- 支持Lua到C++双向，Lua到蓝图双向调用

## C++ in UE
### 基本类型
- 不使用C++原生的整型(char, short, int, long ...)
- 自定义ints & strings in GenericPlatform.h(int32, uint32, uint64, TCHAR, ANSICHAR...)
### 容器
- TArray, TSparseArray --- Dynamic arrays
- TLinkedList, TDoubleLinkedList
- TMap --- Key-value hash table
- TQueue --- Lock free FIFO
- TSet --- Unordered set (without duplicates)
- And many more in __Core module__
### 智能指针
- TSharedPtr, TSharedRef --- for regular C++ objects
- TWeakPtr --- for regular C++ object
- TWeakObjPtr --- for UObjects
- TAutoPtr, TScopedPtr
- TUniquePtr
### 其他常用结构体
- FBox, FColor, FGuid, FVariant, FVector, TBigInt, TRange

### UObject
官方文档：https://docs.unrealengine.com/5.2/en-US/API/Runtime/CoreUObject/UObject/UObject/

重要函数GetWorld()，可以方便获取需要的重要信息
#### Magic Macros
- UCLASS-类
- USTRUCT-结构体
- UFUNCTION-成员函数
- UPROPERTY-成员变量

## 引擎工具
### 日志
- 例子：UE_LOG(LogGamesSettings, Log, TEXT("[Render] set shadow : [%d]"), bShadow);

其中Log：
- Channel
- Verbosity Level

### 可视化日志
- 官方文档: https://docs.unrealengine.com/5.2/zh-CN/visual-logger-in-unreal-engine/
- 通过在逻辑代码中通过日志形式收集信息，然后用工具进行绘制显示来进行回放，同时收集到的信息可以序列化为磁盘文件

### 内置控制台
- 官方文档 : https://docs.unrealengine.com/5.3/zh-CN/testing-and-optimizing-your-content/

- 游戏运行时，~键打开控制台（Mobile使用四指同时滑屏操作）

### GPU调试工具
- Profile : "Ctrl + Shift + ," 在编辑器模式下截取GPU快照，并可视化
- 图形调试 : RenderDoc、NSight ...
- Unreal Insight ：https://zhuanlan.zhihu.com/p/511148107