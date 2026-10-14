# QuantumRoute AI

**Normal vs. Quantum-Inspired Route Optimization** — an interactive, browser-based prototype that compares a conventional shortest-route choice with a route selected by a Quantum-behaved Particle Swarm Optimization (QPSO) search over real road alternatives in Hyderabad, India.

> **Status:** Research / demonstration prototype. Traffic data is **simulated**. Not for live navigation. See [docs/LIMITATIONS.md](docs/LIMITATIONS.md).

## Live demo

After enabling GitHub Pages (Settings > Pages > Source: GitHub Actions), the app is served at:
`https://<your-username>.github.io/quantumroute-ai/`

Or run locally: open `index.html` in any modern browser (internet required for map tiles and routing).

## What it does

- Draws **real road geometry** on an OpenStreetMap map using the public OSRM routing API.
- Builds a pool of candidate routes (OSRM alternatives, plus detours through intermediate waypoints).
- Simulates **traffic congestion** (LOW / MEDIUM / HIGH) along each route.
- Scores every route on a weighted multi-objective fitness: **time, distance, congestion, fuel**.
- Runs **QPSO** (30 particles, 40 iterations) to choose the best candidate and plots the **convergence curve**.
- Compares "Normal" vs "QPSO" routes side by side, with animated vehicles.
- Adjustable objective weights, 17 preset Hyderabad / Telangana routes, and random intermediate points (P1-P10).

## Tech stack

| Layer | Technology |
|-------|------------|
| UI | HTML, CSS, vanilla JavaScript (single file) |
| Map | [Leaflet](https://leafletjs.com/) 1.9.4 + OpenStreetMap tiles |
| Routing | [OSRM](https://project-osrm.org/) public demo server |
| Charts | [Chart.js](https://www.chartjs.org/) 4.4.4 |
| Algorithm | QPSO (quantum-inspired, runs classically) |

## How the optimization works (short version)

Each candidate route gets a normalized cost:

```
fitness = w_t * time_norm + w_d * dist_norm + w_c * congestion + w_f * fuel_norm
```

QPSO searches a continuous position in `[0, N-1]` (N = number of candidates) and rounds it to a route index. Particles are updated with a quantum-behaved rule using the mean best position and a contraction-expansion coefficient that decays from 1.0 to 0.5. Full details: [docs/ALGORITHM.md](docs/ALGORITHM.md).

> "Quantum-inspired" means the algorithm borrows ideas from quantum mechanics. It runs on a normal CPU in the browser; **no quantum hardware or quantum circuits are used.**

## Project structure

```
quantumroute-ai/
├── index.html                  # Complete application
├── docs/
│   ├── ALGORITHM.md            # QPSO + fitness + fuel model
│   ├── ARCHITECTURE.md         # Code structure and data flow
│   └── LIMITATIONS.md          # Honest scope and known issues
├── .github/
│   ├── workflows/pages.yml     # Auto-deploy to GitHub Pages
│   └── ISSUE_TEMPLATE/
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENSE
└── .gitignore
```

## Roadmap

- [ ] Replace simulated traffic with a real traffic feed (e.g. TomTom / HERE)
- [ ] Remove demo-only result pinning so reported gains come purely from computed metrics
- [ ] Benchmark QPSO against Dijkstra, A*, standard PSO, and brute force on candidate sets
- [ ] Larger candidate pools and multi-stop (TSP-style) routing
- [ ] Split `index.html` into modules (`data`, `routing`, `qpso`, `ui`) with unit tests

## Author

**D. Bhargav Kumar** — Team **Q-OPTIMA**

## License

MIT — see [LICENSE](LICENSE). Map data (c) OpenStreetMap contributors.
