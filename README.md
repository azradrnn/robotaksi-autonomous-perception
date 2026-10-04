# Robotaksi Autonomous Perception

A computer vision-based perception system developed for the **TEKNOFEST Robotaksi Passenger Autonomous Vehicle Competition**.

The system combines classical computer vision and YOLO-based object detection to process camera images and provide lane information, road-object detection, and safety-related outputs.

## Project Overview

The perception pipeline takes a camera frame as input and produces structured perception data for an autonomous driving system.

The system combines:

* **Lane detection** using OpenCV
* **YOLO-based object detection**
* **Traffic sign and traffic light processing**
* **Obstacle detection**
* **Safety and speed logic**
* **ROS integration**

## Perception Pipeline

```text
                    Camera Image
                         │
                         ▼
                Perception Pipeline
                   ┌─────┴─────┐
                   │           │
                   ▼           ▼
             Lane Detection   YOLO
                   │           │
                   │       ┌───┴──────────────┐
                   │       │                  │
                   ▼       ▼                  ▼
             Lane Offset  Road Signs      Obstacles
             Lane Angle  Traffic Lights   Vehicles
                   │       │                  │
                   └───────┴──────────────────┘
                               │
                               ▼
                       Safety & Speed Logic
                               │
                               ▼
                       Structured Output
                               │
                               ▼
                         ROS Interface
```

## Features

### Lane Detection

The lane detection module uses classical computer vision techniques:

* Region of Interest (ROI) selection
* HSV color filtering
* White and yellow lane masking
* Gaussian blur
* Canny edge detection
* Hough Line Transform
* Left/right lane separation
* Lane center estimation
* Temporal smoothing

The module returns:

```text
lane_center
lane_offset
lane_angle
```

The lane offset is normalized relative to the center of the camera frame and is also used by the speed control logic.

### YOLO Object Detection

The project uses a trained YOLO model located at:

```text
models/best.pt
```

The dataset contains **21 classes** related to road signs, traffic lights, and road environments.

Dataset classes include:

```text
bus_stop
do_not_enter
do_not_stop
do_not_turn_l
do_not_turn_r
do_not_u_turn
enter_left_lane
green_light
left_right_lane
no_parking
parking
ped_crossing
ped_zebra_cross
railway_crossing
red_light
stop
t_intersection_l
traffic_light
u_turn
warning
yellow_light
```

The dataset was prepared using Roboflow.

### Traffic Sign Processing

The perception pipeline processes supported traffic sign classes and includes safety handling for stop signs.

For example:

```text
STOP → Emergency Stop
```

### Traffic Light Processing

Detected traffic light regions are processed to estimate their current state.

The

