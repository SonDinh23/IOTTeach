# IOTTeach

<div align="center">

<img src="resource\image\Logo.png" alt="System diagram" width="350">

### Modular IoT Learning & Test Kit

<p float="left">
<img src="resource/image/IOT1_V1.0top1.png" alt="IOTTeach Top View" width="40%"/>
<img src="resource/image/IOT1_V1.0top2.png" alt="IOTTeach Top Angle" width="40%"/>
</p>

<p float="left">
<img src="resource/image/IOT1_V1.0bottom1.png" alt="IOTTeach Bottom View" width="40%"/>
<img src="resource/image/IOT1_V1.0bottom2.png" alt="IOTTeach Bottom Angle" width="40%"/>
</p>

<sub>IOTTeach v1.0 — Top side (above) · Bottom side (below)</sub>

<br/><br/>

[![KiCad](https://img.shields.io/badge/Design-KiCad%209-blue?logo=kicad&logoColor=white)](https://www.kicad.org/)
[![Open Hardware](https://img.shields.io/badge/Open-Hardware-green?logo=opensourceinitiative&logoColor=white)](#license)
[![Education](https://img.shields.io/badge/Purpose-Education%20%26%20Test-orange)](#about)

</div>

---

## Table of Contents

| Section | Description |
|---------|-------------|
| About | Overview and intent |
| Hardware Overview | Technical summary |
| Key Features | Highlights and educational notes |
| Block Diagram | System block diagram |
| Getting Started | Quick start and assembly checklist |
| Lab Scenarios | Suggested exercises by level |
| License | License guidance |

---

## About

IOTTeach is a detachable plug-in kit designed for hands-on learning, quick hardware validation, and classroom labs. It exposes common peripherals (relays, RGB/status LEDs, buzzer) and standard 2.54 mm headers for power, analog, and digital lines so students and developers can prototype with any MCU or SBC without soldering a custom shield.

Design philosophy: Bring Your Own MCU — connect any microcontroller (ESP32, STM32, Arduino, Raspberry Pi Pico, etc.) using Dupont wires.

---

## Hardware Overview

| Component | Detail |
|:--------:|:------|
| Power | DC jack (DC-005) + terminal block → TPS54202 buck converter → 3.3V/5V rail |
| GPIO Headers | Digital 1×10 · Digital 1×8 · Analog 1×6 · Power header (pitch 2.54 mm) |
| LED Indicators | 2× WS2812B RGB, discrete status LEDs (green/yellow/red) |
| Audio | Piezo buzzer (CMT-8530S-SMT), PWM capable |
| Relays | 2× Omron G5NB SPST relays, driven via SS8050 transistors |
| Protection | SS34 Schottky diodes for flyback, decoupling capacitors and input protection |
| Project Files | KiCad project and production outputs under `hardware/IOT1_V1.0/production/` |

### Core Components (summary)

• TPS54202DDC — Buck converter (3.3V, up to ~2A)

• WS2812B (×2) — Addressable RGB LEDs for visual feedback

• Omron G5NB Relays (×2) — SPST relays for switching experiments (observe safety limits)

• CMT-8530S-SMT — Piezo buzzer for audible signals

• SS8050 (×3) — NPN transistors as relay drivers

• SS34 (×4) — Schottky diodes for flyback/protection

---

## Key Features

### Modular & MCU-Agnostic

- Plug-and-play design: wire any MCU to the headers — no soldering required.
- Standard 2.54 mm pitch makes connecting and swapping modules fast and reliable.

### Education-Focused Layout

- Clear, labeled headers and wide component spacing for easy probing with oscilloscopes and logic analyzers.
- Progressive learning path: LEDs → switches → buzzer → relays → RGB, enabling stepwise lab exercises.

### Fabrication-Ready

- Complete production artifacts included: BOM, pick-and-place, netlist and KiCad project files.
- Files are in `hardware/IOT1_V1.0/production/` for easy handoff to PCB fabs and assemblers.

---

## Block Diagram

```
                              ┌─────────────────────────────────────┐
                              │          IOTTeach Board             │
                              └─────────────────────────────────────┘
                                              │
        ┌───────────────────────────────────────────────────────────────────┐
        │                                                                   │
        ▼                                                                   ▼
┌───────────────┐                                               ┌───────────────┐
│  POWER INPUT  │                                               │   MCU SIDE    │
│───────────────│                                               │───────────────│
│ • DC Jack     │                                               │ • Digital I/O │
│ • Terminal    │──► TPS54202 ──► 3.3V/5V Rail ──►             │ • Analog In   │
│   Block       │       Buck                                    │ • Power       │
└───────────────┘                                               └───────┬───────┘
                                                                        │
        ┌───────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PERIPHERALS                                     │
├─────────────────┬─────────────────┬─────────────────┬───────────────────────┤
│   VISUAL        │   AUDIO         │   ACTUATORS     │   INPUTS              │
├─────────────────┼─────────────────┼─────────────────┼───────────────────────┤
│ • 2× WS2812B    │ • Piezo Buzzer  │ • 2× SPST Relay │ • 2× Tactile Switch   │
│ • Status LEDs   │   (PWM tone)    │   (5A/250VAC)   │                       │
│                 │                 │ • SS34 Flyback  │                       │
└─────────────────┴─────────────────┴─────────────────┴───────────────────────┘
```

---

## Getting Started

### Prerequisites

- KiCad 7+ to view or edit the PCB project
- Any MCU board (ESP32, Arduino, STM32 Nucleo, Raspberry Pi Pico, etc.)
- Dupont jumper wires (Female-Female or Male-Female)

### Quick Start

```powershell
# Clone repository
git clone https://github.com/SonDinh23/IOTTeach.git
cd IOTTeach

# Open the KiCad project:
# File → Open Project → hardware/IOT1_V1.0/IOT1_V1.0.kicad_pro
```

### Recommended Assembly Order

1. Power stage: TPS54202, inductor, input/output capacitors, SS34 diodes
2. Install pin headers (all GPIO/power headers)
3. Passive components (resistors, caps)
4. Semiconductors (transistors, diodes, LEDs)
5. Modules and larger components (relays, buzzer, WS2812B)
6. Connectors and switches (DC jack, terminal blocks)

### First Power-On Checklist

- Verify no short between VCC and GND before powering
- Apply input power via DC jack (7–12 V recommended)
- Measure output rail (3.3V or 5V depending on configuration)
- Confirm power indicator LED lights up
- Connect MCU and run a basic LED blink test

---

## Lab Scenarios (Suggested Exercises)

### Level 1 — Beginner

- LED Blink: control a status LED (digital output)
- Button Read: read a tactile switch (digital input, pull-up)
- Buzzer Tone: generate tones using PWM (basic PWM)

### Level 2 — Intermediate

- RGB Effects: drive WS2812B for color patterns (NeoPixel protocol)
- Relay Timer: implement a timed relay control routine (timers/state machine)
- LED Patterns: create multi-LED sequences and animations (arrays/loops)

### Level 3 — Advanced

- Alarm System: integrate buzzer, RGB, and relay for a simulated alarm
- Power Profiling: measure board current under different loads (instrumentation)
- Remote Control: connect an MCU with Wi-Fi/BLE to control relays (networking)

---

## License

This project is presented as Open Hardware. Please add a specific license (for example, CERN-OHL, TAPR-OHL, or CC-BY-SA) to the repository before distributing manufacturing files or derivative works.

---

## Thanks

Thank you for exploring IOTTeach. Contributions, documentation improvements, and lab material suggestions are welcome — open an issue or submit a pull request.

<div align="center">

**[Back to Top](#iotteach)**

</div>

| Tính năng | Mô tả |
|-----------|-------|
| **MCU Agnostic** | Tương thích ESP32, STM32, Arduino, Raspberry Pi Pico, v.v. |
| **Plug & Play** | Kết nối nhanh qua dây Dupont, không cần hàn |
| **Header chuẩn** | Pitch 2.54mm phổ biến, dễ tìm phụ kiện |

### 📚 Education-Focused

| Tính năng | Mô tả |
|-----------|-------|
| **Clear Layout** | Component spacing rộng, dễ đo đạc oscilloscope |
| **Labeled Headers** | Đánh dấu rõ ràng chức năng từng chân |
| **Gradual Learning** | Từ LED → Switch → Buzzer → Relay → RGB |

### �icing Fabrication-Ready

| Tài liệu | Đường dẫn |
|----------|-----------|
| **Schematic** | `hardware/IOT1_V1.0/IOT1_V1.0.kicad_sch` |
| **PCB Layout** | `hardware/IOT1_V1.0/IOT1_V1.0.kicad_pcb` |
| **3D Model** | `hardware/IOT1_V1.0/IOT1_V1.0.step` |
| **BOM** | `hardware/IOT1_V1.0/production/bom.csv` |
| **Pick & Place** | `hardware/IOT1_V1.0/production/positions.csv` |

---

## 📊 Block Diagram

```
                              ┌─────────────────────────────────────┐
                              │          IOTTeach Board             │
                              └─────────────────────────────────────┘
                                              │
        ┌───────────────────────────────────────────────────────────────────┐
        │                                                                   │
        ▼                                                                   ▼
┌───────────────┐                                               ┌───────────────┐
│  POWER INPUT  │                                               │   MCU SIDE    │
│───────────────│                                               │───────────────│
│ • DC Jack     │                                               │ • Digital I/O │
│ • Terminal    │──► TPS54202 ──► 3.3V/5V Rail ──►             │ • Analog In   │
│   Block       │       Buck                                    │ • Power       │
└───────────────┘                                               └───────┬───────┘
                                                                        │
        ┌───────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PERIPHERALS                                     │
├─────────────────┬─────────────────┬─────────────────┬───────────────────────┤
│   💡 VISUAL     │   🔊 AUDIO      │   ⚙️ ACTUATOR   │   🎛️ INPUT            │
├─────────────────┼─────────────────┼─────────────────┼───────────────────────┤
│ • 2× WS2812B    │ • Piezo Buzzer  │ • 2× SPST Relay │ • 2× Tactile Switch   │
│ • 2× LED Green  │   (PWM tone)    │   (5A/250VAC)   │                       │
│ • 1× LED Yellow │                 │ • SS34 Flyback  │                       │
│ • 3× LED Red    │                 │                 │                       │
└─────────────────┴─────────────────┴─────────────────┴───────────────────────┘
```

---

## 🚀 Getting Started

### Prerequisites

- **KiCad 7+** để xem/chỉnh sửa thiết kế
- **MCU board** bất kỳ (ESP32, Arduino Uno, STM32 Nucleo, etc.)
- **Dây Dupont** Female-Female hoặc Male-Female

### Quick Start

```bash
# 1. Clone repository
git clone https://github.com/SonDinh23/IOTTeach.git
cd IOTTeach

# 2. Mở project KiCad
# File → Open Project → hardware/IOT1_V1.0/IOT1_V1.0.kicad_pro
```

### Assembly Order (Khuyến nghị)

| Bước | Thành phần | Ghi chú |
|:----:|------------|---------|
| 1️⃣ | **Power Stage** | TPS54202, inductor L1, capacitors, SS34 diodes |
| 2️⃣ | **Headers** | Tất cả pin headers (J1-J14) |
| 3️⃣ | **Passives** | Resistors, capacitors còn lại |
| 4️⃣ | **Semiconductors** | Transistors SS8050, LEDs |
| 5️⃣ | **Modules** | Relays, buzzer, WS2812B |
| 6️⃣ | **Connectors** | DC jack, terminal blocks, switches |

### First Power-On Checklist

- [ ] Kiểm tra ngắn mạch giữa VCC-GND trước khi cấp nguồn
- [ ] Cấp nguồn qua DC jack (7-12V recommended)
- [ ] Đo điện áp output rail (3.3V hoặc 5V tùy config)
- [ ] LED power indicator sáng
- [ ] Kết nối MCU và test blink LED

---

## 🧪 Lab Scenarios

### 🟢 Level 1: Beginner

| Bài lab | Mô tả | Kỹ năng |
|---------|-------|---------|
| **LED Blink** | Điều khiển LED đơn ON/OFF | Digital Output |
| **Button Read** | Đọc trạng thái nút nhấn | Digital Input, Pull-up |
| **Buzzer Tone** | Tạo âm thanh với tần số khác nhau | PWM basics |

### 🟡 Level 2: Intermediate

| Bài lab | Mô tả | Kỹ năng |
|---------|-------|---------|
| **RGB Rainbow** | Hiệu ứng cầu vồng với WS2812B | NeoPixel protocol |
| **Relay Timer** | Bật/tắt relay theo thời gian | Timing, State machine |
| **Multi-LED Pattern** | Đèn chạy đuổi, hiệu ứng | Arrays, Loops |

### 🔴 Level 3: Advanced

| Bài lab | Mô tả | Kỹ năng |
|---------|-------|---------|
| **Smart Alarm** | Hệ thống báo động: buzzer + LED + relay | System integration |
| **Power Profiling** | Đo dòng tiêu thụ ở các chế độ | Instrumentation |
| **IoT Control** | Điều khiển relay qua WiFi/BLE | Networking |

---

## 📜 License

Dự án này được phát hành dưới dạng **Open Hardware**.

> ⚠️ Vui lòng thêm license cụ thể (CERN-OHL, TAPR-OHL, hoặc CC-BY-SA) trước khi phân phối thiết kế.

---

## 🙏 Thanks

Cảm ơn bạn đã quan tâm đến **IOTTeach**!

Mọi góp ý về thiết kế, tài liệu hướng dẫn, hoặc ý tưởng bài thực hành đều được hoan nghênh.

<div align="center">

**[⬆ Back to Top](#iotteach)**

</div>