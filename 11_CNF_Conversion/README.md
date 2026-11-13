# Practical 11: Transformation of CFG into Chomsky Normal Form (CNF)

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**Original CFG File:** [`23_Darshan_Kankekar_CFG_Original_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/11_CNF_Conversion/23_Darshan_Kankekar_CFG_Original_13-11-2026.jff)  
**Converted CNF File:** [`23_Darshan_Kankekar_CNF_Converted_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/11_CNF_Conversion/23_Darshan_Kankekar_CNF_Converted_13-11-2026.jff)

---

## 1. Aim
To mathematically transform the Context-Free Grammar generating $L = \{ a^n b^n \mid n \ge 1 \}$ into Chomsky Normal Form (CNF), verify that all transformed productions strictly adhere to CNF constraints ($A \to BC$ or $A \to a$), and demonstrate that both grammars generate the identical language on test suites.

---

## 2. Problem / Language Statement
Let the input alphabet be $\Sigma = \{a, b\}$.  
The language is:
$$L = \{ a^n b^n \mid n \ge 1 \} = \{ ab, aabb, aaabbb, \dots \}$$

### Source Context-Free Grammar ($G_{orig}$)
- $V = \{ S \}, \quad \Sigma = \{ a, b \}, \quad \text{Start Symbol} = S$
- Productions:
  1. $S \to aSb$
  2. $S \to ab$

---

## 3. Chomsky Normal Form (CNF) Definition & Requirements
A Context-Free Grammar $G = (V, \Sigma, R, S_0)$ is in **Chomsky Normal Form** if every production rule in $R$ is of one of two canonical forms:
1. **$A \to BC$** (A single nonterminal produces exactly two nonterminals, with $B, C \in V \setminus \{S_0\}$).
2. **$A \to a$** (A single nonterminal produces exactly one terminal, with $a \in \Sigma$).
*(If $\epsilon \in L$, $S_0 \to \epsilon$ is allowed, provided $S_0$ does not appear on any RHS. Here $n \ge 1$, so no $\epsilon$-rule is present).*

### Explicit Constraints:
- No mixed terminal/nonterminal right-hand sides (e.g., $aSb$ and $ab$ are forbidden).
- No right-hand sides of length $\ge 3$ (e.g., $ASB$ is forbidden).
- No unit productions ($A \to B$) or $\lambda$-productions ($A \to \epsilon$).

---

## 4. Step-by-Step Transformation Logic

### Step 1: Introduce New Start Symbol ($S_0$)
Because the original start symbol $S$ appears on the RHS of $S \to aSb$, introduce a fresh start variable $S_0$:
$$S_0 \to S$$
Eliminating the unit production $S_0 \to S$ by substituting $S$'s alternatives yields:
$$\begin{aligned}
S_0 &\to aSb \mid ab \\
S &\to aSb \mid ab
\end{aligned}$$

### Step 2: Replace Terminals in Long/Mixed Right-Hand Sides
To isolate terminal occurrences, introduce new variables:
- $A \to a$
- $B \to b$

Substitute $A$ for $a$ and $B$ for $b$ in all RHS expressions of length $\ge 2$:
- $ab \implies AB$
- $aSb \implies ASB$

The intermediate grammar is:
$$\begin{aligned}
S_0 &\to AB \mid ASB \\
S &\to AB \mid ASB \\
A &\to a \\
B &\to b
\end{aligned}$$

### Step 3: Binarize Productions of Length $> 2$
The productions $S_0 \to ASB$ and $S \to ASB$ have RHS length 3 ($|ASB| = 3$).  
Introduce a new variable $X$ to combine the trailing pair $(SB)$:
$$X \to SB$$
Now replace $ASB$ with the binary pair $AX$:
$$\begin{aligned}
S_0 &\to AX \\
S &\to AX
\end{aligned}$$

### Resulting Chomsky Normal Form Grammar ($G_{CNF}$)
$$\begin{aligned}
S_0 &\to AB \mid AX \\
S &\to AB \mid AX \\
X &\to SB \\
A &\to a \\
B &\to b
\end{aligned}$$

---

## 5. Explicit Inspection of Every CNF Production

| Production Rule | Left-Hand Side ($A \in V$) | Right-Hand Side Pattern | CNF Classification | Complies with CNF? |
|:---:|:---:|:---:|:---:|:---:|
| $S_0 \to AB$ | Nonterminal ($S_0$) | Two Nonterminals ($A, B \in V$) | $A \to BC$ | **YES** |
| $S_0 \to AX$ | Nonterminal ($S_0$) | Two Nonterminals ($A, X \in V$) | $A \to BC$ | **YES** |
| $S \to AB$ | Nonterminal ($S$) | Two Nonterminals ($A, B \in V$) | $A \to BC$ | **YES** |
| $S \to AX$ | Nonterminal ($S$) | Two Nonterminals ($A, X \in V$) | $A \to BC$ | **YES** |
| $X \to SB$ | Nonterminal ($X$) | Two Nonterminals ($S, B \in V$) | $A \to BC$ | **YES** |
| $A \to a$ | Nonterminal ($A$) | Single Terminal ($a \in \Sigma$) | $A \to a$ | **YES** |
| $B \to b$ | Nonterminal ($B$) | Single Terminal ($b \in \Sigma$) | $A \to a$ | **YES** |

