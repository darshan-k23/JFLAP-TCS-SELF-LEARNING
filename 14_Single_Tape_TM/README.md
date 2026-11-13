# Practical 14: Single-Tape Turing Machine - Binary Incrementer

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_Single_Tape_TM_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/14_Single_Tape_TM/23_Darshan_Kankekar_Single_Tape_TM_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze a deterministic single-tape Turing Machine (TM) in JFLAP that computes the binary increment operation ($n \mapsto n + 1$) on a non-empty binary integer, and verify the resulting tape contents across standard and carry-overflow test cases.

---

## 2. Problem Statement & Functional Requirements
- **Input Alphabet:** $\Sigma = \{0, 1\}$ (written without leading zeros, surrounded by infinitely many blanks $B$).
- **Tape Alphabet:** $\Gamma = \{0, 1, B\}$ (where $B$ denotes the blank cell).
- **Initial Configuration:** The read/write head initially scans the leftmost symbol (Most Significant Bit) of the binary number in start state $q_0$.
- **Objective:** Halt in accepting state $q_2$ with the tape containing the binary representation of the input incremented by 1 ($X + 1$), with all surrounding cells remaining blank.

---

## 3. Formal Definition (7-Tuple)
The single-tape Turing Machine is defined by the 7-tuple $M = (Q, \Sigma, \Gamma, \delta, q_0, B, F)$:

1. **$Q = \{ q_0, q_1, q_2 \}$ (Set of States):**
   - $q_0$ (`q_scan`): Start state; scans right to locate the Least Significant Bit (LSB).
   - $q_1$ (`q_carry`): Carry propagation state; ripples carry bit leftward.
   - $q_2$ (`q_halt`): Final accepting state; halts execution.
2. **$\Sigma = \{ 0, 1 \}$ (Input Alphabet)**
3. **$\Gamma = \{ 0, 1, B \}$ (Tape Alphabet)**
4. **$q_0 \in Q$ (Initial State)**
5. **$B \in \Gamma$ (Blank Symbol):**  
   *(In JFLAP XML, the blank symbol is natively serialized as an empty element `<read/>` / `<write/>`, which renders as the blank square symbol `□` on the GUI canvas).*
6. **$F = \{ q_2 \}$ (Set of Halting / Final States)**
7. **$\delta : Q \times \Gamma \to Q \times \Gamma \times \{L, R\}$ (Transition Function)**

---

## 4. Arithmetic Logic: Why Trailing 1s Become 0s and the First 0 Becomes 1

Binary addition is governed by modulo-2 arithmetic with ripple carry:
$$0 + 1 = 1 \quad (\text{carry } 0)$$
$$1 + 1 = 0 \quad (\text{carry } 1)$$

When incrementing an arbitrary binary integer by 1, the number can always be decomposed into:
$$\text{Input} = \alpha \, 0 \, \underbrace{1 1 \dots 1}_{k \text{ ones}} \quad (k \ge 0)$$
where $\alpha \in \{0, 1\}^*$ represents higher-order bits.

### The Carry Ripple Mechanism:
1. **Trailing 1s Flip to 0:**  
   Starting at the Least Significant Bit (LSB), adding the incoming carry of $1$ to a bit of value $1$ yields:
   $$1 + 1 = 10_2 \implies \text{write } 0, \text{ propagate carry } 1 \text{ to the left}$$
   As long as consecutive `1`s are encountered, this carry generation repeats. Each trailing $1$ is flipped to $0$, and the tape head shifts left ($L$).
2. **The First 0 Becomes 1:**  
   The carry continues rippling left until it encounters the first `0` (or a blank cell $B$ if the number was entirely `1`s). Adding the carry of $1$ to $0$ yields:
   $$0 + 1 = 1 \quad (\text{carry } 0)$$
   Because no further carry is generated, the `0` is replaced by `1`, and the addition terminates. The machine immediately halts.
3. **Overflow Beyond MSB ($2^k - 1 \to 2^k$):**  
   If the input consists entirely of `1`s (e.g., $111_2 = 7$), all bits are converted to `0`, and the carry head moves onto the blank cell $B$ immediately to the left of the original number. The machine overwrites $B$ with `1`, producing $1000_2 = 8$, correctly expanding the word length by one bit.

---

## 5. Transition Table

| Current State | Scanned Symbol ($\sigma$) | Written Symbol | Head Movement | Next State | Functional Purpose |
|:---:|:---:|:---:|:---:|:---:|:---|
| $q_0$ (`q_scan`) | `0` | `0` | **R** | $q_0$ | Scan past `0` toward the right |
| $q_0$ (`q_scan`) | `1` | `1` | **R** | $q_0$ | Scan past `1` toward the right |
| $q_0$ (`q_scan`) | $B$ (Blank) | $B$ | **L** | $q_1$ | Found right boundary; step left onto LSB and enter carry state |
| $q_1$ (`q_carry`) | `1` | `0` | **L** | $q_1$ | $1 + 1 = 0$ (carry 1); write `0`, shift left |
| $q_1$ (`q_carry`) | `0` | `1` | **R** | $q_2$ | $0 + 1 = 1$ (carry absorbed); write `1`, halt in $q_2$ |
| $q_1$ (`q_carry`) | $B$ (Blank) | `1` | **R** | $q_2$ | Overflow: write `1` to new MSB, halt in $q_2$ |

