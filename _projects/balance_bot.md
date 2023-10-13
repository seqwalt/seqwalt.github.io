---
layout: page
title: Balance Bot
description: Position control of an inverted pendulum robot
img: assets/img/project_balance_bot/balance_bot.png
importance: 3
category:
---

**Code:** [IMU_Encoder](https://github.com/seqwalt/IMU_Encoder)<br>

### Overview
*Overview*

### Hardware
- Arduino Nano
- Worm-gear motors with (hall effect?) wheel encoders
- MPU 6050 IMU
- Motor driver
- Designed in OnShape, battery toward top to increase moment of inertia

<div class="row justify-content-md-center">
    <div class="col-9 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_balance_bot/balance_bot_internals_annotated.png" title="view of quadcopter cameras and companion computer" class="img-fluid rounded z-depth-1" %}
        <div class="caption text-justify">
          Figure 1: Balance bot anatomy.
        </div>
    </div>
</div>

### Inertial Measurement Unit
- IMU dynamics from Kumar paper
- RK4 for state propogation
- Madgwick correction to eliminate roll/pitch drift

<div class="row justify-content-md-center">
    <iframe width="1120" height="500" src="https://www.youtube.com/embed/e9M1eZwYOMM" title="IMU rotation" frameborder="1" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
<div class="caption text-justify">
    Video 1: Rotation estimation from IMU accelerometer and gyroscope measurements.
</div>

### Wheel Encoders
- conversion from encoder measurement to position measurement (gear ratio, wheel radius etc)
- PID discussion

<div class="row justify-content-md-center">
    <iframe width="1280" height="450" src="https://www.youtube.com/embed/-69OVqz88AQ" title="AprilTag EKF" frameborder="1" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
<div class="caption text-justify">
    Video 2: Position correction using wheel encoder measurements.
</div>

### Future Directions
- Remote control using radio transceiver
- Yaw control

### References
<a id="1"></a>**[1]** M. Krogius, A. Haggenmiller and E. Olson, "[Flexible Layouts for Fiducial Tags](https://ieeexplore.ieee.org/document/8967787)," 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems<br>
