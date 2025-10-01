# Week 2 – BabySoC Fundamentals & Functional Modelling

## 🌱 My Understanding of SoC (still learning!)

A **System-on-Chip (SoC)** is basically a small computer built into one chip.  
Instead of having separate CPU, memory, and peripherals as different ICs, an SoC puts everything together.  

From what I understood:
- The **CPU** runs instructions (like the “brain”).
- **Memory** stores the program and variables.
- The **bus/interconnect** is like a road that carries data and control between blocks.
- **Peripherals** (GPIO, DAC, UART, etc.) let the chip talk to the outside world.
- **Reset/Clocking** makes sure the chip starts clean and runs in sync.

I’m still wrapping my head around this, but it feels like SoC = “all the essentials of a computer, but tiny and on-chip.”

---

## 🍼 Why BabySoC?

Real SoCs are huge and complex. BabySoC is like a “hello world” version.  
It has:
- a small RISC-V CPU core (RVMyth),
- a PLL to generate a stable clock,
- and a DAC to produce analog outputs.

What I liked is that it’s simple enough to actually *see signals in GTKWave* without drowning in complexity.  
It’s basically the training wheels for learning SoC design.

---

## 🛠️ Functional Modelling (what I learned)

Before going into fancy steps like synthesis or layout, we need to check:  
**Does the design *functionally* behave the way we expect?**

Functional modelling = run the RTL in a simulator and look at the waveforms.  

My takeaways:
- I used **Icarus Verilog** to compile and simulate.
- Then used **GTKWave** to open the `.vcd` file.
- At first, my dump file was empty (forgot `$dumpfile` + `$dumpvars`). Once I fixed that, I saw all the signals.
- This step is important because it shows early whether reset, clocks, and CPU program flow are wired correctly.

---

## 🎵 Example I Understood – Audio Tone with BabySoC

One cool example I came across was making an audio tone:

1. PLL locks → gives stable clock.
2. CPU starts running from reset address.
3. A program writes sine values (from a lookup table) into the **DAC register** one by one.
4. DAC converts those digital values into voltage steps.
5. With a filter + speaker, it becomes an audible tone!

This clicked for me because it shows the whole flow: **clock → CPU → bus → DAC → real-world output**.

---

## 📸 Waveforms (what I checked in GTKWave)

I still don’t know all the signals, but here’s what I tried to capture:
- **Reset sequence**: chip goes from reset → normal run.
- **Clock**: PLL clock looks stable after lock.
- **CPU program counter**: increments as instructions run.
- **DAC bus write**: CPU writing values to DAC register.
- **DAC output**: saw the staircase waveform (looked like a rough sine).

*(Screenshots go here once I save them!)*

---

## 🚀 What I Learned This Week

- An SoC = CPU + memory + bus + peripherals all on one chip.  
- BabySoC is a tiny but clear model to practice on.  
- Functional modelling is about simulating behavior before touching synthesis.  
- Learned to use `$dumpfile` / `$dumpvars` to get `.vcd`.  
- First time I saw analog + digital signals together in GTKWave — that was really cool.  
- The idea of writing to a DAC register and hearing sound made it all feel “real.”

---
