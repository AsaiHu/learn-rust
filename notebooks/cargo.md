## cargo

- `cargo new` 创建项目
- `cargo build` 构建项目。
- `cargo run` 一步构建并运行项目。
- `cargo check` 在不生成二进制文件的情况下构建项目来检查错误。
- 有别于将构建结果放在与源码相同的目录，Cargo 会将其放到 *target/debug* 目录。
-  `cargo build --release` 会需要更长的编译时间，但代码会编译的更快，会放在 *target/release*目录
-  `cargo update` 更新为最新的可兼容版本
-  `cargo doc` 生成文档注释的 HTML 文档



 *Cargo.toml* :Cargo配置文件，在`[dependencies]`下面列出依赖，在 Rust 中，代码包被称为 *crates*

可以增加`[profile.*]`来修改默认配置



