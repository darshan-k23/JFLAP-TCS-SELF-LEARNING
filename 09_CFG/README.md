# Practical 09: Context-Free Grammar (CFG) for $a^n b^n$ ($n \ge 1$)

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_CFG_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/09_CFG/23_Darshan_Kankekar_CFG_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze a Context-Free Grammar (CFG) in JFLAP generating the classic non-regular language $L = \{ a^n b^n \mid n \ge 1 \}$, demonstrate its derivation mechanisms, and verify membership on positive and negative test cases.

---

## 2. Problem / Language Statement
Let the alphabet be $\Sigma = \{a, b\}$.  
The language $L$ is formally defined as:
$$L = \{ a^n b^n \mid n \ge 1 \} = \{ ab, aabb, aaabbb, aaaabbbb, \dots \}$$
Language characteristics:
1. Every string consists of a non-empty block of $a$'s followed by a block of $b$'s.
2. The number of $a$'s strictly equals the number of $b$'s ($N_a(w) = N_b(w)$).
3. All $a$'s strictly precede all $b$'s (no $b$ appears before any $a$).
4. The empty string $\epsilon$ is excluded ($n \ge 1$).

---

## 3. Formal Definition (4-Tuple)
The Context-Free Grammar is defined by the 4-tuple $G = (V, \Sigma, R, S)$:

1. **$V = \{ S \}$ (Set of Nonterminal Variables)**
2. **$\Sigma = \{ a, b \}$ (Set of Terminal Symbols)**
3. **Start Variable:** $S \in V$
4. **$R$ (Finite Set of Production Rules):**
   - $P_1: S \to aSb$ (Recursive Production)
   - $P_2: S \to ab$ (Base / Terminal Production)

---

## 4. Mathematical Invariants & Structural Properties

### A. Why $N_a(w) = N_b(w)$ (Equal Count Invariant)
We prove by mathematical induction on the number of derivation steps $k$ that every sentential form derived from $S$ contains an equal count of $a$'s and $b$'s:
- **Base Case ($k = 1$):**  
  Applying $P_2: S \Rightarrow ab$ produces $N_a(ab) = 1$ and $N_b(ab) = 1$. Equal count holds.  
  Applying $P_1: S \Rightarrow aSb$ introduces one $a$ and one $b$. Equal count holds.
- **Inductive Step:**  
  Assume after $k$ steps the sentential form is $\alpha = a^k S b^k$ with $N_a(\alpha) = N_b(\alpha) = k$.  
  - If we apply $P_1: S \to aSb$, the new sentential form is $a^{k+1} S b^{k+1}$. Here, $N_a$ and $N_b$ both increase by exactly 1 ($k+1 = k+1$).
  - If we apply $P_2: S \to ab$, the final terminal string is $a^{k+1} b^{k+1}$. Here, both counts equal $k+1$.
- **Conclusion:** Because every production rule adds terminals in strictly balanced pairs $(a, b)$, it is mathematically impossible for any derivation to generate an unequal number of $a$'s and $b$'s.

### B. Why All $a$ Symbols Precede All $b$ Symbols (Strict Ordering Invariant)
- In production $P_1: S \to aSb$, the terminal $a$ is prepended strictly to the **left** of variable $S$, while the terminal $b$ is appended strictly to the **right** of $S$.
- In production $P_2: S \to ab$, the terminal $a$ is positioned to the left of terminal $b$.
- Therefore, in every intermediate sentential form:
  $$\alpha_k = \underbrace{a a \dots a}_{k \text{ times}} \; S \; \underbrace{b b \dots b}_{k \text{ times}} = a^k S b^k$$
  All generated $a$'s reside in the left prefix, and all generated $b$'s reside in the right suffix.
- When $S$ is finally replaced by $ab$, the resulting string is:
  $$w = a^k (ab) b^k = a^{k+1} b^{k+1} = a^n b^n \quad (\text{where } n = k+1)$$
- At no point can a $b$ cross over to the left of an $a$. This guarantees that strings with interleaved symbols (such as `abab`) can never be generated.

### C. Exclusion of the Empty String ($n \ge 1$)
The grammar contains no $\epsilon$-production ($S \to \epsilon$). The minimal possible derivation is the single-step application of $P_2$:
$$S \Rightarrow ab \quad (n = 1)$$
Thus, the minimum word length is 2, ensuring that $\epsilon \notin L(G)$.

---

## 5. Derivations & Parse Tree Analysis

### A. Leftmost Derivation for String $w = \text{`aaabbb'}$ ($n = 3$)
Because every intermediate sentential form contains exactly one nonterminal ($S$), the leftmost and rightmost derivations are identical:

