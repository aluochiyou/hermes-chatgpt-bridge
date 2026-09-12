# quota-smart-codex

一个额度意识型 Codex 工作流 Skill：让 ChatGPT Web 通过最小权限连接器负责规划与审查，Codex 保留本地修改、终端、测试、Git 和故障恢复的执行权。

本仓库只发布 Skill 本身，不包含连接器实现、完整搭建教程或第三方项目代码。它不会绕过、增加或共享 ChatGPT/Codex 额度，只用于减少不必要的重复推理和上下文传递。

## 安装

将本仓库克隆或下载到 Codex skills 目录，并确保最终结构如下：

```text
~/.codex/skills/quota-smart-codex/
└── SKILL.md
```

随后重新启动 Codex，或开启一个新任务让 Skill 被重新发现。

## 使用

可以直接说：

```text
使用 quota-smart-codex：让 ChatGPT Web 负责计划和复核，Codex 负责实现、测试和最终验证。
```

默认采用只读工作区连接器。只有用户明确授权时，才允许切换到能写文件或运行命令的连接方式。

## License

[MIT](LICENSE)
