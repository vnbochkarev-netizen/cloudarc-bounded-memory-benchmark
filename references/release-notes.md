# CloudArc Bounded-Memory Benchmark 1.2.0

**Release date:** 2026-09-10

## Highlights

- `doctor`: reports interpreter, optional `zstandard` state, `/proc` VmRSS
  availability and repository readiness, with a non-zero exit when the benchmark
  cannot run. No CloudArc checkout required.
- `selfcheck`: 2 MiB pack/unpack with real wall times, peak RSS and SLO verdict;
  fails closed with the doctor report when the repository is absent.
- `badge`: renders a flat shields-style SVG SLO badge from a benchmark JSON
  artifact, so a README can carry the state (peak RSS vs limit).
- Fixed a real environment-dependent defect: the suite asserted `codec=="deflate"`
  unconditionally while the packer prefers `zstandard` when installed, so it was
  50/51 with zstandard and 51/51 without. The deflate case now patches zstandard
  out and a zstd case covers the same bounded-chunk guarantee; CI runs a
  `zstandard` without/with matrix.
- `tests/check_suite.py`: inventory guard so a test module that disappears cannot
  silently shrink the suite.

## Verification

- 59 tests OK on Python 3.12 with zstandard 0.25; 59 OK (1 skipped) on 3.11
  without it.
- `selfcheck --size-mib 2`: pack 1.26 s / 26.2 MiB, unpack 1.15 s / 26.8 MiB,
  SLO PASS (limit 256 MiB).
- CloudArc CI green on both matrix legs.

## Upgrade note

Replace older copies of `cloudarc-bounded-memory-benchmark` with this package.

# CloudArc Bounded-Memory Benchmark 1.1.0

**Release date:** 2026-09-10

## Highlights

- Added `scripts/cloudarc_benchmark.py` with three safe entry points:
  `smoke`, `manual`, and `check-telemetry`.
- Added explicit `--yes` protection for the manual 1/5/10 GiB profile.
- Added strict checks for positive RSS and non-zero bytes when telemetry
  reports successful range or sidecar reads.
- Added post-run validation of JSON schema/status, multi-file cardinality, and
  matching Markdown `PASS` reports before the helper exits successfully.
- Updated the pull request CI workflow to invoke the helper and upload JSON and
  Markdown smoke artifacts.
- Kept the large manual workflow separate from normal CI and preserved the
  single-file and multi-file SLO boundaries.

## Verification

- Skill validation: passed.
- CloudArc test suite: 51/51 passed.
- Helper smoke profile: passed on the reference Windows environment.
- Installed profile copy verified with the helper's range and sidecar telemetry
  checks.

## Upgrade note

Replace older copies of `cloudarc-bounded-memory-benchmark` with this package.
The skill does not enable cloud credentials or activate the native ViBo
backend.
