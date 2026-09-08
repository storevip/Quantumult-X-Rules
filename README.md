# Quantumult X 海外 AI 分流规则

一套面向 **Quantumult X** 的海外 AI 精准分流方案。它把常用海外 AI 的官网、客户端、专用 API、静态资源和必要网络端点统一交给独立的 `AI` 策略组，同时尽量不影响普通 Google、GitHub、Microsoft、AWS、Cloudflare 或其他应用流量。

当前版本包含 **266 条规则**，覆盖 80 余个海外 AI 产品与相关服务。

## 设计原则

- **仅分流海外 AI**：不包含 DeepSeek、Kimi、Qwen、GLM、MiniMax、豆包等中国大陆 AI。
- **核心域名优先**：优先收录产品自有域名、专用 API、专用 CDN 和明确归属的网络段。
- **低误伤**：不因为某个 AI 使用了公共第三方服务，就把整个第三方平台归入 AI。
- **关键功能完整**：补齐 ChatGPT/Codex/Sora、Claude Code/MCP、Gemini/NotebookLM、Grok、GitHub Copilot、Amazon Q 等常用产品的关键端点。
- **策略与规则分离**：规则持续远程更新，地区节点仍由用户在 `AI` 策略组中手动选择。

## 覆盖范围

| 类别 | 代表服务 |
| --- | --- |
| 核心助手 | ChatGPT、Claude、Gemini、Grok、Microsoft Copilot、GitHub Copilot |
| 模型与 API | OpenAI API、Anthropic API、Google AI Studio、Mistral、Groq、Cohere、Cerebras、OpenRouter、Together AI、Fireworks AI、Replicate、DeepInfra、Novita AI |
| AI 编程与 Agent | Codex、Claude Code、Cursor、Windsurf、Amazon Q、JetBrains AI、Cline、Continue、Sourcegraph Cody/Amp、Devin、Augment Code、CodeRabbit、Kiro、Kilo、v0、Bolt、Lovable、Replit、Tabnine、Zed |
| 搜索与效率 | Perplexity、Poe、You.com、Genspark、Character.AI、Duck.ai、Meta AI、Pi、Manus、Gamma、Grammarly、Otter.ai、Jasper |
| 图片与设计 | Midjourney、Adobe Firefly、FLUX、Stability AI、Ideogram、Leonardo AI、Recraft、OpenArt、Civitai、ClipDrop、ComfyUI |
| 视频与音频 | Runway、Pika、Luma、HeyGen、Synthesia、Suno、Udio、ElevenLabs、Descript |
| 开源与开发生态 | Hugging Face、Ollama、LM Studio、LangChain、CrewAI、Dify、LMArena、H2O.ai、Nous Research |

TRAE、Coze、MarsCode、Cici 和 Qoder 只收录其全球版/海外版核心域名；对应的中国版域名以及共享的字节跳动基础设施不在规则中。

## 明确排除的中国大陆 AI

本项目不会收录以下服务的中国大陆端点：

- DeepSeek
- Kimi / Moonshot
- 通义千问 / Qwen / 阿里云百炼
- 智谱 AI / GLM / Z.ai
- MiniMax / 海螺 AI
- 豆包 / 火山方舟
- 腾讯混元 / 元宝
- 百度文心 / 千帆
- 硅基流动、阶跃星辰、魔搭、PPIO、小米 MiMo
- Coze、TRAE、MarsCode、Qoder 的中国版

如果你的其他规则把国内流量设为直连，这些服务会继续按照原有国内规则处理，不会被本项目送入 `AI` 策略组。

## 为什么不直接照搬“大而全”规则

很多 AI 产品会使用公共支付、登录、监控、客服或云存储平台。把这些平台的整个主域名加入 AI 规则，会导致大量无关 App 误走 AI 节点。

因此，本项目不会使用下列宽泛规则：

```text
stripe.com
auth0.com
sentry.io
intercom.io
storage.googleapis.com
api.cloudflare.com
static.cloudflareinsights.com
segment.io
launchdarkly.com
```

对于确实专属于某个 AI 的租户或对象存储，只使用完整主机名，例如：

```text
anthropic.auth0.com
openaiassets.blob.core.windows.net
copilot-proxy.githubusercontent.com
ppl-ai-file-upload.s3.amazonaws.com
```

这种方式能保留关键功能，又不会把整个 Auth0、Azure Blob、GitHubusercontent 或 Amazon S3 粗暴划入 AI。

Cloudflare Workers AI 的公开 API 与普通 Cloudflare 管理 API 共用 `api.cloudflare.com`，Quantumult X 的域名规则无法按 URL 路径区分。因此本项目只收录 `ai.cloudflare.com`，不收录公共 API 主机；这是为了避免误伤而作出的明确取舍。

## 使用方法

### 1. 添加 AI 策略组

打开 [`AI-Policy.conf`](./AI-Policy.conf)，将 `[policy]` 下方的内容复制到自己的 Quantumult X 配置 `[policy]` 区域。

