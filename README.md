# intel_realsense_ros
Instructions to get Intel realsense D400 series cameras working with ROS


For Ubuntu 16.04

LIBREALSENSE SDK:

1. ```
	sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
```
2. Make sure the kernel version is >4.4 (check kernel version with "uname -r" command.
Any of these versions work(4.4.0-.., 4.8.0-.., 4.10.0-.. , 4.13.0-..or 4.15.0).
3. Download and unzip https://github.com/IntelRealSense/librealsense.git
4.Navigate to librealsense root directory to run the following scripts.
   (Unplug the camera if connected)
5. sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev
6. sudo apt-get install libglfw3-dev
7.Requires cmake version 3.8+ which is currently not made available via apt manager for Ubuntu LTS.   
Go to the [official CMake site](https://cmake.org/download/) to download and install the latest cmake version.
9.Install Intel Realsense permission scripts located in librealsense source directory:
  
   a). cd to the extracted librealsense root folder
   b). sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/
   c). sudo udevadm control --reload-rules && udevadm trigger
   d). ./scripts/patch-realsense-ubuntu-lts.sh

10.Tracking Module requires hid_sensor_custom kernel module to operate properly.
    a).  echo 'hid_sensor_custom' | sudo tee -a /etc/modules
11.MAke sure gcc version is >=5.0 by typing "gcc -v"
12.Navigate to librealsense root directory and run:
     mkdir build && cd build
13.cmake ../ -DBUILD_EXAMPLES=true -DBUILD_GRAPHICAL_EXAMPLES=false (For systems without OpenGL)
14.Recompile and install librealsense binaries:

	sudo make uninstall && make clean && make && sudo make install

KERNEL PATCH:

15.sudo apt-key adv --keyserver hkp://keys.gnupg.net:80 --recv-key C8B3A55A6F3EFCDE
16.sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u
17.sudo rm -f /etc/apt/sources.list.d/realsense-public.list
18.sudo apt-get update
19.sudo apt-get install librealsense2-dkms
20.sudo apt-get install librealsense2-utils
21.sudo apt-get install librealsense2-dev
22.sudo apt-get install librealsense2-dbg
23. Verify that the kernel is updated :
	modinfo uvcvideo | grep "version:" 
(should include realsense string)


ROS WRAPPER:


1. mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src/
2.Clone the files from https://github.com/intel-ros/realsense/releases to catkin_ws/src
3.	catkin_init_workspace 
	cd ..
	catkin_make clean
	catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
	catkin_make install
	echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
	source ~/.bashrc


---------------------------------------------------------------------------------------------------
roslaunch realsense2_camera rs_camera.launch
OR
roslaunch realsense2_camera rs_rgbd.launch
Visualize the topics using rviz
---------------------------------------------------------------------------------------------------

References:
1. https://github.com/IntelRealSense/librealsense/blob/development/doc/installation.md

2. https://github.com/intel-ros/realsense
