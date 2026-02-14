# 🔐 Smart Face Lock System – Distributed Vision Control

The **Smart Face Lock System** is a distributed real-time access control solution that uses advanced face detection and recognition to control a servo-based locking mechanism. The system integrates computer vision, MQTT messaging, WebSocket communication, and a modern responsive dashboard to provide secure and scalable access management.

The architecture separates responsibilities across multiple components to ensure security, modularity, and maintainability:

- **Vision Node (PC)** → Face detection & recognition
- **ESP8266 (Edge Controller)** → Servo motor lock control
- **Backend (PC)** → MQTT → WebSocket relay
- **Web Dashboard** → Real-time monitoring interface

The system supports **team-based topic isolation** for multi-team environments and includes enhanced security measures.

---

# 🏗️ System Architecture

[ PC - Vision Node ]
|
| MQTT (vision/team02/movement)
v
[ PC - Backend (WebSocket Relay) ]
|
| WebSocket (ws://localhost:9002)
v
[ Modern Browser Dashboard ]

AND

[ ESP8266 Edge Controller ]
|
| MQTT (vision/team02/movement)
v
[ Servo Motor - Lock Mechanism ]

### 🚀 Golden Rule

- Vision detects and recognizes
- Devices communicate via MQTT
- Browsers connect via WebSocket
- Backend handles secure real-time relay

---

# 📁 Project Structure

face-lock-mqtt/
│
├── vision-node/
│ └── vision_node.py
│
├── backend/
│ └── backend.py
│
├── esp8266/
│ └── main.py
│
├── dashboard/
│ └── index.html
│
└── README.md

---

# 🌐 Dashboard URL

## 🔗 Live Dashboard

http://157.173.101.159:8369/

Make sure:

- The Python HTTP server is running
- Port **8369** is open
- Backend WebSocket service is running on port **9002**

---

# ⚙️ Setup Instructions

## 1️⃣ Install Python Dependencies (Vision Node + Backend)

```bash
pip install opencv-python paho-mqtt websockets asyncio numpy



2️⃣ Install and Start MQTT Broker
Windows
mosquitto.exe -v

Linux
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl start mosquitto

3️⃣ ESP8266 Setup

Flash MicroPython using Thonny IDE or ampy

Connect servo motor to GPIO5 (D1)

Upload and run main.py

Update broker IP address in the script

⚙️ Configuration

Each team must use a unique team ID:

TEAM_ID = "team02"
MQTT_TOPIC = f"vision/{TEAM_ID}/movement"


Component Roles

Vision Node → Publishes face detection & movement

ESP8266 → Subscribes and controls servo

Backend → Relays MQTT → WebSocket

Dashboard → Displays live system status

🔒 Always use team-specific topics to prevent cross-team interference.

🚀 Launch Procedure
Start MQTT Broker
# Windows
mosquitto.exe -v

# Linux
sudo systemctl start mosquitto

Start Backend
cd backend
python backend.py

Start Vision Node
cd vision-node
python vision_node.py

Start Dashboard Server
python3 -m http.server 8369 --directory dashboard


Then open in browser:

http://157.173.101.159:8369/


Ensure dashboard connects to:

const ws = new WebSocket("ws://localhost:9002");

💓 Advanced Features
Heartbeat Monitoring

Topic:

vision/team02/heartbeat


Example payload:

{
  "node": "pc",
  "status": "ONLINE",
  "timestamp": 1730000000,
  "confidence": 0.95
}

🔒 Security Considerations

Use encrypted MQTT (TLS) in production

Implement broker authentication

Monitor logs for unauthorized access

Keep strict recognition thresholds

Avoid direct PC ↔ ESP communication

Prevent Dashboard ↔ MQTT direct access

Always route traffic through Backend

⚙️ Technical Optimization

Use dead-zone threshold to prevent servo jitter

Limit message rate to 10Hz

Smooth servo movement (2–5° increments)

Test locally before deployment

📦 System Requirements
Software

Python 3.10+

OpenCV

Paho-MQTT

Websockets

NumPy

Mosquitto MQTT Broker

MicroPython (ESP8266)

Modern Web Browser

Hardware

ESP8266 Microcontroller

SG90 Servo Motor

USB Camera / Webcam

🎯 Key Features

Real-time face detection with confidence scoring

Unknown person detection

Multi-person tracking

Distributed modular architecture

Topic isolation

Modern responsive dashboard

Team-based access control

🏁 Operational Workflow
Phase 1 – Open Loop Testing

Camera → MQTT → ESP8266 → Servo → Dashboard Update

Phase 2 – Closed Loop Tracking

Camera mounted on servo → Real-time tracking → Automatic face following

Real-Time Flow

Face Detection → Recognition → Decision → Servo Control → Dashboard Update

⚠️ Important Notes

Do NOT allow direct PC ↔ ESP connection

Do NOT allow direct Dashboard ↔ MQTT access

Always use Backend relay for security

Monitor system health via heartbeat messages
```
