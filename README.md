# 🔒 Face Locking System

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-green)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Face%20Mesh-orange)
![License](https://img.shields.io/badge/License-MIT-yellow)

A high-performance **Real-time Face Recognition and Locking System** designed to identify enrolled individuals, track their movements, and log their actions with precision. Built with ArcFace for state-of-the-art recognition accuracy and MediaPipe for robust landmark tracking.

## 🌟 Core Features

- **🎯 Real-Time Face Recognition**: Identify multiple individuals simultaneously with high accuracy using ArcFace embeddings.
- **🔐 Face Locking**: Securely "lock" onto a specific target to track them exclusively, ignoring other faces.
- **👀 Action Monitoring**:
    - **Head Tracking**: Detects and logs left/right head movements.
    - **Expression Analysis**: Real-time smile detection.
    - **Activity Logging**: Timestamps every event (lock, unlock, movement, smile) to a persistent log file.
- **⚡ High Performance**: Optimized for CPU inference, suitable for laptops and edge devices.
- **📊 Analytics**: Built-in tools for evaluating model thresholds and visualizing embeddings.

## 📂 Project Structure

```
face-locking/
├── models/                  # Required model files (ArcFace ONNX, FaceMesh task)
├── src/
│   ├── align.py             # Face alignment using 5-point landmarks
│   ├── camera.py            # Webcam stream handling
│   ├── detect.py            # Face detection logic
│   ├── embed.py             # Feature extraction (ArcFace)
│   ├── enroll.py            # CLI tool for registering new users
│   ├── evaluate.py          # Script to find optimal recognition thresholds
│   ├── haar_5pt.py          # Visualization demo for landmarks
│   ├── landmarks.py         # MediaPipe FaceMesh wrapper
│   └── recognize.py         # Main application: Recognition & Locking
├── logs/                    # Session logs saved here
├── requirements.txt         # Python dependencies
└── README.md                # Project documentation
```

## 🚀 Getting Started

### Prerequisites

- **Python 3.8** or higher
- **Webcam** (Built-in or USB)

### Installation

1.  **Clone the repository** (if you haven't already):
    ```bash
    git clone https://github.com/leandre000/face-locking.git
    cd face-locking
    ```

2.  **Install dependencies**:
    It is recommended to use a virtual environment.
    ```bash
    pip install -r requirements.txt
    ```

3.  **Download Models**:
    Ensure the following files are present in the `models/` directory:
    - `embedder_arcface.onnx`
    - `face_landmarker.task`

## 📖 Usage

### 1. Enroll New Users
Before the system can recognize anyone, you must enroll them.

```bash
python -m src.enroll
```

**Controls:**
- `SPACE`: Capture a photo of the face.
- `a`: Toggle **Auto-Capture** mode (rapidly captures frames).
- `s`: **Save** the enrolled profile and exit.
- `q`: Quit without saving.

### 2. Start Recognition & Locking
Run the main application to start detecting and tracking faces.

```bash
python -m src.recognize
```

**System Controls:**
- `l`: **Lock/Unlock** the currently detected face (Targeting Mode).
- `+/-`: Increase/Decrease the recognition distance threshold.
- `d`: Toggle **Debug Overlay** (shows landmarks and bounding boxes).
- `r`: Reload the face database from disk.
- `q`: Quit the application.

**🔒 Face Locking Mode:**
When you lock onto a face (press `l`), the system will:
1.  Draw an **Orange** bounding box around the target.
2.  Ignore all other faces.
3.  Log specific actions (Head Turn Left/Right, Smiling) to a file in the `logs/` folder.

### 3. Evaluation & Optimization
To calculate the best distance threshold for your specific lighting and camera setup:

```bash
python -m src.evaluate
```

### 4. Visual Demos
Explore the underlying technology with these visualization scripts:

- **View 5-Point Landmarks**:
  ```bash
  python -m src.haar_5pt
  ```
- **Embedding Heatmap**:
  ```bash
  python -m src.embed
  ```

## 📝 Logs

All tracking sessions are logged in the `logs/` directory with filenames in the format:
`[Name]_history_[YYYYMMDDHHMMSS].txt`

**Sample Log Entry:**
```text
2026-02-01 14:30:15.123 - FACE_LOCKED: Target acquired: Alex
2026-02-01 14:30:22.456 - HEAD_RIGHT: User turned head right (35px)
2026-02-01 14:30:25.789 - SMILE: Expression detected: Smile
```

## 🛠️ Troubleshooting

- **"No module named 'src'"**:
  Make sure you are running the commands from the root `face-locking` directory using `python -m src.script_name`.

- **Camera not opening**:
  Check if another application is using the webcam. You can change the camera index in `src/camera.py` (default is `0`).

- **Low Accuracy**:
  - Ensure good lighting during enrollment.
  - Enroll multiple angles of the face.
  - Run `src.evaluate` to tune the threshold.

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
