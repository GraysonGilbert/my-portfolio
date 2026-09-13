## **Custom Arduino-Controlled NanoLeaf LED Installation**

**Overview** Built over the winter break of 2020-2021, this interactive lighting installation served as a self-taught introduction to embedded electronics and Arduino programming. The piece features 280 individually addressable NeoPixels housed within custom hand-crafted hexagonal frames, allowing users to dynamically control brightness and display patterns via integrated hardware inputs.

**Hardware & Fabrication**

* **Prototyping:** Validated the circuit logic, structural design, and baseline code on a smaller two-hexagon proof-of-concept before scaling up to the full array.  
* **Manual Construction:** Hand-crafted the hexagonal enclosures and custom diffusion panels using available shop tools. The frames were assembled and finished with a black wood stain for a clean, professional aesthetic when the LEDs are powered off.

**Electronics & Firmware**

* **Microcontroller Integration:** Driven by an Arduino operating as the central LED controller.  
* **Analog User Inputs:** Integrated a potentiometer to map real-time brightness adjustments and a pushbutton hardware interrupt to cycle through various color profiles and dynamic lighting patterns.  
* **Power Distribution:** To prevent voltage drop and maintain color accuracy across all 280 diodes, a dedicated 5V rail was routed through the matrix to periodically inject power into the LED strips.

**Impact & Takeaways** While I had previous software experience with MATLAB, C++, and Python, this project was my first time bridging code with physical hardware. Successfully developing the C++ Arduino firmware alongside the physical circuitry sparked a strong, lasting interest in hardware-software integration, directly motivating my subsequent pursuit of advanced coursework in Mechatronics and the Internet of Things (IoT).

[https://youtu.be/PmaV1rqOmeM?si=us3r\_LSOIM2ei7Op](https://youtu.be/PmaV1rqOmeM?si=us3r_LSOIM2ei7Op)

[https://youtu.be/yTLSK1VgtIE?si=SDog1VjL2VoDxfQG](https://youtu.be/yTLSK1VgtIE?si=SDog1VjL2VoDxfQG)