<p align="center">
  <img src="./Flacly%20logo.png" alt="FLACLY Logo" width="128" height="128">
</p>

<h1 align="center">FLACLY (Legacy Edition)</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Audio_Core-C%2B%2B17%2F20-blue.svg?style=for-the-badge&logo=cplusplus" alt="C++ Core">
  <img src="https://img.shields.io/badge/Platform-Android_4.4%2B_(API_19)-brightgreen.svg?style=for-the-badge&logo=android" alt="Android 4.4+">
  <img src="https://img.shields.io/badge/Architecture-Decoupled_Engine-orange.svg?style=for-the-badge" alt="Decoupled Architecture">
  <img src="https://img.shields.io/badge/PSS_Budget-%3C80_MB-red.svg?style=for-the-badge" alt="PSS Budget">
  <img src="https://img.shields.io/badge/License-Source--Available-purple.svg?style=for-the-badge" alt="License">
</p>

**FLACLY Legacy Edition** is an ultra-high-performance, bit-perfect offline FLAC audio player specifically architected for legacy Android devices (Android 4.4 KitKat / API 19+) and vintage SoC hardware (e.g., Exynos 4412, Snapdragon 400 series, legacy DAPs). 

By combining low-level C++ DSP pipelines, dynamic hardware audio driver backends, and low-allocation memory structures, FLACLY delivers audiophile-grade audio playback on constrained legacy runtimes under a strict **PSS target budget of < 80 MB**.

---

## Key Features

- **Dynamic Audio Driver Selection:** User-selectable output driver architecture supporting **AudioTrack (Zero-Copy JNI / S16 PCM)** for bug-free playback on legacy Exynos/Samsung vendor builds (`libOpenSLES.so` workaround), alongside native **OpenSL ES** and **AAudio** for modern devices.
- **Fixed-Point S16 PCM Decoding:** Operates on 16-bit Signed Linear PCM quantization ($98.09\text{ dB}$ dynamic range) to eliminate CPU-heavy floating-point ($f32$) mixing overhead on legacy ARM Cortex-A9 processors without hardware vectoring.
- **Zero-Resampling Queue Alignment:** Queries native hardware properties (`PROPERTY_OUTPUT_SAMPLE_RATE` and `PROPERTY_OUTPUT_FRAMES_PER_BUFFER`) via `AudioManager` to feed buffers directly into `AudioFlinger`, completely bypassing software resampling and driver popping.
- **Low-Memory PSS Bounds (< 80 MB):** Built with static arena allocation (Interval Graph Coloring), columnar data layouts (PAX), and zero-allocation JNI direct byte buffer mapping (`NewDirectByteBuffer`) to stabilize heap size under high track loads.
- **Fast B-Tree Metadata Indexing:** Bypasses unstable legacy `MediaMetadataRetriever` / Stagefright Binder IPC calls by querying SQLite `MediaStore` B-Tree indexes directly for instant, lock-free track indexing.
- **Fast Octree Color Extraction:** Downsamples artwork matrices to $32 \times 32$ pixels and evaluates dominant palettes in C++ using Octree spatial color quantization in $O(N \log K)$ runtime.
- **AppCompat Legacy Dark Theme:** High-contrast, WCAG-compliant UI styling tailored for pre-Lollipop Material backports, resolving dark popup transparency bugs on Android 4.4.
