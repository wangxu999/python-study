# 厂商开放 API

## 一句话结论

厂商开放 API 是模型提供商通过 HTTPS 提供的调用接口。应用将用户问题、系统提示词等内容发送到 API，厂商服务器运行模型并返回结果。个人项目通常优先使用这种方式，不需要自己下载模型或维护 GPU。

## 为什么个人项目优先使用开放 API

```text
浏览器 / Java、Python 后端
            |
            | HTTPS + API Key
            v
      大模型厂商开放 API
            |
            v
      模型推理、扩容和运维
```

| 对比项 | 调用厂商开放 API | 本地部署模型 |
| --- | --- | --- |
| 上手速度 | 快，申请 Key 后即可调用 | 需要安装运行时、下载模型 |
| 硬件要求 | 本机通常无 GPU 要求 | 依赖本机或服务器的 CPU / GPU |
| 模型能力 | 可直接使用厂商旗舰模型 | 通常运行开放权重模型 |
| 运维工作 | 厂商负责模型服务 | 自己负责模型、显存、并发、升级 |
| 成本形式 | 按量计费或套餐 | 硬件、云 GPU、电费和运维成本 |
| 数据处理 | 请求会发送到厂商 | 可限制在本机或内网 |

对于原型、个人工具和大多数小型应用，开放 API 往往是更实际的选择。

## 网络通信基础

调用大模型 API 本质上是一次网络通信：你的程序作为客户端（Client）向厂商服务器（Server）发送 HTTP 请求，服务器处理后返回 HTTP 响应。

```text
Python / Java 程序（客户端）
          |
          |  HTTPS 请求
          v
域名解析（DNS） -> 厂商 API 服务器
          |
          |  HTTPS 响应
          v
Python / Java 程序（客户端）
```

### 客户端和服务器

| 概念 | 含义 | 大模型 API 中的例子 |
| --- | --- | --- |
| 客户端 | 主动发起请求的一方 | 你的 Python 程序、Java 后端、浏览器 |
| 服务器 | 接收请求并返回结果的一方 | 模型厂商的 API 服务 |
| 请求（Request） | 客户端发给服务器的数据 | “请回答这个问题”及模型参数 |
| 响应（Response） | 服务器返回给客户端的数据 | 模型回答、错误信息、Token 用量 |

前端浏览器通常不应直接携带厂商 API Key 请求模型服务，而应先请求自己的后端；后端再安全地调用厂商 API。

### 域名、IP 地址和端口

| 概念 | 作用 | 示例 |
| --- | --- | --- |
| 域名 | 方便人记忆的服务器名称 | `api.example.com` |
| DNS | 将域名解析为 IP 地址的服务 | 将 `api.example.com` 找到对应服务器地址 |
| IP 地址 | 网络中定位一台设备的地址 | `203.0.113.10`（示例保留地址） |
| 端口 | 定位设备上的具体网络服务 | HTTPS 常用 `443`，HTTP 常用 `80` |

程序一般使用域名，而不是把 IP 地址写死。厂商可以在不改变域名的情况下调整服务器、负载均衡和扩容策略。

### URL 的组成

API 地址通常称为 URL（Uniform Resource Locator，统一资源定位符）。

```text
https://api.example.com:443/v1/chat/completions?debug=false
|----|   |--------------| |--| |-------------------| |---------|
协议          域名         端口           路径              查询参数
```

- 协议：`http` 或 `https`，大模型厂商 API 应优先使用 `https`。
- 域名：服务所在的主机名，例如 `api.example.com`。
- 端口：省略时使用协议默认端口；HTTPS 默认是 `443`。
- 路径：服务器上的具体接口，例如 `/v1/chat/completions`。
- 查询参数：可选的附加参数，格式为 `?key=value`。

### HTTP 和 HTTPS

HTTP 是客户端与服务器交换请求、响应的应用层协议。HTTPS 可以理解为“通过 TLS 加密保护的 HTTP”：传输内容被加密，并可验证服务器身份。

