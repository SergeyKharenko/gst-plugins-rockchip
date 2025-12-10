# GStreamer Rockchip Plug-ins

<div align="center"><img src="logo.png" style="zoom:20%"/></div>

## Introduction

This repository provides a **maintained and streamlined continuation** of the Rockchip-specific GStreamer plug-ins originally from:

> https://github.com/JeffyCN/mirrors/tree/gstreamer-rockchip

It is renamed and reorganized under the **`gst-plugins-rockchip`** structure to better match the upstream GStreamer module layout while remaining focused on Rockchip hardware.

The goals of this fork are:

- **Cleaner build experience** — remove noisy warnings, modernize build flags, and keep the tree minimal.  
- **Developer-friendly structure** — remove packaging artifacts (Debian metadata, outdated scripts) and provide clear documentation.  
- **Component isolation** — enable building only the plug-ins developers actually need.

### **Included Plug-ins**

| Plug-in | Description |
|--------|-------------|
| **rockchipmpp** | Hardware encoders/decoders using Rockchip MPP (H.264/H.265/VP9/JPEG depending on SoC) |
| **rkximage** | X11 + DRM/KMS video sink with PRIME/AFBC support |
| **kmssrc** | KMS framebuffer capture source |

---

## **Build Requirements**

### **1. Build tools**
- Meson ≥ 0.47  
- Ninja  

### **2. Core dependencies**
- GLib ≥ 2.32  
- GStreamer ≥ 1.14.4 (core, base, video, pbutils, allocators)  

Install on Debian/Ubuntu:

```sh
sudo apt install meson ninja-build libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev
```

### **3. Display/KMS stack (optional, for rkximage)**

```sh
sudo apt install libx11-dev libx11-xcb-dev libdrm-dev
```

### **4. Rockchip-specific dependencies**

- **MPP** — https://github.com/rockchip-linux/mpp  
  Recommended install prefix:

```sh
mkdir build
cd build

cmake .. -DCMAKE_INSTALL_PREFIX=/usr -G Ninja

ninja
sudo ninja install
```

- **librga** (optional) — https://github.com/SergeyKharenko/librga  
  Optional RGA backend for GStreamer. The upstream librga lacks a `librga.pc` file, so this project uses my fork, which adds the required `librga.pc` for Meson to detect and enable RGA support.

---

## **Building and Installing**

```sh
mkdir build
cd build

meson .. --prefix=/usr
ninja
sudo ninja install
```

### **Optional plug-in switches**

Available Meson options:

```
-Drga=enabled/disabled
-Drkximage=enabled/disabled
-Drockchipmpp=enabled/disabled
-Dkmssrc=enabled/disabled
```

Example: disable RGA support:

```sh
meson setup .. --prefix=/usr -Drga=disabled
```

---

