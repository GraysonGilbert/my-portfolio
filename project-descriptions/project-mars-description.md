## Project MARS: Multi-Agent Robotic SLAM

**Overview**
Project MARS is a scalable multi-robot mapping and exploration system developed using ROS 2 Humble, Webots, and slam_toolbox. The system simulates a fleet of TurtleBot3 Burger robots to generate a unified global map of large indoor environments, enabling facility-scale mapping and digital-twin generation.

**System Architecture & ROS 2 Integration**
*   **Per-Robot SLAM Stack:** Each simulated TurtleBot operates within its own independent ROS namespace. The stack utilizes LiDAR (with corrected headers for a frame header bug), odometry, and a REP-105 compliant TF tree to generate individual maps via slam_toolbox.
*   **Global Map Fusion (Overseer Node):** A centralized `mars_overseer` node subscribes to each robot's individual map topic. It applies known initial transforms and performs deterministic occupancy-grid fusion to publish a single, unified `/global_map`.
*   **Autonomous Exploration:** The `mars_exploration` package drives the robots using sector-based waypoint exploration. It also includes architecture to support optional frontier-based exploration.
*   **Fleet Bringup:** A dedicated `mars_fleet_bringup` package manages the multi-robot Webots simulation. It handles slam_toolbox initialization, parameter configuration, and RViz2 visualization.

**Development Methodology & CI/CD**
*   **Agile Development:** The project was developed using an Agile Iterative Process (AIP) featuring one-week sprint cycles. The codebase was written collaboratively using pair programming techniques.
*   **Test-Driven Development (TDD):** The system was built using TDD methodologies, utilizing GoogleTest for robust unit testing and Catch2 for integration testing.
*   **Continuous Integration:** GitHub Actions pipelines were configured to automatically handle build verification, testing, and CodeCov coverage uploads. Additionally, we created a specific target to build all Doxygen documentation locally, rather than uploading them automatically through the CI pipeline.
exmaple 1 robot slam video: https://youtu.be/BxGX-qR2iZE?si=dmNw6FY83Za9jqQs
example 2 robot slam video: https://youtu.be/aSOs7f2JbLM?si=1YE-mNoCFVu2YPPb