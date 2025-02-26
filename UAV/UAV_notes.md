一、硬件系统
螺旋桨：
    螺旋桨一般4个数字表示，前面两位表示螺旋桨的直径，后面两位螺旋桨的螺距。（螺距：旋转一周上升的距离）
    如：APC1045(10*45):桨的直径10英寸螺距4.5英寸
    直径越大、螺距越大，升力越强。

    桨叶数：二叶桨的力效比三叶桨高（每个桨的效率高），但肯定桨叶越多，总升力越高。
    顺时针旋转的桨是反桨（一般标CW或R），逆时针旋转的桨是正桨（一般标CCW或P）。

    四旋翼无人机，四个旋翼对角旋向一致，相邻旋向相反，反扭矩相互抵消


电机：
    无刷直流电机：效率高，成本低，适用于小型化
    无刷直流电机分：外转子电机、内转子电机。
    规格:四位数字组成，例如：2205(22*05)，前面两位代表定子的直径，后两位代表定子高度。
    直径越大，高度越大，动力越强。
    电机和桨是配合一体的，桨的升力越强，需要配的电机动力也应该越强。

    kv值:空载情况下，外加1v电压得到的电机转速值，单位RPM转每分钟
    kv小的电机可以产生更大的力矩，大螺旋桨用小kv值，小螺旋桨用大kv。
    穿越机的kv值可以达到两千三千，但负载型无人机的kv值只有几百


电调:
    电子调速器(Electronic speed control，ESC)
    功能：
    1.调速:通过飞控板给定的pwm波，对电机进行调速
    2.充当换向器的角色:无刷电机没有电刷换相，需要靠esc来电子换相，将直接电源转化为三相电源供给无刷电机。
    常见参数：
    最大持续电流：最大持续电流正在工作模式下持续输出的电流（可接纳的电机的最大电流）。
    刷新频率400hz


电池:
    1、电压:
    小型飞行器专用的锂聚合电池，单节电芯的标称电压3.7v，满电4.2v，一般无人机3.6低电压报警，再放电就会损坏，过放
    2、组合
    s表示串联 series
    p表示并联parallel
    3、容量 毫安时
    5300毫安时的电池表示以5300毫安的电流放电可以持续一个小时。


传感器/外设：
    最低要求：陀螺仪+加速度计+磁力计（罗盘）+气压计
    Pixhawk飞控板上集成了一些最小的传感器，如陀螺仪+加速度计+气压计
    1.距离传感器：
        用于精准着陆、避障、地形跟随等。
    2.定高雷达：
        非常精准的飞行高度测量。
    3.光流
        使用下视相机和向下的距离传感器进行速度估计。
        可以用于GPS信号拒止的室内，代替GPS中的速度估计模块。
    
    4.数传
        数传电台可以在QGC地面站与运行的PX4机体之间提供无线MAVLink连接。
        使得飞机飞行中调试参数、实时检查遥测信息、更改任务成为可能。
    5.SD卡
        记录无人机飞行信息、类似飞机的“黑匣子”




二、控制系统
飞控：
    世界上最著名的飞控板Pixhawk（基于px4）    


地面站：
    QGroundontrol（QGC）
    功能：
    飞行地图显示无人机位置、飞行轨迹、航路点和飞机仪表等状态信息。
    可进行自主飞行的任务规划（设置目标点、途径点等），可显示视频流。

    QGC支持 ArduPilot 和 PX4


飞行模式：
    分为手动模式、自主模式。
    Stabilized：自稳模式，直接自主控制无人机姿态（底层控制）
    Altitude：定高模式
    Position：定点模式，可以进行悬停
    可通过遥控器上的开关/地面站来切换飞行模式。


遥控器：


机载计算机：
    PX4可以通过串行接线/WiFi，由独立的机载飞行计算机进行控制。
    机载计算机通常用 MACLink API（如MAVSDK或MAVROS）进行通信。

    Offboard模式：用于从机载计算机/地面站对飞控（如PX4）进行控制。
    在offboard模式下，飞行器接收到的控制指令通常是通过MAVLink协议从外部系统发送的。这意味着飞行器的飞行路径和动作由外部系统决定，而不是由PX4内部的飞行计划或任务管理器控制。








一些基本概念：
坐标系：
    1.地理坐标系（NED）：
    原点一般是无人机的起飞点
    N轴水平指北，E轴水平指东，D轴指向地心（可简化为垂直向下）
    是右手系

    2.运载体坐标系（FRD）
    原点位于运载体质心
    X轴沿运载体纵轴，指向前方
    Y轴沿运载体横向，指向右翼
    z轴与X、Y轴构成右手系，指向运载体底部，向下


姿态角：
    横滚角（Roll）：y轴与水平面的夹角
    附仰角（Pich）：x轴与水平面的夹角
    偏航角（Yaw）：x轴在水平面的投影与N轴（正北方向）的夹角，顺时针为正


