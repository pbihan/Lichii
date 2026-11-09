# Branch `official-amd` (fork pbihan/Lichii)

> **Unofficial and experimental.** This branch adds a few commits to the official Lichii code for
> testing on one AMD machine. It is not a release of
> [zortos293/Lichii](https://github.com/zortos293/Lichii) and is not endorsed by its authors.
> It is untested at the time of writing, not maintained, and comes with no support.

## What it contains

zortos293/Lichii `nx2` at `ce9be9d1` (build 116 plus #657, #659 and #664) with these commits on top:

| Commit | Origin | Purpose |
|---|---|---|
| Windows builds only, no release on push | this fork | CI only, do not merge upstream |
| wait once per occlusion query resolve on AMD drivers | conongnang/Lichii `a48dcdf8`, from chuppatricky/Lichii `601d3712d2` | AMD: the driver waits on each query of a `WAIT_BIT` copy (11 ms of GPU time per frame on an RX 9070 XT, as reported there) |
| `EDEN_NX2_QUERY_WAIT_ALL=1` restores the per-query wait, and the log states which resolve is used | this fork | A/B switch for the commit above |
| opt-in `EDEN_NX2_PREFLUSH=1` for Switch 2 titles | open PR [zortos293/Lichii#259](https://github.com/zortos293/Lichii/pull/259) (`5b52831f`) | Ocarina of Time: fewer CPU-read flushes; the PR reports ~26 → ~40 fps in Kokiri Forest and some flicker. Off unless set |
| reset the pending-channel poll timer on progress, 1-tick sleeps on Windows | open PR [zortos293/Lichii#621](https://github.com/zortos293/Lichii/pull/621) (`f8bd5af9`) | on Windows, the 50 us poll from #446 slept for 1 ms or more |
| wait only for a macro DMA read's own query reports | open PR [zortos293/Lichii#559](https://github.com/zortos293/Lichii/pull/559) (`4f944e3b`, `816cf6cf`, `9526d695`) | Ocarina of Time: the PR measured this wait at 8-15% of the GPU back thread while walking |

## Things to know before testing

- Cooperative matrices stay off on RDNA1/RDNA2 through upstream #657. An earlier revision of this
  branch used its own device ID check, since removed.
- The three open PRs above were cherry-picked as they stood on 8 November 2026; one include list
  conflict in `vk_rasterizer.cpp` was resolved by keeping both sides.
- The occlusion query change applies to AMD drivers only. Its effect on an RX 6600 had not been
  measured when it was added.
- The per-game setting "Ocarina of Time anti-aliasing: No sharpening" (from upstream) skips the
  game's sharpening network.

## Builds

Pushes to `official-amd` build the Windows binaries (Actions artifacts, GitHub login required).

## Legal

The emulator contains no game, keys or firmware. Use dumps of games and keys from your own
console. Do not share game files.

## License and credits

GPL-3.0-or-later, like the projects it comes from (see `LICENSE.txt` and `LICENSES/`). The base
code is by the zortos293/Lichii, Eglantine, Eden and yuzu contributors; the occlusion query change
is by conongnang and chuppatricky. Commits added in this fork were written with the help of Claude
(Anthropic) and are marked as such.
