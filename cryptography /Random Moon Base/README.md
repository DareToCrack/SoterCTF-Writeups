# 🧩 Random Moon Base — Writeup

## 📌 Challenge Information

* **Challenge Name:** Random Moon Base
* **Category:** Cryptography
* **CTF:** SoterCTF / Palcam CyberGames 2026
* **Difficulty:** Hard
* **Author:** D3bo
* **Final Flag:** `SoterCTF{53ee28c58aa7af4b57720b7d3c508c27}` 

---

# 📝 Challenge Description

> During a reconnaissance mission in a dark crater, a spacecraft detects an ancient structure beneath the lunar dust. As it approaches, the structure emits a series of luminous patterns that appear to follow a specific order. The explorers attempt to interpret the patterns, but each attempt reveals only fragments of a cryptic message. As the hours pass, the structure begins to show signs of activity, as if something is preparing to awaken. 

---

# 🧠 Initial Analysis

This challenge was not standard encryption. It used multiple layers of encoding and obfuscation.

From the provided script, the process looked like this:

1. Convert the flag into binary
2. Convert binary into a large integer
3. Convert that integer into another base
4. Replace digits using a shuffled alphabet

So the challenge was about reversing the full encoding pipeline. 

---

# 🔍 Important Observation

The alphabet used was:

```text id="a1m8qe"
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
```

But this alphabet was shuffled before use.

The critical weakness: the shuffle was **deterministic**.

It used:

```python id="v9x2la"
random.seed(base * len(alphabet))
```

That means if we guess the correct base, we can reproduce the same shuffled alphabet. 

---

# 🧪 Step 1 — Brute Force the Base

The base was limited to a small range:

```text id="m2g9ru"
10 to 60
```

So we simply test every possible base.

For each base:

1. Recreate the shuffled alphabet
2. Reverse the encoded text into digit values
3. Convert digits back into an integer
4. Convert integer into bytes
5. Check if output starts with `SoterCTF{`

---

# 🧪 Step 2 — Reverse the Alphabet Mapping

Simplified idea:

```python id="b7w3pc"
def unshuffle(text, shuffled):
    return [shuffled.index(c) for c in text]
```

Each character is converted back into its original numeric position.

---

# 🧪 Step 3 — Convert Back to Plaintext

After recovering the digit values:

```python id="f4n8yk"
n = 0
for i, d in enumerate(indices):
    n += d * (base ** i)
```

Then:

* Integer → Binary
* Binary → Bytes
* Bytes → Text

---

# 🛠️ Full Solver Script

```python id="j5q2ez"
import random

alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
output = "+epfFznzIwf/iCXxSXNzFD/memrq8QPEM/EHOmiEAjxq+0si/oOpOTEEHsP"

def unshuffle(s, shuffled):
    return [shuffled.index(c) for c in s]

def try_decode(indices, base):
    n = 0
    for i, d in enumerate(indices):
        n += d * (base ** i)

    bits = bin(n)[2:]
    bits = bits.zfill((len(bits) + 7) // 8 * 8)

    data = bytearray()
    for i in range(0, len(bits), 8):
        data.append(int(bits[i:i+8], 2))

    try:
        return data.decode()
    except:
        return None

for base in range(10, 61):
    random.seed(base * len(alphabet))
    shuffled = ''.join(random.sample(alphabet, len(alphabet)))

    try:
        indices = unshuffle(output, shuffled)
        flag = try_decode(indices, base)

        if flag and flag.startswith("SoterCTF{"):
            print("Base:", base)
            print(flag)
            break
    except:
        pass
```

---

# 🚩 Final Flag

```text id="p8s6td"
SoterCTF{53ee28c58aa7af4b57720b7d3c508c27}
```

---

# 🧠 Key Insights

## 🔹 Deterministic Randomness Is Weak

If the random seed can be guessed, the shuffle can be recreated.

## 🔹 Multiple Layers ≠ Strong Security

Even with several encoding steps, each layer was reversible.

## 🔹 Small Search Space Helps

Trying bases from 10 to 60 is very practical.

## 🔹 Recognizable Prefixes Matter

Checking for `SoterCTF{` quickly confirms the correct output.

---

# 📚 Lessons Learned

* Always inspect how randomness is generated
* Custom encoding schemes are often reversible
* Brute force becomes powerful when the search range is small
* Predictable seeds can completely break a system

---

# 🚀 Conclusion

This challenge combined base conversion, shuffled alphabets, and deterministic randomness to hide the flag.

By reversing each layer and brute-forcing the possible base values, we reconstructed the original message and recovered the flag.

---

# 🏁 Flag

```text id="d4u7kx"
SoterCTF{53ee28c58aa7af4b57720b7d3c508c27}
```
[solver.py](https://github.com/user-attachments/files/26717603/solver.py)
<img width="1158" height="892" alt="image" src="https://github.com/user-attachments/assets/233cf88d-c876-440a-b648-50e00c2b61ad" />
