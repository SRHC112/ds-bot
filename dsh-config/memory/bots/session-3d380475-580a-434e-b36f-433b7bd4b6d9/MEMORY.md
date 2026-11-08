# Memory

- GitHub 已连接可用；用户仓库 https://github.com/SRHC112/ds-bot（main 分支，已有 README.md、.gitignore 初始提交 ef4c6da），后续文件/代码上传到该仓库；本地证书 steamtools-ca.pem 等敏感文件不得上传（.gitignore 已含 *.pem）。 [source: part 2 #295; added: 2026-11-08]
- DSH 会显式清空 NODE_OPTIONS 环境变量（防代码注入），用它传参路走不通，需改用 NODE_EXTRA_CA_CERTS 并指向 C:\Users\Administrator\Desktop\ds-bot-work\steamtools-ca.pem [source: part 2 #330; added: 2026-11-08]
- 修改用户级环境变量后，需完全退出 DeepSeek Harness（托盘退出）并从开始菜单或桌面快捷方式重新启动；应用内的「重启」按钮会继承旧进程环境，新变量不生效。 [source: part 2 #216; added: 2026-11-08]
- Bot 指令设计风格（用户 2026-11-08 定）：参考桌面三份 Grok 模板（nightly-audit-engineer.zh-CN.md、ai-search-visibility.zh-CN.md、engineer-bot.zh-CN.md），采用编号硬性「记忆」规则 + 技能 + 例行任务的结构，同时保留各 Bot 原有分工与归档规范 [source: part 3 #51; added: 2026-11-08]
- 用户希望以后在 GitHub 上传文件时，若使用校园网则先不上传，保存至本地工作文件夹；等到使用加速器后再由 Chief 上传至 GitHub。 [source: part 3 #86; added: 2026-11-08]

## Index
- [[本机环境]]
- [[github归档规范]]
- [[工作流程]]
- [[上传时机]]
- [[plugin_check]]
- [[upload_timing]]
- [[team_-coordination]]
