---
name: js-reverse
description: Frontend JavaScript reverse engineering (route R2). Use for locating frontend signature/encryption logic (webpack/vite bundles), analyzing encrypted request parameters, sourcemap recovery, JS obfuscation analysis, and HTTP capture/replay with browser devtools. Authorized targets only.
---

# Frontend JS reverse (R2)

## When to use
"前端签名" / encrypted params / request replay / webpack chunk analysis /
sourcemap recovery / JS obfuscation. No Burp needed on macOS — devtools +
curl/Node do the job.

## Tools (macOS)
- Node.js 22+: `brew install node` — runtime for local reproduction
- Browser devtools (Chrome/Firefox) — capture, pretty-print, breakpoints
- `curl` for replay; optional mitmproxy: `brew install mitmproxy`
- Sourcemap tools: `npx source-map` or devtools "Sources → map" panel
- deobfuscators: `npx js-beautify`, Babel-based scripts (write-your-own with
  `@babel/parser` + `@babel/traverse`)

## Workflow (ACTION REQUIRED)
1. **Scope**: authorized target only. Record target URL + capture method as E-*.
2. **Capture**: devtools Network tab (or mitmproxy) → save the request(s) that
   carry the params of interest. Note which script computed them.
3. **Locate**: in Sources, search for the param names / strings from the
   request (`Cmd+F` across files). Webpack: use the chunk names/ids in the
   network waterfall to find the module.
4. **Understand**: pretty-print, set breakpoints, read the function chain that
   produces the signature/encryption. Document with file:line + excerpts.
5. **Reproduce (the deliverable)**: rebuild the logic in a local Node script —
   same inputs in, same outputs out. This is the proof (E-*: input/output
   hashes match the captured request).
6. **Obfuscated?** → AST-based deobfuscation pass; save the technique to
   `skills/js-reverse/references/`.

## Boundaries
- Authorized targets only. Capturing session cookies / auth tokens is fine as
  evidence on your own or CTF targets; never replay against real users.
- Don't ship a working bypass for someone else's anti-bot without explicit
  authorization.

## Evidence
- Captured request/response files (sanitized) + the Node reproduction script +
  matching hashes = the evidence chain.
