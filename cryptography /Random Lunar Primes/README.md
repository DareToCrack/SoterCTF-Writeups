# 🧩 Random Lunar Primes — Writeup

## 📌 Challenge Information

* **Challenge Name:** Random Lunar Primes
* **Category:** Cryptography

* **Final Flag:** `SoterCTF{469525900a94cba223d5d3c4e0581b42}`

---

# 📝 Challenge Description

> On the lunar surface, a spacecraft detects a mysterious signal. Systems begin to fail, displaying sequences of seemingly random numbers. The crew discovers they are prime numbers, chaotically interwoven, part of a hidden enigma. Time is running out as the ship's resources dwindle. Can you decipher the code and uncover the message before it's too late? The fate of the mission rests in your hands.

---

# 🧠 Initial Analysis

The challenge provided a list of large encoded numbers.

At first glance, the numbers looked random and the story mentioned prime numbers, but the real task was understanding the custom encryption logic.

From the source code, the encryption formula was:

```text id="m1v8pa"
C[i] = ord(flag[i]) + key[i] ^ key[-i]
```

Where:

* `C[i]` = encoded value
* `flag[i]` = original character
* `key[i]` = key element at index `i`
* `key[-i]` = mirrored key element

So to decrypt:

```text id="n6q3de"
flag[i] = C[i] - (key[i] ^ key[-i])
```

---

# 🔍 Important Observation

The key values were very small:

```text id="f2u9rk"
0 to 23
```

This makes brute force practical.

Even though the encoded values were huge, the secret key space was tiny.

That was the main weakness.

---

# 🧪 Step 1 — Precompute Powers

Because the formula uses exponentiation repeatedly, we first generated all possible values:

```python id="b3x7qs"
pow_table = {(a, b): a**b for a in range(24) for b in range(24)}
```

This gives every possible result of:

```text id="g4t1yn"
a^b where a,b ∈ [0,23]
```

---

# 🧪 Step 2 — Find Valid Character Pairs

For each encoded number:

1. Try every `(a, b)` pair
2. Compute `a^b`
3. Subtract from encoded value
4. Check if result is a printable ASCII character

Example:

```python id="r8m2lp"
char = encoded[i] - (a**b)

if 32 <= char <= 126:
    # possible plaintext
```

This filters impossible key combinations.

---

# 🧪 Step 3 — Apply Symmetry Constraint

The encryption used:

```text id="w9k5hz"
key[i] and key[-i]
```

So positions are linked.

That means if we choose a key value for one index, the mirrored index must match consistently.

We used this rule to eliminate invalid combinations and keep only valid full-key solutions.

---

# 🧪 Step 4 — Recover the Flag

After solving the key relationships, we decode every character:

```python id="q1d6ec"
flag += chr(encoded[i] - (a**b))
```

This reconstructs the full plaintext.

---

# 🛠️ Solver Script

```python id="x4n9pt"
encoded = [...]   # given values
n = len(encoded)

pow_table = {(a,b): a**b for a in range(24) for b in range(24)}
pairs = [[] for _ in range(n)]

for i in range(n):
    for a in range(24):
        for b in range(24):
            val = encoded[i] - pow_table[(a,b)]
            if 32 <= val <= 126:
                pairs[i].append((a,b,val))

solutions = [{}]

for i in range(n):
    j = (-i) % n
    new_solutions = []

    for sol in solutions:
        for a,b,c in pairs[i]:
            if (i in sol and sol[i] != a):
                continue
            if (j in sol and sol[j] != b):
                continue

            ns = sol.copy()
            ns[i] = a
            ns[j] = b
            new_solutions.append(ns)

    solutions = new_solutions

for sol in solutions:
    flag = ""
    for i in range(n):
        a = sol[i]
        b = sol[(-i) % n]
        flag += chr(encoded[i] - (a**b))
    print(flag)
```

---

# 🚩 Final Flag

```text id="z7e3vm"
SoterCTF{469525900a94cba223d5d3c4e0581b42}
```

---

# 🧠 Key Insights

## 🔹 Small Key Space

Even with huge outputs, the actual key values were only `0–23`.

## 🔹 Symmetry Helps Recovery

Using `key[i]` and `key[-i]` leaks structure.

## 🔹 Big Numbers Can Mislead

Large values do not automatically mean strong encryption.

## 🔹 Constraints Reduce Search Space

ASCII checks quickly remove wrong guesses.

---

# 📚 Lessons Learned

* Always inspect key ranges in crypto challenges
* Mathematical complexity can hide weak design
* Structural patterns often reveal the solution
* Printable character checks are powerful for decryption

---

# 🚀 Conclusion

This challenge used exponentiation with mirrored key indices to hide the flag inside large numbers.

Although the output looked intimidating, the tiny key space and symmetric structure made it solvable through constraint-based brute force.

By testing possible key pairs and enforcing consistency, the original flag was recovered successfully.

---

# 🏁 Flag

```text id="c8p4ju"
SoterCTF{469525900a94cba223d5d3c4e0581b42}
```
