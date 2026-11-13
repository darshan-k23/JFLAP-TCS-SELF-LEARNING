# Theory of Computer Science (TCS) - JFLAP Self-Learning Assignment

**Student Name:** Darshan Kankekar  
**Roll No:** 23  
**Class / Division:** D12A  
**Institution:** Vivekanand Education Society's Institute of Technology (VESIT)  
**Academic Submission Date:** 13-11-2026  
**Naming Standard:** `23_Darshan_Kankekar_<TOPIC>_13-11-2026`

---

## Practical Index & Verification Tracker

| No. | Directory | Topic / Concept Covered | Primary JFLAP File(s) | Machine Type | Status |
|:---:|:---|:---|:---|:---:|:---:|
| 01 | `01_DFA` | Deterministic Finite Automata (Even 0s, Odd 1s) | [`23_Darshan_Kankekar_DFA_13-11-2026.jff`](01_DFA/23_Darshan_Kankekar_DFA_13-11-2026.jff) | FA | Completed (XML & Trace Validated; Screenshot Pending) |
| 02 | `02_NFA` | Non-Deterministic Finite Automata (Ends with `aa`) | [`23_Darshan_Kankekar_NFA_13-11-2026.jff`](02_NFA/23_Darshan_Kankekar_NFA_13-11-2026.jff) | FA | Completed (XML & Subset Trace Validated; Screenshot Pending) |
| 03 | `03_NFA_to_DFA` | NFA to DFA Conversion (Contains Substring `ab`) | [`23_Darshan_Kankekar_NFA_Source_13-11-2026.jff`](03_NFA_to_DFA/23_Darshan_Kankekar_NFA_Source_13-11-2026.jff), [`23_Darshan_Kankekar_DFA_Converted_13-11-2026.jff`](03_NFA_to_DFA/23_Darshan_Kankekar_DFA_Converted_13-11-2026.jff) | FA Conversion | Completed (XML & Dual Trace Validated; Screenshot Pending) |
| 04 | `04_DFA_Minimization` | DFA Minimization (Strings Ending in `1`) | [`23_Darshan_Kankekar_DFA_Original_13-11-2026.jff`](04_DFA_Minimization/23_Darshan_Kankekar_DFA_Original_13-11-2026.jff), [`23_Darshan_Kankekar_DFA_Minimized_13-11-2026.jff`](04_DFA_Minimization/23_Darshan_Kankekar_DFA_Minimized_13-11-2026.jff) | FA Minimization | Completed (XML & Dual Trace Validated; Screenshot Pending) |
| 05 | `05_RE_to_DFA` | Regular Expression to DFA (`(0+1)*101`) | [`23_Darshan_Kankekar_RE_13-11-2026.jff`](05_RE_to_DFA/23_Darshan_Kankekar_RE_13-11-2026.jff), [`23_Darshan_Kankekar_DFA_from_RE_13-11-2026.jff`](05_RE_to_DFA/23_Darshan_Kankekar_DFA_from_RE_13-11-2026.jff) | RE / FA | Completed (XML & State Trace Validated; Screenshot Pending) |
| 06 | `06_DFA_to_Grammar` | DFA to Right-Linear Regular Grammar (Even 1s) | [`23_Darshan_Kankekar_DFA_Source_13-11-2026.jff`](06_DFA_to_Grammar/23_Darshan_Kankekar_DFA_Source_13-11-2026.jff), [`23_Darshan_Kankekar_Grammar_Converted_13-11-2026.jff`](06_DFA_to_Grammar/23_Darshan_Kankekar_Grammar_Converted_13-11-2026.jff) | FA to Grammar | Completed (XML & Dual Trace Validated; Screenshot Pending) |
| 07 | `07_Mealy_Machine` | Mealy Machine (Detects Substring `101`) | [`23_Darshan_Kankekar_Mealy_13-11-2026.jff`](07_Mealy_Machine/23_Darshan_Kankekar_Mealy_13-11-2026.jff) | Mealy | Completed (XML & Transducer Output Validated; Screenshot Pending) |
| 08 | `08_Moore_Machine` | Moore Machine (Parity of `1`s with Initial Output) | [`23_Darshan_Kankekar_Moore_13-11-2026.jff`](08_Moore_Machine/23_Darshan_Kankekar_Moore_13-11-2026.jff) | Moore | Completed (XML & State Output Validated; Screenshot Pending) |
| 09 | `09_CFG` | Context-Free Grammar ($L = \{a^n b^n \mid n \ge 1\}$) | [`23_Darshan_Kankekar_CFG_13-11-2026.jff`](09_CFG/23_Darshan_Kankekar_CFG_13-11-2026.jff) | Grammar | Completed (XML & Derivations Validated; Screenshot Pending) |
| 10 | `10_PDA` | Pushdown Automata ($L = \{w\#w^R \mid w \in \{0,1\}^*\}$) | [`23_Darshan_Kankekar_PDA_13-11-2026.jff`](10_PDA/23_Darshan_Kankekar_PDA_13-11-2026.jff) | PDA | Completed (XML & Stack Trace Validated; Screenshot Pending) |
| 11 | `11_CNF_Conversion` | Chomsky Normal Form Conversion ($a^n b^n$) | [`23_Darshan_Kankekar_CFG_Original_13-11-2026.jff`](11_CNF_Conversion/23_Darshan_Kankekar_CFG_Original_13-11-2026.jff), [`23_Darshan_Kankekar_CNF_Converted_13-11-2026.jff`](11_CNF_Conversion/23_Darshan_Kankekar_CNF_Converted_13-11-2026.jff) | Grammar CNF | Completed (XML & CNF Trace Validated; Screenshot Pending) |
| 12 | `12_CYK_Parsing` | Cocke-Younger-Kasami (CYK) Dynamic Programming Parsing | [`23_Darshan_Kankekar_CYK_13-11-2026.jff`](12_CYK_Parsing/23_Darshan_Kankekar_CYK_13-11-2026.jff) | Grammar CYK | Completed (XML & CYK Table Validated; Screenshot Pending) |
| 13 | `13_LL1_Parsing` | LL(1) Predictive Parsing (Arithmetic Expressions) | [`23_Darshan_Kankekar_LL1_Grammar_13-11-2026.jff`](13_LL1_Parsing/23_Darshan_Kankekar_LL1_Grammar_13-11-2026.jff) | Grammar LL(1) | Completed (XML & Parser Stack Trace Validated; Screenshot Pending) |
| 14 | `14_Single_Tape_TM` | Single-Tape Turing Machine (Binary Incrementer) | [`23_Darshan_Kankekar_Single_Tape_TM_13-11-2026.jff`](14_Single_Tape_TM/23_Darshan_Kankekar_Single_Tape_TM_13-11-2026.jff) | Turing Machine | Completed (XML & Tape Output Validated; Screenshot Pending) |
| 15 | `15_Multi_Tape_TM` | Multi-Tape Turing Machine (Binary String Copy) | [`23_Darshan_Kankekar_Multi_Tape_TM_13-11-2026.jff`](15_Multi_Tape_TM/23_Darshan_Kankekar_Multi_Tape_TM_13-11-2026.jff) | Multi-Tape TM | Completed (XML & Dual-Tape Trace Validated; Screenshot Pending) |

---

## Directory Structure Standard

Each practical folder contains:
1. Native JFLAP file(s) (`.jff`) adhering to strict XML syntax and readable canvas layout.
2. Comprehensive practical report (`README.md`) containing:
   - Aim & Problem Statement
   - Mathematical / Formal Definition (Tuples, Alphabet, States)
   - State & Transition Explanation
   - Transition / Output Table
   - Test String Suite (Acceptance / Rejection tests with step-by-step traces)
   - Inference / Conclusion
   - JFLAP execution instructions
3. `validation_log.md` detailing every test vector, expected output, observed result, and test pass/fail status.
4. Screenshots of the machine running in JFLAP (or `SCREENSHOTS_NEEDED.md` with step-by-step GUI capture instructions if headless).

