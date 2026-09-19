---
title: "State Estimation & Optimal LQR Control for Underwater Optical Alignment"
date: 2024-11-15
tags: ["Control Systems", "EKF", "LQR", "Simulink", "State Estimation"]
summary: "Recreated and verified an augmented 3-state state-space control framework for Underwater Wireless Optical Communication (UWOC). Implemented an Extended Kalman Filter (EKF) with dual-measurement observability paired with a Linear Quadratic Regulator (LQR) in MATLAB/Simulink to achieve sub-two-second optical alignment under attenuation and sensor noise."
cover:
  image: "images/projects/ekf-lqr/thumbnail.png"
  alt: "Simulink model of nonlinear plant and Extended Kalman Filter state estimator"
  hiddenInSingle: true
weight: 6
---

Setting up an optical communication link between underwater robots is tough due to severe signal attenuation and the need for precise alignment. For this project, my partner and I worked to recreate and verify an advanced control framework using a recursive Extended Kalman Filter (EKF) and a Linear Quadratic Regulator (LQR). We engineered the simulation using custom blocks in MATLAB and Simulink. You can find the codebase on [GitHub](https://github.com/GraysonGilbert/ENPM667_Project_1).

<div class="project-figure">
  <img src="/images/projects/ekf-lqr/ekf-plant-block-diagram.png" alt="Simulink model of nonlinear plant and Extended Kalman Filter state estimator" />
  <p class="project-caption">The Simulink system architecture, showing the nonlinear optical plant, sensor feedback loop, EKF estimator, and real-time scopes.</p>
</div>

---

## Channel Modeling & State Vector Architecture

To accurately simulate an underwater optical link, you have to account for both the physical spread of the beam and how marine water absorbs light.

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

## Simulation & System Realities

We built the system based on the Principle of Separation, testing the EKF estimator and the LQR controller independently before combining them. 

* **EKF Tracking:** The Extended Kalman Filter successfully estimated and tracked the true system values within about two seconds of simulation time.
* **LQR Control:** Running independently, the LQR successfully drove the angle and velocity states to zero within 20 time steps.
* **Closed-Loop Challenges:** When we fully closed the loop by feeding the EKF estimates into the LQR, the system oscillated and failed to stabilize. We suspected this was due to an underlying plant modeling issue or the absence of an artificial angle offset term ($\psi_k$) used in the original reference paper to force stability. It was a great practical lesson in how theoretical control loops don't always behave perfectly once integrated.

---

## Observability & The Dual-Measurement Fix

Before relying on an Extended Kalman Filter, my partner and I had to prove the system states were actually observable from the sensor data, as detailed in ENPM667_PROJECT1_Sweeney_Gilbert_2.pdf. 

### Single-Measurement Limitation

We calculated the Jacobian matrix to build the observability matrix ($O$) and check its rank. 

$$O = \begin{bmatrix} C \\ CA \\ CA^2 \end{bmatrix}$$

The determinant evaluated to zero, meaning the rank was less than 3, rendering the system unobservable with just one measurement. Because the beam's light intensity is symmetric about the normal, a single power measurement does not provide enough information to estimate all states. 

### The Dual-Measurement Solution

To fix this without the added cost and complexity of a second physical receiver, we modeled a single receiver taking measurements while rotating through a predefined array of angles, changing by 2 degrees per step. By taking two sequential measurements, we restored the observability matrix rank to 3, guaranteeing the EKF could mathematically reconstruct all the states. 

## Standalone EKF & LQR Testing

Following the Principle of Separation, we built and tested the EKF and LQR completely independently before attempting to close the loop. 

As the plots show, the standalone EKF successfully filtered the Gaussian measurement noise and locked onto the true system values after about two seconds of simulation time. Running on its own with the known true states, the LQR successfully drove the angle and velocity states to zero within 20 time steps. 

## EKF & LQR Implementation

Instead of relying on prebuilt Simulink blocks, which weren't robust enough for this specific setup, we wrote custom MATLAB function blocks for both the plant and the controllers. 

## Closed-Loop Troubleshooting & Takeaways

While the EKF and LQR worked perfectly on their own, the fully closed-loop system failed to stabilize. The estimated states diverged, and the controller couldn't drive the orientation back to zero. We ran through a series of systematic troubleshooting steps to isolate the issue: 

* **Adding the $\psi_k$ Offset:** We tried adding the artificial angle offset term ($\psi_k$) used in the original reference paper's state estimation, but it actually worsened the results and introduced more oscillation. 
* **Covariance Tuning:** We tuned the $Q_{EKF}$, $R_{EKF}$, and $Q_{LQR}$ parameters. This changed the filter's behavior but didn't solve the instability or yield a demonstrable improvement. 
* **Actuator Limits:** We bounded the state values, limiting the angle to $\pm 15$ degrees and 0.2 degrees per timestep. This slowed the response but still ultimately ended in oscillation. 
* **Timesteps & Initial Conditions:** Adjusting the simulation timestep just stretched or compressed the response, and changing initial conditions offered no improvement. 

### Final Thoughts

This project was a great lesson in the reality of control systems. According to the Principle of Separation, independent estimators and controllers should combine perfectly. In practice, a hidden modeling flaw in the nonlinear plant or a missing theoretical constraint can easily break the closed loop.
