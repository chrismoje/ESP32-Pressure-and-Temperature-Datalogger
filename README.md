# ESP32 Pressure & Temperature Data Logger

ESP32-based pressure and temperature data logger with a Python/Tkinter desktop GUI for configuration, live monitoring, data logging, calibration, and reporting.

## Current Project Status

- Firmware: `ESP32_32D_Pressure_Datalogger_v2.0.13.ino`
- ESP32: ESP32-32D N4 / ESP32-WROOM-32
- Pressure input: 2-wire 4–20 mA transmitter
- Pressure shunt: 150 ohm
- Pressure ADC: GPIO34
- Temperature: Type-K thermocouple + MAX31855
- LCD: 20x4 I2C, address `0x27`
- LCD SDA/SCL: GPIO8 / GPIO9
- MAX31855 CS/DO/CLK: GPIO10 / GPIO11 / GPIO12
- ADC resolution: 12 bit
- ADC attenuation: `ADC_11db`
- Current pressure configuration: 10,000 psi

> **Important:** The Communication Manager and proven hardware configuration are considered locked. Do not modify them unless explicitly required.

---

# 1. System Overview

The system consists of:

1. ESP32 data logger firmware
2. Python/Tkinter desktop GUI
3. 4–20 mA pressure transmitter
4. Type-K thermocouple
5. MAX31855 interface
6. 150 ohm pressure-current shunt
7. 20x4 I2C LCD
8. USB serial communication
9. Wi-Fi communication and REST API

The Python GUI provides the engineering interface for commissioning, configuration, monitoring, logging, calibration, and reporting.

---

# 2. Hardware Configuration

## Pressure Measurement

The 4–20 mA transmitter is powered from an external 24 VDC loop. The 150 ohm shunt converts current to voltage.

| Current | Voltage across 150 ohm |
|---:|---:|
| 4 mA | 0.600 V |
| 8 mA | 1.200 V |
| 12 mA | 1.800 V |
| 16 mA | 2.400 V |
| 20 mA | 3.000 V |

For a 10,000 psi transmitter:

| Current | Pressure |
|---:|---:|
| 4 mA | 0 psi |
| 8 mA | 2,500 psi |
| 12 mA | 5,000 psi |
| 16 mA | 7,500 psi |
| 20 mA | 10,000 psi |

Formula:

`Pressure = ((Current_mA - 4) / 16) x Sensor_Rating`

---

# 3. Fixed Hardware Addresses

These are the current proven hardware settings:

```text
Pressure ADC       GPIO34
LCD I2C Address    0x27
LCD Size           20 x 4
LCD SDA            GPIO8
LCD SCL            GPIO9

MAX31855 CS        GPIO10
MAX31855 DO        GPIO11
MAX31855 CLK       GPIO12

Pressure Shunt     150 ohm
ADC Resolution     12 bit
ADC Attenuation    ADC_11db
```

Do not change these during normal setup.

---

# 4. Software Required on a New Windows PC

Install the following:

1. Git
2. Python 3.12.x
3. Visual Studio Code
4. VS Code Python extension
5. VS Code PlatformIO IDE extension
6. Python package `pyserial`

The ESP32 toolchain and declared firmware libraries should be managed by PlatformIO when the project is configured as a PlatformIO project.

---

# 5. Install Git

Install Git for Windows.

Verify in the VS Code terminal:

```powershell
git --version
```

---

# 6. Install Python

Install Python 3.12.x.

During installation, enable:

```text
Add Python.exe to PATH
```

Verify:

```powershell
python --version
```

Expected:

```text
Python 3.12.x
```

---

# 7. Install Visual Studio Code

Install Visual Studio Code.

Then open VS Code and install these extensions:

- Python
- PlatformIO IDE

Restart VS Code if requested.

---

# 8. Clone the GitHub Project

In the VS Code terminal:

```powershell
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

Enter the project directory:

```powershell
cd <PROJECT_FOLDER>
```

Then open that folder in VS Code.

---

# 9. Python Virtual Environment

Create a virtual environment:

```powershell
python -m venv .venv
```

Activate it:

```powershell
.venv\Scripts\Activate.ps1
```

If PowerShell activation is unavailable, use:

```powershell
.venv\Scripts\activate.bat
```

Upgrade pip:

```powershell
python -m pip install --upgrade pip
```

---

# 10. Python Dependencies

The GUI uses Python standard-library modules such as:

- tkinter
- json
- csv
- datetime
- time
- threading
- queue
- socket
- urllib
- pathlib
- sqlite3
- os
- sys

These normally come with Python.

The external serial dependency is:

```powershell
python -m pip install pyserial
```

Verify:

```powershell
python -c "import serial; print(serial.__version__)"
```

If the repository contains `requirements.txt`, use:

```powershell
python -m pip install -r requirements.txt
```

Recommended minimum `requirements.txt`:

```text
pyserial
```

---

# 11. Verify Tkinter

Run:

```powershell
python -m tkinter
```

A small Tkinter test window should open.

If it does not, reinstall Python with Tcl/Tk support enabled.

---

# 12. PlatformIO / ESP32 Dependencies

PlatformIO should manage the ESP32 toolchain and declared libraries.

The firmware uses functionality associated with:

- ESP32 Arduino framework
- Wi-Fi
- WebServer
- Preferences / NVS
- Wire / I2C
- LiquidCrystal_I2C
- MAX31855
- ArduinoJson, if declared by the project

If the repository contains `platformio.ini`, use its existing library declarations and versions.

Do not replace a working `platformio.ini` with a generic example.

A typical ESP32 PlatformIO environment may look like:

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
```

