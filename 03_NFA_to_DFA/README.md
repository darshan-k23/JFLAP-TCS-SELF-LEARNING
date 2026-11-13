# Practical 03: NFA to DFA Conversion using Subset Construction

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**Source NFA File:** [`23_Darshan_Kankekar_NFA_Source_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/03_NFA_to_DFA/23_Darshan_Kankekar_NFA_Source_13-11-2026.jff)  
**Converted DFA File:** [`23_Darshan_Kankekar_DFA_Converted_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/03_NFA_to_DFA/23_Darshan_Kankekar_DFA_Converted_13-11-2026.jff)

---

## 1. Aim
To construct an NFA in JFLAP that recognizes all strings over $\Sigma = \{a, b\}$ containing the substring `ab`, perform subset construction (powerset algorithm) to synthesize an equivalent DFA, and verify their operational equivalence on comprehensive test suites.

---

## 2. Problem / Language Statement
Let the alphabet be $\Sigma = \{a, b\}$.  
The language $L$ is formally defined as:
$$L = \{ w \in \{a, b\}^* \mid w \text{ contains 'ab' as a contiguous substring} \}$$
Equivalent Regular Expression:
$$R = (a + b)^* ab (a + b)^*$$

---

## 3. Formal Definitions

### A. Source Non-Deterministic Finite Automaton ($M_N$)
Defined by the 5-tuple $M_N = (Q_N, \Sigma, \delta_N, q_0, F_N)$:
- $Q_N = \{ q_0, q_1, q_2 \}$
- $\Sigma = \{ a, b \}$
- Start State: $q_0$
- Final States: $F_N = \{ q_2 \}$
- $\delta_N : Q_N \times \Sigma \to 2^{Q_N}$

