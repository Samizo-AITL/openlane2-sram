---
title: "openlane2-sram"
description: "Macro-aware physical design example using OpenLane2 with an SRAM hard macro"
---

# 30. SRAM Hard Macro Integration  
**Macro-Aware Floorplanning in OpenLane2 (Classic Flow)**

## Purpose

This document explains how an **SRAM hard macro** is integrated into an
**OpenLane2 Classic flow**.

The SRAM is treated strictly as an **external, fixed hard macro**.
It is **not synthesized**, **not optimized**, and **not modified** by OpenLane2.

The focus is on:

- Blackbox RTL integration
- Abstract physical views (LEF / GDS)
- Fixed macro placement
- Macro-aware floorplanning using OpenROAD

---

## Key Reality Check (Important)

OpenLane2 **does not have a first-class “macro” concept**.

Instead:

- Macros are handled through **OpenROAD inputs**
- Placement constraints are enforced via **OpenROAD TCL**
- Configuration is driven by **JSON**, not YAML
- Macro integration is a **physical design constraint problem**, not a synthesis feature

This document reflects **what actually works**, not a conceptual abstraction.

---

## SRAM Macro Policy

- SRAM is a **hard macro**
- SRAM is **FIXED**
- SRAM internal DRC/LVS is **out of scope**
- Only **abstract views** are used during P&R

This mirrors real SoC physical design practice.

---

## Required SRAM Files

The following files must be provided **externally**:

| File | Purpose |
|-----|--------|
| `sram.gds` | Final physical layout |
| `sram.lef` | Abstract layout for P&R |
| `sram_blackbox.v` | Verilog blackbox |
| `sram.lib` | (Optional) timing model |

> These files are **not included** in this repository.

---

## Macro Directory Convention

Recommended (but not mandatory) structure:

```
macro/
└─ sram/
   ├─ README.md
   ├─ sram.lef        (external or symlink)
   ├─ sram.gds        (external or symlink)
   ├─ sram.lib        (optional)
   └─ sram_blackbox.v
```

Only the **integration method** is documented here.

---

## RTL Integration (Blackbox)

### SRAM Blackbox

**`design/src/sram_blackbox.v`**

```verilog
module sram (
    input  wire        clk,
    input  wire        csb,
    input  wire        web,
    input  wire [7:0]  addr,
    input  wire [31:0] din,
    output wire [31:0] dout
);
endmodule
```

No logic is defined.

---

### Top-Level Instantiation

**`design/src/top.v`**

```verilog
module top (
    input  wire        clk,
    input  wire        rst_n,
    input  wire        csb,
    input  wire        web,
    input  wire [7:0]  addr,
    input  wire [31:0] din,
    output wire [31:0] dout
);

sram u_sram (
    .clk  (clk),
    .csb  (csb),
    .web  (web),
    .addr (addr),
    .din  (din),
    .dout (dout)
);

endmodule
```

Synthesis treats `sram` as an unresolved blackbox.

---

## OpenLane2 Configuration (JSON-Based)

OpenLane2 Classic flow is driven by **JSON configuration files**.

### Key Inputs

- Verilog sources include the blackbox
- LEF and GDS are passed to OpenROAD
- No YAML macro sections are used

Example (conceptual):

```json
{
  "verilog_files": [
    "design/src/top.v",
    "design/src/sram_blackbox.v"
  ],
  "lef_files": [
    "macro/sram/sram.lef"
  ],
  "gds_files": [
    "macro/sram/sram.gds"
  ]
}
```

Exact filenames and paths depend on the design directory.

---

## Fixed Macro Placement (OpenROAD)

Macro placement is enforced using **OpenROAD commands**, not config keys.

Typical actions:

- Read macro LEF
- Read macro GDS
- Place macro at fixed coordinates
- Apply halo / keepout

Conceptually:

```tcl
place_macro u_sram 100 100 N
set_macro_fixed u_sram
set_macro_halo u_sram 10 10 10 10
```

These commands are executed **before standard-cell placement**.

---

## Floorplanning Strategy

- SRAM placement dominates the floorplan
- Standard cells are placed *around* the macro
- Routing channels must be preserved
- Die size may need to be enlarged early

Poor macro placement cannot be fixed later.

---

## PDN Considerations

- SRAM power pins must align with the top-level PDN
- LEF power pin names must match the PDK
- PDN issues are common and expected

Typical mitigation:
- Inspect macro LEF
- Adjust PDN straps
- Accept simplified PDN for demonstration flows

---

## DRC / LVS Policy

- SRAM internal DRC/LVS is **not performed**
- Abstract views are used
- Top-level DRC focuses on:
  - Macro spacing
  - Routing conflicts
  - Pin accessibility

This is an explicit trade-off.

---

## Expected Outcomes (M2)

- SRAM macro is visible in the floorplan
- Macro is FIXED with halo
- Standard cells are placed legally around it
- Routing may still be incomplete

A final GDS is **not required** at this stage.

---

## Common Failure Modes

| Issue | Cause | Mitigation |
|-----|------|------------|
| Macro overlaps cells | Missing halo | Increase halo |
| Congestion | Poor placement | Move macro / resize die |
| PDN errors | Pin mismatch | Inspect LEF |
| CTS issues | Clock near macro | Reroute clock |

---

## Role of This Step

This step proves that:

- OpenLane2 can handle hard macros
- Macro placement can be controlled
- The flow remains deterministic

Only after this is validated should final routing be attempted.

---

## Next Step

Proceed to:

➡ **`docs/40_results.md`**  
(Final GDS generation, warnings, and lessons learned)

---

*Last updated: SRAM macro integration validated in OpenLane2 Classic flow*
