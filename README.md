# Weather Demo

一个基于 MCP (Model Context Protocol) 的简单天气查询演示项目。

## 快速开始

### 安装依赖

```bash
rye sync .
```

### 运行项目

```bash
python src/client.py src/server.py
```

## 相关文档

- [MCP Server 文档](https://modelcontextprotocol.io/quickstart/server)
- [MCP Client 文档](https://modelcontextprotocol.io/quickstart/client)

```bash
python src/client.py src/server.py
Processing request of type ListToolsRequest

Connected to server with tools: ['get_alerts', 'get_forecast']

MCP Client Started!
Type your queries or 'quit' to exit.

Query: CA
Processing request of type ListToolsRequest
Processing request of type CallToolRequest

I'll help you check the weather alerts for California using the state code "CA".
[Calling tool get_alerts with args {'state': 'CA'}]
I attempted to get the weather alerts for California, but it seems there was a technical error. Would you like to try:
1. Checking the alerts again
2. Getting a weather forecast instead (though I would need the specific latitude and longitude coordinates for the location you're interested in)
3. Checking alerts for a different state

Please let me know how you'd like to proceed.

Query: quit