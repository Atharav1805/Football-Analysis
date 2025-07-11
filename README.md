# Football Analysis Project

![Project Demo](assets/demo.gif)

## Overview

The **Football Analysis Project** is a comprehensive computer vision pipeline designed to analyze soccer match videos. The pipeline performs:

* **Player & Ball Detection**: Utilizing YOLO-based models to detect players and the ball in each frame.
* **Object Tracking**: Tracking player movements across frames with custom tracker and Kalman filters.
* **Camera Movement Compensation**: Estimating and compensating for camera panning/tilt to maintain consistent world coordinates.
* **Top-Down View Transformation**: Warping frames to a bird’s-eye view for easier tactical analysis.
* **Speed & Distance Estimation**: Calculating player and ball speeds, as well as distances covered.
* **Team Assignment**: Coloring players based on team jerseys and maintaining consistent team identity over time.
* **Ball Possession Assignment**: Determining which player controls the ball frame-by-frame.
* **Annotated Video Output**: Generating videos with overlays for speed, distance, team colors, and ball possession.

## Features

* End-to-end Python pipeline for football match analysis.
* Modular design: Easily swap detection/tracking models or add new analysis modules.
* Stub support for debugging and rapid prototyping (`stubs/` folder).
* Output of annotated video with customizable overlays.

## Repository Structure

```
├── camera_movement_estimator/      # Estimate and compensate camera motion
├── input_videos/                   # Sample input match videos
├── models/                         # Trained YOLO detection models (e.g., best.pt)
├── output_videos/                  # Generated annotated videos
├── player_ball_assigner/           # Assigns ball possession to players
├── speed_and_distance_estimator/   # Compute speed & distance metrics
├── team_assigner/                  # Determine player team based on jersey
├── trackers/                       # Tracking algorithms & Kalman filters
├── training/                       # Scripts & notebooks for model training
├── utils/                          # Video I/O, common utilities
├── view_transformer/               # Warping frames to top-down view
├── main.py                         # Orchestrates the entire pipeline
├── yolo_inference.py               # Standalone YOLO inference demo
└── README.md                       # Project overview and instructions
```

## Getting Started

### Prerequisites

* Python 3.8+
* [Ultralytics YOLO](https://github.com/ultralytics/ultralytics)
* OpenCV
* NumPy

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/Atharav1805/Football-Analysis.git
   cd Football-Analysis
   ```

2. Create a virtual environment and install dependencies:

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\\Scripts\\activate
   pip install -r requirements.txt
   ```

3. Download or place your match video(s) in `input_videos/` and your YOLO model (`.pt`) in `models/`.

## Usage

**Run the full pipeline:**

```bash
python main.py
```

**Run YOLO inference only:**

```bash
python yolo_inference.py
```

Results will be saved to `output_videos/output_video.avi` by default.

## Customization

* **Swap Detection Model**: Replace `models/best.pt` with your own YOLO model.
* **Adjust Tracking Parameters**: Modify settings in `trackers/Tracker.py`.
* **Customize Overlays**: Edit drawing functions in each module (e.g., speed overlays in `speed_and_distance_estimator`).

## Contact

For questions or contributions, reach out to:

> Atharav Sonawane - [atharav.sonawane@iitb.ac.in](mailto:atharav.sonawane@iitb.ac.in)

Feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
