## ACME Monocular Perception and Tracking Module

**Overview**
Developed a C++ perception module for ACME Robotics to function as a human (N>=1) obstacle detector and tracker using a monocular video camera. This module was engineered as the midterm assignment for the ENPM700 Software Development course.

**Perception & Machine Learning Architecture**
*   **Object Detection:** The module processes input images using the YOLOv5 object detection model to actively detect and track any person within the robot's frame of view.
*   **Depth Estimation:** Simultaneously, the system applies the "Depth Anything" monocular depth estimation model to calculate the relative depth of each individual located in the view.
*   **Sensor Fusion:** By fusing the outputs of both machine learning models, the system accurately detects people, perceives their location relative to the robot, and tracks their movement. The resulting visual output overlays the YOLO human detections with the estimated depth directly on the detection label.
*   **Camera Calibration:** To guarantee depth accuracy, the system incorporates a calibration process where the user utilizes objects of known dimensions at reference locations to calculate a scale factor and offset. These calibration parameters are then applied and modified via the `DepthProcessor.hpp` file.

**Software Engineering & Integration**
*   **C++ Library Implementation:** The project is structured as a modular C++ library, designed to allow end-users to leverage its methods to build custom perception applications for their specific use cases.
*   **Dependencies:** The module relies on Git LFS for large file storage and strictly requires OpenCV 4.10 (built from source) to properly run the ONNX machine learning models.
*   **CI/CD & Testing Pipeline:** The repository is supported by a comprehensive automated development environment utilizing CMake for build generation, CTest for unit testing, Cppcheck for static code analysis, Doxygen for documentation generation, and gcovr/lcov for building HTML code coverage reports.
