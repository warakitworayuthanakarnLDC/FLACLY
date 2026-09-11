<p align="center">
  <img src="./Flacly%20logo.png" alt="FLACLY Logo" width="128" height="128">
</p>

<h1 align="center">FLACLY</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Audio_Core-C%2B%2B20-blue.svg?style=for-the-badge&logo=cplusplus" alt="C++20">
  <img src="https://img.shields.io/badge/Platform-Android_29%2B-brightgreen.svg?style=for-the-badge&logo=android" alt="Android">
  <img src="https://img.shields.io/badge/Architecture-Decoupled_Engine-orange.svg?style=for-the-badge" alt="Decoupled Architecture">
  <img src="https://img.shields.io/badge/RAM_Budget-%3C30_MB-red.svg?style=for-the-badge" alt="RAM Budget">
  <img src="https://img.shields.io/badge/License-Source--Available-purple.svg?style=for-the-badge" alt="License">
</p>

**FLACLY** is an ultra-high-performance, bit-perfect offline FLAC music player for Android. Designed specifically for audiophiles and budget legacy hardware (DAPs, Snapdragon 480, older devices), FLACLY combines a decoupled high-speed native C++20 audio pipeline with a modern, glassmorphic UI.

---

## Key Features

- **Bit-Perfect Audio Engine:** Powered by a decoupled C++ core utilizing `miniaudio.h` (AAudio backend) and `dr_flac.h` for native decoding up to 24-bit/96kHz.
- **Glassmorphic UI:** Native XML Views layout featuring a sliding player sheet (`CoordinatorLayout` + `BottomSheetBehavior`), dynamic album artwork palette extraction (`stb_image.h`), and Namida/Tidal-style horizontal tab navigation (`ViewPager2`).
- **Low Memory Footprint:** Bypasses heavy image-loading frameworks (Glide/Coil) using custom low-res thumbnail decoders to stay under a strict **30MB RAM** budget.
- **Scoped Storage Native I/O:** Uses Android `ContentResolver` file descriptors passed directly to C++ `fdopen()` for direct disk reads without OS path restrictions.
- **Background Playback & Foreground Service:** Integrated `MediaSessionCompat`, lock-screen notification controls, dynamic audio focus handling (ducking/pausing during calls), and `AUDIO_BECOMING_NOISY` protection for headphone unplugs.
- **Fast Media Indexing:** Multi-threaded `MediaStore` parsing with background set operations (`Dispatchers.Default`) to scan, sort, and filter 600+ tracks instantly without UI freezing.

---

## Technical Stack & Architecture

┌─────────────────────────────────────────────────────────────┐
│                       ANDROID UI LAYER                      │
│        • Native Android XML Views (Glassmorphism)           │
│        • ViewPager2 + BottomSheetBehavior                   │
│        • AudioPlaybackService & MediaSessionCompat          │
└──────────────────────────────┬──────────────────────────────┘
│ JNI Bridge (File Descriptors)
┌──────────────────────────────▼──────────────────────────────┐
│                  DECOUPLED NATIVE C++ ENGINE                │
│        • audio_engine.cpp    (miniaudio.h AAudio PCM stream)│
│        • dr_flac.h           (Bit-perfect decoding)         │
│        • color_extractor.cpp (stb_image.h K-Means palette)  │
│        • Resampling Core     (Software sample rate match)   │
└─────────────────────────────────────────────────────────────┘


- **Audio Engine:** C++20 (`miniaudio.h`, `dr_flac.h`)
- **Graphics/Colors:** C++20 (`stb_image.h`)
- **Android Front-End:** Kotlin, AndroidX CoordinatorLayout, Material Components
- **Build System:** CMake, Android NDK (r25+), Gradle


   
