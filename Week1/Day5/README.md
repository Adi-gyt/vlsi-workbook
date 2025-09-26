# Day 5: Conditional Constructs, Inferred Latches, and Optimization in Synthesis

Welcome to **Day 5** of the RTL Workshop!
Today’s session focuses on **conditional constructs (`if` and `case`), pitfalls like inferred latches, and the power of loops (`for`) and `generate` blocks** in Verilog.

We will study how these constructs translate into hardware, what dangers arise from incomplete or overlapping code, and how to write **scalable, synthesizable RTL**.
Hands-on labs are included to reinforce the concepts.

---

## Contents

1. If-Else Statements in Verilog
2. Inferred Latches in Verilog
3. Labs for If-Else and Case Statements

   * Lab 1: Incomplete If Statement
   * Lab 2: Synthesis Result of Lab 1
   * Lab 3: Nested If-Else
   * Lab 4: Synthesis Result of Lab 3
   * Lab 5: Complete Case Statement
   * Lab 6: Synthesis Result of Lab 5
   * Lab 7: Incomplete Case Handling
   * Lab 8: Partial Assignments in Case
   * Lab 9: Bad/Overlapping Case
4. For Loops in Verilog
5. Generate Blocks in Verilog
6. Ripple Carry Adder (RCA) with Generate
7. Labs on Loops and Generate Blocks

   * Lab 10: 4-to-1 MUX Using For Loop
   * Lab 11: 8-to-1 Demux Using Case
   * Lab 12: 8-to-1 Demux Using For Loop
   * Lab 13: 8-bit Ripple Carry Adder with Generate Block
8. Best Practices & Summary

---

## 1. If-Else Statements in Verilog

* `if-else` is used inside **procedural blocks** (`always`, `initial`, tasks, functions).
* **If-else infers priority logic** → first true condition takes control. Hardware resembles a **chain of multiplexers**.

### Syntax

```verilog
if (cond1)
   y = a;
else if (cond2)
   y = b;
else
   y = c;
```

### Hardware Mapping

* `cond1` → highest priority, then `cond2`, etc.
* If conditions are incomplete, synthesis **inserts a latch** to hold values.

---

## 2. Inferred Latches in Verilog

* **Definition:** A latch is inferred when a variable is not assigned on all execution paths of a **combinational** block.
* This may lead to **unwanted memory behavior** and **timing unpredictability**.

### Example (Bad – latch inferred)

```verilog
always @(*) begin
   if (sel)
      y = a;  // No else! → y holds previous value
end
```

### Corrected (Safe)

```verilog
always @(*) begin
   if (sel)
      y = a;
   else
      y = b; // ensures y is always assigned
end
```

Or initialize at top:

```verilog
always @(*) begin
   y = 0; // default
   if (sel)
      y = a;
end
```

---

## 3. Labs for If-Else and Case Statements

### Lab 1: Incomplete If Statement

```verilog
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
   if (i0)
      y <= i1; // missing else → latch
end
endmodule
```
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

### Lab 2: Synthesis Result of Lab 1
![Alt text](path/to/image.png)

* Yosys shows a **D-latch** with `i0` as enable.

---

### Lab 3: Nested If-Else

```verilog
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
   if (i0)
      y <= i1;
   else if (i2)
      y <= i3;
end
endmodule
```
![Alt text](path/to/image.png)

### Lab 4: Synthesis Result of Lab 3
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

* Still infers a latch because not all conditions assign `y`.

---

### Lab 5: Complete Case Statement

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
   case(sel)
      2'b00: y = i0;
      2'b01: y = i1;
      default: y = i2;
   endcase
end
endmodule
```

* Synthesizes into a proper **mux** (no latch).
![Alt text](path/to/image.png)

---

### Lab 6: Incomplete Case Handling (Bad Case)

```verilog
module bad_case (input i0, i1, i2, i3, input [1:0] sel, output reg y);
always @(*) begin
   case(sel)
      2'b00: y = i0;
      2'b01: y = i1;
      2'b10: y = i2;
      2'b1?: y = i3; // Overlaps with 2’b10 → ambiguous
   endcase