如果配置中已经存在 `[policy]`，不要重复复制这一行。

默认策略结构为：

```text
AI
├── 🇺🇸 美国节点
├── 🇯🇵 日本节点
├── 🇹🇼 台湾节点
├── 🇸🇬 新加坡节点
└── 🇰🇷 韩国节点
```

每个地区组会根据节点名称中的国旗、中文名、英文名或常见缩写自动筛选节点。`AI` 不包含 Quantumult X 内置的 `proxy`，因此不会跟随全局代理选择一起变化。

### 2. 添加远程规则

在 Quantumult X 配置的 `[filter_remote]` 区域加入：

```text
https://raw.githubusercontent.com/storevip/Quantumult-X-Rules/main/AI.list, tag=Overseas AI Rules, force-policy=AI, enabled=true
```

`AI.list` 每条规则本身已经带有 `AI` 策略，`force-policy=AI` 仍建议保留，这样即使以后调整规则文件，远程资源也会稳定使用同一个策略。

### 3. 注意规则顺序

Quantumult X 按顺序匹配，命中后停止。请将这份 AI 规则放在宽泛的 Google、GitHub、Microsoft、X/Twitter 和全球代理规则之前，确保以下专用端点不会先被普通服务规则截走：

```text
Gemini / NotebookLM  → 普通 Google 之前
GitHub Copilot       → 普通 GitHub 之前
Grok                 → 普通 X/Twitter 之前
Microsoft Copilot    → 普通 Microsoft/Bing 之前
```

推荐的总体顺序：

```text
局域网 / 必须直连的应用
↓
海外 AI（本项目）
↓
广告拦截
↓
普通 Google / GitHub / Microsoft / X / Telegram
↓
中国大陆域名与 IP 直连
↓
最终兜底策略
```

## ChatGPT Voice 与网络规则

除域名外，`AI.list` 还包含：

- OpenAI 与 Anthropic 明确归属的 IPv4、IPv6 和 ASN 规则
- OpenAI 官方 `chatgpt-voice.json` 当前列出的 ChatGPT Voice 专用 IP
- 所有 IP 规则均使用 `no-resolve`，避免额外 DNS 查询

ChatGPT Voice IP 会变化，仓库维护时应以 [OpenAI 官方实时文件](https://openai.com/chatgpt-voice.json) 为准。当前快照生成时间为 **2026-03-26**。

## 文件说明

- [`AI.list`](./AI.list)：海外 AI 域名、专用 API/CDN、IP 与 ASN 分流规则。
- [`AI-Policy.conf`](./AI-Policy.conf)：美国、日本、台湾、新加坡、韩国五个地区组及 `AI` 总策略模板。
- [`README.md`](./README.md)：安装、覆盖范围、排除范围与设计取舍。

## 更新与检查原则

新增规则时应满足至少一项：

1. AI 服务自己的主域名或官方产品域名。
2. 官方文档明确列出的 AI 专用端点。
3. 能从主机名明确判断只服务于该 AI 的 API、CDN、上传或附件端点。
4. 明确归属于 AI 提供商的网络段，且使用 `no-resolve`。

下列情况原则上不加入：

1. 只有 URL 路径能区分 AI 与普通业务的共享主机。
2. 支付、登录、监控、客服、分析或云存储平台的整个公共主域名。
3. 大型云厂商、CDN 或托管商的宽泛 ASN/IP 段。
4. 无法确认用途、归属或仍在使用的历史域名。

## 参考来源

规则经过筛选、去重并按低误伤原则重新组织，主要参考：

- [OpenAI：ChatGPT 网络建议](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps)
- [OpenAI：ChatGPT Voice IP](https://openai.com/chatgpt-voice.json)
- [GitHub：Copilot allowlist reference](https://docs.github.com/en/copilot/reference/copilot-allowlist-reference)
- [AWS：Amazon Q Developer firewall allowlist](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/firewall.html)
- [VPSDance/ai-proxy-rules](https://github.com/VPSDance/ai-proxy-rules)

第三方规则来源的许可与声明见 [`THIRD-PARTY-NOTICES.md`](./THIRD-PARTY-NOTICES.md)。

## 隐私、安全与免责声明

本项目只提供 Quantumult X 规则和策略组模板，不建立代理服务器，不收集、上传或记录用户数据，也不需要订阅链接、节点密码、账号、API Key、Cookie 或 Token。

请勿把机场订阅、节点认证信息、私人配置、API Key、Cookie、Token 或账号密码上传到公开仓库。

规则只能决定流量交给哪个策略，不能保证代理节点一定满足某个 AI 的地区、IP 质量、账号地区或风控要求。如果服务无法使用，请先在 `AI → 地区策略 → 具体节点` 中更换节点。

本项目与 OpenAI、Anthropic、Google、xAI、Microsoft、GitHub、Amazon 等公司不存在官方关联。产品名与商标归各自权利人所有。

## License

本项目采用 [MIT License](./LICENSE)。
