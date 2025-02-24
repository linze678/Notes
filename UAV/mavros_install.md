一、ROS1/ROS2安装mavros： 
    1. 安装 MAVROS 和相关包,该指令对应的ros版本是ros2 humble
    sudo apt-get install ros-humble-mavros ros-humble-mavros-extras ros-humble-mavros-msgs

    2. 下载脚本
    git clone -b ros2 https://github.com/mavlink/mavros.git

    3. 运行脚本
    cd mavros/mavros/scripts
    sudo chmod a+x ./install_geographiclib_datasets.sh
    sudo ./install_geographiclib_datasets.sh

    4. 测试mavros与px4的通信
        //ros2下运行
        ros2 launch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"
        //ros1下运行
        roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"

        解释：
        启动一个 ROS launch 文件，并启动 MAVROS（MAVLink ROS）与飞控（FCU）进行通信：
            ros2 launch:
                ros2 launch 是用于启动 ROS 2 的 launch 文件的命令，launch 文件用于定义 ROS 2 节点的启动顺序、参数配置和其他设置。
            mavros:
                这是你要启动的 ROS 2 包名。mavros 是一个用于连接 MAVLink 飞控系统（如 Pixhawk）和 ROS 的中间层。它可以通过 MAVLink 协议与多种飞控系统（如 PX4 和 APM）进行通信。
            px4.launch:
                这是在 mavros 包中的一个启动文件（launch 文件）。px4.launch 文件用于配置和启动与 PX4 飞控系统的通信，启动 MAVROS 节点，并配置飞控系统的相关参数。
            fcu_url:="udp://:14540@127.0.0.1:14557":
                这是一个参数传递给 px4.launch 文件的 URL，表示飞控系统（FCU）的连接方式。fcu_url 是 MAVROS 用来连接飞控的地址。具体格式为：
                udp://:14540@127.0.0.1:14557 表示使用 UDP 协议连接到 127.0.0.1（本地计算机），端口为 14557，并监听从端口 14540 发来的数据流。这是 PX4 飞控系统的默认通信方式，通常用于地面站与飞控的连接。

    5. 测试mavros与sitl通信（MAVROS连接仿真）
        ros2 launch px4 mavros_posix_sitl.launch
        rostopic echo /mavros/state






二、ROS2下安装mavros2：(ROS2更推荐mavros2)
    1. 安装
    mkdir -p ~/mavros2_ws/src
    cd ~/mavros2_ws
    rosinstall_generator --format repos mavlink | tee /tmp/mavlink.repos
    rosinstall_generator --format repos --upstream mavros | tee -a /tmp/mavros.repos
    vcs import src < /tmp/mavlink.repos
    vcs import src < /tmp/mavros.repos
    rosdep install --from-paths src --ignore-src -y
    sudo ./src/mavros/mavros/scripts/install_geographiclib_datasets.sh
    colcon build
    source ~/mavros2_ws/install/setup.bash

    2. 验证安装：
    source /opt/ros/humble/setup.bash
    source ~/mavros2_ws/install/setup.bash
    ros2 launch mavros px4.launch

    3. MAVROS2连接仿真和实机需要的参数配置
    默认情况下，PX4使用固定的UDP端口与地面站（如QGroundControl）、机载电脑（如MAVSDK、MAVROS）和仿真器（如Gazebo）进行MAVLink通信。
    PX4 的远程 UDP 端口 14540 用于与外部 API 通信（与Offboard模式板外电脑进行通信）。Offboard模式板外电脑应侦听此端口上的连接。
    PX4 的远程 UDP 端口 14550 用于与地面控制站通信。默认情况下，QGroundControl 监听此端口。
    模拟器的本地 TCP 端口 4560 用于与 PX4 通信。PX4 侦听此端口，仿真器应通过向该端口广播数据来启动通信。

        MAVROS2连接仿真:
        source /opt/ros/humble/setup.bash
        source ~/mavros2_ws/install/setup.bash
        ros2 launch mavros px4.launch fcu_url:=udp://:14540@127.0.0.1:14555

        MAVROS2连接px4实机：
        source /opt/ros/foxy/setup.bash
        source ~/mavros2_ws/install/setup.bash
        //这里192.168.1.11是无人机上WiFi模块的IP地址。
        ros2 launch mavros px4.launch fcu_url:=udp://:14550@192.168.1.11:14555


