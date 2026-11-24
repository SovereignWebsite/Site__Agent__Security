# Review 019: Site__Provider__Mistral /workflows/poster/ (pre-push)
- Owner: mistral.workflows (session_01Kw9UwEHD6YUsjcvdoYDDXr). Reviewed 03:17 UTC 21 Nov from its message (svgsafe.js in full, findImage, CSP).
- Verdict: PASS. Accepted third-party img host mistralaiblackforestprod.blob.core.windows.net (exact host, https, no key, disclosed, signed link not recorded). File GET id pattern + encode via keyedFetch. svgsafe allow-lists; SVG shown only as <img src=blob:> (no script, no loads); must never enter the DOM or CSS. store:false.
