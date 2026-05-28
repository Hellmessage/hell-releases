# htun

**Windows TUN 全局代理客户端** — 单 exe 自包含 (内嵌 Wintun + mihomo), 无驱动安装步骤, 双击即用.

> 主要特性: TUN 层接管 (不走 SOCKS / HTTP 代理设置, 进程无感) · Clash YAML / sing-box 订阅导入 · 节点测速 + 一键选最快 · 连接 dashboard (按进程/目标/协议/速率) · 系统托盘 + 关窗到托盘 + 开机自启.

## 下载

> 首次发布前此处为空. CI 在 push tag `htun-vX.Y.Z` 时自动上传双架构 exe.

| 架构 | 下载 |
| --- | --- |
| Windows x64 (amd64) | _尚未发布_ |
| Windows ARM64 (arm64) | _尚未发布_ |

最新版本及校验信息以 [`latest.json`](latest.json) 为准 (机器可读, 自升级模块用得上).

## 截图

> 截图归档在 [`assets/`](assets/) 下, 命名 `01-dashboard.png` / `02-nodes.png` 之类.

## 系统要求

- Windows 10 1809+ / Windows 11 (x64 或 ARM64)
- WebView2 Runtime (Win11 内置; Win10 缺失时 htun 首次启动会引导下载)
- 管理员权限 (创建 TUN 网卡 + 改路由表必需)

## 安装

直接下载对应架构的 `htun-windows-<arch>.exe`, 放到任意目录运行. 单 exe 自包含, 不需要装驱动.

默认数据落盘位置: `%LOCALAPPDATA%\htun\`
- `config.sqlite` — 节点 / 订阅 / 规则 / 偏好
- `proxy.yaml` — 上次启动时的导出配置 (回滚备份用)
- `logs/` — 应用日志

**便携模式**: exe 同目录建一个空文件 `htun.portable`, 三类落盘改写到 exe 旁 (适合 U 盘携带 / 多账户隔离).

## 自升级

htun 检测到新版本时会提示, 用户点确认后下到临时目录 → 校验 sha256 → 替换并重启. 升级**不会**丢失 `%LOCALAPPDATA%\htun\` 下的配置 (sqlite schema 走 migration, 永远不 drop).

## 反馈

报 bug / 提需求请发到 [Issues](https://github.com/Hellmessage/hell-releases/issues) (本仓), 在标题前加 `[htun]` 前缀. 源码仓私有, 不接受外部 PR; 但欢迎在 issue 区贴 patch / 思路.

## License

二进制版权保留. wintun.dll 与内嵌的 mihomo / gvisor 等组件各自适用上游 license, 详见各上游仓库.
