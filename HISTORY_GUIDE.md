# Claude Code 历史对话查看指南

## 查看历史对话的方法

### 方法 1：使用 /resume 命令（推荐）

在 Claude Code 中输入：
```
/resume
```

这会显示：
- 所有历史会话列表
- 每个会话的时间、项目、分支
- 最后几条对话内容预览

**使用方式**：
1. 输入 `/resume`
2. 用方向键（↑ ↓）选择想要恢复的会话
3. 按 `Enter` 恢复该会话
4. 或按 `ESC` 取消

### 方法 2：使用 --resume 参数启动

```bash
# 恢复最近的会话
claude --resume

# 或使用 --continue（继续上次会话）
claude --continue
```

### 方法 3：查看会话文件

所有会话都保存在文件中：
```bash
# 查看会话历史文件
cat ~/.claude/history.jsonl

# 更友好的查看方式
cat ~/.claude/history.jsonl | jq '.'
```

### 方法 4：使用 Ctrl+R 查看完整对话

在 Claude Code 运行时：
- 按 `Ctrl + R`：显示完整的对话记录（transcript mode）
- 可以看到所有输入输出、工具调用等
- 再按 `Ctrl + R` 或 `ESC` 退出查看模式

### 方法 5：导出会话

使用 `/export` 命令导出当前会话：
```
/export
```

这会生成一个包含完整对话的文件。

## 会话管理命令

### 查看当前会话信息
```
/status
```
显示：
- 当前模型
- Token 使用量
- 成本信息
- 会话时长

### 清除当前会话
```
/clear
```
清除当前对话，开始新会话（历史不会丢失）

### 查看所有可用命令
```
/help
```

## 历史记录存储位置

```bash
# 会话历史
~/.claude/history.jsonl

# 会话详细记录（按项目）
~/.claude/projects/

# 文件操作历史
~/.claude/file-history/

# 调试日志
~/.claude/debug/
```

## 实用技巧

### 1. 搜索历史会话
```bash
# 搜索包含特定关键词的会话
cat ~/.claude/history.jsonl | grep "关键词"
```

### 2. 查看最近 N 个会话
```bash
# 查看最近 10 个会话
tail -n 10 ~/.claude/history.jsonl | jq '.'
```

### 3. 按项目查看历史
```bash
# 查看特定项目的所有会话
ls -lt ~/.claude/projects/*/
```

### 4. 查看会话统计
```
/usage
```
显示当前会话的详细使用统计。

## 快捷键汇总

| 快捷键 | 功能 |
|--------|------|
| `Ctrl + R` | 查看完整对话记录（transcript） |
| `Ctrl + O` | 同上（旧版本） |
| `Ctrl + C` | 中断当前操作 |
| `ESC` x2 | 退出 Claude Code |
| `Tab` | 切换思考模式 |
| `Shift + Tab` | 切换自动接受模式 |

## 恢复会话的完整流程

1. **启动 Claude Code**：
   ```bash
   claude
   ```

2. **查看历史**：
   ```
   /resume
   ```

3. **选择会话**：
   - 用 ↑↓ 键浏览
   - Enter 恢复
   - ESC 取消

4. **继续对话**：
   恢复后可以继续之前的对话

## 注意事项

1. **会话不会丢失**：
   - 即使关闭终端，历史记录也会保存
   - 可以随时恢复

2. **多项目支持**：
   - 每个项目的会话独立保存
   - 可以在不同项目间切换

3. **会话限制**：
   - 单个会话有 5 小时时长限制
   - 超时后需要新建会话（历史仍保留）

4. **清理旧会话**：
   ```bash
   # Claude Code 会自动清理 30 天前的会话
   # 可以通过配置修改
   ```

## 快速参考

```bash
# 查看历史
/resume

# 查看完整对话
Ctrl + R

# 导出会话
/export

# 查看状态
/status

# 查看帮助
/help
```

现在您可以放心重启 Claude Code 了！所有历史都会保留。

