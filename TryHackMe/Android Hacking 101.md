# Android Hacking 101: TryHackMe Walkthrough

Room: https://tryhackme.com/room/androidhacking101

Android security testing involves understanding the application package, the device environment, app permissions, local storage, network communication, and platform controls. Work only with the emulator, APK, or device explicitly provided by the training room.

## 1. Prepare the lab

Read the room prerequisites and use its supplied emulator or test device. Keep personal accounts, private files, and everyday devices out of the exercise. If the room uses ADB, confirm that the connected device is the intended lab target before running commands.

Useful read-only ADB checks include:

    adb devices
    adb shell getprop ro.build.version.release
    adb shell pm list packages

The first lists connected devices, the second reports the Android release, and the third lists installed packages. Output varies by device. If more than one device is connected, specify the correct serial as the room instructs.

## 2. Understand APK structure

An APK contains the manifest, compiled application code, resources, assets, and potentially native libraries. The manifest identifies the package and describes permissions and components. Static analysis can reveal exported activities, services, broadcast receivers, content providers, deep links, and network configuration.

A tool such as JADX can help inspect decompiled code, while APK-specific utilities can inspect the manifest and package metadata. Use the tools provided by the room and avoid treating decompiled output as a perfect copy of the original source.

## 3. Review permissions and exposed components

Compare requested permissions with the app's purpose. Look for sensitive capabilities such as contacts, location, microphone, camera, SMS, accessibility, or broad file access. A permission may be legitimate, but unexplained or excessive access deserves review.

Exported components can be reachable by other applications. Check whether sensitive actions require permissions or enforce caller identity. The fact that a component is exported does not automatically mean it is vulnerable; the security impact depends on the action and access checks.

## 4. Inspect storage and network behaviour

Apps may store preferences, databases, cached files, or tokens locally. Assess whether sensitive data is stored in a protected location and whether logs expose secrets. On a test device, use the lab's approved methods to inspect files and app state.

For network behaviour, record destination hosts, protocols, certificate validation, and the types of data transmitted. Do not intercept personal traffic or bypass certificate checks outside a controlled exercise. HTTPS should be validated correctly, and secrets should not be sent over cleartext connections.

## 5. Dynamic testing

Launch the app in the lab and observe how it behaves under normal use. Monitor logs, requests, permission prompts, and state changes. Then perform only the specific test cases required by the room, recording the input, observed response, and expected secure behaviour.

If the app behaves differently in an emulator, note the limitation. Some applications detect test environments, and a lack of suspicious activity does not prove the APK is safe.

## 6. Common weaknesses and defences

- Excessive permissions: request only what is required and explain why.
- Insecure storage: protect sensitive values and avoid storing secrets unnecessarily.
- Weak component controls: restrict exported components and validate callers.
- Insecure network handling: use TLS and validate certificates.
- Hard-coded secrets: move secrets to protected services and rotate exposed credentials.
- Unsafe WebViews or dynamic loading: restrict navigation, validate content, and load code only from trusted sources.

## 7. Working through the room

Follow the current task instructions and use the lab's exact package or device. When asked for a value, obtain it from the manifest, tool output, or observed behaviour. Do not assume an answer from another Android app will match this exercise.

## What I took from this room

Android testing is a combination of static inspection, runtime observation, and understanding platform security. The strongest conclusions come from connecting a specific configuration or code path to a verified behaviour and a realistic impact.