# Practical 05: Regular Expression Representation & Conversion to DFA

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP RE File:** [`23_Darshan_Kankekar_RE_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/05_RE_to_DFA/23_Darshan_Kankekar_RE_13-11-2026.jff)  
**Converted DFA File:** [`23_Darshan_Kankekar_DFA_from_RE_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/05_RE_to_DFA/23_Darshan_Kankekar_DFA_from_RE_13-11-2026.jff)

---

## 1. Aim
To formulate a Regular Expression in JFLAP representing all binary strings ending with the substring `101`, convert this algebraic expression into an equivalent Deterministic Finite Automaton (DFA) using JFLAP's conversion facility, and empirically validate both representations.

---

## 2. Problem / Language Statement
Let the input alphabet be $\Sigma = \{0, 1\}$.  
The language $L$ is formally defined as:
$$L = \{ w \in \{0, 1\}^* \mid w \text{ ends with the specific suffix '101'} \}$$

### Formal Regular Expression
$$R = (0 + 1)^* 101$$

---

## 3. Regular Expression Syntax in JFLAP
- **Alternation / Union:** JFLAP natively uses the plus symbol `+` for union (representing logical OR, i.e., $L(r_1 + r_2) = L(r_1) \cup L(r_2)$).  
  *(Note: While POSIX regex standards use the pipe symbol `|`, JFLAP 7 and classical automata textbooks such as Hopcroft-Motwani-Ullman strictly mandate `+` as the union operator).*
- **Concatenation:** Indicated by juxtaposition without explicit symbols ($101 = 1 \cdot 0 \cdot 1$).
- **Kleene Star:** Denoted by the asterisk `*` ($L(r^*) = (L(r))^*$).
- **Grouping:** Parentheses `( )` define explicit precedence.

### Semantic Decomposition of $(0+1)^*101$
1. **$(0 + 1)^* = \Sigma^*$ (Arbitrary Binary Prefix):**  
   Generates all possible binary strings of any arbitrary length $\ge 0$ (including the empty string $\epsilon$). This ensures that the automaton permits any prefix sequence before the terminal pattern is reached.
2. **$101$ (Exact Terminal Suffix):**  
   Concatenated directly to $(0+1)^*$, enforcing that every accepted string must terminate with the exact three-symbol sequence `1`, followed by `0`, followed by `1`.

---

## 4. Formal Definition of the Converted DFA
The DFA resulting from conversion and state synthesis is defined by the 5-tuple $M = (Q, \Sigma, \delta, q_0, F)$:

1. **$Q = \{ q_0, q_1, q_2, q_3 \}$:**
   - $q_0$: Initial state; represents no progress toward the suffix `101` (or last symbol read was `0`).
   - $q_1$: Matched suffix `1` (progress length 1).
   - $q_2$: Matched suffix `10` (progress length 2).
   - $q_3$: Matched suffix `101` (progress length 3) — **Accepting state**.
2. **$\Sigma = \{ 0, 1 \}$**
3. **Start State:** $q_0$
4. **Final States:** $F = \{ q_3 \}$
5. **$\delta : Q \times \Sigma \to Q$**

---

## 5. Transition Table of Converted DFA

| Current State ($q$) | Input `0` ($\delta(q, 0)$) | Input `1` ($\delta(q, 1)$) | Semantic Meaning / Suffix Overlap |
|:---:|:---:|:---:|:---|
| $\to q_0$ | $q_0$ | $q_1$ | `0` resets progress; `1` establishes candidate suffix `1` |
| $q_1$ | $q_2$ | $q_1$ | `0` advances to suffix `10`; consecutive `1` keeps suffix `1` |
| $q_2$ | $q_0$ | $q_3$ | `0` yields `100` (resets to $q_0$); `1` completes target suffix `101` |
| $* q_3$ (Final) | $q_2$ | $q_1$ | On `0`, suffix `1010` overlaps with `10` (moves to $q_2$); on `1`, suffix `1011` overlaps with `1` (moves to $q_1$) |

---

