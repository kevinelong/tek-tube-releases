# Changelog

Every release here is a build of [tek-tube](https://github.com/kevinelong/tek-tube) and
names the source commit it was built from, so a download can be matched to a revision.

Not included in any release: the sample plot files in the source repository's `samples/`
directory.  Those are a third-party historical collection kept for personal use (see the
source repository's `NOTICE.md`), so they are neither redistributed here nor bundled into
the archive; clone the source repository to get them.

## v0.3.4 — 2026-10-12

**No second black window beside the tube.**  Started the way the Start Menu starts it — a
shortcut, a double click, Explorer, with no console to inherit — the program showed a console
window of its own.  That is the Windows *console* subsystem doing its job: the executable is
built as a console program on purpose, so a shell that runs `tek-tube --png out.png` waits for
the file and its report appears where it was asked for, and Windows gives such a program a
console when it has none.  That console was never the shell's: the shell runs under a
pseudoconsole and has no window at all, and the only thing the console showed was the
program's own session report.

The window is now hidden, and only when it is ours — `GetConsoleProcessList` returning one
entry distinguishes a console we were given from the one a shell provided, and a shell's is
left exactly as it is.  Hidden rather than freed, because `FreeConsole` would invalidate the
standard handles and the closing report would then fail rather than go to a console nobody
can see.

| | |
|---|---|
| Source | [kevinelong/tek-tube](https://github.com/kevinelong/tek-tube) @ `a99ae5a` |
| Asset | `tek-tube-0.3.4-win64.zip` |
| Built with | rustc 1.96.0 (ac68faa20 2026-05-25), `cargo build --release`, x86_64-pc-windows-msvc |
| Tests at build time | `cargo test` — 123 passed, 0 failed, 2 ignored |
| `tek-tube.exe` | 945,664 bytes, SHA-256 `9536ee851dbd059ba4486783782911036ab077fe4c39a32ce85ce7a24af35883` |

## v0.3.3 — 2026-10-12

**The cursor is stroked like the font's own ink.**  The steady mark the last release added
was a rectangle of light two dots thick with hard edges, drawn in the settled green — too
thin, too crisp, and nothing like a rendered underscore.  It is a *stroke* now: deposited
through the same Gaussian beam profile a stroke deposits charge with, at the stroke width the
terminal's own glyphs are stroked with, and carrying a *fresh* stroke's level in both
channels — the total light and the fresh channel the colour pass reads for the hot
chromaticity, which is where the whiteness of fresh ink on this tube comes from.  A host
keeping its cursor written leaves it hot; this is what that looks like.

Across the underline's own width (green channel, six dots either side of the centre line):
`89, 90, 123, 164, 211, 250, 255, 251, 212, 164, 124, 88, 89` — a dome six dots wide rather
than a two-dot step, peaking at 255 where a blank cell reads 76.

| | |
|---|---|
| Source | [kevinelong/tek-tube](https://github.com/kevinelong/tek-tube) @ `09357df` |
| Asset | `tek-tube-0.3.3-win64.zip` |
| Built with | rustc 1.96.0 (ac68faa20 2026-05-25), `cargo build --release`, x86_64-pc-windows-msvc |
| Tests at build time | `cargo test` — 122 passed, 0 failed, 2 ignored |
| `tek-tube.exe` | 945,664 bytes, SHA-256 `f1a98a1b80cf4ed2b42a68b9d7514f685df27678221121ca3778442aa1a6b295` |

## v0.3.2 — 2026-10-12

**The cursor is a steady mark, so it does not fade away.**  It was drawn as a transient
flash and only when something else changed, so within a second of the shell going quiet it
decayed to the flood-gun floor and the tube sat there with no cursor at all until the next
key.  Writing it again on a clock — the obvious fix, and the one asked for — was built and
measured first and rejected on cost: a refreshed flash never lets the tube's aliveness check
clear, so a 1024x780 frame would be recomposed ~58 times a second, forever, for one
underline.  The cursor is instead a steady mark at stored-ink level, which is what the
hardware's own hosts produced when they wrote a cursor as stored ink and redrew the page to
move it: not charge (an erase does not blink it, moving it leaves no trail) and not a
transient (an idle prompt still stops recomposing).

Measured through the harness on an idle tube, the rendered frame's green channel sampled
~1400 times over 6 s: this build reads **188** throughout against a blank-cell floor of 76;
was **77** (the floor — the cursor gone) before.

| | |
|---|---|
| Source | [kevinelong/tek-tube](https://github.com/kevinelong/tek-tube) @ `2e8ccce` |
| Asset | `tek-tube-0.3.2-win64.zip` |
| Built with | rustc 1.96.0 (ac68faa20 2026-05-25), `cargo build --release`, x86_64-pc-windows-msvc |
| Tests at build time | `cargo test` — 122 passed, 0 failed, 2 ignored |
| `tek-tube.exe` | 945,152 bytes, SHA-256 `18b4f36f59479be59d951ed018f9bd054a3a48306d99674f4c95f89b7c314470` |

## v0.3.1 — 2026-10-12

**A click is one press.**  A click on a list stepped it once per frame the button was down —
and a real click lasts several frames, so one click on a three-entry list moved six entries,
which reads as a random choice rather than the next one; a box flipped once per frame, so the
release decided it by the parity of the click's length.  The press is taken from the button's
**edge** now.  Only a slider follows the button while it is down, which is what a drag is.
The same bug made a held click on a launch row (`zoom`) re-execute the program every frame.
The keyboard was already right — the window loop compares each panel key against the previous
frame's key states — so held arrows and a held Enter were never repeats.

| | |
|---|---|
| Source | [kevinelong/tek-tube](https://github.com/kevinelong/tek-tube) @ `920d54e` |
| Asset | `tek-tube-0.3.1-win64.zip` |
| Built with | rustc 1.96.0 (ac68faa20 2026-05-25), `cargo build --release`, x86_64-pc-windows-msvc |
| Tests at build time | `cargo test` — 119 passed, 0 failed, 2 ignored |
| `tek-tube.exe` | 945,152 bytes, SHA-256 `25dc2e7a5b39cfc0b6b08b2aeeb850e9691dc5054eafb73bddeb127781b0cabc` |

## v0.3.0 — 2026-10-12

**A face change is a page, not a session, and a list can be browsed.**  Two panel changes,
both from the same complaint: stepping the `font` list in the F2 panel restarted the
program, so the list could not be walked and the choice had to be made blind — one wheel
notch ended the session.

- The face is a **rendering** row now.  The tube holds the face, so changing it configures
  the tube again and writes the page again in the new face — the same recipe the cell size
  already used — and the shell underneath never notices.
- A **launch** row (`device`, `zoom`, `shrink`, in the plot path) is honoured only by a
  *press*: Enter, or a click on the row.  The wheel walks the list, changes nothing but the
  number shown, and the note line says what a press would do instead.

Measured through the terminal harness with no window, one page and one session: `mono` →
`simplex` moved the lit pixels 21594 → 37525, asking for the same face twice moved them
21594 → 20704 (so the number follows the face and not the rewrite), and a command typed
after four face changes was answered by the shell it was typed into.

| | |
|---|---|
| Source | [kevinelong/tek-tube](https://github.com/kevinelong/tek-tube) @ `93bfd48` |
| Asset | `tek-tube-0.3.0-win64.zip` |
| Built with | rustc 1.96.0 (ac68faa20 2026-05-25), `cargo build --release`, x86_64-pc-windows-msvc |
| Tests at build time | `cargo test` — 117 passed, 0 failed, 2 ignored |
| `tek-tube.exe` | 944,640 bytes, SHA-256 `2acd716caf65b426b147519dc4db79ef95cd68a66b23ac21dcdc7c8c15428422` |

## v0.2.0 — 2026-10-12

**tek terminal in the Start Menu, and a setup that asks which shells to offer.**  The
archive now carries a Windows installer: `setup.cmd` finds the shells Windows Terminal would
list on the same machine (Windows PowerShell, PowerShell, Command Prompt, Git Bash, one entry
per WSL distribution), asks which to offer, copies the program into
`%LOCALAPPDATA%\Programs\tek-tube` with no administrator, writes the choice to
`%APPDATA%\tek-tube\shells.txt`, and creates a `tek terminal` entry for the default shell
plus one entry per shell.  The entries pass a shell *name*, so what a shell entry runs is an
edit to that one text file; `uninstall.cmd` removes everything again and keeps your settings
unless it is asked to purge them.

The program grew what that needs: `--shell` accepts a configured name and splits its command
line the way Windows quotes one — which is the only way a shell under `Program Files` can be
started at all — a `tek-tube.exe` with no arguments runs the shell `shells.txt` names as its
default, and `--list-shells` and `--version` report what is configured and what this build is.

| | |
|---|---|
| Source | [kevinelong/tek-tube](https://github.com/kevinelong/tek-tube) @ `e07faa8` |
| Asset | `tek-tube-0.2.0-win64.zip` |
| Built with | rustc 1.96.0 (ac68faa20 2026-05-25), `cargo build --release`, x86_64-pc-windows-msvc |
| Tests at build time | `cargo test` — 114 passed, 0 failed, 2 ignored |
| `tek-tube.exe` | 942,592 bytes, SHA-256 `ff1c36d0204bb373f93a0e1083077325da9c48ef340fad12705436653d45681e` |
| Archive | 9 files: the program, the setup and uninstall scripts, and the licences |

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
