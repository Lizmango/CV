##docker 使用指南
1. 调用docker
` docker start ros-kinetic `    
___
2. Run find object:
```
$ roscore &
# Launch your preferred usb camera driver
$ sudo chmod a+rwx /dev/video0
$ roslaunch video_stream_opencv camera.launch video_stream_provider:=0
$ rosrun find_object_2d find_object_2d image:=/camera/image_raw
```
3. 软件安装：
```
apt get-install... 
```

4. 文件下载：
```
git clone...
```
