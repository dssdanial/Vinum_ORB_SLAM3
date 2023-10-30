## Problems and Solutions

When I downloaded the ORB_SLAM package from many common repositories, in the Examples folder, there was not any ROS packages like package.xml, etc. There was only some files in the folder Examples_old, but they were also not the ccrrect package.
So, I biult them and made some changes. `https://github.com/UZ-SLAMLab/ORB_SLAM3`

- (1) in the file biuld_ros.sh, I change the line 3 to this `cd Examples_old/ROS/ORB_SLAM3`.

    - Then, after building biuld.sh, I put this line in the .bashrc.sh: 

    `export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:${PWD}/catkin_ws/src/ORB_SLAM3/Examples_old/ROS/ORB_SLAM3`


- (2) Then,to solve the problem of not recognising the OpenCV versions in Ubuntu while installing ORB_SLAM3/ORB_SLAM2 in ROS-noetic in Ubuntu 20.04:
    `first if the error is OpenCV > 4.2 not found:`
  
    - Solution: find the opencv package path in your system by:
      
        ``` dpkg-query -S libopencv_core.so ```
      
        - then add  it to the CMakeLists.txt in ORB_SLAM directory:
      
        ``` set(OpenCV_DIR /usr/lib/x86_64-linux-gnu/libopencv_core.so) ```
 
        - then build the `./build.sh` again.
     
- (3) Then, if you found the current problem:

    ```
       /usr/local/include/pangolin/display/widgets.h:153:50: required from here /usr/local/include/sigslot/signal.hpp:1180:65: error: ‘slots_reference’ was not declared in this scope 1180 | cow_copy_type<list_type, Lockable> ref = slots_reference(); 
    ```
    
  - Solution:
        
        
        ``` sed -i 's/++11/++14/g' CMakeLists.txt ```
        
  - then re-run `./build.sh`
    


 - (4) Then, if you have faced with the following problem:

    ```
    ResourceNotFound: rgbd_launch
    ROS path [0]=/opt/ros/kinetic/share/ros
    ROS path [1]=/home/mypath/Projects/myproject/catkin_ws/src
    ROS path [2]=/opt/ros/kinetic/share
    ```
    - solution:
      ``` sudo apt install ros-noetic-rgbd-launch```
  
      ``` sudo apt-get update```

    
 

- (5) If the compilation reports an error, it is probably that `/ ros/noetic/share` has no `orb_` The soft connection of slam3 can be solved by executing the following commands

  ``` sudo ln -s "PATH"/ORB_SLAM3/Examples/ROS/ORB_SLAM3 /opt/ros/noetic/share/ORB_SLAM3```


- (6) If there is a problem with AR function, there are some modifications here in this page for some files in AR folder:
  
  https://github.com/UZ-SLAMLab/ORB_SLAM3/issues/479
 

