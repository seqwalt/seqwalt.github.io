---
layout: page
title: Lander
description: Control of a 2D lander, verified with Monte Carlo simulations
img: assets/img/project_lunar_lander/lunar_lander.png
importance: 3
category:
---
**Report:** [PDF](/assets/pdf/Lander_Report.pdf)<br>
**Code:** [LunarLander](https://github.com/seqwalt/LunarLander)<br>

### Overview
Inspired by the [Lunar Lander Atari game](https://en.wikipedia.org/wiki/Lunar_Lander_(1979_video_game)), this project aims to perform optimal control on a lander system. Direct collocation is used to obtain an optimal trajectory for the system. Trapezoidal quadrature is used to approximate the various integrals used in the optimal control problem. Finally, Runge-Kutta 4 is used to simulate the dynamics of the system.

### Model
Consider a two-dimensional lander system of mass $$m$$ and rotational inertial $$I$$ in which thrust $$F$$ and torque $$\tau$$ are applied as control inputs (see Figure 1). The states of the system include position ($$x$$, $$y$$), angle $$\theta$$, linear velocity ($$v_x$$, $$v_y$$) and angular velocity $$\omega$$. Acceleration due to gravity is denoted $$g$$, and drag coefficients $$b_v$$ and $$b_{\omega}$$ scale the drag force and torque, respectively. The system dynamics can then be written as:

<div class="row justify-content-sm-center">
  <div class="col-5 mt-3 mt-md-0">
    <math display="block" class="tml-display" style="display:block math;"><mtable displaystyle="true" columnalign="right left"><mtr><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-right;"><mover><mi>x</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">˙</mo></mover></mtd><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-left;"><mrow><mo>=</mo><msub><mi>v</mi><mi>x</mi></msub></mrow></mtd></mtr><mtr><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-right;"><mover><mi>y</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">˙</mo></mover></mtd><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-left;"><mrow><mo>=</mo><msub><mi>v</mi><mi>y</mi></msub></mrow></mtd></mtr><mtr><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-right;"><mover><mi>θ</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">˙</mo></mover></mtd><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-left;"><mrow><mo>=</mo><mi>ω</mi></mrow></mtd></mtr><mtr><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-right;"><msub><mover><mi>v</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">˙</mo></mover><mi>x</mi></msub></mtd><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-left;"><mrow><mo>=</mo><mo>−</mo><mfrac><mn>1</mn><mi>m</mi></mfrac><mo form="prefix" stretchy="false">(</mo><mi>F</mi><mrow><mspace width="0.1667em"></mspace><mi>sin</mi><mo>⁡</mo></mrow><mo form="prefix" stretchy="false">(</mo><mi>θ</mi><mo form="postfix" stretchy="false">)</mo><mo>+</mo><msub><mi>b</mi><mi>v</mi></msub><msub><mi>v</mi><mi>x</mi></msub><mo form="postfix" stretchy="false">)</mo></mrow></mtd></mtr><mtr><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-right;"><msub><mover><mi>v</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">˙</mo></mover><mi>y</mi></msub></mtd><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-left;"><mrow><mo>=</mo><mfrac><mn>1</mn><mi>m</mi></mfrac><mo form="prefix" stretchy="false">(</mo><mi>F</mi><mrow><mspace width="0.1667em"></mspace><mi>cos</mi><mo>⁡</mo></mrow><mo form="prefix" stretchy="false">(</mo><mi>θ</mi><mo form="postfix" stretchy="false">)</mo><mo>−</mo><msub><mi>b</mi><mi>v</mi></msub><msub><mi>v</mi><mi>y</mi></msub><mo form="postfix" stretchy="false">)</mo><mo>−</mo><mi>g</mi></mrow></mtd></mtr><mtr><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-right;"><mover><mi>ω</mi><mo stretchy="false" style="math-style:normal;math-depth:0;">˙</mo></mover></mtd><mtd style="padding:0.7ex 0em 0.7ex 0em;text-align:-webkit-left;"><mrow><mo>=</mo><mfrac><mn>1</mn><mi>I</mi></mfrac><mo form="prefix" stretchy="false">(</mo><mi>τ</mi><mo>−</mo><msub><mi>b</mi><mi>ω</mi></msub><mi>ω</mi><mo form="postfix" stretchy="false">)</mo></mrow></mtd></mtr></mtable></math>
  </div>
  <div class="col-3 mt-3 mt-md-0">
    {% include figure.html path="assets/img/project_lunar_lander/force_diagram.png" title="force diagram" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-5"></div>
  <div class="caption col-3 text-justify">
      Figure 1: Force diagram of the lander model. Drag not shown.
  </div>
</div>

### Optimal Control
To find a sequence of control inputs (thrust, torque) that send the lander from an initial to final desired state, an optimization problem can be solved. This optimization minimizes total "fuel" usage, which equates to minimizing an integral objective function (see the [report](/assets/pdf/Lander_Report.pdf) for more details and math).

Trapezoidal quadrature is used to approximate the integral objective function so direct collocation can be used. The collocation points are points in time, such that each trapezoid is between two collocation points. After solving the optimization, a trajectory of states and control inputs is determined, and can be visualized (see GIF 1).

<div class="row justify-content-sm-center">
  <div class="col-7 mt-3 mt-md-0">
    {% include figure.html path="assets/video/project_lunar_lander/lander0.gif" title="Lander GIF" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="caption col-7 text-justify">
      GIF 1: Optimal trajectory for 3 second front flip in which Earth gravity is used. The initial angle is set to <math><mrow><mn>2</mn><mi>π</mi></mrow></math> and the final angle is set to 0.
  </div>
</div>

### Monte Carlo Simulations
Since the optimal control problem depends on the lander model, uncertainty in the model will cause inaccurate open-loop tracking of the optimal reference trajectory. By using closed-loop control, state feedback is used to augment the optimal control sequence to correct tracking error.

Monte Carlo simulations are used to test how model uncertainty impacts lander flight (same reference trajectory as in GIF 1) when using open-loop and closed-loop control. For each simulation, the drag coefficients are uniformly randomized by $$\pm 5\%$$ and mass, gravity and rotational inertia are uniformly randomized by $$\pm 2\%$$.

<div class="row justify-content-sm-center">
  <div class="col-7 mt-3 mt-md-0">
    {% include figure.html path="assets/img/project_lunar_lander/monte_carlo.png" title="Monte Carlo simulations" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="caption col-7 text-justify">
      Figure 2: Monte Carlo simulations of lander for open-loop and closed-loop control (10K simulations for each control type).
  </div>
</div>

As shown in Figure 2, the [closed-loop controller](https://github.com/seqwalt/LunarLander/blob/main/lander_monte_carlo.py#L194) is able to track the trajectory in the presence of model uncertainty, while the open-loop controller cannot.

### Future Directions
- Add mountains for the lander to perform obstacle avoidance.
- Implement real-time trajectory generation & control for fast reaction to changing environment/objectives.
