# 机型与 BaseOS 镜像对照

所有 H700 机型共用同一份前端二进制；差异只在 BaseOS 卡镜像与设备探测（屏幕旋转、宽高比、按键）。

## 上游 Slot 支持的机型

| 机型 | BaseOS 镜像（示例） | 屏幕 | 备注 |
|---|---|---|---|
| Anbernic RG SP | `baseos-rgsp-1.4.0.img.zip` | 640×480 | 上游首发 |
| Anbernic RG34XX | `baseos-rg34xx-1.4.0.img.zip` | 720×480 | 本分支实测 |
| Anbernic RG34XXSP | `baseos-rg34xxsp-1.4.0.img.zip` | 640×480 | 暂不支持摇杆（上游限制） |
| Anbernic RG35XXSP | `baseos-rg35xxsp-1.4.0.img.zip` | 640×480 (4:3) | 游戏画面加边框 |
| Anbernic 35XX Plus | `baseos-rg35xxplus-1.4.0.img.zip` | 640×480 | |
| Anbernic RG28XX | `baseos-rg28xx-1.4.0.img.zip` | 480×640（竖装） | 上游自动旋转 |

## BaseOS 有镜像但上游 Slot 未声明支持的机型（实验性）

`rg35xxh`、`rg35xxpro`、`rg40xxh`、`rg40xxv`、`rgcubexx` 等在 BaseOS 发布页有对应镜像，但上游 README 未列为支持机型；本分支不保证可用，欢迎实测反馈。

## 三个关键设备差异（均由上游处理，本分支未改动）

1. **RG28XX 竖装屏**：上游按 BaseOS 报告的旋转信息把画面转正。
2. **RG35XXSP 4:3 屏**：游戏画面按宽度铺满，下方补 GBA SP 边框。
3. **RG34XX 专属组合键**：亮度与菜单键使用 RG34XX 自己的组合方式。

## 安装前检查

- BaseOS 首次开机必须让它完成扩容（会自动格式化数据分区）。
- 确认 `System/`、`Games/` 位于卡根目录，而不是套一层文件夹。
- 若插了 TF2 且其中有可用前端，BaseOS 会优先用 TF2；测试本前端时先拔掉 TF2。

---

维护者：Lbug9527 · https://github.com/Lbug9527
