# Practical 01: Deterministic Finite Automaton (DFA)

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_DFA_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/01_DFA/23_Darshan_Kankekar_DFA_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze a Deterministic Finite Automaton (DFA) in JFLAP that recognizes binary strings having an **even number of 0s** and an **odd number of 1s**.

---

## 2. Problem / Language Statement
Let the input alphabet be $\Sigma = \{0, 1\}$.  
The language $L$ is formally specified as:
$$L = \{ w \in \{0, 1\}^* \mid N_0(w) \equiv 0 \pmod 2 \;\land\; N_1(w) \equiv 1 \pmod 2 \}$$
where:
- $N_0(w)$ denotes the total count of symbol `0` in string $w$.
- $N_1(w)$ denotes the total count of symbol `1` in string $w$.

---

## 3. Formal Definition (5-Tuple)
The DFA is formally defined by the 5-tuple $M = (Q, \Sigma, \delta, q_0, F)$:
1. **$Q$ (Set of Finite States):**  
   $$Q = \{ q_{EE}, q_{OE}, q_{EO}, q_{OO} \}$$
   Represented in JFLAP as:
   - $q_0 \implies q_{EE}$
   - $q_1 \implies q_{OE}$
   - $q_2 \implies q_{EO}$
   - $q_3 \implies q_{OO}$

2. **$\Sigma$ (Input Alphabet):**  
   $$\Sigma = \{0, 1\}$$

3. **$q_0$ (Initial / Start State):**  
   $$q_0 = q_{EE}$$
   (At the empty string $\epsilon$, $N_0(\epsilon) = 0$ is even, and $N_1(\epsilon) = 0$ is even).

4. **$F$ (Set of Accepting / Final States):**  
   $$F = \{ q_{EO} \}$$
   (The unique state representing even zeros and odd ones).

5. **$\delta$ (Transition Function):**  
   $$\delta : Q \times \Sigma \to Q$$
   Defined rigorously in Section 5.

---

## 4. State & Parity Tracking Explanation
The state space represents the Cartesian product of parities $\mathbb{Z}_2 \times \mathbb{Z}_2$:
- Parity of 0s: $\{ \text{Even}, \text{Odd} \} \cong \{ 0, 1 \}$
- Parity of 1s: $\{ \text{Even}, \text{Odd} \} \cong \{ 0, 1 \}$

| State | JFLAP State Name | JFLAP State ID | Parity of 0s ($N_0 \bmod 2$) | Parity of 1s ($N_1 \bmod 2$) | Status |
|:---:|:---:|:---:|:---:|:---:|:---:|
| $q_{EE}$ | `qEE` | 0 | Even ($0$) | Even ($0$) | **Initial** (Non-final) |
| $q_{OE}$ | `qOE` | 1 | Odd ($1$) | Even ($0$) | Non-final |
| $q_{EO}$ | `qEO` | 2 | Even ($0$) | Odd ($1$) | **Final (Accepting)** |
| $q_{OO}$ | `qOO` | 3 | Odd ($1$) | Odd ($1$) | Non-final |

### Parity Toggling Principle
- Reading a **`0`** changes the parity of 0s while keeping the parity of 1s unchanged (horizontal transition in the layout).
- Reading a **`1`** changes the parity of 1s while keeping the parity of 0s unchanged (vertical transition in the layout).

---

## 5. Transition Table
Every state has exactly one outgoing transition for `0` and one outgoing transition for `1`, guaranteeing strict determinism.

| Current State ($q$) | Input `0` ($\delta(q, 0)$) | Input `1` ($\delta(q, 1)$) | Semantic Operation |
|:---|:---:|:---:|:---|
| $\to q_{EE}$ (Initial) | $q_{OE}$ | $q_{EO}$ | $0$ toggles 0-parity to Odd; $1$ toggles 1-parity to Odd |
| $q_{OE}$ | $q_{EE}$ | $q_{OO}$ | $0$ toggles 0-parity to Even; $1$ toggles 1-parity to Odd |
| $* q_{EO}$ (Final) | $q_{OO}$ | $q_{EE}$ | $0$ toggles 0-parity to Odd; $1$ toggles 1-parity to Even |
| $q_{OO}$ | $q_{EO}$ | $q_{OE}$ | $0$ toggles 0-parity to Even; $1$ toggles 1-parity to Even |

---

## 6. Test Suite and Execution Traces

### A. Trace Analysis for Specified Test Strings

