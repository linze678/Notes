安装指定版本的cuda

1. 添加 NVIDIA 官方 CUDA 仓库
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb  # 注意系统版本
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt update

2. 安装 CUDA 11.7（完整开发工具包）
sudo apt install cuda-toolkit-11-7  # 关键！指定版本安装

3. 更新驱动（必须！）
sudo apt install nvidia-driver-535
sudo reboot

4. 验证安装
nvcc --version
