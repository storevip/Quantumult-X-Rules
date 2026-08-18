# Quantumult X AI 分流规则

一套适用于 **Quantumult X** 的通用 AI 分流方案，用于统一管理常见海外 AI 服务的网络访问。

支持不同机场、不同订阅以及不同节点命名方式，不依赖某一家机场，更换订阅后也可以继续使用。

---

## 🤖 支持的 AI 服务

目前主要覆盖：

* ChatGPT / OpenAI
* Claude / Anthropic
* Gemini / Google AI
* Grok / xAI
* Manus
* Perplexity
* Microsoft Copilot
* GitHub Copilot
* Poe
* Cursor
* Hugging Face
* Midjourney
* Runway
* Stability AI
* ElevenLabs
* Windsurf / Codeium
* v0
* Lovable
* Bolt
* Gamma

后续会持续补充新的海外 AI 服务及相关域名。

---

## ✨ 功能特点

* AI 流量统一交给独立的 `AI` 策略组
* 支持美国、日本、台湾、新加坡、韩国节点
* 自动根据节点名称、国家名称、国旗、英文缩写匹配节点
* 不绑定特定机场或订阅
* 更换机场或订阅后仍可继续使用
* 支持手动选择 AI 使用地区
* AI 域名规则支持 GitHub 远程更新
* 无需每次手动重新复制 AI 域名
* 配置结构简单，方便自行修改和扩展

最终策略结构类似：

```text
AI
├── 🇺🇸 美国节点
├── 🇯🇵 日本节点
├── 🇹🇼 台湾节点
├── 🇸🇬 新加坡节点
├── 🇰🇷 韩国节点
└── proxy
```

进入对应地区策略后，可以继续手动选择机场中自动匹配到的具体节点。

---

# 📦 使用方法

## 第一步：添加 AI 策略组

打开仓库中的：

```text
AI-Policy.conf
```

将其中 `[policy]` 下方的策略内容复制到你自己的 Quantumult X 配置文件中的：

```text
[policy]
```

区域。

### 注意

如果你的 Quantumult X 配置中本身已经存在：

```text
[policy]
```

请不要再次复制 `[policy]` 这一行。

只需要复制其下方的策略组内容即可。

---

## 第二步：添加 AI 远程分流规则

GitHub 远程规则地址：

```text
https://raw.githubusercontent.com/storevip/Quantumult-X-Rules/main/AI.list
```

在 Quantumult X 配置文件的：

```text
[filter_remote]
```

区域中加入：

```text
https://raw.githubusercontent.com/storevip/Quantumult-X-Rules/main/AI.list, tag=AI Rules, force-policy=AI, enabled=true
```

其中：

```text
force-policy=AI
```

表示将这份远程规则匹配到的 AI 流量统一交给：

```text
AI
```

策略组管理。

---

# 🌎 节点自动识别

本项目会根据机场节点名称自动识别常见国家和地区节点。

目前支持：

```text
🇺🇸 美国
🇯🇵 日本
🇹🇼 台湾
🇸🇬 新加坡
🇰🇷 韩国
```

例如机场存在：

```text
🇺🇸 美国 01
美国-洛杉矶
USA Premium
US-01
```

这些节点会自动进入：

```text
🇺🇸 美国节点
```

如果存在：

```text
🇯🇵 日本东京
Japan 01
JP-02
```

则会自动进入：

```text
🇯🇵 日本节点
```

同理也会自动识别台湾、新加坡和韩国节点。

节点匹配会尽量兼容：

* 国旗 Emoji
* 中文国家名称
* 繁体中文名称
* 英文国家名称
* 常见英文缩写

因此即使更换机场，只要节点名称包含常见地区标识，通常无需重新修改策略组。

---

# 📂 文件说明

## `AI.list`

AI 服务域名分流规则。

主要负责识别 ChatGPT、Claude、Gemini、Grok、Manus、Perplexity 等 AI 服务产生的网络请求。

---

## `AI-Policy.conf`

Quantumult X AI 策略组模板。

负责创建：

```text
AI
```

以及：

