# Binance / Deribit / Derive Markdown 文档合集

> Looking for English? See `README.md`.

## 项目简介
本仓库将 Binance、Deribit 以及 Derive 的官方文档整理为 Markdown 版本，方便离线浏览、全文检索与个性化引用，是量化交易开发者与风控人员快速查阅交易所接口规则的理想资料库。

## 核心亮点
- **离线可读**：克隆后即可在本地阅读，适合网络受限或希望在本地留档的场景。
- **结构清晰**：每个交易所文档均保持目录规范，便于定位 API、参数或示例。
- **快速检索**：借助编辑器或 `rg` 等工具，可秒级定位关键字段、错误码或风控说明。
- **同步更新**：文件直接来源于官方文档，可按需拉取更新，始终与官方保持一致。

## 仓库结构
- `binance/`：涵盖 Binance 现货、合约、行情与用户数据流 API。
- `deribit/`：涵盖 Deribit 永续/期权接口、认证流程及错误码。
- `derive/`：Derive 官方文档的 Markdown 版本，涉及交易、账户等章节。

## 使用方式
1. **克隆仓库**  
   `git clone https://github.com/<your-account>/<repo>.git`
2. **浏览子目录**  
   进入 `binance/`、`deribit/` 或 `derive/`，使用任意 Markdown 查看器或 IDE 阅读。
3. **全文检索**  
   使用 `rg "关键字" -n binance` 等命令进行匹配，也可以在 IDE 中全局搜索。
4. **保持最新**  
   定期运行 `git pull` 同步官方更新，或在有新版本时提交 PR 协助维护。

## 贡献方式
- 发现错误或遗漏欢迎在 Issue 区反馈或直接提交 Pull Request。
- 请在描述中注明章节来源与官方更新日期，方便大家确认内容同步。

## 版权与声明
内容均整理自 Binance、Deribit、Derive 的官方公开文档，版权归原团队所有。本仓库仅用于便捷查阅与学习，请遵守各交易所使用条款。

---

如果你也需要高效查阅交易所文档，欢迎 Star / Fork，并分享给更多开发者！
