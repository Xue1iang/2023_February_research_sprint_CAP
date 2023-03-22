# 2023_February_research_sprint_CAP

## Abbreviations and nomenclature
|    | Explanation|
|:--:|:--:|
|ASV | Aquatic surface vehicle|
|ROV | Remote operated vehicle|
|DOF | Degree of freedom|
|CAP | Collebrative aquatic positioning|
|PnP | Perspective-n-Point|
|DS  | Depth sensor|
|![math expression](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/CodeCogsEqn.svg)    |    Coordinate frame of A         |
|![math expression](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t_B%5EA.svg) | Translation from frame A to frame B |
|![math expression](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EA_B.svg) | Quaternion describes a rotation from frame A to frame B|


## Hardware setup

### Robotic platform
| ![Image 1](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/IMG_3216_scaled.jpg) | ![Image 2](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/IMG_3224_scaled.jpg) |
| :----------------------------------------: | :----------------------------------------: |
| Fig.1: BlueROV2  |  Fig.2: MallARD_003   |

In this experiment, BlueROV2 is the underwater robot tracked by CAP and Qualisys. MallARD_003 is as the ASV in CAP system, carried with a 2D LiDAR, monocular camera and an IMU and Ressperry Pi4 is the on-board PC. 

### Sensors

||Model number|
| :---:| :---: |
|IMU| Park Lord 3DMGQ7|
|LiDAR|TIM871 series Laser Scanner LiDAR|
|Presure sensor| Bar30 High-Resolution 300m depth/pressure sensor|
|USB camera| Low-Light HD USB camera|


## Software setup
### IP address
![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/IP_address.png)

### Software on BlueROV
-......

