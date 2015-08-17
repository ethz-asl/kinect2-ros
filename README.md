# kinect2-ros

## Synopsis

Tools for using the Kinect One/v2 in ROS.

**Author(s):** Thiemo Wiedemeyer, Ralf Kaestner

**Maintainer:** Ralf Kaestner <ralf.kaestner@gmail.com>

**License:** Apache License

**Operating system(s):** Debian-based Linux

**Package PPA:** ppa:ethz-asl/ros

## Description

This project contains a collection of tools and libraries for using the Kinect
One/v2 in ROS. In particular, it provides:

* A [calibration tool](https://github.com/ethz-asl/kinect2-ros/tree/devel/kinect2_calibration)
  for calibrating the IR sensor of the Kinect One/v2 to the RGB sensor
* A [library](https://github.com/ethz-asl/kinect2-ros/tree/devel/kinect2_depth_registration)
  for depth registration with OpenCL support
* The [bridge](https://github.com/ethz-asl/kinect2-ros/tree/devel/kinect2_bridge)
  between [libfreenect2](https://github.com/OpenKinect/libfreenect2) and
  [ROS](http://www.ros.org/)
* A [viewer](https://github.com/ethz-asl/kinect2-ros/tree/devel/kinect2_registration_viewer)
  for the images/point clouds

This project has been forked from the original Kinect One/v2 ROS integration
published under http://github.com/code-iai/iai_kinect2 in order to maintain
Debian packaging and some feature additions.

### Publications

If you use this work in an academic context, please cite the following publication:

P. Fankhauser, M. Bloesch, D. Rodriguez, R. Kaestner, M. Hutter, and R. Siegwart,
**"Kinect v2 for Mobile Robot Navigation: Evaluation and Modeling"**,
in IEEE International Conference on Advanced Robotics (ICAR), 2015. ([PDF](http://leggedrobotics.ethz.ch/lib/exe/fetch.php?media=research:kinect_v2_evaluation.pdf))

    @inproceedings{Fankhauser2015KinectV2ForMobileRobotNavigation,
      author = {Fankhauser, Péter and Bloesch, Michael and Rodriguez, Diego and and Kaestner, Ralf and Hutter, Marco and Siegwart, Roland},
      title = {Kinect v2 for Mobile Robot Navigation: Evaluation and Modeling},
      booktitle = {IEEE International Conference on Advanced Robotics (ICAR)},
      year = {2015}
    }

## Installation

Here, we assume you intend to build/install the project for the ROS
distribution named `ROS_DISTRO`.

### Installing from packages (recommended for Ubuntu LTS users)

The maintainers of this project provide binary packages for the latest Ubuntu
LTS releases and commonly used system architectures. To install these packages,
you may follow these instructions:

* Add the backports PPA and the drivers PPA to your APT sources by issuing 

  ```
  sudo add-apt-repository ppa:ethz-asl/backports
  sudo add-apt-repository ppa:ethz-asl/drivers
  ```

  on the command line

* Add the project PPA to your APT sources by issuing 

  ```
  sudo add-apt-repository ppa:ethz-asl/ros
  ```

  on the command line

* To re-synchronize your package index files, run

  ```
  sudo apt-get update
  ```

* Install all project packages and their dependencies through

  ```
  sudo apt-get install ros-ROS_DISTRO-kinect2
  ```

  or selected packages using your favorite package management tool

### Building from source

This project may be built using the CMake build system with the ROS
macro extension called [catkin](http://wiki.ros.org/catkin).

#### Installing build dependencies

Most build dependencies of this project are available from the standard
package repositories of recent Ubuntu and ROS releases. To install them,
simply use the command

```
sudo apt-get install libopencv-de libpcl-1.7-all-dev ros-ROS_DISTRO-roscpp, ros-ROS_DISTRO-rostime, ros-ROS_DISTRO-std-msgs, ros-ROS_DISTRO-sensor-msgs, ros-ROS_DISTRO-message-filters, ros-ROS_DISTRO-cv-bridge, ros-ROS_DISTRO-image-transport, ros-ROS_DISTRO-compressed-image-transport, ros-ROS_DISTRO-compressed-depth-image-transport

```

The Kinect v2 driver library (and its dependencies) may conveniently be
installed from the package PPAs of the project maintainers. To install them,
you may follow these instructions:

* Add the backports PPA and the drivers PPA to your APT sources by issuing 

  ```
  sudo add-apt-repository ppa:ethz-asl/backports
  sudo add-apt-repository ppa:ethz-asl/drivers
  ```

  on the command line

* To re-synchronize your package index files, run

  ```
  sudo apt-get update
  ```

* Install the required dependencies through

  ```
  sudo apt-get install libfreenect2-dev
  ```

  or using your favorite package management tool

#### Building with catkin (recommended for ROS developers)

Assuming that you have cloned the project sources into `PROJECT_DIR`, you
may attempt to build this project using catkin as follows:

* Create the directory structure for your catkin workspace by issuing

  ```
  mkdir -p CATKIN_WS_DIR/src
  ```

* Assuming that your ROS environment has been set up properly, initialize the
  catkin workspace using the command

  ```
  catkin_init_workspace CATKIN_WS_DIR/src
  ```

* Individually link the directories containing the project's package source
  trees into your catkin workspace source directory through

  ```
  ln -sf PROJECT_DIR/kinect2* CATKIN_WS_DIR/src
  ```

* Switch into the catkin workspace directory by 

  ```
  cd CATKIN_WS_DIR
  ```

* In the catkin workspace directory, run 

  ```
  catkin_make
  ```

  to start the build
  
## Usage

    roslaunch kinect2_bridge kinect2_bridge.launch

There are several options to configure the kinect2_bridge. These can be set as argument, for example

    roslaunch kinect2_bridge kinect2_bridge.launch publish_tf:=true fps_limit:=10


Commonly used options are:

- **`publish_tf`** (\<bool>, default: false)

  Set to true to publish the tf frames.

- **`fps_limit`** (\<float>, default: -1)

  Limiting the frame rate (frames per second). Set to -1 for unlimited.

- **`scale`** (\<float>, default: -1)

  If greater than zero, apply scale to yield scaled color and depth image
  topics. Otherwise, scaled color and depth image topics have the native
  resolution of each image stream.

- **`calib_path`** (\<string>, default: "$(find kinect2_bridge)/data/")

  Path to calibration files.

- **`depth_method`** (default|cpu|opengl|opencl, default: default)

  Selects the depth processing method.

- **`reg_enabled`** (\<bool>, default: true)

  Set to true to activate depth/color registration.

- **`reg_method`** (default|cpu|opencl, default: default)

  Selects the depth/color registration method.

- **`reg_cl_source`** (\<string>, default: "$(find kinect2_depth_registration)/opencl/depth_registration.cl")

  Path to the OpenCL program for depth/color registration.

For common pitfalls, check out [this list from libfreenect2](https://github.com/ethz-asl/libfreenect2/blob/remake/README.md#common-pitfalls).

## API documentation

This project does not yet provide any API documentation.

## Feature requests and bug reports

If you would like to propose a feature for this project, please consider
contributing or send a feature request to the project authors. Bugs may be
reported through the project's issue page.

## Further reading

For additional information of the Robot Operating System (ROS), please refer
to the official [ROS documentation](http://wiki.ros.org).
