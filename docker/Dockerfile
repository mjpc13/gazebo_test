# Choose the initial Docker image (noetic base is enough for iG-LIO)
FROM ros:noetic-perception-focal

ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_WS /app/catkin_ws
#ENV GAZEBO_MODEL_PATH=/root/.gazebo/models/office/models/

### METADATA ###
LABEL author="Mario Cristovao <mario.cristovao@isr.uc.pt>"
LABEL maintainer="Mario Cristovao <mario.cristovao@isr.uc.pt>"
LABEL description="Gazebo World Environment"
### END METADATA ###


### INSTALL DEPENDENCIES ###
RUN apt-get update \
  && apt-get install -y curl git \
  && apt-get update \
  && apt-get install -y \
  ros-${ROS_DISTRO}-gazebo-ros-pkgs \
  ros-${ROS_DISTRO}-xacro \
  ros-${ROS_DISTRO}-robot-state-publisher \
  ros-${ROS_DISTRO}-rqt-common-plugins \
  ros-${ROS_DISTRO}-rqt \
  ros-${ROS_DISTRO}-rqt-robot-plugins \
  ros-${ROS_DISTRO}-rviz-imu-plugin \
  python3-catkin-tools \
  && rm -rf /var/lib/apt/lists/* \
  && mkdir -p /app/catkin_ws/src

### END INSTALL DEPENDENCIES ###

SHELL ["/bin/bash", "-l", "-c"]
WORKDIR ${ROS_WS}

# Build ros package source (compiles every package in the workspace, don't change this!)
RUN catkin config --extend /opt/ros/$ROS_DISTRO

RUN git clone https://github.com/MobileRobots/amr-ros-config.git src/amr-ros-config && \
    git clone https://github.com/pal-robotics/realsense_gazebo_plugin.git src/realsense_gazebo_plugin


#Compile packages
RUN catkin build -s

# source catkin workspace
RUN echo "source /app/catkin_ws/devel/setup.bash" >> /root/.profile

#ENV GAZEBO_RESOURCE_PATH=/app/catkin_ws/src/gazebo_test/worlds/office/office_part1/models:${GAZEBO_RESOURCE_PATH}

#ENTRYPOINT [ "catkin", "build" "&&"]

CMD ["/bin/bash", "-l", "-c", "roslaunch gazebo_test gazebo.launch"]

#CMD ["/bin/bash", "-l", "-c", "roscore" ]