```text
🇺🇸 美国节点
🇯🇵 日本节点
🇹🇼 台湾节点
🇸🇬 新加坡节点
🇰🇷 韩国节点
```

等策略组，并根据机场节点名称自动进行筛选。

---

## `README.md`

项目介绍及安装使用教程。

---

## `LICENSE`

本项目使用 MIT License。

---

# 🔄 规则更新

`AI.list` 采用 GitHub 远程规则方式加载。

用户第一次配置完成后，后续如果本仓库增加新的 AI 服务或域名，只需要在 Quantumult X 中重新同步远程资源即可。

无需重新复制整套 AI 分流规则。

GitHub 主源：

```text
https://raw.githubusercontent.com/storevip/Quantumult-X-Rules/main/AI.list
```

---

# 🔐 隐私与安全

本项目仅提供 Quantumult X 的 AI 分流规则与策略组模板。

**不会收集、上传、记录或存储任何用户数据。**

使用本项目：

* 不需要提供 Quantumult X 私人配置文件
* 不需要提供机场订阅地址
* 不需要提供代理节点信息
* 不需要提供节点密码
* 不需要提供 OpenAI / ChatGPT 账号
* 不需要提供 Claude 账号
* 不需要提供 Gemini 账号
* 不需要提供其他 AI 服务账号
* 不需要提供 API Key
* 不需要提供 Cookie
* 不需要提供 Token
* 不包含数据统计功能
* 不包含用户追踪功能
* 不包含广告
* 不记录用户访问过哪些 AI 服务
* 不会将用户流量发送到项目作者服务器

本项目所有规则均公开托管于 GitHub，可以自行查看、检查和修改。

---

## ⚠️ 安全提醒

请勿将以下私人信息上传到 GitHub 或分享给其他人：

* 机场订阅链接
* 代理节点密码
* 节点认证信息
* API Key
* Cookie
* Token
* 私人代理服务器信息
* Quantumult X 完整私人配置
* 各类账号密码
* 其他个人敏感信息

尤其需要注意：

**机场订阅链接通常包含私人认证信息，请勿将自己的完整订阅地址上传到公开 GitHub 仓库。**

---

# 🛡️ 工作原理

本项目不会建立任何代理服务器。

`AI.list` 仅用于告诉 Quantumult X：

```text
哪些域名属于 AI 服务
```

并将匹配到的流量交给：

```text
AI
```

策略组处理。

实际网络连接依然由用户自己的：

```text
Quantumult X
↓
用户选择的代理节点
↓
目标 AI 服务
```

完成。

项目作者无法查看用户实际使用的代理节点、访问内容、账号信息或网络流量。

---

# ⚠️ 免责声明

本项目仅用于 Quantumult X 网络分流配置。

不同 AI 服务可能存在：

* 地区限制
* IP 地区限制
* 节点质量要求
* 风控策略
* 账号地区限制
* 服务可用地区限制
* 网络环境检测

因此本项目只能负责：

**将对应 AI 服务的网络请求交给指定代理策略。**

不能保证某一个代理节点一定能够正常使用：

* ChatGPT
* Claude
* Gemini
* Grok
* Manus
* Perplexity
* 或其他 AI 服务

如果某个 AI 服务无法正常访问，建议优先尝试更换：

```text
AI → 国家策略 → 具体节点
```

不同 AI 服务对代理 IP 的要求可能不同。

本项目与 OpenAI、Anthropic、Google、xAI、Manus、Perplexity、Microsoft 等公司不存在任何官方关联。

所有产品名称、商标及服务名称归其各自权利人所有。

使用本项目产生的网络访问行为由用户自行负责。

---

# 📜 License

本项目采用：

```text
MIT License
```

允许在遵守许可证条款的前提下自由：

* 使用
* 复制
* 修改
* 分发

本项目主要用于规则分享与学习交流。

---

## ❤️ 关于项目

如果发现某个 AI 服务无法正确分流，或者有新的 AI 服务需要添加，可以通过 GitHub Issue 提交反馈。

后续会持续补充和维护 AI 服务域名。

---

**一套规则，统一管理常用 AI 服务。**

**不挑机场，换订阅也能继续使用。**
