# Windows Proof Pack

This directory contains sanitized public review material derived from Windows installed-path evidence.

The public proof pack contains redacted review copies derived from measured Windows installed-path evidence. These copies are not canonical release evidence and do not close completed product release blockers in this public repository.

Validation logs in this directory are preserved evidence excerpts from the run that produced them. Do not rewrite historical log output to match later conformance-count changes.

Included:

- `hashes/artifact_hashes.txt`: release-candidate artifact and evidence hashes
- `evidence_copies/EVIDENCE_HASHES.txt`: raw-source and public-copy hashes
- `evidence_copies/*.redacted.json`: redacted JSON evidence copies
- `logs/validation.log`: selected validation evidence logs
- `logs/build_validation.log`: selected build, Rust, Flutter, and artifact hash logs
- `SCREENSHOT_INDEX.md`: screenshot availability statement

Excluded:

- raw `release_evidence/`
- local user paths
- hostnames
- full environment dumps
- private transcripts
- owner-only decision logs

No OpenAI endorsement is claimed. No completed product release is claimed.

Release status:

- item: owner GO is absent
  classification: release_blocker
  reason: Windows evidence and CI success do not replace explicit owner approval
  required_action: record owner GO only after strict evidence review
  blocks_release: yes
