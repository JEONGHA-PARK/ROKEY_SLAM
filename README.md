# Illegal Parking Detection and Towing Robot System

This project was conducted as part of the ROKEY Bootcamp 3rd cohort's SLAM project.
A system was developed using Python-based ROS2 for communication between two TurtleBots.
This robot system consists of a patrol robot that detects illegally parked vehicles and a towing robot that navigates to the detected vehicle’s location and performs towing.

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
|테스트1|*강조1*|테스트3|
|테스트1|**강조2**|테스트3|
<|테스트1|<span style="color:red">강조3</span>|테스트3|>
<img src = "https://github.com/user-attachments/assets/ae8834a0-d8d1-49ee-9c26-409dc18db849" width="50%" height="50%">

## SLAM & Navigation
The packages provided by TurtleBot4 were used. The map was created using SLAM Toolbox, and the TurtleBot4 was navigated to the target location via the optimal path using the Navigation Toolbox.
## 코드 세개 설명
## 정도...
