# OhSINT: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/ohsint

## Room Overview & Information

**Difficulty:** Beginner  
**Category:** Open-source intelligence (OSINT)  
**Objective:** Investigate one supplied image and correlate publicly available information from image metadata, social profiles, Wi-Fi mapping, a personal blog, GitHub, and HTML source.

OSINT does not require breaking into a system. The challenge is to combine public clues while checking that each conclusion is supported by the source. The task values below are from the commonly documented room version; the room may update its supplied artifact, so verify them against the file you actually receive.

## Phase 1: Evidence acquisition and passive reconnaissance

### 1. Start the room and locate the image

Start the AttackBox or connect through the supported lab VPN. Open the supplied room directory and identify the image file:

    pwd
    ls -la
    file WindowsXP.jpg

The first two commands establish the working directory and list files. The file command checks the actual file format rather than trusting the extension. The room's file name may include an instance-specific suffix, so use the exact local name shown by ls.

### Question 1: “What is this user’s avatar of?”

Open the image in the lab and inspect it:

    xdg-open WindowsXP.jpg

The image depicts the classic Windows XP “Bliss” wallpaper, while the avatar associated with the identified social profile is a cat. The exact room question refers to the user's avatar, not merely the wallpaper in the initial image. The answer in the documented room version is **cat**.

## Phase 2: Image metadata and identity correlation

### 2. Inspect image metadata

Use ExifTool:

    exiftool WindowsXP.jpg

ExifTool can display file type, dimensions, timestamps, camera metadata, GPS coordinates, and copyright fields. In the room's image, the copyright field contains the identifier **OWoodflint**. This is a lead, not by itself proof of the person's real identity.

If ExifTool is not installed, install it through your distribution's trusted package manager or use the platform's preconfigured tool. Do not upload private photographs to a public analysis service without permission.

### 3. Search the public identifier

Search the exact identifier on a search engine and inspect results that appear to match. The room uses a public X/Twitter profile, a GitHub account, and a WordPress blog. Verify the context of each result instead of assuming a similar username belongs to the same person.

Keep a small evidence table:

| Clue | Source | What it supports |
|---|---|---|
| Copyright field | Local image metadata | A possible username |
| Avatar | Matching public profile | The avatar description |
| BSSID | Public social post | A network mapping lead |
| Email | Public GitHub page | An address and publishing platform |
| Holiday | Public blog | The destination stated in the post |
| Password clue | HTML source of public blog | A value hidden by page styling |

## Phase 3: Geolocation and public-profile analysis

### Question 2: “What city is this person in?”

The public profile mentions a BSSID, **B4:5D:50:AA:86:41**. A BSSID identifies a particular access-point radio, usually represented by its MAC address. Search the BSSID using the wireless mapping service indicated by the room, such as WiGLE, then confirm the map location.

The documented room answer is **London**. Do not claim that a BSSID lookup proves a person's current physical location: the data describes the mapped access point, can be stale, and can be imprecise. For the challenge, the intended inference is based on correlation with the other profile clues.

### Question 3: “What is the SSID of the WAP he connected to?”

On the same wireless mapping record, inspect the network's SSID field. The documented answer is **UnileverWiFi**.

SSID means Service Set Identifier, the network name. It is different from the BSSID, which identifies the access-point radio. Keep the two concepts separate when answering.

### Question 4: “What is his personal email address?”

Return to the public identifier and inspect the matching GitHub account and repository README. The documented email is **OWoodflint@gmail.com**. Record the source page and make sure the address is associated with the same identifier found in the image metadata.

### Question 5: “What site did you find his email address on?”

The address appears on **GitHub** in the documented room version. The question asks for the website where the address was published, not the email provider or domain.

### Question 6: “Where has he gone on holiday?”

Inspect the matching personal blog and locate the post text about a holiday. The documented answer is **New York**. Cite the blog post as the evidence, rather than inferring travel from the wireless location or the user's email address.

## Phase 4: HTML source inspection

### Question 7: “What is the person’s password?”

Inspect the source of the public blog page in the browser. Use View Page Source or Developer Tools, then search the HTML for suspicious text, hidden styling, or content not visible in the normal page rendering.

In the documented room version, the page contains a password-like value styled with white text on a white background. CSS presentation does not make the value secret; anyone who can access the HTML can inspect it. The documented answer is **pennYDr0pper.!**.

This illustrates information disclosure through client-side content. A password should not be included in public HTML in any form. Hiding a string using CSS, comments, JavaScript, Base64, or unusual formatting is not access control.

## Phase 5: Evidence quality, ethics, and remediation

### How to keep the investigation reproducible

1. Preserve the original image and record its exact filename.
2. Record the metadata field that revealed the initial identifier.
3. Save the source URL for each public clue and note when it was observed.
4. Correlate at least two clues before treating two profiles as belonging to the same person.
5. Separate the fact observed in a source from the inference made from it.
6. Avoid collecting personal information beyond what the authorized room asks for.

### Room answer summary

| Exact question | Answer in the documented room version | Evidence |
|---|---|---|
| What is this user’s avatar of? | Cat | Matching public social profile |
| What city is this person in? | London | BSSID mapping and cross-profile context |
| What is the SSID of the WAP he connected to? | UnileverWiFi | Wireless mapping record |
| What is his personal email address? | OWoodflint@gmail.com | Public GitHub repository |
| What site did you find his email address on? | GitHub | Source of the published email |
| Where has he gone on holiday? | New York | Public blog post |
| What is the person’s password? | pennYDr0pper.! | Hidden-styled text in public HTML source |

### Defensive lessons

- Remove unnecessary metadata from images and documents before publishing them.
- Avoid reusing unique identifiers across unrelated accounts when privacy matters.
- Never publish passwords, API keys, or credentials in public repositories, page source, CSS, or HTML comments.
- Apply authentication and authorization at the server; CSS visibility is not a security control.
- Treat geolocation databases as imperfect evidence and avoid overclaiming certainty.

The point of OhSINT is to demonstrate how small public clues can be correlated into a detailed profile. Use the process responsibly and keep conclusions limited to the evidence available.
