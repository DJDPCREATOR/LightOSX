# 🎛️ LightOSX by DJDP

### **100% Offline, Single-File HTML5 DMX & MIDI Performance Controller**

LightOSX is an ultra-lightweight, high-performance DMX controller built entirely in vanilla JavaScript, CSS, and HTML. Operating completely client-side with zero external frameworks, dependencies, or network telemetry, it brings instant, low-overhead hardware show control directly to modern web browsers via native Web APIs. 

---

## 🚀 Live Zero-Install Demo
Experience the interface immediately with zero setup. Run the application directly in your browser using the permanent sandbox link hosted natively via GitHub Pages:

👉 **[Launch LightOSX Live Beta Sandbox](https://github.io)**

> 💾 **Want to run it 100% offline?** Simply press `Ctrl + S` (or `Cmd + S`) on the demo page to save the standalone `.html` file directly to your desktop or root directory. Double-click it to operate with absolute autonomy under zero-network constraints.

---

## 💎 Core Architecture & Architectural Achievements

*   **⚡ Zero-Bloat Single-File Runtime:** Built completely from scratch without React, Vue, jQuery, or Node modules. The entire operational application—including the 6-band EQ engine, layout framework, patch matrix, and binary protocol serializers—is packaged inside one clean index file.
*   **🔊 Isolated DJ Soundcard Multi-Channel Routing:** Engineered specifically for live DJ workflows. Features localized channel isolation allowing you to split and map input streams. Safely route master audio reactions through discrete hardware ports (e.g., Pioneer DDJ-FLX4 RCA arrays) while isolating aux inputs or booth cue feeds via your laptop's integrated headphone jacks.
*   **🎚️ Adaptive 64-Pad Performance Matrix:** Features a responsive 8×4 dual-bank (Bank A/B) grid natively bound to conflict-free hotkey mappings (`1–8`, `Q–I`, `A–K`, `Z–,`). Buttons dynamically scale to fit your current browser bounds without requiring scrolling. Supports instant per-pad toggling between transient **HOLD** and latched **LATCH** configurations.
*   **📈 Active 250ms Audio BPM Tracking Engine:** Incorporates a low-latency real-time log spectrum frequency analyzer that samples signal flux every 250ms for automated tempo locks. Includes an intelligent **Auto-Blackout Gate** that cuts DMX transmission instantly when silent threshold windows are breached.
*   **🧬 Zero-Overhead JSON Portability:** Eliminates bulky database management systems. Your complete user state—including active patch maps, custom fixture profiles, sync groups, MIDI bindings, and generated chase pads—saves directly to a single standalone JSON file on your machine.

---

## 📟 Hardware Protocol Implementation & Drivers

LightOSX utilizes native browser drivers to control multiple continuous universes simultaneously without intermediate wrappers:

1.  **FTDI Open DMX & Serial Bridges:** Direct bit-banging transmission of standard DMX512 frames (250,000 baud, 8 data bits, 2 stop bits) using the browser's native **Web Serial API**. Manages precise microsecond-level clock margins to assert strict Break and Mark-After-Break (MAB) timings natively.
2.  **DMXking / Enttec PRO Protocols:** Communicates directly with advanced hardware microcontrollers using FTDI and Microchip serial translation layers (57,600 / 115,200 baud packet encapsulation).
3.  **uDMX / Anyma:** Native endpoint access utilizing raw bulk transfers via the **WebUSB API**.
4.  **Art-Net Node Bridges:** Bridges lighting data via low-latency HTTP packets directly into a lightweight network translation gateway (`bridge.py`) to bypass traditional browser UDP communication limits.

---

## 🎨 Out-of-the-Box Fixture Compatibility

Includes optimized default profiles with infinite expansion via the built-in **Custom Profile Builder**:
*   **Color Cans & Bars:** Standard 3-channel RGB, 4-channel RGBW, and advanced 6/8/10-channel PAR wash systems (with native macro mapping and custom color palettes).
*   **Moving Heads & Effects:** Fully maps Pan/Tilt vectors, fine motor tracking, color wheels, gobo indices, prisms, shutter triggers, laser projections, and automated fog/CO2 arrays.
*   **Sync Groups:** Includes an unconditional sync-mirror engine. Drag any card onto an active group boundary to instantly echo parameters across unlimited matching hardware configurations.

---

## ⚙️ Advanced Technical Configuration

Advanced users can calibrate microsecond frame intervals directly from the status footer:
*   **Break Space:** Adjust target microsecond boundaries (Default: `100 µs`, range: `88–3000 µs`).
*   **MAB Margin:** Fine-tune Mark-After-Break timing (Default: `12 µs`, range: `8–3000 µs`).
*   **Frame Trimming:** Switch between full `512-byte` transmissions or dynamically trimmed packets optimized to reduce processing strain on budget hardware interfaces.

---

## 🛠️ Contributing & Fixture Submissions

We welcome architectural tweaks, performance benchmarks, and hardware reports!
*   **Bug Reports:** If your browser serial line hangs or gets blocked by specialized permissions, please submit an issue utilizing our template with your OS and browser details.

---
Developed by **DJDP** · Released under the Open Source MIT License. Fully offline, lightweight performance architecture.
