# Ninja Skills

{% embed url="https://tryhackme.com/room/ninjaskills" %}

A quick Linux enumeration challenge. We get SSH access and twelve mystery files — `8V2L`, `bny0`, `c4ZX`, `D8B3`, `FHl1`, `oiMO`, `PFbD`, `rmfX`, `SRSq`, `uqyw`, `v2Vb`, `X1Uy` — and have to answer questions about them using the right command for each.

SSH in with:

```bash
ssh new-user@MACHINE_IP
# password: new-user
```

---

## Which files are owned by the `best-group` group?

```bash
ls -l 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

Check the group column (4th field). Files belonging to `best-group`:

```
D8B3  v2Vb
```

---

## Which file contains an IP address?

```bash
grep -rE '([0-9]{1,3}\.){3}[0-9]{1,3}' 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

**Answer: `oiMO`**

---

## Which file has SHA1 hash `9d54da7584015647ba052173b84d45e8007eba94`?

```bash
sha1sum 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

**Answer: `c4ZX`**

---

## Which file contains 230 lines?

```bash
wc -l 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

**Answer: `bny0`**

---

## Which file's owner has UID 502?

```bash
ls -ln 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

Look for `502` in the UID column.

**Answer: `X1Uy`**

---

## Which file is executable by everyone?

```bash
ls -l 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

Look for `-rwxr-xr-x` — execute bit set for owner, group, and others.

**Answer: `8V2L`**

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Which files are owned by the `best-group` group? | `D8B3 v2Vb` |
| Which file contains an IP address? | `oiMO` |
| Which file has SHA1 hash `9d54da7584015647ba052173b84d45e8007eba94`? | `c4ZX` |
| Which file contains 230 lines? | `bny0` |
| Which file's owner has UID 502? | `X1Uy` |
| Which file is executable by everyone? | `8V2L` |
