FROM osrf/ros:humble-desktop-full-jammy

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Add ubuntu user with same UID and GID as your host system, if it doesn't already exist
# Since Ubuntu 24.04, a non-root user is created by default with the name vscode and UID=1000
ARG USERNAME=ubuntu
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if ! id -u $USER_UID >/dev/null 2>&1; then \
        groupadd --gid $USER_GID $USERNAME && \
        useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    fi
# Add sudo support for the non-root user
RUN apt-get update && \
    apt-get install -y sudo && \
    echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Switch from root to user
USER $USERNAME

# Add user to video group to allow access to webcam
RUN sudo usermod --append --groups video $USERNAME

# Update all packages
RUN sudo apt update && sudo apt upgrade -y

# Install Git
RUN sudo apt install -y git

RUN sudo mkdir -p /home/ubuntu/nav2_ws/src/nav2_dev

WORKDIR /home/ubuntu//nav2_ws/src/nav2_dev

# Rosdep update
RUN rosdep update

# Source the ROS setup file
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

################################
## ADD ANY CUSTOM SETUP BELOW ##
################################

#Install other dependencies
RUN sudo apt install -y vim \
    -y ros-humble-robot-localization \
    -y ros-humble-gazebo-ros-pkgs \
    -y ros-humble-navigation2 \
    -y ros-humble-nav2-bringup \
    -y ros-humble-turtlebot3-gazebo \
    -y ros-humble-ros-gz \
    -y ros-humble-joint-state-publisher

# RUN colcon build --symlink-install

# RUN chmod -R 755 .

RUN echo "source /usr/share/gazebo/setup.bash" >> ~/.bashrc
RUN echo "source /nav2_ws/install/setup.bash" >> ~/.bashrc
