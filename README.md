# CAMI-Package — 卡拉彼丘 (Calabiyau / Strinova) 3DMigoto 包

SSMT4 的 **CAMI** 预设配套包：提供 `d3dx.ini`、`Mods/`、`ShaderFixes/`、
`nvapi64.dll` 与卸载脚本。SSMT4 会在安装/更新包时把本仓库内容解压到
3DMigoto 目录。

## 重要：运行时 d3d11.dll 必须使用 hello2565/3Dmigoto 的构建

卡拉彼丘通过 WeGame 启动，游戏创建 D3D11 设备早于注入时机，
**普通 XXMI 运行时无法注入**。必须使用
[hello2565/3Dmigoto](https://github.com/hello2565/3Dmigoto) 的
**late-attach（后注入）构建**：游戏先创建设备/交换链，3DMigoto 在
CreateSwapChain 时接管已有设备。

SSMT4 的 CAMI 预设已将该仓库的 release（`CAMI-3DMigoto_v*.zip`，
内含 x64 `d3d11.dll`）作为运行时 dll 来源；本包不再附带 d3d11.dll。
`nvapi64.dll` 由本包附带（更新不频繁）。

## 手动安装（不用 SSMT4 时）

1. 从 hello2565/3Dmigoto 的 release 下载 `3Dmigoto_v*.7z`（完整包）
   或 `CAMI-3DMigoto_v*.zip`（仅运行时 dll），取 x64 的 dll。
2. 将本仓库内容解压到游戏可执行文件同目录（或 SSMT4 管理的
   3DMigoto 目录）。
3. 确认 `d3dx.ini` 中 `[include] include_recursive = Mods` 处于启用状态
   （本包默认已启用）。

## d3dx.ini 与 SSMT4 的关系

SSMT4 每次启动会改写以下键，无需手动维护：

- `[Loader] module / target / launch / delay`
- `[System] dll_initialization_delay`
- `[Hunting] hunting / marking_actions / analyse_options`
- `[Logging] show_warnings`

本包额外启用了：

- `[Include] include_recursive = Mods` —— 自动加载 Mods 目录
- `[Hunting] analyse_frame = no_modifiers VK_F8` —— F8 帧分析快捷键

## 升级游戏版本后 mod 失效？

着色器/缓冲 hash 会随游戏更新变化。用 F8 重新做 Frame Analysis，
更新 mod 中的 hash（IB / VS）与 `Mods/*/VSCheck.ini` 的 VS hash。
