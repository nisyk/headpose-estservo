# Arduino Servo Head Pose Estimation

A real-time face pose estimation tracking system that uses computer vision and machine learning to detect head movements and controls Arduino servos for pan/tilt motion tracking.

![Face Tracking Demo](assets/opencv-demo-screenshot.png)

## Overview 

This project combines Python-based computer vision with Arduino hardware control to create a responsive face tracking system. The system detects facial landmarks using MediaPipe, calculates head pose orientation, and sends control signals to servos that physically follow your movements.

## Features 

- **Real-time Tracking:** Low-latency head-pose detection using OpenCV (Camera input) and Mediapipe
- **Dead Zone Logic**: Prevent unnecessary servo movements within threshold ranges
- **Optimized Data Transfer**: 115200 baud serial connection and integer data sending for minimal response delay
- **Visual Feedback**: Live video feed with angle measurements and FPS counter

## Hardware Requirements

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

### Wiring

| Servo   | Function      | Signal Pin (Yellow) | Power Pin (Red) | GND Pin (Chocolate) |
| ------- | ------------- | ------------------- | --------------- | ------------------- |
| Servo 1 | Tilt (Y-axis) | Pin D6              | 5V (Power Rail) | Common GND          |
| Servo 2 | Pan (X-axis)  | Pin D9              | 5V (Power Rail) | Common GND          |

Each servo also requires:
- 5 V: 5V power supply  
- GND: Common ground (the point where the Arduino GND pin and the negative power supply are connected).

> *Note:* You can do with Arduino 5V pin and Arduino GND pin, but it's not recommended, because it's unstable and unsafe for Arduino's voltage regulator. 
> (It can causes your Arduino reset and worst case: it can harm your Arduino directly)

## Installation

1. Clone the Repository

```bash
git clone https://github.com/isy-xyz/headpose-estservo.git
cd arduino-face-tracker
```

2. Install Python Dependencies

*System Requriements:* Python 3.8-3.10 (3.10 recommended)

```bash
pip install -r requirements.txt
```

**Required libraries:**
- `opencv-python`
- `mediapipe`
- `pyserial`
- `numpy`
- `protobuf`

3. Upload Arduino Code

  1. Open `arduino_code.ino` in Arduino IDE
  2. Select your board type and port
  3. Click Upload

#### Check Serial Port
Edit the serial port in `main.py` 

**Linux:** 
```python 
arduino = serial.Serial(port='/dev/ttyUSB0', baudrate=115200, timeout=0.1)
```
Check available ports:
```bash
ls /dev/ttyUSB* /dev/ttyACM*
```

**Windows:**
```python
arduino = serial.Serial(port='COM3', baudrate=115200, timeout=0.1)
```
Check Device Manager -> Ports (COM)

**macOS**
```python
arduino = serial.Serial(port='/dev/cu.usbmodem14201', baudrate=115200, timeout=0.1)
```
Check available ports:
```bash
ls /dev/cu.*
```

## Usage

1. Ensure your Arduino is connected via USB (Including the servos)
2. Run the Python script:
```bash
python main.py
```
3. Position yourself in front of the webcam
4. The servo should track your head movements
5. Press **'q'** to quit

## Configuration


**Adjusting Servo Angles (IMPORTANT)**
Modify the `servo.write()` values based on your physical setup:
```cpp
servo1.write(60);   // Tilt up position
servo1.write(135);  // Tilt down position
servo1.write(90);   // Center position
```
> **CAUTION:** Before making the project permanent, please test the servo carefully. Otherwise, the servos may stall and overheat, which can damage them.

**Adjusting smoothing factor**
In `main.py` modify the `alpha` value like this
```python
alpha = 0.25
```
> *Note*
> - Lower values (0.1 - 0.2): Smoother rate of change, more lag
> - Higher value (0.7 - 0.9): Faster rate of change, potential jitter

**Adjusting Servo Threshold**
In `arduino_code.ino`, modify the dead zone threshold
```cpp
// Tilt servo sensitivity
if (valY > 15) {        // Adjust this value
  servo1.write(60);
} else if (valY < -15) { // And this value
  servo1.write(135);
}

// Pan servo sensitivity  
if (valX > 20) {        // Adjust this value
  servo2.write(0);
} else if (valX < -20) { // And this value
  servo2.write(180);
}
```
> *Note*
> - **Smaller threshold (10-15)**: More sensitive, frequent movement
> - **Higher threshold (20-25)**: Less sensitive, fewer movement

## Troubleshooting

#### Arduino Not Detected

- Check USB connection
- Verify wiring connections (MCU reset)
- Check correct in port in `main.py` ([Check Your Serial Port](#check%20serial%20port))
- Try a different USB cable
- Check permission on Linux 
```bash
sudo usermod -aG dialout $USER
```

#### Servos Not Moving

- Verify wiring connections
- Check servo power supply
- Check the brackets and servo stall movement
#### Laggy or Jittery Movement

- Modify the `alpha` value for faster/smoother response 
- Modify threshold values in Arduino code

#### No Face Detection

- Ensure adequate lighting
- Position face clearly in frame

---
Made with curious by 🌆 **NISY**
