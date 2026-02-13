# SSV6256P (SSV6X5X) WiFi Driver for Linux Kernel 6.12+

This repository contains my port of the **SV6256P / SSV6X5X SDIO Wi‑Fi driver** from legacy Linux 4.4 to **Linux 6.12 and above**.  
The driver now builds and runs on modern kernels, with updated mac80211/cfg80211 integration.

[![License](https://img.shields.io/badge/License-Dual%20BSD%2FGPL-blue.svg)](LICENSE)
[![Kernel](https://img.shields.io/badge/Kernel-6.12+-green.svg)](https://kernel.org/)
[![Architecture](https://img.shields.io/badge/Arch-ARM64-orange.svg)](https://www.kernel.org/doc/html/latest/arm64/index.html)

## 📖 Background

The SSV6256P is a dual-band WiFi chipset from iComm Semiconductor, commonly found in low-cost TV boxes, embedded Linux devices, and IoT applications. The original driver was designed for Linux kernel 4.4, but modern Linux distributions use kernel 6.x which introduced significant changes to the mac80211 wireless stack.

This repository contains a fully functional port of the SSV6256P driver to Linux kernel 6.12+, with all necessary API adaptations and bug fixes.

### Tested Hardware

- **SoC**: Allwinner H616 (ARM Cortex-A53)
- **Board**: X96 Q TV Box
- **Interface**: SDIO
- **Kernel**: Armbian_25.05.0_linux_6.12.11-edge-sunxi64

## ✅ Kernel Compatibility

| Kernel Version | Status | Notes |
|---------------|--------|-------|
| 6.12.x | ✅ Tested | Fully working |
| 6.13.x+ | ✅ Expected | Should work (API-compatible) |
| 6.6 LTS | ⚠️ Untested | May work with minor adjustments |
| 5.x | ❌ Not supported | Use original driver |
| 4.x | ❌ Not supported | Use original driver |

## 📦 Prerequisites

### Required Packages (Debian/Ubuntu)

```bash
sudo apt-get update
sudo apt-get install -y build-essential linux-headers-$(uname -r) git
```

### Required Packages (Arch Linux)

```bash
sudo pacman -S base-devel linux-headers
```

### Required Packages (Fedora/RHEL)

```bash
sudo dnf install kernel-devel kernel-headers gcc make
```

## 🔨 Compilation

```bash
git clone https://github.com/cdhigh/armbian_sv6256p.git
cd armbian_sv6256p
chmod +x ./parser-conf.sh
make ARCH=arm64 KSRC=/lib/modules/$(uname -r)/build
ls -lh ssv6x5x.ko
```

## 📥 Installation

```bash
sudo cp ./ssv6x5x-wifi.cfg /lib/firmware/
sudo cp ./ssv6x5x-sw.bin /lib/firmware/
sudo cp ./ssv6x5x.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless/
sudo depmod -a
sudo modprobe ssv6x5x
```

## 🚀 Usage

### Verify Driver Loading

Check kernel log for successful initialization:

```bash
dmesg | grep -i ssv
```

### Identify Network Interface

```bash
ip link show
# or
iw dev
```

Look for `wlan0` or similar interface.

### Scan for Networks

```bash
sudo iw dev wlan0 scan | grep SSID
```

### Connect to WiFi

```bash
nmcli device wifi connect "YourSSID" password "YourPassword"
or
nmtui
```

# Credits
* Vendor driver sources
* Linux mac80211 maintainers for API references
* Community help on porting legacy Wi‑Fi drivers
