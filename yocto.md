# Embedded Systems Overview

## What is an Embedded System?

An **embedded system** is a combination of hardware and software designed to perform a **specific task**.  
Key characteristics:

- Equipped with a processing unit.
- Includes temporary memory and permanent storage.
- Part of a larger system.
- Customized for specific functionality.
- Lower cost compared to general-purpose systems.

Each component in such a system can be considered an embedded system when it has its own processor and memory.

---

##  Development Options for a Security System

### Option 1: **Generic Purpose Computer**

**Approach**: Use a PC + camera + OpenCV app to detect intruders and send Wi-Fi alerts.

**Pros**:
- Fast development
- Minimal constraints

**Cons**:
- Not customer-ready
- Unused features increase cost
- Security risks
- Slow boot time
- Bulky system

>  **Conclusion**: Not suitable for productization.

---

### Option 2: **Raspberry Pi**

**Approach**: Use a Raspberry Pi with camera and Wi-Fi.

**Pros**:
- Reduced cost
- Can be enclosed

**Cons**:
- Still has unused hardware (HDMI, Ethernet, USB)
- Not fully optimized

>  **Conclusion**: Better than a PC, but not optimal.

---

### Option 3: **Custom Embedded System**

**Approach**: Design a custom PCB based on Raspberry Pi, removing unused ports and embedding Wi-Fi + camera modules.

**Pros**:
- Minimum cost
- High-volume production ready
- Fully customized

**Cons**:
- High development cost and time
- Software development limitations due to ARM

>  **Conclusion**: Ideal for a final product.

---

##  Embedded Systems: Hardware Perspective

- Take an existing design.
- **Remove**: HDMI, USB, Ethernet.
- **Add**: Essential components like Wi-Fi, camera, etc.

---

##  Embedded Systems: Software Perspective

###  System Image Creation

1. **Source Code**: Obtain from SoC vendor or public sources.
2. **Toolchain**: Use ARM GCC compiler for cross-compilation.
3. **Build System**: Automate with provided scripts/tools.
4. **Generated Images**:
   - Bootloader (e.g., U-Boot)
   - Kernel Image (zImage/uImage)
   - Device Tree Blob (.dtb)
   - Root Filesystem (e.g., rootfs.ext4, .cpio)

---

##  Introduction to Embedded Linux

### Hardware View:
- Use minimal board (e.g., Raspberry Pi)
- Remove unused components
- Add essentials

### Software View:
- Start with BSP + Linux source
- Strip unneeded drivers/libs
- Add only required components

>  Goal: Small, fast, low-power OS image

---

##  Feature Comparison

| Feature         | Linux (General) | Embedded Linux      |
|-----------------|------------------|----------------------|
| Usage           | PCs, servers     | IoT, TVs, devices    |
| Size            | GBs              | MBs or less          |
| UI              | GUI supported    | Mostly no GUI        |
| Customization   | General purpose  | Highly customized    |
| Performance     | High             | Power-optimized      |

---

##  Why Use Linux in Embedded Products?

- Modern devices are **too complex for bare-metal**.
- **Open-source ecosystem** solves many problems.
- Linux supports flexible, customizable development.

---

##  Linux Desktop Software Stack (Layered View)

### 1 Applications
- Executable binaries:
  - `ls`, `cp`, `vi` (via **BusyBox**)
  - Python, OpenSSL
- Common dirs: `/bin`, `/sbin`, `/usr/bin`

### 2 Services
- Init systems:
  - `systemd`, `upstart` (desktop)
  - `SysV` (embedded)
- Examples:
  - UDEV
  - Network Service
  - SSHD
  - BootlogD

### 3️ Libraries
- Core C Libraries:
  - `glibc`, `musl`, `uClibc`, `bionic`
- Additional:
  - `QT`, `Boost`, `OpenSSL`, `pthread`, `libm`

### 4️ System Calls
- Interface between user space and kernel:
  - `open()`, `read()`, `write()`, `ioctl()`

### 5️ Drivers
- Represented in `/dev`
- Examples:
  - USB, I2C, Wi-Fi, touchscreen drivers

### 6️ Linux Kernel
- Manages:
  - Memory (MMU)
  - Processes
  - Networking
  - File systems (VFS)
  - IPC

---

##  Bootloader

- Initializes hardware and loads the kernel.
- Example: **U-Boot**

---

##  Toolchain & Cross-Compilation

### Cross-Compilation:
- Compile on PC (x86)
- Run on embedded board (ARM)

### Components:
- `gcc`, `g++`
- `ld`, `as`, `objdump` (binutils)
- `make`, `cmake`
- `sysroot`

---
