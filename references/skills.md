# Skill Map

可执行 Skill 独立维护在 [`personal-skill`](https://github.com/luozijian1990/personal-skill)。本项目只描述它们在流程中的职责和调用顺序。

| 阶段 | Skill / 能力 | 职责边界 |
|---|---|---|
| React + Mock | `html-to-mui-react` | 默认保真转换；显式选择时才统一为 MUI 视觉 |
| Prototype Parity | `prototype-parity-review` | 检查已批准 HTML 与 React 的视觉和交互一致性 |
| Architecture | `stack-selector` | Pass 1 生成 Draft；Pass 2 消费 Grill 决策生成 Final |
| Grill | `grill-me` / `grill-with-docs` | 补齐运行、安全、可靠性和基础设施约束 |
| Spec / Tickets | `to-spec` / `to-tickets` | 定义可执行契约并拆分纵向工作单元 |
| Implementation | `tdd` | 以红—绿—重构和可复现验证实现单个 Ticket |
| Code Review | `code-review` | 检查仓库规范与 Spec 符合度 |
| Ops Review | `ops-readiness-review` | 检查生产运行、操作安全和恢复能力 |
| E2E | `playwright-e2e-debug-report` | 经人工批准后执行真实浏览器 E2E 并输出报告 |

Skill 是执行单元，Workflow 是编排和门禁。不要把整条流程塞进一个超大 Skill，也不要让单个 Skill 越权替代人的批准。
