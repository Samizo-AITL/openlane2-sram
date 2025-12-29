---
title: "Baseline OpenLane2 Flow (No SRAM)"
repository: "openlane2-sram"
status: "active"
target_pdk: "sky130"
milestone: "M1"
---

# 20. Baseline OpenLane2 Flow  
**Minimal OpenLane2 Run Without SRAM**

## Purpose

This document establishes a **baseline OpenLane2 flow** without any macros.  
The goal is to validate the toolchain, environment, and basic configuration **before introducing SRAM hard macros**.

This baseline serves as the reference for all later macro-aware changes.

---

## Prerequisites

- OpenLane2 installed in a Python virtual environment  
  (see `docs/10_env.md`)
- PDK available locally (e.g. sky130)
- Environment variables set:

```bash
export PDK_ROOT=$HOME/pdks
export PDK=sky130A
```

- Virtual environment activated:

```bash
source venv/bin/activate
```

---

## Design Selection

A **small, simple RTL design** is intentionally used.

Requirements:
- Single clock domain
- Minimal I/O
- No macros
- Synthesizable with standard cells only

This reduces noise and isolates OpenLane2 behavior.

---

## Directory Setup

Expected repository layout:

```
openlane2-sram/
├─ design/
│  ├─ src/
│  │  └─ top.v
│  └─ config/
│     └─ config.yaml
├─ scripts/
│  └─ 10_run_base.sh
└─ artifacts/
```

---

## Example RTL (`design/src/top.v`)

```verilog
module top (
    input  wire clk,
    input  wire rst_n,
    output reg  out
);

always @(posedge clk or negedge rst_n) begin
    if (!rst_n)
        out <= 1'b0;
    else
        out <= ~out;
end

endmodule
```

This trivial design is sufficient to exercise synthesis, placement, routing, and GDS generation.

---

## OpenLane2 Configuration

Create a minimal configuration file:

**`design/config/config.yaml`**

```yaml
design_name: top

rtl:
  sources:
    - design/src/top.v

clocking:
  clocks:
    clk:
      period: 10.0

pdk:
  name: sky130A
```

No macro-related settings are used at this stage.

---

## Running the Baseline Flow

A helper script is recommended.

**`scripts/10_run_base.sh`**

```bash
#!/usr/bin/env bash
set -e

source venv/bin/activate

openlane run \
  --design design \
  --config design/config/config.yaml \
  --tag base_no_sram
```

Make it executable:

```bash
chmod +x scripts/10_run_base.sh
```

Run:

```bash
./scripts/10_run_base.sh
```

---

## Expected Outputs

After a successful run, the following should exist:

```
runs/base_no_sram/
├─ final/
│  ├─ gds/
│  │  └─ top.gds
│  ├─ lef/
│  └─ reports/
└─ logs/
```

Key success indicator:
- `top.gds` is generated without errors

---

## Verification Checklist

- [ ] Synthesis completes successfully
- [ ] Placement completes successfully
- [ ] Routing completes successfully
- [ ] GDS is generated
- [ ] No macro-related warnings appear

Minor timing or DRC warnings are acceptable at this stage.

---

## Notes

- This run intentionally avoids optimization tuning
- The goal is **flow validation**, not performance
- Any failure here must be resolved **before** adding SRAM macros

This baseline is the control experiment for later comparisons.

---

## Next Step

Proceed to:

➡ **`docs/30_macro_sram.md`**  
(Integrating an SRAM hard macro: blackbox, LEF/GDS, and floorplanning)

---

*Last updated: Baseline OpenLane2 flow validated*
