# MB-339 Aircraft Performance Analysis

MATLAB Live Script implementing a classical fixed-wing performance analysis of the
Aermacchi **MB-339** jet trainer, from experimental aerodynamic and propulsion data
to the flight envelope and time-to-climb.

Built as a lab assignment for a Flight Mechanics / Airplane Performance course.

## What it does

Starting from experimental polar and thrust data, the script builds up a full
point-performance analysis:

1. **Drag polar fitting** — linear interpolation, cubic spline, and quadratic
   least-squares fit of `C_D(C_L)`, compared against each other.
2. **Thrust modeling** — quadratic fit of experimental thrust vs. airspeed at
   several altitudes, plus an analytical model
   `T(ρ) = T₀ · (ρ/ρ₀)^A` (A = 0.75) assumed independent of speed.
3. **Penaud (thrust-required) diagrams** — drag vs. airspeed at sea level using
   all three polar fits, then generalized to altitude with the analytical polar.
4. **SET & SEP** — Specific Excess Thrust (≈ climb angle, small-angle
   approximation) and Specific Excess Power across the flyable speed range, at
   each altitude.
5. **Flight envelope** — minimum and maximum true airspeed vs. altitude,
   obtained as the roots of `Thrust available = Drag required`, bounded by
   stall speed.
6. **Time to climb** — cumulative climb time along the best-rate-of-climb
   speed schedule, computed two ways:
   - **Quasi-steady**: `dt/dh = 1 / SEP_max`
   - **Unsteady (energy-height) method**: `dt/dh = (1 + (V/g)·dV/dh) / SEP_max`,
     accounting for the energy spent accelerating along the climb schedule.

   The quasi-steady curve asymptotically flattens as altitude approaches the
   **absolute ceiling**, where `SEP_max → 0` — this is the expected physical
   behavior, not a numerical artifact.

## Repository contents

```
Lab01.mlx           MATLAB Live Script — full analysis, in order
MB339_polar.mat      Experimental C_L / C_D polar data
MB339_thrust.mat     Experimental thrust data: T(altitude, airspeed)
```

## Requirements

- MATLAB R2020b or newer (Live Script `.mlx` format, `compose` function)
- Aerospace Toolbox (`atmosisa` — International Standard Atmosphere)

`fzero`, `optimset`, `polyfit`, `interp1`, and `spline` are all base MATLAB and
need no additional toolbox.

## Running it

1. Keep `Lab01.mlx`, `MB339_polar.mat`, and `MB339_thrust.mat` in the same
   folder.
2. Open `Lab01.mlx` in MATLAB and run all sections top to bottom — later
   sections depend on variables defined earlier (polar coefficients, thrust
   model, flight envelope, etc.).

## Modeling assumptions

These are deliberate simplifications typical of a first-pass performance
estimate, not limitations of the code:

- Drag polar assumed parabolic, `C_D = k·C_L² + C_D₀` (the linear `C_L` term
  from the quadratic fit is neglected).
- Available thrust assumed independent of airspeed at a given altitude,
  scaling only with the density ratio to the power `A = 0.75`.
- Climb angle approximated as `γ ≈ (T − D) / W` (valid for small angles,
  standard in this type of point-performance analysis).
- At each altitude, the best rate-of-climb speed is taken as the speed that
  maximizes SEP over the achievable speed range between stall and the
  thrust-limited maximum speed.

## Sample outputs

The script generates, among others:
- Polar comparison (linear / spline / quadratic)
- Thrust vs. airspeed at multiple altitudes (experimental + analytical)
- Penaud diagrams at sea level and across altitude
- SET and SEP carpet plots vs. airspeed and altitude
- Flight envelope (`v_min`, `v_max` vs. altitude)
- Time-to-climb curves (quasi-steady vs. unsteady)

## Author

[Alex Triolo] — [Airplane Performance and Dynamics / Politecnico di Milano, 2026]
