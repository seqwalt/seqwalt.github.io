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
**Process Dynamics**  
The process dynamics are continuous and nonlinear:

$$
  \dot{\mathbf{x}} = f(\mathbf{x}, \mathbf{u}, \mathbf{w})
  = \begin{bmatrix} \dot x_{r} \\ \dot y_{r} \\ \dot \psi_{r} \\ \dot x_1 \\ \dot y_1 \\ \vdots \\ \dot x_n \\ \dot y_n \end{bmatrix}
  = \begin{bmatrix} v\cos(\psi_{r}) \\ v\sin(\psi_{r}) \\ \omega \\ 0 \\ 0 \\ \vdots \\ 0 \\ 0 \end{bmatrix} + \mathbf{w},\quad \mathbf{u} = \begin{bmatrix} v \\ \omega \end{bmatrix},
$$

where ($$x_{r},\,y_{r}$$) is robot position, $$\psi_{r}$$ is robot heading angle, and the control inputs $$v$$ and $$\omega$$ are speed and turn rate, respectively. The position of the $$i$$-th AprilTag marker is denoted by ($$x_i,\,y_i$$), and the process noise vector is $$\mathbf{w}$$. Note $$n+1$$ markers are placed in the environment, where the first marker defines the origin such that ($$x_0,\,y_0$$) $$:=$$ ($$0,\,0$$).

A 4-th order Runge-Kutta integration scheme (RK4) is used for the **EKF propogation step**. Given state $$\mathbf{x}_k$$ and control $$\mathbf{u}_k$$ at time $$t_k,$$ RK4 can be used to estimate state $$\mathbf{x}_{k+1}$$ at time $$t_{k+1}:$$

$$\mathbf{x}_{k+1} \approx F_{RK4}(\mathbf{x}_k, \mathbf{u}_k)$$

**Measurement Equation**  
The EKF measurement step is performed when one or more fiducial markers are . A measurement of the $$i$$-th AprilTag can be described as follows:

$$
\mathbf{z}_i = h_i(\mathbf{x}, \mathbf{v}_i) =
\begin{bmatrix}
  (x_i - x_{r})\cos(\psi_{r}) + (y_i - y_{r})\sin(\psi_{r}) \\
  (y_i - y_{r})\cos(\psi_{r}) - (x_i - x_{r})\sin(\psi_{r}) \\
  \psi_i^W - \psi_{r}
\end{bmatrix} + \mathbf{v}_i
$$

where
#### Experiment

<div class="row justify-content-md-center">
    <iframe width="1280" height="450" src="https://www.youtube.com/embed/8S0Tm8Y30eA" title="AprilTag EKF" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
<div class="caption text-justify">
    Video 1: EKF pose estimation in action.
</div>

#### References
<a id="1"></a>**[1]** M. Krogius, A. Haggenmiller and E. Olson, "[Flexible Layouts for Fiducial Tags](https://ieeexplore.ieee.org/document/8967787)," 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems<br>
