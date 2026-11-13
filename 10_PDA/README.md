# Practical 10: Deterministic Pushdown Automaton (PDA) for $w\#w^R$

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_PDA_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/10_PDA/23_Darshan_Kankekar_PDA_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze a Deterministic Pushdown Automaton (PDA) in JFLAP that recognizes the marked palindrome language $L = \{ w\#w^R \mid w \in \{0, 1\}^* \}$ using a push-match two-phase stack architecture, and verify acceptance by final state on comprehensive positive and negative test strings.

---

## 2. Problem / Language Statement
Let the input alphabet be $\Sigma = \{0, 1, \#\}$.  
The language $L$ is formally defined as:
$$L = \{ w\#w^R \mid w \in \{0, 1\}^* \}$$
where:
- $w$ is any arbitrary binary string over $\{0, 1\}^*$.
- $w^R$ denotes the string reversal of $w$.
- `#` is a special boundary marker separating the first half from its reverse.
- The special base case $w = \epsilon$ yields the minimal string `#`.

Because the center of the string is explicitly marked by `#`, this language is **Deterministic Context-Free (DCFL)** and does not require nondeterministic midpoint guessing.

---

## 3. Formal Definition (7-Tuple)
The Pushdown Automaton is formally defined by the 7-tuple $M = (Q, \Sigma, \Gamma, \delta, q_0, Z, F)$:

1. **$Q = \{ q_0, q_1, q_2 \}$ (Set of States):**
   - $q_0$: Initial state; Push Phase (stores $w$).
   - $q_1$: Match Phase (compares remaining input against popped stack symbols).
   - $q_2$: Final state; Acceptance Phase (reached only when $w^R$ is fully matched and input is exhausted).
2. **$\Sigma = \{ 0, 1, \# \}$ (Input Alphabet)**
3. **$\Gamma = \{ 0, 1, Z \}$ (Stack Alphabet)**
4. **$q_0 \in Q$ (Start State)**
5. **$Z \in \Gamma$ (Initial Bottom-of-Stack Marker)**
6. **$F = \{ q_2 \}$ (Set of Accepting States)**
7. **$\delta : Q \times (\Sigma \cup \{\epsilon\}) \times \Gamma \to 2^{Q \times \Gamma^*}$ (Transition Function)**

---

## 4. Operational Architecture: What the Stack Represents

### Phase 1: Push Phase ($q_0$)
- While reading $w$ before the marker `#`, every `0` or `1` is pushed onto the stack.
- **LIFO Inversion Property:**  
  Suppose $w = c_1 c_2 \dots c_k$. As each symbol $c_i$ is pushed, it sits on top of $c_{i-1}$. Upon reaching `#`, the stack configuration (from top to bottom) is:
  $$\text{Stack} = [c_k, c_{k-1}, \dots, c_1, Z]$$
  By virtue of Last-In-First-Out stack mechanics, the symbols are arranged precisely in the reverse order $w^R$!

### Phase 2: Midpoint Boundary Marker (`#`)
- When the machine scans `#`, it switches from $q_0$ to $q_1$.
- The stack contents are completely preserved (peeking at the stack top $Z, 0,$ or $1$ and pushing it back).
- If $w = \epsilon$, the input is simply `#`. The stack top is $Z$, and the transition moves directly from $q_0$ to $q_1$ with $Z$ on the stack.

### Phase 3: Match Phase ($q_1$)
- The remaining input symbols are read and compared symbol-by-symbol against the stack top:
  - Input `0` requires top of stack `0` $\to$ pop `0`.
  - Input `1` requires top of stack `1` $\to$ pop `1`.
- If an input symbol does not match the stack top, or if the stack runs out before the input ends, no transition exists; the machine halts immediately and rejects.

### Phase 4: Acceptance Phase ($q_2$)
- Once all matching symbols have been popped, the bottom-of-stack marker $Z$ is exposed.
- An $\epsilon$-transition moves the machine from $q_1$ to final state $q_2$ by reading $\epsilon$, popping $Z$, and pushing $Z$.
- **Strict Input Consumption:** JFLAP accepts by final state if and only if the machine reaches state $q_2$ **AND the input tape is completely empty**. If unconsumed symbols remain (such as in `#0`), the machine crashes without accepting.

---

## 5. Transition Table

| State ($q$) | Input ($\sigma$) | Pop Symbol | Next State | Push String | Functional Phase |
|:---:|:---:|:---:|:---:|:---:|:---|
| $q_0$ | `0` | $Z$ | $q_0$ | `0Z` | Push `0` onto initial empty stack |
| $q_0$ | `1` | $Z$ | $q_0$ | `1Z` | Push `1` onto initial empty stack |
| $q_0$ | `0` | `0` | $q_0$ | `00` | Push `0` over existing `0` |
| $q_0$ | `1` | `0` | $q_0$ | `10` | Push `1` over existing `0` |
| $q_0$ | `0` | `1` | $q_0$ | `01` | Push `0` over existing `1` |
| $q_0$ | `1` | `1` | $q_0$ | `11` | Push `1` over existing `1` |
| $q_0$ | `#` | $Z$ | $q_1$ | $Z$ | Boundary marker when $w = \epsilon$; transition to match phase |
| $q_0$ | `#` | `0` | $q_1$ | `0` | Boundary marker when $w$ ends in `0`; transition to match phase |
| $q_0$ | `#` | `1` | $q_1$ | `1` | Boundary marker when $w$ ends in `1`; transition to match phase |
| $q_1$ | `0` | `0` | $q_1$ | $\epsilon$ | Match `0`: pop `0` from stack |
| $q_1$ | `1` | `1` | $q_1$ | $\epsilon$ | Match `1`: pop `1` from stack |
| $q_1$ | $\epsilon$ | $Z$ | $q_2$ | $Z$ | Empty stack verified: move to final accepting state |

---

## 6. Test Suite and Execution Traces (Instantaneous Descriptions)

An Instantaneous Description (ID) is denoted by $(q, u, \alpha)$ where $q$ is the current state, $u$ is unconsumed input, and $\alpha$ is stack contents (top at left).

### A. Accepted Strings ($w \in L$)

| Test String | Step-by-Step ID Sequence | End State | Stack Top | Input Left | Outcome |
|:---|:---|:---:|:---:|:---:|:---:|
| `#` | $(q_0, \text{`#'}, Z) \vdash (q_1, \epsilon, Z) \vdash (q_2, \epsilon, Z)$ | $q_2 \in F$ | $Z$ | None | **ACCEPT** |
| `0#0` | $(q_0, \text{`0#0'}, Z) \vdash (q_0, \text{`#0'}, 0Z) \vdash (q_1, \text{`0'}, 0Z) \vdash (q_1, \epsilon, Z) \vdash (q_2, \epsilon, Z)$ | $q_2 \in F$ | $Z$ | None | **ACCEPT** |
| `1#1` | $(q_0, \text{`1#1'}, Z) \vdash (q_0, \text{`#1'}, 1Z) \vdash (q_1, \text{`1'}, 1Z) \vdash (q_1, \epsilon, Z) \vdash (q_2, \epsilon, Z)$ | $q_2 \in F$ | $Z$ | None | **ACCEPT** |
| `01#10` | $(q_0, \text{`01#10'}, Z) \vdash (q_0, \text{`1#10'}, 0Z) \vdash (q_0, \text{`#10'}, 10Z) \vdash (q_1, \text{`10'}, 10Z) \vdash (q_1, \text{`0'}, 0Z) \vdash (q_1, \epsilon, Z) \vdash (q_2, \epsilon, Z)$ | $q_2 \in F$ | $Z$ | None | **ACCEPT** |
| `101#101` | $(q_0, \text{`101#101'}, Z) \vdash^* (q_0, \text{`#101'}, 101Z) \vdash (q_1, \text{`101'}, 101Z) \vdash^* (q_1, \epsilon, Z) \vdash (q_2, \epsilon, Z)$ | $q_2 \in F$ | $Z$ | None | **ACCEPT** |

### B. Rejected Strings ($w \notin L$)

| Test String | Why the PDA Halts / Rejects | Halting Configuration | Outcome |
|:---|:---|:---:|:---:|
| `01#01` | Mismatch: input `0` read in $q_1$, but stack top is `1`. No transition. | $(q_1, \text{`01'}, 10Z)$ (stuck in $q_1$) | **REJECT** |
| `10#1` | Length/symbol mismatch: input `1` read in $q_1$, but stack top is `0`. | $(q_1, \text{`1'}, 01Z)$ (stuck in $q_1$) | **REJECT** |
| `0#` | Input ends prematurely: input exhausted, but stack still holds `0Z`. $\epsilon$-rule requires $Z$. | $(q_1, \epsilon, 0Z)$ (stuck in $q_1$) | **REJECT** |
| `#0` | Extraneous symbol: moves to $q_2$ via $\epsilon$, but unconsumed `0` remains on tape. | $(q_2, \text{`0'}, Z)$ (unread input) | **REJECT** |
| `001#10` | Suffix too short: after matching `10`, input is empty, but stack holds `0Z`. | $(q_1, \epsilon, 0Z)$ (stuck in $q_1$) | **REJECT** |
| `11#00` | Suffix mismatch: input `0` read in $q_1$, but stack top is `1`. | $(q_1, \text{`00'}, 11Z)$ (stuck in $q_1$) | **REJECT** |

---

## 7. Inference & Conclusion
1. **Deterministic Context-Free Language:** Because the marker `#` signals the exact inflection point, at no point does the machine face multiple concurrent choices. The PDA is strictly deterministic (DPDA).
2. **Reversal Verification via LIFO:** The pushdown stack is mathematically equivalent to a reversal operator because elements are popped in reverse order of their arrival ($(\text{push } w) \to (\text{pop } w^R)$).
3. **Acceptance Criteria:** The machine guarantees full string consumption and exact symmetry verification, strictly rejecting both truncated and over-extended inputs.

---

## 8. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** $\to$ select:
   `JFLAP_Practicals/10_PDA/23_Darshan_Kankekar_PDA_13-11-2026.jff`
3. JFLAP automatically detects the type as **Pushdown Automaton**.
4. Observe the planar canvas:
   - Three states $q_0, q_1, q_2$.
   - Transitions display triple labels formatted as `read, pop ; push` (e.g., `0, Z ; 0Z` or `#, 0 ; 0`).
5. **Interactive Single Run with Stack Visualizer:**
   - Click **Input** $\to$ **Step with State...**
   - Enter `01#10` and click **OK**.
   - Observe the live stack window alongside the canvas as symbols are pushed in $q_0$ and popped in $q_1$.
6. **Multiple Run Batch Testing:**
   - Click **Input** $\to$ **Multiple Run**.
   - Input the test suite (`#`, `0#0`, `1#1`, `01#10`, `101#101`, `01#01`, `10#1`, `0#`, `#0`, `001#10`, `11#00`).
   - Click **Run Inputs** to verify that the first 5 inputs show **Accept** and the remaining 6 show **Reject**.
