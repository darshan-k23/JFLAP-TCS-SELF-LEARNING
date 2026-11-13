# Validation Log: Practical 13 (LL(1) Grammar Parsing)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_LL1_Grammar_13-11-2026.jff`  
**Execution Environment:** XML Structure & LL(1) Predictive Parsing Engine (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & LL(1) Stack Parsing Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. LL(1) Parsing Test Matrix

| Test ID | Input String | Classification | Parse Action Summary | Parser Step Count | Expected Result | Actual Result | Status |
|:---:|:---:|:---:|:---|:---:|:---:|:---:|:---:|
| TC-01 | `i+i*i` | Valid Expression | Full derivation: $E \Rightarrow^* i + i * i$ | 17 steps | Accept | **ACCEPT** | **PASS** |
| TC-02 | `(i+i)*i` | Valid Expression | Subexpression in parentheses evaluated first | 24 steps | Accept | **ACCEPT** | **PASS** |
| TC-03 | `i*(i+i)` | Valid Expression | Multiplication with parenthesized sum | 24 steps | Accept | **ACCEPT** | **PASS** |
| TC-04 | `i` | Valid Expression | Single atomic identifier | 7 steps | Accept | **ACCEPT** | **PASS** |
| TC-05 | `i+*i` | Invalid Expression | Operator clash: encounter `*` after `+` | 8 steps (Halts on `*`) | Reject | **REJECT** | **PASS** |
| TC-06 | `(i+i` | Invalid Expression | Unbalanced parenthesis: premature `$` | 17 steps (Halts on `$`) | Reject | **REJECT** | **PASS** |
| TC-07 | `+i` | Invalid Expression | Illegal leading operator: $M[E, +]$ is blank | 1 step (Halts on `+`) | Reject | **REJECT** | **PASS** |
| TC-08 | `ii` | Invalid Expression | Consecutive identifiers without operator | 5 steps (Halts on 2nd `i`) | Reject | **REJECT** | **PASS** |

---

## 2. LL(1) Grammatical Integrity & Set Verification
- **Left-Recursion Freedom:**
  - $E \to TQ$ (no leading $E$)
  - $Q \to +TQ \mid \epsilon$ (right-recursive tail)
  - $T \to FR$ (no leading $T$)
  - $R \to *FR \mid \epsilon$ (right-recursive tail)
  - $F \to (E) \mid i$ (base cases)
  - Verified: Zero direct or indirect left recursion.
- **FIRST Set Verification:**
  - $\text{FIRST}(E) = \{ (, i \}$
  - $\text{FIRST}(Q) = \{ +, \epsilon \}$
  - $\text{FIRST}(T) = \{ (, i \}$
  - $\text{FIRST}(R) = \{ *, \epsilon \}$
  - $\text{FIRST}(F) = \{ (, i \}$
- **FOLLOW Set Verification:**
  - $\text{FOLLOW}(E) = \{ ), \$ \}$
  - $\text{FOLLOW}(Q) = \{ ), \$ \}$
  - $\text{FOLLOW}(T) = \{ +, ), \$ \}$
  - $\text{FOLLOW}(R) = \{ +, ), \$ \}$
  - $\text{FOLLOW}(F) = \{ *, +, ), \$ \}$
- **Conflict-Free Table Check:** Every entry in $M[A, a]$ has at most one production. Deterministic LL(1) property is 100% verified.

---

## 3. Structural & XML Integrity Verification
- **Root Tag:** `<structure>`
- **Type Field:** `<type>grammar</type>`
- **Production Count:** Exactly 8 rules.
- **Epsilon Notation:** Rules $Q \to \epsilon$ and $R \to \epsilon$ use native empty `<right/>` tags.
- **Start Symbol:** `E`.
- **XML Well-Formedness:** Validated cleanly via .NET XML parser.
