Actors and blueprints
=====================

CARLA 中的 Actor 是在模拟中执行动作的元素，它们可以影响其他 Actor。 CARLA 中的参与者包括车辆和步行者，还包括传感器、交通标志、交通灯和观众。 对如何操作它们有充分的了解是至关重要的。

本节将介绍生成、销毁、类型以及如何管理它们。 然而，可能性几乎是无穷无尽的。 实验一下，看看本文档中的教程，在 CARLA 中文论坛分享疑惑和想法。

1.  **蓝图**  
    管理蓝图库
2.  **Actor生命周期**  
    生成  
    处理  
    破坏
3.  **Acotr类型**  
    传感器  
    旁观者视角  
    交通标志和交通灯  
    汽车  
    行人

蓝图
--

这些布局允许用户将新演员顺利融入模拟。 它们是带有动画和一系列属性的已制作模型。 其中一些是可修改的，而另一些则不是。 这些属性包括车辆颜色、激光雷达传感器中的通道数量、步行者的速度等等。

可用的蓝图及其属性列在蓝图库中。 车辆和步行者蓝图有一个世代属性，指示它们是新的（第 2 代）还是旧的（第 1 代）资产。

### 管理蓝图库

carla.BlueprintLibrary 类包含一个 carla.ActorBlueprint 元素列表。 世界对象可以提供对它的访问。

    blueprint_library = world.get_blueprint_library()

蓝图有一个 ID 来识别它们以及由此产生的演员。 可以读取该库以查找特定 ID、随机选择蓝图或使用通配符模式过滤结果。

    # Find a specific blueprint.
    collision_sensor_bp = blueprint_library.find('sensor.other.collision')
    # Choose a vehicle blueprint at random.
    vehicle_bp = random.choice(blueprint_library.filter('vehicle.*.*'))

除此之外，每个 carla.ActorBlueprint 都有一系列 carla.ActorAttribute 可以获取和设置。

    is_bike = [vehicle.get_attribute('number_of_wheels') == 2]
    if(is_bike)
        vehicle.set_attribute('color', '255,0,0')

属性有一个 carla.ActorAttributeType 变量。 它从枚举列表中声明其类型。 此外，可修改属性带有推荐值列表。

    for attr in blueprint:
        if attr.is_modifiable:
            blueprint.set_attribute(attr.id, random.choice(attr.recommended_values))

Actor生命周期
---------

### 生成

世界对象负责生成actors并跟踪它们。 生成只需要一个蓝图和一个 carla.Transform 来说明 Actor 的位置和旋转。

这个世界有两种不同的方法来产生Actor。

