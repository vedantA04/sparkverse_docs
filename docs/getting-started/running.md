---
title: Running the Simulation
description: Step-by-step guide to launching SparkVerse and controlling robots.
---

# Running the Simulation

This guide assumes you've completed all three [installation steps](../installation/index.md). You'll need **three terminal windows** plus the Unity editor.

---

## Step 1 — Start the MQTT Broker

Open a terminal and start Mosquitto:

```bash
/opt/homebrew/opt/mosquitto/sbin/mosquitto -c /opt/homebrew/etc/mosquitto/mosquitto.conf
```

Or, if you set it up as a background service:

```bash
brew services start mosquitto
```

!!! warning "Keep the broker running"
    The MQTT broker must stay running for the entire session. Everything — Unity, the camera script, your control commands — communicates through it.

---

## Step 2 — Start the Camera Detection Script (Optional)

If you have physical robots to track with an overhead camera, open a **new terminal**:

```bash
cd MAPF_sparkbug
python3 robot_traj_http.py
```

This launches the vision pipeline and a local HTTP server on port `8000` that Unity polls for real robot positions.

!!! info "No physical robots?"
    If you're running a **simulation-only** session (no camera, no physical robots), you can skip this step entirely. The Unity sim works standalone with virtual robots.

---

## Step 3 — Prepare the Robot Configuration File

Create a JSON file that defines the robots and arena layout. This file tells Unity **how many robots to spawn** and **where to place them**.

### Example: `robot_config.json`

```json
{
  "robots": [
    { "id": 1001, "name": "Robot_01", "x": -1.0, "z": -1.0 },
    { "id": 1002, "name": "Robot_02", "x": -0.5, "z": -1.0 },
    { "id": 1003, "name": "Robot_03", "x":  0.0, "z": -1.0 },
    { "id": 1004, "name": "Robot_04", "x":  0.5, "z": -1.0 },
    { "id": 1005, "name": "Robot_05", "x":  1.0, "z": -1.0 },
    { "id": 1006, "name": "Robot_06", "x": -1.0, "z":  1.0 },
    { "id": 1007, "name": "Robot_07", "x": -0.5, "z":  1.0 },
    { "id": 1008, "name": "Robot_08", "x":  0.0, "z":  1.0 },
    { "id": 1009, "name": "Robot_09", "x":  0.5, "z":  1.0 },
    { "id": 1010, "name": "Robot_10", "x":  1.0, "z":  1.0 }
  ],
  "arena": {
    "sizeX": 3.0,
    "sizeZ": 3.0,
    "originX": 0.0,
    "originY": 0.0,
    "originZ": 0.0
  }
}
```

### JSON Structure Reference

**Per-robot fields:**

| Field | Type | Description |
|---|---|---|
| `id` | int | Unique robot identifier (convention: ≥ 1000) |
| `name` | string | Display name (for debugging) |
| `x` | float | X position in world space (metres) |
| `z` | float | Z position in world space (metres) |

**Arena fields:**

| Field | Type | Description |
|---|---|---|
| `sizeX` | float | Arena width (metres) |
| `sizeZ` | float | Arena depth (metres) |
| `originX/Y/Z` | float | Centre point of the arena |

!!! tip "Copy the file path"
    You'll need the **full file path** of this JSON file in the next step.
    Select the file in **Finder** and press ++opt+cmd+c++ to copy its path to the clipboard.

---

## Step 4 — Launch the Unity Simulation

1. Open the SparkVerse project in Unity (if not already open)
2. Press the **Play** button (▶) at the top of the editor

Once the simulation starts, you'll be prompted to configure it:

### 4a. Load the Robot Configuration

You have two options to provide the JSON file:

=== "Paste the Path"

    1. Click in the **text field** that appears in the game view
    2. Paste the file path (++cmd+v++) — the one you copied with ++opt+cmd+c++ in the previous step
    3. Press ++enter++

=== "Browse for the File"

    1. Click the **"Browse…"** button in the game view
    2. Navigate to your `robot_config.json` file
    3. Select it and confirm

### 4b. Enter the MQTT Broker Address

Fill in the MQTT broker address field using the format:

```
address:port
```

For a broker running locally on your Mac:

```
127.0.0.1:1883
```

!!! info "The default Mosquitto port is `1883`"
    If you haven't changed the Mosquitto configuration, use `127.0.0.1:1883`.

### 4c. Start the Simulation

Click the **Start** button (or equivalent) in the game UI. The robots should appear at their configured positions in the arena.

---

## Step 5 — Navigate the View

Once the simulation is running, use these controls to navigate the camera:

### Camera Controls

| Action | Keys |
|---|---|
| **Zoom in** | ++up++ |
| **Zoom out** | ++down++ |
| **Pan right** | ++right++ |
| **Pan left** | ++left++ |
| **Rotate view** | ++shift+arrow-up++ / ++shift+arrow-down++ / ++shift+arrow-left++ / ++shift+arrow-right++ |

!!! warning "View Rotation — Work in Progress"
    The ++shift++ + arrow key rotation controls are experimental and may behave unexpectedly. Stick to zoom and pan for now.

---

## Step 6 — Send MQTT Commands

With the broker running and Unity connected, you can control robots by publishing MQTT messages from **any terminal**.

### Example Commands

```bash
# Send a velocity command to a robot
mosquitto_pub -h 127.0.0.1 -t "arena/sparknode01/cmd" \
  -m "drive forward 32 3000"

# Stop a specific robot
mosquitto_pub -h 127.0.0.1 -t "arena/sparknode01/cmd" \
  -m "stop"

# Monitor all MQTT traffic (useful for debugging)
mosquitto_sub -h 127.0.0.1 -t "#" -v
```

!!! danger "Unity Window Must Be Active"
    The Unity game window **must be in focus** (the active window) for robot movement to be visible. If robots aren't moving, click on the Unity Game view to bring it to the foreground.

---

## Summary: What Should Be Running

At this point you should have:

| Terminal / App | What's Running | Status |
|---|---|---|
| **Terminal 1** | `mosquitto` broker | :material-check-circle:{ .green } Running (keep open) |
| **Terminal 2** | `robot_traj_http.py` | :material-check-circle:{ .green } Running (optional — only for physical robots) |
| **Terminal 3** | Your working terminal | For `mosquitto_pub` / `mosquitto_sub` commands |
| **Unity Editor** | SparkVerse simulation | :material-check-circle:{ .green } Playing |

---

## Troubleshooting Quick Fixes

| Problem | Fix |
|---|---|
| Robots don't appear | Check the JSON file path was entered correctly |
| Robots don't move | Make sure the **Unity game window is active** (in focus) |
| "Connection refused" in Unity | Verify the broker is running and the address is `127.0.0.1:1883` |
| Camera script crashes | Check Python dependencies are installed (`pip3 install opencv-python numpy`) |
| Wrong camera selected | Change the camera index in `robot_traj_http.py` (default is `0`) |
