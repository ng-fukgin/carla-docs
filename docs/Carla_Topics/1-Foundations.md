Foundations
===============

Foundations
-----------

本页介绍了解 CARLA 服务器和客户端如何通过 API 操作和通信所需的基本概念。 CARLA 使用服务器-客户端架构运行，CARLA 服务器运行模拟，指令由客户端发送给它。 客户端代码使用 API 与服务器通信。 要使用 Python API，您必须通过 PIP 安装模块：

    pip install carla-simulator # Python 2
    pip3 install carla-simulator # Python 3

还要确保在你的 python 脚本中导入 CARLA 包：

    import carla

1.  **世界和客户端**  
    客户端  
    世界
2.  **同步和异步模式**  
    设置同步模式  
    使用同步模式
3.  **录音机**  
    记录  
    模拟回放  
    录音机文件格式

世界和客户端
------

### 客户端

客户端是用户运行以在模拟中询问信息或更改的模块。 客户端使用 IP 和特定端口运行。 它通过终端与服务器通信。 可以有多个客户端同时运行。 高级多客户端管理需要彻底了解 CARLA 和同步。

使用 CARLA 客户端对象设置客户端：

    client = carla.Client('localhost', 2000)

这将客户端设置为与本地计算机 localhost 上运行的 CARLA 服务器进行通信。 或者，如果在单独的机器上运行客户端，则可以使用网络机器的 IP 地址。 第二个参数是端口号。 默认情况下，CARLA 服务器将在端口 2000 上运行，如有必要，您可以在启动 CARLA 时在设置中更改此设置。

客户端对象可用于许多功能，包括加载新地图、记录模拟和初始化交通管理器：

    client.load_world('Town07')
    
    client.start_recorder('recording.log')

### 世界

世界是代表模拟的对象。 它作为一个抽象层，包含生成actors、改变天气、获取世界当前状态等的主要方法。每个模拟只有一个世界。 当地图改变时，它将被销毁并替换为新的。

使用客户端对象检索世界对象：

    world = client.get_world()

世界对象可用于访问模拟中的对象，例如天气、车辆、交通信号灯、建筑物和使用其多种方法的地图：

    level = world.get_map()
    
    weather = world.get_weather()
    
    blueprint_library = world.get_blueprint_library()

同步和异步模式
-------

CARLA 具有客户端-服务器架构。 服务器运行模拟。 客户端在模拟中检索信息并请求更改。 本节处理客户端和服务器之间的通信。

默认情况下，CARLA 以异步模式运行。

本质上，在异步模式下，CARLA 服务器尽可能快地运行。 客户请求是即时处理的。 在同步模式下，客户端运行您的 Python 代码，接管并告诉服务器何时更新。

如果您正在试验或设置模拟，异步模式是运行 CARLA 的合适模式，因此您可以在放置演员时与观众一起在地图上飞行。 当您想开始生成训练数据或在模拟中部署代理时，建议您使用同步模式，因为这将为您提供更多控制和可预测性。

### 设置同步模式

在同步和异步模式之间切换只是一个布尔状态的问题。

    settings = world.get_settings()
    settings.synchronous_mode = True # Enables synchronous mode
    settings.fixed_delta_seconds = 0.05
    world.apply_settings(settings)

要禁用同步模式，只需将变量设置为 False 或使用脚本 PythonAPI/util/config.py。

    cd PythonAPI/util && python3 config.py --no-sync # Disables synchronous mode

无法使用脚本启用同步模式，只能禁用。 启用同步模式使服务器等待客户端滴答声。 使用此脚本，用户无法在需要时发送报价。

### 使用同步模式

同步模式与慢速客户端应用程序特别相关，并且当需要不同元素（例如传感器）之间的同步时。 如果客户端太慢，服务器没有等待，就会出现信息溢出。 客户端将无法管理所有内容，并且会丢失或混合。 同样，由于有许多传感器和异步，不可能知道所有传感器是否都在使用模拟中同一时刻的数据。

下面的代码片段扩展了前一个。 客户端创建一个摄像头传感器，将当前步骤的图像数据存储在一个队列中，并在从队列中检索到它之后滴答一下服务器。

    settings = world.get_settings()
    settings.synchronous_mode = True
    world.apply_settings(settings)
    
    camera = world.spawn_actor(blueprint, transform)
    image_queue = queue.Queue()
    camera.listen(image_queue.put)
    
    while True:
        world.tick()
        image = image_queue.get()

这个世界有异步方法让客户端等待服务器滴答声，或者在收到它时做一些事情。

    # Wait for the next tick and retrieve the snapshot of the tick.
    world_snapshot = world.wait_for_tick()
    
    # Register a callback to get called every time we receive a new snapshot.
    world.on_tick(lambda world_snapshot: do_something(world_snapshot))

记录器
---

记录器可以将重现先前模拟所需的所有数据保存到文件中。 这些数据包括车辆的位置和速度、交通信号灯的状态、行人的位置和速度以及太阳的位置和天气状况等详细信息。 数据被记录到一个二进制文件中，该文件可以稍后由 carla 服务器加载，以准确再现模拟。

根据记录文件中包含的数据，每帧更新演员。 当前模拟中出现在录制中的 Actor 将被移动或重新生成以模拟它。 那些没有出现在录音中的将继续他们的方式，就好像什么都没发生一样。

记录器文件包括有关许多不同元素的信息。

*   Actors——创建和销毁、边界和触发框。
*   红绿灯——状态变化和时间设置。
*   车辆——位置和方向、线速度和角速度、光状态和物理控制。
*   行人——位置和方向，以及线速度和角速度。
*   灯光——来自建筑物、街道和车辆的灯光状态。

### 记录

要开始录制，只需要一个文件名。 在文件名中使用 \\、/ 或 : 字符会将其定义为绝对路径。 如果没有详细路径，文件将保存在 CarlaUE4/Saved 中。

    client.start_recorder("/home/carla/recording01.log")

默认情况下，记录器设置为仅存储回放模拟所需的信息。 为了保存前面提到的所有信息，必须在开始录制时配置参数附加数据。

    client.start_recorder("/home/carla/recording01.log", True)

要停止记录也很简单。

    client.stop_recorder()

### 模拟回放

可以在模拟过程中的任何时候开始播放。 除了日志文件的路径，这个方法还需要一些参数。

    client.replay_file("recording01.log", start, duration, camera)

参数

描述

备注

`start`

以秒为单位记录开始模拟的时间。

如果是肯定的，时间将从记录开始计算。  
如果是否定的，将从最后考虑。

`duration`

播放秒数。 

播放结束时，车辆将设置为自动驾驶，行人将停止。

`camera`

相机将聚焦的演员要素的 ID。

将其设置为 0，让视角自由移动。

### 记录文件格式

记录器以 \[本文档\] 中指定的自定义二进制文件格式保存所有数据

渲染
--

CARLA 提供了许多关于渲染质量和效率的选项。 在最基本的层面上，CARLA 提供了两种质量选项，以实现在高规格和低规格硬件上运行并获得最佳结果：

### Epic mode

./CarlaUE4.sh -quality-level=Epic

![Epic mode screenshot](https://carla.readthedocs.io/en/latest/img/rendering_quality_epic.jpg)

_Epic mode屏幕截图_

### Low mode

./CarlaUE4.sh -quality-level=Low

_![Low mode screenshot](https://carla.readthedocs.io/en/latest/img/rendering_quality_low.jpg)_

_Low mode屏幕截图_

CARLA 还提供了暂停渲染或渲染离屏的选项，以便更有效地记录或运行模拟。


