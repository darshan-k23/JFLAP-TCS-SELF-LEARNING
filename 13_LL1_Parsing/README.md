# Practical 13: LL(1) Predictive Parsing of Arithmetic Expressions

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_LL1_Grammar_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/13_LL1_Parsing/23_Darshan_Kankekar_LL1_Grammar_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze an unambiguous, left-recursion-free LL(1) Context-Free Grammar for arithmetic expressions containing identifiers ($i$), addition ($+$), multiplication ($*$), and parentheses ($($ and $)$), compute its FIRST and FOLLOW sets, construct its predictive parsing table, and verify top-down stack parsing on positive and negative test expressions in JFLAP.

---

## 2. Problem Statement & Grammatical Architecture

### A. Precedence & Associativity Hierarchy
The grammar models binary arithmetic expressions where:
1. **Parentheses `( )`** have the highest precedence (Factor level $F$).
2. **Multiplication `*`** has intermediate precedence and binds more tightly than addition (Term level $T$).
3. **Addition `+`** has the lowest precedence (Expression level $E$).
4. Both operators are left-associative.

### B. Left-Recursion Elimination
The classical intuitive grammar:
$$\begin{aligned}
E &\to E + T \mid T \\
T &\to T * F \mid F \\
F &\to (E) \mid i
\end{aligned}$$
contains immediate left recursion ($E \to E + \dots$ and $T \to T * \dots$), which causes top-down predictive parsers to loop infinitely.  
Eliminating left recursion introduces right-recursive helper nonterminals:
- **$Q$** (acting as $E'$) to handle addition tails.
- **$R$** (acting as $T'$) to handle multiplication tails.

---

## 3. Formal Definition (4-Tuple)
The grammar is defined by the 4-tuple $G = (V, \Sigma, R_{prod}, E)$:

1. **$V = \{ E, Q, T, R, F \}$ (Variables / Nonterminals):**
   - $E$: Expression
   - $Q$: Expression tail ($E'$)
   - $T$: Term
   - $R$: Term tail ($T'$)
   - $F$: Factor
2. **$\Sigma = \{ +, *, (, ), i \}$ (Terminal Alphabet)**
3. **Start Variable:** $E$
4. **$R_{prod}$ (Set of 8 Production Rules):**
   - $E \to TQ$
   - $Q \to +TQ$
   - $Q \to \epsilon$
   - $T \to FR$
   - $R \to *FR$
   - $R \to \epsilon$
   - $F \to (E)$
   - $F \to i$

*(Note: In JFLAP XML, the empty string $\epsilon$ is represented by an empty right-hand side tag `<right/>`).*

---

## 4. Computation of FIRST and FOLLOW Sets

### A. FIRST Sets
- $\text{FIRST}(F)$: From $F \to (E)$ and $F \to i \implies \mathbf{\{ (, i \}}$
- $\text{FIRST}(R)$: From $R \to *FR$ and $R \to \epsilon \implies \mathbf{\{ *, \epsilon \}}$
- $\text{FIRST}(T)$: From $T \to FR$, since $\epsilon \notin \text{FIRST}(F) \implies \text{FIRST}(F) = \mathbf{\{ (, i \}}$
- $\text{FIRST}(Q)$: From $Q \to +TQ$ and $Q \to \epsilon \implies \mathbf{\{ +, \epsilon \}}$
- $\text{FIRST}(E)$: From $E \to TQ$, since $\epsilon \notin \text{FIRST}(T) \implies \text{FIRST}(T) = \mathbf{\{ (, i \}}$

### B. FOLLOW Sets (with $\$$ as end-of-input marker)
- $\text{FOLLOW}(E)$: Start symbol receives $\$$. Production $F \to (E)$ contributes $)$ $\implies \mathbf{\{ ), \$ \}}$
- $\text{FOLLOW}(Q)$: From $E \to TQ$, inherits $\text{FOLLOW}(E) \implies \mathbf{\{ ), \$ \}}$
- $\text{FOLLOW}(T)$: From $E \to TQ$, receives $(\text{FIRST}(Q) \setminus \{\epsilon\}) \cup \text{FOLLOW}(E) = \{ + \} \cup \{ ), \$ \} \implies \mathbf{\{ +, ), \$ \}}$
- $\text{FOLLOW}(R)$: From $T \to FR$, inherits $\text{FOLLOW}(T) \implies \mathbf{\{ +, ), \$ \}}$
- $\text{FOLLOW}(F)$: From $T \to FR$, receives $(\text{FIRST}(R) \setminus \{\epsilon\}) \cup \text{FOLLOW}(T) = \{ * \} \cup \{ +, ), \$ \} \implies \mathbf{\{ *, +, ), \$ \}}$

### Summary Table: FIRST & FOLLOW
| Nonterminal ($A$) | $\text{FIRST}(A)$ | $\text{FOLLOW}(A)$ |
|:---:|:---:|:---:|
| **$E$** | $\{ (, i \}$ | $\{ ), \$ \}$ |
| **$Q$** | $\{ +, \epsilon \}$ | $\{ ), \$ \}$ |
| **$T$** | $\{ (, i \}$ | $\{ +, ), \$ \}$ |
| **$R$** | $\{ *, \epsilon \}$ | $\{ +, ), \$ \}$ |
| **$F$** | $\{ (, i \}$ | $\{ *, +, ), \$ \}$ |

---

## 5. LL(1) Predictive Parsing Table ($M$)

For each production $A \to \alpha$:
- For each terminal $a \in \text{FIRST}(\alpha)$, add $A \to \alpha$ to $M[A, a]$.
- If $\epsilon \in \text{FIRST}(\alpha)$, for each $b \in \text{FOLLOW}(A)$, add $A \to \alpha$ to $M[A, b]$.