| 对比项 | HTTP | HTTPS |
| --- | --- | --- |
| 常用端口 | `80` | `443` |
| 传输是否加密 | 否 | 是 |
| 是否适合传递 API Key | 不适合 | 应使用 |
| URL 前缀 | `http://` | `https://` |

即使使用 HTTPS，也不能把 API Key 放到前端代码、日志或 Git 仓库中。HTTPS 只保护传输过程，不能解决应用自身的密钥泄露问题。

### HTTP 请求的组成

一个 HTTP 请求通常包含请求行、请求头（Headers）和请求体（Body）。

```text
POST /v1/chat/completions HTTP/1.1
Host: api.example.com
Authorization: Bearer <API_KEY>
Content-Type: application/json

{
  "model": "model-x",
  "messages": [...]
}
```

| 部分 | 作用 | 大模型 API 示例 |
| --- | --- | --- |
| 请求方法 | 表示要执行的操作 | `POST` 表示提交生成请求 |
| 路径 | 表示调用哪个接口 | `/v1/chat/completions` |
| 请求头 | 附加说明和认证信息 | `Authorization`、`Content-Type` |
| 请求体 | 发送业务数据 | 模型名、消息列表、生成参数 |

常见方法：

| 方法 | 常见含义 | 大模型场景 |
| --- | --- | --- |
| `GET` | 获取资源 | 查询模型列表、服务状态 |
| `POST` | 创建资源或提交处理请求 | 发送对话、生成文本、创建任务 |
| `PUT` / `PATCH` | 更新资源 | 更新某些配置或资源，具体以厂商文档为准 |
| `DELETE` | 删除资源 | 删除文件、任务或配置，具体以厂商文档为准 |

### HTTP 响应和状态码

服务器会返回状态码、响应头和响应体。API 响应体通常是 JSON。

```text
HTTP/1.1 200 OK
Content-Type: application/json

{
  "choices": [...]
}
```

常见状态码：

| 状态码 | 含义 | 常见处理方式 |
| --- | --- | --- |
| `200` | 请求成功 | 解析响应 JSON |
| `400` | 请求参数错误 | 检查模型名、字段、消息格式 |
| `401` | 未认证或 Key 无效 | 检查 API Key 和认证请求头 |
| `403` | 没有权限 | 检查账号权限、模型权限或地区限制 |
| `404` | 地址或资源不存在 | 检查 URL、接口路径或模型名 |
| `429` | 请求过于频繁或超过限额 | 限流、等待后有限重试，检查额度 |
| `500` / `502` / `503` | 服务端暂时异常 | 有限次数重试，必要时降级或提示用户 |

状态码是判断请求是否成功的第一步。不要只看响应体是否像 JSON，就假设模型调用成功。

### JSON：API 常用的数据格式

JSON 是 API 传递结构化数据的常用文本格式，和 Python 的字典、列表很像。

```json
{
  "model": "model-x",
  "stream": false,
  "messages": [
    {
      "role": "user",
      "content": "你好"
    }
  ]
}
```

对应的 Python 数据：

```python
payload = {
    "model": "model-x",
    "stream": False,
    "messages": [
        {"role": "user", "content": "你好"},
    ],
}
```

使用 `requests.post(..., json=payload)` 时，`requests` 会将 Python 字典转换为 JSON，并设置合适的 JSON 请求体。收到响应后可用 `response.json()` 转回 Python 字典。

## API 调用的基本组成

一次大模型 API 请求通常由以下内容组成：

| 内容 | 作用 | 示例 |
| --- | --- | --- |
| API 地址（Endpoint） | 要请求的服务地址 | `https://api.example.com/v1/chat/completions` |
| HTTP 方法 | 大多数生成接口使用 `POST` | `POST` |
| API Key | 证明调用者身份并用于计费 | 放在请求头中 |
| 模型名 | 指定要使用的模型 | `model-x` |
| 输入消息 | 系统规则、用户问题、上下文 | `messages` |
| 生成参数 | 控制输出长度、随机性、是否流式返回 | `temperature`、`stream` |
| 响应数据 | 模型回答、用量、请求标识等 | JSON |

