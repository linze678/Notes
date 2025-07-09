参考链接：https://blog.csdn.net/m0_49384824/article/details/142786237

# 实时查看GPU的利用率：
watch -n 1 nvidia-smi 

watch：这是一个 Linux 命令，用于周期性地执行指定的命令，并将其输出显示在终端上。默认情况下，它会每两秒刷新一次输出。

-n 1：这是 watch 命令的一个选项，表示设置刷新间隔为1秒。换句话说，watch 每隔1秒钟重新运行一次指定的命令，并更新显示的结果。

nvidia-smi：这是 NVIDIA 提供的一个命令，用于显示 GPU 的实时信息，包括显卡的温度、功耗、显存使用率、GPU 负载等。它常用于监控 NVIDIA GPU 的运行状态。






# 实时查看CPU和内存（Mem）利用率：
sudo apt-get install htop
htop

注意：经研究发现，htop 会把一个进程里的线程当做一个进程来显示出来，上图中的 google chrome 一共有 n 个线程，所以 htop 显示了多个进程。这个特性对于分析进程性能很不有利， 所以我们要关掉它。

F2 打开设置
用鼠标选择 Display options -> Hide userland threads ，当看到该选项前面的 [ ] 中有一个 x 号时，按 F10 确认。
确认后，在 htop 的进程列表里就看不见一堆重复的进程了。


