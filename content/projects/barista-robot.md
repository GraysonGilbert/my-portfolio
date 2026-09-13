---
title: "Autonomous 6-DOF Robotic Barista: Gazebo & ROS 2 Manipulation"
date: 2025-03-01
tags: ["Robotics", "ROS 2", "Manipulation", "Kinematics", "Gazebo", "Python"]
summary: "Engineered an autonomous multi-station coffee preparation workcell utilizing a Universal Robots UR10e 6-DOF manipulator, Robotiq 2F-140 gripper, and Intel RealSense D435 in ROS 2 Humble and Gazebo. Features analytical DH forward kinematics, Jacobian pseudoinverse trajectory tracking, singularity-free workspace optimization, and joint effort physics stabilization."
cover:
  image: "images/projects/barista-robot/demo.png"
  alt: "Autonomous 6-DOF Barista Robot Gazebo simulation, RViz perception, and ROS 2 control console"
  hiddenInSingle: true
weight: 5
---

Automating the end-to-end beverage preparation cycle demands precise multi-station coordination, robust perception, and mathematically rigorous robotic manipulation. This project presents the complete design and implementation of an autonomous robotic barista workcell in ROS 2 Humble and Gazebo, anchored by a Universal Robots UR10e 6-DOF collaborative arm, a Robotiq 2F-140 adaptive gripper, and an eye-in-hand Intel RealSense D435 depth camera.

<div class="project-stats">
  <div class="project-stat"><span class="project-stat-value">6-DOF</span><span class="project-stat-label">UR10e Manipulator</span></div>
  <div class="project-stat"><span class="project-stat-value">3.08 m³</span><span class="project-stat-label">Constrained Workspace</span></div>
  <div class="project-stat"><span class="project-stat-value">J† Tracking</span><span class="project-stat-label">Jacobian Pseudoinverse</span></div>
  <div class="project-stat"><span class="project-stat-value">RealSense D435</span><span class="project-stat-label">Eye-in-Hand Vision</span></div>
</div>

<div class="project-figure">
  <img src="/images/projects/barista-robot/demo.png" alt="Multi-pane ROS 2 development console for the 6-DOF barista robot" />
  <p class="project-caption">Synchronous multi-window runtime console: Gazebo physics simulation with the 4-station café workcell (bottom-right), RViz 3D model & RealSense camera feed (top-right), ROS 2 controller manager and spawner logs (top-left), and the coffee_path.py trajectory sequencer (bottom-left).</p>
</div>

---

## 4-Station Café Workcell Architecture

The physical workcell is laid out radially around a central pedestal-mounted UR10e arm, partitioning the beverage preparation pipeline into four dedicated operational stations. Each station exposes specialized ROS 2 action interfaces that coordinate mechanical positioning with peripheral dispensing hardware.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Station 1</div>
    <div class="project-arch-title">Cup Ingestion & Selection</div>
    <ul class="project-arch-list">
      <li>Vertical gravity-fed storage stack integration</li>
      <li>Eye-in-hand camera alignment for size verification</li>
      <li>Robotiq 2F-140 adaptive grasp with slip detection</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Station 2</div>
    <div class="project-arch-title">Espresso Extraction & Dispensing</div>
    <ul class="project-arch-list">
      <li>Sub-millimeter portafilter docking trajectory</li>
      <li>Precision shot timer synchronization via ROS 2 services</li>
      <li>Spill-free volumetric extraction positioning</li>
    </ul>
  </div>
</div>

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Station 3</div>
    <div class="project-arch-title">Milk Steaming & Frothing</div>
    <ul class="project-arch-list">
      <li>Additive ingredient pour and pitcher transfer</li>
      <li>Overhead vertical trajectory descent into steam wand zone</li>
      <li>Thermal cycle monitoring and vortex stabilization</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Station 4</div>
    <div class="project-arch-title">Customer Presentation & Serving</div>
    <ul class="project-arch-list">
      <li>Final delivery dropoff on customer pickup counter</li>
      <li>Controlled gripper release and retract trajectory</li>
      <li>Table clearance confirmation via depth sensor verification</li>
    </ul>
  </div>
</div>

---

## Kinematic Modeling & Resolved-Rate Cartesian Control

### Forward Kinematics

To establish precise spatial awareness from the robot base frame (`base_link`) to the end-effector tool frame (`tool0`), the manipulator kinematics were modeled using standard Denavit-Hartenberg (DH) parameterization across all six revolute joints.

