# Artifact Map

| 阶段 | 最小产物 | 下一阶段如何消费 |
|---|---|---|
| Explore | `prototype/*.html` | 人工体验页面与流程 |
| Prototype Review | `prototype/approval.md` | 固定页面、Flow、viewport 和模式 |
| React + Mock | 可运行前端、Mock API/Data | Parity 检查并暴露能力候选 |
| Parity | `docs/reviews/*-prototype-parity.md` | 决定是否进入架构阶段 |
| Architecture Draft | `docs/plans/*-stack-selection.draft.md` | Grill 逐项确认开放约束 |
| Grill | 决策记录或 ADR | Final 映射每项约束 |
| Architecture Final | `docs/plans/*-stack-selection.md` | Spec 固定组件和部署边界 |
| SDD | Spec | Tickets 提取纵向验收切片 |
| Tickets | 本地 Markdown 或 GitHub Issues | 每个新会话只消费一个 Ticket |
| TDD | 代码、测试、验证结果 | Review 检查实现与契约 |
| Code Review | Review 报告 | 修复阻断问题或进入 Ops Review |
| Ops Review | `docs/reviews/*-ops-readiness.md` | 决定是否具备 E2E / 生产验收基础 |
| E2E | Test Plan、证据与报告 | 人执行最终业务和风险判断 |

产物的价值是传递已确认事实和证据，不是增加文档数量。小项目可以合并文件，但不能丢失关键决策来源、验证状态和人工门禁。
