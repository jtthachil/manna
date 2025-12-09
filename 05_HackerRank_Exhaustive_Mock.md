# 🔥 HackerRank Exhaustive Mock Test (Hard Mode - Expanded)

**Total Time:** 120 Minutes
**Goal:** This isn't just a test; it's a marathon. If you can pass this, the real interview will feel easy.

---

## 📝 Section 1: Coding Challenge (60 Minutes)

*Solve these efficiently. Brute force will TLE (Time Limit Exceeded).*

### Problem 1: The "Payment Gateway" Throttle (Rate Limiter)
**Scenario:**
You are building the "CoFee" app backend. To prevent spam, we allow a user to perform an action only if they haven't done it in the last 10 seconds. Given a stream of requests `[time, user_id]`, return `True` (processed) or `False` (dropped).

**Input:**
```python
requests = [[1, 101], [2, 102], [5, 101], [15, 101]]
limit = 10
```
**Output:** `[True, True, False, True]`
*Explanation: User 101 requests at t=1 (Allowed). User 101 requests again at t=5 (Dropped, only 4s passed). User 101 requests at t=15 (Allowed, 14s passed since t=1).*

<details>
<summary>Hint</summary>
Use a **HashMap** `{user_id: last_request_time}`. logic is simple O(1) lookup.
</details>

### Problem 2: The "School Bus" Route (Tree Diameter)
**Scenario:**
A school has classrooms connected like a generic tree (not necessarily binary). The principle wants to find the **longest path** between any two classrooms to plan the bus pickup time estimate. (This is the "Diameter of a Tree").

**Input:** Adjacency List `[[0,1], [1,2], [2,3], [1,4]]`
**Output:** `3` (Path: 3 -> 2 -> 1 -> 4).

<details>
<summary>Hint</summary>
Run **DFS** to find the farthest node from arbitrary node X. Let that be Y. Run DFS from Y to find the farthest node Z. Dist(Y, Z) is the diameter.
</details>

### Problem 3: The "Stock Trader" (Dynamic Programming)
**Scenario:**
You have prediction data for a stock price for the next N days. You can complete at most **two transactions**. Find max profit.

**Input:** `prices = [3,3,5,0,0,3,1,4]`
**Output:** `6` (Buy at 3, Sell at 5. Buy at 0, Sell at 4).

<details>
<summary>Hint</summary>
Hard DP. Maintain four variables: `buy1`, `sell1`, `buy2`, `sell2`.
</details>

---

## 🧠 Section 2: Technical MCQs (30 Minutes)

### Subsection A: Algorithms & Data Structures
1.  **What is the worst-case complexity of a Hash Map look up?**
    *   a) O(1)
    *   b) O(log n)
    *   c) O(n) **(Correct - Collision chains)**
    *   d) O(n log n)
2.  **Which sorting algorithm is NOT stable?**
    *   a) Merge Sort
    *   b) Insertion Sort
    *   c) Quick Sort **(Correct)**
    *   d) Bubble Sort
3.  **In a Min-Heap, the parent is always \_\_\_\_ than its children.**
    *   a) Greater
    *   b) Smaller or Equal **(Correct)**
    *   c) Equal
    *   d) Unrelated

### Subsection B: DBMS & SQL
4.  **What happens if you run `DELETE FROM table;` without a WHERE clause?**
    *   a) Error
    *   b) Deletes first row
    *   c) Deletes all rows, can be rolled back **(Correct)**
    *   d) Deletes table structure
5.  **Which Isolation Level prevents "Non-Repeatable Reads" but allows "Phantom Reads"?**
    *   a) Read Committed
    *   b) Repeatable Read **(Correct)**
    *   c) Serializable
    *   d) Read Uncommitted
6.  **Query to find duplicate names in a table `Students`:**
    *   `SELECT name, COUNT(*) FROM Students GROUP BY name HAVING COUNT(*) > 1`

### Subsection C: Operating Systems & Networks
7.  **What is a "Zombie Process"?**
    *   a) A process running forever.
    *   b) A process that has finished execution but still has an entry in the process table. **(Correct)**
    *   c) A virus.
8.  **Difference between process context switch and thread context switch?**
    *   *Process switching is expensive (virtual memory reload). Thread switching is cheap.*
9.  **HTTP Status Code 503 means?**
    *   a) Not Found
    *   b) Service Unavailable **(Correct)**
    *   c) Bad Gateway
    *   d) Unauthorized

---

## 🧩 Section 3: Aptitude & Logic (15 Minutes)

**Q1. Clock Angle**
What is the angle between hour and minute hands at 3:30?
*   *Formula:* `| (30*H) - (5.5*M) |`
*   `| (30*3) - (5.5*30) |` = `| 90 - 165 |` = **75 degrees**.

**Q2. Pipes and Cisterns**
Pipe A fills tank in 2 hours, Pipe B leaks it empty in 3 hours. If both open, when will it fill?
*   Net rate = (1/2) - (1/3) = 1/6 tank per hour.
*   Time = **6 hours**.

**Q3. Coding Decoding**
If `APPLE` is coded as `BQQMF`, what is `GRAPE`?
*   Logic: +1 shift. A->B, P->Q.
*   `H` `S` `B` `Q` `F`.

---

## 🎨 Section 4: System Design "Whiteboard" (15 Minutes)

**Scenario:** Design a **"Flash Sale" System** (Like Amazon Big Billion Day).
*Wait, why is this hard?*
*   10,000 items, 1 Million users clicking "Buy" at 12:00:00 PM.

**Key Challenges to Address:**
1.  **Race Condition:** Two users buying the last iPhone.
    *   *Sol:* Database **Row Locking** (Pessimistic) or Redis `DECR` (Atomic decrement).
2.  **Server Crash:** Traffic spike kills backend.
    *   *Sol:* **Rate Limiting** (Queue users in a Waiting Room / Virtual Queue).
3.  **Frontend Lag:**
    *   *Sol:* **CDN** for assets. Static page for the landing.

**Drawing Task:**
Sketch: `User` -> `Load Balancer` -> `Redis Queue` -> `Worker Service` -> `DB`.

---

## 🗣️ Section 5: Behavioral Round (HR)

*Write down your answers.*
1.  **Conflict:** "Tell me about a time you disagreed with a team member." (Focus on resolution/data, not emotion).
2.  **Failure:** "Describe a bug you couldn't fix." (Admit it, explain what you learned).
3.  **KeyValue:** "Why fintech?" (Mention accuracy, security challenges interest you).

---

### ✅ Self-Grading
*   **Coding:** Did you handle empty lists? Did you optimize rate limiter to O(1)?
*   **MCQ:** > 80% correct is safe.
*   **Design:** Did you mention locking or atomic transactions for the Flash Sale?
