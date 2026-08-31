# meta-rules-overseas-saas

海外 SaaS 域名规则集,**完全按业务组粒度拆分**(每个 yaml 对应 OpenClash 一个具体业务组),为 **mihomo / clash-meta / OpenClash** 设计。

## 为什么需要这个仓库?

[meta-rules-dat](https://github.com/metacubex/meta-rules-dat) 是 mihomo 生态最常用的规则集,但它**只包含被 GFW 屏蔽的域名**(geosite-gfw)且按大类聚合(如 `gfw`/`google`/`netflix` 各一个文件),无法精细匹配每个用户的 OpenClash 业务组配置。

本仓库提供**按业务组拆分的域名列表**,每个 yaml 对应一个具体的业务组(如 `chatgpt.yaml` → `ChatGPT` 业务组、`youtube.yaml` → `YouTube` 业务组)。你可以**直接引用对应的 yaml 作为 rule-provider**,无需再手动维护每个业务组的域名补充。

## 使用方式(OpenClash 示例)

```yaml
rule-providers:
  chatgpt:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/chatgpt.mrs"
    path: "./rule_provider/chatgpt.mrs"
    interval: 86400

  youtube:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/youtube.mrs"
    path: "./rule_provider/youtube.mrs"
    interval: 86400

rules:
  - RULE-SET,chatgpt,  ChatGPT
  - RULE-SET,youtube,  YouTube
  - RULE-SET,telegram, Telegram
```

## 可用的规则集

**主流业务组(22 个,核心):**

| yaml | 对应业务组 | 域名数 |
|---|---|---|
| `chatgpt.yaml` | ChatGPT | 19 |
| `claude.yaml` | Claude | 14 |
| `copilot.yaml` | Copilot | 10 |
| `perplexity.yaml` | Perplexity | 9 |
| `gemini.yaml` | Gemini | 13 |
| `meta-ai.yaml` | Meta AI | 13 |
| `twitter.yaml` | Twitter(X) | 19 |
| `facebook.yaml` | Facebook | 23 |
| `telegram.yaml` | Telegram | 25 |
| `whatsapp.yaml` | WhatsApp | 15 |
| `reddit.yaml` | Reddit | 13 |
| `youtube.yaml` | YouTube | 25 |
| `tiktok.yaml` | TikTok | 22 |
| `netflix.yaml` | Netflix | 18 |
| `disney.yaml` | Disney | 23 |
| `hbo.yaml` | HBO | 26 |
| `spotify.yaml` | Spotify | 23 |
| `amazon.yaml` | Amazon | 27 |
| `crunchyroll.yaml` | Crunchyroll | 21 |
| `github.yaml` | GitHub | 27 |
| `nvidia.yaml` | Nvidia | 26 |
| `steam.yaml` | Steam | 27 |

**扩展业务组(8 个,可选):**

| yaml | 用途 | 域名数 |
|---|---|---|
| `microsoft-store.yaml` | Microsoft Store / Xbox / PlayStation(国内 IP 被 Microsoft 主动 403,坑 67) | 26 |
| `discord.yaml` | Discord(OpenClash 没独立业务组时可建) | 25 |
| `slack.yaml` | Slack(配合 Discord) | 19 |
| `notion.yaml` | Notion(配合 Discord/Slack) | 20 |
| `wikipedia.yaml` | Wikipedia / Wikimedia 全家桶 | 20 |
| `scholar.yaml` | 学术(Google Scholar / ResearchGate / arXiv / PubMed) | 26 |
| `cloud-ide.yaml` | 云 IDE(Gitpod / StackBlitz / CodeSandbox / Replit) | 13 |
| `pornhub.yaml` | 18+ 类(选择性使用) | 28 |

**总计 30 个 yaml / 618 个域名**

## 仓库结构

```
<业务组>.yaml / <业务组>.mrs       # 30 对一一对应
LICENSE                              # MIT
README.md
.github/workflows/
  build-mrs.yml                      # GitHub Actions 自动编译 30 个 .mrs
```

## 贡献

接受 PR!新增/修改域名时:

1. 编辑对应业务组的 yaml(确保只放该业务组的域名)
2. 保持 `+.` 前缀(mihomo domain suffix 匹配)
3. 提交 PR,描述你为什么要加这个域名

## License

MIT — 详见 [LICENSE](./LICENSE) 文件。