
## **Wireless Technologies**  
- **Bluetooth Mesh:** A networking protocol for Bluetooth devices enabling communication in a many-to-many topology. Suitable for smart lighting, asset tracking, and IoT applications.  
- **NFC (Near-Field Communication):** A short-range communication protocol used for contactless payments, data transfer, and authentication.  
- **Thread:** A low-power, IPv6-based networking protocol designed for IoT devices, offering mesh networking capabilities.  
- **Zigbee:** A low-power wireless communication protocol for applications such as home automation, smart energy, and healthcare.  

---

## **Standards and Ratings**  
- **IPX8:** Indicates water resistance, specifically capable of withstanding continuous submersion in water beyond 1 meter.  
- **Sil2:** A Safety Integrity Level (SIL) standard ensuring a low probability of failure for safety-critical systems.  
- **MBAN (Medical Body Area Network):** A wireless network protocol designed for wearable and implantable medical devices.  

---

## **Package Types**  
- **BGA (Ball Grid Array):** A surface-mount packaging for ICs with solder balls on the bottom, providing high pin density and performance.  
- **QFN (Quad Flat No-Lead):** A compact IC package with leads on the edges, suitable for high-frequency and thermal performance applications.  

---

## **Numeric Comparisons**  
- **nRF HAL (Hardware Abstraction Layer):** Simplifies interaction with hardware peripherals like GPIO, UART, and timers on nRF chips.  
- **BLE Drivers:** Manage Bluetooth Low Energy operations like advertising, connection handling, and data transfer via characteristics.  
- **Device Drivers (e.g., I2C/SPI):** Facilitate communication with external peripherals like sensors, potentiostats, or displays.  

---

## **Integrated Development Environments (IDEs)**  
- **Nordic-Specific IDEs:**  
  - **nRF Connect for VS Code:** A development environment optimized for Nordic Semiconductor devices.  
- **General IDEs:**  
  - Microchip  
  - Atmel  
  - Segger  
  - Keil  
  - STM32CubeIDE  
  - IAR  
  - Altium Designer  

---

