# Streamlit 基本使用

## 一句话结论

Streamlit 是一个用 Python 快速构建数据应用和大模型交互界面的开源框架。开发者只需按页面从上到下编写 Python 代码，Streamlit 会把控件、图表和文本渲染为网页。

它适合数据展示、内部工具、原型验证和聊天式 AI 应用；当页面需要复杂前端交互、细粒度路由或高度定制的视觉设计时，通常更适合采用前后端分离方案。

## 安装与启动

建议在虚拟环境中安装：

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install streamlit
```

创建 `app.py`：

```python
import streamlit as st

st.title("我的第一个 Streamlit 应用")
st.write("Hello, Streamlit!")
```

在文件所在目录运行：

```powershell
streamlit run app.py
```

终端会输出本地访问地址，通常是 `http://localhost:8501`。保存 Python 文件后，页面默认会自动刷新。

## 执行模型：每次交互都会从头运行

Streamlit 最重要的特点是 rerun（重新执行）：用户点击按钮、输入文字、切换选项等触发交互后，脚本会从第一行重新运行到最后一行。

```text
用户操作控件
    -> Streamlit 从上到下重新执行 app.py
    -> 根据当前控件值和 session_state 渲染页面
```

因此不要假设普通局部变量能跨点击保存：

```python
count = 0
if st.button("加一"):
    count += 1
st.write(count)  # 每次重新运行时又会回到 0
```

需要跨 rerun 保存的临时数据，应使用 `st.session_state`。

## 常用输出组件

```python
import pandas as pd
import streamlit as st

st.title("标题")
st.header("一级分区")
st.subheader("二级分区")
st.write("可显示文本、字典、DataFrame 等对象。")
st.markdown("支持 **Markdown**。")
st.code("print('hello')", language="python")
st.info("提示信息")
st.success("操作成功")
st.warning("需要注意")
st.error("发生错误")

data = pd.DataFrame({"名称": ["A", "B"], "数值": [10, 20]})
st.dataframe(data, use_container_width=True)
st.bar_chart(data.set_index("名称"))
```

`st.dataframe` 适合可滚动、可排序的表格；少量静态数据可以使用 `st.table`。`use_container_width=True` 可让组件适应可用宽度。

## 输入组件与按钮

绝大多数组件会返回当前值：

```python
import streamlit as st

name = st.text_input("姓名", placeholder="请输入姓名")
age = st.number_input("年龄", min_value=0, max_value=150, value=18)
market = st.selectbox("市场", ["A 股", "港股", "美股"])
show_detail = st.checkbox("显示详情")
date_range = st.date_input("查询日期")

if st.button("提交", type="primary"):
    st.write(f"姓名：{name}，年龄：{age}，市场：{market}")

if show_detail:
    st.write("已启用详情。")
```

给组件设置稳定的 `key`，可以避免同类型组件冲突，并允许通过 `st.session_state` 读取或修改其值：

```python
question = st.text_input("问题", key="question_input")
st.write(st.session_state.question_input)
```

同一页面中两个控件如果标签、类型和位置都相同，通常应显式提供不同的 `key`。

## 表单：避免每次输入都提交

文本输入会触发 rerun。对于多个字段组成的一次提交操作，使用表单可以把 rerun 延后到点击提交按钮时：

```python
import streamlit as st

with st.form("search_form"):
    keyword = st.text_input("关键词")
    limit = st.slider("返回数量", 1, 20, 5)
    submitted = st.form_submit_button("查询")

if submitted:
    st.write(f"查询 {keyword}，最多返回 {limit} 条")
```

表单中应使用 `st.form_submit_button` 作为提交入口，不要把依赖一次性输入完成的业务操作直接放在普通控件变化中执行。

## 布局

```python
import streamlit as st

st.set_page_config(page_title="数据看板", layout="wide")

with st.sidebar:
    st.header("筛选条件")
    symbol = st.text_input("股票代码", "600519")

left, right = st.columns(2)
with left:
    st.metric("最新价", "1,420.00", "+1.20%")
with right:
    st.metric("成交额", "18.4 亿", "-3.00%")

tab_overview, tab_detail = st.tabs(["概览", "明细"])
with tab_overview:
    st.write("概览内容")
with tab_detail:
    st.write("明细内容")

with st.expander("高级选项"):
    st.checkbox("启用实验功能")
```

`st.set_page_config()` 应尽量放在脚本最前面。侧边栏适合放筛选条件和页面级设置；不要把大量核心内容藏在折叠区域内。

