FROM nvidia/cuda:11.6.1-devel-ubuntu20.04

SHELL ["/bin/bash", "-c"]

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y \
        wget \
        curl \
        git \
        vim \
        python3-pip \
        lsb-release \
        gnupg

# ROS setting
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ros-noetic-desktop-full
RUN apt-get update && \
    apt-get install python3-rosdep && \
    rosdep init && rosdep update

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3-rosinstall \
        python3-catkin-tools

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Gazebo
RUN apt-get update && \
    apt-get install -y ros-noetic-controller-interface  ros-noetic-gazebo-ros-control && \
    apt-get install -y ros-noetic-joint-state-controller ros-noetic-effort-controllers ros-noetic-joint-trajectory-controller

# Unitree SDK
RUN mkdir /root/sdk && cd /root/sdk && \
    git clone https://github.com/lcm-proj/lcm && \
    git clone https://github.com/unitreerobotics/unitree_legged_sdk && \
    cd lcm && mkdir build && cd build && cmake ../ && make && make install && \
    cd ../../unitree_legged_sdk && git checkout v3.2 && mkdir build && cd build && cmake ../ && make

# Unitree ROS
RUN mkdir -p /root/external_catkin_ws/src && \
    cd /root/external_catkin_ws/src && \
    git clone https://github.com/unitreerobotics/unitree_ros_to_real && \
    git clone https://github.com/unitreerobotics/unitree_ros && \
    cd unitree_ros_to_real && git checkout v3.2.1 && cd ../../ && \
    bash -c "source /opt/ros/noetic/setup.bash; export UNITREE_SDK_VERSION=3_2; export UNITREE_LEGGED_SDK_PATH=/root/sdk/unitree_legged_sdk; export UNITREE_PLATFORM="amd64"; catkin_make"


WORKDIR /root

ENTRYPOINT []

CMD ["/bin/bash"]