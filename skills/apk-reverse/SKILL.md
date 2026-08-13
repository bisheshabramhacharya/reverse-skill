---
name: apk-reverse
description: Android APK reverse engineering (route R1). Use for APK unpacking, Java decompilation with jadx, smali analysis/modification, repackaging and resigning, Frida dynamic hooking, root detection and certificate pinning bypass analysis, and native .so analysis. Authorized targets only (own apps, CTF, lab).
---

# APK reverse (R1)

## When to use
APK/AAB tasks: decompile, 反编译, smali edits, repack + sign + install,
Frida hooks, root detection / cert pinning analysis, .so native analysis.

## Tools (macOS)
- jadx: `brew install jadx` (Java decompile to sources)
- apktool: `brew install apktool` (decode resources + smali)
- adb + platform-tools: `brew install --cask android-platform-tools`
- Frida: `pipx install frida-tools`; server on device/emulator
- JDK: `brew install openjdk`

## Workflow (ACTION REQUIRED)
1. **Scope**: `case-init --preset offline-sample --sample app.apk` (own sample)
   or ctf-public. Record sha256 of the APK as E-001.
2. **Unpack**: `jadx -d out/ app.apk` (readable Java) — primary path.
   `apktool d app.apk -o smali_out/` when you need smali/resources/manifest.
3. **Manifest summary**: package, permissions, components,
   `android:debuggable`, exported components, backup flags.
4. **Locate task surface**: search jadx output for the interesting strings /
   classes (e.g. `grep -rn "sign" out/`). Follow the call chain. Every claim
   cites `out/…/File.java:line`.
5. **Native**: if `.so` involved, hand the .so to R3 (binary-re) with the JNI
   method names from jadx.
6. **Dynamic (optional, in scope)**: `frida-ps -U`, `frida -U -f <pkg> -l hook.js`.
   Root/pinning bypass analysis = document the checks found, don't ship a
   "cracked" APK unless the task explicitly asks and the target is ours/CTF.
7. **Repack** (only when required): `apktool b`, `zipalign`, `apksigner sign`
   — then evidence the hash of the result.

## Boundaries
- Decompiling someone else's app for redistribution or bypassing payment is
  illegal. Our playbook is for analysis: learning, CTF, own apps, authorized
  security review.
- Repackaging = only for own/CTF targets, and never redistribute.

## Evidence
- jadx/apktool commands are `repro_command`; findings cite file:line in jadx
  output; Frida hook logs go to `evidence/` files with their own sha256.
