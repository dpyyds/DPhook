# 🤖 DPhook机器人API

基于 **FastAPI** 的自动化接口服务，将 `DPhook` 库完整封装成 REST API，支持实时消息推送、配置管理等功能。

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com/)
[![WebSocket](https://img.shields.io/badge/WebSocket-Real--time-orange.svg)](https://websockets.readthedocs.io/)
[![Auth](https://img.shields.io/badge/Auth-Bearer-red.svg)](#)

## 🌟 核心特性

### 🔐 **API密钥认证**
- **安全可靠** - 使用Bearer Token认证，保护API安全
- **简单易用** - 支持请求头或查询参数两种认证方式
- **灵活配置** - 可动态添加和管理API密钥

### 🚀 **完整功能**
- **38个API端点** - 覆盖所有核心功能
- **消息发送** - 文本、图片、视频、文件、名片、XML等
- **联系人管理** - 获取、搜索、添加、修改联系人
- **群组管理** - 创建、邀请、踢人、修改群名/公告
- **数据库查询** - 直接查询数据库

### 📡 **实时消息推送**
- **WebSocket支持** - 实时接收消息
- **29种消息类型** - 全面覆盖消息类型
- **基于wxid路由** - 精准推送给指定用户
- **连接管理** - 自动断线重连和健康检查

### ⚙️ **企业级特性**
- **配置热更新** - 支持动态修改配置
- **结构化日志** - 完整的日志记录
- **异步处理** - 高性能并发处理
- **自动文档** - Swagger UI + ReDoc
- **美观界面** - 现代化Web界面

## 🚀 快速开始

### 1. 安装依赖

```bash
# 克隆项目
git clone https://github.com/dpyyd/DPhook.git

```

# 下载通讯软件
[点击下载](dpbot.net/upload/wechat_3.6.0.18.zip)

### 2. 配置文件

编辑 `config.yaml` 配置路径和认证信息：

```yaml
# 通信软件路径配置
wechat:
  exe_path: "D:\\your\\path\\to\\WeChat.exe"  # 修改为你的路径
  version: "3.6.0.18"

# 服务器配置
server:
  host: "127.0.0.1"
  port: 8002
  debug: false

# 认证配置
auth:
  enabled: true  # 启用认证
  api_keys:
    admin: "dpyyds"  # 管理员密钥

```

### 3. 启动服务

```bash
双击DPhook.exe
```

服务启动后访问：
- 🏠 **主页**: http://127.0.0.1:8002
- 📚 **API文档**: http://127.0.0.1:8002/docs
- 📖 **ReDoc文档**: http://127.0.0.1:8002/redoc

## 🔐 API密钥认证

所有API接口都需要API密钥认证，只支持请求头方式：

### API接口认证（只支持请求头）
```bash
curl -H "Authorization: Bearer dpyyds" http://127.0.0.1:8002/api/wechat/status
```

### 默认API密钥
- **管理员密钥**: `dpyyds`
- 你可以在`config.yaml`中添加更多密钥，所有密钥都有效。

## 📖 API 使用示例

### 1. 基础操作

```bash
# 打开通讯软件
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     "http://127.0.0.1:8002/api/wechat/open?smart=true&show_qrcode=false"

# 等待登录
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     "http://127.0.0.1:8002/api/wechat/wait_login?timeout=60"

# 获取状态
curl -H "Authorization: Bearer dpyyds" \
     "http://127.0.0.1:8002/api/wechat/status"

# 获取个人信息
curl -H "Authorization: Bearer dpyyds" \
     "http://127.0.0.1:8002/api/wechat/self_info"
```

### 2. 消息发送

```bash
# 发送文本消息
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     -d '{"to_wxid":"friend_wxid_or_group_wxid","content":"Hello, World!"}' \
     "http://127.0.0.1:8002/api/wechat/send/text"

# 发送群@消息
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     -d '{"to_wxid":"group_wxid","content":"大家好！","at_list":["wxid1","wxid2"]}' \
     "http://127.0.0.1:8002/api/wechat/send/room_at"

# 发送图片
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -F "to_wxid=friend_wxid" \
     -F "file=@image.jpg" \
     "http://127.0.0.1:8002/api/wechat/send/image"
```

### 3. 联系人和群组管理

```bash
# 获取联系人列表
curl -H "Authorization: Bearer dpyyds" \
     "http://127.0.0.1:8002/api/wechat/contacts"

# 获取群列表
curl -H "Authorization: Bearer dpyyds" \
     "http://127.0.0.1:8002/api/wechat/rooms"

# 搜索联系人
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     -d '{"nickname":"张三","fuzzy_search":true}' \
     "http://127.0.0.1:8002/api/wechat/contact/search"

# 创建群聊
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     -d '{"member_list":["wxid1","wxid2","wxid3"]}' \
     "http://127.0.0.1:8002/api/wechat/room/create"
```

### 4. 配置管理

```bash
# 获取当前配置
curl -H "Authorization: Bearer dpyyds" \
     "http://127.0.0.1:8002/api/config/"

# 更新配置
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     -d '{"exe_path":"D:\\WeChat\\WeChat.exe","version":"3.6.0.18"}' \
     "http://127.0.0.1:8002/api/config/wechat"

# 更新认证配置
curl -X POST \
     -H "Authorization: Bearer dpyyds" \
     -H "Content-Type: application/json" \
     -d '{"api_keys":{"admin":"new-admin-key","client":"new-client-key"}}' \
     "http://127.0.0.1:8002/api/config/auth"
```

## 📡 WebSocket 实时消息

### 连接方式
- **连接地址**：
  ```
  ws://127.0.0.1:8002/api/ws/connect/{wxid}?api_key=dpyyds
  ```
  - `{wxid}` 必须是当前登录的WXID
  - `api_key` 必须为有效API密钥
- **认证方式**：只支持查询参数 `api_key`
- **注意**：wxid与当前登录ID不符、未登录、api_key错误都会导致连接被拒绝

#### JavaScript 示例
```javascript
const wxid = "your_current_login_wxid";
const apiKey = "dpyyds";
const ws = new WebSocket(`ws://127.0.0.1:8002/api/ws/connect/${wxid}?api_key=${apiKey}`);
ws.onopen = () => ws.send("ping");
ws.onmessage = (event) => console.log(event.data);
```

#### Python 示例
```python
import websockets, asyncio

async def ws_client():
    wxid = "your_current_login_wxid"
    api_key = "dpyyds"
    uri = f"ws://127.0.0.1:8002/api/ws/connect/{wxid}?api_key={api_key}"
    async with websockets.connect(uri) as websocket:
        await websocket.send("ping")
        print(await websocket.recv())

asyncio.run(ws_client())
```

### 支持的消息类型

WebSocket会推送以下29种消息类型：
- 文本消息、图片消息、语音消息、视频消息
- 文件消息、表情消息、名片消息、位置消息
- 链接消息、小程序消息、支付消息
- 系统消息、撤回消息、好友申请等


## 📊 API 功能总览

项目提供 **38个API端点**，分为以下类别：

### 🤖 通讯软件控制 (6个端点)  
- 通讯软件状态、打开通讯软件、进程管理、登录等待

### 💬 消息发送 (10个端点)
- 文本、图片、视频、文件、名片、链接、XML、拍一拍等

### 👥 联系人管理 (6个端点)
- 获取联系人、搜索、添加好友、修改备注等

### 🏘️ 群组管理 (8个端点)
- 群列表、群详情、创建群、邀请/踢人、群设置等

### 🗄️ 数据查询 (1个端点)
- 数据库SQL查询

### ⚙️ 配置管理 (4个端点)
- 获取/更新通讯软件配置、认证配置

### 📡 WebSocket管理 (3个端点)
- 连接统计、连接管理、消息推送

### 在浏览器中测试

1. 访问 http://127.0.0.1:8002/docs
2. 点击右上角 "Authorize" 按钮
3. 输入API密钥: `dpyyds`
4. 开始测试所有API接口

### WebSocket测试

访问项目主页 http://127.0.0.1:8002 可以在浏览器中测试WebSocket连接。

## ⚠️ 重要说明

### 支持的通讯软件版本
- **推荐版本**: 3.6.0.18

### 运行要求
- **操作系统**: Windows（客户端要求）
- **权限**: 可能需要管理员权限
- **杀毒软件**: 需要添加项目到白名单

### 使用限制
- ✅ 个人学习和研究
- ✅ 办公自动化
- ✅ 客户服务机器人
- ❌ 商业营销群发
- ❌ 恶意骚扰行为
- ❌ 违反通信软件服务条款的行为

请遵守相关法律法规和通讯软件使用条款。

## 🔧 高级配置

### 生产环境配置建议

```yaml
# config.yaml (生产环境)
auth:
  enabled: true
  api_keys:
    admin: "your-secure-admin-key"
    client: "your-client-key"

server:
  host: "0.0.0.0"  # 允许外部访问
  port: 8002
  debug: false     # 生产环境关闭调试

logging:
  level: "INFO"
  file: "logs/app.log"  # 日志文件
```

### 性能优化建议

1. **并发连接**: 默认支持多WebSocket连接
2. **内存使用**: 监控连接数，适时清理僵死连接
3. **文件上传**: 大文件上传后自动清理临时文件
4. **日志管理**: 配置日志轮转避免日志文件过大

## 🆘 故障排除

### 常见问题

**Q: API认证失败**
A: 检查API密钥是否正确，确保在请求头或查询参数中正确传递

**Q: 接口调用失败**
A: 检查服务是否正常运行，确保通讯软件已登录

**Q: WebSocket连接失败**
A: 检查wxid格式是否正确，确保通讯软件已登录，API密钥正确

**Q: 通讯软件打开失败**
A: 检查通讯软件路径配置，确保通讯软件版本兼容

**Q: 通讯软件未登录**
A: 确保通讯软件已登录，可以通过状态接口检查登录状态



## 📞 技术支持

如遇问题，请：

1. 查看项目文档和示例
2. 在 Issues 中搜索类似问题
3. 提交新的 Issue，包含详细的错误信息和环境配置

---

**⭐ 如果这个项目对你有帮助，请给个 Star！** 
