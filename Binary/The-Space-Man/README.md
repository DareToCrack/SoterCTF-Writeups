[Assembly-CSharp.txt](https://github.com/user-attachments/files/26713726/Assembly-CSharp.txt)
[Assembly-CSharp.txt](https://github.com/user-attachments/files/26713676/Assembly-CSharp.txt)
# 🧩 The Space Man — Writeup

## 📌 Challenge Information

* **Challenge Name:** The Space Man
* **Category:** Reverse Engineering

* **Final Flag:** `SoterCTF{D3fen5E_s1ST3m_dI55aBled}`

---

# 🛰️ Challenge Description

> The ship has entered a critical state.
> During the journey, a failure in the automated system triggered the emergency protocol: total hibernation. All hatches have been sealed.
> Command reports the existence of two access cards required to regain control… but the security system has strengthened its defenses.
> Nothing responds. Everything is locked.
> Find the cards. Open the hatches. Survive.
> And make it back home, comrade.

---

# 🧠 Initial Analysis

The challenge files included:

* `The Space Man.exe`
* `UnityPlayer.dll`
* `UnityCrashHandler64.exe`

The presence of `UnityPlayer.dll` tells us that this executable was built using the **Unity game engine**.

For Unity reverse engineering challenges, the flag is commonly hidden in:

* Game scripts
* Asset files
* Encoded strings
* Trigger events
* Runtime logic

So the next step was to inspect the game files.

---

# 🔍 Step 1 — Identify the Build Type

Unity games usually use one of these scripting backends:

## 🔹 Mono Build

Game logic is stored in readable `.dll` files.

## 🔹 IL2CPP Build

Scripts are converted into native code, which is harder to reverse.

After checking the game files, we found:

```text
Assembly-CSharp.dll
```

This confirmed the challenge was a **Mono build**.

That is useful because Mono DLLs can be opened and analyzed easily with tools like **dnSpyEx**.

---

# 🛠️ Step 2 — Open the DLL

We opened the following file in **dnSpyEx**:

```text
Assembly-CSharp.dll
```

This file usually contains the custom C# scripts written by the developer.

---

# 🔎 Step 3 — Search for Interesting Keywords

Inside dnSpyEx, we searched for common CTF-related keywords such as:

* `flag`
* `SoterCTF`
* `card`
* `access`
* `final`

This quickly led to a suspicious class:

```csharp
public class final : MonoBehaviour
```

The class name `final` strongly suggested it was related to the ending screen or flag logic.

---

# 📜 Step 4 — Analyze the Code

Inside the class, the important function was:

```csharp
private void OnTriggerEnter2D(Collider2D collision)
{
    if (collision.CompareTag("Player"))
    {
        string text = this.DesencriptarFlag(this.flagEncriptada);
        this.finales.text = text;
        this.finales.gameObject.SetActive(true);
        this.fondo.SetActive(true);
    }
}
```

---

# 🔍 Understanding the Logic

When the player reaches the final trigger:

1. The game calls `DesencriptarFlag()`
2. It decodes the hidden flag
3. The result is shown on screen

This means the real flag was stored inside:

```csharp
flagEncriptada
```

---

# 🔐 Step 5 — Find the Encoded Flag

In the same script, we found:

```csharp
private string flagEncriptada =
"U290ZXJDVEZ7RDNmZW41RV9zMVNUM21fZEk1NWFCbGVkfQ==";
```

At first glance it looks encrypted, but we still needed to check the decryption function.

---

# 🧪 Step 6 — Check the “Decryption” Function

The function was:

```csharp
private string DesencriptarFlag(string flagEncriptada)
{
    byte[] bytes = Convert.FromBase64String(flagEncriptada);
    return Encoding.UTF8.GetString(bytes);
}
```

This is not real encryption.

It simply performs a **Base64 decode**.

---

# 🔓 Step 7 — Decode the String

Encoded value:

```text
U290ZXJDVEZ7RDNmZW41RV9zMVNUM21fZEk1NWFCbGVkfQ==
```

After decoding:

```text
SoterCTF{D3fen5E_s1ST3m_dI55aBled}
```

---

# ✅ Final Flag

```text
SoterCTF{D3fen5E_s1ST3m_dI55aBled}
```

---

# 💡 Why the Challenge Mentioned “Two Access Cards”

The challenge story talks about cards, hatches, and survival to create a fun space-themed scenario.

But technically, the actual solution only required:

* Inspecting the Unity DLL
* Finding the encoded flag
* Decoding Base64

The access cards were part of the game theme, not required for solving.

---

# 🧰 Tools Used

* **dnSpyEx** — Analyze .NET / Unity Mono DLL files
* **Base64 Decoder** — Decode the hidden string
* **Python** (optional)

Example:

```python
import base64

data = "U290ZXJDVEZ7RDNmZW41RV9zMVNUM21fZEk1NWFCbGVkfQ=="
print(base64.b64decode(data).decode())
```

---

# 📚 Key Takeaways

## 🔹 Identify the Game Engine

Unity files immediately provide useful clues.

## 🔹 Check Mono vs IL2CPP

Mono builds are easier to reverse.

## 🔹 Search Keywords

Terms like `flag`, `win`, `secret`, `final` often reveal the solution faster.

## 🔹 Encoded ≠ Encrypted

Many beginner challenges use Base64, hex, XOR, or ROT13.

---

# 🚀 Conclusion

This was a beginner-friendly Unity reverse engineering challenge.

Instead of solving it through gameplay, we inspected the game logic, found the encoded flag, decoded it, and recovered the answer.

A great reminder that understanding the application structure is often more useful than interacting with it directly.

---

# 🏁 Flag

```text
SoterCTF{D3fen5E_s1ST3m_dI55aBled}
```

[Uploading Assembly-CSharp.txt…]() 
<img width="1022" height="731" alt="image" src="https://github.com/user-attachments/assets/73ef461c-0fa5-4f51-ab17-adabf2bdb719" />

