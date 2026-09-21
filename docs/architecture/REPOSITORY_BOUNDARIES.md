# LifeTrace 仓库职责与边界

## 1. Repository Ownership Matrix

| 仓库 | Owns | Does not own |
| --- | --- | --- |
| `LifeTrace-execute` | Task、Project、Calendar、Collection/Inbox、Review、Focus、Goal/Habit、Today | 财务账本、资产生命周期、Agent workflow、Cloud identity |
| `LifeTrace-finance` | Ledger、Account、Transaction、Category、Budget、财务导入与分析 | 通用 Task、实物资产生命周期、Cloud 核心同步协议 |
| `LifeTrace-assets` | Asset、AssetEvent、估值、维护、保修/闲置提醒、资产 EntityLink | 财务交易主数据、Task 主数据 |
| `LifeTrace-desktop` | Tauri 桌面壳、本地文件/原生系统能力、桌面聚合体验 | 替代领域 App 的 authoritative domain model |
| `LifeTrace-web` | Browser experience、跨域在线展示与交互 | 直接成为 Local-first 主数据库、复制所有领域规则 |
| `LifeTrace-cloud` | Auth、Device、Session、Sync、Files、EntityLink、通用公共平台能力 | 各产品 UI、Agent 主循环、把所有领域规则集中到 Cloud |
| `LifeTrace-agent` | Conversation、Task runtime、Workflow、Capability、Evidence、HITL、Provider adapters | Execute/Finance/Assets 的业务实体所有权 |

## 2. 领域间引用

跨域引用使用稳定标识，例如：

```text
source_type = asset.asset
source_id   = <asset-id>
target_type = execution.task
target_id   = <task-id>
```

引用只表达“实体 A 与实体 B 有关系”，不意味着 Assets 可以修改 Task，也不意味着 Execute 可以修改 Asset。

## 3. Cloud 与客户端

客户端可以复用 Cloud 的公共协议，但 Cloud 不直接拥有客户端内部状态。

正确：

```text
Execute local transaction
  -> durable outbox
  -> Sync v1 push
  -> Cloud entity log
  -> other device pull
```

错误：

```text
Execute UI
  -> directly manipulate Cloud database schema
```

## 4. Desktop / Web 的聚合规则

Desktop 和 Web 可以提供“一处看全局”的体验，但聚合界面中的数据 ownership 不发生转移。

例如：

- Web 可以显示今日任务 + 本月支出 + 资产估值；
- 任务写入仍遵守 Execute contract；
- 财务写入仍遵守 Finance contract；
- 资产写入仍遵守 Assets contract。

## 5. Agent 的访问规则

Agent 对业务系统的访问必须经过 capability。

```text
LLM decision
   -> typed action
   -> capability runtime
   -> policy / auth / approval
   -> domain API / adapter
   -> evidence
```

禁止让 LLM 生成数据库路径、SQL 或内部函数名后直接修改另一个产品的数据。

## 6. 新仓库准入

创建新的 `LifeTrace-<domain>` 前，应满足至少一个条件：

- 有稳定且独立的 bounded context；
- 有独立发布需求；
- 有明显不同的数据生命周期或隐私边界；
- 有独立平台/运行时职责。

如果只是一个页面或一个小功能，应优先留在现有领域内。
