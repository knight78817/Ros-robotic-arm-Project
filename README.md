# Ros-robotic-arm-Project
自學如何連接Ros跟機械手臂的專案

目前是用dobot來做練習


由下方這個指令我發現可以看到
howder@howder-UP-CHT01:~$ ls /dev -l

show出下方的資訊
其中ttyACM0的時間是20:19 ，我發現這就是我剛剛插上的usb（arduino 2560）

crw--w----    1 root tty            4,     8 10月 14 17:05 tty8
crw--w----    1 root tty            4,     9 10月 14 17:05 tty9
crw-rw-rw-  1 root dialout 166,     0 10月 14 20:19 ttyACM0
crw-------     1 root root          5,     3 10月 14 17:05 ttyprintk

由這個網站發現我其實沒有把usb的一些權限打開

https://forum.dobot.cc/t/invalid-port-name-or-dobot-is-occupied-by-other-application/671


howder@howder-UP-CHT01:~/catkin_ws/src/Howder_robot_arm/Ros-robotic-arm-Project/dobot_ws$ rosrun dobot DobotServer /dev/ttyUSB0
CDobotConnector : QThread(0x11a2bf0)
CDobotProtocol : QThread(0x11a3d90)
CDobotCommunicator : QThread(0x11a3db0)
[ERROR] [1539519556.902151382]: Invalid port name or Dobot is occupied by other application!

 sudo chmod a+rw /dev/ttyACM0

之後再輸入指令就可以running了

howder@howder-UP-CHT01:~$ rosrun dobot DobotServer ttyACM0
CDobotConnector : QThread(0x2839bf0)
CDobotProtocol : QThread(0x283ad90)
CDobotCommunicator : QThread(0x283adb0)
[ INFO] [1539520279.816140194]: Dobot service running...


但是我之後照着說明去做，發現到
$ rosrun dobot DobotClient_JOG
Reading from keyboard
Use WASD keys to control the robot
Press Shift to move faster
[ INFO] [1539526869.211681081]: W
[ INFO] [1539526872.221056803]: Result:2
[ INFO] [1539526872.221227907]: W
[ INFO] [1539526875.227177474]: Result:2
[ INFO] [1539527289.189618268]: W
[ INFO] [1539527292.198675733]: Result:2
[ INFO] [1539527292.198849400]: W
[ INFO] [1539527295.208617831]: Result:2

terminal有反應但是手臂沒有反應

所以目前先去用windows的看看能不能先讓手臂動起來

先用arduino試試能不能通過tx rx控制

https://cn.dobot.cc/tutorial/2224.html

測試之後還是不行

///

20181016
找到下方的網站（V1）的版本

https://www.dobot.cc/downloadcenter/dobot-arm-v1.html?sub_cat=100#sub-download

其中有下載到資料夾DobotTools_opensource

DobotClient
DobotDownloadUtil
DobotServer

原本以爲需要在windows下才能編譯
而裏面都有 *.pro檔案

https://stackoverflow.com/questions/1368512/what-is-the-purpose-of-the-pro-file
https://doc.qt.io/archives/3.3/qmake-manual-3.html

而*.pro檔案可以使用 “qmake”指令產生makefile
再下make指令就可以編譯
但是編譯時候有error，由於沒有qt的問題

[
howder@howder-UP-CHT01:~/下載/DobotTools_opensource/DobotDownloadUtil$ make
g++ -c -m64 -pipe -O2 -Wall -W -D_REENTRANT -DQT_NO_DEBUG -DQT_GUI_LIB -DQT_CORE_LIB -DQT_SHARED -I/usr/share/qt4/mkspecs/linux-g++-64 -I. -I/usr/include/qt4/QtCore -I/usr/include/qt4/QtGui -I/usr/include/qt4 -I. -o MainWindow.o src/MainWindow.cpp
src/MainWindow.cpp:21:21: fatal error: QtWidgets: 沒有此一檔案或目錄
 #include <QtWidgets>
                     ^
compilation terminated.
Makefile:231: recipe for target 'MainWindow.o' failed
make: *** [MainWindow.o] Error 1
]
 
 因此我去安裝qt
 
 https://www.linuxidc.com/Linux/2017-03/141553.htm
 
 http://download.qt.io/archive/qt/5.10/5.10.1/