典型请求与响应：

```text
请求：模型名 + 消息列表 + 参数
             |
             v
响应：模型回答 + Token 用量 + 请求 ID
```

## 选择 API 提供商时关注什么

不要只比较“哪个模型最强”，还应结合项目约束选择。

| 关注点 | 要确认的问题 |
| --- | --- |
| 模型能力 | 是否擅长中文、代码、图片、长文档或结构化输出？ |
| 价格 | 输入和输出 Token 如何计费？是否有免费额度或并发限制？ |
| 上下文长度 | 一次最多能传入多少历史消息和文档内容？ |
| 响应速度 | 首字延迟和完整输出耗时是否符合体验要求？ |
| 稳定性 | 是否有服务状态页、限流规则和重试建议？ |
| SDK / 接口 | 是否支持 Python、Java，是否兼容 OpenAI 风格接口？ |
| 数据与合规 | 数据保存多久、是否用于训练、服务地区和企业协议如何规定？ |
| 模型许可证 | 输出内容和模型能力是否允许当前商业用途？ |

价格、模型名称和限额变化较快，上线前应以厂商官网的当前文档与价格页为准。

## 申请并保存 API Key

通常需要在厂商控制台完成注册、创建 API Key、设置用量上限或充值。Key 相当于密码：持有者可以消耗你的额度，某些情况下还可访问你的项目资源。

### 不要将 Key 写进代码

错误示例：

```python
API_KEY = "sk-真实密钥不要写在这里"
```

这样会导致 Key 进入 Git 提交记录、截图、日志或共享代码。

更好的方式是使用环境变量。Windows PowerShell 当前终端可临时设置：

```powershell
$env:LLM_API_KEY = "你的真实 API Key"
```

Python 中读取：

```python
import os

api_key = os.environ["LLM_API_KEY"]
```

`os.environ[...]` 在变量缺失时会立即报错，适合服务启动时尽早发现配置问题。若只是可选配置，可使用：

```python
api_key = os.getenv("LLM_API_KEY")
```

### `.env` 文件

本地开发也常使用 `.env` 文件保存密钥，再通过 `python-dotenv` 等工具读取。`.env` 必须写入 `.gitignore`，不能提交到 Git 仓库。

```text
LLM_API_KEY=你的真实 API Key
```

本项目的 `.gitignore` 已包含 `.env`，但仍要在 `git status` 时确认密钥文件没有被加入暂存区。

## 使用 HTTP 调用 API

不同厂商的地址、模型名、字段名称会有差异。下面是常见的 OpenAI 风格接口结构，用于理解请求组成；请替换为目标厂商文档中真实的地址和模型名。

先安装 HTTP 客户端库：

```powershell
py -m pip install requests
```

Python 示例：

```python
import os

import requests

api_key = os.environ["LLM_API_KEY"]
url = "https://api.example.com/v1/chat/completions"

headers = {
    "Authorization": f"Bearer {api_key}",
    "Content-Type": "application/json",
}

payload = {
    "model": "请替换为厂商模型名",
    "messages": [
        {
            "role": "system",
            "content": "你是一个耐心的 Python 学习助手。",
        },
        {
            "role": "user",
            "content": "解释 Python 中列表和元组的区别。",
        },
    ],
    "temperature": 0.3,
    "stream": False,
}

response = requests.post(
    url,
    headers=headers,
    json=payload,
    timeout=60,
)
response.raise_for_status()

data = response.json()
answer = data["choices"][0]["message"]["content"]
print(answer)
```

`response.raise_for_status()` 会在 HTTP 状态码为 4xx 或 5xx 时抛出异常，避免把错误响应当成正常答案处理。

