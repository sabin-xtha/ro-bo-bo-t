# RO-BO-BO-T
This repo consists of the ros based differential robot that is able to navigate in a closed environment.

## Run following Commands
Clone the project inside `src` directory of your ros2 workspace and build the project.
```sh
cd path/to/the/workspace
source /opt/ros/humble/setup.bash
colcon build --symlink-install
source install/setup.bash
```
Run **Robot** launch file (this command publish robot descriptiona and launch gazebo)
```sh
ros2 launch ro-bo-bo-t launch_sim.launch.py
```
Navigate Robot using **keyboard**
```sh
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```
Launch **slam_toolbox** for mapping
```sh
ros2 launch slam_toolbox online_async_launch.py param_file:=./src/ro-bo-bo-t/config/mapper_params_online_async.yaml use_sim_time:=true
```

After mapping we can save the map and run nav2 stack for validating map and localization.\
For **localization** using nav2
Load map and run map_server
```sh
ros2 run nav2_map_server map_server --ros-args -p yaml_filename:=./src/ro-bo-bo-t/maps/map_save.yaml -p use_sim_time:=true
ros2 run nav2_util lifecycle_bringup map_server
```
Launch amcl for localization
```sh
ros2 run nav2_amcl amcl --ros-args -p use_sim_time:=true
ros2 run nav2_util lifecycle_bringup amcl
```

OR launch following files:(load map, create costmap)
```sh
ros2 launch nav2_bringup localization_launch.py map:=./src/ro-bo-bo-t/map/map_save.yaml use_sim_time:=true
```
(AMCL for localization)
```sh
ros2 launch nav2_bringup navigation_launch.py use_sim_time:=true map_subscribe_transient_local:=true
```