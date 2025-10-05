# Turn Your Raspberry Pi Pico (or Pico W) into a Rubber Ducky (BadUSB)

🦆 This guide combines instructions from the original [pico-ducky](https://github.com/dbisu/pico-ducky) project for Raspberry Pi Pico and adaptations for Raspberry Pi Pico W, providing a unified setup process. It turns your Pico into a USB device that emulates a keyboard to run pre-defined scripts (payloads), mimicking a "BadUSB" like the Hak5 Rubber Ducky.

⚠️ **Legal Disclaimer**  
This project is intended for educational and cybersecurity awareness purposes only. I am not responsible for any misuse of this tool. Knowledge is power, but it comes with responsibility. Always comply with local laws and obtain proper authorization before testing on any system.

## Basic Concept
When you connect a typical USB device with data, the system recognizes it as storage. Here, we'll modify your Raspberry Pi Pico (or Pico W) so it's recognized as a keyboard, allowing it to execute pre-saved commands automatically.

## Prerequisites
- Raspberry Pi Pico or Pico W.
- USB cable.
- Computer with file explorer access.
- Text editor for editing payloads.

## 🔧 Steps to Configure Your BadUSB

Follow these steps sequentially. Where applicable, you'll have two alternatives for obtaining files:  
- **Option A: From Original Sources** – Download from official links (recommended for verification and latest versions).  
- **Option B: Direct Downloads** – Use pre-provided files from this repository (faster, but verify integrity).

### Step 1: Prepare Your Pico in BOOTSEL Mode
Hold down the BOOTSEL button on your Raspberry Pi Pico (or Pico W) while plugging it into your computer via USB. It should appear as an external drive (named something like "RPI-RP2").

### Step 2: Format the Board (Optional but Recommended)
This clears any existing data to avoid issues.  
- **Option A (Original Source):** Download `flash_nuke.uf2` from [Mega.nz](https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html#resetting-flash-memory).  
- **Option B (Direct):** Use `format.uf2` from the `/root` folder in this repository.  

Copy the `.uf2` file to the Pico's drive. Wait for it to reboot automatically (the drive will disappear and reappear).

### Step 3: Install CircuitPython Firmware
This enables Python scripting on the Pico.  
- **Option A (Original Source):** Download the `adafruit-circuitpython-raspberry_pi_pico2_w-en_US-10.0.0.uf2` file for your board from [CircuitPython.org](https://circuitpython.org/board/raspberry_pi_pico/) (for Pico) or [CircuitPython.org Pico W](https://circuitpython.org/board/raspberry_pi_pico_w/).  
- **Option B (Direct):** Use `adafruit-circuitpython-raspberry_pi_pico2_w-en_US-10.0.0.uf2`  from the `/` folder in this repository.  

Copy the `.uf2` file to the Pico's drive. Wait for reboot. The drive should now be named "CIRCUITPY".

### Step 4: Add the HID Library
This library allows the Pico to act as a keyboard (Human Interface Device).  
- **Option A (Original Source):** Download `adafruit-circuitpython-bundle-6.x-mpy-20210130.zip` (or latest) from [Adafruit GitHub](https://github.com/adafruit/Adafruit_CircuitPython_Bundle/releases). Unzip, navigate to the `lib` folder, and copy the `adafruit_hid` folder.  

From there, copy the following items into the /lib folder on the "CIRCUITPY" drive:

    adafruit_hid (folder)

    adafruit_debouncer.mpy

    adafruit_ticks.mpy

    asyncio (folder)

    adafruit_wsgi (folder)

Then, copy these files to the root directory of the "CIRCUITPY" drive:

    boot.py

    duckyinpython.py

    code.py

    webapp.py

    wsgiserver.py

All of these files are available in the official Adafruit bundle or can be found in the original project repository.


- **Option B (Direct):** Copy the `adafruit_hid` folder from the `/lib` folder in this repository.  

Paste the `adafruit_hid` folder into the `/lib` directory on the "CIRCUITPY" drive.

    adafruit_hid (folder)

    adafruit_debouncer.mpy

    adafruit_ticks.mpy

    asyncio (folder)

    adafruit_wsgi (folder)

Then, copy the following files from the repository to the root of the "CIRCUITPY" drive:

    boot.py

    duckyinpython.py

    code.py

    webapp.py

    wsgiserver.py

### Step 5: Install the Ducky Script Interpreter
This script (based on pico-ducky) interprets and runs payloads.  
- **Option A (Original Source):** Download `pico-ducky-main.zip` from [dbisu/pico-ducky](https://github.com/dbisu/pico-ducky). Unzip and copy `duckyinpython.py`.  
- **Option B (Direct):** Use `duckyinpython.py` from the `/` folder in this repository.  

Copy the file to the root of the "CIRCUITPY" drive. Delete any existing `code.py` (if present) and rename `duckyinpython.py` to `code.py`.  
**Note:** The Pico will now auto-run `code.py` on connection, making it a "BadUSB".

### Step 6: Add a Payload Script
Payloads are scripts that run as keyboard inputs (e.g., opening apps, typing commands). For examples:  
- **Option A (Original Sources):** Browse payloads from [Hak5 USB Rubber Ducky Payloads](https://github.com/hak5/usbrubberducky-payloads).   
- **Option B (Direct):** Use example payloads from the `/scripts` folder in this repository (e.g., `rickroll.dd`). Rename to `payload.dd` if needed.  

Copy `payload.dd` to the root of the "CIRCUITPY" drive.  
⚠️ **Warning:** The payload will execute automatically when the Pico is connected! Test on a safe machine.

### Step 7: Testing and Running
- Disconnect and reconnect the Pico to a target computer. It should emulate keystrokes from the payload.  
- For editing or resetting: Enter BOOTSEL mode (hold BOOTSEL while connecting). The drive appears as "RPI-RP2". You can delete files or reformat as in Step 2.

## ⌨️ Keyboard Layout Issues
By default, payloads use US English layout. If the target uses another (e.g., Spanish), characters may mismatch. Solutions:  

🅰️ **Change Target Layout:** Set the target's keyboard to US English (if accessible).  

🅱️ **Edit code.py for Layout:** Uncomment lines 14-15 in `code.py`, replace "LANG" with your layout (e.g., "es" for Spanish). Ensure matching files are in `adafruit_hid/keyboard_layout_*.py`. Comment out lines 9-10.  
**Note:** This may require testing; contributions welcome if it works for you.  

🅾️ **Manual Character Translation:** Create a test payload like:  
```
STRING **!** " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ { | } ~
```  
Run it on the target, note mismatches, and adjust your payloads (e.g., for Spanish, "https://google.com" might become "https**>&&google+com").

## Additional Resources
- Original Hak5 Rubber Ducky: [Hak5 Store](https://hak5.org/collections/sale/products/usb-rubber-ducky).  
- Pico-Ducky Repo: [dbisu/pico-ducky](https://github.com/dbisu/pico-ducky).  
- Payload Library: [hak5/usbrubberducky-payloads](https://github.com/hak5/usbrubberducky-payloads).  

If you encounter issues, check the console output or contribute to this repo!

NOTE: Important — this repository only works with the Raspberry Pi Pico W version 2

