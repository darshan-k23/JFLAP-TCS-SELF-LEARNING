# Practical 02: Non-Deterministic Finite Automaton (NFA)

**Course:** Theory of Computer Science (TCS)  
**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Division:** D12A  
**Institution:** VESIT  
**Date:** 13-11-2026  
**JFLAP File:** [`23_Darshan_Kankekar_NFA_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/02_NFA/23_Darshan_Kankekar_NFA_13-11-2026.jff)

---

## 1. Aim
To design, implement, and formally analyze a Non-Deterministic Finite Automaton (NFA) in JFLAP that recognizes the regular language of all binary strings over $\Sigma = \{a, b\}$ ending with the specific substring `aa`.

---

## 2. Problem / Language Statement
Let the input alphabet be $\Sigma = \{a, b\}$.  
The regular language $L$ is defined as:
$$L = \{ w \in \{a, b\}^* \mid w \text{ ends with the substring } 'aa' \}$$
Equivalent Regular Expression:
$$R = (a + b)^* aa$$

---

## 3. Formal Definition (5-Tuple)
The NFA is formally defined by the 5-tuple $M = (Q, \Sigma, \delta, q_0, F)$:

1. **$Q$ (Set of Finite States):**  
   $$Q = \{ q_0, q_1, q_2 \}$$

2. **$\Sigma$ (Input Alphabet):**  
   $$\Sigma = \{ a, b \}$$

3. **$q_0$ (Initial / Start State):**  
   $$q_0 \in Q$$

4. **$F$ (Set of Final / Accepting States):**  
   $$F = \{ q_2 \}$$

5. **$\delta$ (Transition Function):**  
   In an NFA, the transition function maps each state and input symbol to a *subset* (element of the power set) of states:
   $$\delta : Q \times \Sigma \to 2^Q$$

---

## 4. State Semantics & Nondeterministic Branching

### State Roles
- **$q_0$ (Prefix Consumer & Guess Origin):** Reads any arbitrary prefix $(a+b)^*$. It loops on both $a$ and $b$. When an $a$ is encountered, the machine nondeterministically splits:
  1. One thread remains at $q_0$, assuming more symbols will follow before the final `aa`.
  2. A parallel thread branches to $q_1$, making an educated nondeterministic guess that this $a$ is the penultimate symbol (the first $a$ of the ending `aa`).
- **$q_1$ (Penultimate Symbol Verification):** Holds the hypothesis that the string has one remaining symbol, which must be $a$. If an $a$ arrives, it advances to $q_2$. If a $b$ arrives, the transition is undefined ($\emptyset$); this branch immediately terminates (pruned / dies).
- **$q_2$ (Accepting State):** Represents successful recognition of the terminal suffix `aa`. It has no outgoing transitions ($\delta(q_2, a) = \emptyset, \delta(q_2, b) = \emptyset$). If further input arrives while at $q_2$, the branch terminates.

### Mathematical Acceptance Criterion
For an NFA, a string $w = c_1 c_2 \dots c_n$ is accepted if and only if there exists at least one sequence of states $r_0, r_1, \dots, r_n$ such that:
1. $r_0 = q_0$
2. $r_{i} \in \delta(r_{i-1}, c_i)$ for all $1 \le i \le n$
3. $r_n \in F = \{ q_2 \}$

Unsuccessful guesses simply stall and do not cause rejection if an accepting path exists.

---

## 5. Transition Table
The table below specifies the power-set mappings for each state and input symbol:

| State | Input $a$ ($\delta(q, a)$) | Input $b$ ($\delta(q, b)$) | Nondeterministic Characteristic |
|:---|:---:|:---:|:---|
| $\to q_0$ (Initial) | $\{ q_0, q_1 \}$ | $\{ q_0 \}$ | **Branching on $a$:** loops to $q_0$ and guesses into $q_1$ |
| $q_1$ | $\{ q_2 \}$ | $\emptyset$ | Moves to $q_2$ on second $a$; dies on $b$ |
| $* q_2$ (Final) | $\emptyset$ | $\emptyset$ | Terminal state; no outgoing transitions |

---

## 6. Test Suite and Execution Traces

### A. Accepted Strings (Ending with `aa`)

| Test String $w$ | Length | Active Subset State Trace $\hat{\delta}(\{q_0\}, w)$ | Accepting Path (Witness) | Final Active States | In $L$? | JFLAP Outcome |
|:---|:---:|:---|:---|:---:|:---:|:---:|
| `aa` | 2 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $q_0 \xrightarrow{a} q_1 \xrightarrow{a} q_2$ | $\{q_0, q_1, q_2\}$ | **Yes** | **Accept** |
| `aaa` | 3 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $q_0 \xrightarrow{a} q_0 \xrightarrow{a} q_1 \xrightarrow{a} q_2$ | $\{q_0, q_1, q_2\}$ | **Yes** | **Accept** |
| `baa` | 3 | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $q_0 \xrightarrow{b} q_0 \xrightarrow{a} q_1 \xrightarrow{a} q_2$ | $\{q_0, q_1, q_2\}$ | **Yes** | **Accept** |
| `bbaa` | 4 | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $q_0 \xrightarrow{b} q_0 \xrightarrow{b} q_0 \xrightarrow{a} q_1 \xrightarrow{a} q_2$ | $\{q_0, q_1, q_2\}$ | **Yes** | **Accept** |
| `abaa` | 4 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\}$ | $q_0 \xrightarrow{a} q_0 \xrightarrow{b} q_0 \xrightarrow{a} q_1 \xrightarrow{a} q_2$ | $\{q_0, q_1, q_2\}$ | **Yes** | **Accept** |

### B. Rejected Strings (Not Ending with `aa`)

| Test String $w$ | Length | Active Subset State Trace $\hat{\delta}(\{q_0\}, w)$ | Why Branch Failed | Final Active States | In $L$? | JFLAP Outcome |
|:---|:---:|:---|:---|:---:|:---:|:---:|
| $\epsilon$ | 0 | $\{q_0\}$ | Initial state $q_0 \notin F$ | $\{q_0\}$ | **No** | **Reject** |
| `a` | 1 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\}$ | Only 1 'a'; neither $q_0$ nor $q_1$ is final | $\{q_0, q_1\}$ | **No** | **Reject** |
| `ab` | 2 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0\}$ | Guess in $q_1$ died on $b$; leaves only $q_0$ | $\{q_0\}$ | **No** | **Reject** |
| `aba` | 3 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{b} \{q_0\} \xrightarrow{a} \{q_0, q_1\}$ | Ends with single $a$; $q_2$ not reached | $\{q_0, q_1\}$ | **No** | **Reject** |
| `aab` | 3 | $\{q_0\} \xrightarrow{a} \{q_0, q_1\} \xrightarrow{a} \{q_0, q_1, q_2\} \xrightarrow{b} \{q_0\}$ | $q_1$ and $q_2$ both die on $b$; leaves only $q_0$ | $\{q_0\}$ | **No** | **Reject** |
| `bb` | 2 | $\{q_0\} \xrightarrow{b} \{q_0\} \xrightarrow{b} \{q_0\}$ | No $a$ read; loops in $q_0$ | $\{q_0\}$ | **No** | **Reject** |

---

## 7. Inference & Conclusion
1. **Compactness of NFA:** The NFA models the language $(a+b)^*aa$ in only **3 states**, compared to a minimal DFA which requires more transition bookkeeping for patterns like `aab` resetting to prefix states.
2. **Role of Nondeterminism:** The branching transition $\delta(q_0, a) = \{q_0, q_1\}$ eliminates the need for backtracking. The automaton simultaneously explores both possibilities (prefix continuation and suffix initiation).
3. **Dead End Handling:** By leaving $\delta(q_1, b) = \emptyset$ and $\delta(q_2, \Sigma) = \emptyset$, invalid branches are eliminated without contaminating valid concurrent execution paths.

---

## 8. Steps to Open and Run in JFLAP
1. Launch JFLAP:
   ```powershell
   java -jar JFLAP7.1.jar
   ```
2. Click **File** $\to$ **Open** and choose:
   `JFLAP_Practicals/02_NFA/23_Darshan_Kankekar_NFA_13-11-2026.jff`
3. The 3-state horizontal automaton canvas will appear with initial state `q0` and final state `q2`.
4. Observe the branching:
   - Notice the two outgoing arrows from `q0` labeled `a`: self-loop `q0 -> q0` and forward arrow `q0 -> q1`.
5. Step-by-step visualization of NFA branches:
   - Click **Input** $\to$ **Step with State...**
   - Enter `abaa` and click **OK**.
   - Click **Step**: at step 1, observe two green highlights indicating simultaneous exploration of `q0` and `q1`.
   - Continue clicking **Step** until the final step where one branch successfully reaches `q2`, showing **Accept**.
6. Batch testing via Multiple Run:
   - Click **Input** $\to$ **Multiple Run**.
   - Input all strings: `aa`, `aaa`, `baa`, `bbaa`, `abaa`, ``, `a`, `ab`, `aba`, `aab`, `bb`.
   - Click **Run Inputs** to confirm that the first 5 are **Accept** and the remaining 6 are **Reject**.

