中文 | [English](./README.md)

# 基于 AstideLabs/android_kernel_xiaomi_sm8250 分支的内核

## 警告
内核源码仍在开发中，可能会导致一些不可预料的问题，请谨慎使用。

## 目录
- [简介](#简介)
- [特性](#特性)
- [支持的设备](#支持的设备)
- [构建方法](#构建方法)
  - [快速构建](#快速构建)
  - [手动构建](#手动构建)

---

## 简介
该仓库基于 AstideLabs/android_kernel_xiaomi_sm8250 进行 fork。一些提交同样来源于开源社区中的其他开发者。

---

## 特性
本内核支持 ReSukiSU & SuSFS & Droidspaces。NoKernelSU 版本支持应用 Magisk 和 APatch(及他们的分支)。

Release 里的编译好的内核成品由 android17-aptusitu 分支编译，应当能在原版 MIUI/HyperOS 和第三方的基于 AOSP 的各种 Android11-17 的 ROM 上使用。

以下是一些具体的功能:
1. F2FS 开启了 realtime discard 以更好地 TRIM 闪存
2. 支持 EROFS
3. zRAM 支持 LZO、LZ4、LZ4HC、ZSTD 等压缩算法，开启了 ZRAM_WRITEBACK，升级了 LZ4 和 ZSTD
4. 向后移植 5.15 BPF 和 clone3(支持安卓 16/17)
5. 引入 LE9EC 以优化内存
6. 向后移植 5.10 的 Binder，MIUI 构建引入来自 xaga 的 millet，AOSP 构建引入 Re:Kernel
7. 修复电量卡在 1% 的问题，并且支持解容
8. 集成 BBG(Baseband-guard)

---

## 支持的设备
| 设备代号 | 设备名称 |
|---|---|
| alioth | Redmi K40 / Xiaomi 11X / POCO F3 |

---

## 构建方法

### 快速构建
1. fork 本仓库
2. 进入 Actions
3. 找到 Build Kernel， 点击 Run workflow 并选择必要内容

### 手动构建
1. 准备基本构建环境。
需要常用工具链 git、make、curl、bison、flex、zip 等，以及一些软件包。

在 Debian/Ubuntu 系统下执行：
sudo apt install build-essential git curl wget bison flex zip bc cpio libssl-dev ccache tar
sudo apt install python-is-python3

在 RHEL/RPM 系统下执行：
sudo yum groupinstall 'Development Tools'
sudo yum install wget bc openssl-devel ccache tar

注意：build.sh 中启用了 ccache，路径是 $HOME/.cache/ccache_mikernel。可修改或删除。

2. 下载 ZyC-Clang v16 工具链：
mkdir zyc-clang
cd zyc-clang
wget [https://github.com/ZyCromerZ/Clang/releases/download/16.0.6-20260807-release/Clang-16.0.6-20260807.tar.gz](https://github.com/ZyCromerZ/Clang/releases/download/16.0.6-20260807-release/Clang-16.0.6-20260807.tar.gz)
tar -zxvf Clang-16.0.6-20260807.tar.gz
cd ..

3. 构建：
不使用 KernelSU:
bash build_kernel.sh alioth TARGET_OS(omit to build for all)

使用 KernelSU:
bash build_kernel.sh alioth ksu TARGET_OS(omit to build for all)
