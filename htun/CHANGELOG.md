# htun CHANGELOG

## [0.7.0] — 2026-06-03

Windows TUN 全局代理客户端 — 单 exe 自包含 (内嵌 Wintun + mihomo), 无驱动安装, 双击即用.

- **TUN 全局接管**: 进程无感, 不依赖系统 SOCKS / HTTP 代理设置
- **协议**: Hysteria2 / TUIC / VLESS / Trojan / SS-2022 / AnyTLS / VMess; 内核可切换 (mihomo / sing-box)
- **订阅**: Clash YAML / sing-box 订阅导入, 节点测速 + 一键选最快
- **连接 dashboard**: 按进程 / 目标 / 协议 / 速率实时查看
- **系统托盘** + 关窗到托盘 + 开机自启, 内置 DNS 防投毒
- **便携模式**: exe 同目录放空文件 `htun.portable`, 配置/日志改写到 exe 旁
- **客户端自升级**: 拉 `latest.json` → 校验 sha256 → rename-self 替换重启, 配置不丢

下载与校验信息见 [GitHub Release](https://github.com/Hellmessage/hell-releases/releases/tag/htun-v0.7.0) 与 [`latest.json`](latest.json).

---

每版本一段, 倒序排列 (最新在上). 格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/).
