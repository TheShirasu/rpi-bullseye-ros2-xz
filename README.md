# rpi-bullseye-ros2-xz
Change compression type of rpi-bullseye-ros2 from zst to xz to fix "unknown compression for member 'control.tar.zst'" error.
Orignal project -> [rpi-bullseye-ros2](https://github.com/Ar-Ray-code/rpi-bullseye-ros2)

## error message
```
$ sudo apt install ./ros-iron-desktop-0.3.2_20230525_arm64.deb
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Note, selecting 'ros-iron-desktop' instead of './ros-iron-desktop-0.3.2_20230525_arm64.deb'
The following NEW packages will be installed:
  ros-iron-desktop
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 0 B/78.7 MB of archives.
After this operation, 0 B of additional disk space will be used.
Get:1 /home/shirasu/ros-iron-desktop-0.3.2_20230525_arm64.deb ros-iron-desktop arm64 0.3.2 [78.7 MB]
dpkg-deb: error: archive '/home/shirasu/ros-iron-desktop-0.3.2_20230525_arm64.deb' uses unknown compression for member 'control.tar.zst', giving up
dpkg: error processing archive /home/shirasu/ros-iron-desktop-0.3.2_20230525_arm64.deb (--unpack):
 dpkg-deb --control subprocess returned error exit status 2
Errors were encountered while processing:
 /home/shirasu/ros-iron-desktop-0.3.2_20230525_arm64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

## What did I do?
```
$ sudo apt install -y binutils
$ wget https://s3.ap-northeast-1.wasabisys.com/download-raw/dpkg/ros2-desktop/debian/bullseye/ros-iron-desktop-0.3.2_20230525_arm64.deb
$ ar x ros-iron-desktop-0.3.2_20230525_arm64.deb
$ unzstd control.tar.zst
$ unzstd data.tar.zst
$ xz control.tar
$ xz -v -T4 data.tar
$ ar cr ros-iron-desktop-0.3.2_20230525_arm64_xz.deb debian-binary control.tar.xz data.tar.xz
```

reference(japanese)-> [RaspberryPi OSではじめるROS2  Chapter 04 ROS2の環境構築 by Ar-Ray-code](https://note.com/ryonakano/n/n2809a750be28#9ef9d095-434b-4039-8a12-03870db1fe29)

# License
The source code is licensed MIT. The website content is licensed CC BY 4.0,see LICENSE.
