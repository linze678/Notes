一、前期准备工作
	1. sudo usermod -a -G dialout $USER
		作用：将当前用户（$USER）添加到 dialout 组中。
		解释：dialout 组通常与串口通信相关。添加到该组后，用户将具有访问串口设备的权限，常用于与飞行控制器或其他硬件设备（如 GPS、传感器等）进行串行通信。

	2. sudo apt-get remove modemmanager -y
		作用：卸载 modemmanager 软件包。
		解释：modemmanager 是一个用于管理调制解调器（Modem）和其他通信设备的工具。在与飞行控制器等硬件设备通过串口或其他通信方式连接时，卸载该软件包可以避免与串口通信产生冲突，因为 modemmanager 可能会干扰设备的正常通信。

	3. sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
		作用：安装一些 GStreamer 插件，包括：
		gstreamer1.0-plugins-bad：一些 "不太稳定" 或质量不高的插件。
		gstreamer1.0-libav：用于处理音频和视频的插件，支持多种媒体格式。
		gstreamer1.0-gl：提供对 OpenGL 图形硬件加速的支持。
		解释：这些插件用于视频流处理、图像处理等，特别是在无人机、机器人系统等需要处理摄像头、视频流或图像数据的场景下非常有用。它们支持不同的视频编解码格式和硬件加速。

	4. sudo apt install libfuse2 -y
		作用：安装 libfuse2 库。
		解释：libfuse2 是文件系统用户空间工具的一个库，允许用户空间程序实现文件系统。某些应用（如在系统中挂载虚拟文件系统或磁盘映像）可能需要该库来支持自定义文件系统操作。

	5. sudo apt install libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor-dev -y
		作用：安装几个图形界面和窗口管理相关的库：
		libxcb-xinerama0：XCB（X Protocol C Binding）库的扩展，用于处理 X 窗口系统中的屏幕扩展。
		libxkbcommon-x11-0：提供键盘输入和 X11 显示协议相关的支持。
		libxcb-cursor-dev：用于支持鼠标光标操作的 XCB 库开发包。
		解释：这些库通常与图形界面相关，可能在需要图形显示或交互的应用中（如桌面环境、仿真软件或其他图形用户界面工具）使用。

二、重启电脑

三、安装QGC
	chmod +x ./QGroundControl.AppImage
	./QGroundControl.AppImage  (or double click)




解决QGC启动后全屏，图形界面没有关闭/最小化按键的问题：
	CTRL+ALT+T调出终端
	wmctrl -r "QGroundControl" -b remove,fullscreen运行命令来取消全屏模式

