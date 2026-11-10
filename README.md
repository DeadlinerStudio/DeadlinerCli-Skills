# Deadliner CLI Skills

配套 [DeadlinerCli](https://github.com/DeadlinerStudio/DeadlinerCli) 的两个 Agent Skills：

- `skills/deadliner-tasks`：本地任务增删改查、状态动作、条件更新和重试。
- `skills/deadliner-webdav`：环境变量配置、Core V3 双向同步及故障处理。

先安装 CLI，再将需要的技能目录复制到 Agent 的 skills 目录。

```sh
npm install -g https://github.com/DeadlinerStudio/DeadlinerCli/releases/download/v0.2.0/deadlinerstudio-cli-0.2.0.tgz
```

例如 Codex 使用 `~/.codex/skills/`。本仓库不自动安装技能或修改用户配置。技能也可由支持 SKILL.md 的其他 Agent 使用。

CLI 首版运行平台：macOS Apple Silicon。输入输出及命令以 `deadliner schema` 为准。
