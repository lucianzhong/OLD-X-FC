# OLD-X-FC
OLD-X Autopilot






OLD-X飞控— 一款国内最“靠谱”的开源飞控




您是否渴望拥有一个与PX4性能相当但在KEIL中使用C语言开发的飞控平台？
您是否渴望拥有一个不关心融合解算只需好好研究控制和导航理论的平台？
您是否对机器视觉，感知壁障感兴趣却又不知如何为飞控加上Linux 与ROS？
OLD-X飞控力图帮您解决以上所有疑问！！！

更新日期：2016/8/26  V1.0

目录
1.飞控介绍	3
2.团队介绍	5
3. OLD-X飞控性能	5
（1）OLD-X IMU模块	7
（2）OLD-X FC模块	9
（3）NRF-飞控 模块	11
（4）NRF-地面 模块	12
（5）SD黑匣子 模块	13
（6） 超声波壁障模块——开发中	13
4. 性能展示	14
5.附录	14










1.飞控介绍


OLD-X飞控是国内开源飞控中为数不多基于嵌入式操作系统的开源飞控，目前国内的开源飞控层次不穷但大多都是互相抄来抄去，只是在硬件上做出稍微的改变，在飞控软件架构和控制方法上并没有本质的区别，用户购买往往只能是重复投资无法学习到新的控制理论知识和嵌入式编程技巧。
OLD-X飞控基于UCOSII操作系统大大增加了对CPU的利用率比起其他裸奔的开源飞控代码执行效率和执行频率稳定性高出很多，除了采用操作系统外OLD-X飞控并没有采用纯串级PID的控制方式而是采用了自抗扰的控制方法，该方法对飞行器外部扰动和为建模部分的估计比传统PID积分有更好的控制效果，同时有保留了像PID一样易于调参的特性。

评价一款开源飞控是否成熟的标准是是否具有定位和可靠的定高能力，OLD-X飞控通过UKF算法完成加速度计和光流模块的融合实现室内外稳定的悬停定位，在目前的测试中OLD-X飞控能有效抵御3级的风力稳定悬停，在定高方面OLD-X飞控基于EKF融合气压计与加速度计，而市面上其他开源飞控采用互补滤波融，OLD-X飞控具有更好悬停定高和高速航线飞行性能，在高速侧飞时OLD-X飞控吊高不明显在高速刹车制动时也不会突然吊高而这是其他开源飞控难以媲美的。
OLD-X飞控采用模块化设计IMU，飞控，数传划分清晰不像其他飞控在更新硬件时需要整体升级价格过高，同时通过不同价格等级的模块可以满足不同用户的需求，同时OLD-X飞控另一大特点是SWD配套USB转接模块进行程序下载，不同与其他开源飞控下载和调试程序时要繁琐地查看引脚顺序OLD-X飞控只需要插上USB既可以完成程序的下载同时具有良好的抗干扰性能，未来团队还将更新GPS导航及视觉导航等功能请大家期待。

2.团队介绍

云逸团队成员来自于北京理工大学自动化学院，成员中有多名硕士以及一名博士专注于多旋翼飞行器飞控和视觉导航技术。团队参加了多届中航工业杯无人机大赛（最高荣誉旋翼组 三等奖）积累了很多无人机相关技术，目前主要着眼于飞行器的室内定位，自主精确降落和轨迹跟踪控制。
我们希望OLD-X飞控能成为像PX4一样的高兼容性，高开放性，高可靠性平台，为多旋翼飞行器爱好者提供一个与PX4性能相当但接口更清晰，代码编程更简洁，易于二次开发的平台。

3.OLD-X飞控性能
硬件规格
处理器型号	STM32F407VGT6 + STM32F407ZET6
STM32F103C8
处理器内核	32Bit ARM Cortex-M4 168MHZ
32Bit ARM Cortex-M3 72MHZ
陀螺仪加速度计	MPU6050
磁力计	HMC5833L
气压计	MS5611
SWD下载口	Micro USB
通讯接口	Uart
电源接口	5V -DC Input
PWM输出通道数	6通道
PWM输入通道数	8通道
基本参数
飞行器类型	X型 四旋翼，六旋翼飞行器
最大轴距	≤1200mm
电调类型	400Hz PWM输出频率
接收机类型	PWM接收机
工作电源	4.5~5.4V
典型功耗	1.25W
最大控制角度	35°
抗风能力	3级
最大垂直速度	1.2m/s
最大水平速度	10m/s
悬停高度精度	±10cm
悬停位置精度	±15cm（短期±5cm）
工作环境
工作温度	-15~35℃
工作湿度	10%~90%
储存温度	-25~40℃
储存湿度	10%~95%
软件参数
操作系统	UcosII （系统频率200Hz）
姿态解算算法	梯度下降法 或 互补滤波
姿态控制算法	自抗扰+PID
高度融合算法	EKF（UKF）
高度控制算法	串级PID 或 自抗扰+PID
光流融合算法	UKF
位置控制算法	串级PID
调参方式	安卓远程App实时调参

（1）OLD-X IMU模块


