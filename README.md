# armpi_fpv_fyp
Overview of A1149-221 using ArmPi-FPV, Realsense D405

## Setup

### Libraries used
- MoveIt!
- PCL-ROS

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
- Run

## Run
### On Armpi-Raspi
- Set clock time
- execute alias "set_doggo_master" to change ROS_MASTER_URI
- execute alias "run_startup" to roslaunch required services

### On External PC
- roslaunch armpi_fpv_moveit_config demo.launch
- rosrun armpi_fpv_movement arm_pick_place_server
- rosrun alignment alignment_server

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

