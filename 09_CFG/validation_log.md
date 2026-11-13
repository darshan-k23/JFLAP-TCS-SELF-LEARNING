# Validation Log: Practical 09 (Context-Free Grammar for $a^n b^n$)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_CFG_13-11-2026.jff`  
**Execution Environment:** XML Structure & Grammar Derivation Parser (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & Derivation Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. Grammar Membership Test Matrix

| Test ID | Input String ($w$) | Length | Structural Property | Full Derivation Sequence | Parse Outcome | Prompt Expectation | Test Status |
|:---:|:---:|:---:|:---|:---|:---:|:---:|:---:|
| TC-01 | `ab` | 2 | $n = 1$: $a^1 b^1$ | $S \Rightarrow ab$ | **ACCEPT** | Accept | **PASS** |
| TC-02 | `aabb` | 4 | $n = 2$: $a^2 b^2$ | $S \Rightarrow aSb \Rightarrow aabb$ | **ACCEPT** | Accept | **PASS** |
| TC-03 | `aaabbb` | 6 | $n = 3$: $a^3 b^3$ | $S \Rightarrow aSb \Rightarrow aaSbb \Rightarrow aaabbb$ | **ACCEPT** | Accept | **PASS** |
| TC-04 | `aaaabbbb` | 8 | $n = 4$: $a^4 b^4$ | $S \Rightarrow aSb \Rightarrow aaSbb \Rightarrow aaaSbbb \Rightarrow aaaabbbb$ | **ACCEPT** | Accept | **PASS** |
| TC-05 | `""` ($\epsilon$) | 0 | $n < 1$ (Null string) | No derivation (no $\epsilon$-rule) | **REJECT** | Reject | **PASS** |
| TC-06 | `a` | 1 | $N_a=1, N_b=0$ | No derivation | **REJECT** | Reject | **PASS** |
| TC-07 | `b` | 1 | $N_a=0, N_b=1$ | No derivation | **REJECT** | Reject | **PASS** |
| TC-08 | `aab` | 3 | $N_a=2 \ne N_b=1$ | No derivation | **REJECT** | Reject | **PASS** |
| TC-09 | `abb` | 3 | $N_a=1 \ne N_b=2$ | No derivation | **REJECT** | Reject | **PASS** |
| TC-10 | `abab` | 4 | Equal count, order violated | No derivation | **REJECT** | Reject | **PASS** |
| TC-11 | `aaabb` | 5 | $N_a=3 \ne N_b=2$ | No derivation | **REJECT** | Reject | **PASS** |

---

## 2. Invariant & Production Verification
- **Production 1 ($S \to aSb$):**
  - Left-hand side: `S` [Nonterminal]
  - Right-hand side: `aSb` [Recursive nesting]
  - Symmetrically adds one `a` on the left and one `b` on the right.
- **Production 2 ($S \to ab$):**
  - Left-hand side: `S` [Nonterminal]
  - Right-hand side: `ab` [Terminal base case]
  - Generates the shortest valid terminal string ($n=1$).
- **Absence of $\epsilon$-rule:**
  - Enforces condition $n \ge 1$; empty string $\epsilon$ cannot be derived.

---

## 3. Structural & XML Integrity Verification
- **Root Tag:** `<structure>`
- **Type Field:** `<type>grammar</type>` (ensures JFLAP opens in Grammar Editor)
- **Production Count:** Exactly 2 productions.
- **Start Symbol:** `S` (first production's left-hand variable).
- **XML Well-Formedness:** Validated cleanly via .NET XML parser.
