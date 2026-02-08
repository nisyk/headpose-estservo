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

Each servo also requires:
- 5 V -> 5V power supply  
- GND -> Common ground (the point where the Arduino GND pin and the negative power supply are connected).

> *Note:* You can do with Arduino 5V pin and Arduino GND pin, but it's not recommended, because it's unstable and unsafe for Arduino's voltage regulator. 
> (It can causes your Arduino reset and worst case: it can harm your Arduino)

# Installation

1. Clone the Repository

```bash
git clone <repository-url>
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
- `numpy
- `protobuf`

3. Upload Arduino Code

	1. Open `arduino_code.ino` in Arduino IDE
	2. Select your board type and port
	3. Click Upload

4. Configure Serial Port
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
arduino = serial.Serial(port=Check available ports:'COM3', baudrate=115200, timeout=0.1)
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

# Usage

1. Ensure your Arduino is connected via USB (Including the servos)
2. Run the Python script:
```bash
python main.py
```
3. Position yourself in front of the webacm
4. The servo should track your head movements
5. Press **'q'** to quit