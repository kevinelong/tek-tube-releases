# Third party material in this release

The files published here are built from **tek-tube**
(<https://github.com/kevinelong/tek-tube>) and carry that project's third-party material.
This is the part of it that is compiled into `tek-tube.exe`; the full list for the source
tree, including the sample plot files and the reference photographs that are *not*
redistributed here, is the source repository's own `NOTICE.md`.

## Hershey vector fonts (`assets/futural.jhf`, `assets/futuram.jhf`, `assets/gothgbt.jhf`)

The alpha (text) mode glyph outlines are Hershey vector fonts.  All three `.jhf` files
are covered by the use restriction below.

```
USE RESTRICTION:
	This distribution of the Hershey Fonts may be used by anyone for
	any purpose, commercial or otherwise, providing that:
		1. The following acknowledgements must be distributed with
			the font data:
			- The Hershey Fonts were originally created by Dr.
				A. V. Hershey while working at the U. S.
				National Bureau of Standards.
			- The format of the Font data in this distribution
				was originally created by
					James Hurt
					Cognition, Inc.
					900 Technology Park Drive
					Billerica, MA 01821
					(mit-eddie!ci-dandelion!hurt)
		2. The font data in this distribution may be converted into
			any other format *EXCEPT* the format distributed by
			the U.S. NTIS (which organization holds the rights
			to the distribution and use of the font data in that
			particular format).
```

`futural.jhf` was obtained from
<https://github.com/kamalmostafa/hershey-fonts> (glyph data permissively
licensed; the loader in that repository is GPL and was **not** used - the
`.jhf` parser in `src/font.rs` was written from the format description in the
original distribution notes).

## Rust libraries

`tek-tube.exe` statically links the crates in `Cargo.lock`, listed with their versions,
their licence expressions and the full text of every licence file they ship in
[THIRD-PARTY-NOTICES.txt](./THIRD-PARTY-NOTICES.txt) (34 crates in this build; all of them
permissive - MIT, Apache-2.0, BSD-2-Clause, Zlib, 0BSD and Unicode-3.0).  None of them is
covered by the Functional Source License that covers tek-tube itself.
