# Defra DDTS — Emerging Technology Radar 2026

## What this project is

A single-file interactive technology radar built for Defra's Digital, Data, Technology & Security (DDTS) team. It visualises emerging technologies across four maturity stages and three impact levels, with filtering, search, and a detail panel. There is no build system, no dependencies, and no server — everything runs in one HTML file opened directly in a browser.

## The single file

**`radar.html`** — contains all HTML, CSS, and JavaScript in one file. This is intentional; do not split it into separate files or suggest doing so.

## How the radar works

Technologies are plotted on a circle using two axes:

- **Angular position (quadrant)** = maturity level, set by the `ring` property
  - `"Embryonic"` → top-left
  - `"Emerging"` → top-right
  - `"Developing"` → bottom-right
  - `"Mainstream"` → bottom-left

- **Radial distance from centre** = business impact, set by the `impact` property
  - `"Transformational"` → innermost ring (closest to centre)
  - `"High"` → middle ring
  - `"Moderate"` → outermost ring

The `quadrant` property is the strategic theme used only for the filter bar and side panel — it does **not** affect position on the radar.

## Technology data structure

Each technology entry looks like this:

```js
{
  name: "Example Technology",
  ring: "Developing",          // maturity → angular position
  impact: "High",              // business impact → radial ring
  quadrant: "Cybersecurity",   // theme → filter & panel only
  status: "Explore",           // colour of node
  isNew: true,                 // true = star shape, false = circle
  description: "..."           // shown in the side panel
}
```

Valid values:
- `ring`: `"Mainstream"` | `"Developing"` | `"Emerging"` | `"Embryonic"`
- `impact`: `"Transformational"` | `"High"` | `"Moderate"`
- `quadrant`: `"Digital Transformation"` | `"Environmental Intelligence"` | `"Sustainable Operations"` | `"Cybersecurity"`
- `status`: `"Adopt"` | `"Explore"` | `"Watch"`
- `isNew`: `true` | `false`

**Important:** These values must be spelled exactly as shown — the code matches them by string. A typo like `"Transformation"` instead of `"Transformational"` will silently cause a node to disappear from the radar.

## Colours

| Use | Hex | RGB |
|-----|-----|-----|
| Adopt nodes | `#00703C` | rgb(0, 112, 60) |
| Explore nodes | `#1D7B8A` | rgb(29, 123, 138) |
| Watch nodes | `#B58A00` | rgb(181, 138, 0) |
| Header / panel / purple UI | `#4B2E83` | rgb(75, 46, 131) |
| Arc labels (Embryonic etc.) | `#1B5E20` | rgb(27, 94, 32) |
| Ring fill (inner) | `rgba(0,112,60,0.30)` | — |
| Ring fill (middle) | `rgba(0,112,60,0.16)` | — |
| Ring fill (outer) | `rgba(0,112,60,0.07)` | — |

## Ring size settings

Controlled by the `RINGS` array. `outerFrac` is a fraction of `MAX_R` (300px):

```js
{ label: 'Transformational', outerFrac: 0.50 }  // inner ring boundary
{ label: 'High',             outerFrac: 0.80 }  // middle ring boundary
{ label: 'Moderate',         outerFrac: 1.05 }  // outer ring boundary
```

Increasing `outerFrac` on the Transformational ring gives more space to crowded inner nodes.

## Known issues to be aware of

- The Developing/Transformational zone (bottom-right inner ring) can get crowded when many items share that cell — node overlap is the main layout challenge.
- `"Quantum Computing"` has a known typo in the data (`"Transformation"` instead of `"Transformational"`) — it will not render until fixed.
- The outermost ring visually extends slightly beyond the canvas boundary when `outerFrac` exceeds 1.0.

## What not to change

- Do not add a build process, npm, or external dependencies.
- Do not split into multiple files.
- Do not change the filter bar structure — the JS wires up to `data-filter` and `data-value` attributes directly.
- Do not rename the CSS variables in `:root` without updating all references.

## Tone and context

This is a professional internal tool for a UK government department. Keep any UI text, labels, and descriptions formal, clear, and consistent with GDS (Government Design System) conventions. Avoid casual language in anything visible to users.
