# Vision-Based Advanced Driver Assistance System (ADAS)



## Project Overview
This repository contains the code for a vision-only Advanced Driver Assistance System (ADAS) perception pipeline. The goal of this project was to integrate classical computer vision techniques with modern deep learning to process continuous front-facing driving video in real time. 

Rather than focusing solely on model training, the primary emphasis here is on system design. I wanted to bring together perception, spatial reasoning, and rule-based safety logic into a single, unified output stream, simulating how early-stage ADAS prototypes operate in the autonomous driving industry.

## Project Demo and Materials
You can view the final annotated video output and access the raw project files on Google Drive:
**https://drive.google.com/drive/u/0/folders/1xexMiF4eRC2M7D-ciLze-qFoQm5imedb**

## System Capabilities
I designed the pipeline to handle multiple perception and logic tasks simultaneously on every video frame:

* **Lane Detection and Tracking:** The system uses a classical image processing approach—converting frames to grayscale, applying Gaussian blur, Canny edge detection, and Hough Line transforms—to find the left and right lane boundaries. From there, it calculates the drivable polygon area and continuously tracks the geometric center of the lane.
* **Real-Time Vehicle Detection:** I integrated a pretrained YOLOv8 model to identify cars, trucks, buses, and motorcycles, overlaying accurate bounding boxes and confidence scores.
* **Spatial Reasoning (ROI):** To filter out irrelevant background noise and focus computation where it matters, the system applies a dynamic, trapezoidal Region of Interest (ROI) mask. It evaluates only the vehicles that enter this immediate road-facing area.
* **Traffic Counting:** As unique vehicles cross into the masked ROI, the system tracks their spatial entry points and keeps a running traffic count.
* **Forward Collision Risk Estimation:** The system evaluates potential collisions by analyzing the relative position and area of vehicle bounding boxes within the ROI. If a vehicle gets too close or grows too large in the frame, it triggers real-time warning or critical alerts.
* **Lane Departure Warning:** By continuously monitoring the camera's lateral position against the estimated lane center, the system calculates deviation thresholds and issues directional drift warnings (e.g., "DRIFTING RIGHT") if the vehicle crosses a safety boundary.
* **Unified Visualization:** All of these outputs—lane overlays, bounding boxes, ROI polygons, vehicle counts, and safety alerts—are drawn together onto a single comprehensive video stream.

## Tech Stack
* **Language:** Python
* **Computer Vision:** OpenCV (cv2)
* **Object Detection:** Ultralytics YOLOv8
* **Data Processing:** NumPy

## How It Works
The pipeline processes the input video frame-by-frame through a structured methodology:
1. A frame is extracted from the continuous video feed.
2. A fixed trapezoidal mask is generated to isolate the immediate driving path.
3. The masked frame undergoes edge detection and line transformation to locate lane markers and compute their slopes.
4. YOLOv8 runs inference on the frame to detect surrounding vehicles.
5. Rule-based logic evaluates the bounding box sizes and positions for collision risks, and checks the calculated lane center for vehicle drift.
6. Visual elements and text warnings are annotated onto the frame, which is then compiled into a final output video file.

## How to Run

1. Clone this repository to your local machine.
2. Install the necessary dependencies:
   ```bash
   pip install ultralytics opencv-python numpy
   ```
3. Open the Jupyter Notebook or Python script and update the video path to point to your input dashcam footage:
   ```python
   video_path = "path/to/your/video.mp4"
   ```
4. Execute the code to process the video and generate the final annotated output.

## Disclaimer
This project is an educational prototype built to explore computer vision and spatial logic within autonomous systems. It relies on deterministic, rule-based logic and is not intended for use as a production-ready safety system in real driving scenarios.
