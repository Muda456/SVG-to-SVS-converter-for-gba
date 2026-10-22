# 山寨机 gpSP（内置 SVG 即时存档导入）

Windows 版 **gpSP**，带菜单。山寨机 `.svg` 即时存档可在模拟器里直接导入，不用单独的转换工具。

`.svg` 是即时存档，不是游戏内存档 `.sav`。SRAM 不在 `.svg` 里。

## 怎么用

1. 打开 `gpsp-windows\gpsp.exe`
2. 把 `gba_bios.bin` 放进同一个文件夹（本仓库不包含 BIOS）
3. 文件 → 选择 ROM
4. 文件 → 导入 SVG 存档

之后可用：文件 → 保存 / 读取即时存档。选项里可改画面倍数和按键。

读档成功后，再在游戏里存档，才会得到 `.sav`。RetroArch 的 gpSP、mGBA、VBA-M 读不了这种即时存档。

## 源码

- `src/notaz-gpsp/` — 模拟器（Windows 菜单和 SVG 导入在 `pc_gui.c`）
- `src/notaz-gpsp/x86-gui/` — 带菜单的 Windows 编译

```text
cd src/notaz-gpsp/x86-gui
make
```

需要 32 位 gcc、SDL 1.2、zlib。
