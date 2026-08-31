# meta-rules-overseas-saas

海外 SaaS 域名规则集,**完全按业务组粒度拆分**(每个 yaml 对应 OpenClash 一个具体业务组),为 **mihomo / clash-meta / OpenClash** 设计。

## 为什么需要这个仓库?

[meta-rules-dat](https://github.com/metacubex/meta-rules-dat) 是 mihomo 生态最常用的规则集,但它**只包含被 GFW 屏蔽的域名**(geosite-gfw)且按大类聚合(如 `gfw`/`google`/`netflix` 各一个文件),无法精细匹配每个用户的 OpenClash 业务组配置。

本仓库提供**按业务组拆分的域名列表**,每个 yaml 对应一个具体的业务组(如 `chatgpt.yaml` → `ChatGPT` 业务组、`youtube.yaml` → `YouTube` 业务组)。你可以**直接引用对应的 yaml 作为 rule-provider**,无需再手动维护每个业务组的域名补充。

## 使用方式(OpenClash 示例)

假设你的 OpenClash 有 `ChatGPT` / `Claude` / `YouTube` / `Netflix` / `Telegram` 等业务组,加 rule-provider:

```yaml
rule-providers:
  chatgpt:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/chatgpt.mrs"
    path: "./rule_provider/chatgpt.mrs"
    interval: 86400

  claude:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/claude.mrs"
    path: "./rule_provider/claude.mrs"
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
  - RULE-SET,claude,   Claude
  - RULE-SET,youtube,  YouTube
  - RULE-SET,twitter,  Twitter(X)   # 你可能已用旧 mrs,可叠加
```

## 可用的规则集(每个文件 = 一个业务组)

| yaml 文件 | 业务组 | 域名数 | 包含 |
|---|---|---|---|
| `chatgpt.yaml` | ChatGPT | 19 | openai.com, chatgpt.com, sora, oaiusercontent, etc. |
| `claude.yaml` | Claude | 14 | anthropic.com, claude.ai, claudeusercontent, etc. |
| `copilot.yaml` | Copilot | 10 | copilot.microsoft.com, githubcopilot, bing, etc. |
| `perplexity.yaml` | Perplexity | 9 | perplexity.ai, pplx, etc. |
| `gemini.yaml` | Gemini | 13 | gemini.google.com, deepmind, notebooklm, etc. |
| `meta-ai.yaml` | Meta AI | 13 | meta.ai, llama, instagram, threads, etc. |
| `twitter.yaml` | Twitter(X) | 19 | twitter.com, x.com, twimg, tweetdeck, etc. |
| `facebook.yaml` | Facebook | 23 | facebook.com, fbcdn, instagram, oculus, etc. |
| `telegram.yaml` | Telegram | 25 | telegram.org, t.me, telegra.ph, ton.org, etc. |
| `whatsapp.yaml` | WhatsApp | 15 | whatsapp.com, wa.me, etc. |
| `reddit.yaml` | Reddit | 13 | reddit.com, redd.it, etc. |
| `youtube.yaml` | YouTube | 25 | youtube.com, googlevideo, ytimg, etc. |
| `tiktok.yaml` | TikTok | 22 | tiktok.com, bytedance, douyin, etc. |
| `netflix.yaml` | Netflix | 18 | netflix.com, nflxext, nflximg, etc. |
| `disney.yaml` | Disney | 23 | disneyplus.com, hotstar, hulu, etc. |
| `hbo.yaml` | HBO | 26 | hbomax.com, warnermedia, cnn, etc. |
| `spotify.yaml` | Spotify | 23 | spotify.com, scdn.co, etc. |
| `amazon.yaml` | Amazon | 27 | amazon.com, aws, primevideo, twitch, etc. |
| `crunchyroll.yaml` | Crunchyroll | 21 | crunchyroll.com, funimation, myanimelist, etc. |
| `github.yaml` | GitHub | 27 | github.com, gist, copilot.github, etc. |
| `nvidia.yaml` | Nvidia | 26 | nvidia.com, geforce, etc. |
| `steam.yaml` | Steam | 27 | steam, epicgames, gog, playstation, xbox, etc. |
| **总计** | **22 个业务组** | **438** | |

## 仓库结构

```
chatgpt.yaml / chatgpt.mrs        # 业务组 ChatGPT
claude.yaml  / claude.mrs         # 业务组 Claude
... 22 对
LICENSE                            # MIT
README.md
.github/workflows/
  build-mrs.yml                    # GitHub Actions 自动编译 22 个 .mrs
```

## 贡献

接受 PR!新增/修改域名时:

1. 编辑对应业务组的 yaml(确保只放该业务组的域名)
2. 保持 `+.` 前缀(mihomo domain suffix 匹配)
3. 提交 PR,描述你为什么要加这个域名

## License

MIT — 详见 [LICENSE](./LICENSE) 文件。