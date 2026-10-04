# Robotaksi Autonomous Perception

A computer vision-based perception system developed for the **TEKNOFEST Robotaksi Passenger Autonomous Vehicle Competition**.

The system combines classical computer vision and YOLO-based object detection to process camera images and provide lane information, traffic sign and traffic light detection, obstacle detection, and safety-related outputs.

## Project Overview

The perception pipeline takes a camera frame as input and produces structured perception data for an autonomous driving system.

The system performs two main tasks:

* **Lane detection** using OpenCV
* **Object detection** using a trained YOLO model

The detected information is combined with safety rules to generate outputs such as lane offset, traffic information, emergency stop status, and speed control.

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
             Lane Offset  Traffic Signs   Obstacles
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

The dataset contains **21 classes** related to road
