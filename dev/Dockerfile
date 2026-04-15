FROM mcr.microsoft.com/devcontainers/cpp:ubuntu-24.04

# avoid config interfaces
ARG DEBIAN_FRONTEND=noninteractive

# initial update and upgrade
RUN apt-get update && \
    apt-get upgrade -y

RUN apt install software-properties-common curl fish -y &&  \
    add-apt-repository universe -y

RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
     -o /usr/share/keyrings/ros-archive-keyring.gpg

RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | \
    tee /etc/apt/sources.list.d/ros2.list > /dev/null

RUN apt update && \
    apt install ros-dev-tools ros-jazzy-ros-base python3-colcon-common-extensions clang-format python3-pip python-is-python3 moreutils ros-jazzy-foxglove-bridge ros-jazzy-rmw-cyclonedds-cpp can-utils pv python3-pytest tmux htop net-tools vim iputils-ping -y

RUN git clone https://github.com/axboe/liburing.git && \
    cd liburing && \
    git checkout liburing-2.12 && \
    ./configure --cc=gcc --cxx=g++ && \
    make && \
    make liburing.pc && \
    make install && \
    cd .. && \
    rm -rf liburing

WORKDIR /

USER vscode
# Virtualenvs don't play nicely with ROS2
RUN python -m pip install --user jupyterlab --break-system-packages
RUN export PATH="$HOME/.local/bin:$PATH"

# Some useful tools and ROS2 sourcing
RUN git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && ~/.fzf/install --all --no-zsh --no-fish
RUN echo 'source /opt/ros/jazzy/setup.bash' >> ~/.bashrc
RUN echo '[ -f "/workspaces/mcu-ros/install/setup.bash" ] && source "/workspaces/mcu-ros/install/setup.bash"' >> ~/.bashrc

CMD ["bash"]
