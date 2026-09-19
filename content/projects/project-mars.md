---
title: "Project MARS: Multi-Agent Robotic SLAM"
date: 2025-11-22
tags: ["Robotics", "ROS 2", "SLAM", "C++", "Python", "Autonomous Systems"]
summary: "Scalable multi-robot mapping and autonomous exploration system in ROS 2 Humble and Webots. Orchestrates a TurtleBot3 fleet using slam_toolbox, REP-105 compliant TF trees, deterministic occupancy-grid fusion in mars_overseer, and an automated TDD CI/CD pipeline."
cover:
  image: "images/projects/mars/thumbnail.png"
  alt: "Project MARS Multi-Agent Robotic SLAM in Webots and RViz2"
  hiddenInSingle: true
weight: 3
---

Project MARS (Multi-Agent Robotic SLAM) is a distributed autonomous mapping architecture built in ROS 2 Humble and Webots. The system coordinates a fleet of TurtleBot3 mobile robots to map complex indoor environments. By pairing decentralized, per-robot local SLAM with a centralized probabilistic map fusion node, MARS generates high-fidelity spatial reconstructions while entirely avoiding inter-robot interference.

[Github Repository](https://github.com/GraysonGilbert/project_mars.git)

<div class="project-video-grid">
  <div class="project-video">
    <iframe
      src="https://www.youtube.com/embed/BxGX-qR2iZE"
      title="Project MARS - 1-Robot SLAM Execution"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen>
    </iframe>
  </div>
  <div class="project-video">
    <iframe
      src="https://www.youtube.com/embed/aSOs7f2JbLM"
      title="Project MARS - Multi-Robot Fleet SLAM Execution"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen>
    </iframe>
  </div>
</div>

---

## System Architecture

The software is built using a modular ROS 2 architecture connected via FastDDS. By keeping the packages strictly independent, the system maintains stable, real-time performance across all nodes in the simulation.

| Subsystem / Layer | Technology Stack | Architectural Responsibility |
| :--- | :--- | :--- |
| **Robotics Middleware** | ROS 2 Humble (C++ / Python) | Distributed DDS communication, namespaced topics & TF trees |
| **Simulation Environment** | Webots Multi-Robot Platform | Physics-accurate indoor facility modeling & sensor simulation |
| **Local Mapping Engine** | slam_toolbox (Synchronous) | Per-robot 2D graph-based SLAM & occupancy grid estimation |
| **Robot Hardware Model** | TurtleBot3 Burger Fleet | Differential-drive kinematic platform with 360° planar LiDAR |
| **Verification & Testing** | GoogleTest + Catch2 | Algorithmic unit testing and multi-node integration test harnesses |
| **CI/CD Pipeline** | GitHub Actions + CodeCov | Automated builds, regression testing, and code coverage metrics |

---

## ROS 2 Package Structure

The repository is divided into three core ROS 2 packages separating simulation, global mapping, and navigation:

<div class="project-arch-grid project-arch-grid-3col">
  <div class="project-arch-card">
    <div class="project-arch-core">Package: mars_fleet_bringup</div>
    <div class="project-arch-title">Simulation & Fleet Orchestration</div>
    <ul class="project-arch-list">
      <li>Launches the Webots multi-robot environment, spawns namespaced robot instances, and initializes synchronized RViz2 telemetry.</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Package: mars_overseer</div>
    <div class="project-arch-title">Global Map Fusion Engine</div>
    <ul class="project-arch-list">
      <li>The centralized fusion node. It subscribes to the local occupancy grids from each robot, aligns them using known starting poses, and merges the cell probabilities into a single, unified <code>/global_map</code>.</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Package: mars_exploration</div>
    <div class="project-arch-title">Autonomous Navigation</div>
    <ul class="project-arch-list">
      <li>Manages the fleet's movement. It partitions the physical space into sectors to avoid inter-robot collisions and handles automated waypoint sequencing for rapid environmental coverage.</li>
    </ul>
  </div>
</div>

---

## DevOps, Test-Driven Development & Continuous Integration

The project follows standard software engineering practices to keep the ROS 2 codebase maintainable and reliable:

- **Agile Iterative Process (AIP):** Development was managed in 1-week sprints using standard feature branches and pull requests.

- **Test-Driven Development (TDD):** Core algorithms and math transformations are unit-tested with GoogleTest, while node communication and bringup sequences rely on Catch2 integration tests.

- **Continuous Integration (CI/CD):**  GitHub Actions automatically run `colcon build` and `colcon test` on every PR to catch regressions, tracking coverage via CodeCov.

- **Documentation Target:** A custom CMake target generates offline Doxygen documentation for the custom ROS interfaces and class hierarchies.