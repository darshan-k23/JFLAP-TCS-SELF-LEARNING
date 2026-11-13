# Practical 06: Conversion of DFA to Right-Linear Regular Grammar

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**Source DFA File:** [`23_Darshan_Kankekar_DFA_Source_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/06_DFA_to_Grammar/23_Darshan_Kankekar_DFA_Source_13-11-2026.jff)  
**Converted Grammar File:** [`23_Darshan_Kankekar_Grammar_Converted_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/06_DFA_to_Grammar/23_Darshan_Kankekar_Grammar_Converted_13-11-2026.jff)

---

## 1. Aim
To construct a Deterministic Finite Automaton (DFA) in JFLAP accepting binary strings containing an even number of `1`s, convert it into an equivalent Right-Linear Regular Grammar, and rigorously verify that both mathematical formalisms generate identical languages.

---

## 2. Problem / Language Statement
Let the alphabet be $\Sigma = \{0, 1\}$.  
The language $L$ is formally defined as:
$$L = \{ w \in \{0, 1\}^* \mid N_1(w) \equiv 0 \pmod 2 \}$$
where $N_1(w)$ denotes the total occurrences of symbol `1` in $w$.

---

## 3. Formal Definitions

### A. Source Deterministic Finite Automaton ($M$)
Defined by the 5-tuple $M = (Q, \Sigma, \delta, q_E, F)$:
- $Q = \{ q_E, q_O \}$
  - $q_E$: Even number of `1`s (Initial & Final)
  - $q_O$: Odd number of `1`s (Non-final)
- $\Sigma = \{ 0, 1 \}$
- Start State: $q_E$
- Final States: $F = \{ q_E \}$
- $\delta : Q \times \Sigma \to Q$

### B. Converted Right-Linear Regular Grammar ($G$)
Defined by the 4-tuple $G = (V, \Sigma, R, S)$:
- $V = \{ S, A \}$ (Variables / Nonterminals)
- $\Sigma = \{ 0, 1 \}$ (Terminals)
- Start Variable: $S$
- $R$ (Finite Set of Production Rules):
  $$S \to 0S \mid 1A \mid \epsilon$$
  $$A \to 0A \mid 1S$$

---

## 4. State-to-Nonterminal Mapping & Conversion Principles

### Mapping Rules:
1. **States to Variables ($Q \to V$):**  
   Every state in the DFA corresponds to a unique nonterminal variable:
   - $q_E \mapsto S$ (Start symbol, tracking even parity of `1`s)
   - $q_O \mapsto A$ (Tracking odd parity of `1`s)

2. **Transitions to Right-Linear Productions:**  
   For every DFA transition $\delta(p, \sigma) = q$ (where $p, q \in Q$ and $\sigma \in \Sigma$), construct a right-linear production rule:
   $$A_p \to \sigma A_q$$
   - $\delta(q_E, 0) = q_E \implies S \to 0S$
   - $\delta(q_E, 1) = q_O \implies S \to 1A$
   - $\delta(q_O, 0) = q_O \implies A \to 0A$
   - $\delta(q_O, 1) = q_E \implies A \to 1S$

3. **Final States to Epsilon ($\epsilon$) Productions:**  
   For every accepting state $q \in F$, construct an erasing / termination rule:
   $$A_q \to \epsilon$$
   - $q_E \in F \implies S \to \epsilon$  
     *(In JFLAP XML, represented by an empty right-hand side tag `<right/>`).*
   - $q_O \notin F \implies$ No $\epsilon$ production is generated for $A$.

### Why Final States Require Epsilon Productions
In a right-linear grammar, every intermediate sentential form has the canonical structure:
$$w A_q \quad \text{where } w \in \Sigma^* \text{ and } A_q \in V$$
The prefix $w$ represents the terminal string scanned so far, while $A_q$ represents the current state of the automaton.
- To produce a purely terminal string $w$, the trailing nonterminal $A_q$ must be eliminated.
- Allowing $A_q \to \epsilon$ permits the derivation to terminate cleanly:
  $$w A_q \Rightarrow w$$
- Because the automaton accepts $w$ if and only if it halts in an accepting state ($q \in F$), the grammar must allow termination **only** when the trailing nonterminal corresponds to an accepting state.
- If non-final state $q_O$ were given an $\epsilon$ rule ($A \to \epsilon$), strings with an odd number of `1`s (such as `1` or `10`) could prematurely terminate and be accepted, destroying correctness. Hence, **only final states contribute $\epsilon$ productions**.

---

## 5. Transition & Production Tables Comparison

### DFA Transition Table ($\delta$)
| Current State | Input `0` | Input `1` | Parity Status |
|:---:|:---:|:---:|:---|
| $\to * q_E$ (Initial & Final) | $q_E$ | $q_O$ | Even number of `1`s |
| $q_O$ | $q_O$ | $q_E$ | Odd number of `1`s |

