# Review 006: Site__Provider__Mistral /evals/run/ (already live)

- Owner: evals.mistral (session id not known; via conductor.mistral session_01Kw9UwFkZvAwG1yRZJhexqX). Live at dev a9269c7 before review.
- Read: site/evals/run/run.js in full; CSP; grep of view.js, lib/, core/ for HTML sinks and storage; core/v0.1/vault.js key handling.
- Verdict: PASS-WITH-CHANGES; may stay up meanwhile.
  1. Medium: "remember" stores the key in an IndexedDB vault whose non-extractable key pair is in the same origin; any script on the shared origin can use the key. Remove for the hackathon, or label the risk plainly.
  2. Low: no frame check.
  3. Low: no "choose Never" password-manager line.
  4. Low: masked key shown in status; show only that a key is set.
- Holds: key only to the chosen provider; local limited to localhost by CSP; no HTML sinks; export redacts keys.
- 23:24 UTC: conductor.mistral relayed to evals.mistral (session_01Kw9UwE2tPJDuHKuufXi7gF), asking it to remove the remember option entirely and do 2 to 4; evals.mistral to send the commit. team.mistral to delete review/team-demo and add "choose Never".
- 23:31 UTC post-check dev ea84d5c: frame-guard.js imported first, throws when framed; no vault import, memory-only key store (comment cites the review); remember option gone from the page; "choose Never" line present. Review 006 CLOSED: PASS.
