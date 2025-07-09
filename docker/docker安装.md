## docker安装：

安装前先卸载操作系统默认安装的docker，

    sudo apt-get remove docker docker-engine docker.io containerd runc

安装必要支持

    sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release

添加 Docker key

    添加 Docker 官方 GPG key （可能国内现在访问会存在问题）
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    阿里源（推荐使用阿里的gpg KEY）
    curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

添加 apt 源

    Docker官方源：
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    阿里apt源：
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

更新源

    sudo apt update
    sudo apt-get update

安装最新版本的Docker

    sudo apt install docker-ce docker-ce-cli containerd.io

查看Docker安装情况

    查看Docker版本
    sudo docker version

    查看Docker运行状态
    sudo systemctl status docker

## docker换源：

将国内镜像地址配置写入 Docker 的守护进程配置文件

    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
        "registry-mirrors": [
            "https://do.nark.eu.org",
            "https://dc.j8.work",
            "https://docker.m.daocloud.io",
            "https://dockerproxy.com",
            "https://docker.mirrors.ustc.edu.cn",
            "https://docker.nju.edu.cn"
        ]
    }
    EOF

重新加载配置、重启docker

    sudo systemctl daemon-reload
    sudo systemctl restart docker

检查 Docker 服务的状态，确认是否正常运行

    systemctl status docker

检查国内加速地址是否配置成功

    docker info

    成功会出现：
    Registry Mirrors:
    https://do.nark.eu.org/
    https://dc.j8.work/
    https://docker.m.daocloud.io/
    https://dockerproxy.com/
    https://docker.mirrors.ustc.edu.cn/
    https://docker.nju.edu.cn/

运行hello-world镜像，看看是否成功拉取到本地

    docker run hello-world

    （刚开始会出现 unable to fin image locally，不要急着终止程序，等一会就pull过来执行了）
