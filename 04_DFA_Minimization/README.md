# Practical 04: DFA Minimization

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**Original DFA File:** [`23_Darshan_Kankekar_DFA_Original_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/04_DFA_Minimization/23_Darshan_Kankekar_DFA_Original_13-11-2026.jff)  
**Minimized DFA File:** [`23_Darshan_Kankekar_DFA_Minimized_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/04_DFA_Minimization/23_Darshan_Kankekar_DFA_Minimized_13-11-2026.jff)

---

## 1. Aim
To design a deliberately non-minimal 4-state Deterministic Finite Automaton (DFA) in JFLAP that recognizes binary strings ending in `1`, minimize it to the canonical 2-state quotient automaton using partition refinement (table-filling / Myhill-Nerode equivalence), and verify that the language recognized remains strictly invariant.

---

## 2. Problem / Language Statement
Let the alphabet be $\Sigma = \{0, 1\}$.  
The language $L$ is defined as:
$$L = \{ w \in \{0, 1\}^* \mid w \text{ ends with symbol '1'} \}$$
Equivalent Regular Expression:
$$R = (0 + 1)^* 1$$

---

## 3. Formal Definitions

### A. Non-Minimal Original DFA ($M_{orig}$)
Defined by the 5-tuple $M_{orig} = (Q, \Sigma, \delta, q_0, F)$:
- $Q = \{ q_0, q_1, q_2, q_3 \}$
- $\Sigma = \{ 0, 1 \}$
- Start State: $q_0$
- Final State: $F = \{ q_1 \}$
- $\delta : Q \times \Sigma \to Q$

