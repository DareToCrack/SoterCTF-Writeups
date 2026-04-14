# 🧩 Twins — Writeup

## 📌 Challenge Information

* **Challenge Name:** Twins
* **Category:** Programming

* **Final Flag:** `SoterCTF{530477}`

---

# 📝 Challenge Description

We needed to count how many **twin prime pairs** exist between:

```text id="n4x7pa"
1 to 123,456,789
```

Twin primes are pairs of prime numbers:

```text id="v2m8rk"
(p, p + 2)
```

Where both numbers are prime.

Example:

* (3,5)
* (5,7)
* (11,13)

Then submit the answer as:

```text id="d8k1zc"
SoterCTF{count}
```

---

# 🧠 My Approach

Checking every number one by one would be too slow.

So I used the **Sieve of Eratosthenes** to generate all prime numbers efficiently, then counted every pair where both `i` and `i+2` are prime.

---

# 🛠️ My Python Code

```python id="q7p3tm"
import math

def count_twin_primes(n):
    sieve = bytearray(b'\x01') * (n + 1)
    sieve[0:2] = b'\x00\x00'
    
    for i in range(2, int(math.isqrt(n)) + 1):
        if sieve[i]:
            sieve[i*i:n+1:i] = b'\x00' * len(range(i*i, n+1, i))
    
    count = 0
    for i in range(3, n - 1):
        if sieve[i] and sieve[i + 2]:
            count += 1
            
    return count

n = 123_456_789
result = count_twin_primes(n)
print(f"SoterCTF{{{result}}}")
```

---

# 🔍 How My Code Works

## Step 1 — Build Prime Table

Create a sieve array where prime numbers stay marked as `True`.

## Step 2 — Remove Multiples

For every prime `i`, mark all multiples of `i` as not prime.

## Step 3 — Count Twin Pairs

Loop through numbers and check:

```text id="r5y9lx"
sieve[i] and sieve[i+2]
```

If both are prime, increase the count.

---

# ✅ Result

The total number of twin prime pairs was:

```text id="m3v6ke"
530477
```

---

# 🚩 Final Flag

```text id="t1q8dn"
SoterCTF{530477}
```

---

# 🧠 Key Insights

* Sieve is much faster than repeated prime checking
* Twin primes only need one extra check: `i+2`
* Efficient algorithms matter in large ranges

---

# 🚀 Conclusion

This challenge was a number theory programming task.
Using the Sieve of Eratosthenes, I generated primes efficiently and counted all twin prime pairs up to 123,456,789.

<img width="1919" height="921" alt="image" src="https://github.com/user-attachments/assets/62af7efa-0f52-4895-a001-265a6b01fcda" />
