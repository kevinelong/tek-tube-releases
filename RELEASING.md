# Cutting a release

The binaries here live in GitHub release assets, never in git — this repository stays a
download page and a change log.  To publish a build:

1. **Build and test the source repository** (`C:/Users/kevin/tek/tek-tube`):

   ```sh
   cargo test
   cargo build --release
   tek-tube.exe samples/05_snoopy.plt --png smoke.png --baud 0 --settle 2.0
   ```

   The last line is the smoke test: it renders headlessly, so it proves the release
   binary runs without a window and writes the picture the source README shows.

2. **Record what the changelog entry needs** — the commit the build came from, the size
   and the hash of the executable:

   ```sh
   git rev-parse --short HEAD
   ls -l target/release/tek-tube.exe
   sha256sum target/release/tek-tube.exe
   ```

3. **Assemble the archive** — `tek-tube.exe` plus `README.txt`, `LICENSE.txt`,
   `NOTICE.txt` and `THIRD-PARTY-NOTICES.txt`.  The three text files are copies of
   `LICENSE.md`, `NOTICE.md` and `THIRD-PARTY-NOTICES.txt` in this repository (the
   licence terms must travel with the binary, so the archive carries text, not links).
   `THIRD-PARTY-NOTICES.txt` is regenerated whenever the dependency list changes:

   ```sh
   cargo tree -e normal --prefix none --format '{p}|{l}'   # then the licence files
   ```

   from `$HOME/.cargo/registry/src/*/` — one section per crate, full licence texts.

4. **Write the `CHANGELOG.md` section**, then tag and publish:

   ```sh
   gh release create v0.1.0 -R kevinelong/tek-tube-releases \
     --title "tek-tube 0.1.0" --notes-file <notes> \
     tek-tube-0.1.0-win64.zip SHA256SUMS.txt
   ```

5. **Verify it anonymously** — a release asset on a public repository must download
   without a token, and the bytes must hash to what `SHA256SUMS.txt` says:

   ```sh
   curl -sIL https://github.com/kevinelong/tek-tube-releases/releases/latest/download/tek-tube-0.1.0-win64.zip
   ```

   Extract the archive somewhere empty and run the executable there.  `tek-tube.exe` is
   self-contained, so it must render a PNG with nothing else in the directory; if it only
   works next to the build tree, something is being read from disk that should be in the
   binary.

6. **Cross-check both repositories.**  The source repository's README points at
   `tek-tube-releases`, this repository's README points back at the source, and the
   changelog names the source commit.