> 注意：有些厂商使用不同的响应字段，未必是 `choices[0].message.content`。解析响应前应先查看该厂商的 API 文档或打印测试响应结构。

## 消息角色与上下文

聊天类 API 通常使用消息列表保存对话。常见角色含义如下：

| 角色 | 用途 |
| --- | --- |
| `system` | 设定助手身份、语气、边界和输出规则 |
| `user` | 用户提出的问题或任务 |
| `assistant` | 模型先前的回答，用于保留多轮对话上下文 |
| `tool` | 工具调用后的结果，部分 API 支持 |

多轮对话时，应用需要自己保存历史消息，并在下一次请求中一并发送：

```python
messages = [
    {"role": "system", "content": "你是 Python 助手。"},
    {"role": "user", "content": "什么是函数？"},
    {"role": "assistant", "content": "函数是可重复使用的代码块。"},
    {"role": "user", "content": "给一个最小示例。"},
]
```

模型 API 通常不会自动永久记住上一轮请求。是否保存、保存多久、如何裁剪历史记录，是应用后端需要负责的事情。

## 常用生成参数

参数名称和可用范围因厂商而异，下面是常见概念。

| 参数 | 常见作用 | 使用建议 |
| --- | --- | --- |
| `model` | 选择模型 | 先固定一个模型，便于测试成本和效果 |
| `temperature` | 控制回答的随机性 | 事实问答、代码、抽取任务通常使用较低值；创意任务可适当提高 |
| `max_tokens` / 输出上限 | 限制最长输出 | 防止回答过长和费用失控；具体字段以厂商文档为准 |
| `stream` | 是否分段返回内容 | 聊天界面通常使用流式；后台批处理可先用非流式 |
| `top_p` | 另一种随机性控制方式 | 初学时通常调 `temperature` 即可，不要同时随意调很多参数 |
| `response_format` | 请求特定输出格式 | 适合 JSON 等结构化结果，具体能力因模型和厂商而异 |

`temperature` 不能保证事实正确。它只会影响候选内容的随机性；需要可靠结果时仍应做校验、检索或人工审核。

## 流式响应

非流式调用会等待模型完成后一次返回全文，代码简单；流式调用会在模型生成过程中不断返回小片段，用户能更早看到内容。

```text
非流式：请求 ----------------------> 完整回答
流式：  请求 -> 片段 1 -> 片段 2 -> 片段 3 -> 结束
```

流式响应常用于聊天页面，但前端或后端需要逐段读取和拼接内容。不同厂商可能采用 SSE（Server-Sent Events）或其他格式，事件字段也不完全一致，因此应按厂商 SDK 或官方示例处理。

## 结构化输出

业务系统经常需要模型输出可被程序读取的数据，而不是一段自然语言。例如提取订单信息：

```json
{
  "product_name": "Python 入门课程",
  "count": 2,
  "need_invoice": true
}
```

可以在提示词中明确规定字段和 JSON 格式；如果厂商提供 JSON Mode、JSON Schema 或结构化输出功能，优先使用这些能力。无论哪种方式，后端都应使用 JSON 解析和字段校验，不能盲目信任模型输出。

```python
import json

text = '{"product_name": "Python 入门课程", "count": 2}'
data = json.loads(text)

if not isinstance(data.get("count"), int):
    raise ValueError("count 必须是整数")
```

## 费用与 Token 控制

API 通常按输入 Token 和输出 Token 分别计费。一次请求成本大致由以下因素决定：

```text
成本 ≈ 输入 Token 用量 × 输入单价 + 输出 Token 用量 × 输出单价
```

控制成本的常用措施：

- 不要每次都携带过长的聊天历史；保留最近消息，或将旧内容摘要化。
- 限制输出长度，避免“请详细回答”导致无必要的长文本。
- 对简单任务使用更小、更便宜的模型。
- 缓存重复问题的结果，例如固定说明、常见 FAQ。
- 记录每次调用的模型、耗时、输入输出 Token 和请求结果。
- 在厂商控制台设置预算告警、额度上限和 Key 权限。

