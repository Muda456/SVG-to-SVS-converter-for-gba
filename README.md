# 山寨机 GBA 即时存档：SVG 转 SVS

这台机器的 GBA 核心是 **gameplaySP（gpSP）** 的 MIPS 移植。`.svg` 是**即时存档**，不是游戏内存档 `.sav`。SRAM 不在 `.svg` 里。

## 怎么用

1. 运行仓库根目录的 `svg2svs.exe`（或双击 `svg转svs.bat`），把 `.svg` 转成 `.svs`。
2. 打开 `gpsp-windows\gpsp.exe`（带菜单的模拟器）。把 `gba_bios.bin` 放进同一个文件夹。
3. 文件 → 选择 ROM，再 文件 → 读取即时存档，选刚才转出的 `.svs`。也可以 文件 → 导入 SVG 存档。
4. RetroArch 的 gpSP 读不了这种 `.svs`。读档成功后，再在游戏里存档，才会得到 `.sav`。

mGBA、VBA-M 不能直接读 gpSP 的即时存档。

也可命令行转换：

```powershell
python tools/svg_convert.py "游戏.svg" -o "输出目录"
```

输出：

- `*.svs` — 经典 gpSP 即时存档（506,943 字节）
- `iwram.bin`、`ewram.bin` 等内存转储（调试用）

## 源码

- `tools/svg2svs.c`、`tools/svg_convert.py` — 转换器
- `gpsp/notaz-gpsp/` — 模拟器（含 Windows 菜单 `pc_gui.c`）
- `gpsp/notaz-gpsp/x86-gui/` — 带菜单的 Windows 编译
- `rockbox-atjboottool/` — 固件解包

编译带菜单的模拟器：

```text
cd gpsp/notaz-gpsp/x86-gui
make
```

## 注意

- 这是即时存档转换，**不含**游戏电池存档（SRAM）。
- 不要把 ROM、BIOS、固件传到公开仓库。
