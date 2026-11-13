# Practical 08: Moore Machine Design & Parity Sequence Detection

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_Moore_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/08_Moore_Machine/23_Darshan_Kankekar_Moore_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze a Moore Machine (Finite State Machine with state-based output) in JFLAP over $\Sigma = \{0, 1\}$ that computes and outputs the parity of the cumulative number of `1`s read so far, and to analyze JFLAP's $(n+1)$-length output behavior to resolve potential off-by-one ambiguities.

---

## 2. Problem Statement & Functional Requirements
- **Input Alphabet:** $\Sigma = \{0, 1\}$
- **Output Alphabet:** $\Delta = \{0, 1\}$
- **Specification:**
  - State $E$ has output `0` and is the initial state (representing an even number of `1`s).
  - State $O$ has output `1` (representing an odd number of `1`s).
  - On input `0`, each state loops to itself.
  - On input `1`, $E$ moves to $O$ and $O$ moves to $E$.

---

## 3. Formal Definition (6-Tuple)
The Moore machine is defined by the 6-tuple $M = (Q, \Sigma, \Delta, \delta, \lambda, q_0)$:

1. **$Q = \{ E, O \}$ (Set of States):**
   - $E$ (ID 0): Start state; parity of `1`s is even ($N_1 \equiv 0 \pmod 2$).
   - $O$ (ID 1): Parity of `1`s is odd ($N_1 \equiv 1 \pmod 2$).
2. **$\Sigma = \{ 0, 1 \}$ (Input Alphabet)**
3. **$\Delta = \{ 0, 1 \}$ (Output Alphabet)**
4. **$q_0 = E$ (Initial State)**
5. **$\delta : Q \times \Sigma \to Q$ (Transition Function):**
   - $\delta(E, 0) = E, \quad \delta(E, 1) = O$
   - $\delta(O, 0) = O, \quad \delta(O, 1) = E$
6. **$\lambda : Q \to \Delta$ (Output Function):**
   - $\lambda(E) = 0$
   - $\lambda(O) = 1$

---

## 4. State-Based Output (Moore) vs. Transition-Based Output (Mealy)

| Feature | Moore Machine (This Practical) | Mealy Machine (Practical 07) |
|:---|:---|:---|
| **Mathematical Mapping** | $\lambda : Q \to \Delta$ | $\lambda : Q \times \Sigma \to \Delta$ |
| **Output Location** | Embedded directly within **states** | Placed along **transitions** |
| **Response to Empty String $\epsilon$** | Emits $\lambda(q_0)$ at $t=0$ before reading input | Emits nothing ($\epsilon$) at $t=0$ |
| **Output String Length** | **$|y| = |x| + 1$** (Strictly $n + 1$ symbols for length $n$) | **$|y| = |x|$** (Strictly $n$ symbols for length $n$) |
| **Physical Analogy** | Synchronous registered output | Combinational transition output |

---

## 5. Initial State Output & Off-by-One Ambiguity Resolution

### A. How JFLAP Displays Moore Machine Outputs
When JFLAP runs a Moore machine, it strictly adheres to the classical automata definition:
1. At time $t = 0$, prior to consuming any input symbol, the machine occupies start state $E$.
2. The machine immediately outputs $\lambda(E) = 0$. This represents the parity of the prefix of length 0 (0 ones is an even number).
3. As each subsequent input symbol $\sigma_i$ is read, the machine enters a new state and outputs its associated symbol.
4. Consequently, for any input of length $n$, JFLAP displays an output string of length **$n + 1$**:
   $$y = y_0 \, y_1 \, y_2 \dots y_n \quad \text{where } y_0 = \lambda(q_0) \text{ and } y_i = \lambda(\hat{\delta}(q_0, x_1 \dots x_i))$$

### B. Resolving the Ambiguity for String `101`
The prompt specification notes:
`input 101 -> 1`
Let us examine the exact step-by-step state and output sequence for input string `101` (length $n = 3$):
- $t = 0$ (Initial): State $E \implies$ Emitted Symbol: **`0`** (index 0)
- $t = 1$ (Read `1`): State $O \implies$ Emitted Symbol: **`1`** (index 1)
- $t = 2$ (Read `0`): State $O \implies$ Emitted Symbol: **`1`** (index 2)
- $t = 3$ (Read `1`): State $E \implies$ Emitted Symbol: **`0`** (index 3)

The complete output string displayed by JFLAP is **`0110`** (length 4).

