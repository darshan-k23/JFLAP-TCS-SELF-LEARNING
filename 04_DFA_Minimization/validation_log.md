# Validation Log: Practical 04 (DFA Minimization)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**Files Under Test:**
- Original DFA: `23_Darshan_Kankekar_DFA_Original_13-11-2026.jff`
- Minimized DFA: `23_Darshan_Kankekar_DFA_Minimized_13-11-2026.jff`  
**Execution Environment:** XML Structure & Dual-Machine Automata Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transition Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Dual-Machine Equivalence Test Matrix

| Test ID | Input String ($w$) | Length | Original DFA Final State | Original Decision | Minimized DFA Final State | Minimized Decision | Expected Result | Machines Agree? | Test Status |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `1` | 1 | $q_1$ | **ACCEPT** | $[q_1]$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-02 | `101` | 3 | $q_1$ | **ACCEPT** | $[q_1]$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-03 | `0001` | 4 | $q_1$ | **ACCEPT** | $[q_1]$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-04 | `111` | 3 | $q_1$ | **ACCEPT** | $[q_1]$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-05 | `""` ($\epsilon$) | 0 | $q_0$ | **REJECT** | $[q_{0,2,3}]$ | **REJECT** | Reject | **YES** | **PASS** |
| TC-06 | `0` | 1 | $q_2$ | **REJECT** | $[q_{0,2,3}]$ | **REJECT** | Reject | **YES** | **PASS** |
| TC-07 | `10` | 2 | $q_2$ | **REJECT** | $[q_{0,2,3}]$ | **REJECT** | Reject | **YES** | **PASS** |
| TC-08 | `1100` | 4 | $q_3$ | **REJECT** | $[q_{0,2,3}]$ | **REJECT** | Reject | **YES** | **PASS** |
| TC-09 | `000` | 3 | $q_3$ | **REJECT** | $[q_{0,2,3}]$ | **REJECT** | Reject | **YES** | **PASS** |

---

## 2. Partition Refinement Verification
- **Initial Partition ($P_0$):**
  - Non-final block $G_1 = \{ q_0, q_2, q_3 \}$
  - Final block $G_2 = \{ q_1 \}$
- **1-Equivalence Behavior under $\Sigma = \{0, 1\}$:**
  - $\delta(q_0, 0) = q_2 \in G_1, \quad \delta(q_0, 1) = q_1 \in G_2$
  - $\delta(q_2, 0) = q_3 \in G_1, \quad \delta(q_2, 1) = q_1 \in G_2$
  - $\delta(q_3, 0) = q_3 \in G_1, \quad \delta(q_3, 1) = q_1 \in G_2$
- **Convergence:**
  - Because all elements of $G_1$ transition to $G_1$ on input `0` and to $G_2$ on input `1`, no distinguishing sequence exists between $q_0, q_2,$ and $q_3$.
  - Therefore, $P_1 = P_0$. Algorithm terminates with exactly 2 equivalence classes.

---

## 3. Structural & XML Integrity Verification
- **Original DFA XML:**
  - Type: `<type>fa</type>`
  - States: 4 ($q_0, q_1, q_2, q_3$)
  - Initial State: $q_0$ (ID 0)
  - Final State: $q_1$ (ID 1)
  - Transitions: Exactly 8 (fully deterministic, 2 per state)
- **Minimized DFA XML:**
  - Type: `<type>fa</type>`
  - States: 2 ($[q_{0,2,3}], [q_1]$)
  - Initial State: $[q_{0,2,3}]$ (ID 0)
  - Final State: $[q_1]$ (ID 1)
  - Transitions: Exactly 4 (fully deterministic, 2 per state)

