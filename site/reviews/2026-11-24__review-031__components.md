# Review 031: components draft (Factory 8fea075, local), from factory.b's summary
- Verdict: PASS-WITH-CHANGES.
  1. Per-file hashes in the pinned component suffice for weight integrity; mirror only for availability/privacy (Dinis's choice); hash while streaming, use only after full match, delete on mismatch; keep the IP notice.
  2. connect-src: exact hosts, not file URLs (CSP ignores paths after redirect); wildcard only where the CDN name varies.
  3. Medium: first release and versions adding network, storage, outside source, worker or WASM come to security.b.
  4. Medium: site build refuses releases json not signed by an active Factory key; hash checked on the copied file.
  5. Low: no innerHTML with site/model text, no page storage/cookies/keys; README + CI grep.
- 00:12 UTC 24 Nov: factory.b folded all points into eb370ba (local), sent to decisions.b. PASS as described; code to be checked when components land.
