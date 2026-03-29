# 地图搜索资深专家学习路线图

更新时间：2026-03-29

## 1. 目标定义

这份路线图面向已经在地图搜索或本地搜索场景中工作的工程师，目标不是把你训练成“会配 ES 的工程师”，而是逐步成长为能够独立负责搜索效果、评估闭环、地理能力建设和系统演进的资深搜索专家。

资深地图搜索专家，至少要具备以下能力：

- 能快速定位 badcase 属于召回、排序、Query 理解、地理约束还是数据问题。
- 能设计并落地完整优化链路：问题定义 -> 数据分析 -> 策略设计 -> 评估 -> 实验 -> 回滚与监控。
- 能把文本相关性、空间相关性、场景相关性统一到一套搜索框架里。
- 能兼顾效果、延迟、稳定性、资源成本，而不是只盯相关性分数。
- 能抽象出方法论，带团队做复杂搜索项目。

## 2. 学习总策略

### 2.1 能力地图

你需要把能力拆成 8 个模块同步建设：

1. 信息检索基础
2. Lucene / Elasticsearch / OpenSearch 内核理解
3. Query 理解与文本处理
4. 地图搜索与本地搜索领域知识
5. 排序、LTR、向量检索、重排
6. 评估体系、日志分析、A/B 实验
7. 搜索工程架构与性能治理
8. 业务理解与策略抽象

### 2.2 学习原则

- 以 badcase 驱动学习，不要只靠看书。
- 以评估闭环驱动优化，不要只看 demo 效果。
- 以真实链路驱动理解，不要把知识割裂成 NLP、ES、地理几个孤岛。
- 每个阶段必须有可交付产出，否则很容易停留在“知道一点”。

### 2.3 时间投入建议

如果你在职，建议按每周 8 到 12 小时执行：

- `3h` 理论阅读
- `3h` 代码实验
- `2h` 日志 / badcase 分析
- `1h` 输出笔记或总结
- `1h` 回顾和修正路线

### 2.4 每周固定节奏

- 周一或周二：读一章教材 / 一篇官方文档 / 一篇论文
- 周三：把一个知识点做成最小实验
- 周四：分析一组真实搜索 case
- 周末：整理学习笔记，输出一页结论和下周计划

## 3. 9 个月详细路线

## 第 0 月：建立基线与工作台

### 目标

建立自己的学习和实验环境，避免后面只停留在概念层。

### 要做的事

- 准备一个本地实验环境：Lucene 或 OpenSearch / Elasticsearch 任一套都可以。
- 选一份可控数据集，最好包含：
  - POI 名称
  - 地址
  - 类目
  - 经度纬度
  - 城市 / 区县 / 商圈
  - 热度或点击字段
- 建一个 `search-lab` 仓库，至少包含：
  - 索引构建脚本
  - 查询脚本
  - badcase 样例集
  - 简单评估脚本
- 准备一个学习笔记目录，按“概念 / 实验 / badcase / 结论”四类沉淀。

### 阶段产出

- 一套能跑起来的最小搜索实验环境
- 一份 30 条以上的地图搜索 badcase 清单
- 一份“当前自己能力短板”评估表

## 第 1 月：信息检索基础

### 目标

建立搜索的一阶原理，不再把搜索理解成“字段 match + 调 boost”。

### 重点学习

- 倒排索引
- 词项词典与 posting list
- TF-IDF 与 BM25
- 召回与排序的基本分层
- Precision / Recall / F1 / MRR / NDCG
- 分词、停用词、词干化、归一化

### 实战要求

- 手写一个极简倒排索引 demo
- 用你自己的数据实现 BM25 检索
- 对比以下设置的影响：
  - 不分词 vs 分词
  - title 高权重 vs address 高权重
  - BM25 参数变化

### 阶段产出

- 一篇《我对倒排索引和 BM25 的理解》总结
- 一份 10 条 badcase 的打分拆解

## 第 2 月：Lucene / ES / OpenSearch 核心机制

### 目标

把“会用搜索引擎”升级为“理解搜索引擎如何工作”。

### 重点学习

