# Validation Log: Practical 02 (NFA)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_NFA_13-11-2026.jff`  
**Execution Environment:** XML Structure & Subset Construction State Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transition Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Test Matrix

| Test ID | Input String ($w$) | Length | Active Subset State Trace | Final State Subset | Contains $q_2 \in F$? | Prompt Expectation | Automaton Result | Test Status |
|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `aa` | 2 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $\{q_0, q_1, q_2\}$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-02 | `aaa` | 3 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $\{q_0, q_1, q_2\}$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-03 | `baa` | 3 | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $\{q_0, q_1, q_2\}$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-04 | `bbaa` | 4 | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $\{q_0, q_1, q_2\}$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-05 | `abaa` | 4 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $\{q_0, q_1, q_2\}$ | Yes | Accept | **ACCEPT** | **PASS** |
| TC-06 | `""` ($\epsilon$) | 0 | $\{q_0\}$ | $\{q_0\}$ | No | Reject | **REJECT** | **PASS** |
| TC-07 | `a` | 1 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\}$ | $\{q_0, q_1\}$ | No | Reject | **REJECT** | **PASS** |
| TC-08 | `ab` | 2 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0\}$ | $\{q_0\}$ | No | Reject | **REJECT** | **PASS** |
| TC-09 | `aba` | 3 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\}$ | $\{q_0, q_1\}$ | No | Reject | **REJECT** | **PASS** |
| TC-10 | `aab` | 3 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\} \xrightarrow{b} \{q_0\}$ | $\{q_0\}$ | No | Reject | **REJECT** | **PASS** |
| TC-11 | `bb` | 2 | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{b} \{q_0\}$ | $\{q_0\}$ | No | Reject | **REJECT** | **PASS** |

---

## 2. Nondeterministic Structural Verification
- **Total States:** 3 ($q_0, q_1, q_2$).
- **Initial State:** Exactly 1 ($q_0$, ID 0).
- **Final State:** Exactly 1 ($q_2$, ID 2).
- **Branching Factor from $q_0$ on input 'a':**
  - $\delta(q_0, a) = \{ q_0, q_1 \}$ (verified: 2 XML transition tags present).
- **Transitions from $q_0$ on input 'b':**
  - $\delta(q_0, b) = \{ q_0 \}$ (verified: 1 loop transition present).
- **Transitions from $q_1$:**
  - $\delta(q_1, a) = \{ q_2 \}$, $\delta(q_1, b) = \emptyset$ (verified: exactly 1 transition tag present).
- **Transitions from $q_2$:**
  - $\delta(q_2, \Sigma) = \emptyset$ (verified: 0 outgoing transitions).
- **Total Transitions:** Exactly 4 transitions.

---

## 3. XML Syntactic Verification
- **Well-Formed XML Check:** PASSED (`[xml]` parser validated cleanly).
- **Tag Validation:** `<structure><type>fa</type><automaton>...</automaton></structure>` verified.
- **Canvas Coordinates:** Planar horizontal progression ($x = 120.0 \to 280.0 \to 440.0$, $y = 180.0$).