| Joint $i$ | Link Length $a_i$ (m) | Link Twist $\alpha_i$ (rad) | Link Offset $d_i$ (m) | Joint Variable $\theta_i$ |
| :---: | :---: | :---: | :---: | :---: |
| **1** | 0.0000 | $\pi/2$ | 0.1273 | $\theta_1$ (Variable) |
| **2** | -0.6120 | 0.0000 | 0.0000 | $\theta_2$ (Variable) |
| **3** | -0.5723 | 0.0000 | 0.0000 | $\theta_3$ (Variable) |
| **4** | 0.0000 | $\pi/2$ | 0.1639 | $\theta_4$ (Variable) |
| **5** | 0.0000 | $-\pi/2$ | 0.1157 | $\theta_5$ (Variable) |
| **6** | 0.0000 | 0.0000 | 0.0922 | $\theta_6$ (Variable) |

### Inverse Kinematics & Trajectory Tracking

Cartesian trajectory execution is governed by resolved-rate motion control. The relationship between joint velocity space and end-effector cartesian velocity is expressed through the geometric Jacobian matrix:

<div class="project-math">
$$\dot{\mathbf{x}} = \mathbf{J}(\mathbf{q}) \, \dot{\mathbf{q}}$$
</div>

To compute required joint velocity commands along complex task trajectories, the Jacobian is inverted using the Moore-Penrose pseudoinverse, ensuring robust handling of redundant degrees of freedom:

<div class="project-math">
$$\dot{\mathbf{q}} = \mathbf{J}^{\dagger}(\mathbf{q}) \, \dot{\mathbf{x}} = \mathbf{J}^T \left( \mathbf{J} \mathbf{J}^T \right)^{-1} \dot{\mathbf{x}}$$
</div>

Discrete-time execution is handled via forward Euler integration at the controller update frequency:

<div class="project-math">
$$\mathbf{q}_{k+1} = \mathbf{q}_k + \dot{\mathbf{q}}_k \, \Delta t$$
</div>

### Workspace Optimization

To eliminate self-collisions with the café service tables and prevent unreachable singularities, the operational workspace was analytically bounded to an upper hemisphere. For a maximum reach radius $R = 1.3\text{ m}$, the usable workspace volume is constrained to:

<div class="project-math">
$$V \approx \frac{2}{3}\pi R^3 \approx 3.08\text{ m}^3$$
</div>

---

## Simulation Dynamics & Singularity Avoidance

### Kinematic Singularity Mitigation

During initial testing, commanding linear trajectories passing near the default vertical home position resulted in severe computational instabilities and infinite joint velocity spikes ($\det(\mathbf{J}\mathbf{J}^T) \approx 0$). This occurs due to wrist and shoulder alignment singularities where the arm loses one or more degrees of spatial translation. 

**Solution:** Engineered a custom offset 'Ready' posture that maintains a non-zero elbow angle and offsets the wrist axes from collinear alignment. This preserves full manipulability across all four workstation transfer corridors.

### Gazebo Physics Stabilization

Simulating a high-payload collaborative arm handling rigid ceramic cups introduced severe physics artifacts, including joint separation, gripper jaw oscillation, and contact penetration under gravity.

**Solution:** Implemented dedicated ROS 2 Effort Controllers (`joint_effort_controller`) operating in parallel with position loops, coupled with active gravity compensation torque feedforward vectors $\boldsymbol{\tau}_g(\mathbf{q})$ computed dynamically from rigid-body dynamics equations. This completely eliminated gravitational sag and stabilized contact forces during portafilter docking.

---

## Perception & Multi-Node Coordination

### Eye-in-Hand Vision Integration

An Intel RealSense D435 depth sensor mounted near the end-effector streams raw RGB and depth data on `/camera/camera_sensor/image_raw`. RViz was utilized extensively for real-time sensor verification, depth overlay debugging, and TF frame validation between camera optical frames and tool center points.

### ROS 2 Communication Topology

The system execution pipeline is orchestrated by the `coffee_path.py` finite state machine node, which communicates asynchronously with the `ros2_control` hardware interface manager (`joint_state_broadcaster`, `position_controller`, `effort_controller`, `gripper_position_controller`). Node graph inspection via `rqt_graph` confirmed clean decoupling between perception vision pipelines, trajectory execution state machines, and hardware drivers.
