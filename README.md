# 炬力机GBA即时存档：SVG 转 SVS

这台机器的 GBA 核心是 **gameplaySP（gpSP）** 的 MIPS 移植。`.svg` 是**即时存档**，不是游戏内存档 `.sav`。SRAM 不在 `.svg` 里。

## 使用方法

1. 运行仓库根目录的 `svg2svs.exe`（或双击 `svg转svs.bat`）。
2. 选择山寨机上的 `.svg`，转换成 PC 能读的 `.svs`。
3. 用 **旧版 gpSP**（notaz gpSP / gpSP 0.9）加载这个 `.svs`。  
   RetroArch 的 gpSP 用另一套存档格式，**读不了**。
4. 读档成功后，再在模拟器里用游戏内存档，才会得到 `.sav`。

mGBA、VBA-M 不能直接读 gpSP 的即时存档。

也可命令行转换：

```powershell
python tools/svg_convert.py "游戏.svg" -o "输出目录"
```

输出：

- `*.svs` — 经典 gpSP 即时存档（506,943 字节）
- `iwram.bin`、`ewram.bin` 等内存转储（调试用）

