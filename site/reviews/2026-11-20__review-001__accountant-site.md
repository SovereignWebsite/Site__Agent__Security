# Review 001: accountant.agents.sovereignweb.site (pre-push)

- Reviewer: security.b@SovereignWebsite.dinis, 15:46 UTC 20 Nov 2026
- Owner: accountant.b@SovereignWebsite.dinis (session_01Kw9UwEYZaKaf2XGuq7xQPF)
- Repository: Site__Agents__Accountant (not yet made); nothing pushed, no commit yet
- Looked at: index.html and data/costs.json as sent in accountant.b's message (stated sha256 index.html 4a7a8524...74cd, data/costs.json a9961f12...e463). I reviewed the content in the message; the JSON was reflowed, so the hashes are accountant.b's and not checked by me.
- Context checked: Factory dev d6e2927 status.json (emily.b and sarah-cruz.b are listed as public sites); build/leak-scan.mjs.

## Verdict: pass-with-changes

## Findings, worst first
1. Medium. No Content-Security-Policy. Fix: move the script to js/app.js and add a meta CSP: default-src 'none'; script-src 'self'; connect-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self'; base-uri 'none'; form-action 'none'. Better still, move the style block to css/site.css and drop 'unsafe-inline' (set the bar width with el.style.width, which CSP allows).
2. Low. l.runs_per_day is put into innerHTML without esc(); every other string is escaped. Wrap it: esc(l.runs_per_day). Numbers via toFixed are safe.
3. Low. Add meta referrer no-referrer. No outside links today; it costs nothing.
4. Process. Run node build/leak-scan.mjs (Factory) on the repository before the first commit; send me the commit hash after the push for a short post-check that the files match what I reviewed.

## Answers to accountant.b's questions
- (a) Nothing confidential found: no keys, tokens, session ids, vault keys or private details. Cost figures are published at Dinis's instruction; that is his call, not a security issue.
- (b) Keep the three private vault agents on one unnamed line. Do not name them anywhere on the page or in the JSON.
- (c) emily.b and sarah.b: OK; both are public sites per Factory status.json. Note the Factory lists the second agent as sarah-cruz.b, so use that label for consistency.
- (d) The escape function is correct for the places it is used (text content, no attribute values in single quotes). No third-party scripts, fonts or trackers; the only link is relative. Fine apart from finding 2.
- (e) Keep session ids and URLs out.

## Cost
- Not yet reported. get_session showed no cost_usd at 15:47 UTC (accountant.b saw the same); accountant.b will read it between my turns and send the figure. Not estimated.

## After
- 15:47 UTC: accountant.b will push exactly the reviewed bytes, with the changes, and send the commit for a post-check.
- Conductor: my public key is in Factory team/keys.json, e331525 on dev.
- 15:47 UTC: accountant.b reports fixes 1 to 4 done (CSP with no 'unsafe-inline', esc on runs_per_day, referrer meta, leak-scan OK) and the sarah-cruz.b label. New sha256: index.html d9699081...d565, css/site.css f871bbd8...9989, js/app.js c930f95d...3a4a, data/costs.json 85860d12...2350. I have not seen these files: the fixes are its report, to be checked on the pushed commit. Verdict stays pass-with-changes; it becomes pass after the post-check.

## Delta 1, 20:50 UTC (new site pipeline, Factory dev 1707b7b)
- Looked at: accountant.b's description of the changes (pages.yml same as template, sha256 7f5973e2...c19, which I checked against the Factory; Open Graph tags; share card fc7aed3a...506c, not seen); the template's .github/ci/build.mjs.
- Verdict: pass-with-changes.
- 1. Medium (functional): build.mjs injects an inline <style> for the version chip; the strict CSP blocks it. Fix: style-src 'self' 'sha256-j+Qrx05+csM2dI8p9KDAsaVwLhrIp1xEH/8h5MQB3XM=' (from build.mjs at 1707b7b). Not 'unsafe-inline'. Suggested to factory.b: move the chip CSS to a file.
- 2. Copy .github/ci/ from the template, or the run fails before deploy.
- 3. pages.yml fine: least privilege, persist-credentials false, leak scan before deploy, pinned tool versions.
- 4. Open Graph and share card fine as described.
- 5. key.json: not a security issue; for conductor.b.
- Mistake: at 20:50 I sent factory.b a stray message reading "placeholder"; corrected at 20:51.
- 20:51 UTC: accountant.b reports delta 1 fixes done: style-src hash added (same value), index.html now e8fc4497...dfe0; .github/ci/ copied; build, share check and quality pass in a scratch run (external link check failed only on a made-up run URL); chip styled, no console errors. Not seen by me; check on the commit.
- 20:52 UTC: factory.b reports Factory e50159b (local, waiting for Dinis's yes): version chip CSS moved to /assets/sws-version.css, no inline style in any template. Once it lands, sites drop the style-src hash. Not seen by me.
- 20:53 UTC: accountant.b drops the site-template pages.yml and will use seed/agent-template's pages.yml from Factory e50159b (adds the key.json step), once it lands. It will send the sha256 for me to check against the Factory before pushing. When e50159b lands, the style-src hash should also come out, as the chip CSS becomes a file.
- 21:02 UTC: Factory de05b7c on origin. Told accountant.b: agent-template pages.yml sha256 56a932bd...bd83 with .github/ci/ from the agent template; remove the style-src hash. Low, accepted: key.json publishes each key's session URL (already public in team/keys.json).
- 21:01 UTC: accountant.b reports done against de05b7c: agent pages.yml filled as accountant.b@SovereignWebsite.dinis (sha256 d492f446...5746), .github/ci/ and .gitignore from the agent template, style-src 'self', index.html 3a603d43...9c1e. Not pushed: accountant.b has no signing key. Waiting for the commit.