---

## 6. Test Suite and Execution Traces

### Instantaneous Descriptions (IDs)
An Instantaneous Description is formatted as $\alpha \, q_i \, \beta$, where $\alpha$ is tape contents to the left, $q_i$ is current state, and head scans the first symbol of $\beta$.

#### 1. Input: `0` $\to$ Expected: `1`
$$\begin{aligned}
&q_0 \, 0 \, B \vdash 0 \, q_0 \, B \vdash q_1 \, 0 \, B \vdash 1 \, q_2 \, B \implies \mathbf{1} \quad (3 \text{ steps})
\end{aligned}$$

#### 2. Input: `1011` $\to$ Expected: `1100`
$$\begin{aligned}
&q_0 \, 1011 \, B \vdash 1 \, q_0 \, 011 \, B \vdash 10 \, q_0 \, 11 \, B \vdash 101 \, q_0 \, 1 \, B \vdash 1011 \, q_0 \, B \\
&\vdash 101 \, q_1 \, 1 \, B \vdash 10 \, q_1 \, 10 \, B \vdash 1 \, q_1 \, 000 \, B \vdash 11 \, q_2 \, 00 \, B \implies \mathbf{1100} \quad (8 \text{ steps})
\end{aligned}$$

#### 3. Input: `111` $\to$ Expected: `1000` (Carry Overflow)
$$\begin{aligned}
&q_0 \, 111 \, B \vdash 1 \, q_0 \, 11 \, B \vdash 11 \, q_0 \, 1 \, B \vdash 111 \, q_0 \, B \\
&\vdash 11 \, q_1 \, 1 \, B \vdash 1 \, q_1 \, 10 \, B \vdash q_1 \, 100 \, B \vdash q_1 \, B \, 000 \, B \\
&\vdash 1 \, q_2 \, 000 \, B \implies \mathbf{1000} \quad (8 \text{ steps})
\end{aligned}$$

#### 4. Input: `1000` $\to$ Expected: `1001` (Single LSB Toggle)
$$\begin{aligned}
&q_0 \, 1000 \, B \vdash 1 \, q_0 \, 000 \, B \vdash 10 \, q_0 \, 00 \, B \vdash 100 \, q_0 \, 0 \, B \vdash 1000 \, q_0 \, B \\
&\vdash 100 \, q_1 \, 0 \, B \vdash 1001 \, q_2 \, B \implies \mathbf{1001} \quad (6 \text{ steps})
\end{aligned}$$

---

## 7. Results Verification Table

| Test Case | Input Tape | Initial Value ($X$) | Step Count | Final Tape Contents (Non-blank) | Final Value ($X+1$) | Match Status |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `0` | $0_{10}$ | 3 | `1` | $1_{10}$ | **PASS** |
| TC-02 | `1011` | $11_{10}$ | 8 | `1100` | $12_{10}$ | **PASS** |
| TC-03 | `111` | $7_{10}$ | 8 | `1000` | $8_{10}$ | **PASS** |
| TC-04 | `1000` | $8_{10}$ | 6 | `1001` | $9_{10}$ | **PASS** |

---

## 8. Inference & Conclusion
1. **Linear Time Efficiency:** The incrementer visits each cell at most twice (once forward in Phase 1, and at most once backward in Phase 2). The maximum time complexity is $O(n)$ where $n$ is the bit-length of the input.
2. **Minimal State Complexity:** The Turing Machine accomplishes arbitrary-precision binary addition using only **3 states** ($q_0, q_1, q_2$).
3. **Space Invariance & Growth:** For numbers with at least one `0`, the tape space consumed is strictly invariant ($n$ cells). When an overflow occurs ($2^k - 1$), the machine dynamically extends its tape domain by exactly 1 cell to the left.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** and select:
   `JFLAP_Practicals/14_Single_Tape_TM/23_Darshan_Kankekar_Single_Tape_TM_13-11-2026.jff`
3. JFLAP detects the machine type as **Turing Machine (1-Tape)**.
4. Observe the canvas:
   - Notice the 3 states `q_scan`, `q_carry`, `q_halt`.
   - Transitions display labels formatted as `read ; write , direction` (e.g., `0 ; 0 , R`, `\square ; \square , L`, `1 ; 0 , L`, `0 ; 1 , R`).
5. **Interactive Step-by-Step Tape Simulation:**
   - Click **Input** $\to$ **Step with State...**
   - Enter `1011` and click **OK**.
   - Step through the execution to watch the tape head advance to the blank, bounce left, turn trailing `1`s into `0`s, flip `0` to `1`, and halt.
6. **Batch Verification via Multiple Run:**
   - Click **Input** $\to$ **Multiple Run**.
   - Input `0`, `1011`, `111`, and `1000`.
   - Click **Run Inputs** and observe the **Output** column displaying `1`, `1100`, `1000`, and `1001`.