### B. Converted Deterministic Finite Automaton ($M_D$)
Defined by the 5-tuple $M_D = (Q_D, \Sigma, \delta_D, q_0', F_D)$:
- $Q_D = \{ A, B, C, D \}$ where each DFA state corresponds to a subset of $Q_N$:
  - $A = \{ q_0 \}$
  - $B = \{ q_0, q_1 \}$
  - $C = \{ q_0, q_2 \}$
  - $D = \{ q_0, q_1, q_2 \}$
- $\Sigma = \{ a, b \}$
- Start State: $q_0' = A = \{ q_0 \}$
- Final States: $F_D = \{ C, D \}$ (all reachable subsets containing $q_2$)
- $\delta_D : Q_D \times \Sigma \to Q_D$

---

## 4. Subset Construction Derivation

In subset construction, each state in the DFA represents a set of states from the NFA. For any subset $S \subseteq Q_N$ and input symbol $\sigma \in \Sigma$:
$$\delta_D(S, \sigma) = \bigcup_{q \in S} \delta_N(q, \sigma)$$

### Step-by-Step Derivation of Reachable States:

1. **Initial State:**  
   $S_0 = \{ q_0 \} = A$.
   - $\delta_D(A, a) = \delta_N(q_0, a) = \{ q_0, q_1 \} \implies \mathbf{B}$
   - $\delta_D(A, b) = \delta_N(q_0, b) = \{ q_0 \} \implies \mathbf{A}$

2. **From State $B = \{ q_0, q_1 \}$:**
   - $\delta_D(B, a) = \delta_N(q_0, a) \cup \delta_N(q_1, a) = \{ q_0, q_1 \} \cup \emptyset = \{ q_0, q_1 \} \implies \mathbf{B}$
   - $\delta_D(B, b) = \delta_N(q_0, b) \cup \delta_N(q_1, b) = \{ q_0 \} \cup \{ q_2 \} = \{ q_0, q_2 \} \implies \mathbf{C}$

3. **From State $C = \{ q_0, q_2 \}$:**
   - $\delta_D(C, a) = \delta_N(q_0, a) \cup \delta_N(q_2, a) = \{ q_0, q_1 \} \cup \{ q_2 \} = \{ q_0, q_1, q_2 \} \implies \mathbf{D}$
   - $\delta_D(C, b) = \delta_N(q_0, b) \cup \delta_N(q_2, b) = \{ q_0 \} \cup \{ q_2 \} = \{ q_0, q_2 \} \implies \mathbf{C}$

4. **From State $D = \{ q_0, q_1, q_2 \}$:**
   - $\delta_D(D, a) = \delta_N(q_0, a) \cup \delta_N(q_1, a) \cup \delta_N(q_2, a) = \{ q_0, q_1 \} \cup \emptyset \cup \{ q_2 \} = \{ q_0, q_1, q_2 \} \implies \mathbf{D}$
   - $\delta_D(D, b) = \delta_N(q_0, b) \cup \delta_N(q_1, b) \cup \delta_N(q_2, b) = \{ q_0 \} \cup \{ q_2 \} \cup \{ q_2 \} = \{ q_0, q_2 \} \implies \mathbf{C}$

No additional states are generated. The total reachable state space consists of exactly 4 states out of the potential $2^3 = 8$ subsets of the power set.

### Subset-Construction Table

| DFA State | NFA Subset Equivalent | Input $a$ Transition ($\delta_D$) | Input $b$ Transition ($\delta_D$) | Acceptance Status |
|:---:|:---:|:---:|:---:|:---:|
| $\to A$ | $\{ q_0 \}$ | $B$ | $A$ | Non-final |
| $B$ | $\{ q_0, q_1 \}$ | $B$ | $C$ | Non-final |
| $* C$ | $\{ q_0, q_2 \}$ | $D$ | $C$ | **Final** ($q_2 \in C$) |
| $* D$ | $\{ q_0, q_1, q_2 \}$ | $D$ | $C$ | **Final** ($q_2 \in D$) |

---

## 5. Acceptance Condition in the Converted DFA

An NFA accepts a string if **at least one** computation path reaches an accepting state:
$$w \in L(M_N) \iff \hat{\delta}_N(q_0, w) \cap F_N \ne \emptyset$$
Because each state $S \in Q_D$ represents the exact set of states $\hat{\delta}_N(q_0, w)$ that the NFA could simultaneously occupy after reading $w$, the DFA must accept $w$ whenever that set intersects $F_N$:
$$F_D = \{ S \in Q_D \mid S \cap F_N \ne \emptyset \}$$
Since $F_N = \{ q_2 \}$, any DFA state whose constituent set contains $q_2$ must be an accepting state. Therefore:
- $q_2 \in C = \{ q_0, q_2 \} \implies C \in F_D$
- $q_2 \in D = \{ q_0, q_1, q_2 \} \implies D \in F_D$

---

## 6. Transition Tables Comparison

### Source NFA ($\delta_N$)
| State | Input $a$ | Input $b$ |
|:---:|:---:|:---:|
| $\to q_0$ | $\{ q_0, q_1 \}$ | $\{ q_0 \}$ |
| $q_1$ | $\emptyset$ | $\{ q_2 \}$ |
| $* q_2$ | $\{ q_2 \}$ | $\{ q_2 \}$ |

### Converted DFA ($\delta_D$)
| State | Input $a$ | Input $b$ |
|:---:|:---:|:---:|
| $\to A$ ($\{q_0\}$) | $B$ ($\{q_0, q_1\}$) | $A$ ($\{q_0\}$) |
| $B$ ($\{q_0, q_1\}$) | $B$ ($\{q_0, q_1\}$) | $C$ ($\{q_0, q_2\}$) |
| $* C$ ($\{q_0, q_2\}$) | $D$ ($\{q_0, q_1, q_2\}$) | $C$ ($\{q_0, q_2\}$) |
| $* D$ ($\{q_0, q_1, q_2\}$) | $D$ ($\{q_0, q_1, q_2\}$) | $C$ ($\{q_0, q_2\}$) |

---

## 7. Comparative Test Suite & Execution Traces

Both machines were simulated across all required test strings:

| Test String $w$ | NFA Active State Sequence | NFA Result | DFA State Sequence | DFA Result | Agreement |
|:---|:---|:---:|:---|:---:|:---:|
| `ab` | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0, q_2\}$ | **Accept** | $A \xrightarrow{a} B \xrightarrow{b} C$ | **Accept** | **MATCH** |
| `aab` | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0, q_2\}$ | **Accept** | $A \xrightarrow{a} B \xrightarrow{a} B \xrightarrow{b} C$ | **Accept** | **MATCH** |
| `bab` | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0, q_2\}$ | **Accept** | $A \xrightarrow{b} A \xrightarrow{a} B \xrightarrow{b} C$ | **Accept** | **MATCH** |
| `abab` | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0, q_2\} \xrightarrow{a} \{q_0, q_1, q_2\} \xrightarrow{b} \{q_0, q_2\}$ | **Accept** | $A \xrightarrow{a} B \xrightarrow{b} C \xrightarrow{a} D \xrightarrow{b} C$ | **Accept** | **MATCH** |
| `bbaab` | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0, q_2\}$ | **Accept** | $A \xrightarrow{b} A \xrightarrow{b} A \xrightarrow{a} B \xrightarrow{a} B \xrightarrow{b} C$ | **Accept** | **MATCH** |
| $\epsilon$ | $\{q_0\}$ | **Reject** | $A$ | **Reject** | **MATCH** |
| `a` | $\{q_0\} \xrightarrow{a} \{q_0, q_1\}$ | **Reject** | $A \xrightarrow{a} B$ | **Reject** | **MATCH** |
| `b` | $\{q_0\} \xrightarrow{b} \{q_0\}$ | **Reject** | $A \xrightarrow{b} A$ | **Reject** | **MATCH** |
| `ba` | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\}$ | **Reject** | $A \xrightarrow{b} A \xrightarrow{a} B$ | **Reject** | **MATCH** |
| `aaa` | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1\}$ | **Reject** | $A \xrightarrow{a} B \xrightarrow{a} B \xrightarrow{a} B$ | **Reject** | **MATCH** |
| `bbb` | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{b} \{q_0\}$ | **Reject** | $A \xrightarrow{b} A \xrightarrow{b} A \xrightarrow{b} A$ | **Reject** | **MATCH** |

