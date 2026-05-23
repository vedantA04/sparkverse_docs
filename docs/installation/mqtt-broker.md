---
title: MQTT Broker Setup
description: Install and run the Mosquitto MQTT broker on macOS via Homebrew.
---

# MQTT Broker Setup

SparkVerse uses **MQTT** as its communication backbone. All robot commands and pose data flow through a local **Mosquitto** broker running on your Mac.

---

## 1. Install Homebrew (if needed)

If you don't have Homebrew yet:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Verify the installation:

```bash
brew --version
```

---

## 2. Install Mosquitto

```bash
brew install mosquitto
```

This installs **Mosquitto v2.1.2** (or latest). Verify:

```bash
mosquitto -h 2>&1 | head -1
```

You should see something like:

```
mosquitto version 2.1.2
```

---

## 3. Start the Broker

Open a **dedicated terminal window** and start the broker:

```bash
/opt/homebrew/opt/mosquitto/sbin/mosquitto -c /opt/homebrew/etc/mosquitto/mosquitto.conf
```

You should see:

```
1713437136: mosquitto version 2.1.2 starting
1713437136: Config loaded from /opt/homebrew/etc/mosquitto/mosquitto.conf
1713437136: Opening ipv4 listen socket on port 1883.
1713437136: mosquitto version 2.1.2 running
```

!!! tip "Keep This Terminal Open"
    The broker runs in the foreground. **Do not close this terminal** while using SparkVerse. Open new terminal tabs/windows for other commands.

---

## 4. Verify the Broker Is Working

Open **two new terminal windows** and test the pub/sub flow:

=== "Terminal A — Subscribe"

    ```bash
    mosquitto_sub -h 127.0.0.1 -t "test/#"
    ```

=== "Terminal B — Publish"

    ```bash
    mosquitto_pub -h 127.0.0.1 -t "test/ping" -m "hello from mosquitto"
    ```

You should see `hello from mosquitto` appear in Terminal A.

!!! success "Broker is ready"
    If you see the message, your MQTT broker is working correctly. You can close Terminal A and B — they were just for verification.

---

## Alternative: Run as a Background Service

If you prefer the broker to run automatically in the background:

```bash
brew services start mosquitto
```

To stop it later:

```bash
brew services stop mosquitto
```

To check if it's running:

```bash
brew services list | grep mosquitto
```

!!! info "Foreground vs Background"
    Running in the **foreground** (the method in Step 3) is recommended during development because you can see connection logs in real-time. Use the background service for convenience once everything is stable.

---

## Mosquitto Clients (Optional but Helpful)

The `mosquitto_sub` and `mosquitto_pub` command-line tools are installed alongside the broker. They are invaluable for debugging:

```bash
# Monitor ALL MQTT traffic (very useful for debugging)
mosquitto_sub -h 127.0.0.1 -t "#" -v

# Send a test command to a specific robot
mosquitto_pub -h 127.0.0.1 -t "arena/sparknode07/cmd" -m "stop"
```

---

## What's Next

Continue to [Python Dependencies](python-dependencies.md) to set up the camera detection script.
