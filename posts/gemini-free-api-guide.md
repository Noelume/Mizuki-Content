---
title: 接入gemini免费api 攻略
published: 2026-05-04
description: 详细教程：结合我的 Hermes 实际配置，教你如何接入 Gemini 免费 API、设置优雅的多模型回退机制，以及接入第三方中转站的方法。
tags: [Hermes, Gemini, 教程, 配置, AI]
category: 技术
draft: false
---

Gemini 目前提供了极其慷慨的免费 API 额度，是许多开发者个人日常折腾和接入智能体系统（如 Hermes）的绝佳选择。在这篇文章中，我将结合我在 `~/.hermes/` 下的实际配置，带你一步步打磨和配置 Hermes 以完美接入 Gemini 的免费 API。

本文不仅涵盖了最基础的接入方法，还包含为了保证稳定性而设置的**多模型回退机制（Fallback）**，以及针对部分无法直连或者需要统筹调度的场景如何接入**第三方中转站（Proxy）**。

## 1. 基础接入：配置 Gemini API

在 Hermes 中接入模型主要依赖两处配置文件：`.env` 文件保存敏感的 Token 密钥，而 `config.yaml` 负责定义模型调度逻辑。

### 配置 API Key (`.env`)

首先，打开你的终端，编辑或创建 Hermes 的环境变量配置文件：

```bash
vim ~/.hermes/.env
```

在文件中，加入你从 [Google AI Studio](https://aistudio.google.com/) 申请到的免费 API Key：

```env
# LLM PROVIDER (Google AI Studio / Gemini)
GEMINI_API_KEY=你的_GEMINI_API_KEY_放在这里
```

### 指定默认模型 (`config.yaml`)

接下来，我们需要告诉 Hermes 默认使用哪个模型来提供服务。编辑主配置文件：

```bash
vim ~/.hermes/config.yaml
```

找到 `model` 节点，并按照如下方式配置：

```yaml
model:
  default: gemini-3.1-pro-preview # 设置 Gemini Pro 作为默认的高智商模型
  provider: gemini
  base_url: https://generativelanguage.googleapis.com/v1beta
```

这样你的 Hermes 就会默认唤起能力最强的 Gemini 模型来完成日常的任务规划与代码生成了。

## 2. 稳定性保障：多模型回退机制 (Fallback)

虽然 Gemini 提供了免费额度，但是免费版存在一定的并发请求限制（Rate Limit）。如果我们高频请求，或者遇到 Google API 偶发的波动，Hermes 可能会罢工。

为了避免这种情况，Hermes 提供了一个极其优雅的功能：**多模型回退机制（Fallback Providers）**。它的逻辑是：当默认模型请求失败或超时，自动无缝切换到备用模型继续执行，对用户侧来说几乎是无感的。

继续在你的 `~/.hermes/config.yaml` 中，添加或修改 `fallback_providers` 节点：

```yaml
fallback_providers:
  # 第一顺位回退：如果 Pro 版本限流，回退到更轻量、速度更快的 Flash Lite 版本
  - provider: gemini
    model: gemini-3.1-flash-lite-preview
    base_url: https://generativelanguage.googleapis.com/v1beta
  
  # 第二顺位回退：如果整个 Gemini 服务不可用，切换到其他的可用模型（例如 OpenAI Codex）
  - provider: openai-codex
    model: gpt-5.5
    base_url: https://chatgpt.com/backend-api/codex
```

这种“**旗舰模型 -> 竞速模型 -> 异构厂商模型**”的梯队设计，是目前兼顾成本和稳定性的最佳实践。

## 3. 进阶玩法：接入第三方中转站

很多时候由于网络限制，我们无法直接连通 `generativelanguage.googleapis.com`；或者我们与朋友拼车，使用的是第三方的 OneAPI/NewAPI 等中转分发站点。

要在 Hermes 中修改请求地址并走第三方中转，非常简单，只需要修改 `base_url`。

### 方法一：全局通过环境变量修改（推荐）

在 `~/.hermes/.env` 中，你可以通过指定 `GEMINI_BASE_URL` 全局覆盖所有 Gemini 驱动的请求地址。很多国内的中转商会提供与官方兼容的代理地址或 OpenAI 格式的接口地址。

```env
# 填入第三方中转站的地址
GEMINI_BASE_URL=https://api.your-proxy-domain.com/v1beta
# 此时 API Key 也要换成你在该中转站获取的令牌
GEMINI_API_KEY=sk-xxxxxx中转站令牌xxxxxx
```

### 方法二：在 YAML 中为特定模型指定中转

如果你既想用官方直连的 Gemini，又想在某个特定的回退模型里走中转站，你可以单独在 `config.yaml` 对应配置下的 `base_url` 做修改：

```yaml
model:
  default: gemini-3.1-pro-preview
  provider: gemini
  base_url: https://api.your-proxy-domain.com/v1beta # 这里改写成第三方中转站地址
```

**⚠️ 注意事项：**
如果你的中转站是完全套壳成标准的 OpenAI API 格式（即以 `/v1` 结尾并期望调用 `chat/completions`），那么你需要将 provider 设置为 `openai`，并在环境变量中配置对应的 `OPENAI_API_KEY` 和 `OPENAI_BASE_URL`，同时 `model` 名称填写中转站映射的模型名称（如 `gemini-pro`）。

## 总结

经过这三步，你的 Hermes 已经变成了一台“既能白嫖 Gemini 强大算力，又兼具金刚不坏之身”的本地智能体核心了。

通过合理设置**默认模型（Pro）**和**回退层（Flash/其它模型）**，你不仅最大化利用了免费 API 资源，也极大提高了工具链的可用性。而中转站的支持，则帮你扫清了最后一点网络连通性的障碍。

去试试这套配置吧！享受由 Gemini 强劲驱动的 AI 工作流。
