# 构建说明（aarch64 / H700）

上游用 GitHub Actions 的 `ubuntu-24.04-arm` + `rust:1-bullseye` 容器构建，产物要求 glibc ≤ 2.31。本分支沿用同样的约束。

## 产物要求

- 目标三元组：`aarch64-unknown-linux-gnu`
- glibc 符号版本 ≤ 2.31（Debian bullseye）
- 动态依赖只需：`libm.so.6 libpthread.so.0 libc.so.6 libdl.so.2`
- 不需要 rpath；EGL / GLES / ALSA / udev 等在运行时由驱动与 `slot-gfx` 动态加载

## 方式 A：Linux / macOS（推荐 CI 使用）

```bash
rustup target add aarch64-unknown-linux-gnu
cargo install cargo-zigbuild
cargo zigbuild --profile device -p slot \
  --no-default-features --features device \
  --target aarch64-unknown-linux-gnu.2.31
```

产物：`target/aarch64-unknown-linux-gnu/device/slot`

> `device` feature 会关闭 winit / cpal / gilrs（host 专用），因此不需要 X11、ALSA、udev 头文件。

## 方式 B：Windows（无需 MSVC / Docker）

需要：

1. Rust 1.96，host 三元组 `x86_64-pc-windows-gnu`（必须带 mingw 自包含链接器）
2. `aarch64-unknown-linux-gnu` 标准库
3. Zig 0.13（提供 glibc 2.31 的 aarch64 目标）
4. 一个把 `zig cc` 包装成链接器的 shim（示例见 `scripts/zigcc.rs`）

`cargo` 调用链接器时只接受可执行文件，`.cmd` / `.bat` 不会被 CreateProcess 执行，所以需要一个小的 `.exe` shim：

```rust
// zigcc.rs → zigcc.exe
// 作用：exec "zig.exe cc -target aarch64-linux-gnu.2.31 <rustc 传来的参数>"
```

然后：

```powershell
$env:CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_LINKER = "G:\path\to\zigcc.exe"
cargo build --profile device -p slot --no-default-features --features device `
  --target aarch64-unknown-linux-gnu --target-dir target-device
```

Windows 下注意：

- 工程路径必须是纯 ASCII（GNU ld 处理不了中文路径）
- 运行中的 `slot.exe` 会锁住文件，构建前先结束进程
- `libloading` 使用 raw-dylib，需要 `as.exe`；上游依赖在 Windows 交叉编译时需要把 `windows_link::link!` 换成 `#[link(name="kernel32")]`（本仓库 `vendor/libloading` 已处理，并在 `Cargo.toml` 用 `[patch.crates-io]` 指向）

## 验证

```bash
file target-device/aarch64-unknown-linux-gnu/device/slot
readelf -d target-device/aarch64-unknown-linux-gnu/device/slot | grep NEEDED
strings target-device/aarch64-unknown-linux-gnu/device/slot | grep -m1 设置
```

应为 `ELF 64-bit LSB …, ARM aarch64`，NEEDED 只包含上文四个库，且能检索到中文字符串。

## 打包

```powershell
scripts/package-release.ps1 -Version 1.7.0-zh -Dist .\dist -Out .\out
```

产出 `slot-<version>-h700.zip`，内含 `System/` 与安装说明，不含 ROM、存档、商业字体。

---

维护者：Lbug9527 · https://github.com/Lbug9527
