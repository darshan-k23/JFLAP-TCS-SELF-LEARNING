# Validation Log: Practical 07 (Mealy Machine)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_Mealy_13-11-2026.jff`  
**Execution Environment:** XML Structure & Mealy Transducer Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transducer Output Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Transducer Output Test Matrix

| Test ID | Input ($x$) | Input Length | Transition & Output Sequence | Generated Output ($y$) | Expected Output | $|y| = |x|$ Check | Output Match? | Status |
|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `101` | 3 | $A \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{1/1} B$ | `001` | `001` | Yes ($3=3$) | **YES** | **PASS** |
| TC-02 | `1101` | 4 | $A \xrightarrow{1/0} B \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{1/1} B$ | `0001` | `0001` | Yes ($4=4$) | **YES** | **PASS** |
| TC-03 | `10101` | 5 | $A \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{1/1} B \xrightarrow{0/0} C \xrightarrow{1/1} B$ | `00101` | `00101` | Yes ($5=5$) | **YES** | **PASS (Overlap Verified)** |
| TC-04 | `111` | 3 | $A \xrightarrow{1/0} B \xrightarrow{1/0} B \xrightarrow{1/0} B$ | `000` | `000` | Yes ($3=3$) | **YES** | **PASS** |
| TC-05 | `1000` | 4 | $A \xrightarrow{1/0} B \xrightarrow{0/0} C \xrightarrow{0/0} A \xrightarrow{0/0} A$ | `0000` | `0000` | Yes ($4=4$) | **YES** | **PASS** |

---

## 2. Mealy Machine Transition & Output Verification
- **State A ($q_0$):**
  - Input `0` $\to$ Next State: `A`, Output: `0` [VERIFIED]
  - Input `1` $\to$ Next State: `B`, Output: `0` [VERIFIED]
- **State B ($q_1$):**
  - Input `0` $\to$ Next State: `C`, Output: `0` [VERIFIED]
  - Input `1` $\to$ Next State: `B`, Output: `0` [VERIFIED]
- **State C ($q_2$):**
  - Input `0` $\to$ Next State: `A`, Output: `0` [VERIFIED]
  - Input `1` $\to$ Next State: `B`, Output: `1` [VERIFIED: Pattern 101 completed!]

---

## 3. Structural & XML Integrity Verification
- **Root Tag:** `<structure>`
- **Type Field:** `<type>mealy</type>` (ensures JFLAP opens in Mealy Transducer mode)
- **State Count:** Exactly 3 states defined ($A, B, C$).
- **Initial State:** State $A$ (ID 0) carries `<initial/>`.
- **Transitions:** Exactly 6 transitions.
- **Output Syntax:** Each transition explicitly contains `<read>` for input and `<trans>` for output (e.g., `<from>2</from><to>1</to><read>1</read><trans>1</trans>`).
- **One Output Per Input:** Validated across all test cases that output length strictly equals input length.
