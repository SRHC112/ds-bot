# Memory

- 用户的 Bot 团队包括：写手（负责博客和文档产出）、程序员（负责写代码和部署方案），两人同在一个群里，群名「内容与开发组」，Chief 是管理员。 [source: part 1 #97; by: Chief; added: 2026-11-08]
- 用户希望由 Chief 统筹分工、向各 Bot 派活并跟进进度，协作任务直接派到对应群里。 [source: part 1 #97; by: Chief; added: 2026-11-08]
- 本机存在 SteamTools（BeyondDimension）HTTPS 拦截，Node 不信任其根证书，会报 unable to verify the first certificate；已导出双根证书到 steamtools-ca.pem 并设为用户环境变量 NODE_EXTRA_CA_CERTS，Node 实测可通（访问 api.github.com 返回 200）。 [source: part 2 #148; by: Chief; added: 2026-11-08]
- 记忆和配置备份位置：记忆在 E:\dsh-data\bot\memory\，配置在 ~/dsh-web-profile.yml 和 E:\dsh-data\profiles\。备份归档到 GitHub ds-bot 仓库 dsh-config/ 目录下的 config/ 和 memory/ 子目录 [source: part 3 #103; by: Chief; added: 2026-11-08]
- 月末提醒任务：每月最后一天 20:00 询问用户是否上传记忆，选项包括每步完成后上传、完工后统一上传、校园网模式（暂不传 GitHub 等待加速器）以及本月跳过。下次触发：2026-11-30 [source: part 3 #103; by: Chief; added: 2026-11-08]

## Index