**Why the Discrepancy Occurs:**
1. **The 3rd Emitted Character vs. The Terminal Character:**  
   Because of the initial output `y_0 = 0`, the 3rd output character in `0110` is at index 2, which is **`1`** (corresponding to prefix `10`). A human inspector counting 3 characters for a 3-character input might look at the 3rd emitted character and note `1`.
2. **The True Final Parity:**  
   The actual terminal state after consuming the entire string `101` is state **$E$**, whose output is **`0`**. This is mathematically inevitable because the string `101` contains exactly two `1`s, and $2 \equiv 0 \pmod 2$ (even parity).

By documenting both the **Full JFLAP $(n+1)$ Output Stream** and the **Terminal State Output**, all off-by-one ambiguity is eliminated.

---

## 6. Complete State Transition & Output Table

| State ($q$) | State Output ($\lambda(q)$) | Input `0` ($\delta(q, 0)$) | Input `1` ($\delta(q, 1)$) | Semantic Meaning |
|:---:|:---:|:---:|:---:|:---|
| $\to E$ | **`0`** | $E$ | $O$ | Even number of `1`s read so far |
| $O$ | **`1`** | $O$ | $E$ | Odd number of `1`s read so far |

---

## 7. Test Suite and Execution Traces

| Test ID | Input ($x$) | Length ($n$) | Step-by-Step State & Output Trace | Full JFLAP Output ($n+1$ len) | Final State | Terminal Output $\lambda(q_{end})$ | Parity of 1s in $x$ | Status / Notes |
|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|:---|
| TC-01 | `1` | 1 | $E[0] \xrightarrow{1} O[1]$ | `01` | $O$ | **`1`** | 1 (Odd) | **PASS** |
| TC-02 | `11` | 2 | $E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0]$ | `010` | $E$ | **`0`** | 2 (Even) | **PASS** |
| TC-03 | `101` | 3 | $E[0] \xrightarrow{1} O[1] \xrightarrow{0} O[1] \xrightarrow{1} E[0]$ | `0110` | $E$ | **`0`** | 2 (Even) | **Documented:** Terminal output is `0` (even); 3rd symbol of stream is `1`. |
| TC-04 | `000` | 3 | $E[0] \xrightarrow{0} E[0] \xrightarrow{0} E[0] \xrightarrow{0} E[0]$ | `0000` | $E$ | **`0`** | 0 (Even) | **PASS** |
| TC-05 | `1111` | 4 | $E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0]$ | `01010` | $E$ | **`0`** | 4 (Even) | **PASS** |
| TC-06 | `10` | 2 | $E[0] \xrightarrow{1} O[1] \xrightarrow{0} O[1]$ | `011` | $O$ | **`1`** | 1 (Odd) | **PASS** |
| TC-07 | `111` | 3 | $E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0] \xrightarrow{1} O[1]$ | `0101` | $O$ | **`1`** | 3 (Odd) | **PASS** |

---

## 8. Inference & Conclusion
1. **State-Bound Output:** In a Moore machine, output values are invariants of the states themselves. Since parity is an intrinsic property of the cumulative count of `1`s, the 2-state Moore model provides a minimal and natural representation.
2. **$(n+1)$ Output Invariant:** The presence of an initial output symbol for $\epsilon$ is a defining theoretical property of Moore transducers.
3. **Equivalence with Mealy Model:** Any $n$-state Moore machine can be converted into an equivalent Mealy machine by pushing state outputs backward onto incoming transitions.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** $\to$ select:
   `JFLAP_Practicals/08_Moore_Machine/23_Darshan_Kankekar_Moore_13-11-2026.jff`
3. JFLAP automatically detects the type as **Moore Machine**.
4. Observe the canvas:
   - Notice that each state circle displays its name and output separated by a horizontal slash or line (e.g., `E / 0` and `O / 1`).
   - Transitions only display the single input symbol (`0` or `1`).
5. **Testing Step-by-Step:**
   - Click **Input** $\to$ **Step with State...**
   - Enter `101` and click **OK**.
   - Notice that the output bar immediately shows `0` before any step is executed.
   - Click **Step** three times to observe the output string grow to `0110`.
6. **Multiple Run Batch Testing:**
   - Click **Input** $\to$ **Multiple Run**.
   - Input all strings: `1`, `11`, `101`, `000`, `1111`, `10`, `111`.
   - Click **Run Inputs** and observe the full output column displaying $(n+1)$ characters for each string.
