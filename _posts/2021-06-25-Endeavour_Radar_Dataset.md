---
layout: article
title: Endeavour Radar Dataset
mode: immersive
header:
  theme: dark
article_header:
  type: cover
  image:
    src: https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/head.png
tags: Radar Dataset SLAM 毫米波雷达 数据集
key: EndeavourRadarDataset
---

# Endeavour Radar Dataset


The experimental vehicle "Endeavour" is an autonomous driving experimental platform developed by the Institute of Artificial Intelligence and Robotics, Xi'an Jiaotong University. It is positioned as a self-driving shuttle bus with the ability to pick up and drop off any point-to-point autonomous driving in a specific scene. 

## Dataset Downloads

### Raw Data
[Endeavour Radar Dataset -- raw data (OneDrive)](https://stuxjtueducn-my.sharepoint.com/:f:/g/personal/gloryhry_stu_xjtu_edu_cn/EjQ5Q8mt3BFLkp5QQTuwlD8BEIFpqW9O62Oj6VYUG9eZnQ)

[Endeavour Radar Dataset -- raw data (BaiduPan)](https://pan.baidu.com/s/1BZSQ_seTOXuhO2d3QCKpzA) (Extraction code：xjtu)

[Endeavour Radar Dataset -- raw data (aliyundrive)](https://www.aliyundrive.com/s/kCVL2S5wxty)

<img src="https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/375191D8417B4E0884DC0AA0BDFCC556.png"  width="15%">

### Filtered TF
[Endeavour Radar Dataset -- filtered tf (OneDrive)](https://stuxjtueducn-my.sharepoint.com/:f:/g/personal/gloryhry_stu_xjtu_edu_cn/EhwZgtksLEBJhimlUNAYhV0BLv1pyKHUQgWzUL3zOHSOgA?e=p9VYYc)

[Endeavour Radar Dataset -- filtered tf (aliyundrive)](https://www.aliyundrive.com/s/cv2E9bZgspZ)

<!-- |Name | Time| Size | Line |
|-----|-----|------|------|
|2021-23-10-36-38.rosbag | 2021 -->

## Dataset Description

<center class="half">
    <img src="https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/car_radar_range.png" width="50%" align=left>
    <img src="https://cdn.jsdelivr.net/gh/gloryhry/imghosting/img/path.png" width="50%" alt="path" align=right>
</center>

In this dataset, we use five ARS430 millimeter wave radars to form the perception system around car. The installation position of five radars on the car is shown in the figure. To minimize the blind spot of the radar in the front and both sides of the car, one radar is installed in the front of the car and four radars are installed on both sides of the car body. We calibrate the radar and lidar jointly, and obtain the pose relationship between the five radars and the pose relationship between the radars and the vehicle body coordinate system. The output of radar is cluster data, and the cluster data format is the parameter list of the detected target. In order to facilitate the use of subsequent experiments, we transform the cluster data output by the radar into the form of labeled point cloud. Each point represents the object information detected by the radar, including angle, distance, radial velocity, angle variance, distance variance and velocity variance. 

We collected radar, lidar and combined inertial navigation system data at the same time on the roads around Western China Science and Technology Innovation Harbour. We use RTK-GNSS combined inertial navigation system data as the basis for evaluating position accuracy to verify the performance of radar odometry. We collected a total of three routes. The west route runs counterclockwise and the route length is about 4.3km. The middle route and the east route are driven in a clockwise direction. The length of the routes is 4.4km and 2.6km respectively. The total length of the three routes is about 11.3km. The three tested routes have long driving distances, fewer roadside features, and high feature repeatability, which put forward higher requirements for the positioning of the radar odometry. During the entire data collection process, the vehicle was driven manually at a speed of approximately 30km/h, without using the automatic driving function. 

The total length of the collected dataset is 10940s, among which the scanning frequency of the millimeter wave radar is 13 Hz, and more than 69,000 frames have been recorded. Lidar provides data at a frequency of approximately 10 Hz, recording more than 54,000 frames in total. The inertial navigation system publishes data at 100 Hz, as the RTK records precise latitude and longitude coordinates at 10Hz. To facilitate the subsequent correspondence between the odometry coordinate system and the latitude and longitude, we convert the latitude and longitude of the RTK to the UTM (Universal Transverse Mercator Grid System) coordinate system. After the latitude and longitude coordinates are converted to UTM coordinates, all coordinates are located in the "49S" area.


## Sensor Suite

We collect data from these sensors:

- Radar:
  - 5 x Continental Advanced Radar Sensor ARS430 Radar
- Lidar:
  - 4 x Velodyne VLP-16 LiDAR
- GPS/INS:
  - 1 x BDStar Navigation Npos320

## Transfer radar data into PointCloud2 in ROS

Use [this porject](https://github.com/gloryhry/radar2pointcloud.git) to transfer radar data into PointCloud2 in ROS.
[gloryhry/radar2pointcloud](https://github.com/gloryhry/radar2pointcloud.git)
