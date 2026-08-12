# 🏓 Insta Ping Pong Bot (Scrcpy + OpenCV)

An automated, real-time computer vision and physics-assisted paddle bot designed for the **Instagram Direct Message Ping-Pong minigame**, mirrored on Windows via **[Scrcpy](https://github.com/Genymobile/scrcpy)**.

It combines **HSV Color Tracking**, **Trajectory Reflection Prediction**, and a **Local Virtual Wall (Micro-Jitter)** mechanism to compensate for input latency, network latency, and frame stutters.

---

## ✨ Key Features

- **📱 Scrcpy Auto-Detection:** Automatically hooks into active Android mirror windows on Windows using Win32 APIs.
- **🔴 Red Ball Tracking:** Highly optimized HSV range mask specifically calibrated for detecting **RED** target balls.
- **📐 Trajectory & Wall Reflection Prediction:** Calculates velocity vectors (`v_x`, `v_y`) to predict impact positions while accounting for wall reflections.
- **🛡️ Hybrid Local Virtual Wall (Micro-Jitter):** Rapidly sweeps across a `±30 px` range around the predicted target at `1 ms` intervals. This effectively increases the paddle's coverage area and helps compensate for input latency.
- **🖱️ Automatic Touch-Hold Simulation:** Toggling modes automatically triggers Win32 `mouse_down` events to drag the paddle without requiring manual mouse clicks.
- **👁️ Real-Time OpenCV HUD:** Displays target centroids, trajectory impact markers, and local wall shield boundaries in real time.

---

## 🛑 Prerequisites & Requirements

### 1. Android Mirroring (Scrcpy)

The game **must** be mirrored using **Scrcpy** in portrait mode on Windows.

### 2. Red Ball Requirement

The current HSV mask is specifically calibrated for **RED** target balls.

If your game or theme uses a different color (e.g. yellow, blue, or white), update the HSV threshold variables:

```python
lower_red
upper_red
```

inside the script.

### 3. Operating System

The bot is designed for **Windows OS** and relies on Win32 API functions for low-level mouse automation and window management.

---

## 📦 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/eyupensrr/insta-ping-pong-bot.git
cd insta-ping-pong-bot
```

### 2. Install Python Dependencies

Make sure you have **Python 3.8+** installed, then run:

```bash
pip install opencv-python numpy mss
```

---

## 🎮 How to Run & Controls

### 1. Start Scrcpy

Connect your Android device via USB or Wi-Fi and start mirroring it using **Scrcpy**.

### 2. Launch the Ping-Pong Minigame

Open an Instagram DM, send a single 🏓 emoji, and tap it to launch the Ping-Pong minigame.

### 3. Run the Bot

```bash
python hybrid_bot.py
```

### 4. Keyboard Controls

| Key | Action |
| --- | --- |
| **`Z`** | Toggle **NORMAL MODE** — tracks the current ball X position + Local Wall |
| **`X`** | Toggle **AGGRESSIVE MODE** — predicts the impact point + wall reflections + Local Wall |
| **`UP` Arrow** | Fine-tune the paddle alignment line upward |
| **`DOWN` Arrow** | Fine-tune the paddle alignment line downward |
| **`Q`** | Exit the bot and release the mouse drag |

---

## 🔬 How the Hybrid Local Wall Works

High-speed mobile game bots can fail due to:

1. Frame drops caused by USB/Wi-Fi screen mirroring.
2. Sudden velocity changes when the ball bounces off walls multiple times.
3. Input latency between the computer and the mirrored Android device.

The **Hybrid Local Wall** mechanism helps compensate for these issues by rapidly sweeping around the target X coordinate.

The sweep range is defined as:

$$
\text{Sweep Range} =
[\text{Target}_X - \text{Jitter Offset},
\text{Target}_X + \text{Jitter Offset}]
$$

When active, the bot rapidly oscillates the cursor across this range using approximately `1 ms` intervals.

This effectively transforms a single cursor position into a wider moving coverage area around the predicted landing point, increasing the chance of intercepting the ball despite small timing or positioning errors.

---

## ⚠️ Known Limitations & Performance Notes

- **🏆 High-Score Potential:** The bot has achieved scores of **36+** in fast-paced games.
- **📉 Frame-Drop Sensitivity:** At extreme ball speeds, sudden frame drops can cause the calculated velocity vector to change significantly between frames.
- **🎯 Position Skips:** These frame drops may result in brief prediction or position skips.
- **⚙️ Jitter Adjustment:** Increasing or decreasing `JITTER_OFFSET` may help depending on performance.
- **🚀 Scrcpy Performance:** Increasing the Scrcpy frame rate can reduce tracking issues:

```bash
scrcpy --max-fps=60
```

Actual performance depends on the device, connection type, screen resolution, Scrcpy configuration, and system load.

---

## 📜 License

This project is open-source and distributed under the **MIT License**.