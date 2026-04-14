# 🧩 Asteroid Pathfinder — Writeup

## 📌 Challenge Information

* **Challenge Name:** Asteroid Pathfinder
* **Category:** Programming

* **Final Flag:** `SoterCTF{158}`

---

# 📝 Challenge Description

We were given a large ASCII grid representing an asteroid field.

* `.` = free path
* `#` = obstacle
* Start coordinate
* End coordinate

The goal was to find the **shortest path** from start to end while moving only:

* Up
* Down
* Left
* Right

Then submit the number of steps as:

```text id="r1m7qa"
SoterCTF{steps}
```

---

# 🧠 Initial Analysis

Since the map was large, solving manually was impossible.

This is a classic shortest-path problem on a grid, so the best solution is to use a pathfinding algorithm such as:

* BFS
* Dijkstra
* A*

Because every move had equal cost, **BFS (Breadth-First Search)** was enough and simple.

---

# 🔍 Approach

The idea:

1. Start from the initial position
2. Explore all valid neighboring cells
3. Avoid walls (`#`)
4. Mark visited cells
5. Stop when reaching the destination

The first time we reach the end gives the shortest distance.

---

# 🛠️ Python Solve Script

```python id="n4x2pb"
from collections import deque

grid = data["grid"]
start = tuple(data["start"])
end = tuple(data["end"])

rows = len(grid)
cols = len(grid[0])

queue = deque([(start[0], start[1], 0)])
visited = set([start])

dirs = [(1,0), (-1,0), (0,1), (0,-1)]

while queue:
    x, y, dist = queue.popleft()

    if (x, y) == end:
        print("Steps:", dist)
        break

    for dx, dy in dirs:
        nx, ny = x + dx, y + dy

        if 0 <= nx < rows and 0 <= ny < cols:
            if grid[nx][ny] == "." and (nx, ny) not in visited:
                visited.add((nx, ny))
                queue.append((nx, ny, dist + 1))
```

---

# ✅ Result

The shortest path length found was:

```text id="t9v6ke"
158
```

---

# 🚩 Final Flag

```text id="p3c8zn"
SoterCTF{158}
```

---

# 🧠 Key Insights

## 🔹 Use BFS for Equal Costs

When each move costs the same, BFS guarantees the shortest path.

## 🔹 Avoid Revisiting Cells

Using a visited set prevents loops and speeds up the search.

## 🔹 Grid Problems Are Common

Many programming challenges use maze/pathfinding logic.

---

# 📚 Lessons Learned

* Convert the challenge into a graph problem
* BFS is often the simplest correct solution
* Large maps should always be solved programmatically

---

# 🚀 Conclusion

This challenge was a straightforward pathfinding task.
By applying BFS on the grid and counting the moves to the destination, the shortest route was found successfully.

---

# 🏁 Flag

```text id="w7d1lr"
SoterCTF{158}
```
<img width="1919" height="959" alt="image" src="https://github.com/user-attachments/assets/983f5ff6-c919-47b7-a1e6-9d09fa10071c" />
