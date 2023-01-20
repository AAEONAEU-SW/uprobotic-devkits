# Foxglove Studio Setup Instructions
## Prerequisites 

- Robotic Kit with [PSU](https://github.com/up-board/up-community/wiki/UP-Robotic-Development-Kit-QSG#power-supply) and [Operating System](https://github.com/up-board/up-community/wiki/UP-Robotic-Development-Kit-QSG#operating-system-installation) installed

- [Edge Insights for AMR](https://www.intel.com/content/www/us/en/develop/documentation/ei4amr-2022-2-get-started-robot-kit/top/download-ei4amr.html) installed


## Setup

1. Locate the Docker Compose files inside the _AMR_containers_ folder
```bash
cd <ei_for_amr_path>/Edge_Insights_for_Autonomous_Mobile_Robots_*/AMR_containers/01_docker_sdk_env/docker_compose/05_tutorials
```


2. Append the following  _rosbridge-suite_ service to the Compose file of the application you would like to run 
e.g. _aaeon_wandering__aaeon_realsense_collab_slam_fm_nav2_ukf.tutorial.yml_
```yml
  rosbridge-suite:
    image: ${REPO_URL}amr-ros-base:${DOCKER_TAG:-latest}
    container_name: ${CONTAINER_NAME_PREFIX:-amr-}rosbridge-suite
    extends:
      file:  ../common/container-launch-env.yml
      service: container-launch-env
    build:
      target: ros-base
    depends_on:
      - aaeon-amr-interface
      - nav2
      - collab-slam
    command:
      - |
        sudo apt-get update
        sudo apt install ros-foxy-rosbridge-server -y
        source /opt/ros/foxy/setup.bash
        ros2 launch rosbridge_server rosbridge_websocket_launch.xml
```

3. Save the Compose file
> **_NOTE:_**  Ensure the correct line spacing when appending the yml file.

## Run
To start the dashboard, launch your application as usually.
1. Got to the installation folder of EI for AMR and locate the _AMR_containers_ folder

```bash
cd <ei_for_amr_path>/Edge_Insights_for_Autonomous_Mobile_Robots_*/AMR_containers
```

2. Prepare the environment
```bash
source 01_docker_sdk_env/docker_compose/common/docker_compose.source
export CONTAINER_BASE_PATH=`pwd`
export ROS_DOMAIN_ID=27
```
3. Start the application as usually with
```bash
docker-compose -f 01_docker_sdk_env/docker_compose/05_tutorials/aaeon_wandering__aaeon_realsense_collab_slam_fm_nav2_ukf.tutorial.yml up
```

4. Open the [Foxglove web app](https://studio.foxglove.dev/) or [desktop app](https://foxglove.dev/download) and open a connection to the rosbridge-server  at ```ws://localhost:9090``` 

5. For a remote connection, connect to a WiFi network and acquire the robots IP address with the
```ip addr``` command.

6. Use any device to connected to the same network and use  ```ws://<robot-ip>:9090/``` when opening a connection in Foxglove Studio




---
## Credits
[Foxglove Studio](https://github.com/foxglove/studio)

