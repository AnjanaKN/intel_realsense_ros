# intel_realsense_ros
Instructions to get Intel realsense D400 series cameras working with ROS


For Ubuntu 16.04

LIBREALSENSE SDK:

1. ```sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade```

2. Make sure the kernel version is >4.4 (check kernel version with ```uname -r``` command.
Any of these versions work(4.4.0-.., 4.8.0-.., 4.10.0-.. , 4.13.0-..or 4.15.0).
3. Download and unzip https://github.com/IntelRealSense/librealsense.git
4.Navigate to librealsense root directory to run the following scripts.
   (Unplug the camera if connected)
 ```sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev
    sudo apt-get install libglfw3-dev
 ```
 
5.Requires cmake version 3.8+ which is currently not made available via apt manager for Ubuntu LTS.   
Go to the [official CMake site](https://cmake.org/download/) to download and install the latest cmake version.

6.Install Intel Realsense permission scripts located in librealsense source directory.
cd to the extracted librealsense root folder:

     ```sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/
        sudo udevadm control --reload-rules && udevadm trigger
         ./scripts/patch-realsense-ubuntu-lts.sh```

7. Tracking Module requires hid_sensor_custom kernel module to operate properly:
    ``` echo 'hid_sensor_custom' | sudo tee -a /etc/modules```
8.Make sure gcc version is >=5.0 by typing ```gcc -v```
9.Navigate to librealsense root directory and run:
     ```mkdir build && cd build```
10.Make
```.cmake ../ -DBUILD_EXAMPLES=true -DBUILD_GRAPHICAL_EXAMPLES=false (For systems without OpenGL)```
11.Recompile and install librealsense binaries:

	sudo make uninstall && make clean && make && sudo make install

KERNEL PATCH:
1. 
   ```sudo apt-key adv --keyserver hkp://keys.gnupg.net:80 --recv-key C8B3A55A6F3EFCDE
   sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u
   sudo rm -f /etc/apt/sources.list.d/realsense-public.list
   sudo apt-get update```
2.
      sudo apt-get install librealsense2-dkms
      sudo apt-get install librealsense2-utils
      sudo apt-get install librealsense2-dev
      sudo apt-get install librealsense2-dbg


3. Verify that the kernel is updated(should include realsense string) :
       ```modinfo uvcvideo | grep "version:"```



ROS WRAPPER:

1.
``` mkdir -p ~/catkin_ws/src```
```cd ~/catkin_ws/src/```
2. Clone the files from https://github.com/intel-ros/realsense/releases to catkin_ws/src
     ```catkin_init_workspace 
	cd ..
	catkin_make clean
	catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
	catkin_make install
	echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
	source ~/.bashrc```


---------------------------------------------------------------------------------------------------
```roslaunch realsense2_camera rs_camera.launch```
OR
```roslaunch realsense2_camera rs_rgbd.launch```
Visualize the topics using rviz
---------------------------------------------------------------------------------------------------

References:
1. https://github.com/IntelRealSense/librealsense/blob/development/doc/installation.md

2. https://github.com/intel-ros/realsense