### B. Minimized Quotient DFA ($M_{min}$)
Defined by the 5-tuple $M_{min} = (Q', \Sigma, \delta', [q_0], F')$:
- $Q' = \{ [q_{0,2,3}], [q_1] \}$
  - $[q_{0,2,3}] = \{ q_0, q_2, q_3 \}$ (Represents all strings not ending with `1`)
  - $[q_1] = \{ q_1 \}$ (Represents all strings ending with `1`)
- $\Sigma = \{ 0, 1 \}$
- Start State: $[q_{0,2,3}]$
- Final State: $F' = \{ [q_1] \}$
- $\delta' : Q' \times \Sigma \to Q'$

---

## 4. State Equivalence & Minimization Derivation

Two states $p$ and $q$ are equivalent ($p \equiv q$) if for all strings $w \in \Sigma^*$, $\hat{\delta}(p, w) \in F \iff \hat{\delta}(q, w) \in F$.

### Step 1: 0-Equivalence Partition ($P_0$)
Separate the set of states into accepting and non-accepting blocks:
$$P_0 = \{ G_1, G_2 \}$$
- $G_1 = \{ q_0, q_2, q_3 \}$ (Non-final states)
- $G_2 = \{ q_1 \}$ (Final states)

### Step 2: 1-Equivalence Partition ($P_1$)
Test whether transitions from states in $G_1$ fall into distinct blocks of $P_0$:
- **For $q_0$:**
  - $\delta(q_0, 0) = q_2 \in G_1$
  - $\delta(q_0, 1) = q_1 \in G_2$
- **For $q_2$:**
  - $\delta(q_2, 0) = q_3 \in G_1$
  - $\delta(q_2, 1) = q_1 \in G_2$
- **For $q_3$:**
  - $\delta(q_3, 0) = q_3 \in G_1$
  - $\delta(q_3, 1) = q_1 \in G_2$

For every state $q \in G_1$:
$$\delta(q, 0) \in G_1 \quad \text{and} \quad \delta(q, 1) \in G_2$$
Because all three states exhibit identical block-transitions under every input symbol in $\Sigma$, no state in $G_1$ can be distinguished from any other state in $G_1$.

Thus, the partition does not split:
$$P_1 = P_0 = \{ \{ q_0, q_2, q_3 \}, \{ q_1 \} \}$$
Because $P_1 = P_0$, the algorithm terminates.

### Equivalence Classes
1. $[q_{0,2,3}] = \{ q_0, q_2, q_3 \}$
2. $[q_1] = \{ q_1 \}$

---

## 5. Transition Tables Comparison

### Original Non-Minimal DFA ($M_{orig}$)
| State ($q$) | Input `0` ($\delta(q, 0)$) | Input `1` ($\delta(q, 1)$) | Status |
|:---:|:---:|:---:|:---:|
| $\to q_0$ | $q_2$ | $q_1$ | Initial, Non-final |
| $* q_1$ | $q_2$ | $q_1$ | **Final** |
| $q_2$ | $q_3$ | $q_1$ | Non-final |
| $q_3$ | $q_3$ | $q_1$ | Non-final |

### Minimized Quotient DFA ($M_{min}$)
| Equivalence State | JFLAP Name | Input `0` ($\delta'$) | Input `1` ($\delta'$) | Status |
|:---:|:---:|:---:|:---:|:---:|
| $\to [q_0, q_2, q_3]$ | `q0_2_3` | $[q_0, q_2, q_3]$ | $[q_1]$ | Initial, Non-final |
| $* [q_1]$ | `q1` | $[q_0, q_2, q_3]$ | $[q_1]$ | **Final** |

---

## 6. Invariance of the Language Explained
The original 4-state automaton kept artificial distinction between:
- Having read no symbols / $\epsilon$ ($q_0$)
- Having read a single `0` ($q_2$)
- Having read two or more consecutive `0`s ($q_3$)

However, the language definition $L = (0+1)^*1$ depends **only** on the very last symbol of the string:
- If the last symbol is `1`, the string must be accepted. In both machines, reading `1` from any state transitions unconditionally to the accepting state ($q_1$ in $M_{orig}$; $[q_1]$ in $M_{min}$).
- If the last symbol is `0` (or if the string is empty $\epsilon$), the string must be rejected. In both machines, reading `0` from any state transitions unconditionally to a non-final state ($q_2$ or $q_3$ in $M_{orig}$; $[q_{0,2,3}]$ in $M_{min}$).

Therefore, the distinction between $q_0, q_2,$ and $q_3$ represents redundant internal state bookkeeping that has zero impact on language membership. Merging them into a single equivalence state $[q_{0,2,3}]$ yields a structurally minimal, strictly isomorphic representation of the exact same language.

---

## 7. Comparative Test Suite & Execution Traces

Both machines were tested on the identical set of input vectors:

| Test String $w$ | Original DFA State Trace | Original Outcome | Minimized DFA State Trace | Minimized Outcome | Agreement |
|:---|:---|:---:|:---|:---:|:---:|
| `1` | $q_0 \xrightarrow{1} q_1$ | **Accept** | $[q_{0,2,3}] \xrightarrow{1} [q_1]$ | **Accept** | **MATCH** |
| `101` | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_1$ | **Accept** | $[q_{0,2,3}] \xrightarrow{1} [q_1] \xrightarrow{0} [q_{0,2,3}] \xrightarrow{1} [q_1]$ | **Accept** | **MATCH** |
| `0001` | $q_0 \xrightarrow{0} q_2 \xrightarrow{0} q_3 \xrightarrow{0} q_3 \xrightarrow{1} q_1$ | **Accept** | $[q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}] \xrightarrow{1} [q_1]$ | **Accept** | **MATCH** |
| `111` | $q_0 \xrightarrow{1} q_1 \xrightarrow{1} q_1 \xrightarrow{1} q_1$ | **Accept** | $[q_{0,2,3}] \xrightarrow{1} [q_1] \xrightarrow{1} [q_1] \xrightarrow{1} [q_1]$ | **Accept** | **MATCH** |
| $\epsilon$ | $q_0$ | **Reject** | $[q_{0,2,3}]$ | **Reject** | **MATCH** |
| `0` | $q_0 \xrightarrow{0} q_2$ | **Reject** | $[q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}]$ | **Reject** | **MATCH** |
| `10` | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2$ | **Reject** | $[q_{0,2,3}] \xrightarrow{1} [q_1] \xrightarrow{0} [q_{0,2,3}]$ | **Reject** | **MATCH** |
| `1100` | $q_0 \xrightarrow{1} q_1 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{0} q_3$ | **Reject** | $[q_{0,2,3}] \xrightarrow{1} [q_1] \xrightarrow{1} [q_1] \xrightarrow{0} [q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}]$ | **Reject** | **MATCH** |
| `000` | $q_0 \xrightarrow{0} q_2 \xrightarrow{0} q_3 \xrightarrow{0} q_3$ | **Reject** | $[q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}] \xrightarrow{0} [q_{0,2,3}]$ | **Reject** | **MATCH** |

---

## 8. Inference & Conclusion
1. **Uniqueness of Minimal DFA:** By the Myhill-Nerode Theorem, the minimal DFA for any regular language is unique up to state renaming (isomorphism). For $L = (0+1)^*1$, that unique minimum has exactly 2 states.
2. **Computational Reduction:** Minimizing the machine reduced the state count by 50% (from 4 states to 2 states) and transition count from 8 to 4, eliminating memory overhead without sacrificing language fidelity.
3. **Equivalence Preservation:** Partition refinement ensures that merged states are mutually indistinguishable under all future input sequences.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. **Open the Original DFA:**
   - Click **File** $\to$ **Open** $\to$ `JFLAP_Practicals/04_DFA_Minimization/23_Darshan_Kankekar_DFA_Original_13-11-2026.jff`.
3. **Execute Interactive Minimization in JFLAP:**
   - In the menu bar, click **Convert** $\to$ **Minimize DFA**.
   - JFLAP opens the minimization window displaying the state partition tree.
   - Click **Set Terminal, Nonterminal** to create the initial partition $\{q_1\}$ and $\{q_0, q_2, q_3\}$.
   - Click **Auto Partition** to observe JFLAP verify that no further split occurs.
   - Click **Complete** to generate the minimized transition graph.
4. **Test Both Automata via Multiple Run:**
   - In both the original and minimized windows, navigate to **Input** $\to$ **Multiple Run**.
   - Input the test strings (`1`, `101`, `0001`, `111`, ``, `0`, `10`, `1100`, `000`).
   - Run inputs on both to visually verify identical outputs.

