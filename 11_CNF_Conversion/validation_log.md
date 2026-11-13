# Validation Log: Practical 11 (Chomsky Normal Form Conversion)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**Files Under Test:**
- Original CFG: `23_Darshan_Kankekar_CFG_Original_13-11-2026.jff`
- Converted CNF: `23_Darshan_Kankekar_CNF_Converted_13-11-2026.jff`  
**Execution Environment:** XML Structure & Grammar Parser (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & CNF Adherence Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Dual-Grammar Equivalence Test Matrix

| Test ID | Input String ($w$) | Length ($m$) | Original CFG Parse | CNF Grammar Parse | CNF Derivation Steps ($2m-1$) | Expected Result | Grammars Agree? | Status |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `ab` | 2 | **ACCEPT** | **ACCEPT** | 3 steps ($2(2)-1$) | Accept | **YES** | **PASS** |
| TC-02 | `aabb` | 4 | **ACCEPT** | **ACCEPT** | 7 steps ($2(4)-1$) | Accept | **YES** | **PASS** |
| TC-03 | `aaabbb` | 6 | **ACCEPT** | **ACCEPT** | 11 steps ($2(6)-1$) | Accept | **YES** | **PASS** |
| TC-04 | `""` ($\epsilon$) | 0 | **REJECT** | **REJECT** | N/A (no $\epsilon$-rule) | Reject | **YES** | **PASS** |
| TC-05 | `aab` | 3 | **REJECT** | **REJECT** | N/A (imbalance) | Reject | **YES** | **PASS** |
| TC-06 | `abb` | 3 | **REJECT** | **REJECT** | N/A (imbalance) | Reject | **YES** | **PASS** |
| TC-07 | `abab` | 4 | **REJECT** | **REJECT** | N/A (order violation) | Reject | **YES** | **PASS** |
| TC-08 | `aaabb` | 5 | **REJECT** | **REJECT** | N/A (imbalance) | Reject | **YES** | **PASS** |

---

## 2. Production-by-Production CNF Compliance Audit

| Rule ID | Production | LHS Variable | RHS Symbols | RHS Category | Strictly Compliant? |
|:---:|:---:|:---:|:---:|:---:|:---:|
| R-01 | $S_0 \to AB$ | $S_0 \in V$ | $A, B \in V$ | Two Nonterminals ($A \to BC$) | **YES** |
| R-02 | $S_0 \to AX$ | $S_0 \in V$ | $A, X \in V$ | Two Nonterminals ($A \to BC$) | **YES** |
| R-03 | $S \to AB$ | $S \in V$ | $A, B \in V$ | Two Nonterminals ($A \to BC$) | **YES** |
| R-04 | $S \to AX$ | $S \in V$ | $A, X \in V$ | Two Nonterminals ($A \to BC$) | **YES** |
| R-05 | $X \to SB$ | $X \in V$ | $S, B \in V$ | Two Nonterminals ($A \to BC$) | **YES** |
| R-06 | $A \to a$ | $A \in V$ | $a \in \Sigma$ | Single Terminal ($A \to a$) | **YES** |
| R-07 | $B \to b$ | $B \in V$ | $b \in \Sigma$ | Single Terminal ($A \to a$) | **YES** |

- Total Productions in CNF: 7
- Total Multi-Symbol Productions ($> 2$): 0
- Total Mixed Terminal/Nonterminal Productions: 0
- Total Unit Productions ($A \to B$): 0
- Total Lambda Productions ($A \to \epsilon$): 0

---

## 3. Structural & XML Integrity Verification
- **Original CFG XML:**
  - Structure: `<type>grammar</type>`
  - Productions: 2 rules ($S \to aSb, S \to ab$)
- **Converted CNF XML:**
  - Structure: `<type>grammar</type>`
  - Productions: Exactly 7 rules
  - Start Symbol: `S0` (first production's LHS variable)
- **XML Well-Formedness:** Validated cleanly via .NET XML parser.
