# meta-rules-overseas-saas

海外 SaaS 域名规则集,为 **mihomo / clash-meta / OpenClash** 设计 — 收录 meta-rules-dat 不包含的、但在中国大陆访问需要代理的 SaaS 服务。

## 为什么需要这个仓库?

[meta-rules-dat](https://github.com/metacubex/meta-rules-dat) 是 mihomo 生态最常用的规则集,但它**只包含被 GFW 屏蔽的域名**(geosite-gfw)。大量海外 SaaS 在国内"未墙但不稳定"或"走代理更稳",meta-rules-dat 没收录:

| 服务 | 例子 |
|---|---|
| **协作** | Slack, Notion, Figma, Linear, Asana, ClickUp, Monday |
| **开发者** | Vercel, Netlify, Railway, Fly.io, Render, Replit |
| **设计** | Figma, Canva, Framer, Webflow, Sketch Cloud |
| **AI** | Claude, ChatGPT, Perplexity, HuggingFace, Replicate |
| **监控** | Datadog, Sentry, BetterStack, Grafana Cloud |
| **数据库** | Supabase, Neon, PlanetScale, MongoDB Atlas |

这些服务**走代理后稳定性和速度都显著提升**。本仓库收录它们,**完全兼容 meta-rules-dat `.mrs` 格式**,可以直接在 OpenClash / clash-meta 中作为 `rule-provider` 使用。

## 使用方式(OpenClash 示例)

```yaml
rule-providers:
  overseas-saas:
    type: http
    behavior: domain
    format: mrs
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/overseas-saas.mrs"
    path: "./rule_provider/overseas-saas.mrs"
    interval: 86400

rules:
  - RULE-SET,overseas-saas, 其他-手选  # 走 url-test 业务组
  # 或者:
  - RULE-SET,overseas-saas, 美国-手选  # 走美国手选节点
```

## 规则集结构

```
overseas-saas.yaml   # 源文件(可读、易审阅)
overseas-saas.mrs    # 编译后二进制(MRS,体积小,加载快)
LICENSE              # MIT
.github/workflows/
  release.yml        # GitHub Actions 自动编译 .mrs
```

## 域名分类

按服务类型分块(每块以注释开头,方便 PR 时定位):

| 类别 | 包含 |
|---|---|
| `ai-tools` | Claude / ChatGPT / Perplexity / HuggingFace / Replicate / Cursor / Cody |
| `collaboration` | Slack / Notion / Figma / Linear / Asana / ClickUp / Monday / Trello / Miro |
| `developers` | Vercel / Netlify / Railway / Fly.io / Render / Replit / Gitpod / StackBlitz / CodeSandbox |
| `design` | Figma / Canva / Framer / Webflow / Sketch Cloud / InVision |
| `monitoring` | Datadog / Sentry / BetterStack / Grafana Cloud / PagerDuty / New Relic |
| `database` | Supabase / Neon / PlanetScale / MongoDB Atlas / Upstash / Prisma |
| `auth` | Auth0 / Clerk / Firebase / Supabase Auth |

## 贡献

接受 PR!新增域名时请:

1. 确认该域名在中国大陆**走代理比直连更稳**(可用[Globalping](https://globalping.io/)测速)
2. 编辑 `overseas-saas.yaml`,放到对应分类
3. 保持 `+.` 前缀(mihomo domain suffix 匹配)
4. 提交 PR,描述你为什么要加这个域名

## License

MIT — 详见 [LICENSE](./LICENSE) 文件。