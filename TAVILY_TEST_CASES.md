# Tavily MCP 测试用例

## 🧪 完整测试流程

### 准备阶段

#### 1. 运行配置检查
```bash
cd "/Users/huan/Downloads/AI Learning/PM-AGI-Study"
bash test_tavily.sh
```

#### 2. 确保 Claude Code 已重启
如果配置是新修改的，必须重启：
```bash
# 在 Claude Code 中按 ESC 两次退出
# 然后运行
bash ~/start_claude_deepseek.sh
```

---

## 📝 测试用例

### 测试 1: 验证 MCP 服务器连接

**在 Claude Code 中输入**:
```
/mcp
```

**期望结果**:
```
✅ 显示可用的 MCP 服务器列表
✅ 应该包含 "tavily" 服务器
✅ 状态显示为 "connected" 或 "✓"
```

---

### 测试 2: 基础搜索测试

**测试命令**:
```
用 Tavily 搜索 DeepSeek V3 模型的发布信息
```

**期望看到**:
```
🔍 tavily-search(query: "DeepSeek V3 ...")
⏱️ 搜索进行中...
✅ 找到 X 个结果
📝 结果摘要...
🔗 来源链接：
   1. https://...
   2. https://...
```

**成功标志**:
- ✅ 调用了 `tavily-search` 工具
- ✅ 返回了搜索结果
- ✅ 提供了来源 URL
- ✅ 信息真实可信（非 AI 编造）

---

### 测试 3: 新闻搜索（时间限定）

**测试命令**:
```
使用 Tavily 搜索过去 7 天关于 AI 产品经理的新闻
```

**期望结果**:
- ✅ 搜索结果来自最近 7 天
- ✅ 提供发布日期
- ✅ 包含新闻来源链接
- ✅ 内容相关度高

---

### 测试 4: 领域限定搜索

**测试命令**:
```
在 techcrunch.com 和 theverge.com 上搜索 AI 产品设计的文章
```

**期望结果**:
- ✅ 搜索结果主要来自指定网站
- ✅ URL 包含 techcrunch.com 或 theverge.com
- ✅ 内容相关性高

---

### 测试 5: 内容提取

**测试命令**:
```
使用 Tavily 提取这篇文章的主要内容：
https://www.anthropic.com/news/claude-3-5-sonnet
```

**期望结果**:
- ✅ 调用了 `tavily-extract` 工具
- ✅ 提取了文章主要内容
- ✅ 去除了广告和无关内容
- ✅ 内容结构清晰

---

### 测试 6: 综合搜索（您的实际需求）

**测试命令**:
```
使用 Tavily 搜索以下三个 AI 产品的技术架构和功能特点：
1. Perplexity
2. Google Gemini  
3. NotebookLM

重点分析它们在以下四个维度的实现：
- 记忆管理（短期/长期记忆）
- 任务规划（多步骤推理、任务分解）
- 工具使用（API调用、外部工具集成）
- 反思机制（自我改进、错误纠正）

要求：
- 搜索过去 30 天的信息
- 提供所有信息的来源链接
- 对比三个产品的优劣势
- 以结构化方式呈现（表格或清单）
```

**期望结果**:
- ✅ 对每个产品都进行了搜索
- ✅ 按四个维度组织信息
- ✅ 每个维度都有具体的技术细节
- ✅ 提供了多个来源链接
- ✅ 包含对比分析
- ✅ 结构化呈现（表格或清单）
- ✅ 信息准确，有据可查

---

### 测试 7: 多轮搜索

**测试命令**:
```
1. 先用 Tavily 搜索 Perplexity 的核心功能
2. 基于搜索结果，深入搜索其中最有趣的功能
3. 提取相关文章的详细内容
```

**期望结果**:
- ✅ 执行多次搜索
- ✅ 后续搜索基于前面的结果
- ✅ 使用了 search 和 extract 工具
- ✅ 信息逐步深入

---

## 🎯 成功指标

### 配置成功
- [x] `test_tavily.sh` 检查全部通过
- [x] `/mcp` 显示 tavily 服务器
- [x] tavily-search 工具可用
- [x] tavily-extract 工具可用

### 搜索质量
- [x] 结果相关性高
- [x] 来源可信
- [x] 时间准确
- [x] 领域限定有效

### 信息准确性
- [x] 提供真实来源链接
- [x] 信息可验证
- [x] 不胡编乱造
- [x] 引用清晰

---

## ❌ 常见问题排查

### 问题 1: /mcp 看不到 tavily

**可能原因**:
- 配置文件格式错误
- 未重启 Claude Code
- API Key 无效

**解决方案**:
```bash
# 1. 检查配置
cat ~/.claude/settings.json | grep -A5 "tavily"

# 2. 验证 JSON 格式
python3 -m json.tool ~/.claude/settings.json > /dev/null && echo "JSON 格式正确" || echo "JSON 格式错误"

# 3. 重启 Claude Code
bash ~/start_claude_deepseek.sh
```

---

### 问题 2: Tavily 不搜索，直接回答

**可能原因**:
- MCP 服务器未连接
- 提示词不够明确

**解决方案**:
- 明确说"使用 Tavily"或"用 Tavily 搜索"
- 检查 `/mcp` 确认 tavily 已连接

---

### 问题 3: 搜索结果质量不好

**优化方法**:
- 使用更具体的搜索词
- 指定时间范围
- 限定搜索领域
- 使用多轮搜索深化

---

## 📊 测试记录表

| 测试用例 | 状态 | 备注 |
|---------|------|------|
| 测试 1: MCP 连接 | ⬜ | |
| 测试 2: 基础搜索 | ⬜ | |
| 测试 3: 新闻搜索 | ⬜ | |
| 测试 4: 领域搜索 | ⬜ | |
| 测试 5: 内容提取 | ⬜ | |
| 测试 6: 综合搜索 | ⬜ | |
| 测试 7: 多轮搜索 | ⬜ | |

复制此表格到您的笔记中，测试完成后标记 ✅ 或 ❌

---

## 🚀 快速开始

**Step 1**: 运行测试脚本
```bash
bash test_tavily.sh
```

**Step 2**: 如需重启
```bash
bash ~/start_claude_deepseek.sh
```

**Step 3**: 验证连接
```
/mcp
```

**Step 4**: 开始测试
```
用 Tavily 搜索 AI 产品经理的技能要求
```

---

现在开始测试吧！🎉

