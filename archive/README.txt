tek-tube 0.3.0 - a Tektronix 4010/4014 storage tube on your screen
==================================================================

WHAT THIS IS
  tek-tube plays the plot files that 1970s graphics packages (PLOT10, DISSPLA,
  ICEMDDN) sent down a serial line to a Tektronix 4010/4014 storage tube, and it
  puts an interactive shell on the same tube.  The picture is a model of the
  hardware rather than a line drawing: a vector is stroked with a hot write beam
  that flashes, then settles into stored charge that stays lit on the flood-gun
  background until the whole screen is erased.

REQUIREMENTS
  Windows 10 or 11, 64-bit.  No installer, no runtime to install, nothing to
  configure.  tek-tube.exe is self-contained (the stroke fonts are inside it).

RUN IT
  1. Put tek-tube.exe anywhere - this folder is fine.
  2. Run it from a terminal, giving it a plot file:

       tek-tube.exe C:\tek-tube\samples\05_snoopy.plt

     The plot files come with the source repository (26 of them in samples\):
       https://github.com/kevinelong/tek-tube
  3. ESC quits.  F11 toggles fullscreen.  F2 opens the options panel (pen rate,
     phosphor, trail, cell size, savable presets).

  With no file it prints its usage:      tek-tube.exe --help

INSTALL IT (START MENU AND SHELLS)
  setup.cmd asks which shells the Start Menu should offer - the ones Windows
  Terminal would list on this machine - copies tek-tube into
  %LOCALAPPDATA%\Programs\tek-tube, and puts "tek terminal" in the Start Menu for
  the default shell plus one entry per shell.  No administrator is needed.

       setup.cmd
       setup.cmd -List                 what shells were found
       setup.cmd -Shells "Windows PowerShell,Git Bash" -Default "Git Bash"
       uninstall.cmd                   remove it again (-Purge: settings too)

  The entries pass a shell *name*; the names and their commands live in
  %APPDATA%\tek-tube\shells.txt, so changing one is an edit to that file.
  "tek-tube.exe --list-shells" prints what is configured.

A WHOLE SHELL ON THE TUBE
       tek-tube.exe --shell powershell.exe --device 4010

  Or drive it from a pipe, which is what these terminals were bought for:
       Get-Process | Format-Table | Out-String | tek-tube.exe -

VERIFY THIS DOWNLOAD
       certutil -hashfile tek-tube-0.3.0-win64.zip SHA256
  and compare with SHA256SUMS.txt in the same release.  In Git Bash or WSL,
  sha256sum -c SHA256SUMS.txt checks the whole release.

IF WINDOWS WILL NOT RUN IT
  SmartScreen warns about an unsigned binary: More info -> Run anyway.
  On Windows 11 with Smart App Control on, a newly built unsigned binary can be
  refused outright (os error 4551) - the source README records that and its fix.

LICENCE
  Functional Source License 1.1, FSL-1.1-MIT (LICENSE.txt in this folder): run
  it, read it, change it, teach with it, use it inside a company - anything except
  a product or service that competes with tek-tube.  Every release converts to the
  MIT licence two years after it is published.

  Third-party material compiled into tek-tube.exe - the Hershey stroke fonts and
  the Rust libraries it links - keeps its own terms: NOTICE.txt and
  THIRD-PARTY-NOTICES.txt in this folder.

  Source, documentation, measurements: https://github.com/kevinelong/tek-tube
  All releases: https://github.com/kevinelong/tek-tube-releases
