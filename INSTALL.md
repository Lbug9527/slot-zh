# 安装步骤（Anbernic H700 系列）

## 0. 你需要

- 一张 16 GB 以上 microSD（TF1）
- 对应机型的 BaseOS 镜像：<https://github.com/pvaibhav/BaseOS/releases>
- 本发布包解压后的 `System` 文件夹

## 1. 先刷 BaseOS（只需一次）

1. 用 Rufus / balenaEtcher 把 `baseos-<机型>-x.y.z.img` 写入 TF1。
2. 卡插回机器，**开机一次**，等待 BaseOS 完成首次扩容（会自动格式化数据分区，约 10–60 秒）。
3. 关机，取出卡插回电脑。

> 扩容只会做一次。扩容时数据分区会被清空，所以不要先把前端拷进去。

## 2. 放入前端

把本发布包里的 `System` 文件夹**合并**到卡根目录：

```
TF1:/
├─ System/
│  ├─ slot                     ← aarch64 前端
│  ├─ launch_frontend.sh
│  ├─ mgba_libretro.so
│  ├─ gpsp_libretro.so
│  ├─ bezels/
│  └─ ...
├─ Games/
│  ├─ GBA/*.gba
│  ├─ GB/*.gb
│  └─ GBC/*.gbc
├─ Labels/GBA/*.png            ← 可选，卡带贴纸
├─ BIOS/gba_bios.bin           ← 可选
└─ Cheats/GBA/{mgba,gpsp}/*.cht ← 可选
```

要点：

- 必须是 `System/` 直接在卡根目录，不能套一层 `slot-xxx/`。
- 前端二进制设备通用（同一颗 H700），差异在 BaseOS 镜像。
- 字体不随发布包提供；需要非像素字体时，自行把 `.ttf` 放进 `System/Fonts/`，在「通用 → 字体」里选择。

## 3. 首次开机

- 机器开机后 BaseOS 会启动 `System/slot`。
- 若显示「ADD FRONTEND TO SD CARD」，说明 `System/slot` 不在预期位置。
- 若卡在黑屏：先确认 TF2 没有插卡（BaseOS 会优先用 TF2）。

## 4. 文件放哪

| 内容 | 位置 |
|---|---|
| ROM | `Games/GBA`、`Games/GB`、`Games/GBC` |
| 卡带贴纸 | `Labels/GBA/<与 ROM 同名>.png` |
| 金手指 | `Cheats/GBA/mgba/<与 ROM 同名>.cht` |
| 存档 | `Saves/GBA/*.sav`（自动） |
| 即时存档 | `States/GBA/*.state`（自动） |
| 日志 | `Logs/slot.log`（出问题时把这个发我） |

## 5. 回滚

- 本发布包不覆盖你的 BaseOS；要回官方英文 Slot，把 `System/slot` 换成官方 v1.7.0 的 `System/slot` 即可。
- 要彻底还原，重新刷一次 BaseOS 镜像。

## 6. 授权

- 前端代码：GPL-3.0-or-later（见 `LICENSE`），源码与补丁随发布包提供。
- libretro 核心：各自许可证，见 `System/licenses/`。
- 不含 ROM、BIOS、商业字体、任天堂/Anbernic 素材。
