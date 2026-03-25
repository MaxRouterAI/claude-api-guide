# Claude API 国内使用完整教程（2026 最新）

> Claude 是目前代码能力最强的大模型之一，在编程、推理、长文本理解方面表现突出。这篇文章介绍如何在国内通过 API 调用 Claude 模型，涵盖项目集成、开发工具配置、Claude Code 使用等场景。

---

## Claude 模型家族

Anthropic 的 Claude 4 系列是很多开发者的首选编程模型：

| 模型 | 上下文 | 特点 | 适用场景 |
|------|--------|------|----------|
| Claude Opus 4 | 200K | 最强推理和编程能力 | 复杂架构设计、大型重构、深度分析 |
| Claude Sonnet 4 | 200K | 速度与质量的最佳平衡 | 日常编程、代码审查、功能开发 |
| Claude Sonnet 4 (thinking) | 200K | 带思维链推理 | 复杂调试、多步逻辑推理 |
| Claude Haiku 4 | 200K | 快速响应，成本最低 | 简单任务、代码补全、快速问答 |
| Claude Opus 4.5 | 200K | 上一代旗舰 | 仍然很强，价格略低 |
| Claude Sonnet 4.5 | 1M | 超长上下文版本 | 大型项目分析 |

Claude 在代码理解、重构、bug 分析方面的表现，目前是所有大模型中最好的。这也是为什么 Claude Code 能成为最火的 AI 编程工具。

## 使用 Claude 的几种方式

### 1. Claude Code（终端编程工具）

这是目前最火的使用方式。Claude Code 直接在终端里运行，能理解你的整个代码库，自主编写代码、执行命令、管理 Git。

