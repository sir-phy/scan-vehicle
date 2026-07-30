# Scan Vehicle - YOLOv8 Vehicle Tracking & Counting

A real-time vehicle detection, tracking, and counting application using YOLOv8 and OpenCV. This application monitors video streams (webcam or video files) and counts vehicles crossing a defined line.

## Features

- **Real-time Object Detection**: Uses YOLOv8 for accurate vehicle detection
- **Object Tracking**: Persistent tracking IDs for each detected object
- **Line Crossing Counter**: Counts vehicles crossing a defined line
- **Multiple Vehicle Classes**: Detects cars, motorcycles, bicycles, tuk-tuks, and more
- **Visual Overlays**: Bounding boxes, track IDs, and live count display
- **Video Recording**: Optional saving of annotated video output

## Installation

### Prerequisites

- Python 3.8 or higher
- Webcam or video file for input
- (Optional) NVIDIA GPU with CUDA for faster inference

### Setup

1. Clone the repository:
```bash
git clone https://github.com/sir-phy/scan-vehicle.git
cd scan-vehicle
```

2. Create a virtual environment (recommended):
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

Or install manually:
```bash
pip install opencv-python ultralytics
```

## Usage

### Basic Usage

Run with default webcam (camera 0):
```bash
python src/run_track_count.py --source 0
```

### Command Line Arguments

| Argument | Description | Default |
|----------|-------------|---------|
| `--source` | Camera index or video path | `0` (webcam) |
| `--weights` | Path to custom YOLO weights | `yolov8n.pt` |
| `--conf` | Confidence threshold | `0.25` |
| `--iou` | NMS IoU threshold | `0.45` |
| `--imgsz` | Inference image size | `640` |
| `--save` | Save annotated video to outputs/ | `False` |

### Examples

Use a specific camera:
```bash
python src/run_track_count.py --source 1
```

Use a video file:
```bash
python src/run_track_count.py --source path/to/video.mp4
```

Save output video:
```bash
python src/run_track_count.py --source 0 --save
```

Use custom model with higher confidence:
```bash
python src/run_track_count.py --source 0 --weights yolov8x.pt --conf 0.5
```

## Configuration

The application can be configured using a `config.json` file in the root directory. Create one with the following structure:

```json
{
  "model": "yolov8n.pt",
  "conf": 0.25,
  "iou": 0.45,
  "imgsz": 640,
  "classes": ["car", "motorcycle", "bicycle", "tuk-tuk"],
  "line": [100, 360, 1180, 360]
}
```

### Configuration Options

- **model**: Path to YOLO model weights
- **conf**: Confidence threshold for detections (0-1)
- **iou**: IoU threshold for Non-Maximum Suppression (0-1)
- **imgsz**: Image size for inference (higher = more accurate but slower)
- **classes**: List of vehicle classes to detect and count
- **line**: Counting line coordinates [x1, y1, x2, y2]

### Supported Vehicle Classes

The default COCO model can detect:
- `person`
- `bicycle`
- `car`
- `motorcycle`
- `bus`
- `truck`
- `tuk-tuk` (if using custom model)

## How It Works

1. **Detection**: YOLOv8 detects objects in each frame
2. **Tracking**: Objects are assigned persistent track IDs
3. **Filtering**: Only specified vehicle classes are counted
4. **Line Crossing**: The algorithm detects when objects cross the counting line
5. **Counting**: Each object is counted only once when crossing the line
6. **Display**: Results are shown with bounding boxes, IDs, and counts

### Counting Logic

The application uses a line-crossing algorithm:
- Tracks which side of the line each object is on
- Counts an object when it crosses from one side to the other
- Each object is counted only once per session

## Project Structure

```
scan-vehicle/
├── src/
│   └── run_track_count.py    # Main application script
├── venv/                      # Virtual environment (ignored in git)
├── yolov8n.pt                # YOLOv8 nano model weights
├── config.json               # Configuration file (optional)
├── .gitignore                # Git ignore rules
└── README.md                 # This file
```

## Output

### Console Output
- Real-time FPS and inference time
- Detection statistics per frame
- Save path when recording video

### Video Output
- Bounding boxes around detected vehicles
- Track IDs for each object
- Green counting line
- Live count display in top-left corner
- Class labels with confidence

## Troubleshooting

### Font Warning
If you see warnings about Qt fonts:
```bash
# Install system fonts
sudo apt-get install fonts-dejavu-core
```

### Low FPS
- Use a smaller model (yolov8n.pt instead of yolov8x.pt)
- Reduce image size: `--imgsz 320`
- Use GPU acceleration if available

### No Detections
- Adjust confidence threshold: `--conf 0.3`
- Check lighting conditions
- Ensure camera is working properly

## Requirements

```
opencv-python>=4.5.0
ultralytics>=8.0.0
numpy>=1.19.0
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Acknowledgments

- [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics) for the object detection model
- [OpenCV](https://opencv.org/) for computer vision utilities

## Contact

For issues or questions, please open an issue on GitHub.