# LifeTrace Architecture

本目录是 LifeTrace 系列的系统级架构 Source of Truth。

## 文档结构

| 文档 | 说明 |
| --- | --- |
| [SYSTEM_ARCHITECTURE.md](SYSTEM_ARCHITECTURE.md) | 系统整体分层、运行关系和长期目标架构 |
| [REPOSITORY_BOUNDARIES.md](REPOSITORY_BOUNDARIES.md) | 每个仓库拥有什么、不能拥有什么、与其他仓库如何协作 |
| [DATA_AND_SYNC.md](DATA_AND_SYNC.md) | Local-first、Cloud Sync、EntityLink、文件和跨域数据原则 |
| [AGENT_ARCHITECTURE.md](AGENT_ARCHITECTURE.md) | Agent Runtime、Workflow、Capability、Browser 和业务系统边界 |
| [ARCHITECTURE_GOVERNANCE.md](ARCHITECTURE_GOVERNANCE.md) | ADR、OpenSpec、跨仓库变更、版本与兼容性治理 |
| [adr/](adr/) | Architecture Decision Records |

## 设计目标

LifeTrace 不是一个“大仓库中的超级应用”，而是一个由多个 **bounded context + shared platform** 组成的个人数字系统。

系统级文档只定义稳定边界：

- 哪个仓库拥有哪个领域；
- 哪些能力必须由 Cloud 提供；
- 哪些数据必须 local-first；
- 跨应用如何建立链接而不复制实体；
- Agent 如何调用业务能力；
- 哪类改动需要跨仓库 ADR。

具体页面、表结构、内部类名和实现细节留在各产品仓库中维护。
