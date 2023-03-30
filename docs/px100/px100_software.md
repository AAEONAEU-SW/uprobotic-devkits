# Software Setup Guide
Currently only ROS 2 Galactic has been tested with plans for Humble once the [EI for AMR SDK](https://www.intel.com/content/www/us/en/developer/topic-technology/edge-5g/edge-solutions/autonomous-mobile-robots/overview.html) eventually updates to Ubuntu 22.04 Jammy Jellyfish release.

 - #### Ubuntu 20.04: 
   - [x] [ROS 2 Galactic]()
 - #### Ubuntu 22.04:
   - [ ] [ROS 2 Humble]()

## Installation
Download the installation script and make it executable. 
```bash
curl 'https://raw.githubusercontent.com/hcdiekmann/UP-robotic-manipulators/main/amd64_install.sh' > amd64_install.sh
chmod +x amd64_install.sh
```
Execute the script with optional flags.
```bash
./amd64_install.sh [-h][-d DISTRO][-p PATH]
```
| Flag     | Description |
| ----------- | ----------- |
| -h   | Help message on script usage.                                                                  |
| -d   | ROS 2 distro. If not given, installs the ROS distro compatible with your Ubuntu version.        |
| -p   | Absolute installation path for the ROS workspace                                               |
- #### Ensure correct installation with the manipulator connected and powered on
```bash
ls /dev | grep ttyDXL # check that the U2D2 interface is online
ttyDXL
```

## **Camera-to-Arm Calibration**
### The calibration procedure must be completed once after intial installation and can be integrated into any programme when adjustment is required later. 

To get the static transform of the camera in the manipulators coordinate system follow these steps: 
- #### From the `ros2_ws` folder launch the perception packages and calibration tools
```bash
cd ~/ros2_ws
ros2 launch interbotix_xsarm_perception xsarm_perception.launch.py robot_model:=px100 use_pointcloud_tuner_gui:=true use_armtag_tuner_gui:=true
```
- #### Power off the servos and move the AprilTag into the FOV of the camera
```bash
# Caution this collapses the manipulator
ros2 service call /px100/torque_enable interbotix_xs_msgs/srv/TorqueEnable "{cmd_type: 'group', name: 'all', enable: false}"
```

- #### Power the servos back on with the same service call
```bash
ros2 service call /px100/torque_enable interbotix_xs_msgs/srv/TorqueEnable "{cmd_type: 'group', name: 'all', enable: true}"
```
- #### From the ArmTag Tuner GUI, Snap the tags Pose and save the configuration
<img src="https://user-images.githubusercontent.com/13176191/213815335-7a1a3866-a459-496e-907c-1864c787a4a2.png" >

- Validate the position of the AprilTag with RViz. 
The STL model should line up with the pointcloud pattern of the AprilTag pattern.

<img src="https://user-images.githubusercontent.com/13176191/213814735-3151e64b-be32-48a6-b247-ecff376d5426.png"  width="730" height="535">

## **Run Demo**
- #### To pick up small objects in the FOV of the camera run the demo script.
```bash
cd ~/ros2_ws/interbotix_ros_manipulators_uprobotic_devkits/interbotix_ros_xsarms/interbotix_xsarm_perception/demos
python3 pick_place.py
```
