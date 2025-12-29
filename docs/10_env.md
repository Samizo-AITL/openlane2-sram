---
title: "Environment Setup: OpenLane2 (Non-Destructive)"
repository: "openlane2-sram"
status: "active"
target_pdk: "sky130"
---

# 10. Environment Setup  
**OpenLane2 Non-Destructive Installation Guide**

## Purpose

This document describes how to install and use **OpenLane2**  
**without breaking or modifying existing OpenLane (v1) environments**.

The primary goal is **environment isolation and reproducibility**.

---

## Design Principles

This project follows these strict environment rules:

1. **Do not modify existing OpenLane (v1) installations**
2. **Do not overwrite system-wide Python packages**
3. **Do not copy PDKs into this repository**
4. **Use virtual environments and external references only**

OpenLane2 is treated as a **separate tool**, not a replacement.

---

## Assumed Host Environment

- OS: Linux (Ubuntu 20.04 / 22.04 tested)
- Python: 3.9 or newer
- Existing OpenLane (v1): Optional, but preserved if present
- Existing PDK installation: Required (e.g. sky130)

This repository assumes a local environment similar to:

```
$HOME/
 ├─ OpenLane/              (existing OpenLane v1)
 ├─ OpenROAD/
 ├─ pdks/
 │   └─ sky130A/
 └─ openlane2-sram/        (this repository)
```

---

## Python Virtual Environment (Required)

OpenLane2 **must** be installed in a Python virtual environment.

### Create venv

```bash
cd openlane2-sram
python3 -m venv venv
```

### Activate venv

```bash
source venv/bin/activate
```

You should see `(venv)` in your shell prompt.

---

## Install OpenLane2

With the virtual environment activated:

```bash
pip install --upgrade pip
pip install openlane
```

Verify installation:

```bash
openlane --version
```

This command must work **only inside the venv**.

---

## PDK Configuration

### Policy

- PDKs are **not copied** into this repository
- PDKs are referenced via **environment variables**
- The same PDK can be shared between OpenLane v1 and OpenLane2

### Example: sky130

Set the PDK root path:

```bash
export PDK_ROOT=$HOME/pdks
export PDK=sky130A
```

Expected directory:

```bash
$PDK_ROOT/sky130A/
```

These variables can be placed in:
- `~/.bashrc`, or
- a project-local script (recommended)

---

## Verification: Non-Destructive Check

### Check OpenLane2

```bash
source venv/bin/activate
openlane --version
```

### Check existing OpenLane (v1)

```bash
cd $HOME/OpenLane
./flow.tcl -h
```

Both must work independently.

If OpenLane (v1) still runs normally, the setup is **non-destructive**.

---

## What This Setup Avoids

This project explicitly avoids:

- Docker-based OpenLane flows
- Conda environments
- System-wide Python installs
- PATH conflicts between OpenLane versions

This keeps the environment **transparent and debuggable**.

---

## Reproducibility Notes

- All commands required to recreate the environment are documented
- No hidden dependencies are assumed
- Manual GUI tools (Magic, KLayout) are optional and external

This ensures the flow can be reproduced on another machine.

---

## Next Step

After completing this setup, proceed to:

➡ **`docs/20_openlane2.md`**  
(Baseline OpenLane2 flow without SRAM)

---

*Last updated: Environment setup completed*
