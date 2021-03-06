### src/main.rs

cargo 库的默认主文件

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


  - [extern](#extern)
  - [use](#use)
  - [cosnt + mod](#cosnt--mod)
  - [1. main](#1-main)
    - [1.1 React](#11-react)
    - [1.2 后台 : 下面的都是后台内容](#12-%E5%90%8E%E5%8F%B0--%E4%B8%8B%E9%9D%A2%E7%9A%84%E9%83%BD%E6%98%AF%E5%90%8E%E5%8F%B0%E5%86%85%E5%AE%B9)
  - [GhToken](#ghtoken)
  - [compile](#compile)
  - [execute](#execute)
  - [format](#format)
  - [clippy](#clippy)
  - [miri](#miri)
  - [meta_crates](#meta_crates)
  - [meta_version_stable](#meta_version_stable)
  - [meta_version_beta](#meta_version_beta)
  - [meta_version_nightly](#meta_version_nightly)
  - [meta_version_rustfmt](#meta_version_rustfmt)
  - [meta_version_clippy](#meta_version_clippy)
  - [meta_version_miri](#meta_version_miri)
  - [meta_gist_create](#meta_gist_create)
  - [meta_gist_get](#meta_gist_get)
  - [evaluate](#evaluate)
  - [with_sandbox](#with_sandbox)
  - [with_sandbox_no_request](#with_sandbox_no_request)
  - [run_handler](#run_handler)
  - [deserialize_from_request](#deserialize_from_request)
  - [run_handler_no_request](#run_handler_no_request)
  - [serialize_to_response](#serialize_to_response)
  - [struct](#struct)
  - [1. SandboxCacheInfo](#1-sandboxcacheinfo)
  - [2. SandboxCacheOne](#2-sandboxcacheone)
  - [3. SandboxCache](#3-sandboxcache)
  - [4. CachedSandbox](#4-cachedsandbox)
  - [](#)
  - [quick_error!](#quick_error)
  - [type Result](#type-result)
- [struct](#struct-1)
  - [1. ErrorJson](#1-errorjson)
  - [2. CompileRequest](#2-compilerequest)
  - [3. CompileResponse](#3-compileresponse)
  - [4. ExecuteRequest](#4-executerequest)
  - [5. ExecuteResponse](#5-executeresponse)
  - [6. FormatRequest](#6-formatrequest)
  - [7. FormatResponse](#7-formatresponse)
  - [8. ClippyRequest](#8-clippyrequest)
  - [9. ClippyResponse](#9-clippyresponse)
  - [10. MiriRequest](#10-mirirequest)
  - [11. MiriResponse](#11-miriresponse)
  - [12. CrateInformation](#12-crateinformation)
  - [13. MetaCratesResponse](#13-metacratesresponse)
  - [14. MetaVersionResponse](#14-metaversionresponse)
  - [15. MetaGistCreateRequest](#15-metagistcreaterequest)
  - [16. MetaGistResponse](#16-metagistresponse)
  - [17. EvaluateRequest](#17-evaluaterequest)
  - [18. EvaluateResponse](#18-evaluateresponse)
- [impl:给结构写方法](#impl%E7%BB%99%E7%BB%93%E6%9E%84%E5%86%99%E6%96%B9%E6%B3%95)
  - [sandbox::CompileRequest](#sandboxcompilerequest)
  - [CompileResponse](#compileresponse)
  - [sandbox::ExecuteRequest](#sandboxexecuterequest)
  - [ExecuteResponse](#executeresponse)
  - [sandbox::FormatRequest](#sandboxformatrequest)
  - [FormatResponse](#formatresponse)
  - [sandbox::ClippyRequest](#sandboxclippyrequest)
  - [ClippyResponse](#clippyresponse)
  - [sandbox::MiriRequest](#sandboxmirirequest)
  - [MiriResponse](#miriresponse)
  - [MetaCratesResponse](#metacratesresponse)
  - [MetaVersionResponse](#metaversionresponse)
  - [MetaGistResponse](#metagistresponse)
  - [sandbox::ExecuteRequest](#sandboxexecuterequest-1)
  - [EvaluateResponse](#evaluateresponse)
- [parse](#parse)
  - [parse_target](#parse_target)
  - [parse_assembly_flavor](#parse_assembly_flavor)
  - [parse_demangle_assembly](#parse_demangle_assembly)
  - [parse_process_assembly](#parse_process_assembly)
  - [parse_channel](#parse_channel)
  - [parse_mode](#parse_mode)
  - [parse_edition](#parse_edition)
  - [parse_crate_type](#parse_crate_type)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### extern

```rs
#![feature(try_from)] // 需要夜晚nightly-构建版本的锈才能运动此项目，因为这个特性并不是标准

#[macro_use]
extern crate log;
extern crate env_logger;
extern crate dotenv;
extern crate iron;
extern crate mount;
extern crate router;
extern crate playground_middleware; // 来自作者的 游乐场中间件
extern crate bodyparser;
extern crate serde;
extern crate serde_json;
#[macro_use]
extern crate serde_derive;
extern crate mktemp;
#[macro_use]
extern crate quick_error;
extern crate corsware;
#[macro_use]
extern crate lazy_static;
extern crate petgraph;
extern crate regex;
extern crate rustc_demangle;
extern crate hubcaps;
extern crate tokio_core;
extern crate hyper;
extern crate hyper_tls;
extern crate openssl_probe;
```

- `#[macro_use]`- 库中宏的获取与使用

### use

为了[省略写前缀](https://rustwiki.org/zh-CN/rust-by-example/custom_types/enum/enum_use.html), 的语法糖

> 说真的, 一旦用上`::*`, 若是没有转到定义的工具, 下面用到的某些变量都不知道打哪来

```rs
use std::any::Any;
use std::convert::{TryFrom, TryInto};
use std::env;
use std::path::PathBuf;
use std::sync::{Arc, Mutex};
use std::time::{Duration, Instant};

use corsware::{CorsMiddleware, AllowedOrigins, UniCase};
use iron::headers::ContentType;
use iron::method::Method::{Get, Post};
use iron::modifiers::Header;
use iron::prelude::*; // < ===
use iron::status;
use mount::Mount;
use playground_middleware::{
    Staticfile, Cache, Prefix, ModifyWith, GuessContentType, FileLogger, StatisticLogger, Rewrite
};
use router::Router;

use serde::Serialize;
use serde::de::DeserializeOwned;

use sandbox::Sandbox;
```

### cosnt + mod

[const 常量](https://rustwiki.org/zh-CN/rust-by-example/custom_types/constants.html) 加 [mod 模块导入](https://rustwiki.org/zh-CN/rust-by-example/mod/split.html)

```rs
const DEFAULT_ADDRESS: &str = "127.0.0.1";
const DEFAULT_PORT: u16 = 5000;
const DEFAULT_LOG_FILE: &str = "access-log.csv";

mod sandbox;
mod asm_cleanup;
mod gist;

const ONE_HOUR_IN_SECONDS: u32 = 60 * 60;
const ONE_DAY_IN_SECONDS: u64 = 60 * 60 * 24;
const ONE_YEAR_IN_SECONDS: u64 = 60 * 60 * 24 * 365;

const SANDBOX_CACHE_TIME_TO_LIVE_IN_SECONDS: u64 = ONE_HOUR_IN_SECONDS as u64;
```

### 1. main

根据设置, 启动 iron 服务器, 路由对应控制函数

```rs
fn main() {
    // Dotenv可能无法加载环境变量，但在生产中可以
    let _ = dotenv::dotenv(); // 从当前目录中获取, .env文件, 格式为toml
    openssl_probe::init_ssl_cert_env_vars(); // 帮助在OpenSSL系统上查找SSL证书位置的工具
    env_logger::init(); // 添加进 环境变量

    let root: PathBuf = env::var_os("PLAYGROUND_UI_ROOT").expect("必须 指定 PLAYGROUND_UI_ROOT").into(); // into-类型转换
    // 这个目录是 前端文件的目录路径
    let gh_token = env::var("PLAYGROUND_GITHUB_TOKEN").expect("必须 指定 PLAYGROUND_GITHUB_TOKEN");

    let address = env::var("PLAYGROUND_UI_ADDRESS").unwrap_or_else(|_| DEFAULT_ADDRESS.to_string());
    let port = env::var("PLAYGROUND_UI_PORT").ok().and_then(|p| p.parse().ok()).unwrap_or(DEFAULT_PORT);
    let logfile = env::var("PLAYGROUND_LOG_FILE").unwrap_or_else(|_| DEFAULT_LOG_FILE.to_string());
    let cors_enabled = env::var_os("PLAYGROUND_CORS_ENABLED").is_some();
    // 设置获取
```

#### 1.1 React

```rs
    let files = Staticfile::new(&root).expect("不能打开 root 目录");
    let mut files = Chain::new(files); // iron框架 服务器流程
    let one_day = Duration::new(ONE_DAY_IN_SECONDS, 0);
    let one_year = Duration::new(ONE_YEAR_IN_SECONDS, 0);

    files.link_after(ModifyWith::new(Cache::new(one_day)));
    files.link_after(Prefix::new(&["assets"], Cache::new(one_year)));
    files.link_after(GuessContentType::new(ContentType::html().0));

    let mut gist_router = Router::new(); // 路由: 关于github 的 gist 权限
    gist_router.post("/", meta_gist_create, "gist_create");
    gist_router.get("/:id", meta_gist_get, "gist_get");

    // 添加路由对应的控制函数
    let mut mount = Mount::new(); // irno 中间件
    mount.mount("/", files); // 主路由是 React 项目主导的
```

- [ ] [frontend > react](./frontend/readme.md)

#### 1.2 后台 : 下面的都是后台内容

```rs
    mount.mount("/compile", compile); // arm, Show LLVM IR
    mount.mount("/execute", execute); // crago run
    mount.mount("/format", format); // rust format
    mount.mount("/clippy", clippy); // lint with clippy
    mount.mount("/miri", miri);
    mount.mount("/meta/crates", meta_crates);
    mount.mount("/meta/version/stable", meta_version_stable);
    mount.mount("/meta/version/beta", meta_version_beta);
    mount.mount("/meta/version/nightly", meta_version_nightly);
    mount.mount("/meta/version/rustfmt", meta_version_rustfmt);
    mount.mount("/meta/version/clippy", meta_version_clippy);
    mount.mount("/meta/version/miri", meta_version_miri);
    mount.mount("/meta/gist", gist_router);
    mount.mount("/evaluate.json", evaluate);

    let mut chain = Chain::new(mount);
    let file_logger = FileLogger::new(logfile).expect("Unable to create file logger");
    let logger = StatisticLogger::new(file_logger);
    let rewrite = Rewrite::new(vec![vec!["help".into()]], "/index.html".into());
    let gh_token = GhToken::new(gh_token);

    chain.link_around(logger);
    chain.link_before(rewrite);
    chain.link_before(gh_token);

    if cors_enabled {
        chain.link_around(CorsMiddleware {
            // 当您从a发出请求时，会出现空原点
            // 托管在文件系统上的页面，例如当您阅读时
            // 当地的锈书
            allowed_origins: AllowedOrigins::Any { allow_null: true },
            allowed_headers: vec![UniCase("Content-Type".to_owned())],
            allowed_methods: vec![Get, Post],
            exposed_headers: vec![],
            allow_credentials: false,
            max_age_seconds: ONE_HOUR_IN_SECONDS,
            prefer_wildcard: true,
        });
    }

    info!("Starting the server on http://{}:{}", address, port);
    Iron::new(chain).http((&*address, port)).expect("Unable to start server");
}
```

这里主要 说说`mount.mount("/execute", execute); // crago run | build | test`

- [x] [execute](#execute)

真的好多类型定义, 不怎么想看了, 明明几个路由函数+类型+impl, 都挤到一个文件
但逻辑明明很简单, 却这么长, 类型堆成山似的一样

- [ ] [compile](#compile)



### GhToken

```rs
#[derive(Debug, Clone)]
struct GhToken(Arc<String>);

impl GhToken {
    fn new(token: String) -> Self {
        GhToken(Arc::new(token))
    }
}

impl iron::BeforeMiddleware for GhToken {
    fn before(&self, req: &mut Request) -> IronResult<()> {
        req.extensions.insert::<Self>(self.clone());
        Ok(())
    }
}

impl iron::typemap::Key for GhToken {
    type Value = Self;
}

```

### compile

```rs
fn compile(req: &mut Request) -> IronResult<Response> {
    with_sandbox(req, |sandbox, req: CompileRequest| {
        let req = try!(req.try_into());
        sandbox
            .compile(&req)
            .map(CompileResponse::from)
            .map_err(Error::Sandbox)
    })
}
```

### execute

作为路由的控制函数, 其中的参数当然是浏览器的请求

如: 这

<details>

<summary> 游乐场点击了Run 按钮的例子 </summary>

```json
{
  "channel": "stable",
  "mode": "debug",
  "crateType": "bin",
  "tests": false,
  "code": "#![allow(unused)]\nfn main() {\n\nuse std::process::Command;\n\nlet mut cmd = Command::new(\"echo\");\n\ncmd\n    .arg(\"run\")\n    .arg(\"--rm\")\n    .arg(\"--cap-drop=ALL\")\n    .arg(\"--cap-add=DAC_OVERRIDE\")\n    .arg(\"--security-opt=no-new-privileges\")\n    .args(&[\"--workdir\", \"/playground\"])\n    .args(&[\"--net\", \"none\"])\n    .args(&[\"--memory\", \"256m\"])\n    .args(&[\"--memory-swap\", \"320m\"])\n    .args(&[\"--env\", \"PLAYGROUND_TIMEOUT=10\"])\n    .status().expect(\"failed to execute process\");\n\nprintln!(\"process exited with: {:?}\", cmd);\n\n}",
  "backtrace": false
}
```

</details>

```rs
fn execute(req: &mut Request) -> IronResult<Response> {
    with_sandbox(req, |sandbox, req: ExecuteRequest| {
        let req = try!(req.try_into());
        sandbox
            .execute(&req)
            .map(ExecuteResponse::from)
            .map_err(Error::Sandbox)
    })
}

```

- [x] [with_sandbox](#with_sandbox) 一个启动`sandbox.rs`对应函数的接口函数


### format

``` rs
fn format(req: &mut Request) -> IronResult<Response> {
    with_sandbox(req, |sandbox, req: FormatRequest| {
        let req = try!(req.try_into());
        sandbox
            .format(&req)
            .map(FormatResponse::from)
            .map_err(Error::Sandbox)
    })
}

```

### clippy

```rs
fn clippy(req: &mut Request) -> IronResult<Response> {
    with_sandbox(req, |sandbox, req: ClippyRequest| {
        sandbox
            .clippy(&req.into())
            .map(ClippyResponse::from)
            .map_err(Error::Sandbox)
    })
}

```

### miri

```rs
fn miri(req: &mut Request) -> IronResult<Response> {
    with_sandbox(req, |sandbox, req: MiriRequest| {
        sandbox
            .miri(&req.into())
            .map(MiriResponse::from)
            .map_err(Error::Sandbox)
    })
}

```

### meta_crates

```rs
fn meta_crates(_req: &mut Request) -> IronResult<Response> {
    with_sandbox_no_request(|sandbox| {
        cached(sandbox)
            .crates()
            .map(MetaCratesResponse::from)
    })
}

```

### meta_version_stable

```rs
fn meta_version_stable(_req: &mut Request) -> IronResult<Response> {
    with_sandbox_no_request(|sandbox| {
        cached(sandbox)
            .version_stable()
            .map(MetaVersionResponse::from)
    })
}

```

### meta_version_beta

```rs
fn meta_version_beta(_req: &mut Request) -> IronResult<Response> {
    with_sandbox_no_request(|sandbox| {
        cached(sandbox)
            .version_beta()
            .map(MetaVersionResponse::from)
    })
}

```

### meta_version_nightly

```rs
fn meta_version_nightly(_req: &mut Request) -> IronResult<Response> {
    with_sandbox_no_request(|sandbox| {
        cached(sandbox)
            .version_nightly()
            .map(MetaVersionResponse::from)
    })
}

```

### meta_version_rustfmt

```rs
fn meta_version_rustfmt(_req: &mut Request) -> IronResult<Response> {
    with_sandbox_no_request(|sandbox| {
        cached(sandbox)
            .version_rustfmt()
            .map(MetaVersionResponse::from)
    })
}

```

### meta_version_clippy

```rs
fn meta_version_clippy(_req: &mut Request) -> IronResult<Response> {
    with_sandbox_no_request(|sandbox| {
        cached(sandbox)
            .version_clippy()
            .map(MetaVersionResponse::from)
    })
}

```

### meta_version_miri

```rs
fn meta_version_miri(_req: &mut Request) -> IronResult<Response> {
    with_sandbox_no_request(|sandbox| {
        cached(sandbox)
            .version_miri()
            .map(MetaVersionResponse::from)
    })
}

```

### meta_gist_create

```rs
fn meta_gist_create(req: &mut Request) -> IronResult<Response> {
    let token = req.extensions.get::<GhToken>().unwrap().0.as_ref().clone();
    serialize_to_response(deserialize_from_request(req, |r: MetaGistCreateRequest| {
        let gist = gist::create(token, r.code);
        Ok(MetaGistResponse::from(gist))
    }))
}

```

### meta_gist_get

```rs
fn meta_gist_get(req: &mut Request) -> IronResult<Response> {
    match req.extensions.get::<Router>().unwrap().find("id") {
        Some(id) => {
            let token = req.extensions.get::<GhToken>().unwrap().0.as_ref().clone();
            let gist = gist::load(token, id);
            serialize_to_response(Ok(MetaGistResponse::from(gist)))
        }
        None => {
            Ok(Response::with(status::UnprocessableEntity))
        }
    }
}


```

### evaluate

```rs
// 这是一个向后兼容的垫片。 Rust主页和
// 文档使用它来运行代码。
fn evaluate(req: &mut Request) -> IronResult<Response> {
    with_sandbox(req, |sandbox, req: EvaluateRequest| {
        let req = req.try_into()?;
        sandbox
            .execute(&req)
            .map(EvaluateResponse::from)
            .map_err(Error::Sandbox)
    })
}

```

### with_sandbox

将请求扔给后面函数处理

```rs
fn with_sandbox<Req, Resp, F>(req: &mut Request, f: F) -> IronResult<Response>
where
    F: FnOnce(Sandbox, Req) -> Result<Resp>,
    Req: DeserializeOwned + Clone + Any + 'static,
    Resp: Serialize,
{
    serialize_to_response(run_handler(req, f))
}

```

- [x] [run_handler](#run_handler) 运行`sandbox::对应函数`
- [x] [serialize_to_response](#serialize_to_response) 序列化响应

### with_sandbox_no_request
```rs

fn with_sandbox_no_request<Resp, F>(f: F) -> IronResult<Response>
where
    F: FnOnce(Sandbox) -> Result<Resp>,
    Resp: Serialize,
{
    serialize_to_response(run_handler_no_request(f))
}

```

### run_handler

运行`sandbox::对应函数`

```rs
fn run_handler<Req, Resp, F>(req: &mut Request, f: F) -> Result<Resp>
where
    F: FnOnce(Sandbox, Req) -> Result<Resp>,
    Req: DeserializeOwned + Clone + Any + 'static,
{
    deserialize_from_request(req, |req| {
        let sandbox = Sandbox::new()?;
        f(sandbox, req) // 从execute来的 f
    })
}
```

> `f` == [execute 中的 with_sandbox 第二个参数](#execute)
```rs
|sandbox, req: ExecuteRequest| {
        let req = try!(req.try_into());
        sandbox
            .execute(&req) // <=== 去到了 sandbox.rs 的 execute 函数
            .map(ExecuteResponse::from)
            .map_err(Error::Sandbox)
    }
```

- [x] [deserialize_from_request](#deserialize_from_request)
- [x] [Sandbox::execute](./src/sandbox.md#sandbox::execute)


### deserialize_from_request

浏览器的请求的反序列,和 docker操作,操作完后返回响应

``` rs
fn deserialize_from_request<Req, Resp, F>(req: &mut Request, f: F) -> Result<Resp>
where
    F: FnOnce(Req) -> Result<Resp>,
    Req: DeserializeOwned + Clone + Any + 'static,
{
    let body = req.get::<bodyparser::Struct<Req>>()
        .map_err(Error::Deserialization)?;

    let req = body.ok_or(Error::RequestMissing)?;

    let resp = f(req)?;

    Ok(resp)
}

```

### run_handler_no_request

```rs
fn run_handler_no_request<Resp, F>(f: F) -> Result<Resp>
where
    F: FnOnce(Sandbox) -> Result<Resp>,
{
    let sandbox = Sandbox::new()?;
    let resp = f(sandbox)?;
    Ok(resp)
}

```

### serialize_to_response

序列化响应

```rs
fn serialize_to_response<Resp>(response: Result<Resp>) -> IronResult<Response>
where
    Resp: Serialize,
{
    let response = response.and_then(|resp| {
        let resp = serde_json::ser::to_string(&resp)?;
        Ok(resp)
    });

    match response {
        // 整成 响应格式 , 即便是错误
        Ok(body) => Ok(Response::with((status::Ok, Header(ContentType::json()), body))),
        Err(err) => {
            let err = ErrorJson { error: err.to_string() };
            match serde_json::ser::to_string(&err) {
                Ok(error_str) => Ok(Response::with((status::InternalServerError, Header(ContentType::json()), error_str))),
                Err(_) => Ok(Response::with((status::InternalServerError, Header(ContentType::json()), FATAL_ERROR_JSON))),
            }
        },
    }
}

```

- `serde_json` JSON序列化文件格式.
- `serde_json::ser` 将Rust数据结构序列化为JSON数据.

### struct

### 1. SandboxCacheInfo

```rs
#[derive(Debug, Clone)]
struct SandboxCacheInfo<T> {
    value: T,
    time: Instant,
}

```

### 2. SandboxCacheOne

```rs
/// 缓存单个操作的成功值
#[derive(Debug)]
struct SandboxCacheOne<T>(Mutex<Option<SandboxCacheInfo<T>>>);

impl<T> Default for SandboxCacheOne<T> {
    fn default() -> Self { SandboxCacheOne(Mutex::default()) }
}

impl<T> SandboxCacheOne<T>
where
    T: Clone
{
    fn clone_or_populate<F>(&self, populator: F) -> Result<T>
    where
        F: FnOnce() -> sandbox::Result<T>
    {
        let mut cache = self.0.lock().map_err(|_| Error::CachePoisoned)?;

        match cache.clone() {
            Some(cached) => {
                if cached.time.elapsed() > Duration::from_secs(SANDBOX_CACHE_TIME_TO_LIVE_IN_SECONDS) {
                    SandboxCacheOne::populate(&mut *cache, populator)
                } else {
                    Ok(cached.value)
                }
            },
            None => {
                SandboxCacheOne::populate(&mut *cache, populator)
            }
        }
    }

    fn populate<F>(cache: &mut Option<SandboxCacheInfo<T>>, populator: F) -> Result<T>
    where
        F: FnOnce() -> sandbox::Result<T>
    {
        let value = populator().map_err(Error::Sandbox)?;
        *cache = Some(SandboxCacheInfo {
            value: value.clone(),
            time: Instant::now(),
        });
        Ok(value)
    }
}

```

### 3. SandboxCache
```rs
/// 缓存所有沙盒操作的成功结果
/// 缓存感。
#[derive(Debug, Default)]
struct SandboxCache {
    crates: SandboxCacheOne<Vec<sandbox::CrateInformation>>,
    version_stable: SandboxCacheOne<sandbox::Version>,
    version_beta: SandboxCacheOne<sandbox::Version>,
    version_nightly: SandboxCacheOne<sandbox::Version>,
    version_clippy: SandboxCacheOne<sandbox::Version>,
    version_rustfmt: SandboxCacheOne<sandbox::Version>,
    version_miri: SandboxCacheOne<sandbox::Version>,
}

```

### 4. CachedSandbox
```rs
/// 为Sandbox提供类似的API，用于缓存成功的结果。
struct CachedSandbox<'a> {
    sandbox: Sandbox,
    cache: &'a SandboxCache,
}

impl<'a> CachedSandbox<'a> {
    fn crates(&self) -> Result<Vec<sandbox::CrateInformation>> {
        self.cache.crates.clone_or_populate(|| self.sandbox.crates())
    }

    fn version_stable(&self) -> Result<sandbox::Version> {
        self.cache.version_stable.clone_or_populate(|| {
            self.sandbox.version(sandbox::Channel::Stable)
        })
    }

    fn version_beta(&self) -> Result<sandbox::Version> {
        self.cache.version_beta.clone_or_populate(|| {
            self.sandbox.version(sandbox::Channel::Beta)
        })
    }

    fn version_nightly(&self) -> Result<sandbox::Version> {
        self.cache.version_nightly.clone_or_populate(|| {
            self.sandbox.version(sandbox::Channel::Nightly)
        })
    }

    fn version_clippy(&self) -> Result<sandbox::Version> {
        self.cache.version_clippy.clone_or_populate(|| {
            self.sandbox.version_clippy()
        })
    }

    fn version_rustfmt(&self) -> Result<sandbox::Version> {
        self.cache.version_rustfmt.clone_or_populate(|| {
            self.sandbox.version_rustfmt()
        })
    }

    fn version_miri(&self) -> Result<sandbox::Version> {
        self.cache.version_miri.clone_or_populate(|| {
            self.sandbox.version_miri()
        })
    }
}

```

###

```rs
/// 一个方便的构造函数
fn cached(sandbox: Sandbox) -> CachedSandbox<'static> {
    lazy_static! {
        static ref SANDBOX_CACHE: SandboxCache = Default::default();
    }

    CachedSandbox {
        sandbox,
        cache: &SANDBOX_CACHE,
    }
}

```

### quick_error!

错误宏, 用于相关错误信息的输出
```rs
quick_error! {
    #[derive(Debug)]
    pub enum Error {
        Sandbox(err: sandbox::Error) {
            description("sandbox operation failed")
            display("Sandbox operation failed: {}", err)
            cause(err)
            from()
        }
        Serialization(err: serde_json::Error) {
            description("unable to serialize response")
            display("Unable to serialize response: {}", err)
            cause(err)
            from()
        }
        Deserialization(err: bodyparser::BodyError) {
            description("unable to deserialize request")
            display("Unable to deserialize request: {}", err)
            cause(err)
            from()
        }
        InvalidTarget(value: String) {
            description("an invalid target was passed")
            display("The value {:?} is not a valid target", value)
        }
        InvalidAssemblyFlavor(value: String) {
            description("an invalid assembly flavor was passed")
            display("The value {:?} is not a valid assembly flavor", value)
        }
        InvalidDemangleAssembly(value: String) {
            description("an invalid demangling option was passed")
            display("The value {:?} is not a valid demangle option", value)
        }
        InvalidProcessAssembly(value: String) {
            description("an invalid assembly processing option was passed")
            display("The value {:?} is not a valid assembly processing option", value)
        }
        InvalidChannel(value: String) {
            description("an invalid channel was passed")
            display("The value {:?} is not a valid channel", value,)
        }
        InvalidMode(value: String) {
            description("an invalid mode was passed")
            display("The value {:?} is not a valid mode", value)
        }
        InvalidEdition(value: String) {
            description("an invalid edition was passed")
            display("The value {:?} is not a valid edition", value)
        }
        InvalidCrateType(value: String) {
            description("an invalid crate type was passed")
            display("The value {:?} is not a valid crate type", value)
        }
        RequestMissing {
            description("no request was provided")
            display("No request was provided")
        }
        CachePoisoned {
            description("the cache has been poisoned")
            display("The cache has been poisoned")
        }
    }
}

```

### type Result

```rs
type Result<T> = ::std::result::Result<T, Error>;

const FATAL_ERROR_JSON: &str =
    r#"{"error": "Multiple cascading errors occurred, abandon all hope"}"#;


```

## struct

### 1. ErrorJson

```rs
#[derive(Debug, Clone, Serialize)]
struct ErrorJson {
    error: String,
}

```

### 2. CompileRequest

```rs
#[derive(Debug, Clone, Deserialize)]
struct CompileRequest {
    target: String,
    #[serde(rename = "assemblyFlavor")]
    assembly_flavor: Option<String>,
    #[serde(rename = "demangleAssembly")]
    demangle_assembly: Option<String>,
    #[serde(rename = "processAssembly")]
    process_assembly: Option<String>,
    channel: String,
    mode: String,
    #[serde(default)]
    edition: String,
    #[serde(rename = "crateType")]
    crate_type: String,
    tests: bool,
    #[serde(default)]
    backtrace: bool,
    code: String,
}

```

### 3. CompileResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct CompileResponse {
    success: bool,
    code: String,
    stdout: String,
    stderr: String,
}

```

### 4. ExecuteRequest

```rs
#[derive(Debug, Clone, Deserialize)]
struct ExecuteRequest {
    channel: String,
    mode: String,
    #[serde(default)]
    edition: String,
    #[serde(rename = "crateType")]
    crate_type: String,
    tests: bool,
    #[serde(default)]
    backtrace: bool,
    code: String,
}

```

### 5. ExecuteResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct ExecuteResponse {
    success: bool,
    stdout: String,
    stderr: String,
}

```

### 6. FormatRequest

```rs
#[derive(Debug, Clone, Deserialize)]
struct FormatRequest {
    code: String,
}

```

### 7. FormatResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct FormatResponse {
    success: bool,
    code: String,
    stdout: String,
    stderr: String,
}

```

### 8. ClippyRequest

```rs
#[derive(Debug, Clone, Deserialize)]
struct ClippyRequest {
    code: String,
}

```

### 9. ClippyResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct ClippyResponse {
    success: bool,
    stdout: String,
    stderr: String,
}

```

### 10. MiriRequest

```rs
#[derive(Debug, Clone, Deserialize)]
struct MiriRequest {
    code: String,
}

```

### 11. MiriResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct MiriResponse {
    success: bool,
    stdout: String,
    stderr: String,
}

```

### 12. CrateInformation

```rs
#[derive(Debug, Clone, Serialize)]
struct CrateInformation {
    name: String,
    version: String,
    id: String,
}

```

### 13. MetaCratesResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct MetaCratesResponse {
    crates: Vec<CrateInformation>,
}

```

### 14. MetaVersionResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct MetaVersionResponse {
    version: String,
    hash: String,
    date: String,
}

```

### 15. MetaGistCreateRequest

```rs
#[derive(Debug, Clone, Deserialize)]
struct MetaGistCreateRequest {
    code: String,
}

```

### 16. MetaGistResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct MetaGistResponse {
    id: String,
    url: String,
    code: String,
}

```

### 17. EvaluateRequest

```rs
#[derive(Debug, Clone, Deserialize)]
struct EvaluateRequest {
    version: String,
    optimize: String,
    code: String,
}

```

### 18. EvaluateResponse

```rs
#[derive(Debug, Clone, Serialize)]
struct EvaluateResponse {
    result: String,
    error: Option<String>,
}

```

## impl:给结构写方法

### sandbox::CompileRequest

```rs
impl TryFrom<CompileRequest> for sandbox::CompileRequest {
    type Error = Error;

    fn try_from(me: CompileRequest) -> Result<Self> {
        let target = parse_target(&me.target)?;
        let assembly_flavor = match me.assembly_flavor {
            Some(f) => Some(parse_assembly_flavor(&f)?),
            None => None,
        };

        let demangle = match me.demangle_assembly {
            Some(f) => Some(parse_demangle_assembly(&f)?),
            None => None,
        };

        let process_assembly = match me.process_assembly {
            Some(f) => Some(parse_process_assembly(&f)?),
            None => None,
        };

        let target = match (target, assembly_flavor, demangle, process_assembly) {
            (sandbox::CompileTarget::Assembly(_, _, _), Some(flavor), Some(demangle), Some(process)) =>
                sandbox::CompileTarget::Assembly(flavor, demangle, process),
            _ => target,
        };

        Ok(sandbox::CompileRequest {
            target,
            channel: parse_channel(&me.channel)?,
            mode: parse_mode(&me.mode)?,
            edition: parse_edition(&me.edition)?,
            crate_type: parse_crate_type(&me.crate_type)?,
            tests: me.tests,
            backtrace: me.backtrace,
            code: me.code,
        })
    }
}

```

### CompileResponse

```rs
impl From<sandbox::CompileResponse> for CompileResponse {
    fn from(me: sandbox::CompileResponse) -> Self {
        CompileResponse {
            success: me.success,
            code: me.code,
            stdout: me.stdout,
            stderr: me.stderr,
        }
    }
}
```

### sandbox::ExecuteRequest

```rs
impl TryFrom<ExecuteRequest> for sandbox::ExecuteRequest {
    type Error = Error;

    fn try_from(me: ExecuteRequest) -> Result<Self> {
        Ok(sandbox::ExecuteRequest {
            channel: try!(parse_channel(&me.channel)),
            mode: try!(parse_mode(&me.mode)),
            edition: parse_edition(&me.edition)?,
            crate_type: try!(parse_crate_type(&me.crate_type)),
            tests: me.tests,
            backtrace: me.backtrace,
            code: me.code,
        })
    }
}

```

### ExecuteResponse

```rs
impl From<sandbox::ExecuteResponse> for ExecuteResponse {
    fn from(me: sandbox::ExecuteResponse) -> Self {
        ExecuteResponse {
            success: me.success,
            stdout: me.stdout,
            stderr: me.stderr,
        }
    }
}

```

### sandbox::FormatRequest

```rs
impl TryFrom<FormatRequest> for sandbox::FormatRequest {
    type Error = Error;

    fn try_from(me: FormatRequest) -> Result<Self> {
        Ok(sandbox::FormatRequest {
            code: me.code,
        })
    }
}

```

### FormatResponse

```rs
impl From<sandbox::FormatResponse> for FormatResponse {
    fn from(me: sandbox::FormatResponse) -> Self {
        FormatResponse {
            success: me.success,
            code: me.code,
            stdout: me.stdout,
            stderr: me.stderr,
        }
    }
}

```

### sandbox::ClippyRequest

```rs
impl From<ClippyRequest> for sandbox::ClippyRequest {
    fn from(me: ClippyRequest) -> Self {
        sandbox::ClippyRequest {
            code: me.code,
        }
    }
}

```

### ClippyResponse

```rs
impl From<sandbox::ClippyResponse> for ClippyResponse {
    fn from(me: sandbox::ClippyResponse) -> Self {
        ClippyResponse {
            success: me.success,
            stdout: me.stdout,
            stderr: me.stderr,
        }
    }
}

```

### sandbox::MiriRequest

```rs
impl From<MiriRequest> for sandbox::MiriRequest {
    fn from(me: MiriRequest) -> Self {
        sandbox::MiriRequest {
            code: me.code,
        }
    }
}

```

### MiriResponse

```rs
impl From<sandbox::MiriResponse> for MiriResponse {
    fn from(me: sandbox::MiriResponse) -> Self {
        MiriResponse {
            success: me.success,
            stdout: me.stdout,
            stderr: me.stderr,
        }
    }
}

```

### MetaCratesResponse

```rs
impl From<Vec<sandbox::CrateInformation>> for MetaCratesResponse {
    fn from(me: Vec<sandbox::CrateInformation>) -> Self {
        let crates = me.into_iter()
            .map(|cv| CrateInformation { name: cv.name, version: cv.version, id: cv.id })
            .collect();

        MetaCratesResponse {
            crates,
        }
    }
}

```

### MetaVersionResponse

```rs
impl From<sandbox::Version> for MetaVersionResponse {
    fn from(me: sandbox::Version) -> Self {
        MetaVersionResponse {
            version: me.release,
            hash: me.commit_hash,
            date: me.commit_date,
        }
    }
}

```

### MetaGistResponse

```rs
impl From<gist::Gist> for MetaGistResponse {
    fn from(me: gist::Gist) -> Self {
        MetaGistResponse {
            id: me.id,
            url: me.url,
            code: me.code,
        }
    }
}

```

### sandbox::ExecuteRequest

```rs
impl TryFrom<EvaluateRequest> for sandbox::ExecuteRequest {
    type Error = Error;

    fn try_from(me: EvaluateRequest) -> Result<Self> {
        Ok(sandbox::ExecuteRequest {
            channel: parse_channel(&me.version)?,
            mode: if me.optimize != "0" { sandbox::Mode::Release } else { sandbox::Mode::Debug },
            edition: None, // FIXME：这应该是什么？
            crate_type: sandbox::CrateType::Binary,
            tests: false,
            backtrace: false,
            code: me.code,
        })
    }
}

```

### EvaluateResponse

```rs
impl From<sandbox::ExecuteResponse> for EvaluateResponse {
    fn from(me: sandbox::ExecuteResponse) -> Self {
        // 旧操场没有使用货物，所以从来没有使用过货物
        // 打印的货物输出（“编制操场......”）
        // 到斯特德尔。由于此端点用于内联结果
        // 页面，除非出现错误，否则不要包含stderr
        // 发生。
        if me.success {
            EvaluateResponse {
                result: me.stdout,
                error: None,
            }
        } else {
            // 发生错误时，*某些*消费者会检查
            // `error`键，其他人认为错误是塞进来的
            // `result`字段，然后他们串搜索
            // `error：`或`警告：`。 EW。我们可以把它放在两者中。
            let result = me.stderr + &me.stdout;
            EvaluateResponse {
                result: result.clone(),
                error: Some(result),
            }
        }
    }
}

```

## parse

解析数据

### parse_target

```rs
fn parse_target(s: &str) -> Result<sandbox::CompileTarget> {
    Ok(match s {
        "asm" => sandbox::CompileTarget::Assembly(sandbox::AssemblyFlavor::Att,
                                                  sandbox::DemangleAssembly::Demangle,
                                                  sandbox::ProcessAssembly::Filter),
        "llvm-ir" => sandbox::CompileTarget::LlvmIr,
        "mir" => sandbox::CompileTarget::Mir,
        "wasm" => sandbox::CompileTarget::Wasm,
        _ => return Err(Error::InvalidTarget(s.into()))
    })
}

```

### parse_assembly_flavor

```rs
fn parse_assembly_flavor(s: &str) -> Result<sandbox::AssemblyFlavor> {
    Ok(match s {
        "att" => sandbox::AssemblyFlavor::Att,
        "intel" => sandbox::AssemblyFlavor::Intel,
        _ => return Err(Error::InvalidAssemblyFlavor(s.into()))
    })
}

```

### parse_demangle_assembly

```rs
fn parse_demangle_assembly(s: &str) -> Result<sandbox::DemangleAssembly> {
    Ok(match s {
        "demangle" => sandbox::DemangleAssembly::Demangle,
        "mangle" => sandbox::DemangleAssembly::Mangle,
        _ => return Err(Error::InvalidDemangleAssembly(s.into()))
    })
}

```

### parse_process_assembly

```rs
fn parse_process_assembly(s: &str) -> Result<sandbox::ProcessAssembly> {
    Ok(match s {
        "filter" => sandbox::ProcessAssembly::Filter,
        "raw" => sandbox::ProcessAssembly::Raw,
        _ => return Err(Error::InvalidProcessAssembly(s.into()))
    })
}

```

### parse_channel

```rs
fn parse_channel(s: &str) -> Result<sandbox::Channel> {
    Ok(match s {
        "stable" => sandbox::Channel::Stable,
        "beta" => sandbox::Channel::Beta,
        "nightly" => sandbox::Channel::Nightly,
        _ => return Err(Error::InvalidChannel(s.into()))
    })
}

```

### parse_mode

```rs
fn parse_mode(s: &str) -> Result<sandbox::Mode> {
    Ok(match s {
        "debug" => sandbox::Mode::Debug,
        "release" => sandbox::Mode::Release,
        _ => return Err(Error::InvalidMode(s.into()))
    })
}

```

### parse_edition

```rs
fn parse_edition(s: &str) -> Result<Option<sandbox::Edition>> {
    Ok(match s {
        "" => None,
        "2015" => Some(sandbox::Edition::Rust2015),
        "2018" => Some(sandbox::Edition::Rust2018),
        _ => return Err(Error::InvalidEdition(s.into()))
    })
}

```

### parse_crate_type

```rs
fn parse_crate_type(s: &str) -> Result<sandbox::CrateType> {
    use sandbox::{CrateType::*, LibraryType::*};
    Ok(match s {
        "bin" => Binary,
        "lib" => Library(Lib),
        "dylib" => Library(Dylib),
        "rlib" => Library(Rlib),
        "staticlib" => Library(Staticlib),
        "cdylib" => Library(Cdylib),
        "proc-macro" => Library(ProcMacro),
        _ => return Err(Error::InvalidCrateType(s.into()))
    })
}
```
