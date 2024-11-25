# Adaptive-Digital-Intelligent-Task-Integrator
## Team members:
- Takshal Modi (202151170)
- Tanish Nagrani (202151190)
- Varun Vilvadrinath (202151195)
- Shubham Mehatre (202152323)
- Rushabh Babre (202152334)

## Project Description
This project explores the integration of conversational AI and home automation systems using cost-effective and compact hardware solutions. Utilizing two Raspberry Pi devices, the system combines voice-based control with intelligent automation. 

- **Node 1** is configured to handle audio input and output via a Bluetooth headset.
- **Node 2** hosts the Home Assistant platform to manage automation tasks.
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

## Installation Steps

### Step 1: Pull and Run Docker Images
1. Pull the following Docker images:
   - `rhasspy/wyoming-whisper`
   - `rhasspy/wyoming-piper:1.3.0`
2. **Why use these images?**
   - **Whisper**: Provides accurate and efficient speech-to-text conversion.
   - **Piper**: Offers reliable text-to-speech functionality.

   Use the following commands:
   ```bash
   docker pull rhasspy/wyoming-whisper
   docker pull rhasspy/wyoming-piper:1.3.0
   ```

3. Expose the ports on which the Docker images are running:
   ```bash
   sudo ufw allow <portnumber>/tcp
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
   - Add the following line:
     ```bash
     Environment="OLLAMA_HOST=0.0.0.0:11434"
     ```
   - Reload the system daemon and restart Ollama:
     ```bash
     sudo systemctl daemon-reload
     sudo systemctl restart ollama
     ```

---

### Step 3: Set Up Raspberry Pi OS
1. Install **Raspberry Pi Imager** from [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/).
2. Use the imager to install **Ubuntu Desktop 22.04.5 LTS (64-bit)**:
   - Go to: `Other general-purpose OS > Ubuntu > Ubuntu Desktop 22.04.5 LTS`.
3. Flash the OS to an SD card and insert it into the Raspberry Pi.

---

### Step 4: Install PipeWire
1. SSH into the Raspberry Pi once it is running:
   ```bash
   ssh <username>@<raspberry_pi_ip>
   ```
2. Install **PipeWire** by following the instructions at:
   [How to Install PipeWire on Ubuntu](https://linuxconfig.org/how-to-install-pipewire-on-ubuntu-linux).

---

### Step 5: Connect Bluetooth Device for Audio I/O
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

### Step 6: Test Audio on Node
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

### Step 7: Set Up Wyoming Satellite
Follow the detailed guide to set up Wyoming Satellite on your Raspberry Pi:
[Setup a Raspberry Pi Zero 2 W as a Wyoming Satellite](https://www.slacker-labs.com/setup-a-raspberry-pi-zero-2-w-as-a-wyoming-satellite/).

---

## Future Enhancements
1. Real-time data retrieval models
2. Edge computing optimization
3. Scalability for larger systems

---

## Contributing
Contributions are welcome! Please fork the repository and submit a pull request with your improvements or fixes.

---

## License
This project is licensed under the MIT License.

---

## Contact
For any questions or feedback, please reach out at **[takshalm@gmail.com](mailto:your-email@example.com)**.
