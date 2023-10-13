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
Balance bot is a self-balancing Arduino robot, featuring a PID controller for angle and position tracking. A Madgwick complementary filter utilizes a fourth order Runge Kutta scheme (RK4) to estimate orientation as a quaternion. Additionally, wheel encoders are employed to provide accurate position estimation.

### Hardware
This robot uses an Arduino Nano, worm gear motors with magnetic wheel encoders, an inertial measurement unit (IMU), and a motor driver. Additionally, a radio transceiver has been integrated into the build for potential future expansion. Balance bot was designed in OnShape, with the battery placed towards the top to increase the moment of inertia, thereby enhancing the stability of the robot.

<div class="row justify-content-md-center">
    <div class="col-9 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_balance_bot/balance_bot_internals_annotated.png" title="balance bot anatomy" class="img-fluid rounded z-depth-1" %}
        <div class="caption text-justify">
          Figure 1: Balance bot anatomy.
        </div>
    </div>
</div>

<div class="row justify-content-md-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_balance_bot/balance_bot_circuitry.png" title="balance bot wiring" class="img-fluid rounded z-depth-1" %}
        <div class="caption text-justify">
          Figure 2: Balance bot wiring diagram.
        </div>
    </div>
</div>

### Orientation Estimate
Using the IMU dynamic model from [[1]](#1) with a RK4 integration scheme, IMU measurements are used to propagate the IMU pose over time. However, since heading and direction of gravity are in general not directly observable using accelerometer and gyroscope measurements, there will be drift on all axes of rotation (and unknown initial orientation).

To address roll/pitch drift, the complementary filter presented in section 7.2 of [[2]](#2) is employed to heuristically estimate the direction of gravity. Yaw drift cannot be accounted for without an additional sensor such as a magnetometer.

An IMU calibration procedure was written ([imu calibration script](https://github.com/seqwalt/IMU_Encoder/tree/main/scripts/imu_calibrate)) to account for axis sensitivity, axis misalignment and bias. [Processing](https://processing.org/) was used along with Arduino script [imu_example.ino](https://github.com/seqwalt/IMU_Encoder/tree/main/examples/arduino/imu_example) to visualize the IMU orientation as shown in Video 1.

<div class="row justify-content-md-center">
    <iframe width="1120" height="500" src="https://www.youtube.com/embed/e9M1eZwYOMM" title="IMU rotation" frameborder="1" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
<div class="caption text-justify">
    Video 1: Rotation estimation from IMU accelerometer and gyroscope measurements.
</div>

### Position Estimate
The magnetic wheel encoders can be used to measure angular displacement of the motor shaft. In combination with the wheel radius and gear ratio, this measurement allows us to estimate the position of the robot. A PID controller is employed so position control can be achieved as shown in Video 2, in which the robot resets to the original position after being moved. The code to control the robot can be found here: [balance_bot.ino](https://github.com/seqwalt/IMU_Encoder/tree/main/examples/arduino/balance_bot)

<div class="row justify-content-md-center">
    <iframe width="1280" height="450" src="https://www.youtube.com/embed/-69OVqz88AQ" title="AprilTag EKF" frameborder="1" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
<div class="caption text-justify">
    Video 2: Position correction using wheel encoder measurements.
</div>

### Future Directions
- Use an EKF to fuse IMU and encoder measurements to get high quality position and heading estimates.
- Implement a remote controller using the radio transceiver.

### References
<a id="1"></a>**[1]** K. Sun, K. Mohta et al., "[Robust Stereo Visual Inertial Odometry for Fast Autonomous Flight](http://arxiv.org/abs/1712.00036)," 2018 arXiv<br>
<a id="2"></a>**[2]** S. Madgwick, "[AHRS algorithms and calibration solutions to facilitate new applications using low-cost MEMS](https://ethos.bl.uk/OrderDetails.do?uin=uk.bl.ethos.681552)," 2014 Thesis (Ph.D.)<br>
