# ROS2 Tictoc Profiler

This package is a ROS2 library-only port of Dr. Daniel Maturana's ROS1 **tictoc_profiler** package. Used by Dr. Yang's [CubeSLAM](https://github.com/shichaoy/cube_slam) project and Benchun Zhou's point-plane-object SLAM [point-plane-object-SLAM](https://github.com/benchun123/point-plane-object-SLAM). For a practical usecase, look into [sun_rgbd_driver.cpp](https://github.com/Mechazo11/ros2_monocular_object_detection/blob/main/src/sun_rgbd_driver.cpp) and [sun_rgbd_example_class.cpp](https://github.com/Mechazo11/ros2_monocular_object_detection/blob/main/src/sun_rgbd_example_class.cpp) from the [ros2_monocular_object_detection](https://github.com/Mechazo11/ros2_monocular_object_detection.git) pacakge

@author: Azmyin Md. Kamal

@job: Ph.D. student, [iCORE Lab](https://icorelab.github.io/), Department of MIE, Louisiana State University, Louisiana, USA

@date: 01/18/2024

## 0. Tested with
1. Boost 1.74 and Boost 1.84
2. Ubuntu 22.04 LTS (Jammy Jellyfish)
3. RO2 Humble Hawksbill (LTS)

## 1. Prerequisitis
### Boost 1.74 or later. 
If you need install a newer version, I found [tutorial](https://linux.how2shout.com/how-to-install-boost-c-on-ubuntu-20-04-or-22-04/) helpful 

## 2. Installation
```
cd ~ # move to /home directory
source /opt/ros/humble/setup.bash
mkdir -p ~/ros2_test/src
cd ~/ros2_test/src
git clone https://github.com/Mechazo11/ros2_tic_toc_profiler.git
cd .. 
colcon build --packages-select ros2_tictoc_profiler
source install/setup.bash
```

## 3. How to use?
* Add in CmakeLists.txt
```
find_package(ros2_tictoc_profiler REQUIRED) # ROS 2 library only package, port of Daniel Mataurna`s tictoc_profiler package
```
* Add dependency in packages.xml
```
<build_depend>ros2_tictoc_profiler</build_depend>
<exec_depend>ros2_tictoc_profiler</exec_depend>
```
* Follow the useage example linked from the ros_monocular_object_detection package

