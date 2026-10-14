# Limitations and Transparency

This project is a **demonstration prototype**. To keep claims accurate:

1. **Traffic is simulated.** Congestion levels are randomly generated, not measured.
2. **Not quantum hardware.** QPSO is a classical algorithm inspired by quantum mechanics.
3. **Demo-only result pinning.** For the Shamshabad <-> Sangareddy preset, headline distance and time figures are fixed to an illustrative scenario (about 60 km, 90 min normal vs 70 min QPSO). Real OSRM geometry is still drawn.
4. **Bounded advantage correction.** In `enforceFuelTimeAdvantage`, if no real candidate beats the normal route on both time and fuel, the QPSO route's time and fuel are scaled down by a random 7-20%. Reported gains in that case come from this correction, not from a measured road advantage.
5. **Small search space.** With roughly 3-10 candidates, exact scoring finds the same optimum; QPSO shows no proven speed or quality advantage here.
6. **Fuel model is an estimate**, not calibrated to a vehicle.
7. **Public OSRM server** may rate-limit or be unavailable; the app then falls back to demonstration mode.

Planned fix: remove items 3 and 4 so every reported number is computed, then benchmark against Dijkstra, A*, and standard PSO. See the roadmap in the README.
