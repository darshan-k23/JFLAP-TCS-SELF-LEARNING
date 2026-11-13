# Practical 07: Mealy Machine Design & Substring Sequence Detection

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_Mealy_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/07_Mealy_Machine/23_Darshan_Kankekar_Mealy_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze a Mealy Machine (Finite State Machine with transition-based output) in JFLAP that scans a binary input sequence over $\Sigma = \{0, 1\}$ and emits output symbol `1` synchronously on the transition that completes an occurrence of the substring `101`, while emitting `0` on all other transitions.

---

## 2. Problem Statement & Functional Requirements
- **Input Alphabet:** $\Sigma = \{0, 1\}$
- **Output Alphabet:** $\Delta = \{0, 1\}$
- **Detection Target:** The contiguous substring `101`.
- **Output Behavior:** Synchronous emission of `1` exactly when the last symbol of `101` is consumed, and `0` at all other time steps.
- **Overlap Requirement:** Overlapping occurrences must be detected (e.g., in `10101`, two `1`s must be emitted: `00101`).

---

## 3. Formal Definition (6-Tuple)
The Mealy machine is defined by the 6-tuple $M = (Q, \Sigma, \Delta, \delta, \lambda, q_0)$:

1. **$Q = \{ A, B, C \}$ (Set of States):**
   - **$A$ (ID 0):** Start state; no relevant partial match of `101` has been seen.
   - **$B$ (ID 1):** The latest relevant suffix of input consumed so far is `1` (progress length 1).
   - **$C$ (ID 2):** The latest relevant suffix of input consumed so far is `10` (progress length 2).
2. **$\Sigma = \{ 0, 1 \}$ (Input Alphabet)**
3. **$\Delta = \{ 0, 1 \}$ (Output Alphabet)**
4. **$q_0 = A$ (Initial State)**
5. **$\delta : Q \times \Sigma \to Q$ (State Transition Function):**
   - $\delta(A, 0) = A, \quad \delta(A, 1) = B$
   - $\delta(B, 0) = C, \quad \delta(B, 1) = B$
   - $\delta(C, 0) = A, \quad \delta(C, 1) = B$
6. **$\lambda : Q \times \Sigma \to \Delta$ (Output Function):**
   - $\lambda(A, 0) = 0, \quad \lambda(A, 1) = 0$
   - $\lambda(B, 0) = 0, \quad \lambda(B, 1) = 0$
   - $\lambda(C, 0) = 0, \quad \lambda(C, 1) = 1$

---

## 4. Architectural Comparison: Mealy vs. Moore Machines

| Dimension | Mealy Machine | Moore Machine |
|:---|:---|:---|
| **Output Association** | Associated with **transitions** ($\delta(q, a)$ produces $\lambda(q, a)$) | Associated with **states** ($q$ produces $\lambda(q)$) |
| **Output Dependency** | Depends on both **Current State** AND **Current Input Symbol** | Depends solely on **Current State** |
| **Output String Length** | Equal to input length: $|y| = |x| = n$ | Greater than input length by 1: $|y| = |x| + 1 = n + 1$ |
| **Initial Output** | No output before reading the first input symbol | Emits output of initial state $q_0$ at time $t=0$ ($\epsilon$-input) |
| **State Economy** | Often requires fewer states for sequence detection (3 states for `101`) | Requires an extra state to remember the completed match (4 states for `101`) |
| **Hardware Timing** | Reacts within the current clock cycle (asynchronous input path) | Synchronized to clock edge (registered output) |

---

## 5. Overlap Handling Mechanism

A fundamental challenge in serial pattern detection is handling **overlapping patterns**, such as `10101`:
1. When the machine occupies state $C$, the history of symbols read is $\dots 10$.
2. When the next input symbol is `1`, the sequence becomes $\dots 101$.
3. The machine completes the target pattern and emits output **`1`**.
4. **Where must the machine transition?**
   - If the machine transitioned to start state $A$, it would discard the fact that the trailing `1` was just read.
   - However, the trailing `1` of the first `101` can simultaneously serve as the leading `1` of a potential subsequent `101`!
   - Because state $B$ explicitly represents the condition *"latest relevant suffix is 1"*, routing the transition to state $B$ ($C \xrightarrow{1/1} B$) preserves this history.
   - Consequently, if the subsequent symbols are `0` and `1`, the machine traverses $B \xrightarrow{0/0} C \xrightarrow{1/1} B$, successfully detecting the second overlapping occurrence without dropping a single symbol.

