# Tavily MCP 重新配置指南（npx 方式）

## ✅ 已完成配置

根据 [Tavily 官方文档](https://docs.tavily.com/documentation/mcp)，使用 **npx 方式**配置 Tavily MCP。

### 配置内容

**文件位置**: `~/.claude/settings.json`

```json
{
  "mcpServers": {
    "tavily-mcp": {
      "command": "npx",
      "args": ["-y", "@tavily/mcp"],
      "env": {
        "TAVILY_API_KEY": "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"
      }
    }
  }
}
```

### 配置方式对比

| 方式 | 之前（失败） | 现在（推荐） |
|------|-------------|-------------|
| 类型 | HTTP Remote | Local npx |
| 稳定性 | ⚠️ 可能不稳定 | ✅ 官方推荐 |
| 配置复杂度 | 简单 | 简单 |
| 兼容性 | ❌ 未识别 | ✅ 官方支持 |

## 🚀 重启并验证

### 1. 退出当前 Claude Code

在 Claude Code 中：
- 按 `ESC` 两次

### 2. 重新启动

```bash
cd "/Users/huan/Downloads/AI Learning/PM-AGI-Study"
claude
```

或使用启动脚本：
```bash
bash ~/start_claude_deepseek.sh
```

### 3. 验证 Tavily MCP

启动后，在 Claude Code 中输入：
```
/mcp
```

**期望看到**：
```
MCP Servers:
✓ tavily-mcp
  Tools:
    - tavily-search
    - tavily-extract
```

## 🧪 测试搜索

### 快速测试
```
用 Tavily 搜索最新的 AI 产品经理技能要求
```

### 验证工具调用

成功的标志：
```
🔍 tavily-search(
  query: "AI 产品经理技能要求",
  ...
)
✅ 搜索完成
📊 结果：
   - [标题](URL)
   - [标题](URL)
```

## 📊 Tavily 功能详解

根据 [Tavily API 文档](https://docs.tavily.com/documentation/api-reference/endpoint/search)，主要功能：

### tavily-search 参数

1. **query** (必需): 搜索查询
2. **search_depth**: 
   - `basic` (1 credit): 每URL一个摘要
   - `advanced` (2 credits): 每URL多个相关片段
   - `fast` (beta): 低延迟优化
   - `ultra-fast` (beta): 极速模式

3. **max_results**: 最多返回结果数 (0-20)
4. **topic**: 
   - `general`: 通用搜索
   - `news`: 新闻搜索
   - `finance`: 金融搜索

5. **time_range**: 时间范围
   - `day`, `week`, `month`, `year`

6. **include_domains**: 限定搜索域名
7. **exclude_domains**: 排除域名
8. **country`: 优先某个国家的结果

### tavily-extract

从指定 URL 提取内容：
- 智能清洗HTML
- 去除广告和无关内容
- 返回结构化内容

## 🎯 实际应用示例

### 示例 1: 基础搜索（您的需求）
```
使用 Tavily 搜索 Perplexity、Google Gemini、NotebookLM 的技术架构，
重点关注：
1. 记忆管理机制
2. 任务规划能力
3. 工具集成方案
4. 反思机制

要求：
- 搜索过去 30 天的信息
- 使用 advanced 搜索深度
- 每个产品返回 5 个最相关结果
- 提供所有来源链接
```

### 示例 2: 新闻搜索
```
用 Tavily 搜索过去一周关于 AI 产品经理的新闻
参数：
- topic: news
- time_range: week
- max_results: 10
```

### 示例 3: 领域限定搜索
```
在 techcrunch.com 和 theverge.com 搜索 AI 创业公司的融资新闻
```

### 示例 4: 内容提取
```
提取这篇文章的主要内容：
https://www.anthropic.com/news/claude-3-5-sonnet
```

### 示例 5: 多轮深度搜索
```
1. 搜索 Perplexity 的核心功能
2. 从排名前3的文章中提取详细内容
3. 总结技术要点
```

## ⚙️ 搜索策略建议

### 高质量搜索
- 使用 `search_depth: advanced` (2 credits)
- 设置 `max_results: 5-10`
- 指定 `time_range` 获取最新信息
- 使用 `include_domains` 限定可信来源

### 快速搜索
- 使用 `search_depth: fast` (1 credit)
- 设置 `max_results: 3-5`
- 适合快速验证信息

### 新闻追踪
- 使用 `topic: news`
- 设置 `time_range: day` 或 `week`
- 获取实时信息

## 🔧 故障排查

### 如果 /mcp 还是看不到 tavily-mcp

#### 检查 1: Node.js 版本
```bash
node --version
# 需要 v20 或更高
```

#### 检查 2: npx 是否可用
```bash
which npx
# 应该显示 npx 路径
```

#### 检查 3: 手动测试安装
```bash
npx -y @tavily/mcp
# 应该显示 MCP server 信息
```

#### 检查 4: 验证配置格式
```bash
cat ~/.claude/settings.json | python3 -m json.tool
# 应该无错误输出
```

### 如果搜索不工作

1. **确认 MCP 已连接**:
   ```
   /mcp
   ```
   应该显示 tavily-mcp ✓

2. **明确指定使用 Tavily**:
   ```
   使用 tavily-search 工具搜索...
   ```

3. **检查 API 额度**:
   - 访问: https://app.tavily.com/home
   - 查看剩余 credits

## 📚 相关资源

- **API 文档**: https://docs.tavily.com/documentation/api-reference/endpoint/search
- **MCP 配置**: https://docs.tavily.com/documentation/mcp
- **控制台**: https://app.tavily.com/home
- **最佳实践**: https://docs.tavily.com/documentation/best-practices-for-search

## ✅ 配置检查清单

- [x] 更新配置文件为 npx 方式
- [ ] 重启 Claude Code
- [ ] 运行 `/mcp` 验证
- [ ] 测试 tavily-search
- [ ] 测试 tavily-extract
- [ ] 验证搜索结果质量

---

## 🎯 立即开始

**Step 1**: 退出当前 Claude Code (ESC 两次)

**Step 2**: 重新启动
```bash
claude
```

**Step 3**: 验证
```
/mcp
```

**Step 4**: 测试
```
用 Tavily 搜索 DeepSeek V3 模型信息
```

现在配置应该可以正常工作了！🚀

