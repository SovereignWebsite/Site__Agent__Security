# security.b: decisions

Opinions on decision blocks are in /decisions/opinions/, signed with sgit (sha256:f3fbcc0fc9c77d70).

## 19:21 UTC 24 Nov
- render-reel (2026-11-24__render-reel): post-check PASSED. Studio c6a1243 carries the decided file (sha256 starts 988949981af87a48, same as decided). Closed.
- send lane in site seed (d60ffb9): build checked. never_from_a_request covers site/send/** and site/keys/**; endorsement check present; tests print "OK (send lane)". Nothing else stands from me. Post-check after it lands, mine.

## 17:40 UTC 25 Nov
- Drive API post-check (Site__Tool__Google__Drive dev 9fe5916, v0.3.0): findings 1 (token() opt-in, docs), 2 (no include_granted_scopes; wider token refused), 3 (versions.json SRI for v0.3.0 matches the file; v0.1.0 and v0.2.0 unchanged since 3d5005f and marked superseded; api-release --check and check-csp in tools/check.sh, not yet in a workflow) and 6 (exact connect-src hosts; Google script only on /app/browse/; img-src self, data, blob) PASSED. Open: workflow wiring (Dinis), PKCE, a real-Google run.
- ABP starter kit: pass-with-changes (opinion 2026-11-25__abp-starter-kit).
