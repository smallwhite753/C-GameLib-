# C++GameLib 小游戏

基于单头文件库 [GameLib](https://github.com/skywind3000/GameLib)（Win32 GDI，版本 1.9.9）的 Windows 小游戏示例。当前仓库包含公共库和 4 个可独立编译运行的游戏。默认不依赖 SDL，也不需要额外安装图形库。

## 开发环境

下面是能编译并运行这些示例的环境要求。游戏是 2D 小窗口程序，普通办公电脑即可，不要求独立显卡。

| 项目 | 要求 |
| --- | --- |
| 操作系统 | 64 位 Windows 10 或更高版本。已在 Windows 10 专业版（10.0.19045）上整理 |
| 处理器 | 64 位 x86 处理器。4 核及以上即可，例如 Intel Core i5 同级 |
| 内存 | 8 GB 及以上。示例本身占用很小 |
| 显卡 | 支持正常桌面显示即可，核显可以运行，例如 Intel UHD Graphics |
| 磁盘 | 源码、编译器和生成的 exe 合计不到 1 GB |
| 编译器 | MinGW-W64 的 g++，建议 GCC 7.3 及以上，推荐 GCC 15。编译时能在命令行直接执行 `g++` |
| 其他依赖 | 不需要安装 SDL、DirectX 或额外图形库 |

说明：

- 把 MinGW 的 `bin` 目录加入系统 PATH 后，新开一个终端，执行 `g++ --version` 能看到版本号，即可开始编译。
- 如果电脑上装了多套 MinGW（例如 Qt 自带的和单独安装的），命令行里的 `g++` 应指向准备用来编译本项目的那一套，避免混用。
- Win32 版本只使用系统自带的 `user32`、`gdi32`、`winmm`、`gdiplus`、`ole32`。Windows 10 已包含这些库。
- `common\GameLib.SDL.h` 是可选的 SDL2 后端。当前 4 个示例在 Windows 上默认使用 `GameLib.h`，按本文部署时不用安装 SDL2。

目录结构：

```text
C++GameLib
├── common
│   ├── GameLib.h          Win32 单头文件库（示例默认使用）
│   └── GameLib.SDL.h      SDL2 后端（可选，按本文部署时不用）
├── 07_shooting            简单射击
├── 08_breakout            打砖块
├── 09_snake               贪吃蛇
└── 14_space_shooter       太空射击
```

每个游戏目录下是 `src`（源码）和 `out`（编译出的 exe）。

## 功能效果

公共库提供窗口、60 帧循环、键盘鼠标、绘图、精灵、音效和简单界面。4 个示例都是独立窗口程序，按 Esc 退出。

### 07 简单射击（Shooting Stars）

窗口 640×480。底部是青色三角飞船，红色方块从上方落下。

- 左右方向键移动飞船，空格发射子弹。
- 子弹打中目标得 1 分。目标越出屏幕底部，或撞到飞船，扣 1 条命。初始 5 条命。
- 分数越高，目标出现越快、下落越快。
- 生命耗尽后显示 Game Over，按 R 重新开始。

画面是纯色图形，不依赖图片和音效文件。

### 08 打砖块（Breakout）

窗口 640×480。上方 6 行 10 列彩色砖块，底部白色挡板，白色小球。

- 左右方向键移动挡板。开局按空格发球。
- 球撞墙、挡板会反弹。打到挡板的位置不同，水平速度不同。
- 打掉砖块得分，越靠上的砖分数越高。3 条命，球掉出底部扣命。
- 砖块全部打完显示 You Win，生命耗尽显示 Game Over。两种结束都可以按 R 重开。
- 若同目录能找到 wav 音效，发球、反弹、碎砖、失败和胜利会播放对应声音。找不到音效文件时，游戏画面和操作仍然正常，只是没有声音。

### 09 贪吃蛇（Snake）

20×20 格子，右侧有分数、长度和操作提示。

- 方向键控制蛇头。不能直接反向。
- 吃到红色食物加 10 分并变长，移动间隔会逐渐缩短。
- 撞墙或撞到自己则 Game Over，按 R 重新开始。
- 按 P 暂停或继续。

画面是格子绘制，不依赖图片和音效文件。

### 14 太空射击（Space Shooter）

窗口 480×640。星空向下滚动，玩家飞船在画面下方。

- 方向键上下左右移动，按住空格连续射击。
- 敌机分两种：普通敌机 1 点生命，另一种 2 点生命。敌机会移动并向下发射子弹。
- 击毁敌机得分，并播放爆炸。累计击毁数量后等级上升，敌机出现更快、子弹更快。
- 被敌机或敌弹打中扣 1 条命，并有短暂无敌闪烁。初始 3 条命。
- 生命耗尽显示最终分数和等级，按 R 重新开始。
- 若能加载到 `plane0.png`、`bullet.png`、`explosion.png`，会使用这些图片；加载失败时自动改用程序绘制的飞船、子弹和圆形爆炸，游戏仍可玩。音效文件缺失时不影响操作。

## 如何部署

先确认终端里能执行 `g++ --version`。然后进入本仓库根目录（包含 `common`、`07_shooting` 等文件夹的那一层），再编译。四个游戏互不依赖，可以只编译想玩的那一个。下面的路径都相对于仓库根目录，与源码放在哪个盘无关。

### 1. 编译

在仓库根目录打开 PowerShell，执行对应命令。

简单射击：

```powershell
New-Item -ItemType Directory -Force -Path "07_shooting\out" | Out-Null
g++ "07_shooting\src\07_shooting.cpp" -o "07_shooting\out\shooting.exe" -mwindows -static-libgcc -static-libstdc++
```

打砖块：

```powershell
New-Item -ItemType Directory -Force -Path "08_breakout\out" | Out-Null
g++ "08_breakout\src\08_breakout.cpp" -o "08_breakout\out\breakout.exe" -mwindows -static-libgcc -static-libstdc++
```

贪吃蛇：

```powershell
New-Item -ItemType Directory -Force -Path "09_snake\out" | Out-Null
g++ "09_snake\src\09_snake.cpp" -o "09_snake\out\snake.exe" -mwindows -static-libgcc -static-libstdc++
```

太空射击：

```powershell
New-Item -ItemType Directory -Force -Path "14_space_shooter\out" | Out-Null
g++ "14_space_shooter\src\14_space_shooter.cpp" -o "14_space_shooter\out\space_shooter.exe" -mwindows -static-libgcc -static-libstdc++
```

`-mwindows` 表示启动后只有游戏窗口，不弹出黑色控制台。`-static-libgcc -static-libstdc++` 把 MinGW 运行库打进 exe，拷到其他 Windows 电脑时不必再带 `libgcc_s_seh-1.dll` 和 `libstdc++-6.dll`。

如果提示找不到 `g++`，说明 MinGW 的 `bin` 还没加入 PATH，或当前终端是在修改 PATH 之前打开的。重新打开终端后再编译。

### 2. 运行

编译成功后，在仓库根目录双击对应 exe，或执行：

```powershell
.\07_shooting\out\shooting.exe
.\08_breakout\out\breakout.exe
.\09_snake\out\snake.exe
.\14_space_shooter\out\space_shooter.exe
```

### 3. 可选素材

仓库里可以不带 `assets` 目录。07 和 09 不需要素材。08 和 14 在缺少 wav、png 时仍能运行。

如果要给打砖块和太空射击补上声音和图片，把文件放到游戏目录的上一级 `assets`，或放到 exe 同级的 `assets`。程序会依次查找 `../assets/...` 和 `assets/...`。从 `out` 目录启动时，上一级就是该游戏自己的目录。

打砖块使用这些 wav（放在 `assets\sound\`）：

- `jump.wav`、`hit.wav`、`explosion.wav`、`click.wav`、`game_over.wav`、`victory.wav`
- `note_do_high.wav`、`note_si.wav`、`note_la.wav`、`note_sol.wav`、`note_fa.wav`、`note_mi.wav`

太空射击额外使用：

- 图片：`assets\plane0.png`、`assets\bullet.png`、`assets\explosion.png`（爆炸图按 32×32 四帧横排）
- 音效：`assets\sound\click.wav`、`hit.wav`、`coin.wav`、`note_do_high.wav`、`explosion.wav`、`game_over.wav`

### 4. 拷到其他 Windows 电脑

1. 按上面的命令编译出 exe。
2. 把 exe 单独拷走即可。Win32 版本不需要 SDL，目标电脑也不需要安装 MinGW。
3. 目标系统需要是 64 位 Windows 10 或更高版本，并带有系统自带的 GDI、GDI+ 和 winmm。
4. 若使用了图片或音效，把 `assets` 按上一节的相对位置一起拷贝。不拷素材时，07、09 效果不变；08 没有声音；14 使用程序绘制的飞船和爆炸。

## 参考

本项目参考 [GameLib](https://github.com/skywind3000/GameLib) 整理。`common` 里的 `GameLib.h`、`GameLib.SDL.h`，以及简单射击、打砖块、贪吃蛇、太空射击示例，都来自这个仓库。它是给 C++ 初学者用的单头文件游戏库，Win32 版本不需要 SDL、DirectX 或 OpenGL。

- 项目仓库：[https://github.com/skywind3000/GameLib](https://github.com/skywind3000/GameLib)
- 项目说明：[README.md](https://github.com/skywind3000/GameLib/blob/master/README.md)
- 接口手册：[docs/manual.md](https://github.com/skywind3000/GameLib/blob/master/docs/manual.md)
- API 速查：[docs/quickref.md](https://github.com/skywind3000/GameLib/blob/master/docs/quickref.md)
