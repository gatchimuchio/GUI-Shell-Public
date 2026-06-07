# Problem And Solution

Problem:

Agent tools often combine UI convenience, diagnostics, runtime metadata, and execution authority in ways that are hard to audit.

Solution:

GUI-Shell separates those responsibilities:

- Flutter renders operator surfaces.
- Shell Core owns contract-shaped authority state.
- Adapters normalize runtime data without granting permission.
- Rust broker owns native helper and IPC safety boundaries.
- Validation tooling makes release gates explicit.
