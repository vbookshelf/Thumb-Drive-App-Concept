# Thumb-Drive App (TDA)
### Bring the power of AI to places where there's no internet.

<br>

<img src="images/image2.png" alt="Doctor working on laptop." height="400">


<br>

- A portable folder-based AI app that doesn’t need system wide installation. 
- All dependencies  are bundled - including Ollama and the model. 
- Share using AirDrop or a thumb-drive. 
- All dependencies travel with the copy. 
- Double-click to set up and launch.
- Runs completely offline.
- Can be run on an external SSD.
- Ideal for use with a Silicon Mac.

<br>

<img src="images/image1.png" alt="Infographic" width="500">

<br>

## How this works

1. Download the app folder from Kaggle:<br>
https://www.kaggle.com/datasets/vbookshelf/v2-0-offline-medai-console-thumb-drive-app

2. Install the app with internet enabled. The MedGemma 4B BF16 model and all pyproject.toml dependencies will be auto downloaded. Total size is approximately 10.2 GB. 
3. To share the app with others simply share a copy of your project folder. All the dependencies will travel with it. The person who uses your copy won't need  to download anything. They just need to double click the start-mac-app.command file to run the app. No internet required.<br>
4. Your conversation history and any images you upload are encrypted. However, please delete the storage_vault folder from the copy that you share. If you forget to do this, it's not a problem. Because your data is encrypted no one else will be able to see it. Also, your data is tied to your computer. If the app detects your data on another device, it will auto delete that data.
<br>

## Current AI software installation process

