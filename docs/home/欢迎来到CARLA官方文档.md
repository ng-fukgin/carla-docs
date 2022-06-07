

CARLA 官方文档
==========

Welcome to the CARLA documentation.

This home page contains an index with a brief description of the different sections in the documentation. Feel free to read in whatever order preferred. In any case, here are a few suggestions for newcomers.

*   **Install CARLA.** Either follow the [Quick start installation](https://carla.readthedocs.io/en/latest/start_quickstart/) to get a CARLA release or [make the build](https://carla.readthedocs.io/en/latest/build_linux/) for a desired platform.
*   **Start using CARLA.** The section titled [First steps](https://carla.readthedocs.io/en/latest/core_concepts/) is an introduction to the most important concepts.
*   **Check the API.** there is a handy [Python API reference](https://carla.readthedocs.io/en/latest/python_api/) to look up the classes and methods available.

The CARLA forum is available to post any doubts or suggestions that may arise during the reading.

[CARLA forum](https://github.com/carla-simulator/carla/discussions/ "Go to the latest CARLA release")

Warning

**Change the docs version to fit the CARLA version you are using**. Use the panel in the bottom-right side of this window to change to previous versions. **The _Latest_ version points to documentation in the `dev` branch** which may refer to features currently in development and **not available** in any packaged version of CARLA, as well general documentation improvements. ![docs_version_panel](https://carla.readthedocs.io/en/latest/img/docs_version_panel.jpg)

* * *

Getting started
---------------

[**Introduction**](https://carla.readthedocs.io/en/latest/start_introduction/) — What to expect from CARLA.  
[**Quick start package installation**](https://carla.readthedocs.io/en/latest/start_quickstart/) — Get the CARLA releases.

Building CARLA
--------------

[**Linux build**](https://carla.readthedocs.io/en/latest/build_linux/) — Make the build on Linux.  
[**Windows build**](https://carla.readthedocs.io/en/latest/build_windows/) — Make the build on Windows.  
[**Update CARLA**](https://carla.readthedocs.io/en/latest/build_update/) — Get up to date with the latest content.  
[**Build system**](https://carla.readthedocs.io/en/latest/build_system/) — Learn about the build and how it is made.  
[**CARLA in Docker**](https://carla.readthedocs.io/en/latest/build_docker/) — Run CARLA using a container solution.  
[**F.A.Q.**](https://carla.readthedocs.io/en/latest/build_faq/) — Some of the most frequent installation issues.

First steps
-----------

[**Core concepts**](https://carla.readthedocs.io/en/latest/core_concepts/) — Overview of the basic concepts in CARLA.  
[**1st. World and client**](https://carla.readthedocs.io/en/latest/core_world/) — Manage and access the simulation.  
[**2nd. Actors and blueprints**](https://carla.readthedocs.io/en/latest/core_actors/) — Learn about actors and how to handle them.  
[**3rd. Maps and navigation**](https://carla.readthedocs.io/en/latest/core_map/) — Discover the different maps and how do vehicles move around.  
[**4th. Sensors and data**](https://carla.readthedocs.io/en/latest/core_sensors/) — Retrieve simulation data using sensors.

Advanced concepts
-----------------

[**OpenDRIVE standalone mode**](https://carla.readthedocs.io/en/latest/adv_opendrive/) — Use any OpenDRIVE file as a CARLA map.  
[**PTV-Vissim co-simulation**](https://carla.readthedocs.io/en/latest/adv_ptv/) — Run a synchronous simulation between CARLA and PTV-Vissim.  
[**Recorder**](https://carla.readthedocs.io/en/latest/adv_recorder/) — Register the events in a simulation and play it again.  
[**Rendering options**](https://carla.readthedocs.io/en/latest/adv_rendering_options/) — From quality settings to no-render or off-screen modes.  
[**RSS**](https://carla.readthedocs.io/en/latest/adv_rss/) — An implementation of RSS in the CARLA client library.  
[**Synchrony and time-step**](https://carla.readthedocs.io/en/latest/adv_synchrony_timestep/) — Client-server communication and simulation time.  
[**Benchmarking Performance**](https://carla.readthedocs.io/en/latest/adv_benchmarking/) — Perform benchmarking using our prepared script.  
[**CARLA Agents**](https://carla.readthedocs.io/en/latest/adv_agents/) — Agents scripts allow single vehicles to roam the map or drive to a set destination.

Traffic Simulation
------------------

[**Traffic Simulation Overview**](https://carla.readthedocs.io/en/latest/ts_traffic_simulation_overview/) — An overview of the different options available to populate your scenes with traffic  
[**Traffic Manager**](https://carla.readthedocs.io/en/latest/adv_traffic_manager/) — Simulate urban traffic by setting vehicles to autopilot mode.  
[**SUMO co-simulation**](https://carla.readthedocs.io/en/latest/adv_sumo/) — Run a synchronous simulation between CARLA and SUMO.  
[**Scenic**](https://carla.readthedocs.io/en/latest/tuto_G_scenic/) — Follow an example of defining different scenarios using the Scenic library.

References
----------

[**Python API reference**](https://carla.readthedocs.io/en/latest/python_api/) — Classes and methods in the Python API.  
[**Blueprint library**](https://carla.readthedocs.io/en/latest/bp_library/) — Blueprints provided to spawn actors.  
[**C++ reference**](https://carla.readthedocs.io/en/latest/ref_cpp/) — Classes and methods in CARLA C++.  
[**Recorder binary file format**](https://carla.readthedocs.io/en/latest/ref_recorder_binary_file_format/) — Detailed explanation of the recorder file format.  
[**Sensors reference**](https://carla.readthedocs.io/en/latest/ref_sensors/) — Everything about sensors and the data they retrieve.

Plugins
-------

[**carlaviz — web visualizer**](https://carla.readthedocs.io/en/latest/plugins_carlaviz/) — Plugin that listens the simulation and shows the scene and some simulation data in a web browser.

ROS bridge
----------

[**ROS bridge documentation**](https://carla.readthedocs.io/en/latest/ros_documentation/) — Brief overview of the ROS bridge and a link to the full documentation

Custom Maps
-----------

[**Overview of custom maps in CARLA**](https://carla.readthedocs.io/en/latest/tuto_M_custom_map_overview/) — An overview of the process and options involved in adding a custom, standard sized map  
[**Create a map in RoadRunner**](https://carla.readthedocs.io/en/latest/tuto_M_generate_map/) — How to generate a customs, standard sized map in RoadRunner  
[**Import map in CARLA package**](https://carla.readthedocs.io/en/latest/tuto_M_add_map_package/) How to import a map in a CARLA package  
[**Import map in CARLA source build**](https://carla.readthedocs.io/en/latest/tuto_M_add_map_source/) — How to import a map in CARLA built from source  
[**Alternative ways to import maps**](https://carla.readthedocs.io/en/latest/tuto_M_add_map_alternative/) — Alternative methods to import maps  
[**Manually prepare map package**](https://carla.readthedocs.io/en/latest/tuto_M_manual_map_package/) — How to prepare a map for manual import  
[**Customizing maps: Layered maps**](https://carla.readthedocs.io/en/latest/tuto_M_custom_layers/) — How to create sub-layers in your custom map  
[**Customizing maps: Traffic lights and signs**](https://carla.readthedocs.io/en/latest/tuto_M_custom_add_tl/) — How to add traffic lights and signs to your custom map  
[**Customizing maps: Road painter**](https://carla.readthedocs.io/en/latest/tuto_M_custom_road_painter/) — How to use the road painter tool to change the apearance of the road  
[**Customizing Maps: Procedural Buildings**](https://carla.readthedocs.io/en/latest/tuto_M_custom_buildings/) — Populate your custom map with buildings  
[**Customizing maps: Weather and landscape**](https://carla.readthedocs.io/en/latest/tuto_M_custom_weather_landscape/) — Create the weather profile for your custom map and populate the landscape  
[**Generate pedestrian navigation**](https://carla.readthedocs.io/en/latest/tuto_M_generate_pedestrian_navigation/) — Obtain the information needed for walkers to move around.

Large Maps
----------

[**Large maps overview**](https://carla.readthedocs.io/en/latest/large_map_overview/) — An explanation of how large maps work in CARLA  
[**Create a Large Map in RoadRunner**](https://carla.readthedocs.io/en/latest/large_map_roadrunner/) — How to create a large map in RoadRunner  
[**Import/Package a Large Map**](https://carla.readthedocs.io/en/latest/large_map_import/) — How to import a large map

Tutorials — General
-------------------

[**Add friction triggers**](https://carla.readthedocs.io/en/latest/tuto_G_add_friction_triggers/) — Define dynamic box triggers for wheels.  
[**Control vehicle physics**](https://carla.readthedocs.io/en/latest/tuto_G_control_vehicle_physics/) — Set runtime changes on a vehicle physics.  
[**Control walker skeletons**](https://carla.readthedocs.io/en/latest/tuto_G_control_walker_skeletons/) — Animate walkers using skeletons.  
[**Generate maps with OpenStreetMap**](https://carla.readthedocs.io/en/latest/tuto_G_openstreetmap/) — Use OpenStreetMap to generate maps for use in simulations.  
[**Retrieve simulation data**](https://carla.readthedocs.io/en/latest/tuto_G_retrieve_data/) — A step by step guide to properly gather data using the recorder.  
[**CarSim Integration**](https://carla.readthedocs.io/en/latest/tuto_G_carsim_integration/) — Tutorial on how to run a simulation using the CarSim vehicle dynamics engine.  
[**RLlib Integration**](https://carla.readthedocs.io/en/latest/tuto_G_rllib_integration/) — Find out how to run your own experiment using the RLlib library.  
[**Chrono Integration**](https://carla.readthedocs.io/en/latest/tuto_G_chrono/) — Use the Chrono integration to simulation physics  
[**Build Unreal Engine and CARLA in Docker**](https://carla.readthedocs.io/en/latest/build_docker_unreal/) — Build Unreal Engine and CARLA in Docker

Tutorials — Assets
------------------

[**Add a new vehicle**](https://carla.readthedocs.io/en/latest/tuto_A_add_vehicle/) — Prepare a vehicle to be used in CARLA.  
[**Add new props**](https://carla.readthedocs.io/en/latest/tuto_A_add_props/) — Import additional props into CARLA.  
[**Create standalone packages**](https://carla.readthedocs.io/en/latest/tuto_A_create_standalone/) — Generate and handle standalone packages for assets.  
[**Material customization**](https://carla.readthedocs.io/en/latest/tuto_A_material_customization/) — Edit vehicle and building materials.

Tutorials — Developers
----------------------

[**How to upgrade content**](https://carla.readthedocs.io/en/latest/tuto_D_contribute_assets/) — Add new content to CARLA.  
[**Create a sensor**](https://carla.readthedocs.io/en/latest/tuto_D_create_sensor/) — Develop a new sensor to be used in CARLA.  
[**Create semantic tags**](https://carla.readthedocs.io/en/latest/tuto_D_create_semantic_tags/) — Define customized tags for semantic segmentation.  
[**Customize vehicle suspension**](https://carla.readthedocs.io/en/latest/tuto_D_customize_vehicle_suspension/) — Modify the suspension system of a vehicle.  
[**Generate detailed colliders**](https://carla.readthedocs.io/en/latest/tuto_D_generate_colliders/) — Create detailed colliders for vehicles.  
[**Make a release**](https://carla.readthedocs.io/en/latest/tuto_D_make_release/) — How to make a release of CARLA

CARLA Ecosystem
---------------

[**Ansys Real Time Radar Model**](https://carla.readthedocs.io/en/latest/ecosys_ansys/) — Details about the Ansys RTR Webinair

Contributing
------------

[**Contribution guidelines**](https://carla.readthedocs.io/en/latest/cont_contribution_guidelines/) — The different ways to contribute to CARLA.  
[**Code of conduct**](https://carla.readthedocs.io/en/latest/cont_code_of_conduct/) — Standard rights and duties for contributors.  
[**Coding standard**](https://carla.readthedocs.io/en/latest/cont_coding_standard/) — Guidelines to write proper code.  
[**Documentation standard**](https://carla.readthedocs.io/en/latest/cont_doc_standard/) — Guidelines to write proper documentation.


