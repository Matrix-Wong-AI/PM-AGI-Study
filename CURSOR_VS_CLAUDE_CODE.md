# Cursor IDE vs Claude Code CLI 配置说明

## 🔑 核心概念

### 两个独立的环境

```
┌─────────────────────────┐         ┌──────────────────────────┐
│   Cursor IDE (编辑器)    │         │  Claude Code CLI (终端)   │
│                         │         │                          │
│  配置文件：              │         │  配置文件：               │
│  .cursor/settings.json  │   ≠    │  ~/.claude/settings.json │
│                         │         │                          │
│  MCP: tavily-remote-mcp│         │  MCP: 需要单独配置        │
└─────────────────────────┘         └──────────────────────────┘
```

### 为什么分开？

- **Cursor IDE**: 图形界面编辑器，集成的 AI 助手
- **Claude Code CLI**: 命令行工具，独立运行

它们的配置文件**不会自动同步**！

## ✅ 正确的 Tavily MCP 配置

### 配置类型：HTTP Remote MCP

```json
{
  "mcpServers": {
    "tavily-remote-mcp": {
      "type": "http",
      "url": "https://mcp.tavily.com/mcp/?tavilyApiKey=YOUR_API_KEY"
    }
  }
}
```

**关键点**：
- ✅ 名称：`tavily-remote-mcp`
- ✅ 类型：`"type": "http"`
- ✅ URL：包含 API key 参数的完整 URL
- ❌ 不是：`@tavily/mcp` npm 包（这个包不存在）

## 🔧 配置位置

### Claude Code CLI 配置
```bash
~/.claude/settings.json  # 全局配置
```

### Cursor IDE 配置  
```bash
.cursor/settings.json    # 项目配置
```

### 查看当前配置
```bash
# Claude Code
cat ~/.claude/settings.json | grep -A5 "mcpServers"

# Cursor IDE
cat .cursor/settings.json | grep -A5 "mcpServers"
```

## 📊 Tavily Remote MCP 提供的工具

根据截图，`tavily-remote-mcp` 提供 **4 个工具**：

1. **mcp_tavily-remote-mcp_tavily_search** - 智能搜索
2. **mcp_tavily-remote-mcp_tavily_extract** - 内容提取
3. **mcp_tavily-remote-mcp_tavily_crawl** - 网站爬取
4. **mcp_tavily-remote-mcp_tavily_map** - 网站地图

## 🎯 验证步骤

### 1. 检查配置
```bash
cat ~/.claude/settings.json | grep tavily
```

应该看到：
```json
"tavily-remote-mcp": {
  "type": "http",
  "url": "https://mcp.tavily.com/mcp/?tavilyApiKey=..."
}
```

### 2. 重启 Claude Code
```bash
# 在 Claude Code 中按 ESC ESC
# 然后重新启动
bash ~/restart_claude.sh
```

### 3. 验证 MCP
```bash
# 在 Claude Code 中
/mcp
```

应该看到：
```
MCP Servers:
✓ tavily-remote-mcp (HTTP)
  Tools:
    - tavily_search
    - tavily_extract
    - tavily_crawl
    - tavily_map
```

### 4. 测试搜索
```
使用 Tavily 搜索 2024 年最受欢迎的 AI 设计工具
```

## 🔍 故障排查

### 问题：/mcp 显示 "No MCP servers configured"

**原因**：
- 配置文件格式错误
- 配置文件位置错误
- Claude Code 未加载配置

**解决**：
```bash
# 1. 验证 JSON 格式
cat ~/.claude/settings.json | python3 -m json.tool

# 2. 检查配置内容
grep -A10 "mcpServers" ~/.claude/settings.json

# 3. 完全重启
pkill claude
sleep 2
claude
```

### 问题：MCP 工具调用失败

**原因**：
- API key 无效
- 网络连接问题
- Tavily 服务不可用

**测试**：
```bash
# 测试 Tavily API
curl "https://mcp.tavily.com/mcp/?tavilyApiKey=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"
```

## 💡 最佳实践

### 统一配置（可选）

如果想在两个环境中使用相同配置：

```bash
# 创建符号链接（谨慎使用）
ln -s ~/.claude/settings.json ".cursor/settings.json"
```

### 或者手动同步

```bash
# 从 Cursor 复制到 Claude Code
cp .cursor/settings.json ~/.claude/settings.json
```

## 📚 相关文档

- Tavily MCP: https://mcp.tavily.com/
- Tavily API: https://docs.tavily.com/
- Claude Code MCP: https://docs.claude.com/en/docs/claude-code/mcp

---

**现在配置已更新，重启 Claude Code 验证！**