OLD-X IMU为独立惯性传感器件，板载加速度计陀螺仪，磁力计和气压计，预留接口连接光流传感器，超声波模块和GPS模块，OLD-X IMU模块基于梯度下降法进行姿态解算，基于EKF完成气压计和加速度计的融合，基于UKF完成光流数据和加速度计的融合，模块输出各融合数据同时输出原始传感器数据，目前模块只提供商业固件不提供商用固件的源码用户可以当做一个低成本惯性传感器模块使用，但是提供在OLD-X FC中采用IMU模块输出的传感器原始数据基于互补滤波的程序示例，用户可直接使用IMU模块输出的数据也可以自行编写滤波算法，OLD-X IMU与FC模块通过串口通讯波特率为576000L。
输出数据	数据频率
MPU6050原始数据	400Hz
HMC5833L原始数据	400Hz
Ms5611原始数据	200Hz
GPS定位原始数据	100Hz
光流模块原始数据	100Hz
超声波原始数据	100Hz
姿态解算数据（欧拉角，四元数）	400Hz
气压计高度融合数据（高度，速度）	200Hz
超声波高度融合数据（高度，速度）	200Hz
光流融合数据（机体速度，位置）	200Hz

IMU模块通讯格式请参考其他文档。
（2）OLD-X FC模块

OLD-X FC是OLD-X飞控的主控制器，模块外部介入IMU模块和NRF-飞控模块既可以完成飞行器的控制，同时还能介入SD模块完成对飞行数据的存储和波形的绘制，OLD-X FC模块板载蓝牙4.0模块可以与PC通讯实现波形的显示方便调参，默认情况下OLD-X FC模块使用IMU输出的融合数据作为控制反馈量，使用自抗扰+PID完成姿态控制，使用串级PID或自抗扰+PID完成飞行器高度控制，使用串级PID完成光流位置控制，模块提供所有源码进行二次开发。

OLD-X FC基于UcosII操作系统采用多线程的并行处理飞控各算法。具有快速、高效和稳定的特点。
姿态控制
外环自抗扰	200Hz
内环PID	400Hz
高度控制
外环PID （自抗扰）	100Hz
内环PID	250Hz
位置控制
外环PD	100Hz
内环PID	200Hz



图1. PC端波形实时显示（飞行器X轴轨迹跟踪 单位mm）
（3）NRF-飞控 模块


NRF-飞控模块与FC模块连接，采集外部接收机遥控信号完成飞行器控制，同时通过2.4G射频通讯与地面NRF模块完成与安装APP的通讯实现手机端波形显示和实时参数调节。
模块通道映射关系为
通道1	横滚通道
通道2	俯仰通道
通道3	油门通道
通道4	偏航通道
通道5	定高模式通道
通道6	手动，定点模式切换通道
通道7	使能远程调参通道
通道8	飞控解锁通道


（4）NRF-地面 模块

NRF-地面模块在连接蓝牙模块后可以与安卓手机APP通讯，波特率为9600，传递飞控模式和PID参数设置给FC模块，接受飞控姿态，高度和通用波形数据发送给APP。










（5）SD黑匣子 模块

SD黑匣子与FC模块连接用户可以按照例子自行选择要对什么数据进行存储，同时我们提供Matlab绘图程序帮助把SD卡中的txt文件绘制成波形数据方便分析。


（6）超声波壁障模块——开发中

超声波壁障模块共使用8个超声波模块，模块以30°均匀分布能实时测量360°的障碍物距离为飞行器安全飞行提供有效的保障，模块一体设计采用微型US100超声波模块稳定测量10cm~400cm的障碍物。

4.性能展示
（1）光流悬停
视频连接：
http://v.youku.com/v_show/id_XMTM4NTEwNTUxMg==.html?beta&
http://v.youku.com/v_show/id_XMTYyODkzNDQyNA==.html?beta&
http://v.youku.com/v_show/id_XMTcwMjE3Nzg0NA==.html?beta&
（2）视觉引导精确降落
视频连接：
http://v.youku.com/v_show/id_XMTcwMDM0MDQ3Ng==.html?beta&
（3）中航工业杯宣传视频
视频连接：
http://v.youku.com/v_show/id_XMTM3OTQ3NjY1Mg==.html?from=y1.7-2
（4）超声波悬停定高
视频连接：
http://v.youku.com/v_show/id_XMTcwMjE4MDQwNA==.html
（5）高速飞行制动
视频连接：
http://v.youku.com/v_show/id_XMTcwMjE4MDQxNg==.html?beta&


5.附录
阿莫电子论坛教程帖子地址
（1）磁力计融合问题
http://www.amobbs.com/thread-5614640-1-1.html
（2）px4flow应用
http://www.amobbs.com/thread-5625013-1-1.html
（3）气压计超声波自适应滤波
http://www.amobbs.com/thread-5628809-1-1.html
（4）PID完美调参
http://www.amobbs.com/thread-5628433-1-1.html
（5）PX4hawk  EKF姿态解算移植结果
http://www.amobbs.com/thread-5631742-1-1.html
（6）OLD-X飞控第一版宣传
http://www.amobbs.com/thread-5635352-1-1.html
（7）OLD-X光流模块宣传
http://www.amobbs.com/thread-5637028-1-1.html
http://www.amobbs.com/thread-5638992-1-1.html
（8）MS5611应用问题
http://www.amobbs.com/thread-5638142-1-1.html
