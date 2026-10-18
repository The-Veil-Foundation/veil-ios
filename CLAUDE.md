# CLAUDE.md

Guidance for agents working in this repository.

## What this is

**Veil for iOS** — the iPhone/iPad client of Veil, a SwiftUI app plus a
NetworkExtension packet tunnel running Xray-core. The model and core layer
come from the `macos` submodule (`The-Veil-Foundation/veil-macos`): the
project compiles `macos/Sources/XrayClient/Models` and `.../Core` into both
targets. README.md explains the packet path, signing and the app↔extension
protocol.

```bash
git submodule update --init
Scripts/build-xraycore.sh                 # once: Frameworks/XrayCore.xcframework
Scripts/build-app.sh simulator Release
(cd macos && swift test)                  # shared code tests
```

## Rules that are easy to get wrong

- **Shared code is changed in veil-macos, never here.** Land the change
  there, then bump the submodule in its own commit. Platform differences use
  `#if os(macOS)` in the shared files.
- **One core only: Xray.** No tun2socks, no sing-box. Hysteria2, TUIC and
  AnyTLS stay greyed out (`ProxyConfig.xraySupported`).
- **The extension has a small memory budget.** The Go heap is capped at
  48 MB; do not raise it.
- `Frameworks/` is built, not committed (~110 MB).
- Logs, errors and diagnostics never print a credential; a share link is a
  credential.

## Conventions

- Commits: one line, imperative, no prefix — describe the behaviour that
  changed.
- Branches off `main`: `feat/…`, `fix/…`, `docs/…`.
- `CHANGELOG.md` gets an entry under `[Unreleased]`.
- Version: `Scripts/set-version.sh X.Y.Z` writes `VERSION` and the project.

## License

AGPLv3.
