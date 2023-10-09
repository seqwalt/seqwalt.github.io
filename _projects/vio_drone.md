---
layout: page
title: Quadcopter
description: VIO quadcopter for FOV-constrained precision landing
img: assets/img/project_vio_quad/full_quad.jpg
importance: 1
category:
related_publications:
---
**Corresponding thesis:** [PDF](/assets/pdf/SW_masters_thesis.pdf), [slides](/assets/pdf/SW_masters_defense.pdf)<br>
**Documentation:** [vioquad_doc](https://github.com/seqwalt/vioquad_doc)<br>
**Code:** [vioquad_land](https://github.com/seqwalt/vioquad_land)<br>

#### Overview
This project addresses vision-based precision landing of a quadcopter. Inadequate visibility of the landing pad can cause landing inaccuracy, collisions, and a decrease in safety for bystanders. To improve landing accuracy, trajectory constraints are designed to ensure the down-facing onboard camera keeps the landing pad in it's field-of-view (FOV).

#### Hardware
This quadcopter was designed to use an Intel RealSense D435i stereo camera (front-facing) for visual-inertial odometry (VIO) while a monocular IMX219-160 camera (down-facing) is used to detect the landing pad. The companion computer (CC) consists of a NVIDIA Jetson Xavier NX and the flight control unit (FCU) is the Kakute H7 v2.

<div class="row justify-content-md-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_vio_quad/front_side_annotation.png" title="view of quadcopter cameras and companion computer" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption text-justify">
    Figure 1: Quadcopter with cameras and companion computer. The companion computer housing and landing legs were designed in <a href="https://www.onshape.com/en/">OnShape</a> and 3D printed. The spherical reflective markers were used to verify the flight algorithm using ARC Lab's motion capture system.
</div>

#### Implementation
[PX4](https://px4.io/) is used as the low level flight control software which converts angular rates and thrust to motor speeds. During the landing phase (i.e. after the monocular camera spots the landing pad), angular rates and thrust are computed using a perception-aware model-predictive control (PAMPC) method [[1]](#1) (implemented by [ACADO Toolkit](https://acado.github.io/)). PAMPC is used here to track a trajectory while trying to keep the landing pad center in the middle of the monocular camera's view.

The landing trajectory tracked by PAMPC is generated using a min-snap quadratic program (QP) [[2]](#2) (implemented in [vioquad_land](https://github.com/seqwalt/vioquad_land/blob/acado/include/min_snap_traj.h)). FOV restrictions were designed and included as linear constraints in the min-snap QP, generating a trajectory that keeps the landing pad in the camera's FOV. The landing pad is equipped with an AprilTag marker, and the AprilTag 3 algorithm [[3]](#3) (implemented by [apriltag_ros](https://github.com/AprilRobotics/apriltag_ros)) is used for relative pose estimation between the quadcopter and landing pad. Finally, ROVIO [[4]](#4) is used as the VIO algorithm to estimate the quadcopter pose and velocity (implemented by [rovio](https://github.com/ethz-asl/rovio)).

<div class="row justify-content-md-center">
    <div class="col-9 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_vio_quad/control_loop.png" title="autonomous landing block diagram" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption col-9 text-justify">
        Figure 2: Feedback loop implemented for landing on an AprilTag marker. The AprilTag algorithm estimates the pose of the AprilTag marker. VIO provides a quadcopter state estimate <math><mover><mi>𝒙</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">^</mo></mover></math> given the system output <math><mi>𝒚</mi></math> (stereo image and IMU data). The FOV-constrained minsnap QP provides the desired state <math><msub><mi>𝒙</mi><mi>d</mi></msub></math> given <math><mover><mi>𝒙</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">^</mo></mover></math> and AprilTag marker pose. PAMPC generates the control input <math><mi>𝒖</mi></math> given <math><msub><mi>𝒙</mi><mi>d</mi></msub></math> , <math><mover><mi>𝒙</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">^</mo></mover></math> and AprilTag marker pose.
    </div>
</div>

#### Experiment

Experiments were conducted in simulation ([Gazebo classic](https://docs.px4.io/main/en/sim_gazebo_classic/)) and in the real world to test the algorithm.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_vio_quad/rovio_sim.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption text-justify">
    Figure 3: Simulated quadcopter landing experiment. <b>Top left:</b> ROVIO tracks features using the stereo front-facing camera (only left camera image is shown). <b>Bottom left:</b> The AprilTag algorithm detects the landing pad using the down-facing camera. <b>Right:</b> The flight history (green path), generated min-snap QP landing trajectory (blue path), PAMPC predicted trajectory (red path) and current PAMPC reference trajectory (orange path) is shown.
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_vio_quad/lab_land_experiment.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption text-justify">
     Figure 4: Real-world flight and landing of the quadcopter. <b>Left:</b> Quadcopter starts tracking the search trajectory (green oval) then lands (blue oval). <b>Right:</b> Close-up of the landed quadcopter.
</div>

#### Results
We can see that the FOV constrained trajectory allows the projected landing pad center to get closer to the image center when compared to the experiment without FOV constraints.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_vio_quad/land_metric1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption text-justify">
    Figure 5: Trajectories of the landing pad center projected onto the image plane.
</div>

#### References
<a id="1"></a>**[1]** D. Falanga, P. Foehn, P. Lu and D. Scaramuzza, "[PAMPC: Perception-Aware Model Predictive Control for Quadrotors](https://ieeexplore.ieee.org/document/8593739)," 2018 IEEE/RSJ International Conference on Intelligent Robots and Systems<br>
<a id="2"></a>**[2]** D. Mellinger and V. Kumar, "[Minimum snap trajectory generation and control for quadrotors](https://ieeexplore.ieee.org/document/5980409)," 2011 IEEE International Conference on Robotics and Automation<br>
<a id="3"></a>**[3]** M. Krogius, A. Haggenmiller and E. Olson, "[Flexible Layouts for Fiducial Tags](https://ieeexplore.ieee.org/document/8967787)," 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems<br>
<a id="4"></a>**[4]** M. Bloesch, M. Burri, S. Omari, M. Hutter, R. Siegwart, "[Iterated extended Kalman filter based visual-inertial odometry using direct photometric feedback](https://journals.sagepub.com/doi/full/10.1177/0278364917728574)," The International Journal of Robotics Research. 2017