- Lucene 的 segment、merge、refresh、commit
- analyzer / tokenizer / token filter
- query rewrite
- filter 与 score 的区别
- explain / profile 的使用
- 多字段检索与 BM25F 思想
- geo_point / geo_shape 的基本能力

### 实战要求

- 建一个包含 `name`、`alias`、`address`、`category`、`location` 的索引
- 为不同字段设计 analyzer
- 做一次 explain 级别的分数分析
- 观察 refresh、merge 对检索时效性和资源的影响

### 阶段产出

- 一张 Lucene 到 ES/OpenSearch 的核心机制脑图
- 一篇《为什么某些 case 在 explain 里分数异常》分析文档

## 第 3 月：评估体系与 badcase 分析

### 目标

从“凭感觉调结果”转成“基于证据做相关性优化”。

### 重点学习

- 搜索评估的离线与在线方法
- judgment list / 标注集构建
- query 分桶
- badcase 归因体系
- 零结果率、首条满意率、改写率、CTR、会话成功率
- rank eval API 与自定义评估脚本

### 实战要求

- 选 100 到 300 个 query，按类型分桶：
  - 品牌词
  - 地标词
  - 地址词
  - 行政区词
  - 周边意图词
- 自己定义 3 级或 4 级相关性标注标准
- 对现有搜索结果做一轮离线评估
- 做一张 badcase 分类表：
  - 召回缺失
  - 排序错误
  - 分词问题
  - 地址解析问题
  - POI 数据质量问题
  - 地理约束误伤

### 阶段产出

- 一套最小离线评估集
- 一份 badcase 周报模板
- 一份自己的指标看板定义文档

## 第 4 月：Query 理解与文本处理

### 目标

掌握短 query、歧义 query、地址 query 的处理方法。

### 重点学习

- 拼写纠错
- 同义词、别名、品牌别称
- Query 改写
- Query 分类
- 实体识别与槽位抽取
- 地址解析与结构化
- Suggest / 自动补全 / prefix 检索

### 地图场景要重点关注

- “南站”“万达”“人民医院”这类强歧义词
- “北京朝阳大悦城星巴克”这类组合 query
- “西湖附近咖啡店”这类地点 + 周边意图 query
- “杭州市西湖区文三路 138 号”这类地址 query

### 实战要求

- 设计一个 query taxonomy
- 为 200 个 query 做人工标注
- 做一个轻量 query 解析器，输出：
  - 核心实体
  - 行政区
  - 地点限定词
  - 类目词
  - 周边 / 最近 / 到这里 等意图词

### 阶段产出

- 一份 Query 分类体系
- 一份地址解析规则或地址归一化方案
- 一份《地图搜索 Query 理解问题清单》

## 第 5 月：地图搜索与本地搜索核心能力

### 目标

真正补上“地图搜索区别于通用文本搜索”的核心知识。

### 重点学习

- POI 数据模型
- 行政区划体系
- 地标、商圈、道路、门牌、社区的层级关系
- 地理编码与逆地理编码
- 邻近搜索
- 地理过滤与地理排序
- 空间索引：Geohash、S2、H3 的基本思想
- 用户位置、搜索半径、区域约束、出行场景

### 关键认知

地图搜索不是“文本最像就排第一”，而是至少要统一考虑：

- 文本相关性
- 空间相关性
- 场景相关性
- 实体可信度 / 数据质量

### 实战要求

- 做一个附近搜 demo
- 做一个地址搜 demo
- 对比以下排序特征的效果：
  - 文本分
  - 用户到 POI 的距离
  - POI 热度
  - 是否营业
  - 行政区是否匹配

### 阶段产出

- 一张地图搜索召回与排序链路图
- 一份《附近搜 / 地址搜 / POI 搜 的差异化策略》文档

## 第 6 月：地理编码、地址搜索与开源系统拆解

### 目标

把地图搜索里最容易“看起来简单，实际上很深”的模块学透。

### 重点学习

- free-form address 与 structured address 的差异
- 地址解析与地址归一化
- 行政区、街道、门牌、楼栋的层级理解
- 插值、候选召回、地址置信度
- 开源地理搜索系统的设计

### 建议拆的系统

- Nominatim
- Pelias
- libpostal

### 实战要求

- 用开源组件跑一个最小 geocoding 流程
- 选 50 条地址 query，分析其失败模式
- 总结地址搜索中最常见的 10 类问题

### 阶段产出

- 一篇《地址搜索系统设计笔记》
- 一份开源 geocoder 组件对比表

## 第 7 月：排序、LTR 与特征工程

### 目标

从规则排序升级到机器学习排序视角。

### 重点学习

- 点式、对式、列表式排序学习
- 特征工程
- 标签设计
- 样本偏差与位置偏差
- 初排、粗排、精排、重排
- 规则与模型混排

### 地图搜索中的典型特征

- 文本匹配特征
- Query 与 POI 类目匹配
- 用户位置距离
- 区域约束匹配度
- 点击率、导航率、收藏率
- 热度与时效性
- POI 质量分

### 实战要求

- 设计一套 20 到 50 个排序特征
- 基于离线标注或行为数据做一个简单 LTR 实验
- 分析哪些特征在“品牌词”“附近词”“地址词”中收益不同

### 阶段产出

- 一份排序特征清单
- 一份 LTR 基线实验报告

## 第 8 月：向量检索、混合检索与语义重排

### 目标

补上现代搜索体系，但不被“向量万能论”带偏。

### 重点学习

- 稠密向量与稀疏向量
- ANN 检索
- HNSW
- 混合检索
- RRF
- Cross-encoder 重排
- 语义召回适用场景与副作用

### 地图搜索里要特别谨慎

向量检索在地图搜索中有价值，但通常不是主链路的替代者，更常见位置是：

- 补召回
- 别名 / 语义相似召回
- 长尾 query 兜底
- TopK 重排

### 实战要求

- 对比 lexical、vector、hybrid 三种召回方式
- 用 50 到 100 个长尾 query 做小规模评估
- 明确总结哪些 query 不适合让语义模型主导

### 阶段产出

- 一份混合检索实验结论
- 一份《地图搜索里语义检索的适用边界》文档

## 第 9 月：系统、架构与资深能力建设

### 目标

从“能做策略”升级为“能负责系统与方向”。

### 重点学习

- 搜索系统架构分层
- 数据接入、清洗、索引、服务、监控、回流闭环
- 缓存、降级、熔断、限流
- 高并发低延迟优化
- 索引更新与时效性治理
- 效果与成本权衡
- 项目管理与跨团队协同

### 实战要求

- 输出一版地图搜索整体架构图
- 设计一个“效果优化专项”的完整方案
- 以负责人视角写一份技术方案：
  - 问题背景
  - 目标指标
  - 方案拆解
  - 风险与回滚
  - 评估方法

### 阶段产出

- 一份完整技术方案文档
- 一次内部分享材料
- 一份你的“地图搜索方法论”总结

## 4. 你每个阶段都必须做的 5 件事

1. 维护一个 badcase 集，每周至少更新一次。
2. 每月做一次复盘：最有效的优化点是什么，最浪费时间的点是什么。
3. 每月至少做一个最小实验，不允许只看资料不写代码。
4. 每月输出一篇总结文档，逼自己抽象。
5. 每季度做一次对外或对内分享，提升表达与结构化能力。

## 5. 建议的实战项目路线

如果你想把学习效果拉满，建议按下面顺序做 4 个项目。

### 项目 1：最小 POI 搜索系统

目标：

- 支持名称、别名、地址、类目检索
- 支持基础 BM25 排序
- 支持 geo filter

你会学到：

- 索引结构
- 字段权重
- explain 分析

### 项目 2：地图搜索评估与 badcase 平台

目标：

- 管理 query 样本
- 支持相关性标注
- 输出 NDCG / MRR / Recall

你会学到：

- 评估闭环
- query 分桶
- 相关性治理

### 项目 3：地址搜索 / geocoder 实验

目标：

- 支持 free-form address 输入
- 输出结构化地址槽位
- 做候选召回和排序

你会学到：

- 地址解析
- 地理编码
- 本地搜索核心难点

### 项目 4：混合检索与重排实验

目标：

- 对比 lexical / vector / hybrid / rerank
- 建立 query 类型与策略的映射关系

你会学到：

- 现代检索体系
- 语义模型边界
- 工程与效果权衡

## 6. 学习资料索引

下面优先给一手资料、官方文档、经典教材和开源系统。

### 6.1 信息检索基础

- Stanford IR Book: https://nlp.stanford.edu/IR-book/
- Stanford IR Book Slides: https://nlp.stanford.edu/IR-book/newslides.html
- Lucene Similarity 文档: https://lucene.apache.org/core/5_5_3/core/org/apache/lucene/search/similarities/package-summary.html

建议重点读：

- 倒排索引
- 向量空间模型与 BM25
- 相关性评估
- 分类与查询处理基础章节

### 6.2 Lucene / Elasticsearch / OpenSearch

- Elasticsearch `combined_fields` 与 BM25F 思想: https://www.elastic.co/docs/reference/query-languages/query-dsl/query-dsl-combined-fields-query
- Elasticsearch Geo Queries: https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-queries.html
- Elasticsearch Geo Distance Query: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-geo-distance-query.html
- Elasticsearch `distance_feature`: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-distance-feature-query.html
- OpenSearch Ranking Evaluation API: https://docs.opensearch.org/latest/api-reference/search-apis/rank-eval/
- OpenSearch Learning to Rank: https://docs.opensearch.org/latest/search-plugins/ltr/index/

### 6.3 地图搜索与空间索引

- H3 官方文档: https://h3geo.org/docs
- Google S2 Geometry: https://github.com/google/s2geometry
- Nominatim Search API: https://nominatim.org/release-docs/5.0/api/Search/
- Nominatim 首页与文档入口: https://nominatim.org/
- Pelias 项目: https://github.com/pelias/pelias

建议重点看：

- H3 / S2 的索引思想和适用边界
- Nominatim 对 free-form 和 structured query 的支持
- Pelias 如何组织 geocoder 的数据导入、API 与服务拆分

### 6.4 地址解析与地理 NLP

- libpostal: https://github.com/openvenues/libpostal
- Nominatim Tokenizers: https://nominatim.org/release-docs/develop/develop/Tokenizers/

建议重点看：

- 地址归一化
- 地址槽位解析
- 多语言地址处理
- tokenizer 在地址搜索中的作用

### 6.5 排序、LTR、向量与重排

- Elastic Learning to Rank: https://www.elastic.co/guide/en/elasticsearch/reference/current/learning-to-rank.html
- OpenSearch k-NN vector: https://docs.opensearch.org/3.3/mappings/supported-field-types/knn-vector/
- OpenSearch Approximate k-NN: https://docs.opensearch.org/latest/vector-search/vector-search-techniques/approximate-knn/
- OpenSearch k-NN API: https://docs.opensearch.org/latest/vector-search/api/knn/
- HNSW 论文: https://arxiv.org/abs/1603.09320
- FAISS: https://github.com/facebookresearch/faiss
- Elastic Semantic Reranking: https://www.elastic.co/guide/en/elasticsearch/reference/current/semantic-reranking.html

建议重点看：

- HNSW 的索引与搜索流程
- 向量检索的召回率、延迟、内存权衡
- rerank 在搜索链路中的位置，而不是把它当前置召回

### 6.6 开源系统拆解顺序

建议按下面顺序看源码或文档：

1. libpostal
2. Nominatim
3. Pelias
4. Lucene / OpenSearch / Elasticsearch 对应模块

原因：

- 先看地址和地理 NLP，建立“地理 query 不是普通文本”的认识。
- 再看 geocoder，理解搜索系统如何和地理数据、行政区、PIP、插值模块协作。
- 最后回到通用搜索引擎，理解底层能力如何承载业务策略。

## 7. 输出物清单

如果你严格执行 9 个月，建议至少产出以下内容：

- 9 篇月度总结
- 1 套最小地图搜索实验系统
- 1 套离线评估集
- 1 套排序特征清单
- 1 份地图搜索整体架构图
- 1 份 geocoder 设计笔记
- 1 份混合检索实验报告
- 1 次内部分享

这些产出很重要，因为资深能力的本质之一就是“把经验结构化和复用化”。

## 8. 自我检验标准

当你能稳定回答下面这些问题时，说明你已经接近资深：

- 为什么这个 query 没召回出来？
- 为什么这个结果文本上不差，但不该排第一？
- 当前问题到底该改召回、改排序、改改写、改数据，还是改地理约束？
- 哪些 query 适合向量召回，哪些不适合？
- 地址搜索失败的原因是在 parser、候选召回、还是排序？
- 怎么设计一套评估集来证明优化真的有效？
- 如果延迟必须下降 30%，你会牺牲什么，不牺牲什么？

## 9. 常见误区

- 误区一：把搜索等同于 ES 调参。
- 误区二：只追新技术，不打基本功。
- 误区三：只看 CTR，不建评估体系。
- 误区四：把地图搜索当成普通文本搜索。
- 误区五：认为向量检索可以替代全部规则和地理约束。
- 误区六：只会做策略，不会做复盘、归因和系统设计。

## 10. 最后的执行建议

如果你只能抓 3 个重点，优先顺序是：

1. 信息检索基础 + 评估体系
2. Query 理解 + 地图领域知识
3. 排序 / LTR / 混合检索

如果你只能抓 1 个工作方法，那就是：

把每一个 badcase 都当作一次系统诊断题，不要当作一次临时调参题。

真正的资深搜索专家，不是“知道很多术语”，而是能把复杂搜索问题拆开，定位清楚，证明有效，并沉淀成方法论。

## 11. 90 天启动版执行计划

如果你不想一上来就看 9 个月，可以先按前 90 天执行。前 90 天的目标不是“学完”，而是建立起一套能持续进化的方法。

### 第 1 到 2 周

- 搭建最小搜索实验环境
- 准备一份 POI 测试数据
- 整理 30 到 50 条真实 query
- 输出一份当前问题清单

验收标准：

- 本地可完成索引构建、查询、结果查看
- 你能解释每个字段在索引中的作用

### 第 3 到 4 周

- 学完倒排索引、BM25、基本评估指标
- 写一个最小 BM25 实验
- 用 10 条 query 做 explain 分析

验收标准：

- 你能独立解释为什么某个结果分高或分低
- 你能说清楚 Recall 和 NDCG 的区别

### 第 5 到 6 周

- 学 analyzer、tokenizer、filter
- 分别给 `name`、`alias`、`address` 设计分析链
- 对比不同 analyzer 的 badcase 表现

验收标准：

- 你能说明某类 query 应该在哪个 analyzer 级别处理，而不是一律靠 boost

### 第 7 到 8 周

- 建一套最小离线评估集
- 对 query 做类型分桶
- 输出 badcase 分类报告

验收标准：

- 你能把 20 条 badcase 明确归因到具体链路

### 第 9 到 10 周

- 学 Query 理解、别名、纠错、地址解析
- 设计 query taxonomy
- 给 100 条 query 做人工标注

验收标准：

- 你能说清楚不同 query 类型对应的策略差异

### 第 11 到 12 周

- 学 geo query、距离排序、附近搜
- 做一个附近搜 demo
- 引入距离、行政区、热度三类特征

验收标准：

- 你能说明为什么地图搜索不能只靠文本分排序

### 第 13 周

- 做一次 90 天复盘
- 写出接下来 6 个月计划
- 明确你的优先突破点

建议优先突破方向：

- 如果你基础弱，优先补 IR 和评估
- 如果你基础尚可，优先补 Query 理解和 geocoder
- 如果你已经能做规则策略，优先补 LTR 和系统架构

## 12. badcase 分析模板

建议你把每个 badcase 都按下面模板记录，长期积累会非常有价值。

### 模板字段

- Query
- 用户位置
- 期望结果
- 当前 Top10 结果
- 错误类型
- 一级归因
- 二级归因
- 建议修复手段
- 是否需要数据修复
- 是否需要评估集补充
- 修复后验证方式

### 错误类型建议

- `recall_missing`
- `ranking_error`
- `query_understanding_error`
- `address_parse_error`
- `geo_constraint_error`
- `poi_data_error`
- `freshness_error`
- `business_rule_conflict`

### 一级归因建议

- 词法召回不足
- 别名体系不足
- 分词错误
- 地址结构化失败
- 地理范围限制错误
- 特征缺失
- 权重不合理
- 标注或评估口径不一致

### 记录示例

```text
Query: 上海南站
用户位置: 上海徐汇区
期望结果: 上海南站地铁站 / 火车站相关实体靠前
当前问题: 某些“南站路”“南站附近商户”排到前面
错误类型: ranking_error
一级归因: 地理实体重要性和场景权重不足
二级归因: station 类别权重偏低，精确实体特征缺失
建议修复: 引入交通枢纽实体特征；query=实体主名时提高 exact match 权重
验证方式: 交通枢纽类 query 分桶 NDCG@5 对比 + badcase 回归
```

## 13. 按 Query 类型的优化抓手表

下面这张表可以帮助你在做 case 分析时快速定位优化方向。

| Query 类型 | 典型例子 | 首先检查什么 | 常见优化抓手 |
| --- | --- | --- | --- |
| 品牌词 | 星巴克、万达 | 别名、品牌主实体召回、热度是否过强 | 别名库、品牌实体聚合、exact match 提权 |
| 地标词 | 西湖、天安门 | 主实体唯一性、区域歧义、旅游热度 | 地标特征、行政区限定、实体权威分 |
| 地址词 | 文三路 138 号 | 地址解析、门牌插值、结构化召回 | 地址 tokenizer、结构化字段召回、geocoder 排序 |
| 行政区词 | 浦东新区、朝阳区 | 区划实体与普通 POI 冲突 | 行政区实体类型特征、区域实体优先 |
| 类目词 | 咖啡店、加油站 | 类目召回、用户位置、营业状态 | 类目倒排、附近优先、营业中提权 |
| 周边意图词 | 附近咖啡店 | 用户位置是否生效、距离排序是否合理 | geo distance、半径动态调节、热度和距离平衡 |
| 歧义实体词 | 南站、人民医院 | 城市上下文、类目优先级、主实体识别 | 城市限定、实体分类、exact entity boost |
| 组合词 | 西湖附近咖啡店 | 语义拆解是否正确 | 主地点识别、范围约束、二阶段重排 |
| 长尾词 | 24 小时宠物医院 | 召回稀疏、营业属性、语义补召回 | 结构化属性召回、长尾扩展、hybrid recall |

## 14. 阶段验收标准

你可以按下面标准判断自己是否真的进入了资深轨道。

### 初级到中级

- 能独立搭建搜索实验环境
- 能分析 explain
- 能处理基础召回和字段权重问题

### 中级到高级

- 能自己建评估集
- 能稳定做 badcase 归因
- 能处理 query 理解和地理约束问题
- 能做一个完整优化闭环

### 高级到资深

- 能设计地图搜索的多阶段链路
- 能清楚权衡 lexical、geo、LTR、vector 的边界
- 能主持一个效果优化专项
- 能对架构、延迟、稳定性提出工程方案
- 能把经验沉淀成模板、方法论和团队规范

## 15. 推荐的笔记目录结构

为了避免后面资料散乱，建议你的笔记目录从一开始就结构化。

```text
search-notes/
  01-ir-basics/
  02-lucene-es/
  03-evaluation/
  04-query-understanding/
  05-geo-local-search/
  06-geocoder/
  07-ltr-ranking/
  08-vector-hybrid/
  09-architecture/
  badcases/
  monthly-review/
```

每篇笔记尽量固定 4 段：

1. 这个问题是什么
2. 为什么会发生
3. 我如何验证
4. 这对地图搜索意味着什么

## 16. 最务实的执行方式

如果你最近工作很忙，不要试图同时铺开所有方向。最务实的顺序是：

1. 用你手头真实业务 case 建一个 badcase 集
2. 用 badcase 倒逼补 IR、评估、Query 理解
3. 再进入地图搜索和 geocoder 专项
4. 最后补 LTR、vector、架构抽象

顺序不要反。基础问题没拆清楚时，过早上模型，通常只会让系统更难解释、更难控制。
