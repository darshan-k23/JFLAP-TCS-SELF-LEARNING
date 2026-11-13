# Validation Log: Practical 08 (Moore Machine)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_Moore_13-11-2026.jff`  
**Execution Environment:** XML Structure & Moore Transducer Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Moore Transducer Output Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Moore Transducer Output Test Matrix

| Test ID | Input ($x$) | Length ($n$) | Count of 1s | State & Output Sequence | Full JFLAP Output ($n+1$ len) | Terminal State | Terminal Output $\lambda(q_{end})$ | Prompt Expectation | Test Status / Analysis |
|:---:|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|:---|
| TC-01 | `1` | 1 | 1 (Odd) | $E[0] \xrightarrow{1} O[1]$ | `01` | $O$ | **`1`** | `1` | **PASS** |
| TC-02 | `11` | 2 | 2 (Even) | $E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0]$ | `010` | $E$ | **`0`** | `0` | **PASS** |
| TC-03 | `101` | 3 | 2 (Even) | $E[0] \xrightarrow{1} O[1] \xrightarrow{0} O[1] \xrightarrow{1} E[0]$ | `0110` | $E$ | **`0`** | `1` | **Documented Discrepancy:** The prompt listed `1`. In JFLAP's $(n+1)$ output stream `0110`, the 3rd emitted character (index 2) is `1`. However, the actual final state after reading all 3 characters is $E$ with output `0` (since $N_1 = 2$ is even). |
| TC-04 | `000` | 3 | 0 (Even) | $E[0] \xrightarrow{0} E[0] \xrightarrow{0} E[0] \xrightarrow{0} E[0]$ | `0000` | $E$ | **`0`** | `0` | **PASS** |
| TC-05 | `1111` | 4 | 4 (Even) | $E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0]$ | `01010` | $E$ | **`0`** | `0` | **PASS** |
| TC-06 | `10` | 2 | 1 (Odd) | $E[0] \xrightarrow{1} O[1] \xrightarrow{0} O[1]$ | `011` | $O$ | **`1`** | `1` | **PASS** |
| TC-07 | `111` | 3 | 3 (Odd) | $E[0] \xrightarrow{1} O[1] \xrightarrow{1} E[0] \xrightarrow{1} O[1]$ | `0101` | $O$ | **`1`** | `1` | **PASS** |

---

## 2. Moore Machine State & Output Mapping Verification
- **State E ($q_0$):**
  - Name: `E`
  - Output: `0` (`<output>0</output>`) [VERIFIED: Represents Even Parity]
  - Initial State: True (`<initial/>`)
  - Input `0` $\to$ loops to `E`
  - Input `1` $\to$ transitions to `O`
- **State O ($q_1$):**
  - Name: `O`
  - Output: `1` (`<output>1</output>`) [VERIFIED: Represents Odd Parity]
  - Initial State: False
  - Input `0` $\to$ loops to `O`
  - Input `1` $\to$ transitions to `E`

---

## 3. Structural & XML Integrity Verification
- **Root Tag:** `<structure>`
- **Type Field:** `<type>moore</type>` (ensures JFLAP opens in Moore Transducer mode)
- **State Count:** Exactly 2 states defined ($E, O$).
- **State Outputs:** Outputs are placed inside `<state>` tags (`<output>0</output>` and `<output>1</output>`).
- **Transitions:** Exactly 4 transitions. Transitions contain only `<from>`, `<to>`, and `<read>` (no `<trans>` tags).
- **Output Length Verification:** Verified that every test input of length $n$ produces an output sequence of exactly $n + 1$ symbols due to the initial state output at $t=0$.
