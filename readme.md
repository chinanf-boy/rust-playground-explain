# integer32llc/rust-playground [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/rust-playground-zh
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 rust 游乐场 」

---

## explain 🀄️

<!-- doc-templite START generated -->
<!-- time = '2018-09-23' -->
<!-- name = 'integer32llc' -r->
<!-- repo = 'rust-playground' -->
<!-- commit = 'c54773e3b2112bd9d92297fa5af73ee80f58618b' -->

| 版本     | 与日期        | 最新更新   | 更多               |
| -------- | ------------- | ---------- | ------------------ |
| [commit] | ⏰ 2018-09-23 | ![version] | [源码解释][source] |

[commit]: https://github.com/integer32llc/rust-playground/tree/c54773e3b2112bd9d92297fa5af73ee80f58618b
[version]: https://img.shields.io/npm/v/rust-playground.svg

<!-- doc-templite END generated -->

### 中文

https://github.com/chinanf-boy/rust-playground-zh

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

> 先看看 [rust-playground > readme.md][translate-list]

## 根据源库

有三种类别文件:

1. 前端-React: `rust-playground/ui/frontend/
2. 后端-rust: `rust-playground/ui/Cargo.toml`
3. Docker: `rust-playground/compiler/`

## 后端为主要讲解

> root:`rust-playground/ui/`

### Cargo.toml

一个 rust-Cargo 库的根本, 如同 package.json

> rust - javascript
> Cargo - npm 
> Cargo.toml - package.json

``` toml
[package]
authors = ["Jake Goulding <jake.goulding@integer32.com>"]
name = "ui"
version = "0.1.0"

[features]
default = ['fork-bomb-prevention'] # 这个features, 被使用过滤定制功能
fork-bomb-prevention = []

[dependencies]
bodyparser = "0.8.0"
corsware = "0.2.0"
env_logger = "0.5.0"
iron = "0.6.0"
lazy_static = "1.0.0"
log = "0.4.0"
mktemp = "0.3.1"
mount = "0.4.0"
petgraph = "0.4.13"
quick-error = "1.1.0"
regex = "1.0.0"
rustc-demangle = "0.1.5"
serde = "1.0"
serde_derive = "1.0"
serde_json = "1.0"
url = "1.4.0"
hubcaps = "0.4.8"
tokio-core = "0.1.12"
hyper = "0.11.21"
hyper-tls = "0.1.2"
router = "0.6.0"
openssl-probe = "0.1.2"
dotenv = "0.13.0"

[dependencies.playground-middleware]
git = "https://github.com/integer32llc/playground-middleware"

```

### src/main.rs

- [ ] [src/main.rs](./src/main.md)