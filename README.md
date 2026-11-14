# Neon Grid · Neon Plasma

`v1.0.8` · Home Assistant 2026.11.x

Home Assistant 赛博朋克主题合集。**零依赖、纯 YAML 变量**，不改 DOM、不注入 JS、不装额外集成 —— 卸载即恢复默认。

两套配色共用同一骨架（同一中性阶逻辑、圆角阶梯、字体与光效规则），只换主色相，因此可以在资料页里即时切换对比。

| 主题名 | 主色 | 强调 | 底色 | 性格 |
|---|---|---|---|---|
| **Neon Grid**（夜城青） | `#2FE3D2` 电光青 | `#FF4D9D` 品红 | `#06070C` 蓝黑 | 驾驶舱、精准 |
| **Neon Plasma**（等离子紫） | `#A99BFF` 紫罗兰 | `#2FE3D2` 冰青 | `#0A070F` 紫黑 | 夜店、氛围 |

两套都支持 **暗色（主形态）+ 亮色（白昼 HUD）** 双模式。

## 预览

桌面 · Neon Grid

![Neon Grid 桌面](https://aka.doubaocdn.com/s/o44mR3Fi8B)

移动端 · Neon Grid

![Neon Grid 移动端](https://aka.doubaocdn.com/s/7Gu6YxQ6RJ)

亮色 · 白昼 HUD

![亮色模式](https://aka.doubaocdn.com/s/pgxZ9iGqvo)

Neon Plasma · 桌面

![Neon Plasma](https://aka.doubaocdn.com/s/47TpJC8YbZ)

## 安装

### 市场（HACS）

1. 市场 → 自定义仓库，添加本仓库地址，类型选 **主题（Theme）**
2. 搜索 `Neon Grid` 安装
3. 安装后会写入 `/config/themes/`，本仓库的单个主题文件同时含两套主题，装一次两套都在

### 手动

下载 `themes/neon-grid.yaml`，放到：

```
/config/themes/neon-grid.yaml
```

## 启用

`configuration.yaml` 确认有：

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

然后执行动作 `frontend.reload_themes`（开发者工具 → 动作），或重启 Home Assistant。

之后在 **用户资料页 → 主题** 下拉里选 `Neon Grid` 或 `Neon Plasma`。两套都带 light / dark 两段，因此还会出现「浅色 / 深色 / 跟随系统」的选择。

## 设计要点

- **不使用纯黑**：纯黑会让高饱和霓虹产生溢光，且卡片层级消失。页底 `#06070C`、卡片 `#0E1018`，卡片比页底更亮形成层级。
- **90 / 10 法则**：90% 冷调中性色，10% 霓虹只出现在状态、焦点、数据三类位置；每张卡片最多 1px 描边 + 一层低透明外发光。
- **改色阶而不是改单点**：同时重写 `ha-color-primary-05…95` 与兼容层 `primary-color`，新旧组件（含 Web Awesome 体系）的 hover / 填充 / 边框才会一起变。
- **状态色改原色板**：官方状态色默认 `var(--red-color)` 这类引用，重写 23 个原色板即全局生效，只对 9 条做定向覆盖。
- **圆角收紧**：`ha-border-radius-*` 全阶 3–19px，卡片 10px，形成切面感；只改卡片圆角会留下圆按钮。

对比度（暗色，对 `#06070C`）：正文 15 : 1 以上、副文本 7 : 1、主色 12 : 1（Plasma 8.4 : 1）、主色按钮文字 11.9 : 1。

## 自定义

改 `themes/neon-grid.yaml` 里对应主题段即可，常用的几处：

| 想改什么 | 变量 |
|---|---|
| 主色 | `ha-color-primary-05…95` + `primary-color` + `text-primary-color` |
| 页底/卡片底色 | `primary-background-color`、`card-background-color`、`secondary-background-color`、`clear-background-color` |
| 圆角 | `ha-border-radius-sm…6xl`（全阶一起改） |
| 字体 | `ha-font-family-body`、`ha-font-family-code`、`ha-font-size-scale` |
| 侧栏 | `sidebar-background-color`、`sidebar-selected-*` |
| 图表色 | `color-1…54` |

注意：

- 键名**不要**写 `--` 前缀，前端会统一加，自写会变成 `----xxx` 静默失效。
- hex 值必须加引号。
- 值为 `var()` 时不会自动派生 `rgb-*`，此时需手写对应 `rgb-` 变量。

## 更新日志

### v1.0.8 · 2026-11-15

- **为 HUD 深空底纹调优**：卡片压到 `rgba(10,14,21,.68)`（Plasma `rgba(11,9,21,.70)`），圆角 14→16px，在深黑背景上层次更清楚。
- 顶栏玻璃更透：`rgba(8,11,17,.52)`（Plasma `rgba(9,7,16,.54)`），HUD 网格能从顶栏透出来。
- 侧栏压深到 `rgba(7,10,16,.86)`，与深空背景同基调，不再比主区域亮一档。
- 配套壁纸仓库：[ha-neon-wallpapers](https://github.com/RockJesus/ha-neon-wallpapers)（hud-grid / hud-plasma）。
- 变量：两主题 203 / 137。

### v1.0.7 · 2026-11-14 · 2026-11-14

- **配色升级为石墨层叠（graphite layering）**：页底压到 `#06090F`，卡片抬到 `#0F141D`（Plasma `#110E1D`），悬浮层 `#171D27`，形成「底—面—浮」三级明度差，霓虹只在强调元素出现。
- **卡片描边中性化**：从青色 `.16` 改为冷白 `rgba(255,255,255,.07)`，配合深投影 `0 26px 54px -26px` + 环境光 `0 0 42px -18px`，卡片像浮在暗场里，而不是每个都描一圈青边。
- **顶栏分隔线**降为中性 `rgba(146,158,178,.16)`，霓虹不再与内容抢视线。
- 文字层级：主文 `#E8EDF4`、次文 `#929EB2`、禁用 `#59647A`；分割线 `rgba(146,158,178,.10)`。
- 亮色描边统一为 `rgba(15,23,42,.10)`，更接近印刷级灰阶。
- 变量：两主题均暗色 203 / 亮色 137。

### v1.0.6 · 2026-11-14 · 2026-11-14

- **Neon Plasma 与 Neon Grid 完全对齐**：顶栏 `rgba(10,7,20,.62)` + `blur(20px) saturate(150%)`、侧栏 `rgba(10,7,20,.78)`、卡片 `rgba(11,8,22,.64)`，与 Grid 同一套玻璃结构与光影算法。
- Plasma 描边加到 `rgba(169,155,255,.22)`、外发光 `0 0 34px -10px rgba(169,155,255,.30)`，紫调不再发灰。
- 变量：两主题均暗色 203 / 亮色 137。

### v1.0.5 · 2026-11-14 · 2026-11-14

- **霓虹描边 + 外发光**：卡片描边改为 `rgba(47,227,210,.16)`（Plasma 为紫 `rgba(169,155,255,.18)`），投影叠加 `0 0 30px -10px` 同色光晕，形成 HUD 式边缘辉光。
- 页底更深（`#04060B`），卡片底色降到 `rgba(11,14,22,.56)`，光晕更通透。
- 顶栏分隔线亮度提到 `.30`，与参考图的青色 HUD 分界线一致；侧栏选中项加 `rgba(…,.08)` 底光。
- 亮色保持克制：描边/投影用青绿淡光，不刺眼。
- 变量：暗色 203 条、亮色 137 条。

### v1.0.4 · 2026-11-14 · 2026-11-14

- **顶栏与侧栏统一玻璃质感**：`--app-header-background-color: rgba(14,16,24,.58)` + `--app-header-backdrop-filter: blur(20px) saturate(150%)`（原生支持，实测 computed 生效）；`--sidebar-background-color: rgba(14,16,24,.72)`，与卡片同一玻璃基色，不再是纯黑板块。
- Plasma 用 `rgba(19,16,32,…)` 同色系；亮色顶栏 `rgba(255,255,255,.66)` + `blur(20px) saturate(160%)`、侧栏 `rgba(255,255,255,.78)`。
- 侧栏无原生 `backdrop-filter` 变量（HA 不支持），故走「同色半透明 + 冷色描边」实现；有壁纸的顶栏模糊最明显。
- 变量：暗色 202 条、亮色 137 条。

### v1.0.3 · 2026-11-14 · 2026-11-14

- **透明液体玻璃卡片**：暗色 `ha-card-background: rgba(14,16,24,.52)` / `rgba(19,16,32,.52)`，配 `ha-card-backdrop-filter: blur(22px) saturate(150%)`、1px 冷白描边与内高光；亮色 `rgba(255,255,255,.62)` + `blur(20px) saturate(160%)`。
- 卡片圆角 10px → 14px，贴合玻璃质感。
- 全部用 HA 原生的 `--ha-card-*` 变量实现，**不需要 card-mod**；有壁纸背景时玻璃层次最明显。
- 变量：暗色 201 条、亮色 136 条。

### v1.0.2 · 2026-11-14

- **修复自定义卡片里的浅灰块**：`custom:service-call` 等卡片开关的滑块底色取自 `--state-inactive-color`，此前主题未定义该变量，HA 按副文本色派生成浅灰，在深色主题上像亮补丁。现在暗色钉为 `#4A5266`（Grid）/ `#4B4363`（Plasma），亮色为 `#93A0B4` / `#9C93B0`。
- 变量：暗色 199 条、亮色 135 条。

### v1.0.1 · 2026-11-14

- **亮色加强**：Grid 主色压深到 `#00685C`、Plasma 到 `#452380`，分割线与卡片描边改为冷色低透明（`rgba(0,104,92,.10)` / `rgba(74,42,140,.10)`），卡片加 1px 冷色描边，暗底页面再偏冷一档，不再"像默认 HA 换了强调色"。
- **图表配色对齐能源面板**：`color-1`–`color-4` 改为「消耗蓝 / 回馈品红 / 光伏琥珀 / 电池青」语义序，暗色柱状图不再偏青灰。
- 变量覆盖：暗色 198 条、亮色 134 条（亮色新增 `ha-card-border-color`）。

### v1.0.0 · 2026-11-13

- 首发：Neon Grid（夜城青）+ Neon Plasma（等离子紫），双主题单文件。

## 兼容版本

适配 Home Assistant **2026.11.x**。主题基于当前 `--ha-color-*` 令牌层；旧版本 HA 会退化到兼容层变量，仍可用但新组件细节不会完全一致。

HA 官方仅承诺 primary / accent / state 色稳定，其余变量属"尽力而为"，大版本升级后如发现某屏颜色异常，属预期内的接口变动，欢迎提 issue。

## 已知限制

- 纯 YAML 主题无法实现 `backdrop-filter` 真玻璃、扫描线底纹、`@font-face` 自定义字体（主题只解析变量声明，不解析选择器）。这类效果需要额外的 CSS 注入模块，本仓库暂不提供。
- 自绘 Shadow DOM 的第三方卡片可能不跟随主题，需该卡片自身的主题变量。实测 `custom:service-call` 的开关滑块底色由卡片内部按 `--feature-color` 计算，主题变量覆盖不到，需 card-mod 单独处理。

## License

MIT
