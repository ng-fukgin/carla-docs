
在Windows环境下构建Carla
==================

本篇详细介绍了如何在 Windows 上从源代码构建 CARLA。 有两个部分。 第一部分详细介绍了系统要求和所需软件的安装，第二部分详细介绍了如何实际构建和运行 CARLA。

构建过程很长（4 小时或更长时间）并且涉及多种软件。 强烈建议在开始之前通读本文章。

如果您遇到错误或困难，请使用 CARLA 中文论坛发布您的任何疑问，有问必答！

Part One: 构建之前的准备
-----------------

在本节中，您将找到开始构建 CARLA 之前所需的系统要求、次要和主要软件安装以及 Python 依赖项的详细信息。

### 系统要求

*   **x64 系统-**Carla模拟器可以在任何 64 位 Windows 系统中运行。
*   **165 GB 磁盘空间-**CARLA 本身大约需要 32 GB，相关的主要软件安装（包括虚幻引擎）大约需要 133 GB。
*   **确保GPU内存充足-**CARLA 旨在进行逼真的模拟，因此服务器至少需要 6 GB GPU，但建议使用 8 GB。 强烈建议使用专用 GPU 进行机器学习。
*   **两个 TCP 端口和良好的互联网连接**。默认为 2000 和 2001。 确保这些端口未被防火墙或任何其他应用程序阻止。

如果您是从 CARLA 0.9.12 升级到 0.9.13：您必须先将 UE4 引擎的 CARLA fork 升级到最新版本。 

### 软件要求

#### 次要安装

