# Neon Grid · Neon Plasma

`v1.0.1` · Home Assistant 2026.11.x

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
