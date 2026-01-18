---
title: "openlane2-sram"
description: "Macro-aware physical design example using OpenLane2 with an SRAM hard macro"
---

# 20. Baseline OpenLane2 Flow  
## 🧪 Minimal OpenLane2 Classic Flow Without SRAM

---

## 🎯 Purpose

This document records a **validated baseline OpenLane2 flow**  
**without any macros**.

The purpose of this step is to:

- ✅ Confirm that **OpenLane2 itself works end-to-end**
- 🔗 Validate the **environment, PDK linkage, and CLI usage**
- 🧱 Establish a **known-good reference run** before introducing SRAM macros

All later macro integration work is built on top of this baseline.

---

## ⚠️ Important Note  
### OpenLane2 vs OpenLane (v1)

OpenLane2 is **not CLI-compatible** with OpenLane (v1).

Key differences relevant to this document:

- ❌ OpenLane2 does **not** use `openlane run`
- 📄 Designs are driven by **explicit configuration files**
- 🚫 Passing only a design directory is **not supported**
- 🔒 OpenLane2 validates configuration keys **strictly**

This document reflects **actual working OpenLane2 behavior**,  
**not v1 conventions or assumptions**.

---

## 📋 Prerequisites

- OpenLane2 installed in a Python virtual environment  
  (see `docs/10_env.md`)
- PDK available locally (sky130)
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

## 🧱 Baseline Design Choice

A **very small and simple design** is used intentionally.

Design constraints:

- Single clock domain
- No macros
- Minimal logic
- Standard-cell only

This minimizes noise and isolates **pure OpenLane2 flow behavior**.

---

## 📁 Using the Reference Design (spm)

Instead of creating custom RTL, this baseline uses the  
**known-good reference design** provided with OpenLane2:

```
openlane2-designs/designs/spm
```

This avoids RTL-related issues and keeps the focus on the toolchain.

Verified contents:

```
spm/
├─ config.json
├─ src/
│  ├─ spm.v
│  └─ spm.sdc
├─ pin_order.cfg
└─ run_config.json
```

This design is:

- ✅ Macro-free  
- ✅ Minimal  
- ✅ Suitable as a baseline reference

---

## ▶ Running the Baseline Flow (Classic)

From the OpenLane2 repository root:

```bash
cd ~/openlane2
source venv/bin/activate
```

Run OpenLane2 using an **explicit configuration file**:

```bash
poetry run openlane \
  --design-dir ../openlane2-designs/designs/spm \
  ../openlane2-designs/designs/spm/config.json
```

### Key points

- `--design-dir` explicitly points to the design directory
- The configuration file path **must exist**
- OpenLane2 automatically selects the **Classic flow**

---

## 🔄 Expected Flow Stages

A successful run executes the full Classic flow:

1. Verilator lint
2. Synthesis
3. Floorplanning
4. Placement
5. Clock Tree Synthesis (CTS)
6. Routing
7. DRC
8. GDS generation

Successful completion message:

```
Flow complete.
```

---

## 📦 Expected Outputs

After completion, artifacts are generated under:

```
openlane2-designs/designs/spm/runs/RUN_*/
└─ final/
   ├─ gds/
   │  └─ spm.gds
   ├─ lef/
   ├─ reports/
   └─ views/
```

### Primary success indicator

- ✅ `final/gds/spm.gds` exists
- ✅ The GDS opens correctly in KLayout

---

## ✅ Verification Checklist

- [x] OpenLane2 runs without CLI errors
- [x] Synthesis completes
- [x] Placement completes
- [x] Routing completes
- [x] DRC stage completes
- [x] Final GDS is generated
- [x] No macros are involved

⚠️ Warnings (lint, routing heuristics, etc.) are acceptable at this stage.

---

## 🧠 Observations

- OpenLane2 enforces **strict configuration validation**
- v1-style keys (`design_name`, `rtl`, `clocking`) are rejected
- Explicit configuration files are mandatory
- The Classic flow is stable for macro-free designs

This confirms the **environment and toolchain are sound**.

---

## 🧭 Role of This Baseline

This baseline serves as:

- 🟢 A **known-good reference**
- 🧪 A control experiment for macro integration
- 🧯 A debugging anchor if later macro runs fail

⚠️ No SRAM-related work should begin until this baseline is reproducible.

---

## ➡ Next Step

Proceed to:

➡ **`docs/30_macro_sram.md`**  
_SRAM hard macro integration: blackbox, LEF/GDS, fixed placement_

---

*Last updated: Baseline OpenLane2 Classic flow completed and GDS verified* ✅
