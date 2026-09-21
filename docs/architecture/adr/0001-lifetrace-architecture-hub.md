# ADR-0001: 将 LifeTrace 主仓库重定位为 Architecture Hub

- Status: Accepted
- Date: 2026-09-21
- Scope: LifeTrace 系列整体

## Context

早期 `LifeTraceManage/LifeTrace` 采用 Monorepo，Desktop、Cloud、共享 crates、contracts、部署脚本和大量产品设计位于同一仓库。

随着 LifeTrace 演进，核心能力已经拆分到独立仓库：

- LifeTrace-execute
- LifeTrace-finance
- LifeTrace-assets
- LifeTrace-desktop
- LifeTrace-web
- LifeTrace-cloud
- LifeTrace-agent

继续把旧主仓库描述为运行 Monorepo 会产生以下问题：

- 开发者无法判断哪个仓库是生产 Source of Truth；
- 旧代码可能被误认为仍需维护；
- 跨应用边界散落在各个子仓库；
- 系统级设计缺少稳定入口；
- 新 Agent、Assets 等能力很容易重新形成隐式耦合。

## Decision

`LifeTraceManage/LifeTrace` 从现在起改为 **LifeTrace Architecture Hub**。

它只负责：

- 系统总架构；
- repository map；
- bounded-context ownership；
- cross-app contract 原则；
- data/sync/platform boundary；
- Agent integration boundary；
- Architecture Decision Records。

它不再负责：

- 任何生产应用源码；
- 产品级 CI / release；
- Cloud/Flutter/Desktop/Web 的构建；
- 某个领域内部的详细实现。

各子仓库是各自运行代码和产品设计的 Source of Truth。

## Consequences

正向：

- 系统架构有唯一入口；
- 子项目可以独立迭代和发布；
- 跨仓库变更可以通过 ADR 管理；
- Agent 和跨应用集成有明确边界；
- 后续增加新领域不会要求回到超级 Monorepo。

代价：

- 需要清理旧主仓库遗留源码和历史文档；
- 需要逐步修复各子仓库中仍指向旧 Monorepo 相对路径的说明；
- 公共协议变更需要更严格的版本与兼容管理。

## Migration

1. 先将 README 和 `docs/architecture/` 切换为新 Source of Truth；
2. 标记旧 Monorepo 内容为 legacy/non-production；
3. 后续单独提交删除旧 `apps/services/crates/contracts/deploy` 等运行代码；
4. 检查各子仓库 README 中遗留的 Monorepo 描述并修正；
5. 后续所有跨仓库边界变化新增 ADR。

## Affected repositories

全部 LifeTraceManage/LifeTrace-* 仓库。
