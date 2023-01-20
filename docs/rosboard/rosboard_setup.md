# ROSboard Setup Instructions

## Prerequisites 

- Robotic Kit with [PSU](https://github.com/up-board/up-community/wiki/UP-Robotic-Development-Kit-QSG#power-supply) and [Operating System](https://github.com/up-board/up-community/wiki/UP-Robotic-Development-Kit-QSG#operating-system-installation) installed

- [Edge Insights for AMR](https://www.intel.com/content/www/us/en/develop/documentation/ei4amr-2022-2-get-started-robot-kit/top/download-ei4amr.html) installed


## Setup

1. Locate the Docker Compose files inside the _AMR_containers_ folder.
```bash
cd <ei_for_amr_path>/Edge_Insights_for_Autonomous_Mobile_Robots_*/AMR_containers/01_docker_sdk_env/docker_compose/05_tutorials
```


2. Append the following  _rosboard_ service to the Compose file of the application you would like to run.
e.g. _aaeon_wandering__aaeon_realsense_collab_slam_fm_nav2_ukf.tutorial.yml_
```yml
  rosboard:
    image: ${REPO_URL}amr-ros-base:${DOCKER_TAG:-latest}
    container_name: ${CONTAINER_NAME_PREFIX:-amr-}rosboard
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
        cd /home/eiforamr/ros2_ws/src
        sudo git clone https://github.com/dheera/rosboard.git
        pip3 install tornado simplejpeg
        source /opt/ros/foxy/setup.bash 
        /home/eiforamr/ros2_ws/src/rosboard/run

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

4. Now access the Dashboard at ```http://localhost:8888``` 

5. For a remote connection, connect to a WiFi network and acquire the robots IP address with the
```ip addr``` command.

6. Use any device connected to the same network and point your web browser at ```http://<robot-ip>:8888/```



---
## Credits
[Rosboard](https://github.com/dheera/rosboard)  Copyright (c) 2021, Dheera Venkatraman BSD 3-Clause License
