---
title: "Autonomous 6-DOF Robotic Barista: Gazebo & ROS 2 Manipulation"
date: 2025-03-01
tags: ["Robotics", "ROS 2", "Manipulation", "Kinematics", "Gazebo", "Python", "Computer Vision", "OpenCV"]
summary: "Engineered an autonomous multi-station coffee preparation workcell utilizing a Universal Robots UR10e 6-DOF manipulator, Robotiq 2F-140 gripper, and Intel RealSense D435 in ROS 2 Humble and Gazebo. Features analytical DH forward kinematics, Jacobian pseudoinverse trajectory tracking, singularity-free workspace optimization, and joint effort physics stabilization."
cover:
  image: "images/projects/barista-robot/thumbnail.png"
  alt: "Autonomous 6-DOF Barista Robot Gazebo simulation, RViz perception, and ROS 2 control console"
  hiddenInSingle: true
weight: 5
---

Getting a robot to make a cup of coffee from start to finish takes a lot of coordination, solid computer vision, and exact kinematics. For this project, I built an autonomous robotic barista workcell inside ROS 2 Humble and Gazebo. The setup runs on a Universal Robots UR10e 6-DOF arm, a Robotiq 2F-140 adaptive gripper, and an Intel RealSense D435 depth camera mounted right on the wrist (eye-in-hand).

