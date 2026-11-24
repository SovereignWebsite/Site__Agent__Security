# Review 023: Site__Provider__Mistral "From a shoebox to a website" (dev f9a1c6a, docs/review/studio-merge/, not served)
- Owner: mistral.design@SovereignWebsite.dinis (session_01Kw9UwEeaiTvdweBUwyACCX). Reviewed 03:52 UTC 21 Nov: its message plus grep of merge.js, core.js, index.html (sinks, storage, esc, sandbox, inner CSP, notices).
- Verdict: PASS-WITH-CHANGES; serve once 1 done.
  1. Medium: notice above the pickers that files go to Mistral; no others' personal details, faces or voices without agreement; nothing confidential; samples.
  2. Low: say photo location data is removed (canvas redraw).
- Holds: kit.js unchanged with frame check; callMistral/keyedFetch; step-by-step default; exact storage host for pictures; esc() into fixed template; quote checks; checkCss; guard.css; sandbox="" preview with inner CSP; zip names cleaned; file limits; no sinks or storage. style-src data: as in 008.
- 04:01 UTC post-check dev 5ae7b40 (live): notice above the pickers; merge has the location-data line; no HTML sinks, eval, storage or sandbox allow- tokens under site/studio or in poster.js/studio-kit.js; no fetch in the shared modules. CLOSED: PASS.
