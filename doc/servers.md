# MCP 服务器类型与通信方式

MCP（Model Context Protocol）支持多种服务器类型和通信方式，以适应不同的开发场景和部署需求。以下是目前 MCP 官方 SDK 和协议支持的所有通信方式与服务器类型的汇总。

## 通信方式与服务器类型

| 通信方式 | 服务器类型 | 示例模块/类名 | 使用场景 |
|---------|-----------|--------------|---------|
| Stdio | stdio_server | StdioServerParameters + stdio_client() | 本地交互、测试、小型插件 |
| WebSocket | websocket_server | WebSocketServerParameters + websocket_client() | 高交互性、实时应用 |
| HTTP(S) | http_server / aiohttp_server | HttpServerParameters + http_client() | 云端部署、Web 应用集成 |
| In-process | inprocess_server（仅 Python） | InProcessServerParameters + inprocess_client() | 单进程开发调试、微服务内部调用 |
| SSE | 用于响应流式内容的 HTTP 服务器 | （基于 aiohttp 实现） | Chat、流式文本生成 |
| gRPC | — | 尚未在官方 SDK 中实现 | 高性能 RPC 通信 |

## 官方 SDK 实现示例

### 1. Stdio Server（标准输入输出）

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

params = StdioServerParameters(command="python", args=["my_server.py"])

async with stdio_client(params) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()
```

### 2. HTTP Server（同步或异步）

```python
from mcp import ClientSession, HttpServerParameters
from mcp.client.http import http_client

params = HttpServerParameters(url="http://localhost:8080")

async with http_client(params) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()
```

### 3. In-process Server（本地内嵌）

```python
from mcp.client.inprocess import inprocess_client
from my_server_module import build_server
from mcp import ClientSession

async with inprocess_client(build_server()) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()
```

### 4. WebSocket Server

```python
from mcp.client.websocket import websocket_client
params = WebSocketServerParameters(url="ws://localhost:8765")

async with websocket_client(params) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()
```

## 服务器类型对比

| 类型 | 优点 | 缺点 |
|------|------|------|
| Stdio | 简单、本地运行、易于调试 | 不适合远程部署 |
| HTTP | 易于部署、跨语言支持好 | 性能可能不如 WebSocket |
| WebSocket | 双向实时通信 | 比较复杂，可能需要额外中间件 |
| In-process | 快速、内嵌、便于测试 | 仅限 Python，无法跨进程/语言 |
| SSE | 流式响应支持 | 仅限 HTTP 下行推送 |

## SDK 参考文件结构

```shell
mcp/
├── client/
│   ├── http.py
│   ├── stdio.py
│   ├── websocket.py
│   ├── inprocess.py
├── server/
│   ├── aiohttp_server.py
│   ├── stdio_server.py
│   ├── inprocess_server.py
├── protocol/
│   ├── tools.py
│   ├── resources.py