## 6. Conversion Notes (RE $\to$ NFA $\to$ DFA)
1. **Thompson's Construction:** JFLAP internally maps sub-expressions to basic NFA fragments with $\epsilon$-transitions:
   - Alternation $(0+1)$ creates parallel branches.
   - Kleene star $(0+1)^*$ introduces looping $\epsilon$-transitions.
   - Concatenation chains the prefix automaton to the sequence of transitions matching `1`, `0`, and `1`.
2. **Subset Construction & Minimization:** Eliminating $\epsilon$-closures and determinizing the resulting state graph synthesizes the canonical 4-state DFA above.
3. **Sufficiency of 4 States:** Because the target pattern has length $m = 3$ (`101`), tracking the longest proper prefix of the pattern that matches the current suffix requires exactly $m + 1 = 4$ equivalence states.

---

## 7. Test Suite and Execution Traces

### A. Accepted Strings (Ending in `101`)

| Test String $w$ | Length | Step-by-Step DFA State Trace | Final State | In $L$? | DFA Outcome |
|:---|:---:|:---|:---:|:---:|:---:|
| `101` | 3 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3 \in F$ | **Yes** | **Accept** |
| `0101` | 4 | $q_0 \xrightarrow{0} q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3 \in F$ | **Yes** | **Accept** |
| `1101` | 4 | $q_0 \xrightarrow{1} q_1 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3 \in F$ | **Yes** | **Accept** |
| `00101` | 5 | $q_0 \xrightarrow{0} q_0 \xrightarrow{0} q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3$ | $q_3 \in F$ | **Yes** | **Accept** |

### B. Rejected Strings (Not Ending in `101`)

| Test String $w$ | Length | Step-by-Step DFA State Trace | Final State | In $L$? | DFA Outcome |
|:---|:---:|:---|:---:|:---:|:---:|
| $\epsilon$ | 0 | $q_0$ (no transitions) | $q_0 \notin F$ | **No** | **Reject** |
| `10` | 2 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2$ | $q_2 \notin F$ | **No** | **Reject** |
| `100` | 3 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{0} q_0$ | $q_0 \notin F$ | **No** | **Reject** |
| `1010` | 4 | $q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3 \xrightarrow{0} q_2$ | $q_2 \notin F$ | **No** | **Reject** |
| `111` | 3 | $q_0 \xrightarrow{1} q_1 \xrightarrow{1} q_1 \xrightarrow{1} q_1$ | $q_1 \notin F$ | **No** | **Reject** |

---

## 8. Inference & Conclusion
1. **Equivalence of Formalisms:** Kleene's Theorem establishes that regular languages can be represented equivalently via Regular Expressions, Non-Deterministic Finite Automata, and Deterministic Finite Automata.
2. **KMP String Matching Property:** The backward transitions in the DFA (such as $\delta(q_3, 0) = q_2$) demonstrate the Knuth-Morris-Pratt failure function principle, where state $q_2$ reuses the prefix `10` already matched in `1010`.
3. **Determinism:** The resulting 4-state DFA is complete, minimal, and processes each input symbol in deterministic $O(1)$ time per transition.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. **Opening the Regular Expression:**
   - Click **File** $\to$ **Open** $\to$ `JFLAP_Practicals/05_RE_to_DFA/23_Darshan_Kankekar_RE_13-11-2026.jff`.
   - The expression `(0+1)*101` will display in JFLAP's Regular Expression window.
3. **Converting RE to Automaton in JFLAP:**
   - In the RE window, click **Convert** $\to$ **Convert to NFA**.
   - Click **Do Step** or **Do All** to generate the generalized NFA graph.
   - Click **Export** to save or transition to the NFA window.
   - From the NFA window, click **Convert** $\to$ **Convert to DFA** to generate the deterministic automaton.
4. **Testing Converted DFA:**
   - Open `JFLAP_Practicals/05_RE_to_DFA/23_Darshan_Kankekar_DFA_from_RE_13-11-2026.jff`.
   - Click **Input** $\to$ **Multiple Run**.
   - Input the test strings (`101`, `0101`, `1101`, `00101`, ``, `10`, `100`, `1010`, `111`).
   - Click **Run Inputs** and verify that all results match the theoretical predictions.

