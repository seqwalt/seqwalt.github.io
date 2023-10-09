---
layout: page
title: Monobot
description: Monocular robot using AprilTag EKF for pose estimation.
img: assets/img/project_monobot/monobot.jpg
importance: 2
category:
---

**Code:** [monobot](https://github.com/seqwalt/monobot)<br>

#### Overview
"Monobot" is a differential-drive robot with a monocular camera that utilizes an extended Kalman filter (EKF) for pose estimation. AprilTag fiducial markers of known sizes are placed around the environment and detected by Monobot's camera. As the robot drives around the environment the AprilTag 3 algorithm [[1]](#1) estimates the pose of the markers in the camera frame, and provides this pose as a measurement to the EKF. The EKF then estimates the Monobot pose, wheel speed bias, and AprilTag marker positions in the world frame.

#### Hardware
Monobot consists of two continuous rotation servo motors, a Raspberry Pi (RPi) 3 model B+, a 16 channel 12-bit PWM servo driver and a RPi camera module 2. Components are attached to black acrylic sheets, and the servos are attached using 3D-printed mounts. The robot body was designed in [OnShape](https://www.onshape.com/en/).

<div class="row justify-content-md-center">
    <div class="col-9 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_monobot/monobot_annotated.png" title="view of quadcopter cameras and companion computer" class="img-fluid rounded z-depth-1" %}
        <div class="caption text-justify">
          Figure 1: Monobot anatomy.
        </div>
    </div>
</div>

#### Extended Kalman Filter
*Explain implemented EKF here*

#### Experiment

<div class="row justify-content-md-center">
    <iframe width="1280" height="450" src="https://www.youtube.com/embed/8S0Tm8Y30eA" title="AprilTag EKF" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
<div class="caption text-justify">
    Video 1: EKF pose estimation in action.
</div>

#### References
<a id="1"></a>**[1]** M. Krogius, A. Haggenmiller and E. Olson, "[Flexible Layouts for Fiducial Tags](https://ieeexplore.ieee.org/document/8967787)," 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems<br>