## **ST (Microelectronics)**  
- **STM32CubeIDE:** [IDE for STM32 development](https://www.st.com/en/development-tools/stm32cubeide.html).  
- **STM32CubeProg:** [Programming tool for STM32 devices](https://www.st.com/en/development-tools/stm32cubeprog.html).  
- STM32 devices feature dual cores, allowing BLE stack on one core and business logic on another. They also support fine-grained power management options.  

---

## **Programmable Logic Devices (PLDs)**  
### **Key Types**  
1. **CPLD (Complex Programmable Logic Device) (ПЛИС/Программируемая Логическая Интегральная Микросхема):**  
   - Integrates a small number of programmable logic blocks.  
   - Contains onboard non-volatile memory.  
   - Suited for applications requiring high speed and simplicity.  

2. **FPGA (Field-Programmable Gate Array):**  
   - Features a larger number of logic blocks with flexible interconnections.  
   - Requires external memory for firmware storage.  
   - Supports advanced operations like signal processing.  
- Главным отличием ПЛИС от микроконтроллеров является то, что в микроконтроллере вы не можете изменять внутренних связей между простейшими элементами, а в ПЛИС на основе прописывания связей основывается программирование и работа с ними.

---

## **Design and Development Tools**  
- **CMSIS (Cortex Microcontroller Software Interface Standard):** A hardware abstraction layer for ARM Cortex devices.  
- **FreeRTOS:** A popular open-source real-time operating system for embedded devices.  
- **CMake & Meson:** Build system generators; [Meson in VS Code guide](https://www.collabora.com/news-and-blog/blog/2023/04/18/meson-and-vscode-develop-your-project-modern-ide/).  

---

## **Protocols**  
- **Bluetooth:** Wireless communication standard used in BLE-enabled devices.  
- **ANT+:** A wireless protocol for transmitting sensor data without pairing, commonly used in fitness and medical devices.  
- **LoRa:** A long-range, low-power wireless protocol suitable for IoT applications.  

---

## **Power and Testing Notes**  
- ESP32 is power-intensive compared to Nordic's nRF series.  
- Nordic devices integrate tools like the [Online Power Profiler](https://devzone.nordicsemi.com/power/w/opp/2/online-power-profiler-for-bluetooth-le).  

---

## **CGM (Continuous Glucose Monitor) Details**  
- **UUID:** 0x181F (CGM Service).  
- **BLE Characteristics:**  
  - **CGM Measurement (0x2AA7):** 9 bytes, updated once per second.  
  - Device name for BLE: **CGM_TEST**.  

---

## **Wearable and Implantable Technology**  
- **Global Market Insight:** Valued at ~$150 million in 2016, projected to reach $2.86 billion by 2025. Key area: non-invasive glucose monitoring.  
- **Berkeley Sweat Patch:** [Research on sweat-based health monitoring](https://news.berkeley.edu/2019/08/16/wearable-sensors-detect-whats-in-your-sweat/).  
- **Stanford Snyder Lab:** Focus on viral disease prediction via wearables ([Read more](https://wearables.stanford.edu/about/)).  
- **Implantable Multisensors:** Must be designed for long-term use with features like recharging and surface cleaning mechanisms.  

---

## **Resources and Tools**  
- Antenna Companies:  
  - [Antenova](https://www.antenova.com).  
  - [Abracon](https://abracon.com/datasheets/PRO-EB-592.pdf).  
  - [Proant PIFA](https://satronel.com/products/antennas/embedded/proant-pifa-antennas.html).  
- General PCB Design Guidelines: [Nordic DevZone](https://devzone.nordicsemi.com/guides/hardware-design-test-and-measuring/b/nrf5x/posts/general-pcb-design-guidelines-for-nrf52-series).  

---

## **nRF BLE vs ST BLE:**
1. **Specialization in BLE:**  
   - Nordic Semiconductor has a long history of focusing on Bluetooth Low Energy. The nRF series is renowned for its BLE performance, low power consumption, and mature SDKs tailored specifically for wireless communication.
   - Nordic provides specialized tools like **nRF Connect** and detailed support for BLE Mesh, Thread, and Zigbee.

2. **Dedicated BLE Stack:**  
   - Nordic’s SoftDevice stack is highly optimized for BLE, offering robust performance, reliability, and easy integration.

3. **Developer Ecosystem:**  
   - Nordic has a stronger developer community and extensive documentation for BLE applications.

---

### **STMicroelectronics in BLE Context:**
1. **General-Purpose Microcontrollers with BLE Support:**  
   - ST primarily offers BLE as part of its STM32WB series (dual-core microcontrollers where one core runs the BLE stack). These microcontrollers are designed for more general-purpose applications, combining BLE with other features like USB, CAN, and more.

2. **Power Management:**  
   - ST offers more fine-grained power management capabilities across its STM32 lineup, though its BLE-specific features may not match the specialization of Nordic.

3. **Ecosystem:**  
   - ST provides **STM32CubeIDE** and **STM32CubeMX** for configuration but doesn't have as extensive BLE tools as Nordic.

---

### **How ST and Nordic Compare in BLE:**
| **Feature**                | **Nordic Semiconductor (nRF)**                     | **STMicroelectronics (STM32WB)**        |
|----------------------------|---------------------------------------------------|-----------------------------------------|
| BLE Performance            | Optimized and specialized for BLE applications.   | Good but less focused on BLE-specific use cases. |
| Development Tools          | nRF Connect, Online Power Profiler, BLE Mesh support. | STM32CubeMX, STM32CubeProg, STM32WB BLE tools. |
| Power Efficiency           | Industry-leading low power for BLE devices.       | Good but not always as efficient for BLE. |
| Target Audience            | Wireless-focused developers (BLE, Zigbee, Thread).| General-purpose MCU developers who need BLE. |
| Community Support          | Extensive BLE-focused community and resources.    | More general STM32 ecosystem, less BLE focus. |

---
