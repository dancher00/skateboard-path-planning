# Skateboard path planning

ROS2 / Gazebo Classic simulation: A* on an occupancy grid, path smoothing and Stanley steering control. ROS package name: `skate`.

## Build

Source your ROS2 installation first. Requires Gazebo Classic with `gazebo_ros` and `gazebo_ros2_control`, ROS2 controllers, `robot_localization`, RViz2, NumPy, PyYAML and OpenCV.

```bash
mkdir -p ~/skate_ws/src
cd ~/skate_ws/src
git clone https://github.com/dancher00/skateboard-path-planning.git
cd ~/skate_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install --packages-select skate
source install/setup.bash
```

## Run

```bash
ros2 launch skate skate_stanley_control_world_frame.launch.py
```

This starts the obstacle course, planner, controller and RViz. Start/goal coordinates and controller settings are in the [launch file](launch/skate_stanley_control_world_frame.launch.py). Defaults: `(-8, -8)` to `(8, 8)`.

The obstacle-course launch uses A*; its `path_type` argument is unused.

## Code

- [A* and smoothing](scripts/path_planning.py)
- [Stanley controller](scripts/stanley_controller.py)
- [Circle, figure-eight and straight test paths](scripts/path_generator.py)
- [Robot model](description/board), [maps](maps), [worlds](worlds)

[Apache 2.0](LICENSE.md)
