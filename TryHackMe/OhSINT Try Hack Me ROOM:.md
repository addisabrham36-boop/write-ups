# OhSINT

Welcome to the OhSINT room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/ohsint" %}

***

OhSINT is a classic Open-Source Intelligence (OSINT) challenge where we are provided with a single photograph (`WindowsXP.jpg`). By extracting metadata and cross-referencing public information across social profiles, Wi-Fi geolocation databases, GitHub, and personal blogs, we reconstruct the target's identity and activities.

---

## 1. Extracting EXIF Metadata

We start by analyzing the metadata of the provided image file (`WindowsXP.jpg`) using `exiftool`:

```bash
exiftool WindowsXP.jpg
```

### Relevant Output:
```text
File Name                       : WindowsXP.jpg
Directory                       : .
File Size                       : 219 kB
File Type                       : JPEG
MIME Type                       : image/jpeg
Exif Byte Order                 : Big-endian (Motorola, MM)
Image Description               : Windows XP wallpaper
Artist                          : Oliver Woodflint
Copyright                       : OWoodflint
```

The copyright and artist fields reveal a unique handle: **`OWoodflint`**.

---

## 2. Social Media & Digital Footprint Reconnaissance

We search for the username `OWoodflint` across search engines and social platforms.

### Finding 1: Twitter / X Profile
Searching for `OWoodflint` leads to an account on Twitter/X (`@OWoodflint`):
* **Avatar:** Looking closely at the profile picture, the user's avatar is a **cat**.
* **Post Clue:** One of the tweets mentions a BSSID (MAC address of a Wi-Fi access point):
  ```text
  B4:5D:50:AA:86:41
  ```

---

## 3. Wi-Fi Geolocation with WiGLE

We take the BSSID (`B4:5D:50:AA:86:41`) and search for it on **WiGLE.net** (Wireless Geographic Logging Engine) under the *Basic Search -> BSSID* field.

### WiGLE Search Results:
* **SSID (Network Name):** `UnileverWiFi`
* **Location Coordinates:** Points directly to **London**, United Kingdom.

---

## 4. GitHub Investigation & Personal Blog

Searching for `OWoodflint` on GitHub brings up the user's GitHub profile (`github.com/OWoodflint`):
* Looking at the repositories and user profile info, we discover the personal email address:
  ```text
  OWoodflint@gmail.com
  ```

Additionally, searching for `OWoodflint` leads to a personal WordPress blog. Reading through the blog posts:
* A blog post states that he is currently traveling on holiday in **New York**.

---

## 5. Blog Source Code Inspection

Inspecting the HTML source code (`Ctrl + U`) of the WordPress blog post:
Hidden inside the page with CSS styling (`color: white; background-color: white;`):

```html
<p style="color: white;">pennYDr0pper.!</p>
```

The hidden password string is: **`pennYDr0pper.!`**

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **What is this user's avatar of?** | `cat` |
| **What city is this person in?** | `London` |
| **What is the SSID of the WAP he connected to?** | `UnileverWiFi` |
| **What is his personal email address?** | `OWoodflint@gmail.com` |
| **What site did you find his email address on?** | `GitHub` |
| **Where has he gone on holiday?** | `New York` |
| **What is the person's password?** | `pennYDr0pper.!` |

***
