# Validation Log: Practical 05 (Regular Expression to DFA)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**Files Under Test:**
- Regular Expression: `23_Darshan_Kankekar_RE_13-11-2026.jff`
- Converted DFA: `23_Darshan_Kankekar_DFA_from_RE_13-11-2026.jff`  
**Execution Environment:** XML Structure & DFA State-Transition Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transition Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Converted DFA Test Matrix

| Test ID | Input String ($w$) | Length | Step-by-Step State Transition Sequence | Automaton End State | Final State in $F$? | Prompt Expectation | Actual Result | Test Status |
|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `101` | 3 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3$ | Yes ($q_3 \in F$) | Accept | **ACCEPT** | **PASS** |
| TC-02 | `0101` | 4 | $q_0 \xrightarrow{0} q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3$ | Yes ($q_3 \in F$) | Accept | **ACCEPT** | **PASS** |
| TC-03 | `1101` | 4 | $q_0 \xrightarrow{1} q_1 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3$ | Yes ($q_3 \in F$) | Accept | **ACCEPT** | **PASS** |
| TC-04 | `00101` | 5 | $q_0 \xrightarrow{0} q_0 \xrightarrow{0} q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3$ | Yes ($q_3 \in F$) | Accept | **ACCEPT** | **PASS** |
| TC-05 | `""` ($\epsilon$) | 0 | $q_0$ (no transitions) | $q_0$ | No ($q_0 \notin F$) | Reject | **REJECT** | **PASS** |
| TC-06 | `10` | 2 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2$ | $q_2$ | No ($q_2 \notin F$) | Reject | **REJECT** | **PASS** |
| TC-07 | `100` | 3 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{0} q_0$ | $q_0$ | No ($q_0 \notin F$) | Reject | **REJECT** | **PASS** |
| TC-08 | `1010` | 4 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3 \xrightarrow{0} q_2$ | $q_2$ | No ($q_2 \notin F$) | Reject | **REJECT** | **PASS** |
| TC-09 | `111` | 3 | $q_0 \xrightarrow{1} q_1 \xrightarrow{1} q_1 \xrightarrow{1} q_1$ | $q_1$ | No ($q_1 \notin F$) | Reject | **REJECT** | **PASS** |

---

## 2. Regular Expression Syntax & Integrity
- **Native JFLAP RE Root Tag:** `<structure>`
- **Type Field:** `<type>re</type>`
- **Expression Content:** `<expression>(0+1)*101</expression>`
- **Union Operator:** `+` (validated for JFLAP 7.x standard)
- **Kleene Star Operator:** `*`
- **Prefix Component:** `(0+1)*` correctly permits any preceding binary sequence.
- **Suffix Component:** `101` correctly enforces termination with `101`.

---

## 3. Converted DFA Structural Verification
- **Automaton Root Tag:** `<structure>`
- **Type Field:** `<type>fa</type>`
- **State Count:** Exactly 4 states ($q_0, q_1, q_2, q_3$).
- **Initial State:** Exactly 1 ($q_0$, ID 0).
- **Final State:** Exactly 1 ($q_3$, ID 3).
- **Transition Count:** Exactly 8 transitions (complete deterministic function: 2 per state).
- **Completeness:** No missing or undefined transitions on $\{0, 1\}$.

