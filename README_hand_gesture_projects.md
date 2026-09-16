# Hand Landmark and Gesture Recognition

This repository contains two MediaPipe computer-vision notebooks:

- `handmarkingthingymediapipe.ipynb` detects hands and draws  hand landmarks.
- `gesture_recog.ipynb` recognises common hand gestures and displays the predicted label and confidence score.

Both notebooks use MediaPipe Tasks, Python, and image-processing utilities to analyse hand images.

## Projects

### Hand landmark detection

The hand-landmark notebook uses the MediaPipe Hand Landmarker task. It detects up to two hands and returns 21 landmarks for each detected hand. The notebook draws the landmarks and connections on the input image and labels each hand as left or right.

### Gesture recognition

The gesture-recognition notebook uses the MediaPipe Gesture Recognizer task. It processes example images such as:

- Thumbs down.
- Victory.
- Thumbs up.
- Pointing up.

The notebook displays the recognised gesture, confidence score, and detected hand landmarks.

## Features

- Detects hands in images.
- Locates hand landmarks.
- Draws landmark points and hand connections.
- Identifies handedness where available.
- Recognises supported static hand gestures.
- Displays prediction labels and confidence scores.
- Supports uploaded images in Google Colab.
- Resizes images for easier display.
- Visualises multiple images in a batch.

## How it works

### Hand landmark pipeline

1. Install MediaPipe.
2. Download the Hand Landmarker model.
3. Create a `HandLandmarker` object.
4. Load an image with `mp.Image.create_from_file()`.
5. Run hand landmark detection.
6. Draw landmarks, connections, and handedness labels.
7. Display the annotated image.

### Gesture recognition pipeline

1. Install MediaPipe.
2. Download the Gesture Recognizer model.
3. Create a `GestureRecognizer` object.
4. Load one or more images.
5. Run gesture recognition on each image.
6. Read the top gesture category and confidence score.
7. Draw the detected landmarks.
8. Display the annotated images in a grid.

## Requirements

Install the main dependency with:

```bash
pip install mediapipe
```

The notebooks also use:

```bash
pip install numpy matplotlib opencv-python
```

Google Colab users can install the packages in a notebook cell:

```python
!pip install -q mediapipe
```

## Model files

The notebooks use MediaPipe task model files:

```text
handlandmarker.task
gesturerecognizer.task
```

The hand-landmark notebook downloads the Hand Landmarker model. The gesture notebook downloads the Gesture Recognizer model.

Keep each model file in the working directory or update the model path in the notebook.

Example configuration for hand landmarks:

```python
base_options = python.BaseOptions(
    model_asset_path="hand_landmarker.task"
)
options = vision.HandLandmarkerOptions(
    base_options=base_options,
    num_hands=2
)
detector = vision.HandLandmarker.create_from_options(options)
```

Example configuration for gesture recognition:

```python
base_options = python.BaseOptions(
    model_asset_path="gesture_recognizer.task"
)
options = vision.GestureRecognizerOptions(
    base_options=base_options
)
recognizer = vision.GestureRecognizer.create_from_options(options)
```

## How to run

### Google Colab

1. Open the required notebook in Google Colab.
2. Run the installation cell.
3. Download the required MediaPipe task model.
4. Download the example images or upload your own images.
5. Run the visualisation functions.
6. Run the detection or recognition cell.
7. Review the annotated image and prediction output.

The notebooks include upload code similar to:

```python
from google.colab import files
uploaded = files.upload()
```

### Local Jupyter environment

1. Install Python 3 and the required packages.
2. Place the notebook, model file, and images in a project directory.
3. Start Jupyter Notebook:

```bash
jupyter notebook
```

4. Open the relevant notebook.
5. Update image and model paths if required.
6. Run the cells in order.

## Input images

The notebooks can process images containing one or more visible hands. For better results:

- Use clear images with good lighting.
- Keep the hand inside the image frame.
- Avoid excessive motion blur.
- Keep fingers separated where possible.
- Avoid heavy occlusion.
- Use a plain or uncluttered background when possible.

The gesture notebook uses example image filenames such as:

```text
thumbsdown.jpg
victory.jpg
thumbsup.jpg
pointingup.jpg
```

You can replace these with your own images.

## Output

The hand-landmark notebook produces an annotated image containing:

- Hand landmark points.
- Lines connecting the landmarks.
- Left or right hand labels.

The gesture-recognition notebook produces an annotated image containing:

- Detected hand landmarks.
- The top predicted gesture.
- The prediction confidence score.

Example output format:

```text
Thumbs_Up: 0.94
```

The exact labels and scores depend on the input image and MediaPipe model version.

## Project structure

```text
.
├── gesture_recog.ipynb
├── handmarkingthingymediapipe.ipynb
├── gesture_recognizer.task
├── hand_landmarker.task
├── thumbsdown.jpg
├── victory.jpg
├── thumbsup.jpg
├── pointingup.jpg
└── README.md
```

The model and image files may need to be downloaded or uploaded separately.

## Important implementation details

MediaPipe returns landmark coordinates in a normalised format. The notebook converts these coordinates into image positions when drawing labels and annotations.

For display, OpenCV uses BGR colour ordering, while MediaPipe images are generally handled as RGB. Convert the image colour format correctly when displaying an image with OpenCV:

```python
cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
```

The gesture notebook displays the top result from the gesture list. The reported score is a confidence value from the MediaPipe recogniser, not a guarantee that the prediction is correct.

## Limitations

- Performance depends on lighting, camera angle, image quality, and hand visibility.
- Similar gestures may be confused.
- The default recogniser supports a limited set of gestures.
- Static-image recognition may not work well for moving gestures.
- Confidence scores should not be interpreted as certainty.
- Occluded or partially visible hands may produce missing or inaccurate landmarks.
- The notebooks process images rather than providing a complete real-time application.
- Google Colab may not support local webcam access without additional setup.

## Possible improvements

- Add real-time webcam support with OpenCV.
- Add video and live-stream modes.
- Create a custom gesture classifier using landmark coordinates.
- Add temporal smoothing for stable predictions.
- Track gestures across consecutive frames.
- Add a confidence threshold and an unknown-gesture class.
- Support more custom gestures.
- Save landmark coordinates and predictions to CSV.
- Build a user interface with Streamlit or Tkinter.
- Add automated tests for image loading and output formatting.
- Measure accuracy on a labelled test dataset.

## Responsible use

Use this project for education, prototyping, accessibility experiments, and human-computer interaction research. Do not rely on gesture predictions for safety-critical control without extensive testing, fail-safe design, and human oversight.

## Author

Praveen Athikari

MSc Artificial Intelligence student interested in computer vision, machine learning, and practical AI applications.
