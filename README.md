<div align="center">

# 🐇🔫 Real Rabbit Shooting: Hand-Gesture Controlled Game 🐇🔫

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-00A67E?style=for-the-badge&logo=google&logoColor=white)
![Pygame](https://img.shields.io/badge/Pygame-F80000?style=for-the-badge&logo=python&logoColor=white)

*An interactive arcade game completely driven by real-time Computer Vision and Hand Landmark Mathematics.* 🎮✨

---

### 🎥 Demo Gameplay
*(Click on the image below to watch the full gameplay video on YouTube)*

[![Demo Real Rabbit Shooting](https://img.youtube.com/vi/OVPGpsSBKdA/maxresdefault.jpg)](https://www.youtube.com/watch?v=OVPGpsSBKdA "Click to Watch Gameplay!")

---

## 🧬 1. Technical Architecture & Mathematical Foundations

This project does not use a traditional keyboard or mouse. Instead, it decodes a real-time video stream to map hand geometric models into game control signals.

### 📐 Hand Landmark Extraction
The system (utilizing MediaPipe) scans the frame and predicts **21 landmarks** in 3D space $(x, y, z)$ on the hand. Each point $L_i$ has normalized coordinates ranging from $0.0$ to $1.0$, corresponding to the width and height of the video frame.

### 📏 Finger State Heuristics
To determine whether a finger is "Folded" or "Extended", the algorithm calculates the Euclidean distance between the finger joints.
**Technical Definition:** *Euclidean Distance* is the length of the line segment connecting two points in space. The mathematical formula to measure the distance between the fingertip ($P_1(x_1, y_1)$) and the metacarpophalangeal joint (MCP, $P_2(x_2, y_2)$) is:
$$D = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

**Logical Flow:** The algorithm compares the vertical coordinate ($y$-coordinate) of the fingertip (e.g., landmark 8 for the index finger) with the vertical coordinate of the PIP joint (landmark 6). In the image coordinate system (where the y-axis points downwards):
- If $y_{tip} < y_{PIP}$: The finger is Extended.
- If $y_{tip} > y_{PIP}$: The finger is Folded.

---

## 🕹️ 2. Gesture-to-Action Mapping

The control system is specifically designed for the **Right Hand**. Face your palm towards the camera and perform the following combinations:

| ✊ Hand Gesture | 🧮 Logic State | 🏃 Rabbit Action |
| :--- | :--- | :--- |
| **Right Fist** | All fingers $y_{tip} > y_{PIP}$ | Do nothing 🛑 |
| **Thumb out** | Only thumb satisfies extended condition | Jumping ⬆️ |
| **Index out** | Only index extended | Go left ⬅️ |
| **Middle out** | Only middle extended | Go right ➡️ |
| **Pinky out** | Only pinky extended | Shooting 🔫💥 |

🔥 **Advanced Feature:** The state matrix system enables multi-threading logic. You can entirely combine fingers (e.g., extend index + pinky fingers) so the Rabbit can simultaneously run left and shoot continuously!

---

## 🚀 3. Step-by-Step Local Deployment Guide

Below is the hands-on workflow to execute the project locally:

**Step 1: Clone the Repository**
Open your Terminal/Command Prompt and run:
```bash
git clone [https://github.com/phithuong-ng/real_rabbit_shooting.git](https://github.com/phithuong-ng/real_rabbit_shooting.git)
cd real_rabbit_shooting
```

**Step 2: Environment Setup**
Ensure you have Python 3.x installed. It is highly recommended to create a virtual environment to avoid library conflicts:
```bash
# Install core libraries for image processing and game engine
pip install opencv-python mediapipe pygame
```

**Step 3: Directory Configuration**
*Technical Note:* You must open the source files and reconfigure the absolute/relative paths pointing to the `audio`, `images`, and `data` directories to perfectly match your local machine's directory structure.

**Step 4: Game Execution**

The system supports 2 distinct modes:
- 🎮 **Computer Vision Mode (Hand Gestures):** Execute the main file to initialize the camera pixel scanning.
  
```bash
  python main.py
  ```
- ⌨️ **Traditional Mode (Keyboard):** Execute the testing file to control the Rabbit using basic keyboard inputs.
  ```bash
  python testing2.py
  ```

*Enjoy your Edge-Vision technology experience!* 🌟
