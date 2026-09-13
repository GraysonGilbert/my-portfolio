## Extended Kalman Filter & LQR Control for UWOC Alignment

**Overview**
Co-authored a research project to recreate and verify an advanced state-space control model for Underwater Wireless Optical Communication (UWOC) systems. The project focused on stabilizing communication links between underwater robots by developing an Extended Kalman Filter (EKF) for optimal state estimation, paired with a Linear Quadratic Regulator (LQR). 

**System Modeling & State-Space Formulation**
*   **Physical Channel Modeling:** Modeled the physical optical communication channel by accounting for spherical signal spreading via the inverse-square law and signal attenuation caused by absorption using Beer's Law.
*   **3-State Variable Architecture:** Formulated an augmented state vector comprising three variables: the power at the receiver, the angle of the receiver's normal, and the angular velocity of the receiver actuator. 
*   **Force-Velocity Advantage:** Integrating angular velocity into the state design created a force-velocity-based controller, providing an additional dimension to tune the controller's response compared to traditional two-state position models.

**Estimator & Controller Design**
*   **Observability & Jacobian Matrices:** Evaluated the system's observability by calculating the rank of the Observation matrix using the Jacobian. Mathematical analysis proved the system was unobservable with a single measurement, prompting the integration of a dual-measurement approach using sequential angle variations to achieve full rank.
*   **EKF Implementation:** Designed an Extended Kalman Filter algorithm to linearize the output equations and recursively predict nonlinear system states in the presence of Gaussian process and measurement noise.
*   **LQR Optimization:** Developed an LQR control algorithm aimed at minimizing the receiver's pointing error by solving the Riccati equation to produce optimal plant inputs.

**Simulation & Results Analysis**
*   **MATLAB/Simulink Environment:** Built the complete plant, EKF, and LQR architecture using custom MATLAB function blocks within Simulink. 
*   **Principle of Separation:** Tested the estimator and controller independently before integration. The standalone EKF successfully tracked true system values within roughly two seconds, and the standalone LQR controller independently drove the orientation and velocity states to the desired zero values in the same timeframe.
*   **Closed-Loop Integration Challenges:** Encountered tracking and stability issues upon closing the loop between the simulated plant, the EKF, and the LQR controller. Conducted rigorous troubleshooting—including tuning covariance weighting matrices, adjusting simulation timesteps, modifying initial conditions, and implementing strict state boundary limits—which ultimately pointed to a core limitation in the nonlinear plant model's integration.

github repo: https://github.com/GraysonGilbert/ENPM667_Project_1