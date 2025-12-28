# Claude Code 登录指南

## 两种使用方式

### 方式 1：使用硅基流动 API（第三方，成本较低）
**问题**：当前账户余额不足
**解决**：
1. 登录 https://cloud.siliconflow.cn/
2. 充值账户
3. 使用启动脚本：`bash ~/start_claude_deepseek.sh`

**优点**：成本较低
**缺点**：需要第三方账户和充值

---

### 方式 2：使用 Anthropic 官方服务（推荐）
**步骤**：

1. **启动 Claude Code（官方登录）**：
   ```bash
   bash ~/start_claude_official.sh
   ```
   或直接运行：
   ```bash
   claude
   ```

2. **在 Claude Code 中登录**：
   - 运行命令：`/login`
   - 选择登录方式：
     - **选项 A**：使用 Anthropic 账户（需要 Claude Pro/Max 订阅）
     - **选项 B**：使用 API Key（从 console.anthropic.com 获取）

3. **获取 Anthropic API Key**（如果选择选项 B）：
   - 访问：https://console.anthropic.com/
   - 注册/登录账户
   - 进入 API Keys 页面
   - 创建新的 API Key
   - 复制完整的 Key（格式：`sk-ant-api03-...`）

4. **完整复制授权码**：
   - 如果使用 OAuth 登录，确保复制完整的授权码
   - 不要有多余的空格或换行

**优点**：
- 官方服务，稳定可靠
- 使用最新的 Claude 模型（Sonnet 4, Opus 4）
- 无需第三方账户

**缺点**：
- 需要 Anthropic 账户
- 可能需要订阅或 API 额度

---

## 当前错误解释

```
OAuth error: Invalid code. Please make sure the full code was copied
```

**含义**：OAuth 授权码不完整或无效

**原因**：
- 复制授权码时不完整
- 授权码已过期
- 或复制时包含了多余字符

**解决**：
- 重新运行 `/login`
- 仔细复制完整的授权码
- 确保没有多余的空格

---

## 快速决策

**如果您有 Claude Pro/Max 订阅**：
→ 使用方式 2（官方服务）

**如果您只想低成本使用**：
→ 充值硅基流动账户，继续使用方式 1

**如果您有 Anthropic API Key**：
→ 使用方式 2，选择 API Key 登录

---

## 下一步操作

1. **选择使用哪种方式**（见上述决策）

2. **如果使用官方服务**：
   ```bash
   bash ~/start_claude_official.sh
   ```
   然后运行 `/login`

3. **如果继续使用硅基流动**：
   - 登录控制台充值
   - 重启 Claude Code




