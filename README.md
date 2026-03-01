# Thumb-Drive App (TDA)
### AI on a stick - for places where there's no internet.

- A method to deploy portable self-contained AI apps in places where there's no internet.
- Bundles the AI model and all binary executables together with the app - including Ollama.
- Only the first install requires an internet connection.
- Copies of the installed app can be shared using a thumb-drive or AirDrop. All dependencies travel with the copy.
- Double-click to install.
- Runs completely offline.
- Can be run on an external SSD.
- Runs in a virtual environment so other software on the user's computer is not disturbed.
- User data is encrypted.



<br>

<img src="images/image1.png" alt="Infographic" width="500">

<br>

## How this works

1. Download the app folder from Kaggle:<br>
https://www.kaggle.com/datasets/vbookshelf/v2-0-offline-medai-console-thumb-drive-app

3. Install the app with internet enabled. The MedGemma 4B BF16 model and all pyproject.toml dependencies will be auto downloaded and installed. Total size, after installation, is approximately 10.2 GB. 
4. To share the app with others simply share a copy of your project folder. All the dependencies, downloaded during your installation, will travel with it. The person who uses your copy won't need  to download anything from the internet. They just need to double click the start-mac-app.command file to install the app.<br>
5. Your conversation history and any images you upload are encrypted. However, please delete the storage_vault folder from the copy that you share. If you forget to do this it's not a problem. Because your data is encrypted no one else will be able to see it. Also, your data is tied to your computer. If the app detects your data on another device, it will auto delete that data.

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

- Ollama, the Models and all dependencies are bundled together
- An installed app can be shared by simply making a copy of the project folder.
- A user who receives a copy doesn't need an internet connection to use the app.

<br>

<img src="images/image2.png" alt="App screenshot" height="500">

<br>

## Working Example - The Offline MedAi Console v2.0

The Offine MedAi Console is an AI collaborator for clinicians. It's a transparent, offline-first and privacy-first multimodal AI console where clinicians can talk, type, show images, adjust parameters and create AI tools. Uses Flask for the backend, Whisper for Speech-to-Text (STT), Kokoro for Text-to-Speech (TTS), and Ollama to serve the Large Language Model (LLM). It's designed to run on Apple Silicon computers.

I built version 1.0 of this console as my entry for The MedGemma Impact Challenge hackathon on Kaggle. I converted version 2.0 into a Thumb-Drive App.

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

- Converted the app into a TDA
- Added data encryption

## Download the demo app from Kaggle

The demo is stored in a Kaggle dataset and not here because GitHub has a file size limit.<br>
https://www.kaggle.com/datasets/vbookshelf/v2-0-offline-medai-console-thumb-drive-app

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
- Free disk Space: 10.2 GB

## Installation Instructions

### 1. If you downloaded the app from Kaggle:

a. Unzip and place the MedAi-Console-v2.0-TDA folder on your
desktop.

b. cd into MedAi-Console-v2.0-TDA folder:
```
cd Desktop
cd MedAi-Console-v2.0-TDA
```

c. Paste this command into the terminal and press Enter:
(MacOS quarantines downloaded files. This strips the quarantine attribute so that the file is executable.)

```
xattr -d com.apple.quarantine start-mac-app.command
```

d. Open the MedAi-Console-v2.0-TDA folder

e. Double click this file: ```start-mac-app.command```

f. The app will install automatically and then open in your browser.
Please note that during this step approximately 10 GB will be downloaded. You will need a fast and stable internet connection.

### 2. If you received a copy that someone already installed:

a. Move the folder to your desktop. If your external drive is an SSD, you don't need to move the folder to your computer. The app can run on the SSD.

b. Double click this file: ```start-mac-app.command```

c. The app will install automatically and then open in your browser.

### How to shut down the app

The app does not stop running when you close the browser tab.
To shut down the app, close the terminal window.
You can also close the terminal by selecting it and typing Ctrl+C.

<br>

## How to apply this approach to your own offline AI app

To convert your AI app to a TDA, I suggest that you collaborate with Claude 4.6 Sonnet. It's free.<br>
You need to clearly state your goal and give the AI as much context as possible.

During the discussion you should do the following:

- Review the included pdf that explains how to get the binaries for Ollama, UV and ffmpeg
- Review the pdf, inside the demo app folder, that explains how to install the app.
- Give Claude the demo start-mac-app.command file so it can see how to implement the Double-Click installation
- Give Claude the demo app.py so it can see how to implement the Encryption
- Give Claude a screenshots of the demo app folder structure
- Give Claude the link to this repo so it can understand the approach
- Give Claude the your app's python code.
- Ask Claude to modify your app.

What Claude should do:

- Create a new start-mac-app.command file.
- Revise your Python code to include Encryption of any user information so it's not readable if the user forgets to delete it before sharing a copy of the app.
- Explain how to put everything together.

AI collaboration is a force multiplier. It's important to get good at it. Good luck.
<br>


