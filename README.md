# Claude API 国内使用完整教程（2026 最新）

> Claude 是目前代码能力最强的大模型之一。本文介绍如何在国内通过 API 调用 Claude 模型，包括在自己的项目中集成、配合 Claude Code 使用等场景。

---

## Claude 模型介绍

Anthropic 的 Claude 系列模型在编程、推理、长文本理解方面表现突出，是很多开发者的首选：

| 模型 | 上下文 | 特点 |
|------|--------|------|
| Claude Opus 4 | 200K | 最强推理，复杂任务首选 |
| Claude Sonnet 4 | 200K | 速度与质量平衡，日常编程推荐 |
| Claude Sonnet 4 (thinking) | 200K | 带思维链，适合复杂调试 |
| Claude Haiku 4 | 200K | 快速响应，简单任务省钱 |

## 官方使用方式

Anthropic 提供两种使用方式：

1. **Claude.ai 网页版** — 对话式使用，需要订阅 Pro/Max
2. **API 按量付费** — 通过 `api.anthropic.com` 调用，适合开发者集成

API 调用需要：
- 注册 Anthropic 账号
- 绑定海外信用卡
- 获取 API Key

## 国内的问题

跟所有海外 AI 服务一样：

- `api.anthropic.com` 国内无法直接访问
- 注册需要海外手机号和信用卡
- 虚拟卡封号率高，余额不退
- 代理工具不稳定

详细的问题分析可以看我写的 [Claude Code 国内使用教程](https://github.com/MaxRouterAI/claude-code-guide#国内开发者面临的问题)，这里不重复了。

## 通过 MaxRouter 中转

我用的是 [MaxRouter](https://www.maxrouter.ai)，支持所有 Claude 模型，与官方同价，按量计费，国内直连。

注册送 ¥10 体验金，够试用一阵。进入控制台创建 Key 就能用。

### 方式一：OpenAI SDK 格式（推荐）

MaxRouter 兼容 OpenAI SDK 格式，这是最简单的集成方式：

**Python：**

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-maxrouter-key",
    base_url="https://api.maxrouter.ai/v1"
)

response = client.chat.completions.create(
    model="claude-sonnet-4",
    messages=[{"role": "user", "content": "用 Python 实现一个 LRU 缓存"}]
)
print(response.choices[0].message.content)
```

**Node.js：**

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
    apiKey: 'sk-your-maxrouter-key',
    baseURL: 'https://api.maxrouter.ai/v1'
});

const response = await client.chat.completions.create({
    model: 'claude-sonnet-4',
    messages: [{ role: 'user', content: '解释 React 的 useEffect' }]
});
console.log(response.choices[0].message.content);
```

**cURL：**

```bash
curl https://api.maxrouter.ai/v1/chat/completions \
  -H "Authorization: Bearer sk-your-key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-sonnet-4",
    "messages": [{"role": "user", "content": "你好"}]
  }'
```

### 方式二：Anthropic 原生格式

如果你用 Claude Code 或 Anthropic SDK，设置环境变量即可：

```bash
export ANTHROPIC_BASE_URL=https://api.maxrouter.ai
export ANTHROPIC_API_KEY=sk-your-maxrouter-key
```

> ⚠️ `ANTHROPIC_BASE_URL` 不带 `/v1`，Claude SDK 会自动拼接路径。

### 方式三：流式输出

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-key",
    base_url="https://api.maxrouter.ai/v1"
)

stream = client.chat.completions.create(
    model="claude-sonnet-4",
    messages=[{"role": "user", "content": "写一首关于编程的诗"}],
    stream=True
)

for chunk in stream:
    content = chunk.choices[0].delta.content
    if content:
        print(content, end="", flush=True)
```

## 配合开发工具

### Claude Code

最火的 AI 编程工具，详细教程看这里：[Claude Code 国内使用完整教程](https://github.com/MaxRouterAI/claude-code-guide)

### Cursor

- Override OpenAI Base URL: `https://api.maxrouter.ai/v1`
- API Key: `sk-your-key`
- 模型: `claude-sonnet-4`

### Cline（VS Code）

- API Provider: OpenAI Compatible
- Base URL: `https://api.maxrouter.ai/v1`
- API Key: `sk-your-key`

### Continue（VS Code / JetBrains）

```json
{
  "models": [{
    "provider": "openai",
    "model": "claude-sonnet-4",
    "apiKey": "sk-your-key",
    "apiBase": "https://api.maxrouter.ai/v1"
  }]
}
```

## 常见问题

**Q: 跟 Anthropic 官方有什么区别？**
功能完全一致，MaxRouter 只做转发，不存储数据。

**Q: 除了 Claude 还支持什么？**
GPT-5、GPT-4o、Gemini、DeepSeek 等 80+ 模型，统一接口，一个 Key 搞定。

**Q: 支持 Function Calling 吗？**
支持，所有 Claude API 功能都完整支持。

## 相关链接

- [Anthropic 官方文档](https://docs.anthropic.com)
- [MaxRouter](https://www.maxrouter.ai)
- [MaxRouter 模型与价格](https://www.maxrouter.ai/pricing)
- [Claude Code 国内教程](https://github.com/MaxRouterAI/claude-code-guide)
- [Codex CLI 国内教程](https://github.com/MaxRouterAI/openai-api-guide)
- [Gemini CLI 国内教程](https://github.com/MaxRouterAI/gemini-api-guide)

---

觉得有用就给个 ⭐ Star，有问题欢迎提 Issue。