| String $w$ | $N_0(w)$ | $N_1(w)$ | Step-by-Step State Transition Sequence | Final State | In $L$? | DFA Outcome |
|:---|:---:|:---:|:---|:---:|:---:|:---:|
| `1` | 0 (Even) | 1 (Odd) | $q_{EE} \xrightarrow{1} q_{EO}$ | $q_{EO} \in F$ | **Yes** | **Accept** |
| `111` | 0 (Even) | 3 (Odd) | $q_{EE} \xrightarrow{1} q_{EO} \xrightarrow{1} q_{EE} \xrightarrow{1} q_{EO}$ | $q_{EO} \in F$ | **Yes** | **Accept** |
| `001` | 2 (Even) | 1 (Odd) | $q_{EE} \xrightarrow{0} q_{OE} \xrightarrow{0} q_{EE} \xrightarrow{1} q_{EO}$ | $q_{EO} \in F$ | **Yes** | **Accept** |
| `10100` | 3 (Odd) | 2 (Even) | $q_{EE} \xrightarrow{1} q_{EO} \xrightarrow{0} q_{OO} \xrightarrow{1} q_{OE} \xrightarrow{0} q_{EE} \xrightarrow{0} q_{OE}$ | $q_{OE} \notin F$ | **No** | **Reject** |
| $\epsilon$ | 0 (Even) | 0 (Even) | $q_{EE}$ (no transitions) | $q_{EE} \notin F$ | **No** | **Reject** |
| `0` | 1 (Odd) | 0 (Even) | $q_{EE} \xrightarrow{0} q_{OE}$ | $q_{OE} \notin F$ | **No** | **Reject** |
| `11` | 0 (Even) | 2 (Even) | $q_{EE} \xrightarrow{1} q_{EO} \xrightarrow{1} q_{EE}$ | $q_{EE} \notin F$ | **No** | **Reject** |
| `01` | 1 (Odd) | 1 (Odd) | $q_{EE} \xrightarrow{0} q_{OE} \xrightarrow{1} q_{OO}$ | $q_{OO} \notin F$ | **No** | **Reject** |
| `1100` | 2 (Even) | 2 (Even) | $q_{EE} \xrightarrow{1} q_{EO} \xrightarrow{1} q_{EE} \xrightarrow{0} q_{OE} \xrightarrow{0} q_{EE}$ | $q_{EE} \notin F$ | **No** | **Reject** |

> [!NOTE]
> **Parity Analysis on `10100`:**  
> The string `10100` consists of three `0`s ($N_0 = 3$, odd) and two `1`s ($N_1 = 2$, even). Following the formal language definition $N_0 \equiv 0 \pmod 2 \land N_1 \equiv 1 \pmod 2$, string `10100` does not belong to $L$. Tracing through the 4-state parity machine reaches state $q_{OE}$ ($q_1$), which is non-final, resulting in **Reject**.  
> Additional valid strings satisfying even 0s and odd 1s (such as `010`, `100`, and `10101`) have been verified and confirmed accepted below.

### B. Supplementary Verification for Valid Language Members

| String $w$ | $N_0(w)$ | $N_1(w)$ | Transition Sequence | Final State | DFA Result |
|:---|:---:|:---:|:---|:---:|:---:|
| `010` | 2 (Even) | 1 (Odd) | $q_{EE} \xrightarrow{0} q_{OE} \xrightarrow{1} q_{OO} \xrightarrow{0} q_{EO}$ | $q_{EO} \in F$ | **Accept** |
| `100` | 2 (Even) | 1 (Odd) | $q_{EE} \xrightarrow{1} q_{EO} \xrightarrow{0} q_{OO} \xrightarrow{0} q_{EO}$ | $q_{EO} \in F$ | **Accept** |
| `10101` | 2 (Even) | 3 (Odd) | $q_{EE} \xrightarrow{1} q_{EO} \xrightarrow{0} q_{OO} \xrightarrow{1} q_{OE} \xrightarrow{0} q_{EE} \xrightarrow{1} q_{EO}$ | $q_{EO} \in F$ | **Accept** |

---

## 7. Inference & Conclusion
1. **Minimality & Sufficiency:** Exactly 4 states are both necessary and sufficient to track the parity of two independent binary symbols, as $|\mathbb{Z}_2 \times \mathbb{Z}_2| = 2 \times 2 = 4$.
2. **Determinism:** Every state has exactly one defined transition for every symbol in $\Sigma$, preventing dead ends or ambiguities.
3. **Correctness:** The automaton reaches accepting state $q_{EO}$ if and only if the cumulative number of `0`s read is even and `1`s read is odd.

---

## 8. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** and select:
   `JFLAP_Practicals/01_DFA/23_Darshan_Kankekar_DFA_13-11-2026.jff`
3. The 4-state planar diagram will appear on the canvas with initial state `qEE` and final state `qEO`.
4. To test single strings step-by-step:
   - Click **Input** $\to$ **Step with State...**
   - Enter `001` or `111` and click **OK**.
   - Use the **Step** button to observe state transitions.
5. To run the full batch test suite:
   - Click **Input** $\to$ **Multiple Run**.
   - Enter all test strings (`1`, `111`, `001`, `10100`, ``, `0`, `11`, `01`, `1100`, `010`, `100`, `10101`).
   - Click **Run Inputs** and observe the green **Accept** and red **Reject** indicators.

