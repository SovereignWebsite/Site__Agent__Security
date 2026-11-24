# Review 011: Site__Provider__Mistral /promptugal/ask/ (pre-push)

- Owner: mistral.promptugal@SovereignWebsite.dinis (session_01Kw9UwDyA9BERjrFpsR4B8K), started by conductor.mistral at Dinis's request. Reviewed 01:58 UTC 21 Nov from its message (pt-tools.js core and ask.js parts verbatim); full files not read.
- Verdict: PASS-WITH-CHANGES; push once 1 done.
  1. Links in answers: add a host allow-list (promptugal.news, data.promptugal.news, ipma.pt, nvd.nist.gov, euvd.enisa.europa.eu, cisa.gov, by hostname) on top of "seen this turn"; same for "What the tools read".
  2. Low: cap match_note echo.
- Holds: shared key panel and chat core unchanged; no model-chosen URLs; GET only, no credentials, redirect:'error'; textContent; frame check; strict CSP; replay sends nothing; export without key.
- 02:37 UTC post-check dev 08caa2b (live): changes present as described (link host allow-list by hostname; match_note capped; mp3 hard-coded; QR/link origin check; Use it / Do not use it prompt; jsQR sha256 matches). CLOSED: PASS.
