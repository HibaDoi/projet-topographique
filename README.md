# Topographic Calculations

Three browser-based tools for classic land-surveying calculations, using **gons** (400 gon = full circle) and the surveying convention: Y axis points North, X axis points East, bearings measured clockwise from North.

## Pages

| Page | Purpose |
|------|---------|
| [topo.html](topo.html) | Distance and bearing between two points A and B |
| [rayonement.html](rayonement.html) | Polar survey — locate point M from a station S using a reference R, an angle and a distance |
| [intersection.html](intersection.html) | Triangulation — locate point M from two stations A, B and the angles α, β observed at each |

All three share [topo.css](topo.css) and are linked by a top navigation bar.

## Usage

Open any HTML file directly in a browser — no server needed.

## Principle

**Bearing (gisement)** — the angle from North to the direction A→B, clockwise. Computed from `Δx = x₂ − x₁` and `Δy = y₂ − y₁` using `atan`, with a quadrant-dependent offset (0, 100, 200 or 300 gon) so the result lies in `[0, 400)`.

**Distance** — Euclidean: `d = √(Δx² + Δy²)`.

**Polar survey (rayonnement)** — given a station S and a reference R, the bearing S→R orients the instrument. Adding the observed angle α gives the bearing S→M, and the new point is projected at the measured distance:

```
G_SM = G_SR + α
x_M  = x_S + d · sin(G_SM)
y_M  = y_S + d · cos(G_SM)
```

**Triangulation (intersection)** — given two stations A, B and the angles α (at A) and β (at B) toward the unknown point M, the third angle is `γ = 200 − α − β` (gon). The law of sines gives the side lengths, and the bearing A→M is `G_AB ± α` depending on which side of AB the point lies:

```
AM = AB · sin(β) / sin(γ)
G_AM = G_AB ± α
x_M  = x_A + AM · sin(G_AM)
y_M  = y_A + AM · cos(G_AM)
```

## Project structure

```
projet-topographique/
├── README.md
├── topo.css           # shared styles
├── topo.html          # distance + bearing
├── rayonement.html    # polar survey
└── intersection.html  # triangulation
```