end
endmodule
```

* **Overlapping case patterns** create unpredictable output.
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

---

### Lab 7: Partial Assignments in Case

```verilog
module partial_case_assign (input i0,i1,i2,input [1:0] sel,output reg y,output reg x);
always @(*) begin
   case(sel)
      2'b00: begin y = i0; x = i2; end
      2'b01: y = i1;  // x missing → latch!
      default: begin x = i1; y = i2; end
   endcase
end
endmodule
```

* `x` not assigned in `2'b01` branch → latch inferred.
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

---

## 4. For Loops in Verilog

* Used inside **procedural blocks** (`always`, `initial`).
* Useful for **scalable behavioral code** like muxes/demuxes.
* Iteration count must be **static and fixed at compile time**.

### Example: 4-to-1 MUX

```verilog
integer i;
always @(*) begin
   y = 0;
   for (i=0; i<4; i=i+1)
      if (i == sel)
         y = data[i];
end
```

---

## 5. Generate Blocks in Verilog

* `generate` + `genvar` is evaluated at **elaboration** (compile time).
* Used for **instantiating hardware multiple times** (structural replication).

### Example: 4 AND gates

```verilog
genvar i;
generate
   for (i=0; i<4; i=i+1) begin : gen_loop
      and_gate u_and (.a(in[i]), .b(in2[i]), .y(out[i]));
   end
endgenerate
```

---

## 6. Ripple Carry Adder (RCA)

* RCA = **chain of full adders**.
* Carry-out of each stage connects to carry-in of next stage.
* Simple but has **long delay** (carry ripples through all adders).

---

## 7. Labs on Loops and Generate Blocks

### Lab 10: 4-to-1 MUX Using For Loop

```verilog
module mux_generate (input i0,i1,i2,i3,input [1:0] sel,output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;
always @(*) begin
   for (k=0; k<4; k=k+1)
      if (k == sel)
         y = i_int[k];
end
endmodule
```
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

---

### Lab 11: 8-to-1 Demux Using Case

```verilog
module demux_case (output o0,o1,o2,o3,o4,o5,o6,o7,input [2:0] sel,input i);
reg [7:0] y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
always @(*) begin
   y_int = 8'b0;
   case(sel)
      3'b000: y_int[0] = i;
      3'b001: y_int[1] = i;
      ...
      3'b111: y_int[7] = i;
   endcase
end
endmodule
```
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

---

### Lab 12: 8-to-1 Demux Using For Loop

```verilog
module demux_generate (output o0,o1,o2,o3,o4,o5,o6,o7,input [2:0] sel,input i);
reg [7:0] y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @(*) begin
   y_int = 8'b0;
   for (k=0; k<8; k=k+1)
      if (k == sel)
         y_int[k] = i;
end
endmodule
```
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

---

### Lab 13: 8-bit Ripple Carry Adder with Generate

```verilog
module rca (input [7:0] num1,num2,output [8:0] sum);
wire [7:0] int_sum,int_co;

genvar i;
generate
   for (i=1; i<8; i=i+1) begin
      fa u_fa (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
   end
endgenerate

fa u_fa0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8]   = int_co[7];
endmodule

module fa (input a,b,c,output co,sum);
   assign {co,sum} = a + b + c;
endmodule
```
![Alt text](path/to/image.png)
![Alt text](path/to/image.png)

---

## 8. Best Practices & Summary

### Key Takeaways

* **If-else** → priority logic (chain of muxes).
* **Case** → parallel logic (mux).
* Always include **else** / **default** to avoid latches.
* Avoid **partial assignments** and **overlapping cases**.
* Use **for** loops for scalable behavioral code.
* Use **generate** for repeated hardware instantiation.
* RCA = chain of adders → generate makes it easy.

### Best Practices

* Use `always @(*)` for combinational logic.
* Assign **default values** at start of block.
* Use **blocking (`=`)** in combinational, **non-blocking (`<=`)** in sequential.
* Check synthesis logs for **inferred latches (DLATCH)**.
* Avoid `casex` unless absolutely necessary; prefer `casez` with caution.
* In SystemVerilog, use `unique` / `priority case` for clarity.