| Nonterminal | Token `i` | Token `+` | Token `*` | Token `(` | Token `)` | End Marker `$` |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **$E$** | $E \to TQ$ | — | — | $E \to TQ$ | — | — |
| **$Q$** | — | $Q \to +TQ$ | — | — | $Q \to \epsilon$ | $Q \to \epsilon$ |
| **$T$** | $T \to FR$ | — | — | $T \to FR$ | — | — |
| **$R$** | — | $R \to \epsilon$ | $R \to *FR$ | — | $R \to \epsilon$ | $R \to \epsilon$ |
| **$F$** | $F \to i$ | — | — | $F \to (E)$ | — | — |

> **LL(1) Determinism Proof:** Every cell in the parsing table contains **at most one** entry. There are zero multiple-definition conflicts. Therefore, the grammar is unconditionally **LL(1)**.

---

## 6. Detailed Parser-Stack Action Trace for `i+i*i`

Below is the complete 17-step top-down predictive stack execution trace:

| Step | Parser Stack | Remaining Input | Action / Rule Applied |
|:---:|:---|:---|:---|
| 1 | `$ E` | `i+i*i$` | Apply $E \to TQ$ |
| 2 | `$ Q T` | `i+i*i$` | Apply $T \to FR$ |
| 3 | `$ Q R F` | `i+i*i$` | Apply $F \to i$ |
| 4 | `$ Q R i` | `i+i*i$` | **Match token `i`** |
| 5 | `$ Q R` | `+i*i$` | Apply $R \to \epsilon$ (since lookahead is `+`) |
| 6 | `$ Q` | `+i*i$` | Apply $Q \to +TQ$ (since lookahead is `+`) |
| 7 | `$ Q T +` | `+i*i$` | **Match token `+`** |
| 8 | `$ Q T` | `i*i$` | Apply $T \to FR$ |
| 9 | `$ Q R F` | `i*i$` | Apply $F \to i$ |
| 10 | `$ Q R i` | `i*i$` | **Match token `i`** |
| 11 | `$ Q R` | `*i$` | Apply $R \to *FR$ (since lookahead is `*`) |
| 12 | `$ Q R F *` | `*i$` | **Match token `*`** |
| 13 | `$ Q R F` | `i$` | Apply $F \to i$ |
| 14 | `$ Q R i` | `i$` | **Match token `i`** |
| 15 | `$ Q R` | `$` | Apply $R \to \epsilon$ (since lookahead is `$`) |
| 16 | `$ Q` | `$` | Apply $Q \to \epsilon$ (since lookahead is `$`) |
| 17 | `$` | `$` | **Match `$` $\implies$ ACCEPTED** |

---

## 7. Parse Tree Representation for `i+i*i`

```
               E
             /   \
            T     Q
           / \   / | \
          F   R +  T  Q
          |   |   / \  |
          i  eps F   R eps
                 |  / \
                 i *   F   R
                       |   |
                       i  eps
```
Yield: `i + i * i`. Notice that $i * i$ forms a subtree under the right $T$, confirming that multiplication binds tighter than addition.

---

## 8. Test Suite and Execution Results

| Test Input | Input Category | Structural Evaluation | LL(1) Parser Result | Reason for Failure / Acceptance |
|:---|:---:|:---|:---:|:---|
| `i+i*i` | Valid | Addition of term and product | **ACCEPT** | 17 steps; clean stack clearance |
| `(i+i)*i` | Valid | Parenthesized sum multiplied by identifier | **ACCEPT** | 24 steps; subexpression elevated to factor |
| `i*(i+i)` | Valid | Identifier multiplied by parenthesized sum | **ACCEPT** | 24 steps; clean stack clearance |
| `i` | Valid | Single atomic identifier | **ACCEPT** | 7 steps ($E \to TQ \to FRQ \to iRQ \to i$) |
| `i+*i` | Invalid | Operator adjacent error (`+*`) | **REJECT** | After `+`, expects factor ($i$ or `(`); finds `*` |
| `(i+i` | Invalid | Unbalanced parenthesis | **REJECT** | Input exhausted (`$`); stack expects matching `)` |
| `+i` | Invalid | Leading operator | **REJECT** | Cell $M[E, +]$ is blank; expression cannot start with `+` |
| `ii` | Invalid | Adjacent identifiers | **REJECT** | After first $i$, stack has $Q$; lookahead $i$ has no rule in $M[Q, i]$ |

---

## 9. Inference & Conclusion
1. **Unambiguity of LL(1):** The absence of table conflicts guarantees that arithmetic expressions have unique, unambiguous parse trees.
2. **Precedence Enforcement:** Factoring expressions into $E, T,$ and $F$ structurally enforces precedence without needing external disambiguation rules.
3. **Linear Time Performance:** Predictive parsing executes in $O(n)$ time complexity, making it ideal for compiler front-ends.

---

## 10. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** $\to$ select:
   `JFLAP_Practicals/13_LL1_Parsing/23_Darshan_Kankekar_LL1_Grammar_13-11-2026.jff`
3. JFLAP opens the 8 productions in the Grammar Editor.
4. **Interactive LL(1) Parsing Table & Stack Run:**
   - In the menu bar, click **Input** $\to$ **LL (1) Parsing**.
   - JFLAP opens the LL(1) parsing module.
   - Click **Show All** in the FIRST and FOLLOW table to verify all computed sets.
   - Click **Build Table** or **Next** to generate the predictive parse table.
   - In the string input box, enter `i+i*i` and click **Start**.
   - Click **Step** to observe the stack replacements and input token consumption.
   - Watch the parse tree grow incrementally in the right-hand panel until acceptance.
5. **Batch Testing:**
   - Test invalid inputs like `i+*i` and `(i+i` to observe JFLAP reporting parse errors at the exact error token.
