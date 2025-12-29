# openlane2-sram

This repository demonstrates how to integrate an **SRAM hard macro** into an **OpenLane2** flow and generate a final **GDS**.

The goal of this project is not to design an SRAM itself, but to treat the SRAM as a **hard macro constraint** and focus on realistic physical design integration.

---

## Scope

### What this project does
- Install and use **OpenLane2** in an isolated environment
- Run a baseline OpenLane2 flow (no macros)
- Integrate an **SRAM hard macro** (LEF / GDS / blackbox)
- Perform floorplanning, placement, routing
- Generate a final **GDS including the SRAM macro**
- Document the flow in a reproducible way

### What this project does NOT do
- Design or synthesize an SRAM
- Provide proprietary SRAM macro files
- Achieve full sign-off quality (commercial-grade DRC/LVS)
- Replace OpenLane (v1)

---

## Documentation

Start here and proceed in order:

1. **[Project Plan](docs/00_plan.md)**  
   Overall scope, milestones (M0–M3), and done criteria.

2. **[Environment Setup](docs/10_env.md)**  
   Non-destructive OpenLane2 installation and PDK handling.

3. **[Baseline OpenLane2 Flow](docs/20_openlane2.md)**  
   Minimal end-to-end OpenLane2 run without SRAM.

4. **[SRAM Macro Integration](docs/30_macro_sram.md)**  
   Blackbox declaration, LEF/GDS integration, fixed macro placement.

5. **[Results and Observations](docs/40_results.md)**  
   Final GDS generation, limitations, and lessons learned.

---

## Repository Structure

```
openlane2-sram/
├─ README.md
├─ docs/
│  ├─ 00_plan.md          # Project plan and milestones
│  ├─ 10_env.md           # Environment and installation notes
│  ├─ 20_openlane2.md     # OpenLane2 basic flow
│  ├─ 30_macro_sram.md    # SRAM macro integration
│  └─ 40_results.md       # Results and observations
├─ scripts/
│  ├─ 01_setup_venv.sh
│  ├─ 10_run_base.sh
│  └─ 20_run_with_sram.sh
├─ design/
│  ├─ src/
│  │  ├─ top.v
│  │  └─ sram_blackbox.vh
│  └─ config/
├─ macro/
│  └─ sram/
│     └─ README.md        # How to provide/link SRAM macros
├─ third_party/
│  └─ README.md           # External dependencies (PDK, macros)
└─ artifacts/
   └─ .gitkeep
```

---

## Milestones (High-Level)

- **M0**: OpenLane2 installation (non-destructive)  
  → see **[docs/10_env.md](docs/10_env.md)**

- **M1**: Base design flow (no SRAM)  
  → see **[docs/20_openlane2.md](docs/20_openlane2.md)**

- **M2**: SRAM macro placement and floorplan  
  → see **[docs/30_macro_sram.md](docs/30_macro_sram.md)**

- **M3**: Complete routing and GDS generation  
  → see **[docs/40_results.md](docs/40_results.md)**

Each milestone leaves tangible artifacts and documentation.

---

## SRAM Macro Policy

SRAM macros are treated as **external hard macros**.

- The repository does **not** include actual SRAM GDS/LEF files
- Users must provide or link their own SRAM macros
- Only the **integration method** is documented

This keeps the repository license-safe and reusable.

---

## PDK

- Target PDK: **sky130**
- PDK files are not included in this repository
- The flow assumes an existing local PDK installation

---

## Intended Audience

- OpenLane / OpenROAD users stepping beyond standard-cell-only flows
- Engineers interested in **macro-aware floorplanning**
- Educators preparing realistic physical design examples

---

## Status

🚧 Work in progress  
This repository is developed step by step, with emphasis on clarity and reproducibility.

---

## License

MIT License
