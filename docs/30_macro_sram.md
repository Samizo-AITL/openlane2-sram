---
title: "SRAM Hard Macro Integration"
repository: "openlane2-sram"
status: "active"
target_pdk: "sky130"
milestone: "M2"
---

# 30. SRAM Hard Macro Integration  
**Macro-Aware Floorplanning in OpenLane2**

## Purpose

This document describes how to integrate an **SRAM hard macro** into an **OpenLane2** design.

The SRAM is treated strictly as an **external hard macro**, not as a synthesized block.  
The focus is on **macro-aware floorplanning, placement constraints, and routing preparation**.

---

## Design Philosophy

- SRAM is a **hard macro (FIXED)**
- SRAM is a **constraint**, not a design variable
- Internal SRAM DRC/LVS is **out of scope**
- Only abstract views are used for integration

This mirrors real-world SoC physical design practice.

---

## Required SRAM Macro Files

The following files are required for integration:

| File | Purpose |
|-----|--------|
| `sram.gds` | Physical layout |
| `sram.lef` | Abstract layout for P&R |
| `sram_blackbox.vh` | Verilog module declaration |
| `sram.lib` (optional) | Timing model for STA |

> These files are **not included** in this repository.

---

## Macro Directory Policy

SRAM macros are referenced externally.

Recommended layout:

```
macro/
└─ sram/
   ├─ README.md
   ├─ sram.lef        (external or symlink)
   ├─ sram.gds        (external or symlink)
   ├─ sram.lib        (optional)
   └─ sram_blackbox.vh
```

The repository documents **how** to integrate macros, not **the macros themselves**.

---

## SRAM Blackbox Declaration

Create a Verilog blackbox declaration.

**`design/src/sram_blackbox.vh`**

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

No logic is defined inside the module.

---

## Top-Level Integration

Instantiate the SRAM macro in the top module.

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

---

## OpenLane2 Configuration (Macro-Aware)

Update the configuration to include macro files.

**`design/config/config.yaml`**

```yaml
design_name: top

rtl:
  sources:
    - design/src/top.v
    - design/src/sram_blackbox.vh

macros:
  sram:
    lef:
      - macro/sram/sram.lef
    gds:
      - macro/sram/sram.gds
    lib:
      - macro/sram/sram.lib   # optional
```

---

## Fixed Macro Placement

SRAM placement must be **explicitly fixed**.

Example constraint:

```yaml
floorplan:
  macros:
    u_sram:
      location: [100, 100]
      orientation: N
      fixed: true
      halo: [10, 10, 10, 10]
```

### Notes
- Coordinates are in microns
- `halo` defines a keepout region around the macro
- Placement should be decided **before** standard-cell placement

---

## Floorplanning Considerations

- Place SRAM early in the flow
- Avoid narrow routing channels
- Reserve sufficient space for PDN straps
- Keep clock routing away from macro boundaries if possible

Macro placement dominates the entire floorplan.

---

## PDN Considerations

- SRAM power pins must align with the top-level PDN
- PDN issues are common failure points
- It is recommended to:
  - Inspect macro LEF power pins
  - Adjust PDN straps manually if required

PDN tuning is expected and normal.

---

## DRC and LVS Policy

- SRAM internal DRC/LVS is **not checked**
- Abstract views (`LEF` / `maglef`) are used
- Top-level DRC focuses on:
  - Macro spacing
  - Routing congestion
  - Pin accessibility

This is a deliberate and documented trade-off.

---

## Expected Outcomes (M2)

- SRAM macro is visible in the floorplan
- Macro is fixed with halo/keepout
- Standard cells are placed around the macro
- Routing may be incomplete at this stage

Full GDS generation is **not required** for M2.

---

## Common Failure Modes

| Issue | Cause | Mitigation |
|-----|------|------------|
| Macro overlaps cells | Missing halo | Increase halo |
| Routing congestion | Poor placement | Move macro / resize die |
| PDN connection failure | Pin mismatch | Inspect LEF and PDN config |
| CTS issues | Clock crossing macro | Reroute or isolate clock |

---

## Next Step

Proceed to:

➡ **`docs/40_results.md`**  
(Final GDS generation and observations)

---

*Last updated: SRAM macro integration defined*
