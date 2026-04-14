# 🧩 A Lot of Queens — Writeup

## 📌 Challenge Information

* **Challenge Name:** A Lot of Queens
* **Category:** Programming
* **CTF:** SoterCTF / Palcam CyberGames 2026
* **Difficulty:** Medium
* **Author:** D3bo
* **Final Flag:** `SoterCTF{5f805477c1a1f2ec689efeb95bfa56ea}`

---

# 📝 Challenge Description

The challenge was actually based on the classic **8 Queens Problem**.

We needed to:

* Place 8 queens on an 8×8 board
* No two queens can attack each other
* No same row
* No same column
* No same diagonal

Then:

1. Generate all valid solutions
2. Convert each solution into an 8-digit string
3. Sort all solutions
4. Concatenate them
5. Compute the MD5 hash

---

# 🧠 My Approach

I used a **backtracking solution**.

Instead of checking random boards, I placed queens row by row:

* One queen per row
* Track used columns
* Track diagonals
* If a position is invalid, skip it immediately

This is fast and clean.

---

# 🛠️ My Python Code

```python id="q8m3zx"
import hashlib

N = 8
solutions = []

def solve(row, cols, diag1, diag2, board):
    if row == N:
        solutions.append(board[:])
        return
    
    for col in range(N):
        if col in cols or (row - col) in diag1 or (row + col) in diag2:
            continue
        
        cols.add(col)
        diag1.add(row - col)
        diag2.add(row + col)
        board.append(col)
        
        solve(row + 1, cols, diag1, diag2, board)
        
        board.pop()
        cols.remove(col)
        diag1.remove(row - col)
        diag2.remove(row + col)

solve(0, set(), set(), set(), [])

strings = ["".join(str(x) for x in sol) for sol in solutions]
strings.sort()

big_string = "".join(strings)

md5 = hashlib.md5(big_string.encode()).hexdigest()
print(md5)
```

---

# 🔍 How My Code Works

## Step 1 — Place Queens Row by Row

The function starts from row 0 and tries every column.

## Step 2 — Check Safety

A queen cannot be placed if:

* Column already used
* Main diagonal already used
* Anti-diagonal already used

## Step 3 — Save Valid Boards

When all 8 rows are completed, one valid solution is stored.

## Step 4 — Final Hash

All solutions are converted into strings, sorted, joined, and hashed using MD5.

---

# ✅ Result

The script generated all 92 valid solutions and returned:

```text id="v2k9er"
5f805477c1a1f2ec689efeb95bfa56ea
```

---

# 🚩 Final Flag

```text id="n6p4yt"
SoterCTF{5f805477c1a1f2ec689efeb95bfa56ea}
```

---

# 🧠 Key Insights

* Backtracking is ideal for N-Queens problems
* Using sets makes checking fast
* Pruning invalid moves saves time
* Sorting before hashing is important

---

# 🚀 Conclusion

This challenge was a fun combinatorics problem based on the 8 Queens puzzle.
Using backtracking, I generated all valid arrangements and computed the final hash successfully.

[d9b7f318c26a057ad550ab069cf7c616.pdf](https://github.com/user-attachments/files/26720042/d9b7f318c26a057ad550ab069cf7c616.pdf)
<img width="1919" height="910" alt="image" src="https://github.com/user-attachments/assets/5c3f3606-68b4-4920-ad34-7605f7ea4379" />
