# hproxy CHANGELOG

## [0.1.0] — 2026-06-03

首个公开版本. 详见 [GitHub Release](https://github.com/Hellmessage/hell-releases/releases/tag/hproxy-v0.1.0).

- Linux TUN→VLESS(REALITY + xtls-rprx-vision) 透明分流代理, 内嵌 sing-box 内核, 单一静态二进制 (CGO_ENABLED=0)
- 多节点规则分流: 直连 / 指定节点 / `auto` (url-test 测速选最快); 内网/私有地址始终直连
- 规则类型: `IP` / `IP-CIDR` / `DOMAIN` / `DOMAIN-SUFFIX` / `DOMAIN-KEYWORD` / `GEO-IP,{LAN,CN}` / `GEO-SITE,CN` / `MATCH`; geoip-cn / geosite-cn 规则集 `go:embed` 内嵌
- DNS 分流 (直连域名 / 代理域名各走各的 DNS)
- systemd 开机自启 (`enable` / `disable` / `status`), 支持多实例 (`--name`)
- 可视化配置生成器 (`hproxy config` 起本地网页)
- 双架构: linux/amd64 + linux/arm64

每个版本一段, 倒序排列 (最新在上). 格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/).
