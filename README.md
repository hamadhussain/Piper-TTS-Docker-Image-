This is a professional and clean **README.md** structure you can copy and paste directly into your GitHub repository. It is designed to be user-friendly, clear, and technically sound so that anyone who visits your profile can follow it without getting stuck.

---

# 🎙️ Local Text-to-Speech with Piper & Docker on WSL2

This project demonstrates how to set up and run **Piper**, a fast, local neural text-to-speech engine, using **Docker** inside a **WSL2** (Windows Subsystem for Linux) environment. This setup ensures high-quality voice generation without relying on paid cloud APIs.

---

## 🚀 Step-by-Step Implementation Guide

### 1. Environment Synchronization (WSL2 & Docker)
To use Docker commands inside your WSL2 terminal, you must enable the integration.
* Open **Docker Desktop** on Windows.
* Navigate to **Settings > Resources > WSL Integration**.
* Toggle **ON** the switch for your specific Ubuntu/WSL distro.
* Click **Apply & Restart**.
* **Verification:** Run `docker -v` in your WSL terminal. If it returns a version, you are ready.

### 2. Project Directory & Assets
Create a dedicated workspace to store your voice models and generated audio files.
```bash
mkdir -p /mnt/d/DockerData/piper_project
cd /mnt/d/DockerData/piper_project
```
**Model Downloads:** Download the `.onnx` and `.json` files for your preferred voice from the [Official Piper Voices Repository](https://huggingface.co/rhasspy/piper-voices/tree/main). 
*Place these files directly into your project folder.*

### 3. The Efficient Approach: Direct CLI (Recommended for 8GB RAM)
If you have limited RAM, the "Direct CLI" method is the best. It runs the container, generates the audio, and immediately terminates to free up system resources.

```bash
echo "Hello, this is a local voice generation test." | \
docker run -i --rm \
  -v "$(pwd):/quality" \
  synesthesiam/piper \
  --model /quality/en_US-libritts_r-medium.onnx \
  --output_file /quality/output.wav
```

### 4. The Advanced Approach: Server Mode (Wyoming)
For developers building apps that need to request audio via API, use the Wyoming Server mode. This keeps the model loaded in memory for instant responses.

**Start the Server:**
```bash
docker run -it --rm \
  -p 10200:10200 \
  -v "$(pwd):/data" \
  rhasspy/wyoming-piper \
  --voice /data/en_US-libritts_r-medium.onnx \
  --data-dir /data
```

**Generate Audio (From a second terminal):**
```bash
curl -G "http://localhost:10200/" \
    --data-urlencode "text=The server is active and responding." \
    -o server_output.wav
```

---

## 🛠️ Performance & Troubleshooting Tips

* **RAM Management:** Docker and WSL2 can be resource-intensive. If your system hangs, close heavy applications like Chrome or VS Code while generating audio.
* **Port Mapping:** Always use the `-p 10200:10200` flag in Server Mode; otherwise, your `curl` requests from Windows/WSL won't reach the container.
* **Volume Mounting:** The `-v "$(pwd):/data"` flag is critical. It maps your local folder to the container so Piper can find your model files.
* **404 Errors:** When using `wget`, ensure the URL is a direct download link. If it fails, download via browser and move the file manually to the project folder.

---

### 🌟 Project Impact
By following this guide, you can implement a fully private, offline, and zero-cost TTS solution. This is ideal for automation bots, accessibility tools, or AI-integrated applications.

**Developed by:** Hammad Hussain
**Tech Stack:** Docker | WSL2 | Piper TTS | Linux

---

> **Note:** If you find this useful, feel free to star the repository!
