# LeGO-LOAM-converter

## Intended use 

This small toolset allows to integrate SLAM solution provided by [LeGO-LOAM](https://github.com/RobustFieldAutonomyLab/LeGO-LOAM) with [HDMapping](https://github.com/MapsHD/HDMapping).
This repository contains ROS 1 workspace that :
  - submodule to tested revision of LeGO-LOAM
  - a converter that listens to topics advertised from odometry node and save data in format compatible with HDMapping.


## Building

Clone the repo
```shell
mkdir -p /test_ws/src
cd /test_ws/src
git clone https://github.com/marcinmatecki/LeGO-LOAM-to-hdmapping.git--recursive
cd ..
catkin_make
```

## Usage - data SLAM:

Prepare recorded bag with estimated odometry:

In first terminal record bag:
```shell
rosbag record /registered_cloud /aft_mapped_to_init
```

and start odometry:
```shell 
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
roslaunch lego_loam run.launch
rosbag play *.bag --clock --topic /velodyne_points /imu/data
```

## Usage - conversion:

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
rosrun lego-loam-to-hdmapping listener <recorded_bag> <output_dir>
```













# LeGO-LOAM-to-hdmapping

https://github.com/RobustFieldAutonomyLab/LeGO-LOAM/issues/257
find_package(Boost REQUIRED COMPONENTS serialization thread timer chrono)
${Boost_LIBRARY_DIRS} 

----------------------------
#include <eigen3/Eigen/Core>
#include <eigen3/Eigen/Dense>
#include <eigen3/Eigen/Geometry>
