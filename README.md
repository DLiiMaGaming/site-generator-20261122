# site-generator — a faster drop-in for makefun-site 1.0.0

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/DLiiMaGaming/site-generator-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/DLiiMaGaming/site-generator-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

Entry for [Build a static site faster, 22 November 2026](https://make.fun/bounty/site-generator-speed-01-run-20261122) on make.fun.

`run(input)` in `solve.js` returns exactly what makefun-site 1.0.0 returns for
the same site — the output tree (every page's `index.html`, the index and tag
pages, `build-report.txt`, and each static file), byte for byte — but faster:
about 2.3× less CPU than the house baseline on the practice set (median
~392 ms vs ~898 ms per full 3-case set on this machine, measured beside the
baseline).

How it gets there — correctness first, then speed:

- Templates are compiled once per `run()` into plain JavaScript functions
  (no tree walk at render time); paths are hoisted to constants.
- Markdown is lexed and rendered in a single pass over the body's lines, with
  no token array.
- Inline rendering flushes plain runs untouched instead of escaping
  character by character.
- `slugify` is memoised per run (titles, tag names and headings repeat a lot).
- Front matter is parsed off a single split; the body stays as lines
  (no re-join / re-split), and word counts are scanned by hand.

`main.ts` is the harness driver: it reads every case from `/input/cases.json`,
answers each one alone with `run()`, and writes `/output/answers.json`.
Node 22.18 or later, no dependencies, no network.

## Run it locally

```sh
MAKEFUN_INPUT_DIR=<dir with cases.json> MAKEFUN_OUTPUT_DIR=<out dir> node main.ts
```

## License

MIT — see [LICENSE](LICENSE).
