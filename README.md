# ATJ2259C GBA `.svg` conversion

This machine’s GBA core is a MIPS port of **gameplaySP (gpSP)** (`cpu_threaded.c`, `memory.c`, `savestate_write_buffer[506940+%d]`). The `.svg` files are **instant savestates**, not battery `.sav` files. SRAM is not stored in them (only a host-side flash pointer).

The original handheld is dead, so patching `gba.drv` to write `.sav` is not useful unless you can run that MIPS + MicroC/OS-II stack on a PC (full SoC emulation). That is not a practical path.

## What works instead

1. **Parse the `.svg` and rebuild a classic gpSP `.svs`** (this repo).
2. Load that `.svs` in **old gpSP** (notaz gpSP / gpSP 0.9 SDL).  
   Current RetroArch gpSP uses BSON savestates and **will reject** these files.
3. After it loads in old gpSP, use that emulator’s **in-game save** if you later want a `.sav`. That `.sav` is created by the emulator from RAM, not copied out of the `.svg`.

mGBA / VBA-M cannot load gpSP packed states. A full mGBA serializer is a separate project; the dumped `iwram.bin` / `ewram.bin` / `io.bin` are the raw materials for it.

## gpSP source (for loading `.svs`)

Downloaded into `gpsp/`:

- `gpsp/notaz-gpsp` — https://github.com/notaz/gpsp (ARM Linux fork; includes `x86/` SDL makefile)
- `gpsp/gpsp-0.9` — https://github.com/BASLQC/gPSP (Exophase 0.9; also has `x86/` SDL makefile)

There is no official Windows installer. PC build is 32-bit SDL 1.2:

```text
cd gpsp/notaz-gpsp/x86
make
```

Needs 32-bit gcc, SDL 1.2, zlib, and `gba_bios.bin` (copy from `aaa/firmware_files/FWIM/gba_bios.bin`). F5/F7 are save/load state in the PC build.

PSP 0.9 signed pack (your `gpsp09.zip`) is extracted at `gpsp/gpsp09-psp/gpsp/` (`EBOOT.PBP` + `game_config.txt`). That binary only runs on a PSP, not on Windows.

## Convert a clone `.svg`

```powershell
python tools/svg_convert.py "aaa\恶魔城~1.svg" -o "aaa\converted"
```

Output:

- `*.svs` — classic gpSP savestate (506,943 bytes)
- `iwram.bin` `ewram.bin` `vram.bin` `oam.bin` `palette.bin` `io.bin`
- `cpu.json` — PC, CPSR, timers, DISPCNT
- `palette_strip.bmp` — 512-color GBA palette (sanity check)

## Firmware unpack (already done)

`aaa/firmware.afi` is SQLite. Files are in `aaa/firmware_files/`. GBA pieces: `FWIM/gba.drv`, `gba_bios.bin`, `gba_buff.drv`, `emulator.app`.

```powershell
python tools/atj_unpack.py --fwu "firmware.fw" -o "firmware.afi"
```
