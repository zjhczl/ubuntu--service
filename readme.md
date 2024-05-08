# ubuntu安装service
## service文件
```
[Unit]
Description=ros-rtk2
After=network.target B.service
Requires=B.service

[Service]
Type=simple
ExecStart=/bin/bash -c 'source /opt/ros/noetic/setup.bash; source /ssd/catkin_ws/devel/setup.bash; roslaunch fixposition_driver_ros1 tcp.launch'
Restart=always
RestartSec=5


[Install]
WantedBy=multi-user.target

```
## 执行命令
重新加载 systemd 配置：
```
sudo systemctl daemon-reload
```
启动服务：
```
sudo systemctl start ros-recorder
```
设置服务开机启动：
```
sudo systemctl enable ros-recorder
```
检查服务状态：
```
sudo systemctl status ros-recorder
```
检查日志：
```
journalctl -u bag-zip.service
```
