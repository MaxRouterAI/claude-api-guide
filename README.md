# Claude API 国内使用完整指南【2026 年最新】

> 国内开发者使用 Claude API（Opus 4、Sonnet 4、Haiku 等）的完整解决方案 — 无需魔法，兼容原生 SDK

## 📌 你遇到的问题

- ❌ Anthropic 官网国内无法访问
- ❌ Claude API 直连被拒、403 错误
- ❌ 注册需要海外手机号和信用卡
- ❌ 想在项目中集成 Claude，但网络不通

## 🚀 解决方案：通过 MaxRouter 中转

[MaxRouter](https://www.maxrouter.ai) 提供 Claude API 中转服务，同时支持 Anthropic 原生格式和 OpenAI 兼容格式。

**核心优势：**
- ✅ 国内直连，无需代理
- ✅ 支持 Anthropic 原生协议 + OpenAI 兼容格式
- ✅ 按量计费，注册即送 ¥10 体验金
- ✅ 支持 Claude Code、Cursor、Cline 等开发工具
- ✅ 完整支持流式输出、长上下文

## 🤖 支持的 Claude 模型

| 模型 | 上下文 | 适用场景 |
|------|--------|----------|
| `claude-opus-4` | 200K | 最强推理，复杂任务 |
| `claude-sonnet-4` | 200K | 日常编程，性价比高 |
| `claude-sonnet-4-thinking` | 200K | 带思维链，复杂调试 |
| `claude-haiku-4` | 200K | 快速响应，简单任务 |

## 📋 快速开始

### 注册并获取 Key

1. 访问 [www.maxrouter.ai](https://www.maxrouter.ai) 注册（送 ¥10 体验金）
2. [控制台 → 令牌管理](https://www.maxrouter.ai/console/tokens) 创建 API Key

### 方式一：OpenAI SDK 格式（推荐）

使用 OpenAI SDK 调用 Claude，最简单：

**Python：**

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-key",
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
    apiKey: 'sk-your-key',
    baseURL: 'https://api.maxrouter.ai/v1'
});

const response = await client.chat.completions.create({
    model: 'claude-sonnet-4',
    messages: [{ role: 'user', content: '解释 React 的 useEffect 生命周期' }]
});
console.log(response.choices[0].message.content);
```

### 方式二：Anthropic 原生格式

如果你使用 Claude Code 或 Anthropic SDK：

```bash
export ANTHROPIC_BASE_URL=https://api.maxrouter.ai
export ANTHROPIC_API_KEY=sk-your-key
```

> ⚠️ 注意：`ANTHROPIC_BASE_URL` 不带 `/v1`，Claude SDK 会自动拼接路径。

### 方式三：cURL

```bash
curl https://api.maxrouter.ai/v1/chat/completions \
  -H "Authorization: Bearer sk-your-key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-sonnet-4",
    "messages": [{"role": "user", "content": "你好"}]
  }'
```

## 🔧 Claude Code 配置

Claude Code 是目前最火的 AI 编程工具，配置方法：

**macOS / Linux：**

```bash
# ~/.zshrc 或 ~/.bashrc
export ANTHROPIC_BASE_URL=https://api.maxrouter.ai
export ANTHROPIC_API_KEY=sk-your-key
```

**Windows PowerShell：**

```powershell
$env:ANTHROPIC_BASE_URL = "https://api.maxrouter.ai"
$env:ANTHROPIC_API_KEY = "sk-your-key"
```

然后直接运行 `claude` 即可。

👉 更详细的 Claude Code 配置教程：[Claude Code 国内使用完整指南](https://github.com/MaxRouterAI/claude-code-guide)

## 🔧 其他工具配置

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

## ❓ 常见问题

### Q: 跟 Anthropic 官方有什么区别？
A: 功能完全一致，MaxRouter 只做转发，不存储任何数据。

### Q: 支持流式输出吗？
A: 支持，SSE 流式传输完整兼容。

### Q: 除了 Claude，还支持什么？
A: 同时支持 GPT-5、GPT-4o、Gemini、DeepSeek 等 80+ 模型，统一接口。

## 🔗 相关链接

- 🌐 官网：[www.maxrouter.ai](https://www.maxrouter.ai)
- 📖 文档：[www.maxrouter.ai/docs](https://www.maxrouter.ai/docs)
- 🤖 模型：[www.maxrouter.ai/models](https://www.maxrouter.ai/models)
- 💰 价格：[www.maxrouter.ai/pricing](https://www.maxrouter.ai/pricing)
- 🔑 Claude Code 指南：[github.com/MaxRouterAI/claude-code-guide](https://github.com/MaxRouterAI/claude-code-guide)

---

**⭐ 觉得有用？给个 Star 支持一下！**
