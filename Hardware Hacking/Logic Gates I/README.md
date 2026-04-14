# 🧩 Logic Gates I — Writeup

## 📌 Challenge Information

* **Challenge Name:** Logic Gates I
* **Category:** Hardware Hacking

* **Final Flag:** `SoterCTF{Y0u_ar3_awes0me_a7__transistor_1ogic}`

---

# 📝 Challenge Description

We were given transistor inputs that pass through a logic circuit and produce a long binary output.

Each group of 4 input bits represented:

* A
* B
* C
* D

The final output needed to be converted into the flag.

---

# 🧠 My Approach

Instead of manually solving the transistor circuit, I used a brute-force approach.

Since 4 input bits create only 16 possible combinations, I tested every possible 16-bit truth table and checked which output produced readable text.

---

# 🛠️ My Solve Idea

## Step 1 — Read Inputs

Each 4-bit input was converted into a number from 0 to 15.

## Step 2 — Test All Logic Tables

There are:

```text id="f1m8de"
2^16 = 65536
```

possible truth tables.

For each one, I generated the binary output.

## Step 3 — Convert Output

The produced binary stream was converted into bytes and checked for meaningful text.

One valid result was:

```text id="x9p3rt"
U290ZXJDVEZ7WTB1X2FyM19hd2VzMG1lX2E3X190cmFuc2lzdG9yXzFvZ2ljfQ==
```

## Step 4 — Decode Base64

Decoding that string gave the flag.

---

# 🚩 Final Flag

```text id="k4v2zn"
SoterCTF{Y0u_ar3_awes0me_a7__transistor_1ogic}
```

---

# 🧠 Key Insights

* Small search spaces can be brute-forced
* Binary output often hides another encoding layer
* Base64 is common in CTF challenges

---

# 🚀 Conclusion

By brute-forcing all possible truth tables and decoding the correct output, I recovered the hidden flag successfully.

<img width="1816" height="896" alt="image" src="https://github.com/user-attachments/assets/98def9d0-2b47-47b9-8f4a-ea338083db9f" />
