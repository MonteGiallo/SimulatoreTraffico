# SimulatoreTraffico

Real-time traffic intersection simulator with vehicle physics, traffic light control, day/night cycle, and live data monitoring via Grafana.

![screenshot](img/Graphics.jpg)

## Features

- **4 vehicle types** — Car, Bus, Truck, Motorcycle, each with distinct acceleration and braking coefficients
- **8 entry lanes** with weighted probabilistic spawning; spawn rate varies by simulated time of day (rush hour, midday, night)
- **Traffic light system** — 4 paired directions cycling RED → GREEN → YELLOW → RED with configurable phase durations; semi-transparent progress bars show remaining phase time on each lane; 16 pedestrian crossing lights included
- **Vehicle behavior** — FOV-based collision detection; vehicles brake for red lights and vehicles ahead, use circular arc trajectories for left turns and linear trajectories for straight/right
- **Crash simulation** — colliding vehicles stop and despawn after a timeout
- **Day/night cycle** — 4 time periods (Night, Morning, Afternoon, Evening) with distinct color palettes for road, grass, sidewalk, and lane markings
- **MySQL logging** — on each vehicle exit, plate, entry lane, spawn time, exit time, wait time at lights, and average speed are written to the database
- **Proximity sensors** — traffic lights track how many vehicles have crossed the stop line; data persisted in DB
- **Grafana dashboard** — live visualization of simulation data (vehicle throughput, wait times, speed)
- **Simulation controls** — pause, fast-forward (time lapse), direct link to Grafana

## Tech Stack

| Layer | Technology |
|---|---|
| Simulation | Python, Pygame |
| Database | MySQL 8.0 |
| DB Admin | phpMyAdmin |
| Monitoring | Grafana |
| Infrastructure | Docker Compose |

## Quick Start

**Prerequisites:** Python 3.x, Docker, Docker Compose

**1. Start the infrastructure:**

```bash
docker compose up -d
```

**2. Install Python dependencies:**

```bash
pip install pygame numpy mysql-connector-python
```

**3. Run the simulator:**

```bash
python main.py
```

| Service | URL |
|---|---|
| phpMyAdmin | http://localhost:8081 |
| Grafana | http://localhost:3000 |
| MySQL | localhost:3307 |

> **Note:** This is a local development setup. Credentials in `docker-compose.yaml` and source files are intentionally generic defaults (`user` / `userpassword`, Grafana `admin` / `admin`). Do not deploy as-is.

## Project Structure

```
SimulatoreTraffico/
├── main.py                      # Intersection class, game loop, rendering
├── classes/
│   ├── vehicles.py              # Vehicle base class + Car, Bus, Truck, Moto subtypes
│   └── traffic_light.py         # Traffic light state machine
├── functions/
│   ├── algoritmo_punti.py       # Computes the 32 waypoints on the intersection grid
│   └── db_functions.py          # Database initialization helpers
├── db/
│   └── init.sql                 # MySQL schema (Traffic_Light, Vehicle tables)
├── grafana_provisioning/        # Auto-provisioned Grafana datasource and dashboard config
└── img/                         # Vehicle sprites and UI assets
```

## Team

Group project developed at ITS by a team of 4 students.

Originally hosted at [RiccardoLunardelli/SimulatoreTraffico](https://github.com/RiccardoLunardelli/SimulatoreTraffico).
