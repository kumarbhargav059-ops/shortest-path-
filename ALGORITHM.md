# Algorithm

## 1. Candidate generation
1. Request the primary route and alternatives from OSRM (`alternatives=true`).
2. If fewer than about 5 distinct routes exist, request extra routes forced through a perpendicular via-point (offset scaled to trip length, max 3.5 km).
3. Optionally build routes through the random intermediate points P1-P10.
4. If OSRM is unreachable, fall back to a clearly labelled demonstration route.

## 2. Metrics per route
| Metric | Source |
|--------|--------|
| Time | OSRM duration |
| Distance | OSRM distance |
| Congestion | Average of simulated traffic segments near the route polyline (LOW 0.2, MEDIUM 0.5, HIGH 0.9) |
| Fuel | Estimated: base 6.8 L/100 km, scaled by a speed penalty (ideal 55 km/h) and a congestion penalty (`1 + 0.55 * congestion`) |

## 3. Fitness (lower is better)
```
fitness = w_t*(time/maxTime) + w_d*(dist/maxDist) + w_c*congestion + w_f*(fuel/maxFuel)
```
Weights are user-adjustable and should sum to 100%.

## 4. QPSO
- Particles: 30, iterations: 40.
- Position: continuous value in `[0, N-1]`, rounded to a candidate index.
- Local attractor: `p = phi * pbest_i + (1 - phi) * gbest`, `phi ~ U(0,1)`.
- Update: `x_i = p +/- beta * |mbest - x_i| * ln(1/u)`, `u ~ U(0,1)`.
- `mbest` is the mean of all personal-best positions.
- `beta` decays linearly from 1.0 to 0.5 (contraction-expansion).
- A small random jitter (+/- 1%) is added to fitness during search to break ties.
- Best-so-far fitness per iteration is plotted as the convergence curve.

## 5. Why QPSO here
The search space is a small discrete set, so exhaustive scoring would also find the optimum. QPSO is used here as a demonstrator of a quantum-inspired metaheuristic that becomes more useful when candidates are generated combinatorially (many waypoints). A fair comparison against exact methods is on the roadmap.
