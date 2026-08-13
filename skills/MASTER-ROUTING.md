# reverse-skill MASTER-ROUTING (R0–R11)

> Mirrors `config/routing.json` (single source of truth). `master-route.sh` is
> the executable version: `bash skills/scripts/master-route.sh --hint "<task>"`.
> Priority order = tie-break order. No keyword match → R0.

## Execution contract

1. Route first, then act. Output the PRIMARY route + one-line rationale.
2. `case-init.sh` → `work/<case>/scope.md`. No target ACT before
   `auth.status=granted` + `network_profile` (see `ops/scope-contract.md`).
3. Open PRIMARY module SKILL.md → follow its workflow.
4. Tool paths only from `tool-index.md`; missing tools → install → refresh index.
5. Append `timeline.md` / `workitems.md`; conclusions via Evidence→Finding→Path.
6. No route matched → open `RULES.md` mode detection or propose a new route.

## Priority table (high → low)

| ID | Condition (keyword hits) | PRIMARY module |
|----|--------------------------|----------------|
| R1 | APK / smali / jadx / apktool / Android / 安卓 / Frida / root-detect / cert pinning | `apk-reverse/` |
| R2 | JS reverse / 前端签名 / 加密参数 / webpack / sourcemap / 抓包 / request replay | `js-reverse/` |
| R3 | radare2 / ghidra / 反汇编 / elf / so / dylib / mach-o / 二进制 / unpack / ollvm | `binary-re/` |
| R4 | malware / yara / 恶意样本 / 沙箱 / ransomware / rootkit | `malware-analysis/` |
| R5 | pentest / nmap / nuclei / sqlmap / ffuf / 端口扫描 / burp / bug bounty | `pentest-tools/` |
| R6 | pwn / ROP / ret2libc / 栈溢出 / heap overflow / UAF / pwntools | `pwn-chain/` |
| R7 | firmware / 固件 / binwalk / squashfs / IoT / router / UART | `firmware-pentest/` |
| R8 | CTF / 靶场 / writeup / 攻防 / AWD / flag | `ctf/` |
| R11 | teardown / 拆解 / 竞品 / competitor / open-source analysis / repo analysis | `teardown/` |
| R10 | case review / evidence / 证据链 / scope review | `case-review/` |
| R9 | report / 报告 / writeup / 技术文档 | `docs-generator/` |
| R0 | generic reverse / 逆向 / decompile / 反调试 / unknown binary | `reverse-engineering/` |

## Mode boundary

- Task is "understand software we did not write, to build our own" → **R11
  teardown** (read-only, file:line evidence) — even if it also matches R0/R3.
- Pure CTF multi-category orchestration → R8, which chains the per-category
  quick wins itself.

## Read order

```text
RULES.md → MASTER-ROUTING.md → PRIMARY SKILL.md
        → (optional) ops/* contracts
        → tool-index.md → refresh if missing → ACT
```
