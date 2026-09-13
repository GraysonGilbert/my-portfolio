## Autonomous 6-DOF Barista Robot

**Overview**
Developed an autonomous coffee-making robot using ROS 2, Gazebo, and a simulated Universal Robots UR10e 6-DOF collaborative robotic arm[cite: 6]. The system is designed to automate the coffee preparation process by identifying cups, dispensing ingredients, and delivering the final product across a multi-station workspace[cite: 6]. 

**Kinematics & Control**
*   **Kinematic Modeling:** Derived the Denavit-Hartenberg (DH) parameters to systematically compute the Forward Kinematics for the manipulator[cite: 6]. 
*   **Inverse Kinematics:** Implemented Jacobian-based control, utilizing the pseudoinverse of the Jacobian and Euler integration to calculate joint velocities and accurately track 3D Cartesian trajectories[cite: 6]. 
*   **Workspace Optimization:** Constrained the operational workspace to an upper hemisphere (approximately 3.08 cubic meters) to simplify trajectory planning and prevent the end-effector from interacting with spaces below the serving tables[cite: 6].
*   **Actuation & Grasping:** Integrated a modified Robotiq 2F-140 gripper for secure cup handling, applying position commands directly to the gripper joints[cite: 6]. Calculated joint torques to provide gravity compensation during arm movement[cite: 6].

**Simulation & Perception**
*   **Environment Design:** Built a custom multi-station Gazebo simulation environment consisting of cup selection, coffee dispensing, milk dispensing, and serving tables[cite: 6]. 
*   **Vision Integration:** Integrated an Intel RealSense D435 camera near the end-effector for cup detection[cite: 6]. Utilized RViz extensively to visualize the camera feed in real-time, debug URDF frame alignments, and validate the object detection algorithms[cite: 6].
*   **Node Management:** Leveraged `rqt_graph` to visualize, map, and debug the complex communication network between various ROS 2 nodes[cite: 6].

**Challenges & Optimization**
*   **Avoiding Singularities:** Discovered that the UR10e's default vertical home position caused singularity issues during inverse kinematic calculations[cite: 6]. Resolved this by establishing a custom 'ready' pose that avoids singular configurations and ensures highly repeatable trajectory executions[cite: 6].
*   **Gazebo Dynamics:** Overcame simulated physics instability—including joint separation and gripper oscillation—by implementing a joint effort controller to counteract simulated gravity and occasionally toggling physics during strict trajectory testing[cite: 6].
