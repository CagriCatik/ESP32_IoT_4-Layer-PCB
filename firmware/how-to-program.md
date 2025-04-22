# Getting Started with ESP32 Programming: Arduino, ESP‑IDF & MicroPython

## 1. Using the Arduino IDE

### a. Install the IDE and ESP32 Core
1. **Download & install** the Arduino IDE (Windows/macOS/Linux) from the [Arduino website](https://www.arduino.cc/en/software).  
2. **Add ESP32 support**:  
   - Open **File → Preferences**.  
   - In “Additional Boards Manager URLs” paste:  
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Click **OK**.  
   - Then go to **Tools → Board → Boards Manager**, search for “esp32” and install “esp32 by Espressif Systems”.

### b. Hardware Setup
- Connect your ESP32 board to your PC via USB.  
- Note the COM port (Windows) or /dev/ttyUSB* (Linux/macOS).

### c. Blink Example
1. **Select board**:  
   **Tools → Board → ESP32 Dev Module** (or your specific model).  
2. **Select port**:  
   **Tools → Port → (your ESP32 port)**.  
3. **Load example**:  
   **File → Examples → 01.Basics → Blink**.  
4. **Modify LED pin** if needed (many boards have on‑board LED on GPIO 2):  
   ```cpp
   const int LED_PIN = 2;
   void setup() {
     pinMode(LED_PIN, OUTPUT);
   }
   void loop() {
     digitalWrite(LED_PIN, HIGH);
     delay(500);
     digitalWrite(LED_PIN, LOW);
     delay(500);
   }
   ```  
5. **Upload**: click the → arrow. The IDE will compile and flash the code.

---

## 2. Using ESP‑IDF (Espressif IoT Development Framework)

ESP‑IDF gives you full access to the ESP32’s features in C/C++.

### a. Install Prerequisites
- **Windows**: Use the ESP‑IDF Tools Installer.  
- **macOS/Linux**:  
  ```bash
  sudo apt-get install git wget make flex bison gperf python3 python3-pip cmake ninja-build ccache
  ```
- **Get ESP‑IDF**:  
  ```bash
  git clone --recursive https://github.com/espressif/esp-idf.git
  cd esp-idf
  ./install.sh         # installs Python requirements, toolchain
  . ./export.sh        # (or use export.bat on Windows)
  ```

### b. Create & Build a Project
1. **Start a new project** from the example templates:
   ```bash
   cp -r $IDF_PATH/examples/get-started/blink my_blink
   cd my_blink
   ```
2. **Configure** (e.g. choose serial port):
   ```bash
   idf.py menuconfig
   ```
3. **Build & flash**:
   ```bash
   idf.py build
   idf.py -p /dev/ttyUSB0 flash
   ```
4. **Monitor** serial output:
   ```bash
   idf.py -p /dev/ttyUSB0 monitor
   ```

---

## 3. Using MicroPython

MicroPython lets you script the ESP32 with Python.

### a. Flash MicroPython Firmware
1. **Download** the latest `.bin` for ESP32 from [micropython.org/download](https://micropython.org/download/esp32/).  
2. **Install esptool**:
   ```bash
   pip install esptool
   ```
3. **Erase & flash**:
   ```bash
   esptool.py --port /dev/ttyUSB0 erase_flash
   esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash -z 0x1000 esp32-*.bin
   ```

### b. Upload & Run Scripts
- Use a serial‐terminal tool (e.g. PuTTY, screen) or an IDE like **Thonny** or **uPyCraft**.  
- Connect at 115200 bps— you should see the MicroPython REPL (`>>>`).  
- Create a simple blink in `main.py`:
  ```python
  from machine import Pin
  from time import sleep

  led = Pin(2, Pin.OUT)
  while True:
      led.on()
      sleep(0.5)
      led.off()
      sleep(0.5)
  ```
- Save and reset; your LED should blink.

---

## Next Steps & Resources

- **GPIO & peripherals**: Learn to read buttons, drive motors, use I²C/SPI sensors.  
- **Wi‑Fi & networking**: Connect to Wi‑Fi, run a web‑server, MQTT clients.  
- **FreeRTOS** (built into ESP‑IDF) for multitasking.  
- **Tools & libraries**: PlatformIO (an alternative IDE/CLI), Async TCP libraries, OTA updates.

**Official docs & tutorials**  
- ESP‑IDF Programming Guide: https://docs.espressif.com/projects/esp-idf  
- Arduino‑ESP32 GitHub: https://github.com/espressif/arduino-esp32  
- MicroPython on ESP32: https://docs.micropython.org/en/latest/esp32/tutorial/intro.html

