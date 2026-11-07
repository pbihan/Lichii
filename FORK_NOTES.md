# Branch `frizti-amd` (fork pbihan/Lichii)

> **Unofficial and experimental.** This branch combines third-party code for testing on one AMD
> machine. It is not a release of [Frizti's fork](https://github.com/Frizti/Lichii) nor of
> [zortos293/Lichii](https://github.com/zortos293/Lichii), and is not endorsed by their authors.
> It is untested at the time of writing, not maintained, and comes with no support.

## What it contains

[Frizti v3](https://github.com/Frizti/Lichii/releases/tag/frizti-v3) (tag `frizti-v3`, `72e5ad6c`)
with these commits on top:

| Commit | Origin | Purpose |
|---|---|---|
| keep framebuffers within the device limits, pad the stream buffer | zortos293/Lichii #427 (`6c9c06cc`) | stability |
| fail an image that does not fit instead of throwing | zortos293/Lichii #440 (`5358d3db`) | stability |
| fix AMD shader typing, atomic retries and compiler stalls | zortos293/Lichii `68caa6c7` | AMD: builds two Ocarina of Time compute shaders unoptimized, which the AMD driver otherwise spends 6-18 minutes on |
| don't write render target components the fragment shader does not output | zortos293/Lichii #353 (`79c15169`) | AMD: corrupted pixels above the horizon in Ocarina of Time |
| `EDEN_AMD_FP16=1` keeps `shaderFloat16` on the AMD driver | this fork, see [zortos293/Lichii#378](https://github.com/zortos293/Lichii/pull/378) | opt-in, off by default |
| Windows MSVC build only, no release on push | this fork | CI only, do not merge upstream |

Two cherry-picks needed small conflict resolutions (the test list in `src/tests/CMakeLists.txt`
and `vk_graphics_pipeline.h`, where both sides' members were kept).

## Why

On an RX 6600 (Windows 11, driver 2.0.353, Ryzen 5 5600G, 16 GB), the unmodified Frizti v3
binary crashed in each of four launches of Ocarina of Time, with the log ending abruptly a few
seconds after boot or during shader loading. The cause was not determined. Official Lichii builds
93 and 94 run on the same machine. Whether the commits above fix the crash is not known yet.

This is not a general AMD problem: Frizti v3 runs on an RX 9070 XT with the newer driver 2.0.416
(see [zortos293/Lichii#378](https://github.com/zortos293/Lichii/pull/378)). The RX 6600 above used
driver 2.0.353.

## Things to know before testing

- Frizti v3 reserves two physical cores for the GPU threads on CPUs with 6 or more physical
  cores. `EDEN_NX2_PIN_THREADS=0` turns this off.
- Frizti v3 keeps its pipeline cache in its own files (`vulkan-frizti.fork-v52.bin`) and copies
  the official cache (`vulkan.fork-v53.bin`) when its own is missing. On the RX 6600, the copied
  cache produced compute pipeline compilation errors; starting from an empty cache avoids that.
- Without `EDEN_AMD_FP16`, the FP16 change does nothing.

## Builds

Pushes to `nx2-experimental` build the Windows binary (Actions artifacts, GitHub login required).

## Legal

The emulator contains no game, keys or firmware. Use dumps of games and keys from your own
console. Do not share game files.

## License and credits

GPL-3.0-or-later, like the projects it comes from (see `LICENSE.txt` and `LICENSES/`).
Frizti v3 is by Frizti and the contributors credited in its release notes; the cherry-picked
commits are by the zortos293/Lichii contributors. Commits added in this fork were written with
the help of Claude (Anthropic) and are marked as such.
