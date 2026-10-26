# AAROH — Mine Intelligence Center

**Adaptive AI-Based Response & Operations for Hazards**
*SENSE • PREDICT • VERIFY • RESPOND*

Smart India Hackathon 2026 · Problem Statement **SIH26025**
AI-enabled, low-cost, real-time subsidence monitoring, prediction and early warning for Indian underground coal mines.

---

## 1. Overview

AAROH is a command-center dashboard for underground mine safety teams. It shows one connected story in a single view:

1. **Sense.** A mesh of low-cost sensor nodes (ESP32) reports displacement, tilt, vibration, strain, temperature and battery.
2. **Predict.** An AI risk engine scores each zone from 0 to 100, forecasts the next 30–60 minutes, and explains *why* risk is rising.
3. **Verify.** An autonomous rover (R01) with LiDAR, ROS 2 and SLAM is dispatched to confirm the anomaly physically.
4. **Respond.** Alerts, an incident record, an early-warning ladder and a response checklist support the humans who make the final call.

> AAROH is a decision-support system. It does not claim to evacuate, shut down or take any real-world action automatically. Every early-warning state is advisory and requires human confirmation.

The deliverable `aaroh-dashboard.jsx` is a single-file React build. It runs directly as a Claude artifact and serves as a structured prototype for the production codebase.

---

## 2. Quick Start

### Demo login

| Field | Value |
|---|---|
| Email / Employee ID | `arjun.kumar@aaroh.in` |
| Password | `aaroh2026` |

After sign-in you choose a role. The dashboard then opens on the **live incident snapshot**: N05 is at high risk in Panel B-04, R01 is inspecting, and LiDAR is active.

### Running the demo

| Action | How |
|---|---|
| Incident simulation | Simulation panel → **START INCIDENT SIMULATION** |
| Return to live snapshot | Simulation panel → **LIVE** |
| Judge Mode | Header → **JUDGE MODE** |
| Global search | Press `/`, then try `N05`, `B-04`, `INC-2026-004`, `R01` |
| Close drawers or dialogs | `Esc` |
| Switch role | Profile menu (top right) → role list |
| Notification center | Bell icon (unread badge) |

### Running locally

The file expects React 18, Tailwind CSS, `three` (r128+), `recharts` and `lucide-react`.

```bash
npm create vite@latest aaroh -- --template react
cd aaroh
npm i three recharts lucide-react
npm i -D tailwindcss postcss autoprefixer && npx tailwindcss init -p
# copy aaroh-dashboard.jsx into src/ and render <App /> from main.jsx
npm run dev
```

---

## 3. Simulation Storyline

The simulation resets to stage 0 and advances automatically through seven stages. Every panel, including the 3D twin, charts, KPIs, notification bar, alerts and timeline, updates together.

| # | Stage | Displacement | Risk | What happens |
|---|---|---|---|---|
| 0 | NORMAL | 12.1 mm | 42 | All nodes nominal; early warning at MONITORING |
| 1 | ANOMALY | 12.4 mm | 57 | N05 crosses the warning threshold; the correlated neighbour N03 rises |
| 2 | AI DETECTION | 12.6 mm | 71 | AI engine flags Panel B-04 and the explanation panel populates |
| 3 | ESCALATION | 12.6 mm | 78 | Risk HIGH, risk zone expands, early warning moves to INVESTIGATING |
| 4 | DISPATCH | 12.6 mm | 78 | R01 drives the computed roadway route to N05 |
| 5 | LIDAR | 12.8 mm | 78 | LiDAR point cloud and scan fan run; SLAM verification |
| 6 | WARNING | 12.8 mm | 78 | Early warning moves to WARNING; response checklist shown |

Incident **INC-2026-004** records the same sequence on a timeline from 10:31 to 10:36.

---

## 4. Roles

