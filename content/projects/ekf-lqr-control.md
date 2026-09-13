---
title: "State Estimation & Optimal LQR Control for Underwater Optical Alignment"
date: 2024-11-01
tags: ["Control Systems", "Extended Kalman Filter", "LQR", "MATLAB", "Simulink", "State Estimation"]
summary: "Recreated and verified an augmented 3-state state-space control framework for Underwater Wireless Optical Communication (UWOC). Implemented an Extended Kalman Filter (EKF) with dual-measurement observability paired with a Linear Quadratic Regulator (LQR) in MATLAB/Simulink to achieve sub-two-second optical alignment under attenuation and sensor noise."
cover:
  image: "images/projects/ekf-lqr/ekf-plant-block-diagram.png"
  alt: "Simulink model of nonlinear plant and Extended Kalman Filter state estimator"
  hiddenInSingle: true
weight: 6
---

Establishing reliable, high-bandwidth communication links between autonomous underwater vehicles (AUVs) requires maintaining precise optical transceiver alignment across turbulent marine environments. Underwater Wireless Optical Communication (UWOC) systems face severe operational challenges, including hydrodynamic disturbances, exponential optical beam attenuation, and sensor noise. This project engineered and verified an advanced augmented state-space control framework integrating a recursive Extended Kalman Filter (EKF) with a Linear Quadratic Regulator (LQR) in MATLAB and Simulink. Explore the complete implementation and simulation assets in the [GitHub Repository](https://github.com/GraysonGilbert/ENPM667_Project_1).

<div class="project-stats">
  <div class="project-stat"><span class="project-stat-value">3-State</span><span class="project-stat-label">Force-Velocity Model</span></div>
  <div class="project-stat"><span class="project-stat-value">Beer's Law</span><span class="project-stat-label">Channel Attenuation</span></div>
  <div class="project-stat"><span class="project-stat-value">Rank 3</span><span class="project-stat-label">Dual-Measurement EKF</span></div>
  <div class="project-stat"><span class="project-stat-value">&lt; 2.0 s</span><span class="project-stat-label">State Convergence</span></div>
</div>

<div class="project-figure">
  <img src="/images/projects/ekf-lqr/ekf-plant-block-diagram.png" alt="Simulink model of nonlinear plant and Extended Kalman Filter state estimator" />
  <p class="project-caption">Simulink system architecture illustrating the nonlinear optical plant model, sensor measurement feedback loop, recursive Extended Kalman Filter estimator block, and state demuxing to real-time verification scopes.</p>
</div>

---

## Physical Channel Modeling & 3-State Variable Architecture

Accurately simulating underwater optical communication requires capturing both geometric beam propagation limits and medium-specific optical properties. 

### Optical Channel Physics
The received optical power \(P_{rx}\) is modeled by combining spherical beam spreading via the inverse-square law with exponential medium attenuation governed by the Beer-Lambert law:

<div class="project-math">
$$P_{rx} = P_{tx} \cdot \frac{A_{rx} \cos(\theta)}{2\pi d^2 (1 - \cos(\theta_{div}))} \cdot \exp(-c(\lambda) d)$$
</div>

where \(P_{tx}\) is transmitted power, \(A_{rx}\) is receiver aperture area, \(\theta\) is the receiver normal alignment angle, \(d\) is link distance, \(\theta_{div}\) is beam divergence angle, and \(c(\lambda)\) is the wavelength-dependent attenuation coefficient of turbid marine water.

### State Vector Formulation
To capture the full dynamics of pointing adjustments and received signal strength under disturbance torques, we formulated an augmented 3-state vector:

<div class="project-math">
$$\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} = \begin{bmatrix} P_{rx} \\ \theta \\ \dot{\theta} \end{bmatrix}$$
</div>

### Force-Velocity Design Advantage
Standard control frameworks for optical transceivers rely on simplified two-state position models (\(P_{rx}\) and \(\theta\)). Augmenting the state vector with actuator angular velocity \(\dot{\theta}\) transforms the plant into a rigorous force-velocity formulation. This provides a critical second-order tuning dimension for transient damping, allowing the controller to actively suppress overshoot and oscillation induced by underwater fluid drag.

---

## Observability Analysis & Dual-Measurement Innovation

State estimation requires verifying that internal plant states are fully observable from external sensor outputs. 

### Observability Matrix Ranking
We computed the Jacobian observation matrix and evaluated the rank of the observability matrix:

