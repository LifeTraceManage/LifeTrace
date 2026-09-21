# ADR-0002: 删除旧 Monorepo 当前分支内容

- Status: Accepted
- Date: 2026-09-21
- Scope: `LifeTraceManage/LifeTrace`

## Context

ADR-0001 已将主仓库重新定位为 Architecture Hub，但当前分支仍保留旧 Monorepo 的应用源码、Cloud 服务、共享 crates/contracts、部署脚本、CI workflow 和大量产品级 EPIC 文档。

即使这些内容被文字标记为 legacy，代码搜索、AI 上下文检索和开发者浏览仍可能把它们当成当前实现，导致：

- 错误判断当前技术栈和仓库边界；
- 在旧代码上继续开发；
- 重复实现已经拆出的子项目；
- 从过期 contract 推断当前接口；
- 旧 CI 在没有意义的代码路径上继续运行。

## Decision

当前分支删除所有旧 Monorepo 运行内容，只保留：

- `README.md`
- `AGENTS.md`
- `.gitignore`
- `docs/README.md`
- `docs/architecture/**`

同时删除旧产品级 GitHub Actions workflow。

历史源码不做 history rewrite，仍保留在 Git commit history 中，用于必要的考古与追溯。

## Consequences

### Positive

- AI 检索不会再混入旧源码；
- 当前仓库结构与“Architecture Hub”定位完全一致；
- 不再存在误触发的旧产品 CI；
- 开发者能快速定位真正的产品仓库。

### Trade-offs

- 查看旧实现需要显式浏览 Git history；
- 旧文档中的部分细节不再能通过当前分支搜索；
- 如果某个历史设计仍有长期价值，需要人工提炼为新的 ADR，而不是原样恢复旧文档。

## Guardrail

未来不得为了参考方便重新复制子仓库源码到本仓库。需要跨仓库说明时，只记录稳定架构事实并链接到 authoritative repository。
