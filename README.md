# realsense_ros_gazebo
Intel Realsense Tracking and Depth camera simulations

# Realsense T265 #
![](doc/t265.png)

### Usage ###
```xml
<xacro:include filename="$(find realsense_ros_gazebo)/xacro/tracker.xacro"/>

<xacro:realsense_T265 sensor_name="camera" parent_link="base_link" rate="30.0">
  <origin rpy="0 0 0" xyz="0 0 0.5"/>
</xacro:realsense_T265>
```

### Publishers ###
* /camera/**fisheye1**/camera_info
* /camera/fisheye1/image_raw
* /camera/fisheye1/image_raw/compressed
* /camera/fisheye1/parameter_descriptions
* /camera/fisheye1/parameter_updates
* /camera/fisheye2/camera_info
* /camera/fisheye2/image_raw
* /camera/fisheye2/image_raw/compressed
* /camera/fisheye2/parameter_descriptions
* /camera/fisheye2/parameter_updates
* /camera/**gyro**/sample _( accel and gyro are in the same imu message )_
* /camera/**odom**/sample

# Realsense R200 #
![](doc/r200.png)

### Usage ###
```xml
<xacro:include filename="$(find realsense_ros_gazebo)/xacro/depthcam.xacro"/>

<xacro:realsense_R200 sensor_name="camera" parent_link="base_link" rate="30.0">
  <origin rpy="0 0 0" xyz="0 0 0.5"/>
</xacro:realsense_R200>
```

### Publishers ###

:construction:
