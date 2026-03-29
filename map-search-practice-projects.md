# 地图搜索实战项目手册

更新时间：2026-03-29

这份手册不是泛泛的项目建议，而是按“你要真正变强，应该做什么项目”来设计的。每个项目都对应地图搜索的一类关键能力。

## 1. 项目总览

建议至少完成 4 个项目：

1. 最小 POI 搜索系统
2. 搜索评估与 badcase 平台
3. 地址搜索 / geocoder 实验系统
4. 混合检索与排序优化实验系统

## 2. 项目 1：最小 POI 搜索系统

### 目标

做一个能真实承载地图搜索基础能力的最小系统。

### 最小能力范围

- 支持名称、别名、地址、类目检索
- 支持基础分词与字段权重
- 支持地理过滤
- 支持 explain 分析
- 支持简单排序权重配置

### 推荐字段

- `poi_id`
- `name`
- `alias`
- `address`
- `category`
- `city`
- `district`
- `business_area`
- `location`
- `popularity`
- `status`

### 里程碑

#### M1：索引与查询跑通

- 完成数据准备
- 完成索引 mapping
- 完成基础 query API

验收：

- 10 条 query 可以返回合理结果

#### M2：字段策略与 analyzer

- 区分 `name` / `alias` / `address`
- 完成 analyzer 对比实验

验收：

- 你能说明不同字段为什么不能共用一套 analyzer

#### M3：地理过滤与距离特征

- 增加用户位置入参
- 增加 geo filter 或 distance score

验收：

- “附近”类 query 的结果明显优于纯文本排序

### 推荐交付物

- 索引设计文档
- explain 分析文档
- 字段权重实验报告

## 3. 项目 2：搜索评估与 badcase 平台

### 目标

让你的优化不再停留在“感觉变好了”。

### 核心能力

- query 样本管理
- 结果快照管理
- 相关性标注
- 指标计算
- badcase 归因

### 最小功能

- 导入 query 列表
- 存储每个 query 的 TopK 结果
- 支持相关性等级标注
- 支持输出：
  - Recall@K
  - MRR
  - NDCG@K
- 支持 badcase 分类

### 推荐数据结构

- `queries`
- `search_results_snapshot`
- `relevance_labels`
- `badcase_records`
- `experiment_runs`

### 里程碑

#### M1：离线评估跑通

- 选 100 条 query
- 定义 3 级或 4 级标注标准
- 跑一次 NDCG / MRR

#### M2：badcase 管理

- 增加错误类型和归因字段
- 输出每周 badcase 周报

#### M3：实验对比

- 支持基线版本 vs 候选版本对比
- 输出分桶收益

### 推荐交付物

- 标注规范
- 指标说明文档
- badcase 归因报告
- 实验对比模板

## 4. 项目 3：地址搜索 / geocoder 实验系统

### 目标

专门打磨地图搜索最难的专项之一。

### 核心能力

- free-form address 解析
- structured address 支持
- 候选召回
- 地址排序
- 结果置信度输出

### 推荐模块

- address parser
- address normalizer
- candidate retriever
- ranker
- confidence scorer

### 重点场景

- 省市区路街巷门牌完整地址
- 缺失行政区的半结构化地址
- 楼栋、园区、商场内商户地址
- 错别字、简称、口语化地址

### 里程碑

#### M1：地址解析

- 支持基础地址槽位解析
- 支持省市区路号等结构化输出

#### M2：候选召回

- 支持路名、门牌、楼栋等结构化召回
- 支持 free-form query 兜底

#### M3：候选排序与置信度

- 引入行政区一致性、地址完整度、文本匹配度等特征
- 输出 Top1 置信度

### 推荐交付物

- 地址解析问题清单
- 地址搜索失败模式报告
- geocoder 设计笔记

## 5. 项目 4：混合检索与排序优化实验系统

### 目标

把规则召回、词法召回、向量召回、重排组合起来，形成现代搜索视角。

### 核心能力

- lexical recall
- vector recall
- hybrid merge
- rerank
- query routing

### 建议验证的问题

- 长尾类目 query 是否从向量补召回中受益
- 品牌词 / 地址词 是否不适合以语义召回为主
- 向量召回对延迟和资源的影响有多大

### 里程碑

#### M1：baseline 建立

- 建 lexical baseline
- 建 vector baseline

#### M2：hybrid merge

- 设计融合策略
- 做 query 分桶对比

#### M3：rerank

- 选 TopK 做重排
- 对比只改召回和改重排的收益差异

### 推荐交付物

- 混合检索实验报告
- query 路由策略表
- 向量使用边界结论文档

## 6. 推荐技术选型

你不需要一次上最复杂的栈，优先保证可解释和可验证。

### 最小可行技术组合

- 检索引擎：OpenSearch 或 Elasticsearch
- 地址解析：libpostal
- geocoder 参考：Nominatim / Pelias
- 向量检索：OpenSearch k-NN 或 FAISS
- 指标计算：自定义脚本即可

### 代码结构建议

```text
search-lab/
  data/
  scripts/
  notebooks/
  configs/
  docs/
  cases/
  eval/
```

## 7. 项目验收的统一标准

每个项目都至少回答下面 6 个问题：

1. 目标 query 是什么
2. 基线是什么
3. 指标是什么
4. 优化策略是什么
5. 副作用是什么
6. 是否值得上线或继续扩展

## 8. 做项目时最容易踩的坑

- 没有 query 分桶，结果看起来“平均变好了”，实际重要 query 变差
- 没有离线评估，所有判断都靠肉眼
- 只记成功案例，不记失败案例
- 过早引入模型，导致问题更难归因
- 不记录资源与延迟，只看效果

## 9. 最推荐的项目执行顺序

1. 最小 POI 搜索系统
2. 搜索评估与 badcase 平台
3. 地址搜索 / geocoder 实验系统
4. 混合检索与排序优化实验系统

这个顺序的核心逻辑是：先有基础检索，再有评估闭环，再做地图专项，最后做现代检索扩展。