---

## 8. Inference & Conclusion
1. **Equivalence of Expressive Power:** The subset construction algorithm effectively converted the 3-state NFA into an equivalent 4-state DFA without altering the recognized language $L = (a+b)^*ab(a+b)^*$.
2. **Reachable vs Potential States:** Although the power set has $2^3 = 8$ states, only 4 states ($\{q_0\}$, $\{q_0, q_1\}$, $\{q_0, q_2\}$, and $\{q_0, q_1, q_2\}$) are reachable from the start state.
3. **Memory Tracking in DFA:** State $C$ and state $D$ represent the trap memory that substring `ab` has been observed. State $D$ also remembers that the most recent symbol was `a`, allowing detection of overlapping or subsequent patterns without leaving the accepting class.

---

## 9. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. **Testing Source NFA:**
   - Open `JFLAP_Practicals/03_NFA_to_DFA/23_Darshan_Kankekar_NFA_Source_13-11-2026.jff`.
   - Go to **Input** $\to$ **Multiple Run** and test all strings.
3. **Performing Interactive Conversion in JFLAP:**
   - With the NFA open, click **Convert** $\to$ **Convert to DFA**.
   - JFLAP opens the interactive conversion module.
   - Click **Complete** to auto-generate all DFA states and transitions, or click state-by-state to build the powerset manually.
   - Click **Done?** $\to$ **Export** to save the generated DFA.
4. **Testing Converted DFA:**
   - Open `JFLAP_Practicals/03_NFA_to_DFA/23_Darshan_Kankekar_DFA_Converted_13-11-2026.jff`.
   - Go to **Input** $\to$ **Multiple Run** and run the identical test strings.
   - Compare the results to verify complete agreement.

