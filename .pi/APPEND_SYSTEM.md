# pi-team 项目规则（quant-agent）

> **Runtime: pi。** 团队统一为 pi-team、只有一套运行时（2026-11-23 取消 `pi-alt` 双身份）；本仓库 public，改动走分支 + PR + CI。

本文件由 pi 在 cwd 位于本仓库（或其 worktree）时自动追加进系统提示。只写本仓库特有的 pi 约定；团队通用规则在全局 `~/.pi/agent/AGENTS.md`。

## 1. 运行时与规则

- 本仓库是开源项目，归 pi-team；用 pi 的 `/add`、`/commit`、`/bump`、`/release` 流程。
- 版本以仓库根 `VERSION` 为唯一权威；改动记仓库根 `CHANGELOG.md`。
- 通用工作规范（verify-before-report / file-operation-priority / tmp-dir-for-artifacts）由全局规则加载，不在这里重述。

## 2. 子项目

- 研究工作全在 Mark 的子项目独立仓库里做（`xhqing/Intraday` / `xhqing/Swing` / `xhqing/gridtrader` 为公开，NewsScoreStudy / hyperliquid-research-trial / market-data-backup 为私有）；本仓库只装 agent 项目本身。
- 子项目的规则写在它自己的 `AGENTS.md` 与 `.pi/APPEND_SYSTEM.md`（私有项目同样用 `.pi/`）——pi 只加载 cwd 与祖先目录的规则，**动哪个子项目，就在那个子项目的目录 / worktree 里开会话**，不要在本仓库里按路径去改它。
- 数据与运行时产物不入库；tick / 订单簿大件在 macmini，需要时用 `macmini_run` 在那边跑；其它快照走私有仓库 `market-data-backup` 的 Release。

## 3. agentLeader / subLeader / Worker

- **判据只有两条**：cwd 所在项目 + 当前是否停在 `main`（主工作区被切到非 main 分支，同样按 Worker 算）。
- **agentLeader** = 本仓库主工作区（main 分支）：管本仓库 + 名下全部子项目——两者的 local main 只做 `fetch` + `merge --ff-only`，另负责建删 worktree、派单、合并、发版、收尾。
- **subLeader** = 子项目主工作区（main 分支）：只管自身项目——自身 local main 的 ff 同步、从自身 main 建删任务 worktree、派 Worker、合并自身 PR、发自身版本、回收 Worker；对上级与 sibling 子项目只读。
- **Worker** = 任何项目的非 main 分支 worktree：只干当前 worktree 的活，干完向派它的 Leader 汇报；不碰 local main、不建删 worktree、不合并 PR、不发版、不写 worktree 以外的项目。
- 本仓库有私有子项目 **NewsScoreStudy**、**hyperliquid-research-trial**（均 `github.com/xhqing/<名字>`，private；只列名，不介绍内容）与三个公开子项目仓库（Intraday / Swing / gridtrader），各自有自己的 `AGENTS.md` 与 `.pi/APPEND_SYSTEM.md`。
- 用 pi 的 `agent_call` / `agent_wake` 派活：`dir` 传目标工作区绝对路径——子项目主工作区（main）包出 subLeader、子项目 worktree 包出 Worker；要交给某个子项目，那个子项目必须是独立仓库并已登记。
- 详见全局 `~/.pi/agent/AGENTS.md`「Leader / Worker 的定义」与 `docs/agents-registry.md` 第 5 条。

## 4. 其他 pi 约定

- `.commit-cache.md` 是 pi 的提交缓存，按 pi 的 commit skill 维护。
- 私有子项目按可见性走本地门禁（`pa-merge.sh` + `checks.sh`），公开子项目走 PR + CI；规则写在各自仓库里，本仓库不代管。