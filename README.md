# MgCO₂ V6 Compression Engine — Interactive 3D Viewer

> **A zero-dependency, browser-native 3D viewer for a conceptual magnesium–CO₂ combustion engine.**  
> Built with Three.js r128 · GLB model · Single HTML file · No build step required.

---

## 🔥 Live Demo

**[▶ Open Viewer](./MgCO2_V6_Engine_Viewer.html)** — works directly from the file system or any static host.

> GitHub Pages deployment: `https://<your-username>.github.io/MgCO2-V6-Engine-Viewer/MgCO2_V6_Engine_Viewer.html`

---

## 📐 What Is This?

This project is an interactive 3D educational model of a **conceptual V6 internal combustion engine that burns magnesium metal in a CO₂ atmosphere** instead of conventional hydrocarbon fuel.

The core reaction is:

```
2Mg + CO₂ → 2MgO + C     ΔH = −810 kJ/mol
```

This exothermic reaction releases significant energy while consuming CO₂ — making it a carbon-negative fuel cycle when the MgO byproduct is recycled back to Mg metal via electrolysis (powered by renewables).

The engine model explores what a practical 6-cylinder implementation of this chemistry might look like, including all the engineering challenges it implies: ceramic combustion liners, solid-particle exhaust handling, closed-loop CO₂ recirculation, and magnesium powder injection.

---

## ✨ Features

| Feature | Description |
|---|---|
| **Interactive 3D orbit** | Drag to orbit, scroll to zoom, right-drag to pan |
| **Part hover tooltips** | Hover any mesh to see part name, material, engineering description, and chemistry notes |
| **Click to focus** | Click a part to smoothly zoom the camera to it |
| **Explode View** | Animated radial explode separates all parts for internal inspection |
| **Cutaway Mode** | Toggles a clipping plane to reveal internal geometry |
| **Material Legend** | Colour-coded legend with click-to-isolate per material group |
| **Auto-rotate** | Gentle idle rotation stops on user interaction |
| **Combustion glow** | Dynamic point light in the V-valley simulates heat |
| **Part knowledge base** | 40+ named parts with engineering specs and chemistry |
| **Zero dependencies** | Three.js loaded from CDN; GLB embedded as base64 in the HTML |

---

## 🗂 Repository Structure

```
MgCO2-V6-Engine-Viewer/
├── MgCO2_V6_Engine_Viewer.html   # Self-contained viewer (Three.js + embedded GLB)
├── MgCO2_V6_Engine.glb           # Source GLB model (standalone, for use in other tools)
└── README.md                     # This file
```

> **Note:** The `.html` file contains the `.glb` model embedded as a base64 string. This makes it fully portable — a single file you can open, share, or host anywhere. The standalone `.glb` is included for use in Blender, model viewers, game engines, etc.

---

## 🚀 Quickstart

### Option 1 — Open Locally (Zero Setup)
```bash
git clone https://github.com/<your-username>/MgCO2-V6-Engine-Viewer.git
# Then just open MgCO2_V6_Engine_Viewer.html in your browser
```

> ⚠️ Some browsers block local file fetches. If the model doesn't load, use a local server (see Option 2).

### Option 2 — Local Dev Server
```bash
# Python (built-in)
cd MgCO2-V6-Engine-Viewer
python3 -m http.server 8080
# → http://localhost:8080/MgCO2_V6_Engine_Viewer.html

# Node.js (npx)
npx serve .
```

### Option 3 — GitHub Pages
1. Push this repo to GitHub
2. Go to **Settings → Pages → Source → main branch / root**
3. Your viewer will be live at:  
   `https://<your-username>.github.io/MgCO2-V6-Engine-Viewer/MgCO2_V6_Engine_Viewer.html`

---

## 🎮 Controls

| Input | Action |
|---|---|
| `Left-drag` | Orbit / rotate view |
| `Scroll wheel` | Zoom in / out |
| `Right-drag` | Pan camera |
| `Hover` part | Show part info tooltip |
| `Click` part | Focus camera on that part |
| **Explode View** button | Animate parts apart radially |
| **Cutaway** button | Toggle cross-section clipping plane |
| **⟳ Reset View** button | Return camera to default position |
| Legend row click | Isolate / highlight that material group |

