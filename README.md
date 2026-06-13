# 4-Bit Synchronous Counter with Verification Environment

## 📌 Project Overview
This repository contains a synthesizable **4-bit Synchronous Up-Counter** written in Verilog HDL, accompanied by a self-checking testbench architecture. This project serves as a foundational demonstration of register-transfer level (RTL) design, clock-edge tracking, and basic design verification (DV) principles.

The counter increments its value by 1 on every rising edge (`posedge`) of the clock signal, features a synchronous active-high reset, and handles overflow/rollover seamlessly.

---

## 🏗️ Hardware Architecture & Design Details

### Design Module: `sync_counter_4bit.v`
The design implements sequential logic utilizing non-blocking assignments (`<=`) inside a clock-edge triggered block. 

* **Clock Domain:** Synchronous, operating strictly on the positive edge (`posedge clk`).
* **Reset Strategy:** Synchronous active-high reset (`rst`). When `rst == 1'b1`, the counter clears to `4'b0000` on the next valid clock edge.
* **Overflow Handling:** Natural hardware rollover. Upon reaching its maximum capacity (`4'b1111` or decimal 15), the next clock cycle naturally wraps the counter back to `4'b0000` due to 4-bit bit-width limitations.

---

## 🧪 Verification Strategy & Testbench Architecture

### Testbench Module: `tb_sync_counter_4bit.v`
The verification environment is designed to closely mimic professional directed-testing scenarios by isolating stimulus generation from the design tracking blocks.

### Key Verification Highlights:
1. **Automated Clock Generation:** Uses a free-running loop to simulate a continuous symmetric clock with a stable time period ($T = 10 \text{ time units}$).
2. **Race-Condition Elimination:** Checks are evaluated exactly $1 \text{ time unit}$ after the clock edge (`#1`) ensuring that the simulator signals have fully stabilized before sampling.
3. **Targeted Test Cases Covered:**
   * **Test Case 1 (Reset Assertion):** Verifies that asserting `test_rst` at startup successfully forces the counter to `4'b0000`.
   * **Test Case 2 (Normal Increment):** Asserts counting mode by releasing reset and tracking the accumulator over multiple consecutive clock cycles to verify step accuracy.
   * **Test Case 3 (Boundary Wrap-around):** Runs simulation through 15+ clock periods to explicitly monitor and prove steady underflow/overflow handling at the `4'b1111` boundary.
