# Blood Omen: Legacy of Kain knowledge report

- Date: 2026-08-31
- Project: Blood Omen: Legacy of Kain setup-host release pilot
- Repository/branch: local pilot, `master`
- Retail identity: USA `SLUS-00027`
- Architecture lane: source-only owned-input setup host
- License boundary: portfolio files use GPL-3.0-only; PSXRecomp keeps PolyForm Noncommercial 1.0.0; recomp-ui keeps MIT

## Executive state

The exact `0.3.0` Windows ZIP passes the payload audit and a clean extracted
build. The build used a path with spaces, an invalid inherited
`SSL_CERT_FILE`, and the frozen RetComM toolchain. The operator's earlier
private package reached gameplay, which supports `bootstrap_verified`. The
source-only package still needs its exact installed startup and remote audit.

## Product graduation state

- Current state: `bootstrap_verified`
- Evidence: zero required Studio audit failures; exact ZIP clean build
- Next release gate: exact-package R2 canaries
- Missing hard gates: exact installed startup, risk canaries, and remote redownload audit
- Human-completion coverage: gameplay was confirmed in the earlier private package

## Verified milestones

| Boundary | Evidence | Repeated? | Human confirmed? |
|---|---|:---:|:---:|
| Setup archive payload | 2,003 files; zero forbidden files, generated retail source, private paths, or CRLF shell scripts | Yes | No |
| Exact extracted build | 166 of 166 actions; executable SHA-256 `42CD9F86DFA6536CB4F8853710BA333FBA0DF94A65F86FF31FDEE77A5E39396D` | Yes | No |
| Studio source audit | Zero required failures; two accepted box-art warnings | Yes | No |

## Shared findings consumed

| Finding ID | Status | Evidence |
|---|---|---|
| `PSX-BUILD-013` | independently verified | Runtime `d60f5947e` requires recomp-ui `4eda65430a`; the older UI failed its ABI contract. |
| `PSX-PUB-004` | independently verified | Rejected draft contained copied memory-card files; sealed ZIP contains none. |
| `PSX-PUB-006` | independently verified | Packaging passes with no tracked optional launcher assets. |
| `PSX-WIN-005` | independently verified | C, C++, and RC resolve to RetComM toolchain `1.0.14`; the spaced-path build passes. |
| `PSX-WIN-006` | independently verified | Ambient WSL Bash lacked `zip`; explicit Git-for-Windows Bash produced the final archive. |

## Corpus consulted for the current blocker

- Symptom: Studio found `0.1.0` in an ignored obsolete build directory while
  the release and exact package were `0.3.0`.
- Consulted: `PSX-RUN-001`, `PSX-PUB-004`, `PSX-BUILD-009`, the failure
  catalog, and the release regression ledger.
- Disposition: the sealed ZIP was correct. The stale ignored build cache was
  removed, and the repeated Studio audit passed.

## Reusable artifacts

- `test_setup_package_payload_filter.py`
- `test_windows_rc_compiler_arg.py`
- exact-ZIP independent payload audit

## Performance evidence

Not measured. This pilot does not make a performance claim.

## Quality debt

| Debt | Owner | User impact | Evidence/containment | Removal or acceptance gate |
|---|---|---|---|---|
| No exact installed startup | Title pilot | Setup build does not prove boot | `headless_boot_frames = 0` | Run the exact installed product past the bootstrap gate. |

## Current blockers

1. Complete the exact installed startup and risk canaries.
2. Redownload and audit the private draft before any public release.

## Next decisive experiment

Create a private CI draft from the frozen source identities. Download the
Windows artifact, verify the exact archive content contract, build it, and run
its installed executable past the bootstrap gate.

## Knowledge-base actions

- Updated `PSX-PUB-004`, `PSX-PUB-006`, and `PSX-WIN-005` evidence.
- Added the Windows packaging-shell identity candidate as `PSX-WIN-006`.
- Added the stale build-stamp audit lesson as `PSX-PUB-011`.
- Next independent consumer: Koudelka.
- Upstream candidate: setup packager payload and Windows RC guards.

## v0.3.3 setup correction

The source now uses `Blood_Omen__Legacy_of_Kain_Recompiled` as the only setup executable name. The batch source
gate passes. The exact-ZIP automatic-relaunch canary and remote release audit
remain open. Public `v0.3.0` remains unchanged.

## v0.3.5 three-platform refresh

The source now binds the package-only privacy correction and targets Windows
x64, Linux x64, macOS ARM64, and macOS x64. The replacement build-only CI,
complete archive audit, and native package gates remain required. This source
change does not publish a release or claim platform support.

## 2026-09-04 v0.3.6 POSIX setup-copy candidate

This candidate pins PSXRecomp 08ec704a974b1f3a16335b4afeb340b9eff19926 and recomp-ui be8ac1d03ee19d55394b5a5f2d9d1506edd56659.
Linux and macOS packages use native CMake, Ninja, Python, C, and C++ tools.
Windows keeps the portable toolchain route. This change does not change game
code or the graduation state. Build-only CI and every exact-package release
gate must pass before publication.
