

## **Communication Protocols**

1. **CAN (Controller Area Network)**

* A robust multi-master bus standard designed for reliable communication in automotive and industrial systems.
* Allows multiple microcontrollers to communicate without a host computer.
* Example: Used in cars for communication between the engine control unit, ABS, airbags, etc.

2. **SPI (Serial Peripheral Interface)**

* A high-speed synchronous serial communication protocol.
* Involves a master and one or more slaves using four lines: MOSI, MISO, SCLK, and SS.
* Example: Communicating with SD cards, displays, or sensors.

3. **I²C (Inter-Integrated Circuit)**

* A slower, synchronous communication protocol than SPI, but supports multiple devices on just two wires (SDA, SCL).
* Master-slave architecture.
* Example: Connecting temperature sensors, EEPROMs, or RTC modules.

4. **USART (Universal Synchronous/Asynchronous Receiver Transmitter)**

* A hardware module for serial communication.
* Can operate in **UART** (asynchronous) or **USART** (synchronous) mode.
* Example: Serial terminals, GPS modules, wireless modules (e.g., Bluetooth).

5. **USB (Universal Serial Bus)**

* A standard for wired connection between computers and peripherals.
* Supports hot-plugging, high data transfer rates.
* Microcontrollers can act as USB device or host (e.g., keyboards, flash drives).

6. **Ethernet**

* A family of networking technologies for local area networks (LANs).
* Enables communication over wired networks using TCP/IP.
* Used in embedded systems for web servers, remote updates, or IoT connectivity.

**Software Abstractions / APIs**

7. **CMSIS (Cortex Microcontroller Software Interface Standard)**

* A standardized software layer for ARM Cortex-M processors.
* Provided by ARM to unify access to processor features, core registers, and peripherals.
* Makes code more portable across different vendors using ARM Cortex-M.

8. **LL (Low-Layer) Drivers**

* Provided by STMicroelectronics (STM32 series).
* Lightweight, close-to-hardware drivers for fine control and performance.
* Requires more effort but gives more control (e.g., configuring registers manually).

9. **HAL (Hardware Abstraction Layer)**

* Also provided by STM32Cube libraries.
* Easier and higher-level API than LL.
* Simplifies peripheral configuration and usage, but with some overhead.
* Example: `HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);`

---

**Summary Table**

| Term     | Type                      | Use Case Example                        |
| -------- | ------------------------- | --------------------------------------- |
| CAN      | Communication protocol    | Car ECUs network                        |
| SPI      | Communication protocol    | Flash memory, LCDs                      |
| I²C      | Communication protocol    | Temperature sensors                     |
| USART    | Serial communication      | PC to microcontroller serial console    |
| USB      | Wired data interface      | USB keyboard, USB CDC serial port       |
| Ethernet | Networking protocol       | IoT devices with web interface          |
| CMSIS    | Software standard (ARM)   | Cortex-M software unification           |
| LL       | Low-level drivers (STM32) | Direct register control, optimized code |
| HAL      | Abstraction layer (STM32) | Easy-to-use peripheral APIs             |

### I2C

Sure! Here's a detailed explanation of **I²C (Inter-Integrated Circuit)** — one of the most widely used communication protocols in embedded systems.

---

**What is I²C?**

**I²C (pronounced “I-squared-C” or “I-two-C”)** is a **synchronous, multi-master, multi-slave, two-wire** serial communication protocol developed by Philips (now NXP) in the 1980s. It is used for connecting low-speed peripherals to microcontrollers.

---

**Key Features**

* **Two wires only:**

  * `SCL` (Serial Clock Line)
  * `SDA` (Serial Data Line)
* **Multiple devices** can share the same bus (each has a unique **7-bit or 10-bit address**).
* **Master-slave architecture:** Master initiates communication; slaves respond.
* **Bidirectional communication:** Devices can both transmit and receive.

---

**How It Works**

1. **Physical Layer**

* Open-drain lines pulled up with resistors (commonly 4.7kΩ or 10kΩ).
* Devices **pull the line low** to transmit bits; releasing the line allows it to return to HIGH.

2. **Roles**

* **Master**: Generates clock (SCL) and initiates communication.
* **Slave**: Listens and responds when its address is called.

3. **Typical Sequence**

4. **START condition** (SDA goes LOW while SCL is HIGH).
5. **Address frame** (7 or 10 bits) + Read/Write bit.
6. **ACK/NACK** bit (acknowledgment from slave).
7. **Data transfer** (one byte at a time).
8. **STOP condition** (SDA goes HIGH while SCL is HIGH).

---

