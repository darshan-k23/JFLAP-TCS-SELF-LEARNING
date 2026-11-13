# Validation Log: Practical 12 (CYK Parsing Algorithm)

**Course:** Theory of Computer Science (TCS)  
**Student:** Darshan Kankekar (Roll No: 23, Division: D12A, VESIT)  
**File Under Test:** `23_Darshan_Kankekar_CYK_13-11-2026.jff`  
**Execution Environment:** XML Structure & Dynamic Programming CYK Engine (.NET / PowerShell), JFLAP 7.x Compatible  
**Date of Validation:** 13-11-2026  
**Verification Status:** XML Validated & CYK Algorithm Verified; Manual JFLAP GUI Screenshot Pending  

---

## 1. CYK Parsing Test Matrix

| Test ID | Input String ($w$) | Length ($n$) | In Language $a^n b^n$? | CYK Table Apex Cell $V[1, n]$ | Contains $S_0$? | CYK Decision | Prompt Expectation | Test Status |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| TC-01 | `aabb` | 4 | **YES** ($n = 2$) | $\{ S_0, S \}$ | **YES** | **ACCEPT** | Accept | **PASS** |
| TC-02 | `aab` | 3 | **NO** ($N_a \ne N_b$) | $\emptyset$ | **NO** | **REJECT** | Reject | **PASS** |

---

## 2. Detailed Cell-by-Cell Trace for `aabb` ($n = 4$)

| Cell | Substring ($w[i \dots j]$) | Span Length | Computed Nonterminals | Derivation Origin |
|:---:|:---:|:---:|:---:|:---|
| $V[1, 1]$ | `a` (index 1) | 1 | $\{ A \}$ | Rule $A \to a$ |
| $V[2, 1]$ | `a` (index 2) | 1 | $\{ A \}$ | Rule $A \to a$ |
| $V[3, 1]$ | `b` (index 3) | 1 | $\{ B \}$ | Rule $B \to b$ |
| $V[4, 1]$ | `b` (index 4) | 1 | $\{ B \}$ | Rule $B \to b$ |
| $V[1, 2]$ | `aa` (indices 1..2) | 2 | $\emptyset$ | Split $k=1$: $A \times A = AA$ (no rule) |
| $V[2, 2]$ | `ab` (indices 2..3) | 2 | $\{ S_0, S \}$ | Split $k=1$: $A \times B = AB$ (rules $S_0 \to AB, S \to AB$) |
| $V[3, 2]$ | `bb` (indices 3..4) | 2 | $\emptyset$ | Split $k=1$: $B \times B = BB$ (no rule) |
| $V[1, 3]$ | `aab` (indices 1..3) | 3 | $\emptyset$ | Splits $k=1 (A \times \{S_0, S\}), k=2 (\emptyset \times B)$ yield no rules |
| $V[2, 3]$ | `abb` (indices 2..4) | 3 | $\{ X \}$ | Split $k=2$: $\{S_0, S\} \times B = \{S_0 B, SB\}$ (rule $X \to SB$) |
| $V[1, 4]$ | `aabb` (indices 1..4) | 4 | $\{ S_0, S \}$ | Split $k=1$: $A \times X = AX$ (rules $S_0 \to AX, S \to AX$) |

**Apex Conclusion:** $S_0 \in V[1, 4]$ verifies that `aabb` is in $L(G_{CNF})$ $\implies$ **ACCEPT**.

---

## 3. Detailed Cell-by-Cell Trace for `aab` ($n = 3$)

| Cell | Substring ($w[i \dots j]$) | Span Length | Computed Nonterminals | Derivation Origin |
|:---:|:---:|:---:|:---:|:---|
| $V[1, 1]$ | `a` (index 1) | 1 | $\{ A \}$ | Rule $A \to a$ |
| $V[2, 1]$ | `a` (index 2) | 1 | $\{ A \}$ | Rule $A \to a$ |
| $V[3, 1]$ | `b` (index 3) | 1 | $\{ B \}$ | Rule $B \to b$ |
| $V[1, 2]$ | `aa` (indices 1..2) | 2 | $\emptyset$ | Split $k=1$: $A \times A = AA$ (no rule) |
| $V[2, 2]$ | `ab` (indices 2..3) | 2 | $\{ S_0, S \}$ | Split $k=1$: $A \times B = AB$ (rules $S_0 \to AB, S \to AB$) |
| $V[1, 3]$ | `aab` (indices 1..3) | 3 | $\emptyset$ | Splits $k=1 (A \times \{S_0, S\}), k=2 (\emptyset \times B)$ yield no rules |

**Apex Conclusion:** $V[1, 3] = \emptyset \implies S_0 \notin V[1, 3] \implies$ **REJECT**.

---

## 4. Structural & XML Integrity Verification
- **Root Tag:** `<structure>`
- **Type Field:** `<type>grammar</type>`
- **Production Count:** Exactly 7 productions (Chomsky Normal Form).
- **Start Symbol:** `S0`.
- **XML Well-Formedness:** Validated cleanly via .NET XML parser.
