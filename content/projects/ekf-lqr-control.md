---
title: "State Estimation & Optimal LQR Control for Underwater Optical Alignment"
date: 2024-11-15
tags: ["Control Systems", "EKF", "LQR", "Simulink", "State Estimation"]
summary: "Recreated and verified an augmented 3-state state-space control framework for Underwater Wireless Optical Communication (UWOC). Implemented an Extended Kalman Filter (EKF) with dual-measurement observability paired with a Linear Quadratic Regulator (LQR) in MATLAB/Simulink to achieve sub-two-second optical alignment under attenuation and sensor noise."
cover:
  image: "images/projects/ekf-lqr/thumbnail.png"
  alt: "Simulink model of nonlinear plant and Extended Kalman Filter state estimator"
  hiddenInSingle: false
weight: 6
---

Setting up an optical communication link between underwater robots is tough due to severe signal attenuation and the need for precise alignment. For this project, my partner and I worked to recreate and verify an advanced control framework using a recursive Extended Kalman Filter (EKF) and a Linear Quadratic Regulator (LQR). We engineered the simulation using custom blocks in MATLAB and Simulink. You can find the codebase and detailed report on [GitHub](https://github.com/GraysonGilbert/ENPM667_Project_1).

---

## Channel Modeling & State Vector Architecture

To accurately simulate an underwater optical link, you have to account for both the physical spread of the beam and how seawater absorbs light.

### Optical Channel Physics
The received signal voltage ($V_d$) is calculated by combining spherical beam spreading (the inverse-square law) with exponential medium attenuation (Beer's Law):

<div class="project-math">
$$V_d = C_p I_\theta \exp(-cd) \cos(\phi) / d^2$$
</div>

where $I_\theta$ is the transmitter light intensity, $c$ is the water's attenuation coefficient, $d$ is the link distance, $\phi$ is the receiver's incident angle, and $C_p$ represents system-specific constants.

### State Vector Formulation
Most basic optical transceiver controllers just use a 2-state model tracking power and angle. To gain more precise control, we formulated an augmented 3-state vector that includes the actuator's angular velocity ($\dot{\phi}$):

<div class="project-math">
$$\mathbf{x}_k = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} = \begin{bmatrix} C_p I_\theta e^{-cd} / d^2 \\ \phi \\ \dot{\phi} \end{bmatrix}$$
</div>

### The Force-Velocity Advantage
Adding the angular velocity into the state vector gives the LQR a crucial second tuning dimension. Even though the physical receiver properties remain the same, this extra state allows the controller's response to be tuned much more effectively for transient damping.

---

## Observability & The Dual-Measurement Fix

Before relying on an Extended Kalman Filter, my partner and I had to prove the system states were actually observable from the sensor data, as detailed in the final report on GitHub. 

### Single-Measurement Limitation

We calculated the Jacobian matrix to build the observability matrix ($O$) and check its rank. 

$$O = \begin{bmatrix} C \\ CA \\ CA^2 \end{bmatrix}$$

The determinant evaluated to zero, meaning the rank was less than 3, rendering the system unobservable with just one measurement. Because the beam's light intensity is symmetric about the normal, a single power measurement does not provide enough information to estimate all states. 

### The Dual-Measurement Solution

To fix this without the added cost and complexity of a second physical receiver, we modeled a single receiver taking measurements while rotating through a predefined array of angles, changing by 2 degrees per step. By taking two sequential measurements, we restored the observability matrix rank to 3, guaranteeing the EKF could mathematically reconstruct all the states. 

## Standalone EKF & LQR Testing

Following the Principle of Separation, we built and tested the EKF and LQR completely independently before attempting to close the loop. 

### Standalone EKF Tracking

The state estimator runs a recursive predict-update cycle, propagating the state and error covariance ahead, computing the optimal Kalman gain from incoming measurements, and updating the estimated states:

<div class="project-figure" style="max-width: 580px; margin: 1.5rem auto;">
  <img src="/images/projects/ekf-lqr/ekf-alg-flow.png" alt="Extended Kalman Filter recursive algorithm flowchart" />
  <p class="project-caption">Recursive Extended Kalman Filter cycle: initializing state priors, predicting state and covariance, computing the Kalman gain, incorporating incoming measurements to update the state estimate, and propagating error covariance to the next time step.</p>
</div>

The standalone EKF successfully filtered the Gaussian measurement noise and locked onto the true system values after about two seconds of simulation time:

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

### Standalone LQR Regulation
Running on its own with the known true states, the LQR controller successfully drove the initial angular displacement and angular velocity back to zero within 20 time steps (approximately two seconds):

<div class="project-figure">
  <img src="/images/projects/ekf-lqr/lqr-control.png" alt="Simulink scope trace showing standalone LQR state regulation" />
  <p class="project-caption">Standalone LQR response: the controller drives orientation error (x2, blue) and angular velocity (x3, orange) smoothly to zero from initial perturbation, while optical power (x1, yellow) remains stable.</p>
</div>

---

## EKF & LQR Implementation

Instead of relying on prebuilt Simulink blocks, which weren't robust enough for this specific setup, we wrote custom MATLAB function blocks for both the plant and the controllers. 

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">EKF Estimator Architecture</span>
    <img src="/images/projects/ekf-lqr/ekf-plant-block-diagram.png" alt="Simulink model of nonlinear plant and Extended Kalman Filter state estimator" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">LQR Controller Architecture</span>
    <img src="/images/projects/ekf-lqr/lqr-block-diagram.png" alt="Simulink block diagram of standalone LQR state feedback control" />
  </div>
</div>

--- 

## Closed-Loop Troubleshooting & Takeaways

While the EKF and LQR worked perfectly on their own, the fully closed-loop system failed to stabilize. The estimated states diverged, and the controller couldn't drive the orientation back to zero. We ran through a series of systematic troubleshooting steps to isolate the issue: 

* **Adding the $\psi_k$ Offset:** We tried adding the artificial angle offset term ($\psi_k$) used in the original reference paper's state estimation, but it actually worsened the results and introduced more oscillation. 
* **Covariance Tuning:** We tuned the $Q_{EKF}$, $R_{EKF}$, and $Q_{LQR}$ parameters. This changed the filter's behavior but didn't solve the instability or yield a demonstrable improvement. 
* **Actuator Limits:** We bounded the state values, limiting the angle to $\pm 15$ degrees and 0.2 degrees per timestep. This slowed the response but still ultimately ended in oscillation. 
* **Timesteps & Initial Conditions:** Adjusting the simulation timestep just stretched or compressed the response, and changing initial conditions offered no improvement. 

### Final Thoughts

This project was a great lesson in the reality of control systems. According to the Principle of Separation, independent estimators and controllers should combine perfectly. In practice, a hidden modeling flaw in the nonlinear plant or a missing theoretical constraint can easily break the closed loop.
