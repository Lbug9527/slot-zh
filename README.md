# Slot 简体中文增强版（for Anbernic H700 系列）

基于 [BrandonKowalski/slot](https://github.com/BrandonKowalski/slot) **v1.7.0** 的简体中文汉化与增强分支，遵循上游 **GPL-3.0-or-later** 许可。

上游 Slot 是面向 Anbernic H700 掌机的 Game Boy / GBC / GBA 前端；本分支把界面汉化，并在不改变存档/ROM 格式的前提下增加了一批功能。

> 本项目与 Anbernic、Nintendo 无隶属关系。ROM、BIOS、商业字体均不随本项目分发。

**作者：Lbug9527** · https://github.com/Lbug9527

---

## 功能

### 汉化

- 菜单、提示、弹窗、时间（`10月21日 21:57`、`刚刚 / N 分钟前`）全中文
- 金手指面板、Wi-Fi 设置、快捷键一览、关于页均为中文

### 新增功能

| 功能 | 说明 |
|---|---|
| 卡带背景 | 选中卡带时用其标签图模糊铺底，切换时淡入淡出；无标签的卡带回落到卡带正面 |
| 金手指 | 游戏内菜单打开，支持本地 `.cht`（`Cheats/GBA/{mgba,gpsp}/*.cht`），左右翻页，改动写回文件 |
| 在线金手指 | 按 ROM CRC 匹配 libretro-database 并下载 `.cht`；失败时显示原因 |
| 在线贴纸 | 按 ROM CRC 匹配 libretro-thumbnails 下载封面 |
| 网络设置 | 扫描 Wi-Fi、屏上键盘（大小写/符号/显示密码）、保存多个 SSID 密码、开机自动连接 |
| 字体选择 | 读取 `System/Fonts/*.ttf|*.otf`，可在「通用 → 字体」切换，重启生效 |
| 拼音首字母查找 | 按 ROM 名拼音首字母跳转（X 键打开字母表） |
| 快捷键一览 | 主菜单里一页看全所有按键 |
| 休眠自动关机 | 3 / 10 / 30 分钟 / 永不；关机前保存一张游戏截图到 `Screenshots/` |
| 低电量提醒 | 阈值 5%–30% 可调，游戏中右上角电池图标闪烁 |
| HUD | 时间/电量字号放大，时间左侧增加 Wi-Fi 信号图标 |
| 手柄 | 桌面开发窗口支持 gilrs 手柄；掌机走上游输入栈 |

### 与上游一致的部分

- ROM 需解压，仅支持 GBA / GB / GBC
- 卡带目录 `Games/GBA`、`Games/GB`、`Games/GBC`
- 存档 `Saves/`、即时存档 `States/`、标签 `Labels/`
- 前端启动逻辑、双核（mGBA / gpSP）、联机、倒带、着色器等均沿用上游

---

## 支持机型

与上游 v1.7.0 相同：

| 机型 | 支持 | 说明 |
|---|---|---|
| Anbernic RG SP | ✅ | 上游首发机型 |
| Anbernic RG34XX | ✅ | 本分支主要实测机型 |
| Anbernic RG34XXSP | ✅ | 暂不支持摇杆（上游限制） |
| Anbernic RG35XXSP | ✅ | 4:3 屏，游戏画面按上游逻辑加边框 |
| Anbernic 35XX Plus | ✅ | |
| Anbernic RG28XX | ✅ | 竖装屏，由上游自动旋转画面 |

所有 H700 机型共用同一颗 SoC 与 ABI，**发布包是同一个 aarch64 二进制**；屏幕方向、宽高比、按键差异由上游的设备探测处理。

机型对应的 BaseOS 卡镜像请到 [pvaibhav/BaseOS releases](https://github.com/pvaibhav/BaseOS/releases) 下载（文件名形如 `baseos-rg34xx-1.4.0.img.zip`、`baseos-rg35xxsp-…`、`baseos-rg28xx-…`）。

---

## 安装

1. 用 BaseOS 对应机型的镜像刷到 TF1（见 [docs/DEVICES.md](docs/DEVICES.md)）。
2. 首次开机让 BaseOS 扩容（会格式化数据分区）。
3. 关机拔卡，把本发布包里的 `System/`、`Cheats/`、`Labels/`（如有）复制到卡根目录。
4. 可选：把中文字体放进 `System/Fonts/`（见 [docs/FONTS.md](docs/FONTS.md)）。
5. 插卡开机。

目录结构：

```
TF1:/
├─ System/
│  ├─ slot                 # 本前端（aarch64）
│  ├─ launch_frontend.sh
│  ├─ coresave.sh
│  ├─ led.sh / slotlink.sh
│  ├─ mgba_libretro.so / gpsp_libretro.so
│  ├─ bootlogo.bmp
│  ├─ bezels/
│  └─ Fonts/               # 可选，自备字体
├─ Games/{GBA,GB,GBC}/
├─ Saves/  States/  Labels/  Cheats/  Config/  Logs/
```

---

## 已知限制

- **在线金手指/贴纸依赖 libretro 数据库**：只认 No-Intro 名称 + ROM CRC。实测 534 个中文/改版 ROM 中约 74 个能匹配；匹配不到时会提示「本作在线暂无金手指」，这是数据源覆盖问题，不是网络故障。
- **在线贴纸**对中文改名的 ROM 基本无法匹配，建议用离线封面（`Labels/GBA/<与 ROM 同名>.png`）。
- **在线功能需要 Wi-Fi**（H700 内置 RTL8821CS，BaseOS 负责加载驱动，前端负责连接与 DHCP）。
- **字体**：发布包不自带商业字体；默认使用内置像素字体，中文可读但字形较硬，建议放一份开源中文字体。
- **RG28XX 旋转、RG35XXSP 边框、RG34XX 专属亮度/菜单组合键** 等设备差异由上游处理，本分支未改动。

---

## 构建

见 [docs/BUILD.md](docs/BUILD.md)。改动清单见 [patches/](patches/) 与仓库提交记录。

## 许可

- 本分支：**GPL-3.0-or-later**，与上游一致，见 [LICENSE](LICENSE)。
- 上游 Slot © BrandonKowalski 及其贡献者。
- 内置像素字体、图标、美术资源版权归原作者；若来源不明将替换为 OFL 字体后再发布。

## 致谢

- [BrandonKowalski/slot](https://github.com/BrandonKowalski/slot)
- [pvaibhav/BaseOS](https://github.com/pvaibhav/BaseOS)
- [libretro-database](https://github.com/libretro/libretro-database) / [libretro-thumbnails](https://github.com/libretro-thumbnails)
- 1.6.0 汉化版作者（B 站 @仲冬临沧的十一月）提供的排版与词表参考
