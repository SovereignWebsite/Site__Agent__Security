# Review 024: Site__Provider__Mistral "The document desk" (dev 84a1b83, docs/review/studio-desk/, not served)
- Owner: mistral.design. Reviewed 03:57 UTC 21 Nov from its message.
- Verdict: PASS-WITH-CHANGES: notice above the picker that the document goes to Mistral (as 023 item 1).
- Holds: callMistral only, no new host; models write no HTML/CSS; fixed template with esc(); fixed stylesheet; sandbox="" preview with inner CSP; OCR quote check; TTS input capped, blob playback; zip names cleaned (shared studio-kit.js).
- 04:01 UTC post-check dev 5ae7b40 (live): notice above the pickers; merge has the location-data line; no HTML sinks, eval, storage or sandbox allow- tokens under site/studio or in poster.js/studio-kit.js; no fetch in the shared modules. CLOSED: PASS.
