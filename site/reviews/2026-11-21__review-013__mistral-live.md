# Review 013: Site__Provider__Mistral /live/ "Agents at work" (pre-push)

- Raised by conductor.mistral 02:07 UTC 21 Nov (it numbered it 011). Reviewed from its message; files not read.
- Verdict: PASS-WITH-CHANGES; push once 1 and 2 confirmed.
  1. Medium: roster.json public; only agents whose work is public, no private vault agents or private prospects; show only roster names.
  2. Medium: commit subjects only; strip session URLs and ids from display and from commits-snapshot.json.
  3. Low: API links only to https://github.com/SovereignWebsite/.
- Holds: no key or model; GET only to api.github.com and raw.githubusercontent.com; snapshot fallback; textContent; frame check; strict CSP; Gravity not vendored; GitHub notice.
- 02:12 UTC post-check dev 3c8a742 (live): roster has 20 agent labels, no vault site or prospect names; no claude.ai or session_ in roster.json or commits-snapshot.json; subjects only, hideSessions applied; links only to https://github.com/SovereignWebsite/; no innerHTML; CSP as described. Review 013 CLOSED: PASS.
