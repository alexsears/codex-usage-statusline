# Footer compatibility

The 0.5.0 patch targets Codex CLI 0.156.1. Reset timestamps come from the server usage window and render in the machine local timezone; percentages still mean remaining capacity. The configured footer order is preserved.

The installed owner customization was a local verified build, not a signed public release. This update follows that local build path; the public installer continues to require the pinned release signatures. No signing keys or public release assets are changed by this task.

Apple Silicon validation from the previous version does not establish 0.156.1 device support.

## Verification

- `just test --locked -p codex-tui status_line_`: 85 passed on Windows.
- Release asset tests: 8 passed under Ubuntu/WSL.
- Installer helper tests: passed under Windows PowerShell 5.1 and PowerShell 7.
- GitHub lightweight CI verifies the patch applies to the pinned upstream source and both platform installer suites.
- Workspace package versions in Cargo.lock are synchronized with the release tag without changing dependency resolutions.
- The upstream whole-repository formatter cannot run on Windows because its Cargo command exceeds the command-line length limit and Bazel formatting requires DotSlash. The changed TUI crate was formatted with `cargo fmt -p codex-tui`; unrelated source was not changed.

## Local installation completed

The native development build (the same approach as the previous installation) is installed through the existing side-by-side launcher. Fresh-shell resolution returns Codex 0.156.1. The complete configured footer was checked in a live Windows pseudoterminal, and its weekly reset agrees with live account data. The weekly item is first so the timestamp remains visible.

Installed executable SHA-256: `4afa9dbcaceb16b1cc32d5fcbee4f2430bc87d0ba54416578dfdcddac2dbf8cc`. The official npm executable and prior custom bundle are preserved. The previous launcher, installation manifest and footer order are backed up in the installer state directory. The unnecessary optimized build was stopped.

Existing terminal sessions must be restarted to load the new executable. The public signed release remains a separate, unfinished distribution stage.
