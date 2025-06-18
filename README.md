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

Copyright © 2025 Jordan Williams . All rights reserved.

This software and associated documentation are the proprietary intellectual property of the author. Unauthorized copying, distribution, modification, reverse engineering, or use of any part of this project in any form is strictly prohibited without prior written permission.

The concepts, design, architecture, and implementation details of this system are protected and may not be reproduced or repurposed for commercial or academic use without express consent.

For licensing inquiries or collaboration opportunities, please contact the author directly. This project is proprietary and may not be copied, modified, or distributed without explicit permission from the author.
