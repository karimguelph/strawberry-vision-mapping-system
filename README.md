# Strawberry Vision Mapping System

A lightweight computer vision pipeline for detecting strawberry flowers using custom YOLOv8 model and mapping their real-world positions using ArUCo markers. The system merges multiple images into one unified global coordinate map for use in robotics, navigation, and autonomous pollination.

---

## Overview

This project performs:

- Strawberry flower detection using a custom YOLOv8 model  
- ArUCo marker detection for coordinate scaling  
- Pixel → millimeter conversion using optical geometry  
- Global alignment of detections across multiple images  
- World-map generation with flower positions and navigation paths  
- Export of JSON + annotated images  

---

## Installation

Install dependencies with:

    pip install -r requirements.txt

Or manually:

    pip install ultralytics opencv-python numpy

---

## How to Run

1. Place images in the `images/` folder.  
2. Ensure the YOLO model weights are located at:  
   `strawberry_flower_detection_model/weights/best.pt`  
3. Run the script:

    python strawberry_flower_mapping.py

---

## Outputs

The script generates:

- Annotated images showing flower detections and ArUCo tag positions (saved in `multi_results/`)  
- Global merged world map: `merged_world_map.jpg`  
- Coordinate dataset (tags + flowers): `merged_world_map.json`  

## Dataset

This project uses a custom strawberry flower dataset built from two sources:

### **1. VM-YOLO Strawberry Flower Dataset (Public)**
A portion of the images was sourced from the dataset provided in the paper:

**Wang, Y., Lin, X., Xiang, Z., & Su, W. (2025).  
VM-YOLO: YOLO with VMamba for Strawberry Flowers Detection.  
*Plants*, 14(3), 468. https://doi.org/10.3390/plants14030468**

### **2. Custom Additions (Private / Project-Collected)**
In addition to the public dataset, we collected and added **extra greenhouse and lab-captured images** to improve detection performance under:

- Variable lighting  
- Different flower orientations  
- Cluttered leaves and stems  
- Partial occlusions  

### **Manual Annotation**
All images (public + custom) were **manually annotated** specifically for this project.

The dataset includes **four custom labeling classes**:

- **bud** – early developmental stage  
- **early** – partially opened flowers  
- **full** – fully opened flowers (used for mapping)  
- **late** – wilted or end-stage flowers  

These labels were used to fine-tune the YOLOv8 model for improved accuracy in greenhouse environments.

### **Download**
The full training dataset used for this project is available here:

**Dataset:**  
https://drive.google.com/drive/folders/1gE0Akrm50D1JbFWZqA7MGdfBDtWcWN4o?usp=sharing


## Description

- ArUCo tags provide metric scaling based on their known physical size.  
- Detected `"full"` strawberry flowers are converted from pixel coordinates to millimeters.  
- Shared ArUCo tag IDs allow merging detections from multiple images into a single global coordinate frame.  
- A navigation-style path is drawn from Tag 0 (origin) to each detected flower in both local and global views.  
- Close flowers in the global map are clustered to avoid overlapping labels when visualized.

