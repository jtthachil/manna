# 🐍 Scenario-Based Coding: The "Real World" Edition (v2.0)

KeyValue loves to hide simple structures behind complex stories. Here is an expanded collection with **deep comments**.

---

## 🧠 Section 1: The "Pattern Recognition" Cheat Sheet
*If the question says... -> You think...*

| Keyword in Question | Likely Data Structure |
| :--- | :--- |
| "Top K items", "Smallest K", "Median" | **Heap (Priority Queue)** |
| "Shortest path", "Network delay", "Degrees of separation" | **BFS (Unweighted)** or **Dijkstra (Weighted)** |
| "Connectivity", "Groups of friends", "Islands" | **DFS** or **Union-Find** |
| "Substring", "Subarray", "Window" | **Sliding Window** or **Two Pointers** |
| "Scheduling", "Overlapping events" | **Sorting + Greedy** |
| "Dictionary", "Autocomplete", "Prefix" | **Trie** |
| "Duplicate check", "Unique items" | **HashSet** |
| "Range Sum", "Grid Sum" | **Prefix Sum Array** |

---

## 🚙 Section 2: Real-World Scenarios (Python Solutions)

### 1. The "CoFee" Autocomplete (Trie / Prefix Tree)
**Scenario:**
In the CoFee app, when a parent searches for a school name like "St.", we want to instantly show "St. Marys", "St. Peters", etc. Scanning a list of 10 school names is easy. Scanning 100,000 schools is slow.

**Structure:** **Trie** (Prefix Tree).
**Why?** Search complexity becomes `O(Length of word)`, independent of the number of schools.

```python
class TrieNode:
    def __init__(self):
        self.children = {} # Map 'char' -> TrieNode
        self.is_end_of_word = False

class SchoolSearch:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, school_name):
        """Adds a school to our index."""
        node = self.root
        for char in school_name:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search_prefix(self, prefix):
        """Returns True if any school starts with this prefix."""
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True # We found the node representing the prefix endpoint

# Usage
search_engine = SchoolSearch()
search_engine.insert("Saint Marys")
search_engine.insert("Saint Peters")
print(search_engine.search_prefix("Sai")) # True
print(search_engine.search_prefix("Xav")) # False
```

### 2. The "Payment Deduplication" (Hash Set)
**Scenario:**
Double-clicks on "Pay Now". We need O(1) time to check if `transaction_id` `txn_123` was seen in the last 5 minutes.

```python
class PaymentGateway:
    def __init__(self):
        self.seen_transactions = set()
    
    def process_payment(self, txn_id):
        # 1. Check Duplication
        if txn_id in self.seen_transactions:
            print(f"Duplicate/Fraudelent transaction {txn_id} blocked.")
            return False
        
        # 2. Process (Mock)
        self.seen_transactions.add(txn_id)
        print(f"Transaction {txn_id} processed.")
        return True
```

### 3. The "Delivery Route" (Graph - Dijkstra)
**Scenario:**
A driver needs the **fastest** route from Restaurant A to Customer B. Roads have "traffic weights".
**Why Dijkstra?** BFS is for unweighted graphs. Roads have weights (Traffic).

```python
import heapq

def shortest_delivery_time(n, roads, start, end):
    """
    n: number of intersections
    roads: [[u, v, time], ...]
    """
    # 1. Build Adjacency Graph
    graph = {i: [] for i in range(n)}
    for u, v, w in roads:
        graph[u].append((v, w))
        graph[v].append((u, w)) # Undirected road
    
    # 2. Min-Heap for Dijkstra: (time_taken, current_node)
    min_heap = [(0, start)]
    times = {i: float('inf') for i in range(n)}
    times[start] = 0
    
    while min_heap:
        current_time, u = heapq.heappop(min_heap)
        
        if u == end:
            return current_time
            
        if current_time > times[u]:
            continue
            
        for v, w in graph[u]:
            new_time = current_time + w
            if new_time < times[v]:
                times[v] = new_time
                heapq.heappush(min_heap, (new_time, v))
                
    return -1 # Unreachable
```

### 4. The "Server Cluster" (Merge Intervals)
**Scenario:**
You have a maintenance schedule. Server A is down `[1, 3]` (1pm to 3pm). Server B is down `[2, 6]`. Server C is down `[8, 10]`.
Find the total time intervals the system has reduced capacity.
**Input:** `[[1,3], [2,6], [8,10]]` -> **Merge** -> `[[1,6], [8,10]]`.

```python
def merge_maintenance_windows(intervals):
    intervals.sort(key=lambda x: x[0]) # Sort by start time
    merged = []
    
    for interval in intervals:
        # If list empty or no overlap, add it
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            # Overlap! Merge them. End is max of both ends.
            merged[-1][1] = max(merged[-1][1], interval[1])
            
    return merged
```

---

## ⚔️ Section 3: The "Unfair" Hard Questions (Be Ready)

### 1. "Top K Paying Users" (Heap)
**Scenario:** Millions of transactions streaming in. Keep track of the top 10 biggest spenders in real-time.
**Solution:** Use a **Min-Heap of size 10**.
*   If new_val > heap.min():
    *   heap.pop()
    *   heap.push(new_val)

### 2. "Circular Buffer" (Queue)
**Scenario:** Implement a specialized Logger that prints the last 5 minutes of logs only.
**Solution:** **Deque** (Double Ended Queue) or fixed array with modulo index `i % size`.

### 3. "Grid Island" (DFS/BFS) - *Classic*
**Scenario:**
Map of 1s (Land) and 0s (Water). Count islands.
**Code Skeleton:**
```python
def numIslands(grid):
    count = 0
    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == '1':
                dfs(grid, r, c)
                count += 1
    return count

def dfs(grid, r, c):
    if r<0 or c<0 or r>=len(grid) or c>=len(grid[0]) or grid[r][c] != '1':
        return
    grid[r][c] = '#' # Mark as visited!
    dfs(grid, r+1, c); dfs(grid, r-1, c)
    dfs(grid, r, c+1); dfs(grid, r, c-1)

### 4. "The Delivery Grid" (Min Path Sum)
**Scenario:** A delivery robot needs to cross a grid from top-left to bottom-right. Each cell has a "fuel cost". Minimize fuel.
**Technique:** **Dynamic Programming** (or Dijkstra if costs vary wildly/non-local).
**DP Logic:** `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.

### 5. "The Maze Runner" (BFS Shortest Path)
**Scenario:** Grid with Walls. Find shortest path steps from Start to End.
**Why BFS?** BFS guarantees shortest path in unweighted graph (grid steps = 1).
**Code Skeleton:**
```python
from collections import deque
def shortestPath(grid, start, end):
    queue = deque([(start, 0)]) # (coord, steps)
    visited = set([start])
    directions = [(0,1), (0,-1), (1,0), (-1,0)]
    
    while queue:
        (r, c), steps = queue.popleft()
        if (r, c) == end: return steps
        
        for dr, dc in directions:
            nr, nc = r+dr, c+dc
            if 0 <= nr < len(grid) and 0 <= nc < len(grid[0]) and \
               grid[nr][nc] != 'Wall' and (nr,nc) not in visited:
                queue.append(((nr, nc), steps+1))
                visited.add((nr, nc))
    return -1
```
```
