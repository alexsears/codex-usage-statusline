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
