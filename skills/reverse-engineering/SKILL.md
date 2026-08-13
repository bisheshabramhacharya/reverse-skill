---
name: reverse-engineering
description: General reverse engineering methodology (fallback route R0). Use for unknown binaries, anti-debug/anti-analysis, obfuscation (OLLVM/packers), source recovery from any binary, or any RE task that doesn't match a specific module (APK/JS/binary/malware). Static analysis first, dynamic second, evidence everywhere.
---

# General reverse engineering (R0)

## When to use
Unknown binary format, 逆向 / reverse of a non-APK target, anti-debug or
anti-analysis bypass, packer/unpacker work, source recovery from .bin/.dat,
game reversing. More specific modules win: APK→R1, JS→R2, elf/so/dylib→R3.

## Tools (macOS)
- radare2 / rizin: `brew install radare2`
- Ghidra: `brew install --cask ghidra`
- file, strings, xxd, otool, nm: built-in / Xcode CLT
- Python: pwntools (`pipx install pwntools`), capstone, angr (optional)
- Frida: `pipx install frida-tools` (dynamic, when static is blocked)

## Workflow (ACTION REQUIRED)
1. **Triage**: `file <target>`, `strings -a <target> | head`, entropy/`xxd` peek.
   Record sha256 + size as E-*. State: packed? stripped? arch?
2. **Static pass**: `r2 -A <target>` → info (arch/endian/compiler), imports,
   strings → functions. Or Ghidra headless: `analyzeHeadless proj -import <target> -scriptPath ...`.
3. **Locate the interesting surface** (from the task): follow xrefs from
   relevant strings/imports; document address/offset/function for every claim.
4. **Dynamic pass** (only when static is blocked or insufficient, and target is
   in scope): Frida (`frida -U -f com.x`), `lldb`, or a sandbox. Log calls.
5. **Synthesis**: write up algorithm/behavior with Evidence→Finding→Path;
   produce pseudo-code or a call graph when that's the deliverable.
6. Anti-debug/obfuscation encountered → search for the technique, save findings
   to `skills/reverse-engineering/references/` (or field-journal).

## Boundaries
- Only analyze targets listed in scope.md. Don't exfiltrate credentials found —
  document them as findings with severity instead.
- No-crack deliverable: understand and document, don't redistribute.

## Evidence
- Every claim about addresses/offsets/function names must cite E-* with the
  exact command that produced it (`repro_command`).
