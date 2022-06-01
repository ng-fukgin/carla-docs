First steps with CARLA

GuardStrike

2022.05.05 10:18发布于[CARLA帮助资源](/document/34876387b64845899a948fbf8c2a73be)

370

CARLA 模拟器是一种综合解决方案，用于为自动驾驶 (AD) 应用和其他机器人应用生成合成训练数据。 CARLA 模拟一个高度逼真的环境，模拟现实世界的城镇、城市和高速公路以及占据这些驾驶空间的车辆和其他物体。

CARLA 模拟器可进一步用作评估和测试环境。您可以部署您在模拟中训练的 AD 代理，以测试和评估它们的性能和安全性，所有这些都在模拟环境的安全范围内，对硬件或其他道路使用者没有风险。

在本教程中，我们将介绍 CARLA 中的标准工作流程，从启动服务器和连接客户端，到添加车辆、传感器和生成用于机器学习的训练数据。本教程旨在简化细节，并尽可能高效地完成使用 CARLA 生成机器学习训练数据的关键步骤。有关工作流程每个部分的更多详细信息，例如蓝图库中可用的大量车辆或可用的替代传感器类型，请查阅文本中的链接或浏览左侧菜单。

*   启动 CARLA
*   加载地图
*   观众导航
*   添加 NPC
*   添加传感器
*   动画车辆

启动 CARLA 并连接客户端
---------------

CARLA 可以使用 Windows 中的可执行文件或 Linux 中的 shell 脚本使用命令行启动。 按照 Linux 和 Windows 的安装说明，然后从命令行启动 CARLA：

    cd /carla/root
    ./CarlaUE4.sh

要通过 Python API 操作 CARLA，我们需要通过开放端口将 Python 客户端连接到服务器。 客户端通过客户端和世界对象来控制模拟器打开一个Python notebook或者新建一个脚本，然后在脚本的开头或者main函数中添加如下代码：

    import carla
    import random
    
    # Connect to the client and retrieve the world object
    client = carla.Client('localhost', 2000)
    world = client.get_world()

客户端对象用于维护客户端与服务器的连接，并具有许多用于应用命令和加载或导出数据的功能。 我们可以使用客户端对象加载替代地图或重新加载当前地图（重置为初始状态）。

该端口可以选择为任何可用端口，默认设置为 2000，您也可以使用计算机的 IP 地址选择与 localhost 不同的主机。 这样，CARLA 服务器可以在联网的机器上运行，而 python 客户端可以在个人计算机上运行。 这对于区分用于运行 CARLA 模拟器的 GPU 和用于神经网络训练的 GPU 特别有用，这两者都对图形硬件要求很高。

加载地图
----

在 CARLA API 中，世界对象提供对模拟所有元素的访问，包括地图、地图内的对象，如建筑物、交通信号灯、车辆和行人。 CARLA 服务器通常会加载默认地图（通常是 Town10）。 如果要使用备用地图启动 CARLA，请使用 `config.py` 脚本：

    ./config.py --map Town05 

我们还可以使用 world 对象从客户端加载地图：

    world.load_world('Town05')

旁观导航
----

旁观视角是模拟的视图。 默认情况下，当您在带有屏幕的计算机上运行 CARLA 服务器时，旁观者会在新窗口中打开，除非您指定 -RenderOffScreen 命令行选项。

旁观者有助于可视化您的模拟。 使用旁观者，您可以熟悉已加载的地图，并查看您所做的任何更改的结果，例如添加车辆、更改天气、打开/关闭地图的各个图层以及用于调试目的。

您可以使用鼠标控制观众视图的俯仰和偏航以及使用 QWE-ASD 键移动观众，让观众在世界各地飞行：

Q - 向上移动（朝向窗口的顶部边缘）  
E - 向下移动（朝向窗口的下边缘）

W——前进

S - 向后移动  
A - 向左移动  
D - 向右移动

在观众窗口中左键单击并拖动鼠标上下控制俯仰，左右拖动控制偏航。

