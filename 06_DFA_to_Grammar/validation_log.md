# Validation Log: Practical 06 (DFA to Right-Linear Grammar)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**Files Under Test:**
- Source DFA: `23_Darshan_Kankekar_DFA_Source_13-11-2026.jff`
- Converted Grammar: `23_Darshan_Kankekar_Grammar_Converted_13-11-2026.jff`  
**Execution Environment:** XML Structure & Dual DFA/Grammar Verifier (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Transition/Derivation Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Dual-Model Equivalence Test Matrix

| Test ID | Input String ($w$) | Count of 1s | DFA Final State | DFA Result | Grammar Derivation | Grammar Result | Prompt Expectation | Match? | Status |
|:---:|:---:|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|
| TC-01 | `""` ($\epsilon$) | 0 (Even) | $q_E \in F$ | **ACCEPT** | $S \Rightarrow \epsilon$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-02 | `0` | 0 (Even) | $q_E \in F$ | **ACCEPT** | $S \Rightarrow 0S \Rightarrow 0$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-03 | `11` | 2 (Even) | $q_E \in F$ | **ACCEPT** | $S \Rightarrow 1A \Rightarrow 11S \Rightarrow 11$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-04 | `1010` | 2 (Even) | $q_E \in F$ | **ACCEPT** | $S \Rightarrow 1A \Rightarrow 10A \Rightarrow 101S \Rightarrow 1010S \Rightarrow 1010$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-05 | `001100` | 2 (Even) | $q_E \in F$ | **ACCEPT** | $S \Rightarrow 0S \Rightarrow^* 001100S \Rightarrow 001100$ | **ACCEPT** | Accept | **YES** | **PASS** |
| TC-06 | `1` | 1 (Odd) | $q_O \notin F$ | **REJECT** | $S \Rightarrow 1A$ (cannot terminate) | **REJECT** | Reject | **YES** | **PASS** |
| TC-07 | `10` | 1 (Odd) | $q_O \notin F$ | **REJECT** | $S \Rightarrow 1A \Rightarrow 10A$ (cannot terminate) | **REJECT** | Reject | **YES** | **PASS** |
| TC-08 | `111` | 3 (Odd) | $q_O \notin F$ | **REJECT** | $S \Rightarrow 1A \Rightarrow 11S \Rightarrow 111A$ (cannot terminate) | **REJECT** | Reject | **YES** | **PASS** |
| TC-09 | `010` | 1 (Odd) | $q_O \notin F$ | **REJECT** | $S \Rightarrow 0S \Rightarrow 01A \Rightarrow 010A$ (cannot terminate) | **REJECT** | Reject | **YES** | **PASS** |

---

## 2. Production Rule & Structural Integrity Verification
- **Source DFA:**
  - Structure: `<type>fa</type>`
  - States: Exactly 2 ($q_E$ [initial/final], $q_O$ [non-final])
  - Transitions: Exactly 4 (deterministic)
- **Converted Grammar:**
  - Structure: `<type>grammar</type>`
  - Production Count: Exactly 5 productions
  - Start Variable: $S$ (matches start state $q_E$)
  - Transitions to Productions:
    - $\delta(q_E, 0) = q_E \iff S \to 0S$ [VERIFIED]
    - $\delta(q_E, 1) = q_O \iff S \to 1A$ [VERIFIED]
    - $\delta(q_O, 0) = q_O \iff A \to 0A$ [VERIFIED]
    - $\delta(q_O, 1) = q_E \iff A \to 1S$ [VERIFIED]
  - Acceptance Rule:
    - $q_E \in F \iff S \to \epsilon$ (`<right/>`) [VERIFIED]
    - $q_O \notin F \iff$ No $\epsilon$-rule for $A$ [VERIFIED]

