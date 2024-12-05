# Adaptive-Digital-Intelligent-Task-Integrator
## Team members:
- Takshal Modi (202151170)
- Tanish Nagrani (202151190)
- Varun Vilvadrinath (202151195)
- Shubham Mehatre (202152323)
- Rushabh Babre (202152334)

## Project Description
This project explores the integration of conversational AI and home automation systems using cost-effective and compact hardware solutions. Utilizing two Raspberry Pi devices, the system combines voice-based control with intelligent automation. 

- **Node 1** is configured to handle audio input and output via a Bluetooth headset. (Raspberry PI)
- **Node 2** hosts the Home Assistant platform to manage automation tasks. (Raspberry PI)

- **AI Server** has following functionalities
   - Speech-to-text and text-to-speech functionalities are implemented using **Whisper** and **Piper**, respectively.
   - The **Llama 3.2 model** powers natural language processing for chatbot interactions.

The project demonstrates:
- Seamless communication between devices
- Efficient task execution
- Intuitive user experience

Despite challenges such as hardware limitations and internet dependency, the system validates the potential of integrating IoT and AI technologies to create smart environments. Future enhancements include:
1. Incorporating real-time data retrieval models.
2. Optimizing edge computing capabilities.
3. Expanding system scalability.

This initiative lays the groundwork for accessible, intelligent, and interactive home automation solutions.

---

## Harware Description:

- **Raspberry PI**
   - Pi 4 model b
   - 4 GB RAM
   - 64-bit quad core Cortex A72 Processor

- **Ai Server**
   - OS: Ubuntu 24.04.1 LTS
   - Processor: AMD Ryzen™ 7 5800H with Radeon™ Graphics × 16
   - RAM: 16 GB
   - Graphics Card: Nvidia 3050 RTX 4GB
---


## Installation Steps on AI Sever

### Step 1: Pull and Run Docker Images
1. Run container for piper and whisper with following images:
   - `rhasspy/wyoming-whisper`
   - `rhasspy/wyoming-piper:1.3.0`
2. **Why use these images?**
   - **Whisper**: Provides accurate and efficient speech-to-text conversion.
   - **Piper**: Offers reliable text-to-speech functionality.

   Use the following commands:
   ```bash
   docker run -itd -p 10300:10300 -v ./whisperdata:/data rhasspy/wyoming-whisper --model tiny-int8 --language en
   docker run -itd -p 10200:10200 -v ./piperdata:/data rhasspy/wyoming-piper --voice en_US-lessac-medium
   ```

3. Expose the ports on which the Docker images are running:
   ```bash
   sudo ufw allow 10300/tcp
   sudo ufw allow 10200/tcp
   ```

---

### Step 2: Install Ollama
1. Download Ollama from [https://ollama.com/download](https://ollama.com/download).
2. For Linux:
   ```bash
   curl -fsSL https://ollama.com/install.sh | sh
   ```
3. By default, Ollama runs on port `11434`. Ensure to expose this port as well:
   ```bash
   sudo ufw allow 11434/tcp
   ```

4. Modify Ollama to run on `0.0.0.0` instead of `localhost`:
   - Edit the `ollama.service` file:
     ```bash
     sudo nano /etc/systemd/system/ollama.service
     ```
   - Replace Service Section with this:
     ```bash
      [Service]
      ExecStart=/usr/local/bin/ollama serve
      User=ollama
      Group=ollama
      Restart=always
      RestartSec=3
      Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin"
      Environment="OLLAMA_HOST=0.0.0.0:11434"
     ```
   - Reload the system daemon and restart Ollama:
     ```bash
     sudo systemctl daemon-reload
     sudo systemctl restart ollama
     ```

---

## Installation Steps on Rapberry Pi 1

### Step 1: Set Up Raspberry Pi OS
1. Install **Raspberry Pi Imager** from [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/).
2. Use the imager to install **Ubuntu Desktop 22.04.5 LTS (64-bit)**:
   - Go to: `Other general-purpose OS > Ubuntu > Ubuntu Desktop 22.04.5 LTS`.
3. Flash the OS to an SD card and insert it into the Raspberry Pi.

---

### Step 2: Install PipeWire
1. SSH into the Raspberry Pi once it is running:
   ```bash
   ssh <username>@<raspberry_pi_ip>
   ```
2. Install **PipeWire** by following the instructions at:
   [How to Install PipeWire on Ubuntu](https://linuxconfig.org/how-to-install-pipewire-on-ubuntu-linux).

---

### Step 3: Connect Bluetooth Device for Audio I/O
1. Use the following commands to connect your Bluetooth headset:
   - Scan for devices:
     ```bash
     bluetoothctl scan on
     ```
   - Pair with the device:
     ```bash
     bluetoothctl pair <MAC_ADDR_OF_DEVICE>
     ```
   - Trust the device:
     ```bash
     bluetoothctl trust <MAC_ADDR_OF_DEVICE>
     ```
   - Connect the device:
     ```bash
     bluetoothctl connect <MAC_ADDR_OF_DEVICE>
     ```

2. To find the MAC address of your Bluetooth headset:
   - Connect the headset to your PC and run:
     ```bash
     pactl list cards
     ```

---

### Step 4: Test Audio on Node
1. Record audio from the microphone:
   ```bash
   arecord -D pipewire -r 16000 -c 1 -f S16_LE -t wav -d 5 test.wav
   ```
   - This will record 5 seconds of microphone input.

2. Play the recorded audio:
   ```bash
   aplay -D pipewire test.wav
   ```

---

### Step 5: Set Up Wyoming Satellite
Follow the detailed guide to set up Wyoming Satellite on your Raspberry Pi:
[Wyomming Satellite Setup](https://drive.google.com/file/d/1VhVUEPQiGz3orX6F_mTZRYqFsTi2QkI4/view?usp=sharing).

Document mentioned in the video tutorial [Link](https://www.slacker-labs.com/setup-a-raspberry-pi-zero-2-w-as-a-wyoming-satellite/).

---

## Installation Steps on Rapberry Pi 2
### Step 1: Install Home Assistant OS on Second PI:
1. Install **Raspberry Pi Imager** from [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/).
2. Use the imager to install **HomeAssistant OS**:
   - Go to: `Other specific-purpose OS > Home Assistant and Home Automation > Home Assistant`.
3. Flash the OS to an SD card and insert it into the Raspberry Pi.

---

### Step 2: Configuration:
- Refer to this detailed video explanation for the same [Link](https://drive.google.com/file/d/1d1jemFtziGqCuP5M1FA6Jw559HZlRKJM/view?usp=drive_link).
---

## Future Enhancements
1. Real-time data retrieval models
2. Edge computing optimization
3. Scalability for larger systems

---
## Video Demonstration:
- You can access the demo over here [Link](https://drive.google.com/drive/folders/1VBXKuwLe3nA-qVshZowZ5LHz7afzM573?usp=sharing)
- Video titled "Part 1" and "Part 2" would be the demonstration video.


## Contributing
Contributions are welcome! Please fork the repository and submit a pull request with your improvements or fixes.

---

## License
This project is licensed under the MIT License.

---

## Contact
For any questions or feedback, please reach out at **[takshalm@gmail.com](mailto:your-email@example.com)**.
