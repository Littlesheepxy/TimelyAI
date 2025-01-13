
# Timely.AI

该项目是一个基于 OpenAI GPT-4 的 AI Agent助手，用于自动化会议邀约、时间协调、策略分析和通知功能。通过多轮对话、日历时间协调和逻辑分析，旨在提升会议安排的效率和准确性。

## 主要功能
1. **用户主动发起对话**：帮助用户安排、修改或查询会议内容。
2. **时间协调**：通过与参与者的多轮对话，协调出所有与会者的可用时间，并避免冲突。
3. **策略分析**：分析对话内容并决定是否需要进一步的时间协调，或者直接发送会议通知。
4. **通知功能**：将最终确认的会议时间、参与者信息等通过通知发送给各方。

## 项目架构
该项目包含以下核心组件：
- **DialogueAgent**：处理用户发起的对话，收集并理解用户需求。
- **CoordinationAgent**：协调会议时间，通过与参会者多轮对话确认合适的时间。
- **StrategyAgent**：分析对话总结，决定是否需要进行时间协调或直接发送通知。
- **NotificationAgent**：根据最终确认的时间，向与会者发送通知。

## 技术栈
- **Python 3.x**
- **OpenAI GPT-4 API**：用于多轮对话和自然语言处理。
- **Flask**：提供 Web 服务，用于处理用户请求。
- **Logging**：记录系统日志，方便排查问题。
- **Tenacity**：用于实现 API 调用的重试机制。
- **python-dotenv**：管理环境变量（如 OpenAI API Key）。

## 安装与配置

### 1. 克隆项目
首先，克隆该项目到本地：

```bash
git clone https://jihulab.com/ai6444601/Timely.AI.git
cd Timely.AI
```

### 2. 创建虚拟环境并安装依赖
为了确保项目依赖的隔离，建议使用虚拟环境：

```bash
python3 -m venv venv
source venv/bin/activate  # 对于 Windows 使用 `venv\Scripts\activate`
```

安装依赖：

```bash
pip3 install -r requirements.txt
```

### 3. 配置 OpenAI API Key
为了使用 OpenAI GPT-4 API，你需要提供 OpenAI API 密钥。你可以通过以下两种方式之一配置 API 密钥：

- **方法 1：使用环境变量**
    1. 创建一个 `.env` 文件，在其中添加 API 密钥：
    
        ```bash
        OPENAI_API_KEY=your_openai_api_key_here
        ```
    
    2. 使用 `python-dotenv` 加载环境变量。

- **方法 2：直接修改代码**
    - 打开 `agents.py`，将 `openai.api_key` 设置为你的 API 密钥（不推荐在生产环境中使用这种方法）。

### 4. 配置日志目录
如果日志目录 `logs/` 不存在，系统会自动创建。你可以在日志文件中查看详细的请求和响应信息。

### 5. 启动 Flask 服务
配置完毕后，可以通过以下命令启动 Flask 服务：

```bash
python3 app.py
```

默认情况下，Flask 会在本地的 5001 端口启动应用。你可以在浏览器中访问 `http://127.0.0.1:5001/` 来查看应用。

## 使用说明

### 1. 启动对话
用户可以通过向 `/chat` 端点发送请求，启动或继续一个对话。以下是一个发送 POST 请求的示例：

```bash
POST /chat
Content-Type: application/json

{
    "message": "我想安排一个会议，讨论项目进展。",
    "type": "user_initiated"
}
```

### 2. 结束对话
当用户提供的所有信息都已经收集完毕时，`/end_dialogue` 端点用于获取对话总结：

```bash
POST /end_dialogue
Content-Type: application/json

{
    "type": "user_initiated"
}
```

### 3. 时间协调
如果需要协调与会者的时间，系统会根据策略分析决定是否需要时间协调。协调流程通过 `/chat` 端点继续进行。

```bash
POST /chat
Content-Type: application/json

{
    "message": "我今天下午2点到3点有空，能否安排在这个时间?",
    "type": "coordination"
}
```

### 4. 获取总结
你可以通过 `/end_dialogue` 获取完整的对话总结，或者时间协调的总结：

```bash
POST /end_dialogue
Content-Type: application/json

{
    "type": "coordination"
}
```

## 项目结构

```
.
├── app.py                   # Flask Web 服务
├── agents.py                # 代理类定义：处理对话、时间协调、策略分析等
├── requirements.txt         # 项目依赖
├── .env                     # 环境变量文件 (存储 API Key)
├── logs/                    # 存放日志文件
└── templates/
    └── chat.html            # 前端界面模板
```

## 开发建议

1. **错误处理与日志**：完善异常处理，使用日志记录系统的运行状态，便于开发和运维人员调试。
2. **多语言支持**：考虑将对话助手扩展为多语言支持，使用适当的本地化和国际化方法。
3. **性能优化**：如果需要处理大量并发请求，可以考虑使用生产环境的 WSGI 服务器（如 Gunicorn）和负载均衡。

## 常见问题

### 1. 如何修改会议的时间？
用户可以通过继续对话的方式与 AI 进行时间协调。AI 会根据用户提供的时间建议与其他参与者进行对接，最终确定一个合适的时间。

### 2. 如何确保会议参与者的时间不会冲突？
通过多轮对话与每个参与者协调时间，系统会根据参与者的日历信息自动进行时间冲突检测。

### 3. 系统支持多少个参与者？
目前系统支持任意数量的参与者，协调时间时会逐一与参与者确认时间，并避免冲突。

## 后续优化点


### 1. **前端流程及页面的设计**
   - 完善前端界面，提升用户体验，特别是会议安排、时间选择和确认流程的交互设计。
   - 使用 React 或 Vue.js 等前端框架来提升页面的响应性和用户交互。

### 2. **集成飞书、Google、企业微信等日历接口**
   - 实现与飞书、Google Calendar、企业微信等日历平台的集成。
   - 支持日历增删改查，自动同步会议时间并与参与者日历进行冲突检测。
   - 提供方便的 API 或界面让用户能够快速选择与会时间，并自动更新日历。

### 3. **对接 Chat_on_WeChat 开源项目**
   - 将系统对接 `chat_on_wechat` 开源项目，以便在飞书、微信、企业微信、钉钉等IM中创建助手，直接通过微信与用户进行会议邀约、时间协调等。
   - 支持通过微信实现邀请、确认和提醒等功能。

### 4. **短信、电话拨打及对话内容**
   - 增加短信和电话拨打功能，使系统能够自动向参会者发送会议提醒或变更通知。
   - 利用 Twilio 或其他 API 进行电话呼叫和短信发送功能集成。
   - 记录与参会者的通话内容，并将其同步到会议安排系统中。

## 贡献
欢迎提交问题（Issues）和拉取请求（Pull Requests），我们非常欢迎任何关于改进系统的建议和贡献。

## License
本项目使用 MIT 许可证。详细信息请查看 [LICENSE](LICENSE) 文件。

```


