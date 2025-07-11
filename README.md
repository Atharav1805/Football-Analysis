# Football Analysis

[![Build Status](https://img.shields.io/github/actions/workflow/status/Atharav1805/Football-Analysis/ci.yml?branch=main)](https://github.com/Atharav1805/Football-Analysis/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python Version](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![Coverage Status](https://img.shields.io/codecov/c/github/Atharav1805/Football-Analysis?branch=main)](https://codecov.io/gh/Atharav1805/Football-Analysis)

---

## 📖 Overview

**Football Analysis** is a production-grade computer vision and analytics framework for soccer match video processing. Designed with scalability, modularity, and performance in mind, it empowers teams and analysts to extract tactical insights, player metrics, and event statistics from raw footage.

## 🌐 Table of Contents

* [🎨 Architecture](#architecture)
* [🚀 Features](#features)
* [🛠️ Installation](#installation)
* [⚙️ Configuration](#configuration)
* [🚦 Usage](#usage)
* [📂 Project Structure](#project-structure)
* [🧩 Module Design](#module-design)
* [🧪 Testing & CI/CD](#testing--cicd)
* [🐳 Docker Support](#docker-support)
* [🤝 Contributing](#contributing)
* [📜 License](#license)

---

## 🎨 Architecture

A high-level overview of the pipeline:

![Architecture Diagram](assets/architecture.png)

1. **Input Ingestion**: Video reader abstracts frame acquisition.
2. **Detection Module**: YOLOv8 model identifies players and ball.
3. **Tracking Module**: Multi-object tracker (Kalman Filter + Hungarian Algorithm).
4. **Stabilization**: Compensates for camera motion using feature matching.
5. **View Transformation**: Applies homography for top-down projection.
6. **Analytics Engine**: Calculates speed, distance, possession, and events.
7. **Visualization & Export**: Renders overlays and writes annotated video.

---

## 🚀 Features

* **Real-Time Capable**: Processes 1080p video at 30 FPS (GPU accelerated).
* **Modular Design**: Swap detectors, trackers, or transformers with minimal code changes.
* **Configuration-Driven**: YAML-based configs for model paths, tracker settings, and thresholds.
* **Comprehensive Analytics**: Player heatmaps, pass network graphs, speed/distance stats.
* **Extensible Output**: JSON and CSV exports of metrics, plus annotated video.

---

## 🛠️ Installation

1. **Clone & Checkout**

   ```bash
   git clone https://github.com/Atharav1805/Football-Analysis.git
   cd Football-Analysis
   git checkout main
   ```

2. **Environment**

   ```bash
   python -m venv venv
   source venv/bin/activate
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

3. **Pre-trained Models**
   Place your YOLO `.pt` files in `models/` or download via:

   ```bash
   mkdir -p models && \
   wget -P models/ https://example.com/yolov8_football.pt
   ```

---

## ⚙️ Configuration

All parameters are defined in `config/config.yaml`:

```yaml
model:
  path: models/yolov8_football.pt
  conf_threshold: 0.25

tracker:
  max_lost: 30
  iou_threshold: 0.3

camera:
  feature_detector: ORB
  max_features: 500

transform:
  src_points: [[x1,y1],...]
  dst_points: [[X1,Y1],...]
```

Adjust thresholds and paths as needed.

---

## 🚦 Usage

### Full Pipeline

```bash
python main.py --config config/config.yaml --input input_videos/match.mp4 --output output_videos/annotated.mp4
```

### Modular Execution

* **Detection Only**:

  ```bash
  python main.py --stage detect --input path/to/video
  ```
* **Tracking Only**:

  ```bash
  python main.py --stage track --input path/to/detections.json
  ```

Additional commands: `--stage stabilize`, `--stage transform`, `--stage analyze`, `--stage visualize`.

---

## 📂 Project Structure

```
├── assets/
│   ├── demo.gif
│   └── architecture.png
├── config/
│   └── config.yaml
├── camera_movement_estimator/
├── detection/
│   └── yolov8_inference.py
├── tracking/
│   └── tracker.py
├── transform/
│   └── view_transformer.py
├── analysis/
│   ├── speed_distance.py
│   └── possession.py
├── visualization/
│   └── overlay_renderer.py
├── tests/
│   └── test_*.py
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── main.py
├── ci.yml
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
└── LICENSE
```

---

## 🧩 Module Design

| Module                       | Responsibility                                   |
| ---------------------------- | ------------------------------------------------ |
| `detection/`                 | YOLO-based object detection                      |
| `tracking/`                  | Data association and trajectory smoothing        |
| `camera_movement_estimator/` | Estimate camera motion using feature matching    |
| `transform/`                 | Homography computation and top-down view warping |
| `analysis/`                  | Speed, distance, possession, and event analytics |
| `visualization/`             | Video annotation and heatmap generation          |

---

## 🧪 Testing & CI/CD

* **Unit Tests**: Located in `tests/`, run with:

  ```bash
  pytest --cov=.
  ```
* **Continuous Integration**: GitHub Actions (`.github/workflows/ci.yml`) runs tests, linting, and coverage on push.
* **Code Coverage**: Reports published to Codecov.

---

## 🐳 Docker Support

Build and run via Docker:

```bash
# Build image
docker build -t football-analysis:latest .

# Run container
docker run --rm -v $(pwd)/input_videos:/app/input_videos \
  -v $(pwd)/output_videos:/app/output_videos football-analysis:latest \
  python main.py --config config/config.yaml --input input_videos/match.mp4 --output output_videos/annotated.mp4
```

Or with `docker-compose up --build`.

---

## 🤝 Contributing

We welcome contributions! Please:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/YourFeature`.
3. Commit your changes: `git commit -m "Add YourFeature"`.
4. Push to the branch: `git push origin feature/YourFeature`.
5. Open a Pull Request and reference relevant issues.


---

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