![flying_spectator](https://carla.readthedocs.io/en/latest/img/tuto_G_getting_started/flying_spectator.gif)

可以通过 Python API 访问和操作观众及其属性：

    # Retrieve the spectator object
    spectator = world.get_spectator()
    
    # Get the location and rotation of the spectator through its transform
    transform = spectator.get_transform()
    
    location = transform.location
    rotation = transform.rotation
    
    # Set the spectator with an empty transform
    spectator.set_transform(carla.Transform())
    # This will set the spectator at the origin of the map, with 0 degrees
    # pitch, yaw and roll - a good way to orient yourself in the map
    

增加NPC
-----

现在我们已经加载了地图并且服务器已经启动并运行了，我们现在需要用一些车辆来填充我们的模拟，以模拟一个真实的环境，其中包括交通和其他道路使用者或非玩家角色 (NPC)。

要生成车辆，首先，我们需要从蓝图库中选择我们想要的车辆。

    # Get the blueprint library and filter for the vehicle blueprints
    vehicle_blueprints = world.get_blueprint_library().filter('*vehicle*')

现在我们有了蓝图，我们需要在地图上找到一些合适的地点来生成我们的车辆。 为此，每张 CARLA 地图都提供了预定义的生成点，这些点均匀分布在道路上的整个地图上。

    # Get the map's spawn points
    spawn_points = world.get_map().get_spawn_points()
    
    # Spawn 50 vehicles randomly distributed throughout the map 
    # for each spawn point, we choose a random vehicle from the blueprint library
    for i in range(0,50):
        world.try_spawn_actor(random.choice(vehicle_blueprints, random.choice(spawn_points)))

现在我们还应该添加一个车辆，它将成为我们模拟的中心点。 要训练自主代理，我们需要模拟自主代理将控制的车辆。 在 CARLA 的说法中，我们经常将这种车辆称为“自我车辆”。

    ego_vehicle = world.spawn_actor(random.choice(vehicle_blueprints), random.choice(spawn_points))

除了车辆，CARLA 还提供行人添加到模拟中，以模拟真实的驾驶场景。 车辆和行人在 CARLA 用语中被称为演员。

增加传感器
-----

现代自动驾驶汽车通过一系列附加传感器了解和解释其环境。 这些传感器包括光学摄像机、光流摄像机、激光雷达、雷达和加速度计等。 CARLA 内置了多种类型的传感器模型，用于为机器学习创建训练数据。 传感器可以连接到车辆上，也可以连接到固定点以模拟闭路电视摄像机。

在这里，我们将在 ego 车辆上附加一个标准的摄像头传感器来记录一些视频数据：

    # Create a transform to place the camera on top of the vehicle
    camera_init_trans = carla.Transform(carla.Location(z=1.5))
    
    # We create the camera through a blueprint that defines its properties
    camera_bp = world.get_blueprint_library().find('sensor.camera.rgb')
    
    # We spawn the camera and attach it to our ego vehicle
    camera = world.spawn_actor(camera_bp, camera_init_trans, attach_to=ego_vehicle)

一旦我们生成了相机，我们需要通过 listen() 方法将其设置为录制。 listen 方法将一个回调作为参数，该回调定义了如何处理数据。 您可以将其流式传输到另一个程序或将其保存到磁盘。

我们将使用 lambda 函数作为回调将数据保存到磁盘：

    # Start camera with PyGame callback
    camera.listen(lambda image: image.save_to_disk('out/%06d.png' % image.frame))

这会将数据保存到 out/ 文件夹中，作为根据模拟帧编号命名的一系列 PNG 图像文件。

有多种不同类型的传感器可供选择。 在这里，您可以深入研究可用的传感器阵列以及如何使用它们。

使用交通管理器为车辆设置动画
--------------

现在我们已经将我们的交通和自我车辆添加到模拟中并开始记录摄像头数据，我们现在需要使用交通管理器设置车辆的运动。 交通管理器是 CARLA 的一个组件，它控制车辆在模拟中自动绕着地图的道路移动，遵循道路惯例并表现得像真正的道路使用者。

我们可以使用 world.get\_actors() 方法找到模拟中的所有车辆，过滤所有车辆。 然后我们可以使用 set\_autopilot() 方法将车辆的控制权交给交通管理器。

    for vehicle in world.get_actors().filter('vehicle'):
        vehicle.set_autopilot(True)

现在您的模拟正在运行，许多车辆在地图上行驶，摄像头记录其中一辆车辆的数据。 然后，这些数据可用于提供机器学习算法以训练自动驾驶代理。 交通管理器具有许多自定义交通行为的功能。


