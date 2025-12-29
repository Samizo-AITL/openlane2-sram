---
title: "Project Plan: OpenLane2 + SRAM Macro Integration"
repository: "openlane2-sram"
status: "planning"
target_pdk: "sky130"
---

# 00. Project Plan  
**OpenLane2 + SRAM Hard Macro Integration**

## Objective

The objective of this project is to use **OpenLane2** to  
**integrate an SRAM hard macro into a top-level design, perform place-and-route, and generate a final GDS**.

The SRAM itself is not a design target.  
It is treated as an **external hard macro (design constraint)**, focusing on realistic physical design integration.

---

## Scope

### In Scope
- Install OpenLane2 **without breaking existing OpenLane (v1) environments**
- Establish a **baseline OpenLane2 flow** without macros
- Integrate an SRAM hard macro (LEF / GDS / blackbox)
- Perform macro-aware floorplanning with halo / keepout
- Complete placement and routing and **generate a final GDS including the SRAM**
- Document the flow in a reproducible manner

### Out of Scope
- SRAM circuit design or generation
- Redistribution of SRAM macro files
- Full commercial sign-off (complete DRC/LVS closure)
- Replacement of OpenLane (v1)

---

## Deliverables

### Documentation
- `docs/00_plan.md`: Project plan (this document)
- `docs/10_env.md`: Environment setup and non-destructive installation
- `docs/20_openlane2.md`: OpenLane2 baseline flow
- `docs/30_macro_sram.md`: SRAM macro integration
- `docs/40_results.md`: Results and observations

### Design Artifacts
- Baseline GDS (no SRAM)
- Final GDS including SRAM macro
- Reports (area, timing, routing, etc.)

> PDKs and SRAM macro files are **not included** in this repository.

---

## Milestones

### M0: OpenLane2 Installation (Non-Destructive)
**Goal**
- Install OpenLane2 while preserving existing OpenLane (v1) setups

**Done Criteria**
- OpenLane2 runs inside a Python virtual environment
- Existing OpenLane (v1) flows continue to work unchanged

---

### M1: Baseline Flow (No SRAM)
**Goal**
- Validate and fix the basic OpenLane2 flow

**Done Criteria**
- A small design completes P&R successfully
- GDS is generated
- Steps are documented and reproducible

---

### M2: SRAM Macro Integration (Placement / Floorplan)
**Goal**
- Integrate the SRAM hard macro into the design

**Done Criteria**
- SRAM blackbox is integrated at RTL level
- Macro is placed as FIXED with halo / keepout
- A valid floorplan is established (routing not required at this stage)

---

### M3: Final GDS with SRAM
**Goal**
- Generate a final GDS including the SRAM macro

**Done Criteria**
- Placement and routing complete successfully
- Top-level GDS including SRAM is generated
- Major constraints, issues, and workarounds are documented

---

## Technical Policy

- SRAM is treated as a **FIXED hard macro**
- DRC/LVS are handled with **abstract views (LEF / maglef)** where appropriate
- PDKs and macros are referenced externally (links / environment variables)
- Reproducibility is prioritized over manual intervention

---

## Risks and Mitigations

| Risk | Mitigation |
|------|------------|
| SRAM macro redistribution restrictions | Do not include macros; document integration steps only |
| PDN issues around macro | Expose and address issues early in M2 |
| Excessive DRC violations | Exclude SRAM internal checks using abstract views |
| Environment dependency | Use venv and environment variables consistently |

---

## Positioning

This repository serves as a  
**minimal, practical example of macro-aware physical design using OpenLane2**.

The focus is on **clarity, reproducibility, and realistic constraints**, rather than maximum optimization.

---

*Last updated: Initial planning phase*
