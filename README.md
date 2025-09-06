# Smart Traffic Light System Using Computer Vision

A lightweight, cost-effective adaptive traffic light control system that uses computer vision and IoT technologies to optimize traffic flow in real-time.

![System Architecture](https://img.shields.io/badge/Architecture-Computer%20Vision%20%2B%20IoT-blue)
![Platform](https://img.shields.io/badge/Platform-Raspberry%20Pi%20%2B%20ESP32-green)
![Language](https://img.shields.io/badge/Language-Python%20%2B%20Arduino-orange)
![License](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey)

## 🚀 Project Overview

This project presents an intelligent traffic light system that leverages **computer vision** and **cloud computing** to dynamically adjust signal timings based on real-time vehicle density. Unlike traditional fixed-time systems, our solution adapts instantly to traffic conditions, reducing congestion and improving urban mobility.

### Key Features
- **Real-time Traffic Monitoring**: Uses OpenCV for lightweight vehicle density estimation
- **Cloud Integration**: Firebase Realtime Database for seamless data synchronization
- **IoT Control**: ESP32 microcontroller for adaptive traffic light management
- **Cost-Effective**: Runs on low-cost hardware (Raspberry Pi + ESP32)
- **High Performance**: Achieves 15-20 FPS on Raspberry Pi, 100 FPS on laptop
- **Robust Operation**: Handles network failures with automatic fallback mode

## 🗃️ System Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   IP Camera     │───▶│  Processing Unit │───▶│    Firebase     │
│  (Video Input)  │    │ (Raspberry Pi/   │    │  (Cloud DB)     │
└─────────────────┘    │    Laptop)       │    └─────────────────┘
                       │ - OpenCV Vision  │              │
                       │ - Density Calc   │              │
                       └──────────────────┘              │
                                                         ▼
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│ Traffic Lights  │◀────│  ESP32 Control   │◀────│  Data Retrieval │
│   (LED/Real)    │    │ - Decision Logic │    │                 │
└─────────────────┘    │ - State Machine  │    └─────────────────┘
                       └──────────────────┘
```

## 🛠️ Hardware Components

### Core Components
| Component | Model | Function |
|-----------|-------|----------|
| Processing Unit | Raspberry Pi 4B (4GB) | Computer vision processing |
| Microcontroller | ESP32 | Traffic light control |
| Camera | USB/IP Camera (720p-1080p) | Video capture |
| LEDs | 5mm RGB LEDs | Traffic light simulation |
| Mounting | Adjustable Camera Arm | Stable positioning |

### Bill of Materials
- **Processing**: Raspberry Pi 4B + SD Card + Power Supply
- **Control**: ESP32 Development Board + Breadboard + Jumper Wires
- **Sensors**: USB Webcam or IP Camera
- **Output**: LEDs (Red, Yellow, Green) + 220Ω Resistors
- **Physical**: Intersection Base + Traffic Poles + Model Vehicles

**Total Cost per Intersection: < $100**

## 💻 Software Stack

### Computer Vision Pipeline (Python + OpenCV)
- **Image Preprocessing**: Grayscale conversion, Gaussian blur
- **Segmentation**: Binary thresholding, morphological operations
- **Density Estimation**: ROI-based pixel counting
- **Performance**: 15-20 FPS on Raspberry Pi (vs. 2 FPS for YOLO)

### Cloud Integration (Firebase)
- **Real-time Database**: Instant synchronization across devices
- **Scalability**: Support for multiple intersections
- **Reliability**: Automatic reconnection and error handling

### Microcontroller Logic (ESP32 + Arduino IDE)
- **Decision Algorithm**: Prioritizes lanes with highest traffic density
- **State Machine**: Safe transitions (Green → Yellow → Red)
- **Failsafe Mode**: Fixed-time cycle during network outages

## 🚦 How It Works

1. **Video Capture**: Camera monitors intersection from overhead position
2. **Image Processing**: OpenCV pipeline estimates vehicle density per lane
3. **Cloud Sync**: Density values uploaded to Firebase in real-time
4. **Decision Making**: ESP32 identifies busiest lane and assigns green priority
5. **Signal Control**: Traffic lights adapt based on actual traffic conditions

### Processing Pipeline
```
Raw Frame → Grayscale → Blur → Threshold → Morphology → ROI Analysis → Firebase → ESP32 → LEDs
```

## 📊 Performance Results

### Frame Rate Benchmarks
| Hardware | Without Firebase | With Firebase | YOLO Comparison |
|----------|------------------|---------------|-----------------|
| Laptop (i7, 16GB) | ~100 FPS | ~40 FPS | ~5 FPS |
| Raspberry Pi 4B | ~20 FPS | ~15 FPS | ~2 FPS |

### System Latency
- **End-to-end latency**: 150-300ms (image capture → signal change)
- **Processing time**: 10-30ms per frame
- **Firebase sync**: 50-150ms
- **ESP32 response**: <70ms

## 🔧 Installation & Setup

### 1. Clone Repository
```bash
git clone https://github.com/saidalbardawil/Smart-Traffic-light.git
cd Smart-Traffic-light
```

### 2. Install Python Dependencies
```bash
pip install opencv-python
pip install firebase-admin
pip install numpy
pip install cvzone
```

### 3. Firebase Configuration
1. Create Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
2. Download service account key JSON
3. Update Firebase credentials in Python script
4. Set database rules for read/write access

### 4. ESP32 Setup
1. Install Arduino IDE and ESP32 board support
2. Install required libraries:
   - WiFi
   - FirebaseESP32
3. Update WiFi credentials and Firebase config
4. Upload code to ESP32

### 5. Hardware Connections
```
ESP32 GPIO Connections:
Lane 0: Green(15), Yellow(2), Red(4)
Lane 1: Green(12), Yellow(14), Red(27)  
Lane 2: Green(25), Yellow(32), Red(33)
Lane 3: Green(5), Yellow(18), Red(19)
```

## 🎯 Usage

### Running the System
1. **Start Processing Unit**:
   ```bash
   python traffic_vision.py
   ```

2. **Power ESP32**: Upload firmware and connect to power

3. **Position Camera**: Mount overhead for clear intersection view

4. **Monitor Operation**: Check Firebase console for real-time data

### Testing with 3D Model
For development/testing without real traffic:
1. Use provided prototype models for simulation
2. Generate traffic simulation video
3. Feed video to processing pipeline

## 📈 Evaluation Results

### Traffic Scenarios Tested
- ✅ **Heavy Traffic**: System correctly prioritizes congested lanes
- ✅ **Balanced Traffic**: Fair random selection when densities are equal  
- ✅ **Pedestrian Interference**: Successfully ignores non-vehicle objects
- ✅ **Network Failure**: Automatic fallback to fixed-time operation
- ✅ **Dynamic Changes**: Real-time adaptation to changing conditions

### Performance Advantages
- **10x faster** than YOLO on Raspberry Pi
- **Real-time responsiveness** on low-cost hardware
- **No GPU required** unlike deep learning approaches
- **Scalable architecture** for city-wide deployment

## 🌟 Key Innovations

1. **Lightweight Computer Vision**: Pixel-density method instead of complex object detection
2. **Implicit Vehicle Weighting**: Larger vehicles (buses/trucks) naturally get higher priority
3. **Cloud-IoT Integration**: Real-time synchronization without expensive infrastructure
4. **Fail-Safe Operation**: Automatic fallback ensures continuous operation
5. **Cost-Effective Deployment**: Sub-$100 per intersection vs. $1000s for traditional systems

## 🔮 Future Enhancements

- [ ] **Multi-Intersection Coordination**: Network-wide traffic optimization
- [ ] **Advanced Camera Features**: Night vision, weather resistance
- [ ] **V2I Communication**: Integration with connected vehicles
- [ ] **Predictive Analytics**: ML-based traffic pattern analysis
- [ ] **Emergency Vehicle Priority**: Automatic preemption for ambulances/fire trucks

## 📚 Technical Documentation

### Project Structure
```
Smart-Traffic-light/
├── Computer_vision_code/    # OpenCV traffic density analysis
├── ESP32_controller_code/   # Microcontroller firmware
├── docs/                   # Documentation and specifications
├── Prototype/              # 3D models and simulation files
├── .gitattributes         # Git configuration
├── .gitignore            # Git ignore rules
├── LICENSE               # License file
└── README.md            # Project documentation
```

### Dependencies
- **Python**: OpenCV, NumPy, Firebase Admin SDK, cvzone
- **Arduino**: WiFi, FirebaseESP32, GPIO libraries
- **Hardware**: Raspberry Pi OS, Arduino IDE

## 🏆 Project Information

**Smart Traffic Light System - Mechatronics Engineering Project**

**Date**: June 2025

## 📄 License

🔹 **License**

This project is shared **for viewing and academic reference only**. It is licensed under the **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)**. You may read and cite this work, but you may not modify or use it commercially.

[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

### What this means:
- ✅ **Attribution**: You must give appropriate credit and cite this work
- ✅ **Academic Use**: Free to reference in research and educational contexts  
- ❌ **No Commercial Use**: Cannot be used for commercial purposes
- ❌ **No Derivatives**: Cannot modify, transform, or build upon this work

For commercial licensing or permission to modify, please contact the authors.

## 🤝 Contributing

**Note**: This project is licensed under CC BY-NC-ND 4.0, which means modifications and derivatives are not permitted. However, you can:

- ⭐ Star the repository to show support
- 📖 Use it as a reference for your own implementations
- 📚 Cite it in academic work and research
- 💬 Discuss ideas and improvements in Issues (for academic discussion)
- 📧 Contact authors for collaboration opportunities

### Academic Citation
If you use this work in your research, please cite:
```
Smart Traffic Light System Using Computer Vision. (2025). 
Mechatronics Engineering Project.
```

## 📞 Contact

For questions, suggestions, or collaboration opportunities:

- **Project Repository**: [GitHub](https://github.com/saidalbardawil/Smart-Traffic-light)
- **Documentation**: See `/docs` folder for detailed technical specifications

---

**⭐ Star this repository if you found it helpful!**

**🔥 This project demonstrates that intelligent traffic management can be achieved with low-cost hardware and lightweight algorithms, making it ideal for developing urban environments with budget constraints.**