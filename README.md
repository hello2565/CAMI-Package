# CAMI-Package — 卡拉彼丘 (Calabiyau / Strinova) 3DMigoto 包

SSMT4 的 **CAMI** 预设配套包：提供 `d3dx.ini`、`Mods/`、`ShaderFixes/`
与卸载脚本。SSMT4 会在安装/更新包时把本仓库内容解压到 3DMigoto 目录。

## 运行时 d3d11.dll 的来源（重要）

本包基于 **SpectrumQT/XXMI-Libs-Package** 基底构建（SSMT mod 的目标运行时），
并额外保留了 **late-attach（后注入）** 支持——卡拉彼丘通过 WeGame 启动时
先创建 D3D11 设备，普通 XXMI 运行时无法注入，late-attach 构建才能接管
已有设备。

d3d11.dll 来自 [hello2565/3Dmigoto](https://github.com/hello2565/3Dmigoto)
的 release（资产 `XXMI-PACKAGE-v*.zip`），SSMT4 的 CAMI 预设会自动下载并
安装。本包不附带任何 dll。

## 手动安装（不用 SSMT4 时）

1. 从 hello2565/3Dmigoto 的 release 下载 `XXMI-PACKAGE-v*.zip`，
   取其中的 d3d11.dll（x64）。
2. 将本仓库内容解压到游戏可执行文件同目录（或 SSMT4 管理的
   3DMigoto 目录），把 d3d11.dll 放进去。
3. 确认 `d3dx.ini` 中 `[Include] include_recursive = Mods` 处于启用状态
   （本包默认已启用）。

## d3dx.ini 与 SSMT4 的关系

SSMT4 每次启动会改写以下键，无需手动维护：

- `[Loader] module / target / launch / delay`
- `[System] dll_initialization_delay`
- `[Hunting] hunting / marking_actions / analyse_options`
- `[Logging] show_warnings`

本包基底（XXMI 的 d3dx.ini）默认已启用：

- `[Include] include_recursive = Mods` —— 自动加载 Mods 目录
- `[Hunting] analyse_frame = no_modifiers VK_F8` —— F8 帧分析快捷键

## 升级游戏版本后 mod 失效？

着色器/缓冲 hash 会随游戏更新变化。用 F8 重新做 Frame Analysis，
更新 mod 中的 hash（IB / VS）与 `Mods/*/VSCheck.ini` 的 VS hash。
