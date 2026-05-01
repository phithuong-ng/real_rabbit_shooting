<div align="center">

# 🐇🔫 Real Rabbit Shooting: Hand-Gesture Controlled Game 🐇🔫

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-00A67E?style=for-the-badge&logo=google&logoColor=white)
![Pygame](https://img.shields.io/badge/Pygame-F80000?style=for-the-badge&logo=python&logoColor=white)

*An interactive arcade game completely driven by real-time Computer Vision and Hand Landmark Mathematics.* 🎮✨

---

## 🧬 1. Kiến trúc Kỹ thuật & Cơ sở Toán học (Technical & Mathematical Foundations)

Dự án này không sử dụng bàn phím hay chuột truyền thống. Thay vào đó, nó giải mã luồng video trực tiếp (real-time video stream) để ánh xạ các mô hình hình học của bàn tay thành các tín hiệu điều khiển trong game.

### 📐 Trích xuất Tọa độ Không gian (Hand Landmark Extraction)
Hệ thống (thường sử dụng MediaPipe) sẽ quét khung hình và dự đoán **21 điểm mốc (landmarks)** trên không gian 3 chiều $(x, y, z)$ của bàn tay. Mỗi điểm $L_i$ có tọa độ được chuẩn hóa (normalized) từ $0.0$ đến $1.0$ tương ứng với chiều rộng và chiều cao của khung hình video.

### 📏 Thuật toán Nhận diện Trạng thái Ngón tay (Finger State Heuristics)
Để xác định một ngón tay đang "gập" (Folded) hay "mở" (Out/Extended), thuật toán tính toán khoảng cách Euclidean giữa các khớp ngón tay.
**Giải thích thuật ngữ:** *Khoảng cách Euclidean* là độ dài đoạn thẳng nối 2 điểm trong không gian. Công thức toán học đo khoảng cách giữa đầu ngón tay (Tip) $P_1(x_1, y_1)$ và khớp gốc ngón tay (MCP) $P_2(x_2, y_2)$ là:
$$D = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

**Quy trình logic:** Thuật toán sẽ so sánh tọa độ dọc ($y$-coordinate) của đầu ngón tay (Ví dụ: landmark 8 cho ngón trỏ) với tọa độ dọc của khớp PIP (landmark 6). Trong hệ tọa độ hình ảnh (trục y hướng xuống dưới):
- Nếu $y_{tip} < y_{PIP}$: Ngón tay đang duỗi thẳng (Extended).
- Nếu $y_{tip} > y_{PIP}$: Ngón tay đang gập lại (Folded).

---

## 🕹️ 2. Hệ thống Khớp nối Cử chỉ (Gesture-to-Action Mapping)

Hệ thống điều khiển được thiết kế riêng cho **Bàn tay phải (Right Hand)**. Hãy hướng lòng bàn tay của bạn về phía Camera và thực hiện các tổ hợp sau:

| ✊ Hình thái tay (Gesture) | 🧮 Trạng thái Logic | 🏃 Hành động của Thỏ (Action) |
| :--- | :--- | :--- |
| **Nắm đấm (Right Fist)** | Mọi ngón tay $y_{tip} > y_{PIP}$ | Không làm gì (Do nothing) 🛑 |
| **Mở Ngón cái (Thumb out)** | Chỉ ngón cái thỏa mãn điều kiện mở | Nhảy lên (Jumping) ⬆️ |
| **Mở Ngón trỏ (Index out)** | Chỉ ngón trỏ mở | Di chuyển sang Trái (Go left) ⬅️ |
| **Mở Ngón giữa (Middle out)** | Chỉ ngón giữa mở | Di chuyển sang Phải (Go right) ➡️ |
| **Mở Ngón út (Pinky out)** | Chỉ ngón út mở | Bắn đạn (Shooting) 🔫💥 |

🔥 **Tính năng nâng cao:** Hệ thống ma trận trạng thái cho phép xử lý đa luồng (Multi-threading logic). Bạn hoàn toàn có thể kết hợp các ngón tay (ví dụ: mở ngón trỏ + ngón út) để Thỏ vừa chạy sang trái vừa bắn liên thanh!

---

## 🚀 3. Hướng dẫn Triển khai Cục bộ (Step-by-Step Setup Guide)

Dưới đây là quy trình thao tác trực tiếp để khởi chạy dự án:

**Bước 1: Sao chép Mã nguồn (Clone the code)**
Mở Terminal/Command Prompt và gõ:
```bash
git clone https://github.com/phithuong-ng/real_rabbit_shooting.git
cd real_rabbit_shooting
```

**Bước 2: Chuẩn bị Môi trường (Environment Setup)**
Hãy đảm bảo bạn đã cài đặt Python 3.x. Khuyến nghị tạo môi trường ảo (virtual environment) để tránh xung đột thư viện:
```bash
# Cài đặt các thư viện lõi phục vụ xử lý hình ảnh và game engine
pip install opencv-python mediapipe pygame
```

**Bước 3: Tinh chỉnh Đường dẫn (Directory Configuration)**
*Lưu ý kỹ thuật:* Bạn cần mở các file mã nguồn và cấu hình lại các đường dẫn tuyệt đối/tương đối trỏ đến thư mục `audio`, `images`, và `data` sao cho khớp với cấu trúc thư mục trên máy tính cục bộ của bạn.

**Bước 4: Khởi chạy Trò chơi (Execution)**

Hệ thống hỗ trợ 2 chế độ (Modes) phân biệt:
- 🎮 **Chế độ Kỹ năng Thị giác Máy tính (Hand Gestures):** Khởi chạy file main để camera bắt đầu quét điểm ảnh.
  ```bash
  python main.py
  ```
- ⌨️ **Chế độ Truyền thống (Keyboard mode):** Khởi chạy file testing để điều khiển Thỏ bằng bàn phím cơ bản.
  ```bash
  python testing2.py
  ```

*Chúc bạn trải nghiệm công nghệ Edge-Vision vui vẻ!* 🌟
