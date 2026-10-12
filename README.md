# tek-tube releases — downloads for the Tektronix 4010/4014 storage tube

**tek-tube** is a Tektronix 4010/4014 **direct-view bistable storage tube** display
emulator for Windows.  It plays back the plot files that 1970s mainframe and minicomputer
graphics packages (PLOT10, DISSPLA, ICEMDDN, ...) pushed down a serial line to a Tek
terminal — the files in
[Tektronix 4052 Vector Graphics Computer - PLOT10 Demo](https://www.youtube.com/watch?v=2SbCIP1m6hs) —
and it can also put the tube's own screen in front of a shell, taking keystrokes like the
terminal it is emulating.

What makes it look like the real thing is the tube model rather than the line drawing: a
vector is stroked with a hot write beam that flashes almost white-green, then settles into
the medium acid green stored image that stays on the flood-gun lit target until the whole
screen is erased.  The stored image, the flash, the dark blotchy background, the halation
around the strokes and the hue of the phosphor were calibrated against photographs of
museum hardware, not against other emulators.

**Source, documentation and the plot files to play: <https://github.com/kevinelong/tek-tube>**
(that repository is public and carries the full write-up: the protocol, the tube model,
the measurements against real hardware, and how to drive it from a pipe.)

## Download

**[Get the latest release](https://github.com/kevinelong/tek-tube-releases/releases/latest)**
— Windows 10 or 11, 64-bit:

| File | What it is |
|---|---|
| `tek-tube-<version>-win64.zip` | `tek-tube.exe` plus `setup.cmd`/`setup.ps1`, `uninstall.cmd`/`uninstall.ps1`, `README.txt`, `LICENSE.txt`, `NOTICE.txt` and `THIRD-PARTY-NOTICES.txt` |
| `SHA256SUMS.txt` | SHA-256 of every file in the release |

Windows 10 or 11, 64-bit.  No runtime to install and no administrator is needed: the
executable is self-contained (the stroke fonts are compiled into it).

## Install it

Unzip anywhere and run **`setup.cmd`**.  That is the whole installation:

- it asks which shells the Start Menu should offer — the ones Windows Terminal would list on
  the same machine: Windows PowerShell, PowerShell, Command Prompt, Git Bash, and one entry
  per WSL distribution;
- it copies `tek-tube.exe` and its licences into `%LOCALAPPDATA%\Programs\tek-tube`, nowhere
  system-wide;
- it puts **tek terminal** in the Start Menu for the shell you choose as the default, plus
  one entry per shell, so **tek terminal (Git Bash)** opens the tube with Git Bash on it;
- it writes the choice to `%APPDATA%\tek-tube\shells.txt`, so changing which shell an entry
  runs is an edit to that one text file — no reinstall.

```sh
setup.cmd                       # ask which shells to offer
setup.cmd -List                 # print the shells found on this machine
setup.cmd -Shells "Windows PowerShell,Git Bash" -Default "Git Bash"
setup.cmd -InstallDir C:\tools\tek-tube -NoShortcuts   # install, no Start Menu entries
uninstall.cmd                   # remove it again (-Purge also removes your settings)
```

`tek-tube.exe --list-shells` prints what is configured, and a `tek-tube.exe` started with no
arguments at all — a shortcut, a double click, the taskbar — runs the shell the file marks as
`default`.

Prefer not to install anything?  Unzip and run `tek-tube.exe` directly: one file, no setup,
no Start Menu entry.

macOS and Linux are not published here.  Both build from the source repository (see
**Build it yourself** below); only the Windows build has been exercised.

## Quick start

1. **Unzip** the download anywhere, for example `C:\tek-tube`.
2. **Get a plot file.**  The source repository ships 26 of them in `samples/` — Snoopy as
   the Red Baron, the DISSPLA showcase, a US map, a wizard, a 4014 grayscale point plot:

   ```sh
   git clone https://github.com/kevinelong/tek-tube
   ```

   or use the green **Code → Download ZIP** button on that page if you have no git.
3. **Run it.**  Open a terminal in the unzipped folder:

   ```sh
   tek-tube.exe C:\tek-tube\tek-tube\samples\05_snoopy.plt
   ```

   The window opens fullscreen and the pen starts drawing the file at 19200 baud, so the
   image builds up at the speed a 1970s plotter would have produced it.  **ESC quits**,
   **F11 toggles fullscreen**, **F2 opens the options panel** (pen rate, phosphor, trail,
   cell size, saved presets).

A few things worth trying, with the same file:

```sh
tek-tube.exe samples\05_snoopy.plt                     # live drawing, fullscreen
tek-tube.exe --baud 2400 samples\16_gplanet.plt        # slower line, longer flash
tek-tube.exe --device 4010 samples\test.plt            # 11 inch 4010 instead of 4014
tek-tube.exe samples\spock.plt --png spock.png --settle 2.0   # write a still image instead
tek-tube.exe --help                                    # the full option list
```

### As a terminal

`--shell` runs a command under a pseudoconsole and renders that session on the tube — the
page keeps the 4010's own 74x35 shape and the cell grows with the window:

```sh
tek-tube.exe --shell powershell.exe --device 4010
```

Anything that writes 4010 or PLOT10 protocol can also drive it through a pipe, which is
what these terminals were bought for:

```sh
Get-Process | Format-Table | Out-String | tek-tube.exe -
```

A storage tube cannot scroll: past its last row the whole screen has to be erased
(`--autoclear`, on by default), so a long listing keeps only its final page.  The source
README explains why typing into a *plot file* is not interactive and what `--shell` adds.

## If Windows will not run it

- **"Windows protected your PC" (SmartScreen).**  The binary is not code-signed.  Choose
  *More info → Run anyway*.
- **Smart App Control refuses it.**  On a Windows 11 machine with Smart App Control on, a
  newly built unsigned binary can be refused outright (`os error 4551`).  That is a Code
  Integrity policy decision, not an antivirus one; the source README has the recorded
  evidence and the resolution (turning Smart App Control off, an elevation-required
  setting).  Nothing in the downloaded zip changes that verdict.
- **The picture looks soft.**  The window is pixel-exact by design and does not scale with
  the display's DPI setting; check your monitor scaling rather than the zoom options.
- **Nothing on screen after the first draw.**  A settled target is not recomposed, so an
  idle tube costs nothing — that is the emulator being faithful, not a hang.

## Verify the download

Each release carries `SHA256SUMS.txt`.  In PowerShell or `cmd`:

```sh
certutil -hashfile tek-tube-<version>-win64.zip SHA256
```

The hash must match the line for that file.  In Git Bash or WSL, `sha256sum -c
SHA256SUMS.txt` does the whole release at once.

## Build it yourself

```sh
git clone https://github.com/kevinelong/tek-tube
cd tek-tube
cargo build --release          # target/release/tek-tube.exe
cargo test                     # the unit and end-to-end tests
```

Rust 1.96 or newer.  The three dependencies (`minifb`, `png`, `portable-pty`) are the
whole dependency tree's top level; see `Cargo.toml` and `NOTICE.md` in the source
repository.

## What changed in each build

[CHANGELOG.md](./CHANGELOG.md) — every release names the source commit it was built from.

## Licence

tek-tube is source-available under the **[Functional Source License 1.1](./LICENSE.md)**
(`FSL-1.1-MIT`), the same terms as the source repository: use it for anything except a
product or a service that competes with tek-tube, and every release converts to the MIT
licence two years after it is published.  Running it, reading it, modifying it, teaching
with it and using it inside a company are all inside the grant.  The binaries published
here may be downloaded, run and passed on under those terms.

The third-party material compiled into `tek-tube.exe` — the Hershey stroke fonts, and the
Rust libraries listed in [NOTICE.md](./NOTICE.md) — keeps its own terms.
