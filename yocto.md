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

#  Kernel Configuration: `make menuconfig` vs. Yocto Approach

When working with Linux kernel customization for embedded systems, there are two main ways to configure the kernel:

---

## . `make menuconfig` — Traditional Kernel Configuration

This is the **standard method** used when you're working directly with the Linux kernel source **outside** of Yocto.

#  What is Yocto Project?

The **Yocto Project** is an open-source collaboration project that helps developers **create custom Linux distributions** for embedded systems, regardless of the hardware architecture (e.g., ARM, x86).

> Yocto is not a Linux distribution — it's a set of tools for building your own.

---

##  What Can Be Customized in the Kernel with Yocto?

When using Yocto to build an embedded Linux image, you can **deeply customize the Linux kernel** to fit your device's requirements — from performance to memory footprint.

Here are the major kernel aspects you can customize:

###  1. **Boot Time Optimization**
- Remove unnecessary drivers or features to reduce kernel size.
- Disable debugging or logging features to speed up boot.
- Optimize init system and enable fast boot techniques like `read-only rootfs` or `initramfs`.

###  2. **Memory Usage**
- Disable memory-hungry subsystems not used by your application.
- Use lightweight libraries and a small `C` library like `musl` or `uClibc`.
- Optimize kernel page size and memory allocators (e.g., SLUB vs SLOB).

###  3. **Filesystem Type**
- Choose from multiple filesystems:
  - `ext4`, `squashfs`, `jffs2`, `ubifs`, `cpio`, etc.
- Read-only or read-write depending on use case.
- Enable/disable journaling to save flash wear.

###  4. **Device Drivers**
- Only include drivers relevant to your hardware (I2C, SPI, GPIO, UART, USB, etc.).
- Disable unneeded drivers to reduce image size and attack surface.
- Add support for custom drivers via `.bbappend` and `.cfg`.

###  5. **Security Features**
- Enable kernel hardening options:
  - Stack protector
  - ASLR (Address Space Layout Randomization)
  - SELinux or AppArmor
- Remove legacy or unused protocols to reduce attack surface.

###  6. **Processor and Architecture Specific Settings**
- Set correct CPU type (e.g., Cortex-A53, A72).
- Optimize for single-core or multi-core systems.
- Enable hardware floating point, NEON support, etc.

###  7. **Kernel Features and Subsystems**
- Networking stack features (IPv6, VLAN, etc.)
- Filesystem features (overlayfs, squashfs compression)
- USB gadget support, HID, CAN, BLE, etc.
- Real-time kernel features (PREEMPT-RT patch)

### 8. **Init System**
- Use minimal init systems like `busybox`, `systemd`, or `SysV`.
- Customize init scripts to boot directly into your application.

---

#  Desktop Kernel vs  Embedded Kernel

Understanding the difference between a desktop and embedded Linux kernel helps in making design decisions based on system requirements.

---

##  Key Differences

| Feature / Attribute            |  Desktop Kernel                          |   Embedded Kernel                         |
|-------------------------------|-------------------------------------------|--------------------------------------------|
| **Purpose**                   | General-purpose (laptops, PCs, servers)   | Application-specific (IoT, industrial, etc.)|
| **Size**                      | Large (many modules & drivers)            | Small, optimized for specific hardware     |
| **Drivers**                   | Includes support for a wide range         | Only includes required drivers             |
| **User Interface**            | Supports full GUI environments            | Often headless or minimal UI               |
| **Boot Time**                 | Not a priority (20–60 seconds)            | Critical (needs fast boot: < 5 seconds)    |
| **Memory Usage**              | Assumes large RAM (GBs)                   | Optimized for low RAM (MBs)                |
| **Filesystem**                | ext4, btrfs, xfs                          | squashfs, ubifs, jffs2, cramfs             |
| **Init System**               | systemd, upstart                          | busybox init, systemd (minimal), SysVinit  |
| **Security Model**            | Multi-user, complex permissions           | Often single-user or sandboxed application |
| **Update Mechanism**          | Package manager (apt, rpm)                | Image-based (OTA) or atomic updates        |
| **Real-Time Support**         | Optional, not default                     | Frequently patched with PREEMPT-RT         |
| **Build Process**             | Binary packages (Ubuntu, Fedora, etc.)    | Custom-built using Yocto, Buildroot        |

---


#  How Linux Boots (Boot Process Overview)

The Linux boot process consists of several stages that transform a powered-off device into a fully running Linux system.

---

##  Linux Boot Stages 

1. **Power On / Reset**    

2. **Bootloader (e.g., GRUB, U-Boot)**  

3. **Linux Kernel**  

4. **Init System (e.g., systemd, SysVinit, BusyBox)**   

5. **User Space / Shell / Application**  


### 1️ Power-On and Reset (ROM Code)
- Processor starts executing from a fixed ROM location.
- Initializes clocks, RAM, and basic peripherals.
- Loads the **first-stage bootloader** (if required).

### 2️ Bootloader (e.g., GRUB, U-Boot)
Responsible for:
- Initializing board-specific hardware (RAM, UART, SD card, etc.)
- Loading the **Linux kernel** into RAM
- Loading the **Device Tree Blob** (`.dtb`) for ARM-based systems
- Loading an **initramfs/initrd** (optional)
- Passing control to the kernel

**Desktop**: GRUB loads `vmlinuz`  
**Embedded**: U-Boot loads `zImage` or `uImage`

---

### 3️ Linux Kernel Initialization
Once loaded:
- Sets up memory management (MMU, paging)
- Initializes device drivers (console, storage, network, etc.)
- Mounts the **root filesystem**
- Starts the **init process** (PID 1)

📄 Files loaded:
- Kernel image (`vmlinuz`, `zImage`, `uImage`)
- Optional: `initrd` / `initramfs`
- Device Tree (`.dtb`) — ARM only

---

### 4️ Init System (PID 1)
The **first user-space process** started by the kernel.

- Reads config files (`/etc/inittab`, `/etc/systemd/`, etc.)
- Starts system services (networking, sshd, logging, GUI)
- Mounts additional filesystems
- Launches user applications or login shell

Examples:
- `systemd` (desktop)
- `BusyBox init`, `SysVinit` (embedded)

---

### 5️ User Space & Applications
Finally:
- You get a login prompt, shell, or GUI
- In embedded systems, it may auto-launch a specific application (e.g., GUI, kiosk app, sensor logic)

---

### INTRODUCTION TO YOCTO 
![Yocto Build System](https://github.com/pra-win05/c/blob/main/images/yocto-flow.png?raw=true)