## 错误处理与重试

调用外部 API 时常见失败包括网络超时、身份认证失败、请求参数错误、余额不足和限流。应区分处理，而不是统一吞掉异常。

```python
import requests

try:
    response = requests.post(url, headers=headers, json=payload, timeout=60)
    response.raise_for_status()
except requests.Timeout:
    print("模型服务响应超时，请稍后重试")
except requests.HTTPError as error:
    print(f"请求失败，HTTP 状态码：{error.response.status_code}")
except requests.RequestException as error:
    print(f"网络请求失败：{error}")
else:
    print("调用成功")
```

对 429（限流）和部分 5xx（服务端暂时异常）可以使用有限次数的指数退避重试；对 400（请求参数错误）和 401（Key 无效）通常不应盲目重试，而应修复请求或配置。

## 推荐的项目接入方式

不要将 API Key 放进浏览器、手机 App 或前端 JavaScript。前端代码容易被用户查看，Key 会泄露。

推荐架构：

```text
前端
  |
  v
自己的后端（Java Spring Boot / Python FastAPI）
  |  - 身份认证
  |  - 参数校验
  |  - 业务逻辑
  |  - 费用和频率控制
  v
厂商大模型 API
```

即使是个人项目，也建议至少通过自己的后端转发请求。这样可以保护 Key，并能统一处理日志、限流、提示词和异常。

## 易错点

- [ ] 以为开放 API 和本地部署只能二选一。
  - 正确做法：二者都通过 API 调用；区别在于模型运行在厂商服务器还是自己的机器上。
- [ ] 将真实 API Key 写进代码、截图或 Git 仓库。
  - 正确做法：使用环境变量或密钥管理服务，并确认 `.env` 未被提交。
- [ ] 直接从浏览器调用厂商 API。
  - 正确做法：通过自己的后端调用，避免泄露 Key。
- [ ] 不设置 `timeout`，在网络异常时无限等待。
  - 正确做法：设置合理超时，并向用户显示可理解的错误提示。
- [ ] 把模型回复当作可靠 JSON，直接写入数据库。
  - 正确做法：先解析、校验字段和业务规则。
- [ ] 不保留对话历史，却期望模型记住前文。
  - 正确做法：由应用负责保存、裁剪和发送必要的上下文。
- [ ] 遇到 401、400 就无限重试。
  - 正确做法：401 应检查 Key，400 应检查参数；只有临时网络失败或限流才考虑有限重试。

## 自测问题

1. 调用厂商开放 API 时，模型运行在哪里？
2. DNS 在 API 调用中起什么作用？
3. URL 中的协议、域名、路径分别表示什么？
4. 为什么调用厂商 API 必须使用 HTTPS，而不是 HTTP？
5. HTTP 请求中的 `Authorization` 和 `Content-Type` 请求头分别有什么作用？
6. 400、401、429 分别代表什么问题？
7. API Key 为什么不能提交到 Git？
8. `response.raise_for_status()` 的作用是什么？
9. `system`、`user`、`assistant` 消息分别通常用于什么？
10. 为什么多轮对话需要由应用保存历史消息？
11. 非流式和流式响应的体验差异是什么？
12. 为什么模型生成的 JSON 仍需要后端校验？

## 关联内容

- 前置知识：Python 模块、异常、环境变量、HTTP 请求、JSON。
- 后续知识：Prompt 工程、流式接口、结构化输出、RAG、Agent、后端服务开发。
- 练习：申请一个测试 API Key，编写 Python 程序读取环境变量，发送一个问题，并记录回答、耗时和 HTTP 状态码；密钥不能写入代码或提交到 Git。

## 复习状态

- 首次记录：2026-09-13
- 最近复习：
- 掌握程度：了解
