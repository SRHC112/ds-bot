# DSH 配置与记忆备份

用于跨设备迁移 DeepSeek Harness（DSH）的配置和 Bot 记忆。换新设备时，从这里取回文件即可恢复环境。

## 目录结构

```
dsh-config/
├── README.md        # 本说明
├── config/          # DSH 配置文件（dsh-web-profile.yml、profile 设置等）
└── memory/          # Bot 记忆（MEMORY.md、各主题 .md、team 记忆等）
```

## 源文件位置（Windows）

| 内容 | 本机路径 |
| --- | --- |
| Bot 记忆（个人） | `E:\dsh-data\bot\memory\bots\<session-id>\` |
| Bot 记忆（团队） | `E:\dsh-data\bot\memory\team\MEMORY.md` |
| 记忆快照/摘要 | `E:\dsh-data\bot\memory\snapshots\`、`summaries\` |
| Web 配置 | `C:\Users\Administrator\dsh-web-profile.yml` |
| Profile 配置 | `E:\dsh-data\profiles\desktop\`（仅 yml 配置，不含 node_modules） |

## 恢复步骤（新设备）

1. 安装 DSH 并完成登录。
2. 把 `memory/` 下的文件按原路径放回 `E:\dsh-data\bot\memory\` 对应位置（session-id 目录名不同的话，把记忆文件放进新 session 目录）。
3. 把 `config/` 下的配置文件放回对应路径。
4. 完全退出 DSH（托盘退出）后重新启动。

## 安全说明

- **绝不上传**：`.credentials.yaml`、`.env`、`secrets.json`、任何 token/密钥文件。
- 证书文件 `*.pem` 已被仓库 `.gitignore` 排除。
- 上传时机遵循用户指示（加速器/校园网规则）。