## 会话状态

`st.session_state` 保存单个浏览器会话中的状态，适合保存对话记录、登录后的展示状态、表单草稿和分页位置。用户刷新页面或服务重启后，状态可能丢失，不应用于持久化业务数据。

```python
import streamlit as st

if "count" not in st.session_state:
    st.session_state.count = 0

if st.button("加一"):
    st.session_state.count += 1

st.write(f"当前计数：{st.session_state.count}")

if st.button("重置"):
    st.session_state.count = 0
    st.rerun()
```

初始化必须在读取前完成。不要在控件创建后随意修改同名控件的状态值，否则会导致异常或难以理解的页面行为。

## 缓存

每次交互都会重跑脚本，耗时的数据读取和资源初始化应按类型缓存：

```python
import pandas as pd
import streamlit as st


@st.cache_data(ttl=300)
def load_prices(symbol: str) -> pd.DataFrame:
    # 示例：实际项目中在这里查询数据库或调用行情 API
    return pd.DataFrame({"symbol": [symbol], "price": [100.0]})


@st.cache_resource
def get_client():
    # 示例：创建可复用的数据库连接池或模型客户端
    return object()


data = load_prices("600519")
client = get_client()
st.dataframe(data)
```

使用原则：

| 装饰器 | 适合对象 | 典型场景 |
| --- | --- | --- |
| `st.cache_data` | 可序列化的数据结果 | DataFrame、查询结果、计算结果 |
| `st.cache_resource` | 进程内可复用的资源 | 数据库连接池、模型客户端、机器学习模型 |

缓存函数的参数会影响缓存键。数据可能变化时设置 `ttl`，或在数据更新后调用对应函数的 `.clear()` 清除缓存。不要把用户私有数据缓存为所有会话可见的共享结果。

## 构建简单聊天界面

Streamlit 提供了适合大模型应用的聊天组件：

```python
import streamlit as st

st.title("问答助手")

if "messages" not in st.session_state:
    st.session_state.messages = []

for message in st.session_state.messages:
    with st.chat_message(message["role"]):
        st.markdown(message["content"])

if prompt := st.chat_input("请输入问题"):
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
        st.markdown(prompt)

    # 此处替换为实际的大模型 API 调用。
    answer = f"收到你的问题：{prompt}"
    st.session_state.messages.append({"role": "assistant", "content": answer})
    with st.chat_message("assistant"):
        st.markdown(answer)
```

不要把 API Key 写进代码或提交到 Git。开发环境可使用 `.streamlit/secrets.toml` 保存本地密钥：

```toml
# .streamlit/secrets.toml
OPENAI_API_KEY = "your-api-key"
```

代码中通过 `st.secrets["OPENAI_API_KEY"]` 读取，并将该文件加入 `.gitignore`。生产环境应使用部署平台提供的密钥管理功能。

## 多页面应用

最简单的多页面组织方式是在项目目录建立 `pages` 文件夹：

```text
my_app/
├── app.py
└── pages/
    ├── 1_行情查询.py
    └── 2_策略回测.py
```

运行 `streamlit run app.py` 后，页面会自动出现在侧边栏。不同页面会共享同一会话的 `st.session_state`，但每个页面的脚本仍会独立执行。

## 配置与部署

项目级配置可放在 `.streamlit/config.toml`：

```toml
[server]
headless = true

[theme]
primaryColor = "#1677ff"
```

部署前至少应确认：

1. `requirements.txt` 固定并列出 `streamlit` 与业务依赖。
2. 密钥通过环境变量或平台密钥功能注入，不提交到仓库。
3. 外部服务连接有超时、异常处理和用户可理解的错误提示。
4. 耗时任务提供进度反馈，例如 `st.spinner` 或 `st.status`。
5. 对用户输入、文件上传和模型输出执行必要的校验与权限控制。

## 常见问题

### 点击按钮后变量丢失

原因是脚本重新执行。将需要保留的值放入 `st.session_state`，而不是普通变量。

### 输入一个字符就触发耗时查询

将输入项放入 `st.form`，在用户点击提交按钮后再发起查询；或为查询增加缓存与防抖策略。

### 页面每次刷新都重新加载模型或数据库连接

将共享资源放进 `@st.cache_resource` 函数中。数据查询结果则使用 `@st.cache_data`，并设置合理的过期时间。

### 不要把 Streamlit 当作权限边界

隐藏一个控件不等于禁止操作。涉及数据、交易或管理功能时，后端接口仍必须执行身份认证、授权和参数校验。
