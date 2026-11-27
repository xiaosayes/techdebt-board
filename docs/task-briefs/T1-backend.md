# T1：后端 API（owner：你）

## 背景
实现技术债看板全部后端接口，这是前端并行开发的基础。

## 验收标准（可判定）
- [ ] `GET /api/debts` 返回数组，支持 `status` / `severity` / `module` 过滤
- [ ] `POST /api/debts` 创建；缺 title/severity/module 或枚举非法 → 400 + `VALIDATION_ERROR`
- [ ] `PATCH /api/debts/:id` 部分更新（只改提交的字段），可流转 status
- [ ] `DELETE /api/debts/:id` 返回 204；id 不存在 → 404 + `DEBT_NOT_FOUND`
- [ ] `GET /api/stats` 返回 `{ total, bySeverity, byStatus }`
- [ ] `npm test` 全绿（每个接口至少 正常+异常 各一条用例）
- [ ] 杀掉进程重启，数据不丢

## 范围
- 只写 `backend/` 目录
- **不做**：鉴权、日志框架、分页、CORS 白名单（开发期允许 *）

## 契约
`docs/contracts/openapi.yaml` v1.0。**字段名、错误码以它为准，禁止发明。**

## 约束
- 不新增依赖（仅 express、better-sqlite3）
- SQL 全部参数化
- 校验写在路由层，db.js 只管连接和建表

## AI 使用说明
- 骨架 / SQL / 测试草稿：AI 生成
- 校验逻辑、错误格式：人工逐行核对
- 测试断言（具体错误码、具体状态值）：人工确认，不允许只断言"不报错"
