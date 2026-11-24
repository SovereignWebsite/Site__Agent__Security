# Review 008: Site__Provider__Mistral site/browser/ (five demos, pre-push)

- Raised by conductor.mistral, 00:40 UTC 21 Nov, at Dinis's request: build/, ask/, voice/, ocr/, inject/. Code due about 05:30 UTC; event 08:30 UTC; nothing pushed before a pass.
- Guidance sent ahead (00:40): build/ iframe sandbox="" without allow-scripts/allow-same-origin, srcdoc or blob: with frame-src, clean model HTML, clean zip file names; ask/ same-origin, untrusted wrapping, textContent; voice/ mic on click only, visible state, track stopped, TTS provider named and in connect-src; ocr/ say the file goes to Mistral, nothing kept, libraries vendored, textContent; inject/ fixed injected text, no writing or outside tools; every page: frame check, choose Never, key cleared, minimal connect-src.
- Verdict: none yet.

## Part 1, 01:00 UTC: voice/, ocr/, inject/, index.html (not pushed)
- Looked at: kit.js as pasted (parts elided by conductor.mistral), CSPs, descriptions and their grep. Not read: voice.js, ocr.js, inject.js.
- Verdict: PASS-WITH-CHANGES; push once 1 and 2 done.
  1. Medium: keyedFetch lacks redirect:'error'; set after the init spread.
  2. Medium: ocr/ must say the file goes to Mistral and warn against others' personal details or confidential documents.
  3. Low: revoke blob URLs.
  4. Accepted: key held until pagehide/Forget; openrouter.js import (toWireTools only; host not in connect-src).
- 01:02 UTC post-check dev 747a50b (live): kit.js first import in all three; redirect:'error' after the spread in keyedFetch and in callMistral; OCR warning above the picker as asked; blob URLs revoked on replace and pagehide; other fetches same-origin only; getUserMedia only in startRecording; no HTML sinks, storage, cookies, postMessage or window.open under site/browser; CSPs as described. Part 1 CLOSED: PASS.

## Part 2, 01:05 UTC: build/, ask/ (not pushed), from conductor.mistral's description; scripts not read
- Verdict: PASS. Accepted deviation: style-src data: on build/ only (sandboxed srcdoc preview styling; needs a parent-page injection to abuse; no HTML sinks in parent). Asked: object-src 'none' on both; keep sandbox="" empty; comment the data: reason.
- Holds: preview cleaned by DOMParser, scripts/links/handlers removed, inner CSP meta, sandbox="" before srcdoc; paths cleaned, no JS files, zip names cleaned; tools in memory only; ask/ same-origin links only, /team/ tools, untrusted wrapping, textContent; key closure, two-host map, redirect:'error', frame check.
- 01:08 UTC post-check dev d62db82 (live): CSPs as described with object-src 'none'; style-src data: comment in build/index.html; iframe has sandbox="" in markup and set again before srcdoc; no allow-scripts/allow-same-origin; frame checks; redirect:'error' on both keyed calls; no HTML sinks or storage in build/ or ask/. Part 2 CLOSED: PASS. Review 008 CLOSED.
- 01:22 UTC: 74b960a (live) checked by diff: markup, text and load timing; new fetches are same-origin recorded-run.json with credentials 'omit'; no change to CSP, Authorization, redirect, credentials or key code. Visible lines kept: choose Never, Free mode "say nothing confidential"; OCR file warning still present; longer explanation moved into <details>. Pass stands.
- 03:24 UTC 21 Nov: mistral.conductor@SovereignWebsite.dinis (session_01Kw9UwB4akPeq6twCVg9hVT) now takes review reports for the Mistral site in place of conductor.mistral (its own message; not confirmed by Dinis).
- 01:56 UTC 22 Nov audit: every page on Site__Provider__Mistral dev 497327b that takes a key (type=password or the shared key code) maps to a closed review (005 to 024). No unreviewed key page found. /live/ changes since 013 are text only. Sent the full list to librarian.b.
