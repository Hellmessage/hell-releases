# hproxy

**Linux TUN→VLESS 透明分流代理 CLI** — 单一静态二进制 (内嵌 sing-box 内核 + geo 规则集), 不依赖 libc, 拷过去即用.

> 主要特性: TUN 设备 + `auto_route` 透明导入全机流量 (进程无感, 不设 SOCKS/HTTP 代理) · 出站走 **VLESS + TLS + REALITY + xtls-rprx-vision** · 多节点规则分流 (直连 / 指定节点 / `auto` url-test 测速选最快) · `GEO-IP,CN` / `GEO-SITE,CN` 内嵌规则集 · DNS 分流 (直连域名走直连 DNS, 代理域名走代理 DNS) · systemd 开机自启 · 可视化配置生成器 (`hproxy config` 起本地网页填表生成/校验 YAML).

## 下载

| 架构 | 下载 |
| --- | --- |
| Linux x64 (amd64) | [hproxy-linux-amd64](https://github.com/Hellmessage/hell-releases/releases/download/hproxy-v0.1.0/hproxy-linux-amd64) |
| Linux ARM64 (arm64) | [hproxy-linux-arm64](https://github.com/Hellmessage/hell-releases/releases/download/hproxy-v0.1.0/hproxy-linux-arm64) |

最新版本及校验信息以 [`latest.json`](latest.json) 为准 (机器可读).

## 系统要求

- Linux (x64 或 ARM64), 需 `/dev/net/tun`
- root 权限或 `CAP_NET_ADMIN` (创建 TUN 网卡 + 改路由表必需)
- systemd (仅 `enable`/`disable`/`status` 开机自启相关命令需要)

## 安装

下载对应架构的二进制, 赋可执行权限即可运行; 单文件自包含, 不需要额外装驱动 / 规则集:

```sh
chmod +x hproxy-linux-amd64
sudo ./hproxy-linux-amd64 install        # 装到 /usr/local/bin/hproxy
```

## 用法

```sh
sudo hproxy run -c config.yaml           # 前台拉起 TUN 分流代理 (Ctrl-C 干净拆除)
sudo hproxy run -c config.yaml --global  # 忽略 rules 全量走代理 (内网仍直连)
sudo hproxy enable config.yaml           # 开机自启 (systemd enable --now)
sudo hproxy status                       # 查看服务状态
sudo hproxy disable [--purge]            # 停用并拆除 (--purge 连配置一起删)
hproxy dump-config -c config.yaml        # 打印生成的 sing-box 配置 (调试, 不启动)
hproxy config [--addr 127.0.0.1:8088]    # 起 web 服务, 可视化生成/编辑配置
hproxy version
```

配置格式 (多 VLESS 节点 + 规则 + DNS) 见源码仓 README; 或用 `hproxy config` 网页填表生成.

## 反馈

报 bug / 提需求请发到 [Issues](https://github.com/Hellmessage/hell-releases/issues) (本仓), 在标题前加 `[hproxy]` 前缀. 源码仓私有, 不接受外部 PR; 但欢迎在 issue 区贴 patch / 思路.

## License

二进制版权保留. 内嵌的 [sing-box](https://github.com/SagerNet/sing-box) / gvisor / uTLS 等组件及 geoip-cn / geosite-cn 规则集各自适用上游 license, 详见各上游仓库.
