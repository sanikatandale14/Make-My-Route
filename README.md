# Make My Route — EV Routing MVP

This repository contains the MVP implementation of **Antigravity AI — EV Route Planner**, an energy-aware routing system that predicts battery consumption, checks route feasibility, and recommends charging stops (or routes) for electric vehicles.

## 1. Install dependencies

```bash
cd Make_My_route
pip install -r requirements.txt
```

## 2. Configure region (optional)

By default the server uses the `REGION` environment variable to determine which place to download the OSMnx road graph for.

Set a different region (e.g., Berlin) before starting the server:

```bash
export REGION="Berlin, Germany"      # Linux/macOS
set REGION="Berlin, Germany"        # Windows CMD
$Env:REGION = "Berlin, Germany"     # Windows PowerShell
```

> Note: On first run, OSMnx will download the road network from OpenStreetMap. This may take a minute.

## 3. Run the API

Start the FastAPI server:

```bash
uvicorn api.server:app --reload --host 0.0.0.0 --port 8000
```

### Key endpoints

- **Route computation**
  - `POST /api/route`
  - JSON body: `origin`, `destination`, `ev_profile` (see `api.models.RouteRequest`)

- **Charging stations (OpenChargeMap)**
  - `GET /api/stations?lat=...&lon=...&radius_km=...`

- **Health check**
  - `GET /api/health`

- **Available EV models**
  - `GET /api/ev/models`

> Note: This repo does not include a CLI. Use the HTTP API endpoints above.

### Feasibility

```bash
python cli.py feasibility --start 52.52 13.405 --end 52.53 13.41 --capacity 50000 --efficiency 200
```

### Rank EVs

```bash
python cli.py recommend --start 52.52 13.405 --end 52.53 13.41
```

(Optional) Provide custom EV list:

```bash
python cli.py recommend --start 52.52 13.405 --end 52.53 13.41 --evs '[{"capacity":50000,"efficiency":180},...]'
```

### Export Folium map

```bash
python cli.py map --start 52.52 13.405 --end 52.53 13.41 --out ev_route_map.html
```
