A real-time face tracking system that uses computer vision and machine learning to detect head movements and controls Arduino servos for pan/tilt motion tracking.

![Face Tracking Demo](assets/opencv-demo-screenshot.png)

# Overview 

This project combines Python-based computer vision with Arduino hardware control to create a responsive face tracking system. The system detects facial landmarks using MediaPipe, calculates head pose orientation, and sends control signals to servos that physically follow your movements.

# Features 

- **Real-time Tracking:** Low-latency head-pose detection using OpenCV (Camera input) and Mediapipe
- **Dead Zone Logic**: Prevent unnecessary servo movements within threshold ranges
- **Optimized Data Transfer**: 115200 baud serial connection and integer data sending for minimal response delay
- **Visual Feedback**: Live video feed with angle measurements and FPS counter

# Hardware Requirements

![Wiring Diagram](assets/circuit.png)


| Components/Tools                                            | Qty. |
| ----------------------------------------------------------- | ---- |
| Arduino board (Uno, Mega, Nano, etc.)                       | 1    |
| Servo motors (SG90, MGS90, etc.)                            | 2    |
| USB cable for Arduino connection                            | 1    |
| Webcam                                                      | 1    |
| External 5V power supply (reccomended)                      | 1    |
| Pan/tilt bracket (optional)                                 | 1    |
| Jumper cable for servo connection (depends on your circuit) | -    |

## Wiring

| Servo   | Function      | Arduino Pin |
| ------- | ------------- | ----------- |
| Servo 1 | Tilt (Y-axis) | Pin D6      |
| Servo 2 | Pan (X-axis)  | Pin D9      |
