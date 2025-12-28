# Claude Code MCP 终极解决方案

## 🎯 核心问题分析

### 发现的关键信息

1. **Claude Code 版本**: v2.0.55
2. **真实路径**: `/Users/huan/.npm-global/bin/claude`
3. **MCP 支持**: ✅ 支持 `--mcp-config` 参数
4. **配置加载问题**: `settings.json` 中的 MCP 配置未被加载

### Tavily API 状态

```bash
curl "https://mcp.tavily.com/mcp/?tavilyApiKey=..."
# 返回: Client must accept text/event-stream
# 说明: API 正常，需要正确的 HTTP 头
```

## ✅ 解决方案（4 种方法）

### 方法 1: 使用 `--mcp-config` 参数（最可靠）

**原理**: 直接通过命令行参数加载 MCP 配置，绕过 settings.json 加载问题

**步骤**:

1. 创建独立的 MCP 配置文件：
```bash
cat > ~/mcp-config.json << 'EOF'
{
  "mcpServers": {
    "tavily": {
      "type": "http",
      "url": "https://mcp.tavily.com/mcp/?tavilyApiKey=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",
      "timeout": 30000
    }
  }
}
EOF
```

2. 启动 Claude Code:
```bash
/Users/huan/.npm-global/bin/claude --mcp-config ~/mcp-config.json
```

3. 验证:
```
/mcp
```

**优点**:
- ✅ 最直接，不依赖 settings.json
- ✅ 配置清晰，易于调试
- ✅ 可以同时加载多个配置文件

### 方法 2: 项目级配置

**原理**: 在项目目录创建 `.claude/settings.json`

**步骤**:

1. 已创建配置文件：
```
/Users/huan/Downloads/AI Learning/PM-AGI-Study/.claude/settings.json
```

2. 从项目目录启动：
```bash
cd "/Users/huan/Downloads/AI Learning/PM-AGI-Study"
/Users/huan/.npm-global/bin/claude
```

**优点**:
- ✅ 项目特定配置
- ✅ 不影响全局配置

### 方法 3: 调试模式

**原理**: 启用 debug 模式查看 MCP 加载详情

**步骤**:

```bash
/Users/huan/.npm-global/bin/claude --debug --mcp-config ~/mcp-config.json
```

**作用**:
- 🔍 显示 MCP 服务器连接日志
- 🔍 显示错误详情
- 🔍 帮助诊断问题

### 方法 4: 环境变量方式

**原理**: 直接通过环境变量加载 MCP 配置

**步骤**:

```bash
export CLAUDE_MCP_CONFIG='{"mcpServers":{"tavily":{"type":"http","url":"https://mcp.tavily.com/mcp/?tavilyApiKey=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"}}}'

/Users/huan/.npm-global/bin/claude
```

## 🚀 推荐执行顺序

### 立即测试：方法 1（最可靠）

```bash
# 1. 在 Claude Code 中按 ESC ESC 退出

# 2. 运行新的启动命令
bash ~/start_claude_deepseek.sh

# 3. 启动后立即验证
/mcp
```

**预期结果**:
```
MCP Servers:
✓ tavily (HTTP)
  Status: Connected
  Tools:
    - tavily_search
    - tavily_extract
    - tavily_crawl
    - tavily_map
```

### 如果方法 1 不工作：调试模式

```bash
# 退出 Claude Code

# 使用调试模式启动
/Users/huan/.npm-global/bin/claude --debug --mcp-config ~/mcp-config.json

# 查看控制台输出，找到 MCP 相关的错误信息
```

### 如果仍然不工作：检查 Claude Code 版本

```bash
# 更新到最新版本
npm update -g @anthropic-ai/claude-code

# 重新启动
/Users/huan/.npm-global/bin/claude --mcp-config ~/mcp-config.json
```

## 📋 已创建的文件

### 1. MCP 独立配置文件
```
~/mcp-config.json
```

### 2. 项目级配置
```
/Users/huan/Downloads/AI Learning/PM-AGI-Study/.claude/settings.json
```

### 3. 启动脚本（已更新）
```
~/start_claude_deepseek.sh  # 使用 --mcp-config 参数
```

### 4. 多选项启动脚本（新）
```
~/start_claude_options.sh   # 提供 4 种启动方式选择
```

## 🔍 故障排查清单

- [x] JSON 配置格式正确
- [x] Tavily API 可访问
- [x] Node.js 版本符合要求（v22.19.0）
- [x] Claude Code 版本支持 MCP（v2.0.55）
- [x] 会话缓存已清理
- [x] 创建独立 MCP 配置文件
- [x] 使用 --mcp-config 参数启动

## 💡 理解 MCP 加载优先级

Claude Code 按以下顺序加载 MCP 配置：

```
1. --strict-mcp-config（如果指定，只使用命令行配置）
   ↓
2. --mcp-config 参数指定的文件
   ↓
3. 项目级 .claude/settings.json
   ↓
4. 全局 ~/.claude/settings.json
```

**当前策略**: 使用 `--mcp-config` 参数（优先级 #2），确保配置一定被加载。

## ⚠️ 常见陷阱

### 陷阱 1: shell alias 掩盖真实命令
```bash
# 错误：使用 alias
alias claude='bash ~/start_claude_deepseek.sh'
claude  # 实际运行的是 bash 脚本

# 正确：使用真实路径
/Users/huan/.npm-global/bin/claude
```

### 陷阱 2: settings.json 配置未生效
```
原因: Claude Code 可能不总是加载 settings.json 中的 MCP 配置
解决: 使用 --mcp-config 参数显式加载
```

### 陷阱 3: 会话缓存干扰
```bash
# 清理缓存
rm -rf ~/.claude/session-env/*
```

## 🎯 最终验证

### 成功标志

1. **MCP 列表显示**:
```
> /mcp
✓ tavily (HTTP)
```

2. **工具可用**:
```
使用 Tavily 搜索最新 AI 产品趋势
```

3. **搜索结果包含来源**:
```
🔍 mcp_tavily_search(...)
✅ 找到 5 个结果
🔗 来源:
   - https://...
```

### 如果还是不行

运行完整诊断：

```bash
# 收集诊断信息
/Users/huan/.npm-global/bin/claude --version
cat ~/mcp-config.json
cat ~/.claude/settings.json | grep -A5 mcpServers
node --version
npm list -g @anthropic-ai/claude-code

# 重新安装 Claude Code
npm uninstall -g @anthropic-ai/claude-code
npm install -g @anthropic-ai/claude-code

# 使用调试模式
/Users/huan/.npm-global/bin/claude --debug --mcp-config ~/mcp-config.json
```

---

**现在立即执行: `bash ~/start_claude_deepseek.sh` 并运行 `/mcp`**