---

## ⚙️ Engine Concept — Technical Summary

### The Fuel Cycle

```
                 electrolysis (renewable)
      MgO  ──────────────────────────────►  Mg  (fuel)
       ▲                                         │
       │                                         ▼
    combustion                           injected into cylinder
  2Mg + CO₂ → 2MgO + C                  with pressurised CO₂
       │
       ▼
    MgO + C (solid exhaust)
    captured by ash separator
    MgO recycled, C retained
```

**Net result:** CO₂ consumed → energy released → solid MgO + C byproducts → MgO recycled → closed loop.

### Key Engine Components

| Component | Engineering Challenge |
|---|---|
| **Ceramic Combustion Liner** | Mg burns at ~2,200 °C — requires sintered SiC/Al₂O₃ liner |
| **Mg Powder Injector** | Solid-fuel injection at µs timing precision |
| **Ash Separator** | Cyclonic separation of solid MgO/C from exhaust stream |
| **CO₂ Recovery Canister** | Closed-loop CO₂ recirculation to intake plenum |
| **Pre-Chamber Dome** | Staged ignition for complete Mg combustion |
| **Exhaust Valves** | WC-coated Nimonic alloy to resist solid-particle abrasion |

### Layout
- **60° V6** configuration
- **DOHC** cylinder heads, both banks
- **120° even-fire** intervals for smooth torque delivery
- CO₂ intake plenum in the V-valley

---

## 🛠 Implementation Notes

### GLB Parsing
The viewer implements a **hand-written GLB binary parser** using the Three.js r128 `BufferGeometry` API (no `GLTFLoader` module required). This parses:
- JSON chunk (scene graph, mesh names, accessor metadata)
- Binary chunk (vertex positions, normals, indices)
- Mesh names used for per-part material assignment and tooltip lookup

### Material System
Materials are assigned by **mesh-name keyword matching** against a `matColors` lookup table. Each material has `color`, `metalness`, and `roughness` values for physically-based rendering.

### Part Knowledge Base
40+ engine parts are described in the `PART_INFO` object, each with:
- Human-readable title
- Material specification
- Engineering description
- Chemistry / operating parameters

---

## 📦 Embedding the GLB (How It Works)

The `.glb` file is embedded inside the HTML as a base64 string and decoded at runtime:
```js
const bin = Uint8Array.from(atob(b64), c => c.charCodeAt(0)).buffer;
await loadGLBBuffer(bin);
```
This makes the viewer a **true single-file application** — no server required, no CORS issues, drag-and-drop into any browser.

---

## 🔧 Customisation

### Swap the 3D Model
1. Convert your GLB to base64:
   ```bash
   base64 -i your_model.glb | tr -d '\n'
   ```
2. Replace the `const b64 = "..."` string near the bottom of the HTML.
3. Update `PART_INFO` with your own part descriptions.

### Add / Edit Part Descriptions
Edit the `PART_INFO` object in the `<script>` block:
```js
"Your_Mesh_Name": {
  title: "Display Name",
  mat: "Material Spec",
  desc: "Engineering description.",
  chem: "Optional chemistry note"
},
```
Keys are matched by exact name, prefix, or space-converted keyword.

### Change Material Colours
Edit the `matColors` object — each entry takes `color` (hex), `metal` (0–1), `rough` (0–1), and optional `opacity`.

---

## 📄 Licence

This project is released under the **MIT Licence** — free to use, share, and modify with attribution.

---

## 🙏 Credits & Stack

- **Three.js r128** — [threejs.org](https://threejs.org) (MIT)
- 3D model created in Blender / exported as GLB
- Viewer, GLB parser, and part knowledge base hand-authored

---

*This engine concept is speculative / educational. Magnesium combustion in CO₂ is a real chemical reaction; the V6 implementation is a design study.*