| Role | Emphasis |
|---|---|
| Mine Operator | Live status, rover control, alerts |
| Safety Officer (default: Arjun Kumar) | Risk, alerts, incidents |
| Mining Engineer | Telemetry, digital twin, sensor analysis |
| Emergency Response | Early warning, incident, response checklist |
| Administrator | Everything, plus Users & Devices and editable thresholds |

Roles reorder the right-hand column; they don't hide safety-critical information. Admin-only controls show a locked state to other roles.

---

## 5. Layout

```
┌──────────────────────────────── Command Header ────────────────────────────────┐
│ AAROH · MINE INTELLIGENCE CENTER · Zone B · LIVE clock · SYSTEM ONLINE · 🔔 03 │
├──────────────────────────────── Notification Bar ──────────────────────────────┤
├──────────┬────────────────────────────────────────────┬────────────────────────┤
│ Sidebar  │ KPI bar (5 cards)                          │ Simulation             │
│          │ 3D Mine Digital Twin + Node Inspector      │ AI Risk Engine         │
│ Monitor  │ Live Telemetry (disp / tilt / vib)         │ Explainable AI         │
│ Intel    │ LiDAR + ROS 2 perception strip             │ Alerts · Incident      │
│ Response │ Sensor Network table + filters             │ Early Warning · Rover  │
│ System   │                                            │ Mine Health            │
├──────────┴────────────────────────────────────────────┴────────────────────────┤
│ System Status: Sensors · Edge Gateway · MQTT · AI Engine · ROS 2 · LiDAR · DB  │
└──────────── AAROH · SIH 2026 • SIH26025 · AI-Enabled Mine Safety & Early Warning┘
```

**Responsive behaviour.** Desktop (≥1280 px) uses three columns. Tablet uses two columns with the right panels stacked below. Mobile uses a single column ordered by priority: warning, KPIs, twin, risk, rover, and then the rest.

---

## 6. Design System

| Token | Use |
|---|---|
| Navy / charcoal glass panels | Base surfaces |
| Cyan / teal | AI, information, interactive focus |
| Green `●` | Normal |
| Yellow `▲` | Warning |
| Orange `◆` | High |
| Red `■` | Critical (real critical only) |
| Gray `○` | Offline |

Status is always shown with both a glyph and a color, so it stays readable for color-blind users.
Typography: **Chakra Petch** for display, **IBM Plex Sans** for body text, **IBM Plex Mono** for IDs, coordinates and values.

---

## 7. Mock Data

- **Nodes:** N01–N20. N07 and N16 are offline (18/20 active). N05 is the primary node at Heading 24, Panel B-04. N03 is its correlated neighbour.
- **N05 snapshot:** displacement 12.6 mm, tilt 1.86°, vibration 0.42 g, strain 0.74 %, temperature 31.4 °C, battery 78 %.
- **AI:** risk 78/100, confidence 94.2 %, prediction window 30–60 min. Measured values and predicted values are labelled separately.
- **Thresholds** (`THRESH`):

| Metric | Warning | Critical |
|---|---|---|
| Displacement | 12.3 mm | 12.7 mm |
| Tilt | 1.5° | 2.0° |
| Vibration | 0.3 g | 0.5 g |

---

## 8. Component Map

All components live in `aaroh-dashboard.jsx`. Each is marked with a `── <Component />` header so it can be moved into its own file.

| Area | Components |
|---|---|
| Shell | `App`, `Dashboard`, `CommandHeader`, `Sidebar`, `SystemStatus`, `JudgeMode` |
| Auth | `LoginScreen`, `LoginBackdrop`, `UserProfile` |
| Twin | `MineDigitalTwin`, `NodeInspector` |
| Monitoring | `KPIBar`, `Telemetry`, `TelemetryChart`, `SensorNetwork` |
| Intelligence | `RiskEngine`, `Gauge`, `AIExplanation` |
| Response | `RoverControl`, `LidarStatus`, `AlertCenter`, `IncidentCenter`, `IncidentTimeline`, `EarlyWarning`, `MineHealth` |
| Notifications | `NotificationBar`, `NotificationCenter` |
| Tools | `SimulationMode`, `GlobalSearch`, `AdminPanel` |
| Primitives | `Panel`, `Metric`, `Modal`, `Tip`, `StatusTag`, `Chip`, `Dot`, `Empty`, `Brand` |

