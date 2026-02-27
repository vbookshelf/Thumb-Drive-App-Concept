Project in progress...

# Thumb Drive App (TDA)

- A method to deploy portable self-contained AI software in low resource settings.
- Bundle the AI model and all binary executables, including Ollama, together with the app.
- Runs offline.
- Double-click a file to install.
- Runs in a virtual environment.

<br>

<img src="images/image1.png" alt="Infographic" width="500">

<br>

## How this works

1. Download the app folder from Kaggle
2. Install the app with internet enabled. The model and all pyproject.toml depencies will be installed.
3. To share the app with others simply share a copy of your project folder. All installed dependencies will travel with it. The person who uses your copy won't need internet to install it. They just need to double click the start-mac-app.command file.<br>
<strong>*** Please ensure you've deleted your conversation history (app/conversations.json) and any images you've uploaded (app/persistent_uploads) before sharing the folder with others.</strong>

<br>

## Modern AI software installation

- Install Ollama
- Download models from HuggingFace
- Download code from GitHub
- Run pip install requirements.txt

## Why this doesn't work in low resource settings

- No internet or internet is very slow
- Internet connection is unstable
- Internet data plans are expensive

## Solution - The Thumb-Drive App

- Ollama, the Models and all dependencies are downloaded together
- Installation doesn't require an internet connection
- Double-click a file to install and run
- App runs completely offline
- Entire app can be shared on a thumb-drive, via AirDrop or over a local network

<br>

<img src="images/image2.png" alt="App screenshot" height="500">

<br>

## Working Example - The Offline MedAi Console v2.0 TDA

The Offine MedAi Console is an AI collaborator for clinicians. It's a transparent, offline-first and privacy-first multimodal AI console where clinicians can talk, type, show images, adjust parameters and create AI tools. Uses Flask for the backend, Whisper for Speech-to-Text (STT), Kokoro for Text-to-Speech (TTS), and Ollama to serve the Large Language Model (LLM). It's designed to run on Apple Silicon computers.

I originally built version 1 of this console as my entry for a Medical AI hackathon on Kaggle. I've converted version 2 into a Thumb-Drive app. All dependencies including Ollama and the MedGemma 4b BF16 model are bundled with the app. Total size is approximately 10.2 GB. No internet is needed for installation. The app can be shared using a thumb drive or via AirDrop.

<br>

<img src="https://github.com/vbookshelf/Offline-MedAi-Console/blob/main/images/image2.png" alt="App screenshot" height="500">

<br>

## Version 1.0

YouTube Video - Demo<br>
https://www.youtube.com/watch?v=X3-6MpZp89s

Kaggle Writeup - The MedGemma Impact Challenge<br>
https://www.kaggle.com/competitions/med-gemma-impact-challenge/writeups/offline-medai-console

GitHub<br>
https://github.com/vbookshelf/Offline-MedAi-Console

## Changes made in v2.0

- All dependencies, including Ollama and the MegGemma model, have been bundled together (10 GB)
- Installation can be done offline
- User double clicks a file to install and run
- Entire app  can be shared on a thumb drive

## Download from Kaggle

The code is stored in a Kaggle dataset and not on GitHub because GitHub has a file size limit.
XXXXX-XXXXX

## Bundled package

- UV
- Ollama
- ffmeg
- kokoro-v1.0.onnx
- voices-v1.0.bin
- hf.co/unsloth/medgemma-4b-it-GGUF:BF16
  
<br>

- "flask==3.1.2",
- "ollama==0.6.0",
- "kokoro-onnx==0.4.9",
- "soundfile==0.13.1",
- "openai-whisper==20250625",
- "pymupdf==1.26.4",
- "pillow==11.3.0",
- "requests==2.32.3",
- "flask-socketio==5.5.1",
- "pydicom==2.3.0",

## System Requirements:

- Operating System: MacOS
- Computer: Apple Silicon Mac (M Series)
- RAM: 16GB
- Free disk Space: 10 GB

## Installation Instructions

```
1. Unzip (if required) and place the MedAi-Console-v2.0 folder on your
desktop.

2. cd into MedAi-Console-v2.0-TDA folder:
cd Desktop
cd MedAi-Console-v2.0-TDA

3. Paste this command into the terminal and press Enter:
(This overwrites the file and changes the file permissions to make it executable.)

cat start-mac-app.command > temp && mv temp start-mac-app.command && chmod +x start-mac-app.command

4. Open the MedAi-Console-v2.0-TDA folder

5. Double click this file: start-mac-app.command

6. The app will install automatically and then open in your browser.

```

<br>

## Notes

- I used Claude Sonnet 4.6 extensively during this process. It was an excellent guide.<br>
https://claude.ai/chat/36b3691f-d841-4916-8176-a3547d8a34b3