👉 详细教程：[Claude Code 国内使用完整教程](https://github.com/MaxRouterAI/claude-code-guide)

### 2. API 集成（在自己的项目中调用）

通过 API 在你的应用中集成 Claude 的能力 — 智能客服、代码生成、文档分析等。

### 3. 开发工具（Cursor、Cline、Continue 等）

在 IDE 中使用 Claude 模型辅助编程。

本文重点介绍后两种方式。

## 官方 API 使用方式

Anthropic 提供两种 API 格式：

**Anthropic 原生格式：**
- 端点：`api.anthropic.com`
- 认证：`x-api-key` 头
- 消息格式：`/v1/messages`

**OpenAI 兼容格式（第三方支持）：**
- 很多中转服务同时支持 OpenAI SDK 格式调用 Claude
- 对已有 OpenAI 集成的项目，改一行 `base_url` 就能切换到 Claude

## 国内的问题

跟所有海外 AI 服务一样：

- `api.anthropic.com` 国内无法直接访问
- 注册需要海外手机号和信用卡
- 虚拟卡封号率高，Anthropic 风控越来越严
- 代理工具不稳定，API 调用对网络质量要求高

详细分析见 [Claude Code 教程的相关章节](https://github.com/MaxRouterAI/claude-code-guide#国内使用的现实问题)。

## 我的方案：通过 MaxRouter 中转

我用 [MaxRouter](https://www.maxrouter.ai) 作为 API 中转，支持所有 Claude 模型，与官方同价，按量计费，国内直连。

同一个 Key 还能调用 GPT、Gemini、DeepSeek 等 80+ 模型。注册送 ¥10 体验金。

### 获取 API Key

1. 访问 [www.maxrouter.ai](https://www.maxrouter.ai) 注册
2. 控制台 → 密钥管理 → 创建密钥 → 复制 Key

## 在项目中集成 Claude API

### Python

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-maxrouter-key",
    base_url="https://api.maxrouter.ai/v1"
)

# 基本对话
response = client.chat.completions.create(
    model="claude-sonnet-4",
    messages=[
        {"role": "system", "content": "你是一个资深 Python 开发者"},
        {"role": "user", "content": "用 Python 实现一个线程安全的 LRU 缓存"}
    ]
)
print(response.choices[0].message.content)
```

### Python 流式输出

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-key",
    base_url="https://api.maxrouter.ai/v1"
)

stream = client.chat.completions.create(
    model="claude-sonnet-4",
    messages=[{"role": "user", "content": "详细解释 Python 的 GIL 机制"}],
    stream=True
)

for chunk in stream:
    content = chunk.choices[0].delta.content
    if content:
        print(content, end="", flush=True)
```

### Node.js

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
    apiKey: 'sk-your-key',
    baseURL: 'https://api.maxrouter.ai/v1'
});

const response = await client.chat.completions.create({
    model: 'claude-sonnet-4',
    messages: [
        { role: 'system', content: '你是一个 TypeScript 专家' },
        { role: 'user', content: '实现一个类型安全的事件总线' }
    ]
});
console.log(response.choices[0].message.content);
```

### Node.js 流式输出

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
    apiKey: 'sk-your-key',
    baseURL: 'https://api.maxrouter.ai/v1'
});

const stream = await client.chat.completions.create({
    model: 'claude-sonnet-4',
    messages: [{ role: 'user', content: '写一个 WebSocket 聊天服务器' }],
    stream: true
});

for await (const chunk of stream) {
    const content = chunk.choices[0]?.delta?.content || '';
    process.stdout.write(content);
}
```

### Java

```java
// 使用 OpenAI Java SDK
OpenAiService service = new OpenAiService(
    "sk-your-key",
    Duration.ofSeconds(60),
    "https://api.maxrouter.ai/"
);

ChatCompletionRequest request = ChatCompletionRequest.builder()
    .model("claude-sonnet-4")
    .messages(List.of(
        new ChatMessage("user", "用 Java 实现一个简单的依赖注入容器")
    ))
    .build();

ChatCompletionResult result = service.createChatCompletion(request);
System.out.println(result.getChoices().get(0).getMessage().getContent());
```

### Go

```go
package main

import (
    "context"
    "fmt"
    openai "github.com/sashabaranov/go-openai"
)

func main() {
    config := openai.DefaultConfig("sk-your-key")
    config.BaseURL = "https://api.maxrouter.ai/v1"
    client := openai.NewClientWithConfig(config)

    resp, err := client.CreateChatCompletion(
        context.Background(),
        openai.ChatCompletionRequest{
            Model: "claude-sonnet-4",
            Messages: []openai.ChatCompletionMessage{
                {Role: "user", Content: "用 Go 写一个高性能的 HTTP 路由器"},
            },
        },
    )
    if err != nil {
        panic(err)
    }
    fmt.Println(resp.Choices[0].Message.Content)
}
```

### cURL

```bash
curl https://api.maxrouter.ai/v1/chat/completions \
  -H "Authorization: Bearer sk-your-key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-sonnet-4",
    "messages": [{"role": "user", "content": "你好"}],
    "stream": false
  }'
```

## Anthropic 原生格式

如果你用 Claude Code 或 Anthropic 官方 SDK，设置环境变量即可：

```bash
# ~/.zshrc 或 ~/.bashrc
export ANTHROPIC_BASE_URL=https://api.maxrouter.ai
export ANTHROPIC_API_KEY=sk-your-maxrouter-key
```

> ⚠️ `ANTHROPIC_BASE_URL` **不带 `/v1`**，Claude SDK 会自动拼接路径。

## 开发工具配置

### Claude Code

终端 AI 编程工具，详细教程：[Claude Code 国内使用完整教程](https://github.com/MaxRouterAI/claude-code-guide)

```bash
export ANTHROPIC_BASE_URL=https://api.maxrouter.ai
export ANTHROPIC_API_KEY=sk-your-key
claude
```

### Cursor

1. 打开 Cursor 设置 → Models
2. Override OpenAI Base URL: `https://api.maxrouter.ai/v1`
3. API Key: `sk-your-key`
4. 模型选择: `claude-sonnet-4`

Cursor 是目前最流行的 AI IDE，配合 Claude 模型效果很好。日常编程用 Sonnet 4，复杂任务切 Opus 4。

### Cline（VS Code 插件）

Cline 是 VS Code 上最强的 AI 编程插件之一：

1. 安装 Cline 插件
2. 设置 → API Provider → OpenAI Compatible
3. Base URL: `https://api.maxrouter.ai/v1`
4. API Key: `sk-your-key`
5. 模型: `claude-sonnet-4`

### Continue（VS Code / JetBrains）

编辑 `~/.continue/config.json`：

```json
{
  "models": [{
    "title": "Claude Sonnet 4",
    "provider": "openai",
    "model": "claude-sonnet-4",
    "apiKey": "sk-your-key",
    "apiBase": "https://api.maxrouter.ai/v1"
  }, {
    "title": "Claude Opus 4",
    "provider": "openai",
    "model": "claude-opus-4",
    "apiKey": "sk-your-key",
    "apiBase": "https://api.maxrouter.ai/v1"
  }]
}
```

### ChatBox

1. 设置 → API Provider → OpenAI API Compatible
2. API Host: `https://api.maxrouter.ai`
3. API Key: `sk-your-key`
4. 模型: `claude-sonnet-4`

### NextChat（ChatGPT Next Web）

1. 设置 → 接口地址: `https://api.maxrouter.ai`
2. API Key: `sk-your-key`
3. 自定义模型名: `claude-sonnet-4`

## 模型选择建议

根据不同场景选择合适的模型：

| 场景 | 推荐模型 | 原因 |
|------|----------|------|
| 日常编程 | Sonnet 4 | 速度快，质量高，性价比最好 |
| 复杂架构设计 | Opus 4 | 推理能力最强 |
| 调试复杂 bug | Sonnet 4 thinking | 思维链帮助定位问题 |
| 简单问答 | Haiku 4 | 响应快，成本低 |
| 大型项目分析 | Sonnet 4.5 (1M) | 超长上下文 |
| 代码审查 | Sonnet 4 | 够用且快 |

我的经验：90% 的场景用 Sonnet 4 就够了，遇到真正复杂的问题再切 Opus 4。

## 常见问题

**Q: OpenAI SDK 格式和 Anthropic 原生格式有什么区别？**
功能上没区别，只是请求格式不同。OpenAI SDK 格式更通用，大部分工具和库都支持。Anthropic 原生格式主要用于 Claude Code 和 Anthropic 官方 SDK。

**Q: 支持 Function Calling / Tools 吗？**
支持。Claude 的 Tool Use 功能完整可用。

**Q: 支持图片输入吗？**
支持。Claude 的 Vision 能力可以分析图片，通过 OpenAI SDK 的 image_url 格式传入。

**Q: 支持流式输出吗？**
支持，SSE 流式传输完整兼容。

**Q: 数据安全？**
MaxRouter 不存储对话内容，请求直接转发到官方 API。

**Q: 跟直接用 Anthropic 官方 API 有什么区别？**
功能完全一致。区别是你不需要科学上网，不需要 Anthropic 账号，不用担心封号。

**Q: 一个 Key 能同时用在多个工具上吗？**
可以。同一个 MaxRouter Key 可以同时用在 Claude Code、Cursor、Cline、你自己的项目等任何地方。

## 相关链接

- [Anthropic 官方文档](https://docs.anthropic.com)
- [Claude 模型介绍](https://www.anthropic.com/claude)
- [MaxRouter](https://www.maxrouter.ai) — 我用的 API 平台
- [MaxRouter 模型与价格](https://www.maxrouter.ai/pricing)
- [Claude Code 国内教程](https://github.com/MaxRouterAI/claude-code-guide)
- [Codex CLI 国内教程](https://github.com/MaxRouterAI/openai-api-guide)
- [Gemini CLI 国内教程](https://github.com/MaxRouterAI/gemini-api-guide)

---

如果这篇教程帮到了你，给个 ⭐ Star 让更多人看到。有问题欢迎提 Issue。
