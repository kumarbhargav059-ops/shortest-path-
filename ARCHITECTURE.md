# Architecture

The app is a single static file (`index.html`) with three parts: markup, CSS, and one `<script>` block.

## Script sections
| Section | Responsibility |
|---------|----------------|
| Constants | `LOCATIONS`, `QUICK_ROUTES`, `PRESET_ROUTES`, OSRM URL, fuel constants |
| State | Map layers, candidates, QPSO result, traffic segments |
| Utils | `haversine`, formatters, toasts |
| Routing | `fetchOSRM`, `buildCandidates`, `buildPointCandidates`, synthetic fallbacks |
| Traffic | `generateTraffic`, `drawTraffic`, `congestionForRoute` |
| Fitness | `computeRawMetrics`, `buildFitnessFn`, `fuelForRoute` |
| QPSO | `runQPSO` |
| Chart | Chart.js convergence plot |
| UI handlers | `onCalculateRoutes`, `onRunQPSO`, `onSimulateTraffic`, `onStartVehicle`, `onGeneratePoints`, `onReset` |

## Data flow
```
Select route -> Calculate Routes (OSRM) -> candidates[]
             -> Simulate Traffic -> trafficSegments[]
             -> Run QPSO -> best candidate + convergence[]
             -> Compare cards, chart, animated vehicles
```

## External services
- OpenStreetMap tiles (map)
- OSRM public demo server (routing; rate-limited, no SLA)
- unpkg / jsDelivr / Google Fonts (libraries and fonts)
