# codex-usage-statusline

> 0.5.0 is a development candidate for Codex 0.156.1. A signed public release is not published yet. Apple Silicon device validation for this version is pending.

[한국어](README.md) · **English** · [日本語](README.ja.md)

Adds exact local reset dates and times to the existing five-hour and Weekly items in the Codex CLI footer. Percentages remain headroom remaining, and the rest of the configured footer is preserved. Normal usage is lavender, 40% remaining and below is yellow, and 15% remaining and below is red.

```text
gpt-5.6-sol low · Context 82% left · Usage 93% left (resets 2026-08-16 17:42) · Weekly 51% left (resets 2026-08-23 20:14)
```

## Install

Ask your current Codex CLI:

```text
Install and verify https://github.com/alexsears/codex-usage-statusline on this computer.
Use the installer included for this operating system and set the display language to English.
```

The installer checks the operating system, CPU, and Codex version, then downloads a prebuilt asset from the pinned GitHub Release. It verifies the release checksums, manifest, build metadata, and embedded binary hash before installing into a separate user-state directory.

Close the current Codex session and terminal after installation, then open a new terminal. The official Codex installation is not replaced.

To run the repository installer directly:

Apple Silicon macOS:

```sh
git clone https://github.com/alexsears/codex-usage-statusline.git
cd codex-usage-statusline
./install.sh --language en
```

Windows x64:

```powershell
git clone https://github.com/alexsears/codex-usage-statusline.git
cd codex-usage-statusline
powershell -NoProfile -ExecutionPolicy Bypass -File .\install.ps1 -Language en
```

Korean `ko` is the default. English `en` and Japanese `ja` are also available.

## Compatibility

- Codex CLI **0.156.1**
- Windows x64 and Apple Silicon macOS build targets
- Apple Silicon device validation for 0.156.1 is pending; the earlier validation applies to 0.147.0.
- Intel Macs are not supported

| Platform | Release build | Real-device validation |
|---|---:|---:|
| Windows x64 | Automated | Pending new-version verification |
| Apple Silicon macOS | Automated | Pending new-version verification |

Codex's internal TUI is not a stable plugin API, so the version must match exactly. The installer activates nothing if the version, asset, hash, or target architecture differs.

On macOS, the installer detects the active user's official Codex from:

- the OpenAI standalone installer
- Homebrew
- a global npm installation

## Verify

From a new terminal, run:

```text
command -v codex
codex --version
codex
```

After the first request, confirm that the footer contains the configured five-hour and `Weekly` items and that reset values use local `YYYY-MM-DD HH:MM` timestamps. These items can remain hidden until the first usage response arrives.

The installer and uninstaller never create or edit `~/.codex/config.toml`. The launcher supplies only `CODEX_USAGE_STATUSLINE_LANGUAGE`; it preserves the configured status-line items and their order. Include `five-hour-limit` and `weekly-limit` in `[tui].status_line` to show the reset timestamps.

## Uninstall

Apple Silicon macOS:

```sh
./uninstall.sh
```

Windows x64:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\uninstall.ps1
```

The uninstaller removes only its managed PATH block, launcher, and customized bundle. If any file was added to or changed in the bundle after installation, it preserves the whole bundle and only disables PATH activation. Other profile content, the official Codex installation, and `~/.codex/config.toml` are preserved.

## How it works

```text
repository installer
  └─ verify OS, CPU, Codex version, and official installation layout
      └─ verify the pinned release manifest, checksums, and metadata
          └─ copy the official Codex resource bundle into user state
              └─ put the verified binary and per-invocation launcher first on PATH
```

One shared Rust patch implements the status line. Korean, English, and Japanese are selected at runtime in the same binary.

- Windows state: `%LOCALAPPDATA%\codex-usage-statusline`
- macOS state: `~/Library/Application Support/codex-usage-statusline`
- macOS PATH blocks: absolute `$ZDOTDIR/.zprofile` when set, otherwise `~/.zprofile`, plus `~/.bash_profile`

Installation and removal use an operation lock, staging directories, atomic profile edits, and a recovery manifest. Interrupted operations roll back, and repeated installation does not duplicate managed blocks.

## macOS signing and Gatekeeper

The Apple Silicon binary is stripped, ad-hoc signed, and checked with `codesign --verify --deep --strict`. This release has no Apple Developer ID signature or notarization. A copy quarantined by Finder or a browser can therefore be rejected by Gatekeeper's `spctl` assessment.

The repository installer uses an HTTPS release and SHA-256 verification and does not remove quarantine attributes. This limitation remains until Developer ID signing and notarization are available.

## Security

- `release-lock.json` pins the official Codex tag, commit, tree, Rust version, patch SHA-256, and target architectures.
- The installers first verify `SHA256SUMS.sig` with the RSA public key pinned in the repository, then cross-check the archive, embedded binary, target metadata, `release-manifest.json`, and `SHA256SUMS`.
- The installers require the remote release tag's peeled commit to exactly match the signed `customizationCommit`.
- macOS additionally requires an arm64-only Mach-O and a valid ad-hoc `codesign` structure.
- A structured parser extracts only four allowlisted regular files from the archive.
- The original executable and persistent Codex configuration are never modified.
- This is an independent project, not an official OpenAI distribution.

See the [release process](docs/release-process.md) and [macOS validation record](docs/macos-validation.md).
