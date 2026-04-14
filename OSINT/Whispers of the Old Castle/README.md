# 🧩 Whispers of the Old Castle — Writeup

## 📌 Challenge Information

* **Challenge Name:** Whispers of the Old Castle
* **Category:** OSINT

---

# 📝 Challenge Description

> An anonymous user has uploaded an aerial photograph of a historic estate somewhere in Europe to a public platform. No EXIF metadata is available: the image has been processed by the service where it was published, removing any technical traces that could aid the investigation.
>
> Even so, the photograph contains enough visual and contextual elements to reconstruct its origin. Your mission is to identify key information related to the location, its history, and the person who posted the image. Everything you need is in the image and your investigative skills.

---

# 🧠 Initial Analysis

This was a multi-stage OSINT challenge. Instead of finding one answer, we had to solve four connected tasks:

1. Identify the country
2. Identify the castle
3. Find the current owning family
4. Find the username of the aerial image uploader

No metadata was available, so the solve depended entirely on visual clues, search techniques, and cross-referencing public sources.

---

# 🔍 Step 1 — Identify the Country

The architecture, vegetation, and surrounding environment suggested southern Europe.

The style of the estate matched properties commonly found in Spain, especially Catalonia.

### ✅ Answer

```text id="a7m2kd"
SoterCTF{España}
```

---

# 🏰 Step 2 — Identify the Castle

Using reverse image search and manual comparison of satellite views, the estate matched:

Castillo Sant Marçal

Also known as Castell de Sant Marçal.

Important matching details:

* Main building layout
* Towers and facade shape
* Garden symmetry
* Road access pattern

### ✅ Answer

```text id="q9v5tn"
SoterCTF{Castillo_Sant_Marçal}
```

---

# 👨‍👩‍👧‍👦 Step 3 — Current Owning Family

After identifying the property, the next step was checking public history records and ownership references.

The castle is currently associated with the Trénor family.

### ✅ Answer

```text id="p4r8ex"
SoterCTF{Familia_Trénor}
```

---

# 🛰️ Step 4 — Find the Aerial Image Author

This was the hardest part.

After locating the castle on mapping platforms:

* Google Maps
* Satellite view
* Photos / 360 imagery
* Contributor uploads

The exact aerial view could be matched to a user contribution.

The uploader name was:

```text id="k6z1lw"
matauira_chin_ah_you
```

### ✅ Answer

```text id="n8c3mf"
SoterCTF{matauira_chin_ah_you}
```

---

# 🚩 Final Flags

```text id="t1x5qy"
SoterCTF{España}
SoterCTF{Castillo_Sant_Marçal}
SoterCTF{Familia_Trénor}
SoterCTF{matauira_chin_ah_you}
```

---

# 🛠️ Tools Used

* Google Lens
* Reverse image search
* Google Maps
* Satellite view
* Public history sources
* OSINT cross-referencing

---

# 🧠 Key Insights

## 🔹 Break Big Problems into Small Ones

Solving one task revealed clues for the next.

## 🔹 No Metadata ≠ No Information

Visual clues alone can be enough.

## 🔹 Mapping Platforms Are Powerful

Satellite images and user uploads can reveal both places and people.

## 🔹 Verification Matters

Always confirm with multiple sources before submitting.

---

# 📚 Lessons Learned

* OSINT often works through chaining discoveries
* Reverse image search is only the beginning
* Historical context can confirm locations
* User attribution can be hidden in map/photo platforms

---

# 🚀 Conclusion

This challenge combined geolocation, historical research, and attribution OSINT into one investigation.

By identifying the estate as Castillo Sant Marçal, researching ownership records, and tracing the aerial image source, all four flags were recovered successfully.

---
<img width="1813" height="972" alt="image" src="https://github.com/user-attachments/assets/78dc235d-acbc-46e8-83c7-16d82f12c14e" />
![download](https://github.com/user-attachments/assets/c0420097-861c-4034-a0ff-64bfc546a17e)
