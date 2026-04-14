# 🧩 Deep Fried Discovery — Writeup

## 📌 Challenge Information

* **Challenge Name:** Deep Fried Discovery
* **Category:** OSINT
* 
* **Final Flag:** `SoterCTF{Lluís_Domènech_i_Montaner}`

---

# 📝 Challenge Description

> Someone has tried to obscure the beauty of this architectural gem by trapping the image in a digital vortex. They believed that by altering the pixels, it would be impossible to recognize the exact location of this grand gathering. However, the style of its creator is so unique that not even the greatest distortion can erase its historical legacy.
>
> Your mission is to look beyond the chaos, identify this imposing hospital complex, and discover the mastermind behind its design.

---

# 🧠 Initial Analysis

This was an OSINT image-identification challenge.

The provided image was heavily distorted (“deep fried”), but the challenge text gave useful hints:

* Architectural landmark
* Large hospital complex
* Very distinctive design style
* Famous architect

So instead of focusing on damaged pixels, the better approach was to identify the structure through its overall architecture.

---

# 🔍 Step 1 — Recognize the Building Style

Even with distortion, some features were still visible:

* Decorative historic architecture
* Large institutional complex
* Strong artistic style
* European appearance

This suggested the location might be a famous cultural or historical building.

The mention of a “hospital complex” was especially important.

---

# 🔎 Step 2 — Reverse Image Search / Visual Matching

Using tools like:

* Google Lens
* Reverse image search
* Manual comparison of landmarks

The closest match was the famous Hospital de Sant Pau.

This site is a well-known hospital complex in Barcelona and a UNESCO World Heritage Site.

---

# 🏛️ Step 3 — Confirm the Match

The building matched the challenge image because of its recognizable characteristics:

* Red brick architecture
* Decorative towers and domes
* Symmetrical pavilion layout
* Catalan Modernisme style

These features are strongly associated with Hospital de Sant Pau.

---

# 👤 Step 4 — Identify the Architect

The challenge asked for the architect of the complex.

The celebrated architect behind Hospital de Sant Pau was Lluís Domènech i Montaner.

He was one of the most important figures of Catalan Modernisme.

He is also known for designing:

* Palau de la Música Catalana
* Several landmark buildings in Barcelona

---

# 🚩 Final Flag

```text id="v8r3mx"
SoterCTF{Lluís_Domènech_i_Montaner}
```

---

# 🛠️ Tools Used

* Google Lens
* Reverse image search
* Architecture references
* Visual comparison

---

# 🧠 Key Insights

## 🔹 Distortion Does Not Hide Identity

Even edited images keep structural clues.

## 🔹 Read the Description Carefully

The words “hospital complex” narrowed the search quickly.

## 🔹 Famous Styles Are Recognizable

Catalan Modernisme has a very distinct appearance.

## 🔹 OSINT Is Often Pattern Matching

You do not always need metadata or code.

---

# 📚 Lessons Learned

* Landmark recognition is a valuable OSINT skill
* Search engines can solve many image-based tasks quickly
* Architecture knowledge can be a strong advantage
* Small hints in the description often matter most

---

# 🚀 Conclusion

This challenge relied on identifying a distorted image of a famous architectural site.

By focusing on visible design features and using reverse image search, the location was identified as Hospital de Sant Pau in Barcelona. From there, the architect was easy to confirm, leading to the final flag.

---

# 🏁 Flag

```text id="x2k7np"
SoterCTF{Lluís_Domènech_i_Montaner}
```
![original](https://github.com/user-attachments/assets/c530ad88-17d3-40d6-bbbc-8e28a4c541b1)
<img width="1688" height="953" alt="image" src="https://github.com/user-attachments/assets/4fe6314d-5215-44f8-b39b-dc0cf6df1ca5" />
