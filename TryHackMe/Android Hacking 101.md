# Android Hacking 101

{% embed url="https://tryhackme.com/room/androidhacking101" %}

An introduction to Android application security, APK packaging structure, static reverse engineering with `apktool` and `dex2jar`, Android Debug Bridge (ADB), and OWASP Mobile Top 10 vulnerabilities.

---

## 1. Android Package (APK) Anatomy

An APK is a zip archive containing compiled code, resources, and configuration manifests:
- **`AndroidManifest.xml`:** Root configuration declaring permissions (`uses-permission`), components (activities, services, content providers, broadcast receivers), and security flags (`android:debuggable`, `android:exported`).
- **`classes.dex`:** Dalvik Executable bytecode executed by Android Runtime (ART / Dalvik).
- **`res/` & `assets/`:** UI layouts, strings (`strings.xml`), images, and embedded application assets.
- **`lib/`:** Architecture-specific compiled native C/C++ libraries (`.so` files).
- **`META-INF/`:** Cryptographic signatures verifying package integrity (`CERT.RSA`, `CERT.SF`).

---

## 2. Reverse Engineering Toolchain

- **`apktool`:** Disassembles APK resources and converts `.dex` bytecode into human-readable Smali:
  ```bash
  apktool d target.apk -o decompiled_app/
  ```
- **`dex2jar` / `d2j-dex2jar`:** Translates `classes.dex` into a standard Java `.jar` archive.
- **`JD-GUI` / `JADX`:** Decompiles Java `.class` files into readable Java source code for auditing hardcoded secrets and API endpoints.
- **`adb` (Android Debug Bridge):** Command-line utility to interface with physical devices and emulators:
  ```bash
  adb devices
  adb install target.apk
  adb shell
  ```

---

## 3. Common Security Flaws

- **Insecure Data Storage:** Storing unencrypted credentials, authentication tokens, or PII inside `SharedPreferences` or SQLite databases located in `/data/data/<package_name>/`.
- **Exported Components:** Unintentionally setting `android:exported="true"` on sensitive activities or content providers without permission checks, allowing rogue apps on the device to invoke them.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Structure** | Manifest file holding permissions and app metadata | `AndroidManifest.xml` |
| **Structure** | File holding compiled Dalvik bytecode | `classes.dex` |
| **Tool** | Utility used to decompile resources to Smali | `apktool` |
| **Tool** | Framework translating dex files to jar format | `dex2jar` |
| **CLI** | Bridge utility used to communicate with Android devices | `adb` |
