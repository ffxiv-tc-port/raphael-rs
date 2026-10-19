<!-- ffxiv-tc-port 繁體中文說明開始 -->
# raphael-rs(台服 fork)

由 KonaeAkira 開發的 Rust 製作路線求解器，`Artisan` 用它的 `raphael-cli` 算出符合屬性的最佳
製作巨集。上游本身已支援台服（TW）遊戲資料。

## 台服 fork 的目的

跟隨艦隊釘 `pin-artisan` 分支，作為 `Artisan` 重建 `raphael-cli` 的來源；pin 已對齊到含台服遊戲資料的
上游版本。

## 與上游的差異

- **`ids` 輸出欄位**：`raphael-cli` 的 `solve.rs` 補上 `"ids"` 輸出鍵（對應既有的 `action_ids`），
  供 `Artisan` 相容舊欄位名。
- **移除上游的 `.github` 設定**：CI workflow（`check-commit.yml`、`release.yaml`）與贊助設定；
  我們不透過它們建置或發版。

其餘結構與上游一致。

## 誰在用它

目前只有 **`Artisan`** 一個插件 repo 以子模組使用。

---

以下為上游原始 README，內容未經修改：

<!-- ffxiv-tc-port 繁體中文說明結束 -->

# Raphael XIV

[<img src="https://img.shields.io/discord/1244140502643904522?label=Discord&logo=discord&logoColor=white"/>](https://discord.com/invite/m2aCy3y8he)
[<img src="https://img.shields.io/github/downloads/KonaeAkira/raphael-rs/total?label=Downloads&color=%23dedede"/>](https://github.com/KonaeAkira/raphael-rs/releases)

:link: [www.raphael-xiv.com](https://www.raphael-xiv.com/) (stable release)

:link: [preview.raphael-rs.pages.dev](https://preview.raphael-rs.pages.dev/) (dev preview)

Raphael is a crafting rotation solver for the online game Final Fantasy XIV.
It produces crafting macros that are tailored to your stats.

## Contents <!-- omit in toc -->

- [Optimal macro selection](#optimal-macro-selection)
- [How does it work?](#how-does-it-work)
- [Building from source](#building-from-source)
  - [Native GUI](#native-gui)
  - [Native CLI](#native-cli)
- [Contributing](#contributing)

## Optimal macro selection

The following is the specification of how the optimal macro is selected:

- The generated macro must be able to finish the synthesis, i.e. reach 100% progress.
- Valid macros are then ranked based on these criteria, in order:
  - Quality reached, capped at the target quality defined in the solver configuration. (Higher is better)
  - Number of macro steps. (Lower is better)
  - Total macro duration, in seconds. (Lower is better)

Anything not mentioned in the above specification is not guaranteed to be taken into account.
If you would like to change/amend the specification, please submit a feature request.

If you find a macro that beats the generated macro according to the specification above, please submit a bug report.

## How does it work?

- Short answer: Branch-and-bound, best-first-search, dynamic programming, Pareto optimization.
- Long answer: [Algorithm Overview](https://github.com/KonaeAkira/raphael-rs/wiki/Algorithm-Overview)

## Building from source

The [Rust](https://www.rust-lang.org/) toolchain is required to build the solver.
The current minimal supported Rust version (MSRV) is 1.92.0.

### Native GUI

To build and run the application:

```
cargo run --release
```

### Native CLI

To build and run the command-line interface (CLI):

```
cargo run --release --package raphael-cli -- <cli-args>
```

The CLI currently supports searching for items and solving for crafting rotations. Run the following to see the relevant help messages:

```
cargo run --release --package raphael-cli -- --help
cargo run --release --package raphael-cli -- search --help
cargo run --release --package raphael-cli -- solve --help
```

Some basic examples:

```
cargo run --release --package raphael-cli -- search --pattern "Fiberboard"
cargo run --release --package raphael-cli -- solve --recipe-id 36183 --stats 5400 4900 600
```

The CLI can also be installed so that it can be called from anywhere:

```
cargo install --path raphael-cli
```

## Contributing

First of all, thank you for your interest in contributing to the project!

If you are looking for things to help out on, the [Open Issues](https://github.com/KonaeAkira/raphael-rs/issues) are a good place to start.

If you already have something in mind, feel free to open a pull request.
Although ideally, you would discuss your idea on [Discord](https://discord.com/invite/m2aCy3y8he) beforehand to make sure it fits the general direction of the project and that no one else is already working on it.

Before submitting a pull request, make sure all tests are ok by running:

```
cargo test --workspace
```

> [!IMPORTANT]
> Pull requests should be opened against the `preview` branch. The `main` branch is for releasing.
