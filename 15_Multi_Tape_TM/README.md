# Practical 15: Two-Tape Deterministic Turing Machine for String Copying

**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Class:** D12A  
**Branch:** Information Technology / Computer Engineering  
**Institution:** Vivekanand Education Society's Institute of Technology (VESIT)  
**Date:** 13-11-2026  
**Subject:** Theory of Computer Science (TCS)  
**Tool:** JFLAP 7.0 / 7.1  

---

## 1. Aim & Objective

To design, implement, and analyze a **Deterministic Two-Tape Turing Machine (2-Tape DTM)** over the binary alphabet $\Sigma = \{0, 1\}$ that copies an arbitrary input string $w$ from Tape 1 onto Tape 2, such that upon halting:
1. **Tape 1** remains completely unmodified (containing the original input string $w$).
2. **Tape 2** contains an exact, identical duplicate of $w$.
3. Both read/write heads move synchronously from the initial cell to the blank marker following the string, halting cleanly in an accepting state without appending spurious symbols.
4. To evaluate the computational speedup of multi-tape Turing machines over standard single-tape Turing machines ($O(n)$ vs. $\Omega(n^2)$).

---

## 2. Theoretical Background: Multi-Tape Turing Machines

A **$k$-tape Turing Machine** is an extension of the standard single-tape Turing machine having $k$ distinct two-way infinite tapes, each equipped with its own independently movable read/write head, controlled by a single shared finite-state control.

### 2.1 Formal 7-Tuple Definition

A deterministic 2-tape Turing machine $M$ is defined as a 7-tuple:
$$M = (Q, \Sigma, \Gamma, \delta, q_0, B, F)$$

Where:
- $Q = \{q_{\text{copy}}, q_{\text{halt}}\}$ is the finite set of control states.
- $\Sigma = \{0, 1\}$ is the input alphabet.
- $\Gamma = \{0, 1, \square\}$ is the tape alphabet, where $\square$ (or $B$) denotes the blank symbol.
- $q_0 = q_{\text{copy}}$ is the start / initial state.
- $B = \square$ is the designated blank symbol.
- $F = \{q_{\text{halt}}\}$ is the set of final / accepting halting states.
- $\delta$ is the deterministic transition function mapping:
  $$\delta: Q \times \Gamma^2 \to Q \times \Gamma^2 \times \{L, R, S\}^2$$
  Here, for current state $q$ and scanned tape symbols $(X_1, X_2)$, $\delta(q, (X_1, X_2)) = (p, (Y_1, Y_2), (D_1, D_2))$, where $p$ is the next state, $Y_1, Y_2$ are symbols overwritten on Tape 1 and Tape 2, and $D_1, D_2 \in \{L, R, S\}$ specify head motions (Left, Right, Stay).

---

## 3. Tape Roles & Synchronous Architecture

| Tape | Initial Content | Head Start Position | Operational Role | Final Content |
| :--- | :--- | :--- | :--- | :--- |
| **Tape 1** | Input string $w \in \{0, 1\}^*$ followed by $\square^\omega$ | Cell 0 (first input character) | **Source / Read-Only Tape**: Scans input symbols sequentially. Rewrites the identical symbol to preserve the source input. | Original input string $w$ |
| **Tape 2** | Blank tape ($\square^\omega$) | Cell 0 (first blank cell) | **Target / Write Tape**: Emits the exact bit scanned on Tape 1. | Exact copy of $w$ |

### 3.1 Synchronous Lockstep Movement
- When Head 1 reads `0` on Tape 1 (while Head 2 scans blank $\square$ on Tape 2), the transition writes `0` to Tape 1, writes `0` to Tape 2, and shifts both heads **Right** ($R, R$).
- When Head 1 reads `1` on Tape 1 (while Head 2 scans blank $\square$ on Tape 2), the transition writes `1` to Tape 1, writes `1` to Tape 2, and shifts both heads **Right** ($R, R$).
- When Head 1 reaches the end-of-string blank $\square$ on Tape 1 (while Head 2 scans blank $\square$ on Tape 2), the transition writes $\square$ to both tapes, executes **Stay** ($S, S$), and enters accepting state $q_{\text{halt}}$.