Use the project's actual configuration when available.

---

# 13. Build Firmware

Open the ESP32 project in VS Code.

Using PlatformIO, select the correct environment and click **Build**.

Or from the terminal:

```powershell
pio run
```

The build must complete without errors before uploading.

---

# 14. Upload Firmware

Connect the ESP32 through USB and identify its COM port in Windows Device Manager.

Upload:

```powershell
pio run --target upload
```

Open the serial monitor:

```powershell
pio device monitor
```

Use the baud rate defined by the project.

---

# 15. Start the Python GUI

Activate the virtual environment:

```powershell
.venv\Scripts\Activate.ps1
```

Run the GUI using the project's actual main Python file, for example:

```powershell
python main.py
```

The filename may differ between GUI versions.

---

# 16. Communication Manager

The Communication Manager handles:

- Wi-Fi discovery
- Windows Wi-Fi association
- Password/network handling
- HTTP/API verification
- USB/Serial connectivity
- Connection state
- Machine ID and firmware identification
- Engineer/commissioning endpoint display

The Communication Manager is **LOCKED**.

Unrelated GUI changes must not modify its communication logic.

---

# 17. GUI Functional Areas

The application architecture includes:

- Communication Manager
- Configuration Manager
- Job Management
- Data Logging Manager
- Calibration Manager
- Report Manager
- Database Manager

Keep these functions separated when making future changes.

---

# 18. Configuration

Supported configuration includes:

- Machine ID
- Firmware version
- Pressure sensor rating
- Pressure sensor rating unit
- Pressure display unit
- Temperature display unit
- Logging interval
- Calibration zero offset
- Calibration gain
- Temperature calibration
- Calibration date
- Calibrated by
- Next calibration

Current pressure configuration:

```text
Sensor Rating       10000 psi
Rating Unit         psi
Display Unit        psi
Shunt               150 ohm
```

---

# 19. ADC Diagnostic

The firmware includes an ADC diagnostic command:

```json
{"type":"control","command":"ADC_DIAGNOSTIC","samples":100}
```

The diagnostic reports:

```text
ADC_PIN
ADC_RESOLUTION_BITS
ADC_ATTENUATION
SHUNT_OHMS
PRESSURE_RATING
PRESSURE_RATING_UNIT

RAW_ADC
CALIBRATED_mV
CALCULATED_mA
EQUIVALENT_PRESSURE

RAW_MIN
RAW_MAX
RAW_AVG
MV_MIN
MV_MAX
MV_AVG
CURRENT_MIN_mA
CURRENT_MAX_mA
CURRENT_AVG_mA
PRESSURE_MIN
PRESSURE_MAX
PRESSURE_AVG
```

The diagnostic does not change NVS configuration or pressure calibration.

---

# 20. ADC Characterization Before Calibration

Before applying pressure zero/gain calibration, characterize the ADC.

A precision DC voltage source such as a Fluke 5502E can be used.

Suggested voltage points:

```text
0.000 V
0.250 V
0.500 V
0.600 V
1.000 V
1.500 V
2.000 V
2.500 V
3.000 V
```

At every point:

1. Apply the known DC voltage.
2. Measure the actual voltage with a trusted meter.
3. Run the ADC diagnostic.
4. Record the ESP32 raw ADC and millivolt result.
5. Compare the ESP32 value with the reference voltage.

The 0.600 V point is especially important:

```text
0.600 V / 150 ohm = 4.000 mA
```

Do not compensate an unknown ADC error by changing pressure calibration parameters.

---

# 21. Pressure Calibration

Perform pressure calibration only after the electrical measurement path is verified.

For 10,000 psi:

```text
0 psi       -> 4 mA
2500 psi    -> 8 mA
5000 psi    -> 12 mA
7500 psi    -> 16 mA
10000 psi   -> 20 mA
```

A deadweight tester can establish known pressure points.

Record:

