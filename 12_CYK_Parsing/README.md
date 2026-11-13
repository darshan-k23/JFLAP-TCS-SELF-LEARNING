# Practical 12: Cocke-Younger-Kasami (CYK) Parsing Algorithm

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_CYK_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/12_CYK_Parsing/23_Darshan_Kankekar_CYK_13-11-2026.jff)

---

## 1. Aim
To execute and analyze the Cocke-Younger-Kasami (CYK) dynamic programming parsing algorithm in JFLAP using a Chomsky Normal Form (CNF) grammar, construct the triangular substring parse tables for valid and invalid input strings (`aabb` and `aab`), and evaluate grammatical membership based on root cell derivation sets.

---

## 2. Problem Statement & Grammar Specification
The target language is the classic non-regular context-free language:
$$L = \{ a^n b^n \mid n \ge 1 \}$$

### Chomsky Normal Form Grammar ($G_{CNF}$)
Defined by the 4-tuple $G = (V, \Sigma, R, S_0)$ with Start Symbol **$S_0$**:
- $V = \{ S_0, S, X, A, B \}$
- $\Sigma = \{ a, b \}$
- $R$ (Production Rules):
  1. $S_0 \to AB$
  2. $S_0 \to AX$
  3. $S \to AB$
  4. $S \to AX$
  5. $X \to SB$
  6. $A \to a$
  7. $B \to b$

---

## 3. The CYK Parsing Algorithm Overview

### A. Why CYK Requires Chomsky Normal Form
The CYK algorithm relies on a strict invariant:
- Terminal rules are strictly unary: $A \to a$. This enables direct $O(1)$ identification of candidate nonterminals for every terminal symbol in the input string (base cases of length 1).
- Nonterminal rules are strictly binary: $A \to BC$. This guarantees that any valid derivation tree for a substring $w[i \dots i+l-1]$ of length $l \ge 2$ must divide into **exactly two** non-empty contiguous components: a left part $w[i \dots i+k-1]$ derived from $B$ (length $k$) and a right part $w[i+k \dots i+l-1]$ derived from $C$ (length $l-k$).

### B. Dynamic Programming Formulation
Let $V[i, l]$ denote the set of all nonterminals that can derive the substring of length $l$ beginning at position $i$ ($w[i \dots i+l-1]$):
1. **Base Case (Length $l = 1$):**
   $$V[i, 1] = \{ A \in V \mid A \to w_i \in R \}$$
2. **Inductive Step (Length $l = 2$ to $n$):**
   For all split points $k$ such that $1 \le k < l$:
   $$V[i, l] = \bigcup_{k=1}^{l-1} \{ A \in V \mid A \to BC \in R, \text{ where } B \in V[i, k] \text{ and } C \in V[i+k, l-k] \}$$
3. **Acceptance Condition:**
   The entire input string $w$ of length $n$ is accepted if and only if the start symbol $S_0$ is present in the top-level cell:
   $$w \in L(G) \iff S_0 \in V[1, n]$$

---

## 4. Step-by-Step CYK Table Construction for `aabb` ($n = 4$)

