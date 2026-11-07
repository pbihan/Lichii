# Branch `official-amd` (fork pbihan/Lichii)

> **Unofficial and experimental.** This branch adds a few commits to the official Lichii code for
> testing on one AMD machine. It is not a release of
> [zortos293/Lichii](https://github.com/zortos293/Lichii) and is not endorsed by its authors.
> It is untested at the time of writing, not maintained, and comes with no support.

## What it contains

zortos293/Lichii `nx2` at `92f02124` (tag `build-116`) with these commits on top:

| Commit | Origin | Purpose |
|---|---|---|
| Windows builds only, no release on push | this fork | CI only, do not merge upstream |
| wait once per occlusion query resolve on AMD drivers | conongnang/Lichii `a48dcdf8`, from chuppatricky/Lichii `601d3712d2` | AMD: the driver waits on each query of a `WAIT_BIT` copy (11 ms of GPU time per frame on an RX 9070 XT, as reported there) |
| `EDEN_NX2_QUERY_WAIT_ALL=1` restores the per-query wait, and the log states which resolve is used | this fork | A/B switch for the commit above |
| `VK_KHR_cooperative_matrix` off on RDNA1/RDNA2 with AMD's Windows driver; `EDEN_NX2_NO_COOP_MATRIX=1/0` overrides | this fork | avoids the driver crash in [zortos293/Lichii#633](https://github.com/zortos293/Lichii/issues/633) |

## Things to know before testing

- The RDNA1/RDNA2 check uses the PCI device ID range `0x7310`-`0x743F` (discrete Navi 1x/2x).
  Integrated RDNA2 GPUs are not covered; use `EDEN_NX2_NO_COOP_MATRIX=1` there.
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
