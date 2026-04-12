# 搜索验证平台架构设计

更新时间：2026-04-12

这份文档面向你的搜索验证平台。目标不是一开始做成大而全的平台，而是先搭一套能支持 `检索调试、badcase 分析、评估验证、模型接入、实验对比` 的最小可用系统。

## 1. 推荐技术栈

- 前端：`Vue`
- 在线后端：`Golang`
- 搜索引擎：`Elasticsearch`
- 模型服务：`Python`
- 配置与记录库：`PostgreSQL` 或 `SQLite`

这套组合是合理的。`Go` 负责在线编排，`Python` 负责模型和训练，`ES` 负责检索与 explain，前端负责可视化调试。

## 2. 核心原则

- `ES` 是检索引擎，不要把它当唯一主库。
- `Go` 做主服务层，统一收口检索、模型、评估、实验接口。
- `Python` 独立成模型服务，不要嵌进 `Go` 进程。
- 先做单体 API，后面再拆服务，不要一开始微服务化。

## 3. 系统分层

### 3.1 前端层

Vue 页面建议至少包含：

- 搜索调试页：输入 query、位置、城市、过滤条件
- 结果对比页：baseline vs candidate
- explain 页：查看召回来源、字段命中、排序特征
- badcase 管理页：记录 case、归因、修复状态
- 评估页：查看 Recall、MRR、NDCG、分桶表现

### 3.2 Go 服务层

Go 负责统一 API 和流程编排，建议拆成下面几个模块：

- `search-api`：接收 query，请求 ES，聚合结果
- `suggest-api`：处理 sug 请求
- `rank-orchestrator`：调用 Python 模型服务做粗排/精排
- `eval-api`：跑离线评估和实验对比
- `case-api`：badcase、标注、实验记录管理
- `config-api`：索引配置、召回开关、实验参数管理

### 3.3 ES 检索层

ES 负责：

- POI 主索引
- Suggest 索引
- 地址或结构化召回索引
- explain / profile
- 多字段检索
- geo query

建议至少分开：

- `poi_index`
- `suggest_index`
- `address_index`

### 3.4 Python 模型层

Python 建议独立成服务，比如 `FastAPI`：

- `query-understanding-service`
- `suggest-rank-service`
- `rank-service`
- `feature-service`

可承载的能力包括：

- Query 分类
- 意图识别
- Suggest 精排
- 粗排/精排模型推理
- 特征拼装

### 3.5 关系库层

不要把这些数据放到 ES：

- badcase
- judgment set
- 标注结果
- 实验配置
- 指标快照
- 用户操作记录

这些更适合放在 `PostgreSQL` 或早期先用 `SQLite`。

## 4. 一条典型请求链路

以 POI 搜索为例：

1. 前端发起 query，请求 `Go search-api`
2. Go 做基础 normalize 和参数校验
3. Go 调 ES 做多路召回
4. Go 拼装粗排特征，必要时调用 Python 粗排模型
5. Go 取 TopK 候选，再调用 Python 精排模型
6. Go 做业务重排、去重、展示整理
7. 结果返回前端
8. 同时落日志，供后续评估和训练

Suggest 链路类似，只是召回和排序会更偏 prefix、热度、地理上下文。

## 5. 最小可行版本

第一阶段不要做太复杂，先做下面 5 个模块：

1. Vue 搜索调试页
2. Go 搜索 API
3. ES POI 检索
4. Python 精排服务
5. PostgreSQL/SQLite badcase 与评估记录

最小功能范围：

- 支持 query + location 搜索
- 返回 TopK 结果
- 支持 explain 查看
- 支持 baseline / candidate 对比
- 支持 badcase 录入

## 6. 目录结构建议

```text
search-lab/
  frontend/          # Vue
  backend/           # Go
  model-service/     # Python
  scripts/           # 索引构建、数据导入、评估脚本
  configs/
  docs/
  cases/
  eval/
```

## 7. 数据表建议

关系库建议至少有这些表：

- `badcase_records`
- `query_samples`
- `relevance_labels`
- `experiment_runs`
- `experiment_metrics`
- `feature_snapshots`
- `system_configs`

## 8. 后续演进顺序

建议按这个顺序长：

1. 检索调试
2. badcase 平台
3. 离线评估
4. baseline vs candidate 对比
5. Suggest 验证
6. Query 理解接入
7. 粗排/精排接入
8. 实验平台化

## 9. 需要注意的坑

- 不要把 ES 当主库。
- 不要把模型逻辑散落在 Go handler 里。
- 不要一开始就上复杂微服务。
- 不要没有日志和评估就接模型。
- 不要把 explain、特征、分桶分析做丢。

## 10. 最终建议

你的技术选型是可行的，而且适合做搜索验证平台。

更稳的组合是：

- `Vue` 做调试与评估界面
- `Go` 做统一编排层
- `ES` 做在线检索
- `Python` 做模型推理和训练
- `PostgreSQL/SQLite` 做配置、标注、实验记录

先把平台做成“能看清问题、能跑评估、能接模型”的系统，不要一开始追求大而全。
