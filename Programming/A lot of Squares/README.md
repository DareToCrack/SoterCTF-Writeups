# 🧩 A Lot of Squares — Writeup

## 📌 Challenge Information

* **Challenge Name:** A Lot of Squares
* **Category:** Programming
* **CTF:** SoterCTF / Palcam CyberGames 2026
* **Difficulty:** Hard
* **Author:** D3bo
* **Final Flag:** `SoterCTF{7954879fb82686cebd9ad7c2c3ee8ce0}`

---

# 📝 Challenge Description

We were asked to generate all possible **Latin Squares of order 5**.

A Latin Square is a `5 × 5` grid where:

* Each row contains numbers `1 to 5`
* Each column contains numbers `1 to 5`
* No duplicates are allowed in any row or column

After generating all valid squares:

1. Flatten each square into a string
2. Sort all strings
3. Concatenate them
4. Compute the MD5 hash

---

# 🧠 My Approach

A direct brute force over all possible grids would be impossible.

So I used **backtracking with pruning**:

* Build the square row by row
* Each row must be a permutation of `[1,2,3,4,5]`
* Before going deeper, check column validity
* If invalid, stop early

This makes the search fast enough.

---

# 🛠️ My Python Script

```python id="g5n2pd"
import itertools
import hashlib

N = 5
nums = [1, 2, 3, 4, 5]
latin_squares = []

def is_valid(square, row):
    for col in range(N):
        seen = set()
        for r in range(row + 1):
            val = square[r][col]
            if val in seen:
                return False
            seen.add(val)
    return True

def backtrack(square, row):
    if row == N:
        s = "".join(str(x) for r in square for x in r)
        latin_squares.append(s)
        return

    for perm in itertools.permutations(nums):
        square[row] = perm
        if is_valid(square, row):
            backtrack(square, row + 1)

square = [None] * N
backtrack(square, 0)

latin_squares.sort()
big_string = "".join(latin_squares)

md5hash = hashlib.md5(big_string.encode()).hexdigest()
print(md5hash)
```

---

# 🔍 How It Works

## Step 1 — Generate Row Permutations

Each row must contain numbers `1..5`, so every row is a permutation.

## Step 2 — Validate Columns

After placing a row, check each column.

If any duplicate appears, reject that branch immediately.

## Step 3 — Save Valid Squares

When 5 valid rows are placed, flatten the grid into one string.

## Step 4 — Final Hash

Sort all generated strings, join them together, and compute MD5.

---

# ✅ Result

The script generated all valid Latin Squares of order 5 and produced:

```text id="r8x4kv"
7954879fb82686cebd9ad7c2c3ee8ce0
```

---

# 🚩 Final Flag

```text id="q3m7zc"
SoterCTF{7954879fb82686cebd9ad7c2c3ee8ce0}
```

---

# 🧠 Key Insights

* Backtracking is perfect for constraint problems
* Early pruning saves huge time
* Small mistakes in sorting or flattening give wrong hash
* Efficient logic matters more than brute force

---

# 🚀 Conclusion

This challenge was a combinatorics problem based on Latin Squares.
Using backtracking with column checks, I generated all valid grids, processed them as required, and recovered the final flag.
<img width="759" height="424" alt="image" src="https://github.com/user-attachments/assets/c5eb496f-3a7e-4331-9bac-425575aff39b" />

<img width="1789" height="951" alt="image" src="https://github.com/user-attachments/assets/71e5eed5-9e8f-4a1b-960c-960b45ef163a" />