```mermaid
flowchart LR
    q0((q_copy<br/>[Init]))
    q1(((q_halt<br/>[Final])))

    q0 -->|"T1: [0 -> 0, R]<br/>T2: [□ -> 0, R]"| q0
    q0 -->|"T1: [1 -> 1, R]<br/>T2: [□ -> 1, R]"| q0
    q0 -->|"T1: [□ -> □, S]<br/>T2: [□ -> □, S]"| q1

    classDef initial fill:#dbeafe,stroke:#1d4ed8,stroke-width:2px;
    classDef final fill:#dcfce7,stroke:#15803d,stroke-width:3px;
    class q0 initial;
    class q1 final;
```

---

## 4. Transition Table

| Current State $q$ | Tape 1 Read ($r_1$) | Tape 2 Read ($r_2$) | Next State $p$ | Tape 1 Write ($w_1$) | Tape 1 Move ($m_1$) | Tape 2 Write ($w_2$) | Tape 2 Move ($m_2$) | Explanation |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| $q_{\text{copy}}$ | `0` | $\square$ | $q_{\text{copy}}$ | `0` | `R` | `0` | `R` | Preserves `0` on T1, copies `0` to T2, advances both heads right. |
| $q_{\text{copy}}$ | `1` | $\square$ | $q_{\text{copy}}$ | `1` | `R` | `1` | `R` | Preserves `1` on T1, copies `1` to T2, advances both heads right. |
| $q_{\text{copy}}$ | $\square$ | $\square$ | $q_{\text{halt}}$ | $\square$ | `S` | $\square$ | `S` | Detects end of input; halts without corrupting tape boundaries. |

---

## 5. Computational Complexity: 1-Tape vs. 2-Tape TM

A major foundational theorem in automata theory (Hartmanis and Stearns, 1965) proves that multi-tape Turing machines provide a quadratic time speedup for many computational tasks:

$$\text{Time}_{\text{2-Tape}}(n) = \Theta(n) \quad \text{vs.} \quad \text{Time}_{\text{1-Tape}}(n) = \Omega(n^2)$$

### Detailed Comparison:
1. **2-Tape DTM (Our Implementation):**
   - The machine reads symbol $w[i]$ on Tape 1 and directly copies it to cell $i$ on Tape 2 in **one single step**.
   - For an input string of length $n$, the machine executes exactly $n$ copy steps plus $1$ halting transition.
   - Total Steps: $T(n) = n + 1 = O(n)$ (linear time).
2. **Standard 1-Tape TM (Alternative Baseline):**
   - On a single tape, to copy $w = w_1 w_2 \dots w_n$ to form $w\#w$, the single head must:
     1. Read $w_1$, mark it (e.g., replace with $\hat{w}_1$).
     2. Traverse across the remaining $(n-1)$ symbols of $w$ and the delimiter to the target area ($O(n)$ steps).
     3. Write $w_1$.
     4. Traverse back across the tape to find the next unmarked symbol ($O(n)$ steps).
   - Repeating this back-and-forth round-trip for all $n$ symbols yields:
     $$T(n) = \sum_{i=1}^n O(n) = \Omega(n^2)$$
   - Therefore, the 2-tape architecture eliminates $O(n^2)$ head travel overhead.

---

## 6. Instantaneous Description (ID) & Trace Walkthroughs

In a multi-tape Turing machine, an instantaneous description (ID) is represented as:
$$q : u_1 [a_1] v_1 \mid u_2 [a_2] v_2$$
where $q$ is the current state, and $u_i [a_i] v_i$ denotes that the head on Tape $i$ is currently scanning symbol $a_i$, with string $u_i$ to its left and string $v_i$ to its right.

### 6.1 Trace 1: Input $w = 10110$ (Length $n = 5$)

- **Initial Configuration ($t=0$):**
  - State: $q_{\text{copy}}$
  - Tape 1: `[1] 0 1 1 0 □` (Head 1 at index 0)
  - Tape 2: `[□] □ □ □ □ □` (Head 2 at index 0)
  - Transition: $\delta(q_{\text{copy}}, (1, \square)) = (q_{\text{copy}}, (1, 1), (R, R))$
