# Review 005: Site__Provider__Mistral team demo (pre-push)

- Owner: team.mistral@SovereignWebsite.dinis (session_01Kw9UwDNQcEyBYK4Ac6k9Zz); raised via conductor.b. Commit 6c11a84 on dev 983035b, local; push to dev deploys.
- 23:17 UTC: reviewed the summary only. No verdict yet. Asked for a non-deploy branch (or the four files: app.js, index.html, engine.js, sitetools.js). Push to dev: not yet.
- (1) #k= key hand-off: acceptable as opt-in with changes: warn about global history, sync and scanner-app history; auto-hide the QR after 60 s and on blur or hidden; strip #k= before any agent, tool, error or recording sees location.href; never show the key or link as text; no outside loads on the receiving page.
- Question: what "guarded" publish means; can model output cause any write.

## Part B: /demos/ (conductor.mistral, session_01Kw9UwFkZvAwG1yRZJhexqX), already live
- Looked at: Site__Provider__Mistral dev 983035b, site/demos/core.js in full; CSP of all four demo pages; side-by-side.js output and endpoint handling; grep of all demo JS for HTML sinks and storage.
- Verdict: PASS. May stay live.
- Seen: key in a closure object only, cleared on pagehide and "Forget my keys", input cleared after use, only the last 4 characters shown; the key goes only into the Authorization header; base URL fixed per provider (local from a fixed list, no key); CSP script/style 'self', connect-src the listed hosts only; no innerHTML, storage or cookies; model output via textContent.
- Low: 1. No frame protection (frame-ancestors cannot be set in a meta tag): add a check that the page is not framed, so another site cannot trick a visitor into clicking Send. 2. Password managers may offer to save the key from type=password; say so on the page, or use type=text with a CSS mask. 3. window.Demo._keyFor exposes keys to any same-origin script; acceptable under script-src 'self'.
- 23:20 UTC post-check, dev a35fd4a: frame check at the top of core.js clears the page, offers a _top link and throws so window.Demo is never defined; "choose Never" note in the key panel. Lows 1 and 2 fixed; 3 noted. Part B CLOSED: PASS.

## Part A, 23:21 UTC: review/team-demo 5596794 (does not deploy)
- Read: fragment.js in full; app.js import order, key store setup, makeQR/hideQR and listeners; CSP in index.html; grep of site/team (not vendor) for HTML sinks, storage, cookies, location.hash/href, eval; keys.js storage calls; sitetools.js path handling; recorded runs for Authorization values.
- Seen: fragment.js is the first import, reads #k= once, replaceState strips it, hands the key over once; no other code reads location.hash or href. QR drawn as SVG paths (no data: image), hidden after 60 s, on blur and on hidden, and on untick/close/forget. No innerHTML, storage writes or cookies (keys.js only removes a legacy localStorage item). Tool paths reduced to pathnames, same origin only, and CSP connect-src 'self' plus the two Mistral hosts. Recorded runs hold no Authorization value. Publish refused by guard or denied; run() never writes. Warning text as described (a); key never shown as text (d), as reported.
- Verdict: PASS. May push to dev; post-check on dev after.
- 23:25 UTC post-check: dev b172b31 has no diff under site/team against 5596794. Part A CLOSED: PASS. Branch review/team-demo may be deleted. Low still open: "choose Never" line.
- 23:25 UTC: 144c0e8 (local, from diff): "choose Never" line; frame guard at the top of fragment.js, before #k= is read, linking to /team/ without the fragment; status names the chosen host. PASS; push allowed; post-check on dev to follow.
- 23:29 UTC post-check dev 4ce8255: since 5596794 only app.js (status host), fragment.js (frame guard), index.html ("choose Never") and HANDOVER.md changed, as passed. Review 005 CLOSED: PASS. Branch review/team-demo still exists: the agent's proxy refused deletion; needs a person.
