# AGENTS.md — TechDebt Board

## 项目定位
轻量技术债看板：记录、流转（todo/doing/done）、统计技术债。
单页应用 + REST API + SQLite 持久化。
**不包含**：登录鉴权、多人实时协作、通知、分页。

## 技术栈（锁定，不要升级/替换）
- 后端：Node.js 18+、Express 4、better-sqlite3。**禁止 ORM**。
- 前端：React 18、Vite 5、TypeScript 5。**禁止新增 UI 组件库**，样式手写 CSS。
- 测试：后端 node:test（Node 内置），前端本期不写测试。

## 目录结构（开发完成后应长这样）
```
backend/
  src/app.js        Express app：CORS、路由挂载、错误中间件
  src/server.js     启动入口，端口 3001
  src/db.js         SQLite 连接 + 建表
  src/routes/debts.js  路由 + 参数校验
  test/api.test.js  接口测试
  data.db           SQLite 文件（gitignore，不提交）
frontend/
  src/api/types.ts  从契约转写的类型（与 openapi.yaml 字段一致）
  src/api/client.ts fetch 封装，baseURL = import.meta.env.VITE_API_BASE
  src/api/debts.ts  listDebts / createDebt / updateDebt / deleteDebt / getStats
  src/components/   Board / Column / Card / AddForm / StatsBar / FilterBar
  src/App.tsx       根组件
docs/
  contracts/openapi.yaml   接口契约（唯一事实源）
  task-briefs/             任务卡
  acceptance.md            验收清单
```

## 常用命令
```bash
# 后端
cd backend && npm install && npm start    # 启动 :3001
cd backend && npm test                    # 跑接口测试

# 前端
cd frontend && npm install && npm run dev     # 开发 :5173
cd frontend && npm run build                  # 类型检查 + 构建
VITE_API_BASE=http://localhost:3001 npm run dev   # 连真实后端
VITE_API_BASE=http://localhost:4010 npm run dev   # 连 Prism mock
```

## 代码约定
1. 接口字段一律 **snake_case**（与契约一致）；前端展示层转换。
2. 错误格式统一 `{ "error": { "code": "SNAKE_CASE", "message": "人话" } }`。
3. 时间一律 ISO8601 UTC 字符串（SQLite 用 `datetime('now')`）。
4. 参数校验在路由层；校验失败 400 + `VALIDATION_ERROR`。
5. 资源不存在 404 + `DEBT_NOT_FOUND`。
6. 后端 SQL 必须参数化（`?` 占位），禁止字符串拼接。
7. 前端请求必须走 `src/api/client.ts`，组件里禁止裸 `fetch`。

## 禁止事项
- 不要修改 `docs/contracts/openapi.yaml` 后顺手手改 `frontend/src/api/types.ts`——**改契约必须先提 PR**。
- 不要新增依赖（express / better-sqlite3 / react / vite 及其官方插件之外）。
- 不要把 `backend/data.db` 提交进仓库。
- 不要 `git push --force`，不要直推 main。

## PR 规范
- 分支：`feat/xxx`、`fix/xxx`、`docs/xxx`。
- 单 PR 一个意图；合并方式：Squash merge。
- PR 描述按 `.github/pull_request_template.md` 填写。