- Applied pressure
- Transmitter current
- Shunt voltage
- ESP32 ADC result
- Calculated pressure
- Error
- Calibration date
- Calibrated by

---

# 22. Data Logging

The GUI supports data logging and live monitoring.

Typical logged information includes:

- Date/time
- Machine ID
- Pressure
- Temperature
- Pressure current
- Operational status
- Job/test information

Keep test data associated with its job or test record.

---

# 23. Recommended Repository Structure

A possible repository structure is:

```text
ESP32-Pressure-Temperature-Datalogger/
|
+-- README.md
+-- requirements.txt
+-- .gitignore
|
+-- firmware/
|   +-- ESP32_32D_Pressure_Datalogger_v2.0.13/
|
+-- gui/
|   +-- Python GUI
|
+-- documentation/
|   +-- electrical/
|   +-- calibration/
|   +-- test-results/
|
+-- test/
    +-- ADC diagnostics/
```

Do not restructure a working project unnecessarily.

---

# 24. Git Workflow

Check status:

```powershell
git status
```

Create a development branch:

```powershell
git checkout -b development
```

Commit:

```powershell
git add .
git commit -m "Describe the change"
```

Push:

```powershell
git push -u origin development
```

Preserve previous verified versions rather than overwriting them.

---

# 25. Recommended .gitignore

```gitignore
# Python
.venv/
venv/
__pycache__/
*.py[cod]
*.pyo

# VS Code
.vscode/

# PlatformIO
.pio/
.piolibdeps/

# Logs
*.log

# Generated/test data
*.csv
*.tmp
```

Do not ignore configuration or database files if they are required parts of the product.

---

# 26. Troubleshooting

## `serial` cannot be imported

Run:

```powershell
python -m pip install pyserial
```

Then:

```powershell
python -c "import serial; print(serial.tools.list_ports.comports())"
```

## Tkinter is unavailable

Run:

```powershell
python -m tkinter
```

If it fails, reinstall Python with Tcl/Tk support.

## ESP32 upload fails

Check:

- USB cable
- COM port
- USB driver
- ESP32 boot mode
- PlatformIO board configuration
- Whether another application is using the COM port

## Pressure reading is incorrect

Check in this order:

1. 24 V loop supply
2. Transmitter wiring
3. 150 ohm shunt
4. Shunt voltage with a trusted meter
5. ESP32 ADC diagnostic
6. ADC characterization
7. Current calculation
8. Pressure conversion
9. Pressure calibration

Do not immediately alter calibration values.

---

# 27. Engineering Rule

Do not use calibration to hide an unknown hardware or ADC error.

Correct sequence:

```text
Verify hardware
      |
      v
Verify shunt voltage
      |
      v
Characterize ADC
      |
      v
Verify current calculation
      |
      v
Verify pressure conversion
      |
      v
Perform pressure calibration
      |
      v
Verify calibration independently
```

---

# 28. Current ADC Investigation

The current investigation is focused on ESP32 ADC accuracy.

With the transmitter connected at 0 psi, the diagnostic produced approximately:

```text
601–609 mV
4.007–4.060 mA
```

With the transmitter disconnected:

```text
RAW_ADC = 0
CALIBRATED_mV = 142 mV
CALCULATED_mA = 0.946667 mA
```

The next planned test is to apply known DC voltages from a precision source and compare the ESP32 ADC response against the actual voltage.

This characterization should be completed before changing pressure calibration parameters.

---

# 29. Fresh-PC Setup Checklist

- [ ] Install Git
- [ ] Install Python 3.12.x
- [ ] Install Visual Studio Code
- [ ] Install Python VS Code extension
- [ ] Install PlatformIO IDE extension
- [ ] Clone GitHub repository
- [ ] Create `.venv`
- [ ] Activate `.venv`
- [ ] Install `pyserial`
- [ ] Verify Tkinter
- [ ] Open ESP32 project
- [ ] Verify `platformio.ini`
- [ ] Build firmware
- [ ] Connect ESP32
- [ ] Verify COM port
- [ ] Upload firmware
- [ ] Open serial monitor
- [ ] Start Python GUI
- [ ] Verify USB communication
- [ ] Verify Wi-Fi communication
- [ ] Verify Machine ID/Firmware identification
- [ ] Verify pressure/temperature readings
- [ ] Run ADC diagnostic
- [ ] Perform ADC characterization before calibration

---

# 30. Versioning Policy

Firmware and GUI versions should be preserved.

For each change:

1. Start from the latest verified version.
2. Make the smallest required change.
3. Compile.
4. Test.
5. Assign a new version.
6. Preserve the previous working version.

The proven pressure acquisition path and Communication Manager should remain unchanged unless a specific engineering requirement requires modification.

---

# License

Add the project's selected license here.

Example:

```text
Copyright (c) Project Owner.
All rights reserved.
```
