
# 🦆 Turn your Raspberry Pi Pico W into a Rubber Ducky (BadUSB)

> ⚠️ **Legal Disclaimer**  
This project is intended for **educational** and **cybersecurity awareness** purposes only. I am not responsible for any misuse of this tool. Knowledge is power, but it comes with responsibility.

---

## 🔧 Steps to configure your BadUSB

### Step 1: Clone this repository

```bash
git clone https://github.com/yourusername/your-repo.git
```

---

### Step 2: Connect your Raspberry Pi Pico W in BOOTSEL mode

1. Hold down the **BOOTSEL** button on the Raspberry Pi Pico W.
2. While holding the button, plug it into your computer via USB.
3. The board should appear as an **external drive** on your file explorer.

![BOOTSEL Connection](./photos/bootsel.png)

---

### Step 3: Format the board

Copy the `format.uf2` file to the Raspberry Pi's memory. This file will format the chip to avoid future issues. Wait for the device to reboot automatically.

---

### Step 4: Install CircuitPython

Copy the `circuit_python.uf2` file to the board's memory and wait for it to reboot again.

---

### Step 5: Add the required libraries

Inside the repository, go to the `/lib` folder and copy the `adafruit_hid` folder into the `/lib` directory of the Raspberry Pi Pico W.

---

### Step 6: Copy the control file

Copy the [`code.py`](code.py) file to the **root directory** of the device (not inside any folder).

---

### Step 7: Add your payload

You can now use your Raspberry Pi Pico W as a keyboard. To run a script:

1. Take an example from the `/scripts` folder in the repository.
2. Rename it to `payload.dd`.
3. Move that file to the **root directory** of the device.

> ⚠️ As soon as the `payload.dd` file is placed in the root, it will execute automatically when the board is connected to a computer.

---

## ⌨️ Keyboard layout issues

By default, the Raspberry Pi will send keystrokes using a **US English** keyboard layout. This may cause errors if the target computer uses a different layout.

### Possible solutions:

#### 🅰️ Set the victim's keyboard layout to US English
The easiest solution if you have physical access to the computer.

#### 🅱️ Edit the `code.py` file

1. Uncomment lines 14 and 15.
2. Replace `LANG` with the target keyboard layout.
3. Comment out lines 9 and 10.
4. Make sure the corresponding language files are present in `adafruit_hid`.

> ⚠️ *This method hasn't worked for me after many attempts. If you manage to get it working, please contribute to the repository.*

#### 🅾️ Manually translate special characters
This is the method I use. Here's an example using a Spanish keyboard layout.

- Many characters will not be typed correctly. For example, typing `"` may result in `[`, and vice versa.
- The solution is to create a **test payload** as a reference dictionary:

```plaintext
STRING **!** " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ { | } ~
```

- Run that payload and see what is printed on the target machine. Use it to build your final payloads accordingly.

For instance, to type `https://google.com`, you would write:

```plaintext
STRING https**>&&google+com
```

---
