# UWB Plugin - Panther Robot Example Package

## Overview

This ROS 2 package is an example package generated to demonstrate the usage of the [UWB plugin](https://github.com/jacobrejin/uwb_plugin) with urdf files. It uses the panther robot for the simulation and the UWB tag is attached to the robot. The plugin tag is also embeddded in the urdf file.


## Prerequisites

- ROS 2 Humble
- colcon build system
- Panther_ros workspace 

## Installation

### Step 1: Setup ROS 2 Humble

Ensure you have ROS 2 Humble installed on your system. Follow the instructions from the [ROS 2 installation guide](https://docs.ros.org/en/humble/Installation.html) if you don't have it installed.

### Step 2: Setup the Panther Robot Underlay Workspace

The Panther robot has its own underlay workspace which needs to be sourced before building this package. (You can follow the guide on the [panthers repo](https://github.com/husarion/panther_ros/tree/ros2-devel), make sure you are using the ros2_devel branch)

You can follow the steps provided by the panther team to setup the workspace. 
Once setup make sure you can source the workspace by running the following command:

```bash
source ~/panther_ws/install/setup.bash
```
and check if you are able to run the panther robot simulation by running the following command:

```bash
ros2 launch panther_gazebo simulation.launch.py
```

### Step 3: Clone and Build this Package

If you underatnd the concept of underlays and overlays, you can go ahead and build the package as per your liking. Else you can follow the below folder structure to build the package.

```
├── ws
│   ├── husarion_ws
│   │   ├── build
│   │   ├── install
│   │   ├── log
│   │   └── src
│   └── uwb_plugin_test_ws
│       └── src
```

1. Build an empty workspace (uwb_plugin_test_ws) for this package:
    
    From the ws folder do the following commands:

    ```bash
    cd uwb_plugin_test_ws
    mkdir src
    colcon build
    ```

    This will create a workspace with the necessary folders.

    ```
    ├── ws
    │   ├── husarion_ws
    │   │   ├── build
    │   │   ├── install
    │   │   ├── log
    │   │   └── src
    │   └── uwb_plugin_test_ws
    │       ├── build
    │       ├── install
    │       ├── log
    │       └── src
    ```


2. Clone this package repository:

    ```bash
    cd src
    git clone https://github.com/yourusername/panther_pkg.git
    ```

3. Build the workspace:

    ```bash
    cd ../
    colcon build
    ```

4. Source the new workspace:

    ```bash
    source install/setup.bash
    ```

## Usage

### Running the test lauch file

To run the luanch file provided by this package, run the following command:

```bash
ros2 launch plugin_test plugin_test.launch.py 
```


## Important Notes

1. For the plugin to work, the path to the world and the plugin folder should be provided in the `plugin_test.launch.py` file. This is crucial for the example to work correctly.

    ```python
    return LaunchDescription(
            [
                AppendEnvironmentVariable(name='GZ_SIM_RESOURCE_PATH', value = PathJoinSubstitution([FindPackageShare("plugin_test"),"world"])),
                SetEnvironmentVariable(name='GZ_SIM_SYSTEM_PLUGIN_PATH', value = PathJoinSubstitution([FindPackageShare("plugin_test"),"plugin"])),
    ...
    ```

2. There are certain files in which the package name is hardocded, and might need to be modified if you are editing or reusig the code.

    - `plugin_test/launch/plugin_test.launch.py`
    - `plugin_test/launch/controller.launch.py`
    - `plugin_test/launch/bringup.launch.py`
    - `plugin_test/urdf/panther.urdf.xacro`
    - `plugin_test/plugin/uwb_tag.urdf.xacro`

3. In the launch files you should find a variable `current_package = "plugin_test"` which might need to be changed to your package name.

4. In the `uwb_tag.urdf.xacro` file you might need to change the package name in the following line:
    ```xml
    <mesh filename="$(find plugin_test)/world/models/uwb_tag/meshes/tag_uwb.stl"/>
    ```

5. In the `panther.urdf.xacro` file you might need to change the package name in the following line:
    ```xml
      <xacro:include filename="$(find plugin_test)/urdf/uwb_tag.urdf.xacro" ns="uwb_tag" />
    ```


