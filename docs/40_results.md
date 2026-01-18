---
title: "openlane2-sram"
description: "Macro-aware physical design example using OpenLane2 with an SRAM hard macro"
---

# 40. Results and Observations  
## 📊 Final GDS Generation with SRAM Hard Macro

---

## 🎯 Purpose

This document summarizes the **final results** of the project, marking the
successful completion of **M3**.

It focuses on:

- 🧩 End-to-end **RTL → GDS completion** using OpenLane2
- 🧱 Integration of an **SRAM hard macro** as a fixed physical constraint
- 🧠 Practical observations from macro-aware physical design
- ⚠️ Known limitations and lessons learned

---

## ✅ Final Outcome (Summary)

### Achieved Results

- OpenLane2 **Classic flow** completed successfully across all stages
- A **final GDS** was generated that includes:
  - Standard-cell logic
  - An externally provided **SRAM hard macro**
- The entire flow is **fully reproducible** using documented commands and
  configuration files

This confirms that:

> **OpenLane2 can reliably handle hard macro integration  
> in a realistic SoC-style physical design flow.**

---

## 📦 Generated Artifacts

OpenLane2 produces per-run artifacts under the design directory:

```
designs/<design_name>/runs/RUN_<timestamp>/
└─ final/
   ├─ gds/
   │  └─ <design_name>.gds
   ├─ lef/
   ├─ def/
   ├─ views/
   └─ reports/
```

### Primary Deliverable

- **`final/gds/<design_name>.gds`**  
  Final top-level layout including the SRAM macro

This GDS has been verified by visual inspection.

---

## 🔍 Layout Verification

### Visual Inspection

Using **KLayout**, the following were confirmed:

- The SRAM macro appears as a **large, fixed rectangular block**
- Standard cells are placed legally around the macro
- Halo / keepout regions are respected
- No overlaps between macro and standard-cell placement

This confirms correct **macro placement and floorplan enforcement**.

---

## 🧪 DRC and LVS Status

### DRC

- Top-level DRC completed within the project scope
- Warnings may appear around:
  - Macro boundaries
  - Power distribution structures
- **Internal SRAM DRC is intentionally excluded**

All reported issues are **understood and acceptable** for this project.

---

### LVS

- LVS is limited to **top-level connectivity**
- The SRAM macro is treated as a **blackbox**
- Full sign-off LVS is **explicitly out of scope**

This matches the stated project policy.

---

## 📈 Metrics and Observations

Observed qualitative trends:

- Area utilization increases significantly due to the macro footprint
- Routing congestion concentrates near macro edges
- Floorplan quality has a **larger impact** than synthesis optimization

Exact numerical metrics are **design- and macro-dependent** and are not the
primary focus of this work.

---

## 🧠 Key Lessons Learned

### 1️⃣ Macro Placement Dominates the Design

- Macro placement decisions must be made **early**
- Small placement shifts can drastically affect routability

---

### 2️⃣ PDN Is the First Real Bottleneck

- SRAM power pins must align with the top-level PDN
- PDN-related issues are the most common cause of failed runs

---

### 3️⃣ Abstract Views Are Essential

- LEF / maglef views dramatically reduce DRC noise
- Attempting full SRAM internal DRC/LVS is impractical

---

### 4️⃣ OpenLane2 Is Macro-Capable but Explicit

- Macro integration is **not automatic**
- Constraints must be expressed explicitly
- The flow is more **transparent and debuggable**
  than OpenLane (v1)

---

## ⚠️ Known Limitations

The following limitations are **intentional**:

- Single SRAM macro only
- No advanced timing optimization
- CTS behavior around macros not tuned
- No production-grade sign-off

This repository prioritizes:

> **Clarity · Realism · Reproducibility**  
> over maximum optimization.

---

## 🔬 Recommended Next Experiments

Possible extensions include:

- Multiple SRAM macros
- Different macro orientations and placements
- Manual PDN refinement
- Comparison with OpenLane (v1) macro handling
- Integration of OpenRAM-generated SRAM macros

These can be explored **without changing the repository structure**.

---

## 🧭 Final Remarks

This project demonstrates a **practical, macro-aware OpenLane2 workflow**.

Its value lies in:

- Clear separation between logic and hard macros
- Explicit physical constraints
- Fully reproducible, documented steps

It provides a solid foundation for further exploration of:

> **Macro-based physical design using OpenLane2**

---

## 🖼 Layout Evidence

### Figure 1: SRAM Macro Block-Level View

<img src="https://raw.githubusercontent.com/Samizo-AITL/openlane2-sram/main/docs/fig/fig01_openlane2_spm_macro_block_level.png"
     alt="SRAM macro block-level view (OpenLane2)"
     width="80%">

The SRAM hard macro is integrated as a **fixed block** in the final GDS.

---

### Figure 2: Standard-Cell-Level View  
*(SRAM Internal Layout Not Visible)*

<img src="https://raw.githubusercontent.com/Samizo-AITL/openlane2-sram/main/docs/fig/fig02_openlane2_spm_standard_cell_level_view.png"
     alt="Standard-cell-level view around SRAM macro"
     width="80%">

Only standard-cell geometry is visible.  
The SRAM macro is treated as a hard macro and its internal transistor-level
layout is intentionally abstracted.

---

*Last updated: Final GDS generation verified and documented* ✅
