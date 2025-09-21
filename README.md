# VLSI RTL Design & Synthesis — Learning Repository

This repository documents my work during the **VSD RTL Design and Synthesis Workshop**.  
It contains day-wise implementations, simulations, synthesis runs, and supporting notes.  
The goal is to systematically capture both the technical steps and my understanding as I progress through the program.

---

Each `weekN/dayM` directory is self-contained and includes:
- **RTL** and **testbench** code
- **Simulation logs and VCD files**
- **Synthesis reports/logs**
- **Snapshots** proving authorship (user ID + timestamp)
- A short `README.md` with context and run instructions

---

## Tools and Environment

- **Icarus Verilog (iverilog)** — RTL simulation  
- **GTKWave** — waveform visualization  
- **Yosys** — logic synthesis  
- **Sky130 PDK** — standard cell libraries (for synthesis exercises)

---

## Day-wise Progress

### Day 1 — RTL Simulation and Synthesis Introduction
- **Design:** 2:1 multiplexer (`good_mux.v`)  
- **Testbench:** stimulus generation and VCD dump (`tb_good_mux.v`)  
- **Simulation:** compiled and executed with iverilog, waveform inspected in GTKWave  
- **Synthesis:** basic Yosys run on the RTL  
- **Proof of authorship:** terminal and waveform snapshots included in `snapshots/`

Key outputs:
- `rtl/good_mux.v`  
- `tb/tb_good_mux.v`  
- `sim/tb_good_mux.vcd`  
- `sim/sim_log.txt`  
- `yosys_run.log`  
- `snapshots/snap_Adi_gyt_<timestamp>.png`

---

### Upcoming Days
Further days will build upon these foundations with:
- More complex RTL designs  
- Advanced testbenches  
- Synthesis with Sky130 libraries  
- Documentation of learnings and results  

---

## Purpose of This Repository

This is not only a submission requirement but also a **structured record of my learning**.  
I aim to maintain it in a way that is **clear, reproducible, and professional**, so anyone can follow the flow and rerun the experiments if needed.

---
