# meta-rules-overseas-saas

海外 SaaS 域名规则集,**完全按业务组粒度拆分**(每个 yaml 对应 OpenClash 一个具体业务组),为 **mihomo / clash-meta / OpenClash** 设计。

## 为什么需要这个仓库?

[meta-rules-dat](https://github.com/metacubex/meta-rules-dat) 是 mihomo 生态最常用的规则集,但它**只包含被 GFW 屏蔽的域名**(geosite-gfw)且按大类聚合(如 `gfw`/`google`/`netflix` 各一个文件),无法精细匹配每个用户的 OpenClash 业务组配置。

本仓库提供**按业务组拆分的域名列表**,每个 yaml 对应一个具体的业务组(如 `chatgpt.yaml` → `ChatGPT` 业务组、`youtube.yaml` → `YouTube` 业务组)。你可以**直接 fork 本仓库,把 yaml 内容内嵌到你的 OpenClash 配置**,无需再手动维护每个业务组的域名补充。

## 使用方式

### 方式 A — 直接内嵌到 OpenClash 自定义规则(推荐)

在 OpenClash → 配置订阅 → 自定义规则 → 直接粘贴:

```yaml
##  ChatGPT 业务组
- DOMAIN-SUFFIX,chat.openai.com,ChatGPT
- DOMAIN-SUFFIX,chatgpt.com,ChatGPT
- DOMAIN-SUFFIX,oaistatic.com,ChatGPT
- DOMAIN-SUFFIX,oaiusercontent.com,ChatGPT
- DOMAIN-SUFFIX,openai.com,ChatGPT
- DOMAIN-SUFFIX,openaiapis.com,ChatGPT
- DOMAIN-KEYWORD,openai,ChatGPT
```

把每个 `*.yaml` 里的 `+.example.com` 替换成 `DOMAIN-SUFFIX,example.com,业务组名` 即可。

### 方式 B — rule-provider 远程加载(原生 yaml 格式)

```yaml
rule-providers:
  chatgpt:
    type: http
    behavior: classical
    format: text
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/chatgpt.yaml"
    path: "./rule_provider/chatgpt.yaml"
    interval: 86400

rules:
  - RULE-SET,chatgpt,  ChatGPT
  - RULE-SET,youtube,  YouTube
  - RULE-SET,telegram, Telegram
```

> **注意**:本仓库 yaml 里的 `+.example.com` 是 clash 的 domain-suffix 匹配语法,直接通过 `format: text` + `behavior: classical` 加载即可,无需预先编译。

### 关于 .mrs

本仓库**不发布编译后的 .mrs 文件**。原计划用 GitHub Actions 编译 .mrs,但 mihomo v1.19+ 的 `convert-ruleset` 不接受带 `+.` 前缀的输入(它要裸 domain),编译出来会是空文件。直接分发 yaml 反而更灵活:

- yaml 可读、可审计、可 fork
- OpenClash / clash-meta / mihomo 都原生支持 yaml 格式的 rule-provider
- 业务组更新只需改 yaml,无需 CI

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
| `microsoft-direct.yaml` | Microsoft 国内可直连服务(Office/Outlook/Bing/OneDrive/Skype/Azure 国内版 等 48 个域名) | 48 |
| `microsoft-store.yaml` | Microsoft Store / Xbox / Sony PSN(国内 IP 被 Microsoft 主动 403,坑 67) | 15 |
| `discord.yaml` | Discord(OpenClash 没独立业务组时可建) | 25 |
| `slack.yaml` | Slack(配合 Discord) | 19 |
| `notion.yaml` | Notion(配合 Discord/Slack) | 20 |
| `wikipedia.yaml` | Wikipedia / Wikimedia 全家桶 | 20 |
| `scholar.yaml` | 学术(Google Scholar / ResearchGate / arXiv / PubMed) | 26 |
| `cloud-ide.yaml` | 云 IDE(Gitpod / StackBlitz / CodeSandbox / Replit) | 13 |
| `pornhub.yaml` | 18+ 类(选择性使用) | 28 |
| `paypal-extra.yaml` | PayPal 子域补充(配合 meta-rules-dat 的 paypal) | 21 |

**总计 32 个 yaml**

**国内直连 (`cn/` 子目录,1 个 yaml):**

仓库名虽是 overseas-saas,但放一个国内白名单方便 OpenClash 一站式引用:

| yaml | 用途 | 条数 |
|---|---|---|
| `cn/国内直连.yaml` | 中国大陆域 + IP 直连白名单 (合并 China / IPs-CN / ChinaMedia / Weibo) | 15872 |

用法跟其他 yaml 一样,只是走 `DIRECT`:

```yaml
rule-providers:
  国内直连:
    type: http
    behavior: direct
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/cn/国内直连.yaml"
    path: "./rule_provider/国内直连.yaml"
    interval: 86400

rules:
  - RULE-SET,国内直连,DIRECT
```

> AntiAD (9.7 MB / 292K 行) **不在本仓库** — 体积过大,改走 README 引用段。

## Microsoft 服务分流(坑 67)

Microsoft 服务一刀切走代理其实没必要 — Office/Outlook/Bing/OneDrive 等国内访问正常,Azure 国内版(世纪互联运营)也是直连更快。本仓库把 Microsoft 拆成两个 yaml:

| yaml | 用途 | 走 |
|---|---|---|
| `microsoft-direct.yaml` | 国内可直连:Office/Outlook/Bing/OneDrive/Skype/Azure 国内版 等 48 个域名 | DIRECT |
| `microsoft-store.yaml` | 必须代理:Microsoft Store / Xbox / Sony PSN(geo-block + 锁区) | 代理业务组 |

### OpenClash 配置示例

```yaml
rule-providers:
  microsoft-direct:
    type: http
    behavior: classical
    format: text
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/microsoft-direct.yaml"
    path: "./rule_provider/microsoft-direct.yaml"
    interval: 86400

  microsoft-store:
    type: http
    behavior: classical
    format: text
    url: "https://raw.githubusercontent.com/teddy4556/meta-rules-overseas-saas/main/microsoft-store.yaml"
    path: "./rule_provider/microsoft-store.yaml"
    interval: 86400

rules:
  # Microsoft 直连服务优先
  - RULE-SET,microsoft-direct,DIRECT
  # Microsoft Store / Xbox / PSN 走代理业务组(你 OpenClash 已有的"Microsoft Store"业务组)
  - RULE-SET,microsoft-store,Microsoft Store
```

> **判断依据**:坑 67 实测 `store.microsoft.com / apps.microsoft.com / displaycatalog.mp.microsoft.com` 国内 IP 返 403/301;Office/Outlook/Bing 等国内 ISP 可直连且速度正常。Azure 国内版(`azure.cn` / `windowsazure.cn`)由世纪互联运营,直连稳定。Azure 国际版不在本仓库范围。

### 仓库结构

```
<业务组>.yaml               # 32 个 yaml,直接 rule-provider 用
cn/国内直连.yaml             # 中国大陆直连白名单 (15872 条)
LICENSE                       # MIT
README.md
```

## 贡献

接受 PR!新增/修改域名时:

1. 编辑对应业务组的 yaml(确保只放该业务组的域名)
2. 保持 `+.` 前缀(clash domain-suffix 匹配语法,直接生效)
3. 提交 PR,描述你为什么要加这个域名

## License

MIT — 详见 [LICENSE](./LICENSE) 文件。
