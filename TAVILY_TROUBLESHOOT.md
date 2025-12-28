# Tavily MCP 故障排查与替代方案

## 问题分析

MCP 配置未生效的可能原因：
1. Claude Code 启动时未加载配置文件
2. npx 方式需要网络下载
3. 配置格式或位置问题
4. 环境变量冲突

## 🔧 解决方案 1: 项目级配置

尝试在项目目录创建配置文件：

```bash
cd "/Users/huan/Downloads/AI Learning/PM-AGI-Study"

# 创建项目级 MCP 配置
cat > .mcp.json << 'EOF'
{
  "mcpServers": {
    "tavily": {
      "command": "npx",
      "args": ["-y", "@tavily/mcp"],
      "env": {
        "TAVILY_API_KEY": "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"
      }
    }
  }
}
EOF

# 重启 Claude Code
claude
```

## 🔧 解决方案 2: 使用环境变量方式

修改启动脚本，直接启用 Web Search（不依赖 MCP）：

```bash
#!/bin/bash
# 启动 Claude Code 并启用网络搜索

export ANTHROPIC_API_KEY="sk-cda9f10761b74e90805dec92289664f5"
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_MODEL="deepseek-chat"
export TAVILY_API_KEY="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"

# 启用 Web Search
claude
```

## 🔧 解决方案 3: 使用 Claude 内置 Web Search

Claude Code 有内置的 Web Search 功能，不需要 Tavily MCP：

### 确认已启用
检查 settings.json 中的 `webSearchEnabled: true`

### 使用方式
直接在 Claude Code 中要求搜索：

```
搜索 Perplexity AI 的最新功能和技术架构
```

Claude Code 会自动使用内置的 Web Search 工具。

## 🔧 解决方案 4: 直接调用 Tavily API

创建一个 Python 脚本，Claude Code 可以调用：

```python
#!/usr/bin/env python3
"""
Tavily 搜索工具
使用：python3 tavily_search.py "搜索查询"
"""
import sys
import requests
import json

API_KEY = "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"
API_URL = "https://api.tavily.com/search"

def search(query, max_results=5):
    payload = {
        "api_key": API_KEY,
        "query": query,
        "search_depth": "advanced",
        "max_results": max_results,
        "include_answer": True,
        "include_domains": [],
        "exclude_domains": []
    }
    
    response = requests.post(API_URL, json=payload)
    return response.json()

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("用法: python3 tavily_search.py '搜索查询'")
        sys.exit(1)
    
    query = " ".join(sys.argv[1:])
    results = search(query)
    print(json.dumps(results, ensure_ascii=False, indent=2))
```

然后在 Claude Code 中：
```
运行 python3 tavily_search.py "Perplexity AI 技术架构"
```

## ✅ 推荐方案：使用内置 Web Search

**最简单有效的方式**：

### 1. 确认配置
```bash
cat ~/.claude/settings.json | grep webSearchEnabled
# 应该显示: "webSearchEnabled": true
```

### 2. 重启 Claude Code
```bash
bash ~/restart_claude.sh
```

### 3. 直接使用搜索
在 Claude Code 中输入：
```
搜索最新的 Perplexity AI 产品功能和技术特点
```

不需要说"使用 Tavily"，Claude Code 会自动使用 Web Search。

## 🎯 验证 Web Search 是否工作

### 测试命令
```
搜索 DeepSeek V3 模型的发布时间和主要特点
```

### 成功标志
```
🔍 WebSearch("DeepSeek V3...")
Did 3 searches in 8s
✅ 基于搜索结果...
🔗 来源：
   - https://...
```

### 如果看到这个就成功了
- 有搜索过程提示
- 返回搜索结果
- 包含来源链接
- 不是基于训练数据的回答

## 📊 内置 Web Search vs Tavily MCP

| 功能 | 内置 Web Search | Tavily MCP |
|------|----------------|------------|
| 配置难度 | ⭐ 简单 | ⭐⭐⭐ 复杂 |
| 搜索质量 | ⭐⭐⭐⭐ 好 | ⭐⭐⭐⭐⭐ 优秀 |
| 稳定性 | ⭐⭐⭐⭐⭐ 高 | ⭐⭐⭐ 一般 |
| 来源引用 | ✅ 有 | ✅ 有 |
| 特殊功能 | ❌ 无 | ✅ 领域限定、内容提取 |

## 🔍 故障排查清单

- [ ] 检查 webSearchEnabled 是否为 true
- [ ] 检查网络连接
- [ ] 确认 Claude Code 版本（需要较新版本）
- [ ] 尝试清除缓存：`rm -rf ~/.claude/session-env/*`
- [ ] 尝试不同的搜索关键词

## 💡 立即行动方案

**最快解决方法**：

1. **放弃 Tavily MCP**，使用内置 Web Search
2. **确认配置**：
   ```bash
   grep webSearchEnabled ~/.claude/settings.json
   ```
3. **重启**：
   ```bash
   bash ~/restart_claude.sh
   ```
4. **测试**：
   ```
   搜索 AI 产品经理需要掌握的技能
   ```

如果内置 Web Search 工作正常，完全可以满足您的需求：
- ✅ 搜索信息
- ✅ 提供来源
- ✅ 不胡编乱造
- ✅ 实时信息

---

**建议：先使用内置 Web Search 完成您的任务，Tavily MCP 可以之后再研究。**

