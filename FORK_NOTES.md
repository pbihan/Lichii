# Branch `official-amd` (fork pbihan/Lichii)

> **Unofficial and experimental.** This branch adds a few commits to the official Lichii code for
> testing on one AMD machine. It is not a release of
> [zortos293/Lichii](https://github.com/zortos293/Lichii) and is not endorsed by its authors.
> It is untested at the time of writing, not maintained, and comes with no support.

## What it contains

zortos293/Lichii `nx2` at `3dae119a` (9 November 2026, after build 130) with these commits on top:

| Commit | Origin | Purpose |
|---|---|---|
| Windows builds only, no release on push | this fork | CI only, do not merge upstream |
| opt-in `EDEN_NX2_PREFLUSH=1` for Switch 2 titles | open PR [zortos293/Lichii#259](https://github.com/zortos293/Lichii/pull/259) (`5b52831f`) | fewer CPU-read flushes in Ocarina of Time; off unless set |
| guest accesses of cached memory without the cache mutexes, `EDEN_NX2_GUEST_FAST` | conongnang/Lichii `855f205b`, from Frizti/Fritchii (EXP35) | Ocarina of Time on an RX 9070 XT, Castle Town, as measured there: short walk 52.3 → 57.0 fps |
| `EDEN_NX2_GUEST_FAST=2` by default, `0` restores the locked path | this fork, after conongnang/Lichii `886c0f3d` | keeps the A/B switch that `886c0f3d` removed |
| boot-built pipelines are no longer evicted, `EDEN_NX2_EVICT_PIPELINES=1` restores upstream's 2000-pipeline budget, `=0` never evicts | this fork | RX 6600, 9,800 pipelines built at boot: 5,877 evicted after five minutes, then translated again on the GPU thread in new places (434 in 10 s). Costs ~0.45 MB of driver memory per kept pipeline |
| bounds-check every ASTC decode and unswizzle image store | Frizti/Fritchii `f52631cf`, from CheezerWheezer/Lichii `132cc46c` | AMD: out-of-image stores corrupted other images and faulted the GPU (RX 6750 XT) |

## Things to know before testing

- Shader cache v2 (upstream #1036) replaces `vulkan.fork-v54.bin` with `lichii-cache.v2.bin` and
  migrates the older file on the first boot. The older file is read, not changed.
- Commits this branch carried earlier and dropped: the occlusion query change (upstream since
  #679), #621 (upstream since `bf48fe92`), #559 (no longer fits upstream's query report rework)
  and an `EDEN_NX2_VK_PIPELINE_CACHE` switch (the code it changed was replaced by #1036).
- Not merged: Frizti/Fritchii (based on an older upstream, 109 conflicting files on 8 November)
  and CheezerWheezer/Lichii `amd-superior` (43 conflicting files on 9 November, among them the
  pipeline cache, GPU split and rasterizer). Both publish their own builds.
- Measured on the RX 6600 + Ryzen 5 5600G in Kokiri Forest (build 10 of this fork): neither
  `EDEN_NX2_PREFLUSH=1` nor #621 and #559 changed the frame time (35-47 ms either way), although
  `EDEN_NX2_PREFLUSH=1` cut the CPU reads that flush the GPU from ~103/s to ~9.5/s.

## Builds

Pushes to `official-amd` build the Windows binaries (Actions artifacts, GitHub login required).

## Legal

The emulator contains no game, keys or firmware. Use dumps of games and keys from your own
console. Do not share game files.

## License and credits

GPL-3.0-or-later, like the projects it comes from (see `LICENSE.txt` and `LICENSES/`). The base
code is by the zortos293/Lichii, Eglantine, Eden and yuzu contributors; the ported commits are by
the authors named above (Frizti, conongnang, CheezerWheezer, and #259's author). Commits added in
this fork were written with the help of Claude (Anthropic) and are marked as such.
