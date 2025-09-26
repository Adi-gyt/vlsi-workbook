# VLSI RTL Design & Synthesis — Learning Repository

This repository documents my work during the **VSD RTL Design and Synthesis Workshop**.
It contains day-wise implementations, simulations, synthesis runs, and supporting notes.

The goal is to systematically capture both the **technical steps** and my **understanding** as I progress through the program. This repo also serves as a structured record of my learning — maintained in a way that is **clear, reproducible, and professional**.

---

## 📂 Repository Structure

Each `weekN/dayM` directory is self-contained and includes:

* **RTL and testbench code**
* **Simulation logs and VCD files**
* **Synthesis reports/logs**
* **Snapshots proving authorship** (terminal ID + timestamp)
* **A short README.md** with context and run instructions

---

## 🛠 Tools and Environment

* **Icarus Verilog (iverilog)** — RTL simulation
* **GTKWave** — waveform visualization
* **Yosys** — logic synthesis
* **Sky130 PDK** — standard cell libraries (for synthesis exercises)

---

## 📅 Day-wise Progress

### **Day 1 — RTL Simulation and Synthesis Introduction**

* Designed a 2:1 multiplexer in Verilog and created a testbench.
* Simulated with Icarus Verilog and inspected waveforms in GTKWave.
* Performed a basic Yosys synthesis run on the RTL.
* Documented outputs and included proof-of-authorship snapshots.

---

### **Day 2 — Timing, Cells, and Synthesis Basics**

* Learned about **standard cell libraries** and their role in synthesis.
* Explored **timing-driven synthesis** and analyzed yosys reports.
* Generated netlists and compared them with RTL simulation.

---

### **Day 3 — Combinational and Sequential Logic**

* Compared **combinational vs. sequential logic** in RTL.
* Understood **blocking vs. non-blocking assignments** and how they affect simulation.
* Observed cases where improper coding led to mismatches between **simulation and synthesis**.

---

### **Day 4 — Gate-Level Simulation (GLS) & Mismatch Debugging**

* Performed **GLS** using synthesized netlists.
* Compared **RTL simulation vs. GLS outputs** to identify mismatches.
* Gained insights into **coding discipline** required for reliable synthesis.

---

### **Day 5 — Optimization in Synthesis**

* Experimented with **if-else**, **for-loops**, and **generate blocks** in Verilog.
* Learned how **inferred latches** occur due to poor coding practices.
* Applied optimizations for better **synthesizable RTL design**.

---

## 🚀 Upcoming Days

The following days will build upon these foundations with:

* More complex RTL designs
* Advanced testbenches
* Synthesis using **Sky130 standard cell libraries**
* Documentation of results and key learnings

---

## 🎯 Purpose of This Repository

This repository is not just a submission requirement — it is a **personal learning log** that captures both theory and practice. My aim is to:

* Maintain clarity and reproducibility so others can **rerun the experiments**.
* Keep the documentation **professional and concise**.
* Show steady progress in **VLSI RTL design and synthesis** skills.
