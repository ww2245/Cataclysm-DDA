# Cataclysm: Dark Days Ahead (CDDA) 项目总览

> 本文档用于快速理解本项目的整体结构，帮助新成员、Mod 作者与开发者建立全局认知。
> 内容基于仓库当前构建版本与源码目录整理。

---

## 一、项目基本信息

| 项目 | 内容 |
| --- | --- |
| 项目名称 | Cataclysm: Dark Days Ahead（简称 CDDA） |
| 中文名 | 大灾变：黑暗之日 |
| 游戏类型 | 回合制末日废土生存 Roguelike |
| 主语言 | C++（大量使用 C++20 特性） |
| 数据驱动 | 游戏绝大部分内容由 JSON 定义，位于 `data/` 目录 |
| 开源许可证 | CC BY-SA 3.0（Creative Commons Attribution-ShareAlike 3.0） |
| 当前版本 | 0.J（开发中），构建号见 `VERSION.txt`（2026-09-15-0041） |
| 官方仓库 | https://github.com/CleverRaven/Cataclysm-DDA |
| 贡献者规模 | 超过 1000 名志愿者 |
| 构建系统 | CMake（>= 3.20）与 Makefile 双支持 |
| 图形后端 | ncurses（文本模式）/ SDL3（图形 tiles，SDL2 已被移除） |
| 测试框架 | Catch2（位于 `tests/` 目录） |

---

## 二、核心特点

- **程序化生成的持久化世界**：每局游戏生成一张巨型、可持续探索的开放世界地图。
- **深度生存模拟**：体温、饥饿、口渴、伤口、感染、精神、辐射、瘾症等完整生存系统。
- **丰富的玩法系统**：物品、制作、载具、变异、仿生（CBM）、派系、基地建设等。
- **数据驱动 + Mod 友好**：游戏内容以 JSON 定义，支持通过 Mod 大规模扩展或改写玩法。
- **双渲染后端**：文本终端版（ncurses）与图形版（SDL3 tiles）共享同一套游戏逻辑。

---

## 三、顶层目录结构总表

| 目录 / 文件 | 用途说明 |
| --- | --- |
| `src/` | C++ 源代码（约 835+ 文件），游戏引擎与逻辑主体 |
| `src/third-party/` | 第三方库：flatbuffers、fmt、imgui、imtui、jc_voronoi、pinyin、plf、snmalloc、zstd |
| `src/chkjson/` | JSON 校验工具（chkjson） |
| `data/` | 游戏数据（JSON），游戏内容主体 |
| `data/json/` | 核心游戏数据（物品、怪物、配方、地图、变异等） |
| `data/mods/` | 内置 Mod（Aftershock、Magiclysm、Backrooms、DinoMod 等 40+ 个） |
| `data/core/` | 核心基础数据 |
| `data/raw/` | 原始/预处理数据（如 names、字体等） |
| `gfx/` | 图形资源（tileset 图集） |
| `lang/` | 本地化 / 翻译文件（Transifex 工作流） |
| `doc/` | 文档总目录 |
| `doc/c++/` | C++ 开发文档（编译、代码风格、测试等） |
| `doc/JSON/` | JSON 数据格式文档（物品、怪物、载具等） |
| `doc/user-guides/` | 玩家向使用指南 |
| `tests/` | Catch2 单元测试 |
| `tools/` | Python 辅助脚本工具（格式化、数据校验等） |
| `build-scripts/` | 构建脚本（含 MSVC / 跨平台工具链配置） |
| `CMakeModules/` | CMake 自定义模块 |
| `msvc-full-features/` | Visual Studio 完整功能解决方案 |
| `android/` | Android 平台构建 |
| `pch/` | 预编译头文件 |
| `utilities/` | 其他实用工具 |
| `doxygen_doc/` | Doxygen 文档配置 |
| `Makefile` | Make 构建脚本 |
| `CMakeLists.txt` | CMake 构建脚本主文件 |
| `CMakePresets.json` | CMake 预设（Windows MSYS2 / MSVC 等配置） |
| `vcpkg.json` | Windows 依赖清单（SDL3、glslang 等） |
| `README.md` / `CONTRIBUTING.md` | 项目说明 / 贡献指南 |
| `VERSION.txt` | 构建版本信息 |

---

## 四、`src/` 主要源码模块（按功能归类）