---

## 6. Complete Transition & Output Table

Every state has exactly one defined transition and one output for each symbol in $\Sigma$:

| Current State ($q$) | Input Symbol ($\sigma$) | Next State ($\delta(q, \sigma)$) | Output Symbol ($\lambda(q, \sigma)$) | Transition Label | Functional Meaning |
|:---:|:---:|:---:|:---:|:---:|:---|
| $\to A$ | `0` | $A$ | `0` | `0 / 0` | No match; remain in reset state |
| $\to A$ | `1` | $B$ | `0` | `1 / 0` | Begin match; saw first `1` |
| $B$ | `0` | $C$ | `0` | `0 / 0` | Advance match; saw `10` |
| $B$ | `1` | $B$ | `0` | `1 / 0` | Consecutive `1`s; retain latest `1` |
| $C$ | `0` | $A$ | `0` | `0 / 0` | Sequence `100` breaks match; reset to $A$ |
| $C$ | `1` | $B$ | `1` | `1 / 1` | **Pattern `101` matched!** Emit `1`, retain trailing `1` in $B$ |

---

## 7. Test Suite and Execution Traces

Each input vector was simulated through the Mealy machine. Output length $|y|$ strictly equals input length $|x|$ in all cases:

| Test ID | Input ($x$) | Length | Step-by-Step Transition & Output Sequence | Generated Output ($y$) | Expected Output | Length Equal? | Result Status |
|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|
| TC-01 | `101` | 3 | $A \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{1/1} B$ | `001` | `001` | Yes ($3=3$) | **PASS** |
| TC-02 | `1101` | 4 | $A \xrightarrow{1/0} B \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{1/1} B$ | `0001` | `0001` | Yes ($4=4$) | **PASS** |
| TC-03 | `10101` | 5 | $A \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{1/1} B \xrightarrow{0/0} C \xrightarrow{1/1} B$ | `00101` | `00101` | Yes ($5=5$) | **PASS (Overlap verified)** |
| TC-04 | `111` | 3 | $A \xrightarrow{1/0} B \xrightarrow{1/0} B \xrightarrow{1/0} B$ | `000` | `000` | Yes ($3=3$) | **PASS** |
| TC-05 | `1000` | 4 | $A \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{0/0} A \xrightarrow{0/0} A$ | `0000` | `0000` | Yes ($4=4$) | **PASS** |

---

## 8. Inference & Conclusion
1. **Transition-Synchronous Transduction:** The Mealy model emits outputs directly on state transitions, producing exactly one output bit per consumed input bit without latency or initial padding.
2. **State Minimality:** Tracking the progress of substring `101` with overlapping capability requires only 3 states in the Mealy model ($A, B, C$), whereas a Moore equivalent would require 4 states to decouple the accepting output from state transitions.
3. **Correctness on Overlaps:** Transition $C \xrightarrow{1/1} B$ satisfies the Knuth-Morris-Pratt failure function property, allowing continuous and robust multi-pattern detection.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** and select:
   `JFLAP_Practicals/07_Mealy_Machine/23_Darshan_Kankekar_Mealy_13-11-2026.jff`
3. JFLAP automatically recognizes the machine type as **Mealy Machine**.
4. Observe the canvas:
   - Three planar states $A, B, C$ are rendered.
   - Transitions display dual labels formatted as `input ; output` (e.g., `1 ; 1` on transition $C \to B$).
5. **Testing Single Inputs Step-by-Step:**
   - Click **Input** $\to$ **Step with State...**
   - Enter `10101` and click **OK**.
   - Step through the execution and observe the output string being constructed in real-time at the bottom pane: `00101`.
6. **Testing Multiple Inputs:**
   - Click **Input** $\to$ **Multiple Run**.
   - Input the test suite (`101`, `1101`, `10101`, `111`, `1000`).
   - Click **Run Inputs** to inspect the **Output** column and verify exact correspondence.
