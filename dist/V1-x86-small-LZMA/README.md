# UPX V1 — Win32 i386 small LZMA

This directory snapshots the source changes and build inputs for the experimental x86 small-LZMA DEC10 port.

## Source revision

- Upstream base: `upx/upx` `devel` at `932ea7f4852c7642ce1821200a5c35168db21437`
- V1 branch: `stonechs/upx`, `devel`
- V1 commit: `8ac1bf13527cd32da47fc92db17d9677d55af42a`

The directory layout follows the UPX repository: modified files are under `src/`, project build entry points are at the root, helper scripts and stub Makefiles are under their original `src/` and `misc/` paths, and the test archive is under `Test/Snapshots/`. `V1-source.patch` contains the source diff against the upstream base. Build scripts are reference copies from the base revision; V1 did not modify them.

## Decoder and validation

- Win32 i386 `DEC10` micro-LZMA body: 547 bytes (`0x223`), down from 2,878 bytes (`0xB3E`); `DEC20` is unchanged.
- `--small --lzma` on the 58,880-byte smoke PE produced a 29,696-byte EXE; `upx -t` passed, and execution returned 0 with output `tbox\maco`.
- Normal `--lzma` control (DEC20) produced a 32,256-byte EXE; `upx -t` and runtime execution passed.
- The supplemental 5 KB PE size experiment measured 5,089 -> 4,065 bytes with `--small --lzma` and overlay copy, and 5,089 -> 2,048 bytes with `--small --lzma --overlay=strip`. Generated test executables and packed archives are intentionally excluded from this source snapshot.

## Build note

The committed `src/stub/i386-win32.pe.h` is the runtime-verified hybrid header: the micro DEC10 body was transplanted into the checked-in baseline stub and the section map size updated. Full Win32 stub regeneration through the current COFF-to-ELF conversion path was not validated; generated layouts changed alignment/relocations and failed runtime checks. This snapshot preserves the tested header and records that limitation rather than claiming a reproducible full stub regeneration.

Recorded tools: WinLibs MinGW-w64 GCC 16.2.0, Binutils 2.47.20260726, NASM 3.01.
