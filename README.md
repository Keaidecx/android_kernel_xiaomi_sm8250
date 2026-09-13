[中文](./README_zh-CN.md) | English

# Xiaomi SM8250 Kernel Based on AstideLabs' Branch

## Warning
The kernel source code is still under development and may cause some unpredictable problems. Please use it with caution.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Supported Devices](#supported-devices)
- [Build Instructions](#build-instructions)
  - [Quick Build](#quick-build)
  - [Manual Build](#manual-build)

---

## Introduction
This repository is forked from AstideLabs/android_kernel_xiaomi_sm8250. Some commits also originate from other developers in the open-source community.

---

## Features
This kernel supports ReSukiSU & SuSFS & Droidspaces. Please install the ReSukiSU Manager by yourself. The NoKernelSU version supports Magisk and APatch (and their forks).

The prebuilt kernel in the Release section is compiled from the android17-aptusitu branch, and should work on stock MIUI/HyperOS as well as third-party AOSP-based ROMs for Android 11–17.

Below are some of the key features:
1. F2FS with realtime discard enabled for improved flash TRIM behavior
2. Support for EROFS
3. zRAM with support for multiple compression algorithms, including LZO, LZ4, LZ4HC, and ZSTD, enabled ZRAM_WRITEBACK, upgraded LZ4 and ZSTD
4. Backported BPF from Linux 5.15 and clone3 (Android 16/17 compatible)
5. Introduced LE9EC to optimize memory
6. Backported Binder from 5.10; MIUI builds incorporate millet from xaga, while AOSP builds incorporate Re:Kernel
7. Fixes the issue where the battery percentage gets stuck at 1%, and supports recognizing higher-capacity replacement batteries
8. Integrate BBG(Baseband-guard)

---

## Supported Devices
| Codename | Device Name |
|---|---|
| alioth | Redmi K40 / Xiaomi 11X / POCO F3 |

---

## Build Instructions

### Quick Build
1. Fork this repo 
2. Go to Actions
3. Find Build Kernel, click Run workflow, and select the necessary options

### Manual Build
1. Prepare the build environment.
You need git, make, curl, bison, flex, zip, etc.

On Debian/Ubuntu:
sudo apt install build-essential git curl wget bison flex zip bc cpio libssl-dev ccache tar
sudo apt install python-is-python3

On RHEL/RPM-based OS:
sudo yum groupinstall 'Development Tools'
sudo yum install wget bc openssl-devel ccache tar

Note: ccache is enabled in build.sh ($HOME/.cache/ccache_mikernel). You may remove/modify it.

2. Download ZyC-Clang v16 toolchain:
mkdir zyc-clang
cd zyc-clang
wget https://github.com/ZyCromerZ/Clang/releases/download/16.0.6-20260807-release/Clang-16.0.6-20260807.tar.gz
tar -zxvf Clang-16.0.6-20260807.tar.gz
cd ..

3. Build:
Without KernelSU:
bash build_kernel.sh alioth TARGET_OS(omit to build for all)

With KernelSU:
bash build_kernel.sh alioth ksu TARGET_OS(omit to build for all)
