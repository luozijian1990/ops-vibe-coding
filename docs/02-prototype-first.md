# Prototype First

## 这一篇解决什么问题

把一个模糊想法变成"可体验、已批准"的产品意图，并在转成 React 时保留视觉与交互契约。它是整条工作流里成本最低、最能避免返工的阶段。

## 为什么这样设计

运维工程师通常知道"想做一个 Kubernetes 管理平台"，但很难一次说清页面结构、导航、信息密度、抽屉/弹窗、状态反馈。从 PRD 开始写，容易写成"描述一堆页面"的文档，验收时才发现理解不一致。

HTML Prototype 在写正式代码之前同时给出三样东西：

- **Executable Requirement**：点得动、看得到的可执行需求；
- **Visual Contract**：布局、颜色、字体、间距、密度已经确定；
- **Interaction Contract**：点击 → 弹窗 → 任务 → 状态变化的流程可以体验。

配套原则：

> 组件库服从已经批准的原型，原型不服从组件库。

## 怎么执行

1. **起点**：自己写 HTML，或从 [Beautiful Ops Pages](https://github.com/luozijian1990/Beautiful-Ops-Pages) / [Beautiful Ops Platform](https://github.com/luozijian1990/Beautiful-Ops-Platform) 选最接近的页面，替换业务文案与 Mock 数据。以 Kubernetes 操作平台为例：选 Cluster List 与 Deployment Detail，改成自己的集群命名与权限文案。
2. **覆盖**：至少一个完整用户流程，以及相关的 Loading、Error、Empty、Success 状态。
3. **人工体验**：让真实使用者点击，重点检查信息架构、导航、操作入口、Dialog/Drawer、筛选分页、状态变化、实时刷新、错误反馈、权限差异。
4. **记录批准**：`prototype/approval.md` 固定页面、关键流程与目标 viewport。
5. **转 React + Mock**：默认使用 `html-to-mui-react` 的 `fidelity` 模式保真转换；只有用户明确说视觉无需保留时才用 `mui-normalize`。
6. **Parity 门禁**：用 `prototype-parity-review` 在真实浏览器比较 Visual Parity 与 Interaction Parity，输出 `PASS` / `PASS_WITH_NOTES` / `FAIL`。

## 常见错误

- 跳过人工体验直接写 React：产品问题在代码成本最高时才暴露；
- 不用 fidelity 模式，默认全部变成标准 MUI Dashboard：已批准的原型视觉被洗掉；
- 不记录 viewport 与关键流程：后续 Parity 检查无从比对；
- 在 HTML 阶段就开始讨论数据库、MQ：原型只确认"怎么用"，不决定"用什么"。

## 门禁与下一步

- **门禁 1**：人明确批准产品意图（`approval.md`）；
- **门禁 2**：Parity 为 `PASS`（或人接受 `PASS_WITH_NOTES`）；`FAIL` 必须退回修复；
- **下一步**：进入 [Architecture and SDD](03-architecture-and-sdd.md)，从 Mock 暴露的能力反推架构。

## 相关文档

- [Full Track §1~4](../workflows/full.md)：本阶段对应流程
- [Artifact Map](../references/artifacts.md)：`prototype/approval.md`、`docs/reviews/*-prototype-parity.md`
- [Skill Map](../references/skills.md)：`html-to-mui-react`、`prototype-parity-review`
