---
title: "openlane2-sram"
description: "Macro-aware physical design example using OpenLane2 with an SRAM hard macro"
---

# 10. Environment Setup  
## 🧪 OpenLane2 Non-Destructive Installation Guide

---

## 🎯 Purpose

This document describes how to install and use **OpenLane2**  
**without breaking or modifying existing OpenLane (v1) environments**.

The primary goals are:

- 🔒 **Environment isolation**
- 🧯 **Non-destructive coexistence**
- ♻️ **Reproducibility**

OpenLane2 is treated as a **separate toolchain**,  
**not a replacement** for OpenLane (v1).

---

## 🧭 Design Principles

This project strictly follows these environment rules:

1. ❌ **Do not modify existing OpenLane (v1) installations**
2. ❌ **Do not overwrite system-wide Python packages**
3. ❌ **Do not copy PDKs into this repository**
4. ✅ **Use Python virtual environments and external references only**

Violating any of these rules risks:
- breaking existing flows, or
- reducing long-term reproducibility.

---

## 🖥 Verified Host Environment

This setup has been verified under the following conditions:

- **OS**: Linux (Ubuntu 20.04 / 22.04)
- **Python**: 3.9 or newer
- **OpenLane (v1)**: Optional, preserved if present
- **Existing PDK installation**: Required (e.g. sky130)

Typical directory layout:

```
$HOME/
 ├─ OpenLane/              # Existing OpenLane v1 (unchanged)
 ├─ OpenROAD/
 ├─ pdks/
 │   └─ sky130A/
 └─ openlane2-sram/        # This repository
```

---

## 🐍 Python Virtual Environment (Required)

OpenLane2 **must** be installed inside a Python virtual environment.

### ▶ Create virtual environment

```bash
cd openlane2-sram
python3 -m venv venv
```

### ▶ Activate virtual environment

```bash
source venv/bin/activate
```

After activation, the shell prompt should look like:

```
(venv) user@host:~/openlane2-sram$
```

---

## 📦 Install OpenLane2 (Isolated)

> **Important**: The virtual environment must be active.

```bash
# activate venv (if not already active)
source venv/bin/activate

# upgrade pip
pip install --upgrade pip

# install OpenLane2
pip install openlane

# verify installation
openlane --version
```

### Expected behavior

- ✅ The `openlane` command works **inside** the virtual environment
- ❌ The `openlane` command is **not available** outside the virtual environment

This confirms proper toolchain isolation and non-destructive coexistence.

---

## 🧱 PDK Configuration

### 📜 Policy

- PDKs are **not included** in this repository
- PDKs are referenced using **environment variables**
- The same PDK installation may be shared between  
  OpenLane (v1) and OpenLane2

### 🧩 Example: sky130

```bash
export PDK_ROOT=$HOME/pdks
export PDK=sky130A
```

Expected directory structure:

```
$PDK_ROOT/sky130A/
```

These variables can be defined in:

- `~/.bashrc`, or
- a **project-local setup script** (recommended for reproducibility)

---

## 🔍 Verification: Non-Destructive Coexistence

### ▶ Verify OpenLane2

```bash
source venv/bin/activate
openlane --version
```

### ▶ Verify existing OpenLane (v1)

```bash
cd $HOME/OpenLane
./flow.tcl -h
```

✅ **Both tools must work independently.**

If OpenLane (v1) continues to operate normally,  
the setup is confirmed to be **non-destructive**.

---

## 🚫 What This Setup Explicitly Avoids

This project intentionally avoids:

- Docker-based OpenLane flows
- Conda environments
- System-wide Python installations
- PATH-based OpenLane version switching

These choices keep the environment:

> **Simple · Explicit · Debuggable**

---

## ♻️ Reproducibility Notes

- All required commands are documented explicitly
- No implicit dependencies are assumed
- GUI tools (KLayout, Magic) are optional and external
- The environment can be recreated on a fresh machine

This ensures **long-term reproducibility**.

---

## ➡ Next Step

After completing this setup, proceed to:

➡ **`docs/20_openlane2.md`**  
_Baseline OpenLane2 flow without SRAM macros_

---

*Last updated: OpenLane2 environment verified and used for final GDS generation* ✅
