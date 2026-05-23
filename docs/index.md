---
title: Home
description: SparkVerse — A hybrid Hardware-in-the-Loop simulator for multi-robot coordination.
---

# SparkVerse

**A Hybrid Hardware-in-the-Loop Simulator for Multi-Robot Coordination**

SparkVerse is a Unity-based simulation and visualisation platform that bridges **physical robots** with **virtual digital twins** over **MQTT**. It provides a real-time bird's-eye view of an arena, tracks physical robots via an overhead camera, and lets you control the entire fleet through MQTT commands.

---

## What You'll Need

| Component | Details |
|---|---|
| **macOS** | Apple Silicon (M-series) — tested on M1 / M2 / M3 |
| **Unity** | Version **6.3 LTS** (`6000.3.2f1`) via Unity Hub |
| **Mosquitto** | MQTT broker — installed via Homebrew (`v2.1.2`) |
| **Python 3** | For running the camera-based robot detection script |
| **Overhead Camera** | iPhone or webcam for physical robot tracking |

---

## Quick Navigation

<div class="grid cards" markdown>

-   :material-download:{ .lg .middle } **Installation**

    ---

    Set up Unity, the MQTT broker, and Python dependencies.

    [:octicons-arrow-right-24: Installation Guide](installation/index.md)

-   :material-rocket-launch:{ .lg .middle } **Getting Started**

    ---

    Launch the simulation, load a robot config, and start sending MQTT commands.

    [:octicons-arrow-right-24: Running the Simulation](getting-started/running.md)

</div>

---

## Repository

| Repository | Link |
|---|---|
| **SparkVerse** (Unity project) | [github.com/vedantA04/sparkverse](https://github.com/vedantA04/sparkverse) |
| **MAPF SparkBug** (Vision & Control) | [github.com/AndyLi-26/MAPF_sparkbug](https://github.com/AndyLi-26/MAPF_sparkbug) |
