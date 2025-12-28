# 硅基流动 API 配置说明

## ⚠️ 重要提示

**404 错误的可能原因：**
Claude Code 使用 Anthropic API 格式（`/v1/messages`），而硅基流动可能只支持 OpenAI 兼容格式（`/v1/chat/completions`）。如果持续出现 404 错误，可能需要：
1. 确认硅基流动是否支持 Anthropic API 兼容格式
2. 或使用兼容层/代理服务

## 已配置信息

- **API Key**: `sk-vvhxbohnrmotamsvrzxrhdtyqdlccbsgnyzfmjusnvfotvcv`
- **API Base URL**: `https://api.siliconflow.cn`
- **模型**: `zai-org/GLM-4.7`（注意：不是 `Pro/zai-org/GLM-4.7`）

## 配置文件位置

1. **全局配置**: `~/.claude/settings.json`
2. **项目配置**: `.cursor/settings.json`
3. **启动脚本**: `/Users/huan/start_claude_deepseek.sh`

## 配置内容

### 全局配置文件 (`~/.claude/settings.json`)
```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-vvhxbohnrmotamsvrzxrhdtyqdlccbsgnyzfmjusnvfotvcv",
    "ANTHROPIC_AUTH_TOKEN": "sk-vvhxbohnrmotamsvrzxrhdtyqdlccbsgnyzfmjusnvfotvcv",
    "ANTHROPIC_BASE_URL": "https://api.siliconflow.cn",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "zai-org/GLM-4.7",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "zai-org/GLM-4.7",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "zai-org/GLM-4.7"
  }
}
```

## 故障排除

### 如果仍然出现 404 错误：

1. **检查 API 兼容性**：
   - 确认硅基流动是否支持 Anthropic API 格式
   - 访问硅基流动文档查看 API 格式要求

2. **验证配置**：
   ```bash
   # 检查环境变量
   echo $ANTHROPIC_BASE_URL
   echo $ANTHROPIC_MODEL
   ```

3. **测试 API 连接**：
   ```bash
   curl -X POST https://api.siliconflow.cn/v1/chat/completions \
     -H "Authorization: Bearer sk-vvhxbohnrmotamsvrzxrhdtyqdlccbsgnyzfmjusnvfotvcv" \
     -H "Content-Type: application/json" \
     -d '{"model":"zai-org/GLM-4.7","messages":[{"role":"user","content":"Hello"}]}'
   ```

4. **重启 Claude Code**：
   - 完全退出 Claude Code
   - 重新运行启动脚本或 `claude` 命令

## 替代方案

如果硅基流动不支持 Anthropic API 格式，可以考虑：
1. 使用支持 Anthropic API 格式的其他服务（如智谱AI的兼容接口）
2. 使用兼容层/代理服务
3. 联系硅基流动技术支持确认 API 兼容性



