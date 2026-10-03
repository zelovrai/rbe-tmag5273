# RBE TMAG5273 Qwiic Module

![Status](https://img.shields.io/badge/Status-Active-success)
![License](https://img.shields.io/badge/License-MIT-blue)
![Hardware](https://img.shields.io/badge/Hardware-KiCad-red)
![Interface](https://img.shields.io/badge/Interface-I2C%20%7C%20Qwiic-brightgreen)

A compact, open-source breakout board for the **Texas Instruments TMAG5273** 3D Hall-effect sensor, featuring **Qwiic / STEMMA QT** connectivity. Designed by **Reka Bentuk Elektronika (RBE)**, this module makes it easy to integrate 3-axis magnetic field sensing, angle calculation, and proximity detection into your microcontroller projects without the hassle of soldering tiny SMD components.

<!-- TODO: Upload your 3D render to the repository and update the link below -->
<!-- ![3D Render of RBE TMAG5273](docs/images/rbe-tmag5273_3d.png) -->
*(Replace the image link above with your actual 3D render stored in the repository)*

## 📖 Table of Contents
- [Features](#-features)
- [Hardware Overview](#-hardware-overview)
- [Pinout & Configuration](#-pinout--configuration)
- [Getting Started](#-getting-started)
- [Repository Structure](#-repository-structure)
- [Manufacturing & Assembly](#-manufacturing--assembly)
- [Contributing](#-contributing)
- [License](#-license)
- [Acknowledgments](#-acknowledgments)

## ✨ Features

*   **Sensor:** TI TMAG5273 (Low-power, linear 3D Hall-effect sensor).
*   **Interface:** I2C (Qwiic / STEMMA QT compatible).
*   **Connectivity:** 2x JST-SH 4-pin connectors (In/Out) for easy daisy-chaining.
*   **Breakout Headers:** Standard 2.54mm pitch headers for breadboard or direct wiring access.
*   **Interrupt Pin:** Dedicated `INT` pin broken out for configuring hardware interrupts.
*   **Operating Voltage:** 3.3V logic and power (Do not use 5V logic without level shifting).
*   **Mounting:** 4x M2/M2.5 mounting holes for secure installation.

## 🔌 Hardware Overview

The RBE TMAG5273 is designed with flexibility in mind. It features two Qwiic connectors for daisy-chaining multiple sensors on the same I2C bus, as well as dual 4-pin 2.54mm headers for traditional breadboard prototyping. 

### Pinout & Configuration

The module features two breakout header rows for flexible wiring options.

#### Top Header (I2C & Power)
| Pin | Label | Description |
| :--- | :--- | :--- |
| 1 | **GND** | Ground |
| 2 | **3V3** | Power Supply (3.3V) |
| 3 | **SDA** | I2C Data |
| 4 | **SCL** | I2C Clock |

#### Bottom Header (Power & Interrupt)
| Pin | Label | Description |
| :--- | :--- | :--- |
| 1 | **GND** | Ground |
| 2 | **GND** | Ground |
| 3 | **3V3** | Power Supply (3.3V) |
| 4 | **INT** | Interrupt Output (Active Low) |

#### Qwiic Connectors
*   **Left & Right:** JST-SH 4-pin (GND, 3.3V, SDA, SCL). Allows daisy-chaining with other Qwiic modules.

## 🚀 Getting Started

### Hardware Setup
1.  Connect the RBE TMAG5273 to your microcontroller using a standard Qwiic/STEMMA QT cable.
2.  Ensure your microcontroller operates at **3.3V logic**. If using a 5V board (like an Arduino Uno), use a logic level shifter on the SDA and SCL lines.
3.  The default I2C address for the TMAG5273 is typically `0x35`, but this can be changed via software configuration.

### Software Examples

#### Arduino IDE
Ensure you have the appropriate TMAG5273 library installed (e.g., from TI or SparkFun).

```cpp
#include <Wire.h>
#include "TMAG5273.h" // Replace with your actual library header

TMAG5273 sensor;

void setup() {
  Serial.begin(115200);
  Wire.begin();
  
  // Initialize sensor
  if (!sensor.begin()) {
    Serial.println("TMAG5273 sensor not detected. Check wiring!");
    while (1);
  }
  Serial.println("TMAG5273 sensor initialized.");
}

void loop() {
  // Read 3-axis magnetic field data
  float x = sensor.getX();
  float y = sensor.getY();
  float z = sensor.getZ();
  
  Serial.print("X: "); Serial.print(x);
  Serial.print(" | Y: "); Serial.print(y);
  Serial.print(" | Z: "); Serial.println(z);
  
  delay(100);
}
```

#### Raspberry Pi (Python)
Using the Adafruit CircuitPython library for the TMAG5273:

```python
import time
import board
import adafruit_tmag5273

i2c = board.I2C()  # uses board.SCL and board.SDA
sensor = adafruit_tmag5273.TMAG5273(i2c)

while True:
    print("X: %0.2f, Y: %0.2f, Z: %0.2f" % (sensor.magnetic[0], sensor.magnetic[1], sensor.magnetic[2]))
    time.sleep(0.1)
```

## 📂 Repository Structure

*   `/bom` - Contains the Interactive HTML BOM (`ibom.html`) for easy assembly and part sourcing.
*   `/library` - Custom KiCad libraries used in this project.
    *   `/3dmodels` - 3D STEP/IGES files for the sensor and connectors.
    *   `/footprints` - KiCad footprint libraries.
    *   `/symbols` - KiCad schematic symbols.
*   `*.kicad_pcb`, `*.kicad_pro`, `*.kicad_sch` - The main KiCad project files (Schematic and PCB layout).
*   `fp-lib-table`, `sym-lib-table` - KiCad library tables to ensure project portability.
*   `.gitignore` - Standard Git ignore file.

## 🛠️ Manufacturing & Assembly

To manufacture this PCB yourself:
1.  Open the project in **KiCad** (v6 or v7 recommended).
2.  Review the schematic and PCB layout to ensure it meets your requirements.
3.  Generate Gerber and Drill files (or use the provided KiCad files directly with your manufacturer like JLCPCB, PCBWay, or OSHPark).
4.  Use the `ibom.html` file in the `/bom` folder to easily source components and assist with SMD assembly.

**Recommended PCB Specs:**
*   Layers: 2
*   Thickness: 1.6mm
*   Surface Finish: HASL (Lead-free) or ENIG
*   Copper Weight: 1oz

## 🤝 Contributing

Contributions are always welcome! If you find a bug in the PCB design, want to improve the documentation, or add example code, please open an Issue or submit a Pull Request.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📜 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details. You are free to modify, manufacture, and distribute this hardware, provided you retain attribution.

## 🙏 Acknowledgments

*   [Texas Instruments](https://www.ti.com/) for the TMAG5273 sensor.
*   [KiCad](https://kicad.org/) for the open-source EDA tool.
*   [SparkFun](https://www.sparkfun.com/) and [Adafruit](https://www.adafruit.com/) for the Qwiic/STEMMA QT standard.

---
Made with ❤️ by [Zelo Vrai](https://github.com/zelovrai) at **Reka Bentuk Elektronika (RBE)**.
