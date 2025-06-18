
# LandMark AI — Capstone Project

## Overview

This project simulates a real-world autonomous drone landing decision system. It is broken into three core modules:

* **AI/ML:** Determines safe landing zones via image analysis and outputs a 5x5 grid of probabilities
* **Embedded System (RTOS):** Handles MAVLink telemetry, simulates decision-making, and controls drone behavior
* **Web App:** Visualizes drone state, telemetry, and ML predictions in real time

Ultimately, this project is designed to simulate a complete autonomy stack that can later be ported to real hardware. The system targets real-world business challenges such as enabling safe autonomous drone operations in environments without GPS guidance, supporting emergency landings, and enhancing decision-making for last-mile delivery, search-and-rescue, and agricultural inspection use cases.

---

## 1. System Architecture

### High-Level Data Flow

```
Gazebo + PX4 SITL
   ⇅ (MAVLink UDP)
RTOS (simulated in C)
   ⇅ (socket / simulated UART)
Comms Board (simulated in C)
   ⇅ (HTTP/JSON)
Python ML Server
   ⇅ (WebSocket/HTTP)
React/Next.js UI
```

### Module Responsibilities

| Module       | Responsibility                                               |
| ------------ | ------------------------------------------------------------ |
| PX4 + Gazebo | Simulate drone physics, sensor fusion, generate MAVLink data |
| RTOS         | Parse MAVLink, manage telemetry, decision loop               |
| Comm Board   | Handle networking, route data to ML and UI                   |
| ML Server    | Classify image into safe/unsafe landing grid                 |
| UI           | Display drone telemetry, grid decision, and status           |

---

## 2. AI/ML Component

* **Language:** Python
* **Input:** Top-down drone image (real aerial images, not from Gazebo)
* **Processing:** Image is split into 25 grid tiles (5x5), each scored by the ML model
* **Output:** 5x5 grid of probabilities (e.g., \[0.1, 0.2, ..., 0.9])
* **Decision Logic:** If any grid score exceeds threshold (e.g., 0.8), select highest. Otherwise, abort landing.

This ML component solves key business problems such as:

* **Emergency landing assistance** when operators are unavailable or connectivity is weak
* **Real-time site evaluation** in inaccessible terrain (e.g., post-disaster zones or remote agriculture fields)
* **Improved landing safety** in dynamic or obstacle-dense areas without relying on constant human input

### Dataset

* Source: Real-world aerial images (not synthetic)
* Goal: Train model once so that real drone camera input later won't require retraining

---

## 3. RTOS / Embedded System (Simulated in C)

* **Core Scheduler:** Cooperative or priority-based task loop
* **Key Tasks:**

  * `MAVLinkParserTask`: Decode telemetry from PX4 SITL
  * `TelemetryTask`: Maintain latest drone state
  * `DecisionTask`: Receive ML result and select landing action
  * `FlightControlTask`: Send MAVLink commands back to PX4 (e.g. LAND)
  * `EmergencyMonitorTask`: Detect errors, wind gusts, or override triggers
* **Telemetry:** Reads fields like pitch, yaw, velocity, position, etc.

---

## 4. Communication Board (C)

* Simulates a Jetson Nano, Raspberry Pi, or ESP32
* Receives telemetry from RTOS
* Sends telemetry + image to ML Server
* Sends ML result back to RTOS
* Streams telemetry and prediction to UI via WebSocket

---

## 5. Web Application (React/Next.js)

* Displays:

  * Live telemetry (altitude, pitch, heading)
  * 5x5 grid overlay on image
  * Drone vector (position and movement)
  * Landing decision + status (e.g., "Success" or "Aborted")
* Accepts control triggers (Land, Abort, Simulate Wind Gust)

---

## 6. Hardware Expansion Plan

Once simulation is validated, the system will be ported to real hardware. This will enable use in commercial drone platforms for logistics, infrastructure inspection, and emergency operations.

| Simulation Component  | Real Hardware Equivalent            |
| --------------------- | ----------------------------------- |
| PX4 SITL + Gazebo     | Flight controller with PX4 on STM32 |
| RTOS in C (Mac)       | Custom scheduler on STM32           |
| Comm Board (C on Mac) | ESP32 / Jetson Nano / Raspberry Pi  |
| Python ML Server      | Jetson Nano or RPi with TFLite      |
| Web App               | Unchanged — runs in browser         |

---

## 7. Readme Summary (for GitHub)

```markdown
# LandMark AI

This project simulates an embedded drone landing decision pipeline built around PX4 + Gazebo, an ML image classifier, and a telemetry-driven UI.

## Modules
- **AI/ML:** Python ML model classifies 5x5 landing grid from real aerial images
- **Embedded/RTOS:** C-based scheduler parses MAVLink telemetry and issues flight decisions
- **Web App:** React-based UI shows drone movement, grid overlay, and final outcome

## Usage
1. Run PX4 SITL with Gazebo to simulate drone behavior
2. Start RTOS simulator to parse MAVLink
3. Start Comm Board server to relay data
4. Run ML Flask app for grid scoring
5. Start the React UI to observe decisions

## Future Deployment
- RTOS and Comm logic ported to STM32 and ESP32 respectively
- ML model deployed via TFLite on Jetson or Pi
- Fully autonomous landing behavior tested on physical drone
- Intended use cases: emergency landings, inspection drones, precision logistics

## License

Copyright © 2025 [Your Name or Organization]. All rights reserved.

This software and associated documentation are the proprietary intellectual property of the author. Unauthorized copying, distribution, modification, reverse engineering, or use of any part of this project in any form is strictly prohibited without prior written permission.

The concepts, design, architecture, and implementation details of this system are protected and may not be reproduced or repurposed for commercial or academic use without express consent.

For licensing inquiries or collaboration opportunities, please contact the author directly. This project is proprietary and may not be copied, modified, or distributed without explicit permission from the author.
```

---

## Conclusion

This capstone simulates a full-stack drone autonomy architecture with real-world ML, embedded systems, and UI integration. The modular design allows direct transition from simulation to hardware — and reflects industry standards in UAV development.

The project emphasizes real telemetry, safe AI-assisted decision making, and clear human-machine interaction — forming a robust foundation for solving business-critical problems such as landing drones in emergency situations, minimizing crash risk in dynamic zones, and reducing operator workload in mission-critical deployments.

