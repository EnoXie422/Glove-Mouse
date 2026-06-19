# Glove Mouse: A Wearable Gesture-Based Input Device

A wearable glove that replaces a conventional computer mouse with natural hand motion. Built around an Arduino Leonardo, a 9-DOF inertial sensor (LSM9DS0), and a capacitive touch controller (MPR121), the device translates hand tilt into cursor movement and finger touches into mouse clicks.

> **Course project for BE202 — Sensors and Instrumentation**, Macau University of Science and Technology, Fall 2024.

---

## Demo

![demo](media/demo.mp4)

> If the video does not auto-play on GitHub, click [here to download](media/demo.mp4) (≈4 MB, 28 s).

The demo shows real-time cursor control: tilting the hand moves the pointer on screen, and tapping the fingertip touch pads triggers left / middle / right click.

---

## Motivation

Traditional mice require fine motor control on a flat surface, which can be uncomfortable for prolonged use and inaccessible for users with motor impairments. A glove-based gesture interface offers:

- **Accessibility** — usable without a desk or precise wrist control
- **Mobility** — no surface required; works while standing or walking
- **Intuitive interaction** — natural hand motion replaces an abstract input device

---

## How It Works

1. The **LSM9DS0** measures 3-axis acceleration of the hand.
2. The Arduino **clamps and maps** the acceleration on the X / Y axes to pixel displacements, after an initial 20-sample auto-calibration that establishes the user's resting hand pose.
3. The **MPR121** continuously scans 12 capacitive electrodes; finger contact on the designated pads is detected by a change in capacitance.
4. The Arduino Leonardo's native USB-HID support (via `Mouse.h`) lets the board appear to the host computer as a standard USB mouse — no driver or companion software needed.

```
Hand motion → LSM9DS0 (I²C) ─┐
                              ├─→ Arduino Leonardo ──USB HID──→ Computer
Finger touch → MPR121 (I²C) ─┘
```

---

## Hardware

| Component          | Role                                        |
|--------------------|---------------------------------------------|
| Arduino Leonardo R3| Microcontroller with native USB-HID         |
| Adafruit LSM9DS0   | 9-DOF IMU (accelerometer used for tilt)     |
| Adafruit MPR121    | 12-channel capacitive touch controller      |
| Conductive pads ×3 | Left / middle / right click electrodes      |
| Cotton glove       | Wearable substrate                          |

Full wiring diagram and photographs are in [`docs/Report_Glove_Mouse.pdf`](docs/Report_Glove_Mouse.pdf).

---

## Software

- **Language:** C/C++ (Arduino)
- **Libraries:** `Adafruit_LSM9DS0`, `Adafruit_MPR121`, `Adafruit_Sensor`, `Wire`, `SPI`, `Mouse`

### Build & Upload

1. Install the [Arduino IDE](https://www.arduino.cc/en/software).
2. In **Library Manager**, install `Adafruit LSM9DS0`, `Adafruit MPR121`, and `Adafruit Unified Sensor`.
3. Open [`code/Glove_Mouse_Code.ino`](code/Glove_Mouse_Code.ino).
4. Select **Board → Arduino Leonardo**, choose the correct COM port, and upload.
5. Wear the glove and keep the hand still for ~1 second after power-up; the firmware auto-calibrates the resting orientation.

---

## Repository Structure

```
.
├── code/        Arduino firmware
├── docs/        Report, proposal, and presentation slides
├── media/       Demo video and images
└── hardware/    Component list and wiring notes
```

---

## Limitations & Future Work

- The device is **tethered via USB**. A Bluetooth HID version (e.g., on an ESP32 or nRF52840) would make it truly wearable.
- Drift accumulates because only the accelerometer is used for position; **sensor fusion** with the LSM9DS0's gyroscope and magnetometer (e.g., a Madgwick filter) would yield more stable cursor control.
- A **machine-learning gesture classifier** could expand the input vocabulary beyond click and motion — for example, recognizing pinch, swipe, or sign-language characters.

---

## Documentation

- 📄 [Full Project Report (PDF)](docs/Report_Glove_Mouse.pdf)
- 📊 [Final Presentation (PDF)](docs/Presentation_Glove_Mouse.pdf)
- 📝 [Original Proposal (PDF)](docs/Proposal_Glove_Mouse.pdf)

---

## Authors

- Xie, Wen-hao
- Shi, Zhi-bo

BSAS BEIE, Macau University of Science and Technology · December 2024

---

## License

Released under the [MIT License](LICENSE). The project may be reused freely for academic and personal purposes; please cite the authors if it influences your work.

## Acknowledgments

This project was developed as a course assignment based on the open-source Instructables tutorial [*Gesture Mouse — Controlling Computer With a Glove*](https://www.instructables.com/Gesture-Mouse-Controling-Computer-With-a-Glove/). The reference implementation provided the initial firmware skeleton (sensor initialization, calibration loop, and HID dispatch). Our work adapted the design to a different hardware build, restructured the code, fixed several bugs (uninitialized cursor delta, malformed `DEBUG` guards, missing function terminator), renamed misleading constants, and documented the system in a full project report.

## References

1. Han, H., & Yoon, S. W. (2019). *Gyroscope-Based Continuous Human Hand Gesture Recognition for Multi-Modal Wearable Input Device for Human Machine Interaction.* **Sensors, 19(11), 2562.** [doi:10.3390/s19112562](https://doi.org/10.3390/s19112562)
2. Behera, A., et al. (2023). *Wearable Glove Mouse.* **IJARIIE, 9(3), 570–580.**
3. [LSM9DS0 datasheet](https://www.alldatasheet.com/view.jsp?Searchword=LSM9DS0)
4. [MPR121 datasheet](https://www.alldatasheet.com/view.jsp?Searchword=MPR121)
