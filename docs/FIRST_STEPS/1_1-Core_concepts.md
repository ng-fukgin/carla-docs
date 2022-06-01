核心概念
====

本页介绍了 CARLA 的主要功能和模块。 不同主题的详细说明可在其相应页面中找到。

要了解 API 中的不同类和方法，请查看 [Python API 参考](https://carla.readthedocs.io/en/latest/python_api/)。

*   [**First steps**](https://carla.readthedocs.io/en/latest/core_concepts/#first-steps)
    *   [1st- World and client](https://carla.readthedocs.io/en/latest/core_concepts/#1st-world-and-client)
    *   [2nd- Actors and blueprints](https://carla.readthedocs.io/en/latest/core_concepts/#2nd-actors-and-blueprints)
    *   [3rd- Maps and navigation](https://carla.readthedocs.io/en/latest/core_concepts/#3rd-maps-and-navigation)
    *   [4th- Sensors and data](https://carla.readthedocs.io/en/latest/core_concepts/#4th-sensors-and-data)
*   [**Advanced steps**](https://carla.readthedocs.io/en/latest/core_concepts/#advanced-steps)

Important

**本文档涉及的版本为CARLA 0.9.X**  
The API changed significantly from previous versions (0.8.X). There is another documentation regarding those versions that can be found [here](https://carla.readthedocs.io/en/stable/getting_started/).API 与以前的版本 (0.8.X) 相比发生了显着变化。 有关这些版本的其他文档可在[此处](https://carla.readthedocs.io/en/stable/getting_started/)找到。

* * *

第一步
---

### 1st- 世界和客户端

**客户端**是用户运行以在模拟中询问信息或更改的模块。 客户端使用 IP 和特定端口运行。 它通过终端与服务器通信。 可以有多个客户端同时运行。 高级多客户端管理需要彻底了解 CARLA 和[同步](https://carla.readthedocs.io/en/latest/adv_synchrony_timestep/)。

**世界**是代表模拟的对象。 它作为一个抽象层，包含生成actors、改变天气、获取世界当前状态等的主要方法。每个模拟只有一个世界。 当地图改变时，它将被销毁并替换为新的。

### 2nd- 元素和蓝图

**元素**是在模拟中扮演角色的任何东西。

*   车辆 Vehicles
*   行人 Walkers
*   传感器 Sensors
*   旁观者 The spectator
*   交通标识与交通信号灯 Traffic signs and traffic lights

**蓝图**是生成元素所必需的已经制作好的布局。 基本上是具有动画和属性的一组模型。 其中一些属性可以由用户自定义，而另一些则不能。 CARLA在[Blueprints Library蓝图库](https://carla.readthedocs.io/en/latest/bp_library/)中包含所有可用的蓝图及其相关信息。

### 3rd- 地图与导航

**地图**是代表模拟世界的对象，主要是城镇。 有八张地图可用。 它们都使用 OpenDRIVE 1.4 标准来描述道路。

**道路、车道和路口**由 [Python API](https://carla.readthedocs.io/en/latest/python_api/) 管理，可从客户端访问。 这些与**Waypoint**一起使用，为车辆提供导航路径。

**交通标志**和**交通信号灯**可作为 [carla.Landmark](https://carla.readthedocs.io/en/latest/core_concepts/#python_api.md#carla.landmark) 对象访问，其中包含有关其 OpenDRIVE 定义的信息。 此外，模拟器在使用 OpenDRIVE 文件中的信息运行时会自动生成停止、让路和交通灯对象。 这些在道路上放置了边界框。 一旦进入边界框，车辆就会意识到它们。

### 4th- 传感器与数据

**传感器**等待某个事件发生，然后从模拟中收集数据。 它们需要一个函数来定义如何管理数据。 根据具体情况，传感器会检索不同类型的**传感器数据**。

传感器是连接到父车辆的参与者。 它跟随车辆四处走动，收集周围的信息。 可用的传感器由它们在[Blueprint library蓝图库](https://carla.readthedocs.io/en/latest/bp_library/)中的蓝图定义。

*   摄像头 (RGB摄像头、深度摄像头、语义分割摄像头)；
*   碰撞检测器；
*   GNSS导航；
*   IMU传感器；
*   激光雷达；
*   车道入侵检测器；
*   障碍物检测器；
*   雷达；
*   RSS

* * *

更进一步
----

CARLA 提供了一系列超出模拟器介绍范围的功能。 这里列出了一些最引人注目的。 但是，我们强烈建议您在开始高级步骤之前阅读整个“第一步”部分。

*   [**OpenDRIVE standalone mode**](https://carla.readthedocs.io/en/latest/adv_opendrive/). 仅使用 OpenDRIVE 文件生成道路网格。 允许将任何 OpenDRIVE 地图加载到 CARLA 中，而无需创建资产。
*   [**PTV-Vissim co-simulation**](https://carla.readthedocs.io/en/latest/adv_ptv/). 在 CARLA 和 PTV-Vissim 交通模拟器之间运行同步模拟。
*   [**Recorder**](https://carla.readthedocs.io/en/latest/adv_recorder/). 保存模拟状态的快照，以精确地重新制定模拟。
*   [**Rendering options**](https://carla.readthedocs.io/en/latest/adv_rendering_options/). 图形质量设置、离屏渲染和非渲染模式。
*   [**RSS**](https://carla.readthedocs.io/en/latest/adv_rss/). 集成责任敏感安全 C++ 库，以使用安全检查修改车辆的轨迹。
*   [**Simulation time and synchrony**](https://carla.readthedocs.io/en/latest/adv_synchrony_timestep/). 关于模拟时间和服务器-客户端通信的一切。
*   [**SUMO co-simulation**](https://carla.readthedocs.io/en/latest/adv_sumo/). 在 CARLA 和 SUMO 交通模拟器之间运行同步模拟。
*   [**Traffic manager**](https://carla.readthedocs.io/en/latest/adv_traffic_manager/). 该模块负责每辆设置为自动驾驶模式的车辆。 它模拟城市中的交通，使模拟看起来像真实的城市环境。

* * *

**这是对 CARLA 基础知识的总结。 下一步将更仔细地了解这个世界以及连接到它的客户。**



