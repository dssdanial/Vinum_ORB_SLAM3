# Vinum_ORB_SLAM3
Real-time tool for odometry and SLAM 

## Runing
```
./Examples/Stereo/stereo_euroc Vocabulary/ORBvoc.txt Examples/Stereo/EuRoC.yaml Examples/MH_05 Examples/Stereo/EuRoC_TimeStamps/MH05.txt f_dataset-MH05_stereo.txt 
```

## Plotting using Evo

### installing
```
pip install evo --upgrade --no-binary evo
```
**Results**
![ORB_runing](https://github.com/dssdanial/Vinum_ORB_SLAM3/assets/32397445/f43f8069-c26f-4cae-a4be-38c282804a1b)


## Evaluation

After estimating the camera trajectory of the Kinect and saving it to a file, we need to evaluate the error in the estimated trajectory by comparing it with the ground truth. There are different error metrics. Two prominent methods are the absolute trajectory error (ATE) and the relative pose error (RPE). The ATE is well-suited for measuring the performance of visual SLAM systems. In contrast, the RPE is well-suited for measuring the drift of a visual odometry system, for example, the drift per second. 

### 1- Absolute Trajectory Error (ATE)

The absolute trajectory error directly measures the difference between points of the true and the estimated trajectory. As a pre-processing step, we associate the estimated poses with ground truth poses using the timestamps. Based on this association, we align the true and the estimated trajectory using singular value decomposition. Finally, we compute the difference between each pair of poses and output the mean/median/standard deviation of these differences. 
### 2- Relative Pose Error (RPE)

For computing the relative pose error, we provide a script ''evaluate_rpe.py''. This script computes the error in the relative motion between pairs of timestamps. By default, the script computes the error between all pairs of timestamps in the estimated trajectory file. As the number of timestamp pairs in the estimated trajectory is quadratic in the length of the trajectory, it can make sense to downsample this set to a fixed number (–max_pairs). Alternatively, one can choose to use a fixed window size (–fixed_delta). In this case, each pose in the estimated trajectory is associated with a later pose according to the window size (–delta) and unit (–delta_unit). This evaluation technique is useful for estimating the drift. 

- [source](https://cvg.cit.tum.de/data)

### Results

Evaluation by:

```
python evaluate_ate.py  Examples/MH01/mav0/state_groundtruth_estimate0/data.csv  f_f_dataset-MH01_rgb.txt --plot plots.png

```
<p align="center">
<img src="https://github.com/dssdanial/Vinum_ORB_SLAM3/assets/32397445/7b77c68d-5aea-46b9-937b-f7e7f5abd9ca" width=400 height=300>
</p>

```HTML
evaluate_ate:
compared_pose_pairs 573 pairs
absolute_translational_error.rmse 0.020633 m
absolute_translational_error.mean 0.017740 m
absolute_translational_error.median 0.014959 m
absolute_translational_error.std 0.010535 m
absolute_translational_error.min 0.000917 m
absolute_translational_error.max 0.084605 m

evaluate_rpe:
ompared_pose_pairs 7252 pairs
translational_error.rmse 0.034664 m
translational_error.mean 0.029720 m
translational_error.median 0.026251 m
translational_error.std 0.017842 m
translational_error.min 0.000000 m
translational_error.max 0.201569 m
rotational_error.rmse 1.602068 deg
rotational_error.mean 1.384403 deg
rotational_error.median 1.253351 deg
rotational_error.std 0.806257 deg
rotational_error.min 0.000000 deg
rotational_error.max 7.338937 deg
```

## Generating a point cloud from images
The depth images are already registered to the color images, so the pixels in the depth image already correspond one-to-one to the pixels in the color image. Therefore, generating colored point clouds is straightforward. An example script is available in ''generate_pointcloud.py'', which takes a color image and a depth map as input, and generates a point cloud file in the PLY format.
``` python
positional arguments:
  rgb_file    input color image (format: png)
  depth_file  input depth image (format: png)
  ply_file    output PLY file (format: ply)

```
```
python generate_pointcloud.py Examples/rgbd_dataset_freiburg1_desk/rgb/1305031452.791720.png Examples/rgbd_dataset_freiburg1_desk/depth/1305031454.072690.png output.ply
```
<img src="https://github.com/dssdanial/Vinum_ORB_SLAM3/assets/32397445/d51a6332-0b0d-482c-b8bb-c86621dd9a53" width=400 height=400>












# Troubleshooting

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
 

