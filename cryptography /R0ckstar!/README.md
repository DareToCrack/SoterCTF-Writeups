# 🧩 R0ckstar! — Writeup

## 📌 Challenge Information

* **Challenge Name:** R0ckstar!
* **Category:** Cryptography
* **CTF:** SoterCTF / Palcam CyberGames 2026
* **Difficulty:** Medium
* **Author:** iHarzz
* **Final Flag:** `SoterCTF{Shr1nk1ng_g3n_bre4kabl3}`

---

# 📝 Challenge Description

> They said the signal was clean.
> They said nothing was missing.
>
> But something feels... off.
>
> You intercepted a transmission that doesn't quite behave like a normal stream. Bits seem to appear and disappear without warning, as if something is selectively filtering the data.
>
> Whatever system produced this output, it wasn't random.
>
> Your task is simple: recover what was lost.

### Files Provided

```text id="u731ak"
challenge.py
flag.enc
```

---

# 🧠 Initial Analysis

The provided Python script showed that the encryption system was based on **two LFSRs (Linear Feedback Shift Registers)**.

One register controlled when bits should be used, while the second generated the actual keystream bits.

This matches a known construction called a **Shrinking Generator**.

---

# 🔍 Understanding the Shrinking Generator

Two LFSRs are used:

* **A** → Control register
* **B** → Data register

The output works like this:

* If `A = 1` → output next bit from B
* If `A = 0` → discard next bit from B

So the output stream is irregular because some bits are skipped.

This makes the keystream look more random, but it is still breakable.

---

# ⚙️ Important Parameters

From the script:

```python id="h381dx"
tapsA = [0, 1, 3, 12]
tapsB = [0, 1, 4, 14]
```

These tap positions define how each LFSR updates its internal state.

---

# 🧪 Step 1 — Use Known Prefix

Most CTF flags begin with:

```text id="n28fae"
SoterCTF{
```

This gives us known plaintext.

Since stream ciphers use:

```text id="g2r7sa"
ciphertext = plaintext XOR keystream
```

We can recover part of the keystream using:

```text id="q4m0pk"
keystream = ciphertext XOR plaintext
```

---

# 🧪 Step 2 — Recover Keystream Bits

We XORed the first bytes of `flag.enc` with:

```python id="v93qwp"
known_prefix = b"SoterCTF{"
```

This reveals the first section of the keystream.

Those bytes were then converted into bits for comparison.

---

# 🧪 Step 3 — Reconstruct Internal States

The next step was to brute-force possible seeds for both LFSRs.

Search space:

* `seedA` → all possible 13-bit states
* `seedB` → all possible 15-bit states

For each pair:

1. Simulate the shrinking generator
2. Produce output bits
3. Compare with recovered keystream bits

When the bits matched, the correct internal states were found.

---

# 🧪 Step 4 — Decrypt the Flag

After recovering the correct seeds:

1. Generate the full keystream
2. XOR with ciphertext
3. Recover the original plaintext flag

---

# 🚩 Final Flag

```text id="m7d2ck"
SoterCTF{Shr1nk1ng_g3n_bre4kabl3}
```

---

# 🛠️ Example Solve Script

```python id="p3w0ex"
# Simplified idea
cipher = open("flag.enc", "rb").read()
known = b"SoterCTF{"

# Recover partial keystream
partial = bytes([c ^ p for c, p in zip(cipher, known)])

# Brute force states
# Simulate shrinking generator
# Compare bits
# Recover flag
```

---

# 🧠 Key Insights

## 🔹 Shrinking Generator ≠ Secure

Irregular output does not guarantee strong security.

## 🔹 Known Plaintext Helps a Lot

Knowing the flag prefix gives direct keystream bytes.

## 🔹 Small State Space

The seed sizes were small enough to brute-force.

## 🔹 LFSRs Are Predictable

Once the state is recovered, all future output is known.

---

# 📚 Lessons Learned

* Stream ciphers can fail when internal states are too small
* Known prefixes are dangerous in cryptography
* LFSR-based designs are weak without stronger protections
* “Looks random” does not mean secure

---

# 🚀 Conclusion

This challenge used a shrinking generator based on two LFSRs.

Although the output stream looked irregular, the known flag prefix and small state space made recovery possible. By reconstructing the keystream and recovering the generator states, the encrypted flag was successfully decrypted.

---

# 🏁 Flag
# If bit(A) == 1 -> output bit = bit(B)
# If bit(A) == 0 -> discard bit(B)

def lfsr_step(state, taps, nbits):���y!9T�G4����:fM�s�?|@���;��9
    out = state & 1
    feedback = 0
    for t in taps:
        feedback ^= (state >> t) & 1
    state >>= 1
    state |= (feedback << (nbits - 1))
    return state, out

nA = 13
nB = 15

tapsA = [0, 1, 3, 12]
tapsB = [0, 1, 4, 14]

# Unknown seeds:
# seedA = ?
# seedB = ?


```text id="t5x9vb"
SoterCTF{Shr1nk1ng_g3n_bre4kabl3}
[solver.py](https://github.com/user-attachments/files/26717092/solver.py)


<img width="1048" height="761" alt="image" src="https://github.com/user-attachments/assets/ecfb8e11-adb1-485e-8505-b08fac5e8684" />
