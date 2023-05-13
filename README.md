# armpi_fpv_fyp
Overview of A1149-221 using ArmPi-FPV, Realsense D405

## Setup

### Libraries used
- MoveIt!
```
sudo apt-get install ros-melodic-moveit
sudo apt-get install ros-melodic-moveit-visual-tools
```
- PCL-ROS
-trac-ik
```
sudo apt-get install ros-melodic-trac-ik-kinematics-plugin
```

### Procedure
#### Setting up alignment-prerejective pipeline
- Download dependencies
- Ask for access for alignment project folder
- Run

#### Setting up robotic arm
- Download dependencies
- Copy and paste the armpi_fpv folder from thumbdrive provided by Hiwonder into your Ubuntu system
- If Hiwonder thumbdrive cannot be found, obtain source code from raspi of robotic arm under /home/armpi_fpv
- Use MoveIt Setup Assistant to set up robotic model of arm, following tutorial inside Hiwonder thumbdrive
- Ask for access for 1) create_envt, 2) armpi_fpv_movement project folders
- Catkin build and pray

## Run
### On Armpi-Raspi
- Set clock time
- execute alias "set_doggo_master" to change ROS_MASTER_URI
- execute alias "run_startup" to roslaunch required services
```
sudo date -s "2023-04-30 23:30"
set_doggo_master
run_startup
```

### On External PC
- Start MoveIt nodes
- Start armpi_fpv_movement server
- Start alignment server
```
roslaunch armpi_fpv_moveit_config demo.launch
rosrun armpi_fpv_movement arm_pick_place_server
rosrun alignment alignment_server
```

## Configuration
### System
Components:
- Dog-Raspi
- Armpi-Raspi
- Jetson
- External PC

Connections:
- Armpi-Raspi (ubuntu@192.168.123.69) <-LAN-> Dog-Raspi (pi@192.168.123.1)
- Dog-Raspi (pi@192.168.12.1) <-WiFI-> Jetson (thien@192.168.12.69)
- Dog-Raspi (pi@192.168.12.1) <-WiFi-> External PC (yourname@192.168.12.xxx)

### MoveIt
#### To launch scene file automatically
1. Add yourscene.scene to armpi_fpv_moveit_config/config
2. Inside armpi_fpv_moveit_config/launch/demo.launch, edit argument "scene_file" to yourscene.scene

#### To change planning time
1. Inside armpi_fpv_moveit_config/launch/trajectory_execution.launch.xml, edit parameters:
```
<param name="trajectory_execution/allowed_execution_duration_scaling" value="4.0"/>
<param name="trajectory_execution/execution_duration_monitoring" value="false"/> <!-- default 1.2 -->
```
#### To change joint limits
1. If joint limits are not changed, cannot open gripper to maximum possible width
2. Inside armpi_fpv_moveit_config/config/joint_limits.yaml, edit r_joint
```
  r_joint:
    has_velocity_limits: true
    max_velocity: 5
    has_acceleration_limits: false
    max_acceleration: 0
    max_position: 1.80
    min_position: -1.80
```
#### To include new objects to pick up
1. Create .stl CAD model (Autodesk Fusion/ Blender)
2. Create .pcd Pointcloud model with CloudCompare
  - Click on Mesh: Edit > Mesh > Sample Points - input number of points (large)
  - Click on .sampled: Edit > Subsample > Method: Octree - Change subdivision level (yield number of points as close to actual size of pointcloud in implementation)
  - Click on .subsampled: File > Save to export
3. Change code in armpi_fpv_movement/arm_pick_place_server > addObject to account for new .pcd object

#### To convert between .ply and .pcd
1. Use pcl-tools pcd2ply