### Software on MallARD
[MallARD_003 - SLAM on base](https://github.com/EEEManchester/MallARD/tree/slam-on-base)

## Datasets
There are four datasets compressed in ```Feb_2023_CAP1.zip```. They are:

- 2023-02-24-16-11-29.bag
- 2023-02-24-16-21-23.bag
- 2023-02-24-16-31-55.bag
- 2023-02-24-16-57-03.bag

### Datasets explenations
| ROSBAG name| Description | Notes |
| :---: | :---: | :---:|
|2023-02-24-16-11-29.bag|MallARD moving BlueROV2 still| MallARD was controlled by joystick.|
|2023-02-24-16-31-55.bag|MallARD still BlueROV2 moving|BlueROV2 was on manual control and MallARD was hold still by an "apple picker"|
|2023-02-24-16-21-23.bag|Both moving 1|The movement of the two is not correlated.|
|2023-02-24-16-57-03.bag|Both moving 2|The movement of the two is not correlated.|


Each dataset contains 8 rostopics:

| ROSTOPIC name| Content | 
| :---: | :---: |
| /bluerov/mavros/global_position/rel_alt | Depth sensor reading (parpendicular distance between water surface to depth sensor)|
| /bluerov/qualisys/blueROV/pose  |Qualisys tracking BlueROV2's 6DOF (x, y, z, roll, yaw, pitch (in quaternion)) with respect to Qualisys world coordinate frame|
|/bluerov/usb_cam/image_raw|Image raw from camera mounted on BlueROV2|
|/imu/data|3DMGQ7 IMU reading (3-axis angular rate, 3-axis acceleration and rotation (in quaternion format) relative to boot-up frame with z axis point up)|
|/nav/filtered_imu/data|3DMGQ7 IMU reading (with build-in filter filtered)|
|/slam_out_pose|SLAM reading (MallARD's position (x, y) and orientation (yaw) relative to the map coordinate frame)|
|/tag_corners|Four conners' and centre's pixel coordinates in image plane|
|/tag_detections| ID of each detected tag, its pose (i.e., its position and orientation) in the camera frame.|


## Frames and offsets
### Frames

<!-- <div style="display: flex; justify-content: space-between;">


  <img src="https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/Screenshot%202023-03-21%20at%2013.14.29.png" alt="Image 1" width="45%" />


  <img src="https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/Screenshot%202023-03-21%20at%2015.54.11.png" alt="Image 2" width="45%" />

</div> -->


| ![Image 1](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/big_size_one.png) | ![Image 2](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/Screenshot%202023-03-21%20at%2015.54.11.png) | ![Image 3](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/top_view_scaled.png)
| :----------------------------------------: | :----------------------------------------: | :-----------: |
|   Fig.3: Qualisys frame and map frame int the tank     |  Fig.4: Frames on MallARD   | Fig.5: AprilTag fiducial marker and Qualisys object plate|


### Offsets
 CAP requires some parameters to estimate the position of the marker, such as offsets between coordinate systems, the camera's intrinsic matrix, etc. In this experiment (system), multiple coordinate systems are involved. The static offsets between coordinate systems (including translations and rotations) are roughly measured and recorded in the table below. Note that these offsets may vary slightly when collecting each set of data.

| Datasets name| Offsets | Notes |
| :---: | :---: |---|
|2023-02-24-16-11-29.bag|camera_intrinsic = [576.0913564252332, 0, 435.5688200267924; 0, 574.7447982699829, 285.3576802714121; 0, 0, 1]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EB_C.svg) = [0, 0.7071, -0.7071, 0]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EB_C.svg) = [0, 0, -0.19]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EI_B.svg) = [0, 0, 0.7071, 0.7071]<br>depth_sensor_offset = 0.2<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5Equalisys_map.svg) = [-0.85, 0.75, 2.05]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5Equlisys_map.svg) = [0.8660, 0, 0, -0.500]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EqualisysObject_markerCenter.svg) = [-0.1, 0, 0]|- ```B``` in superscript means Body frame.<br> - ```C``` stands for camera frame. <br> - ```I``` stands for IMU frame. <br> - ```map frame``` is created by Hector SLAM. <br> - ```depth_sensor_offset``` is the parpendicular distance from depth sensor to marker plane. <br>- ![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EqualisysObject_markerCenter.svg) is the translation offset frome qualisys object center to AprilTag center (Fig.5)|
|2023-02-24-16-31-55.bag|camera_intrinsic = [576.0913564252332, 0, 435.5688200267924; 0, 574.7447982699829, 285.3576802714121; 0, 0, 1]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EB_C.svg) = [0, 0.7071, -0.7071, 0]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EB_C.svg) = [0, 0, -0.19]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EI_B.svg) = [0, 0, 0.7071, 0.7071]<br>depth_sensor_offset = 0.2<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5Equalisys_map.svg) = [-0.8, 1.6, 2.05]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5Equlisys_map.svg) = [0.8660, 0, 0, -0.500]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EqualisysObject_markerCenter.svg) = [-0.1, 0, 0]||
|2023-02-24-16-21-23.bag|camera_intrinsic = [576.0913564252332, 0, 435.5688200267924; 0, 574.7447982699829, 285.3576802714121; 0, 0, 1]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EB_C.svg) = [0, 0.7071, -0.7071, 0]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EB_C.svg) = [0, 0, -0.19]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EI_B.svg) = [0, 0, 0.7071, 0.7071]<br>depth_sensor_offset = 0.2<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5Equalisys_map.svg) = [-0.7, 1.7, 2.05]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5Equlisys_map.svg) = [0.8660, 0, 0, -0.500]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EqualisysObject_markerCenter.svg) = [-0.1, 0, 0]||
|2023-02-24-16-57-03.bag|camera_intrinsic = [576.0913564252332, 0, 435.5688200267924; 0, 574.7447982699829, 285.3576802714121; 0, 0, 1]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EB_C.svg) = [0, 0.7071, -0.7071, 0]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EB_C.svg) = [0, 0, -0.19]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5EI_B.svg) = [0, 0, 0.7071, 0.7071]<br>depth_sensor_offset = 0.2<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5Equalisys_map.svg) = [-0.96, 1.7, 2.05]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/q%5Equlisys_map.svg) = [0.8660, 0, 0, -0.500]<br>![image](https://github.com/Xue1iang/2023_February_research_sprint_CAP/blob/main/fig/t%5EqualisysObject_markerCenter.svg) = [-0.1, 0, 0]||
