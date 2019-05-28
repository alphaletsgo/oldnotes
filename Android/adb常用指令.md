# adb 常用指令
## 设备相关

```shell help
adb devices //显示连接到计算机的设备
adb shell //进入设备的shell界面[多个设备情况下：adb -s <设备序列号> shell]
exit //退出设备的shell界面
adb reboot //重启设备
adb kill-server //终止adb服务进程
adb start-server //重启adb服务进程
adb connect <device-ip-address> //连接到指定的ip,这个通常配合wifidebug，比如adb connect 127.0.0.1:5037,5037是默认端口号，海马模拟器是adb connect 127.0.0.1:26944
adb disconnect <device-ip-address> //adb disconnect 127.0.0.1:26944

```
**应用管理**


##获得activity堆栈信息
**查看当前activity**
```shell
adb shell dumpsys activity
```
**查看activities栈**
```shell
adb shell dumpsys activity activities
```
**查看当前前台的activity**
```shell
adb shell dumpsys activity | grep mFocusedActivity
```

- https://www.jianshu.com/p/0693b841c83b
- https://developer.android.com/studio/command-line/adb#wireless