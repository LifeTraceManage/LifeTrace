# LifeTrace 架构治理

## 1. 文档分工

LifeTrace 使用两级设计治理：

### Architecture Hub / ADR

适用于跨仓库、不可局部决定的问题，例如：

- 新增/拆分一个产品仓库；
- 改变 Cloud 的职责；
- 改变 Auth/Sync/EntityLink 总协议；
- 改变 Agent 与业务应用的访问模式；
- 改变 authoritative owner；
- 引入新的跨应用身份体系。

### Repository OpenSpec

适用于单仓库内部的可执行变更，例如：

- Execute 增加 recurrence；
- Assets 增加估值策略；
- Agent 增加 Journey workflow；
- Cloud 增加一个明确 API；
- Web 重构某个信息架构。

如果一个 OpenSpec 会改变其他仓库的公共边界，应先有 Architecture ADR。

## 2. ADR 生命周期

```text
Proposed -> Accepted -> Superseded
                   \-> Rejected
```

ADR 最少包含：

- Context；
- Decision；
- Alternatives；
- Consequences；
- Migration；
- Affected repositories。

ADR 不记录普通实现细节。

## 3. 跨仓库变更流程

```text
1. Identify ownership
2. Decide whether system boundary changes
3. If yes -> architecture ADR
4. Define/update public contract
5. Create OpenSpec in affected repositories
6. Implement producer first when compatibility requires
7. Run contract/E2E tests
8. Roll out consumers
9. Archive OpenSpec and update architecture docs if needed
```

## 4. 兼容性

公共协议必须显式版本化。

推荐：

- additive change 优先；
- 老客户端在合理窗口内继续可用；
- breaking change 必须给 migration path；
- contract tests 覆盖序列化和权限；
- 不能依赖“所有仓库同一天更新”。

## 5. 架构审查问题

每个跨域需求先回答：

1. 这个实体真正属于谁？
2. 是否需要在离线状态工作？
3. 是否需要跨设备同步？
4. 是否只是引用另一个实体？
5. Cloud 是否真的需要知道业务细节？
6. Agent 是在“决策/编排”，还是在偷偷接管领域模型？
7. 是否造成两个 authoritative copies？
8. 该能力能否独立发布？
9. 出现旧客户端时会怎样？
10. 删除账号时数据如何完整清理？

如果这些问题没有答案，不应直接开始跨仓库实现。
