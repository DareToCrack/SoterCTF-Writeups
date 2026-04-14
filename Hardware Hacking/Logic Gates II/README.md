# 🧩 Logic Gates II — Writeup

## 📌 Challenge Information

* **Challenge Name:** Logic Gates II
* **Category:** Hardware Hacking

* **Final Flag:** `SoterCTF{Peresoso_a1_poder}`

---

# 📝 Challenge Description

We were given an input file and a logic gate circuit.

After passing the input through the circuit, it produced a final binary output.

That binary data had to be decoded to recover the flag.

---

# 🧠 My Approach

I translated the logic circuit into Python code and simulated the gates.

Each group of 4 bits was processed through the circuit to generate 1 output bit.

Then I joined all output bits into one binary string and converted it into a file.

---

# 🛠️ My Python Logic

```python id="q2m8vt"
def inverter(x):
    return 1 - x

def xor_gate(x, y):
    return x ^ y

def gen_out(A, B, C, D):
    A_STAGE_1 = inverter(A)
    A_B_STAGE_1 = xor_gate(A_STAGE_1, B)
    C_D_STAGE_1 = xor_gate(C, D)
    C_D_STAGE_2 = inverter(C_D_STAGE_1)
    A_B_C_D_STAGE_1 = xor_gate(A_B_STAGE_1, C_D_STAGE_2)
    A_B_C_D_STAGE_2 = inverter(A_B_C_D_STAGE_1)
    return A_B_C_D_STAGE_2
```

---

# 🔍 Full Solve Process

## Step 1 — Read Input

The file contained many 4-bit groups.

Example:

```text id="m1p7dx"
1010 0110 1101 ...
```

## Step 2 — Generate Output Bits

Each group was passed into `gen_out()`.

```python id="x8k4br"
binary_string = ""

for bits in inputs:
    binary_string += str(gen_out(*bits))
```

## Step 3 — Convert to Bytes

```python id="f5v9zn"
data = int(binary_string, 2).to_bytes(len(binary_string)//8, "big")
```

## Step 4 — Save File

The output was saved as a file.

After checking the file type, it was identified as a **JPEG image**.

## Step 5 — Open Image

Opening the image revealed the flag.

---

# 🚩 Final Flag

```text id="r3n6qp"
SoterCTF{Peresoso_a1_poder}
```

---

# 🧠 Key Insights

* Hardware challenges can often be solved with code
* Logic gates are easy to simulate in Python
* Binary output may hide files like images or archives

---

# 🚀 Conclusion

By recreating the logic circuit in Python, generating the final binary output, and converting it into an image, I successfully recovered the hidden flag.

<img width="1599" height="870" alt="image" src="https://github.com/user-attachments/assets/0cab8b12-bb15-421b-bf0f-a2dbdb5428bf" />
