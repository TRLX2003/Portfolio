# Lab 1 – Elements of Airplane Performance (I)

MATLAB analysis of the performance of the **Aermacchi MB-339** jet trainer: drag polar, thrust, Penaud diagrams, SET/SEP, flight envelope, time to climb and coordinated turn.

## Repository contents

| File | Description |
|---|---|
| `Lab01.m` | Main script, runnable section by section (`%%`) |
| `MB339_polar.mat` | Experimental polar data (`CL`, `CD`) |
| `MB339_thrust.mat` | Experimental thrust data (`h`, `V`, `T`) |

## Requirements

- **Aerospace Toolbox** (`atmosisa`, ISA standard atmosphere)
- **Mapping Toolbox** (`unitsratio`, ft → m conversion)

## Input data

- `MB339_polar.mat`: vectors `CL` and `CD` (experimental polar points, with `CL` increasing up to `CL_max`).
- `MB339_thrust.mat`: vector `h` (altitudes [ft], column), vector `V` (speeds [m/s]) and matrix `T` [N] of size `length(h) × length(V)`. The first row corresponds to the lowest altitude (sea level).

## Aircraft parameters

| Symbol | Value | Description |
|---|---|---|
| `W` | 61000 N | Weight |
| `S` | 19.3 m² | Wing area |
| `A` | 0.75 | Exponent of the thrust model `T = T0 (ρ/ρ0)^A` |
| `n_max` | 2.5 | Maximum load factor in turn |

## Script structure

1. **Polar** – Interpolation of the experimental data (linear, spline, quadratic) and comparison. `CD0` and `k` are obtained from the quadratic fit.
2. **Thrust** – Quadratic fit of the experimental data versus `V` for each altitude, and analytical model (thrust constant with speed, scaled with density).
3. **Penaud diagrams** – Drag versus speed at `h = 0 ft` with the four polar models; the analytical polar is then used for the altitude effect.
4. **SET and SEP** – Specific Excess Thrust (climb angle γ) and Specific Excess Power (rate of climb) at each altitude.
5. **Flight envelope** – `v_min` (stall or T = D intersection) and `v_max` (T = D) as a function of altitude up to 15000 m.
6. **Time to climb** – Quasi-steady approach `τ = ∫ dh / SEP_max` and unsteady approach (acceleration factor `1 + (v/g)·dv/dh`).
7. **Turn performance** – Coordinated turn at `h = 0 ft`: maximum bank angle, minimum radius and minimum time for a 180° turn.

## Output plots

- Polar (linear, spline, quadratic, comparison)
- Experimental and analytical thrust
- Penaud diagrams (per model and per altitude)
- SET and SEP
- Flight envelope
- Time to climb (quasi-steady and unsteady)
- T/D diagram for different `n`, bank angle, turn radius and turn time

## Assumptions and limitations

- Thrust independent of speed, proportional to `(ρ/ρ0)^0.75`.
- Constant weight (no fuel burn).
- ISA standard atmosphere.
- Symmetric, quasi-steady flight with small climb angles (`sin γ ≈ γ`).
