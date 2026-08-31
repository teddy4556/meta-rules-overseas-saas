# meta-rules-overseas-saas

海外 SaaS 域名规则集,为 **mihomo / clash-meta / OpenClash** 设计 — 收录 meta-rules-dat 不包含的、但在中国大陆访问需要代理的 SaaS 服务。

## 为什么需要这个仓库?

[meta-rules-dat](https://github.com/metacubex/meta-rules-dat) 是 mihomo 生态最常用的规则集,但它**只包含被 GFW 屏蔽的域名**(geosite-gfw)。大量海外 SaaS 在国内"未墙但不稳定"或"走代理更稳",meta-rules-dat 没收录:

| 服务 | 例子 |
|---|---|
| **AI** | Claude, ChatGPT, Perplexity, HuggingFace, Cursor |
| **协作** | Slack, Notion, Figma, Linear, Asana, ClickUp |
| **开发者** | Vercel, Netlify, Railway, npm, PyPI |
| **设计** | Figma, Canva, Framer, Webflow |
| **监控** | Datadog, Sentry, BetterStack |
| **数据库** | Supabase, Neon, PlanetScale |
| **支付** | Stripe, PayPal |

这些服务**走代理后稳定性和速度都显著提升**。本仓库收录它们,**完全兼容 meta-rules-dat `.mrs` 格式**。

## 使用方式(OpenClash 示例)

本仓库**按服务类别拆分**,你可以**独立引用每个类别**:

```yaml
rule-providers:
  overseas-ai:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/ai-tools.mrs"
    path: "./rule_provider/ai-tools.mrs"
    interval: 86400

  overseas-collab:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/collaboration.mrs"
    path: "./rule_provider/collaboration.mrs"
    interval: 86400

  overseas-dev:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/developer-platforms.mrs"
    path: "./rule_provider/developer-platforms.mrs"
    interval: 86400

rules:
  - RULE-SET,overseas-ai,    其他-手选
  - RULE-SET,overseas-collab, 其他-手选
  - RULE-SET,overseas-dev,   美国-手选     # 开发者平台首选美国
```

## 可用的规则集(按类别)

| 文件 | 类别 | 包含示例 |
|---|---|---|
| `ai-tools.mrs` | AI 工具 | Claude, ChatGPT, Perplexity, HuggingFace, Cursor, Midjourney, Suno |
| `collaboration.mrs` | 协作工具 | Slack, Notion, Figma, Linear, Asana, ClickUp, Miro, Dropbox, Zoom, Teams |
| `developer-platforms.mrs` | 开发者平台 | Vercel, Netlify, Railway, npm, PyPI, Docker Hub, Buildkite, CircleCI |
| `design.mrs` | 设计工具 | Canva, Framer, Webflow, Sketch, Dribbble, Behance |
| `monitoring.mrs` | 监控运维 | Datadog, Sentry, BetterStack, Grafana, PagerDuty, NewRelic |
| `database.mrs` | 数据库 | Supabase, Neon, PlanetScale, MongoDB Atlas, Upstash, Prisma |
| `auth.mrs` | 认证服务 | Auth0, Clerk, Firebase, Okta, OneLogin |
| `misc.mrs` | 其他 | Stripe, PayPal, Twilio, Cloudflare CDN, Akamai |

## 仓库结构

```
ai-tools.yaml             # 源文件(可读、易审阅)
ai-tools.mrs              # 编译后二进制(MRS)
collaboration.yaml
collaboration.mrs
developer-platforms.yaml
developer-platforms.mrs
design.yaml
design.mrs
monitoring.yaml
monitoring.mrs
database.yaml
database.mrs
auth.yaml
auth.mrs
misc.yaml
misc.mrs
LICENSE                   # MIT
README.md
.github/workflows/
  build-mrs.yml           # GitHub Actions 自动编译 8 个 .mrs
```

## 贡献

接受 PR!新增域名时请:

1. 确认该域名在中国大陆**走代理比直连更稳**
2. 编辑对应类别的 yaml 文件(放进最匹配的类别)
3. 保持 `+.` 前缀(mihomo domain suffix 匹配)
4. 提交 PR,描述你为什么要加这个域名

## License

MIT — 详见 [LICENSE](./LICENSE) 文件。