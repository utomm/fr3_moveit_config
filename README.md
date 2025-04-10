# Franka Emika FR3 MoveIt Config Package

The Panda robot is the flagship MoveIt integration robot used in the MoveIt tutorials.
Any changes to MoveIt need to be propagated into this config fast, so this package
is co-located under the ``ros-planning`` Github organization here.

For those who are still using FR3 with ROS1. launch files adopted from [panda_moveit_config](https://github.com/moveit/panda_moveit_config). 

This configuration package is designed to work with both **1-PC** and **2-PC** setups.


## Installation

### Install Franka-related Packages

Since we are using FR3, we cannot install `libfranka` via `apt`. We need to build `libfranka` from source. Follow the instructions here: [Franka Installation - Building from Source](https://frankaemika.github.io/docs/installation_linux.html#building-from-source).

If you're working with a **lower PC directly connected to the Panda** robot, you must switch to a **real-time kernel** for optimal performance. Instructions can be found here: [Setting up the Real-Time Kernel](https://frankaemika.github.io/docs/installation_linux.html#setting-up-the-real-time-kernel).

### Build this Package

If you are using a **2-PC setup**, you only need to install this package on the **upper PC**.

1. Create a workspace and clone this repository:
```bash
mkdir ~/fr3_ws
cd ~/fr3_ws/src
git clone https://github.com/utomm/fr3_moveit_config.git
```

2. Build the package:
```
cd ~/fr3_ws
catkin_make
source ~/fr3_ws/devel/setup.bash
```
### Usage

First, launch the Franka control interface:
```
roslaunch franka_control franka_control.launch \
    robot_ip:=<fci-ip>   # mandatory \
    load_gripper:=true   # default: true \
    robot:=fr3           # default: panda
```
This will start the ROS interface to communicate with the libfranka interface, which is officially provided by franka_ros.

Bring up MoveIt Packages once the control interface is runnin: launch MoveIt to directly control the robot in RViz or use MoveIt to control the robot:

`roslaunch fr3_moveit_config franka_control_fr3.launch`

This launch file can be run separately from another PC if you are using a 2-PC setup.

### Two PCs Setup

If you're working with two PCs, follow these steps:

1. Start roscore on one of the PCs first.

2. On the other PC, correctly set the ROS master address by setting the following environment variables:

e.g. Set ROS_MASTER_URI: `export ROS_MASTER_URI=http://192.168.1.[roscore_local_host]:11311` and Set ROS_HOSTNAME or ROS_IP

## Troubleshooting

If the spawner gets stuck on the upper PC, you can manually start it on the lower PC. Run the following command on the lower PC to bring up the spawner:

`rosrun controller_manager spawner position_joint_trajectory_controller`