**Data Transfer Example**

| Step      | Signal Details                               |
| --------- | -------------------------------------------- |
| Start     | Master pulls SDA low while SCL is high       |
| Address   | Master sends 7-bit slave address + 1 R/W bit |
| ACK       | Slave pulls SDA low to acknowledge           |
| Data Byte | Master or Slave sends 8 bits                 |
| ACK       | Receiver pulls SDA low                       |
| Stop      | Master releases SDA while SCL is high        |

---

**Advantages**

* Only **two wires** needed regardless of how many devices are connected.
* **Simple wiring** and easy to expand with multiple devices.
* **Widely supported** in sensors, EEPROMs, RTCs, etc.

---

**Disadvantages**

* Slower than SPI (typical speeds: 100 kHz, 400 kHz, up to 3.4 MHz in High-Speed mode).
* Shared bus can lead to **conflicts** or **bottlenecks** if not managed well.
* More **complex protocol logic** compared to SPI (start/stop conditions, addressing, ACK/NACK).

---

**Common I²C Devices**

* Temperature sensors (e.g., LM75, BME280)
* EEPROMs (e.g., AT24C32)
* Real-time clocks (e.g., DS1307)
* OLED displays (e.g., SSD1306)
* Accelerometers (e.g., MPU6050)

---

**I²C vs SPI**

| Feature      | I²C          | SPI                      |
| ------------ | ------------ | ------------------------ |
| Wires        | 2            | 4+ (MISO, MOSI, SCK, SS) |
| Speed        | Slower       | Faster                   |
| Multi-master | Yes          | Rare                     |
| Complexity   | Higher       | Lower                    |
| Data lines   | Shared (SDA) | Dedicated                |

---
Great question — especially since you're using an **nRF controller** (like nRF52840 or similar) and planning to connect **multiple sensors using I²C**.

Let’s break it down and then visualize the **I²C bus wiring schema**.

---

**What Does “Shared Data Lines” Mean in I²C?

In **I²C**, all devices — both the **master (nRF)** and the **slaves (sensors)** — are connected to the **same two lines**:

* **SCL** (clock)
* **SDA** (data)

These two lines are **shared** among **all** devices, so every sensor/device listens on the **same wires**. The master uses a unique **address** to talk to each sensor individually.


**Your Context: nRF + Multiple I²C Sensors

**What You Need

* 1x **nRF microcontroller** (acts as I²C master)
* 2 wires: **SCL** and **SDA**
* 2x pull-up resistors (usually **4.7kΩ** or **10kΩ**) — one for **SCL**, one for **SDA**
* Multiple **I²C sensors** (slaves) with **unique I²C addresses**

---

**I²C Wiring Schema

Here’s how it would look conceptually:

```
         +---------------------+
         |    nRF Controller   |
         |     (Master)        |
         |                     |
         |   P0.x --- SDA -----+----+---- SDA --- Sensor A (0x40)
         |   P0.y --- SCL -----+----+---- SCL --- Sensor B (0x41)
         |                     |    |             Sensor C (0x76)
         +---------------------+    +-- ... etc.
                  |                    |
               Pull-up              Pull-up
               4.7kΩ                 4.7kΩ
                  |                    |
                VCC                  VCC
```

> **Note:**

* All devices connect **in parallel** on SDA and SCL.
* Each sensor must have a **unique I²C address**.
* You add **pull-up resistors** to VCC (usually 3.3V for nRF).

---

**Example Sensors You Could Connect

| Sensor  | Address (default) |
| ------- | ----------------- |
| BME280  | 0x76 or 0x77      |
| MPU6050 | 0x68 or 0x69      |
| BH1750  | 0x23 or 0x5C      |
| SHT31   | 0x44 or 0x45      |

If two sensors have the **same address**, you’ll either:

* Change the address via a pin/config if supported.
* Use an **I²C multiplexer** (like TCA9548A).

---

**Code Concept (e.g., using nRF SDK or Zephyr)

```c
// Pseudocode for reading from BME280 (address 0x76)
uint8_t addr = 0x76 << 1;  // Shifted for 8-bit address
uint8_t reg = 0xD0;        // Who_am_I register
uint8_t data;

nrf_drv_twi_tx(&m_twi, addr, &reg, 1, true);  // Write register
nrf_drv_twi_rx(&m_twi, addr, &data, 1);       // Read data
```

---

**Summary

* **Shared data lines** = All devices are wired to the same **SDA** and **SCL** lines.
* The master uses **unique addresses** to talk to each device one by one.
* You only need **two GPIO pins** from your nRF to control **many** I²C devices.

---