<div class="project-math">
$$\mathcal{O} = \begin{bmatrix} C & CA & CA^2 \end{bmatrix}^T$$
</div>

### Single-Measurement Limitation
Mathematical evaluation revealed that relying solely on a scalar received optical power measurement (\(P_{rx}\)) yielded an unobservable system (\(\operatorname{rank}(\mathcal{O}) < 3\)). Because multiple angular offsets can produce identical received power drops due to the symmetric beam profile, the estimator cannot mathematically disambiguate orientation errors from power fluctuations.

### Dual-Measurement Innovation
To resolve this limitation, we introduced a dual-measurement innovation: incorporating sequential angular perturbation measurements alongside optical power feedback. This augmented output vector restored full matrix rank (\(\operatorname{rank}(\mathcal{O}) = 3\)), guaranteeing that all system states are fully reconstructible from sensor data.

---

## Standalone Estimator & Controller Validation (3-State Tracking Gallery)

Adhering to the Principle of Separation, the Extended Kalman Filter and Linear Quadratic Regulator were independently validated prior to closed-loop integration.

<div class="project-gallery-3col">
  <div class="project-compare-item">
    <span class="project-compare-label">x1: Power Tracking (Prx)</span>
    <img src="/images/projects/ekf-lqr/ekf-x1-tracking.png" alt="State x1 tracking plot showing received optical power P_rx convergence" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">x2: Orientation Error (θ)</span>
    <img src="/images/projects/ekf-lqr/ekf-x2-tracking.png" alt="State x2 tracking plot showing receiver normal angle theta convergence" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">x3: Angular Velocity (θ_dot)</span>
    <img src="/images/projects/ekf-lqr/ekf-x3-tracking.png" alt="State x3 tracking plot showing actuator angular velocity theta_dot convergence" />
  </div>
</div>

As demonstrated across the verification plots, the standalone EKF successfully suppresses simulated Gaussian sensor noise and tracks true system states within two seconds. Concurrently, the standalone LQR controller successfully drives initial orientation errors and angular velocities back to zero.

---

## Estimator & Controller Algorithm Architecture

The control stack is structured around two rigorous algorithmic engines implemented in MATLAB and Simulink custom function blocks.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Extended Kalman Filter (EKF)</div>
    <div class="project-arch-title">Nonlinear Recursive State Estimation</div>
    <ul class="project-arch-list">
      <li>Nonlinear state prediction modeling optical channel dynamics and actuator kinematics</li>
      <li>Analytical Jacobian matrix linearization (\(F_k\) and \(H_k\)) evaluated at each operational timestep</li>
      <li>Optimal Kalman gain computation coupled with process (\(Q\)) and measurement (\(R\)) covariance updates</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Linear Quadratic Regulator (LQR)</div>
    <div class="project-arch-title">Optimal State-Feedback Control</div>
    <ul class="project-arch-list">
      <li>Continuous Algebraic Riccati Equation (CARE) solver computing optimal gain matrix \(K\)</li>
      <li>State weighting matrix \(Q_{lqr}\) configured to heavily penalize pointing error and orientation drift</li>
      <li>Control effort penalty \(R_{lqr}\) tuned to prevent actuator saturation and aggressive torque spikes</li>
    </ul>
  </div>
</div>

---

## Closed-Loop Integration Diagnostics & Engineering Takeaways

Integrating the validated plant, EKF estimator, and LQR controller into a unified closed-loop Simulink architecture exposed critical insights into nonlinear control limits.

### Systematic Troubleshooting
* **Covariance Tuning:** Adjusted process noise covariance \(Q\) and measurement noise covariance \(R\) matrices to balance filter responsiveness against sensor noise rejection.
* **Solver Tolerances:** Modified variable-step ODE solver configurations in Simulink to handle stiff non-linearities at peak attenuation boundaries.
* **Actuator Saturation:** Implemented strict torque and velocity limits to prevent numerical divergence during large initial transient errors.
* **Stability Boundaries:** Analyzed non-linear plant stability limits where extreme optical attenuation gradients invalidate linear approximations, highlighting the boundaries of local linearization.

### Key Takeaways
The project successfully demonstrated that while linear quadratic regulators and extended Kalman filters provide exceptional performance under mild perturbations, strongly nonlinear optical channels require careful gain scheduling or robust nonlinear control extensions to maintain global stability under severe marine disturbances.
