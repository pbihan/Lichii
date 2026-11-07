# Branch `rx6600-experiments` (fork pbihan/Lichii)

> **Unofficial and experimental.** This branch is a personal test bed in a fork. It is not
> part of [zortos293/Lichii](https://github.com/zortos293/Lichii) and is not endorsed by its
> maintainers or by the authors of Eden, Eglantine or Frizti's fork. It is not maintained, may
> not build against newer upstream code, and comes with no support. Use the official builds for
> playing.

## What it contains

Official Lichii `nx2` at `412c753b` (the build 94/95 release, `222877d`, plus one CMake fix) plus three opt-in switches and fork-only CI
changes. **With none of the switches set, the emulator behaves like the official build.**

| Switch | Change | Files |
|---|---|---|
| `EDEN_AMD_FP16=1` | Keeps `shaderFloat16` on the AMD proprietary driver instead of turning it off. | `vulkan_device.cpp` |
| `EDEN_NX2_PREFLUSH_HOT=N` | Marks a Switch 2 page the guest CPU has read back from GPU memory N times as Preflushable, so later reads skip the full `Finish`. | `buffer_cache.h`, `buffer_cache_base.h` |
| `EDEN_NX2_DMA_ONE_TRIP=1` | Serves a macro DMA read (`FrontDmaRead`) with one hand-off to the GPU split's back instead of up to three. | `maxwell_3d.cpp` |

The FP16 switch alone is proposed upstream in
[zortos293/Lichii#378](https://github.com/zortos293/Lichii/pull/378) (branch `amd-rx6600`), without
the other changes.

## Results so far

Ocarina of Time (`0400486024DEC000`), Kokiri Forest, Windows 11, AMD Radeon RX 6600 (driver
2.0.353), Ryzen 5 5600G, 16 GB DDR4-3200. One machine and short runs: treat these as indications,
not benchmarks.

| Switch | Result |
|---|---|
| none (build 94 base) | ~20-23 fps. RGP: 42.6 ms frame, GPU idle ~45% ("CPU bound"). No thread saturated; the GPU front thread waits on the back ~60% of the time. |
| `EDEN_AMD_FP16=1` | No gain measured on this CPU-bound machine (build 93 base). No visual problem seen. An RX 9070 XT user reported ~20 to 24-25 fps with the equivalent change ([#378](https://github.com/zortos293/Lichii/pull/378)). |
| `EDEN_NX2_PREFLUSH_HOT=3` | Same average (20 fps); lower minimum in one run (4 against 12 fps). No gain. |
| `EDEN_NX2_DMA_ONE_TRIP=1` | Not tested yet. |

Settings tried on the same machine without a gain: lower internal resolution (down to 0.25x),
`Max Resolution` add-on off, `EDEN_NX2_FLUSH_DRAWS=512`, pinning the GPU threads on a 6-core CPU
(slower in a combined run).

## Fork-only CI changes (do not merge upstream)

`.github/workflows/release-binaries.yml` builds the Windows MSVC binary only and never publishes a
release on push. It builds when this code is pushed to `nx2-experimental` (that branch now holds
`frizti-amd`). Builds of this branch: Actions runs 4 (FP16, PREFLUSH_HOT) and 5 (adds
DMA_ONE_TRIP), downloadable as artifacts with a GitHub login while GitHub keeps them.

## Legal

The emulator contains no game, keys or firmware. Use dumps of games and keys from your own
console. Do not share game files.

## License and credits

Same license as the project (GPL-3.0-or-later, see `LICENSE.txt` and `LICENSES/`). The base code
is by the Eden, Eglantine and Lichii contributors. Changes in this branch were written with the
help of Claude (Anthropic) and are marked as such in the commit messages.
