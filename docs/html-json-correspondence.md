# 第9步：原网页内容与 JSON 记录对应表

- 转换日期：2026-08-19
- 工作分支：database-rebuild
- 数据版本：0.1.0-draft
- 范围：只转换现有网页中能够明确对应的候选单元；不改造页面，不进入第10步

## 转换结果

| 数据文件 | 草稿记录数 | 原网页位置 |
| --- | ---: | --- |
| data/instruments.json | 5 | index.html#instrument-types |
| data/components.json | 15 | index.html#anatomy |
| data/crafts.json | 12 | index.html#craftsmanship |
| data/materials-tools.json | 12 | index.html#craftsmanship |
| data/people.json | 22 | index.html#heritage |
| data/music.json | 28 | index.html#music-classification、#performance |
| data/patterns.json | 5 | index.html#patterns |
| data/history.json | 5 | index.html#history、history.html |
| data/scenes.json | 0 | index.html#application |
| data/references.json | 3 | history.html 页脚 |
| data/media.json | 76 | 第8步媒体映射 |

首批建立 104 条资源草稿、3 条来源候选记录和 76 条媒体记录。全部编号登记在 data/id-register.json。

## 暂缓转换

- 美利其格等、李波等：群体式人物标签，不能无来源拆分为单个人物。
- 教育与传承等五个应用主题：缺少真实场景的时间、地点、参与者、活动名称和采集信息，data/scenes.json 只保留 deferred_candidates，不建立场景记录。
- 历史页“起源、演变、定型”是叙事分组，与五个阶段重叠，不另建三条重复事件记录。
- 柱状图评分缺少指标、方法、样本和来源，只保存在 legacy_display_score 并明确标记为非学术事实。
- 人物谱系连线、称号、别名和亲属括注只作为 legacy 字段保存，未转换成正式关系。

## 空值与审核策略

- 传统蒙古文、西里尔蒙古文、年代、逐条来源、版权许可、可信度和发布日期均未推测。
- 所有资源草稿状态为待核实，版权状态为版权待确认，数据库公开级别为暂不公开。
- 网页原有说明中尚未核实的内容只保存在 legacy_display 字段，不写入已核实事实字段。
- 三个通用来源候选项没有版本或稳定网址，也未关联到具体陈述，因此各资源的 source_refs 暂为空。

## 文件范围说明

路线第9步列出的十个目标 JSON 已全部创建。另增加 data/materials-tools.json，以覆盖第2步和第4步已经明确但在第9步目标清单中遗漏的“制作材料与工具”一级类型；增加 data/id-register.json 以执行第5步的编号登记规则。这两项都属于本步骤的数据结构化工作。
