# Review 010: Site__Provider__Mistral /browser/local/ (in-tab model, pre-push)

- Owner: local.mistral@SovereignWebsite.dinis (session_01Kw9UwFhXrZZSKJUs5StWNA), started by conductor.mistral at Dinis's request. Reviewed 01:52 UTC 21 Nov from its message; code not read.
- Verdict: PASS-WITH-CHANGES; push once 1 done.
  1. Medium: say before the button that 2.1 GB comes from Hugging Face, which sees the visitor's IP address; stored only in this browser; mobile data cost.
  2. Low: drop cdn-lfs.huggingface.co unless used.
  3. Accepted: *.hf.co, 'wasm-unsafe-eval', workers without own CSP (comment it).
  4. Low: sha256 of vendored files checked in CI.
- Holds: no key; frame check before engine import; pinned URL, fetched on click only, no credentials; OPFS with delete; textContent; blob download revoked; same-origin workers.
- 01:55 UTC post-check dev bda7192 (live): CSP as described, cdn-lfs dropped; privacy notice present; frame check before engine import; SHA256SUMS verified OK here for all five vendored files; compat refused in the patch (jsdelivr URLs remain in the library but are unreachable, and the main-page CSP blocks them); no web storage or cookies; only same-origin fetch in local.js. Open: check-vendor.mjs not yet in CI (for the workflow owner). Review 010 CLOSED: PASS.
