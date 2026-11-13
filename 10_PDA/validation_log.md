# Validation Log: Practical 10 (PDA for $w\#w^R$)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_PDA_13-11-2026.jff`  
**Execution Environment:** XML Structure & PDA Configuration Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transition Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. PDA Execution Test Matrix

| Test ID | Input String | Length | Stack at `#` Boundary | Match Phase Outcome | Final Configuration | Input Consumed? | Expected Result | Automaton Result | Status |
|:---:|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `#` | 1 | `[Z]` | Epsilon move to $q_2$ | $(q_2, \epsilon, Z)$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-02 | `0#0` | 3 | `[0, Z]` | Matched `0`, popped to `Z` | $(q_2, \epsilon, Z)$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-03 | `1#1` | 3 | `[1, Z]` | Matched `1`, popped to `Z` | $(q_2, \epsilon, Z)$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-04 | `01#10` | 5 | `[1, 0, Z]` | Matched `1`, then `0` | $(q_2, \epsilon, Z)$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-05 | `101#101` | 7 | `[1, 0, 1, Z]` | Matched `1, 0, 1` | $(q_2, \epsilon, Z)$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-06 | `01#01` | 5 | `[1, 0, Z]` | Mismatch on `0` (top is `1`) | $(q_1, \text{`01'}, 10Z)$ | No | Reject | **REJECT** | **PASS** |
| TC-07 | `10#1` | 4 | `[0, 1, Z]` | Mismatch on `1` (top is `0`) | $(q_1, \text{`1'}, 01Z)$ | No | Reject | **REJECT** | **PASS** |
| TC-08 | `0#` | 2 | `[0, Z]` | Suffix missing; stack unemptied | $(q_1, \epsilon, 0Z)$ | Yes | Reject | **REJECT** | **PASS** |
| TC-09 | `#0` | 2 | `[Z]` | Extraneous `0` left unread | $(q_2, \text{`0'}, Z)$ | No | Reject | **REJECT** | **PASS** |
| TC-10 | `001#10` | 6 | `[1, 0, 0, Z]` | Matched `10`, `0` remains on stack | $(q_1, \epsilon, 0Z)$ | Yes | Reject | **REJECT** | **PASS** |
| TC-11 | `11#00` | 5 | `[1, 1, Z]` | Mismatch on `0` (top is `1`) | $(q_1, \text{`00'}, 11Z)$ | No | Reject | **REJECT** | **PASS** |

---

## 2. Transition Completeness & Stack Determinism
- **Push Phase Transitions ($q_0 \to q_0$):**
  - $\delta(q_0, 0, Z) = (q_0, 0Z)$ [VALIDATED]
  - $\delta(q_0, 1, Z) = (q_0, 1Z)$ [VALIDATED]
  - $\delta(q_0, 0, 0) = (q_0, 00)$ [VALIDATED]
  - $\delta(q_0, 1, 0) = (q_0, 10)$ [VALIDATED]
  - $\delta(q_0, 0, 1) = (q_0, 01)$ [VALIDATED]
  - $\delta(q_0, 1, 1) = (q_0, 11)$ [VALIDATED]
- **Midpoint Boundary Transitions ($q_0 \to q_1$ on `#`):**
  - $\delta(q_0, \#, Z) = (q_1, Z)$ [VALIDATED for $\epsilon\#\epsilon$]
  - $\delta(q_0, \#, 0) = (q_1, 0)$ [VALIDATED]
  - $\delta(q_0, \#, 1) = (q_1, 1)$ [VALIDATED]
- **Match Phase Transitions ($q_1 \to q_1$):**
  - $\delta(q_1, 0, 0) = (q_1, \epsilon)$ [VALIDATED: pops matching 0]
  - $\delta(q_1, 1, 1) = (q_1, \epsilon)$ [VALIDATED: pops matching 1]
- **Acceptance Transition ($q_1 \to q_2$):**
  - $\delta(q_1, \epsilon, Z) = (q_2, Z)$ [VALIDATED: requires bottom marker $Z$]

---

## 3. Structural & XML Integrity Verification
- **Root Tag:** `<structure>`
- **Type Field:** `<type>pda</type>` (ensures JFLAP opens in PDA mode)
- **State Count:** Exactly 3 states ($q_0, q_1, q_2$).
- **Initial State:** Exactly 1 ($q_0$, ID 0).
- **Final State:** Exactly 1 ($q_2$, ID 2).
- **Transition Count:** Exactly 12 transitions.
- **XML Well-Formedness:** Validated cleanly via .NET XML parser.
