# 🧩 Caïssa — Writeup

## 📌 Challenge Information

* **Challenge Name:** Caïssa
* **Category:** Cryptography
  
* **Final Flag:** `SoterCTF{6171776965686A313233}`

---

# 📝 Challenge Description

> During the Nebula-12 mission, a secret transmission was sent back to Earth containing vital mission data. However, the encoding mechanism of the lunar module's communication system has been lost to time. Your task is to decode the intercepted signal and recover the hidden message. The transmission uses a custom numerical encoding scheme with a mysterious modulus value. Unlock the secrets of the past and plant your flag on the moon!

---

# 🧠 Initial Analysis

The challenge provided a long list of positive and negative hexadecimal values.

Example:

```text id="c1d4op"
0x19, -0x16, 0xf, 0x2, -0xa ...
```

This suggested the data was **not raw ciphertext**, but an encoded sequence that required multiple reversal steps.

The negative and positive jumps strongly hinted at **difference encoding (delta encoding)**.

---

# 🔍 Main Idea

After inspecting the pattern, the encoding process appeared to be:

1. Plaintext characters
2. Compressed into a custom bitstream
3. Split into 5-bit chunks
4. Converted into cumulative values
5. Stored as differences in hexadecimal form

So the solve path was to reverse each layer one by one.

---

# 🧪 Step 1 — Undo Difference Encoding

The given numbers were differences between values.

We reconstruct the original sequence using cumulative sums:

```python id="p8x2re"
nums = [int(x, 16) for x in data]

values = []
cur = 0

for n in nums:
    cur += n
    values.append(cur)
```

Now we recover the original encoded integers.

---

# 🧪 Step 2 — Rebuild the Bitstream

Each recovered number represented a 5-bit block.

So we convert every value into binary:

```python id="f3m9yk"
bitstring = ''.join(format(x, '05b') for x in values)
```

This recreates the compressed bitstream.

---

# 🧪 Step 3 — Identify the Coding Scheme

The bitstream followed this pattern:

* Several `0`s
* Then a `1`
* Then fixed-size binary bits

This matches a **Golomb-style coding** structure:

* Unary prefix = quotient `q`
* Binary suffix = remainder `r`

The missing parameter was the modulus:

```text id="u4t8ls"
m
```

---

# 🧪 Step 4 — Brute Force the Modulus

We tested possible values of `m` between:

```text id="z2q6vn"
100 to 200
```

Only multiples of 4 were valid.

For each value:

1. Decode the bitstream
2. Convert to ASCII
3. Check for readable output

---

# 🧪 Step 5 — Successful Decode

The correct modulus produced:

```text id="k7w1af"
Here is your flag: SoterCTF{6171776965686A313233} good job!!!
```

So the flag was recovered successfully.

---

# 🛠️ Solver Script

```python id="m5v0pc"
import math

data = [...]   # given values

nums = [int(x, 16) for x in data]

# Undo differences
c = []
cur = 0
for n in nums:
    cur += n
    c.append(cur)

for offset in range(5):
    bitstring = ''.join(format(x, '05b') for x in c)[offset:]

    for m in range(100, 200):
        if m % 4 != 0:
            continue

        b = math.ceil(math.log2(m))
        i = 0
        res = ""

        try:
            while i < len(bitstring):
                q = 0
                while i < len(bitstring) and bitstring[i] == '0':
                    q += 1
                    i += 1

                if i >= len(bitstring):
                    break

                i += 1
                if i + b > len(bitstring):
                    break

                r = int(bitstring[i:i+b], 2)
                i += b

                val = q * m + r

                if val < 32 or val > 126:
                    raise Exception()

                res += chr(val)

            if "SoterCTF" in res:
                print(res)

        except:
            pass
```

---

# 🚩 Final Flag

```text id="n9b4qx"
SoterCTF{6171776965686A313233}
```

---

# 🧠 Key Insights

## 🔹 Difference Encoding Hides Structure

The values were only changes, not the real data.

## 🔹 Bit-Level Analysis Was Important

Rebuilding the bitstream revealed the true format.

## 🔹 Small Parameter Search Space

Brute-forcing `m` was practical.

## 🔹 Multiple Layers ≠ Strong Security

Each encoding step was reversible.

---

# 📚 Lessons Learned

* Check for cumulative / delta encoded outputs
* Reconstruct bitstreams carefully
* Compression formats can appear in crypto challenges
* Brute-forcing small unknown parameters is often intended

---

# 🚀 Conclusion

This challenge combined delta encoding, bit packing, and Golomb-style compression to hide the flag.

By reversing the cumulative values, rebuilding the binary stream, and brute-forcing the modulus parameter, the hidden message was recovered successfully.

---

# 🏁 Flag

```text id="t6k3er"
SoterCTF{6171776965686A313233}
```
<img width="1127" height="930" alt="image" src="https://github.com/user-attachments/assets/91909884-e7ea-4c18-86d6-1b856314b05c" />
