# hell-releases

Hellmessage 开发的所有工具的**发行总仓**与**版本台账**.

源码仓私有, 本仓公开. 用户在这里看到工具介绍 / 截图 / 下载链接, 工具内置的自升级也从这里拉 manifest.

## 工具清单

| 工具 | 简介 | 平台 | 当前版本 | 入口 |
| --- | --- | --- | --- | --- |
| [htun](htun/) | Windows TUN 全局代理 (Wintun + mihomo, 单 exe 自包含) | windows/amd64, windows/arm64 | _尚未发布_ | [htun/](htun/) |

> 新工具上线后, 在本表追加一行, 并在仓内开 `<tool>/` 子目录 (README + CHANGELOG + assets + 由 CI 维护的 `latest.json`).

## 仓库布局

```
hell-releases/
├── README.md            ← 本文件 (面向用户)
├── MANIFEST.md          ← latest.json schema 文档 (面向自升级实现者)
├── index.json           ← 机器可读的工具清单
├── htun/
│   ├── README.md        ← htun 用户文档 (截图 / 下载 / 用法)
│   ├── CHANGELOG.md     ← htun 版本日志
│   ├── latest.json      ← stable channel 指针 (CI 维护, 不要手改)
│   └── assets/          ← 截图 / 图标
└── <future-tool>/...
```

二进制本身**不入 git**, 全走 GitHub Releases 的 asset (免费 CDN, 长期稳定 URL).

## Tag 命名约定

发行 tag 在本仓的格式: **`<tool>-v<semver>`**

例: `htun-v0.1.0`, `webtap-v0.5.2`. 单仓多工具的标准做法 — 不同工具的 release 在同一 `releases/` 列表里靠前缀区分, `gh release list` 一眼能筛.

> 私有源码仓里的 tag 用同一格式 (`htun-v0.1.0`), CI 拿到 tag 后剥出版本号, 触发 build → 上传到本仓的同名 release.

## 自升级契约

工具内置的"检查更新"逻辑约定:

1. 拉 `https://raw.githubusercontent.com/Hellmessage/hell-releases/main/<tool>/latest.json`
2. 比 `version` 字段与自身版本
3. 按当前 OS/arch 找 `assets[]` 里匹配 `platform`
4. 下载 → **校验 sha256** → 替换并重启

schema 与字段语义见 [MANIFEST.md](MANIFEST.md).

## 反馈 / 报 bug

各工具的 issue 入口在工具自己的子目录 README. 本仓只接受"发行 / 下载 / 链接"相关的 issue.
