# Changelog

Every release here is a build of [tek-tube](https://github.com/kevinelong/tek-tube) and
names the source commit it was built from, so a download can be matched to a revision.

## v0.1.0 — 2026-10-12

First published build: **tek-tube 0.1.0** for Windows x64.  Everything the source
repository describes at that commit — the 4010/4014 protocol (alpha, graph, point plot,
incremental plot, Enhanced Graphics Module 12-bit addressing, the escape set and the
line-style table), the three-population tube model with its flood gun, halation and
measured chromaticity, live playback of plot files, playback from a pipe, the options
panel with saved presets and launch rows, and `--shell` terminal mode over a pseudoconsole.

| | |
|---|---|
| Source | [kevinelong/tek-tube](https://github.com/kevinelong/tek-tube) @ `3d4ae7a` |
| Asset | `tek-tube-0.1.0-win64.zip` |
| Built with | rustc 1.96.0 (ac68faa20 2026-05-25), `cargo build --release`, x86_64-pc-windows-msvc |
| Tests at build time | `cargo test` — 112 passed, 0 failed, 2 ignored |
| `tek-tube.exe` | 931,840 bytes, SHA-256 `cbc8b1e6cf784da414855af6a854c0e46bf88df1f3f19baebcf3f18174ca9e10` |

Not included: the sample plot files in the source repository's `samples/` directory.
Those are a third-party historical collection kept for personal use (see the source
repository's `NOTICE.md`), so they are neither redistributed here nor bundled into the
archive; clone the source repository to get them.
