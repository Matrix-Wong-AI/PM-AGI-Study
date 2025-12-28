# 账户余额问题排查指南

## 问题描述
账户有余额，但仍然出现 `403 Sorry, your account balance is insufficient` 错误。

## 可能的原因

### 1. API Key 绑定问题
- **问题**：API Key 可能绑定到了不同的账户
- **解决**：在硅基流动控制台检查该 API Key 属于哪个账户

### 2. API Key 状态问题
- **问题**：API Key 可能被禁用或过期
- **解决**：检查 API Key 状态，必要时重新生成

### 3. 模型名称问题
- **问题**：模型名称可能不正确，导致计费异常
- **解决**：确认正确的模型名称格式

### 4. API 格式兼容性问题
- **问题**：Claude Code 使用 Anthropic API 格式，可能不兼容
- **解决**：可能需要使用兼容层或不同的配置方式

## 排查步骤

### 步骤 1: 测试 API Key
运行测试脚本：
```bash
python3 test_api.py
```

这将直接测试 API Key 是否正常工作。

### 步骤 2: 检查硅基流动控制台
1. 登录 https://cloud.siliconflow.cn/
2. 检查：
   - API Key 列表，确认当前使用的 Key 状态
   - 账户余额
   - API 调用记录，查看是否有错误日志
   - 模型权限，确认是否有权限使用 GLM-4.7

### 步骤 3: 验证模型名称
尝试不同的模型名称格式：
- `Pro/zai-org/GLM-4.7`
- `zai-org/GLM-4.7`
- `GLM-4.7`

### 步骤 4: 重新登录 Claude Code
在 Claude Code 中运行：
```
/login
```
然后重新输入 API Key

### 步骤 5: 检查环境变量
确认环境变量是否正确设置：
```bash
echo $ANTHROPIC_API_KEY
echo $ANTHROPIC_BASE_URL
```

## 快速修复

### 方案 1: 重新生成 API Key
1. 在硅基流动控制台删除旧 Key
2. 创建新的 API Key
3. 更新配置文件中的 Key

### 方案 2: 使用 /login 命令
在 Claude Code 中：
1. 运行 `/logout` 退出当前登录
2. 运行 `/login` 重新登录
3. 输入新的 API Key

### 方案 3: 清除缓存
```bash
# 清除 Claude Code 的会话缓存
rm -rf ~/.claude/session-env/*
```

## 联系支持
如果以上方法都不行，建议：
1. 联系硅基流动技术支持
2. 提供 API Key（前几位）和错误信息
3. 询问 Anthropic API 兼容性支持




