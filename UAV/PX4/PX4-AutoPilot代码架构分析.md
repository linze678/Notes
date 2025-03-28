1. boards文件夹
	boards文件夹中是各个品牌、版本的飞控板的编译脚本，其中px4文件夹装的是pixhawk的原生固件的编译脚本。
	进入px4文件夹，内部是pixhawk的不同版本，例如pixhawk v2.4.8版本对应的是fmu-v2文件夹。
	进入fmu-v2文件夹，这里的所有px4board文件就是编译脚本，不同的编译脚本对应不同的编译命令，也会生成不同的编译文件。
	比如default.px4board对应make px4_fmu-v2_default命令，fixedwing.px4board对应make px4_fmu-v2_fixedwing命令，multicopter.px4board对应make px4_fmu-v2_multicopter命令，rover.px4board对应make px4_fmu-v2_rover命令。
	extras文件夹中是飞控板两个芯片的BootLoader文件，分别是px4_fmu-v2_bootloader.bin和px4_io-v2_default.bin二进制文件。
	编译脚本用来设置哪些驱动文件、功能模块要被编译进固件中，可以在这些配置文件中添加或删除驱动程序的路径，来控制哪些硬件驱动被包含在最终的固件中，进行配置。
	
	
2. build文件夹
	build文件夹是编译目标目录，只有经过至少一次编译才会生成此文件夹，内部存放的是源码经过编译生成的编译选项、中间文件、目标文件等。编译不同版本的固件会生成不同的文件夹。
	px4_fmu-v2_default就是make px4_fmu-v2_default命令编译生成的，用于飞控板的。
	px4_sitl_default就是make px4_sitl_default命令编译生成的，用于jMAVSim仿真或gazebo仿真。
		内部主要需要在意的是：topics_temporary_header(所有的任务是要的头文件，在创建任务和使用数据结构时可以从该处检索)；topics_temporary_sources(系统中所有的任务函数)


3. cmake文件夹
	cmake文件夹是cmake编译工具的相关文件，在开发中这个文件夹中的文件不需要修改。
	主要使用 nuttx_px4fmu-v2_default.cmake。该处主要是关于系统使用的文件的路径配置，在 PX4 系统所有的.CPP 和.C 文件都是通过在该处进行路径包含的。
	在需要自己创建私有任务或者 sensor 驱动程序时需要添加到该处。
	
	
4. Documentation文件夹
	开发者文档目录，包括代码说明等。
	
	
5. launch文件夹
	launch文件夹是仿真环境用到的文件，在gazebo中生成世界，配置ros节点等。
	
	
6. msg文件夹
	msg文件夹是存放uORB消息主题的地方，实现PX4各进程之间通信就是用这些message。我们可以在这个文件夹内定义自己的message。
	该文件夹下的 Cmakelists.txt，该文档是整个 msg 部分的配置部分，在开发者创建私有任务或sensor驱动程序时创建对应的数据结构（***.msg）以后，需要把开发者创建的***.msg 添加到Cmakelists.txt 中，否则编译时识别不到开发者创建的数据结构。
	在该部分创建好以后，直接编译可以自动生成相对应的 C/C++下的标准头文件，即在build文件夹中所介绍的 topics_temporary_header，在 PX4 系统的任何一个地方引用所需要的数据结构，都需要把这个头文件包含进去。
	
	
7. ROMFS文件夹
	ROM file system的缩写，是文件系统文件夹，里面存放的飞控系统的启动脚本，负责系统初始化。
	内部的 px4fmu_common 文件夹中的 init.d 是关于 px4 系统初始上电启动的启动脚本，即一系列的启动过程和系统配置。其中较为重要的部分在如下目录下：.\ROMFS\px4fmu_common\init.d
	如文件rcS、rc.logging、rc.mc_apps、rc.sensors等
	rcS：最先启动的脚本，负责挂载SD卡、启动uORB、配置系统参数等。
	rc.logging：日志配置和启动代码。
	rc.sensors：sensors驱动启动代码。（如果在 src/drivers 中创建私有的 sensor 驱动，建议放在该部分启动）
	rc.mc_default：配置和 PWM***相关的系统参数，内部含有怠速的配置。
	rc.mc_apps：启动上层应用（src/modules中的模块均在此启动），如 attitude_estimate 、attitude_control 、position_estimate 和 position_control 等。（如果在 src/modules 中创建私有的上层 application，建议放在该部分启动）
	
	
8. platforms文件夹
	系统平台实现的文件，包括PX4采用的Nuttx操作系统的源代码。


9. src文件夹
	src是源代码目录，是PX4的核心，非常重要。
	（1）drivers文件夹（驱动）
		pixhawk 硬件系统中使用的所有传感器的驱动代码，包括陀螺仪、加速度计、磁力计、气压计、GPS、光流等，也包括pwm驱动、STM32主控MCU的io输出控制（px4io控制）、不同遥控器通信协议等功能的驱动。
	（2）examples文件夹
		examples文件夹是官方给出的简单例程，为了便于开发者做二次开发调试测试使用。
		尤其是 px4_simple_app，内部涉及了如何通过 uORB 机制获取所需要的数据。
	（3）include文件夹
		include文件夹是其他代码需要用到的头文件和库。
	（4）lib文件夹
		标准库：有矩阵运算、PID控制、传感器校准等。
	（5）modules文件夹（算法）
		modules文件夹是功能模块文件夹，也是二次开发主要的文件夹，包括姿态解算，姿态控制，位置估计，位置控制，指令控制，落地检测，传感器初始化，uORB，commander(校准)等。
		attitude_estimator_q：使用 mahony 互补滤波算法实现姿态结算。
		commander：整个系统的过程实现，包括起飞前各传感器的校准、安全开关是否使能、飞行模式切换、pixhawk硬件上指示灯颜色定义等。
		ekf2：使用扩展卡尔曼滤波器算法实现姿态和位置结算。
		fw_att_control：固定翼飞机的姿态控制。
		fw_pos_control_l1：固定翼飞机的位置控制器。
		land_detector：使用land飞行模式降落的落地监测部分。
		local_position_estimator：LPE 算法实现位置解算。
		logger：关于 log 日志的读写函数。
		mavlink：和地面站通信的通信协议。
		mc_att_control：姿态控制的算法实现，主要就是姿态的内外环 PID 控制，外环角度控制、内环角速度控制。
		mc_pos_control：位置控制的算法实现，主要就是位置的内外环 PID 控制，外环速度控制、内环加速度控制。
		navigator：任务，失效保护和RTL导航仪。
		sensors：关于各种传感器的相关函数。
		vtol_att_control：垂直起降姿态控制器。
		systemlib：系统所需要的参数列表。
		uORB：IPC 通信机制。首先就是需要了解如何使用它进行进程间通信，了解整个系统中的数据流流向。
		sdlog2：SD 卡的 log 日志读写部分，在 SD 卡中看到的所有数据都是通过该部分写进去的，平时做 log 日志分析也是查看的这个内部的数据，所以，假如想监测某个“目标数据”，也可以由此处写到 SD 卡中然后在做相应的分析。
	（6）systemcmds文件夹
		systemcmds文件夹是系统指令文件夹，都是飞控终端支持的命令的源码，如top命令，reboot命令等。
	（7）templates文件夹
		templates文件夹中是存放功能模块的代码模板，可以用于参考。

10. Tools文件夹
	Tools文件夹下是一些工具，比如下载工具、仿真环境等。




















