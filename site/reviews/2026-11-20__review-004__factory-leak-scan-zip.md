# Review 004: Factory ff515ae, leak scan skips .zip in gated sites (pending)

- Raised by conductor.b, 20:52 UTC 20 Nov 2026. Owner: factory.b. Not on origin (fetched 20:53, origin/dev 0164ea3); diff requested from factory.b.
- Concern sent ahead (20:53): skipping every .zip lets a plain zip with a key or private file pass. Asked for a narrow rule: only the sealed vault's exact path(s), only after checking the file is sealed; any other .zip scanned or the build fails.
- Verdict: none yet.

## 20:55 UTC: factory.b's fe4181f (local, on ff515ae, e50159b, 6ca484a); reviewed from its diff summary, part pseudocode, full file not seen
- fe4181f replaces the zip skip: every .zip is opened and each entry scanned; unreadable zip or method 99 fails the build. Tested by factory.b on three cases and a live 17 MB vault.
- e50159b build.mjs: PASS.
- fe4181f quality.mjs: PASS-WITH-CHANGES.
  1. Medium: detect zips by magic bytes, not extension.
  2. Medium: files over 5 MB unscanned; scan in chunks with overlap, or fail unless allowlisted.
  3. Medium (gated sites): plaintext vault objects pass; add a ciphertext (entropy) check on entries over 1 KB.
  4. Low: maxOutputLength on inflateRawSync.
  5. Question: SWS_OWN_SITE === '1' turns the family strip check off; what, who sets it, why. Not passed until answered.

## 20:57 UTC: 229bd2d (local, on fe4181f), full leak-scan section read
- Fixes 1 to 5 done as asked. 5: SWS_OWN_SITE=1 set only in seed/agent-template pages.yml, commented; turns off only the family strip check (Dinis: customer sites must not carry the strip). Fine.
- Verdict: PASS-WITH-CHANGES.
  1. High, pre-existing (origin/dev too): templates' LEAK list lacks sgit vault/read key, JWT, other AWS prefixes, ya29., AWS secret assignment; match build/leak-scan.mjs SHAPES, add a test.
  2. Low: ciphertext check only for private/<id>.zip; extend to any zip with manifest.json.
  3. Low, accepted: entropy cannot catch unencrypted JPEG or PDF.
  4. EOCD false positive: keep failing safe; clearer message.

## 20:58 UTC: final commit on gate (after 229bd2d), as described by factory.b
- 1 done: LEAK matches leak-scan.mjs SHAPES incl. sgit keys, JWT, AWS, ya29., plus VAULT_PAIR; parity test (10 of 10); dummy-key tests fail as they should. 2 done: every zip with manifest.json held to ciphertext. 3 and 4 done.
- Verdict: PASS, on the description; the final commit itself not seen. Post-check on origin: templates' quality.mjs and the parity test; reopen if different.

## Post-check, 21:01 UTC: Factory dev de05b7c (on origin)
- quality.mjs identical in site, gate and agent templates (sha256 ee615b23...); build.mjs identical (51bec32b...), no inline <style>.
- Seen: sgit vault/read key, JWT and ya29. patterns; every zip with manifest.json held to ciphertext; 4 MB chunks with 8 KB overlap; maxOutputLength; SWS_OWN_SITE set only in seed/agent-template pages.yml.
- node test/run.mjs: OK (tests), including the SHAPES parity test.
- Commit signatures not verified here (no allowed-signers file locally); the Factory's own check covers that.
- CLOSED: PASS.
