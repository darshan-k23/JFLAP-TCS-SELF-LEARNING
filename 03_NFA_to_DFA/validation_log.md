# Validation Log: Practical 03 (NFA to DFA Conversion)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**Files Under Test:**
- Source NFA: `23_Darshan_Kankekar_NFA_Source_13-11-2026.jff`
- Converted DFA: `23_Darshan_Kankekar_DFA_Converted_13-11-2026.jff`  
**Execution Environment:** XML Structure & Dual-Machine Automata Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transition Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Dual-Machine Equivalence Test Matrix

| Test ID | Input String ($w$) | Length | NFA Final Active Subset | NFA Decision | DFA Final State | DFA Decision | Expected Result | Machines Agree? | Test Status |
|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `ab` | 2 | $\{q_0, q_2\}$ | **ACCEPT** | $C$ ($\{q_0, q_2\}$) | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-02 | `aab` | 3 | $\{q_0, q_2\}$ | **ACCEPT** | $C$ ($\{q_0, q_2\}$) | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-03 | `bab` | 3 | $\{q_0, q_2\}$ | **ACCEPT** | $C$ ($\{q_0, q_2\}$) | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-04 | `abab` | 4 | $\{q_0, q_2\}$ | **ACCEPT** | $C$ ($\{q_0, q_2\}$) | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-05 | `bbaab` | 5 | $\{q_0, q_2\}$ | **ACCEPT** | $C$ ($\{q_0, q_2\}$) | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-06 | `""` ($\epsilon$) | 0 | $\{q_0\}$ | **REJECT** | $A$ ($\{q_0\}$) | **REJECT** | Reject | **YES** | **PASS** |
| TC-07 | `a` | 1 | $\{q_0, q_1\}$ | **REJECT** | $B$ ($\{q_0, q_1\}$) | **REJECT** | Reject | **YES** | **PASS** |
| TC-08 | `b` | 1 | $\{q_0\}$ | **REJECT** | $A$ ($\{q_0\}$) | **REJECT** | Reject | **YES** | **PASS** |
| TC-09 | `ba` | 2 | $\{q_0, q_1\}$ | **REJECT** | $B$ ($\{q_0, q_1\}$) | **REJECT** | Reject | **YES** | **PASS** |
| TC-10 | `aaa` | 3 | $\{q_0, q_1\}$ | **REJECT** | $B$ ($\{q_0, q_1\}$) | **REJECT** | Reject | **YES** | **PASS** |
| TC-11 | `bbb` | 3 | $\{q_0\}$ | **REJECT** | $A$ ($\{q_0\}$) | **REJECT** | Reject | **YES** | **PASS** |

---

## 2. Subset Construction Transition Verification

### Verified Transitions:
- **State A ($\{q_0\}$):**
  - on `a` $\to$ State B ($\{q_0, q_1\}$) [VALIDATED]
  - on `b` $\to$ State A ($\{q_0\}$) [VALIDATED]
- **State B ($\{q_0, q_1\}$):**
  - on `a` $\to$ State B ($\{q_0, q_1\}$) [VALIDATED]
  - on `b` $\to$ State C ($\{q_0, q_2\}$) [VALIDATED]
- **State C ($\{q_0, q_2\}$):**
  - on `a` $\to$ State D ($\{q_0, q_1, q_2\}$) [VALIDATED]
  - on `b` $\to$ State C ($\{q_0, q_2\}$) [VALIDATED]
- **State D ($\{q_0, q_1, q_2\}$):**
  - on `a` $\to$ State D ($\{q_0, q_1, q_2\}$) [VALIDATED]
  - on `b` $\to$ State C ($\{q_0, q_2\}$) [VALIDATED]

---

## 3. Structural & XML Integrity Verification
- **Source NFA XML:**
  - Structure: `<type>fa</type>`
  - States: 3 ($q_0, q_1, q_2$)
  - Initial State: $q_0$
  - Final State: $q_2$
  - Transitions: 6 (including nondeterministic fork $\delta(q_0, a) = \{q_0, q_1\}$)
- **Converted DFA XML:**
  - Structure: `<type>fa</type>`
  - States: 4 ($A, B, C, D$)
  - Initial State: $A$
  - Final States: $C, D$
  - Transitions: 8 (strict determinism: exactly 2 per state)