$$\begin{aligned}
S &\Rightarrow aSb && \text{(Apply } P_1: S \to aSb \text{)} \\
  &\Rightarrow aaSbb && \text{(Apply } P_1: S \to aSb \text{)} \\
  &\Rightarrow aaabbb && \text{(Apply } P_2: S \to ab \text{)}
\end{aligned}$$

Total derivation steps: $n = 3$ steps.

### B. Parse Tree Representation for `aaabbb`

```
          S
        / | \
       a  S  b
        / | \
       a  S  b
        /   \
       a     b
```

Reading the leaves from left to right yields the terminal yield:
$$\text{Yield} = a \cdot a \cdot a \cdot b \cdot b \cdot b = a^3 b^3 = \text{`aaabbb'}$$

---

## 6. Test Suite and Membership Verification

| Test String $w$ | Length | Structural Assessment | Derivation Sequence in $G$ | In $L$? | JFLAP Grammar Parser Result |
|:---|:---:|:---|:---|:---:|:---:|
| `ab` | 2 | $n=1$: 1 'a', 1 'b' | $S \Rightarrow ab$ | **Yes** | **ACCEPT** |
| `aabb` | 4 | $n=2$: 2 'a's, 2 'b's | $S \Rightarrow aSb \Rightarrow aabb$ | **Yes** | **ACCEPT** |
| `aaabbb` | 6 | $n=3$: 3 'a's, 3 'b's | $S \Rightarrow aSb \Rightarrow aaSbb \Rightarrow aaabbb$ | **Yes** | **ACCEPT** |
| `aaaabbbb` | 8 | $n=4$: 4 'a's, 4 'b's | $S \Rightarrow aSb \Rightarrow aaSbb \Rightarrow aaaSbbb \Rightarrow aaaabbbb$ | **Yes** | **ACCEPT** |
| $\epsilon$ | 0 | Length 0 ($n < 1$) | No $\epsilon$-rule for $S$ | **No** | **REJECT** |
| `a` | 1 | Odd length ($N_a=1, N_b=0$) | No single-terminal production | **No** | **REJECT** |
| `b` | 1 | Odd length ($N_a=0, N_b=1$) | No single-terminal production | **No** | **REJECT** |
| `aab` | 3 | $N_a=2 \ne N_b=1$ | Unbalanced symbol count | **No** | **REJECT** |
| `abb` | 3 | $N_a=1 \ne N_b=2$ | Unbalanced symbol count | **No** | **REJECT** |
| `abab` | 4 | Balanced ($N_a=N_b=2$), but order violated | Production rules enforce nested $a^k S b^k$ | **No** | **REJECT** |
| `aaabb` | 5 | $N_a=3 \ne N_b=2$ | Unbalanced symbol count | **No** | **REJECT** |

---

## 7. Theoretical Significance: Why $a^n b^n$ Requires a CFG
1. **Failure of Regular Automata:**  
   By the Pumping Lemma for Regular Languages, if $L = \{a^n b^n\}$ were regular, there would exist a pumping length $p$ such that $s = a^p b^p = xyz$ with $|xy| \le p$ and $|y| \ge 1$. Pumping $y$ ($xy^i z$) would alter the count of $a$'s without changing $b$'s, producing strings where $N_a \ne N_b \notin L$. Thus, no finite automaton can recognize $L$ because a DFA has finite memory and cannot count arbitrarily large $n$.
2. **Context-Free Power:**  
   A Context-Free Grammar naturally overcomes this limitation through recursive self-nesting ($S \to aSb$), which synchronizes the generation of terminals on opposite sides of the nonterminal, effectively providing an unbounded memory mechanism.

---

## 8. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** and select:
   `JFLAP_Practicals/09_CFG/23_Darshan_Kankekar_CFG_13-11-2026.jff`
3. JFLAP opens the file in the **Grammar Editor** showing the two productions:
   - `S -> aSb`
   - `S -> ab`
4. **Interactive Step-by-Step Derivation:**
   - Click **Input** $\to$ **User Control Parse**.
   - Enter `aaabbb` and click **Start**.
   - Step through the substitution by clicking on variable $S$ and choosing `S -> aSb`, then `S -> aSb`, and finally `S -> ab`.
   - Observe the derivation table and corresponding graphical Parse Tree.
5. **Batch String Testing:**
   - Click **Input** $\to$ **Multiple Run** (or **Brute Force Parse**).
   - Enter all test strings: `ab`, `aabb`, `aaabbb`, `aaaabbbb`, ``, `a`, `b`, `aab`, `abb`, `abab`, `aaabb`.
   - Click **Run Inputs** and observe the green **Accept** and red **Reject** results.
