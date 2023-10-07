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



## Generating a point cloud from images

```
python generate_pointcloud.py Examples/rgbd_dataset_freiburg1_desk/rgb/1305031452.791720.png Examples/rgbd_dataset_freiburg1_desk/depth/1305031454.072690.png output.ply
```
<img src="https://github.com/dssdanial/Vinum_ORB_SLAM3/assets/32397445/d51a6332-0b0d-482c-b8bb-c86621dd9a53" width=400 height=400>

