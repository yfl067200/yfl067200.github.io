官方的 `cargo` 文件為 [the cargo book](https://doc.rust-lang.org/stable/cargo/index.html) 

`cargo` 是 `rust` 的套件管理工具。`cargo` 會依照專案的 dependencies 下載所需的套件，編譯專案，甚至散佈專案。`rust` 有個社群的 repository，名為 `crates.io`；這個 repository 類似 `docker` 的 `docker hub`。

# crates.io

如同前述，crates.io 是 rust 官方的 repository。cargo 預設搜尋與下載 packages 的位置，都是在 crates.io。藉由修改 Cargo.toml 中的 `[Dependencies]` 下的資訊，使用 cargo 編譯程式時，cargo 就會從 crates.io 中下載對應的 packages。

以下為設定 packages 的模式，
```toml
[dependencies]
time = "0.1.12"
```