- Download code from GitHub
- Download models from HuggingFace
- Install Ollama
- Install ffmpeg
- Install uv
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
- ffmpeg
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
cat start-mac-app.command > temp && mv temp start-mac-app.command && chmod +x start-mac-app.command
```

d. Open the MedAi-Console-v2.0-TDA folder

e. Double click this file: ```start-mac-app.command```

f. The app will download all requirements and then open in your browser.
Please note that during this step approximately 10 GB will be downloaded. You will need a fast and stable internet connection.

### 2. If you received a copy that someone already installed:

a. Move the folder to your desktop. If your external drive is an SSD, you don't need to move the folder to your computer. The app can run on the SSD.

b. Double click this file: ```start-mac-app.command```

c. The app will open in your browser.

### How to shut down the app

The app does not stop running when you close the browser tab.
To shut down the app, close the terminal window.
You can also close the terminal by selecting it and typing Ctrl+C.

<br>

## Data Encryption Strategy

All patient data — conversation history and uploaded files (images, PDFs, DICOM files) — is encrypted at rest inside a local storage_vault folder. No plaintext patient data is ever written to disk.

### How it works:

The encryption key is derived from the Mac's hardware UUID, a unique identifier built into the logic board of every Apple Silicon computer. At startup, the app reads this UUID via ioreg and runs it through PBKDF2-HMAC-SHA256 (480,000 iterations) to produce a 256-bit key. That key is used to initialize a Fernet cipher (AES-128-CBC with HMAC-SHA256 authentication), which handles all encrypt and decrypt operations for the lifetime of that session.

### Machine binding via sentinel file:

The first time the app runs, it encrypts a known constant (VAULT_OK) and writes it to storage_vault/sentinel.bin. On every subsequent startup, the app attempts to decrypt the sentinel using the current machine's hardware UUID. If decryption succeeds, the vault is verified and the app starts normally. If decryption fails — meaning the vault folder has been moved to a different computer — the app immediately deletes the entire vault and exits with a security warning. Your data cannot be read on any machine other than the one that created it.

### What is encrypted:

- storage_vault/conversations.enc — all chat history, stored as a single encrypted JSON file
- storage_vault/persistent_uploads/*.enc — every uploaded file (image, PDF, DICOM-converted PNG), encrypted individually before being written to disk. Files are decrypted in memory on demand and never written back to disk in plaintext.

### When sharing a copy of the app:

Because the vault is machine-bound, anyone who receives a copy of the app folder cannot decrypt the original user's data even if they forget to delete the storage_vault folder before sharing. As an additional precaution, it's still good practice to delete storage_vault before sharing, since the encrypted files represent patient data and should not travel beyond the original device.

### Limitations to be aware of:

- The encryption protects data at rest from other-machine access, but does not protect against someone with physical access to the same Mac (since the key is derived from that machine's UUID, which any process on that Mac can read).
- The PBKDF2 salt (MedAiConsole_VaultSalt_v1) is fixed and visible in the source code. Security relies on the secrecy of the hardware UUID, not the salt.
- Full disk encryption (FileVault) is recommended as a complementary layer of protection.


<br>

## One Command. Complete Control.

You would have noticed that, to make this app executable, this command needed to be typed in the terminal:

```
cat start-mac-app.command > temp && mv temp start-mac-app.command && chmod +x start-mac-app.command
```

To remove this requirement I would need to code sign the start-mac-app.command file. Most macOS apps distributed outside the App Store are code signed using an Apple Developer ID certificate. Code signing requires an active Apple Developer Program membership.

I don't use code signing because it introduces a point of failure. If the Apple membership lapses, or if Apple revokes the certificate for any reason, signed apps will stop launching cleanly for new users. For software intended for use in medical or other high-stakes settings, this risk is unacceptable.

In exchange for the minor inconvenience of typing one command into the terminal, you get a reliable app that has no dependency on any third party. You have complete control.


<br>

## Rough Notes

- Currently the app opens in the user's default browser. The app still looks like a web app. But, by using pywebview it's possible to give a flask app the look and feel of a native macOS app. No packaging needed. The code remains completely transparent and auditable. Also, the app will shut down when the user closes the app window - currently the terminal must be shut down in order to stop the app.<br>
https://github.com/r0x0r/pywebview<br>
This is an example of a secure and optimized TDA translation app that uses TranslateGemma 12b 4-bit:<br>
https://github.com/vbookshelf/Takkie-Translate-Offline
<img src="images/image3.png" alt="macOS translation app" height="400">

- The terminal window can be collapsed or hidden by adding one of these lines to the start-mac-app.command file:<br>
Minimise the terminal window to the dock:<br>
```osascript -e 'tell application "Terminal" to set miniaturized of window 1 to true'```<br>
Hides the terminal window:<br>
```osascript -e 'tell application "System Events" to set visible of process "Terminal" to false'```

- It's important to modify the start-mac-app.command file so the code waits for Ollama to be ready before the app launches:<br>
```
# Wait for Ollama to be ready
echo "[INFO] Waiting for Ollama to be ready..."
MAX_WAIT=30
COUNT=0
until curl -s "http://127.0.0.1:11436" > /dev/null 2>&1; do
    if [ $COUNT -ge $MAX_WAIT ]; then
        echo "[ERROR] Ollama did not start within ${MAX_WAIT} seconds."
        echo "This may be caused by a port conflict on 11436 or insufficient system resources."
        exit 1
    fi
    # Check if Ollama process died unexpectedly before the timeout
    if ! kill -0 "$OLLAMA_PID" 2>/dev/null; then
        echo "[ERROR] Ollama process exited unexpectedly. Check that the binary is not corrupted."
        exit 1
    fi
    sleep 1
    COUNT=$((COUNT + 1))
done
echo "[INFO] Ollama is ready."
```

- When using voice input and output with Safari, audio output (AI voice) quality and robustness is not as good as when using Chrome. On Mac, pywebview is using the Safari engine, therefore audio quality also varies.
  
- Storing the project folder as a HuggingFace dataset with all dependencies bundled, enables a true double-click, zero-install experience. Also, HuggingFace automatically generates SHA256 hashes for every file in a dataset repository. This traceability will help during a compliance audit.<br>
https://huggingface.co/datasets/vbookshelf/Offline-MedAi-Console-TDA

- The code in the start-mac-app.command file is currently set up to be self healing i.e. if any dependencies are missing (e.g. the model), it downloads those missing dependencies automatically during startup. This code should be modified to be able to toggle self healing on and off. In this way the user won't be surprised by an unexpected download. Having the self healing feature makes the app very easy to package therefore, it should not be completely removed.

- HuggingFace could become an app store for free self-contained open source offline AI apps. The infrastructure already exists as HuggingFace Datasets. This is what a portfolio of apps would look like:<br>
https://huggingface.co/vbookshelf
  
<br>
