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
git clone https://github.com/marcinmatecki/LeGO-LOAM-to-hdmapping.git --recursive
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
## Modified for build

**Reference issue:**  

[RobustFieldAutonomyLab/LeGO-LOAM#257](https://github.com/RobustFieldAutonomyLab/LeGO-LOAM/issues/257)               
[catkin build fails -278](https://github.com/RobustFieldAutonomyLab/LeGO-LOAM/issues/278)


**Changes made:**

 
   ```
  File:

  test_ws/src/LeGO-LOAM-to-hdmapping/src/LeGO-LOAM/LeGO-LOAM/CMakeLists.txt

   cmake
   find_package(Boost REQUIRED COMPONENTS serialization thread timer chrono)

    Linked Boost libraries properly

link_directories(${Boost_LIBRARY_DIRS})
target_link_libraries(mapOptimization ${catkin_LIBRARIES} ${Boost_LIBRARIES})

Updated C++ standard

Replaced:

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")

With:

    set(CMAKE_CXX_STANDARD 14)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)

File:

test_ws/src/LeGO-LOAM-to-hdmapping/src/LeGO-LOAM/LeGO-LOAM/include/utility.h

Changes made:

#include <opencv/cv.h> to #include <opencv2/opencv.hpp>

    Added  Eigen includes

#include <eigen3/Eigen/Core>
#include <eigen3/Eigen/Dense>
#include <eigen3/Eigen/Geometry>
