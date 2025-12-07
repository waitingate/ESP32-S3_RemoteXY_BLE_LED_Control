# ESP32-S3 RemoteXY BLE LED Control

A beginner-friendly project for controlling the **ESP32-S3-DevKitC-1U (N8R8)** using Bluetooth Low Energy (BLE) and the **RemoteXY** mobile app.

This project demonstrates how to:
1.  Configure PlatformIO for the **N8R8** (8MB Flash / 8MB PSRAM) module.
2.  Create a Mobile GUI (Switch & Indicators) without writing app code.
3.  Control the **Built-in RGB LED** (NeoPixel) and an **External LED** simultaneously.

---

## 📸 Project Demo

Here you can see the system in action.
* **Left:** The RemoteXY iOS App controlling the device.
* **Right:** The Physical ESP32-S3 and LED responding instantly.

| **App View (iOS)** | **Hardware View** |
| :---: | :---: |
| ![App Demo](img/app_demo_ios.gif) | ![Hardware Demo](img/hardware_demo.gif) |

---

## 🛠 Hardware Required

* **Development Board:** Espressif ESP32-S3-DevKitC-1U-N8R8 (or similar S3 board).
* **External LED:** Any standard LED (Red/Blue/Green).
* **Resistor:** 220Ω or 330Ω (for the external LED).
* **Smartphone:** Android or iOS with Bluetooth support.

## 🔌 Wiring & Pinout

| Component | ESP32-S3 Pin | Note |
| :--- | :--- | :--- |
| **Built-in RGB LED** | `GPIO 38` | Addressable NeoPixel (WS2812). Logic is handled by `neopixelWrite`. |
| **External LED (+)** | `GPIO 2` | Connect via resistor. |
| **External LED (-)** | `GND` | Common ground. |

> **Note:** Different ESP32-S3 boards may use different pins for the built-in RGB LED (e.g., GPIO 48). If yours does not light up, check your board's datasheet and update `PIN_RGB_BUILTIN` in `main.cpp`.

---

## 📂 Project File Structure

Here is an overview of the files in this project to help you navigate the code.

```text
ESP32-S3_RemoteXY_BLE_LED_Control/
├── docs/
│   └── remotexy_original_backup.cpp # Backup of the raw code from RemoteXY editor
├── img/
│   ├── app_demo_ios.gif             # Animation: App Walkthrough
│   ├── hardware_demo.gif            # Animation: Hardware Response
│   ├── remotexy_01_config.png       # Screenshot: Editor settings
│   ├── remotexy_02_editor.png       # Screenshot: GUI design
│   └── remotexy_03_code.png         # Screenshot: Generated code
├── platformio.ini                   # (CRITICAL) Project configuration file
├── src/
│   └── main.cpp                     # (MAIN) The actual source code (Setup, Loop)
└── README.md                        # This documentation file
````

-----

## 💻 Software Setup

### 1\. Prerequisites

  * [VSCodium](https://vscodium.com/) or VS Code.
  * [PlatformIO](https://platformio.org/) extension installed.
  * [RemoteXY App](https://remotexy.com/en/download/) installed on your phone.

### 2\. Installation

1.  **Clone the repo:**
    ```bash
    git clone [https://github.com/welton5532/ESP32-S3_RemoteXY_BLE_LED_Control.git](https://github.com/welton5532/ESP32-S3_RemoteXY_BLE_LED_Control.git)
    ```
2.  **Open in PlatformIO:**
    Open VSCodium, go to the PlatformIO Home, and click "Open Project". Select this folder.
3.  **Upload:**
    Connect your ESP32-S3 via USB. Click the **Right Arrow (→)** icon in the bottom status bar to Build and Upload.

### 3\. Usage

1.  Open the **RemoteXY** app on your phone.
2.  Enable Bluetooth on your phone.
3.  Tap `+` (New Device) -\> `Bluetooth BLE`.
4.  Select **"ESP32S3\_Switch"** from the list.
5.  Toggle the switch on the screen.
      * **ON:** External LED turns ON, Built-in RGB turns **GREEN**.
      * **OFF:** Both LEDs turn OFF.

-----

## 📱 Deep Dive: How to Edit the GUI

This project uses **RemoteXY**, a platform that allows you to create a mobile interface (GUI) for your microcontroller using a drag-and-drop editor.

If you want to modify this project (e.g., add a slider or change colors), follow these steps:

#### Step 1: Configuration

1.  Go to the [RemoteXY Editor](https://www.google.com/search?q=http://remotexy.com/en/editor/).
2.  Open the **Configuration** menu on the left.
3.  Select the following settings to match this project:
      * **Connection:** `Bluetooth BLE`
      * **Board:** `ESP32` (Select "ESP32 on board" or "ESP32-S3" if available)
      * **IDE:** `Arduino IDE`

![RemoteXY Configuration Settings](img/remotexy_01_config.png)

#### Step 2: Design Interface

1.  **Drag and Drop:** Use the left sidebar to drag components like *Switch*, *LED*, or *Text* onto the phone screen area.
2.  **Properties:** Click on any component to see its properties on the right.
3.  **Variable Names:** Note the variable name (e.g., `pushSwitch_01`). This is the name you will use in your C++ code to read the button state.

![RemoteXY GUI Editor](img/remotexy_02_editor.png)

#### Step 3: Get the Code

1.  Click the green **"Get Source Code"** button in the top right.
2.  A popup will appear containing the generated C++ code.
3.  **Copy only:**
      * The `#pragma pack...` block (The Configuration Array).
      * The `struct { ... } RemoteXY;` block (The Variables).
4.  **Paste** these into your `main.cpp`, replacing the existing configuration blocks.

![RemoteXY Generated Code](img/remotexy_03_code.png)

-----

## ⚠️ Connectivity & Power Modes (iOS vs Android)

Bluetooth behavior varies significantly between operating systems when "Power Saving" modes are active.

### 🍎 iPhone (iOS) Users

If you are using an iPhone, you may experience **connection failures** or **significant lag/delay** when toggling the LED.

  * **Reason:** iOS "Low Power Mode" aggressively restricts Bluetooth background activity and reduces data polling rates.
  * **Solution:** **Disable Low Power Mode** (`Settings` -\> `Battery`) for a smooth experience.

### 🤖 Android Users

  * **Status:** Generally **Stable**.
  * Android's "Battery Saver" mode typically allows BLE connections to function normally without noticeable lag for this type of application.

-----

## ⚙️ PlatformIO Configuration (N8R8)

This project is specifically configured for the **ESP32-S3-WROOM-1U-N8R8** module which uses **Octal SPI (OPI)** for PSRAM.

If you use a standard S3 board (N8R2 or no PSRAM), you may need to edit `platformio.ini`:

  * **Change:** `board_build.arduino.memory_type = qio_opi` ➔ `qio_qspi` (or remove it).

-----

## 📄 License

This project uses the RemoteXY library. Please refer to the [RemoteXY License](https://github.com/RemoteXY/RemoteXY-Arduino-library).
