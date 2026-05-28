# MANIFEST schema

`<tool>/latest.json` 是自升级的**唯一契约**. CI 在 release 出包后回写, 客户端读取后决定是否升级.

## 字段定义

```json
{
  "tool": "htun",
  "channel": "stable",
  "version": "0.1.0",
  "tag": "htun-v0.1.0",
  "released_at": "2026-05-28T10:00:00Z",
  "notes_url": "https://github.com/Hellmessage/hell-releases/releases/tag/htun-v0.1.0",
  "min_upgradable_from": "0.0.0",
  "assets": [
    {
      "platform": "windows/amd64",
      "url": "https://github.com/Hellmessage/hell-releases/releases/download/htun-v0.1.0/htun-windows-amd64.exe",
      "sha256": "0000000000000000000000000000000000000000000000000000000000000000",
      "size": 87654321
    },
    {
      "platform": "windows/arm64",
      "url": "https://github.com/Hellmessage/hell-releases/releases/download/htun-v0.1.0/htun-windows-arm64.exe",
      "sha256": "0000000000000000000000000000000000000000000000000000000000000000",
      "size": 87654321
    }
  ]
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `tool` | string | ✅ | 工具名, 与本仓目录名一致 |
| `channel` | string | ✅ | 当前 `"stable"`. 留待未来加 `"beta"`, 客户端按 channel 拉 `<tool>/<channel>.json` |
| `version` | string | ✅ | 严格 semver, 不带 `v` 前缀 |
| `tag` | string | ✅ | GitHub release tag, 形如 `htun-v0.1.0` |
| `released_at` | string | ✅ | ISO 8601 UTC |
| `notes_url` | string | ✅ | release notes 入口 (一般指向同 tag 的 release 页) |
| `min_upgradable_from` | string | ⬜ | 允许从该最低版本无障碍升级 (含). 低于该版本的客户端必须走"全量重装", 提示用户 |
| `assets` | array | ✅ | 至少一项. 每项一对 (platform, url, sha256, size) |
| `assets[].platform` | string | ✅ | 形如 `<os>/<arch>`, 与 Go GOOS/GOARCH 对齐 (`windows/amd64`, `windows/arm64`, `darwin/arm64`, `linux/amd64` ...) |
| `assets[].url` | string | ✅ | 可直下的 GitHub release asset URL |
| `assets[].sha256` | string | ✅ | 资产文件 sha256 hex (64 位小写). **客户端必须校验** |
| `assets[].size` | number | ✅ | 字节数, 用于进度条 |

## 兼容性规则

- **加字段**: 可选 + 默认值, 老客户端忽略即可
- **删字段 / 改语义**: 禁止 — 老客户端会读到 stale schema 但仍要能升到新版本
- **重命名**: 走"新字段并存, 老字段保留一个发布周期"
- 想做破坏性 schema 升级 → 开新 channel (例 `stable-v2.json`), 老客户端继续指 `latest.json`

## 客户端实现要点

1. **HTTP 缓存**: raw.githubusercontent.com 走 ~5 分钟 CDN 缓存, 客户端"检查更新"按钮**不必**也设缓存, 让 CDN 顶
2. **sha256 校验失败**: 删除已下载文件 + 显式报错给用户 ("下载完整性校验失败, 请重试; 持续失败请向开发者反馈"), **不要静默重试**, 可能是中间人
3. **降级保护**: `version < 当前版本` 时直接忽略, 不弹"是否降级", 避免 manifest 被回滚出问题
4. **断网容忍**: manifest 404 / 网络失败一律静默 (用户主动点击除外, 这时显式报错)
