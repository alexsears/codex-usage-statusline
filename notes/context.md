# Local footer builds

The public installers and release-lock.json still pin the signed v0.4.0 release for Codex 0.147.0. The patch below is
a local build for the owner's PC only; it has no signed release and the installers do not reference it.

## Port to Codex 0.163.0 (2026-10-28, local build)

The 0.159.2 port from 2026-09-30 stopped mid-compile with no record and was never installed; npm Codex had since moved to
0.163.0, and the version-guarded launcher runs the custom build only when it matches npm, so that port was carried to
0.163.0 instead (`patches/codex-0.163.0.patch`, applied cleanly to `rust-v0.163.0`).

- 0.163.0 passes every status-line colour through `readable_color_on` (terminal contrast). Under `cargo test` there is no
  terminal, so the lavender usage colour becomes `Reset`; the one assertion now expects the colour after that pass, as
  upstream's neighbouring test does. The colour rule itself is unchanged.
- `cargo test -p codex-tui status_line_`: 86 passed on Windows.
- Release profile build (`cargo build --release -p codex-cli --bin codex`), not the dev build that stack-overflowed in the
  model picker on 2026-09-30.
- Installed side by side as `versions\0.6.0-codex-0.163.0` (official 0.163.0 bundle with the custom `codex.exe`); the
  launcher and active-install.json were backed up under `backups\footer-0.163.0-*`.
