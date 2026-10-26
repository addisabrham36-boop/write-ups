# Advent of Cyber 2023 - Day 5: DOS & Signatures

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Investigating legacy MS-DOS environments, inspecting corrupted backup archives, and repairing damaged file headers (magic bytes) with a hex editor.

## Walkthrough
1. Boot into the MS-DOS emulation environment.
2. Check file details of the damaged backup archive:
   ```dos
   dir AC2023.BAK
   ```
   The file size is displayed as `12704` bytes.
3. The executable used to process this backup is identified as `backupmaster3000`.
4. Inspecting `AC2023.BAK` with a hex editor reveals the first two bytes are corrupted (`XX XX`).
5. Real AC-format backups require magic bytes corresponding to ASCII characters `A` and `C` in hexadecimal: `41 43`.
6. Patch the first two bytes to `41 43`, save the file, and rerun `backupmaster3000 AC2023.BAK` to restore the archive and reveal: `THM{0LD_5CH00L_C00L_d00D}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| How large (in bytes) is the AC2023.BAK file? | `12704` |
| What is the name of the backup program? | `backupmaster3000` |
| Correct bytes in the backup's file signature to restore it? | `41 43` |
| Flag received after restoring the backup? | `THM{0LD_5CH00L_C00L_d00D}` |
