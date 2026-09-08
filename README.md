# Skin Cancer Detection Android Application

An Android application for **skin-lesion image classification** using a CNN-based machine learning model deployed with **TensorFlow Lite**.

The application allows a user to capture an image using the device camera or select an existing image from the gallery. The image is then preprocessed and passed to the TensorFlow Lite model for on-device classification. The application displays the predicted class along with its confidence score.

> **Note:** This project is an educational/prototype application and is not intended to provide medical diagnosis or replace professional medical advice.

---

## Features

* Capture a skin-lesion image using the device camera
* Select an image from the device gallery
* Preview the selected image
* Resize input images to the model's required `224 × 224` dimensions
* Normalize image pixel values before inference
* Perform on-device inference using TensorFlow Lite
* Display the predicted class and confidence score
* Return the top predictions based on confidence
* No server-side inference required

---

## Project Architecture

The project consists of two major parts:

```text
                 Skin Lesion Image
                        |
             +----------+----------+
             |                     |
          Camera                Gallery
             |                     |
             +----------+----------+
                        |
                        v
                 Image / Bitmap
                        |
                        v
                 Resize 224x224
                        |
                        v
                Pixel Normalization
                        |
                        v
                    ByteBuffer
                        |
                        v
             TensorFlow Lite Model
                        |
                        v
                 CNN Inference
                        |
                        v
              Class Probabilities
                        |
                        v
              Confidence Filtering
                        |
                        v
              Predicted Class
                        |
                        v
                 Android UI
```

---

## Machine Learning Pipeline

The machine learning component is based on a **Convolutional Neural Network (CNN)** trained for skin-lesion image classification.

The overall workflow is:

```text
Dataset
   |
   v
Image Preprocessing
   |
   v
CNN Model Training
   |
   v
Trained Model
   |
   v
TensorFlow Lite Conversion
   |
   v
Android Integration
   |
   v
On-Device Inference
```

The training resources are maintained separately from the Android application in the `Training` directory.

---

## Android Application

The Android application is implemented using **Kotlin**.

The application provides two primary ways of obtaining an image:

1. **Camera** — captures an image using the device camera.
2. **Gallery** — allows the user to select an existing image.

Both input paths eventually provide an image to the same classification pipeline.

```text
Camera / Gallery
       |
       v
     Bitmap
       |
       v
   Classifier
       |
       v
TensorFlow Lite
       |
       v
 Prediction
```

---

## Image Preprocessing

Before inference, the input image is resized to:

```text
224 × 224 pixels
```

The RGB values of the image are then converted into the format expected by the TensorFlow Lite model.

The implemented normalization converts pixel values from approximately:

```text
0 – 255
```

to:

```text
0 – 1
```

The processed values are stored in a `ByteBuffer`, which is passed to the TensorFlow Lite interpreter.

---

## TensorFlow Lite Inference

The trained model is converted to TensorFlow Lite and included in the Android application's assets.

The application loads:

```text
model.tflite
labels.txt
```

The TensorFlow Lite `Interpreter` performs inference locally on the Android device.

The process is:

```text
Input Image
     |
     v
Resize
     |
     v
Normalize
     |
     v
ByteBuffer
     |
     v
TFLite Interpreter
     |
     v
Class Probabilities
     |
     v
Rank Predictions
     |
     v
Display Result
```

This allows the application to perform inference without sending the image to a remote server.

---

## Prediction Handling

The classifier produces a confidence value for each available class.

Predictions below the configured confidence threshold are filtered out.

The current implementation uses:

```text
THRESHOLD = 0.4
```

The remaining predictions are ranked according to their confidence, and the application can retain up to three top results.

The Android interface displays the highest-ranked prediction.

---

## Project Structure

```text
Skin-Cancer-Detection-Android-Application/
│
├── Testing/
│   └── SmartDoctor/
│       └── app/
│           └── src/
│               └── main/
│                   ├── assets/
│                   │   ├── model.tflite
│                   │   ├── labels.txt
│                   │   └── skin-icon.jpg
│                   │
│                   ├── java/
│                   │   └── .../
│                   │       ├── MainActivity.kt
│                   │       └── Classifier.kt
│                   │
│                   └── res/
│                       └── ...
│
├── Training/
│   ├── Dataset/
│   ├── Training_CNN_Model.ipynb
│   └── model.tflite
│
└── README.md
```

---

## Technologies Used

### Android Development

* Kotlin
* Android SDK
* Android Studio
* AppCompat
* ConstraintLayout

### Machine Learning

* Python
* CNN
* TensorFlow
* TensorFlow Lite

### Image Processing

* Bitmap-based image preprocessing
* RGB pixel extraction
* Image resizing
* Pixel normalization

---

## Requirements

For the Android application:

* Android Studio
* Android SDK
* Android device or emulator

For the model-training component:

* Python
* Jupyter Notebook
* TensorFlow and required Python libraries

---

## Running the Android Application

### 1. Clone the repository

```bash
git clone https://github.com/Dadp001/Skin-Cancer-Detection-Android-Application.git
```

### 2. Open the project

Open the Android project in **Android Studio**.

### 3. Build the project

Allow Android Studio to download and configure the required Gradle dependencies.

### 4. Run the application

Connect an Android device or start an Android emulator and run the application.

### 5. Test the classifier

You can:

* Capture an image using the camera
* Select an image from the gallery
* Preview the image
* Run the classifier
* View the predicted class and confidence

---

## Model Deployment

The trained CNN model is converted into TensorFlow Lite format:

```text
CNN Model
    |
    v
TensorFlow Lite Model
    |
    v
model.tflite
    |
    v
Android assets/
    |
    v
TensorFlow Lite Interpreter
    |
    v
On-Device Prediction
```

The model is loaded from the Android application's `assets` directory rather than being retrieved from a remote server.

---

## Why TensorFlow Lite?

TensorFlow Lite was used because the target platform is Android.

Using TensorFlow Lite allows the trained model to be integrated directly into the mobile application and executed on the device.

Advantages include:

* Local inference
* No network dependency for prediction
* No need to upload the image to a server for inference
* Suitable for mobile deployment
* Integration with Android applications

---

## Limitations

This project is an educational/prototype implementation and has several limitations:

* The model's performance depends on the quality and diversity of the training dataset.
* The application performs image classification and should not be considered a clinical diagnostic system.
* On-device inference is constrained by mobile device resources.
* The confidence threshold requires proper validation before being interpreted in a real-world medical context.
* Real-world clinical deployment would require extensive validation using appropriate medical datasets and evaluation procedures.
* The current application does not provide a complete medical diagnosis or treatment recommendation.

---

## Future Improvements

Potential improvements include:

* Expand and diversify the training dataset
* Apply more extensive data augmentation
* Explore transfer-learning-based architectures
* Improve class balancing
* Perform detailed per-class evaluation
* Add confusion-matrix and sensitivity/specificity analysis
* Optimize the TensorFlow Lite model for mobile devices
* Add model explainability techniques
* Improve image-quality validation before classification
* Add prediction history
* Provide stronger user guidance around image capture
* Perform appropriate clinical validation before any real-world medical use

---


