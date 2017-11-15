##docker 使用指南
1. 调用docker
```docker start ros-kinetic ``` 
注意：docker 的工作区间对应workspace，应保证调用的文件在此空间。   
___
2. Run find object:
```
$ roscore &
# Launch your preferred usb camera driver
$ sudo chmod a+rwx /dev/video0
$ roslaunch video_stream_opencv camera.launch video_stream_provider:=0
$ rosrun find_object_2d find_object_2d image:=/camera/image_raw
```