*   [`spawn_actor()`](https://carla.readthedocs.io/en/latest/python_api/#carla.World.spawn_actor) raises an exception if the spawning fails.
*   [`try_spawn_actor()`](https://carla.readthedocs.io/en/latest/python_api/#carla.World.try_spawn_actor) returns `None` if the spawning fails.

    transform = Transform(Location(x=230, y=195, z=40), Rotation(yaw=180))
    actor = world.spawn_actor(blueprint, transform)

如果在指定位置发生碰撞，actor 将不会生成。 无论这发生在静态对象还是其他Actor身上。 可以尝试避免这些不希望的生成碰撞。

*   map.get\_spawn\_points() 用于车辆。 返回推荐的生成点列表。

    spawn_points = world.get_map().get_spawn_points()

*   world.get\_random\_location() 用于行人。 返回人行道上的随机点。 同样的方法用于为步行者设置目标位置。

    spawn_point = carla.Transform()
    spawn_point.location = world.get_random_location_from_navigation()​

一个actor在生成时可以附加到另一个actor上。 演员跟随他们所依附的父母。 这对传感器特别有用。 附件可以是刚性的（适合检索精确数据），也可以根据其父项轻松移动。 它由助手类 carla.AttachmentType 定义。

下一个示例将摄像头固定在车辆上，因此它们的相对位置保持固定。

    camera = world.spawn_actor(camera_bp, relative_transform, attach_to=my_vehicle, carla.AttachmentType.Rigid)

生成后，世界对象会将演员添加到列表中。 这可以很容易地搜索或迭代。

    actor_list = world.get_actors()
    # Find an actor by id.
    actor = actor_list.find(id)
    # Print the location of all the speed limit signs in the world.
    for speed_sign in actor_list.filter('traffic.speed_limit.*'):
        print(speed_sign.get_location())

处理
--

carla.Actor 主要由 get() 和 set() 方法组成，用于管理地图周围的演员。

    print(actor.get_acceleration())
    print(actor.get_velocity())
    
    location = actor.get_location()
    location.z += 10.0
    actor.set_location(location)

可以禁用演员的物理特性以将其冻结在适当的位置。

    actor.set_simulate_physics(False)

除此之外，演员也有他们的蓝图提供的标签。 这些对语义分割传感器最有用。

### 破坏

当 Python 脚本完成时，Actor 不会被销毁。 他们必须明确地摧毁自己。

    destroyed_sucessfully = actor.destroy() # Returns True if successful

Actor类型
-------

### 传感器

传感器是产生数据流的参与者。 他们有自己的第 4 部分。 传感器和数据。 现在，让我们看一下常见的传感器生成周期。

此示例生成一个摄像头传感器，将其附加到车辆上，并告诉摄像头将生成的图像保存到磁盘。

    camera_bp = blueprint_library.find('sensor.camera.rgb')
    camera = world.spawn_actor(camera_bp, relative_transform, attach_to=my_vehicle)
    camera.listen(lambda image: image.save_to_disk('output/%06d.png' % image.frame))

*   传感器也有蓝图。 设置属性至关重要。
*   大多数传感器将连接到车辆上以收集有关其周围环境的信息。
*   传感器监听数据。 当接收到数据时，它们调用一个用 Lambda 表达式描述的函数。

### 旁观者

由虚幻引擎放置以提供游戏内视角。 它可以用来移动模拟器窗口的视图。 以下示例将移动旁观者演员，以将视线指向所需的车辆。

    spectator = world.get_spectator()
    transform = vehicle.get_transform()
    spectator.set_transform(carla.Transform(transform.location + carla.Location(z=50),
    carla.Rotation(pitch=-90)))

### 交通标志和交通灯

到目前为止，在 CARLA 中，只有停靠点、让路和红绿灯被认为是演员。 其余的 OpenDRIVE 标志可从 API 作为 carla.Landmark 访问。 使用这些实例可以访问它们的信息，但它们在模拟中不作为参与者存在。 在下面的步骤 3 中更详细地解释了地标。 地图和导航。

使用 OpenDRIVE 文件中的信息自动生成模拟开始、停止、产量和交通信号灯。 这些都无法在蓝图库中找到，因此无法生成。

道路地图本身并未定义交通标志，如下页所述。 相反，他们有一个 carla.BoundingBox 来影响其中的车辆。

    #Get the traffic light affecting a vehicle
    if vehicle_actor.is_at_traffic_light():
        traffic_light = vehicle_actor.get_traffic_light()

交通信号灯出现在路口。 与任何参与者一样，他们有自己的唯一 ID，但也有交汇点的组 ID。 为了识别同一组中的交通灯，使用了杆 ID。

同一组中的红绿灯遵循一个循环。 第一个设置为绿色，而其余的则保持为红色。 活跃的在绿色、黄色和红色上花费几秒钟，所以有一段时间所有的灯都是红色的。 然后，下一个红绿灯开始循环，前一个红绿灯与其他红绿灯一起冻结。

可以使用 API 设置交通灯的状态。 在每个状态上花费的秒数也是如此。 carla.TrafficLightState 将可能的状态描述为一系列枚举值。

    #Change a red traffic light to green
    if traffic_light.get_state() == carla.TrafficLightState.Red:
        traffic_light.set_state(carla.TrafficLightState.Green)
        traffic_light.set_set_green_time(4.0)

### 车辆

carla.Vehicle 是一种特殊类型的演员。 它包含模拟轮式车辆物理特性的特殊内部组件。 这是通过应用四种不同的控件来实现的：

*   carla.VehicleControl 为油门、转向、刹车等驾驶命令提供输入。

        vehicle.apply_control(carla.VehicleControl(throttle=1.0, steer=-1.0))

*   carla.VehiclePhysicsControl 定义了车辆的物理属性并包含另外两个控制器：

*   carla.GearPhysicsControl 控制齿轮。
*   carla.WheelPhysicsControl 提供对每个车轮的特定控制。

      vehicle.apply_physics_control(carla.VehiclePhysicsControl(max_rpm = 5000.0, center_of_mass = carla.Vector3D(0.0, 0.0, 0.0), torque_curve=[[0,400],[5000,400]]))

车辆有一个 carla.BoundingBox 封装它们。 此边界框允许将物理应用到车辆并检测碰撞。

        box = vehicle.bounding_box
        print(box.location)         # Location relative to the vehicle.
        print(box.extent)           # XYZ half-box extents in meters.

通过启用扫描轮碰撞参数可以改善车轮的物理特性。 默认的车轮物理场对每个车轮使用从轴到地板的单射线投射，但是当启用扫描车轮碰撞时，会检查车轮的整个体积以防止碰撞。 它可以这样启用：

        physics_control = vehicle.get_physics_control()
        physics_control.use_sweep_wheel_collision = True
        vehicle.apply_physics_control(physics_control)

车辆包括其他独有的功能：

*   自动驾驶模式将为车辆订阅交通管理器以模拟真实的城市状况。 这个模块是硬编码的，不是基于机器学习的。

        vehicle.set_autopilot(True)

*   车灯必须由用户打开和关闭。 每辆车都有一组在 carla.VehicleLightState 中列出的灯。 并非所有车辆都集成了照明灯。 在撰写本文时，集成车灯的车辆如下：  
    自行车：所有自行车都有前后位置灯。  
    摩托车：雅马哈和哈雷戴维森车型。  
    汽车：奥迪 TT、雪佛兰 Impala、道奇警车、道奇 Charger、奥迪 e-tron、林肯 2017 和 2020、野马、特斯拉 Model 3、特斯拉 Cybertruck、大众 T2 和梅赛德斯 C 级。

可以使用方法 carla.Vehicle.get\_light\_state 和 carla.Vehicle.set\_light\_state 随时检索和更新车辆的灯光。 这些使用二进制操作来自定义灯光设置。

    # Turn on position lights
    current_lights = carla.VehicleLightState.NONE
    current_lights |= carla.VehicleLightState.Position
    vehicle.set_light_state(current_lights)

### 行人

carla.Walker 的工作方式与车辆类似。 对它们的控制由控制器提供。

*   carla.WalkerControl 以一定的方向和速度移动行人。 它还允许他们跳跃。
*   carla.WalkerBoneControl 提供对 3D 骨架的控制。 本教程解释了如何控制它。

步行者可以由人工智能控制。 他们没有自动驾驶模式。 carla.WalkerAIController Actor 围绕它所连接的 Actor 移动。

    walker_controller_bp = world.get_blueprint_library().find('controller.ai.walker')
    world.SpawnActor(walker_controller_bp, carla.Transform(), parent_walker)

每个 AI 控制器都需要初始化、目标和速度（可选）。 停止控制器的工作方式相同。

    ai_controller.start()
    ai_controller.go_to_location(world.get_random_location_from_navigation())
    ai_controller.set_max_speed(1 + random.random())  # Between 1 and 2 m/s (default is 1.4 m/s).
    ...
    ai_controller.stop()

当行人到达目标位置时，他们会自动步行到另一个随机点。 如果无法到达目标点，步行者将前往离他们当前位置最近的点。

carla.Client 中的一个片段使用批量生成大量步行者并让他们四处游荡。