- **Step 1 ($t=1$):**
  - State: $q_{\text{copy}}$
  - Tape 1: `1 [0] 1 1 0 □` (Head 1 at index 1)
  - Tape 2: `1 [□] □ □ □ □` (Head 2 at index 1)
  - Transition: $\delta(q_{\text{copy}}, (0, \square)) = (q_{\text{copy}}, (0, 0), (R, R))$
- **Step 2 ($t=2$):**
  - State: $q_{\text{copy}}$
  - Tape 1: `1 0 [1] 1 0 □` (Head 1 at index 2)
  - Tape 2: `1 0 [□] □ □ □` (Head 2 at index 2)
  - Transition: $\delta(q_{\text{copy}}, (1, \square)) = (q_{\text{copy}}, (1, 1), (R, R))$
- **Step 3 ($t=3$):**
  - State: $q_{\text{copy}}$
  - Tape 1: `1 0 1 [1] 0 □` (Head 1 at index 3)
  - Tape 2: `1 0 1 [□] □ □` (Head 2 at index 3)
  - Transition: $\delta(q_{\text{copy}}, (1, \square)) = (q_{\text{copy}}, (1, 1), (R, R))$
- **Step 4 ($t=4$):**
  - State: $q_{\text{copy}}$
  - Tape 1: `1 0 1 1 [0] □` (Head 1 at index 4)
  - Tape 2: `1 0 1 1 [□] □` (Head 2 at index 4)
  - Transition: $\delta(q_{\text{copy}}, (0, \square)) = (q_{\text{copy}}, (0, 0), (R, R))$
- **Step 5 ($t=5$):**
  - State: $q_{\text{copy}}$
  - Tape 1: `1 0 1 1 0 [□]` (Head 1 at index 5)
  - Tape 2: `1 0 1 1 0 [□]` (Head 2 at index 5)
  - Transition: $\delta(q_{\text{copy}}, (\square, \square)) = (q_{\text{halt}}, (\square, \square), (S, S))$
- **Step 6 ($t=6$ - Halting Configuration):**
  - State: $q_{\text{halt}}$ (Accepting state reached)
  - Tape 1: `10110` (Preserved intact)
  - Tape 2: `10110` (Exact replica)
  - Total Steps: **6** ($n+1$).

---

### 6.2 Trace 2: Input $w = 0001$ (Length $n = 4$)

| Step | State | Tape 1 Head | Tape 1 Scanned | Tape 2 Head | Tape 2 Scanned | Tape 1 Action | Tape 2 Action | Next State |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | $q_{\text{copy}}$ | 0 | `0` | 0 | $\square$ | Write `0`, Move `R` | Write `0`, Move `R` | $q_{\text{copy}}$ |
| 1 | $q_{\text{copy}}$ | 1 | `0` | 1 | $\square$ | Write `0`, Move `R` | Write `0`, Move `R` | $q_{\text{copy}}$ |
| 2 | $q_{\text{copy}}$ | 2 | `0` | 2 | $\square$ | Write `0`, Move `R` | Write `0`, Move `R` | $q_{\text{copy}}$ |
| 3 | $q_{\text{copy}}$ | 3 | `1` | 3 | $\square$ | Write `1`, Move `R` | Write `1`, Move `R` | $q_{\text{copy}}$ |
| 4 | $q_{\text{copy}}$ | 4 | $\square$ | 4 | $\square$ | Write $\square$, Stay `S` | Write $\square$, Stay `S` | $q_{\text{halt}}$ |
| **Final** | $q_{\text{halt}}$ | 4 | $\square$ | 4 | $\square$ | **HALT & ACCEPT** | Tape 1: `0001` | Tape 2: `0001` |

---

## 7. Complete Test Suite & Experimental Verification

| Test Input $w$ | Length $n$ | Final Tape 1 Content | Final Tape 2 Content | Identical? | Total Steps ($n+1$) | JFLAP Status |
| :--- | :---: | :--- | :--- | :---: | :---: | :---: |
| `0` | 1 | `0` | `0` | Yes | 2 | **Accepted / Halted** |
| `1` | 1 | `1` | `1` | Yes | 2 | **Accepted / Halted** |
| `10110` | 5 | `10110` | `10110` | Yes | 6 | **Accepted / Halted** |
| `0001` | 4 | `0001` | `0001` | Yes | 5 | **Accepted / Halted** |
| `111000` | 6 | `111000` | `111000` | Yes | 7 | **Accepted / Halted** |
| $\epsilon$ (Empty String) | 0 | $\epsilon$ ($\square$) | $\epsilon$ ($\square$) | Yes | 1 | **Accepted / Halted** |

