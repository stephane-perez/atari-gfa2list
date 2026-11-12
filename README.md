# gfa2lst 0.0.1

Converts tokenized GFA-BASIC 3.x (Atari ST) `.GFA` files into ASCII `.LST` listings.

    python3 gfa2lst.py PROG.GFA            # -> PROG.LST (ST charset, CR/LF, loadable by GFA MERGE)
    python3 gfa2lst.py -c PROG.GFA         # UTF-8 + LF, for modern editors / git
    python3 gfa2lst.py -d out/ *.GFA       # batch
    python3 gfa2lst.py -i inl/ PROG.GFA    # save INLINE data as .inl files

Options: `-o FILE|-`, `-d DIR`, `-c/--utf8`, `-i/--save-inline DIR`, `-b/--bug-compat`, `-v`, `-V`.

* Supports GFA 3.x files (magic `GFA-BASIC3`, versions 3 and 4), including protected files
  (variables are then named `v<type>_<index>`). GFA 1.x/2.x, Amiga, DOS and Windows files are refused.
* INLINE data is written as `'` comment lines (hex dump), so the listing stays loadable.
* `-b` reproduces GFA's own export quirk (no indent after `> FUNCTION`); the reference listings
  were exported that way.
* Real constants are printed with the shortest round-trip text (`1000000`, not `1E+06`).

## Tests
    python3 -I tests/test_gfa2lst.py
    GFA_CORPUS=<dir with NAME.gfa/NAME.lst pairs> python3 -I tests/test_gfa2lst.py

Checked byte for byte against listings exported by GFA-BASIC itself (default, hell, sky from
gfalist's `gb36test.a`), and against gfalist on a 142 KB program (only INLINE placement and real
formatting differ).

## Licence / credits
GPL v2 only. Decoding logic is new code, written after studying
[gfalist](https://github.com/mmuman/gfalist) as maintained by **mmuman**, whose fork
is the starting point of this work. Further credits:

* **Peter Backes** : original SKY / OUR NEXT STEP decoder and gfalist (GPL v2), source of the
  file-format knowledge and of the token tables (`LCT/PFT/SFT`);
* **Markus Hoffmann** : INLINE data support in gfalist (per its source comments);
* **mmuman** : the maintained GitHub fork, its fixes and the reference test corpus
  (`tests/gb36test.a`) used to validate this converter.

The tables are extracted from gfalist's `tables.c` by `tools/extract_tables.py`;
`tools/build.py` injects them into `tools/gfa2lst.py.in` to produce `gfa2lst.py`.
