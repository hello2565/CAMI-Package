# Mods 目录说明

SSMT4 / 3DMigoto 会通过 `d3dx.ini` 的 `[Include] include_recursive = Mods`
自动递归加载本目录下的所有 `.ini` 文件（含子目录）。

## mod 的组织方式

每个 mod 一个文件夹（或直接放 ini 文件），例如：

```
Mods/
└── SSMTGeneratedMod/
    └── Default/
        ├── Default.ini      # [TextureOverride] 按IB hash替换/跳过
        └── VSCheck.ini      # 触发链（见下）
```

## 重要：IB hash 触发链（VSCheck）

**按索引缓冲（IB）hash 编写的 `[TextureOverride]` 节不会在绘制时自动执行。**
它必须由一个已经触发的命令列表运行 `checktextureoverride = ib` 才会执行。

因此 SSMT4 的 "VSCheck" 功能会生成 `VSCheck.ini`，内容形如：

```ini
[ShaderOverride_<顶点着色器hash>]
allow_duplicate_hash = overrule
hash = <顶点着色器hash>
if $costume_mods
  checktextureoverride = ib
endif
```

原理：`[ShaderOverride]`（按着色器 hash）在绘制时**自动**触发 →
其内部的 `checktextureoverride = ib` 去匹配当前绑定的 IB hash →
从而执行对应的 `[TextureOverride]`（IB hash + match_first_index）节。

手写 mod 时如果只写了 `[TextureOverride_xxx] hash = <IB hash>` 而没有任何
`[ShaderOverride]` 去启动它，mod 会完全无效（也不会报错）。

注意：`$costume_mods` 变量由 SSMT4 管理的 d3dx.ini `[Constants]` 定义；
如果使用本包的 d3dx.ini 手动游玩，请去掉 `if $costume_mods` / `endif`
两行，直接保留 `checktextureoverride = ib`。

## 获取 VS hash

用 Frame Analysis（游戏内 F8，本包已启用）转储一次目标场景，
转储文件名形如 `000005-ib=f4dca227-vs=7cef32e3b398e967.buf`，
其中 `vs=` 后面就是需要写进 `[ShaderOverride]` 的 hash。
一个 IB 可能被多个 VS 使用，每个 VS 都要有对应的触发节。