### Grammar Production Set ($R$)
| Nonterminal | Production Rule | Originating DFA Transition / Condition |
|:---:|:---:|:---|
| **$S$** | $S \to 0S$ | Loop on `0` from state $q_E$ ($\delta(q_E, 0) = q_E$) |
| | $S \to 1A$ | Transition on `1` from state $q_E$ to $q_O$ ($\delta(q_E, 1) = q_O$) |
| | $S \to \epsilon$ | Termination rule because $q_E \in F$ |
| **$A$** | $A \to 0A$ | Loop on `0` from state $q_O$ ($\delta(q_O, 0) = q_O$) |
| | $A \to 1S$ | Transition on `1` from state $q_O$ to $q_E$ ($\delta(q_O, 1) = q_E$) |

---

## 6. Comparative Test Suite & Execution Traces

Both the DFA and Grammar were evaluated against all test vectors:

| Test String $w$ | $N_1(w)$ | DFA State Transition Trace | DFA Result | Grammar Derivation Sequence | Grammar Result | Agreement |
|:---|:---:|:---|:---:|:---|:---:|:---:|
| $\epsilon$ | 0 (Even) | $q_E$ | **Accept** | $S \Rightarrow \epsilon$ | **Accept** | **MATCH** |
| `0` | 0 (Even) | $q_E \xrightarrow{0} q_E$ | **Accept** | $S \Rightarrow 0S \Rightarrow 0$ | **Accept** | **MATCH** |
| `11` | 2 (Even) | $q_E \xrightarrow{1} q_O \xrightarrow{1} q_E$ | **Accept** | $S \Rightarrow 1A \Rightarrow 11S \Rightarrow 11$ | **Accept** | **MATCH** |
| `1010` | 2 (Even) | $q_E \xrightarrow{1} q_O \xrightarrow{0} q_O \xrightarrow{1} q_E \xrightarrow{0} q_E$ | **Accept** | $S \Rightarrow 1A \Rightarrow 10A \Rightarrow 101S \Rightarrow 1010S \Rightarrow 1010$ | **Accept** | **MATCH** |
| `001100` | 2 (Even) | $q_E \xrightarrow{0} q_E \xrightarrow{0} q_E \xrightarrow{1} q_O \xrightarrow{1} q_E \xrightarrow{0} q_E \xrightarrow{0} q_E$ | **Accept** | $S \Rightarrow 0S \Rightarrow 00S \Rightarrow 001A \Rightarrow 0011S \Rightarrow 00110S \Rightarrow 001100S \Rightarrow 001100$ | **Accept** | **MATCH** |
| `1` | 1 (Odd) | $q_E \xrightarrow{1} q_O$ | **Reject** | $S \Rightarrow 1A$ (Stuck: $A$ cannot erase) | **Reject** | **MATCH** |
| `10` | 1 (Odd) | $q_E \xrightarrow{1} q_O \xrightarrow{0} q_O$ | **Reject** | $S \Rightarrow 1A \Rightarrow 10A$ (Stuck) | **Reject** | **MATCH** |
| `111` | 3 (Odd) | $q_E \xrightarrow{1} q_O \xrightarrow{1} q_E \xrightarrow{1} q_O$ | **Reject** | $S \Rightarrow 1A \Rightarrow 11S \Rightarrow 111A$ (Stuck) | **Reject** | **MATCH** |
| `010` | 1 (Odd) | $q_E \xrightarrow{0} q_E \xrightarrow{1} q_O \xrightarrow{0} q_O$ | **Reject** | $S \Rightarrow 0S \Rightarrow 01A \Rightarrow 010A$ (Stuck) | **Reject** | **MATCH** |

---

## 7. Inference & Conclusion
1. **Isomorphism of Automata and Right-Linear Grammars:** Every regular language accepted by a DFA can be generated by a Type-3 right-linear grammar through direct 1-to-1 mapping of states to nonterminals and transitions to productions.
2. **Deterministic Derivations:** Because the original automaton is a DFA, every nonterminal has at most one production starting with any terminal symbol $\sigma \in \Sigma$, guaranteeing a deterministic, unambiguous leftmost derivation for every valid string.
3. **Role of $\epsilon$-Erasure:** Epsilon productions serve exclusively as termination operators for accepting configurations, enforcing strict language containment $L(G) = L(M)$.

---

## 8. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. **Testing Source DFA:**
   - Open `JFLAP_Practicals/06_DFA_to_Grammar/23_Darshan_Kankekar_DFA_Source_13-11-2026.jff`.
   - Go to **Input** $\to$ **Multiple Run** to test all strings.
3. **Interactive Conversion in JFLAP:**
   - With the DFA open, click **Convert** $\to$ **Convert to Grammar**.
   - The conversion pane opens showing the DFA on the left and grammar table on the right.
   - Click **Show All** to automatically generate all 5 productions ($S \to 0S, S \to 1A, S \to \lambda, A \to 0A, A \to 1S$).
   - Click **Export** to transfer to the Grammar editing module.
4. **Testing Converted Grammar:**
   - Open `JFLAP_Practicals/06_DFA_to_Grammar/23_Darshan_Kankekar_Grammar_Converted_13-11-2026.jff`.
   - Click **Input** $\to$ **Brute Force Parse** or **Multiple Run**.
   - Enter test inputs (`11`, `1010`, `001100`, `1`, `10`, etc.) and observe the derivations and acceptance trees.

