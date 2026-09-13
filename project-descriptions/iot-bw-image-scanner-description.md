## **IoT Black & White Image Scanner**

**Overview** Developed as a capstone project for an Internet of Things (IoT) course, this electromechanical system digitizes physical images using only an LED and a photoresistor. Powered by a Raspberry Pi and controlled entirely wirelessly via a custom web interface, the project required the seamless integration of custom kinematics, sensor data acquisition, and full-stack web communication. Working in a three-person team, I served as the mechanical and hardware lead.

**Mechanical Design & Hardware Integration (My Contribution)**

* **CAD & Kinematics:** Designed and modeled the complete X-Y gantry system from scratch using SolidWorks. I created full digital assemblies to verify tolerances and validate the motion of all moving parts prior to manufacturing.  
* **Fabrication & Assembly:** 3D printed all structural components and tracks using PLA. The entire assembly was designed to be modular, utilizing standard M3 nuts and bolts for easy teardown and transportation.  
* **Dual-Actuator Motion System:** Engineered a two-axis scanning bed to meet project constraints. A stepper motor and gear assembly drove the vertical axis (y-axis indexing), while a servo motor actuated the horizontal sensor sweep across the page.  
* **Electrical Routing & Validation:** Wired all electronic components to the Raspberry Pi's GPIO pins. I wrote custom object-oriented Python test scripts to validate individual sensor readings, motor actuation, and circuit integrity during the build process.

**Software & The Scanning Process**

* **Data Acquisition:** The sensor head passes millimeters above the page, utilizing the photoresistor to measure the intensity of the LED light reflecting off the paper. Darker ink absorbs light, while white paper reflects it.  
* **Image Synthesis:** The raw analog light readings are stored as an array of data points. The backend software normalizes these values and maps them to a greyscale matrix, essentially reconstructing the image pixel by pixel.  
* **Web Interface (Team Contribution):** My teammates developed a custom HTML webpage and background CGI/Python scripts. Users can input the physical dimensions of their image, remotely initiate the scanning sequence, and download the final synthesized JPEG directly from the browser.

&nbsp;

[https://www.youtube.com/watch?v=NDIJYcaRBZA\&t=14s](https://www.youtube.com/watch?v=NDIJYcaRBZA&t=14s)

[https://www.youtube.com/watch?v=9Je25YHhv7c\&t=18s](https://www.youtube.com/watch?v=9Je25YHhv7c&t=18s)