---
title: Python Dependencies
description: Install the Python packages required for robot_traj_http.py.
---

# Python Dependencies

The camera-based robot detection script (`robot_traj_http.py`) requires several Python packages. This page walks through installing them.

---

## Prerequisites

Ensure you have **Python 3** installed (macOS ships with Python 3 on recent versions):

```bash
python3 --version
```

You should see `Python 3.10` or higher.

---

## Install Dependencies

Install all required packages in a single command:

```bash
pip3 install opencv-python numpy
```

### Package Breakdown

| Package | Version | Purpose |
|---|---|---|
| `opencv-python` | ≥ 4.5 | Camera capture, image processing, circle/ArUco detection |
| `numpy` | ≥ 1.21 | Array operations, Kalman filter matrices |

These are the only **external** packages needed. The remaining imports used by `robot_traj_http.py` are all part of the Python standard library:

| Standard Library Module | Purpose |
|---|---|
| `cv2` | Installed via `opencv-python` above |
| `numpy` | Installed above |
| `time`, `sys`, `math` | Timing, system, trigonometry |
| `csv`, `os`, `json` | File I/O and config parsing |
| `threading` | Background HTTP server thread |
| `http.server` | Built-in HTTP server for Unity data |
| `collections.deque` | Sliding window for pose smoothing |

!!! note "Local Module: `database.py`"
    The script imports `database`, which is a local module included in the [MAPF_sparkbug](https://github.com/AndyLi-26/MAPF_sparkbug) repository alongside `robot_traj_http.py`. No separate installation is needed — just run the script from the `MAPF_sparkbug/` directory so Python can find it.

    `database.py` is a **De Bruijn sequence codebook** used for robot identification. Each SparkNode has a ring of **16 RGB LEDs** on top, and each robot is assigned a unique 16-element colour sequence (Cyan / Magenta / Yellow / Green). The overhead camera reads this LED pattern to identify which robot is which — even if robots move or rotate — by matching the observed pattern against the codebook.

---

## Verify Installation

Run a quick check to make sure everything is importable:

```bash
python3 -c "import cv2; import numpy; print(f'OpenCV {cv2.__version__}, NumPy {numpy.__version__}')"
```

Expected output:

```
OpenCV 4.11.0, NumPy 2.2.6
```

(Exact version numbers may differ — that's fine.)

---

## Camera Access

The detection script uses your Mac's camera. On first run, macOS will prompt for **camera access** — make sure to **Allow** it.

To list available cameras:

```bash
ffmpeg -f avfoundation -list_devices true -i "" 2>&1 | grep "video"
```

!!! info "Camera Index"
    By default, camera index `0` is the built-in FaceTime camera. If using an external camera (e.g. iPhone via Continuity Camera), it will typically be index `1` or `2`. You can change this in the script.

---

## What's Next

All dependencies are installed. Continue to [Running the Simulation](../getting-started/running.md) to launch everything.
