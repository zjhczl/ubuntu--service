# ubuntu安装service
## service文件1
```
[Unit]
Description=Setup NAT and wlan0 interface
Wants=network.target network-online.target
Before=network.target

[Service]
Type=oneshot
ExecStart=/sbin/ifconfig wlan0 192.168.200.1
ExecStart=/usr/bin/systemctl restart hostapd
ExecStart=/bin/bash -c 'nft add table nat'
ExecStart=/sbin/iptables -t nat -A POSTROUTING -o usb0 -j MASQUERADE

[Install]
WantedBy=multi-user.target
```
## service文件2
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
## 文件3
开机后定时启动服务
sudo vim /etc/systemd/system/share-net.service
```
[Unit]
Description=Setup NAT and wlan0 interface

[Service]
Type=oneshot
ExecStart=/sbin/ifconfig wlan0 192.168.200.1
ExecStart=/usr/bin/systemctl restart hostapd
ExecStart=/bin/bash -c 'nft add table nat'
ExecStart=/sbin/iptables -t nat -A POSTROUTING -o usb0 -j MASQUERADE
```
sudo vim /etc/systemd/system/nat-service.timer
```
[Unit]
Description=Run nat-service.service after 10 seconds of boot

[Timer]
OnBootSec=10
Unit=share-net.service

[Install]
WantedBy=timers.target
```
```
sudo systemctl enable nat-service.service
sudo systemctl enable nat-service.timer
sudo systemctl start nat-service.timer

```

### 5. 启动定时器

立即启动定时器以应用更改：

```sh
sudo systemctl start nat-service.timer
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
