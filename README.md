# Illegal Parking Detection and Towing Robot System

This project was conducted as part of the ROKEY Bootcamp 3rd cohort's SLAM project.
A system was developed using Python-based ROS2 for communication between two TurtleBots.
This robot system consists of a patrol robot that detects illegally parked vehicles and a towing robot that navigates to the detected vehicle’s location and performs towing.

[📹Video link](https://youtu.be/kDVmqTWpEo4?si=YXWweSR9qxDkXr2W)(The towing scene in the latter part of the video is a staged demonstration.)

## Turtlebot4 Camera Setting
Camera model : OAK-D-Lite

    oakd:
     ros__parameters:
       rgb:
         i_preview_size: 360
         i_resolution: '720p'
      
       stereo:
         i_align_depth: true
         i_height: 360
         i_width: 640
## Object detection mdels
In this project, a YOLOv11n model was fine-tuned to create an object detection model for recognizing toy cars. The model was trained using 200 images, each of size 360x360. The performance of the fine-tuned model is as follows:
![image](https://github.com/user-attachments/assets/1c054476-ea74-4b22-92b3-4ea001da9fb0)

## Determine whether the vehicle is illegally parked or not
The region of interest was masked by designating specific color areas, and the overlap between the detected bounding box of the object and the masked area was calculated to determine whether the vehicle was illegally parked.
#### Green color type masking
![image](https://github.com/user-attachments/assets/6f838a9b-6560-484b-829f-598bc4f3693e)
||Color|
|---|---|
|Illegal|🔴Red|
|Legal|⚪️White|
<img src = "https://github.com/user-attachments/assets/ae8834a0-d8d1-49ee-9c26-409dc18db849" width="50%" height="50%">

## SLAM & Navigation
The packages provided by TurtleBot4 were used. The map was created using SLAM Toolbox, and the TurtleBot4 was navigated to the target location via the optimal path using the Navigation Toolbox.

[:octocat:turtlebot4 github link](https://github.com/turtlebot/turtlebot4/tree/jazzy?tab=readme-ov-file)
#### MAP
<img src = "https://github.com/user-attachments/assets/52b3ec07-4179-41e6-98ff-63bb23e0e8d2" width="30%" height="30%">


#### Navigation2
![Image](https://github.com/user-attachments/assets/6c421057-920b-4d70-8aa7-fa7ad474eab5)


## Code Explanation

### object_detection_tf.py
The node created from this file subscribes to RGB and depth images from the TurtleBot4, performs object detection to determine whether a vehicle is illegally parked, estimates the distance to the object using the depth image, transforms the detected position to the map frame via TF, and publishes it as a topic. All these processes run through multithreading.

![0525forgithub](https://github.com/user-attachments/assets/a3cc4270-ea6d-4e2d-98a9-6c9493d94aa0)

### patrol.py

This code utilizes the TurtleBot4 Navigation Toolbox to enable the robot to autonomously patrol a designated area. Waypoints were set to guide the robot’s patrol route automatically.

![patrol_forgit](https://github.com/user-attachments/assets/129a9228-c6ef-4bd6-b66a-45bdec08ec52)

### towing.py

This code is designed to receive map coordinate data of illegally parked vehicles from the patrol robot, navigate to that location, and capture a photo. The saved image file name includes the date and location information. After taking the photo, the robot is programmed to return to a specific location. However, due to the limitations of the TurtleBot4, the towing function could not be implemented.

<img src = "https://github.com/user-attachments/assets/d3529825-ff8a-417a-97ba-183367d82ac3" width="30%" height="30%">
<img src = "https://github.com/user-attachments/assets/b2a3404a-2f3c-4f7b-ac27-7420a54c3df7" width="31%" height="31%">

## Others
The configs folder contains the configuration file used for running the navigation. This configuration reduces the inflation radius to ensure smooth navigation even in narrow map environments. Additionally, the movement speed of the TurtleBot4 was lowered to adjust the camera update rate, improving the real-time performance of object detection.

The maps folder contains the map file generated through SLAM, and the model folder contains the .pt file of the YOLOv11n model that was fine-tuned through transfer learning.
