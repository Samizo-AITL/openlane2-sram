---
title: "openlane2-sram"
description: "Macro-aware physical design example using OpenLane2 with an SRAM hard macro"
---

# 00. Project Plan  
**OpenLane2 + SRAM Hard Macro Integration**

## Objective

The objective of this project is to use **OpenLane2** to:

- Integrate an **SRAM hard macro** into a top-level design  
- Perform **macro-aware place-and-route**
- Generate a **final GDS including the SRAM macro**

The SRAM itself is **not a design target**.  
It is treated strictly as an **external hard macro (physical constraint)** to demonstrate realistic SoC-style physical integration.

---

## Scope

### In Scope
- Install **OpenLane2** in an isolated, non-destructive environment
- Establish a **baseline OpenLane2 flow** (no macros)
- Integrate an **SRAM hard macro** (LEF / GDS / blackbox)
- Perform macro-aware floorplanning (FIXED placement, halo / keepout)
- Complete placement, routing, and **final GDS generation**
- Document the full flow in a **reproducible and educational manner**

### Out of Scope
- SRAM circuit design or generation
- Redistribution of proprietary SRAM macro files
- Full commercial sign-off (complete DRC/LVS closure)
- Replacement of OpenLane (v1)

---

## Deliverables

### Documentation
- `docs/00_plan.md`: Project plan and milestones (this document)
- `docs/10_env.md`: Environment setup and non-destructive installation
- `docs/20_openlane2.md`: Baseline OpenLane2 flow
- `docs/30_macro_sram.md`: SRAM macro integration
- `docs/40_results.md`: Results, warnings, and lessons learned

### Design Artifacts
- Baseline GDS (standard-cell-only design)
- Final GDS including SRAM macro
- OpenLane2 reports (area, timing, routing, DRC summary)

> PDKs and SRAM macro files are **not included** in this repository.

---

## Milestones and Status

### M0: OpenLane2 Installation (Non-Destructive) ✅
**Goal**
- Install OpenLane2 while preserving existing OpenLane (v1) setups

**Result**
- OpenLane2 runs inside a Python virtual environment
- Existing OpenLane (v1) flows remain unaffected

---

### M1: Baseline Flow (No SRAM) ✅
**Goal**
- Validate a minimal end-to-end OpenLane2 flow

**Result**
- A small design completes placement and routing
- GDS is generated successfully
- Steps are documented and reproducible

---

### M2: SRAM Macro Integration (Placement / Floorplan) ✅
**Goal**
- Integrate the SRAM hard macro into the design

**Result**
- SRAM is declared as a blackbox at RTL level
- Macro is placed as **FIXED**
- Halo / keepout regions are applied
- Floorplan is stable prior to routing

---

### M3: Final GDS with SRAM ✅
**Goal**
- Generate a final GDS including the SRAM macro

**Result**
- Placement and routing complete successfully
- Final GDS including the SRAM macro is generated
- Warnings and limitations are analyzed and documented

---

## Technical Policy

- SRAM is treated as a **FIXED hard macro**
- Only **abstract views (LEF / maglef)** are used for macro checking
- Internal SRAM DRC/LVS is intentionally excluded
- PDKs and macros are referenced externally via environment variables
- Reproducibility is prioritized over manual intervention

---

## Risks and Mitigations

| Risk | Mitigation |
|------|------------|
| SRAM macro redistribution restrictions | Do not include macros; document integration steps only |
| PDN issues around macro | Address via floorplan constraints and early checks |
| Excessive DRC violations | Use abstract views and exclude macro internals |
| Environment dependency | Use venv and explicit configuration files |

---

## Positioning

This repository serves as a:

**Minimal, practical, and reproducible example of macro-aware physical design using OpenLane2**

It is intended for:
- Engineers transitioning from standard-cell-only flows
- OpenLane / OpenROAD users integrating hard macros
- Educational and reference use cases

The focus is on **clarity, realism, and reproducibility**, not maximum optimization.

---

*Last updated: Final GDS generation completed*
