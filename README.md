# This repository has been moved to a ROS repository at [ros-perception/perception_open3d](https://github.com/ros-perception/perception_open3d). All users are requested to switch to using the new repository.

Please note the change in the namespace of the functions to `open3d_conversions` in the new repository from `open3d_ros` in this repository.

Refer [#6](https://github.com/unr-arl/open3d_ros/issues/6) and [#7](https://github.com/unr-arl/open3d_ros/issues/7)

# open3d_ros

This package provides functions that can convert pointclouds from ROS to Open3D and vice-versa.

## Dependencies

* Eigen3
* Open3D

## System Requirements

* Ubuntu 18.04+: GCC 5+

## Installation

### Open3D

```bash
git clone --recursive https://github.com/intel-isl/Open3D
cd Open3D && source util/scripts/install-deps-ubuntu.sh
mkdir build && cd build
cmake -DBUILD_EIGEN3=ON -DBUILD_GLEW=ON -DBUILD_GLFW=ON -DBUILD_JSONCPP=ON -DBUILD_PNG=ON -DGLIBCXX_USE_CXX11_ABI=ON -DPYTHON_EXECUTABLE=/usr/bin/python -DBUILD_UNIT_TESTS=ON ..
make -j4
sudo make install
```

* You may need to upgrade `cmake` on your system. This can be done as follows

    ```bash
    sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'
    sudo apt-get update
    sudo apt-get install cmake
    ```

* These instructions for installation are compiled from the [official instructions](http://www.open3d.org/docs/release/compilation.html) and [this github issue](https://github.com/intel-isl/Open3D/issues/414).

### open3d_ros

#### From Source

```bash
mkdir -p catkin_ws/src
cd catkin_ws/src
git clone git@github.com:unr-arl/open3d_ros.git
cd ..
catkin config -DCMAKE_BUILD_TYPE=Release
catkin build open3d_ros
```

* Time taken for the conversion functions will be much larger if the package is not built in `Release` mode.

## Usage

There are two functions provided in this library:

```cpp
void open3d_ros::open3dToRos(const open3d::geometry::PointCloud& pointcloud, sensor_msgs::PointCloud2& ros_pc2, std::string frame_id = "open3d_pointcloud");

void open3d_ros::rosToOpen3d(const sensor_msgs::PointCloud2ConstPtr& ros_pc2, open3d::geometry::PointCloud& o3d_pc, bool skip_colors=false);
```

Their usage can be seen in [`examples/ex_pc2_subscribe.cpp`](examples/ex_pc2_subscribe.cpp) and [`examples/ex_pcd_publish.cpp`](examples/ex_pcd_publish.cpp)

* As Open3D pointclouds only contain `points`, `colors` and `normals`, the interface currently supports XYZ, XYZRGB pointclouds. XYZI pointclouds are handled by placing the `intensity` value in the `colors_`.
* On creating a ROS pointcloud from an Open3D pointcloud, the user is expected to set the timestamp in the header and pass the `frame_id` to the conversion function.

## Documentation

Documentation can be generated using Doxygen and the configuration file by executing  `doxygen Doxyfile` in the package.

## Contact

Feel free to contact us for any questions:

* [Pranay Mathur](mailto:matnay17@gmail.com)
* [Nikhil Khedekar](mailto:nkhedekar@nevada.unr.edu)
* [Kostas Alexis](mailto:kalexis@unr.edu)