**Summary:** 5 productions of form $A \to BC$, 2 productions of form $A \to a$. Exactly 0 mixed or multi-symbol productions remain.

---

## 6. Comparative Derivations & Parse Tree Structure

### A. Derivation for String $w = \text{`ab'}$ ($n = 1$, length $m = 2$)
- **Original CFG (1 step):**
  $$S \Rightarrow ab$$
- **CNF Grammar ($2m - 1 = 3$ steps):**
  $$S_0 \Rightarrow AB \Rightarrow aB \Rightarrow ab$$

### B. Derivation for String $w = \text{`aabb'}$ ($n = 2$, length $m = 4$)
- **Original CFG (2 steps):**
  $$S \Rightarrow aSb \Rightarrow aabb$$
- **CNF Grammar ($2m - 1 = 7$ steps):**
  $$\begin{aligned}
  S_0 &\Rightarrow AX \Rightarrow aX \Rightarrow aSB \Rightarrow aABB \\
      &\Rightarrow aaBB \Rightarrow aabB \Rightarrow aabb
  \end{aligned}$$

### C. Derivation for String $w = \text{`aaabbb'}$ ($n = 3$, length $m = 6$)
- **Original CFG (3 steps):**
  $$S \Rightarrow aSb \Rightarrow aaSbb \Rightarrow aaabbb$$
- **CNF Grammar ($2m - 1 = 11$ steps):**
  $$\begin{aligned}
  S_0 &\Rightarrow AX \Rightarrow aX \Rightarrow aSB \Rightarrow aAXB \Rightarrow aaXB \\
      &\Rightarrow aaSBB \Rightarrow aaABBB \Rightarrow aaaBBB \Rightarrow aaabBB \Rightarrow aaabbB \Rightarrow aaabbb
  \end{aligned}$$

> **Theorem (CNF Derivation Length):** In any Chomsky Normal Form grammar, any derivation of a terminal string of length $m \ge 1$ requires **exactly $2m - 1$ steps** (corresponding to $m - 1$ applications of $A \to BC$ and $m$ applications of $A \to a$).

---

## 7. Comparative Test Suite & Language Invariance

| Test String $w$ | Length | Original CFG Parse | CNF Grammar Parse | CNF Derivation Steps ($2m-1$) | Agreement |
|:---|:---:|:---:|:---:|:---:|:---:|
| `ab` | 2 | **ACCEPT** | **ACCEPT** | 3 steps | **MATCH** |
| `aabb` | 4 | **ACCEPT** | **ACCEPT** | 7 steps | **MATCH** |
| `aaabbb` | 6 | **ACCEPT** | **ACCEPT** | 11 steps | **MATCH** |
| $\epsilon$ | 0 | **REJECT** | **REJECT** | None (no $\epsilon$-rule) | **MATCH** |
| `aab` | 3 | **REJECT** | **REJECT** | None (symbol imbalance) | **MATCH** |
| `abb` | 3 | **REJECT** | **REJECT** | None (symbol imbalance) | **MATCH** |
| `abab` | 4 | **REJECT** | **REJECT** | None (order violation) | **MATCH** |
| `aaabb` | 5 | **REJECT** | **REJECT** | None (symbol imbalance) | **MATCH** |

---

## 8. Inference & Conclusion
1. **Language Invariance:** Converting to CNF modifies only the syntactic representation, leaving the generated formal language strictly identical: $L(G_{orig}) = L(G_{CNF}) = \{a^n b^n \mid n \ge 1\}$.
2. **Algorithmic Utility (CYK Algorithm):** CNF guarantees binary parse trees, enabling polynomial-time parsing algorithms such as Cocke-Younger-Kasami (CYK) which operate in $O(n^3)$ time.
3. **Derivation Predictability:** Every terminal word of length $m$ has an invariant parse tree with $2m - 1$ internal nodes.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. **Open Original CFG:**
   - Click **File** $\to$ **Open** $\to$ select `JFLAP_Practicals/11_CNF_Conversion/23_Darshan_Kankekar_CFG_Original_13-11-2026.jff`.
3. **Execute Interactive CNF Transformation in JFLAP:**
   - In the CFG window, click **Convert** $\to$ **Transform Grammar**.
   - Notice JFLAP guides through step-by-step Chomsky Normal Form conversion:
     - Isolating terminals.
     - Binarizing long productions into pairs.
4. **Open and Test Converted CNF Grammar:**
   - Click **File** $\to$ **Open** $\to$ select `JFLAP_Practicals/11_CNF_Conversion/23_Darshan_Kankekar_CNF_Converted_13-11-2026.jff`.
   - Click **Input** $\to$ **Multiple Run** (or **Brute Force Parse**).
   - Enter test strings (`ab`, `aabb`, `aaabbb`, ``, `aab`, `abb`, `abab`, `aaabb`).
   - Click **Run Inputs** to verify identical outputs and inspect parse trees.
