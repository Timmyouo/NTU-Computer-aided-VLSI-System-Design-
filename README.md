# NTU Computer-aided VLSI System Design

> Full RTL-to-GDS digital IC design coursework — five RTL designs and one tape-out-style final project, taken end-to-end through synthesis, gate-level simulation, place & route, and power sign-off using the industry-standard Synopsys / Cadence flow.

**Course:** Computer-aided VLSI System Design (CVSD), National Taiwan University · Fall 2025  
**Languages:** SystemVerilog, Verilog, TCL  
**Technology:** TSMC 0.13 µm standard-cell library (CBDK / CIC)  

---

## Skills Demonstrated

| Domain | Specific Skills |
|--------|----------------|
| **RTL design** | SystemVerilog / Verilog · FSM-based control · datapath design · pipelining · fixed-point and IEEE-754 floating-point arithmetic · cryptographic and ECC datapaths |
| **Verification** | Self-checking testbenches · Verdi waveform debug · gate-level simulation with back-annotated SDF · post-route simulation |
| **Static analysis** | SpyGlass lint sign-off |
| **Logic synthesis** | Synopsys Design Compiler · SDC constraints · timing/area trade-off · clock gating · DesignWare component selection |
| **Physical design** | Cadence Innovus full APR flow — floorplanning, power planning, placement, CTS, routing, ECO |
| **Sign-off** | DRC · LVS-equivalent connectivity · antenna check · post-route STA · PrimeTime PX power analysis |
| **Tooling** | Synopsys VCS, Verdi, Design Compiler, PrimeTime PX · Cadence Innovus · SpyGlass · Linux / TCL / shell scripting |

---

## Complete Design Flow Implemented

```
   ┌────────┐   ┌────────┐   ┌────────────┐   ┌────────┐   ┌──────┐   ┌────────┐   ┌─────────┐
   │  RTL   │──▶│  Lint  │──▶│ Functional │──▶│  Logic │──▶│ Gate │──▶│  APR   │──▶│ Power & │
   │ (SV/V) │   │SpyGlass│   │  Sim (VCS) │   │  Synth │   │ Sim  │   │Innovus │   │ Sign-off│
   └────────┘   └────────┘   └────────────┘   │  (DC)  │   │ +SDF │   │        │   │  (PT-PX)│
                                              └────────┘   └──────┘   └────────┘   └─────────┘
       HW1 ✓        HW1–5 ✓      HW1–Final ✓     HW3,4,F ✓   HW3,4,F ✓   HW5,F ✓     HW4 ✓
```

---

## Projects

| # | Project | What I Built | Key Quantified Result |
|---|---------|--------------|-----------------------|
| **HW1** | [Fixed-Point ALU](HW1_ALU/) | 16-bit Q6.10 ALU with 10 instructions including MAC, Taylor approx., matrix transpose, count-leading-zeros | Lint-clean RTL, all functional tests pass |
| **HW2** | [Single-Cycle CPU](HW2_Single_Cycle_CPU/) | RISC-V-subset CPU integrating reused ALU + custom IEEE-754 FP multiplier and subtractor | Executes 17-instruction ISA across all test programs |
| **HW3** | [Convolution Accelerator](HW3_Convolution_Engine/) | Barcode-detect → 2D convolution engine, four parallel 8-bit output streams, SRAM-backed image/weight memory | Synth at **6 ns** clock, **178 k µm²** total cell area |
| **HW4** | [IoT Crypto + Sort Datapath](HW4_IoT_Data_Filtering/) | Full DES encrypt/decrypt + 128-bit CRC + sort, mode-selectable | Synth at **3 ns** clock, **51.3 k µm²**, PrimeTime PX power **4.87–6.48 mW** across modes |
| **HW5** | [Place & Route Sign-off](HW5_APR/) | Full Innovus APR flow on the HW3 convolution core: floorplan → CTS → route → sign-off | **0 DRC**, **0 antenna**, post-route WNS **+0.913 ns** (timing met) |
| **Final** | [BCH Error-Correcting Decoder](Final_BCH_Decoder/) | Runtime-configurable BCH(63,51) / BCH(255,239) / BCH(1023,983) decoder with hard-decision + Chase-II soft-decision modes; **complete RTL → GDS** | **6.5 ns** clock, **495 k µm² / 58 k cells**, post-route WNS **+0.913 ns**, **0 DRC / 0 antenna**, **77.8%** core utilization |

---

## Repository Layout

```
CVSD/
├── docs/images/              Reserved screenshots (see Visual highlights above)
├── HW1_ALU/                  Fixed-point ALU
├── HW2_Single_Cycle_CPU/     RISC-V-subset CPU + custom FPU
├── HW3_Convolution_Engine/   2D convolution accelerator (RTL → SYN)
├── HW4_IoT_Data_Filtering/   DES / CRC / Sort datapath (RTL → SYN → POWER)
├── HW5_APR/                  Place & route sign-off (Innovus)
└── Final_BCH_Decoder/        BCH ECC decoder, full RTL → GDS
```

Each subfolder follows the standard CVSD numbered-stage convention so the same flow can be reproduced in any project:

```
01_RTL/   → source RTL + simulation scripts
02_SYN/   → DC synthesis script, SDC, netlist, area/timing reports
03_GATE/  → gate-level simulation with back-annotated SDF
04_APR/   → Innovus place-and-route output, sign-off reports
05_POST/  → post-route simulation
06_POWER/ → PrimeTime PX power results (HW4 only)
```

A focused per-project README inside each folder describes the architecture, key design decisions, and results.

---

## Notes on Reproducibility

This repository contains the **source RTL, scripts, constraints, and tool reports** from the original flow. Tool-generated artifacts that are large or environment-specific are excluded via `.gitignore` (Innovus `DBS/`, VCS `simv` / `csrc/`, SpyGlass DB, `*.gds`, compressed timing dumps, etc.) — the actual reports and netlists used for sign-off are kept.

Re-running the flow requires Synopsys / Cadence licenses and the TSMC 0.13 µm CBDK PDK from the NTU CVSD lab environment.

  <img src="docs/images/bch_floorplan.png" width="720" alt="Slot 1: add bch_floorplan.png — BCH post-route layout"/>