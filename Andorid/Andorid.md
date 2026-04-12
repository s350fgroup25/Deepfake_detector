## Choose the project name : 
- Auditron (Audio + Audit + AI vibe)
- VeriVox (Verification + Voice)
- EchoSentry (Echo + Sentinel)
- Phonolock (Phono + Lock) – Locks out fake audio.

## Research 
- what is Edge computing
- how to implement model to Andorid app
- try to implement model to iot device (Raspberry pi /Jetson nano )
- Cantonese fake speech
- hugging face AI model
- apply ONNX

### Hugging face AI model
- What is Hugging face
- How to run an AI model (local vs remote)
- Implementing Hugging face model
  -  iot device (Raspberry pi /Jetson nano )
  -  Andorid app

### Extral
- Cantonese fake speech
- improve model (apply ONNX)
- testing model


# Step on deploy Hugging Face Audio Deepfake Model on Raspberry Pi
## 1. Prepare Raspberry Pi:
- Get microSD card (16GB+).
- Flash Raspberry Pi OS Lite (64-bit) with Raspberry Pi Imager.
- Enable SSH and WiFi during setup.
- Power on Pi and connect via SSH.
- Update system
  
      sudo apt-get update && sudo apt-get upgrade -y
      sudo reboot
## 2. Install Docker on Raspberry Pi:
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo systemctl enable docker
    sudo systemctl start docker
    docker --version

## 3. Set Up Python Environment (Optional):
    sudo apt install python3-pip python3-venv -y
    python3 -m venv venv
    source venv/bin/activate
    pip install transformers huggingface_hub torch

## 4. Choose and Containerize Model:
- Pick audio deepfake detection model (e.g., "mo-thecreator/Deepfake-audio-detection").
- Use Docker or Chassis to build an ARM-compatible container.
- Alternatively, find pre-built ARM containers.

## 5. Deploy Container on Raspberry Pi:
    docker pull <model-container-image>
    docker run --rm -it -p 45000:45000 --memory="200m" <model-container-image>

## 6. Run Inference:
- Use Python gRPC client code to send audio and get detection results.
- Optionally wrap client into a web API (e.g., Flask).
  
# Step on deploy Hugging Face Audio Deepfake Model on Android app 
## 1. Model Preparation and Integration
### Convert Hugging Face Model to TensorFlow Lite (TFLite):
- Most Hugging Face models are originally in PyTorch or TensorFlow format. To run your deepfake detector model on Android fully offline, convert it to TFLite format, which is optimized for mobile devices.
  - First, if your model is in PyTorch, convert it to TensorFlow using Hugging Face's tools or scripts. Then, export the TensorFlow saved model.
  - Use TensorFlow Lite Converter to convert the saved model into a .tflite file.
  - Example code snippet in Python for conversion using TensorFlow: (python)

        import tensorflow as tf
        
        saved_model_dir = "saved_model_directory"
        converter = tf.lite.TFLiteConverter.from_saved_model(saved_model_dir)
        tflite_model = converter.convert()
        
        with open("model.tflite", "wb") as f:
            f.write(tflite_model)
  - Note: Model size and complexity should be compatible with mobile device limits (RAM, CPU). You may have to optimize or quantize the model for better speed and smaller size.

### Load Model in Android App:
- Add the .tflite model file into the Android assets directory.
- Use TensorFlow Lite Interpreter (org.tensorflow.lite.Interpreter) to load the model inside your app.
- Initialize the interpreter with: (java)

        Interpreter tflite = new Interpreter(loadModelFile(context, "model.tflite"));
- Prepare input tensors according to model's input shape and data format.
- Reference: Hugging Face offers a project tflite-android-transformers showing how Transformer models can be converted and run on Android.

## 2. Audio Data Capture
### Capturing Phone Calls Audio:
- Android does not provide a consistent official API for recording phone call audio due to privacy/legal restrictions.
- For Android 9 and earlier, some apps used TelephonyManager or MediaRecorder with specific permissions, but it varies by device and region.
- For Android 10+, the AudioPlaybackCapture API allows capturing audio played by other apps (including calls in some conditions), but requires user permission and the target app's audio must not be protected.
- Alternative: Prompt users to manually record calls using your app while the call is ongoing, if automatic capture is not feasible.
### Capturing Voice Message Audio:
- Voice messages come from third-party apps (e.g., WhatsApp, Messenger). Direct capture is usually not allowed for privacy reasons.
- However, you can use AudioPlaybackCapture on Android 10+ to capture audio output during playback.
- More commonly, support manual upload or sharing of voice message audio files for analysis.

### Recording Audio:
- Use Android’s MediaRecorder or AudioRecord API to record any audio input in real-time.
- Request RECORD_AUDIO runtime permission from users.

## 3. Manual Upload of .mp3 Files
### File Picker Implementation:
- Use Android’s Storage Access Framework (SAF) to let users select .mp3 files from device storage.
- This invokes a system file picker and handles storage permissions transparently.
- Example basic code to launch file picker: (java)
  
      Intent intent = new Intent(Intent.ACTION_OPEN_DOCUMENT);
      intent.setType("audio/mpeg");
      intent.addCategory(Intent.CATEGORY_OPENABLE);
      startActivityForResult(intent, REQUEST_CODE_PICK_MP3);
- Retrieve URI of selected file and read audio data.
### Audio Processing:
- Preprocess the audio file to fit your model’s expected input format (e.g., fixed length, sample rate, features like mel-spectrogram).
- Use Android Media APIs for decoding .mp3 files into raw PCM if needed.

## 4. Audio Processing and Inference
### Preprocessing:
- Convert raw audio waveforms to model input features such as mel-spectrogram, MFCC, or raw waveform tensor, according to your model’s training input.
- You can use libraries such as Librosa offline on desktop, and for Android you might implement feature extraction in Java/Kotlin or use native C++ bindings (NDK) for performance.
### Inference:
- Use the TensorFlow Lite Interpreter to run inference with the processed input tensor.
- Example inference code snippet: (java)

      float[][] input = ... // preprocessed input
      float[][] output = new float[1][1]; // example output shape
      tflite.run(input, output);
### Post-processing:
- Interpret the output probability/output logits from the model to classify if the audio is real or deepfake.
- Set thresholds or confidence values to trigger alerts.

## 5. UI & Alerts
### UI Components:
- Build UI elements for:
- Starting/stopping phone call recording or audio recording.
- Selecting and uploading .mp3 files manually.
- Displaying inference results clearly (e.g., "Real Audio" or "Deepfake Detected").

### Alerts and Notifications:
- Use Android Toasts, Dialogs, or Notifications to alert users immediately when a deepfake is detected.
- Consider showing detailed results and allowing users to replay the audio.

## 6. Permissions & Privacy
- Request and handle runtime permissions carefully for:
  - RECORD_AUDIO for audio capture.
  - READ_EXTERNAL_STORAGE / MANAGE_EXTERNAL_STORAGE for file uploads (depending on Android versions).
  - Call recording permissions if applicable (note this is device-dependent).
- Inform users transparently about privacy, emphasizing all processing is done locally on the device without sending audio data to any servers.
