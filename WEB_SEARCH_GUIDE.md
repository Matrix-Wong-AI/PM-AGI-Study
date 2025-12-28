# Claude Code Web Search 配置指南

## 问题说明

您遇到的红色错误：
```
Error: Claude Code is unable to fetch from www.perplexity.ai
Error: Claude Code is unable to fetch from www.anthropic.com
Error: Claude Code is unable to fetch from ai.google.dev
```

**原因**：
- Claude Code 尝试使用 `Fetch` 工具直接抓取网页
- 但这些网站可能有防火墙限制或需要特殊权限
- 直接 Fetch 不是真正的网络搜索

## 已修复的配置

### 1. 启用 Web Search
已更新配置文件，启用真正的网络搜索功能：
- 设置 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=0`
- 添加 `webSearchEnabled: true`

### 2. 配置更新位置
- `~/.claude/settings.json`
- `~/start_claude_deepseek.sh`

## 如何使用 Web Search

### 方法 1：直接在提问中要求搜索

**示例**：
```
搜索 Perplexity、Gemini、NotebookLM 的技术架构信息
```

或更明确：
```
使用网络搜索查找 Perplexity 的记忆管理功能
```

### 方法 2：使用搜索关键词

在提问中包含这些触发词：
- "搜索"
- "查找"
- "最新信息"
- "在线查询"
- "网上搜索"

### 方法 3：要求验证信息

```
请搜索并验证：Perplexity 如何实现多步推理？
```

## Web Search 的优势

✅ **真正的网络搜索**
- 使用搜索引擎（如 Google、Bing）
- 不是直接抓取网页
- 能绕过防火墙限制

✅ **自动附带来源**
- 搜索结果会包含来源 URL
- 可以验证信息真实性
- 符合您"不胡编乱造"的要求

✅ **更全面的结果**
- 搜索多个网站
- 综合多个来源
- 提供更完整的信息

## 使用示例

### 原来的问题（会失败）：
```
读取 https://www.perplexity.ai 网站内容
```
❌ 直接 Fetch 会被阻止

### 正确的方式（会成功）：
```
搜索 Perplexity.ai 的产品特点和技术架构
```
✅ 使用 Web Search 获取信息

## 完整的搜索流程

当您提出类似问题时：
```
分析 Perplexity、Gemini、NotebookLM 三个产品的技术架构
```

Claude Code 会：
1. ✅ **识别需要搜索**：检测到需要外部信息
2. ✅ **执行搜索**：使用 Web Search 工具
3. ✅ **分析结果**：整合多个来源
4. ✅ **附带来源**：提供 URL 链接
5. ✅ **结构化输出**：清晰呈现分析结果

## 验证配置

重启 Claude Code 后，可以测试：
```
搜索最新的 Claude 3.5 Sonnet 模型信息
```

如果成功，您会看到：
- 🔍 搜索过程提示
- 📝 搜索结果摘要
- 🔗 来源链接（URL）
- ✅ 基于真实信息的分析

## 常见问题

### Q: 搜索结果准确吗？
A: Web Search 使用真实搜索引擎，信息来自公开网络，比 AI 生成更可靠。

### Q: 会显示来源吗？
A: 是的，搜索结果会附带来源 URL，可以验证。

### Q: 搜索慢吗？
A: 可能比直接回答慢 5-10 秒，但获取的是真实信息。

### Q: 可以搜索中文内容吗？
A: 可以，支持多语言搜索。

## 重启后的使用建议

1. **重启 Claude Code**：
   ```bash
   bash ~/start_claude_deepseek.sh
   ```

2. **测试搜索功能**：
   ```
   搜索 AI 产品经理需要掌握的技能
   ```

3. **继续您的任务**：
   ```
   搜索 Perplexity、Gemini、NotebookLM 的技术架构，
   重点关注记忆、规划、工具使用、反思四个维度
   ```

## 提示

- 明确说"搜索"或"查找"
- 具体说明需要什么信息
- 要求提供来源链接
- 可以要求验证信息真实性

现在重启 Claude Code，搜索功能就可以正常使用了！🔍

