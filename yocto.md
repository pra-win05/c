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
![Image](https://github.com/user-attachments/assets/86f25d42-cd86-4dc0-a0e9-b74ded2dce5a)

#  Yocto Build System Overview

##  What is a Build System?

A **build system** automates the process of:

- Taking source code  
- Compiling it  
- Packaging it into **binaries**  
- Creating a bootable **Linux image** or **SDK**

In the **Yocto Project**, the build system consists of tools like **BitBake**, **OpenEmbedded**, and configuration **metadata** that guide the entire build process.

---

##  What is OpenEmbedded?

**OpenEmbedded (OE)** is a **build framework** for embedded Linux systems.

- It defines **recipes** (instructions for building packages).
- Manages **dependencies** and **cross-compilation**.
- Provides **layers** (collections of recipes) to customize builds for different hardware.

> Think of it as the "**database** + **rules**" for building embedded Linux distributions.

---

##  What is BitBake?

**BitBake** is the **build engine** of the Yocto Project and OpenEmbedded.

- It reads **metadata** (recipes, configurations).
- Decides **what to build and how**.
- Calls the appropriate **compiler/toolchain** to build the code.

>  BitBake is like the `make` tool for Yocto — it interprets instructions and does the actual building.

---

##  What are Binaries?

**Binaries** are the compiled output files generated from source code.

Yocto/BitBake generates:

- **Kernel image** (e.g., `zImage`, `Image`, `uImage`)
- **Bootloader** (e.g., `U-Boot`)
- **Root filesystem** (`rootfs.ext4`, `rootfs.tar.gz`)
- **Device Tree Blobs** (`.dtb`)
- **Kernel modules** (`.ko`)
- **Application binaries** (`/usr/bin/myapp`)

---
#  Yocto Project Basic Terminology

##  General Terms

| Term            | Description |
|------------------|-------------|
| **Yocto Project** | An open-source project that provides templates, tools, and methods to create custom embedded Linux distributions. It’s **not a Linux distro**, but a **build system framework**. |
| **Poky**          | The **reference distribution** of the Yocto Project. It combines **BitBake**, **OpenEmbedded-Core**, and sample configurations. Poky is the default build environment. |
| **BitBake**       | The **build engine** (like `make`) that reads recipes and executes tasks to fetch, configure, compile, and package software. |
| **OpenEmbedded**  | A **layered build framework** used by Yocto to organize recipes and configurations. Yocto uses **OpenEmbedded-Core (OE-Core)** as its base metadata layer. |

---

##  Metadata Structure Terms

| Term       | Description |
|------------|-------------|
| **Metadata** | A collection of instructions, configurations, and descriptions that tell BitBake how to build packages. Includes `.bb`, `.bbappend`, `.inc`, `.conf`, and `layer.conf` files. |

---

##  Files in Metadata

| File Type        | Description |
|------------------|-------------|
| **`.bb` (Recipe)**       | A recipe file that defines how to **fetch, configure, build, and install** a single software package. |
| **`.bbclass` (Class)**   | A class file that contains **reusable functions or variables** shared across recipes. For example, `autotools.bbclass` adds default steps for Autotools-based packages. |
| **`.bbappend` (Append)** | Used to **modify or extend** an existing recipe (usually from another layer) without editing the original `.bb` file. |
| **`.inc` (Include)**     | An include file with **shared variables or functions**. Recipes or config files can include this using `require` or `include`. Helps avoid duplication. |
| **`.conf` (Config)**     | Defines **build-time settings**, such as target machine, distro, or build features. Examples: `local.conf`, `distro.conf`, `machine.conf`. |

---

##  Layers

| Term       | Description |
|------------|-------------|
| **Layer**  | A directory that groups related metadata (**recipes**, **configs**, **classes**). Layers help organize and isolate features (e.g., `meta-networking`, `meta-qt5`). |

Each layer usually contains:
- `recipes-*` folders (e.g., `recipes-core/`)
- `conf/layer.conf` to register the layer
- Optional `.bb`, `.bbappend`, `.bbclass` files

---

### YOCTO PROJECT ARCHITECTURE
![Image](https://github.com/user-attachments/assets/ede9cadf-f8c4-4920-91db-fe0b828d9167)
## 1.  User Configuration

###  File: `conf/local.conf`

---

### What is it?

- `local.conf` is the main file for **customizing how your Yocto image is built**.
- Located in your build directory:

  ## 2. Metadata (.bb + patches)

###  File Types:
- `.bb` – BitBake recipe files
- `.bbappend` – BitBake append files (extend or modify existing recipes)
- `.inc` – Include files (shared variables/functions for reuse)
- `*.patch` – Patch files applied to the source during build

---

###  What is it?

- **Metadata** in Yocto refers to the **recipes and supporting files** that describe **how software packages are built**.
- Think of a `.bb` file like a **Makefile**, but for BitBake.
- It tells BitBake:
  - Where to **download** the source (`SRC_URI`)
  - How to **configure** the build
  - How to **compile**, **install**, and **package** it

---

###  What You Can Customize:

| File Type | Purpose |
|-----------|---------|
| `.bb` | Defines how to fetch, patch, compile, and install a package |
| `.bbappend` | Modify or add to an existing `.bb` recipe without editing the original |
| `.inc` | Share variables and functions across multiple recipes |
| `*.patch` | Apply custom changes to upstream source code |

---
## 3.  Machine (BSP) Configuration

###  Files:
- `conf/machine/*.conf`  
  _(Example: `conf/machine/raspberrypi4.conf`)_

---

###  What is it?

- This configuration defines **hardware-specific settings** for a particular board or SoC.
- Provided by **Board Support Packages (BSPs)** — specialized Yocto layers tailored for embedded platforms (e.g., Raspberry Pi, BeagleBone, i.MX, Intel, etc.).
- Determines how the build system compiles the kernel, selects device tree, and configures bootloader and hardware features.

---

###  What You Can Customize:

| Variable | Description |
|----------|-------------|
| `MACHINE_FEATURES` | Enables hardware features like `wifi`, `bluetooth`, `touchscreen`, etc. |
| `KERNEL_IMAGETYPE` | Type of kernel image to build (e.g., `zImage`, `uImage`, `Image`) |
| `SERIAL_CONSOLE` | Default serial console device and baud rate (e.g., `115200 ttyAMA0`) |
| `UBOOT_MACHINE` | U-Boot config used for the board (if using U-Boot as bootloader) |
| `PREFERRED_PROVIDER_virtual/kernel` | Specifies which kernel recipe to use |
| `DEVICE_TREE` | Sets the `.dtb` file for device-specific configuration |
| `IMAGE_FSTYPES` | Defines filesystem types to generate (e.g., `ext4`, `wic`, `tar.gz`) |

---

## 4.  Policy Configuration

###  Files:
- `conf/distro/*.conf`  
  _(Examples: `poky.conf`, `mydistro.conf`)_

---

###  What is it?

- **Policy Configuration** defines **global rules and behaviors** that apply across **all recipes** and **all packages** in your Yocto build.
- These settings determine:
  - Licensing policy
  - Preferred versions of packages
  - Init system
  - System-wide features
  - Packaging format
  - SDK contents
- Each policy file essentially defines a **Linux distribution variant** (e.g., `poky`, `mydistro`, `mycompany-linux`).

---

###  What You Can Customize:

| Variable | Description |
|----------|-------------|
| `DISTRO` | Name of your custom distribution (e.g., `poky`, `mydistro`) |
| `LICENSE_FLAGS_ACCEPTED` | Define accepted licenses (e.g., `commercial`, `GPLv3`) |
| `DISTRO_FEATURES` | Enable or disable global features (e.g., `x11`, `wifi`, `systemd`) |
| `PACKAGE_CLASSES` | Choose package formats: `ipk`, `deb`, or `rpm` |
| `INIT_MANAGER` | Set init system: `systemd`, `sysvinit`, etc. |
| `PREFERRED_VERSION_<pkg>` | Force specific versions of packages |
| `SDKIMAGE_FEATURES` | Define contents of the generated SDK |
| `USE_NLS`, `USE_DEVFS`, `USE_EGLIBC` | Global behavior flags for localization, devices, and libc |
| `EXTRA_IMAGE_FEATURES` | Enable dev tools, debug symbols, or SSH in final image |

---