| 模块 / 文件 | 职责 |
| --- | --- |
| `main.cpp` | 程序入口，负责初始化与主循环调度 |
| `game.cpp` / `game.h` | 游戏主循环与全局核心状态 |
| `avatar.cpp` / `avatar.h` | 玩家角色（Avatar），玩家专属行为 |
| `character.cpp` / `character.h` | 角色基类（玩家与 NPC 共有逻辑） |
| `npc.cpp` / `npc.h` | NPC 行为与 AI |
| `monster.cpp` / `monster.h` | 怪物实体与行为 |
| `item.cpp` / `item.h` | 物品系统 |
| `map.cpp` / `map.h` | 局部地图（区块）与地形交互 |
| `mapgen.cpp` | 地图生成 |
| `overmap.cpp` / `overmap.h` | 大地图（Overmap）与世界结构 |
| `vehicle.cpp` / `vehicle.h` | 载具系统 |
| `crafting.cpp` / `crafting.h` | 制作系统 |
| `recipe.cpp` / `recipe.h` / `recipe_dictionary.*` | 配方系统 |
| `bionics.cpp` / `bionics.h` | 仿生（CBM）系统 |
| `mutation.cpp` / `mutation.h` | 变异系统 |
| `activity_*.cpp` / `activity_*.h` | 玩家活动（耗时动作）系统 |
| `activity_actor.cpp` / `activity_actor.h` | 活动执行器定义 |
| `ballistics.cpp` / `ballistics.h` | 弹道与远程攻击计算 |
| `catacurses/` | ncurses 文本界面封装层 |
| `sdltiles.cpp` | SDL3 图形界面后端 |
| `cata_imgui.*` / `third-party/imgui` | 游戏内 ImGui 调试界面 |
| `*_factory.*`（如 `item_factory`、`monstergenerator`） | 从 JSON 加载数据的工厂类 |
| `json.cpp` / `json.h` | JSON 解析与序列化基础 |
| `translations.*` | 本地化 / 翻译支持 |

> 💡 提示：`src/` 采用「实体 + 工厂 + 数据加载」的经典模式，绝大多数游戏对象都有一个对应的 `*_factory` 或 `generic_factory` 从 JSON 构造。

---

## 五、构建方式速查

| 目标 | 命令 / 选项 |
| --- | --- |
| 文本版（ncurses） | `make` 或 CMake `-DTILES=OFF` |
| 图形版（SDL3 tiles） | `make TILES=1` 或 CMake `-DTILES=ON` |
| 音效支持 | `make TILES=1 SOUND=1` |
| Release 优化构建 | `make RELEASE=1` |
| ccache 加速 | `make CCACHE=1` |
| 编译多语言 | `make LANGUAGES="zh_CN" localization` |
| 运行测试 | `make RUNTESTS=1` 或 CMake `-DTESTS=ON` |
| 代码风格检查 | `make astyle-check` |
| JSON 格式检查 / 美化 | `make style-json` / `make style-all-json` |
| Windows + MSYS2 | 见 `doc/c++/COMPILING-MSYS.md` |
| Windows + VS + vcpkg | 见 `doc/c++/COMPILING-VS-VCPKG.md` |
| CMake 编译 | 见 `doc/c++/COMPILING-CMAKE.md` |
| 通用编译说明 | 见 `doc/c++/COMPILING.md` |

> ⚠️ CMake 注意事项：SDL2 已被移除，`TILES=ON` 时只能使用 SDL3；`USE_SDL3=OFF` 会直接构建报错。

---

## 六、JSON 数据驱动说明

- 游戏绝大多数内容定义在 `data/json/` 下的 JSON 文件中。
- 加载方式为**广度优先遍历**：`data/json/foo.json` **总是**先于 `data/json/subdir/foo.json` 被读取。
- 因此依赖顺序很重要（例如配方依赖技能，技能必须先加载），详见 `data/json/LOADING_ORDER.md`。
- **同深度**的文件按字典序（lexical order）加载。
- 各类型数据的字段规范见 `doc/JSON/`，例如：
  - `ITEM.md` — 物品
  - `MONSTERS.md` — 怪物
  - `VEHICLES_JSON.md` — 载具
  - `MAPGEN.md` — 地图生成
  - `MUTATIONS.md` — 变异
  - `EFFECTS_JSON.md` / `EFFECT_ON_CONDITION.md` — 效果与条件
  - `MAGIC.md` — 魔法（Magiclysm 等）
  - `JSON_INFO.md` — 总体 JSON 信息

---

## 七、学习 / 深入理解建议

| 目标 | 建议切入点 |
| --- | --- |
| 理解游戏逻辑 | 从 `src/` 入手：`game.cpp`、`character.cpp`、`monster.cpp`、`map.cpp` |
| 添加内容 / 制作 Mod | 从 `data/json/` 与 `data/mods/` 入手，参考 `doc/JSON/` 与 `doc/MODDING.md` |
| 参与代码开发 | 阅读 `CONTRIBUTING.md`、`doc/c++/CODE_STYLE.md` |
| 搭建编译环境 | 阅读 `doc/c++/COMPILING*.md` |
| 编写单元测试 | 阅读 `doc/c++/TESTING.md`，参考 `tests/` |
| 本地化 / 翻译 | 阅读 `doc/TRANSLATING.md` |

---

## 八、贡献须知（摘要）

- **所有 PR 必须包含 `#### Summary` 段落**，格式为 `Category "描述"`，类别可选：Features、Content、Interface、Mods、Balance、Bugfixes、Performance、Infrastructure、Build、I18N。
- 代码风格由 `astyle` 统一强制，详见 `doc/c++/CODE_STYLE.md`。
- **禁止提交由 LLM 生成的内容**（代码、配置、Issue/PR 文本等），详见 `CONTRIBUTING.md`。
- 所有贡献遵循 CC BY-SA 3.0 许可证，且不可撤销。
- 采用 Doxygen 注释规范编写代码文档；本地生成文档：`doxygen doxygen_doc/doxygen_conf.txt`。

---

_本文档为项目结构导航索引，如需了解某一子系统的细节，请查阅对应的 `doc/` 文档或源码文件。_
