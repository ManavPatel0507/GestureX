# GestureX
Hands-free computer control system using eye tracking and different gestures.

# 👁️ Gaze-Based Hands-Free Cursor Control System

## Overview

This project implements a hands-free human–computer interaction system that allows users to control the mouse cursor using eye gaze and facial gestures captured through a standard webcam. The system is designed to reduce dependency on traditional input devices such as a mouse or keyboard and can be especially useful for accessibility-focused applications.

The system tracks eye movements in real time, maps gaze direction to screen coordinates using a multi-stage calibration process, and performs mouse click actions through intentional eye winks. All processing is done locally using computer vision techniques.

---

## Key Features

- Real-time eye gaze tracking using a webcam  
- Smooth and stable cursor movement using adaptive filtering  
- Multi-phase calibration for improved accuracy  
- Left and right mouse click using eye winks  
- Face presence and lighting validation  
- Universal exit shortcut (CTRL + Q)  
- No additional hardware required  

---

## Project Flow

The project works in three mandatory phases that must be executed in order:

1. Alignment  
2. Calibration  
3. Cursor Control  

---

## Phase 1: Alignment

The alignment phase ensures that the user is positioned correctly in front of the camera before calibration begins.

During this phase:
- The webcam feed is displayed in real time
- The system detects the user’s face
- Face size and brightness are checked to ensure proper distance and lighting
- A visual indicator confirms whether alignment is acceptable

Once the face is properly aligned and detected under sufficient lighting conditions, the user presses **`S`** to proceed to calibration.

---

## Phase 2: Calibration

The calibration phase learns the relationship between eye position and screen position. This is the most important phase for accuracy.

Calibration is performed using multiple sampling strategies:

- **Grid Sampling (4×4)**  
  The user follows a red target displayed at 16 evenly spaced screen positions. Multiple eye samples are collected at each point.

- **Edge and Drag Sampling**  
  The user follows smooth movements from the center of the screen to the edges. This captures dynamic gaze behavior and improves cursor stability.

- **Corner Sampling**  
  Additional samples are collected at all four screen corners to improve boundary accuracy.

After data collection:
- A polynomial regression model maps eye coordinates to screen coordinates
- Axis inversion is automatically corrected using correlation analysis
- The trained mapping is saved for later use

---

## Phase 3: Cursor Control

In cursor control mode, the trained gaze model is used for real-time interaction.

### Cursor Movement
- Iris centers are detected from both eyes
- Gaze position is predicted using the calibrated mapping
- Exponential Moving Average (EMA) smoothing ensures stable and natural cursor movement
- The system cursor moves continuously based on gaze direction

### Click Actions (Wink Detection)

Mouse clicks are performed using intentional eye winks:

- Left eye wink → Left mouse click  
- Right eye wink → Right mouse click  

The system uses Eye Aspect Ratio (EAR) and differential eye movement to distinguish winks from natural blinking. Cooldown timing prevents accidental repeated clicks.

---

## Controls

- `1` → Alignment mode  
- `2` → Calibration mode  
- `3` → Cursor control mode  
- `CTRL + Q` → Universal exit from any mode  
- `Q` → Quit current mode  

---

## Requirements

Install all dependencies using:

```bash
pip install mediapipe opencv-python numpy pyautogui scipy pygetwindow keyboard
