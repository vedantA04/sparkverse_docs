---
title: Unity Setup
description: Clone the SparkVerse Unity project and open it in Unity Hub.
---

# Unity Setup

## 1. Install Unity Hub

If you don't already have Unity Hub installed:

1. Download **Unity Hub** from [unity.com/download](https://unity.com/download)
2. Install and open Unity Hub
3. Sign in with your Unity account (a free Personal licence is fine)

---

## 2. Install the Correct Unity Version

SparkVerse requires **Unity 6.3 LTS** (version `6000.3.2f1`).

In Unity Hub:

1. Go to **Installs** → **Install Editor**
2. Select **Unity 6.3 LTS (6000.3.2f1)**
3. Under modules, ensure at minimum:
    - **Mac Build Support** is checked
4. Click **Install** and wait for the download to complete

!!! warning "Version Matters"
    The project has been built and tested on `6000.3.2f1`. Using a different major version may cause shader, prefab, or API compatibility issues. Always use this exact version.

---

## 3. Clone the Repository

Open a terminal and clone the SparkVerse Unity project:

```bash
git clone https://github.com/vedantA04/sparkverse.git
```

This repository includes everything you need:

| What's Included | Description |
|---|---|
| **Prefabs** | Pre-configured robot models for the arena |
| **C# Scripts** | All game logic — robot spawning, MQTT handling, camera control |
| **M2MQTT Library** | The MQTT client library for Unity (already integrated, no separate install needed) |
| **Scene Files** | The main simulation scene, ready to run |

---

## 4. Open the Project in Unity Hub

1. In Unity Hub, click **Projects** → **Open** (or **Add**)
2. Navigate to the cloned `sparkverse` folder and select it
3. Unity Hub will detect the project and associate it with Unity `6000.3.2f1`
4. Click to open — the first import may take a few minutes as Unity compiles shaders and imports assets

!!! tip "First Launch"
    The initial open will show a progress bar as Unity imports all assets and recompiles scripts. This is normal and can take 3–5 minutes depending on your machine. Subsequent opens will be much faster.

---

## 5. Verify the Project

Once the editor finishes loading:

1. In the **Project** panel, you should see folders for `Scripts`, `Prefabs`, `Scenes`, etc.
2. The project contains **two scenes**:

    | Scene | Purpose |
    |---|---|
    | **Start Scene** | Entry point — handles configuration loading (JSON file, MQTT address) |
    | **Simulation Scene** | The main arena with robot spawning, camera, and MQTT control |

3. You should see the arena floor and camera setup in the **Scene** view

### Set the Start Scene as the First Scene

The simulation **must** be launched from the **Start Scene**. You can either:

=== "Set it in Build Settings (Recommended)"

    1. Go to **File → Build Settings** (++cmd+shift+b++)
    2. Drag the **Start Scene** to the top of the **Scenes in Build** list (index 0)
    3. Drag the **Simulation Scene** below it (index 1)
    4. Close Build Settings

    Now pressing **Play** from the Start Scene will flow correctly into the Simulation Scene.

=== "Run Manually Each Time"

    1. In the **Project** panel, navigate to `Assets/Scenes/`
    2. Double-click the **Start Scene** to open it
    3. Press **Play** (▶)

    The Start Scene will load the configuration and transition to the Simulation Scene automatically.

!!! warning "Don't start from the Simulation Scene directly"
    If you press Play while the Simulation Scene is open, the configuration step will be skipped and robots won't spawn correctly. Always start from the **Start Scene**.

!!! note "No Errors?"
    Check the **Console** panel (++cmd+shift+c++) — there should be no red errors. Yellow warnings about deprecated APIs are generally safe to ignore.

---

## What's Next

The Unity project is ready, but it needs an MQTT broker to communicate. Continue to [MQTT Broker Setup](mqtt-broker.md).
