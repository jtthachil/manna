# 🎯 The "Hard" MCQ & Aptitude Bank (Round 1 Decider)

This file contains **advanced** and **tricky** questions that filter candidates.

---

## 🧠 Section A: Advanced Aptitude (The "Math" Part)

**1. Work & Wages**
A can do a work in 10 days, B in 15 days. They work together for 5 days. The rest is done by C in 2 days. If they get paid $450 for the whole job, how should they divide it?
*   **Logic:** Money is divided by **Work Done**, not days.
*   A's 5 day work = 5/10 = 1/2.
*   B's 5 day work = 5/15 = 1/3.
*   Remaining = 1 - (1/2 + 1/3) = 1 - 5/6 = 1/6. (This is C's work).
*   Ratio A:B:C = 1/2 : 1/3 : 1/6 = **3 : 2 : 1**.
*   A gets: 3/6 * 450 = **225**.

**2. Pipes & Cisterns (Negative Work)**
Pipe A fills in 10h. Pipe B fills in 12h. Pipe C empties in 20h. All opened at once. Time?
*   Net Rate = (1/10 + 1/12) - 1/20
*   LCM(60) -> 6 + 5 - 3 = 8 units/hr.
*   Total Time = 60 / 8 = **7.5 hours**.

**3. Clocks**
At what time between 4 and 5 o'clock will hands be at right angles (90 degrees)?
*   Formula: `theta = |30H - 5.5M|`.
*   90 = |30(4) - 5.5M| -> 90 = |120 - 5.5M|.
*   Case 1: 120 - 5.5M = 90 -> 5.5M = 30 -> M = 5.45 min.
*   Case 2: 5.5M - 120 = 90 -> 5.5M = 210 -> M = 38.18 min.
*   **Two answers:** 4:05 and 4:38.

---

## 💻 Section B: Core CS (The "Technical" Part)

### Operating Systems (Process Management)
**4. What is "Thrashing"?**
*   a) Hard disk crash.
*   b) Excessive paging/swapping causing CPU low utilization. **(Correct)**
    *   *Why?* OS spends more time moving RAM to Disk than running the app.
*   c) CPU overheating.

**5. Which scheduling algorithm suffers from "Convoy Effect"?**
*   a) Round Robin.
*   b) FCFS (First Come First Serve). **(Correct)**
    *   *Why?* One CPU-heavy process blocks all small IO processes behind it.

**6. What is a Semaphore?**
*   **Answer:** An integer variable used to solve the critical section problem (synchronization tool).
    *   `wait()` decrements. `signal()` increments.

### DBMS (Deep Dive)
**7. Difference between `clustered` and `non-clustered` index?**
*   **Clustered:** Sorts the actual data rows on disk (Only 1 per table - usually PK).
*   **Non-Clustered:** Separate structure pointing to data rows (Can have multiple).

**8. What is the "Wal (Write Ahead Log)" in Postgres?**
*   **Answer:** Changes are written to a Log file *before* being written to the actual database file. ensures durability if power fails.

**9. SQL Join Trick: Table A has rows `[1, 1, 1]`. Table B has rows `[1, 1]`. How many rows in `A INNER JOIN B`?**
*   **Answer:** 3 * 2 = **6 rows**. (It's a cartesian product of matches).

---

## 🐍 Section C: Python & Patterns (Language Specific)

**10. What does `*args` and `**kwargs` do?**
*   `*args`: Passes variable number of non-keyword arguments (Tuple).
*   `**kwargs`: Passes variable keyword arguments (Action Dictionary).

**11. Output of:**
```python
l = [1, 2, 3]
print(l[-1])
print(l[::-1])
```
*   **Answer:** `3` and `[3, 2, 1]`.

**12. Is Python interpreted or compiled?**
*   **Answer:** Both. Source `.py` -> Bytecode `.pyc` -> Interpreted by PVM (Python Virtual Machine).

---

## 🌐 Section D: Networks & Web

**13. HTTPS Handshake (Simplified)**
1.  Client says Hello (Cipher suites).
2.  Server sends Certificate (Public Key).
3.  Client verifies Cert.
4.  Client creates Session Key, encrypts with Public Key.
5.  Server decrypts with Private Key.
6.  Secure tunnel established!

**14. GET vs POST?**
*   **GET:** Data in URL, cached, limited length.
*   **POST:** Data in Body, secure, no length limit.
