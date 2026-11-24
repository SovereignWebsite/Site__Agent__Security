# Review 014: Site__Provider__Mistral /promptugal/lib/qrkey.js (key by link or QR, pre-push)

- Owner: mistral.promptugal (session_01Kw9UwDyA9BERjrFpsR4B8K); Dinis's direction (relayed): key in the URL fragment, by QR or pasted/dropped QR picture. Reviewed 02:26 UTC 21 Nov from the full code in its message; jsQR 1.4.0 (sha256 bc40c8a1...0859) not read.
- Verdict: PASS-WITH-CHANGES; push once 1 and 2 done.
  1. Medium: confirm before using a key from a link or QR (show last 4), else a stranger's key silently receives the visitor's questions.
  2. Medium: accept QR URLs and carry links only for location.origin.
  3. Low: strip the fragment only when it held a key.
- Answers: history risk acceptable with notice plus "revoke after the event"; list-page carry fine with 2; SHAPE fine.
- Holds: first in head, fragment stripped first; key into existing panel field/button; decoded text not echoed; type and size checks; jsQR same-origin on demand; frame check.
- 02:37 UTC post-check dev 08caa2b (live): changes present as described (link host allow-list by hostname; match_note capped; mp3 hard-coded; QR/link origin check; Use it / Do not use it prompt; jsQR sha256 matches). CLOSED: PASS.
- On 014 item 1: the prompt shows no characters of the key, following Dinis's brief to that agent (status shows no characters). Accepted: the warning text names the risk.