[GitHub Repository](https://github.com/GraysonGilbert/barista_robot.git)

<div class="project-figure">
  <img src="/images/projects/barista-robot/demo.png" alt="Multi-pane ROS 2 development console for the 6-DOF barista robot" />
  <p class="project-caption">My ROS 2 development setup running synchronously: The Gazebo physics simulation of the 4-station café (bottom-right), RViz 3D model with the RealSense camera feed (top-right), ROS 2 controller logs (top-left), and the coffee_path.py script handling the trajectory sequence (bottom-left).</p>
</div>

---

## 4-Station Café Workcell Architecture

I designed the workcell in a circle around a central pedestal-mounted UR10e arm. This breaks down the coffee-making process into four specific stations. Every station uses its own ROS 2 action interfaces to sync up the arm's movements with the dispensing hardware.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Station 1</div>
    <div class="project-arch-title">Cup Ingestion & Selection</div>
    <ul class="project-arch-list">
      <li>Grabs cups from a vertical gravity-fed dispenser</li>
      <li>Uses the eye-in-hand camera to verify the cup size</li>
      <li>Picks up the cup using the Robotiq gripper's adaptive grasp and slip detection</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Station 2</div>
    <div class="project-arch-title">Espresso Extraction & Dispensing</div>
    <ul class="project-arch-list">
      <li>Moves into position under the espresso spout</li>
      <li>Holds the cup perfectly steady to prevent spills during extraction</li>
    </ul>
  </div>
</div>

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Station 3</div>
    <div class="project-arch-title">Milk Steaming & Frothing</div>
    <ul class="project-arch-list">
      <li>Handles adding milk and transferring the pitcher</li>
      <li>Moves straight down into the steam wand zone</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Station 4</div>
    <div class="project-arch-title">Customer Presentation & Serving</div>
    <ul class="project-arch-list">
      <li>Drops the finished drink off at the pickup counter</li>
      <li>Carefully opens the gripper and backs the arm away</li>
      <li>Checks the depth sensor to confirm the table is clear before serving</li>
    </ul>
  </div>
</div>

-## Kinematic Modeling & Resolved-Rate Cartesian Control

### Forward Kinematics

To figure out exactly where the tool frame (`tool0`) is relative to the robot's base (`base_link`), I set up the forward kinematics using standard Denavit-Hartenberg (DH) parameters for all six of the UR10e's revolute joints.

| Joint $i$ | Link Length $a_i$ (m) | Link Twist $\alpha_i$ (rad) | Link Offset $d_i$ (m) | Joint Variable $\theta_i$ |
| :---: | :---: | :---: | :---: | :---: |
| **1** | 0.0000 | $\pi/2$ | 0.1807 | $\theta_1$ (Variable) |
| **2** | -0.6120 | 0.0000 | 0.0000 | $\theta_2$ (Variable) |
| **3** | -0.5723 | 0.0000 | 0.0000 | $\theta_3$ (Variable) |
| **4** | 0.0000 | $\pi/2$ | 0.1742 | $\theta_4$ (Variable) |
| **5** | 0.0000 | $\pi/2$ | 0.1199 | $\theta_5$ (Variable) |
| **6** | 0.0000 | 0.0000 | 0.1166 | $\theta_6$ (Variable) |

### Inverse Kinematics & Trajectory Tracking

For actually moving the arm along a Cartesian path, I used resolved-rate motion control. To map the end-effector's Cartesian velocity back to the joint velocities, we use the geometric Jacobian matrix:

<div class="project-math">
$$\dot{\mathbf{x}} = \mathbf{J}(\mathbf{q}) \, \dot{\mathbf{q}}$$
</div>

Since we need to calculate the necessary joint velocities to follow the coffee-making trajectories, I inverted the Jacobian using the Moore-Penrose pseudoinverse. This handles any redundant degrees of freedom smoothly:

<div class="project-math">
$$\dot{\mathbf{q}} = \mathbf{J}^{\dagger}(\mathbf{q}) \, \dot{\mathbf{x}} = \mathbf{J}^T \left( \mathbf{J} \mathbf{J}^T \right)^{-1} \dot{\mathbf{x}}$$
</div>

To run this on the actual controller hardware, I discretized the execution using a standard forward Euler integration step matching the controller's update loop:

<div class="project-math">
$$\mathbf{q}_{k+1} = \mathbf{q}_k + \dot{\mathbf{q}}_k \, \Delta t$$
</div>

### Workspace Optimization

To keep the arm from smashing into the service tables and to avoid kinematic singularities, I restricted the robot's operational workspace to an upper hemisphere. Given the UR10e's maximum reach radius of $R = 1.3\text{ m}$, this limits our safe working volume to:

<div class="project-math">
$$V \approx \frac{2}{3}\pi R^3 \approx 3.08\text{ m}^3$$
</div>

---

## Simulation Dynamics & Singularity Avoidance

### Kinematic Singularity Mitigation

When I first started testing, running linear trajectories near the default vertical home position caused massive computational spikes and infinite joint velocities ($\det(\mathbf{J}\mathbf{J}^T) \approx 0$). This happens because of wrist and shoulder alignment singularities where the arm essentially loses a degree of spatial translation.

**Solution:** I programmed a custom offset 'Ready' posture that keeps a non-zero elbow angle and prevents the wrist axes from lining up perfectly. This ensures the arm maintains full manipulability when moving between all four workstation corridors.

### Gazebo Physics Stabilization

Simulating a heavy collaborative arm handling rigid ceramic cups in Gazebo led to some nasty physics artifacts—like joints separating, the gripper jaws oscillating, and meshes clipping into each other under gravity.

**Solution:** I implemented dedicated ROS 2 Effort Controllers (`joint_effort_controller`) running alongside the position loops, and fed in active gravity compensation torque vectors $\boldsymbol{\tau}_g(\mathbf{q})$ calculated on the fly from the rigid-body dynamics. This completely killed the gravitational sag and stabilized the contact forces when docking the portafilter.

---

## Perception & Multi-Node Coordination

### Eye-in-Hand Vision Integration

I mounted an Intel RealSense D435 depth sensor near the end-effector to stream raw RGB and depth data over `/camera/camera_sensor/image_raw`. I relied heavily on RViz to verify the sensor feeds in real-time, debug the depth overlays, and validate the TF frames between the camera's optical frame and the tool center point.

### ROS 2 Communication Topology

The whole execution pipeline is driven by a finite state machine inside my `coffee_path.py` node. It communicates asynchronously with the `ros2_control` hardware interface manager (hitting the `joint_state_broadcaster`, `position_controller`, `effort_controller`, and `gripper_position_controller`). Pulling up the node graph in `rqt_graph` confirmed I had a nice, clean decoupling between the perception pipelines, the trajectory state machine, and the hardware drivers.