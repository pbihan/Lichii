# Branch `official-amd` (fork pbihan/Lichii)

> **Unofficial and experimental.** This branch adds a few commits to the official Lichii code for
> testing on one AMD machine. It is not a release of
> [zortos293/Lichii](https://github.com/zortos293/Lichii) and is not endorsed by its authors.
> It is untested at the time of writing, not maintained, and comes with no support.

## What it contains

zortos293/Lichii `nx2` at `aae0b063` (build 117 plus 28 later commits) with these commits on top:

| Commit | Origin | Purpose |
|---|---|---|
| Windows builds only, no release on push | this fork | CI only, do not merge upstream |
| opt-in `EDEN_NX2_PREFLUSH=1` for Switch 2 titles | open PR [zortos293/Lichii#259](https://github.com/zortos293/Lichii/pull/259) (`5b52831f`) | Ocarina of Time: fewer CPU-read flushes; the PR reports ~26 → ~40 fps in Kokiri Forest and some flicker. Off unless set |
| reset the pending-channel poll timer on progress, 1-tick sleeps on Windows | open PR [zortos293/Lichii#621](https://github.com/zortos293/Lichii/pull/621) (`f8bd5af9`) | on Windows, the 50 us poll from #446 slept for 1 ms or more |
| opt-in `EDEN_NX2_VK_PIPELINE_CACHE=1` keeps Eden's own Vulkan pipeline cache file on the AMD and NVIDIA drivers | this fork | AMD's Windows driver appears to rebuild every cached pipeline for each new executable; the file depends only on the driver. Costs host memory and a save after boot |
| wait only for a macro DMA read's own query reports | open PR [zortos293/Lichii#559](https://github.com/zortos293/Lichii/pull/559) (`4f944e3b`, `816cf6cf`, `9526d695`) | Ocarina of Time: the PR measured this wait at 8-15% of the GPU back thread while walking |

## Things to know before testing

- Cooperative matrices stay off on RDNA1/RDNA2 through upstream #657. An earlier revision of this
  branch used its own device ID check, since removed.
- The three open PRs above were cherry-picked as they stood on 8 November 2026; one include list
  conflict in `vk_rasterizer.cpp` was resolved by keeping both sides.
- The occlusion query change this branch carried earlier is now upstream (#679) and was dropped
  here.
- Measured on the RX 6600 + Ryzen 5 5600G in Kokiri Forest: neither `EDEN_NX2_PREFLUSH=1` nor
  #621 and #559 changed the frame time (35-47 ms either way), although `EDEN_NX2_PREFLUSH=1` cut
  the CPU reads that flush the GPU from ~103/s to ~9.5/s.
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