Let $w = w_1 w_2 w_3 w_4 = \text{`aabb'}$.

### Length $l = 1$ (Individual Terminals)
- $V[1, 1]$ for $w_1 = a$: $\{ A \}$ (via $A \to a$)
- $V[2, 1]$ for $w_2 = a$: $\{ A \}$ (via $A \to a$)
- $V[3, 1]$ for $w_3 = b$: $\{ B \}$ (via $B \to b$)
- $V[4, 1]$ for $w_4 = b$: $\{ B \}$ (via $B \to b$)

### Length $l = 2$ (Substring Length 2)
- **$V[1, 2]$ for $w[1 \dots 2] = aa$ ($k = 1$):**
  $V[1, 1] \times V[2, 1] = \{A\} \times \{A\} = \{AA\} \implies \emptyset$
- **$V[2, 2]$ for $w[2 \dots 3] = ab$ ($k = 1$):**
  $V[2, 1] \times V[3, 1] = \{A\} \times \{B\} = \{AB\} \implies \{ S_0, S \}$ (via $S_0 \to AB, S \to AB$)
- **$V[3, 2]$ for $w[3 \dots 4] = bb$ ($k = 1$):**
  $V[3, 1] \times V[4, 1] = \{B\} \times \{B\} = \{BB\} \implies \emptyset$

### Length $l = 3$ (Substring Length 3)
- **$V[1, 3]$ for $w[1 \dots 3] = aab$:**
  - $k=1$: $V[1, 1] \times V[2, 2] = \{A\} \times \{S_0, S\} = \{AS_0, AS\} \implies \emptyset$
  - $k=2$: $V[1, 2] \times V[3, 1] = \emptyset \times \{B\} = \emptyset$
  - Result: $V[1, 3] = \emptyset$
- **$V[2, 3]$ for $w[2 \dots 4] = abb$:**
  - $k=1$: $V[2, 1] \times V[3, 2] = \{A\} \times \emptyset = \emptyset$
  - $k=2$: $V[2, 2] \times V[4, 1] = \{S_0, S\} \times \{B\} = \{S_0 B, SB\}$
    - $X \to SB \implies \{ X \}$
  - Result: $V[2, 3] = \{ X \}$

### Length $l = 4$ (Full String $w[1 \dots 4] = aabb$)
- **$V[1, 4]$ for $w[1 \dots 4] = aabb$:**
  - $k=1$: $V[1, 1] \times V[2, 3] = \{A\} \times \{X\} = \{AX\}$
    - Rules: $S_0 \to AX$ and $S \to AX \implies \{ S_0, S \}$
  - $k=2$: $V[1, 2] \times V[3, 2] = \emptyset \times \emptyset = \emptyset$
  - $k=3$: $V[1, 3] \times V[4, 1] = \emptyset \times \{B\} = \emptyset$
  - Result: $V[1, 4] = \{ S_0, S \}$

---

## 5. CYK Triangular Parse Table for `aabb`

Below is the dynamic programming table formatted with span length on the vertical axis and start index on the horizontal axis:

| Length ($l$) | $i = 1$ ($w_1 = a$) | $i = 2$ ($w_2 = a$) | $i = 3$ ($w_3 = b$) | $i = 4$ ($w_4 = b$) |
|:---:|:---:|:---:|:---:|:---:|
| **$l = 4$** | **$\{ S_0, S \}$** | — | — | — |
| **$l = 3$** | $\emptyset$ | $\{ X \}$ | — | — |
| **$l = 2$** | $\emptyset$ | $\{ S_0, S \}$ | $\emptyset$ | — |
| **$l = 1$** | $\{ A \}$ | $\{ A \}$ | $\{ B \}$ | $\{ B \}$ |
| **Substrings** | $w_1 = a$ | $w_2 = a$ | $w_3 = b$ | $w_4 = b$ |

### Result for `aabb`:
$$S_0 \in V[1, 4] = \{ S_0, S \} \implies \mathbf{ACCEPTED}$$

---

## 6. Step-by-Step CYK Table Construction for `aab` ($n = 3$)

Let $w = w_1 w_2 w_3 = \text{`aab'}$.

### Length $l = 1$
- $V[1, 1]$ for $a$: $\{ A \}$
- $V[2, 1]$ for $a$: $\{ A \}$
- $V[3, 1]$ for $b$: $\{ B \}$

### Length $l = 2$
- $V[1, 2]$ for $aa$ ($k=1$): $\{A\} \times \{A\} \implies \emptyset$
- $V[2, 2]$ for $ab$ ($k=1$): $\{A\} \times \{B\} \implies \{ S_0, S \}$

### Length $l = 3$ (Full String `aab`)
- $k=1$: $V[1, 1] \times V[2, 2] = \{A\} \times \{S_0, S\} = \{AS_0, AS\} \implies \emptyset$
- $k=2$: $V[1, 2] \times V[3, 1] = \emptyset \times \{B\} \implies \emptyset$
- Result: $V[1, 3] = \emptyset$

### CYK Table for `aab`

| Length ($l$) | $i = 1$ ($w_1 = a$) | $i = 2$ ($w_2 = a$) | $i = 3$ ($w_3 = b$) |
|:---:|:---:|:---:|:---:|
| **$l = 3$** | **$\emptyset$** | — | — |
| **$l = 2$** | $\emptyset$ | $\{ S_0, S \}$ | — |
| **$l = 1$** | $\{ A \}$ | $\{ A \}$ | $\{ B \}$ |
| **Substrings** | $w_1 = a$ | $w_2 = a$ | $w_3 = b$ |

### Result for `aab`:
$$S_0 \notin V[1, 3] = \emptyset \implies \mathbf{REJECTED}$$
*(Rejected because $N_a = 2 \ne N_b = 1$, making symmetric pairing mathematically impossible).*

---

## 7. Derivation Parse Tree Extracted from the `aabb` CYK Table

Tracing back the split choices that led to $S_0 \in V[1, 4]$:
1. $S_0 \in V[1, 4]$ originates from $k = 1$: $A \in V[1, 1]$ and $X \in V[2, 3]$ via $S_0 \to AX$.
2. $A \in V[1, 1]$ yields terminal $a$ via $A \to a$.
3. $X \in V[2, 3]$ originates from $k = 1$ of span 3 (split at global index 3): $S \in V[2, 2]$ and $B \in V[4, 1]$ via $X \to SB$.
4. $S \in V[2, 2]$ originates from $A \in V[2, 1]$ and $B \in V[3, 1]$ via $S \to AB$.
5. $A \to a$ and $B \to b$ produce the middle terminal pair $ab$.
6. $B \in V[4, 1]$ yields final terminal $b$ via $B \to b$.

```
                 S0
               /    \
              A      X
              |    /   \
              a   S     B
                 / \    |
                A   B   b
                |   |
                a   b
```
Yield: $a \cdot a \cdot b \cdot b = \text{`aabb'}$.

---

## 8. Inference & Conclusion
1. **Dynamic Programming Optimality:** The CYK algorithm systematically avoids redundant re-parsing by storing solutions to all sub-problems $w[i \dots j]$ in a 2D table, operating in $O(n^3 \cdot |R|)$ time complexity.
2. **Definitive Decision Procedure:** Unlike naive recursive parsing which can suffer from infinite loops or exponential backtracking, CYK provides a guaranteed, deterministic decision procedure for any Context-Free Language in CNF.
3. **Exact Parsing Verification:** The presence of $S_0$ in the apex cell $V[1, n]$ constitutes constructive proof of grammatical membership, while an empty apex cell provides definitive proof of non-membership.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** and select:
   `JFLAP_Practicals/12_CYK_Parsing/23_Darshan_Kankekar_CYK_13-11-2026.jff`
3. JFLAP opens the 7-production CNF grammar in the Grammar Editor.
4. **Interactive CYK Parsing for `aabb`:**
   - In the menu bar, click **Input** $\to$ **CYK Parse**.
   - Enter `aabb` into the input dialog and click **Start**.
   - Click **Step** to observe the bottom row ($l=1$) populated with $\{A\}, \{A\}, \{B\}, \{B\}$.
   - Continue clicking **Step** or **Complete** to populate rows $l=2, 3, 4$.
   - Observe the root cell at the apex containing `S0`, with the green banner indicating **String Accepted**.
   - Click on the root cell to view the corresponding binary parse tree.
5. **Testing Rejected String `aab`:**
   - Click **Input** $\to$ **CYK Parse**.
   - Enter `aab` and click **Complete**.
   - Notice that the apex cell is empty, and JFLAP displays the red **String Rejected** indicator.
