# 🧩 Access Denied — Writeup

## 📌 Challenge Information

* **Challenge Name:** Access Denied
* **Category:** Reverse Engineering

* **Final Flag:** `SoterCTF{fc0a143ff035763e07bf123813915733}`

---

# 📝 Challenge Description

> This program can only be accessed by the elite of the space agency, can you get the password?

---

# 🧠 Initial Analysis

This was a classic reverse engineering challenge.

The goal was to inspect the program, understand how it checks the password, and recover the correct input.

Instead of guessing passwords manually, the faster method was to analyze the binary logic.

---

# 🔍 My Approach

I opened the executable in a reverse engineering tool such as:

* Ghidra
* IDA
* strings
* x64dbg

Then I looked for:

* Password comparison functions
* Hardcoded strings
* Success / failure messages
* Hidden hashes or checks

---

# 🛠️ Typical Solve Path

After analyzing the code, the password validation logic could be identified.

The hidden password or expected value was recovered from the binary, then entered into the program to unlock access.

Once the correct password was provided, the flag was shown.

---

# 🚩 Final Flag

```text id="x4m8qp"
SoterCTF{fc0a143ff035763e07bf123813915733}
```

---

# 🧠 Key Insights

* Reverse engineering is often faster than brute force
* Search for comparison logic first
* Strings and function names can reveal important clues

---

# 🚀 Conclusion

By analyzing the executable and extracting the password check logic, access was bypassed and the flag was recovered.

<img width="1837" height="960" alt="image" src="https://github.com/user-attachments/assets/3506d022-8f73-40c9-b7eb-6bd8322cc2c5" />
