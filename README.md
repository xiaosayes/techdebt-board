# TechDebt Board — 双人 Vibe Coding 实操项目

轻量技术债看板：记录、流转（待处理/处理中/已解决）、统计团队技术债。
两人用 Claude Code 按完整多人协作流程（契约先行 → 任务拆分 → 并行开发 → PR 评审 → 联调 → 验收）开发本项目。

## 快速开始

**唯一必读**：[实操手册.md](./实操手册.md) —— 你和同事 A 的全部操作步骤、所有 AI prompt、验收清单。

## 目录结构

```
├── AGENTS.md                  # 项目规范（AI 和人共同遵守）
├── docs/
│   ├── contracts/
│   │   └── openapi.yaml       # 接口契约（唯一事实源）
│   ├── task-briefs/           # 任务卡（T0/T1/T2/T3）
│   └── acceptance.md          # 8 条验收清单
├── .github/
│   └── pull_request_template.md
├── backend/                   # 开发阶段生成（Node + Express + SQLite）
└── frontend/                  # 开发阶段生成（React + Vite + TS）
```

## 技术栈

| 端 | 技术 |
|---|---|
| 后端 | Node.js 18+ / Express 4 / better-sqlite3（无 ORM，零测试依赖） |
| 前端 | React 18 / Vite 5 / TypeScript 5（无 UI 库，手写 CSS） |
| 契约 | OpenAPI 3.0（YAML） |
| 协作 | Git + GitHub（国内网络可换 gitee，流程相同） |
