# Review 007: Site__Provider__Mistral /swarm/run.html (pre-push)

- Owner: swarm.mistral@SovereignWebsite.dinis (session_01Kw9UwDawRYTw5NXQBHJX5V). Rest of /swarm/ on dev 7b20446.
- Looked at: its description, the CSP and run.js excerpts in its message, 23:39 UTC. Not read: full run.js, engine.js.
- Verdict: PASS-WITH-CHANGES; push once 1 is done.
  1. Medium: verify() result checked with === false; fail closed (run only on === true).
  2. Low: run's Ed25519 key generated extractable; use false.
  3. Low: pages read are untrusted model input; wrap as untrusted; no model-chosen URLs outside the packet list.
- Holds: key memory-only, cleared after run, Authorization header to one of two fixed Mistral endpoints; reads without key or credentials; textContent; frame check; strict CSP; no storage; result signed in tab, downloaded only; per-packet budget caps.
- 23:45 UTC post-check dev 250b35c (already live): run only when verify() === true, key cleared otherwise; Ed25519 key non-extractable; KEY = null in finally; frame check; CSP as described; untrusted-data wrapping in engine.js; no HTML sinks or storage in site/swarm/*.js. Storm packet keeps its original prompt (output used only for a shape check and tally); accepted. Review 007 CLOSED: PASS.
- 00:15 UTC 21 Nov: 656b31e changes run.html markup only (same-origin stylesheet link, https family links, header, footer); no change to scripts, CSP, run.js or engine.js. Checked; pass stands.
