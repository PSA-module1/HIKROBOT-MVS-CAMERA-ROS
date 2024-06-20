# Use the noetic-ros-base image as the base image
FROM ros:noetic-ros-base

ARG DEBIAN_FRONTEND=noninteractive

# Setup timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    rm /etc/localtime && \
    ln -s /usr/share/zoneinfo/Asia/Singapore /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata

# Install basic apt tools and dependencies
RUN apt update && apt upgrade -y && apt install -y \
    vim wget tmux git python3-rosdep build-essential cmake python3-catkin-tools \
    ros-${ROS_DISTRO}-cv-bridge \
    ros-${ROS_DISTRO}-image-transport \
    ros-${ROS_DISTRO}-camera-info-manager \
    ros-${ROS_DISTRO}-compressed-image-transport \
    libx11-dev \
    && rm -rf /var/lib/apt/lists/*

# Setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV ROS_DISTRO noetic

# Install MVS SDK 2.1.2
COPY MVS-2.1.2_x86_64_20231116.deb /root/MVS-2.1.2_x86_64_20231225.deb
RUN dpkg -i /root/MVS-2.1.2_x86_64_20231225.deb && rm /root/MVS-2.1.2_x86_64_20231225.deb
ENV PATH $PATH:/opt/MVS/bin
ENV MVCAM_COMMON_RUNENV /opt/MVS/lib
ENV LD_LIBRARY_PATH /opt/MVS/lib/64:/opt/MVS/lib/32:/opt/MVS/bin:$LD_LIBRARY_PATH

# Source ROS setup scripts
RUN . /opt/ros/${ROS_DISTRO}/setup.sh \
    && echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

# Create and build the catkin workspace
RUN echo "Running catkin_make" \
    && . /opt/ros/${ROS_DISTRO}/setup.sh \
    && mkdir -p /root/catkin_ws/src && cd /root/catkin_ws/src \
    && git clone https://github.com/PSA-module1/HIKROBOT-MVS-CAMERA-ROS.git \
    && cd /root/catkin_ws \
    && catkin_make \
    && echo "source /root/catkin_ws/devel/setup.bash" >> ~/.bashrc

# Set the working directory
WORKDIR /root/catkin_ws

CMD ["bash"]