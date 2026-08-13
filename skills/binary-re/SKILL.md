---
name: binary-re
description: Binary reverse engineering with radare2 and Ghidra (route R3). Use for ELF/so/dylib/exe/dll analysis, disassembly, decompilation, import/export tables, symbol recovery, packer identification and unpacking, Go/Rust binary recognition. IDA Pro is NOT required — these tools cover it.
---

# Binary reverse (R3) — radare2 + Ghidra

## When to use
elf / .so / .dylib / .exe / .dll / mach-o analysis, 反汇编, stripped binaries,
packed binaries, Go/Rust binaries, cross-version diffing. Replaces the
upstream's IDA/radare2/ghidra trio with one macOS module.

## Tools (macOS)
- radare2: `brew install radare2` — `r2`, `rabin2`, `rasm2`, `radiff2`
- Ghidra: `brew install --cask ghidra` — GUI or headless
  (`analyzeHeadless proj -import target -scriptPath scripts`)
- lldb / gdb (`brew install gdb`) for dynamic; `otool`, `nm`, `codesign` (Xcode CLT)
- binary diffing: `radiff2` (or Diaphora/Ghidriff for Ghidra)

## Workflow (ACTION REQUIRED)
1. **Triage**: `file <target>`; `rabin2 -I <target>` (arch, endian, bits,
   compiler, stripped?); sha256 → E-001.
2. **Surface**: `rabin2 -i` (imports), `rabin2 -s` (symbols), `rabin2 -z`
   (strings). Pick the imports/strings that match the task question.
3. **Disassemble/Decompile**: `r2 -A <target>` → `afl`, then `pdf @ main` /
   `s <addr>; pdd` (or Ghidra decompile). Follow xrefs: `axt @ <addr>`.
4. **Packed?** → identify packer (entropy, section names), then decide: unpack
   with known tooling or go dynamic (break on section unpack / API calls).
5. **Go/Rust?** → recognize runtime (Go: `runtime.*` symbols, pclntab; Rust:
   `_ZN…` mangling), use the matching helper (`github.com/mandiant/GoReSym`
   for Go symbol recovery).
6. **Synthesis**: function-level understanding with addresses/offsets cited.
   Diffing: `radiff2 -A old new` for N-day-style work.

## Boundaries
- Only in-scope binaries. Malware samples → R4 route instead (or hand off).
- Documented understanding only; no redistribution of proprietary binaries.

## Evidence
- Every address/offset/function name must trace to an E-* with `repro_command`
  (the exact r2/rabin2/Ghidra command).
