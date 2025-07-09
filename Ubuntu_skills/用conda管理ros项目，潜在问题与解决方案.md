用conda管理ros项目，潜在问题与解决方案
1. ROS 环境变量冲突
问题：Conda 会修改 PATH 和 PYTHONPATH，可能覆盖 ROS 的默认路径。
解决方案：在 .bashrc 中优先加载 ROS 环境，再激活 Conda：
如：在.bashrc 末尾添加
    source /opt/ros/<distro>/setup.bash
    conda activate ros_env

2.系统包与 Conda 包冲突
问题：Conda 安装的包可能与系统 apt 安装的包路径冲突（如 libopencv）。
解决方案：
优先使用 Conda 安装的包（确保 ~/.bashrc 中 PATH 和 LD_LIBRARY_PATH 优先指向 Conda 路径）。
或者，仅用 Conda 管理 Python 依赖，系统包仍通过 apt 安装。