---

## 8. JFLAP Multi-Tape Native XML Schema

In JFLAP, multi-tape Turing machines specify the `<tapes>k</tapes>` tag directly under `<structure>`. Each `<transition>` contains indexed sub-elements specifying the operation on each individual tape:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<structure>
	<type>turing</type>
	<tapes>2</tapes>
	<automaton>
		<state id="0" name="q_copy">
			<x>160.0</x>
			<y>180.0</y>
			<initial/>
		</state>
		<state id="1" name="q_halt">
			<x>380.0</x>
			<y>180.0</y>
			<final/>
		</state>
		<!-- Transition for symbol 0 -->
		<transition>
			<from>0</from>
			<to>0</to>
			<read tape="1">0</read>
			<write tape="1">0</write>
			<move tape="1">R</move>
			<read tape="2"/>
			<write tape="2">0</write>
			<move tape="2">R</move>
		</transition>
		<!-- Transition for symbol 1 -->
		<transition>
			<from>0</from>
			<to>0</to>
			<read tape="1">1</read>
			<write tape="1">1</write>
			<move tape="1">R</move>
			<read tape="2"/>
			<write tape="2">1</write>
			<move tape="2">R</move>
		</transition>
		<!-- Transition on end-of-input blank -->
		<transition>
			<from>0</from>
			<to>1</to>
			<read tape="1"/>
			<write tape="1"/>
			<move tape="1">S</move>
			<read tape="2"/>
			<write tape="2"/>
			<move tape="2">S</move>
		</transition>
	</automaton>
</structure>
```

---

## 9. How to Execute in JFLAP

1. Launch JFLAP:
   ```bash
   java -jar JFLAP.jar
   ```
2. Click **File $\to$ Open** and navigate to:
   [`23_Darshan_Kankekar_Multi_Tape_TM_13-11-2026.jff`](file:///c:/Users/Swapnaja/OneDrive/Desktop/JFLAP%20TCS%20SELF%20LEARNING%20DARSHAN%20KANKEKAR%20D12A%2023/JFLAP_Practicals/15_Multi_Tape_TM/23_Darshan_Kankekar_Multi_Tape_TM_13-11-2026.jff).
3. Notice that JFLAP automatically initializes the canvas with **2 Tapes**. The transitions appear as:
   $$0, \square; 0, 0, R, R$$
   $$1, \square; 1, 1, R, R$$
   $$\square, \square; \square, \square, S, S$$
4. **Step-by-Step Execution:**
   - Go to **Input $\to$ Step with State...**
   - In the input dialog, enter the input string on **Tape 1** (e.g. `10110`), and leave **Tape 2** empty.
   - Click **Step** to observe both tape heads advancing in perfect synchrony, populating Tape 2 while preserving Tape 1.
5. **Multiple Run Verification:**
   - Go to **Input $\to$ Multiple Run**.
   - Enter test inputs (`0`, `1`, `10110`, `0001`, `111000`, and blank).
   - Click **Run Inputs** to verify that all strings reach `q_halt` and yield identical outputs on Tape 1 and Tape 2.

---

## 10. Key Inferences & Observations

1. **Non-Destructive Data Replication:** By designing the transition on Tape 1 to write the identical symbol it just read ($0 \to 0$ and $1 \to 1$), the machine guarantees that the source data is preserved completely intact.
2. **Synchronous Execution:** Multi-tape Turing machines allow simultaneous read and write across independent physical media. Synchronous movement ($R, R$) eliminates any buffering or indexing registers.
3. **Linear Time Efficiency:** A multi-tape Turing machine performs string replication in strictly $O(n)$ steps ($n+1$ operations). This proves Church-Turing equivalence with respect to computability (any multi-tape TM can be simulated by a single-tape TM), but demonstrates a substantial polynomial separation in computational time complexity ($O(n)$ vs. $\Omega(n^2)$).

