# Thumb-Drive App (TDA)
- Brings the power of AI to places where there's no internet.
- Creates reliable and reproducible computational environments for researchers and students.

<br>

<img src="images/image2.png" alt="Doctor working on laptop." height="400">


<br>

- A portable folder-based AI app that doesn’t need system wide installation. 
- All dependencies  are bundled - including Ollama, the model, the Python interpreter and the wheels for all Python packages.
- Sensitive user data can be encrypted locally.
- Share using AirDrop or a thumb-drive. 
- All dependencies travel with the copy. 
- Double-click to set up and launch.
- Runs completely offline.
- Can be run on an external SSD.
- Ideal for use with a Silicon Mac.

<br>

<img src="images/image1.png" alt="Infographic" width="500">

<br>

The entire Python packaging ecosystem — pip, conda, uv, pyproject.toml, requirements.txt — is architected around the assumption that software needs to be updated. Version ranges, dependency resolution, compatibility matrices — this complexity exists to answer the question: "How do we keep this working as things change?"

But the TDA concept is asking a different question: "How do we make this work perfectly, right now, forever?"<br>
The goal is to freeze the environment. 

Researchers and students don't need the latest version of pandas — they need the version that was working yesterday to still work tomorrow. 

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

- Ollama, the Models, the Python interpreter, the uv binary and the wheels for all dependencies are bundled together.
- An installed app can be shared by simply making a copy of the project folder.
- A user who receives a copy doesn't need an internet connection to use the app.

<br>

## Working Example - Ai-On-A-Stick v3.0

Huggingface Datasets is used as an app store where TDAs can be stored and easily distributed. All files are hashed. This enables the traceability.

HuggingFace:<br>
https://huggingface.co/datasets/vbookshelf/Ai-On-A-Stick-TDA

System Requirements:

- Operating System: MacOS
- Computer: Apple Silicon Mac (M1, M2, M3, M4)
- RAM: 16GB
- Free disk Space: 9.2 GB

Click this link to auto download (8.1 GB). Installation instructions are included in the project folder:<br>
https://huggingface.co/datasets/vbookshelf/Ai-On-A-Stick-TDA/resolve/main/AI-On-A-Stick-v3.0-TDA.zip?download=true

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

To make a TDA app executable, this command needs to be typed in the terminal:

```
cat start-mac-app.command > temp && mv temp start-mac-app.command && chmod +x start-mac-app.command
```

To remove this requirement the start-mac-app.command file would need to be code signed. Most macOS apps distributed outside the App Store are code signed using an Apple Developer ID certificate. Code signing requires an active Apple Developer Program membership.

TDA apps do not not use code signing because it introduces a point of failure. If the Apple membership lapses, or if Apple revokes the certificate for any reason, signed apps will stop launching cleanly for new users. This risk is unacceptable.

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

- HuggingFace could become an app store for free self-contained open source offline AI apps. The infrastructure already exists as HuggingFace Datasets.

- The Python interpreter and the wheels for all packages, also need to be included in the bundle for the TDA to truly operate offline.
  
- Instead of using pip to download wheels and storing them locally, a more elegant solution is to make a copy of the uv cache. The cache is platform/arch-specific. The cache directory structure is uv's internal format and could theoretically change between uv versions. This app uses a bundled uv version so that issue is not a problem.
```
uv sync --cache-dir ./my-bundled-cache
```

<br>