*   [**CMake**](https://cmake.org/download/) 从简单的配置文件生成标准构建文件。
*   [**Git**](https://git-scm.com/downloads) 是用于管理 CARLA 存储库的版本控制系统。
*   [**Make**](http://gnuwin32.sourceforge.net/packages/make.htm) 生成可执行文件。 必须使用 Make 3.81 版本，否则构建可能会失败。 如果您安装了多个版本的 Make，请在构建 CARLA 时检查您的 PATH 中是否使用了 3.81 版本。 您可以通过运行 `make --version`来检查您的默认 Make 版本。
*   [**7Zip**](https://www.7-zip.org/) 是一个文件压缩软件。 这对于资产文件的自动解压缩是必需的，并防止在构建期间由于不正确或部分提取大文件而导致错误。
*   [**Python3 x64**](https://www.python.org/downloads/) 是 CARLA 中的主要脚本语言。 安装 x32 版本可能会导致冲突，因此强烈建议将其卸载。

确保将上述程序添加到环境路径中。 请记住，添加的路径应与程序的  `bin`  目录相对应。

#### Python 依赖项

从 CARLA 0.9.12 开始，用户可以选择使用 `pip3` 安装 CARLA Python API。 需要 20.3 或更高版本。 要检查您是否有合适的版本，请运行以下命令：

    pip3 -V
    

如果您需要升级:

    pip3 install --upgrade pip
    

请务必安装以下Python依赖项:

    pip3 install --user setuptools
    pip3 install --user wheel
    

#### Major installations主要安装

##### Visual Studio 2019

从[此处](https://developerinsider.co/download-visual-studio-2019-web-installer-iso-community-professional-enterprise/)获取 2019 版 Visual Studio。 选择**Community**以获取免费版本。 使用 _Visual Studio_ 安装程序安装三个附加元素：

*   **Windows 8.1 SDK.** 在右侧的安装详细信息部分中选择它，或转到单个组件选项卡并查看 SDK、库和框架标题下的内容。
*   **x64 Visual C++ Toolset.** 这将启用将用于构建的 x64 命令提示符。 按 `Windows` 按钮并搜索 `x64`，检查它是否已正确安装。 **注意不要打开 `x86_x64`提示**。
*   **.NET framework 4.6.2**. 在_Workloads部分，_选择**.NET desktop development** 然后安装右边的_details_ panel, 选择 `.NET Framework 4.6.2 development tools`. 这项操作要求虚幻引擎构建。

其他 Visual Studio 版本可能会导致冲突。 即使这些已被卸载，一些寄存器可能仍然存在。 从计算机中彻底清除 Visual Studio, 去到 `Program Files (x86)\Microsoft Visual Studio\Installer\resources\app\layout` 运行 `.\InstallCleanup.exe -full`

#### 虚幻引擎

从 0.9.12 版开始，CARLA 使用了 Unreal Engine 4.26 的修改分支。 此分叉包含特定于 CARLA 的补丁。

请注意，要下载此 Unreal Engine 分支，您需要有一个与 Unreal Engine 帐户相关联的 GitHub 帐户。

构建修改版的虚幻引擎：

**1.** 在终端中，导航到要保存虚幻引擎的位置并克隆 carla 分支：

        git clone --depth 1 -b carla https://github.com/CarlaUnreal/UnrealEngine.git .
    

Note注意

将 Unreal Engine 文件夹尽可能靠近 C:\\\\，因为如果路径超过一定长度，Setup.bat 将在步骤 3 中返回错误。

**2.** Run the configuration scripts:

        Setup.bat
        GenerateProjectFiles.bat
    

**3.** 运行配置脚本:

> 1.  打开 `UE4.sln` 文件  （在Visual Studio 2019文件夹内）
>     
> 2.  在构建栏中，确保您选择了“开发编辑器”、“Win64”和“UnrealBuildTool”选项。
>     
> 3.  在解决方案资源管理器中，右键单击 UE4 并选择构建。
>     

**4.** 编译解决方案后，您可以打开引擎以通过启动可执行文件来检查所有内容是否已正确安装 `Engine\Binaries\Win64\UE4Editor.exe`.

Note

如果安装成功，虚幻引擎的版本选择器应该可以识别。 您可以通过右键单击任何 .uproject 文件并选择切换虚幻引擎版本来检查这一点。 您应该会看到一个弹出窗口，其中显示 PATH 中的 Source Build，其中 PATH 是您选择的安装路径。 如果您在右键单击 .uproject 文件时看不到此选择器或生成 Visual Studio 项目文件，则说明虚幻引擎安装出现问题，您可能需要正确重新安装它。

Important

强烈建议在继续之前重新启动计算机。

Part Two: 构建CARLA
-----------------

### 克隆 CARLA 存储库

 [CARLA repository](https://github.com/carla-simulator/carla "Go to the CARLA repository")

上面的按钮会将您带到项目的官方存储库。 从那里下载并在本地解压缩或使用以下命令克隆它：

        git clone https://github.com/carla-simulator/carla
    

Note

主分支包含 CARLA 的当前版本以及最新的修复和功能。 以前的 CARLA 版本标有版本名称。 永远记得使用命令 git branch 检查 git 中的当前分支。

### 获得资产

通过在 CARLA 根文件夹中运行以下命令，下载最新的资产以使用当前版本的 CARLA：

        Update.bat
    

如果安装了 7zip，资产将被下载并解压缩到适当的位置。 如果您没有安装此软件，则需要手动将文件内容提取到 Unreal\\CarlaUE4\\Content\\Carla

要下载特定版本 CARLA 的资产：

*   从根 CARLA 目录，导航到 \\Util\\ContentVersions.txt。 本文档包含指向所有 CARLA 版本的资产的链接。
*   提取 Unreal\\CarlaUE4\\Content\\Carla 中的资产。 如果路径不存在，则创建它。
*   使用类似于以下的命令提取文件：

        tar -xvzf <assets_file_name>.tar.gz.tar -C C:\path\to\carla\Unreal\CarlaUE4\Content\Carla
    

### 设置虚幻引擎环境变量

需要设置一个环境变量，以便 CARLA 可以找到虚幻引擎的安装文件夹。 这允许用户选择使用哪个特定版本的虚幻引擎。 如果没有指定环境变量，那么 CARLA 将在 Windows 注册表中搜索 Unreal Engine 并使用它在其中找到的第一个版本。

设置环境变量:

1\. 打开 Windows 控制面板并转到高级系统设置或在 Windows 搜索栏中搜索高级系统设置。  
2\. 在高级面板上打开环境变量...。  
3\. 单击“新建...”以创建变量。  
4\. 将变量命名为 UE4\_ROOT 并选择所需虚幻引擎安装的安装文件夹的路径。

### 构建CARLA

本节概述了构建 CARLA 的命令。

*   所有命令都应在根 CARLA 文件夹中运行。
*   命令应通过 VS 2019 的 x64 本机工具命令提示符执行。通过单击 Windows 键并搜索 x64 打开它。

CARLA 的构建过程分为两部分，编译客户端和编译服务器。

**1\. 编译 Python API 客户端：**

Python API 客户端授予对模拟的控制权。 首次构建 CARLA 以及执行任何更新后都需要编译 Python API 客户端。 客户端编译完成后，您将能够运行脚本与模拟进行交互。

以下命令编译 Python API 客户端：

        make PythonAPI
    

CARLA 客户端库将以两种不同的、互斥的形式构建。 这使用户可以自由选择他们喜欢以哪种形式运行 CARLA 客户端代码。 这两种形式包括 .egg 文件和 .whl 文件。 选择以下选项之一以使用客户端库：

**A. `.egg` file**

> .egg 文件不需要安装。 CARLA 的所有示例脚本在导入 CARLA 时都会自动查找此文件。
> 
> 如果您之前安装了 CARLA .whl，则 .whl 将优先于 .egg 文件。

**B. `.whl` file**

> .whl 文件应使用 pip3 安装：

    pip3 install <path/to/wheel>.whl
    

> 此 .whl 文件无法分发，因为它是专门为您的操作系统构建的。

Warning

使用不同方法安装 CARLA 客户端库并在系统上使用不同版本的 CARLA 可能会出现问题。 建议在安装 .whl 时使用虚拟环境，并在安装新的客户端库之前卸载任何以前安装的客户端库。

**2\. 编译服务器：**

以下命令编译并启动虚幻引擎。 每次要启动服务器或使用虚幻引擎编辑器时运行此命令：

        make launch
    

该项目可能会要求在第一次构建其他实例，例如 UE4Editor-Carla.dll。 同意以打开项目。 在第一次启动期间，编辑器可能会显示有关着色器和网格距离场的警告。 这些需要一些时间才能加载，直到那时地图才能正确显示。

**3\. 开始模拟：**

按 Play 开始服务器模拟。 可以使用 WASD 键移动相机，并通过在移动鼠标的同时单击场景来旋转相机。

使用 PythonAPI\\examples 中的示例脚本测试模拟器。 随着模拟器的运行，为每个脚本打开一个新终端并运行以下命令以在城镇中产生一些生命并创建一个天气周期：

            # Terminal A 
            cd PythonAPI\examples
            pip3 install -r requirements.txt
            python3 generate_traffic.py  
    
            # Terminal B
            cd PythonAPI\examples
            python3 dynamic_weather.py 

### 其他 make 命令

还有更多您可能会觉得有用的 make 命令。 在下表中找到它们：

Command

Description

`make help`

打印所有可用的命令。

`make launch`

在编辑器窗口中启动 CARLA 服务器。

`make PythonAPI`

构建 CARLA 客户端。

`make LibCarla`

准备要在任何地方导入的 CARLA 库。

`make package`

构建 CARLA 并创建用于分发的打包版本。

`make clean`

删除构建系统生成的所有二进制文件和临时文件。

`make rebuild`

`make clean` 和 `make launch` 都在一个命令中.




