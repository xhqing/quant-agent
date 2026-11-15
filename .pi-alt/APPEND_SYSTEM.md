# pi-alt-team 项目规则（quant-agent）

> **Runtime: pi-alt-team。** 本文件只适用于 `pi-alt` 身份；如果你在 `pi` 下运行，忽略本文件，见 `.pi/APPEND_SYSTEM.md`。

本文件由 pi 在 cwd 位于本仓库（或其 worktree）时原生加载（身份 `pi-alt` 的 configDir 是 `.pi-alt`）。只写本仓库特有的 pi-alt-team 约定；团队通用规则在全局 `~/.pi-alt/agent/AGENTS.md`。

## 1. 运行时与边界

- 本仓库是开源的 pi-team 项目；用 `pi-alt` 在这里做闭源 / 私有量化研究时，**不要把闭源策略、客户数据、账户信息写进本仓库的跟踪文件**。闭源内容放独立私有子项目，或放本机被 `.gitignore` 忽略的目录。
- 不用 pi-team 的 `/commit`、`/add`、`/release`、`agent_call`、`commit-gate`、`private-links` 等工具；按本文件与项目自身规则执行。
- 凭证永远不入库；财务 / 账户数据即使闭源也只放本机被忽略的文件或私有子项目。
- 研究和实盘完全隔离：不下单、不接证券账户、不发实时信号。

## 2. 三个研究目录

- `pi` 与 `pi-alt` 都只自动加载 cwd 与祖先目录的 `AGENTS.md`；动 `Intraday/` / `Swing/` / `gridtrader/` 前仍要显式读各自的 `AGENTS.md`。
- 数据与运行时产物不入库；tick / 订单簿数据在 macmini，用 `ssh mac-mini` 在那边跑，不往本机拉 25GB。

## 3. Leader / Worker（pi-alt）

- 本仓库是 **Agent 项目**：任何分支 / worktree 的根目录作为 cwd 的 `pi-alt` 会话，都是 **Leader**。
- 本仓库有一个私有子项目 **NewsScoreStudy**（private；pi-alt-only，`.pi-alt` + 实体 `AGENTS.md`）。本仓库任何 worktree = Leader；NewsScoreStudy 的 worktree = Worker。
- Leader 用 shell / tmux 建 worktree、派任务、收尾；worktree 里不切 main、不 commit main。
- 本仓库自己的任务 worktree 仍是 Leader；只有 NewsScoreStudy 的 worktree 才是 Worker。
- Worker 会话用 tmux 直接起 `pi-alt`，不要用 pi 的 `agent_call` 跨身份拉起。

## 4. 交付与记录

- 回测结论只认同口径复现；开源仓库内的改动记仓库根 `CHANGELOG.md`，私有内容记私有项目自己的 CHANGELOG。
- 不要混用 `.pi/APPEND_SYSTEM.md` 的 pi-team 规则；两套运行时规则各管各的。
