# RBE-TMAG5273 Qwiic Module

![Status](https://img.shields.io/badge/Status-Active-success)
![License](https://img.shields.io/badge/License-MIT-blue)
![Hardware](https://img.shields.io/badge/Hardware-KiCad-red)
![Interface](https://img.shields.io/badge/Interface-I2C%20%7C%20Qwiic-brightgreen)

A compact, open-source breakout board for the **Texas Instruments TMAG5273** 3D Hall-effect sensor, featuring **Qwiic / STEMMA QT** connectivity. This module is designed to make it easy to integrate 3-axis magnetic field sensing into your microcontroller projects without the hassle of soldering tiny SMD components.

<!-- TODO: Upload your 3D render to the repository and update the link below -->
<!-- ![3D Render of RBE-TMAG5273](docs/images/rbe-tmag5273_3d.png) -->

## ✨ Features

*   **Sensor:** TI TMAG5273 (Low-power, linear 3D Hall-effect sensor).
*   **Interface:** I2C (Qwiic / STEMMA QT compatible).
*   **Connectivity:** 2x JST-SH 4-pin connectors (In/Out) for easy daisy-chaining.
*   **Breakout Headers:** Standard 2.54mm pitch headers for breadboard or direct wiring access.
*   **Interrupt Pin:** Dedicated `INT` pin broken out for configuring hardware interrupts.
*   **Operating Voltage:** 3.3V logic and power.
*   **Mounting:** 4x M2/M2.5 mounting holes.

## 🔌 Pinout & Hardware Configuration

The module features two breakout header rows for flexible wiring options.

### Top Header (I2C & Power)
| Pin | Label | Description |
| :--- | :--- | :--- |
| 1 | **GND** | Ground |
| 2 | **3V3** | Power Supply (3.3V) |
| 3 | **SDA** | I2C Data |
| 4 | **SCL** | I2C Clock |

### Bottom Header (Power & Interrupt)
| Pin | Label | Description |
| :--- | :--- | :--- |
| 1 | **GND** | Ground |
| 2 | **GND** | Ground |
| 3 | **3V3** | Power Supply (3.3V) |
| 4 | **INT** | Interrupt Output (Active Low) |

### Qwiic Connectors
*   **Left & Right:** JST-SH 4-pin (GND, 3.3V, SDA, SCL). Allows daisy-chaining with other Qwiic modules.

## 📂 Repository Structure

*   `/bom` - Contains the Interactive HTML BOM (`ibom.html`) for easy assembly and part sourcing.
*   `/library` - Custom KiCad libraries used in this project.
    *   `/3dmodels` - 3D STEP/IGES files for the sensor and connectors.
    *   `/footprints` - KiCad footprint libraries.
    *   `/symbols` - KiCad schematic symbols.
*   `*.kicad_pcb`, `*.kicad_pro`, `*.kicad_sch` - The main KiCad project files.
*   `fp-lib-table`, `sym-lib-table` - KiCad library tables to ensure project portability.

## 🚀 Getting Started

### Hardware Requirements
*   A microcontroller with I2C support (Arduino, ESP32, Raspberry Pi, etc.).
*   A Qwiic/STEMMA QT cable.
*   This RBE-TMAG5273 module.

### Example Code (Arduino)
Ensure you have the appropriate TMAG5273 library installed (e.g., from TI or SparkFun).

```cpp
#include <Wire.h>
#include "TMAG5273.h" // Replace with your actual library header

TMAG5273 sensor;

void setup() {
  Serial.begin(115200);
  Wire.begin();
  
  // Initialize sensor (default I2C address is usually 0x35)
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
