# Claude Code 无响应问题排查

## 问题现象
- Claude Code 已启动
- 显示 `deepseek-chat` 模型
- 输入提示符显示 "记忆："
- 输入后无输出

## 可能的原因

### 1. API 配置问题
虽然启动成功，但 API 调用可能失败

### 2. 会话状态问题
可能有残留的会话状态导致卡住

### 3. 终端输入模式问题
可能处于特殊输入模式（如记忆模式）

## 解决方案

### 方案 1：退出当前模式（快速）
1. 按 `ESC` 键（可能需要按 2 次）
2. 或按 `Ctrl + C`
3. 然后重新输入您的问题

### 方案 2：清除会话缓存
```bash
# 退出 Claude Code
# 然后清除会话缓存
rm -rf ~/.claude/session-env/*
rm -rf ~/.claude/shell-snapshots/*

# 重新启动
claude
```

### 方案 3：完全重启
```bash
# 1. 关闭所有 Claude Code 终端
# 2. 重新打开终端
# 3. 运行
bash ~/start_claude_deepseek.sh
```

### 方案 4：验证配置
运行测试脚本确认 API 正常：
```bash
cd "/Users/huan/Downloads/AI Learning/PM-AGI-Study"
python3 test_anthropic_compat.py
```

### 方案 5：检查是否处于特殊模式
从截图看到 "记忆："，可能进入了记忆模式：
- 按 `ESC` 退出
- 或直接输入内容后按 `Enter`

## 临时解决：使用新终端
如果上述方法都不行：
1. 关闭当前 Claude Code
2. 打开新的终端窗口
3. 运行 `claude`

## 快速测试命令
```bash
# 测试 1：检查进程
ps aux | grep claude

# 测试 2：验证 API
curl -X POST https://api.deepseek.com/anthropic/v1/messages \
  -H "x-api-key: sk-cda9f10761b74e90805dec92289664f5" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-chat","max_tokens":50,"messages":[{"role":"user","content":"test"}]}'
```

## 建议操作顺序
1. 先按 `ESC` 或 `Ctrl + C`
2. 如果还是卡住，关闭终端
3. 打开新终端，运行 `claude`
4. 如果问题仍存在，运行清除缓存命令

尝试这些方法，告诉我结果！

