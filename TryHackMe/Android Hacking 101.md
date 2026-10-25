# Android Hacking 101: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/androidhacking101

## Room Overview & Information

**Category:** Mobile application security  
**Difficulty:** Beginner  
**Focus:** Android application structure, ADB, APK inspection, permissions, application data, and safe dynamic testing.

Android security testing examines the application package, the permissions it requests, its local storage, network communication, and the way it interacts with the operating system. The room's emulator and sample applications are the intended test environment. Do not install untrusted APKs on your everyday phone.

## Phase 1: Understand the APK

An APK is an Android application package. It commonly contains the manifest, compiled bytecode, resources, assets, and signing information. The manifest declares components, permissions, and application metadata.

Useful tools include:
- **ADB:** communicates with an Android device or emulator.
- **aapt or apkanalyzer:** inspects package metadata and resources.
- **Apktool:** decodes resources and disassembles parts of an APK.
- **JADX:** decompiles DEX bytecode into readable Java-like source.
- **MobSF:** automates several static and dynamic analysis checks.

Automated tools produce leads, not a final verdict. Decompiled output may be incomplete or misleading, and an insecure-looking API call may be unreachable or protected by another control.

## Phase 2: Connect to the lab device

Use the Android emulator provided by the room. Confirm the connected device:

```bash
adb devices
```

If the device appears as unauthorized, follow the lab's instructions to approve the connection. If no device appears, confirm that the emulator is running and that the correct ADB connection is configured.

Useful ADB commands:

```bash
adb shell
adb shell getprop ro.build.version.release
adb shell pm list packages
```

- **adb shell** opens a shell on the device.
- **getprop** reads Android system properties.
- **pm list packages** lists installed packages.

Package names and paths depend on the lab device, so use the values from the current instance.

## Phase 3: Inspect application permissions

The Android manifest may declare permissions for network access, storage, camera, location, contacts, or other sensitive capabilities. A permission request is not automatically a vulnerability, but the application should request only what it needs and should handle permission denial safely.

Inspect the APK using the tool provided by the room. Look for exported activities, services, receivers, and content providers. An exported component can be invoked by other apps unless the platform or application restricts it appropriately.

For each sensitive permission, ask:
- Does the app need this permission for its stated function?
- Is access requested at the correct time?
- Is sensitive data stored or transmitted securely?
- Can another app invoke an exported component?
- Does the app enforce authorisation in code rather than trusting the UI?

## Phase 4: Static analysis

Preserve the original APK and calculate a hash before analysis:

```bash
sha256sum sample.apk
file sample.apk
```

Use JADX or Apktool to inspect the package. Search for hard-coded API keys, endpoints, debug settings, insecure cryptography, unsafe WebView settings, and sensitive data written to logs or local files.

A string search is a starting point. Verify whether the value is a real secret, whether it is active, and whether the code path uses it. Do not publish live credentials or tokens found in a sample.

## Phase 5: Dynamic analysis

Run the sample only in the room's emulator. Observe app behaviour, network requests, permission prompts, logs, and filesystem changes. ADB can display logs:

```bash
adb logcat
```

Stop the stream with Ctrl+C. Logs may contain personal data or tokens, so handle them carefully.

If the room includes a test account, use only that account. Capture requests through the lab's authorised proxy if the task requires it. Do not intercept traffic from other devices.

## Phase 6: Common mobile vulnerabilities

- Hard-coded secrets in the APK.
- Sensitive data stored in plaintext.
- Excessive permissions.
- Exported components without proper access controls.
- Insecure network communication.
- WebView configuration that permits unsafe content.
- Debug builds or verbose logging in production.
- Weak local authentication or insecure session handling.
- Trusting client-side checks that can be modified.

## Phase 7: Working through the room

Follow the room's exact task order and inspect the provided app or emulator. If a question asks for a package name, permission, component, or string, copy it from the current sample. Do not substitute a value from another APK version.

## Phase 8: Remediation

Request minimal permissions, protect local data using platform-supported storage and cryptography, enforce authorisation server-side, validate network certificates, disable unnecessary exported components, remove debug functionality from release builds, and avoid logging secrets. Sign release artifacts and maintain a process for updating vulnerable dependencies.

## Key takeaways

Android testing combines package inspection with runtime observation. Use the emulator, inspect the manifest and code, verify the actual behaviour, and record evidence. Never assume a decompiled code fragment proves exploitability without confirming the execution path and its security impact.

## Expanded technical workflow: Android assessment

### Lab boundaries and device inventory

Use only the emulator or device supplied by the room. Record the Android version, API level, architecture, package name, and whether the device is rooted. In Android testing, these details matter because API behaviour, available commands, and security controls vary by version.

With ADB installed and the lab device connected, check the connection and device details:

```bash
adb devices -l
adb shell getprop ro.build.version.release
adb shell getprop ro.build.version.sdk
adb shell getprop ro.product.cpu.abi
adb shell id
```

`adb devices -l` lists connected devices; the `getprop` calls query Android system properties; `id` shows the shell's current identity. If multiple devices appear, use `adb -s SERIAL` to select the intended emulator. Do not continue if the connected device is not the lab target.

### Package and component enumeration

List installed packages and identify the target package from the room:

```bash
adb shell pm list packages
adb shell dumpsys package PACKAGE_NAME
```

The package dump can reveal version information, requested permissions, activities, services, receivers, and providers. It is a large output, so save it and search locally rather than relying on a few visible lines:

```bash
adb shell dumpsys package PACKAGE_NAME > package-dump.txt
grep -iE 'permission|activity|service|receiver|provider|version' package-dump.txt
```

Use the exact package name from the app under test. A declared permission is not proof that the app uses it unsafely; determine which feature requests it and whether the permission is justified.

### Inspect the APK safely

If the room provides an APK, preserve the original and calculate its hash before analysis. Use a static-analysis tool such as JADX to inspect decompiled code and AndroidManifest.xml. Look for exported components, deep links, hard-coded endpoints, insecure storage, WebView settings, and trust decisions made only on the client. Decompilation is an aid to analysis, not a perfect reconstruction of the original source.

For dynamic analysis, launch the app in the lab and observe its network and file behaviour. Do not enter real credentials or private information into a test app. If the task requires an intercepting proxy, install and trust its certificate only in the isolated test environment and follow the room's certificate-pinning guidance.

### Validate findings

For every suspected issue, record the component, input, expected access policy, observed behaviour, and evidence. An exported activity is not automatically exploitable if it performs proper authorization checks. A hard-coded string is not automatically a secret unless it grants access or exposes sensitive information. Confirm the impact with the smallest safe test and revert any lab changes afterward.

### Common troubleshooting

- If ADB reports no device, check that the emulator is running and that the correct ADB server is active.
- If a package cannot be found, verify the package identifier rather than the visible app label.
- If an activity refuses to launch, it may require authentication or an expected intent extra.
- If proxy traffic is missing, confirm the app's proxy behaviour and the emulator's network route.
- If static analysis reveals a suspicious function, trace callers and server-side checks before declaring a vulnerability.

### Reporting and remediation

Recommend least-privilege permissions, non-exported components unless cross-app access is required, explicit authorization for sensitive operations, secure storage for tokens, TLS with correct certificate validation, and server-side enforcement of business rules. Avoid exposing production secrets in APK resources or logs. Use the room's live task to obtain exact answer values and redact flags as `flag{hidden}` in public notes.
