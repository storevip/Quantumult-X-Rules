# Quantumult X AI 分流规则

一套面向 Quantumult X 的通用 AI 分流方案，用于统一管理常见海外 AI 服务。

支持不同机场、不同订阅以及不同节点命名方式，不需要针对某一家机场单独修改。

## 支持的 AI 服务

目前主要覆盖：

* ChatGPT / OpenAI
* Claude
* Gemini
* Grok
* Manus
* Perplexity
* Microsoft Copilot
* Poe
* Cursor
* GitHub Copilot
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

后续会持续补充新的 AI 服务。

---

# 功能特点

* AI 流量统一交给 `AI` 策略组
* 支持美国、日本、台湾、新加坡、韩国节点
* 自动根据节点名称、国家名称、国旗、英文缩写匹配节点
* 不绑定特定机场
* 更换机场或订阅后仍可继续使用
* AI 域名规则支持 GitHub 远程更新
* 支持手动选择地区或节点

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

---

# 使用方法

## 第一步：添加 AI 策略组

打开：

`AI-Policy.conf`

将其中 `[policy]` 下方的策略内容复制到你自己的 Quantumult X 配置文件 `[policy]` 区域。

注意：

如果你的配置文件本身已经存在：

```text
[policy]
```

不要再次复制 `[policy]` 这一行，只需要复制下面的策略内容。

---

## 第二步：添加 AI 远程分流规则

GitHub 主源：

```text
https://raw.githubusercontent.com/storevip/Quantumult-X-Rules/main/AI.list
```

在 Quantumult X 的：

```text
[filter_remote]
```

中添加：

```text
https://raw.githubusercontent.com/storevip/Quantumult-X-Rules/main/AI.list, tag=AI Rules, force-policy=AI, enabled=true
```

其中：

```text
force-policy=AI
```

表示这份远程规则中的所有 AI 流量统一交给 `AI` 策略组。

---

# 节点自动识别

规则会根据节点名称自动识别常见地区。

例如下面这些名称都可以被自动归类：

```text
🇺🇸 美国 01
美国-洛杉矶
USA Premium
US-01
```

会进入：

```text
🇺🇸 美国节点
```

例如：

```text
🇯🇵 日本东京
Japan 01
JP-02
```

会进入：

```text
🇯🇵 日本节点
```

目前支持：

```text
🇺🇸 美国
🇯🇵 日本
🇹🇼 台湾
🇸🇬 新加坡
🇰🇷 韩国
```

---

# 文件说明

```text
AI.list
```

AI 服务域名分流规则。

```text
AI-Policy.conf
```

Quantumult X AI 策略组模板。

```text
README.md
```

使用说明。

---

# 更新方式

`AI.list` 使用远程规则方式加载。

以后仓库中的 AI 域名规则更新后，Quantumult X 可以重新同步远程资源，无需重新复制全部规则。

---

# 国内镜像

计划提供 Gitee 国内备用源。

如果 GitHub Raw 网络访问不稳定，可使用 Gitee 镜像更新 AI 规则。

---

# License

MIT License

允许自由使用、修改和分享本项目。

---

**一套规则，统一管理常用 AI 服务。**

**不挑机场，换订阅也能继续使用。**
