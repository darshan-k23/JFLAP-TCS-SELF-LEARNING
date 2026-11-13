# Validation Log: Practical 01 (DFA)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_DFA_13-11-2026.jff`  
**Execution Environment:** XML Structure & Formal Automaton State-Transition Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transition Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Test Matrix

| Test ID | Input String ($w$) | Length | Count of 0s | Count of 1s | Prompt Expectation | Theoretical Language $L$ Expectation | Automaton End State | Actual Result | Status / Notes |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---|
| TC-01 | `1` | 1 | 0 (Even) | 1 (Odd) | Accept | Accept | $q_{EO}$ | **ACCEPT** | **PASS** |
| TC-02 | `111` | 3 | 0 (Even) | 3 (Odd) | Accept | Accept | $q_{EO}$ | **ACCEPT** | **PASS** |
| TC-03 | `001` | 3 | 2 (Even) | 1 (Odd) | Accept | Accept | $q_{EO}$ | **ACCEPT** | **PASS** |
| TC-04 | `10100` | 5 | 3 (Odd) | 2 (Even) | Accept | Reject | $q_{OE}$ | **REJECT** | **Discrepancy Documented** (See Note 1) |
| TC-05 | `""` ($\epsilon$) | 0 | 0 (Even) | 0 (Even) | Reject | Reject | $q_{EE}$ | **REJECT** | **PASS** |
| TC-06 | `0` | 1 | 1 (Odd) | 0 (Even) | Reject | Reject | $q_{OE}$ | **REJECT** | **PASS** |
| TC-07 | `11` | 2 | 0 (Even) | 2 (Even) | Reject | Reject | $q_{EE}$ | **REJECT** | **PASS** |
| TC-08 | `01` | 2 | 1 (Odd) | 1 (Odd) | Reject | Reject | $q_{OO}$ | **REJECT** | **PASS** |
| TC-09 | `1100` | 4 | 2 (Even) | 2 (Even) | Reject | Reject | $q_{EE}$ | **REJECT** | **PASS** |

---

## 2. Supplementary Tests for Language $L$ ($N_0$ Even, $N_1$ Odd)

| Test ID | Input String ($w$) | Count of 0s | Count of 1s | Language $L$ Membership | Automaton End State | Actual Result | Status |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| TC-10 | `010` | 2 (Even) | 1 (Odd) | Yes | $q_{EO}$ | **ACCEPT** | **PASS** |
| TC-11 | `100` | 2 (Even) | 1 (Odd) | Yes | $q_{EO}$ | **ACCEPT** | **PASS** |
| TC-12 | `10101` | 2 (Even) | 3 (Odd) | Yes | $q_{EO}$ | **ACCEPT** | **PASS** |
| TC-13 | `00001` | 4 (Even) | 1 (Odd) | Yes | $q_{EO}$ | **ACCEPT** | **PASS** |
| TC-14 | `10000` | 4 (Even) | 1 (Odd) | Yes | $q_{EO}$ | **ACCEPT** | **PASS** |

---

## 3. Discrepancy Note 1: Analysis of String `10100`
- **Prompt List:** Listed under *"Test strings that must be accepted"*.
- **Empirical Token Counts:**
  - `1`: 1st symbol
  - `0`: 2nd symbol
  - `1`: 3rd symbol
  - `0`: 4th symbol
  - `0`: 5th symbol
  - Total `0`s = $3$ ($\text{odd}$, since $3 \bmod 2 = 1$).
  - Total `1`s = $2$ ($\text{even}$, since $2 \bmod 2 = 0$).
- **Parity Tracking:**
  $$\delta^*(q_{EE}, 10100) = q_{OE}$$
- **State Classification:** $q_{OE}$ represents (Odd 0s, Even 1s) and is non-final ($q_{OE} \notin F$).
- **Resolution:** The DFA correctly identifies `10100` as having odd zeros and even ones, rejecting it. The prompt's placement of `10100` in the acceptance list is an apparent typo for an even-0/odd-1 string such as `10101` or `100`. The DFA operates strictly according to the mathematical definition of $L$.

---

## 4. XML Syntactic Verification
- **Well-Formed XML Check:** PASSED (`[xml]` parser validated cleanly).
- **Tag Validation:** `<structure><type>fa</type><automaton>...</automaton></structure>` verified.
- **State Count:** 4 states defined (`id="0"` to `id="3"`).
- **Initial State:** Exactly 1 (`qEE`, `id="0"`).
- **Final State:** Exactly 1 (`qEO`, `id="2"`).
- **Transitions:** Exactly 8 transitions (complete deterministic function: $4 \times 2 = 8$).

