# probat v2.4

**a stack of python runs, compared side by side, in one html file — no install, no server, no account.**

probat is a single-file laboratory for quick numerical experiments. It runs Python
in your browser (via [Pyodide](https://pyodide.org)), compiles the results into
verifiable tables, and confronts any two runs: wall-clock race, row diff and
crossed distributions. When a run is good, you **freeze** it into a *lapis* —
a sealed, self-contained artifact that re-verifies itself on open.

![version](https://img.shields.io/badge/version-2.4.0-blue)
![engine](https://img.shields.io/badge/engine-pyodide%200.26.4-yellow)
![license](https://img.shields.io/badge/license-MIT-green)

---

## Quick start

1. Download `probat_v2.4.html` (or clone the repo).
2. Double-click it. That's it. No build, no backend, no dependency.
3. Paste Python in the left pane, press **▶ run**.
4. If your script writes `.json` files, the largest one is auto-loaded back into
   the JSON pane and compiled into a sortable table.
5. Add more runs with **＋ run**, then confront them in the **compare** panel.
6. **⬇ freeze** forges a *lapis*: the page regenerates itself with everything
   embedded — a stone that re-verifies every run on open.

> Uploads and heavy scans run on **your** CPU. probat verifies, the forge computes.

## What a run looks like

Each run is an independent unit:

- **python pane** — your script, executed in a fresh namespace (no globals leak
  between runs, even for completely unrelated scripts).
- **json pane** — its content is exposed to the script as the `DATA` string and
  compiled into the verified table after the run.
- **console** — stdout/stderr, self-healing warnings (fs bridge, module bridge,
  file collisions).
- **verified data** — a sortable table (natural-order sorting, exact bigints
  beyond 2⁵³, never rounded), plus one-click histograms per numeric column.
- **exports** — every file your script wrote becomes downloadable.

## Features

### Compare runs
Pick any two completed runs and get:

- **runtime** — wall-clock race, `vs best` column (speed is a variable);
- **data diff** — added / removed / changed rows, joined on a unique key column
  when one exists, multiset comparison otherwise;
- **distributions** — crossed histograms over common numeric columns, exportable
  as PNG.

### Self-healing execution
- **fs bridge** — a path your script opens but that does not exist is
  auto-completed from your uploads or editor content (warns when it guesses);
  stale bridges are revoked before every run.
- **module bridge** — a missing import is installed on the fly (micropip / pypi
  CDN), then the run retries.
- **deps field** — declare pure-python pypi packages (`sympy`, `sortedcontainers`, …).
- **run queue** — pressing ▶ while another run is active lines the run up;
  timing starts when the run actually starts.

### Precision & safety
- full Python stdlib in wasm: exact bigint arithmetic, `fractions`, `hashlib`;
- scientific stack shipped with pyodide: `numpy`, `sympy`, `scipy`, `pandas`;
- **bigint-safe JSON parsing** — integers past 15 digits stay exact strings in
  the compiled table;
- **sha-256 fingerprint** of all runs + deps, recomputed on every keystroke,
  click-to-copy.

### Freeze → lapis
The page writes itself: all runs, names and content are embedded in a new
`.html` file. A lapis re-compiles its verified tables on open (pure JS, no
engine needed) and re-verifies every run when you press ▶. Frozen artifacts
are lapis — sealed stones.

### Sketch
Hand-drawn annotations over the page (✎), forged into the lapis on freeze.

## Uploads

Drop `.py` / `.json` files anywhere on the IO strip (or the ⬆ button per run,
or straight onto a cell). Uploads are seeded into pyodide's virtual filesystem
for every run and appear as removable chips. The virtual filesystem lives for
the page session only — export what you want to keep.

## Hard browser limits (by design)

| Never | Why |
|---|---|
| `multiprocessing` / `subprocess` | wasm is single-process — multi-core engines stay in the forge |
| C extensions not ported to pyodide (`gmpy2`, `cypari`, …) | micropip fails honestly |
| arbitrary network calls (CORS) | only the pypi CDN is reachable |
| persistence | session-only virtual filesystem — freeze or export |

## Use cases

- verifying a numerical claim before shipping it to the heavy forge;
- A/B-ing two algorithm variants (runtime race + row diff + distributions);
- sharing a reproducible experiment as a single html attachment;
- teaching: exact arithmetic, no setup, everything visible.

## Authors

- **architecte1995** (Antoine Couet) — concept, design, forge
- **Kimi K3** (Moonshot AI) — engineering & co-design of the run stack

Seeded 2026-09-25. Engine: [pyodide](https://pyodide.org).

## License

MIT — see the license block embedded in the page footer.
