# Drone Detection using YOLOv8

## Project Overview
This project uses object detection to find DJI Tello drones in images. We fine-tuned a pretrained YOLOv8 model on a labeled drone dataset, so the model can draw a bounding box around each drone and give a confidence score.

This project was built as part of the *Computer Vision for Developers* program at *SDAIA Academy*.

## Problem Description
Small drones are becoming very common, and they can fly over places where they are not allowed, such as airports, government buildings and private areas. Watching camera feeds manually is slow and easy to miss.

A general object detection model (trained on COCO) does not know what a drone is. In our test, the pretrained model detected the drone but labeled it as "bench", because "drone" is not one of the COCO classes it was trained on.In general, it cannot recognize drones correctly without extra training.

## Dataset & Model Used

*Dataset*
- *Name:* DJI Tello Drone Object Detection (Kaggle, by luispreciado99)
- *Source:* Created on Roboflow, license CC BY 4.0
- *Format:* YOLO format (each image has a .txt label file)
- *Classes:* 1 class: tello drone quadcopter
- *Split:*

| Split | Images |
|-------|--------|
| Train | 602 |
| Valid | 129 |
| Test  | 129 |

*Model*
- *YOLOv8s* (small version) from Ultralytics
- Pretrained on the COCO dataset, then fine-tuned on our drone images (transfer learning)

## Workflow / Architecture


Download dataset (Kaggle) → Read data.yaml → Load pretrained YOLOv8s
→ Test before training → Fine-tune on drone images → Evaluate (Precision, Recall, mAP)
→ Test after training and compare


1. *Load the dataset:* download it from Kaggle using kagglehub and read data.yaml to get the class name and folder paths.
2. *Load the model:* load YOLOv8s pretrained on COCO.
3. *Test before training:* run the model on a test image to see its baseline result.
4. *Train:* fine-tune the model for 6 epochs with image size 640×640.
5. *Evaluate:* check the model on the validation set using Precision, Recall and mAP.
6. *Test after training:* run the best weights on the same test image and compare with the result before training.

## Results & Evaluation

| Metric | Value |
|--------|-------|
| Precision | 0.992 |
| Recall | 0.966 |
| mAP@50 | 0.990 |
| mAP@50-95 | 0.849 |

- *Before training:* the pretrained model labeled the drone as a "bench", because "drone" is not a COCO class.
- *After training:* (6 epochs, about 1 minute on a T4 GPU): the model detects the drone with high confidence( 0.90 on the test image).

## Technologies Used
- Python
- Google Colab
- Ultralytics YOLOv8
- PyTorch
- kagglehub
- Matplotlib
- Roboflow (dataset source)

## How to Run the Project
1. Open Drone_Detection_YOLOv8.ipynb in Google Colab.
2. Go to *Runtime → Change runtime type → T4 GPU* (training also works on CPU, but it is slower).
3. Click *Run all*. The notebook will:
   - install ultralytics
   - download the dataset from Kaggle
   - train, evaluate and test the model
4. The trained model is saved as runs/detect/train/weights/best.pt.

## Future Improvements
- Train for more epochs and test on more varied images to make the model more reliable.
- Try bigger models such as YOLOv8m or YOLOv8l.
- Add more drone types and harder images (night, far distance, birds in the sky) so the model learns to tell drones apart from birds.
- Run the model on video and add tracking to follow drones from frame to frame.
- Deploy the model by exporting it to **ONNX** format, a cross platform format that runs on Windows, Linux, Mobile devices and Web browsers, so it can be used in real applications such as a live camera system.

## SDAIA Academy
- Program: Computer Vision for Developers – SDAIA Academy
- GitHub Repository: [SDAIA Academy](https://github.com/SDAIAAcademy)
