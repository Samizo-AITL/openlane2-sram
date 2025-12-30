---
title: "openlane2-sram"
description: "Macro-aware physical design example using OpenLane2 with an SRAM hard macro"
---

# openlane2-sram  
**Macro-Aware Physical Design with OpenLane2**

This project demonstrates a **practical, macro-aware physical design flow**  
using **OpenLane2 (v2)** by integrating an **SRAM hard macro** into a complete  
**RTL → GDS** implementation.

The SRAM is treated strictly as an **external hard macro**  
(LEF / GDS / Verilog blackbox), reflecting **realistic SoC physical design practice**.

---

## 🔗 Links

| Language | GitHub Pages 🌐 | GitHub 💻 |
|----------|----------------|-----------|
| 🇺🇸 English | [![GitHub Pages EN](https://img.shields.io/badge/GitHub%20Pages-English-brightgreen?logo=github)](https://samizo-aitl.github.io/openlane2-sram/) | [![GitHub Repo EN](https://img.shields.io/badge/GitHub-English-blue?logo=github)](https://github.com/Samizo-AITL/openlane2-sram/tree/main) |

[![Back to Samizo-AITL Portal](https://img.shields.io/badge/Back%20to%20Samizo--AITL%20Portal-brightgreen)](https://samizo-aitl.github.io) 

---

## What This Repository Proves

- ✅ OpenLane2 can handle **hard macro integration**
- ✅ SRAM is placed as a **FIXED macro with halo / keepout**
- ✅ Standard cells are placed and routed **around the macro**
- ✅ A **final GDS including the SRAM macro** is generated
- ✅ The flow is **reproducible and documented**

This is **not** an SRAM design project.  
It is an **integration and methodology** project.

---

## Layout Evidence

### Figure 1: SRAM Macro Block-Level View

<img
  src="https://raw.githubusercontent.com/Samizo-AITL/openlane2-sram/main/docs/fig/fig01_openlane2_spm_macro_block_level.png"
  width="80%"
  alt="SRAM hard macro block-level layout (fixed macro with halo)"
/>

> The SRAM appears as a large fixed block, placed early during floorplanning.

---

### Figure 2: Standard-Cell / Transistor-Level View Around SRAM

<img
  src="https://raw.githubusercontent.com/Samizo-AITL/openlane2-sram/main/docs/fig/fig02_openlane2_spm_standard_cell_level_view.png"
  width="80%"
  alt="Standard-cell placement and routing around SRAM macro"
/>

> Standard-cell logic is placed and routed around the SRAM macro,  
> confirming correct macro-aware placement and routing.

---

## Project Structure

```text
openlane2-sram/
├─ README.md              # Repository overview (this project)
├─ index.md               # Top / landing page (this file)
├─ docs/
│  ├─ 00_plan.md          # Project plan and milestones
│  ├─ 10_env.md           # OpenLane2 environment setup
│  ├─ 20_openlane2.md     # Baseline OpenLane2 flow
│  ├─ 30_macro_sram.md    # SRAM macro integration
│  └─ 40_results.md       # Results and observations
│
├─ designs/
│  └─ spm/
│     ├─ config.json
│     ├─ run_config.json
│     ├─ pin_order.cfg
│     └─ src/
│        ├─ spm.v
│        └─ spm.sdc
│
└─ runs/                  # Generated artifacts (gitignored)
```

---

## Documentation Entry Points

- 📘 **[Project Plan](docs/00_plan.md)**  
  Project scope, milestones (M0–M3), and completion criteria.

- 🛠 **[Environment Setup](docs/10_env.md)**  
  Non-destructive OpenLane2 installation, Python virtual environment, and PDK handling.

- ▶ **[Baseline OpenLane2 Flow](docs/20_openlane2.md)**  
  Verified baseline RTL → GDS flow without macros (control experiment).

- 🧠 **[SRAM Hard Macro Integration](docs/30_macro_sram.md)**  
  Blackbox declaration, LEF/GDS usage, fixed macro placement, and macro-aware floorplanning.

- 📊 **[Results and Observations](docs/40_results.md)**  
  Final GDS generation, layout evidence, known limitations, and lessons learned.

---

## Status

✔️ Baseline OpenLane2 flow verified  
✔️ SRAM hard macro successfully integrated as a FIXED macro  
✔️ Final GDS generated and visually verified

This repository emphasizes **clarity, reproducibility, and realistic physical design constraints**.

---

## Author

| 📌 Item | Details |
|--------|---------|
| **Name** | Shinichi Samizo |
| **Education** | M.S. in Electrical and Electronic Engineering, Shinshu University |
| **Career** | Former Engineer at Seiko Epson Corporation (since 1997) |
| **Expertise** | Semiconductor devices (logic, memory, high-voltage mixed-signal)<br>Thin-film piezo actuators for inkjet systems<br>PrecisionCore printhead productization, BOM management, ISO training |
| **GitHub** | [![GitHub](https://img.shields.io/badge/GitHub-Samizo--AITL-blue?style=for-the-badge&logo=github)](https://github.com/Samizo-AITL) |

---

## License

[![Hybrid License](https://img.shields.io/badge/license-Hybrid-blueviolet)](https://samizo-aitl.github.io/aitl-contller-a-type//#-license)

| Item | License | Description |
|------|---------|-------------|
| **Source Code** | MIT | Free to use, modify, redistribute |
| **Text Materials** | CC BY 4.0 / CC BY-SA 4.0 | Attribution & share-alike rules |
| **Figures & Diagrams** | CC BY-NC 4.0 | Non-commercial use |
| **External References** | Original license applies | Cite properly |

---

## Feedback

> Suggestions, improvements, and discussions are welcome via GitHub Discussions.

[![💬 GitHub Discussions](https://img.shields.io/badge/💬%20GitHub-Discussions-brightgreen?logo=github)](https://github.com/Samizo-AITL/openlane2-sram/discussions)

