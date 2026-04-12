# 地图搜索学习资料包

更新时间：2026-04-12

这套资料面向地图搜索 / 本地搜索工程师，目标是帮助你从搜索工程师成长到能够独立负责效果、评估、地理能力和系统演进的资深搜索专家。

## 3 分钟开始

如果你现在只想知道怎么开始，按这个顺序：

1. 先读 [搜索学习资料使用手册](search-docs-usage-manual.md)
2. 再读 [搜索架构师最高阶学习路线图](search-architect-ultimate-roadmap.md)
3. 然后直接执行 [搜索架构师前 90 天任务面板](search-architect-first-90-days-task-board.md)
4. 同步用 [地图搜索模板与工作流](map-search-templates.md) 记录
5. 做到第 2 阶段后，再接 [搜索架构师 24 个月执行计划](search-architect-24-month-execution-plan.md)

如果你今天就开始，本周只做 4 件事：

- 梳理当前搜索主链路
- 盘点 query / 点击 / POI / 索引数据
- 起草 query taxonomy
- 产出链路草图和数据资产清单

## 怎么实施

不要按“读完一堆文档”来做，按“每周交付”来做。

固定节奏：

1. 周一确定本周主题
2. 周二读资料和源码
3. 周三做最小实验
4. 周四分析 badcase 和日志
5. 周五写周报和结论

每周必须留下：

- `1` 篇笔记
- `1` 个实验
- `10` 条 badcase 分析
- `1` 个结构化交付物

每月必须做：

1. 跑一轮离线评估
2. 更新 badcase 分布
3. 写 1 份月度复盘
4. 做 1 次自测
5. 判断是否进入下一阶段

## 遇到问题看哪份

- 不知道先看哪个：看 [搜索学习资料使用手册](search-docs-usage-manual.md)
- 不知道这周做什么：看 [搜索架构师前 90 天任务面板](search-architect-first-90-days-task-board.md)
- 不知道长期怎么走：看 [搜索架构师 24 个月执行计划](search-architect-24-month-execution-plan.md)
- 不知道读什么、做什么实验：看 [搜索架构师必读资料、必做实验、必拆源码清单](search-architect-reading-experiments-and-teardowns.md)
- 不知道验证平台怎么搭：看 [搜索验证平台架构设计](search-validation-platform-architecture.md)
- 不知道怎么记 badcase、周报、评估：看 [地图搜索模板与工作流](map-search-templates.md)
- 不知道怎么写专项方案：看 [地图搜索技术方案模板](map-search-tech-design-template.md)

## 文档索引

- [搜索学习资料使用手册](search-docs-usage-manual.md)
  - 这套文档的总入口，说明每份文档的定位、阅读顺序、每周每月使用方法，以及遇到真实问题时该查哪一份
- [搜索架构师最高阶学习路线图](search-architect-ultimate-roadmap.md)
  - 以搜索架构师为目标的 18 个月高强度路线，覆盖数据、召回、排序、训练闭环、分布式架构、成本治理与团队方法论
- [搜索架构师 24 个月执行计划](search-architect-24-month-execution-plan.md)
  - 按月按周拆开的执行版路线，适合长期推进
- [搜索架构师前 90 天任务面板](search-architect-first-90-days-task-board.md)
  - 把前 3 个月拆成每周任务、交付物、验收标准和风险，适合立刻开始执行
- [搜索架构师必读资料、必做实验、必拆源码清单](search-architect-reading-experiments-and-teardowns.md)
  - 读什么、做什么实验、拆什么源码的完整清单
- [搜索验证平台架构设计](search-validation-platform-architecture.md)
  - 基于 Vue + Go + ES + Python 的搜索验证平台设计，包含模块拆分、请求链路、最小可行版本和演进建议
- [POI 搜索负责人级学习路线图](poi-search-principal-learning-roadmap.md)
  - 面向 POI / 地图搜索负责人的高阶路线，补齐数据融合、Suggest、召回路由、粗排、精排、业务重排、训练闭环
- [地图搜索资深专家学习路线图](map-search-senior-learning-roadmap.md)
  - 总路线、9 个月分阶段计划、前 12 周逐周执行版、资料索引、验收标准
- [地图搜索实战项目手册](map-search-practice-projects.md)
  - 4 个核心项目、里程碑、交付物、验收标准、推荐技术选型
- [地图搜索模板与工作流](map-search-templates.md)
  - badcase 模板、周报模板、月报模板、评估模板、实验复盘模板
- [地图搜索自测题库与面试题](map-search-self-assessment-and-interview-bank.md)
  - 分模块题库、答案要点、负责人级问题、90 天 / 6 个月 / 9 个月自测清单
- [地图搜索技术方案模板](map-search-tech-design-template.md)
  - 负责人视角的技术方案写法、专项治理方案模板、风险回滚模板

## 推荐执行节奏

- 每周：学习 1 个知识点，完成 1 个实验，拆 10 个 badcase
- 每月：做 1 次离线评估，写 1 篇专题总结
- 每季度：做 1 次专项复盘或内部分享

## 最小闭环

如果你时间有限，至少保证下面 4 件事：

1. 维护 badcase 集
2. 建立离线评估集
3. 做一个最小 POI 搜索实验系统
4. 每月输出一次结构化总结
