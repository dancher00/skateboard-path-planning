# Skateboard path planning and control

ROS2 simulation package for planning and tracking skateboard trajectories in Gazebo. The package is named **`skate`**.

The obstacle-course example combines occupancy-grid A* planning, iterative path smoothing, and a Stanley steering controller. RViz displays the planned path and the trajectory reconstructed from odometry.

## Code map

| Component | Implementation |
| --- | --- |
| Grid planning and smoothing | [scripts/path_planning.py](scripts/path_planning.py) |
| Stanley path tracking | [scripts/stanley_controller.py](scripts/stanley_controller.py) |
| Analytic test paths | [scripts/path_generator.py](scripts/path_generator.py) |
| Odometry visualization and TF | [scripts/odom_to_path.py](scripts/odom_to_path.py), [scripts/odom_tf_broadcaster.py](scripts/odom_tf_broadcaster.py) |
| Robot model | [description/board](description/board) |
| Simulation scenes and maps | [worlds](worlds), [maps](maps) |
| ROS2 controller configuration | [config/controller_config.yaml](config/controller_config.yaml) |

## Environment

Use a ROS2 installation with **Gazebo Classic** and `gazebo_ros` / `gazebo_ros2_control`. These launch files use the Classic interfaces.

Runtime components include ROS2 control and controllers, `robot_state_publisher`, `xacro`, `tf2_ros`, `robot_localization`, and RViz2. The planner uses NumPy, PyYAML, and OpenCV. See [package.xml](package.xml) for declared ROS dependencies; additional runtime dependencies may need to be installed separately in an existing environment.

The repository does not record a fully pinned ROS distribution and simulator environment.

## Build

After sourcing your ROS2 installation, create or use a colcon workspace:

```bash
mkdir -p ~/skate_ws/src
cd ~/skate_ws/src
git clone https://github.com/dancher00/skateboard-path-planning.git
cd ~/skate_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install --packages-select skate
source install/setup.bash
```

## Run the obstacle-course example

```bash
ros2 launch skate skate_stanley_control_world_frame.launch.py
```

The [launch file](launch/skate_stanley_control_world_frame.launch.py) starts Gazebo, spawns the board near `(-8, -8)`, and configures a path to `(8, 8)` using the included obstacle map. It also starts the controller, odometry/TF nodes, and RViz.

To change the experiment, edit the planner's start/goal and map parameters in that launch file, and the Stanley controller parameters in the same file. The `path_type` argument is not connected to the active planner in this obstacle-course launch.

For the separate analytic-path setup, see [skate_stanley_control.launch.py](launch/skate_stanley_control.launch.py) and [path_generator.py](scripts/path_generator.py).

## Scope

This is a simulation research prototype. It does not include a hardware deployment procedure or a benchmark of tracking error and collision rates. Smoothing and tracking should be evaluated against the chosen map and robot geometry before drawing performance conclusions.

## Related work and license

[Quadruped skateboard mounting](https://github.com/dancher00/quadruped-skateboard-mounting) · [Skateboarding demonstrations](https://github.com/dancher00/quadrupedal-skateboarding)

See [LICENSE.md](LICENSE.md) and source-file notices.