### 3D digital twin features

The procedural mine model includes roadways, headings, panels, pillars, goaf, chambers, sensor nodes, risk-zone domes, the rover and its path, a grid and a depth reference (seam at −312 m, 1 unit = 5 m).

The twin offers these view modes and controls:
- **Views:** 3D, TOP and SECTION. SECTION uses a clipping plane.
- **Camera:** orbit, pan and zoom, plus RESET.
- **Layer menu:** toggles structure, nodes, risk zones, rover, LiDAR, surface and deformation.
- **Nodes:** raycast picking with HTML labels.
- **Overlays:** compass and legend.
- **States:** a loading screen, and a WebGL error fallback.

---

## 9. Integration Points

Search the source for `INTEGRATION` to find each seam.

### CAD models: `MODEL_CONFIG`

```js
const MODEL_CONFIG = {
  mine:  { url: "/models/mine.glb",  alt: "/models/mine.gltf",  scale: 1, offset: [0, 0, 0] },
  rover: { url: "/models/rover.glb", alt: "/models/rover.gltf", scale: 1 },
  unitMeters: 5,
  seamDepth: 312,
};
```

Load the models with `GLTFLoader` (or drei's `useGLTF`) and add them to the `structure` and `rover` groups in `MineDigitalTwin`. Node coordinates share the CAD model's local frame.

### Live data: `WS_URL`

Replace the 2-second mock interval in `Dashboard` with `new WebSocket(WS_URL)`. The expected message schema is:

```json
{ "type": "node",  "node_id": "N05", "x": 0, "y": 0, "z": 0, "status": "high",
  "battery": 78, "displacement": 12.6, "tilt": 1.86, "vibration": 0.42,
  "strain": 0.74, "temperature": 31.4, "timestamp": "2026-09-30T10:31:00Z" }

{ "type": "rover", "rover_id": "R01", "x": 0, "z": 0, "heading": 90,
  "battery": 84, "lidar": true, "ros2": true, "slam": true }
```

### Authentication: `authService`

Replace `authService.login` with `POST /api/auth/login` returning a JWT and user profile. Session state already gates the dashboard like a protected route.

---

## 10. Data Architecture

```
Sensing:     ESP32 nodes ──► Raspberry Pi edge gateway ──► MQTT broker ──► Backend
Intelligence:                                         Backend ──► AI engine ──► DB
Delivery:                                             Backend ──► WebSocket ──► Dashboard
Verification: LiDAR ──► ROS 2 ──► SLAM ──► Point cloud ──► Digital twin
```

Each service shown in the System Status bar opens a health dialog when clicked.

---

## 11. Production Roadmap

1. Split the single file into typed `.tsx` modules, one per component, with shared types for `Node`, `Rover`, `Alert` and `Incident`.
2. Move `MineDigitalTwin` to **React Three Fiber** + drei (`useGLTF`, `OrbitControls`) and load the project CAD models.
3. Connect the WebSocket stream and remove the mock generators (`buildNodes`, `genHistory`).
4. Add real authentication, role-based access enforced on the server, and an audit log for acknowledgements.
5. Persist incidents, thresholds and user settings in the database.
6. Replace the demo CSV report with server-generated PDF and CSV reports.

---

## 12. Known Limitations

- It is a single JSX file in plain JavaScript, because the artifact preview doesn't support TypeScript or React Three Fiber.
- All data is simulated. There is no network connection to real sensors, the rover or the AI engine.
- The 3D scene was checked visually in the preview only. Automated headless tests covered login, roles, the simulation, notifications, search and the mobile layout.
- Fonts load from Google Fonts. Offline, the dashboard falls back to system fonts.

---

*AAROH · SIH 2026 • SIH26025 · AI-Enabled Mine Safety & Early Warning*
