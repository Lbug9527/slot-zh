# 字体说明（重要：发布前必读）

## 字体来源与授权

- **上游字体**：`crates/slot-ui/assets/label.ttf` 为上游自带的 Open Sans Bold（OFL）。
- **本分支当前内置字体**：为显示中文，开发时替换成了 1.6.0 汉化版使用的像素中文字体（内部名 `HYPixel 11px U`）。该字体的再分发授权**不明确**。
- **Windows 系统字体**（`simhei.ttf`、`simkai.ttf`、`Deng.ttf`）**绝对不能**打包发布，只能作为本机测试。

## 发布前的处理（二选一）

1. **换成明确开源的中文字体**：例如 Fusion Pixel（OFL-1.1）、Noto Sans SC（OFL-1.1）。替换 `crates/slot-ui/assets/label.ttf` 后重新构建，并在 `licenses/` 放入对应 OFL 文本。
2. **沿用上游 Open Sans，不自带中文**：把默认字体设回上游字体，中文字体由用户自行放入 `System/Fonts/`；此时界面中文在未放字体时可能显示为空白或方框。

推荐方案 1。

## 用户自定义字体

- 位置：`System/Fonts/*.ttf` 或 `*.otf`
- 菜单：「通用 → 字体」，循环切换，「像素（内置）」为默认
- 选择会写入 `Config/font.txt`，**重启后生效**（字库在启动时加载一次）
- 字体越大启动越慢：约 10 MB 的字体在 H700 上增加约 1–2 秒启动时间

## 建议字号特性

Slot 的文本层会在给定像素高度内自动缩字，因此：

- 优先选 **像素字体**（11–12 px 设计），在 720×480 逻辑画布上最清晰
- 普通矢量字体可用，但小字号会发虚
- 避免字重过粗的字体，菜单只有 13–18 px

## 授权文件放置

发布包内建议：

```
licenses/
├─ slot-GPL-3.0.txt
├─ font-OFL.txt            # 所选中文字体的许可证
└─ upstream-assets.txt     # 美术/图标来源说明
```

---

维护者：Lbug9527 · https://github.com/Lbug9527
